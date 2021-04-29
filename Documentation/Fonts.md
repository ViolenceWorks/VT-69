# Fonts

## Capabilities of the font subsystem

The font subsystem is capable of displaying monochrome bitmap fonts, with each character being 10 pixels wide and 20 pixels high.

## Structure of font data

![Font Data Structure](https://github.com/ViolenceWorks/VT-69/blob/main/Documentation/ArtAssets/FontDiagram.png)

Each individual font is a uint8_t array with a size of 256 by 25. The individual characters are bitpacked into those 256 entries. Fonts are read from the top left to the bottom right, one row at a time.

The code used to display the character is as follows:
```C
for(uint16_t i=0; i <= 24; i++)
{
	for(int j=0;j<8;j++)
	{
		if((font[character][i]&(1<<(7-j)))!=0)
		{
			setPixel(Foreground);	
		}
		else
		{
			setPixel(Background);
		}
	}
}
```
Generating these bitmap arrays is done with a script, image_to_c_buff.py, found in the folder Scripts.

## Font Rendition Data Structure

Font data, or the options for bold, underscore, reverse video, blink, as well as foreground and background color, are stored as arrays:

![Font Rendition Data Structure](https://github.com/ViolenceWorks/VT-69/blob/main/Documentation/ArtAssets/GraphicRenditiondatastructre.png)

## Code Page 437

Code page 437 is the default font of the IBM PC and the core of EGA and VGA graphics cards. Also known as 'High ASCII' and 'Extended ASCII', characters 0x80-0xFF in Code page 437 (especially 0xC0-0xDF, 'box characters') are commonly used in Text-based pseudo-GUIs.

![Code Page 437](https://github.com/ViolenceWorks/VT-69/blob/main/Documentation/ArtAssets/CodePage437.png)
