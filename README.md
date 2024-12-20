# Run your personal Postgres database server on the Apolo platform with apolo-flow

This is a [`apolo-flow`](https://github.com/neuro-inc/neuro-flow) action launching an instance of [Postgres SQL server](https://hub.docker.com/_/postgres).

## Usage example could be found in the [.neuro/live.yaml](.neuro/live.yaml) file.

## Arguments


### Required

- `db_user`

    Postgres DB server username.
    It's set as environment variable `POSTGRES_USER` within the server instance.

    ```yaml
    args:
        db_user: "postgres"
    ```


- `db_password`

    Postgres DB server password.
    It's set as environment variable `POSTGRES_PASSWORD` within the server instance.
    Consider usage of [Apolo secrets](https://docs.apolo.us/index/core/apps/pre-installed/secrets).

    ```yaml
    args:
        db_password: "I@#PN$)9!"
    ```


- `db_dir_remote`

    A place to store Postgres files.
    This should be either `storage:`, or `disk:`. 

    **Examples**:
    
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


### Optional
- `job_name`

    Predictable subdomain name which replaces the job's ID in the full job URI. `""` by default (in this case, the job name will be [generated](https://docs.apolo.us/apolo-flow-reference/workflow-syntax/live-workflow-syntax#jobs.less-than-job-id-greater-than.name) by `apolo-flow`).

    ```yaml
    args:
        job_name: "postgres-server"
    ```


- `preset`

    Resource preset to use when running the Postgres job. `""` by default (i.e., the first preset specified in the `apolo config show` list will be used).

    ```yaml
    args:
        preset: cpu-small
    ```


- `life_span`

    A value specifying how long the Postgres server job should be running. `"10d"` by default.

    ```yaml
    args:
        life_span: 1d2h3m
    ```


- `db_version`

    PostgreSQL server version, used as an image tag. `"14"` by default.
    Checkout list of available tags [here](https://hub.docker.com/_/postgres?tab=tags).


    ```yaml
    args:
        db_version: "12-bullseye"
    ```

- `postgres_initdb_args`

    Value of Postgress env var `POSTGRES_INITDB_ARGS`. Empty be default.

    ```yaml
    args:
        POSTGRES_INITDB_ARGS: "--data-checksums"
    ```


# Known issues
### `chmod: changing permissions of '...': Operation not permitted`
This might happen if the platform `storage:` backend does not support such an operation (happened with Azure Files and CIFS).
A work-around for this is to use a platform `disk:` to host the Postgres data.
