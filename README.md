prettybasic
# ZX Spectrum 1982 ROM w/syntax highlighting

![BORDER 7: PAPER 7](https://raw.githubusercontent.com/reclaimed/prettybasic/master/b7p7.png)

### What's it?
It's a little hack of the original ZX Spectrum firmware that adds a few pretty features to the BASIC 48k editor:

* letters, digits, function, operators, and flow operators (goto, gosub...return, if...then, for...next) now all have distinguishable colors
* automatic color schemes based on the current PAPER setting
* colon in code now acts as a carriage return
* control codes 00...31 are hidden in the editor mode
* IF...THEN block has a multiline appearance
* REMs are impossible to overlook

### But... why?
Mostly to read the old BASIC games. They were written in the infamous style "squeeze everything in 9999 lines" and quite hard to read due to it. Below are examples compared side to side:

![NEW vs CLASSIC](https://raw.githubusercontent.com/reclaimed/prettybasic/master/new_vs_classic.png)

### Boring details
Based on the disassembly of the original ZX Spectrum 48k ROM from 1982.
[Zeus Z80 Assembler](http://www.desdes.com/products/oldfiles/zeus.htm) was used to edit and translate the asm code.
The original procedure that prints the LISTing char-by-char is what has been modified:

```assembly
# original OUT_CHARACTER procedure
$1937:  JP $386E
        # now unused space
        RET
...
# free space in ROM
$386E:  ...
        # char_code = A register
        # if char_code in letters set_color(magenta)
        # if char_code in operators set_color(blue)
        # ...
        # put char_code to the screen
        # restore_color
        ...
        RET
```

### Color schemes
Colors schemes are being calculated dynamically based on the value of PAPER. How it looks in pseudocode:

```pseudocode
function set_color(byte modifier){
    current = PEEK(23695)   # current color in FBPPPIII format, where
                            # F - flash bit
                            # B - bright bit
                            # PPP - paper GRB bits
                            # III - ink GRB bits
    PUSH current
    current.III = current.PPP
    new = current XOR modiifier
    POKE 23695, new
}

function restore_color(){
    POP current
    POKE 23695, current
}
```

Examples:

![BORDER 7: PAPER 7](https://raw.githubusercontent.com/reclaimed/prettybasic/master/b7p7.png)

![BORDER 0: PAPER 0](https://raw.githubusercontent.com/reclaimed/prettybasic/master/b0p0.png)

![BORDER 1: PAPER 1](https://raw.githubusercontent.com/reclaimed/prettybasic/master/b1p1.png)

### License

You can do whatever you want as long as it's not a killing spree in kindergartens. Also, it would be rather smart to read AMSTRAD license as well.

