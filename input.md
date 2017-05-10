# INPUT LIBRARY (input.h)

 | Header     | [{z88dk}/include/input.h](https///raw.githubusercontent.com/z88dk/z88dk/master/include/input.h?view=markup)    |   
 | -----------------------------------------------------------------------------------------------------------------------------   
 | Source     | [{z88dk}/libsrc/input](https///github.com/z88dk/z88dk/tree/master/libsrc/input/)                               |   
 | Include    | #include `<input.h>`                                                                                              |  
 | Linking    | n/a                                                                                                             |  
 | Compile    | n/a                                                                                                             |  
 | Supported  | [zx spectrum](platform/zx), [zx81](platform/zx81), [sam](platform/samcoupe)                                       |
 | Comments   | **1.** These library functions are compiled as part of each target's implicit library.                          |  
 | | **2.** Platform-specific input functions are documented as comments in the header file.                         |             
 | | **3.** The functions in this library access input device hardware directly; this is usually                     |             
 | | different from the stdin functions which most commonly rely on the native operating                             |             
 | | system to supply input at a level higher than the hardware layer.                                               |             

The input library contains functions for reading the keyboard, joysticks and mice.  It defines common functions and a style for handling these devices across z80 platforms.  Each z80 target will also have platform-specific functions for specific devices whose definitions are mentioned in comments in the header file.

# Keyboard

These functions directly access the keyboard hardware and do not delegate to any existing native operating system.  All functions deal in ascii character codes to communicate keypresses.

There are three ways to read the keyboard:

 1.  like a typewriter with key repeat features; see in_GetKey()
 2.  like the common BASIC keyword INKEY$ where the instantaneous state of keyboard is tested but only one keypress is allowed; see in_Inkey()
 3.  like an arcade game where many keys may be simultaneously pressed and the desire is test the state of specific keys; see in_KeyPressed() and in_JoyKeyboard()

## Keyboard API

**1. in_GetKeyReset()**

Resets in_GetKey()'s state machine.  If in_GetKey() is used in your program call this function during initialization.

	:::c
	void in_GetKeyReset(void);


**2. in_GetKey()**

Returns the ascii code of a keypress.  If either no key is pressed or more than one key is simultaneously pressed, 0 is returned.  This function implements key debouncing and key repeat and is meant for typing applications.  To use this function your program must declare the following variables:

	:::c
	uchar in_KeyDebounce;        // Number of ticks before a keypress is acknowledged. Set to 1 for no debouncing.
	uchar in_KeyStartRepeat;     // Number of ticks after first time key is registered (after debouncing) before a key starts repeating.
	uchar in_KeyRepeatPeriod;    // Repeat key rate measured in ticks.
	uint in_KbdState;            // Reserved variable holds in_GetKey() state


Initialize these variables to adjust the typing response desired.  The time measurement "ticks" refers to the number of times the in_GetKey() function is called.  If, for example, in_GetKey() is called from within a 50Hz interrupt service routine, one tick would be equal to 1/50 of a second.

	:::c
	unsigned int in_GetKey(void);


**3. in_Inkey()**

Tests the instantaneous state of the keyboard and returns the ascii code of a keypress.  If either no key is pressed or more than one key is simultaneously pressed, 0 is returned.

	:::c
	unsigned int in_Inkey(void);


**4. in_LookupKey()**

Returns a 16-bit keyboard scancode corresponding to the ascii character passed in as parameter.  This scancode encodes information that describes which keys on the keyboard must be pressed in order to generate that ascii character.  If no key combination can generate the ascii character, 0 is returned and the carry flag is set.  The scancode is used by in_KeyPressed() and in_JoyKeyboard() to test keyboard state.

	:::c
	unsigned int in_LookupKey(unsigned char c);
	// c = ascii character to generate from keyboard


**5. in_KeyPressed()**

Tests whether the key combination indicated by scancode is currently pressed on the keyboard.  This function is designed to work properly when many keys may be simultaneously pressed.

	:::c
	unsigned int in_KeyPressed(uint scancode);
	// scancode = word returned by in_LookupKey() that encodes keypresses required to generate a specific ascii character


**6. in_WaitForKey()**

Waits for a keypress before returning.

	:::c
	void in_WaitForKey(void);


**7. in_WaitForNoKey()**

Waits until no keys are pressed before returning.

	:::c
	void in_WaitForNoKey(void);


**8. in_Pause()**

Pause for a period of time measured in milliseconds.  Returns early with time remaining in the pause if a key is pressed.  This function is implemented as a busy-wait and is guaranteed to work even with interrupts disabled.  If msec==0, executes in_WaitForNoKey() followed by in_WaitForKey() effectively waiting until a key is pressed.

	:::c
	unsigned int in_Pause(unsigned int msec);
	// msec = time to pause measured in milliseconds, if 0 waits until key is pressed


**9. in_Wait()**

Wait for a period of time measured in milliseconds.  This function is implemented as a busy-wait and is guaranteed to work even with interrupts disabled.

	:::c
	void in_Wait(uint msec);
	// msec = time to wait measured in milliseconds


## Code Example

This scenario concerns the ZX Spectrum target.  On the ZX Spectrum, the firmware's interrupt service routine is called 50 times per second at the beginning of a video frame.  One of the responsibilities of this ISR is to scan the keyboard and store the ascii code of any keypresses in the system variable LAST_K.  The system consumes keypresses by reading the contents of LAST_K and then writing a 0 to LAST_K to ensure keypresses are not consumed more than once.  The system's ISR implements key repeat features which manifest as periodic updates of the LAST_K system variable.

The situation is that an IM2 interrupt is desired to be run in place of the firmware interrupt for a couple of reasons:  1) The firmware interrupt service routine is slow and performs activities in support of the basic environment which make no sense in a C program.   2) The firmware's interrupt routine expects exclusive use of the IY register which interferes with some of z88dk's library functions.

