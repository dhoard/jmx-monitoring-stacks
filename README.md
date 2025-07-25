# Overview

This repo demonstrates examples of JMX monitoring stacks that can monitor Confluent Cloud and Confluent Platform.
While Confluent Cloud UI and Confluent Control Center provides an opinionated view of Apache Kafka monitoring, JMX monitoring stacks serve a larger purpose to our users, allowing them to setup monitoring across multiple parts of their organization, many outside of Kafka, and to have a single pane of glass.

This project provides metrics and dashboards for:

- [Confluent Platform with Prometheus and Grafana](jmxexporter-prometheus-grafana)
- [Confluent Platform on Kubernetes with Prometheus and Grafana](jmxexporter-prometheus-grafana/cfk)
- [Confluent Platform with New Relic](jmxexporter-newrelic)
- [Confluent Platform with Prometheus, Metricbeat and Kibana](metricbeat-elastic-kibana)
- [Confluent Platform with Datadog agent](datadog)
- [Confluent Cloud with Prometheus and Grafana](ccloud-prometheus-grafana)
- [Confluent Cloud with Metricbeat and Kibana](ccloud-metricbeat-elastic-kibana)
- [Confluent Cloud with Opentelemetry and New Relic](ccloud-opentelemetry-newrelic)
- [Confluent Cloud with Opentelemetry and Amazon CloudWatch](ccloud-opentelemetry-aws-cloudwatch)

## 📊 Dashboards

**Some examples:**

<p float="left">
  <img src="jmxexporter-prometheus-grafana/img/kafka-cluster-0.png" width="250" height="200" />
  <img src="jmxexporter-prometheus-grafana/img/kraft_2.png" width="250" height="200" />
  <img src="jmxexporter-prometheus-grafana/img/kafka-quotas.png" width="250" height="200" />
  <img src="jmxexporter-newrelic/img/Cluster.png" width="250" height="200" />
  <img src="metricbeat-elastic-kibana/img/kafka-overview.png" width="250" height="200" />
  <img src="datadog/img/overview.png" width="250" height="200" />
</p>

**List of available dashboards for Confluent Platform:**

| Dashboard                      | Prometheus/Grafana  | New Relic |Metricbeat/Kibana|Datadog|
|--------------------------------|---------------------|-----------|---------|-----------
| Kafka Cluster                  | yes                 | yes       |yes|yes|
| Zookeeper                      | yes                 | yes       |yes|
| KRaft                          | yes                 |
| Confluent Schema Registry      | yes                 |           |yes|
| Kafka Connect                  | yes                 |           |yes|
| ksqlDB                         | yes                 |           |yes|
| Producer/Consumer              | yes                 | yes       |yes|yes|
| Lag Exporter                   | yes                 |
| Topics                         | yes                 |           |yes|
| Kafka Streams                  | yes                 |
| Kafka Streams RocksDB          | yes                 |
| Quotas                         | yes                 |
| TX Coordinator                 | yes                 |
| Confluent Rest Proxy           | yes                 |
| Confluent Cluster Linking      | yes                 |
| Confluent Oracle CDC connector | yes                 |
| Debezium connectors            | yes                 |
| MongoDB connectors             | yes                 |
| librdkafka clients             | yes                 |
| Confluent RBAC                 | yes                 |
| Confluent Replicator           | yes                 |
| Confluent Tiered Storage       | yes                 |
| Confluent Flink                | yes                 |

**List of available dashboards for Confluent Cloud:**

| Dashboard|Prometheus/Grafana|New Relic|Metricbeat/Kibana|AWS Cloud Watch|
|-----------------------|---------------|--------|----------------|--------|
| Cluster               |yes|yes|yes|yes
| Producer/Consumer     ||yes|
| ksql                  |yes|
| Billing/Cost tracking |yes|

## ⚠️ Alerts

