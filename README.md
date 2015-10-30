# drill-fuzzy-search

drill-fuzzy-search is a plugin for [Apache Drill] that supports simple similarity and distance search.

What is supported:
  - [Levenshtein]
  - [Jaro]
  - [Cosine similarity]

It's based on SimMetrics library which you may refer to for more informations: [SimMetrics]

## Installation
Clone/download the sourcecode and run:
```sh
$ mvn clean package
```
then copy target/drill-fuzzy-search* jars to drill's jars/3rdparty directory. Also ensure that to copy simmetrics-core-3.2.3.jar in the same place.

After building the code you can start Drill with:
```sh
$ bin/drill-embedded
```
You can also use [my fork of Apache Drill with drill-gis] - drill-fuzzysearch branch.
## Usage

### Sample dataset

There is a test CSV dataset included. You can copy it to Drill's sample-data directory.

The structure of the CSV is as follows:
```
text A, text B, similarity level
```

Following examples are based on queries to sample file which is embedded in drill-fuzzy-search jar file (classpath) i.e.:
```
select * from cp.`sample-data/similarities.csv`;
```

but you can also query dataset from filesystem:
```
select * from dfs.deault.`/home/k255/drill/sample-data/similarities.csv`;
```

### Fuzzy queries

Comparison of texts with different metrics:
```
select columns[0] as textA, columns[1] as textB, 
    levenshtein(columns[0], columns[1]) as distance
    from cp.`sample-data/similarities.csv`;
```

```
select columns[0] as textA, columns[1] as textB,
    levenshtein(columns[0], columns[1]) as distance
    from cp.`sample-data/similarities.csv`
    where levenshtein(columns[0], columns[1]) > 0.7;
```

```
select columns[0] as textA, columns[1] as textB, 
    jaro(columns[0], columns[1]) as distance
    from cp.`sample-data/similarities.csv`;
```

```
select columns[0] as textA, columns[1] as textB, 
    cosine_similarity(columns[0], columns[1]) as distance
    from cp.`sample-data/similarities.csv`;
```

## Author

Karol Potocki

## License
----

Apache 2.0 License


   [Apache Drill]: <https://drill.apache.org>
   [Apache Big Data]: <http://events.linuxfoundation.org/events/apache-big-data-europe>
   [my fork of Apache Drill with drill-gis]: <https://github.com/k255/drill.git>
   [SimMetrics]: <https://github.com/Simmetrics/simmetrics>
   [Levenshtein]: <https://en.wikipedia.org/wiki/Levenshtein_distance>
   [Jaro]: <https://en.wikipedia.org/wiki/Jaro%E2%80%93Winkler_distance>
   [Cosine similarity]: <https://en.wikipedia.org/wiki/Cosine_similarity>
