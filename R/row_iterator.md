# Iterate over a set of rows in rows R

```R
rows = function(x) lapply(seq_len(nrow(x)), function(i) lapply(x,function(c) c[i]))
```

I put together an improved version that is able to add an index column if desired:

```R
rows <- function(tab, idx=TRUE) {
  # Set idx to attach an index column
   if (is.na(tab) || is.null(tab) || nrow(tab) == 0) {
      return(list())
  }
  lapply(seq_len(nrow(tab)), 
         function(i) {
           row <- unclass(tab[i,,drop=F])
           if (idx) {
             row$idx <- i
           }
           row
         })
}
```