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

However, there are a few inconsistencies and less than practical arrangements in
their keymap so this layout has come out a bit different after a some corpus
research into Lao letter frequencies:

* It's meant primarily for writing Lao, not putting the odd Lao character within
  English text, so Lao is in the main group (i.e. without using the Alt key)
* Keys with multiple combining code points have been left out. E.g. you can't
  have "am" (ອໍາ) on a single key because in XKB a single key can only ever
  produce a single code point; you have to type it as O-A instead. Those few
  keys are marked TODO and I haven't decided what else to use them for yet.
* Long vowels aa, ee, ii and uu are all in non-shifted positions while the short
  ones are shifted. The long ones are significantly more frequent so this helps
  both consistency and typing speed.
* As Lao numerals seem to be used less frequently than the Arabic ones, the
  latter are the same as on a US keyboard. With the shift key you get the Lao
  equivalents; Alt+NUM has a few tone marks and Alt+Shift+NUM gets you the usual
  US keyboard punctuation marks.
* You get mai kan and mai kon on x and z as on Windows, but they are also on
  AltGr-a and AltGr-o because that's where they belong phonetically.
* Tone marks mai ek and mai tho are on 1 and 2 as on Windows, too, but also on q
  and Q respectively for easier access without AltGr. Being very similar
  phonetically, ay and ai both go on the W key.

There is an SVG version of the layout included, based on Wikipedia user
Michka_B's [excellent hand-optimized US
layout](https://commons.wikimedia.org/wiki/File:KB_USA-standard.svg), licensed
under the [GNU FDL](https://gnu.org/licenses/fdl.html)

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
