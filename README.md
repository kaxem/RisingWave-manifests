# Rising Wave
RisingWave is a distributed SQL streaming database that enables simple, efficient, and reliable processing of streaming data.

## Prerequisites
Before you start, make sure you have:

* x86_64 (64-bit Intel/AMD CPUs)
* Minimum:
    2 CPU cores
    8 GB memory
* Prometheus up and running

## Steps to Deploy RisingWave with MinIO:
First create a bucket in minio named `risingwave-hummock-1` (any name is ok but you should change address in meta-deployment file)
2- Apply each manifest in kubernetes.

### Important Notes:
**1- It's better to separate Compactor and Compute node in two servers and give compactor more CPU and Compute more memory.** to make sure they're in seperate nodes you can use `affinity`
example:
```
spec:
      affinity:
        nodeAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
            nodeSelectorTerms:
            - matchExpressions:
              - key: kubernetes.io/hostname
                operator: In
                values:
                - <node-name>
```
**2- "--meta-address" value MUST have `http://` and it's value is meta endpoint address in k8s**
3- Prometheus metrics could be enabled/diabled with: 
```
annotations:
        prometheus.io/port: '1222'
        prometheus.io/scrape: 'true'
        prometheus.io/path: '/'
```