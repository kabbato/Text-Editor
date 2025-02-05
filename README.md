# Text-Editor
Text editor built using C, compiled in Linux Bash with GCC, using terminal as editor interface.

This text editor is built based on this project: https://viewsourcecode.org/snaptoken/kilo/index.html. I built this project as a learning experience on advanced terminal and memory manipulation with C. This project is probably the most complex one I've worked on to date, especially as it started to get past 300 lines, as it was hard to keep track of all the different functions and data that each function was managing, while only working on one or two functions at a time. Despite that, it was a very rewarding experience, as I find myself much more knowledgeable about what you can do with the terminal and how to manage memory in C. 

As for future changes, I still need to implement the recommended feature of syntax highlighting, as well as implement my own plans to move some of the code sections into separate files in order to better separate and manage them in my head. I'd also like to see if I can implement some sort of spellcheck feature, as well as undo/redo options. 

## Makefile
File with instructions on how to compile kilo.c when using the "make" command, provided by GCC, in Bash.

## kilo
Executable made by compiling kilo.c with "make", can be run with the command "./kilo" in Bash. If you want to open another txt file in the editor, you can do so by running kilo with the format "./kilo [filename].txt". 

## kilo.c
File containing all the code for the program, has been separated into the following major sections:

### Includes
The 3 defines at the top of this section are feature test macros that insure that the compiler doesn't have an issue with getline(). The rest of this section is all the includes needed for the program.

### Defines
The first 3 defines here are constants for later use. The 4th define takes a parameter and then edits it's ASCII value to match what it is when the key passed as a parameter is pressed in conjunction with the CTRL key. The last define is one for an empty buffer which will be used later. The editorKey enum assigns large integer values to a couple different keys, so that theyre out of the 127 character ASCII range, except for BACKSPACE, which is given 127, which traditionally belongs to DELETE. We do this so that those keypresses don't conflict with any other characters when we process them later, except for BACKSPACE, which in modern computers is apparently 127. 

### Data
The erow struct stands for editor row, and each isntance of it tracks a single row of text in the editor. The editorConfig struct tracks the global state of the editor, including things like cursor position, terminal rows and columns, how "dirty" the file is (whether its been edited and if so, how much), filenames, and more. The abuf struct stores the data for a buffer we will use later. 

### Prototypes
These functions are defined here so that they can be called by functions that call on them before their full definition. 

### Terminal 
These functions handle terminal operations, such as enabling and disabling raw mode, reading user input, getting the window size, and handling errors with die(). Raw mode is something we need to enable by disabling a number of flags that exist in the terminal normally, which will otherwise interfere with the programs ability to pick up user keypresses and input. 

### Row Operations
These functions exclusively work on handling rows within the editor, and do not mess with cursor position at all. This includes things like inserting characters into rows, deleting characters from rows, and inserting and deleting whole rows.

### Editor Operations
These functions are called to handle certain user input and can edit cursor position, as well as call on row operations to handle row edits and updates.

### File I/O
These functions handle everything related to file management, i.e. opening files, and saving/creating files. 

### Find
This implements a functionality within the editor that mimics the "CTRL-f" feature that many text editors and web browsers have. It allows the user to press "CTRL-f" to open a search prompt at the bottom of the editor from where the user can type in characters and/or words/phrases that they are looking for within the document. The editor will then move their cursor to the first instance of their search, from where the user can use the arrow keys to move between each instance of their searched value, and use ESC or ENTER to exit the search prompt. 

### Append Buffer
Here is where the define and struct from earlier come into play. Here, we create a buffer where we store everything that needs to be updated to the screen when the screen gets refreshed, and then write() it all to the screen in one go to reduce screen flickering and other issues that come with using a bunch of write()s to update the screen. 

### Output
These functions deal with output related function, such as scrolling past the edges of the window, refreshing the screen, and creating the status bar. 

### Input
These functions deal with user input and handle things like processing keypresses, moving the cursor, and getting the input from the prompt for the find and saving functions. 

### Init
The init editor function here exists to initialize values for the editorConfig struct, followed by the main function which runs the whole program. 
