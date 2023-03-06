# Bosanquet

--------------------------------------------------------------------------------

## What is Bosanquet?

_Bosanquet_ is an experimental isomorphic keyboard for the Teletype, in
conjunction with the Monome Grid. It takes inspiration from controllers like the
[Lumatone keyboard](https://www.lumatone.io/), as well as other keyboards that
use a Bosanquet-Wilson layout.

What 'isomorphic' means is that you can use 'shapes' to more easily and
efficiently recall musical structures, much like you would on the guitar by
moving a hand shape up and down the neck.

## How do I use it?

When you load the scene, the grid will light up to show the Bosanquet layout of
the default temperament (12-TET), and the live mode dashboard will print out
some information about the current keyboard state. Specifically, the dashboard
displays the current temperament, octave offset, Hold Mode state, and Shift Mode
state. Before we start changing temperaments, octaves, and modes, let's go over
how the keyboard layout works.

### Light Levels

The lightest keys (arranged in descending lines of 3 notes, then 4 notes) show
the placement of the closest approximation for the major scale in a particular
temperament. As you progress along the X-axis (left to right), you increase by
one "whole tone". As you progress along the Y-axis (bottom to top), you increase
by one "semitone". More on these values when we get to how we store temperament
information in the Teletype's patterns.

The darker keys denote other tones in the temperament. Some temperaments, such
as 12-TET, only have 2 light levels. Others, such as 19-TET, need 3 or more
light levels to distinguish between various amounts of microtonal offsets. In
this way, you can visually navigate around the keyboard while using the same
"shapes" to create the same musical structures, across many different
temperaments.

Keep in mind that these shapes will vary slightly between some temperaments as
more and more notes are added to the octave, but they will remain consistent
within the same temperament.

### Grid Layout

The actual keyboard takes up the leftmost 15 columns of the grid, and contains
120 playable keys. The single rightmost column of the grid contains various
controls for changing temperaments, octaves, and modes. Let's go over those one
at a time.

### Temperament Controls

The top three buttons of the control column are for switching temperaments.
Hitting the top button will switch to the previous temperament, while hitting
the bottom button will switch to the next. Hitting the middle button will reset
the keyboard back to the default temperament, which is 12-TET. The specific
order of the temperaments is defined by the pattern data where the temperament
information is stored.

Pattern 0 contains the division of the temperament (12 for 12-TET, 19 for
19-TET, and so on); Pattern 1 contains the semitone step for the temperament
(vertical grid interval); Pattern 2 contains the whole tone step for the
temperament (horizontal grid interval).

The numbers you will find if you browse the pattern data for these intervals
represent how many "unit intervals" make up a semitone and whole tone. In this
case a unit interval is just the interval between two adjacent notes in the
octave. In 12-TET, this is 1/12th of an octave, in 19-TET, this is 1/19th of an
octave, and so on.

Using this setup for data storage, we can use the index of the first pattern to
track which temperament we are in, and construct the keyboard layout dynamically
using that information. This is exactly what happens when you load the scene or
switch layouts.

### Octave Controls

Now onto the bottom three buttons of the control column. These are for changing
the octave offset of the Teletype's CV outputs. These function very similarly to
the temperament controls, in that the upper button increases the offset by 1
octave (1 volt), the lower button decreases the offset by 1 octave (1 volt), and
the middle button resets it back to the default (2 volts).

### Mode Controls

The middle two buttons of the control column are where things get fun. These are
our mode controls.

#### Hold Mode

The upper button is a latching button that toggles Hold Mode. By default, Hold
Mode is disabled, which means hitting a key on the keyboard will pulse the
trigger outputs. When you toggle Hold Mode on, the trigger outputs will now act
as gates that remain high as long as you are holding a key.

Regardless of whether Hold Mode is on or off, the number of trigger outs that
will fire is exactly the number of keys being held down on the grid keyboard, up
to a max of 4 (since the Teletype only has 4 trigger outs). This sets us up for
some pseudo-polyphony down the road after we introduce Shift Mode.

#### Shift Mode

The lower button is a latching button that toggles Shift Mode. By default, Shift
Mode is disabled, which means only the first CV output is being used by the
keyboard. When you toggle Shift Mode on, the remaining three CV outs will be set
to their left neighbor's pitch value in a shift register style. The trigger for
the shift is on key press.

By combining these outputs into a single voice, or plugging them into multiple
voices playing simultaneously, you can achieve the pseudo-polyphony that was
mentioned earlier. Of course, you could also toss them into an arpeggiator, or
any number of different musical use cases. The sky's the limit.

If you're looking to create a traditional synthesizer type of setup, just toggle
both Hold Mode and Shift Mode on, and enjoy your four-voice polyphony!

When viewing the dashboard, the mode indicators will show a '0' when the mode is
disabled, and a '1' when it is enabled. You can also tell if a mode is
enabled/disabled by the light level of its toggle button on the grid.

### Notes on Experimental (AKA Broken) Features

Currently, _Bosanquet_ can properly display layouts for a small subset of
tunings, while other tunings with more notes per octave (41-TET, 53-TET, etc.)
don't render their light levels correctly. However, the scene will still
calculate the correct microtonal pitch on key press, so if you don't mind having
to navigate around the keyboard with incorrect light levels, you can feel free
to use these extra tunings.

#### Using Extra Tunings

If you go into the pattern data, you'll notice that the length of pattern 0 is
manually set to limit the available tunings by default. You can increase this
length to include the other tunings that exist below the default ones. You can
also feel free to add your own tunings, as long as they are compatible with this
type of isomorphic layout.

#### Compatible Tunings

This keyboard is meant to be compatible with any Pythagorean-like scale system.
If there is one that you want to use that isn't in the pattern data, you can add
it with the necessary interval information, as described in the section about
the Temperament Controls.

Just don't be disappointed if the light level layout doesn't look right when you
recall the tuning on the keyboard.

### Conclusion

That's pretty much all you need to know before getting your hands dirty and
making some music with this quirky little keyboard. Have fun! :)
