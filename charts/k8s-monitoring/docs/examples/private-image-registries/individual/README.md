<!--
(NOTE: Do not edit README.md directly. It is a generated file!)
(      To make changes, please modify values.yaml or description.txt and run `make examples`)
-->
# Private Image Registries

This example shows how to override the individual container image registries for every subchart. This can be used to
support air-gapped environments, or in environments where you might not want to use public image registries.

If you want to change the image registry for every subchart, you can use the `global` object to set the registry
[globally](../globally).

## Values

<!-- textlint-disable terminology -->
```yaml
---
cluster:
  name: private-image-registries-example-cluster

destinations:
  - name: prometheus
    type: prometheus
    url: http://prometheus.prometheus.svc:9090/api/v1/write

  - name: loki
    type: loki
    url: http://loki.loki.svc:3100/loki/api/v1/push

  - name: tempo
    type: otlp
    url: http://tempo.tempo.svc:3200/api/v1/metrics
    metrics: {enabled: false}
    logs: {enabled: false}
    traces: {enabled: true}
    processors:
      tailSampling:
        enabled: true
        policies:
          - name: always_sample-policy
            type: always_sample
        collector:
          image:
            registry: my.registry.com
            repository: grafana/alloy
            pullSecrets:
              - name: my-registry-creds
          configReloader:
            image:
              registry: my.registry.com
              repository: prometheus-operator/prometheus-config-reloader


clusterMetrics:
  enabled: true

  kube-state-metrics:
    image:
      registry: my.registry.com
      repository: kube-state-metrics/kube-state-metrics
    imagePullSecrets:
      - name: my-registry-creds

  node-exporter:
    image:
      registry: my.registry.com
      repository: prometheus/node-exporter
    imagePullSecrets:
      - name: my-registry-creds

  windows-exporter:
    image:
      registry: my.registry.com
      repository: prometheus-community/windows-exporter
    imagePullSecrets:
      - name: my-registry-creds

  opencost:
    imagePullSecrets:
      - name: my-registry-creds
    opencost:
      exporter:
        image:
          registry: my.registry.com
          repository: opencost/opencost

  kepler:
    image:
      repository: quay.io/sustainable_computing_io/kepler
    imagePullSecrets:
      - name: my-registry-creds


autoInstrumentation:
  enabled: true
  beyla:
    image:
      registry: my.registry.com
      repository: grafana/beyla
      pullSecrets:
        - name: my-registry-creds

applicationObservability:
  enabled: true
  receivers:
    otlp:
      http:
        enabled: true

alloy-operator:
  image:
    registry: my.registry.com
    repository: grafana/alloy-operator
    pullSecrets:
      - name: my-registry-creds

alloy-metrics:
  enabled: true
  image:
    registry: my.registry.com
    repository: grafana/alloy
    pullSecrets:
      - name: my-registry-creds
  configReloader:
    image:
      registry: my.registry.com
      repository: prometheus-operator/prometheus-config-reloader

alloy-receiver:
  enabled: true
  image:
    registry: my.registry.com
    repository: grafana/alloy
    pullSecrets:
      - name: my-registry-creds
  configReloader:
    image:
      registry: my.registry.com
      repository: prometheus-operator/prometheus-config-reloader
```
<!-- textlint-enable terminology -->
