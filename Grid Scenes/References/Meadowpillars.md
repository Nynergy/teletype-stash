# Meadowpillars

--------------------------------------------------------------------------------

## What is Meadowpillars?

_Meadowpillars_ is a hybrid generative 16-step hocketing sequencer for the
Teletype, in conjunction with the Monome Grid. It can play a constructed
sequence as arranged, or generate any number of notes to fit into a selected
scale (set using the N.B operator). It's billed as a 16-step sequencer, but with
some clever switches and timing you can treat it as a 32-step or even 64-step
sequencer. It is also designed to 'hocket' the sequences, or pass them around
the different outputs of the Teletype, and allows the user to reverse playback
of sequences. This, combined with the ability to change the loop length and
start/end positions of each sequence, gives _Meadowpillars_ a lot of live
playing appeal.

## How do I use it?

When you load the scene, the grid will light up to show the two sections of the
layout. The top four rows should be unlit (besides the playheads of each
sequence), while the bottom four rows contain some buttons for controlling the
scene. Trigger inputs 1 through 4 are for clocking sequences 1 through 4,
respectively. Trigger input 5 resets all the sequences to their start points. Of
course, you'll need to set up your sequences first, so let's go over what the
grid looks like, and how to set up sequences.

### Grid Layout

The top four rows of buttons are where we display the state of our four
sequences (our 'meadow'). The bottom four rows are where our controls live.
There should be five distinct columns of lit buttons (our 'pillars'), which
we'll go into detail about in a later section. First, let's go over the sequence
display, and dig deeper into how sequences are stored in the pattern data.

### Sequence Display ('Meadow')

Each row of the sequence display, or 'meadow', represents one of our 16-step
sequences. A sequence can be considered a list of 16 'beats' that denote rests,
normal notes, and generative notes, as well as a list of 16 'pitches' that
denote what degree of a user-selected scale will play. Each row displays a
bright playhead to denote where in the sequence we currently are, step-wise.

If you look in the Teletype's pattern data, you'll see that all four patterns
are by default set to be of length 16, going from index 0 to index 15. This is
where our 'beats' live, represented by the values 0 (rest), 1 (normal), and 2
(generative). A rest will produce no trigger output for that step, a normal note
will produce a trigger and set the corresponding CV out to the user-defined
pitch at that step, and a generative note will produce a trigger and set the
corresponding CV out to a random note within the first 12 degrees of the
user-selected scale.

You can go into the pattern data and set these steps to 0, 1, or 2, but that's a
bit tedious. Rather, the meadow is actually a set of buttons that allow you to
modify these beats on the fly. Hit a button in the meadow, and you'll see it
light up. Hit it again, and it will get brighter. Once more, and it will go dark
again. Dark buttons are rests, dim buttons are normal notes, and bright buttons
are generative notes. If you watch the pattern data while you do this, you can
see the changes happen to the data in real time.

Using the meadow like this, you can sequence the beats on the grid, from left to
right (step 1 to step 16). There is also a way to generate beats on the fly
using one of the controls, which we will talk about in a later section.

#### How do I actually make a sequence?

Notice that so far, all we've done is sequence the beats, so even if we start
running our sequences, the pitch information never changes. That's because we
need to add our pitch information to the pattern data. For each sequence, the
beats are contained within indices 0 to 15. Our pitch information will live
between indices 16 to 31, another 16 steps after our beats.

For pitch information, you can set it to any positive number, but keep in mind
that the number will be used with the N.B operator like so: CV 1 N.B Y, where Y
is the number from the pattern data. Unfortunately you cannot use the grid to
add pitch information by hand, but there is a button to generate a random pitch
sequence for you, which we'll introduce later.

Keep in mind that, even though you set a pitch degree for a certain step, it
will only play that pitch if the corresponding beat is set to 1. If it is set to
0, it will not play anything, and if it is set to 2, then a random pitch will be
played instead.

### Sequence Controls ('Pillars')

Each column of buttons in the sequence controls, or 'pillars', does something to
control the scene. We have controls to, from left to right:
  - Start/Stop Sequence Playback
  - Generate Random Beat Sequence
  - Hocket Sequences Up/Down
  - Generate Random Pitch Sequence
  - Reverse Sequence Playback

Let's go over how they work in couple at a time.

#### Playback Controls

The leftmost column of four buttons effectively 'mutes' the sequences. If the
button is active (bright), then the sequence will be clocked normally from its
trigger input. If the button is inactive (dim), then the sequence will not move
forward and trigger outputs will not fire, even as it receives clock. Each
button mutes the sequence in the order they appear on the grid from top to
bottom (top button mutes top sequence, bottom button mutes bottom sequence, and
so on).

The rightmost column of four buttons determines the playback direction of each
sequence (in the same top-to-bottom order as the leftmost column). If the button
is inactive (dim), playback proceeds forward as normal. If the button is active
(bright), playback is reversed. This can be a fun toggle to play with during a
performance.

#### Generative Controls

The second set of buttons from the left is a single column of two-wide buttons
that will generate a random beat sequence for the chosen sequence. You can watch
the meadow to see how this visually changes the chosen sequence. You can always
go in afterward and manually change some of the steps using the meadow buttons.

The second set of buttons from the right is a similar column of two-wide
controls, but instead of generating beats, it will generate a random pitch
sequence for the chosen sequence. This generated sequence will use scale degrees
0 to 12, inclusive, and just like the other generative controls, you can always
go into the pattern data afterward and modify the generated sequence to your
liking.

#### Hocketing Controls

The main reason I made this scene was to create a fun way to hocket sequences
between voices. The middle controls in the pillars do just that; there are two
2x2 buttons that move sequences up and down the meadow, respectively. When you
hit either button, you can watch how the sequences shift in the meadow. This
shift will be reflected in the outputs of the Teletype, meaning that if you have
each sequence patched to a different voice, you will hear them be hocketed
around.

You may also notice the bright indicators on either side of the hocketing
controls. This denotes the 'offset' of your current hocket. By default this is
0, and shows the indicator at the bottom of the pillars. As you increase the
offset by hocketing 'upward', the indicators will follow. This is handy for
remembering which sequence was the original pattern 1, and so on. If you
increase the hocket offset beyond 3, it will wrap back around to 0, shown by the
indicators returning to the bottom of the pillars.

### Loop Lengths, and Other Manual Modifications

There are some things that you cannot do using the grid at the moment. We
already discussed manually adding pitch sequences, though you can always
generate pitches using the grid controls.

One thing you may want to do is resize your sequences. There is no way to change
the loop length of a sequence using the grid controls, but you can always use
the pattern commands in LIVE mode, or go into the pattern data and set the
start/end points using Shift+S and Shift+E, respectively. Keep in mind that when
you resize a sequence, if the playhead is now outside the new loop range, it
will have to travel to the end of the old loop and go back to the old start
point before it enters the new loop range.

Another thing you may want to do is change your scale using the N.B operator.
This is normally something you do while setting up the sequences, but you can
also just do it during a performance using LIVE mode.

If the generated pitches you get are too low for you, feel free to add an offset
to any CV out, or add some slew. Playing around with the Teletype in LIVE mode
is a great way to add some variation to performances with _Meadowpillars_.

### Conclusion

That's pretty much all you need to get started with this fun little sequencer.
It's quite open-ended, so I hope you have fun making some interesting noise with
this thing. :)
