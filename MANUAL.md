# Manual de Instalação e Uso

Este manual fornece um guia passo a passo para configurar e executar o ambiente SIEM para logs da Vercel usando o Elastic Stack e Docker.

## Pré-requisitos

- **Docker e Docker Compose**: Essencial para executar os contêineres. [Instale aqui](https://docs.docker.com/get-docker/).
- **Conta na Vercel**: Com pelo menos um projeto em produção para gerar logs.
- **Ferramenta de Túnel**:
  - **Cloudflare Tunnel (`cloudflared`)**: Instale aqui. (Recomendado)
  - ou **ngrok**: Instale aqui.

## Passo 1: Estrutura do Projeto

Crie a seguinte estrutura de pastas e arquivos:

```
siem-gemini/
├── docker-compose.yml
├── logstash/
│   ├── config/
│   │   └── logstash.yml
│   └── pipeline/
│       └── vercel.conf
└── README.md
└── MANUAL.md
```

Copie o conteúdo de cada arquivo de configuração deste repositório para os arquivos correspondentes em sua máquina local.

## Passo 2: Iniciar os Serviços do Elastic Stack

1.  Abra um terminal e navegue até a pasta raiz do projeto (`siem-gemini`).
2.  Execute o comando para iniciar todos os serviços em segundo plano:
    ```shell
    docker compose up -d
    ```
3.  Aguarde alguns minutos para que os contêineres iniciem. Valide se os três serviços (`elasticsearch`, `kibana`, `logstash`) estão em execução:
    ```shell
    docker ps
    ```

## Passo 3: Expor o Logstash para a Internet

O Logstash está escutando na porta `8080` localmente. Precisamos de uma URL pública para que a Vercel possa enviar os logs.

1.  Abra um **novo terminal**.
2.  Execute o comando usando sua ferramenta de túnel preferida:

    **Com Cloudflare Tunnel (recomendado):**
    ```shell
    cloudflared tunnel --url http://localhost:8080
    ```

    **Com ngrok:**
    ```shell
    ngrok http 8080
    ```
3.  Copie a URL `https` fornecida pela ferramenta (ex: `https://random-words.trycloudflare.com` ou `https://random-string.ngrok-free.app`).

## Passo 4: Configurar o Vercel Log Drains

1.  Acesse seu projeto na Vercel.
2.  Navegue até `Settings` > `Log Drains`.
3.  Cole a URL pública obtida no passo anterior e salve.

A Vercel começará a enviar os logs do seu projeto para o seu Logstash imediatamente.

## Passo 5: Verificação em Tempo Real (Crucial)

Antes de ir para o Kibana, vamos confirmar que os logs estão chegando.

1.  **Gere Tráfego**: Acesse seu site na Vercel, navegue por algumas páginas para gerar atividade.
2.  **Verifique o Túnel**: Olhe para o terminal onde o `cloudflared` (ou `ngrok`) está rodando. Você deve ver novas linhas de log para cada requisição HTTP que a Vercel envia. Se você vê atividade aqui, a conexão Vercel -> Túnel está funcionando.
3.  **Verifique o Logstash**:
    - No terminal onde o Docker está, execute:
      ```shell
      docker logs -f logstash
      ```
    - Você **deve** ver blocos de texto formatados (JSON) aparecendo. Esta é a saída do `stdout` que configuramos e é a prova de que o Logstash está recebendo e processando os dados.

**Se você não vir logs no Logstash, pare e revise os passos anteriores. O problema mais comum é um erro de digitação na URL configurada na Vercel.**

## Passo 6: Visualizar no Kibana

1.  **Acesse o Kibana**: Abra seu navegador e vá para `http://localhost:5601`.
2.  **Crie uma Data View**:
    - No menu lateral (ícone de hambúrguer), vá para `Stack Management` > `Data Views`.
    - Clique em `Create data view`.
    - No campo `Name`, digite um nome (ex: "Logs da Vercel").
    - No campo `Index pattern`, digite `gestao_atipicos_logs-*`. O Kibana deve encontrar os índices correspondentes.
    - Selecione `@timestamp` como o campo de tempo.
    - Clique em `Create data view`.
3.  **Explore os Logs**:
    - No menu principal, vá para `Analytics` > `Discover`.
    - **Importante**: No canto superior direito, ajuste o filtro de tempo para "Last 15 minutes" para garantir que você veja os dados mais novos.
    - Você verá os logs da sua aplicação Vercel chegando em tempo real.

A partir daqui, você pode criar visualizações e dashboards para monitorar seus dados.

## Passo 7: Parar o Ambiente

Para parar e remover todos os contêineres, redes e volumes, navegue até a pasta do projeto e execute:

```shell
docker compose down
```