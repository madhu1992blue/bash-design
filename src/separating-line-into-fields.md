# Separate Each Line Into Fields

Intuition says to use `cut` in a loop but this is quite inefficient:
```bash
#!/bin/bash
# Naive way to separate line into fields using cut in a loop
while read line do
  field1=$(echo "$line" | cut -d',' -f1)
  echo "field1: $field1"
  field2=$(echo "$line" | cut -d',' -f2)
  echo "field2: $field2"
  field3=$(echo "$line" | cut -d',' -f3)
  echo "field3: $field3"
done < file.txt
```
This approach launches a separate `cut` process for each field of every line, which is inefficient for large files.

## Good: "read" when first few fields are needed

A better way is to use `IFS` (Internal Field Separator) to split the line into fields:
```bash
#!/bin/bash
# Better way to separate line into fields using IFS
while IFS=',' read -r field1 field2 field3; do
    echo "field1: $field1"
    echo "field2: $field2"
    echo "field3: $field3"
done < file.txt
```

This method reads each line and splits it into fields based on the specified delimiter (`,` in this case). It is more efficient than using `cut` in a loop.

### Caveat: Avoid "read" when you need fields of higher numbers

1. The last field will contain all remaining data if there are more fields than variables.
2. If there are large number of fields, you need to define enough variables to hold them. Assume you have 100 fields, and need the 98th field, you need to define 98 variables which is not practical.

## Good: 'cut' when higher number field is needed

If you have a large number of fields and only need specific ones, using `cut` once per line is more efficient:
```bash
#!/bin/bash
# A better way(but still naive way) to get specific fields when number of fields is large and you need a field of higher number.
while read -r line; do
    field98=$(echo "$line" | cut -d',' -f98)
    echo "field98: $field98"
done < file.txt
```
However, if the number of lines are large, and the lines are in a file, this approach is still forking `cut` for each line.

When number of lines is large, combining `cut` with input redirection is even more efficient:
```bash
#!/bin/bash
# More efficient way to get specific fields using cut with input redirection
while IFS= read -r field98; do # We make this robust by using IFS and using `read` with `-r`
    echo "field98: $field98"
done < <(cut -d',' -f98 file.txt)
```
This approach leverages `cut` to process the entire file at once, and then send the output to the `while` loop for further processing. This minimizes the number of times `cut` is invoked, making it more efficient for large files.
