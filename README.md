
![Elastic Stack](https://img.shields.io/badge/Elastic%20Stack-8.11.1-005571?logo=elastic-stack) ![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker) ![License](https://img.shields.io/badge/License-MIT-blue) ![Status](https://img.shields.io/badge/Status-Production-green)

# 🔐 SIEM de Logs da Vercel com Elastic Stack (ELK)

**Monitoramento, segurança e análise de logs em tempo real** para projetos hospedados na Vercel.
Este projeto combina **Elastic Stack (Elasticsearch, Logstash, Kibana)** com **Docker Compose** para criar um ambiente **robusto, seguro e escalável**.

---

## ✨ Funcionalidades Principais

| Funcionalidade                    | Descrição                                                                            |
| --------------------------------- | ------------------------------------------------------------------------------------ |
| 🕒 **Ingestão em Tempo Real**     | Coleta logs da Vercel via Log Drains, garantindo processamento contínuo e seguro.    |
| 🌍 **Geolocalização**             | Identifica cidade e país do IP de origem.                                            |
| 🛡️ **Threat Intelligence**       | Compara IPs com uma `blocklist.csv` personalizável para detectar ameaças conhecidas. |
| 🔒 **Segurança Integrada**        | Elasticsearch e Kibana protegidos por senha desde o início.                          |
| 🗝️ **Gerenciamento de Segredos** | Credenciais armazenadas de forma segura em `.env`.                                   |
| 📊 **Visualização Centralizada**  | Dashboards interativos e alertas configuráveis via Kibana.                           |
| 🚀 **Setup Simplificado**         | Orquestração completa via Docker Compose para iniciar rapidamente.                   |

---

## 🚀 Quick Start

### 1️⃣ Preparar o Ambiente

```bash
# Clone o repositório
git clone https://github.com/seuusuario/siem-vercel-elk.git
cd siem-vercel-elk

# Certifique-se de ter Docker Desktop instalado e rodando
docker --version
```

### 2️⃣ Configurar Segredos

Crie um arquivo `.env` na raiz do projeto e adicione:

```dotenv
# Senha do usuário 'elastic' e comunicação interna do Stack
ELASTIC_PASSWORD=sua_senha_super_forte_aqui_123!

# Credenciais para Logstash receber logs da Vercel
VERCEL_USER=vercel
VERCEL_PASSWORD=outra_senha_forte_para_o_log_drain_456@
```

### 3️⃣ Iniciar o SIEM

```bash
docker compose up -d
```

### 4️⃣ Configurar Conexão com a Vercel

* Consulte **MANUAL.md** para expor o Logstash à internet e configurar o Log Drain.

---

## ⚙️ Configuração Avançada

* **Lista de Ameaças:** Atualize `logstash/pipeline/blocklist.csv` para adicionar IPs maliciosos.
* **Dashboards Personalizados:** Crie dashboards no Kibana para visualizações estratégicas.
* **Alertas Inteligentes:** Configure alertas de segurança e performance em tempo real.

---

## 🔧 Tecnologias Utilizadas

| Tecnologia     | Propósito                                      |
| -------------- | ---------------------------------------------- |
| Elasticsearch  | Armazenamento e busca de logs em tempo real    |
| Logstash       | Pipeline de ingestão e enriquecimento de dados |
| Kibana         | Visualização de logs e criação de dashboards   |
| Docker Compose | Orquestração de containers e setup rápido      |
| CSV Blocklist  | Threat Intelligence personalizada              |

---

## 🔒 Boas Práticas de Segurança

* Nunca exponha senhas no código.
* Use credenciais **fortes e únicas**.
* Atualize a **lista de ameaças** regularmente.
* Monitore os dashboards para **análise proativa de segurança**.

---

## 📂 Estrutura do Projeto

```
siem-vercel-elk/
│
├─ docker-compose.yml
├─ .env
├─ logstash/
│   ├─ pipeline/
│   │   ├─ blocklist.csv
│   │   └─ logstash.conf
├─ MANUAL.md
└─ README.md
```

---

Se você quiser, posso criar **uma versão ainda mais “corporativa” com imagens de dashboards, diagramas do fluxo de logs e cores, como se fosse um README de portfólio da Microsoft**, que impressiona visualmente.

Quer que eu faça isso também?
