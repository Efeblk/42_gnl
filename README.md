# get_next_line

A C function that reads one line at a time from a file descriptor. This 42 project repository contains both the mandatory and bonus source variants.

```c
char *get_next_line(int fd);
```

Each returned line is allocated on the heap and includes its trailing newline when one is present. The caller must free it. `NULL` indicates that no line remains or that a descriptor/read check failed.

## Source variants

| Variant | Files | Buffered state |
| --- | --- | --- |
| Mandatory | `get_next_line.c`, `get_next_line_utils.c`, `get_next_line.h` | One static buffer |
| Bonus | `get_next_line_bonus.c`, `get_next_line_utils_bonus.c`, `get_next_line_bonus.h` | Separate static buffers indexed by file descriptor |

Use one variant at a time. Both export the same function and helper names, so they should not be linked together. Use the bonus variant when alternating between multiple file descriptors.

## Build and usage

Requires a C compiler and a system providing POSIX `read` and the headers included by the project. There is no root Makefile or bundled application entry point.

```sh
git clone https://github.com/Efeblk/42_gnl.git
cd 42_gnl
```

`BUFFER_SIZE` is the number of bytes requested per read. The headers do not define a default, so supply a positive value when compiling.

Save this example as `main.c` in the repository root:

```c
#include "get_next_line.h"
#include <stdio.h>

int main(void)
{
    char *line;

    line = get_next_line(STDIN_FILENO);
    while (line != NULL)
    {
        fputs(line, stdout);
        free(line);
        line = get_next_line(STDIN_FILENO);
    }
    return (0);
}
```

Compile the mandatory sources and read a text file through standard input:

```sh
gcc -Wall -Wextra -Werror -D BUFFER_SIZE=42 \
    main.c get_next_line.c get_next_line_utils.c -o example
./example < path/to/file.txt
```

For the bonus variant, include `get_next_line_bonus.h` in `main.c` and compile:

```sh
gcc -Wall -Wextra -Werror -D BUFFER_SIZE=42 \
    main.c get_next_line_bonus.c get_next_line_utils_bonus.c -o example_bonus
```

The bonus source uses `OPEN_MAX` from `<limits.h>`; the build environment must provide that constant. File descriptors used with this variant must be below `OPEN_MAX`, because they index its static buffer array.

## Repository guide

- `get_next_line*.c` — reading, line extraction, and leftover-buffer handling.
- `get_next_line_utils*.c` — string and allocation helpers.
- `get_next_line*.h` — declarations and required headers.
- [en.subject.pdf](en.subject.pdf) — the project subject included in this repository.
- `gnlTester/` — bundled third-party tester; see its [README](gnlTester/README.md) for mandatory and bonus test commands.

## Credits

The source file headers credit `jdecorte`. The bundled tester includes its own documentation and credits; preserve those when reusing the project.
