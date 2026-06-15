## Diretórios

```
gsk/
├── dev/          # Wrappers Git e comandos executados diretamente pelos usuários
├── system/       # Scripts, funções e constantes utilizados sistematicamente pelos demais scripts
├── functions/    # Funções reutilizáveis internas
├── manager/      # Scripts de gerenciamento do GSK
├── scm/          # Scripts executados diretamente pelos usuários SCM
├── personal/     # Scripts personalizados (não sobrescritos em atualizações)
├── drafts/       # Scripts em fase de testes ou validação, não prontos para produção
├── deprecated/   # Scripts legados mantidos para compatibilidade
└── .agents/      # Contexto para agentes de IA
```

> `manager/` e `scm/` são acessíveis apenas pelo perfil `[scm]`.

# Scripts

```bash
gskConstants()   # Constantes específicas do script (se necessário)
gskHelp()        # Exibição de ajuda (-h ou --help)
gskValidate()    # Validação de parâmetros
gskExec()        # Lógica principal
```