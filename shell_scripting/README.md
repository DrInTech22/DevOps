
# Shell Scripting

An overview of shell scripting with projects 


## Shell
Shell is a command line interpreter that execute commands in Linux. It is an environment where we can run our commands.


## Types of shell
There several types of shell. Each ones has a unique properties. Major ones are:

- Bourne-Again Shell (bash)
- Bourne Shell (sh)
- C Shell (csh)
- Korn Shell (ksh)

## Shell script
Shell script is simply a list of commands to be run by the shell.
## Shebang
Shebang is a special line that tells the OS which shell should be used to execute scripts. E.g

```bash
#!/bin/bash
echo "hello world"
```


## Assigning variables
- Assigning text to a variable;

    Name="Adroit"
- Assigning commands to a variable;

      List="$(ls)"
- Declaring variables in script;

    ```echo $Name```  or

    ```echo ${List}```

## Conditionals
Conditionals enable the execution block of commands when certain conditions are met and a different block of commands when those requirements are not.
- Types

    If statement

    Case statement


## Loops
It executes set of commands repeatedly until 
- Types

    For loop

    While loop

    Until loop

## How to run shell scripts
Make shell script executable, then run;

```bash
echo "ls -a" > list.sh
chmod +x list.sh  #make script executable
./list.sh  #run script
``` 