Since z88dk's stdio library for the ZX Spectrum reads the system variable LAST_K to gather keyboard input, by not running the firmware's ISR no more keypresses will be read by stdin.  Instead we need to duplicate the key-scanning and key-repeat features of the firmware ISR in our own IM2 interrupt service routine so that stdin can continue to function as normal.  The keyboard scanning replacement used will be in_GetKey().

	:::c
	#include `<im2.h>`
	#include `<input.h>`
	
	// bind memory location of system variable LAST_K
	
	extern char LAST_K(23560);
	
	// im2 interrupt service routine called each 1/50s
	
	M_BEGIN_ISR_LIGHT(my_isr)
	{
	   char c;
	
	   if ((c = in_GetKey()) != 0)
	      LAST_K = c;
	}
	M_END_ISR_LIGHT
	
	// variables required by in_GetKey()'s state machine
	
	uchar in_KeyDebounce = 1;       // no debouncing
	uchar in_KeyStartRepeat = 20;   // wait 20/50s before key starts repeating
	uchar in_KeyRepeatPeriod = 10;  // repeat every 10/50s
	uint in_KbdState;               // reserved
	
	...
	
	main()
	{
	
	...
	
	   in_GetKeyReset();
	
	...
	
	   // installing im2 interrupt routine not shown
	
	...
	
	}


# Joystick

These functions directly access joystick hardware and do not delegate to any existing native operating system.  All joystick functions return a commonly formatted byte that indicates the state of four directions and up to two fire buttons.  In addition all joystick functions have the same signature, namely "unsigned int in_JoyName(void)" with the exception of the keyboard joystick "unsigned int in_JoyKeyboard(struct in_UDK *)".  The common function signature allows a function pointer to refer to one of many user-selectable joystick devices.

The return value of all joystick functions is the byte FG00RLDU active high, with F = fire1, G = fire2, R = right, L = left, D = down and U = up.  The following masks can be used to test the result:

	:::c
	#define in_FIRE  0x80
	#define in_UP    0x01
	#define in_DOWN  0x02
	#define in_LEFT  0x04
	#define in_RIGHT 0x08
	
	#define in_FIRE1 0x80
	#define in_FIRE2 0x40


Because joysticks may be emulated by devices other than a real joystick (the keyboard for example) do not make the assumption that certain direction combinations are mutually exclusive.  For example it is impossible to have both left and right asserted at the same time on a real joystick, but not on a joystick simulated by keypresses.

The input library only supplies one cross-platform joystick function: the keyboard joystick.  Joystick functions specific to each platform are described in the header file.

## Joystick API

**1. in_JoyKeyboard()**

Simulates a joystick using the keyboard.  Returns the state of the keyboard joystick using the common joystick result F000RLDU active high as described above.  This function accepts a single parameter struct_in_UDK that holds scancodes for each of the four joystick directions and a single fire button:

	:::c
	struct in_UDK {          // user defined keys structure
	   uint fire;
	   uint right;
	   uint left;
	   uint down;
	   uint up;
	};


This structure should be filled in by calls to in_LookupKey().

	:::c
	unsigned int in_JoyKeyboard(struct in_UDK *u);
	// u = struct containing keyboard scancodes for four joystick directions and a single fire button


## Code Example

