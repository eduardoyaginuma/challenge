# Arquitetura On-Premises - Aplicação de Cashback

## 1. Visão Geral

Esta solução foi projetada para operar em ambiente on-premises, garantindo:

- Alta Disponibilidade (HA)
- Segurança em múltiplas camadas
- Escalabilidade horizontal
- Observabilidade
- Resiliência a falhas

---

## 2. Componentes da Arquitetura

### Nginx (API Gateway)

Responsável por:

- Roteamento de requisições
- Rate limiting
- TLS termination
- Autenticação via JWT
- Balanceamento de carga interno

Configuração:
- 2 instancias
- Keepalived/VRRP para IP virtual
- TLS 1.3

---

### Redis (Cache de Leitura)

Uso:
- Cache de saldo de cashback
- Sessões
- Rate limiting

Configuração HA:
- Redis Cluster
- Redis Sentinel
- 1 Master + 1 Replica por shard

Segurança:
- ACL habilitado
- TLS interno
- Proteção de bind interno

---

### Kafka (Mensageria)

Uso:
- Processamento assíncrono
- Eventos de cashback
- Integração com antifraude

Configuração:
- 3 intermediários
- Replication factor: 3
- Min ISR: 2

Segurança:
- SASL/SCRAM
- TLS
- ACL por tópico

---

### PostgreSQL (Persistência)

Uso:
- Dados transacionais
- Histórico financeiro

Configuração HA:
- Streaming Replication
- 1 Primary
- 1 Standby síncrono
- WAL Archiving

Segurança:
- SSL obrigatório
- Role-based access
- pg_hba.conf restritivo
- Backup criptografado

---

## 3. Estratégias de Alta Disponibilidade

| Camada | Estratégia |
|--------|------------|
| Gateway | Nginx em cluster + IP virtual |
| Cache | Redis Sentinel |
| Mensageria | Kafka replication factor 3 |
| Banco | Streaming Replication + WAL |
| Infra | RAID 10 + Dual Power |

---

## 4. Estratégias de Segurança

- Firewall segmentando redes
- TLS de "ponta-a-ponta"
- Princípio do menor privilégio
- 'Auditoria' habilitada
- Backup criptografado
- Segregação de ambientes

---

## 5. Fluxo de Cashback

1. Usuário realiza compra
2. Evento é publicado no Kafka
3. Serviço calcula cashback
4. PostgreSQL persiste transação
5. Redis atualiza cache
6. API retorna saldo atualizado

---

## 6. Observabilidade

- Prometheus
- Grafana
- Alertmanager
- Logs centralizados via ELK
