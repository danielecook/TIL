# Download file if it is not present

In bash you can use an OR operator (`||`) to quickly test whether a file exists and download. The command below will be skipped if the file `populations.tsv` is present and has greater than 0 bytes.

```bash
[ -s populations.tsv ] || curl ftp://ftp.1000genomes.ebi.ac.uk/vol1/ftp/release/20110521/phase1_integrated_calls.20101123.ALL.panel > populations.tsv
```

To ensure this works you might want to ensure the download completes successfully by downloading the file and moving it into a location to be checked, while using the `-e` flag to stop your script if errors occur.