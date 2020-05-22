# Open dataset in google sheets

Use the following function to open up a dataset in google sheets

```R
gsheet <- function(df) {
   gs4_browse(googlesheets4::write_sheet(df))
}
```


