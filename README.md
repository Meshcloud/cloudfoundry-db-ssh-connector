# cloudfoundry-db-ssh-connector

Easily connect to database services running on Cloud Foundry using `cf ssh`.
Sets up a `cf ssh` tunnel to an app running on Cloud Foundry and establishes a port-forwarding to a database instance
of your choosing

## Usage

```bash
dbconnect <app> <service instance name> [port]
```

For example:

```text
$ cf a
Getting apps in org meshstack / space playground as 1db36b1c-8102-4af1-9f33-8b9929d2ee95...
OK

name    requested state   instances   memory   disk   urls
dbssh   started           1/1         32M      1G

$ cf s
Getting services in org meshstack / space playground as 1db36b1c-8102-4af1-9f33-8b9929d2ee95...

name   service          plan   bound apps   last operation
ps     osb-postgresql   s      dbssh        create succeeded


10:32 $ ./dbconnect dbssh ps
Here's some helpful commands for you:
psql postgres://GfZlXQ5moI:J7m9VVn5gMHkpGd@127.0.0.1:63306/ea89234e-9660-44e1-8cee-e33e6b7ada99

Establishing connection... (exit to close tunnel)
cf ssh -L 63306:10.0.64.47:5432 dbssh
```

## Prepare an SSH gateway app

In case you don't have a `cf ssh` enabled app running, you can quickly deploy a simple `dbssh` app that you can leverage
as a gateway.

```bash
cf push -o bash --no-manifest --no-route --health-check-type none -m 32M dbssh
cf bind-service dbssh <my-db-service>
cf restage dbssh
```

## Installation / Requirements

Simply git clone this tool.
Needs a valid [node](https://nodejs.org/) interpreter on your system. No packages/npm etc. required.

## Contributing

The `$VCAP_SERVICES` parsing is simplified and works great on the clouds and services that we used so far.
If you want to add detection for a new service, please send us a PR!