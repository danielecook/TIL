# Output multiple commands as a single stream

Previously when I needed to 'build up' a file from multiple commands I would start a new file with `>` and use `>>` to append to that file with successive commands. For example:

```bash
touch chrom_names.txt && truncate chrom_names.txt
for i in `seq 1 22`; 
        do echo ${i} >> chrom_names.txt
done;
echo "X" >> chrom_names.txt
echo "Y" >> chrom_names.txt
echo "MT" >> chrom_names.txt
```

However, you can easily combine commands within brackets and send the output of multiple commands to the file:

```bash
{
    for i in `seq 1 22`; 
        do echo ${i};
    done;
    echo "X";
    echo "Y";
    echo "MT";
} > chrom_names.txt
```