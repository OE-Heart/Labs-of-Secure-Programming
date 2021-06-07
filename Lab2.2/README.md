# Lab2.2 Running a Hello World Program in C using GCC

#### **Overview**

The lab helps familiarize you with writing a simple Hello World program using C, the GCC compiler [link](http://gcc.gnu.org/), and Pico(a text editor, [link](http://en.wikipedia.org/wiki/Pico_(text_editor))). It uses Ubuntu VM created in Lab 2.1.Here is lab objective:                

1. Learn to run a program in gcc.
2. Learn to debug a program in gdb.

#### Steps

1. Open the Terminal in Ubuntu.                

2. Create 'debug_me.c':

   ```c
   #include 
   
   /* print a given string and a number if a pre-determined format. */
   void
   print_string(int num, char* string)
   {
       printf("String '%d' - '%s'\n", num, string);
   }
   
   int
   main(int argc, char* argv[])
   {
       int i;
   
       /* check for command line arguments */
       if (argc < 2) { /* 2 - 1 for program name (argv[0]) and one for a param. */
           printf("Usage: %s [ ...]\n", argv[0]);
   	exit(1);
       }
   
       /* loop over all strings, print them one by one */
       for (argc--,argv++,i=1 ; argc > 0; argc--,argv++,i++) {
           print_string(i, argv[0]);  /* function call */
       }
   
       printf("Total number of strings: %d\n", i);
      
       return 0;
   }
   ```

3. Invoke gdb to debug 'debug_me.c':

   ```shell
   $ gcc -g debug_me.c -o debug_me
   $ gdb debug_me
   ```

4. Run the program inside gdb:

   ```shell
   (gdb) run "hello, world" "goodbye, world"
   ```

5. Set breakpoints. You can set breakpoints using two methods:

   - Specifying a specific line of code to stop in:

     ```shell
     (gdb) break debug_me.c:9
     ```

   - Specifying a function name, to break every time it is being called:

     ```shell
     (gdb) break main
     ```

6. Step a command at a time. After setting breakpoints, you can run the program step by step. There are also two methods:

   ```shell
   (gdb) next
   ```

   or

   ```shell
   (gdb) step
   ```

   You should tell the difference between "next" and "step".

7. Print variables. When running the program step by step, you can print the contents of a variable with a command like this:

   ```shell
   (gdb) print i
   ```

8. Examine the function call stack. 

   - Run the the program step by step to line 7. To examine the function call stack, type:

     ```shell
     (gdb) where
     ```

   - You will see currently executing function   "print_string" and the function "main" which called it. Then type "frame 0" and "frame 1" to see the difference:

     ```shell
     (gdb) frame 0
     (gdb) print i
     ...
     (gdb) frame 1
     (gdb) print i
     ```

#### Deliverables

Send it to TA ([liuyuchen0921@zju.edu.cn](mailto:liuyuchen0921@zju.edu.cn)) before **2021.7.4 23:59:59**, with title: **SP2021-LAB2.2-ID-NAME**

- Your **lab** **report** will be the summary of detailed steps and observations. Some screenshots or photoes is included.
- All in one PDF document.