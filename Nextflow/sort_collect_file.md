# Sort lines in a file assembled using `collectFile` with a closure

Nextflow has a great operator called [`collectFile`](https://www.nextflow.io/docs/latest/operator.html?highlight=collectfile#collectfile) that makes it easy to combine the output files from a process run across a group of samples. There are a number of options for sorting, but for custom sorts you have to implement a closure or comparator. Implementing a custom sort requires using a closure or comparator. 

This illustrates how you can use [Groovy Grapes](https://docs.groovy-lang.org/latest/html/documentation/grape.html) to import external libraries.

### Sorting by content with `collectFile`

The `collectFile` operator is able to take as input a list of files and combine them into a single file. It offers a `sort` option. However, for custom sorts, when supplying a closure, it will iterate over the full file paths instead of their content. In order to sort by the content within files you will need to read the files.

### The Scenerio

This is a slightly complicated scenerio, but it illustrates a number of useful tricks and ideas.

I have four files output from an analysis script. Each one is two lines: a header, and results. I want to combine by header, and sort first on the `sample` column, and second on `sample_type`. Importantly, I want the `original` sample to appear first in the list.

| sample   |   score_a | sample_type   |
|:---------|----------:|:--------------|
| xr1      |        42 | modified      |

| sample   |   score_a | sample_type   |
|:---------|----------:|:--------------|
| xr1      |        20 | original      |

| sample   |   score_a | sample_type   |
|:---------|----------:|:--------------|
| xr1      |        30 | modified      |

| sample   |   score_a | sample_type   |
|:---------|----------:|:--------------|
| xr2      |        37 | original      |

The basic idea is to output a string which will be sorted alphabetically, but which corresponds to the custom sort we desire.

```groovy
// Put this at the top of the script. It will automatically download
// groovycsv, a library we can use to parse the output.
@Grab('com.xlson.groovycsv:groovycsv:1.1')
// import the parseCsv function.
import static com.xlson.groovycsv.CsvParser.parseCsv

// This uses the DSL 2 syntax, but will work with the original syntax as well.
my_analysis.out.result
			   .collectFile(name: "aggregate_results.tsv",
                          	keepHeader: true,
                          	skip: 1,
                          	sort: { // it is default argument supplied
                          		    // first, parse the line using parseCsv
                                    line = parseCsv(new FileReader(it.toString()), separator: "\t")[0];
                                    orig_sort = (line.sample_type == "original") ?  0 : 1; 
                                    "${line.sample}+${orig_sort}" } ) 
```

A few notes on the above implementation.

1. `it` acts as the default parameter.
2. `it.toString()` is required because the closure is iterating over file paths which are of class `sun.nio.fs.UnixPath`.
3. `parseCsv` returns an iterator, but because our result files are only a single line we can simply take the first element `[0]` which is a hashmap of the headers as keys and the result row as values.
4. `orig_sort` is used to redefine the `sample_type` column as `0=original` and `1=modified`.
5. At the end, I simply output a string with the sample name, and a 0 or 1 corresponding to `original` or `modified`. This string will be sorted, ascending, so sample name is sorted first, followed by sample_type with `original` values coming first as `0`.

### Resulting table

Below is the end result with an added column illustrating the values files were sorted on.

| sample   |   score_a | sample_type   | (sort value)   |
|:---------|----------:|:--------------|:---------------|
| xr1      |        20 | original      | xr1 0          |
| xr1      |        30 | modified      | xr1 1          |
| xr1      |        42 | modified      | xr1 1          |
| xr2      |        37 | original      | xr2 1          |

### Conclusion

This is a lot of work just to sort the output. But for my purposes the output file will be reviewed extensively and the sorting there has an important purpose.
