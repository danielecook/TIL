# Use the `groupKey` operator to prevent blocks and improve caching

If you use the [`groupTuple` operator](https://www.nextflow.io/docs/latest/operator.html?highlight=groupkey#grouptuple) in Nextflow, you should know about the `groupKey` function (see the "tip" section in the documentaiton under `groupTuple`).

A `groupKey` can specify the number of entries that are part of a `groupTuple`. This has two major benefits:

1. It prevents blocking. Nextflow "flows" data through channels. Any grouping operation will necesarily have to observe the entire "flow", or all data before being able to accurately group, unless you can tell Nextflow how many elements are part of a group.
2. In my experience, it prevents caching issues. I am not sure why.

Consider the following example. Each row corresponds to data flowing through Nextflows channels.

| letter  |   var_x |   n_obs    |
|:--------|--------:|-----------:|
| A       |       1 |          1 |
| B       |       3 |          2 |
| B       |       5 |          2 |
| C       |       2 |          3 |
| C       |       1 |          3 |
| C       |       2 |          3 |

```groovy
my_data = Channel.fromPath("input.tsv", checkIfExists: true)
 	             .splitCsv(header:true, sep: "\t")

grouped_data = my_data.map { row -> [groupKey(row.letter, row.n_obs), var_x] }
		  			  .groupTuple(by:0)

```

In order for Nextflow to aggregate by the "letter" column, it needs to know how many elements of `A`, `B`, `C`, etc. there are in the dataset. The groupKey can be used to make Nextflow aware that for `A` there is only 1 element - so once it has seen a single instance of A, it can group and continue. For B, it must see 2 rows with the letter B. And so on and so forth.


