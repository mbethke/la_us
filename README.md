# la_us

## An attempt at a phonetically intuitive Lao keymap for XKB and US keyboards

This is a keymap designed to write Lao on a US keyboard under X11 without
learning a complete new layout such as the STEA one that comes with XKB, but
rather as close to phonetically as possible. 

## What?

If you know the Cyrillic "YAWERTY" keyboard, you know how this works: the "ess"
sound in Russian is a letter that looks like a Latin C (с) but YAWERTY has it
on the "S" key, so you can just type more or less like a word sounds and you
get the right Cyrillic letters. The [Lao Software Windows
keyboards](http://www.laosoftware.com/include/lus.jpg) have a layout in the
same spirit: you have ະ and າ on the "A" key, as that is how they sound; ກ and
ຂ are on "K", and so on.

This keymap is basically the same as the one for Windows with a few differences:

* It's meant primarily for writing Lao, not putting the odd Lao character within
  English text, so Lao is in the main group (i.e. without using the Alt key)
* Combining code points have been left out (e.g. you can't have "am" (ອໍາ) on a
  single key because a single key can only produce a single code point in XKB;
  you have to type it as O-A instead. Those few keys are marked TODO and I
  haven't decided what else to use them for yet.
* As Lao numerals seem to be used less frequently than the Arabic ones, the
  latter are the same as on a US keyboard. With the shift key you get the usual
  punctuation marks while Lao numerals can be reached with the alt key.
  Alt+shift has a few Lao tone marks.


## How?

Unfortunately, there seems to be no way of installing per-user XKB variants
that would show up in common keyboard setup tools such as GNOME's, so you need
to modify the system-wide config files.

The contents of the `la` file need to be appended to
`/usr/share/X11/xkb/symbols/la` (your path may vary depending on where your X11
directory is). Then, look for a section in `/usr/share/X11/xkb/rules/evdev.xml`
(may also vary, use `/usr/share/X11/xkb/rules/base.xml` if you're not using
evdev) that looks like this:

    <layout>
      <configItem>
        <name>la</name>

        <shortDescription>lo</shortDescription>
        <description>Lao</description>
        <languageList>
          <iso639Id>lao</iso639Id>
        </languageList>
      </configItem>
      <variantList>
 
After that, there are usually two sections between `<variant>` tags. Add
another one like this:

    <variant>
      <configItem>
        <name>la-us</name>
        <description>Lao (phonetic US)</description>
        <languageList>
          <iso639Id>lao</iso639Id>
        </languageList>
      </configItem>
    </variant>
 
Make sure the new section sits between the `<variantList>` tags! This should enable a new variant in your keyboard layout chooser.
