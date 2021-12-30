# Run your personal Postgres database server on the Neu.ro platform with neuro-flow

This is a [`neuro-flow`](https://github.com/neuro-inc/neuro-flow) action launching an instance of [Postgres SQL server](https://hub.docker.com/_/postgres).

## Usage example could be found in the [.neuro/live.yaml](.neuro/live.yaml) file.

## Arguments

### `db_dir_remote`

A place to store Postgres files.
This should be either `storage:`, or `disk:`. 

#### Example

You can use platform storage as a backend.
```yaml
args:
	db_dir_remote: storage:might/be/with/some/path
```

Or an example with the disk (using its name, or ID).
Note, you cannot pass a disk internal path, since only entire disk could be mounted. 
```yaml
args:
    db_dir_remote: disk:disk-e7440b1a-7d09-4a15-a237-02ec022e6c22
```


### `job_name`

Predictable subdomain name which replaces the job's ID in the full job URI. `""` by default (in this case, the job name will be [generated](https://neu-ro.gitbook.io/neuro-flow/reference/live-workflow-syntax#jobs.less-than-job-id-greater-than.name) by Neuro-Flow).

#### Example

```
args:
	job_name: "postgres-server"
```

### `preset`

Resource preset to use when running the Postgres job. `""` by default (i.e., the first preset specified in the `neuro config show` list will be used).

#### Example

```
args:
    preset: cpu-small
```

### `http_auth`

Boolean value specifying whether to use platform HTTP authentication for Postgres or not. `False` by default.

#### Example

Enable platform provided HTTP authentication for Postgres by setting this argument to True.
```yaml
args:
    http_auth: True
```


### `life_span`

A value specifying how long the Postgres server job should be running. `"10d"` by default.

#### Example

```
args:
	life_span: 1d2h3m
```

### `db_version`

PostgreSQL server version, used as an image tag. `14` by default.
Checkout list of available tags [here](https://hub.docker.com/_/postgres?tab=tags).

#### Examples

```yaml
args:
    db_version: "12-bullseye"
```


### `db_user`

Postgres DB server username. `postgres` by default.
It's set as environment variable `POSTGRES_USER` within the server instance.

#### Examples

```yaml
args:
    db_user: "neu.ro"
```


### `db_password`

Postgres DB server password. `password` by default.
It's set as environment variable `POSTGRES_PASSWORD` within the server instance.

#### Examples

```yaml
args:
    db_password: "I@#PN$)9!"
```

### `postgres_initdb_args`

Value of Postgress env var `POSTGRES_INITDB_ARGS`. Empty be default.

#### Example

```yaml
args:
    POSTGRES_INITDB_ARGS: "--data-checksums"
```


# Known issues
### `chmod: changing permissions of '...': Operation not permitted`
This might happen if the platform `storage:` backend does not support such an operation (happened with Azure Files and CIFS).
A work-around for this is to use a platform `disk:` to host the SQLite data.
