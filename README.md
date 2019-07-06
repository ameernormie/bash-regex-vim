## Learning Basics of Bash, Vim & Regex

### Bash

#### Bash Scripts

---

whenever you find yourself typing same sequence of commands several times, consider making a script. Just the put the commands you would normally type into a file and add `#!/bin/bash` to the top of the file.

**_Naming_**
You can name your bash script without any extension. **e.g** If I want to make a script named **logger**, I can just make a file called **_logger_** and put my commands in it.
Bash scripts can have an extension of **sh** but it's not necessary.
e.g **logger.sh**

###### Sample Script (move in directory "home" and delete file `cool.txt`)

```
#!bin/bash
cd home && rm cool.txt
```

**_Permissions_**
You can not run bash script directly after creating them, first you have to give the script the permission to `execute`

Give the `executable` permission to the bash file.
Usually setting the **_executable_** bit on the file does the trick.

Give the permission by following command.

```
chmod +x <bash-file-name>
```

**_Execute Script_**
To run the bash script you need to provide a relative path for the script in command line. e.g **./logger**
