# R operators (`||` vs `|`, `&&` vs `&`)

Double operators (`||` and `&&`) "short-circuit" as illustrated below:

```R
# A double bar || OR in R "short circuits"
# The || operator will skip evaluating the second argument if the first argument is TRUE.
{ print("left"); TRUE } || { print("right"); TRUE } # Prints left
{ print("left"); FALSE } || { print("right"); TRUE } # Prints left, right

# The && operator will skip evaluating the second argument if the first argument is TRUE.
{ print("left"); TRUE } && { print("right"); TRUE } # Prints left, right
{ print("left"); FALSE } && { print("right"); TRUE } # Prints right

# Both arguments will be evaluated, even though the first one is TRUE
{ print("left"); TRUE } | { print("right"); FALSE }

# Both arguments will be evaluated, even though the first one is FALSE:
{ print("left"); FALSE } & { print("right"); FALSE }
```


