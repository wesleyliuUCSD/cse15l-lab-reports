# Lab 1 Report
This lab explored various terminal commands we will be looking at below.

## cd
An example with no arguments.
```
[user@sahara ~]$ cd 
[user@sahara ~]$ 
```
**Working directory**: ~/\
The output makes sense, as the command **cd** without any arguments simply changes the working directory to ~/, and as the working directory was already ~/, seemingly nothing happens. A better example would be below where the working directory changes after using **cd** without any arguments.\
*This is not an error output*

```
[user@sahara ~/lecture1]$ cd
[user@sahara ~]$ 
```

An example with a path to a directory as an argument.
```
[user@sahara ~]$ cd lecture1/
[user@sahara ~/lecture1]$
```
**Working directory**: ~/lecture1\
As cd changes the directory, by giving it an argument to the path to a directory, the working directory is changed to the path given in the argument, as shown above.\
*This is not an error output*

An example with a file as an argument.
```
[user@sahara ~/lecture1]$ cd Hello.java
bash: cd: Hello.java: Not a directory
[user@sahara ~/lecture1]$ 
```
**Working directory**: ~/\
This makes sense as cd is meant to change the working directory, but as the argument given was a file, it cannot change the directory to a file, and thus throws an error saying our file is not a directory.\
*This is an error output*

## ls
An example with no arguments.
```
[user@sahara ~/lecture1]$ ls
Hello.class  Hello.java  messages  README
[user@sahara ~/lecture1]$ 
```
**Working directory**: ~/lecture1\
This output makes sense, as ls shows information on files or directories that are in the directory or a given file, and as we gave no input it uses the current working directory, which are those files above we got from cloning the github repo we were given, as we are in the directory of the cloned repo.\
*This is not an error output*

An example with a path to a directory as an argument.
```
[user@sahara ~/lecture1]$ ls ./messages
en-us.txt  es-mx.txt  fi.txt  zh-cn.txt
[user@sahara ~/lecture1]$ 
```
**Working directory**: ~/lecture1\
The output above simply displays what files or directories are inside the path given as an argument, as we gave a path to a directory. Currently this shows the files that are in our messages directory, which are the 4 .txt files above.\
*This is not an error output*

An example with a file as an argument.
```
[user@sahara ~/lecture1]$ ls Hello.java
Hello.java
[user@sahara ~/lecture1]$ 
```
**Working directory**: ~/lecture1\
As we gave the path to a file as an argument, ls will give us information on the file. As we gave no additional parameters, it simply returns to us the name of the file. Since we used Hello.java, it makes sense that it returns Hello.java to us.\
*This is not an error output*

## cat
An example with no arguments.
```
[user@sahara ~/lecture1]$ cat
^C
[user@sahara ~/lecture1]$ 
```
**Working directory**: ~/lecture1\
As cat is typically used to display the contents of a file, or interacting with the contents of a file, by giving it no arguments we are stuck in an output screen with no output, and eventually we will need to exit out with ctrl+c. This output makes sense as we did not give it a file to interact with\
*This is not an error output*

An example with a directory argument.
```
[user@sahara ~/lecture1]$ cat messages/
cat: messages/: Is a directory
[user@sahara ~/lecture1]$ 
```
**Working directory**: ~/lecture1\
Since cat is used to interact with files, by giving cat a directory argument it ends up giving us the message that ./messages/ is simply a directory and nothing more, as it does nothing else meaningful to directories.
\
*This is not an error output*

An example with a file argument.
```
[user@sahara ~/lecture1]$ cat Hello.java
import java.io.IOException;
import java.nio.charset.StandardCharsets;
import java.nio.file.Files;
import java.nio.file.Path;

public class Hello {
  public static void main(String[] args) throws IOException {
    String content = Files.readString(Path.of(args[0]), StandardCharsets.UTF_8);    
    System.out.println(content);
  }
[user@sahara ~/lecture1]$ 
```
**Working directory**: ~/lecture1\
By giving cat a file output, it prints out the data inside the file. In this case, we used Hello.java which simply printed out all of the java code inside.\
*This is not an error output*