Alerts are available for the stacks:

 - [jmxexporter-prometheus-grafana](jmxexporter-prometheus-grafana/assets/prometheus/prometheus-alerts) including alerts on broker, zookeeper and kafka connect.


# How to use with Confluent cp-ansible

To add JMX exporter configurations to [Confluent cp-ansible](https://docs.confluent.io/ansible/current/overview.html), please refer to this [README](jmxexporter-prometheus-grafana/cp-ansible/README.md)

# How to use with Kubernetes and Confluent for Kubernetes Operator (CFK)

To add JMX exporter configurations to your Kubernetes workspace, please refer to this [README](jmxexporter-prometheus-grafana/cfk/README.md)

# How to use with Confluent cp-demo

This repo is intended to work smoothly with [Confluent cp-demo](https://github.com/confluentinc/cp-demo).

Make sure you have enough system resources on the local host to run this.
Verify in the advanced Docker preferences settings that the memory available to Docker is at least 8 GB (default is 2 GB).

**NOTE:** [jq](https://jqlang.github.io/jq/) is required to be installed on your machine to run the demo.

1. Ensure that cp-demo is not already running on the local host.

2. Decide which monitoring stack to demo: and set the `MONITORING_STACK` variable accordingly.

**NOTE:** New Relic requires a [License Key](https://docs.newrelic.com/docs/apis/intro-apis/new-relic-api-keys/#overview-keys) to be added in [_jmxexporter-newrelic/start.sh_](jmxexporter-newrelic/start.sh)

**NOTE:** Datadog requires a [DATADOG_API_KEY](https://docs.datadoghq.com/account_management/api-app-keys/) and [DATADOG_SITE](https://docs.datadoghq.com/getting_started/site/) to be added in [_datadog/start.sh_](datadog/start.sh). Datadog offers 14 day trial for new users.

```bash
# Set only one of these
MONITORING_STACK=jmxexporter-prometheus-grafana
MONITORING_STACK=metricbeat-elastic-kibana
MONITORING_STACK=jmxexporter-newrelic
MONITORING_STACK=datadog
```

3. Clone `cp-demo` and checkout a branch.

```bash
# Example with CP-DEMO 7.8.1 version
CP_DEMO_VERSION=7.8.1-post

[[ -d "cp-demo" ]] || git clone https://github.com/confluentinc/cp-demo.git
(cd cp-demo && git fetch && git checkout $CP_DEMO_VERSION && git pull)
```

4. Clone `jmx-monitoring-stacks` and checkout main branch.

```bash
[[ -d "jmx-monitoring-stacks" ]] || git clone https://github.com/confluentinc/jmx-monitoring-stacks.git
(cd jmx-monitoring-stacks && git fetch && git checkout main && git pull)
```

5. Start the monitoring solution with the STACK selected. This command also starts cp-demo, you do not need to start cp-demo separately.

```bash
${MONITORING_STACK}/start.sh
```

6. Stop the monitoring solution. This command also stops cp-demo, you do not need to stop cp-demo separately.

```bash
${MONITORING_STACK}/stop.sh
```

# How to use with Apache Kafka client applications (producers, consumers, kafka streams applications)

For an example that showcases how to monitor Apache Kafka client applications, and steps through various failure scenarios to see how they are reflected in the provided metrics, see the [Observability for Apache Kafka® Clients to Confluent Cloud tutorial](https://docs.confluent.io/cloud/current/get-started/examples/ccloud-observability/docs/index.html).

# How to use with specific configurations: DEV-toolkit

[![Open in Gitpod](https://gitpod.io/button/open-in-gitpod.svg)](https://gitpod.io/#https://github.com/confluentinc/jmx-monitoring-stacks/)

Dev-toolkit is an environment that allows you to easily create different configurations and deployments to verify the metrics exposed by different components of the Confluent Platform.

Dev-toolkit is based on:
 - Confluent Platform _(current 8.0.0)_
 - Prometheus and Grafana stack.
 - JMX Exporter 1.1.0

To run a lightweight a **Default** environment, follow the next steps:

1. `cd dev-toolkit`
2. [Optional]: Put your new dashboards into the `grafana-wip` folder. All [existing grafana dashboards](jmxexporter-prometheus-grafana/assets/grafana/provisioning/dashboards) will be anyway loaded.
3. Execute script `start.sh`
4. For Grafana, go to http://localhost:3000
5. To teardown, execute script `stop.sh`

## Run with profiles

**Default** profile will create:
 - 1 Confluent Platform with KRaft and 4 brokers (_kafka1_,_kafka2_,_kafka3_,_kafka4_)
 - 1 kafka application implemented with Spring to fetch producer and consumer metrics
 - 1 kafka lag exporter
 - 1 grafana
 - 1 prometheus

To add more use cases, we are leveraging the docker profiles.

Example, to run replicator scenario, run `start.sh --profile replicator`.

It's possible to combine profiles as well, i.e. `start.sh --profile schema-registry --profile ksqldb`.

To run a full stack, just use: `start.sh --profile schema-registry --profile connect --profile restproxy --profile ksqldb --profile control-center`

Currently supported profiles:
- _clusterlinking_: add Cluster Linking between _kafka1-kafka2-kafka3-kafka4_ and a new cluster with 1 broker _broker-dest_
- _connect_: it will add Kafka Connect with a datagen source connector and a file sink connector. It requires _schema-registry_ profile.
- _consumer_: it will add a demo application implemented with Spring with full client metrics
- _consumer-minimal_: it will add a demo application implemented with Spring with a limited number of client metrics
- _control-center_: it will add Confluent Control Center. It requires _schema-registry_, _connect_ and _ksqldb_ profiles.
- _jr_: it will add [JR](https://jrnd.io/) to generate random traffic for kafka.
- _ksqldb_: it will add ksqldb server. It requires _schema-registry_ profile.
- _kstream_: it will add a demo stateful kafka streams application with full client metrics (_TRACE_ level selected)
- _kui_: it will add an instance of _kafka-ui_ for topics data visualization (available on port 18080).
- _mongo-connect_: it will add Kafka Connect with MongoDB replica set and a MongoDB source connector and a MongoDB sink connector. It requires _schema-registry_ profile.
- _otel_: it will retrieve metrics from kafka brokers and expose them as a prometheus endpoint. More info at [kafkametricsreceiver](https://github.com/open-telemetry/opentelemetry-collector-contrib/tree/main/receiver/kafkametricsreceiver).
- _replicator_: it will add a Kafka connect cluster with Confluent Replicator between _kafka1-kafka2-kafka3-kafka4_ and a new cluster with 1 broker _broker-dest_
- _restproxy_: it will add Confluent Rest Proxy. It requires _schema-registry_ profile.
- _schema-registry_: it will add Confluent Schema Registry.
- _schema-registry-primary-secondary_: it will add 2 Confluent Schema Registry, primary and secondary.
- _tieredstroage_: it will configure Confluent Platform to use Confluent Tiered Storage and a compatible S3 storage.

## DEV-toolkit FAQ

- What if I need more components?

More docker-compose envs will be released in the future, in the meantime you can use [Kafka Docker Composer](https://github.com/sknop/kafka-docker-composer) to create yours.

- What if I need more prometheus jobs?

You can add them to the `start.sh`, i.e.

```
# ADD client monitoring to prometheus config
cat <<EOF >> assets/prometheus/prometheus-config/prometheus.yml

  - job_name: 'spring-client'
    static_configs:
      - targets: ['spring-client:9191']
        labels:
          env: "dev"
EOF
```

You can also change the prometheus configuration [here](https://github.com/confluentinc/jmx-monitoring-stacks/blob/main/jmxexporter-prometheus-grafana/assets/prometheus/prometheus-config/prometheus.yml).
