---
name: validate-gsk-script
description: Validar se um script segue os padrões do projeto GSK
---

# Validar Script GSK

Verifica se um script GSK segue todas as convenções do projeto.

## Checklist de Validação

### 1. Estrutura Obrigatória
```bash
# O script deve conter estas 4 funções:
gskConstants()   # Constantes específicas do script
gskHelp()        # Exibição de ajuda (-h ou --help)
gskValidate()    # Validação de parâmetros
gskExec()        # Lógica principal
```

### 2. Convenções de Retorno
```bash
# Verificar se o script retorna:
exit 0  # ou return 0  -> Sucesso
exit 1  # ou return 1  -> Erro padrão
exit 2  # ou return 2  -> Erro com tipo específico
```

### 3. Variáveis Locais
```bash
# TODAS as variáveis internas devem usar escopo local:
local minhaVariavel="valor"  # CORRETO
minhaVariavel="valor"        # INCORRETO - sem local
```

### 4. Strings com Variáveis de Ambiente
```bash
# Deve usar eval para interpretação correta:
eval "GSK_CONTEXT_ASSUME_CURRENT_BRANCH_REMOTE_EXISTS=true gsk_comando $params"
```

### 5. Logging (Obrigatório apenas para /dev e /scm)
```bash
# Scripts em /dev e /scm devem registrar início e fim:
local startTime=$(date +%s)
log_gsk_start "$0" "$*"
# ... lógica ...
log_gsk_finish "$0" "$startTime"
```

> **Nota:** A estrutura de logging (`log_gsk_start` / `log_gsk_finish`) é **obrigatória apenas para scripts das pastas `/dev` e `/scm`** (comandos executados diretamente pelos usuários). Scripts de outras pastas (`system/`, `functions/`, `manager/`, `drafts/`, `deprecated/`) não precisam implementar este logging.

### 6. Constantes (se necessário)
```bash
# Scripts que usam constantes devem carregar:
source load_gsk_constants_system.sh
source load_gsk_constants_branch.sh
source load_gsk_constants_git.sh
source load_gsk_constants_regex.sh
```

## Validação de Parâmetros

### Formato Correto
```bash
gskValidate () {
	# Validação de parâmetros obrigatórios
	if [ -z "$1" ]; then
		log_error "Mensagem explicativa!"
		gskHelp -h
	fi
	
	# Validação de existência de arquivos/diretórios
	if ! [ -e "$1" ]; then
		log_error "Arquivo não existe: '$1'!"
		gskHelp -h
	fi
	
	# Processamento de opções
	if [ $# -gt 0 ]; then
		validate_parameters "$(basename $0)" "$@"
		[ $? -eq 0 ] || { exit 1; }
	fi
}
```

## Validação de Help

### Formato Correto
```bash
gskHelp () {
	if [ $# -gt 0 ] && [ "$(show_help $*)" == true ]; then
		get_help "$*" -r "Descrição resumida do comando."
		get_help "$*" -l "$0"
		get_help "$*" -p  # Lista parâmetros
		get_help "$*" --p-obg "nome" "descricao" "posicao"
		get_help "$*" --p-opc "-s|--short, descrição da opção."
		exit 1
	fi
}
```

## Comandos de Validação

```bash
# Verificar existência das 4 funções
grep -E "^gsk(Constants|Help|Validate|Exec)\s*\(\)" script

# Verificar uso de 'local' em variáveis
# (deve haver local em atribuições dentro de funções)

# Verificar log_gsk_start e log_gsk_finish
grep "log_gsk_start" script
grep "log_gsk_finish" script

# Verificar retornos corretos
grep -E "(exit|return)\s+[0-2]" script
```

## Correções Comuns

### Problema: Variável sem escopo local
```bash
# ANTES
minhaVar="valor"

# DEPOIS
local minhaVar="valor"
```

### Problema: Falta de validação de parâmetros
```bash
# ANTES
gskValidate () {
	# sem validação
}

# DEPOIS
gskValidate () {
	if [ -z "$1" ]; then
		log_error "Informe o parâmetro!"
		gskHelp -h
	fi
}
```

### Problema: Sem logging de início/fim (scripts /dev e /scm)
```bash
# ANTES (para scripts em /dev ou /scm)
gskExec () {
	# lógica apenas
}

# DEPOIS
# Nota: Obrigatório apenas para scripts em /dev e /scm
gskExec () {
	local startTime=$(date +%s)
	log_gsk_start "$0" "$*"
	# lógica
	log_gsk_finish "$0" "$startTime"
}
```

> **Atenção:** Esta correção se aplica **apenas a scripts das pastas `/dev` e `/scm`**. Scripts de suporte (`system/`, `functions/`) não precisam deste logging.
