
# Quick start

    zcc +m100 -create-app program.c

This command will build a file named "A.CO", to be loaded on a real or emulated TRS80 Model 100.
Once the program is in memory, enter BASIC and give the following command:

    CLEAR 0,49999

now the program can be loaded, either via the BASIC command "RUNM "A.CO" or by pressing F8 to get back to the menu and choosing it with the cursors.
