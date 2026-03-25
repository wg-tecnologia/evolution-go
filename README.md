# Evolution Go

> ⚠️ **IMPORTANTE - LEIA ANTES DE PROSSEGUIR**
> 
> Este projeto é um **FORK** do projeto open-source [EvolutionAPI/evolution-go](https://github.com/EvolutionAPI/evolution-go).
> 
> - ✅ Podemos evoluir o código conforme necessidade
> - ✅ Precisamos manter o link com o projeto original
> - ✅ Para atualizar: `git fetch upstream && git merge upstream/main`
> 
> **Repositórios que podem ser deletados:**
> - ❌ `cantinho.lapis.na.mao` (projeto temporário de testes)
> - ❌ `evolution-go-local` (testes locais)
> - ❌ `whats-evolution-api-go` (fork desnecessário - usar este)
> 
> **Repositório oficial:** https://github.com/wg-tecnologia/evolution-go

---

API de WhatsApp de alta performance escrita em Go.

<div align="center">

[![Docker Image](https://img.shields.io/badge/Docker-image-blue)](https://hub.docker.com/r/evoapicloud/evolution-go)
[![License](https://img.shields.io/badge/license-Apache--2.0-blue)](./LICENSE)
[![Go Version](https://img.shields.io/badge/Go-1.24+-00ADD8?logo=go)](https://golang.org/)
[![Documentation](https://img.shields.io/badge/Documentation-Official-green)](https://docs.evolutionfoundation.com.br)
[![Fork](https://img.shields.io/badge/Fork%20of-EvolutionAPI-blue)](https://github.com/EvolutionAPI/evolution-go)

</div>

<div align="center"><img src="./public/images/cover.png" width="400"></div>

## Sobre

Evolution Go é uma API WhatsApp de alta performance construída em Go, parte do ecossistema [Evolution](https://evolutionfoundation.com.br/). Utiliza a biblioteca [whatsmeow](https://github.com/tulir/whatsmeow).

## Recursos

- **Alta Performance** — Construído em Go para uso mínimo de recursos
- **API RESTful** — Endpoints REST bem documentados com Swagger
- **Eventos em Tempo Real** — WebSocket, Webhook, AMQP/RabbitMQ e NATS
- **Suporte a Mídia** — Imagens, vídeos, áudios e documentos
- **Armazenamento de Mensagens** — Persistência opcional no PostgreSQL
- **QR Code** — Geração de QR Code para pareamento
- **Gestão de Licenças** — Sistema de licenciamento integrado
- **Docker** — Configuração pronta para produção

---

## Quick Start (Português)

### Pré-requisitos

- Docker 20.10+
- Docker Compose v2.x
- Git

### Instalação

```bash
# Clone o repositório
git clone https://github.com/wg-tecnologia/evolution-go.git
cd evolution-go

# Inicialize submódulos
git submodule update --init --recursive

# Build da imagem Docker
make docker-build

# Execute
make docker-run
```

**Ou com Docker Compose:**

```bash
# Clone
git clone https://github.com/wg-tecnologia/evolution-go.git
cd evolution-go

# Inicialize submódulos
git submodule update --init --recursive

# Configure
cp .env.example .env
# Edite o .env com suas configurações

# Suba os serviços
docker compose up -d
```

---

## Configuração

### Variáveis de Ambiente

| Variável | Descrição | Padrão |
|----------|-----------|--------|
| `SERVER_PORT` | Porta do servidor | `4000` |
| `CLIENT_NAME` | Nome do cliente | `evolution` |
| `GLOBAL_API_KEY` | Chave de autenticação | **Obrigatório** |
| `POSTGRES_AUTH_DB` | Banco de autenticação | - |
| `POSTGRES_USERS_DB` | Banco de usuários | - |
| `DATABASE_SAVE_MESSAGES` | Salvar mensagens | `false` |
| `CONNECT_ON_STARTUP` | Reconectar ao iniciar | `false` |

### Exemplo .env

```env
SERVER_PORT=4000
CLIENT_NAME=evolution
GLOBAL_API_KEY=SUA_CHAVE_API_AQUI
POSTGRES_AUTH_DB=postgresql://postgres:postgres@postgres:5432/evogo_auth?sslmode=disable
POSTGRES_USERS_DB=postgresql://postgres:postgres@postgres:5432/evogo_users?sslmode=disable
DATABASE_SAVE_MESSAGES=false
WADEBUG=INFO
LOGTYPE=console
CONNECT_ON_STARTUP=false
```

---

## Endpoints Principais

| Método | Endpoint | Descrição |
|--------|----------|-----------|
| `POST` | `/instance/create` | Criar instância |
| `GET` | `/instance/{name}/qrcode` | Obter QR Code |
| `POST` | `/message/sendText` | Enviar texto |
| `POST` | `/message/sendMedia` | Enviar mídia |
| `GET` | `/instance/{name}/status` | Status |
| `DELETE` | `/instance/{name}` | Deletar |

### Documentação Swagger

```
http://localhost:4000/swagger/index.html
```

### Teste Rápido

```bash
# Verificar se API está rodando
curl http://localhost:4000/

# Criar instância
curl -X POST http://localhost:4000/instance/create \
  -H "Content-Type: application/json" \
  -H "apikey: SUA_CHAVE_API" \
  -d '{"instanceName": "teste"}'

# Obter QR Code
curl http://localhost:4000/instance/teste/qrcode \
  -H "apikey: SUA_CHAVE_API"
```

---

## Gestão de Licenças

1. Acesse: `http://SEU_IP:4000/manager/login`
2. Na primeira vez, ative a licença com seu email
3. Após ativado, faça login com a API Key

---

## Comandos Úteis

```bash
# Docker Compose
docker compose up -d        # Iniciar
docker compose down         # Parar
docker compose logs -f      # Ver logs

# Makefile
make docker-build           # Build
make docker-run             # Run
make help                   # Ver ajuda

# Containers
docker ps                   # Ver containers
docker logs evolution-go -f # Logs
```

---

## Deploy no Servidor

```bash
# Clone
git clone https://github.com/wg-tecnologia/evolution-go.git
cd evolution-go

# Submódulos
git submodule update --init --recursive

# Build
make docker-build

# Iniciar
docker compose up -d

# Firewall (Ubuntu)
sudo ufw allow 4000/tcp
```

---

## Estrutura do Projeto

```
evolution-go/
├── cmd/evolution-go/     # Entry point
├── pkg/
│   ├── core/            # Licenciamento
│   ├── instance/        # Gestão de instâncias
│   ├── message/         # Mensagens
│   ├── routes/          # Rotas HTTP
│   ├── middleware/       # Auth
│   └── config/          # Configuração
├── whatsmeow-lib/       # Biblioteca WhatsApp
├── docs/                # Swagger
├── Dockerfile
└── Makefile
```

---

## Stack

| Componente | Tecnologia |
|------------|-----------|
| Linguagem | Go 1.24+ |
| HTTP | Gin |
| WhatsApp | whatsmeow |
| Banco | PostgreSQL |
| ORM | GORM |

---

## Suporte

- [Documentação](https://docs.evolutionfoundation.com.br)
- [Comunidade](https://evolutionfoundation.com.br/community)

---

## Licença

Apache License 2.0

---

## Atualizações do Projeto Original

Este é um fork de [EvolutionAPI/evolution-go](https://github.com/EvolutionAPI/evolution-go). Para manter atualizado com o projeto original:

```bash
# Adicionar upstream (se não existir)
git remote add upstream https://github.com/EvolutionAPI/evolution-go.git

# Buscar atualizações
git fetch upstream

# Verificar diferenças
git log --oneline upstream/main

# Fazer merge das atualizações
git merge upstream/main

# Resolver conflitos se necessário

# Push para o fork
git push origin main
```

### Rebase (alternativa ao merge)

```bash
git fetch upstream
git rebase upstream/main
git push --force-with-lease origin main
```
