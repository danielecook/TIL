# Set vectors from plain text lists

Often in R it is convenient to define a list of items as a constant. For example, you might want to test that a column only contains items belonging to a set.

One way to do this is to define the items using a vector notation, but this can be annoying to edit if the list is very long. For example:

```R
ITEMS <- c("A", "B", "C")
```

Alternatively, you can define a utility function to split a list to define a set.

```R
vectorize <- function(x) {
    x <- strsplit(x, "\n")[[1]]
    x[!is.na(x) & x != ""]
}

LANDSCAPE <- vectorize("
Agricultural_land
Forest
Rural_garden
Urban_garden
Botanical_garden_or_zoo
Beach
Dry_shrubland
Wet_shrubland
Riverside
Grassland
")

```

Now you do not need to worry about quotes or commas and can copy/paste items into a list.
