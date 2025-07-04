<!--
(NOTE: Do not edit README.md directly. It is a generated file!)
(      To make changes, please modify values.yaml or description.txt and run `make examples`)
-->
# Include By Namespace

This example demonstrates how to include telemetry data from a list of namespaces, dropping data from all other
namespaces.

## Values

<!-- textlint-disable terminology -->
```yaml
---
cluster:
  name: include-by-namespace-cluster

destinations:
  - name: prometheus
    type: prometheus
    url: http://prometheus.prometheus.svc:9090/api/v1/write
  - name: loki
    type: loki
    url: http://loki.loki.svc:3100/api/push
  - name: tempo
    type: otlp
    protocol: http
    url: http://tempo.tempo.svc:443/otlp
    metrics: {enabled: false}
    logs: {enabled: false}
    traces: {enabled: true}
  - name: pyroscope
    type: pyroscope
    url: http://pyroscope.pyroscope.svc:4040

annotationAutodiscovery:
  enabled: true
  namespaces: [alpha, bravo, delta]

applicationObservability:
  enabled: true
  receivers:
    otlp:
      grpc:
        enabled: true
  metrics:
    filters:
      metric:
        - not(IsMatch(resource.attributes["k8s.namespace.name"], "^alpha|bravo|delta$"))
      logs:
        - not(IsMatch(resource.attributes["k8s.namespace.name"], "^alpha|bravo|delta$"))
      traces:
        - not(IsMatch(resource.attributes["k8s.namespace.name"], "^alpha|bravo|delta$"))

autoInstrumentation:
  enabled: true
  beyla:
    config:
      data:
        discovery:
          services:
            - k8s_namespace: alpha
            - k8s_namespace: bravo
            - k8s_namespace: delta

clusterEvents:
  enabled: true
  namespaces: [alpha, bravo, delta]

clusterMetrics:
  enabled: true
  cadvisor:
    metricsTuning:
      includeNamespaces: [alpha, bravo, delta]
  kube-state-metrics:
    namespaces: [alpha, bravo, delta]

podLogs:
  enabled: true
  namespaces: [alpha, bravo, delta]

profiling:
  enabled: true
  ebpf:
    namespaces: [alpha, bravo, delta]
  java:
    namespaces: [alpha, bravo, delta]
  pprof:
    namespaces: [alpha, bravo, delta]

prometheusOperatorObjects:
  enabled: true
  probes:
    namespaces: [alpha, bravo, delta]
  podMonitors:
    namespaces: [alpha, bravo, delta]
  serviceMonitors:
    namespaces: [alpha, bravo, delta]

alloy-metrics:
  enabled: true

alloy-singleton:
  enabled: true

alloy-logs:
  enabled: true

alloy-receiver:
  enabled: true
  alloy:
    extraPorts:
      - name: otlp-grpc
        port: 4317
        targetPort: 4317
        protocol: TCP

alloy-profiles:
  enabled: true
```
<!-- textlint-enable terminology -->
