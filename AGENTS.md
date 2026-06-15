# GSK - Git Script Kit

## Projeto

O GSK (Git Script Kit) é um framework Shell Script para Git, Software Configuration Management (SCM) e DevOps, desenvolvido para automatizar processos, simplificar operações e fortalecer a governança do ciclo de desenvolvimento de software.

Disponibiliza uma CLI unificada que padroniza fluxos de ramificação por meio do GSK Flow, encapsula operações complexas do Git (merge, rebase, cherry-pick, entre outras), oferece suporte a recursos avançados (Git LFS e submodules), integra-se nativamente à API do Azure DevOps e permite automatizar pipelines de CI/CD no Jenkins e no Azure DevOps.

É construído sobre uma arquitetura modular orientada ao princípio da responsabilidade única (SRP), com scripts internos reutilizáveis que reduzem duplicação de código e 
simplificam a manutenção. Cada comando inclui validação de parâmetros, sistema de ajuda estruturado e registro de execução voltado para auditoria e resolução de problemas. É compatível com Windows, WSL e Linux, com suporte completo a Bash e Zsh.

## Padrões de codificação dos scripts

- Retornos: `0` = sucesso, `1` = erro padrão, `2` = erro com tipo específico
- Variáveis internas sempre com escopo `local`
- Strings com variáveis de ambiente interpretadas com `eval`

## Docs disponíveis

- Use `.agents/docs/conventions.md` para documentação sobre context flags, constantes globais e convenções de mensagens dos commits
- Use `.agents/docs/gsk-flow.md` para documentação sobre fluxo de branches do GSK Flow: permanentes, temporários e regras de merge/rebase
- Use `.agents/docs/structure.md` para documentação sobre estrutura dos diretórios e scripts

## Skills disponíveis

- Use `.agents/skills/coding-standards/SKILL.md` ao escrever ou revisar código
- Use `.agents/skills/commit-message/SKILL.md` para gerar mensagens de commit seguindo as convenções do projeto
- Use `.agents/skills/gsk-flow/SKILL.md` para trabalhar com o fluxo de branches do GSK Flow
- Use `.agents/skills/new-gsk-script/SKILL.md` para criar novos scripts no GSK
- Use `.agents/skills/validate-gsk-script/SKILL.md` para validar scripts no GSK