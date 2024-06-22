# Projeto de Infraestrutura em Docker Compose

Este projeto utiliza o Docker Compose para definir e executar múltiplos serviços de banco de dados, mensageria e cache em um único ambiente isolado. Os serviços incluem MySQL, RabbitMQ e Redis, além de um serviço dedicado para reiniciar o Redis semanalmente.

## Serviços

### MySQL

- **Imagem:** `mysql:latest`
- **Portas:** `3306`
- **Volumes:** Dados persistidos em `C:/Users/breno/Documentos/Docker/Volumes/DB/mysql`
- **Variáveis de Ambiente:**
    - `MYSQL_ROOT_PASSWORD`: Senha do usuário root
    - `MYSQL_DATABASE`: Nome do banco de dados padrão
    - `MYSQL_ROOT_HOST`: Permite conexões remotas

### RabbitMQ

- **Imagem:** `rabbitmq:3-management`
- **Portas:**
    - `25672`: Comunicação entre os nós e a ferramenta CLI
    - `15672`: Acesso à API de gerenciamento web
- **Volumes:** Dados persistidos em `/docker_conf/rabbitmq/data/`
- **Variáveis de Ambiente:**
    - `RABBITMQ_DEFAULT_USER`: Nome de usuário padrão
    - `RABBITMQ_DEFAULT_PASS`: Senha padrão
- **Opções:**
    - `restart: always`: Garante que o RabbitMQ seja reiniciado automaticamente

### Redis

- **Imagem:** `redis:6.2-alpine`
- **Portas:** `6379`
- **Comando:** Configurações personalizadas do Redis
- **Volumes:** Dados persistidos em `redis_data`
- **Opções:**
    - `restart: unless-stopped`: Reinicia o Redis automaticamente, exceto se o contêiner for explicitamente parado

### Redis Restarted

- **Imagem:** `docker:cli`
- **Volumes:** Socket do Docker para controle
- **Entrypoint e Comando:** Script para reiniciar o Redis toda segunda-feira

## Volumes

- **redis_data:** Armazena os dados do Redis.
- **logs-folder (log_rabbitmq):** Armazena os logs do RabbitMQ.

## Uso

1. Clone este repositório ou baixe o arquivo `docker-compose.yml`.
2. Certifique-se de que o Docker e o Docker Compose estão instalados no seu sistema.
3. Execute `docker-compose up -d` para iniciar os serviços em segundo plano.
4. Acesse a interface de gerenciamento do RabbitMQ em `http://localhost:15672` com o usuário `admin` e a senha `1234`.
5. Para parar os serviços, execute `docker-compose down`.

Este projeto é projetado para ser facilmente escalável e adaptável a diferentes ambientes, desde desenvolvimento pessoal até produção.
