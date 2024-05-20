-- Instrucciones guiadas del proyecto individual --


---- Para instalar las herramientas necesarias en el entorno de ejecición ----
1. Se clona el repositorio dentro del servidor 
Código -----------------git clone https://github.com/soyHenry/DS-M4-Herramientas_Big_Data

2. Se utiliza el comando para ejecutar el container, estando en la carpeta que lo contiene
Código -----------------sudo docker-compose -f docker-compose.yml up -d


![SS01](/screenShots/ss01.png)




				1.---HDFS----
a. Se utiliza el entorno de docker compose v1
Código -----------------sudo docker-compose -f docker-compose-v1.yml up -d


b. El codigo siguiente ingresa al contenedor namenodo de HDFS y ahí se crean directorios y copian archivos
sudo socker exec -it namenode bash
cd home 
mkdir Datasets
cd Datasets
mkdir canaldeventa
mkdir calendario
mkdir cliente
mkdir compra
mkdir empleado
mkdir gasto
mkdir producto
mkdir proveedor
mkdir sucursal
mkdir tipodegasto
mkdir venta
mkdir data_nvo
exit
![SS02](/screenShots/ss02.png)

Dentro de la carpeta que contiene los Datasets, copiaremos esos archivos a la direccionq ue tiene el contenedor namemode con el diguiente código, que se encuentra en un archivo shell sh:

	sudo docker cp Datasets/canaldeventa/CanalDeVenta.csv namenode:/home/Datasets/canaldeventa/CanalDeVenta.csv
	sudo docker cp Datasets/calendario/Calendario.csv namenode:/home/Datasets/calendario/Calendario.csv
	sudo docker cp Datasets/cliente/Cliente.csv namenode:/home/Datasets/cliente/Cliente.csv
	sudo docker cp Datasets/compra/Compra.csv namenode:/home/Datasets/compra/Compra.csv
	sudo docker cp Datasets/empleado/Empleado.csv namenode:/home/Datasets/empleado/Empleado.csv
	sudo docker cp Datasets/gasto/Gasto.csv namenode:/home/Datasets/gasto/Gasto.csv
	sudo docker cp Datasets/producto/Producto.csv namenode:/home/Datasets/producto/Producto.csv
	sudo docker cp Datasets/proveedor/Proveedor.csv namenode:/home/Datasets/proveedor/Proveedor.csv
	sudo docker cp Datasets/sucursal/Sucursal.csv namenode:/home/Datasets/sucursal/Sucursal.csv
	sudo docker cp Datasets/tipodegasto/TiposDeGasto.csv namenode:/home/Datasets/tipodegasto/TiposDeGasto.csv
	sudo docker cp Datasets/venta/Venta.csv namenode:/home/Datasets/venta/Venta.csv
	sudo docker cp Datasets/data_nvo/Cliente.csv namenode:/home/Datasets/data_nvo/Cliente.csv
	sudo docker cp Datasets/data_nvo/Empleado.csv namenode:/home/Datasets/data_nvo/Empleado.csv
	sudo docker cp Datasets/data_nvo/Producto.csv namenode:/home/Datasets/data_nvo/Producto.csv

![SS02](/screenShots/ss02.png)


c. Ubicarse en el contenedor "namenode"
Código -----------------sudo docker exec -it namenode bash

Crear un directorio en HDFS llamado "/data".
Código -----------------hdfs dfs -mkdir -p /data

