# Day 02 - File Ops & Editors (cp, mv, wc, vi, ln, cut, tee, sort, clear, diff)

Today I practiced the following Linux commands to get familiar with file copying/moving, counting, editing, linking, text processing, sorting, clearing the terminal, and comparing files.  
These commands are essential for Linux usage, scripting, and DevOps workflows.

---

## 1️⃣ Copy & Move

### cp
Copy files or directories.

```bash
cp source.txt dest.txt            # copy file
cp -r src_folder/ dest_folder/    # copy directory recursively
cp -i file.txt file.bak           # interactive (ask before overwrite)
```

Example output:
```text
# no output on success; dest.txt created/overwritten
```

### mv
Move or rename files and directories.

```bash
mv oldname.txt newname.txt        # rename
mv file.txt /path/to/destination/ # move
mv -i file.txt /dest/             # interactive
```

Example output:
```text
# no output on success; file moved/renamed
```

---

## 2️⃣ Count Words/Lines/Bytes

### wc
Count lines, words, and bytes.

```bash
wc file.txt               # prints lines, words, bytes, filename
wc -l file.txt            # only lines
wc -w file.txt            # only words
wc -c file.txt            # only bytes
```

Example:
```text
$ wc -l notes.txt
42 notes.txt
```

---

## 3️⃣ vi Editor (basic)

Open and edit files with vi/vim.

```bash
vi file.txt
```

Basic commands (in vi):
- i → enter insert mode
- ESC → exit insert mode
- :w → save
- :q → quit
- :wq → save and quit
- :q! → quit without saving
- dd → delete current line
- yy → yank (copy) line
- p → paste

---

## 4️⃣ Links: ln (hard & soft/symbolic)

### Hard link
Creates another directory entry pointing to the same inode.

```bash
ln original.txt hardlink.txt
```

### Symbolic (soft) link
Creates a pointer to the original file.

```bash
ln -s /path/to/original.txt symlink.txt
```

Notes:
- Hard links cannot span filesystems and cannot link directories (without special options).
- Removing the original still leaves a hard link valid; removing original breaks a symlink.

---

## 5️⃣ Text Processing: cut, tee, sort

### cut
Extract columns/fields from lines.

```bash
cut -d',' -f1,3 file.csv        # fields 1 and 3 using comma delimiter
cut -c1-10 file.txt             # characters 1-10
```

Example:
```text
$ cut -d':' -f1 /etc/passwd
root
daemon
bin
...
```

### tee
Read from stdin and write to file(s) and stdout (useful in pipelines).

```bash
echo "log line" | tee logfile.txt
command | tee out.txt | grep something
tee -a logfile.txt    # append
```

### sort
Sort lines of text.

```bash
sort file.txt                  # sort lexicographically
sort -n numbers.txt            # numeric sort
sort -r file.txt               # reverse
sort -u file.txt               # unique lines
```

Example:
```text
$ sort names.txt
Alice
Bob
Charlie
```

---

## 6️⃣ clear
Clear the terminal screen.

```bash
clear
```

Effect: empties the terminal view (no output, just clears screen).

---

## 7️⃣ diff
Compare files line-by-line.

```bash
diff file1.txt file2.txt         # show differences
diff -u file1.txt file2.txt      # unified format (good for patches)
```

Example output (unified):
```diff
--- file1.txt 2026-03-08 10:00:00.000000000 +0000
+++ file2.txt 2026-03-08 10:01:00.000000000 +0000
@@ -1,3 +1,3 @@
-Hello world
+Hello World!
 This is a test
 Another line
```

---

## Quick Tips
- Combine commands with pipes (|) for powerful one-liners: cat file | cut -d',' -f2 | sort -u
- Use -i with cp/mv/rm to avoid accidental overwrites.
- Use tee when you want to both save pipeline output and see it on screen.
- Use diff -u when preparing patches or code reviews.

---
