# CodeLines
[![Build status](https://ci.appveyor.com/api/projects/status/oach5ro1fk86xa23?svg=true)](https://ci.appveyor.com/project/waiting/codelines)


A tool for counting lines of code, which can also be used to remove comments and blank lines from code. Supports all C-family programming languages.


## 命令详细

    CodeLines [--m] [--l] [--r] [--s] [--v] [--j] [--help] ext1 [ext2] [ext3] ... <-|+> search_path ... [-o output_path[</|:>{name}.{ext}]]

    --m:
        Count the number of lines containing comments
    --l:
        Count the number of blank lines
    --r:
        Use regular expressions
    --s:
        Silent mode
    --v:
        Display detailed warnings per code line
    --j:
        Output JSON-formatted results
    --help:
        显示帮助
    ext1 ext2 ext3 ...:
        When --r is specified, this can be a regular expression matching filenames. (To match file extensions, append $, e.g., ext$)
    -:
        Indicates recursive search of the specified path list
    +:
        Indicates search of the specified path list without subdirectories
    search_path ...:
        Search path
    -o:
        Output to the output_path directory based on the current directory.
        When using /, output the processed source code files to the output_path directory.
        When using :, output the processed source code files to the output_path directory while preserving the original directory structure.
        The source code file naming rules are specified by the subsequent string. Available variables are {name} and {ext}, representing the filename and extension respectively.

## Notes on the + - Directory Pattern
When the pattern is -, subdirectories are expanded. Therefore, the search_path must not contain nested patterns, otherwise files will be counted repeatedly.
For example:

    C:\aaa\bbb C:\aaa\bbb\ccc

The first path already includes the second path, so the latter need not be specified to avoid duplication.

When the pattern is `+`, subfolders are not expanded. Therefore, the paths above will not be counted twice.


## 例子
1. Count the lines in cpp, c, and h files in the current directory:

        CodeLines cpp c h - .

2. Count the lines in cpp, c, and h files under C:\Project:

        CodeLines cpp c h - C:\Project
3. Count the lines in cpp, c, and h files under C:\Project, D:\Project, and E:\Project:

        CodeLines cpp c h - C:\Project D:\Project E:\Project

4. Use regular expressions to match source code files:

        CodeLines --r cpp$ main\.c .+\.h - C:\Project D:\Project

5. Output the processed code file to the current directory:

        CodeLines cpp hpp c h - C:\Project -o .
        or
        CodeLines cpp hpp c h - C:\Project -o ./
        or
        CodeLines cpp hpp c h - C:\Project -o ./{name}.{ext}
        rename
        CodeLines cpp hpp c h - C:\Project -o ./{name}_tmp.{ext}

6. Output the processed code files in the source code directory structure within the current directory:

        CodeLines cpp hpp c h - C:\Project -o .:
        or
        CodeLines cpp hpp c h - C:\Project -o .:{name}.{ext}
        rename
        CodeLines cpp hpp c h - C:\Project -o .:{name}_tmp.{ext}
