# Infraestrutura Docker – Stack de Desenvolvimento

Este repositório contém uma **stack de infraestrutura Docker para desenvolvimento**, inspirada em práticas amplamente adotadas no mercado.

O objetivo é fornecer um ambiente **padronizado, reutilizável e próximo de produção**, facilitando o onboarding, reduzindo inconsistências entre projetos e evitando configurações manuais no host.

---

## Objetivos do Stack

- Centralizar serviços de infraestrutura comuns
- Evitar exposição excessiva de portas
- Usar domínios locais ao invés de portas
- Separar responsabilidades por perfis (profiles)
- Permitir subir apenas o que cada projeto precisa

---

## Serviços Disponíveis

### Traefik
Reverse proxy responsável por:
- Roteamento por domínio
- Exposição HTTP (porta 80)
- Dashboard de observabilidade

### MailHog
- Captura e visualização de e-mails enviados pela aplicação
- Simula um servidor SMTP local

### MinIO
- Storage compatível com S3
- Ideal para desenvolvimento local de uploads e arquivos

### Redis
- Cache
- Sessões
- Filas simples

### MongoDB
- Banco NoSQL orientado a documentos
- Útil para eventos, logs e dados flexíveis

### PostgreSQL
- Banco relacional principal
- Indicado para dados transacionais

### Keycloak
- Identity and Access Management (IAM)
- OAuth2 / OpenID Connect
- SSO para múltiplos sistemas

### RabbitMQ
- Mensageria baseada em filas
- Processamento assíncrono e background jobs

---

## Estrutura do Projeto

```
root/
├── docker-compose.yml
├── .env
└── traefik/
    └── traefik.yml
```

---

## Pré-requisitos

- Docker
- Docker Compose (v2+)

Nenhuma configuração manual de DNS ou `/etc/hosts` é necessária.
Todos os domínios `.localhost` resolvem automaticamente para `127.0.0.1`.

---

## Como subir a stack

### Stack básica
Inclui serviços essenciais:
- Traefik
- MailHog
- MinIO
- Redis

```bash
docker compose --profile core up -d
```

---

### Stack completa
Inclui todos os serviços disponíveis:
- Core
- Bancos de dados
- IAM
- Mensageria

```bash
docker compose \
  --profile core \
  --profile databases \
  --profile iam \
  --profile messaging \
  up -d
```

---

## Endpoints disponíveis

| Serviço   | URL |
|---------|-----|
| Traefik Dashboard | http://localhost:8080 |
| MailHog | http://mailhog.localhost |
| MinIO | http://minio.localhost |
| Keycloak | http://keycloak.localhost |
| RabbitMQ | http://rabbitmq.localhost |

---

## Variáveis de Ambiente

Todas as credenciais e configurações sensíveis estão centralizadas no arquivo:

```
.env.example
```

---

## Boas práticas adotadas

- Containers não expõem portas desnecessárias
- Comunicação interna via rede Docker
- Roteamento por domínio (igual produção)
- Serviços desacoplados por perfil
- Volumes persistentes para dados

---

## Observações importantes

- Este stack é **exclusivo para desenvolvimento**
- Dashboard do Traefik é exposto sem autenticação
- Credenciais são simplificadas para uso local

---

## Filosofia do projeto

> Subir apenas o que você precisa.

Este stack foi pensado para ser:
- previsível
- reutilizável
- fácil de manter

Cada projeto deve consumir apenas os serviços realmente necessários.

---

## Evoluções futuras possíveis

- HTTPS local automático
- Certificados TLS
- Domínios customizados (`*.dev.local`)
- Integração com CI/CD
- Profiles adicionais por tipo de projeto

---

## Conclusão

Este setup representa um **ambiente de desenvolvimento profissional**, alinhado com práticas de empresas que valorizam organização, previsibilidade e escalabilidade.

Ele permite trabalhar localmente com conforto, sem abrir mão de disciplina técnica.