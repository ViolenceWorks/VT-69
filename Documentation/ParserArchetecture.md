# Parser Archetecture

The most important subsystem of the VT-69 is the terminal parser. Without it, the VT-69 is a mere keyboard and screen that will display serial data. With it, it 
blah blah blah blah

## State Machine

The parser blah blah blah

## Implementation of the state machine

Blah blah blah

### SGR - Select Graphic Rendition

Of note is the SGR function of the terminal parser. This function changes character attributes on the VT-69 display, allowing for bold characters, characters displayed in reverse video, and eight colors for foreground and background. Like the display caracter array, these attributes are bitpacked into a unit8_t array, where each character may be individually assigned a foreground color, background color, and two attribute bits for bold and reverse video:

![Image of SGR data structure](https://github.com/ViolenceWorks/VT-69/blob/main/Documentation/ArtAssets/GraphicRenditiondatastructre.png)

A 1 in the bold or reverse video positions turns those attributes on, while a 0 turns those attributes off. Color is determined by the combination of bits in the foreground and background colors as such:

Color displayed | Red bit | Green bit | Blue bit
----------------|---------|-----------|---------
Black | 0 | 0 | 0
Red  | 1 | 0 | 0
Blue | 0 | 0 | 1
Green | 0 | 1 | 0
Yellow/Brown | 1 | 1 | 0
Magenta | 1 | 0 | 1
Cyan | 0 | 1 | 1
White | 1 | 1 | 1



