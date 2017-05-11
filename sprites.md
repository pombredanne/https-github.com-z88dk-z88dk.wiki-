~~NOTOC~~
# SPRITES

One of the main activities of retro-computer enthusiasts is development of new games for old computers.  To help support this activity, z88dk will accumulate a number of sprite engines capable of generating quality games in C.  One of the goals of these sprite engines is to make them as cross-platform as possible so that it will one day be possible to write a game in C for one platform and have it compile for another platform with only trivial modifications.

Sprites come in two varieties: hardware and software.

# Hardware Sprites

Hardware sprites take advantage of specialized circuitry on a specific platform that is capable of displaying sprites without software intervention.  The hardware typically supports a limited number of sprites of fixed size that can be displayed in each video frame.  These sprites are also typically limited in the number of colours that can be used and the number that may be displayed in one horizontal scan line.  However because they are supported by specialized circuitry the software overhead is minimal and it is quite easy to write playable games using them even in interpretted languages like BASIC.  The characteristics of each platform's sprite hardware varies and therefore the hardware sprite APIs supplied by z88dk are necessarily specialized.  However the APIs attempt to add value to the basic functionality supplied by the underlying hardware and this added value is written to be as cross-platform in nature as possible.

# Software Sprites

Software sprites can be implemented on any platform with a memory-mapped display.  Software sprites are movable graphical overlays that are written into the video frame's display buffer by software.  The software emulates hardware sprites by allowing these overlays to be moved about the display buffer whilst preserving display information "underneath" them.  The CPU effectively takes on the burden of the specialized hardware sprite circuitry.  Because these sprites are software generated there is no limit to the number that can be displayed at once on screen nor is there a limit to the number of colours involved, aside from limitations imposed by the display buffer itself.  The only limit is CPU speed, which places a ceiling on the number of sprites that can be managed before a game becomes too sluggish to be playable.  One material difference between software and hardware sprites is that since hardware sprites do not interact with the display buffer, a phenomenon known as colour clash does not occur.  Most display buffers in old computers did not allow individual pixels to be coloured independently.  Instead, in order to conserve memory, pixels were grouped together and had to share one of two colours: a foreground colour for set pixels and a background colour for reset pixels.  When software sprites are written into the display buffer, the sprites' colours interact with surrounding background pixels, resulting in colour clash.

It turns out that the CPU speed to display buffer size ratio was quite precarious on old generation computers, meaning it was not possible, due to limited CPU speed, to generate a single software sprite engine capable of doing everything imaginable with software sprites.  Instead software sprite engines had to be tailored to each specific type of game being written.  For this reason z88dk will accumulate a number of software sprite engines, each with different strengths and weaknesses and suitable for different applications.

# Mix and Match

Keep in mind nothing says you cannot use both hardware and software sprites in your software if a specific platform supports both!  Hardware sprites come with a number of limitations that using software sprites may alleviate.  If your goal is to write cross-platform games it might make sense to do most of the game using software sprites, which are most portable, and then use hardware sprites on specific platforms to enhance presentation.

# Porting Sprite Libraries

If your favourite machine is not currently supported and you would like to help port one of the sprite libraries, why not familiarize yourself with the library and then contact us offering assistance on the [z88dk forum](https://www.z88dk.org/).

