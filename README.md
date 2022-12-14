# Elfos-TMS9X18-Library
This code provides a library for the [Elf/OS TMS9X18 Video Driver](https://github.com/fourstix/Elfos-TMS9X18-Driver) API.  This library can be linked with code like the [Elfos-TMS9118-Demos](https://github.com/fourstix/Elfos-TMS9118-Demos) to access TMS9X18 Video functions independent of the platform hardware such as the [1802-Mini TMS9x18 Video Card](https://github.com/dmadole/1802-Mini-9918-Video) by David Madole or the [Pico/Elf v2 TMS9118/9918 Color Video card](https://web.archive.org/web/20220918120827/http://www.elf-emulation.com/hardware.html) by Mike Riley.  The source files for this library were assembled into object files using the [Asm/02 1802 Assembler](https://github.com/rileym65/Asm-02) by Mike Riley.

Platform  
--------
This code written with this library should run on any platform with the Elf/OS TMS9X18 Video Driver loaded, such as the  [1802-Mini](https://github.com/dmadole/1802-Mini) with the [1802-Mini TMS9918 Video Card](https://github.com/dmadole/1802-Mini-9918-Video) created by David Madole or a [Pico/Elf v2](http://www.elf-emulation.com/hardware.html) created by Mike Riley with the [TMS9118/9918 Color Board](https://web.archive.org/web/20220918120827/http://www.elf-emulation.com/hardware/tms9918.png) or the [Pico/Elf TMS9918 Video IO card](https://github.com/fourstix/PicoElfTMS9118VIOCard). A lot of information and software for the 1802-Mini and the Pico/Elf v2 can be found in the [COSMAC ELF Group](https://groups.io/g/cosmacelf) at groups.io.

Examples
----------
Please see the [Elfos-9118-Demos](https://github.com/fourstix/Elfos-TMS9118-Demos) repository on GitHub for example code that uses this library.

TMS9X18 Video Library API
-------------------------
These API functions support Graphics II Mode for images and Text Mode for character output. They use R7 and R8 
as scratch registers, and some use R9 as well.

**vdp_g2  -- Graphics II Mode API for the TMS9X18 Video Library**
<table>
<tr><th>API Name</th><th colspan="2">Description</th><th>Parameter 1</th><th>Parameter 2</th><th>Notes</th></tr>
<tr><td>beginG2Mode</td><td colspan="2">Set up video card to draw an image in Graphics II mode.</td><td colspan="2">(None)</td><td>Sets group, clears memory and initializes video card.</td></tr>
<tr><td>endG2Mode</td><td colspan="2">Reset the video card, if desired, and then reset the expansion group back to default.</td><td colspan="2">D = V_VDP_KEEP to keep display, or D = V_VDP_RESET to reset the video card.</td><td>Resetting the video card turns off interrupts and clears the display.</td></tr>
<tr><td>updateG2Mode</td><td colspan="2">Update the video card to display an image in Graphics II mode.</td><td colspan="2">(None)</td><td>Writes to the VR0 register in the video card to cause an update.</td></tr>
<tr><td>sendBitmap</td><td colspan="2">Send bitmap data to VDP Pattern Table.</td><td colspan="2">dw ptr  (inlined) to bitmap data buffer.</td><td>The bitmap data size should be 6144 bytes.</td></tr>
<tr><td>sendColors</td><td colspan="2">Send  color map data to VDP Color Table.</td><td colspan="2">dw ptr  (inlined) to color map data buffer.</td><td>The color map data size should be 6144 bytes.</td></tr>
<tr><td>setBackground</td><td colspan="2">Fill VDP Color table with single background color</td><td colspan="2">D = background color byte.</td>&nbsp;<td></tr>
<tr><td>sendNames</td><td colspan="2">Set name table entries for Graphics II mode.</td><td colspan="2">(None)</td><td>Fills Names Table with triplet series 0..255, 0..255, 0..255.</td></tr>
<tr><td>setSpritePattern</td><td colspan="2">Send sprite pixel pattern data to VDP Sprite Patterns Table.</td><td >dw ptr (inlined) to sprite pattern buffer</td><td >dw size (inlined) of sprite pattern</td><td>&nbsp;</td></tr>
<tr><td>setSpriteData</td><td colspan="2">Send sprite attributes data to VDP Sprite Attribures Table.</td><td >dw ptr (inlined) to sprite attributes buffer</td><td >dw size (inlined) of sprite attributes</td><td>&nbsp;</td></tr>
<tr><td>sendBmapData</td><td colspan="2">Send file bitmap data to VDP Pattern Table.</td><td >dw ptr (inlined) to bitmap data buffer</td><td >dw ptr (inlined) to data size field in header buffer</td><td>Useful when reading bitmap data from a Sun Raster image file.</td></tr>
<tr><td>sendRleBmapData</td><td colspan="2">Send Sun RLE bitmap data to VDP Pattern Table.</td><td >dw ptr (inlined) to RLE compressed bitmap data buffer</td><td >dw ptr (inlined) to compressed data size field in header buffer</td><td>Useful when reading bitmap data a from Sun Raster image file.</td></tr>
<tr><td>sendCmapData</td><td colspan="2">Send file color map data to VDP Color Table.</td><td >dw ptr (inlined) to color map data buffer</td><td >dw ptr (inlined) to color map size field in header buffer</td><td>Useful when reading color map data from a Sun Raster image file.</td></tr>
<tr><td>sendRleCmapData</td><td colspan="2">Send Sun RLE color map data to VDP Color Table.</td><td >dw ptr (inlined) to RLE compressed color map data buffer</td><td >dw ptr (inlined) to compressed color map size field in header buffer</td><td>Useful when reading color map data a from Sun Raster image file.</td></tr>
<tr><td>setG2CharXY</td><td colspan="2">Set Graphics 2 Mode x,y position from character co-ordinates (1..31,0..23)</td><td>R9.0 = x byte</td><td>R9.1 = y byte</td><td>Sets the x,y position used by drawG2String and drawG2Char.</td></tr>
<tr><td>getG2CharXY</td><td colspan="2">Get Graphics 2 Mode x,y position from character co-ordinates (1..31,0..23)</td><td colspan="2">(None)</td><td>Returns the x,y position in R9 with R9.0 = x byte, R9.1 = y byte</td></tr>
<tr><td>updateG2Idx</td><td colspan="2">Update Graphics 2 position index with offset</td><td colspan="2">R9 = offset to add to current position index</td><td>If the new position exceeds the maximum buffer size, the index wraps around to the top.</td></tr>
<tr><td>setG2BmapAddr</td><td colspan="2">Set Graphics 2 bitmap address from position index.</td><td colspan="2">(None)</td><td>R7 and R8 are used as scratch registers for calculating address</td></tr>
<tr><td>setG2CmapAddr</td><td colspan="2">Set Graphics 2 color map address from position index.</td><td colspan="2">(None)</td><td>R7 and R8 are used as scratch registers for calculating address</td></tr>
<tr><td>putG2Char</td><td colspan="2">Put a character at the current x,y position in graphics II mode.</td><td colspan="2">D contains the character to draw</td><td>Used by drawG2Char and drawG2ColorChar.</td></tr>
<tr><td>putG2String</td><td colspan="2">Put a character at the current x,y position in graphics II mode.</td><td colspan="2">Rf points to a null terminated string</td><td>Used by drawG2String and drawG2ColorString. R9 contains the byte count.</td></tr>
<tr><td>drawG2String</td><td colspan="2">Draw a text string at the current x,y position in graphics II mode.</td><td colspan="2" >RF - pointer to null terminated string</td><td>Draws text at the x,y position set by getG2CharXY.</td></tr>
<tr><td>drawG2ColorString</td><td colspan="2">Draw a text string at the current x,y position in graphics II mode.</td><td>RF - pointer to null terminated string</td><td> D contains the color byte</td><td>Draws text in color at the x,y position set by getG2CharXY.</td></tr>
<tr><td>drawG2Char</td><td colspan="2">Draw a character at the current x,y position in graphics II mode.</td><td colspan="2" >D contains the character to draw</td><td>Draws a character at the x,y position set by getG2CharXY.</td></tr>
<tr><td>drawG2ColorChar</td><td colspan="2">Draw a character in color at the current x,y position in graphics II mode.</td><td colspan="2">D contains the character to draw</td><td>Draws character at the x,y position set by getG2CharXY using the color in the user info set by setColor.</td></tr>
<tr><td>setColor</td><td colspan="2">Set color in user info.</td><td colspan="2">D contains the color byte to set as current color</td><td>Set the color byte in R8.0 and save as user info.</td></tr>
<tr><td>resetColor</td><td colspan="2">Reset the current color to the default value from user info.</td><td colspan="2">(None)</td><td>Set the color byte in R8.0 with the default value in R8.1 from the user info.</td></tr>
<tr><td>invertColor</td><td colspan="2">Swap the foreground and background values for the current color in user info.</td><td colspan="2">(None)</td><td>Swap the foreground and background values for the current color byte in R8.0 and save as user info.</td></tr>
<tr><td>getInfo</td><td colspan="2">Get the data values from the user info.</td><td colspan="2">(None)</td><td>Set R7 and R8 to the values in the user info.</td></tr>
<tr><td>setInfo</td><td colspan="2">Save data values in the user info.</td><td>R7 (position index)</td><td>R8 (R8.1 = default color byte and R8.0 = current color byte)</td><td>Save the values in R7 and R8 in the user info.</td></tr>
<tr><td>clearInfo</td><td colspan="2">Set the data values from the user info to zero.</td><td colspan="2">(None)</td><td>Clears the values in the user info.</td></tr>
<tr><td>blankG2Line</td><td colspan="2">Erase a line of characters in graphics II mode.</td><td colspan="2">(None)</td><td>Each line has 32 characters of 8 bytes each. This function does not change the position index.</td></tr>
<tr><td>blankG2Screen</td><td colspan="2">Erase the entire character screen in graphics II mode and set the position to home.</td><td colspan="2">(None)</td><td>Each screen has 24 lines of 32 characters. This function sets the position index to zero (home).</td></tr>
</table>

**vdp_text -- Text Mode API for the TMS9X18 Video Library**

<table>
<tr><th>API Name</th><th colspan="2">Description</th><th>Parameter 1</th><th>Parameter 2</th><th>Notes</th></tr>
<tr><td>beginTextMode</td><td colspan="2">Set up video card to write characters in Text mode.</td><td colspan="2">(None)</td><td>Sets group, clears memory and initializes video card.</td></tr>
<tr><td>endTextMode</td><td colspan="2">Reset the video card, if desired, and then reset the expansion group back to default.</td><td colspan="2">D = V_VDP_KEEP to keep display, or D = V_VDP_RESET to reset the video card.</td><td>Resetting the video card turns off interrupts and clears the display.</td></tr>
<tr><td>setTextColor</td><td colspan="2">Set the text foreground and background color.</td><td colspan="2"> D = text color as byte (foreground color,background color)</td><td>Each color value is 4 bits, 0x0 to 0xF.</td></tr>
<tr><td>setTextCharXY</td><td colspan="2">Position the text cursor to x,y for writing subsequent text.</td><td colspan="2" > db x, db y (inlined) x,y cursor co-ordinates</td><td>Sets the x,y position used by writeTextString.</td></tr>
<tr><td>writeTextString</td><td colspan="2">Write a null-terminated text string at the cursor position.</td><td colspan="2" >RF - pointer to null terminated string</td><td>Write text at the x,y cursor position set by setTextCharXY.</td></tr>
</table>

Other TMS9X18 Video Library Functions
-------------------------------------
These library functions are mainly used internally by the API functions, but they are available for user programs.

**vdp_base -- Base functions used by the TMS9X18 Video Library API**
<table>
<tr><th>API Name</th><th colspan="2">Description</th><th>Parameter 1</th><th>Parameter 2</th><th>Notes</th></tr>
<tr><td>checkVideo</td><td colspan="2">Check to see if the video driver is loaded.</td><td colspan="2">(None)</td><td>Returns DF = 0 if loaded, DF = 1 if not loaded.</tr>
<tr><td>setAddress</td><td colspan="2">Set the VDP address with the inlined value</td><td colspan="2">dw Address (inlined)</td>&nbsp;<td></tr>
<tr><td>readStatus</td><td colspan="2">Read VDP status byte.</td><td colspan="2">(None)</td><td>Returns D = VDP Status byte.</tr>
<tr><td>setGroup</td><td colspan="2">Set expansion group for video card.</td><td colspan="2">(None)</td><td>Always call this function before communication with the video card begins. It does nothing if group for the video card is defined as "none".</tr>
<tr><td>resetGroup</td><td colspan="2">Reset expansion group back to the default value.</td><td colspan="2">(None)</td><td>Always call this function after communication with the video card ends. It does nothing if group for the video card is defined as "none".</tr>
</table>

**vdp_charset -- Character Set font used by other functions in the TMS9X18 Video Library**
<table>
<tr><th>API Name</th><th colspan="2">Description</th><th colspan="3">Notes</th></tr>
<tr><td>VDP_CHARSET</td><td colspan="2">Font definitions for TMS9x18 character set.</td><td colspan="3">Default characters set font is CP437, but it can be assembled to use the smaller TI99 font by changing the font definition in charset.inc include file.  The character set size is defined in charset.inc as VDP_CHARSET_SIZE.</td></tr>
</table>

**vdp_math -- Math primitives used by other functions in the TMS9X18 Video Library**
<table>
<tr><th>API Name</th><th colspan="2">Description</th><th>Parameter 1</th><th>Parameter 2</th><th>Notes</th></tr>
<tr><td>ADD16</td><td colspan="2">Add to 16 bit integers.</td><td>R7, 16 bit addend.</td></td><td>R8, 16 bit addend.</td><td>Returns R7 with the 16-bit sum of R7+R8, DF = 1 indicates overflow.</tr>
<tr><td>MULT8</td><td colspan="2">Multiply to 8-bit values.</td><td colspan="2">R8.1 contains the 8-bit multiplier, R8.0 contains the 8-bit multiplicand.</td><td>Returns R7 with the 16-bit product of byte R8.0 x byte R8.1.</td></tr>
</table>

**vdp_init -- Initialization functions for the TMS9X18 Video Library API**
<table>
<tr><th>API Name</th><th colspan="2">Description</th><th>Parameter 1</th><th>Parameter 2</th><th>Notes</th></tr>
<tr><td>clearMem</td><td colspan="2">Clear the VDP VRAM memory.</td><td colspan="2">(None)</td><td>Clears all 16K bytes of VDP memory.</tr>
<tr><td>initRegs</td><td colspan="2">Initialize the 8 VDP registers.</td><td colspan="2">dw ptr (inlined) to 8-byte vreg data buffer.</td>&nbsp;<td></tr>
<tr><td>intCharset</td><td colspan="2">Initialize character set data in the text Pattern Table.</td><td colspan="2">(None)</td><td>Used in Text Mode to load the character set font data defined in vdp_charset.</tr>
</table>

TMS9X18 Text Mode Memory Map
----------------------------
<table>
<tr><th>Address</th><th colspan="2">Description</th><th>Notes</th></tr>
<tr><td>0000h</td><td colspan="2">Pattern Table</td><td>256 8-byte font codes, 2048 bytes</td></tr>
<tr><td>0800h</td><td colspan="2">Names Table</td><td>40x24 characters, 960 bytes</td></tr>
<tr><td>0BC0h</td><td rowspan="2" colspan="2">Unused</td><td rowspan="2">&nbsp;</td></tr>
<tr><td>3FFFh</td></td></tr>
</table>

TMS9X18 Graphics II Mode Memory Map
----------------------------
<table>
<tr><th>Address</th><th colspan="2">Description</th><th>Size</th></tr>
<tr><td>0000h</td><td colspan="2">Pattern Table</td><td>6144 bytes</td></tr>
<tr><td>1800h</td><td colspan="2">Sprite Pattern Table</td><td>512 bytes</td></tr>
<tr><td>2000h</td><td colspan="2">Color Table</td><td>6144 bytes</td></tr>
<tr><td>3800h</td><td colspan="2">Names Table</td><td>768 bytes</td></tr>
<tr><td>3B00h</td><td colspan="2">Sprite Attributes Table</td><td>256 bytes</td></tr>
<tr><td>3C00h</td><td rowspan="2" colspan="2">Unused</td><td rowspan="2">&nbsp;</td></tr>
<tr><td>3FFFh</td></td></tr>
</table>

TMS9X18 Color Values 
--------------------
<table>
<tr><th>Hex value</th><th colspan="2">Color</th><th>Hex value</th><th colspan="2">Color</th></tr>
<tr><td>0</td><td colspan="2">Transparent</td><td>8</td><td colspan="2">Red</td></tr>
<tr><td>1</td><td colspan="2">Black</td><td>9</td><td colspan="2">Light Red</td></tr>
<tr><td>2</td><td colspan="2">Green</td><td>A</td><td colspan="2">Yellow</td></tr>
<tr><td>3</td><td colspan="2">Light Green</td><td>B</td><td colspan="2">Light Yellow</td></tr>
<tr><td>4</td><td colspan="2">Blue</td><td>C</td><td colspan="2">Dark Green</td></tr>
<tr><td>5</td><td colspan="2">Light Blue</td><td>D</td><td colspan="2">Magenta</td></tr>
<tr><td>6</td><td colspan="2">Dark Red</td><td>E</td><td colspan="2">Gray</td></tr>
<tr><td>7</td><td colspan="2">Cyan</td><td>F</td><td colspan="2">White</td></tr>
</table>
Color byte is ffffbbbb where ffff is the 4-bit foreground hex value and bbbb is 
the 4-bit background hex value.  For example f1h is white foreground on a black background. 

Repository Contents
-------------------
* **/src/**  -- Source files for the TMS9X18 Video Library.
  * asm.bat - Windows batch file to assemble source files with Asm/02 to create object files. Use the command *asm vdp_base.asm* to assemble the source file.  Replace *[YOUR_PATH]* with the path to the Asm/02 directory.
  * vdp_base.asm - Base library functions to access the TMS9X18 Video Driver.
  * vdp_charset.asm - Character set data used by the TMS9X18 Video Library.
  * vdp_init.asm - Initialization functions used by the TMS9X18 Video Library.
  * vdp_math.asm - Math primitives used by the TMS9X18 Video Library.
  * vdp_g2.asm - Graphics Mode II functions provided by the TMS9X18 Video Library.
  * vdp_text.asm - Text Mode functions provided by the TMS9X19 Video Library.
* **/src/include/**  -- Included source files for Elf/OS TMS9X18 Video Driver.
  * ops.inc - Opcode definitions for Asm/02.
  * bios.inc - Bios definitions from Elf/OS
  * kernel.inc - Kernel definitions from Elf/OS
  * charset.inc - Character set definitions for the library. The default is CP437 font, but the code can be re-assembled for the smaller TI99 font.
  * vdp.inc - API constants and useful VDP constants to be used by programs calling the driver.
* **/obj/**  -- Object files for TMS9X18 video driver.
  * build.bat - Windows batch file to create the vdp_video.lib library file from the object files. Use the command *build* to concatenate the object files into the library file.
  * vdp_base.prg - Object file with base library functions to access the TMS9X18 Video Driver.
  * vdp_charset.prg - Object file with CP437 character set data used by the TMS9X18 Video Library.
  * vdp_init.prg - Object file with initialization functions used by the TMS9X18 Video Library.
  * vdp_math.prg - Object file with math primitives used by the TMS9X18 Video Library.
  * vdp_g2.prg - Graphics Mode II functions provided by the TMS9X18 Video Library.
  * vdp_text.prg - Text Mode functions provided by the TMS9X19 Video Library.
* **/lib/**  -- Library files for TMS9X18 video driver.
  * vdp_video.lib - Library file assembled with the CP437 character set font.
  * vdp_video_ti99.lib - Alternative library file assembled with the TI99 character set font.


License Information
-------------------

This code is public domain under the MIT License, but please buy me a beer
if you use this and we meet someday (Beerware).

References to any products, programs or services do not imply
that they will be available in all countries in which their respective owner operates.

Other company, product, or services names may be trademarks or services marks of others.

All libraries used in this code are copyright their respective authors.

The 1802-Mini Hardware
Copyright (c) 2022-2022 by David Madole

The Pico/Elf v2 1802 microcomputer hardware and software
Copyright (c) 2004-2022 by Mike Riley.

Elf/OS and RcAsm Software
Copyright (c) 2004-2022 by Mike Riley.

Many thanks to the original authors for making their designs and code available as open source.

This code, firmware, and software is released under the [MIT License](http://opensource.org/licenses/MIT).

The MIT License (MIT)

Copyright (c) 2022 by Gaston Williams

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

**THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.**
