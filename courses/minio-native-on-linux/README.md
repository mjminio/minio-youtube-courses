# Installing and Upgrading MinIO Native on Linux

At the end of this hands-on course, system admins will be able to configure Linux, install the MinIO native linux binary, configure the Systemd file and MinIO configuration files for a robust and secure production grade installation.

Having this knowledge should easily allow you to deploy MinIO on Linux using your automation platform of choice and repeatedly and reliably deploy to all of your desired environments in a production friendly way.

## YouTube Links

- Playlist: https://youtube.com
- Video 1: https://youtube.com
- Video 2: https://youtube.com
- Video 3: https://youtube.com

## Purpose of Repo and Disclaimer

This repo is intended to share with you the commands, code, and configuration files that were used in the YouTube course videos. These are not necessarily the exact production files you should use. We recommend you always refer to the official documentation.

The content here comes with no guarantee of proper function and are intended as examples.

## Downloading MinIO

We recommend that you follow along directly with the MinIO documentation as these instructions and versions will be updated over time.

- MinIO Object Storage for Linux Docs: https://min.io/docs/minio/linux/index.html
- MinIO Releases on Github: https://github.com/minio/minio/releases 

### Download and Install on Debian Based
```
wget https://dl.min.io/server/minio/release/linux-amd64/archive/minio_20221029062133.0.0_amd64.deb -O minio.deb
sudo dpkg -i minio.deb
```

### Download and Install on RHEL Based
```
wget https://dl.min.io/server/minio/release/linux-amd64/archive/minio-20221029062133.0.0.x86_64.rpm -O minio.rpm
sudo dnf install minio.rpm
```

### Download and Install Generic Binary
```
wget https://dl.min.io/server/minio/release/linux-amd64/minio
chmod +x minio
sudo mv minio /usr/local/bin/
```

## Running MinIO from the CLI
```
mkdir ~/minio
minio server ~/minio --console-address :9090
```

## Running MinIO with systemd
```
# Add minio-user group
groupadd -r minio-user

# Add minio-user with homedir and add to group
useradd -m -d /home/minio-user -r -g minio-user minio-user

# Create directory for MinIO to store data and change ownership
mkdir /mnt/data
chown minio-user:minio-user /mnt/data

# Create systemd file (for generic MinIO server install) from docs or sample-minio.service in this repo
vi /etc/systemd/system/minio.service

# Create MinIO configuration file from the docs or sample-minio in this repo
vi /etc/default/minio

# Change ownership of /etc/default/minio
chown minio-user:minio-user /etc/default/minio

# Create /home/minio-user/.minio/certs dir
mkdir -p /home/minio-user/.minio/certs

# Change ownership of /home/minio-user/.minio/certs
chown -R minio-user:minio-user /home/minio-user/.minio/certs

# Copy public.crt and private.key to /home/minio-user/.minio/

# Change ownership of keys
chown minio-user:minio-user /home/minio-user/.minio/certs/public.crt
chown minio-user:minio-user /home/minio-user/.minio/certs/private.key

# Enable and start the MinIO Service
sudo systemctl enable --now minio.service

# Check the status of the MinIO service
sudo systemctl status minio.service
journalctl -f -u minio.service

```

## Updating the MinIO Binary on Linux

Updating MinIO Docs: https://min.io/docs/minio/linux/operations/install-deploy-manage/upgrade-minio-deployment.html#minio-upgrade-systemctl

1. Download and install the latest binary
2. Connect the `mc` client to the server and run `mc admin service restart ALIAS`
3. Validate the update with the `mc admin info ALIAS` command
4. Update the MinIO client with `mc update`

## About Minio

MinIO offers high-performance, S3 compatible object storage.

Native to Kubernetes, MinIO is the only object storage suite available on every public cloud, every Kubernetes distribution, the private cloud and the edge. MinIO is software-defined and is 100% open source under GNU AGPL v3.

- **Website:** https://min.io/
- **Official Github:** https://github.com/minio
