# Laboratorio 3-2 mapreduce

En este laboratorio realizaremos un demo de MapReduce en Python a través de MRJOB. 

# Creación de un entorno virtual

A la hora de realizar el laboratiorio, se recomienda crear un entorno virtual con el fin de tener una buena ejecución y desarrollo del laboratorio. 

Instalamos `virtualenv`

```bash
sudo pip install virtualenv
```

Creamos un nuevo entorno virtual y lo ejecutamos. 

```bash
sudo virtualenv MapReduce_env

source MapReduce_env/bin/activate
```

# WordCount en Python

Ahora podemos proceder a correr la versión serial con todos los datos locales.

```bash
$ cd 02-mapreduce
$ python wordcount-local.py ../datasets/gutenberg-small/*.txt > salida-serial.txt
$ more salida-serial.txt
```

A pesar de que se cuenta con variedad de librerías en Python para acceder a servicios de MapReduce en Hadoop, utilizaremos MRJOB. 

Instalación MRJOB.

```bash
user@master$ sudo yum install python3-pip
user@master$ sudo pip3 install mrjob
```

Ahora podemos probar MRJOB en Python de manera local. 

```bash
user@master$ cd 02-mapreduce
user@master$ python wordcount-mr.py ../datasets/gutenberg-small/*.txt
```