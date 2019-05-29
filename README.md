# BOSH Release for newrelic infrastructure monitoring agent

## Usage

To use this bosh release, first upload it to your bosh:

```
bosh target BOSH_HOST
cd newrelic-infra-boshrelease
bosh upload release releases/newrelic/newrelic-infra-<version>.yml
```

Then add the newrelic license (you can find this at https://rpm.newrelic.com/accounts/YOUR_ACCOUNT_NUMBER) to the properties section of your manifest file and the newrelic release to the releases section:

```
properties:
  ...
  newrelic:
    license_key: foobar
    deployment_tag: my_deployment
releases:
- ...
- name: newrelic-infra
  version: latest
```

Finally add the `newrelic-infra` job to your manifest:

```
- instances: 1
  name: runner_z1
  ...
  jobs:
  - ...
  - name: newrelic-infra
    release: newrelic-infra
```


## This release is packaged with custom integrations for Redis, RabbitMQ, or Vault bosh releases


## [RabbitMQ](https://github.com/jordanbcooper/newrelic-integration-rabbitmq) - Haproxy Node
```
instance_groups:
- azs:
  ...
  instances: 1
  jobs:
  - name: rabbitmq-haproxy
    release: cf-rabbitmq
  - name: newrelic-infra-rabbitmq
    release: newrelic-infra
  - name: toolbelt
    release: toolbelt
  - name: toolbelt-everything
    release: toolbelt
  name: rabbitmq_haproxy
- ...
properties:
  azs:
  - LTE
  newrelic:
    deployment_tag: ...
    hostname: ...-rabbitmq
    license_key: ...
```

## [Redis](https://docs.newrelic.com/docs/integrations/host-integrations/host-integrations-list/redis-monitoring-integration)
```
- azs:
  ...
  instances: 3
  jobs:
  - name: ...
    release: ...
  - name: newrelic-infra-redis
    release: newrelic-infra
  ...
```

## [Vault](https://github.com/jordanbcooper/newrelic-integration-vaultstatus)
```
- azs:
  ...
  instances: 3
  jobs:
  - name: ...
    release: ...
  - name: newrelic-infra-vault
    release: newrelic-infra
  ...
properties:
  newrelic:
    deployment_tag: env-vault
    license_key: ...
    vault_url: ((redirect_address))

```


