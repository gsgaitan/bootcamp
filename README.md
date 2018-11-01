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
7)
