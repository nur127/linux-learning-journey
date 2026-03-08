# Day 01 - Basic Linux Commands

Today I practiced the following Linux commands to get familiar with navigation, directory & file management, and viewing file contents.  
These commands are foundational for Linux, Cloud Engineering, and DevOps.

---

## 1️⃣ Navigation Commands

### pwd
Prints the current working directory.

```bash
pwd
```

Example output:

```text
/home/user/linux-learning-journey/Day01
```

### ls
Lists files and directories in the current directory.

```bash
ls
ls -l
ls -a
ls -la
```

Example output:

```text
Day01  Day02  README.md  .gitignore
```

### cd
Change directory.

```bash
cd Day01       # go into a folder
cd ..          # move one directory up
cd ~           # go to home directory
cd /home/user/linux-learning-journey
```

---

## 2️⃣ Directory Management

### mkdir
Create a new directory.

```bash
mkdir new_folder
mkdir -p parent_folder/child_folder   # nested directories
```

Example output:

```text
# directory created, nothing is printed
```

### rmdir
Remove an empty directory.

```bash
rmdir foldername
```

Example output:

```text
# directory removed, nothing is printed
```

---

## 3️⃣ File Management

### rm
Remove files or directories.

```bash
rm file.txt          # remove a file
rm -r foldername     # remove directory recursively
rm -f file.txt       # force remove file without prompt
```

Example output:

```text
# file or folder removed
```

### touch
Create an empty file or update the timestamp.

```bash
touch newfile.txt
```

Example output:

```text
# file created, nothing is printed
```

---

## 4️⃣ Viewing File Contents

### cat
Display the contents of a file.

```bash
cat file.txt
```

Example output:

```text
Hello World!
This is a test file.
```

### zcat
View contents of compressed files (gzip).

```bash
zcat file.txt.gz
```

Example output:

```text
Hello World!
This is a compressed file.
```

### head
View the first few lines of a file.

```bash
head file.txt
head -n 5 file.txt
```

Example output:

```text
Hello World!
This is a test file.
Line 3
Line 4
Line 5
```

### tail
View the last few lines of a file.

```bash
tail file.txt
tail -n 5 file.txt
```

Example output:

```text
Line 6
Line 7
Line 8
Line 9
Line 10
```

#### tail -f
Monitor a file in real-time (useful for logs).

```bash
tail -f file.txt
```

Example output (will keep updating as the file changes):

```text
Line 10
Line 11
Line 12
```

### less / more
View long files page by page.

```bash
less file.txt
more file.txt
```

- less → scroll forward and backward
- more → scroll forward only

Example usage:

```text
Hello World!
This is line 2
This is line 3
...
Press SPACE to continue...
```

---
