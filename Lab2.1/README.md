# Lab2.1 Setting up Ubuntu Linux with VMWare Player

## Overview

Linux is a great environment for programming because the tools are readily available. And, there some utilities you can only  access using Linux. You have a few options for running Linux on a  Windows PC:                

- Use Putty (or other terminal emulation software) to log into a remote Linux server via SSH.
- Run Linux on your local Windows PC. Using :                         
  - Dual boot computer                             
    - This can be time consuming to setup
    - This is good if you are doing lots of computational work or graphics
  - Use cygwin                             
    - People rarely do this anymore
  - Use a Linux from a bootup disk                             
    - This is great for certain uses
  - You can set up Linux to run in virtually using VMware player -  So EASY!                             
    - There are other utilities you can use such as [VirtualBox](https://www.virtualbox.org/).
    - In our lab, we will use VMWare Player

The lab uses Ubuntu Linux.The cool thing about using  VMWare Player is that you can have different flavors of Linux running  (at the same time) in different players. This gives you a chance to  evaluate which one you like best. Here is lab objective:

1. Setting up Ubuntu Linux with VMWare Player
2. Learn to edit a file in **vi** Text Editor.                

## What is VMWare Player

VMware Player is software that enables users to easily  create and run virtual machines on a Windows or Linux PC. For additional information you can refer to the vmware website [link](http://www.vmware.com/products/player/faqs.html).                

## What is Ubuntu

Ubuntu is a complete desktop Linux operating system,  freely available with both community and professional support. Ubuntu is suitable for both desktop and server use. Additional information about  Ubuntu can be found [link](https://help.ubuntu.com/12.04/installation-guide/i386/what-is-ubuntu.html).

## What is Vi

Vi is a screen-oriented text editor originally  created for the Unix operating system. Alternate editors for UNIX  environments include pico and emacs, a product of GNU.The advantage of  learning vi and learning it well is that one will find vi on all Unix  based systems and it does not consume an inordinate amount of system  resources. Vi works great over slow network ppp modem connections and on systems of limited resources. One can completely utilize vi without  departing a single finger from the keyboard. (No hand to mouse and  return to keyboard latency). 

The vi editor has two modes of operation:

1. **Command mode** which cause action to be taken on the file.
2. **Insert mode** in which entered text is inserted into the file.

In the command mode, every character typed is a  command that does something to the text file being edited; a character  typed in the command mode may even cause the vi editor to enter the  insert mode. In the insert mode, every character typed is added to the  text in the file; pressing the  (Escape) key turns off the Insert mode.

While there are a number of vi commands, just a  handful of these is usually sufficient for beginning vi users. You can  learn more from [link](http://www.cs.colostate.edu/helpdocs/vi.html).                

## Steps

1. Download & Install VMWare Player [link](https://my.vmware.com/web/vmware/free#desktop_end_user_computing/vmware_player/6_0).                

2. Download Ubuntu [link](http://www.ubuntu.com/download/desktop) by choosing the option of downloading it onto a CD or USB stick. Remember to choose either 32-bit or 64-bit.
   - Ubuntu 12.04 is recommended.
   - A local copy is provided [link](http://121.40.131.130/sec_prog_2021_summer/files/ubuntu-12.04.4-desktop-i386.zip).

3. Set up Ubuntu Linux with VMWare Player. 

4. Edit a file in **vi** Text Editor. 

   - Create a file using vi by entering the command 'vi hello.txt'.          

   - Press i key to switch to Insert Mode.
   
   - Type the following: "Hello world!".

   - Press Esc key to go back to Commond Mode.

   - Enter :wq to save and exit.

## Deliverables

Send it to TA ([liuyuchen0921@zju.edu.cn](mailto:liuyuchen0921@zju.edu.cn)) before **2021.7.4 23:59:59**, with title: **SP2021-LAB2.1-ID-NAME**

- Your **lab** **report** will be the summary of detailed steps and observations. Some                   screenshots or photoes is included.
- All in one PDF document.