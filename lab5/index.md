# Lab 5

## Part 1

**Original Post 1.**\
Hello! I am having some issues, when I run my test script test.sh, the juint tests give me a memory error, but I do not know what could be using so much memory in my code. Please help!

Here is my terminal output:
```
wesleyliu@Wesleys-MacBook-Pro lab7 % bash test.sh
JUnit version 4.13.2
..E
Time: 8.312
There was 1 failure:
1) testMerge2(ListExamplesTests)
java.lang.OutOfMemoryError: Java heap space
	at java.base/java.util.Arrays.copyOf(Arrays.java:3513)
	at java.base/java.util.Arrays.copyOf(Arrays.java:3482)
	at java.base/java.util.ArrayList.grow(ArrayList.java:237)
	at java.base/java.util.ArrayList.grow(ArrayList.java:244)
	at java.base/java.util.ArrayList.add(ArrayList.java:483)
	at java.base/java.util.ArrayList.add(ArrayList.java:496)
	at ListExamples.merge(ListExamples.java:42)
	at ListExamplesTests.testMerge2(ListExamplesTests.java:19)
	at java.base/java.lang.invoke.LambdaForm$DMH/0x0000007001012400.invokeVirtual(LambdaForm$DMH)
	at java.base/java.lang.invoke.LambdaForm$MH/0x0000007001013000.invoke(LambdaForm$MH)
	at java.base/java.lang.invoke.Invokers$Holder.invokeExact_MT(Invokers$Holder)

FAILURES!!!
Tests run: 2,  Failures: 1

```
It seems the error occured from testMerge2, which has this input:

```java
@Test()
        public void testMerge2() {
		List<String> l1 = new ArrayList<String>(Arrays.asList("a", "b", "c"));
		List<String> l2 = new ArrayList<String>(Arrays.asList("c", "d", "e"));
		assertArrayEquals(new String[]{ "a", "b", "c", "c", "d", "e" }, ListExamples.merge(l1, l2).toArray());
        }
```
**Response 2.**

Hello! First of all, I would reccomend writing tests with a timeout, like the sample below:
```java
@Test(timeout = 500)
        public void test1() {
            ...
        }
```
This allows you to tell if the error is due to an infinite loop or if it is actually a memory issue. Second, could you try a test case where while merging, l2 will be fully empty first and then l1? This will let us fully isolate what kind of cases are causing your error! Finally, it woul be great to see your code for this task!

**New post 3.**

Here is me running with the new timeout and the new test case:
```
wesleyliu@Wesleys-MacBook-Pro lab7 % bash test.sh
JUnit version 4.13.2
..E.
Time: 0.62
There was 1 failure:
1) testMerge2(ListExamplesTests)
org.junit.runners.model.TestTimedOutException: test timed out after 500 milliseconds
	at java.base/java.lang.System.arraycopy(Native Method)
	at java.base/java.util.Arrays.copyOf(Arrays.java:3515)
	at java.base/java.util.Arrays.copyOf(Arrays.java:3482)
	at java.base/java.util.ArrayList.grow(ArrayList.java:237)
	at java.base/java.util.ArrayList.grow(ArrayList.java:244)
	at java.base/java.util.ArrayList.add(ArrayList.java:483)
	at java.base/java.util.ArrayList.add(ArrayList.java:496)
	at ListExamples.merge(ListExamples.java:42)
	at ListExamplesTests.testMerge2(ListExamplesTests.java:19)

FAILURES!!!
Tests run: 3,  Failures: 1

```
It seems there may be an infinite loop as we reached the timeout! Furthermore testMerge3, which had the test case you reccomennded also passed! Therefore, we must for some reason be reaching an infinite loop only when l1 is empty first! We have while loops in our merge function, so it muse be one of those, and since the error only occurs when l1 is empty first, then the bug must be with the l2 loop! The bug is that instead of incrementing index2 we increment index1 and thus the 2nd while loop never breaks, creating an infinite loop!

**Part 4**

File structure needed:

lab7\
---ListExamples.java\
---ListExamplesTests.java\
---test.sh

The content in each file before editing is as follows:

