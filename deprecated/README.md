# deprecated

Esta pasta contém scripts prontos para uso em produção.

## Objetivo 
Armazenar os scripts legados do GSK. Por exemplo, para manter a compatibilidade em integrações existentes com ALM, CI/CD e hooks.

## Regras
- Acesso permitido para os perfis `[dev]` e `[scm]`.
- Se for necessário renomear um script que já está em produção, deve-se mover o script antigo para esta pasta. Em seguida, editá-lo para apenas fazer uma chamada para o novo script. Se possível, utilizar o script `log_deprecated_alert`, para que os usuários possam ser alertados que o script antigo está em desuso.

## Observação
Após um período de transição, os scripts podem ser removidos definitivamente sem aviso prévio.