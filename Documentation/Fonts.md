# Fonts

## Capabilities of the font subsystem

The font subsystem is capable of displaying monochrome bitmap fonts, with each character being 10 pixels wide and 20 pixels high.

## Structure of font data

Each individual font is a uint8_t array with a size of 256 by 25. The individual characters are bitpacked into those 256 entries. Fonts are read from the top left to the bottom right, one row at a time; like reading English. An example of bitpacking for the letter 'a' is as follows:

```C
//Character 0x61 =
{
	0x00, 0x00, 0x00, 0x00, 0x00,
	0x00, 0x00, 0x03, 0xf0, 0xfc,
	0x01, 0x80, 0x63, 0xf8, 0xfe,
	0xc0, 0xb0, 0x23, 0xf8, 0xfe,
	0x00, 0x00, 0x00, 0x00, 0x00
},
```

The code used to display the character is as follows:
```C
for(uint16_t i=0; i <= 24; i++)
{
	for(int j=0;j<8;j++)
	{
		if((fontArray[character][i]&(1<<(7-j)))!=0)
		{
			setPixel((fore_Color_High<<8)|fore_Color_Low);	
		}
		else
		{
			setPixel((back_Color_High<<8)|back_Color_Low);
		}
	}
}
```


## Code Page 437

Code page 437 is the default font of the IBM PC and the core of EGA and VGA graphics cards. Also known as 'High ASCII' and 'Extended ASCII', characters 0x80-0xFF in Code page 437 (especially 0xC0-0xDF, 'box characters') are commonly used in Text-based pseudo-GUIs.

![Code Page 437](https://github.com/ViolenceWorks/VT-69/blob/main/Documentation/ArtAssets/CodePage437.png)
