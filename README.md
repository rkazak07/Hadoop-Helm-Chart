# Helm chart kubernetes Deploy

## `values.yaml` please revise the file according to your own system. Set the StorageClass, image version, Persistence Volume, Replicas.
## Danile/hadoop 3.2.1 was used as the image version. If we want different images, we can use them.

# values.yaml files edited

```
nano values.yaml
```


## Creating Namespaces

```
kubectl create ns hadoop-cls
```

## Installing the Chart

```
helm install hadoop-cls . -f values.yaml -n hadoop-cls
```

## Upgrade the Chart

```
helm upgrade --install hadoop-cls . -f values.yaml -n hadoop-cls
```

```
kubectl get all
```


## Configuration

The following table lists the configurable parameters of the Hadoop chart and their default values.

| Parameter                                         | Description                                                                        | Default                                                          |
| ------------------------------------------------- | -------------------------------                                                    | ---------------------------------------------------------------- |
| `image.repository`                                | Hadoop image ([source](https://github.com/Comcast/kube-yarn/tree/master/image))    | `danisla/hadoop`                                                 |
| `image.tag`                                       | Hadoop image tag                                                                   | `3.2.1`                                                          |
| `imagee.pullPolicy`                               | Pull policy for the images                                                         | `IfNotPresent`                                                   |
| `hadoopVersion`                                   | Version of hadoop libraries being used                                             | `3.2.1`                                                          |
| `antiAffinity`                                    | Pod antiaffinity, `hard` or `soft`                                                 | `hard`                                                           |
| `hdfs.nameNode.pdbMinAvailable`                   | PDB for HDFS NameNode                                                              | `1`                                                              |
| `hdfs.nameNode.resources`                         | resources for the HDFS NameNode                                                    | `requests:memory=256Mi,cpu=200m,limits:memory=2048Mi,cpu=2000m`   |
| `hdfs.dataNode.replicas`                          | Number of HDFS DataNode replicas                                                   | `1`                                                              |
| `hdfs.dataNode.pdbMinAvailable`                   | PDB for HDFS DataNode                                                              | `1`                                                              |
| `hdfs.dataNode.resources`                         | resources for the HDFS DataNode                                                    | `requests:memory=256Mi,cpu=100m,limits:memory=2048Mi,cpu=2000m`   |
| `hdfs.webhdfs.enabled`                            | Enable WebHDFS REST API                                                            | `false`
| `yarn.resourceManager.pdbMinAvailable`            | PDB for the YARN ResourceManager                                                   | `1`                                                              |
| `yarn.resourceManager.resources`                  | resources for the YARN ResourceManager                                             | `requests:memory=256Mi,cpu=50m,limits:memory=4096Mi,cpu=2000m`   |
| `yarn.nodeManager.pdbMinAvailable`                | PDB for the YARN NodeManager                                                       | `1`                                                              |
| `yarn.nodeManager.replicas`                       | Number of YARN NodeManager replicas                                                | `2`                                                              |
| `yarn.nodeManager.parallelCreate`                 | Create all nodeManager statefulset pods in parallel (K8S 1.7+)                     | `false`                                                          |
| `yarn.nodeManager.resources`                      | Resource limits and requests for YARN NodeManager pods                             | `requests:memory=2048Mi,cpu=2000m,limits:memory=4096Mi,cpu=2000m`|
| `persistence.nameNode.enabled`                    | Enable/disable persistent volume                                                   | `false`                                                          | 
| `persistence.nameNode.storageClass`               | Name of the StorageClass to use per your volume provider                           | `-`                                                              |
| `persistence.nameNode.accessMode`                 | Access mode for the volume                                                         | `ReadWriteOnce`                                                  |
| `persistence.nameNode.size`                       | Size of the volume                                                                 | `20Gi`                                                           |
| `persistence.dataNode.enabled`                    | Enable/disable persistent volume                                                   | `false`                                                          | 
| `persistence.dataNode.storageClass`               | Name of the StorageClass to use per your volume provider                           | `-`                                                              |
| `persistence.dataNode.accessMode`                 | Access mode for the volume                                                         | `ReadWriteOnce`                                                  |
| `persistence.dataNode.size`                       | Size of the volume                                                                 | `50Gi`                                                          |




# References

- Original K8S Hadoop adaptation this chart was derived from: https://github.com/Comcast/kube-yarn
- Chart can use the hadoop config for the hadoop cluster and use the YARN executor: https://github.com/kubernetes/charts/tree/master/stable/zeppelin
- Image Referance: https://github.com/Comcast/kube-yarn/tree/master/image
