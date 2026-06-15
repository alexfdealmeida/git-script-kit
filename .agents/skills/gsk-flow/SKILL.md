---
name: gsk-flow
description: Auxiliar no fluxo de branches GSK Flow (feature, bugfix, release, hotfix)
---

# GSK Flow Workflow

Guia para trabalhar com o fluxo de branches do GSK Flow.

## Branches Permanentes (Baselines)

| Branch | Propósito | Base para |
|--------|-----------|-----------|
| `develop` | Ambiente de desenvolvimento | feature, bugfix, epic, release |
| `production` | Ambiente de produção (LTE) | hotfix-production |
| `main-rc` | Produção pré-comercial (LTS-RC) | hotfix-rc |
| `main` | Produção comercial (LTS) | hotfix |

> Nota: Estrutura mínima requer `develop` e `main`. `production` e `main-rc` são opcionais.

## Branches Temporários

### Feature (`feature-<taskId>`)
```bash
# Padrões aceitos:
# ^feature-[0-9]+$  ou  ^temp/develop/feature/[0-9]+$

# Criar a partir de develop
gsk_create_remote_branch_type 12345

# Finalizar: merge em develop via PR
# (branch é excluído automaticamente após merge)
```

### Bugfix (`bugfix-<taskId>`)
```bash
# Padrões aceitos:
# ^bugfix-[0-9]+$  ou  ^temp/develop/bugfix/[0-9]+$

# Criar a partir de develop
gsk_create_remote_branch_type 12345

# Finalizar: merge em develop via PR
```

### Epic (`epic-<taskId>`)
```bash
# Padrões aceitos:
# ^epic-[0-9]+$  ou  ^temp/develop/epic/[0-9]+$

# Para múltiplas tarefas (evolução + correção)
gsk_create_remote_branch_type 12345
```

### Release (`release-X-Y-Z`)
```bash
# Padrões aceitos:
# ^release-[0-9]+-[0-9]+-[0-9]+$  ou  ^release-[0-9]+\.[0-9]+\.[0-9]+$

# Criar a partir de develop para homologação
gsk_create_remote_branch_release 1.2.3

# Correções durante homologação:
gsk_commit  # commits diretos no release

# Finalizar: merge em production
# (não permite rebase)
```

### Hotfix Production (`hotfix-production-<taskId>`)
```bash
# Padrões aceitos:
# ^hotfix-production-[0-9]+$  ou  ^temp/production/hotfix/[0-9]+$

# Criar a partir de production
gsk_create_remote_branch_type 12345

# Finalizar: merge em production via PR
```

### Hotfix RC (`temp/main-rc/hotfix/<taskId>`)
```bash
# Criar a partir de main-rc
gsk_create_remote_branch_type 12345
```

### Hotfix (`hotfix-<taskId>` ou `temp/main/hotfix/<taskId>`)
```bash
# Padrões aceitos:
# ^hotfix-[0-9]+$  ou  ^temp/main/hotfix/[0-9]+$

# Criar a partir de main
gsk_create_remote_branch_type 12345
```

## Fluxos entre Branches

### Fluxo de Manutenção (Merge - Subindo)
```
main → (main-rc) → (production) → (release) → develop
```
Correções sobem das branches de produção para develop.

### Fluxo de Sprints (Rebase - Descendo)
```
develop → (release) → (production) → (main-rc) → main
```
Funcionalidades descem de develop para produção via rebase.

## Restrições Importantes

| Branch | Operação | Restrição |
|--------|----------|-----------|
| `release` | rebase | NÃO PERMITIDO |
| permanentes | rebase | NÃO PERMITIDO (exceto com `--allow-permanent-branch`) |
| qualquer | push force | SEMPRE precedido de backup em `.gsk-aux` |

## Passos de Implementação

1. Utilizar o comando apropriado para criar o branch temporário
2. Realizar os commits necessários no branch
3. O PR deve ser aberto executando a respectiva pipeline no Azure DevOps
4. Após a aprovação do PR, o branch temporário será excluído automaticamente
5. Para release branches, fazer o rebase manualmente na production