# Lab 4 Report


## Step 4:

Keys pressed:

```cmd+C``` this command:


    ssh wyl002@ieng6-201.ucsd.edu


```cmd+v <enter>``` 

Command:
![](./ssh1.png)
Result:
![](./ssh2.png)
This logs us into ieng6 as we have already set up an access key so no password is needed.

## Step 5:

```cs1<tab> <enter>``` 

Command:
![](./cs1.png)
Result:
![](./cs2.png)

This helps us set up our ieng6 before we continue, running the cs15lw24 command


```cmd+C``` this clone url: 

    git@github.com:wesleyliuUCSD/lab7.git


```git clo<tab><cmd+v><enter>``` 

Command:
![](./clone1.png)
Result:
![](./clone2.png)

This clones our already forked repository to the ieng6 computer using the SSH url.

## Step 6:

```cd l<tab> <enter>```

Command:
![](./cd1.png)
Result:
![](./cd2.png)

This changes our working directory to the forked repository we just cloned, so we can directly work on the files

```bash t<tab><enter>```

Command:
![](./bashfail1.png)
Result:
![](./bashfail2.png)

This runs the bash file test.sh so we can see the results of running it without making changes, this gives us several errors.

## Step 7:

```vim L<tab>.<tab><enter>```

Command:
![](./vim1.png)
Result:
![](./vim2.png)

This command opens up vim on ListExamples.java, allowing us to fix the bugs on the file

```?ind<enter>``` 

Command (the screen updated with each input so it is hard to show before the command is sent, to see that look at the previous image):
![](./search1.png)
Result:
![](./search2.png)

This command searches for the last occurance of the string "ind" which is the location of our bug

```er2:wq```

Result (to see before, look at previous image):
![](./replace.png)



The e command jumps to the end of "index1" as we only need to replace the "1" and not the rest of the string, and then r command begins a replace on the end of the string, or the "1" character. The 2 input does the replacement, swiching 1 with 2, and finally :wq saves and exits out of the program

## Step 8:

```bash t<tab><enter>``` 

Command:
![](./bashsuc1.png)
Result:
![](./bashsuc2.png)

This runs the bash file test.sh so we can see the results of running it after making changes, this gives us no errors.

## Step 9:

```git add -u```

Command:
![](./add1.png)
Result:
![](./add2.png)

This command adds all of our updated files (not new ones like compiled java files which we do not need) to be committed.

```git com<tab> -m "c"```

Command:
![](./commit1.png)
Result:
![](./commit2.png)

This commits all of the changes we added before, with a commit message. The -m option lets us set the commit message in the same like, and here we set it to string "c"

```git push```

Command:
![](./push1.png)
Result:
![](./pushed.png)

This command pushes our commits to the remote repository, letting us store our changes onto github.
