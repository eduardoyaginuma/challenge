# 💰 Solution Architecture: On-Premises Customer System - Cashback

Este repositório contém a proposta arquitetural e automação de operações (Day 2) para uma plataforma de cadastro de clientes resiliente, seguindo as melhores práticas de alta disponibilidade (HA) e segurança em camadas.

## 🏗️ Estrutura do Repositório

O projeto está organizado da seguinte forma:

* **/ansible**: Playbook para automação de tarefas críticas (Backup, Restore, Healthcheck).
* **/diagrams/arquitetura.drawio**: Diagramas de arquitetura e fluxo de dados.
* **/docs**: Documentação detalhada da arquitetura proposta.

## 📊 Arquitetura Proposta

A solução foi desenhada para operar em ambiente **On-Premises**, garantindo redundância em todos os níveis:

1.  **Entrypoint (DMZ):** Nginx configurado em High Availability (Keepalived + VIP).
2.  **Caching:** Redis Cluster para otimização de leituras.
3.  **Messaging:** Kafka Cluster para processamento assíncrono de eventos.
4.  **Database:** PostgreSQL com replicação por streaming e failover automático.



## 🛠️ Stack Tecnológica

* **Nginx:** API Gateway e Load Balancer.
* **Redis:** Cache de alta performance.
* **Apache Kafka:** Mensageria distribuída.
* **PostgreSQL:** Banco de dados relacional.
* **Ansible:** Automação de infraestrutura (IaC).

## ⚙️ Operações de Dia 2 (Ansible)

O playbook incluído em `ansible/postgres_day2.yml` automatiza as seguintes rotinas para o PostgreSQL:

* **Backup:** Geração de dumps diários automatizados.
* **Healthcheck:** Verificação de status do serviço.
* **User Management:** Criação de usuários e bases de dados com privilégios restritos.
