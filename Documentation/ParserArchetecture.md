# Parser Archetecture

The most important subsystem of the VT-69 is the terminal parser. Without it, the VT-69 is a mere keyboard and screen that will display serial data. With it, it 
blah blah blah blah

## State Machine

The parser blah blah blah

## Implementation of the state machine

Blah blah blah

### SGR - Select Graphic Rendition

Of note is the SGR function of the terminal parser. This function changes character attributes on the VT-69 display, allowing for bold characters, characters displayed in reverse video, and eight colors for foreground and background. Like the display caracter array, these attributes are bitpacked into a unit8_t array, where each character may be individually assigned a foreground color, background color, and two attribute bits for bold and reverse video:

