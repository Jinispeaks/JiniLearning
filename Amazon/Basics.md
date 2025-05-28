### Amazon
1. Glue (etl)
2. Redshift(datawarehouse)
3. Athena (SERVERLESS)
4. S3
5. EMR
6.Lambda


## Athena 
serverless service.
It allows to read from data stored in S3.
it charges based on the data scanned. 

how to improve performance to reduce scan while reading from s3.
i) use compression techniques like snappy, gzip.
ii) use column based storage like Parqut, ORC