Notice that the function pointer is called with the parameter expected by in_JoyKeyboard().  Since no other joystick function expects a parameter, only the in_JoyKeyboard() function, if chosen by the user, will make use of it.

	:::c
	#include `<input.h>`
	#include `<spectrum.h>`         // for spectrum-specific joystick functions
	
	// example for ZX Spectrum target which supplies
	// the following device specific joystick functions:
	// in_JoyKempston(), in_JoySinclair1(), in_JoySinclair2()
	
	uchar choice, dirs;
	void *joyfunc;                // pointer to joystick function
	char *joynames[] = {          // an array of joystick names
	   "Keyboard QAOPM",
	   "Kempston",
	   "Sinclair 1",
	   "Sinclair 2"
	};
	struct in_UDK k;
	
	// initialize the struct_in_UDK with keys for use with the keyboard joystick
	
	k.fire  = in_LookupKey('m');
	k.left  = in_LookupKey('o');
	k.right = in_LookupKey('p');
	k.up    = in_LookupKey('q');
	k.down  = in_LookupKey('a');
	
	// print menu and get user to select a joystick
	
	printf("You have selected the %s joystick\n", joynames[choice]);
	switch (choice) {
	   case 0 : joyfunc = in_JoyKeyboard; break;
	   case 1 : joyfunc = in_JoyKempston; break;
	   case 2 : joyfunc = in_JoySinclair1; break;
	   default: joyfunc = in_JoySinclair2; break;
	}
	
	...
	
	// read the joystick through the function pointer
	
	dirs = (joyfunc)(&k);
	if (dirs & in_FIRE)
	   printf("pressed fire!\n");
	
	...


# Mouse

These functions directly access mouse hardware and do not delegate to any existing native operating system.  All supported mice implement three functions:

 1.  an initialization function "void in_MouseNameInit(void)" that initializes the mouse hardware and places initial coordinate at the top left corner of the screen
 2.  a set mouse position function "void in_MouseNameSetPos(uint x, uint y)" that places the mouse's current position at the coordinates specified
 3.  a mouse read function "void in_MouseName(uchar *b, uint *x, uint *y)" that returns the current mouse coordinate and state of the mouse buttons

To better support the cross-platform nature of the mouse functions, mouse coordinates are always passed in 16-bit values; out of range coordinates are normally handled by setting coordinates to their maximum value.  The mouse button state is returned in a commonly formatted byte 00000MRL active high, with M = middle button, R = right button and L = left button.  The following masks can be used to test the result:

	:::c
	#define in_MLEFT   0x01
	#define in_MRIGHT  0x02
	#define in_MMID    0x04
	
	#define in_BUT1    0x01
	#define in_BUT2    0x02
	#define in_BUT3    0x04


As with the joystick functions, all mouse functions conform to the function signatures listed above.  The common function signature allows a single function pointer to refer to one of many user-selectable mouse devices. 

## Mouse API

This library supplies a single cross-platform mouse type: the joystick mouse.  Other platform-specific mouse functions are described in the header file.

The joystick mouse attempts to simulate a real mouse using a joystick by accelerating the mouse pointer the longer a direction is asserted.  The acceleration profile used is user-definable.  The information about what joystick to use as input device (this can be a platform-specific joystick function or the keyboard joystick), the acceleration profile to use and the current mouse coordinates are held in a struct_in_UDM:

	:::c
	struct in_UDM {              // User Defined Mouse Struct
	   struct in_UDK  *keys;     // parameter if in_JoyKeyboard() is used else ignored
	   void          *joyfunc;   // joystick function for reading input
	   struct in_MD **delta;     // pointer to array of in_MD; last max count must be 255
	   uchar          state;     // current index into delta array
	   uchar          count;     // current count
	   uint           y;         // current (x,y) coordinate, fixed point
	   uint           x;
	};


The joyfunc member is the address of a joystick input subroutine like in_JoyKeyboard().  If in_JoyKeyboard() is used, the keys member must point at the struct_in_UDK used by in_JoyKeyboard().  delta holds the acceleration profile to use in an array of mouse deltas.

	:::c
	struct in_MD {               // Mouse Deltas in acceleration profile
	   uchar maxcount;           // number of times to use this mouse delta before moving to the following one
	   uint  dx;                 // x += dx if mouse moved in horizontal dir, fixed point
	   uint  dy;                 // y += dy if mouse moved in vertical dir, fixed point
	};


Each mouse delta holds a displacement added to the current mouse coordinate and indicates how long it is used before moving to the next one.  In this way you can customize the acceleration of the pointer by beginning with small changes to the mouse coordinate and moving on to larger changes the longer a direction is asserted.  The last mouse delta in the array must have a maxcount of 255.

