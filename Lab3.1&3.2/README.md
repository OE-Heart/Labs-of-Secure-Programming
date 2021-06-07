# **Lab 3.1** Using splint for C static analysis

#### **Overview**

The learning objective of this lab is for students to gain the first-hand experience on using static code analysis tools to   check c program for security vulnerabilities and coding mistakes.

Splint [link](http://www.splint.org/) is a tool for statically checking C programs for security vulnerabilities  and programming mistakes.  Splint does many of the traditional lint  checks including unused declarations, type inconsistencies, use before  definition, unreachable code, ignored return values, execution paths  with no return, likely infinite loops, and fall through cases.  More  powerful checks are made possible by additional information given in  source code annotations.  Annotations are stylized comments that  document assumptions about functions, variables, parameters and types.   In addition to the checks specifically enabled by annotations, many of  the traditional lint checks are improved by exploiting this additional  information. 

11 kinds of problems detected by Splint include:

- Dereferencing a possibly null pointer;
- Using possibly undefined storage or returning storage that is not properly defined;
- Type mismatches, with greater precision and flexibility than provided by C compilers;
- Violations of information hiding;
- Memory management errors including uses of dangling references and memory leaks;
- Dangerous aliasing;
- Modifications and global variable uses that are inconsistent with specified interfaces;
- Problematic control flow such as likely infinite loops, fall through cases or incomplete switches, and suspicious statements;
- Buffer overflow vulnerabilities;
- Dangerous macro implementations or invocations;
- Violations of customized naming conventions.

More details you can get from Splint User's Manual [link](http://www.splint.org/manual/manual.html#memory).

With such knowledge, your goal is to achieve the followings:

- Install splint;
- Finish code samples with 2 different kinds of problems which can be detected by Splint. You can choose any 2 of 11 problems as above.
- Use splint to detect the 2 kinds of problems. Descibe your observations in your report.

#### Steps                

1. Download Splint source code distribution [link](http://www.splint.org/downloads/splint-3.1.2.src.tgz).

2. Extract and setup Splint.

   ```shell
   $ tar -zxvf splint-3.1.2.linux.tgz
   $ sudo mkdir /usr/local/splint
     [sudo]password for ***: (enter password)
   $ cd splint-3.1.2
   $ ./configure --prefix=/usr/local/splint
   $ sudo apt-get install flex
   $ make
   $ sudo make install
   ```

3. Use vi Text Editor to add the following to the  environment variable:

   ```shell
   $ vi ~/.bashrc
   
   (add the following to the environment variable in .bashrc)
   export LARCH_PATH=/usr/local/splint/share/splint/lib
   export LCLIMPORTDIR=/usr/splint/share/splint/imports
   export PATH=$PATH:/usr/local/splint/bin
   
   $ source ~/.bashrc
   ```

4. Finish code samples with 2 different kinds of problems.

5. Use Splint to detect problems in code samples.

   ```shell
   $ splint codeSamoleOne.c
   ```

#### Deliverables

In lab3.1, you don’t have to write a single  lab report, but submit the report together with lab3.2 (see details of lab 3.2 for  deadline).  



#### **Lab 3.2** Using eclipse for java static analysis

#### **Overview**

The learning objective of this lab is for students to gain the first-hand experience on using static code analyzers in  Eclipse to check  Java program for security vulnerabilities and coding  mistakes.

In this Lab, your goal is to achieve the followings:

- Install plugins in Java;
- Learn to check Java code by using static code analyzers in Eclipse. Descibe your observations in your report.

#### Open Source Code Analyzers in Java

Here we introduce 3 kinds of open source code analyzers in Java.

- **FindBugs**

- FindBugs looks for bugs in Java programs. It can  detect a variety of common coding mistakes, including thread  synchronization problems, misuse of API methods, etc. Go To FindBugs. 

- **PMD**

- PMD scans Java source code and looks for potential problems like: 

- \* Unused local variables 
  \* Empty catch blocks 
  \* Unused parameters 
  \* Empty 'if' statements 
  \* Duplicate import statements 
  \* Unused private methods 
  \* Classes which could be Singletons 
  \* Short/long variable and method names

- Go To PMD.

- **Checkstyle**

- Checkstyle is a development tool to help programmers write Java code  that adheres to a coding standard. It automates the process of checking  Java code to spare humans of this boring (but important) task. This  makes it ideal for projects that want to enforce a coding standard.  Checkstyle is highly configurable and can be made to support almost any  coding standard. An example configuration file is supplied supporting  the Sun Code Conventions. As well, other sample configuration files are  supplied for other well known conventions. Can be integrated into  CruiseControl and Eclipse. Go To Checkstyle.

#### Two Ways of Installing Eclipse Plugin

Here we introduce two normal ways of installing Eclipse Plugins.

- **Install plugins online**

- All plugins can be installed with the standard update manager of Eclipse. Following is the procedure: 

- \* Choose the menu item "Help->Intall New Software".
  \* Click on "Add..." and enter a name  and the URL of the plugin.
  \* Now you can select the feature (plugin) you wish to install.
  \* Confirm the upcoming dialogs and restart Eclipse.

- **Install plugins offline**

- For Eclipse 3.4 or later, you can install downloaded plugins by copying the contents of plugins to the folder "dropins". And you can delete the plugins in folder "dropins" to uninstall plugins. After  installing or uninstalling, you need to restart Eclipse.

#### Steps                

1. Install one kind of the  three code analyzers plugins in Eclipse. The URL of the plugins is as follow:

| plugin name | URL                                                          |
| ----------- | ------------------------------------------------------------ |
| findBugs    | http://findbugs.cs.umd.edu/eclipse                           |
| PMD         | http://sourceforge.net/projects/pmd/files/pmd-eclipse/update-site/ |
| checkStyle  | http://eclipse-cs.sourceforge.net/update                     |

2. Create arbitrary sample Java project.

3. Begin to check Java code.

- \* Right-click  the entire project or some java file selected.

- \* Select the check option and click.

#### Deliverables

Send it to TA ([liuyuchen0921@zju.edu.cn](mailto:liuyuchen0921@zju.edu.cn)) before **2021.7.4 23:59:59**, with title: **SP2021-LAB3.1&3.2-ID-NAME**

- Your **lab** **report** will be the summary of detailed steps and observations. Some screenshots or photoes is included.
- All in one PDF document.