# Observability Stack

Stack de observabilidade utilizando Prometheus + Grafana rodando em Kubernetes.

A stack é responsável por:

- coleta de métricas
- monitoramento de APIs
- visualização de dashboards
- métricas de containers e microsserviços

---

# Tecnologias

- Prometheus
- Grafana
- Kubernetes
- Docker
- ASP.NET Metrics
- Prometheus-net

---

# Estrutura

```text
.
├── grafana
│   ├── deployment.yaml
│   ├── service.yaml
│   └── pvc.yaml
│
├── prometheus
│   ├── configmap.yaml
│   ├── deployment.yaml
│   └── service.yaml
│
└── README.md
```

---

# Arquitetura

```text
                ┌───────────────┐
                │   Grafana     │
                │ Dashboards UI │
                └───────┬───────┘
                        │
                        │ Query
                        ↓
                ┌───────────────┐
                │  Prometheus   │
                │ Metrics Store │
                └───────┬───────┘
                        │
        ┌───────────────────────────────┐
        ↓                               ↓
   Users API                      Catalog API
```

---

# Prometheus

O Prometheus é responsável por coletar métricas dos microsserviços.

---

# Deployando Prometheus

```bash
kubectl apply -f prometheus/
```

---

# Acessando Prometheus

```bash
kubectl port-forward service/prometheus 9090:9090
```

URL:

```text
http://localhost:9090
```

---

# Grafana

O Grafana é responsável pela visualização dos dashboards.

Os dados são persistidos utilizando Persistent Volume Claim (PVC).

---

# Deployando Grafana

```bash
kubectl apply -f grafana/
```

---

# Acessando Grafana

```bash
kubectl port-forward service/grafana 3000:3000
```

URL:

```text
http://localhost:3000
```

---

# Login padrão do Grafana

```text
Usuário: admin
Senha: admin
```

---

# Configurando datasource

No Grafana:

```text
Connections → Add data source → Prometheus
```

URL do datasource:

```text
http://prometheus:9090
```

---

# Métricas expostas pelas APIs

As APIs utilizam prometheus-net para exposição de métricas.

Endpoint padrão:

```text
/metrics
```

Exemplo:

```text
http://localhost:8080/metrics
```

---

# Métricas coletadas

- requests HTTP
- latência
- status codes
- throughput
- métricas de containers
- métricas de microsserviços

---

# Fluxo de métricas

```text
 Microsserviços
      ↓
   /metrics
      ↓
  Prometheus
      ↓
   Grafana
      ↓
  Dashboards
```

---

# Persistência

O Grafana utiliza Persistent Volume para persistir:

- dashboards
- usuários
- configurações
- datasources

Sem PVC os dados seriam perdidos ao reiniciar o pod.

---

# Stack monitorada

- Users API
- Catalog API
- Kubernetes
- Containers Docker