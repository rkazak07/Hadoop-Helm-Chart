# Helm chart kubernetes Deploy
------------------------------------------------------------------------------------------------------------------------------------------

## `values.yaml` please revise the file according to your own system. Set the StorageClass, image version, Persistence Volume, Replicas.


# values.yaml files edited

```
nano values.yaml
```


## Creating Namespaces

```
kubectl create ns hadoop-cls
```

## Installing the Chart

Note that you need at least 2GB of free memory per NodeManager pod, if your cluster isn't large enough, not all pods will be scheduled.

The optional `calc_resources.sh` script is used as a convenience helper to set the yarn.numNodes, and yarn.nodeManager.resources appropriately to utilize all nodes in the Kubernetes cluster and a given percentage of their resources. For example, with a 3 node n1-standard-4 GKE cluster and an argument of 50, this would create 3 NodeManager pods claiming 2 cores and 7.5Gi of memory.


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

| Parameter                              | Description                                                    | Default                                                           |
| -------------------------------------- | -------------------------------------------------------------- | ----------------------------------------------------------------- |
| `image.repository`                     | Hadoop image                                                   | `rkazak1/hadoop`                                           |
| `image.tag`                            | Hadoop image tag                                               | `3.3.4`                                                           |
| `imagee.pullPolicy`                    | Pull policy for the images                                     | `IfNotPresent`                                                    |
| `hadoopVersion`                        | Version of hadoop libraries being used                         | `3.3.4`                                                           |
| `antiAffinity`                         | Pod antiaffinity, `hard` or `soft`                             | `hard`                                                            |
| `hdfs.nameNode.pdbMinAvailable`        | PDB for HDFS NameNode                                          | `1`                                                               |
| `hdfs.nameNode.resources`              | resources for the HDFS NameNode                                | `requests:memory=256Mi,cpu=10m,limits:memory=2048Mi,cpu=1000m`    |
| `hdfs.dataNode.replicas`               | Number of HDFS DataNode replicas                               | `1`                                                               |
| `hdfs.dataNode.pdbMinAvailable`        | PDB for HDFS DataNode                                          | `1`                                                               |
| `hdfs.dataNode.resources`              | resources for the HDFS DataNode                                | `requests:memory=256Mi,cpu=10m,limits:memory=2048Mi,cpu=1000m`    |
| `hdfs.webhdfs.enabled`                 | Enable WebHDFS REST API                                        | `true`                                                            |
| `yarn.resourceManager.pdbMinAvailable` | PDB for the YARN ResourceManager                               | `1`                                                               |
| `yarn.resourceManager.resources`       | resources for the YARN ResourceManager                         | `requests:memory=256Mi,cpu=10m,limits:memory=2048Mi,cpu=1000m`    |
| `yarn.nodeManager.pdbMinAvailable`     | PDB for the YARN NodeManager                                   | `1`                                                               |
| `yarn.nodeManager.replicas`            | Number of YARN NodeManager replicas                            | `1`                                                               |
| `yarn.nodeManager.parallelCreate`      | Create all nodeManager statefulset pods in parallel (K8S 1.7+) | `false`                                                           |
| `yarn.nodeManager.resources`           | Resource limits and requests for YARN NodeManager pods         | `requests:memory=2048Mi,cpu=1000m,limits:memory=2048Mi,cpu=1000m` |
| `persistence.nameNode.enabled`         | Enable/disable persistent volume                               | `false`                                                           |
| `persistence.nameNode.storageClass`    | Name of the StorageClass to use per your volume provider       | `longhorn`                                                               |
| `persistence.nameNode.accessMode`      | Access mode for the volume                                     | `ReadWriteOnce`                                                   |
| `persistence.nameNode.size`            | Size of the volume                                             | `50Gi`                                                            |
| `persistence.dataNode.enabled`         | Enable/disable persistent volume                               | `false`                                                           |
| `persistence.dataNode.storageClass`    | Name of the StorageClass to use per your volume provider       | `longhorn`                                                               |
| `persistence.dataNode.accessMode`      | Access mode for the volume                                     | `ReadWriteOnce`                                                   |
| `persistence.dataNode.size`            | Size of the volume                                             | `200Gi`                                                           |

# References

- Original K8S Hadoop adaptation this chart was derived from: https://github.com/Comcast/kube-yarn
- https://github.com/pfisterer/apache-hadoop-helm
- Image Referance: https://github.com/Comcast/kube-yarn/tree/master/image
