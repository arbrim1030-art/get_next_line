*This project has been created as part of the 42 curriculum by asulejma.*

# Description

`get_next_line` is a project from the 42 curriculum. The goal is to create a function that reads and returns a file one line at a time.

The main function is:

```c
char *get_next_line(int fd);
```

Each time the function is called, it returns the next line from the file descriptor. The `\n` character is included when it is present.

The project focuses on file descriptors, the `read()` function, dynamic memory allocation, string manipulation and static variables.

# Instructions

## Compilation

The project can be compiled with:

```bash
cc -Wall -Wextra -Werror -D BUFFER_SIZE=42 main.c get_next_line.c get_next_line_utils.c -o gnl
```

`BUFFER_SIZE` determines how many characters are read at a time. It can be changed to test the function with different values.

For example:

```bash
cc -Wall -Wextra -Werror -D BUFFER_SIZE=1 main.c get_next_line.c get_next_line_utils.c -o gnl
```

## Execution

Create a file called `test.txt`, then run:

```bash
./gnl
```

The test program opens the file and repeatedly calls `get_next_line()` until there are no more lines.

# Algorithm

The implementation uses a **static variable**, called a stash, to keep the data that has been read but has not yet been returned.

The algorithm works as follows:

1. `read()` reads a block of data from the file descriptor using `BUFFER_SIZE`.
2. The data read is added to the stash.
3. The stash is searched for a newline character (`\n`).
4. If no newline is found, another block is read and added to the stash.
5. When a newline is found, the line is copied and returned.
6. Any data after the newline remains in the stash.
7. On the next call, the remaining data is used before reading more from the file.
8. When `read()` reaches the end of the file, the remaining characters are returned as the last line.

For example, if the stash contains:

```text
Hello\nWorld\n
```

the first call returns:

```text
Hello\n
```

and keeps:

```text
World\n
```

for the next call.

### Why this algorithm?

A single call to `read()` can contain several lines or only part of a line. The stash makes it possible to keep unused data between calls without losing it.

This also allows the function to work correctly with different `BUFFER_SIZE` values.

### Complexity

For a line containing `n` characters, the amount of memory used is `O(n)`.

Because the stash may be repeatedly copied when new data is added, the worst-case time complexity can be `O(n²)` for a long line with a small `BUFFER_SIZE`.

# Resources

## Documentation

* `man read` — information about the `read()` system call.
* `man open` — information about opening files.
* `man close` — information about closing file descriptors.
* `man malloc` — information about dynamic memory allocation.
* `man free` — information about freeing allocated memory.
* 42 `get_next_line` subject.

## AI Usage

AI was used during the project as a learning and debugging tool.

It was used to:

* Understand the requirements of the project.
* Understand file descriptors, `read()` and static variables.
* Help identify potential memory leaks and errors.
* Review and debug helper functions.
* Help organize and write the README.

The implementation was written and tested by asulejma, with AI used as an assistance tool rather than as a replacement for understanding the code.
