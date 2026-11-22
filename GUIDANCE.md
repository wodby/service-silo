# Silo on Wodby

What Wodby sets up for Silo on this service.

## What it is

[Silo](https://silo.pgsty.com) is an S3-compatible object storage server, a maintained fork of MinIO. The service runs
the stock `pgsty/silo` image as one replica, with its data on the `data` volume at `/data`. It is meant for apps in the
same environment that store files in S3, such as Plane.

## Connecting an app

- The S3 API is on the private port `s3` (9000), at `http://<link host>:<link port>`. It has no public address.
- One access key pair is generated per service: the tokens `access_key` and `secret_key`, also its root credentials
  (`MINIO_ROOT_USER`, `MINIO_ROOT_PASSWORD`). A linking service reads them as `{{link.tokens.access_key}}` and
  `{{link.tokens.secret_key}}`.
- Any region works; use `us-east-1`. Use path-style addressing (`http://host:9000/<bucket>/<key>`).
- Buckets are created by the app, for example with its own "create bucket" step, or with any S3 client.
- An app that gives browsers presigned URLs must sign them for its own public address and route the bucket path to
  this service, keeping the browser's `Host` header, because the signature covers the host.

## Limits

- One server, one volume: no erasure coding, replication or high availability. Back up the `data` volume.
- No web console (`MINIO_BROWSER=off`) and no public endpoint.
- The volume size is fixed by the service setting; there are no per-bucket quotas.

## Check the result

- `curl -s -o /dev/null -w '%{http_code}' localhost:9000/minio/health/ready` in the container returns `200`.
