# Lab 3-4 Spark

A través del nodo master y suando pyspark

```bash
$ pyspark
>>> files_rdd = sc.textFile("hdfs:///datasets/gutenberg-small/*.txt")
>>> files_rdd = sc.textFile("s3://emontoyadatasets/gutenberg-small/*.txt")
>>> wc_unsort = files_rdd.flatMap(lambda line: line.split()).map(lambda word: (word, 1)).reduceByKey(lambda a, b: a + b)
>>> wc = wc_unsort.sortBy(lambda a: -a[1])
>>> for tupla in wc.take(10):
>>>     print(tupla)
>>> wc.saveAsTextFile("hdfs:///tmp/wcout1")

$ pyspark
>>> ...
>>> ...
>>> wc.coalesce(1).saveAsTextFile("hdfs:///tmp/wcout2")
```

Por otro lado, podemos realizar esto mismo a través de ejecutar un archivo .py 

```bash
$ spark-submit --master yarn --deploy-mode cluster wc-pyspark.py
```

Además, lo podemos realizar a través de la aplicación Zeppelin. Allí deberemos crear un nuevo notebook.

```bash
%spark2.pyspark
# WORDCOUNT COMPACTO
#files_rdd = sc.textFile("s3://emontoyadatasets/gutenberg-small/*.txt")
files_rdd = sc.textFile("hdfs:///datasets/gutenberg-small/*.txt")
wc_unsort = files_rdd.flatMap(lambda line: line.split()).map(lambda word: (word, 1)).reduceByKey(lambda a, b: a + b)
wc = wc_unsort.sortBy(lambda a: -a[1])
for tupla in wc.take(10):
    print(tupla)
wc.coalesce(1).saveAsTextFile("hdfs:///tmp/wcout1")
```