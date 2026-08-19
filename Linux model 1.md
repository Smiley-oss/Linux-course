# Linux Model 1 — Homework Report

Course: Linux (Johanna Heinonen)

Student: [Your name]
Date: [YYYY-MM-DD]

## 1. Objectives
- Get familiar with the Linux command line and basic shell utilities.
- Learn working with files and directories, permissions, and simple process management.
- Practice using common tools: ls, cp, mv, rm, cat, less, grep, chmod, chown, ps, kill, top, tar, gzip.

## 2. Environment
- Distribution: [e.g., Ubuntu 22.04 / Fedora 38 / Arch]
- Kernel version: `$(uname -r)`
- Shell: [e.g., bash, zsh]

Commands I ran to capture environment:

```bash
# show kernel and uname
uname -a

# distribution and release (example)
cat /etc/os-release

# shell
echo $SHELL
```

## 3. Tasks and steps

### Task A — Files & directories
1. Create a directory structure for the assignment:

```bash
mkdir -p ~/linux-models/model1/{data,scripts,notes}
cd ~/linux-models/model1
```

2. Create example files and inspect them:

```bash
echo "Hello Linux" > data/example.txt
ls -la data
cat data/example.txt
```

3. Copy, move, and remove files:

```bash
cp data/example.txt data/example-copy.txt
mv data/example-copy.txt data/old-example.txt
rm data/old-example.txt
```

### Task B — Searching and viewing
- Use grep and less to find and view content:

```bash
grep -n "Hello" -R .
less data/example.txt
```

### Task C — Permissions and ownership
1. Inspect permissions:

```bash
ls -l data
```

2. Change permissions and ownership (if permitted):

```bash
chmod 644 data/example.txt
# sudo chown $USER:$USER data/example.txt
```

### Task D — Processes and system info
- List processes and resource usage:

```bash
ps aux | head -n 10
top -b -n 1 | head -n 20
```

- Kill a process (example):

```bash
# find PID
ps aux | grep some_process
kill <PID>
```

### Task E — Archiving and compression

```bash
tar -czvf model1-data.tar.gz data/
ls -lh model1-data.tar.gz
```

## 4. Results / Output
- (Paste command outputs or short summaries here.)

Example:

```
$ ls -la data
-rw-r--r-- 1 student student 12 Aug 19 10:00 example.txt
```

## 5. Reflection and notes
- What went well:
  - I could navigate the filesystem and use core commands.
- What was challenging:
  - Remembering exact options for tar and grep; I used the man pages.
- Follow-ups / questions for the instructor:
  - [Any questions you have]

## 6. References
- man pages (e.g., `man ls`, `man grep`)
- The course slides and recommended readings

---

*End of Linux Model 1 homework template.*
