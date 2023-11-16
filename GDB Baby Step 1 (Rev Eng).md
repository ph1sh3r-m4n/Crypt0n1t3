This one was comparatively easy.
First I watched a whole video on GNU and GDB to understand what am I dealing with here.  
I watched a video on how to use GDB to disassemble the file.  
I used Kali Linux CLI to operate further.  
I downloaded the gdb package into my computer using _sudo apt install gdb_ and loaded the file *debugger0_a* into it.  
Then all I did was type _disassemble main_ in the Terminal ans voila a hex eax register number was right on there waiting for me!
I transtaled the hex number into decimal and got the flag as _picoCTF{549698}_

