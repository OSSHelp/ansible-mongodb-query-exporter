# mongodb-query-exporter

[![Build Status](https://drone.osshelp.ru/api/badges/ansible/mongodb-query-exporter/status.svg)](https://drone.osshelp.ru/ansible/mongodb-query-exporter)

Ansible role, that installs and configures the [mongodb-query-exporter](https://github.com/raffis/mongodb-query-exporter).

## Usage (example)

```yaml
  roles:
    - { role: mongodb-query-exporter,
        mongodb_exporter_metrics_config: 'files/exporters/mongo/metrics-config.yml',
        mongodb_exporter_params: {
          bind: '0.0.0.0:9412',
          logLevel: info,
          mongodb: {
            uri: 'mongodb://127.0.0.1:27017',
            connectionTimeout: 9,
            maxConnection: 2,
            defaultCacheTime: 14
          }
        }
    }
```

## Available parameters

### Main

Main params, read carefully before using the role!

| Param | Description |
| -------- | -------- |
| mongodb_exporter_params | You need to describe all needed params in this array. In fact, it will be moved "as is" to the exporter config. Available params for the exporter could be found [here](https://github.com/raffis/mongodb-query-exporter/blob/master/README.md#configuration). Do not try to describe "metrics" section via playbook (see below) |
| mongodb_exporter_metrics_config | relative path to the file in repo, that contains the "metrics" section. The contents of the file will be merged with main config |

### Misc

Generic params (paths, names, etc.), usually there is no need in changing these, but feel free to override them if the default ones are not suitable for some reason.

| Param | Description |
| -------- | -------- |
| mongodb_exporter_version | we build the binary by ourselves in this [internal repo](https://gitea.osshelp.ru/infra/build-query-exporter) |
| mongodb_exporter_download_url | url to download the binary |
| mongodb_exporter_config_file | name of the configuration file, that will be generated and used by the exporter |
| mongodb_exporter_group_name | name of the group, that will be created for the exporter |
| mongodb_exporter_user_name | name of the user, that will be created for the exporter |
| mongodb_exporter_binary_name | name of the exporter binary  |
| mongodb_exporter_service_name | name of the systemd service, that will be created |
| mongodb_exporter_systemd_unit | absolute path to systemd unit file (generates from mongodb_exporter_service_name) |
| mongodb_exporter_dirs.binary_dir | path to binary dir |
| mongodb_exporter_dirs.config_dir | path to config dir |

## FAQ

### How do I manage the "metrics" section in config

This is the tricky part, due to the quantity and complexity of possible variations of metrics that you need to describe. The "metrics" section could not be passed to the role via playbook. Instead of this - place the metrics section to the separate YAML and put it to appropriate path in your build repository (e.g.: files/exporters/mongo/metrics.yml). After that, you need to tell the role the relative path to this YAML, and it will be automatically merged with the main config.

## Useful links

- [Exporter documentation](https://github.com/raffis/mongodb-query-exporter/blob/master/README.md)
- [MongoDB documentation](https://docs.mongodb.com/)

## TODO

- improve tests: tcp port checking is not possible yet, because exporter does not start listening on port until it successfully connects to Mongo

## License

GPL3

## Author

OSSHelp Team, see <https://oss.help>
