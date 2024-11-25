# Lab 3-3 HIVE y SparkSQL, gestión de datos vía SQL

Inicialmente debemos de establecer la conexión por medio de ssh. 

Una vez relizado esto, debemos de ingresar con nuestro username y contraseña en HUE. 

```bash
User: hadoop
Contraseña: ******
```

Debido a que vamos a trabajar con HDFS, debemos de copiar los datos en el cluster debido a que este es temporal.

### Gestión y consultas

Creamos la tabla HDI en HDFS/beeline. 

Esta tabla tendrá las siguientes columnas:
- id 
- country
- hdi
- lifeex
- mysch
- eysch
- gni

```bash
$ beeline 
use usernamedb;
CREATE TABLE HDI (id INT, country STRING, hdi FLOAT, lifeex INT, mysch INT, eysch INT, gni INT) 
ROW FORMAT DELIMITED FIELDS TERMINATED BY ','
STORED AS TEXTFILE
```

Una vez creada la tabla, debemos de cargar los datos, esto lo podemos hacer de diferentes maneras:

- Copiando los datos directamente

```bash
hdfs dfs -put hdfs:///user/hadoop/datasets/onu/hdi-data.csv hdfs:///user/hive/warehouse/usernamedb.db/hdi
```

- Cargando los archivos desde hive:

```bash
$ hdfs dfs -chmod -R 777 /user/hadoop/datasets/onu/
$ beeline
0: jdbc:hive2://sandbox-hdp.hortonworks.com:2> LOAD DATA INPATH '/user/hadoop/datasets/onu/hdi-data.csv' INTO TABLE HDI
```

- Usando una tabla externa en hdfs:

```bash
use usernamedb;
CREATE EXTERNAL TABLE HDI (id INT, country STRING, hdi FLOAT, lifeex INT, mysch INT, eysch INT, gni INT) 
ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
STORED AS TEXTFILE 
LOCATION '/user/hadoop/datasets/onu/hdi/'
```

Además de esto, podmeos crear la tabla HDI en EMR/S3/HUE/Hive:

```bash
use usernamedb;
CREATE EXTERNAL TABLE HDI (id INT, country STRING, hdi FLOAT, lifeex INT, mysch INT, eysch INT, gni INT) 
ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
STORED AS TEXTFILE 
LOCATION 's3://emontoyadatasets/onu/hdi/'
```

También podemos realizar consultas y cálculos de la siguiente manera:

```bash
use usernamedb;
show tables;
describe hdi;

select * from hdi;

select country, gni from hdi where gni > 2000; 
```

También podemos ejecutar un JOIN con Hive de la siguiente manera:

```bash
CREATE EXTERNAL TABLE EXPO (country STRING, expct FLOAT) 
ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' 
STORED AS TEXTFILE 
LOCATION 's3://datasetsb/datasets/onu/export'
```
Además podemos realizar el JOIN de dos tablas:

```bash
SELECT h.country, gni, expct FROM HDI h JOIN EXPO e ON (h.country = e.country) WHERE gni > 2000;
```

### Wordcount en Hive

Debemos tener en cuenta que debemos de copiar el contenido de la carpeta local a la carpeta en hadoop.

```bash
hdfs dfs -put st0263-242/bigdata/datasets/gutenberg-small/* hdfs:///user/hadoop/datasets/gutenberg-small/
```

Ahora creamos la tabla.

A través de hdfs.

```bash
CREATE EXTERNAL TABLE docs (line STRING) 
STORED AS TEXTFILE 
LOCATION 'hdfs:///user/hadoop/datasets/gutenberg-small/';
```

A través de s3.

```bash
CREATE EXTERNAL TABLE docs (line STRING) 
STORED AS TEXTFILE 
LOCATION 's3://emontoyadatasets/gutenberg-small/';
```

Ahora podemos realizar consultas como:

- Ordenar por palabra

```bash
SELECT word, count(1) AS count FROM (SELECT explode(split(line,' ')) AS word FROM docs) w 
GROUP BY word 
ORDER BY word DESC LIMIT 10;
```
- Ordenado por frecuencia de mayor a menor

SELECT word, count(1) AS count FROM (SELECT explode(split(line,' ')) AS word FROM docs) w 
GROUP BY word 
ORDER BY count DESC LIMIT 10;

### Reto

Esto se puede realizar fácilmente si creamos directamente la tabla con el resultado arrojado por el query.

```bash
CREATE TABLE word_frequency_ctas AS
    SELECT word, count(1) AS count 
    FROM (
        SELECT explode(split(line,' ')) AS word 
        FROM docss3
    ) w 
    GROUP BY word 
    ORDER BY count DESC;
```