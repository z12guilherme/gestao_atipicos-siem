# SIEM com Elastic Stack para Logs da Vercel

Este projeto implementa um sistema SIEM (Security Information and Event Management) simplificado, utilizando o Elastic Stack (Elasticsearch, Logstash e Kibana) para coletar, processar, armazenar e visualizar logs de aplicações hospedadas na Vercel.

O objetivo é criar um dashboard centralizado para monitorar eventos, identificar padrões e analisar a segurança e o desempenho de projetos na Vercel.

## Arquitetura

O fluxo de dados segue a seguinte arquitetura:

1.  **Vercel**: Gera logs de acesso e de funções para uma aplicação.
2.  **Log Drains**: A Vercel envia esses logs em tempo real via HTTP POST para uma URL pública.
3.  **Túnel (ngrok/Cloudflare)**: Uma ferramenta de túnel expõe o serviço Logstash local para a internet, fornecendo a URL pública necessária.
4.  **Logstash**: Recebe os logs brutos, os processa (parsing, enriquecimento com GeoIP, correção de timestamp) e os envia para o Elasticsearch.
5.  **Elasticsearch**: Armazena e indexa os logs de forma eficiente para busca e análise.
6.  **Kibana**: Conecta-se ao Elasticsearch para permitir a exploração dos dados e a criação de dashboards e visualizações.

Todo o ambiente do Elastic Stack é orquestrado com Docker Compose, facilitando a configuração e a execução.

## Tecnologias Utilizadas

- **Docker & Docker Compose**: Para containerização e orquestração dos serviços.
- **Elasticsearch 8.11.3**: Para armazenamento e indexação dos logs.
- **Logstash 8.11.3**: Para ingestão e processamento dos logs.
- **Kibana 8.11.3**: Para visualização e criação de dashboards.
- **Vercel Log Drains**: Para exportação de logs em tempo real.
- **Cloudflare Tunnel / ngrok**: Para expor o endpoint local do Logstash à internet.

## Funcionalidades do Pipeline

O pipeline do Logstash (`vercel.conf`) está configurado para:
- Receber logs no formato `json_lines` via HTTP.
- Extrair o endereço de IP do cliente (`[proxy][clientIp]`).
- Enriquecer o log com dados de geolocalização (país, cidade, coordenadas) usando o filtro `geoip`.
- Converter o timestamp da Vercel (em milissegundos) para o formato de data padrão do Elastic.
- Enviar os dados processados para um índice diário no Elasticsearch (`vercel-gemini-%{+YYYY.MM.dd}`).

## Como Começar

Para um guia detalhado de instalação e configuração, consulte o arquivo **MANUAL.md**.