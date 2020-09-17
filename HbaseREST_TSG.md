# **Hbase REST Servers TSG**

## **Purpose of this TSG:**
This TSG will describe how HBase REST API works and show how to start HBase REST API and run simple commands.

## **How HBase REST Servers works on clusters**

The HBase REST Server exposes endpoints that provide CRUD (create, read, update, delete) operations for each HBase process, as well as tables, regions, and namespaces. For a given endpoint, the HTTP verb controls the type of operation (create, read, update, or delete).

Cloudera Resource: https://docs.cloudera.com/documentation/enterprise/5-9-x/topics/admin_hbase_rest_api.html

## **Getting Started**
By default HBase REST Servers are not started when cluster creation is finished.  To start them you will need to run a python script within SSH session on all workernodes.

Please run this command on all workernodes:

```sudo python /usr/lib/python2.7/dist-packages/hdinsight_hbrest/HbaseRestAgent.py```

You should see this when you run the python script.
```
sshuser@wn0-hbasec:~$ sudo python /usr/lib/python2.7/dist-packages/hdinsight_hbrest/HbaseRestAgent.py
2020-09-16 14:58:21,112 - HbaseRestAgent.py [120358] __main__ - INFO - startup_hbase_rest_server: Checking cluster manifest for hbase rest server config...
2020-09-16 14:58:21,113 - HbaseRestAgent.py [120358] __main__ - INFO - startup_hbase_rest_server: 'start_hbase_rest' is set to 1, restarting hbase rest server...
2020-09-16 14:58:21,113 - HbaseRestAgent.py [120358] __main__ - INFO - HbaseRestAgent: Starting rest server...
2020-09-16 14:58:21,113 - HbaseRestAgent.py [120358] __main__ - INFO - startup_hbase_rest_server: Reading the port number from hbase-site.xml, attempt 1...
2020-09-16 14:58:21,122 - HbaseRestAgent.py [120358] __main__ - INFO - startup_hbase_rest_server: Using port number '8090' from hbase-site.xml.
2020-09-16 14:58:21,122 - HbaseRestAgent.py [120358] __main__ - INFO - startup_hbase_rest_server: Restarting hbase rest server...
2020-09-16 14:58:21,175 - HbaseRestAgent.py [120358] __main__ - INFO - startup_hbase_rest_server: Successfully restarted hbase rest server.
```

After starting the servers you will be able to use HBase REST APIs


## **Examples:**
Using the public IPs:
```
export password='MyPassword'
```

```
sshuser@wn0-hbasec:~$ curl -u admin:$password -G https://hbasecssallee.azurehdinsight.net/hbaserest/namespaces

default
hbase
```
When running the above command this will hit the gateway first.  You can see this happening because we use the public endpoint which uses the FQDN.  Also, we use the /hbaserest/namespaces endpoint to communicate to the cluster.

**Remember that we do this from a workernode.  We can also perform the same in zookeepers.**

Using private IPs:

Here we use the private ip of the workernode.  This will not hit the gateway for HDInsight.
```
export password='MyPassword'
sshuser@wn0-hbase3:~$ curl -u admin:$password -G 10.0.0.30:8090/namespaces
default
hbase
```
Both scenerios are within the cluster.  To run this outside of the cluster first you will need to start the HBase REST Servers as above.  After the REST Servers are up you can run the same command as the public ip.  This works for both **HDI 4.0 and HDI 3.6**.

```
allee@MININT-TEF7OFT:~$ export password='MyPassword'

allee@MININT-TEF7OFT:~$ curl -u admin:$password -G https://hbasecssallee.azurehdinsight.net/hbaserest/namespaces
default
hbase
allee@MININT-TEF7OFT:~$ curl -u admin:$password -G https://hbase36cssallee.azurehdinsight.net/hbaserest/namespaces
default
hbase
```