In a pique of ~~masochism~~nostalgia I decided to write a snake game in Apple II BASIC.

# To Run

The code runs under the emulator at [appleIIjs](https://www.scullinsteel.com/apple2/#prodos) and under [AppleWin](https://github.com/AppleWin/AppleWin) and likely others. Just boot the emulator into BASIC and paste the code from SNAKE.BAS. Because emulators steal the keyboard, pasting using standard shortcut keys might not work. For online emulators, use your browser's menu to paste. For AppleWin, Shift-Insert does the trick. 

Using AppleWin and appleIIjs under ProDOS the above instructions work fine. Under DOS 3.3, before pasting you'll have to clear out the existing BASIC code using "DEL 0,300."

To run on an actual Apple II you'll need to get SNAKE.BAS onto the machine somehow. I don't own one so ¯\\_(ツ)_/¯.

Once pasted, hit enter to be sure you're at a "]" prompt then type "RUN" and press enter.

Anachronistically, WASD keys move the snake[^1]. Q quits any time. I didn't build any controller/joystick logic into the game but it should be easy to do.

# About the Code

Shockingly, while I've written plenty of BASIC on 8 bit computers, I've never written a snake game on any platform. So maybe there's a better approach than mine. I wanted to avoid anything that was O(length of snake) because looping in BASIC on the old machines would get very slow as the snake got longer, making the game slower and slower. My approach was to have 3 arrays. The first array holds a map of the screen. It's used to quickly discover if the snake has crashed into itself. Likely if I spent a bit more time I could have just PEEKed into character memory to save an array. That would speed up initialization quite a bit. Maybe someday.

The second and thrid arrays hold the x and y coordinates (respectively) of the snake's segments. They're arranged in the style of a circular buffer with HI (head index) pointing at the location for the first segment and TI (tail index) pointing at the location for the last segment. When the snake moves without eating food the old tail segment is erased on the screen and in the map then both HI and TI are incremented. When the snake does eat food only HI is incremented. Both HI and TI "wrap around" individually when they go past the end of the segment array. The ciruclar buffer style means there's no need to move coordinates in the array, only the indexes change.

There are no GOSUBs, it's all GOTOs. I'll just pretend that I was handrolling tail call optimization. :)

# Portability

8 bit BASICs tend to be so-so portable. It's been a loooong time so I can't be certain that I know all the likley unportable behavior until I try to port (someday maybe). Likely spots of incompatibility:

1. The specific PEEK used to get the current keypress
2. The use of HTAB,VTAB pairs to locate the text cursor for the next print. Some BASICs have "PRINT AT", some have LOCATE and some need a specific set of POKEs
3. The use of GET to to grab a key from the input buffer. I honestly can't remember how common GET was. 


[^1]: Anachronistic because WASD wasn't a standard control scheme on 8 bit machines.  WASD is something that came from the mouse+keyboard shooter genre were common on more powerful machines.