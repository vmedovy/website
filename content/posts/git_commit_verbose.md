---
author: "Vladimir Medovy"
date: 2021-04-05
lastmod: 2021-04-05
title: "Improve your `git commit` experience"

tags: ["Git", "VCS"]
categories: ["Tips and Tricks", "Productivity"]
---

Who of us has not been there: After staging your changes and typing 
```bash
git commit
```
in your command prompt, you are already not certain about the actual commit contents anymore.
Did you really stage that one line in that one file?
What information should you provide with your commit message?

Here is a convenient solution to make this task that much more convenient.

## TL;DR

To get

```diff
 
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
#
# Initial commit
#
# Changes to be committed:
#       new file:   main.cpp
#
# ------------------------ >8 ------------------------
# Do not modify or remove the line above.
# Everything below it will be ignored.
diff --git a/main.cpp b/main.cpp
new file mode 100644
index 0000000..4f9de56
--- /dev/null
+++ b/main.cpp
@@ -0,0 +1,7 @@
+#include <iostream>
+
+int main()
+{
+    std::cout << "Hello, World!" << std::endl;
+    return 0;
+}
```

instead of

```diff
 
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
#
# Initial commit
#
# Changes to be committed:
#       new file:   main.cpp
#
```

when committing, either add the option `-v` on every commit

```bash
git commit -v
```

or run the following once to make this option the new default

```bash
git config --global commit.verbose true
```

If you want to know more details, read on. :nerd_face:

## Introduction
To show case the functionality, let us look at adding a file to an empty repo.
Such a repo can be created in an empty directory by simply calling:

```bash
git init
```

The file content is actually not important.
For fun, we will create a file called `main.cpp` containing a very simple C++ program.
If you would like to follow along, you can copy and paste the following lines into the file:

```cpp
#include <iostream>

int main()
{
    std::cout << "Hello, World!" << std::endl;
    return 0;
}

```

Now we can call 

```bash
git add main.cpp
```

to stage the new file.

## Problem

If you now run

```bash
git commit
```

Git will open an editor with the following content for you to enter your commit message:

```diff
 
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
#
# Initial commit
#
# Changes to be committed:
#       new file:   main.cpp
#
```

This does not provide you with any information on the actual content of the file.
The only information you get, is that the file `main.cpp` was added.
If you would like to get more context information, you would have to abort the commit and call

```bash
git diff --staged
```

But there is a better solution.

## Solution

You just need to add the additional option `-v` to your call:

```bash
git commit -v
```

This appends the file diff to the end of the content of the commit message prompt:

```diff
 
# Please enter the commit message for your changes. Lines starting
# with '#' will be ignored, and an empty message aborts the commit.
#
# On branch master
#
# Initial commit
#
# Changes to be committed:
#       new file:   main.cpp
#
# ------------------------ >8 ------------------------
# Do not modify or remove the line above.
# Everything below it will be ignored.
diff --git a/main.cpp b/main.cpp
new file mode 100644
index 0000000..4f9de56
--- /dev/null
+++ b/main.cpp
@@ -0,0 +1,7 @@
+#include <iostream>
+
+int main()
+{
+    std::cout << "Hello, World!" << std::endl;
+    return 0;
+}
```

This way you know exactly what the content of the commit is, which helps you write better commit messages and avoid mistakes during commits.

For me, there is no reason to use the commit command without the `-v` option.
If you do not need the provided information in some situations, you do not have to look at it.
After all, it is appended to the normal prompt and the commit message is entered starting on line 1.
But in those cases where you actually need context, it is there just waiting to be read. :smile:

To make this option the new default, you can modify your global `.gitconfig` by calling

```bash
git config --global commit.verbose true
```
