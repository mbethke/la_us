# la_us

## A phonetically intuitive Lao keymap for XKB and US keyboards

This is a keymap designed to write Lao on a US keyboard under X11 without
learning a complete new layout such as the STEA one that comes with XKB, but
rather as close to phonetically as possible. 

## What?

For someone who doesn't speak Lao natively (and, I suspect, even for those who
do), the organization of keys on the standard Lao STEA keyboard is highly
unintuitive, so without a printed keymap, writing is impossible on an
international keyboard. There have been several proposed keymaps for US
and French keyboards that place Lao characters on the keys with similar-sounding
Latin characters (see [GMSware's LaoWord page for an
overview](http://www.gmsware.org/Common/GMSWord/Contents/Lao/LaoWord-en.htm)),
but none of these are very consistent nor optimized with regard to easy access
to the most frequent characters.

This layout used to be a port of the LaoWord one to XKB but has since been
substantially redesigned using corpus research into Lao letter frequencies.

* It's meant primarily for writing Lao, not putting the odd Lao character within
  English text, so Lao is in the main group (i.e. without using the Alt key)
* Long vowels າ, ເ, ◌ີ and ◌ູ are all in non-shifted positions while the short
  ones are shifted. The long ones are significantly more frequent so this helps
  both consistency and typing speed.
* As Lao numerals are used much less frequently than the Arabic ones, the
  latter are the same as on a US keyboard. With the shift key you get the Lao
  equivalents; Alt+NUM has a few tone marks and Alt+Shift+NUM gets you the usual
  US keyboard punctuation marks.
* You get ◌ັ and ◌ົ on x and z as on Windows, but they are also on
  AltGr-a and AltGr-o because that's where they belong phonetically.
* Tone marks ◌່ and ◌້ are on 1 and 2 as on Windows, but also on q and Q
  respectively for easier access without AltGr. Being very similar phonetically,
  ໄ and ໃ both go on the W key.
* Generally (for letters so/fo/ho/kho), the Sung variant is found in the
  unshifted position and Tam is shifted. Exceptions are Pho and Tho where the
  Tam variant is the more frequent one, so I chose typing speed over consistency
  here.

There is an SVG version of the layout included, based on Wikipedia user
Michka_B's [excellent hand-optimized US
layout](https://commons.wikimedia.org/wiki/File:KB_USA-standard.svg), licensed
under the [GNU FDL](https://gnu.org/licenses/fdl.html)

## How?

### Wayland

Wayland itself doesn't deal with keymap loading but compositors usually
implement their own keymap handling. [Sway](https://github.com/swaywm/sway) can
load custom keymaps with
```
input "type:keyboard" {
    xkb_file ~/.config/xkb/custom
}
```
I have no experience with other compositors, so additions are welcome.

### X11

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
 
Make sure the new section sits between the `<variantList>` tags! This should
enable a new variant in your keyboard layout chooser.

Note: libxkb definitely looks in `~/.xkb/symbols/` as well, so putting the `la`
file there might work. It didn't work for me, but perhaps that's my distro's
problem.