Copiar los archivos csv provistos a HDFS:
Código -----------------hdfs dfs -put /home/Datasets/* /data


Este proceso de creación de la carpeta data y copiado de los arhivos, debe poder ejecutarse desde un shell script.

Nota: Busque dfs.blocksize y dfs.replication en http://<IP_Anfitrion>:9870/conf para encontrar los valores de tamaño de bloque y factor de réplica respectivamente entre otras configuraciones del sistema Hadoop.

![SS03](/screenShots/ss03.png)
![SS04](/screenShots/ss04.png)





				2.---HIVE----

Dentro de Hive se ejecutan varios archivos hql para que se craguen bases de datos. 
Para ello es necesario copiarlos al contenedor de Hive y posterioemente ejecutarlos con el comando:
codigo -----------------sudo docker-compose -f docker-compose-v2.yml up -d
codigo -----------------sudo docker exec -it hive-server bash
codigo -----------------mkdir Files_hql
codigo -----------------sudo docker cp Paso02_ConConsultas.hql hive 							server:/opt/hive/Files_hql/Paso02_ConConsultas.hql

![SS05](/screenShots/ss05.png)

Después nos posicionamos dentro de la carpeta de hive (sin ejecutralo) y corremos:
código -----------------hive -f Paso02_ConConsultas.hql


![SS06](/screenShots/ss06.png)
![SS07](/screenShots/ss07.png)




			3.---Formatos de Almacenamiento----

Hacemos el mismo procedimiento para los formatos de almacenamiento pero en este caso se guardan los datos en otro tipo de formatos. Dentro de Hive se ejecutan varios archivos hql para que se craguen bases de datos. 
Para ello es necesario copiarlos al contenedor de Hive y posterioemente ejecutarlos con el 
comando:
codigo -----------------sudo docker-compose -f docker-compose-v2.yml up -d
codigo -----------------suo docker exec -it hive-server bash
codigo -----------------sudo docker cp Paso03.hql hive server:/opt/hive/Files_hql/Paso03.hql

Después nos posicionamos dentro de la carpeta de hive (sin ejecutralo) y corremos:
código -----------------hive -f Paso03.hql

![SS08](/screenShots/ss08.png)





				4.---SQL----
Hacemos el mismo procedimiento para SQL, sin embargo en este caso se crea un index. Dentro de Hive se ejecutan varios archivos hql para que se craguen bases de datos. 
Para ello es necesario copiarlos al contenedor de Hive y posterioemente ejecutarlos con el 
comando:
codigo -----------------sudo docker-compose -f docker-compose-v2.yml up -d
codigo -----------------sudo docker exec -it hive-server bash
codigo -----------------sudo docker cp Paso04_ConConsulta.hql hive-server:/opt/hive/Files_hql/Paso04_ConConsulta.hql

Después nos posicionamos dentro de la carpeta de hive (sin ejecutralo) y corremos:
código -----------------hive -f Paso04_ConConsulta.hql

![SS09](/screenShots/ss09.png)




				5.---NoSQL----
Para ejecutar NoSQL dentro de la consola, se ejecuta el 
código -----------------sudo docker-compose -f docker-compose-v3.yml up -d 


1- sudo docker exec -it hbase-master hbase shell
código -----------------create 'personal','personal_data'
código -----------------list 'personal'
código -----------------put 'personal',1,'personal_data:name','Juan'
código -----------------put 'personal',1,'personal_data:city','Córdoba'
código -----------------put 'personal',1,'personal_data:age','25'
código -----------------put 'personal',2,'personal_data:name','Franco'
código -----------------put 'personal',2,'personal_data:city','Lima'
código -----------------put 'personal',2,'personal_data:age','32'
código -----------------put 'personal',3,'personal_data:name','Ivan'
código -----------------put 'personal',3,'personal_data:age','34'
código -----------------put 'personal',4,'personal_data:name','Eliecer'
código -----------------put 'personal',4,'personal_data:city','Caracas'
código -----------------get 'personal','4'

![SS10](/screenShots/ss10.png)


2-En el namenode del cluster: (para colocar el archivo en el hdfs)
código -----------------sudo docker exec -it namenode bash
código -----------------cd home
código -----------------cd Datasets
código -----------------hdfs dfs -put personal.csv /hbase/data/personal.csv


3- Entramos nuevamente al habse-master container
código -----------------sudo docker exec -it hbase-master bash
		
Cargamos los datos del csv a la tabla hbase
código -----------------hbase org.apache.hadoop.hbase.mapreduce.ImportTsv -Dimporttsv.separator=',' -Dimporttsv.columns=HBASE_ROW_KEY,personal_data:name,personal_data:city,personal_data:age personal hdfs://namenode:9000/hbase/data/personal.csv

![SS11](/screenShots/ss11.png)


código -----------------hbase shell
código -----------------scan 'personal'
código -----------------create 'album','label','image'
código -----------------put 'album','label1','label:size','10'
código -----------------put 'album','label1','label:color','255:255:255'
código -----------------put 'album','label1','label:text','Family album'
código -----------------put 'album','label1','image:name','holiday'
código -----------------put 'album','label1','image:source','/tmp/pic1.jpg'
código -----------------get 'album','label1




a) HBase
Después corremos el comando para entrar en el contenedor 
código -----------------sudo docker exec -it hbase-master hbase shell



Continuará ...