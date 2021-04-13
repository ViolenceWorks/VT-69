# Parser Archetecture

The most important subsystem of the VT-69 is the terminal parser. Without it, the VT-69 is a mere keyboard and screen that will display and transmit characters over a serial connection. The parser is what turns a terminal into a useful device.

The VT-69 parser is an ANSI-compliant parser that reads an incoming stream of bytes (either through a serial connection, or optionally from the keyboard subsystem) and transforms the output into the same visible behavior as other ANSI-compliant terminals. 

## State Machine

The parser is effectively a state machine with six states:

* Ground

	The 'default' state. If the parser is in the Ground state it will display nearly all incoming characters. There is one exception to this -- Escape. On reception of the Escape character, the parser will transition to the Escape State.

* Escape Intermediate

	Escape Intermediate is the state entered when an intermediate character, '#' (0x23), '%' (0x25), '(' (0x28), ')' (0x29),or ']' (0x5D), arrives in an escape sequence; there are no paramaters, and the action taken is determined by the intermediate and final characters. These actions are listed in the chapter [Escape Codes](https://github.com/ViolenceWorks/VT-69/blob/main/Documentation/EscCodes.md).

* CSI Entry

	Upon reception of the '[' character (0x5B) while in the Escape State, the terminal will enter the CSI Entry state. This state will alternatively collect paramaters, supported escape codes, or characters that are not part of escape codes. 

	Upon reception of a parameter (0x30-0x39, 0x3B), the parser will transition to the CSI Parameter state.

	Upon reception of a supported escape code, the parser will perform an action dependant on the final character. The function of these actions may be dependant on parameters collected in the CSI Parameter state. These actions are listed in the chapter [Escape Codes](https://github.com/ViolenceWorks/VT-69/blob/main/Documentation/EscCodes.md).

	Upon reception of a character that is neither a parameter nor a supported escape code, the parser will transition to the CSI Ignore state.

* CSI Parameter

	Upon reception of a parameter (0x30-0x39, 0x3B) from the CSI Entry state, the parser will add parameters to a queue. Parameters are collected until a valid CSI Escape code is received, at which time the queue will be transformed into a queue of parameters for parsing inside the CSI Entry state.

* CSI Ignore

	The CSI Ignore state serves to collect malformed Escape Codes.

![Image of parser state machine](https://github.com/ViolenceWorks/VT-69/blob/main/Documentation/ArtAssets/StateMachine.png)


## Implementation of the state machine

The core of the parser is a state machine. Each incoming character is sent to a function representing the current state:
```C
void parseChar(uint8_t character)
{	
	parserState state = currentState;
	switch(state)
	{
		case stateGround:
			groundState(character);
			break;
		case stateESC:
			escState(character);
			break;
		case stateESCinter:
			escIntState(character);
			break;
		case stateCSIentry:
			CSIentryState(character);
			break;
		case stateCSIparam:
			CSIparamState(character);
			break;
		case stateCSIignore:
			CSIignoreState(character);
			break;
	}
}
```
From there, each state acts upon an incoming character according to the state machine. An exception to this rule is the Ground state. This handles all characters, including control characters in the range 0x00-0x1F. Many of these characters do nothing (0x07, BEL, is nonfunctional, because there is no speaker), although some are important. Implemented control characters are as follows:

ASCII Code | Mnemonic | Function
-----------|----------|-----------
0x00 | NUL | Always ignore
0x08 | BS | Backspace
0x09 | TAB | Horizontal Tab
0x0A | LF | Line Feed
0x0B | VT | Vertical Tab
0x0C | FF | Form Feed
0x0D | CR | Carriage Return
0x1B | ESC | Transition to Escape state
0x7F | DEL | Ignored by terminal

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

While this imlementation is limited in that it does not offer complete compatability with Linux console escape codes, it is far more capable than a hardware VT-100. The 'blink' attribute is not implemented, because web browsers got rid of that tag and I'm still salty about it, and the marquee tag is too hard to build, or I don't care enough to do it.



