# Loop Lines

Let's say you have a file called lines:
```text
l1
l 2
\h
```

If we want to loop over each line of this file, our naive attempt might look like this:
```bash
#!/bin/bash
# Bad way to loop over lines in a file
for line in $(cat "lines"); do
  echo "line: $line"
done
```

The output that you see will be:
```text
line: l1
line: l
line: 2
line: \h
```

Notice that the line with a space (`l 2`) is split into two separate lines (`l` and `2`), This happens because the `for` loop splits the output of `cat` on whitespace instead of new lines as we intended.
The issues with above approach are:
1. Lines with spaces are split into multiple words.
2. Large file contents can lead to command line length limits being exceeded.

A better way to loop over each line of a file is to use a `while` loop with `read`:
```bash
#!/bin/bash
# Better way to loop over lines in a file
# This approach handles lines with spaces and special characters correctly.

cat lines | while IFS= read -r line; do
  echo "line: $line"
done
```
This method reads the file line by line, preserving spaces and special characters.
- IFS is set to an empty value to prevent leading/trailing whitespace from being trimmed.
- `-r` prevents backslashes from being interpreted as escape characters.

Alternatively, you can use input redirection to avoid using `cat`:
```bash
#!/bin/bash
# Good way to loop over lines in a file using input redirection
while IFS= read -r line; do
  echo "line: $line"
done < lines
```
This approach is more efficient as it avoids subshell created by the pipe.
