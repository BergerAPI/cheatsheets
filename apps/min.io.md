Min.io is a high performance object storage solution that provides an Amazon Web Services S3-compatible API and supports all core S3 features. This example will show how to create and deploy a Manifest-File to run Min.IO.

## Kubernetes

### Downloading

Linux
```shell
curl "https://raw.githubusercontent.com/minio/docs/master/source/extra/examples/minio-dev.yaml" -O
```

Windows:
```powershell
(New-Object System.Net.WebClient).DownloadString("https://raw.githubusercontent.com/minio/docs/master/source/extra/examples/minio-dev.yaml") >> minio-dev.yaml
```

### Applying

Applying is as simple as running:
```shell
kubectl apply -f minio-dev.yaml
```

## Docker

Create a ``config`` file:
```
MINIO_ROOT_USER=CHANGEME
MINIO_ROOT_PASSWORD=CHANGEME
MINIO_VOLUMES="/mnt/data"
```

Create a ``run.sh`` file:
```shell
docker run -dt                                  \
  -p 9000:9000 -p 9090:9090                     \
  -v "YOUR_PATH/data/:/mnt/data"                \
  -v "YOUR_PATH/config:/etc/config.env"         \
  -e "MINIO_CONFIG_ENV_FILE=/etc/config.env"    \
  --name "minio"                                \
  minio/minio server --console-address ":9090"
```
This will start the console on port ``9090`` and the actual API on port ``9000``, and expose both ports.