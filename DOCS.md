## Description

This plugin enables you to cache build resources in an [s3](https://aws.amazon.com/s3/) compatible store for a Vela pipeline.

Source Code: https://github.com/go-vela/vela-s3-cache

Registry: https://hub.docker.com/r/target/vela-s3-cache

## Usage

Sample of restoring a cache:

```yaml
steps:
  - name: restore_cache
    image: target/vela-s3-cache:v0.1.0
    pull: true
    parameters:
      action: restore
      root: mybucket
      server: mybucket.s3-us-west-2.amazonaws.com
```

Sample of rebuilding a cache:

```yaml
steps:
  - name: rebuild_cache
    image: target/vela-s3-cache:v0.1.0
    pull: true
    parameters:
      action: rebuild
      root: mybucket
      server: mybucket.s3-us-west-2.amazonaws.com
      mount:
        - .gradle
```

Sample of flushing a cache:

```yaml
steps:
  - name: flushing_cache
    image: target/vela-s3-cache:v0.1.0
    pull: true
    parameters:
      action: flush
      root: mybucket
      server: mybucket.s3-us-west-2.amazonaws.com
```

## Secrets

**NOTE: Users should refrain from configuring sensitive information in your pipeline in plain text.**

You can use Vela secrets to substitute sensitive values at runtime:

```diff
steps:
  - name: restore_cache
    image: target/vela-s3-cache:v0.1.0
    pull: true
+   secrets: [ cache_access_key, cache_secret_key ]
    parameters:
      action: restore
      root: mybucket
      server: mybucket.s3-us-west-2.amazonaws.com
-     access_key: AKIAIOSFODNN7EXAMPLE
-     secret_key: 123456789QWERTYEXAMPLE
```

## Parameters

The following parameters can used to configure all image actions:

| Name        | Description                          | Required | Default |
| ----------- | ------------------------------------ | -------- | ------- |
| `action`    | action to perform against s3         | `true`   | `N/A`   |
| `log_level` | set the log level for the plugin     | `true`   | `info`  |
| `server`    | s3 instance to communicate with      | `true`   | `N/A`   |
| `access_key`| access key for communication with s3 | `true`   | `N/A`   |
| `secret_key`| secret key for communication with s3 | `true`   | `N/A`   |
| `root`      | name of the s3 bucket                | `true`   | `N/A`   |
| `prefix`    | path prefix for the object(s)        | `true`   | `N/A`   |

#### Restore

The following parameters are used to configure the `restore` action:

| Name       | Description                                                | Required | Default |
| ---------- | ---------------------------------------------------------- | -------- | ------  |
| `filename` | the name of the cache object                               | `false`  | `true`  |
| `mount`    | the file or directories locations to build your cache from | `true`   | `N/A`   |
| `timeout`  | the timeout for the call to s3                             | `false`  | `true`  |


#### Rebuild

The following parameters are used to configure the `rebuild` action:

| Name       | Description                    | Required | Default |
| ---------- | ------------------------------ | -------- | ------  |
| `filename` | the name of the cache object   | `false`  | `true`  |
| `timeout`  | the timeout for the call to s3 | `false`  | `true`  |

#### Flush

The following parameters are used to configure the `flush` action:

| Name  | Description                                             | Required | Default |
| ----- | ------------------------------------------------------- | -------- | ------- |
| `age` | delete the objects past a specific age (i.e. 60m, 7d)   | `false`  | `14d`   |

#### Upload

## Template

COMING SOON!

## Troubleshooting

Below are a list of common problems and how to solve them:
