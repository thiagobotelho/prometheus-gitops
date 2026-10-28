# Seleção de alvos e alertas


namespaceSelector seleciona namespaces rotulados; resourceSelector seleciona monitores. Ambos precisam corresponder aos recursos. Retenção e armazenamento definem a janela observável. O Alertmanager deste MonitoringStack está desabilitado: definir PrometheusRule não entrega notificações por si só. Configure uma rota de alertas explicitamente antes de depender dela.

## Regras dos componentes

Mantenha regras e monitores específicos junto ao componente que os produz. As regras do mesh são entregues pelo perfil opcional de service-mesh-gitops e selecionadas pelas labels da MonitoringStack. Verifique alvos, regras carregadas e estado pending/firing no Prometheus Apps antes de investigar notificações.
