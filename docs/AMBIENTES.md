# Ambientes

Este repositório usa `base/` e `overlays/{desenvolvimento,aceite,producao}`.

- `desenvolvimento`: perfil CRC com 1 réplica, 10Gi e retenção de 15 dias.
- `aceite`: mantém 1 réplica, aumenta armazenamento para 25Gi e retenção para 30 dias.
- `producao`: mantém o desenho declarativo, aumenta armazenamento e recursos, mas deve ser validado com storage, HA e retenção reais antes do sync.

## Seletores de coleta

O `MonitoringStack` coleta apenas recursos que atendem aos dois critérios:

- namespace com label `observability=enabled`;
- `ServiceMonitor`, `PodMonitor`, `PrometheusRule`, `Probe` ou `ScrapeConfig` com label `observability=platform`.

Essa regra evita duplicidade acidental com o `UserWorkloadMonitoring`. Se uma
aplicação precisar migrar para o Prometheus dedicado, rotule o namespace e o
recurso de scrape explicitamente.

## Validação

```bash
oc kustomize overlays/desenvolvimento >/tmp/prometheus-apps-dev.yaml
oc kustomize overlays/aceite >/tmp/prometheus-apps-aceite.yaml
oc kustomize overlays/producao >/tmp/prometheus-apps-prod.yaml
oc apply --dry-run=client -k overlays/desenvolvimento
```

Depois do sync:

```bash
oc -n observability-apps get monitoringstack,pods,svc,pvc,prometheusrule
oc -n observability-apps port-forward svc/apps-monitoring-prometheus 9090:9090
curl -fsS 'http://127.0.0.1:9090/api/v1/query?query=up'
```

## Operação

- Não remova o monitoramento nativo do OpenShift sem plano de rollback.
- Use este Prometheus para workloads que precisam de exemplares e customizações.
- Use `prometheus-ocp`/Thanos para métricas da plataforma OpenShift.
- Revise retenção, PVC, Alertmanager e HA fora do CRC.
