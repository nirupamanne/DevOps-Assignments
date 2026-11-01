# Assignment – Vim Editor

## Assignment Questions

1. Create `practice.txt` using vim. Add 10 lines of content, save and exit.
2. Open the file. Turn on line numbers, jump to line 7, delete line 3, save and exit.
3. Open the file. Search for a word, move through its occurrences, then clear the highlight.
4. Replace every occurrence of one word in the file with another — in one command.
5. Go to the bottom of the file, add 3 lines, then undo the last one. Save.
6. Copy one line and paste it 5 times in a single step. What is the command?

---

## Solutions

### Step 1: Create practice.txt using Vim and add 10 lines

**Command:**

```bash
vim practice.txt
```

**Steps:**

1. Press `i` to enter Insert mode.
2. Add the following 10 lines:

```text
Linux is an operating system.
Linux is widely used in DevOps.
Vim is a powerful text editor.
We use Vim to edit files.
Linux supports multiple users.
We can manage files using commands.
Vim has different operating modes.
We can search text using Vim.
We can replace words using Vim.
Practice makes Linux learning easier.
```

3. Press `Esc` to exit Insert mode.
4. Type `:wq` and press Enter.

**Explanation:**

- `vim practice.txt` – Opens or creates a file.
- `i` – Enters Insert mode.
- `Esc` – Exits Insert mode.
- `:w` – Saves the file.
- `:q` – Exits Vim.
- `:wq` – Saves and exits Vim.

**Verify the file:**

```bash
cat practice.txt
```
![Vim Practice Editor](<vim practice editor.png>)

### Step 2: Turn on line numbers, jump to line 7, and delete line 3

**Command:**

```bash
vim practice.txt
```

**Steps:**

1. Press `Esc` to enter Normal mode.
2. Type the following command and press Enter:

```vim
:set number
```

3. Jump to line 7:

```vim
7G
```

4. Delete line 3:

```vim
:3d
```

5. Save and exit:

```vim
:wq
```

**Explanation:**

- `:set number` – Displays line numbers.
- `7G` – Moves the cursor to line 7.
- `:3d` – Deletes line 3.
- `:wq` – Saves and exits Vim.
  
![Third Line Is Deleted](<third line is deleted.png>)

### Step 3: Search for a word, move through occurrences, and clear the highlight

**Command:**

```bash
vim practice.txt
```

**Steps:**

1. Press `Esc` to enter Normal mode.
2. Search for the word `Linux`:

```vim
/Linux
```

3. Press Enter.
4. Press `n` to move to the next occurrence.
5. Press `N` to move to the previous occurrence.
6. Clear the search highlight:

```vim
:nohlsearch
```

**Explanation:**

- `/Linux` – Searches for the word Linux.
- `n` – Moves to the next matching occurrence.
- `N` – Moves to the previous matching occurrence.
- `:nohlsearch` – Clears the search highlighting.

![Vim Linux 3](<vim Linux3.png>)

### Step 4: Replace every occurrence of one word with another in one command

**Command:**

```vim
:%s/Linux/AWS/g
```

**Explanation:**

- `%` – Applies the replacement to the entire file.
- `s` – Substitute command.
- `Linux` – Word to search for.
- `AWS` – New word to replace it with.
- `g` – Replaces all occurrences in each line.

**Example:**

Before replacement:

```text
Linux is an operating system.
Linux is widely used in DevOps.
```

After replacement:

```text
AWS is an operating system.
AWS is widely used in DevOps.
```
![Replace One Word](<replace one word 4.png>)

**Note:** Run the replacement command in Vim's Normal mode by first pressing `Esc`.

### Step 5: Go to the bottom, add 3 lines, undo the last one, and save

**Command:**

```bash
vim practice.txt
```

**Steps:**

1. Press `Esc` to enter Normal mode.
2. Go to the bottom of the file:

```vim
G
```

3. Add the first line:

```vim
o
```

Type:

```text
This is the first new line.
```

Press `Esc`.

4. Add the second line using `o` again:

```text
This is the second new line.
```

Press `Esc`.

5. Add the third line using `o` again:

```text
This is the third new line.
```

Press `Esc`.

6. Undo the last line:

```vim
u
```

7. Save the file:

```vim
:w
```
![Third Line Is Deleted](<third line is deleted.png>)

**Explanation:**

- `G` – Moves the cursor to the bottom of the file.
- `o` – Opens a new line below and enters Insert mode.
- `Esc` – Returns to Normal mode.
- `u` – Undoes the last change.
- `:w` – Saves the file.

**Important:** Press `Esc` after adding each line. This makes each new line a separate change, so `u` removes only the third line.

### Step 6: Copy one line and paste it 5 times in a single step

**Command:**

```vim
yy5p
```

**Steps:**

1. Open the file:

```bash
vim practice.txt
```

2. Move the cursor to the line you want to copy.
3. Press `Esc` to enter Normal mode.
4. Type:

```vim
yy
```

5. Paste the copied line 5 times:

```vim
5p
```

**Explanation:**

- `yy` – Copies (yanks) the current line.
- `p` – Pastes the copied line below the current line.
- `5p` – Pastes 5 copies of the line.

**Answer:** `yy5p` is the combined command to copy the current line and paste five copies below it.

![Paste 5 Times](<paste 5times.png>)

---

## Quick Reference – Vim Commands

| Command | Purpose |
|---|---|
| `vim practice.txt` | Opens or creates a file |
| `i` | Enters Insert mode |
| `Esc` | Returns to Normal mode |
| `:w` | Saves the file |
| `:q` | Exits Vim |
| `:wq` | Saves and exits |
| `:set number` | Displays line numbers |
| `7G` | Moves to line 7 |
| `:3d` | Deletes line 3 |
| `/Linux` | Searches for Linux |
| `n` | Moves to the next occurrence |
| `N` | Moves to the previous occurrence |
| `:nohlsearch` | Clears search highlighting |
| `:%s/Linux/AWS/g` | Replaces all occurrences of Linux with AWS |
| `G` | Moves to the bottom of the file |
| `o` | Opens a new line below |
| `u` | Undoes the last change |
| `yy` | Copies the current line |
| `5p` | Pastes the copied line 5 times |

## Assignment Completed
