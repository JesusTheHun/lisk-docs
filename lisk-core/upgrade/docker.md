# Lisk Core Docker Upgrade

## Upgrade minor versions

To upgrade a Lisk installation using Docker Compose you simply need to export the new version number in `ENV_LISK_VERSION`.

Example: Upgrade from Lisk Core 1.5 to 1.6 by executing:

```bash
export ENV_LISK_VERSION=1.6.0
docker-compose up -d
```

To upgrade the Lisk installation run `docker-compose up -d`.

## Upgrade major versions

For major version upgrades, there can be additional changes in `docker-compose.yml`.

In this case, Lisk Core provides an updated version of `docker-compose.yml` in the [Lisk Core repository](https://github.com/LiskHQ/lisk-core) on Github, inside the `docker` folder: `https://github.com/LiskHQ/lisk-core/tree/<CURRENT_VERSION>/docker`

(Where `<CURRENT_VERSION>` is the [version tag](https://github.com/LiskHQ/lisk-core/tags) of the version you want to upgrade to.)

Use this file as a new template for your `docker-compose.yml`.

The rest of the upgrade process is analog to the [minor version upgrade](#upgrade-minor-versions).
