SSH to EMR master node

`ssh -i ~/your-key-name.pem hadoop@ec2-000-000-000-000.compute-1.amazonaws.com`

`hive`

Run below command to create the external table:

`create external table lineitem_presto_vs_hive_test (
L_ORDERKEY INT,
L_PARTKEY INT,
L_SUPPKEY INT,
L_LINENUMBER INT,
L_QUANTITY INT,
L_EXTENDEDPRICE DOUBLE,
L_DISCOUNT DOUBLE,
L_TAX DOUBLE,
L_RETURNFLAG STRING,
L_LINESTATUS STRING,
L_SHIPDATE STRING,
L_COMMITDATE STRING,
L_RECEIPTDATE STRING,
L_SHIPINSTRUCT STRING,
L_SHIPMODE STRING, L_COMMENT STRING)
ROW FORMAT DELIMITED FIELDS TERMINATED BY '|'
LOCATION 's3://bigdatalabridge/emrdata/';`

Time taken: 10.547 seconds

Run below query:

  `Select l_shipdate, sum(l_discount) as total_discount from lineitem_presto_vs_hive_test group by l_shipdate;`

>1992-04-11	1041.5200000000004

>...

>...

>1998-10-21	437.75000000000085

>1998-11-18	145.89999999999984

Time taken: 186.084 seconds, Fetched: 2527 row(s)

Exit Hive by:

  `quit;`


Start Presto and execute the same query

  `presto-cli`

  `use hive.default;`

  `Select l_shipdate, sum(l_discount) as total_discount from lineitem_presto_vs_hive_test group by l_shipdate;`

Query 20180930_131803_00006_8ir9m, FINISHED, 2 nodes

Splits: 323 total, 323 done (100.00%)

1:57 [75M rows, 8.87GB] [642K rows/s, 77.8MB/s]

You can see the Presto dashboard at:

`http://ec2-000-000-000-000.compute-1.amazonaws.com:8889/`

![](https://laiase.com/png/Presto-dashboard.png)


Testing result, Presto is faster, difference may vary based on the node type and cluster size.
