# Stellar Bifrost

## The simplest and cleanest Docker image for running bifrost

Build and run:
```
docker-compose build
docker-compose up -d
```

Defaults to testnet.

Creates a folder 'stellar' in your home folder.  Everything is stored there, delete it to reset.

Edit docker-compose.yml for mainnet

Pull requests welcome!

## Setup

Look at `docker-compose.yml` as a guide.

### Config

- *From environment variables*. You can specify the environment variables (see `docker-compose.yml`) and allow `entry.sh` to generate the config file for you.
- *Supply your own `bifrost.cfg`*. If `entry.sh` sees a file in `/opt/bifrist/bifrost.cfg`, it will use that and ignore any environment variables. This is useful when you need a more customized cfg, and attaching a volume (like a configmap or secret in Kubernetes) is easier.

## Donations
If you like this code, a [`donation`](https://stellarkit.io/#/donate) would be appreciated.

```
XLM: GCYQSB3UQDSISB5LKAL2OEVLAYJNIR7LFVYDNKRMLWQKDCBX4PU3Z6JP
```
