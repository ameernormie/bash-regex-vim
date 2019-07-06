## Learning Basics of Bash, Vim & Regex

### 1.1 Bash

#### Bash Scripts

---

whenever you find yourself typing same sequence of commands several times, consider making a script. Just the put the commands you would normally type into a file and add `#!/bin/bash` to the top of the file.

**_Naming_** <br/>
You can name your bash script without any extension. **e.g** If I want to make a script named **logger**, I can just make a file called **_logger_** and put my commands in it.
Bash scripts can have an extension of **sh** but it's not necessary.
e.g **logger.sh**

###### Sample Script (move in directory "home" and delete file `cool.txt`)

```
#!bin/bash
cd home && rm cool.txt
```

**_Permissions_** <br/>
You can not run bash script directly after creating them, first you have to give the script the permission to `execute`

Give the `executable` permission to the bash file.
Usually setting the **_executable_** bit on the file does the trick.

Give the permission by following command.

```
chmod +x <bash-file-name>
```

**_Execute Script_** <br/>
To run the bash script you need to provide a relative path for the script in command line. e.g **./logger**

**You can use arguments in your script.**
What ever argument you pass it will be passed to you as environment variables as `$1`, `$2`, `$3` and so on.

For the following script `$1` argument will be passed when the user will execute the script.

_argument_script.sh_

```
#!/bin/bash
echo $1
```

Running the above script with `./argument_script hello` , **hello** will be the argument passed.
If argument is multiple words like `hello world` , you can use backticks around the argument. Or you can use **\$\*** for the argument.

**READ** command <br/>
if you want to ask the user for input, there is a command called `read`. This sets the result in the environment variable.

For the following script, running the script will prompt the user for the input. This input will be saved in the environment variable and will be used from the name given to the variable.

```
#!/bin/bash
read INPUT
echo $INPUT
```

Whatever input you'll give will be printed out to the terminal as a result of the above script. <br/><br/><br/>

#### Permissions

---

###### There are three kinds of permissons for files and directories:

1. User (Owner of the file) represented by `u`
2. Group (Group of the file) represented by `g`
3. Other (everyone else) represented by `o`

To check the permissions use `ls -l` command.

**how to read permissions**
`-rwxr-xr-x` -> First 3 bits for `u`, Middle three bits for `g`, last bits for `o`. The `-` means the permission is not available.
`rwx -> read write executable`

Set permissions with `chmod`
**_In octal_**
644 -> ugo

```
#4 for write
#2 to read
#1 to execute
```

`-rwxr-xr-x`
Each character describes a permission. (r)ead, (w)rite, and e(x)cute.
A '-' in place of one of those letters mean the permission is not available.

**_x is the executable bit_**
=> If set for directory, that means you can list the directory contents
=> If set for file, that means you can run it

**Example of setting a permission.**
If I want to add a (w)rite permission to the group.
`chmod g+w <file>`. (Grant the permission)
`chmod g-w <file>`. (Revoke the permission)

<br/><br/><br/>

### 1.2 Regular Expressions

Regex is a pattern matching language. Many programming languages and system tools have a regex engine

**_System tools with regex:_**<br/>

- sed
- grep
- perl
- vim
- less

**_regex in javascript_**<br/>

- str.split(re)
- str.match(re)
- str.replace(re)
- re.test(str)
- re.exec(str)

##### 1.2.1 Flags

```
/PATTERN/FLAGS
s/PATTERN/REP/FLAGS
```

- `i` - case insensitive
- `g` - match all occurences (global)
- `m` - treat string as multiple lines
- `s` - treat string as single line

##### 1.2.2 Meta Characters

- `.` - matches any character
- `?` - zero or one time
- `*` - zero or more times
- `+` - one or more times
- `[]` - character class
- `^` - anchor at the beginning
- `$` - anchor to the end
- `(a|b)` - match `a` or `b`
- `()` - capture group
- `(?:)` - non capture group
- `\d` - digit `[0-9]`
- `\w` - word `[A-Za-z0-9_]`
- `\s` - whitespace `[ \t\r\n\f]`

**Examples**

```
echo 'hello beep boop' | sed 's/b..p/honk/'

Output: hello honk boop
```

```
echo 'hello beep boop' | sed 's/b..p/honk/g'

Output: hello honk honk
```

###### Wild card

`.` is the wild card

###### Quantifiers

- `?` - zero or one time
- `*` - zero or more times
- `+` - one or more times

how many times a thing should appear

**When to escape meta characters**
In some engines you need to escape metacharacters such as `+` and `?`. In others you don't.

In javascript and perl, you generally don't need to escape metacharacters. To use `grep` and `sed` in a similar way, use:

- `sed -r`
- `grep -E`
