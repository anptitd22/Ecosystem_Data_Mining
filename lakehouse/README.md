# Test Command Spark Hadoop 

- Step1: Kiểm tra spark
```bash
docker exec -it lakehouse-spark-master-1 /opt/spark/bin/spark-submit \
  --master spark://spark-master:7077 \
  --conf spark.eventLog.enabled=true \
  --conf spark.eventLog.dir=/opt/spark/spark-events \
  --class org.apache.spark.examples.SparkPi \
  /opt/spark/examples/jars/spark-examples_2.12-3.5.3.jar 10
```

- Step2: Kết nối vào pyspark
```bash
docker exec -it lakehouse-spark-master-1 /opt/spark/bin/pyspark --master spark://spark-master:7077
```

- Step3: Chạy test đọc ghi trên hadoop
```python
data = [("Hadoop_Test", 100), ("Spark_Test", 200), ("Connection_Ok", 300)]
df = spark.createDataFrame(data, ["Name", "Value"])
df.write.mode("overwrite").csv("hdfs://namenode:8020/tmp/test_connection")
check_df = spark.read.csv("hdfs://namenode:8020/tmp/test_connection")
check_df.show()
```

- Step4: Chạy test iceberg
```python
spark.sql("CREATE DATABASE IF NOT EXISTS hadoop_prod.db_test")
spark.sql("CREATE TABLE hadoop_prod.db_test.table_iceberg (id bigint, data string) USING iceberg")
spark.sql("INSERT INTO hadoop_prod.db_test.table_iceberg VALUES (1, 'Iceberg_Test_Ok')")
spark.table("hadoop_prod.db_test.table_iceberg").show()
```

- Step5: Cài dbeaver, kết nối và chạy test tạo bảng
```sql
create schema gold
create table iceberg.gold.test (testcol int)
insert into iceberg.gold.test (1)
```