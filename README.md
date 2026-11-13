# CEUB — Laboratório de Entrega Contínua com .NET 10

## 1. Repositório

**Repositório:** https://github.com/danielzinrx/ceub-cd-dotnet10

Projeto acadêmico desenvolvido para praticar integração contínua (CI), entrega contínua (CD), Docker, GitHub Actions e implantação com validação e rollback.

## 2. Como reproduzir

### Requisitos

- Ubuntu 24.04 x64
- .NET SDK 10
- Git
- Docker
- Runner self-hosted do GitHub Actions com label `ceub-cd`

### Executar localmente

Clone o repositório:

```bash
git clone https://github.com/danielzinrx/ceub-cd-dotnet10.git
cd ceub-cd-dotnet10
```

Restaure as dependências e execute os testes:

```bash
dotnet restore Ceub.Delivery.slnx
dotnet test Ceub.Delivery.slnx -c Release
```

### Executar a aplicação com Docker

```bash
docker build \
  --build-arg APP_VERSION=ensaio-local \
  --build-arg APP_COMMIT=sem-commit \
  -t ceub-api:local .
```

```bash
docker run -d \
  --name ceub-api-local \
  -p 127.0.0.1:5081:8080 \
  ceub-api:local
```

Teste a aplicação:

```bash
curl http://127.0.0.1:5081/health
```

## 3. Estrutura e arquivos do projeto

O repositório contém:

- Código-fonte da API .NET 10.
- Projeto de testes automatizados.
- `Dockerfile`: construção da imagem da aplicação.
- `scripts/deploy.sh`: implantação, validação de saúde e rollback.
- `.github/workflows/ci-staging.yml`: integração contínua e implantação em staging.
- `.github/workflows/promote-production.yml`: promoção manual para produção.

## 4. Experimento 1 — CI e Staging saudáveis

**Resultado:** sucesso.

- Execução: [GitHub Actions — Run #43442686574](https://github.com/danielzinrx/ceub-cd-dotnet10/actions/runs/43442686574)
- Run ID: `43442686574`
- Branch: `main`
- Commit: `fb8bddb3f5ca6e9d52567cb12d9a03e5d8cd6592`
- Artefato: `release-fb8bddb3f5ca6e9d52567cb12d9a03e5d8cd6592`

A execução realizou build, testes, publicação da aplicação, criação da imagem Docker e implantação em staging.

**Validação:** staging respondeu com HTTP 200 no endpoint `/health`.

## 5. Experimento 2 — Promoção para Produção

**Resultado:** preencher após executar a promoção.

- Link da execução:
- Run ID:
- Commit promovido:
- Identificação do artefato:

### Comparação das imagens

| Ambiente | Image ID |
|---|---|
| Staging | PREENCHER |
| Produção | PREENCHER |

A promoção utiliza o artefato produzido pelo CI aprovado, evitando reconstruir uma imagem diferente para produção.

## 6. Experimento 3 — Teste unitário bloqueando o deploy

**Resultado:** preencher após executar o experimento.

- Link da execução com falha:
- Teste que falhou:
- Evidência de que o deploy foi bloqueado:
- Link da execução após a correção:

O objetivo é demonstrar que uma falha nos testes automatizados impede a implantação de uma versão não aprovada.

## 7. Experimento 4 — Falha controlada e rollback em Produção

**Resultado:** preencher após executar o experimento.

- Link da execução com falha controlada:
- Log do rollback:
- Resposta HTTP após a recuperação:

O script de implantação valida a saúde da nova versão. Caso a validação falhe, remove a versão candidata e tenta restaurar o container anterior.

A execução permanece marcada como falha mesmo quando o rollback recupera o serviço, permitindo identificar que a implantação não foi bem-sucedida.

## 8. Experimento 5 — Feature Flag (Extensão C)

**Aplicável somente se a extensão C tiver sido implementada.**

- Resposta HTTP com a funcionalidade desabilitada: `404`.
- Resposta HTTP com a funcionalidade habilitada: `200`.
- Image ID utilizado nos dois testes:

O objetivo é demonstrar que a funcionalidade pode ser habilitada ou desabilitada por configuração, sem reconstruir a imagem Docker.

## 9. Conceitos aplicados

### Gate manual

A promoção para produção depende de uma ação manual. Esse controle impede que toda versão aprovada em staging seja automaticamente enviada para produção.

O responsável informa o Run ID de uma execução aprovada, garantindo que a promoção utilize um artefato previamente validado.

### Estratégia de deploy

A atividade utiliza containers Docker e uma estratégia de substituição (*recreate*).

O processo:

1. Verifica se a imagem candidata existe.
2. Preserva o container anterior.
3. Interrompe a versão atual.
4. Inicia a nova versão.
5. Executa verificações de saúde.
6. Se houver falha, tenta restaurar a versão anterior.

### Limitações do ambiente

O laboratório utiliza uma máquina local com runner self-hosted e containers Docker.

Portanto, não representa uma infraestrutura de produção altamente disponível. A estratégia *recreate* pode causar interrupção temporária durante a substituição dos containers.

Também não há isolamento de infraestrutura, redundância ou alta disponibilidade entre os ambientes.

## 10. Evidências e entrega

As evidências de cada experimento devem ser registradas com:

- Link da execução no GitHub Actions.
- Run ID.
- Commit correspondente.
- Identificação do artefato ou imagem.
- Logs relevantes.
- Capturas de tela dos resultados.

---

**Projeto acadêmico — CEUB**  
Laboratório de Entrega Contínua com .NET 10
