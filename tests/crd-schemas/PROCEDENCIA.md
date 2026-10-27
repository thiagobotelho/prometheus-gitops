# Schemas de recursos customizados

Schemas OpenAPI v3 extraídos das definições de CRD dos operadores correspondentes. Não incluem status de instâncias, logs, credenciais ou resultados de execução. Servem para validação estrutural offline da versão de API declarada; extensões CEL e regras de admissão precisam de dry-run no servidor.

Ao atualizar um operador, obtenha sua CRD da distribuição oficial, confira procedência e revise o diff do schema. Não remova validação para aceitar campos desconhecidos. O schema não representa matriz de compatibilidade entre produtos nem comprova funcionamento em cluster.

Tipos cobertos: monitoring.rhobs__v1__Prometheus, monitoring.rhobs__v1__PrometheusRule, monitoring.rhobs__v1alpha1__MonitoringStack, operators.coreos.com__v1__OperatorGroup, operators.coreos.com__v1alpha1__Subscription.
