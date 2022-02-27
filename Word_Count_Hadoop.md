# Word Count Using Hadoop

# Installation

## 1. Copy Files to Namenode Filesystem

 `cd $HADOOP_HOME/bin`

### Create a directory in hadoop filesystem.

`hdfs dfs -mkdir -p /user/hadoop/input`

### Copy copy some text file to hadoop filesystem inside input directory.

`hdfs dfs -put <Your_Text_File> /user/hadoop/input/`

### Running Wordcount Command

```
cd $HADOOP_HOME
hadoop jar share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar wordcount input output
```

### Check the names of result file created under dfs@/user/hadoop/output filesystem using following command.

`hdfs dfs -ls /user/hadoop/output`

### Now show the content of result file where you will see the result of wordcount. You will see the count of each word.

`hdfs dfs -cat /user/hadoop/output/part-r-00000`



    
