# AI4ME: `stats4me`

A telemetry stack for use in the research & development of OBM services.

---

## Overview

  - **Collection:** This stack uses Telegraf to collect metrics from the
    local system via "inputs" and export them to the data store.

  - **Storage:** InfluxDB is a time-series database that simply stores
    timestamped (and labeled) data that can be queried using the flux query
    language.

  - **Visualization:** For pretty graphs, Grafana can house web-based dashboards
    that contain graphs etc that visualize data. It can be set to collect data
    from InfluxDB via flux queries.

## Usage

This is a `docker compose` stack that will download run all the required tools
using [docker](https://docs.docker.com/engine/install/) and [docker
compose](https://docs.docker.com/compose/install/linux/).

  - **Downloading:** `docker compose pull`
  - **Running:** `docker compose up -d`
  - **Logs:** `docker compose logs -f`
  - **Stop:** `docker compose down`

Accessing these services is done via a browser on the localhost. InfluxDB's UI
can be accessed at port `8086` by default and Grafana at port `3000` (again, by
default).

## Configuration

Each service has a configuration, and each can be complex to configure if used
in complex ways. A basic configuration setup for each is generated by this
repository.

### InfluxDB

To get started with InfluxDB, start with providing an initial config. In this
repo, this can be done via env vars found in the [.env](./.env) file.

 - **`INFLUXDB_USERNAME`**: Is the username for accessing the default InfluxDB account.
 - **`INFLUXDB_PASSWORD`**: Is the password for accessing the default InfluxDB account.
 - **`INFLUXDB_BUCKET`**: Is the name of the initial bucket that can be used to
   store data in.
 - **`INFLUXDB_ORG`**: Is the name of the initial organization in the InfluxDB.
 -  **`INFLUXDB_TOKEN`**: The admin API token used to get and push data to the database.

> These values are also used by `telegraf` in the initial config so that it can
> be autoconfigured to have an influx output to the initial bucket.

### Telegraf

Telegraf is configured by a `toml` file that can be found at
[telegraf/telegraf.conf](./telegraf/telegraf.conf). Initially, this is setup
with a few basic input plugins (such as docker) and it outputs data to the
`influxdb` instance at the initial bucket. For more about telegraf plugins and
configuration, see [here](https://docs.influxdata.com/telegraf/v1.27/plugins).

So Telegraf can collect data from the docker engine, it needs to be running in
the docker group. For this, set the `DOCKER_GID` in the [.env](./.env) file to
the value found for the docker group ID. This can be found by running `id` in
the command line (provided your current user is in the docker group too).

### Grafana

Grafana has very little configuration done by this repository... The default
credentials are unchanged (typically: `admin`, `admin`). For more on Grafana
config, see [here](https://grafana.com/docs/).

