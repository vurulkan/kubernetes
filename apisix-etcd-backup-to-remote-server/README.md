# 🚀 Kubernetes CronJob for APISIX ETCD Backup

This guide explains how to configure a **Kubernetes CronJob** to back up APISIX's **ETCD data** and transfer it to a remote server via SSH.

## 🛠️ Configuration

Modify the relevant sections in the `etcd-backup-cronjob.yaml` file:

```bash
LOCAL_SERVER="10.42.0.200"
LOCAL_USER="etcdbackup"
LOCAL_PATH="/home/etcdbackup/apisix-etcd-backup"
```
## 🔑 Generating an SSH Key Pair

```bash
ssh-keygen
```

## 🔐 Adding the Private Key to Kubernetes

```bash
kubectl create secret generic sftp-secret --from-file=id_rsa=./path/to/privatekey -n yournamespace
```

## 🖥️ Configuring SSH Access on the Remote Server
```bash
cat path/to/publickey >> /home/$USER/.ssh/authorized_keys
```
| Example Public Key  | Example Private Key |
| ------------- | ------------- |
| ssh-ed25519 AAAAC... mustafav@vurulkan.local  | -----BEGIN OPENSSH PRIVATE KEY----- b3BlbnNzaC1... -----END OPENSSH PRIVATE KEY-----  |

## 📦 Deploying Kubernetes Resources
#### Apply the Service Account:

```bash
kubectl apply -f kubernetes-service-account.yaml -n yournamespace
```

#### Deploy the CronJob
```bash
kubectl apply -f etcd-backup-cronjob.yaml -n yournamespace
```

## ✅ Verify the CronJob Execution

Manually trigger the CronJob and check the logs:
```bash
kubectl create job --from=cronjob/etcd-backup-cronjob -n yournamespace
kubectl logs -l job-name=etcd-backup-cronjob -n yournamespace
```
