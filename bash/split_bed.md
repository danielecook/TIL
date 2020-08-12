# Split a bed file by size and on chromosomes

It may be helpful to split a bed file into files by chromosome and by size. In other words, every split file should have only one chromosome and have a maximum number of lines.

The function below will do just that. Keep in mind, it is possible to have a split file with a single line using this approach, but every file will have only one chromosome.

```bash
function split_bed() {
# This function will split a bed file by chromosome and chunk_size=1000
# In other words, split files will only possess one chromosome max.
# File sizes may be variable.
awk -v chunk_size=1000 'NR == 1 { chrom=$1; iter=0; fname_iter=0; print chrom } 
    {
            if(chrom == $1 && iter <= chunk_size) {
                print > sprintf("x%04d_%s.segment.txt", fname_iter, $1);
                iter++; 
            } else {
                chrom=$1;
                iter=0;
                fname_iter++;
            }
    }' $1
}
split_bed capture_targets.bed
```