**1. in_MouseSimInit()**

Initializes simulated joystick mouse state machine and initial coordinates to (0,0).  The struct_in_UDM should be separately filled in with joystick function used to read the input device and a pointer to the acceleration profile to use.

	:::c
	void in_MouseSimInit(struct in_UDM *u);
	// u = user defined mouse struct


**2. in_MouseSim()** ; *Uses IX*

Read the simulated mouse's current coordinate and button state.

	:::c
	void in_MouseSim(struct in_UDM *u, uchar *buttons, uint *xcoord, uint *ycoord);
	//       u = user defined mouse struct
	// buttons = return button state in byte with format 00000MRL active high
	//  xcoord = return 16-bit x coordinate
	//  ycoord = return 16-bit y coordinate


**3. in_MouseSimSetPos()**

Sets the simulated mouse's current coordinates.

	:::c
	void in_MouseSimSetPos(struct in_UDM *u, uint xcoord, uint ycoord);
	//      u = user defined mouse struct
	// xcoord = 16-bit x coordinate set value
	// ycoord = 16-bit y coordinate set value


## Code Example

Notice that the function pointers are called with the parameters expected by the simulated joystick mouse.  This is because the simulated joystick mice take one additonal parameter.  Since no other mouse functions expect this extra parameter, only the simulated mouse functions, if chosen by the user, will make use of it.

	:::c
	#include `<input.h>`
	#include `<spectrum.h>`        // for spectrum-specific mouse functions
	
	// example for ZX Spectrum target which supplies
	// the following device specific mouse functions:
	// in_MouseAMXInit(), in_MouseAMXSetPos(), in_MouseAMX()
	// in_MouseKempInit(), in_MouseKempSetPos(), in_MouseKemp()
	
	...
	
	uchar choice, b;
	uint x, y;
	
	void *mouseinit, *mouseread, *mousesetpos;
	
	char *mousename[] = {
	   "Keyboard Mouse QAOPM",
	   "AMX Mouse",
	   "Kempston Mouse"
	};
	
	void dummy(void) {}          // dummy function doing nothing
	
	// variables required by simulated joystick mouse
	
	struct in_UDM m;
	struct in_UDK k;
	
	struct in_MD deltas[] = {
	   3,0x10,0x10,              // 3 times one pixel move
	   3,0x20,0x20,              // 3 times two pixel move
	   5,0x40,0x40,              // 5 times four pixel move
	   255,0x80,0x80             // forever eight pixel move
	};
	
	// variables required by kempston mouse
	
	unsigned char in_KempcoordX, in_KempcoordY, in_KemprawX, in_KemprawY;
	
	// variables required by amx mouse
	
	unsigned int in_AMXcoordX, in_AMXcoordY, in_AMXdeltaX, in_AMXdeltaY;
	
	...
	
	main()
	{
	
	   ...
	
	   // not shown: the AMX Mouse uses IM2 mode and must have
	   // its interrupt service routines registered during init
	
	   ...
	
	   // set up the simulated mouse to use keyboard joystick
	
	   k.fire  = in_LookupKey('m');   // fill in keys for in_JoyKeyboard()
	   k.left  = in_LookupKey('o');
	   k.right = in_LookupKey('p');
	   k.up    = in_LookupKey('q');
	   k.down  = in_LookupKey('a');
	
	   m.keys = &k;
	   m.joyfunc = in_JoyKeyboard;   // simulated mouse will use the keyboard joystick for input
	   m.delta = deltas;             // acceleration profile
	
	   ...
	
	   // have user select mouse device from menu
	
	   printf("You have selected the %s mouse\n", mousename[choice]);
	   switch (choice) {
	      case 0 : mouseinit   = in_MouseSimInit;
	               mousesetpos = in_MouseSimSetPos;
	               mouseread   = in_MouseSim;
	               break;
	      case 1 : mouseinit   = dummy;                // AMX mouse initialized during im2 setup
	               mousesetpos = in_MouseAMXSetPos;
	               mouseread   = in_MouseAMX;
	               break;
	      default: mouseinit   = in_MouseKempInit;
	               mousesetpos = in_MouseKempSetPos;
	               mouseread   = in_MouseKemp;
	               break;
	   }
	
	   (mouseinit)(&m);
	
	   ...
	
	   (mouseread)(&m, &b, &x, &y);
	   if (b & in_BUT1)
	      printf("button pressed at coord (%d,%d)!\n", x, y);
	
	   ...
	
	}


# Questions & Answers

Post common questions and answers here.  Report any bugs or ask a question not listed here on the [z88dk mailing list](root/support).

