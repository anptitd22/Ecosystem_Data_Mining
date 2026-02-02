# Test Command Spark Hadoop 

- Step1: Kiểm tra spark
```bash
docker exec -it lakehouse-spark-master-1 /opt/spark/bin/spark-submit \
  --master spark://spark-master:7077 \
  --conf spark.eventLog.enabled=true \
  --conf spark.eventLog.dir=/opt/spark/spark-events \
  --class org.apache.spark.examples.SparkPi \
  /opt/spark/examples/jars/spark-examples_2.12-3.5.8.jar 10
```

- Step2: Kết nối vào pyspark
```bash
docker exec -it lakehouse-spark-master-1 /opt/spark/bin/pyspark --master spark://spark-master:7077
```

- Step3: Chạy test đọc ghi trên hadoop
```bash
# Tạo danh sách dữ liệu mẫu
data = [("Hadoop_Test", 100), ("Spark_Test", 200), ("Connection_Ok", 300)]
df = spark.createDataFrame(data, ["Name", "Value"])

# Ghi file vào thư mục /tmp trên HDFS để test
df.write.mode("overwrite").csv("hdfs://namenode:8020/tmp/test_connection")

# Đọc dữ liệu vừa ghi
check_df = spark.read.csv("hdfs://namenode:8020/tmp/test_connection")
check_df.show()
```