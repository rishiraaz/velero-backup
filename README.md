# velero-backup

Velero is a tool that enables backup and restore Kubernetes cluster resources and persistent volumes. It simplifies the task of taking backups/restores, migrating resources to other cluster and replication of clusters.
Velero is made up of two components:
•	A server that runs on cluster
•	A command-line client that runs locally
Installing the Client component of Velero:

```
$ wget https://github.com/vmware-tanzu/velero/releases/tag/v1.2.0
$ tar -xvzf velero-v1.2.0-darwin-amd64.tar.gz 
$ sudo mv velero /usr/local/bin/ 
$ velero help
```

Before installing the server component, we will create a s3 bucket to store the backup, an IAM role to allow s3 access to velero pod. We are assuming that AWS access, in the cluster, is being manager by kiam or kube2iam.
Create an s3 bucket: velero-bucket

```
$ aws s3api create-bucket --bucket velero-bucket --region us-east-1
```

Create IAM policy: velero-policy
velero-policy.json

Create an IAM role (velero-role) and associate this policy to it. Add the below trust relation to the role.

velero-trust-policy.json

To install the server component in cluster:

```
helm repo add vmware-tanzu https://vmware-tanzu.github.io/helm-charts
helm install --namespace backup -f values.yaml vmware-tanzu/velero

```

values.yaml

Verify if the pod is up:
```
kubectl get pods -n backup |grep velero
```
