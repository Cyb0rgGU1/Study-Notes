# _BASH_
_Any command that you can run in the terminal can be run in a Bash script._

`#!/bin/bash`
 - Script should begin with this to help the computer tell which type of interpreter to use. 

`~/bin/`
- When saving a script, place used scripts in said directory as a good practice. 

`chmod +x script.sh`
- Ensure that the script has permission to execute itself. 

`~/.bashrc` or `~/.bash_profile`
- Terminal runs a file every time it is opened to load a configuration. **Linux** style shells, this is`~/.bashrc` while **OSX** is `~/.bash_profile`.

> To ensure that any scripts within the `~/.bin/` are available for use, you need to add the directory to your path within the configuration file. 
- `PATH=~/bin:$PATH`

**_VARIABLES_**

`greeting='Hello'`
- Variables are declared by setting the variable name equal to anothe value. **Note that there is no space between the variable name.**

**_CONDITIONALS_**

![Conditional](https://www.dezlearn.com/wp-content/uploads/2021/03/B3.jpg)

> Make sure you leave a space between a bracket and the conditional statement!

**_Comparison operators for NUMBERS_**
| -eq | -ne | -le | -lt | -ge | -gt | -z |
| ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| Equal | Not equal | Less than or equal | Less than | Great than or equal | Greater than | Null |

**_Comparison operators for STRINGS_**
| == | != |
| ----- | ----- |
|Equal| Not equal |

> When comparing **strings**, it's for the best to use " as it prevents errors if it is null or contains spaces. 
- `if [ "$foo" == "$bar" ]`

**LOOPS**

_There are **three** different types of loops within a bash script._

- `For`
  - used to iterate through a list and execute an action at each step.

`for word in $paragraph`
`do`
`echo $word`
`done`




































