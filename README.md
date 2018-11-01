# bootcamp
desafio bootcamp
1) realice wget de los log en mi directorio tmp/   wget http://34.205.65.241/access.log
2) me conecte a la base de datos por medio de mysql -u bootcamp -h ip -p bootcamp
3) le proporsione permmisos la linea prrporcionada por el profesor export HADOOP_USER_NAME=hdfs
3) realice el la ingesta de datos a traves de sqoop import --connect jdbc:mysql://34.205.65.241:3306/ecommerce_cloudera --username bootcamp --password bootcamp --table product_transaction  --hive-import a una tabla en hive
4)cree un directorio en hdfs hdfs dfs -mkdir '/desafio'
5)movi el archivo access.log que estaba en mi tmp al hdfs con  hdfs dfs -put /tmp/access.log /desafio
6) cree la tabla a partir del archivo con:
CREATE TABLE access_log (
  ip STRING, 
  time_local STRING,
  method STRING,
  uri STRING,
  protocol STRING,
  status STRING,
  bytes_sent STRING,
  referer STRING, 
  useragent STRING
  )
ROW FORMAT SERDE 'org.apache.hadoop.hive.contrib.serde2.RegexSerDe'
WITH SERDEPROPERTIES  (
"input.regex" = '^(\\S+) \\S+ \\S+ \\[([^\\[]+)\\] "(\\w+) (\\S+) (\\S+)" (\\d+) (\\d+) "([^"]+)" "([^"]+)".*',
"output.format.string" = "%1$s %2$s %3$s %4$s %5$s %6$s %7$s %8$s %9$s"
)
STORED AS TEXTFILE
LOCATION '/desafio';
7) no se podian poner funsiones asi que la clase se cambio por org.apache.hadoop.hive.serde2.RegexSerDe
8) se crea tabla a partir de 
SELECT product_id, SUM(product_cantity) cantidad
  from product_transaction
  GROUP BY product_id;
 que es la cantidad de productos vendidos

9) se crea tabla a partir de 
create table Cantidad_Visual AS
select substr(uri,16,20) producto, count(uri)
from access_log
where uri like '%/item/id?skuID%'
group by uri;
que contiene la cantidad de visualizaciones por producto

10) creo una tabla que se llama conversion a partir de el resultado de la consulta
create table conversion_G AS
select product_id, cantidad/cantidad_visual.`_c1` as total from productos_vendidos inner join cantidad_visual where product_id = producto;
11) visualisacion de la ruta de la tabla con show create table conversion_g;

12) se exporta la tabla con sqoop con 
sqoop export --connect jdbc:mysql://34.205.65.241:3306/ecommerce_cloudera --driver com.mysql.jdbc.Driver --username bootcamp --password bootcamp --table conversion_10 --hcatalog-table conver

10) despues del export se consulta en la base de datos proporcionada por el profesor a mi me asignaron la numero 10;

Gracias.
