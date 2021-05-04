# Escape and Control Codes

The purpose of this document is to introduce the concepts of terminal control
through in-band signalling via control characters, escape sequences, and CSI
sequences, as well as provide a listing of control characters, escape, and 
command codes supported by the VT-69.

The VT-69 supports much of the ECMA-48/ISO6429/ANSI X3.64 terminal control 
standards, as well as many private-mode sequences employed by the Linux console
and DEC hardware. It must be noted the VT-69 does not support _all_ terminal
escape codes, as the philosophy of the ANSI X3.64 design committee was something
like this:

> We need a standard for terminal escape codes.
> But all of these escape codes are optional.
> And the implementation is left up to the manufacturer.
> Also manufacturers can make their own private escape codes.
> We're calling this a 'Standard'.


In developing this document, and writing the firmware for which this document
documents, I believe the rise of GUI-based interfaces in the late 1970s and
early 1980s was an attempt to dig us all out of the immense technological debt
incurred during the development of terminals and Text-based User Interfaces.

---

## Definitions
The following listing defines the basic elements of the ANSI 
escape sequences.

Control Sequence Introducer (CSI) -- An escape sequence that provides 
supplementary controls and is itself a prefix affecting the interpretation of a
limited number of contiguous characters. The CSI for the VT-69 is ESC [
(0x1B, 0x5B).

Parameter -- A string of one or more decimal character that represent a single
value. Parameter values are are locked to the range {0 .. 255}; parameter values
above 255 will default to 1.

Numeric Parameter -- A string of characters that represent a number, designated
by Pn. Pn has a range of 0 to 9.

Mnemonic -- Code as described in ANSI X3.64-1979 and DEC VT100 Series Video
Terminal Technical Manual. These mnemonics will are used as internal codes or
functions for the VT-69 where applicable.

---

## Control characters
A control character is in the range 0x00-0x1F and with 8-bit support, 0x80-0x9F.
These characters are acted upon immediately, even in the middle of an escape 
sequence, and the escape sequence continues with the next character.

Mnemonic | Hex Code | Escape Code | Function
---------|----------|-------------|---------
NUL | 0x00 | Null | Null
BEL | 0x07 | ^G | Beeps
BS  | 0x08 | ^H | Backspace one column
HT  | 0x09 | ^I | Goes to the next tab stop
LF  | 0x0A | ^J | Linefeed
VT  | 0x0B | ^K | Linefeed
FF  | 0x0C | ^L | Linefeed
CR  | 0x0D | ^M | Carriage Return
SO  | 0x0E | ^N | Activates G1 character set
SI  | 0x0F | ^O | Activates G0 character set
CAN | 0x18 | ^X | Abort Escape Sequence
SUB | 0x1A | ^Z | Abort Escape Sequence
ESC | 0x1B | ^[ | Start Escape Sequence
DEL | 0x7F | Null | Ignored
CSI | 0x9B | Null | Equivalent to Esc [ in 8-bit mode

---

## ESC Sequences
Escape sequences that are not CSI sequences usually take the form of ESC c,
where c is a character. The most common Escape sequences are listed in the
table below.

Exceptions to the 'two character' escape sequences use an introducer character
either [ , % , ( , ) , > , = , or ]. The meaning and mnemonic of these escape
sequences is as follows:

Escape Code | Mnemonic | Function
------------|----------|------------
Esc [  | CSI | Control sequence Indtroducer
Esc %  | | Start sequence selecting character set
Esc % @ | | Select default (Latin-1, ISO 646)
Esc % G | | Select UTF-8
Esc % 8 | | Select UTF-8 (obsolete)
Esc # 8 | DECALN | DEC screen algnment test
Esc (  | | Start sequence selecting G0 character set
Esc ( B | | Select default (ISO 8859-1)
Esc ( 0 | | Select VT100 graphics mapping
Esc ( U | | Select null mapping - character ROM
Esc )  | | Start sequence selecting G1 character set
Esc ) B | | Select default (ISO 8859-1)
Esc ) 0 | | Select VT100 graphics mapping
Esc ) U | | Select null mapping - character ROM
Esc > | DECPNM | Set numeric keypad mode
Esc = | DECPAM | Set application keypad mode

---

## CSI Sequences
CSI (or ESC [ ) is followed by a sequence of paramaters, at most 20, that
are seperated by semicolons ( ; ). Empty or absent parameters are usually
taken to be 1, however there are exceptions to this. The action of the CSI
sequence is determined by its final character.

A description of the sequences is listed below.

---

## Escape and CSI Sequences and Mnemonics

Mnemonic | Name	| Sequence		
---------|------|----------
SC | Save Cursor Position | Esc 7
RC | Recall Cursor Position | Esc 8
ICH | Insert Character | Esc [ Pn @		
CUU | Cursor up | Esc [ Pn A		
HPR | Horizontal Position Relative | Esc [ pn a		
CUD | Cursor down | Esc [ Pn B		
CUF | Cursor forward (right) | Esc [ Pn C		
DA | Device Attribute | Esc [ c		
RIS | Reset to Initial State | Esc c
IND | Index | Esc D
CUB | Cursor backward (left) | Esc [ Pn D		
VPA | Vertical Position Absolute | Esc [ Pn d		
NEL | Next Line	 | Esc E
CNL | Cursor Next Line | Esc [ Pn E		
VPR | Vertical Position Relative | Esc [ Pn e		
CPL | Cursor Preceding Line | Esc [ Pn F		
HVP | Horizontal Vertical Position | Esc [ Pn f	
CHA | Cursor Horizontal Absolute | Esc [ Pn G		
TBC | Tabulation Clear | Esc [ pn g		
CUP | Cursor Position | Esc [ Pn ; Pn H		
SM | Set Mode | Esc [ pn h		
HTS | Horizontal Tab Set | Esc H
CHT | Cursor Horizontal Tab | Esc [ Pn I		
ED | Erase In Display | Esc [ Ps J		
EL | Erase In Line | Esc [ Ps K		
IL | Insert Lines | Esc [ Ps L		
RM | Reset Mode | Esc [ ps l		
RI | Reverse Index | Esc M
DL | Delete Lines | Etc [ Ps M		
SGR | Select Grapic Rendition | Esc [ Ps m
DSR | Device Status Report | Esc [ ps n
DCH | Delete Character | Esc [ Pn P		
SEM | Select Edit Extent Mode | Esc [ Pn Q		
DECSTBM | Set scrolling region | Esc [ Pn ; Pn r			
SCP | Save Cursor Position | Esc [ s
RCP | Restore Cursor Position | Esc [ u	
CTC | Cursor Tabulation Control | Esc [ Ps W		
ECH | Erase Character | Esc [ Pn X		
CBT | Cursor Backwards Tab | Esc [ Ps Z	
HPA | Horizontal Position Absolute | Esc [ Ps '		

---

## CBT - Cursor Backwards Tab
	Moves the cursor horizontally and backwards to the preceding tabulation
	stop. A parameter of zero or one will indicate the preceding horizontal
	tabulation stop. A larger value (N) will indicate the Nth preceding
	horizontal tabulation stop is desired.

## CHA - Cursor Horizontal Absolute
	Moves the curor forward or backward along the active line to the 
	character position specified by the parameter. A parameter value of 
	zero or one moves the character position to the first character position
	of the active line. A parameter value of N moves the active position to
	character position N of the active line.

## CHT - Cursor Horizontal Tab
	Advance the active position horizontally to the next tabulation stop
	according to the parameter value. A parameter value of zero or one will
	indicate the next horizontal tabulation stope, and a larger value (N) will
	indicate the Nth following horizontal tabulation stop is desired.

## CNL - Cursor New Line
	Moves the cursor to the first position of the next display line or Nth
	following display line depending on the parameter. A parameter value of
	zero or one indicates the next line. A parameter value of N indicates the
	Nth following line.

## CPL - Cursor Preceding Line
	Moves the active position to the first position of the preceding display
	line or Nth preceding display line, depending on the parameter. A parameter
	value of zero or one indicates the preceding line. A parameter value of N
	indicates the Nth preceding line.

## CTC - Cursor Tabulation Control
	Causes one or more tabulation stops to be set or cleared according to the
	parameters. All lines are effected (as we are not dealing with Tabulation
	Stop Mode):

	0 or none	Set a horizontal tabulation stop at the active position
	2		Clear horizontal tabulation at the active position
	5		Clear all horizontal tabulation stops in the device

	All other parameters are ignored.

## CUB - Cursor Backward
	Moves the cursor in the backward direction. The distance moved is
	determined by the parameter. If the parameter value is zero or one,the
	cursor is moved one position. If the parameter value is N, the cursor is 
	moved N positions.

## CUD - Cursor Down
	Moves the cursor in the down direction. The distance moved is determined by
	the parameter. If the parameter value is zero or one, the cursor is moved
	one position. If the parameter value is N, the cursor is moved N positions.

## CUF - Cursor Forward
	Moves the cursor in the forward direction. The distance moved is determined
	by the parameter. If the parameter value is zero or one, the cursor is 
	moved one position. If the parameter value is N, the cursor is moved N 	
	positions.

## CUP - Cursor Postition
	Moves the active position to the position specified by the parameters. 
	This sequence has two parameter values, the first specifing the vertical
	position and the second specifying the horizontal position. A parameter 
	value of zero or one for the first or second parameter moves the active 
	position of the first line or column in the display respectively. The
	default condition with no parameters present is equivalent to a Cursor to
	Home action.

## CUU - Cursor Up
	Moves the cursor in the up direction. The distance moved is determined by
	the parameter. If the parameter value is zero or one, the cursor is moved
	one position. If the parameter value is N, the cursor is moved N positions.

## DA - Device Attribute
	Terminal answers ESC [ ? 6 c -- "I am a VT102"

	This may be changed in regards to the options present on the emulated VT100:

	Option Present			Sequence Sent
	No Options			ESC [ ? 1 ; 0 c
	Processor Option (STP)		ESC [ ? 1 ; 1 c
	Advanced Video Option (AVO)	ESC [ ? 1 ; 2 c
	AVO and STP			ESC [ ? 1 ; 3 c
	Graphics Option (GPO)		ESC [ ? 1 ; 4 c
	GPO and STP			ESC [ ? 1 ; 5 c
	GPO and AVO			ESC [ ? 1 ; 6 c
	GPO, STP, and AVO		ESC [ ? 1 ; 7 c

	It appears that the first parameter is not necessary, therefore all paramaters
	may take the form ESC [ ? (0..7) c.

## DCH - Delete Character
	Deletes the character at the cursor and possibly other adjacent characters
	according to the parameter. The adjacent string of characters, if any, is
	shifted towards the active position. The vacated character positions at the
	other end are erased. A parameter value of zero or one indicates one
	character is removed. A parameter value of N indicates that N characters
	are removed.

	The extent (display, line) of the string affected is determined by the
	setting of the Editing Extent Mode (see SEM).

## DECSTBM - Set Scrolling Region
	This control sets the values of the top and bottom margins of the scrolling
	region. The new settings are determined by the parameter values; first
	parameter sets the value of the top margin, second parameter sets the value
	of the bottom margin. Default values if either or both parameters are
	omitted are 1 for the top margin and 24 for the bottom margin.
	
	### Format:
	Esc [ Pt ; Pb r
	defualt Pt: 1
	default Pb: last line (24)
	
	Notes:
	Execution of this contro causes the active position to be set to the page
	origin obeying the Origin Mode (DECOM) : First column of the first line if
	Origin Mode is in the reset (Absolute) state; Top and Lef margins if the 
	Origin Mode is in the set (Displaced) state.
	
	If the value specified for the Top Margin is equal to or greater than the 
	value specified for the Bottom Margin, this control will be ignored and 
	not executed.
	
	If the value specified for the Bottom Margin is greater than the number 
	of lines in the Logical Display Page, this control will be ignored and '
	not executed.

## DL - Delete Lines
	Deletes the indicated number of lines, inclusive of current line. Default
	numberof lines is 1.

## DSR - Device Status Report
	
	Esc [ 5 n
		Device Status Report (DSR): Answer is ESC [ 0 n (Terminal OK)

	Esc [ 6 n
		Cursor Position Report (CPR): Answer is Esc [ y ; x R, where
		x,y is the cursor location.

## ECH - Erase Character
	Erases the character at the cursor and possibly other following characters,
	according to the parameter. The position of the cursor is unchanged. A
	numeric parameter of zero or one indicates that one character is erased. A
	numeric parameter of N indicates that N characters are erased.

## EL - Erase In Line
	Erases line, depending on parameter:

	0 			Erase from cursor to end of line
	1			Erase from beginning of line to cursor
	2 or none		Erase entire line containing cursor

	Any other parameters are ignored.

## ED - Erase In Display
	Erases display, depending on parameter:

	0 			Erase from cursor to end of screen
	1			Erase from beginning of screen to cursor
	2 or none		Erase entire screen

	Any other parameters are ignored.

## HPA - Horizontal Position Absolute
	Moves the cursor to the indicated column on the current row, default 1.

## HPR -  Horizontal Position Relative
	Moves the cursor right the indicated # of columns, default 1.

## HTS - Horizontal Tab Set
	Sets one horizontal tabulation stop at the cursor.

## HVP - Horizontal Vertical Position
	Move cursor to the indicated row, column

## ICH - Insert Character
	Inserts the indicated number of blank characters.

## IL - Insert indicated number of lines
	Inserts the indicated number of blank lines, defualt number of 1.

## IND - Index
	Causes the cursor to move downward one line without changing the horizontal
	position.

## NEL - Next Line
	Causes the cursor to move to the first position on the next line downward.

## RC - Restore Cursor Position
	Restores the cursor to the position saved with SC. If no cursor position 
	has been saved, the default is the first character position of the first
	line.

## RCP - Restore Cursor Position
	Mirror of RC, included for ANSI.SYS compatability.

	All parameters are ignored.

## RI - Reverse Index
	Moves the cursor to the same horizontal position on the preceding line.

## RIS - Reset to Initial State
	Resets a device to its initial state, that is, the state it has after it is
	switched on. This affects resetting of all tabulation, reset graphic
	rendition, erasure of all positions, and moving the cursor to the first
	character position of the first line.

## RM - Reset Mode
	Performs the inverse of Set Mode (see below).

## SC - Save Cursor Position
	Saves cursor position.

## SCP - Save Cursor Position
	Mirror of SC, included for ANSI.SYS compatability.

	All parameters are ignored.


## SEM - Select Edit Extent Mode
	Selects the extent of the display to be affected by the Insert Character 
	(ICH) and Delete Character (DCH) actions according to the parameter:

	0 or none	Edit in Display
	1		Edit in Line

	Any other parameters are ignored.

## SGR - Select Graphic Rendition
	PS refers to a selective parameter. Multiple parameters may be seperated by
	the semicolon character (Esc [ Ps;Ps;Ps m). An empty parameter (between
	semicolons or string initiator or terminator) is interpreted as zero. The 
	parameters are executed in order and have the following meanings:

	0 or none		All attributes off
	1			Bold on
	2			Set half-bright
	4			Set underscore
	5			Set blink
	7			Reverse video on
	21			Set underline
	22			Bold off
	24			Underline off
	25			Blink off
	27			Reverse video off
	30			Set black foreground
	31			Set red foreground
	32			Set green foreground
	33			Set brown foreground
	34			Set blue foreground
	35			Set magenta foreground
	36			Set cyan foreground
	37			Set white foreground
	38			256/24 bit foreground color (see below)
	39			Set default foregrougn color (white)
	40			Set black background
	41			Set red background
	42			Set green background
	43			Set brown background
	44			Set blue background
	45			Set magenta background
	46			Set cyan background
	47			Set white background
	48			256/24 bit background color (see below)
	49			Set default background color (black)
				
	Any other parameters are ignored.

	Commands 38 and 48 require further arguments:

	ESC [ 38 ; 5 ; x m: 256 color, where x values 0..15 are IBGR (black, red,
	green, ... white), 16..231 a 6x6x6 color cube, 232..255 a grayscale ramp.
	These 24-bit colors (8-8-8 r/g/b) are compressed into 16-bit values,
	(5-6-5 r/g/b) defined by the uint16 array below:
	
```C
uint16_t eightBitColor[256] = 
{
	0x0000,0x8000,0x0400,0x8400,0x0010,0x8010,0x0410,0xC618,0x8410,0xF800,
	0x07E0,0xFFE0,0x001F,0xF81F,0x07FF,0xFFFF,0x0000,0x000B,0x0010,0x0015,
	0x001A,0x001F,0x02E0,0x02EB,0x001D,0x02F5,0x02FA,0x02FF,0x0420,0x042B,
	0x0430,0x0435,0x043A,0x043F,0x0560,0x056B,0x0570,0x0575,0x057A,0x057F,
	0x06A0,0x06AB,0x06B0,0x06B5,0x06BA,0x06BF,0x07E0,0x07EB,0x07F0,0x07F5,
	0x07FA,0x07FF,0x5800,0x580B,0x5810,0x5815,0x581A,0x581F,0x5AE0,0x5AEB,
	0x5AF0,0x5AF5,0x5AFA,0x5AFF,0x5C20,0x5C2B,0x5C30,0x5C35,0x5C3A,0x5C3F,
	0x5D60,0x5D6B,0x5D70,0x5D75,0x5D7A,0x5D7F,0x5EA0,0x5EAB,0x5EB0,0x5EB5,
	0x5EBA,0x5EBF,0x5FE0,0x5FEB,0x5FF0,0x5FF5,0x5FFA,0x5FFF,0x8000,0x800B,
	0x8010,0x8015,0x801A,0x801F,0x82E0,0x82EB,0x82F0,0x82F5,0x82FA,0x82FF,
	0x8420,0x842B,0x8430,0x8435,0x843A,0x843F,0x8560,0x856B,0x8570,0x8575,
	0x857A,0x857F,0x86A0,0x86AB,0x86B0,0x86B5,0x86BA,0x86BF,0x87E0,0x87EB,
	0x87F0,0x87F5,0x87FA,0x87FF,0xA800,0xA80B,0xA810,0xA815,0xA81A,0xA81F,
	0xAAE0,0xAAEB,0xAAF0,0xAAF5,0xAAFA,0xAAFF,0xAC20,0xAC2B,0xAC30,0x0566,
	0xAC3A,0xAC3F,0xAD60,0xAD6B,0xAD70,0xAD75,0xAD7A,0xAD7F,0xAEA0,0xAEAB,
	0xAEB0,0xAEB5,0xAEBA,0xAEBF,0xAFE0,0xAFEB,0xAFF0,0xAFF5,0xAFFA,0xAFFF,
	0xD000,0xD00B,0xD010,0xD015,0xD01A,0xD01F,0xD2E0,0xD2EB,0xD2F0,0xD2F5,
	0xD2FA,0xD2FF,0xD420,0xD42B,0xD430,0xD435,0xD43A,0xD43F,0xD560,0xD56B,
	0xD570,0xD575,0xD57A,0xD57F,0xD6A0,0xD6AB,0xD6B0,0xD6B5,0xD6BA,0xD6BF,
	0xD7E0,0xD7EB,0xD7F0,0xD7F5,0xD7FA,0xD7FF,0xF800,0xF80B,0xF810,0xF815,
	0xF81A,0xF81F,0xFAE0,0xFAEB,0xFAF0,0xFAF5,0xFAFA,0xFAFF,0xFC20,0xFC2B,
	0xFC30,0xFC35,0xFC3A,0xFC3F,0xFD60,0xFD6B,0xFD70,0xFD75,0xFD7A,0xFD7F,
	0xFEA0,0xFEAB,0xFEB0,0xFEB5,0xFEBA,0xFEBF,0xFFE0,0xFFEB,0xFFF0,0xFFF5,
	0xFFFA,0xFFFF,0x0841,0x1082,0x18E3,0x2124,0x3186,0x39C7,0x4228,0x4A69,
	0x5ACB,0x630C,0x632C,0x73AE,0x8410,0x8C51,0x94B2,0x9CF3,0xAD55,0xB596,
	0xBDF7,0xC638,0xD69A,0xDEDB,0xE73C,0xEF7D
};
```

	ESC [ 38 ; 2 ; r ; g ; b m: 24-bit color, r/g/b/ components are in the range
	0..255. These 24-bit values are compressed into 16-bit colors (5-6-5 r/g/b)
	by the following algorithm:
	
```C
result = ((r & 0xf8) << 8) | ((g & 0xfc) << 3) | (b >> 3);
```

	

## SM - Set Mode

	### ECMA-48 Mode Switches
	
	Esc [ 4 h
		DECIM (default off): Set Insert Mode.

	Esc [ 20 h
		LF/NL (default off): Automatically follow echo of LF, VT, or FF
		with CR.

	### DEC Private Mode (DECSET/DECRST) Mode Switches

	ESC [ ? 1 h
		DECCKM (default off): When set, the cursor keys send an ESC 0 prefix,
		rather than ESC [.

	ESC [ ? 5 h
		DECSCNM (defualt off): Set reverse video mode.

	ESC [ ? 6 h
		DECOM (defualt off): When set, cursor addressing is relative to the
		upper left corner of the scrolling region.

	ESC [ ? 7 h
		DECAWM (default on): Set autowrap on. In this mode, a graphic character
		emitted after column 80 forces a wrap to the beginning of the following
		line.

	ESC [ ? 8 h
		DECARM (default on): Set keyboard autorepeat on.

	ESC [ ? 25 h
		DECTECM (default on): Make cursor visible.

## TBC - Tabulation Clear
	Clears tab stop according to the following parameters:

	0 or none	Tabulation clear at current column
	3		Tabulation clear (all tabs)

	Any other parameters are ignored.

## VPA - Vertical Position Absolute
	Move fursor down to the indicated row, current column.

## VPR - Vertical Position Relative
	Move the cursor down the indicated number of rows.

---

# Bibliography / References

1. Article 4810 of alt.hackers from Dom@sound.demon.co.uk (The Dark Stranger)
	http://user.it.uu.se/~andersb/kurs/dark/mn97/VT100codes.html

2. Linux man page for console_codes(4)
	https://man7.org/linux/man-pages/man4/console_codes.4.html

3. "Towards Standardized Video Terminals: ANSI X3.64 Device Control" Mark L.
	Siegel, BYTE Magazine April 1984, pp365-374.

4. "A parser for DEC’s ANSI-compatible video terminals" by Paul Flow William
	vt100.net/emu/dec_ansi_parser

5. VT100 Series Video Terminal Technical Manual (EK-VT100-TM-003) 
	Appendix A and D

6. ANSI Code Extension ANSI Standard X3.64-1974, X3.64-1979 
	(ISO 2022 and 6429)
	
7. DEC STD 070 Video Systems Reference Manual (A-MN-ELSM070-00-0000
	Rev H, 03-Dec-1991), Digital Equipment Corporation

EOF
