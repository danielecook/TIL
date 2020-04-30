# Create boxed text using a bash alias

Add this to your `.bashrc`:

```
function box {
  size=${#1}
  printf '#'
  printf '=%.0s' $(seq 1 $(( ${size} + 6 )))
  printf '#\n'
  echo -n "#   "
  echo -n ${1}
  echo -n "   #"
  printf '\n'
  printf '#'
  printf '=%.0s' $(seq 1 $(( ${size} + 6 )))
  printf '#\n'
}
```

__Run:__

```
box this is cool
```

__Output:__

```
#==================#
#   this is cool   #
#==================#
```