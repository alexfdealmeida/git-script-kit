---
name: gsk
description: Framework Shell Script para automação Git, SCM e DevOps. Use quando o usuário mencionar scripts gsk, GSK Flow, branches feature/bugfix/epic/hotfix/release, context flags (GSK_CONTEXT_*), integração com Azure DevOps ou qualquer modificação nos scripts do GSK.
---

# GSK - Git Script Kit

## O que é este projeto

O GSK (Git Script Kit) é um framework Shell Script para Git, Software Configuration Management (SCM) e DevOps, desenvolvido para automatizar processos, simplificar operações e fortalecer a governança do ciclo de desenvolvimento de software.

Disponibiliza uma CLI unificada que padroniza fluxos de ramificação por meio do [GSK Flow](docs/assets/gsk-flow.png), encapsula operações complexas do Git (merge, rebase, cherry-pick, entre outras), oferece suporte a recursos avançados (Git LFS e submodules), integra-se nativamente à API do Azure DevOps e permite automatizar pipelines de CI/CD no Jenkins e no Azure DevOps.

É construído sobre uma arquitetura modular orientada ao princípio da responsabilidade única (SRP), com scripts internos reutilizáveis que reduzem duplicação de código e 
simplificam a manutenção. Cada comando inclui validação de parâmetros, sistema de ajuda estruturado e registro de execução voltado para auditoria e resolução de problemas. É compatível com Windows, WSL e Linux, com suporte completo a Bash e Zsh.

## Por que este projeto existe

O GSK foi criado para resolver desafios reais em ambientes de desenvolvimento de grande escala, onde o uso do Git varia significativamente entre equipes e erros operacionais podem gerar retrabalho, pipelines quebrados e inconsistências.

Antes do GSK, os problemas mais comuns incluíam:

- Inconsistências na gestão de branches.
- Operações manuais de Git perigosas ou repetitivas.
- Conflitos de merge causados por fluxos de trabalho inadequados.
- Falta de uniformidade entre usuários de Windows, WSL e Linux.
- Ausência de integração entre ações do Git e o Azure DevOps.
- Erros humanos recorrentes afetando processos de SCM e DevOps.

## Estrutura de diretórios

```
siagrigsk/
├── dev/          # Scripts prontos para uso ([dev] e [scm])
├── system/       # Constantes e funções/scripts sistêmicos
├── functions/    # Funções reutilizáveis internas
├── manager/      # Scripts de gerenciamento (apenas [scm])
├── scm/          # Scripts para usuários SCM (apenas [scm])
├── deprecated/   # Scripts em desuso
└── .context/     # Contexto para agentes de IA
```

## Convenções de commit

```
Branch: <nomeBranch>; #<ticket> - <descrição em português>
```

Exemplos:
```
Branch: feature-20978; #20978 - gsk_rebase: Corrigido exitCode do método "abortRebase".
Branch: feature-21040; #21040 - load_gsk_constants_system.sh: Adicionadas constantes X e Y.
```

## Convenções de branch (GSK Flow)

### Branches permanentes: `develop`, `production`, `main-rc`, `main`

### Branches temporários: `feature-<taskId>`, `bugfix-<taskId>`, `feature-<taskId>`, `epic-<taskId>`, `release-<version>`, `hotfix-production-<taskId>`, `temp/main-rc/hotfix/<taskId>`, `hotfix-<taskId>`

Detalhamento completo em `.context/docs/gsk-flow.md`.

## Padrão de scripts GSK

Cada script segue esta estrutura de funções:

```bash
gskConstants()   # Constantes específicas do script (se necessário)
gskHelp()        # Exibição de ajuda (-h ou --help)
gskValidate()    # Validação de parâmetros
gskExec()        # Lógica principal
```

Retornos: `0` = sucesso, `1` = erro com log, `2` = erro com tipo específico.

Variáveis internas sempre com escopo `local`. Strings com variáveis de ambiente interpretadas com `eval`.

## Context flags (otimização)

Evitam buscas repetidas em validações encadeadas. Passadas via `eval`:

```bash
eval "GSK_CONTEXT_ASSUME_CURRENT_BRANCH_REMOTE_EXISTS=true gsk_prepare_merge $parameters"
```

Flags disponíveis em `.context/docs/conventions.md`.

## Documentação

- `.context/docs/conventions.md` - flags, constantes e convenções detalhadas
