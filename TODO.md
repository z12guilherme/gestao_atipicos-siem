# Roteiro: Do Zero ao Primeiro Dashboard SIEM

Este arquivo acompanha o progresso da configuração do nosso SIEM com Elastic Stack e Vercel.

## Fase 0: A Base (Infraestrutura)
- [x] Decidir por setup local
- [x] Instalar Docker e Docker Compose

## Fase 1: A Instalação (Elastic Stack com Docker)
- [x] Criar estrutura de pastas (`siem-gemini`, `logstash/config`, `logstash/pipeline`)
- [x] Criar arquivo `logstash/config/logstash.yml`
- [x] Criar arquivo `logstash/pipeline/vercel.conf`
- [x] Criar arquivo `docker-compose.yml`
- [x] Iniciar os serviços com `docker compose up -d`
- [x] Validar que os três contêineres (elasticsearch, kibana, logstash) estão rodando (`docker ps`)

## Fase 2: A Conexão (Vercel Log Drains)
- [x] Expor a porta local para a internet (usando um túnel) e obter a URL pública
- [x] Acessar o projeto na Vercel e ir para `Settings > Log Drains`
- [x] Adicionar a URL do Logstash como um novo Log Drain
- [x] Validar o recebimento de logs no Logstash (verificar logs do contêiner)

## Fase 3: A Primeira Conquista (Dashboard no Kibana)
- [ ] Acessar o Kibana em `http://localhost:5601`
- [ ] Criar uma "Data View" com o padrão `vercel-gemini-*`
- [ ] Acessar a aba "Discover" para confirmar que os logs estão chegando
- [ ] Criar um dashboard simples para visualização dos dados