ListExamples.java:
```java
import java.util.ArrayList;
import java.util.List;

interface StringChecker { boolean checkString(String s); }

class ListExamples {

  // Returns a new list that has all the elements of the input list for which
  // the StringChecker returns true, and not the elements that return false, in
  // the same order they appeared in the input list;
  static List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(0, s);
      }
    }
    return result;
  }


  // Takes two sorted list of strings (so "a" appears before "b" and so on),
  // and return a new list that has all the strings in both list in sorted order.
  static List<String> merge(List<String> list1, List<String> list2) {
    List<String> result = new ArrayList<>();
    int index1 = 0, index2 = 0;
    while(index1 < list1.size() && index2 < list2.size()) {
      if(list1.get(index1).compareTo(list2.get(index2)) < 0) {
        result.add(list1.get(index1));
        index1 += 1;
      }
      else {
        result.add(list2.get(index2));
        index2 += 1;
      }
    }
    while(index1 < list1.size()) {
      result.add(list1.get(index1));
      index1 += 1;
    }
    while(index2 < list2.size()) {
      result.add(list2.get(index2));
      // change index1 below to index2 to fix test
      index1 += 1;
    }
    return result;
  }


}
```
ListExamplesTests.java:
```java
import static org.junit.Assert.*;
import org.junit.*;
import java.util.*;
import java.util.ArrayList;


public class ListExamplesTests {
	@Test()
	public void testMerge1() {
    		List<String> l1 = new ArrayList<String>(Arrays.asList("x", "y"));
		List<String> l2 = new ArrayList<String>(Arrays.asList("a", "b"));
		assertArrayEquals(new String[]{ "a", "b", "x", "y"}, ListExamples.merge(l1, l2).toArray());
	}
	
	@Test()
        public void testMerge2() {
		List<String> l1 = new ArrayList<String>(Arrays.asList("a", "b", "c"));
		List<String> l2 = new ArrayList<String>(Arrays.asList("c", "d", "e"));
		assertArrayEquals(new String[]{ "a", "b", "c", "c", "d", "e" }, ListExamples.merge(l1, l2).toArray());
        }

}
```
test.sh:
```bash
javac -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar *.java
java -cp .:lib/hamcrest-core-1.3.jar:lib/junit-4.13.2.jar org.junit.runner.JUnitCore ListExamplesTests
```


Command to trigger the bug is:

```
bash test.sh
```

To Debug and Fix the bug:

First, add timeout timers to each of the tests so we can tell when infinite loops form as the time for the method to run will be much longer than expected. We can do this like so:
```java
@Test(timeout = 500)
        public void test1() {
            ...
        }
```
We then added a new test testMerge3 shown below to isolate the types of inputs which cause this error to only those who empty list 1 first.
```java
@Test(timeout = 500)
        public void testMerge3() {
		List<String> l2 = new ArrayList<String>(Arrays.asList("a", "b", "c"));
		List<String> l1 = new ArrayList<String>(Arrays.asList("c", "d", "e"));
		assertArrayEquals(new String[]{ "a", "b", "c", "c", "d", "e" }, ListExamples.merge(l1, l2).toArray());
        }
```

Finally, after all of that, we can figure out that we simply need to replace index1 with index2 in the 2nd while loop. The fixed code along with the change with comments is shown below

```java
import java.util.ArrayList;
import java.util.List;

interface StringChecker { boolean checkString(String s); }

class ListExamples {

  // Returns a new list that has all the elements of the input list for which
  // the StringChecker returns true, and not the elements that return false, in
  // the same order they appeared in the input list;
  static List<String> filter(List<String> list, StringChecker sc) {
    List<String> result = new ArrayList<>();
    for(String s: list) {
      if(sc.checkString(s)) {
        result.add(0, s);
      }
    }
    return result;
  }


  // Takes two sorted list of strings (so "a" appears before "b" and so on),
  // and return a new list that has all the strings in both list in sorted order.
  static List<String> merge(List<String> list1, List<String> list2) {
    List<String> result = new ArrayList<>();
    int index1 = 0, index2 = 0;
    while(index1 < list1.size() && index2 < list2.size()) {
      if(list1.get(index1).compareTo(list2.get(index2)) < 0) {
        result.add(list1.get(index1));
        index1 += 1;
      }
      else {
        result.add(list2.get(index2));
        index2 += 1;
      }
    }
    while(index1 < list1.size()) {
      result.add(list1.get(index1));
      index1 += 1;
    }
    while(index2 < list2.size()) {
      result.add(list2.get(index2));
      // change index1 below to index2 to fix test
      index2 += 1;//THIS WAS CHANGED FROM index1 to index2
    }
    return result;
  }


}
```

## Part 2

Reflection

One cool thing I learned was how to is jdb. I have coded with java a little bit before but I have never used a tool like jdb, and it seems very helpful for debugging java, especially when it allows you to set stop points and check out local variables to see if they match with what you expect at that point. Normally to debug I would probably print out all of the local variables I wanted to check but this makes it much faster and cleaner. The step feature is also very nice as it allows you to follow changes while your code is running one step at a time. This is especially useful when the code jumps to other methods which may be confusing without this tool. All in all, jdb is quite a nice tool to use.