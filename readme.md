# UTFZ

UTFZ is a small utf16 compression library.

## How does it work?

Consider the following utf16 representation of "Hello":

```hex
48 00 65 00 6c 00 6c 00 6f 00
```

You can see that all the odd bytes are `00`.

Consider the following utf16 representation of the Farsi word "سلام":

```hex
33 06 44 06 27 06 45 06
```

You can see that all the odd bytes are `06` and this pattern repeats itself.
These are wasted bytes. The letters of each language, are incrementally one
after another and share the same high byte most of the time. Utfz assumes
the high byte is equal to `00` and changes it every time it encounters a `00`.

With utfz, the first word becomes:

```
48 65 6c 6c 6f
```

and the second one becomes:

```
00 06 33 44 27 45
```

Where the first `00` tells the decoder to set the high byte to `06`. Words of
different languages with different high bytes can be combined:

```
48 65 6c 6c 6f 00 06 33 44 27 45
```

To escape a low byte that equals `00`, utfz adds the current high byte after
the low byte in the sequence.

Check the source code for more info.
