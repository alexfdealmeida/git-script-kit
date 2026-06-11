# functions

Esta pasta contém scripts prontos para uso em produção.

## Objetivo 
Armazenar funções reutilizáveis por outros scripts do GSK.

## Regras
- O nome dos scripts deve iniciar com o prefixo `gsk_`.
- Acesso permitido para os perfis `[dev]` e `[scm]`.

## Observação
Evite depender de scripts externos a esta pasta. Pois, espera-se que essas funções tenham um retorno "limpo" - sem execução de logs, validações ou fluxos adicionais.