# liquidcrystal440
enhanced arduino liquidcrystal library

supports character LCDs based on HD44780 designs on the arduino

Modifications to LiquidCrystal for the Arduino

I made several modifications to the LiquidCrystal library module from Arduino17:

40x4 LCDs I added support for an LCD of 4 LInes and 40 characters. I think that if 24x4, 32x4 LCDs exist, they would also work with the software as I have modified it although I have not had the opportunity to test that. The 40x4 LCD (and any HD44780 based LCD with between 81 and 160 characters) will have 2 enable lines. To use an LCD with 4 lines and 40 columns you would declare your LiquidCrystal object as: LiquidCrystal lcd(RS,RW,Enable1,Enable2, data3,data2,data1,data0); at this time I don’t support 8 data lines. (You can pass 255 as the RW item, ground RW and save an Arduino pin.) Then in the setup function you would call: lcd.begin(40,4);

Linewrap When you declare the dimensions of the LCD in your begin call, the LiquidCrystal library remembers how long the lines are. Now when it reaches the end of line 1, text wraps onto line 2 (not line 3 as previously).

println Although print has worked properly in the past, println has not. Now the ‘\r’ and ‘\n’ characters are not sent to the screen as though they were visible characters and the ‘\r’ resets the character position to the top of the next line.

16x4 LCDs The begin statement also correctly positions text at the beginning of the line on 16x4 (and 40x4) LCDs, which were not correctly handled before.

setCursor In the past setCursor selected a location in the HD44780’s RAM not actually a screen location. If you use any of the commands that shift the display left or right with the previous routines, then setCursor and print, text appears in an unexpected location on the screen. With the new software, if you call either scrollDisplayLeft() or scrollDisplayRight(), the LiquidCrystal package keeps track of the relationship between RAM and the LCD so that setCursor coordinates are pegged to a specific spot on the screen, rather than a spot in RAM. The sotware does not handle autoScroll, however. Call home() after autoScroll to restore the expected relationship between setCursor and the LCD screen.
