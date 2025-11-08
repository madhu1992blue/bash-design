# Loop Arrays

Let's say have an array in bash:

```bash
#!/bin/bash
# Define an array
arr=("elem1" "elem 2" "elem3" "elem$4")
```

If we want to loop over each element of this array, our naive attempt might look like this:

```bash
#!/bin/bash
# Define an array
arr=("elem1" "elem 2" "elem3")
# Bad way to loop over array elements
for elem in ${arr[*]}; do
  echo "element: $elem"
done
```
The output that you see will be:

```text
element: elem1
element: elem
element: 2
element: elem3
```

So, another naive approach is to use `${arr[@]}` instead of `${arr[*]}`:

```bash
#!/bin/bash
# Define an array
arr=("elem1" "elem 2" "elem3")
# Yet another naive way to loop over array elements
for elem in ${arr[@]}; do
  echo "element: $elem"
done
```

The output that you see will be:

```text
element: elem1
element: elem
element: 2
element: elem3
```
Notice that the element with a space (`elem 2`) is split into two separate elements (`elem` and `2`).

While using `${arr[@]}` is generally preferred over `${arr[*]}` for looping over arrays, both approaches suffer from the same issue of word splitting on spaces.

The right way to loop over each element of an array in bash is to quote the expansion while using `${arr[@]}`:
```bash
#!/bin/bash
# Define an array
arr=("elem1" "elem 2" "elem3")
# Correct way to loop over array elements
for elem in "${arr[@]}"; do
  echo "element: $elem"
done
```

Dangerously, Now one might assume that quoting `${arr[*]}` would also work:
```bash
#!/bin/bash
# Define an array
arr=("elem1" "elem 2" "elem3")
# Incorrect way to loop over array elements. Un
for elem in "${arr[*]}"; do
  echo "element: $elem"
done
```
Nope, it does not work correctly and sufferes from the same issue of splitting on spaces.
