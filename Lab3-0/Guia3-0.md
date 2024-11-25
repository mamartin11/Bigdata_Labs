# Guía de creación Cluster EMR en Amazon AWS

Debemos de ingresar a Amazon AWS, buscar el servicio EMR y allí haremos clic en el botón 'Crear Cluster'.

![Crear cluster](Fotos3-0/creacion_cluster.png)

Para estos laboratorios, estaremos utilizando diversas aplicaciones, es por esto que debemos de seleccionar los paquetes adecuados para que el cluster funcione de manera correcta.

- HCatalog 3.1.3
- Hue 4.11.0
- Livy 0.8.0
- Sqoop 1.4.7
- Hadoop 3.3.6
- JupyterEnterpriseGateway 2.6.0
- Zeppelin 0.11.1
- Hive 3.1.3
- JupyterHub 1.5.0
- Spark 3.5.1
- ZooKeeper 3.9.1

Además de esto debemos las siguientes configuraciones del AWS Glue Data Catalog settings.

- Use for Hive table metadata
- Use for Spark table metadata


Ahora debemos de elegir las máquinas EC2 del cluster. Es recomendable cambiar la versión que viene por defecto (m5.xlarge) por una menor (m4.xlarge)

![Máquinas EC2](Fotos3-0/soft_config.png)

Dejar los siguientes ajustes por defecto hasta el apartado de Software settings.

Antes de configurar esto, debemos de ir al servicio S3 y crear nuestro propio bucket.

![Bucket](Fotos3-0/s3.png)


Ahora podemos realizar la siguiente configuración. 

```bash
[
  {
    "Classification": "jupyter-s3-conf",
    "Properties": {
      "s3.persistence.bucket": "mmartineznotebooks",
      "s3.persistence.enabled": "true"
    }
  }
]
```

Antes de crear el cluster, recuerde descargar el archivo .pem del par de claves. Esto será indispensable para conectarnos al nodo master de la instancia EC2 del cluster.

Por otro lado, el los grupos de seguridad del nodo master, debemos de habilitar las reglas de entrada para los siguientes puertos:

- 22
- 8888
- 8890
- 9443
- 9870
- 14000

Una vez creado el cluster, podemos acceder a la aplicación de HUE, en donde crearemos nuestro usuario (debe ser hadoop) y nuestra contraseña. 

![HUE](Fotos3-0/Hue.png)

Podemos realizar lo mismo con JupyterHub. en este caso nuestro usuario será jovyan y nuestra contraseña jupyter.

![Jupyter](Fotos3-0/pyspark.png)