---
title: Apache Livy
tags:
  - Spark
categories:
  - Develop
date: 2025-04-21
draft: true
---

gcs 上傳檔案至 VM
```
gsutil cp 'gs://pchome-hadoopincloud-codes-dev/tmp/mia/make_hotsearch.sh' '/home/webuser/htdocs/projects/ec_token/'
```

gcs 傳輸檔案至地端 hadoop
1.  copy form gcs to local or server
2. 
```
# BOTO Config
PYTHONPATH=/usr/lib/python3.6/site-packages BOTO_CONFIG=/home/webuser/.boto-pchome-hadoopincloud-with-proxy gsutil ls gs://pchome-hadoopincloud-codes-dev
# 信義機器
download from gcs to my computer -> upload file from my computer to 信義機器（10.160.2.158 /home/webuser/mia_test/....）
```
3. server to hadoop filesystem
```
hadoop fs -put local_path /projects/ec_token/.....
```


cloud shell ssh VM

```
# 一般情況下
gcloud compute ssh --zone "asia-east1-b" "eccronj-dev"  --project "bigdata-hadoopincloud"

# 當機器有設定外網
gcloud compute ssh --zone "asia-east1-b" "eccronj-dev"  --project "bigdata-hadoopincloud" --tunnel-through-iap
```
