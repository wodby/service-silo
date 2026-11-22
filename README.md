# Silo service for Kubernetes on Wodby

Run Silo, an S3-compatible object storage server, on Kubernetes with Wodby.

This repository defines the Wodby service manifests and operational
configuration for Silo.

- [Browse Wodby services](https://wodby.com/services)
- [Wodby service documentation](https://wodby.com/docs/2.0/services/)
- [Service manifest reference](https://wodby.com/docs/2.0/services/template/)

## Wodby stacks using this service

- [Plane application stack](https://github.com/wodby/stack-plane)

## Service overview

| Property | Manifest configuration |
| --- | --- |
| Service name | `silo` |
| Type | Datastore |
| Versions | `2026` |
| Endpoints | `silo`: `s3` 9000, private |
| Tokens | `access_key`, `secret_key` |
| Volumes | `data` |
| Image | Stock [`pgsty/silo`](https://github.com/pgsty/silo) |

## Use this service

Link it from a service that stores files in S3, as the [Plane application stack](https://github.com/wodby/stack-plane)
does. The linking service reads the endpoint from the link and the credentials from the link's tokens.

A service is a reusable component and does not deploy by itself. The stack
defines its links, settings, versions, resources, and relationship to the rest
of the application.

## Maintain a custom version

1. Fork this repository.
2. Edit the service manifest and referenced files.
3. Import the repository as a [Git-backed service](https://wodby.com/docs/2.0/services/create/#create-a-git-backed-service).
4. Reference the service from a stack manifest.

Validate the manifests with:

```bash
wodby service validate-manifest service.yml --org <org-id>
```

See the [service manifest reference](https://wodby.com/docs/2.0/services/template/) and the [managed services index](https://github.com/wodby/services).
