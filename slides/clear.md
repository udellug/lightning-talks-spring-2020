# Clear
### Providing clarity regarding the workings of a clearly useful command...transparent...\<pun\>
#### By: J. Reynolds

---
## What is clear?
`clear` is a command that is used to remove all previous commands and output from consoles and terminal windows on unix-like operating systems <sup>1</sup> A similar effect can be achieved by pressing `ctrl-l`. This will remove previous lines but keep the current line.


## What is it *realllllly*
Let's have a look. if we look at the hexdump by preforming `clear |hexdump`, we see the following output:

        0000000 5b1b 1b48 325b 1b4a 335b 004a
        000000b

And just for the lulz, if we run `echo -e "\x1b\x5b\x48\x1b\x5b\x32\x4a"` we see that it will in fact clear the screen. *Weird*...

The deal here is that these are terminal control escape sequences. While these may vary from terminal to terminal, most use ANSI control sequences.

In this case our sequnece is `1b 5b 48 1b 5b 32 4a 1b  5b 33 4a`.
The escape character is `1b`, and the other characters follow the normal ASCII encoding. So we are looking at:

        1b 5b 48 1b 5b 32 4a 1b  5b 33 4a
        <ESC> 5b 48 <ESC> 5b 32 4a <ESC> 5b 33 4a
        <ESC>[H <ESC>[2J <ESC>[3J

This `<ESC>[` pattern that we see used repeatedly is the control sequence introducer (CSI) and denotes the beginning of a contorl sequence.<sup>7</sup>

What we see now is that there are three simple commands here.
1. #### Cursor Home: 
    Sets the cursor position where subsequent text will begin. If no row/column parameters are provided the cursor will move to the home position, at the upper left of the screen.<sup>2,3</sup>
2. #### Erase Screen:
    Erases the screen with the background colour and moves the cursor to home.<sup>2,3</sup>
3. #### Erase Whole display including scroll-back buffer
    Erase Whole display including scroll-back buffer.<sup>4</sup>

The first two commands are from the ANSI VT100/VT52 Terminal Control Codes. These date way back to this [bad mama jamma](https://en.wikipedia.org/wiki/VT52#/media/File:Terminal-dec-vt52.jpg) The Digital Equipment Corporation's VT-52 CRT-based computer terminal. Introduced in September 1975. It had a limited set of commands, including `<ESC>J` to clear the screen. While this isn't the exact same command, it is the precursor. The actual commands as they are now date back to the [The VT100](https://en.wikipedia.org/wiki/VT100#/media/File:DEC_VT100_terminal.jpg) that was released in August 1978. If anyone is interested, [here is the VT100 instruction manual.](https://www.vt100.net/docs/vt100-tm/ek-vt100-tm-002.pdf)

These commands were largely comaptible with the ANSI Standard X3.64 from 1979.


The third command is arguably older. It is from the [ECMA-48](https://www.ecma-international.org/publications/files/ECMA-ST/Ecma-048.pdf) CSI sequences circa 1976. ECMA stands for the European Computer Manufacturers Association which was founded on May 17th, 1961 around the mission of stadardizing operational techinques such as programming, and input and output codes.<sup>6</sup>

The ECMA-48 standard dealt specifically with control functions for coded charcter sets. They were even good enough to include this handy definition for "To delete":

        "To remove the contents from character positions and closing the resulting gap by moving adjacent graphic characters into the empty positions. "

To be honest, it is a little tricky to figure out exactly when each of these commands were originally established since each of these standards have been republished over and over again in new editions.

## CTRL-L
CTRL-L has a heritage that seperatly dates back to the VT-52. Apparently, `ctrl+character` is translated to the ASCII code of the letter minus 64. "L" is ASCII 76. Now, 76 - 64 = 12 and 12 is the ASCII character FF (form feed).<sup>8</sup> Back on our dear friend the VT-52, typing FF would move to a new page on a line printer and clear the terminal screen.

## In Conclusion
When you type the `clear` command, or `ctrl-l`, you are taking part in a great computing tradition of clearing the terminal of all the junk that you've cluttered it with, just like our ancestors did in the 1970's.

---
## References
<sup>1</sup>http://www.linfo.org/clear.html<br>
<sup>2</sup>http://ascii-table.com/ansi-escape-sequences-vt-100.php<br>
<sup>3</sup>http://www.termsys.demon.co.uk/vtansi.htm<br>
<sup>4</sup>http://man7.org/linux/man-pages/man4/console_codes.4.html<br>
<sup>5</sup>http://toshyp.atari.org/en/VT_52_terminal.html<br>
<sup>6</sup>https://www.ecma-international.org/memento/history.htm<br>
<sup>7</sup>https://en.wikipedia.org/wiki/ANSI_escape_code<br>
<sup>8</sup>https://upload.wikimedia.org/wikipedia/commons/8/85/ASCII_Code_Chart-Quick_ref_card.jpg<br>
