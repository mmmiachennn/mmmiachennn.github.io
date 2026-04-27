---
title: Apache Livy
tags:
  - Spark
categories:
  - Develop
date: 2025-04-21
draft: true
---

# Dataproc

/opt/conda/default/bin/spark-submit --master yarn --deploy-mode cluster --py-files /tmp/00356df89ee3410d9003e96ae9e2b443/sst_mylib-0.0.1-py3.8.egg /tmp/00356df89ee3410d9003e96ae9e2b443/user_identification_v2.py --action_date 20230712


yarn application --list




# secondary worker
在 Google Cloud Dataproc 中，您可以使用三種類型的次級工作者（secondary workers）：標準的可搶占型（standard preemptible）、競價型（spot）和不可搶占型（non-preemptible）。這三種類型的機器有不同的特性和用途。以下是它們的主要區別：

1. 標準可搶占型 (Standard Preemptible)
價格：比標準不可搶占型便宜得多，通常能節省 70% 到 90% 的成本。
搶占：這些 VM 可能在任意時間被 Google Cloud 搶占（即關閉），不提供任何提前通知。
使用期限：最多只能運行 24 小時。
適用場景：適合短期和容錯的批處理任務，如數據分析、數據處理和機器學習工作負載。
2. 競價型 (Spot)
價格：價格比標準不可搶占型和標準可搶占型更低，因為它基於市場供需波動，價格是動態的。
搶占：這些 VM 可能在任意時間被搶占，但相較於標準可搶占型，競價型提供了更長的運行時間和更高的穩定性。
使用期限：沒有固定的使用期限限制，但隨時可能被搶占。
適用場景：適合具備高容錯性和分布式計算的工作負載。
3. 不可搶占型 (Non-Preemptible)
價格：比可搶占型和競價型貴。
搶占：這些 VM 不會被 Google Cloud 搶占，除非您手動停止或刪除。
使用期限：無使用期限限制，除非手動關閉。
適用場景：適合需要長期穩定運行且不能中斷的工作負載。
匯總表


|特性|標準可搶占型<br>(Standard Preemptible)|競價型(Spot)|不可搶占型<br>(Non-Preemptible)|
|-----|--------|-------|------|
|價格|便宜（節省 70% 到 90%）|最便宜（動態定價）|最貴|

| Name  | Quantity |
| ----- | -------- |
| Apple | 3        |
| Egg   | 12       |


## dataproc GPU
- GPU 與 VM 機器型號 （https://cloud.google.com/compute/docs/gpus）
- GPU 與 region 、zone （特別注意臺灣地區如果要使用比較高階的 GPU 要指定 zone）
- GPU attach script (script 自動代入)
	- gs://goog-dataproc-initialization-actions-${region}/gpu/install_gpu_driver.sh
- GPU 與 VM 機器型號
	- --master-accelerator='type=nvidia-tesla-t4,count=1'
	- --worker-accelerator='type=nvidia-tesla-t4,count=1'
	- 參考型號
- GPU 與 region、zone  
	- --zone=asia-east1-a
	- 參考區域
- 其他設定 (shell script)
	- --initialization-actions=”gs://hadooopincloud-codes/projects/XXX…/XXX/add_libcudnn8.sh”
	- example：tensorflow 需要 libcudnn8 但是 install_gpu_driver.sh 沒有寫到，所以需要自行寫腳本安裝 。

``` python
> physical_devices = tf.config.list_physical_devices('GPU')
> print("GET GPU:", physical_devices)
# 失敗
GET GPU：[] 
# 成功
GET GPU: [PhysicalDevice(name='/physical_device:GPU:0', device_type='GPU')]
```