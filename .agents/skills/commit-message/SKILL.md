---
name: commit-message
description: Gerar mensagens de commit seguindo as convenções do projeto GSK
---

# Mensagens de Commit GSK

Guia para formatar mensagens de commit conforme convenções do projeto.

## Formato Padrão

```
Branch: <nomeBranchCorrente>; #<taskId> - <descrição em português>
```

### Componentes

| Componente | Descrição | Exemplo |
|------------|-----------|---------|
| `Branch:` | Prefixo obrigatório | `Branch: feature-12345;` |
| `<nomeBranchCorrente>` | Nome da branch atual | `feature-12345` |
| `#<taskId>` | Número do ticket/tarefa | `#12345` |
| `-` | Separador | `-` |
| `<descrição>` | Descrição em português | `gsk_commit: Adicionado parâmetro --force` |

## Exemplos

### Feature
```
Branch: feature-20978; #20978 - gsk_rebase: Corrigido exitCode do método "abortRebase".
Branch: feature-21040; #21040 - load_gsk_constants_system.sh: Adicionadas constantes X e Y.
```

### Bugfix
```
Branch: bugfix-23911; #23911 - gsk_status: Corrigida regex para mapeamento de status.
Branch: bugfix-12345; #12345 - gsk_merge: Tratado erro em branch não existente.
```

### Epic
```
Branch: epic-15000; #<taskIdChild> - Implementado módulo de integração Azure DevOps.
```

### Hotfix
```
Branch: hotfix-99999; #99999 - gsk_flow: Corrigido loop infinito no rebase.
Branch: hotfix-production-88888; #88888 - Corrigida vulnerabilidade de segurança.
```

## Commit de Merge

### Formato
```
Branch: <nomeBranchCorrente>; Merge branch '<origin/branch>' into '<currentBranch>' --no-verify-policies-hook

Merged files: <quantidade>
<lista de arquivos modificados>
```

### Exemplo
```
Branch: develop; Merge branch 'origin/feature-12345' into 'develop' --no-verify-policies-hook

Merged files: 3
src/gsk_new_feature.sh
dev/gsk_command
README.md
```

## Geração Automática

### Usando gsk_commit
```bash
# Gera mensagem automaticamente a partir do nome da branch
gsk_commit

# Com mensagem personalizada (sobrescreve padrão)
gsk_commit --message="Descrição personalizada"
```

### Função Interna
```bash
# Obter mensagem semântica a partir do nome da branch
get_semantic_commit_message_from_branch_name "$branchName" --message="$customMessage"
```

## Checklist da Mensagem

- [ ] Prefixo `Branch:` presente
- [ ] Nome da branch correto
- [ ] Ticket iniciado com `#`
- [ ] Separador `-` após o ticket
- [ ] Descrição em português
- [ ] Descrição clara e objetiva
- [ ] Pontuação adequada

## Exemplos por Cenário

### Correção de bug simples
```
Branch: bugfix-12345; #12345 - gsk_validate: Corrigido retorno de erro quando arquivo não existe.
```

### Nova funcionalidade
```
Branch: feature-67890; #67890 - gsk_azure_api: Implementado método para buscar work items.
```

### Refatoração
```
Branch: feature-11111; #11111 - gsk_merge: Refatorado método de validação de conflitos.
```

### Documentação
```
Branch: feature-22222; #22222 - README: Atualizada documentação de instalação.
```
