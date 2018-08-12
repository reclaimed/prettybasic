prettybasic
# ZX Spectrum 1982 ROM with syntax highlighting

![BORDER 7: PAPER 7](https://raw.githubusercontent.com/reclaimed/prettybasic/master/images/b7p7.png)

### What's it?
An extended ZX Spectrum 48K ROM with syntax highlighting and basic formatting.

Color groups:
*  letters
*  digits
*  functions
*  operators
*  flow operators (GO TO, GO SUB...RETURN, IF...THEN, FOR...NEXT)
*  REM and its content

Formatting:
* colon (:) is now acting as a carriage return
* IF...THEN is multiline

Misc.:
* control codes (ASCII 00...31) are now hidden in the LIST mode (and HAVE to be shown in edit mode (CS+1), but they're not)
* contrast-aware dynamic palette

Bugs and todo:
* line numbers are being printed in the current INK color - there is a separate subroutine that prints the line numbers
* control codes (ASCII 00...31) must be visible in edit mode


### But... why?
Mostly to read the old BASIC games. They were written in the infamous style "squeeze everything in 9999 lines" and quite hard to read due to it. Below are examples compared side by side:

![NEW vs CLASSIC](https://raw.githubusercontent.com/reclaimed/prettybasic/master/images/new_vs_classic.png)

### Behind the scenes
* The modification is based on the [disassembly](http://www.worldofspectrum.org/infoseekid.cgi?id=2000076) done by Dr Ian Logan & Dr Frank O’Hara
* The original procedure at the address 0x1937 was replaced with a new one 0x386E
* [Zeus Z80 Assembler](http://www.desdes.com/products/oldfiles/zeus.htm)

### Dynamic palette
Colors schemes are being calculated dynamically based on the value of PAPER, as this pseudocode is illustrating:
```pseudocodess
# address of the system register containing the current color
const uint16 ATTR_T = 23695

uint8 current_color
uint8 original_color
uint8 paper_color

function set_color(byte modifier){
    current_color = PEEK(ATTR_T)
    original_color = current_color
    paper_color = bit_extract(current_color, 3-5)
	paper_color = bit_right_shift(paper_color, 3)
	current_color = current_color AND b11111000 AND paper_color XOR modifier
    POKE (ATTR_T), current_color
}

function restore_color(){
    POKE (ATTR_T), original_color
}
```
### ATTR T description

"ATTR T" is a [system register](http://www.worldofspectrum.org/ZXBasicManual/zxmanchap25.html) containing the value of the color attribute to be written into the VRAM attribute area. 

The system registers are basically the bytes in the RAM, and "ATTR T" has fixed address of 0x5C8F (decimal 23695).

ROM screen output routines are seemingly using "ATTR T" each time a character or a graphical primitive is to be drawn, thus writing in 0x5C8F is the fastest way to set the output color.

```
Address:	0x5C8F

#bit	description
0		ink blue
1		ink red
2		ink green
3		paper blue
4		paper red
5		paper green
6		bright
7		flash
```

Examples of the dynamic palette:

![BORDER 7: PAPER 7](https://raw.githubusercontent.com/reclaimed/prettybasic/master/images/b7p7.png)

![BORDER 0: PAPER 0](https://raw.githubusercontent.com/reclaimed/prettybasic/master/images/b0p0.png)

![BORDER 1: PAPER 1](https://raw.githubusercontent.com/reclaimed/prettybasic/master/images/b1p1.png)

### Legal status

ZX Spectrum and its firmware (ROM) are owned by AMSTRAD and their position on this matter is quite liberal:
* Users allowed to change the firmware if they want to
* Users allowed to distribute the ROM modified or not, as soon as it contains the original copyright message
* No one is allowed to sell the ROM, doesn't matter how many time and efforts they invested in modifications
* Whoever is distributing the ROM as a part of the illegal hardware, "nailing them up by the testicles using rusty nails" is strongly recommended (lol they have a whole Eastern Block of nails to rust)
* However, authors of emulators are allowed to charge customers for the code they produced (i.e. the emulator) even if the ROM included into distribution


### Thanks for help to

* Guys from the forum http://zx-pk.ru :
  * weiv
  * AndTorp
  * iceoflame
  * Andrew771
  * Alex Rider
* [Andrew Owen](https://github.com/cheveron/)



