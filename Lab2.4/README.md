# **Lab 2.4** Format String Vulnerability

## Overview

The learning objective of this lab is for students to gain the first-hand experience on format-string vulnerability by  putting what they have learned about the vulnerability from class into  actions. The format-string vulnerability is caused by code like  printf(user input), where the contents of variable of user input is  provided by users. When this program is running with privileges (e.g.,  Set-UID program), this printf statement becomes dangerous, because it  can lead to one of the following consequences: (1) crash the program,  (2) read from an arbitrary memory place, and (3) modify the values of in an arbitrary memory                  place. The last consequence is very dangerous because  it can allow users to modify internal variables of a privileged program, and thus change the behavior of the program. 

In this lab, you will be given a program with a  format-string vulnerability; your task is to develop a scheme to exploit the vulnerability. It uses Ubuntu VM created in Lab 2.1. Ubuntu 12.04  is recommended.

In the following program, you will be asked to  provide an input, which will be saved in a buffer called user input. The program then prints out the buffer using printf. The program is a  Set-UID program (the owner is root), i.e., it runs with the root  privilege. Unfortunately, there is a format-string vulnerability in the  way how the printf is called on the user inputs. We want to exploit this vulnerability and see how much damage we can achieve.

```c
/* vul_prog.c */ 

#define SECRET1 0x44
#define SECRET2 0x55

int main(int argc, char *argv[])
{
  char user_input[100];
  int *secret;
  int int_input;
  int a, b, c, d; /* other variables, not used here.*/

  /* The secret value is stored on the heap */
  secret = (int *) malloc(2*sizeof(int));

  /* getting the secret */
  secret[0] = SECRET1; secret[1] = SECRET2;

  printf("The variable secret's address is 0x%8x (on stack)\n", &secret);
  printf("The variable secret's value is 0x%8x (on heap)\n", secret);
  printf("secret[0]'s address is 0x%8x (on heap)\n", &secret[0]);
  printf("secret[1]'s address is 0x%8x (on heap)\n", &secret[1]);

  printf("Please enter a decimal integer\n");
  scanf("%d", &int_input);  /* getting an input from user */
  printf("Please enter a string\n");
  scanf("%s", user_input); /* getting a string from user */

  /* Vulnerable place */
  printf(user_input);  
  printf("\n");

  /* Verify whether your attack is successful */
  printf("The original secrets: 0x%x -- 0x%x\n", SECRET1, SECRET2);
  printf("The new secrets:      0x%x -- 0x%x\n", secret[0], secret[1]);
  return 0;
}
```

The program has two secret values stored in its memory, and you are  interested in these secret values. However, the secret values are  unknown to you, nor can you find them from reading the binary code (for  the sake of simplicity, we hardcode the secrets using constants 0x44 and 0x55). Although you do not know the secret values, in practice, it is  not so difficult to find out the memory address (the range or the exact  value) of them (they are in consecutive addresses), because for many  operating systems, the addresses are exactly the same anytime you run  the program. In this lab, we just assume that you have already known the exact addresses. To achieve this, the program "intentionally" prints  out the addresses for you. With such knowledge, your goal is to achieve  the followings (not necessarily at the same time):

- Crash the program named "vul_prog.c".
- Print out the secret[1] value.
- Modify the secret[1] value.
- Modify the secret[1] value to a pre-determined value.

#### Steps

1. Open the Terminal in Ubuntu.

2. To create the program named "vul_prog.c".

3. Compile the  program.

4. Run the program and enter proper content to finish the lab.

#### Deliverables

Send it to TA ([liuyuchen0921@zju.edu.cn](mailto:liuyuchen0921@zju.edu.cn)) before **2021.7.4 23:59:59**, with title: **SP2021-LAB2.4-ID-NAME**

- Your **lab** **report** will be the summary of detailed steps and observations. Some screenshots or photoes is included.
- All in one PDF document.