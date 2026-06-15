---
name: coding-standards
description: Padrões de codificação para scripts e código - idiomas, nomenclatura, SRP, constantes
---

# Padrões de Codificação

Guia completo de padrões para manter consistência e qualidade no código.

## Idiomas

### Código: Sempre em Inglês
```bash
# CORRETO
local currentBranchName="develop"
local filePath="/tmp/test.txt"
validateUserInput()

# INCORRETO
local nomeBranchAtual="develop"
local caminhoArquivo="/tmp/test.txt"
validarEntradaUsuario()
```

**Aplica-se a:**
- Variáveis
- Funções
- Classes
- Parâmetros
- Chaves de dicionário/objetos
- Nomes de arquivos (quando possível)

### Comentários: Sempre em Português
```bash
# CORRETO
# Verifica se o branch existe no repositório remoto
if [ -z "$remoteBranch" ]; then
	exit 1
fi

# INCORRETO
# Checks if branch exists in remote repository
if [ -z "$remoteBranch" ]; then
	exit 1
fi
```

**Aplica-se a:**
- Comentários inline
- Docstrings/documentação
- Mensagens de commit (conforme convenção GSK)
- Descrições em arquivos de configuração

## Nomenclatura

### Descritiva e Autoexplicativa
```bash
# CORRETO
local currentGitRepositoryPath="/path/to/repo"
getRemoteBranchNameFromLocalBranch()
validateGitConfigurationBeforeCommit()

# INCORRETO
local repoPath="/path/to/repo"  # muito curto
getBrName()                       # abreviado demais
validate()                        # muito genérico
```

### Evitar Abreviações
```bash
# EVITAR
idx, tmp, str, num, ctx, cfg, fn, val, err, msg, param, opts, ref

# PREFERIR
index, temporary, string, number, context, configuration, 
function, value, error, message, parameter, options, reference
```

### Exceções Aceitáveis
Abreviações amplamente reconhecidas no domínio:
```bash
# Aceitável (contexto Git)
repo      # repository (contexto claro)
commitId  # commit identifier
```

## Princípio da Responsabilidade Única (SRP)

### Funções Pequenas e Focadas
```bash
# CORRETO - Função faz uma única coisa
validateBranchName() {
	# Apenas valida o nome do branch
	[[ "$1" =~ $GSK_REGEX_BRANCH_NAME ]] && exit 0 || exit 1
}

createBranch() {
	# Apenas cria o branch
	git branch "$1" "$2"
}

switchToBranch() {
	# Apenas faz checkout
	git checkout "$1"
}

# Uso coordenado
createAndSwitchBranch() {
	validateBranchName "$1" || exit 1
	createBranch "$1" "$2" || exit 1
	switchToBranch "$1"
}

# INCORRETO - Função faz muitas coisas
branchOperations() {
	# Valida, cria, faz checkout, configura upstream, envia para remoto...
	# Muita lógica misturada
}
```

### Componentes/Scripts Coesos
```bash
# Cada script deve ter um propósito claro:
- gsk_create_remote_branch      # Cria branches
- gsk_checkout                  # Faz checkout
- gsk_delete_remote_branch      # Remove branches
- gsk_validate_repository_clean # Valida se o repo está limpo

# NÃO misturar responsabilidades:
- gsk_branch_manager   # Faz tudo (criar, deletar, listar, validar)
```

## Magic Numbers

### Extrair em Constantes Nomeadas
```bash
# INCORRETO - Magic numbers soltos
if [ $exitCode -eq 1 ]; then
	exit 60  # por quê 60?
fi

sleep 300    # 300 o quê? segundos? minutos?

for i in {1..5}; do  # 5 tentativas? ou 5 segundos?
	attemptConnection
 done

# CORRETO - Constantes descritivas
GSK_EXIT_CODE_ERROR=1
GSK_ERROR_TIMEOUT_SECONDS=60
GSK_CONNECTION_RETRY_ATTEMPTS=5
GSK_CONNECTION_RETRY_DELAY_SECONDS=5

if [ $exitCode -eq $GSK_EXIT_CODE_ERROR ]; then
	exit $GSK_ERROR_TIMEOUT_SECONDS
fi

sleep $GSK_CACHE_EXPIRATION_SECONDS

for ((i=1; i<=GSK_CONNECTION_RETRY_ATTEMPTS; i++)); do
	attemptConnection
	sleep $GSK_CONNECTION_RETRY_DELAY_SECONDS
done
```

### Constantes em Arquivos Dedicados
```bash
# Arquivos de constantes do GSK:
load_gsk_constants_system.sh   # Constantes do sistema
load_gsk_constants_branch.sh   # Constantes de branches
load_gsk_constants_git.sh      # Constantes Git
load_gsk_constants_regex.sh    # Padrões regex

# Uso:
source load_gsk_constants_system.sh
source load_gsk_constants_git.sh

# Agora GSK_GIT_MERGE_STRATEGY_FAST_FORWARD está disponível
```

## Padrões Específicos - Scripts GSK

### Estrutura de Funções
```bash
gskConstants() {
	# Definir constantes específicas do script
	local cOptionShort="o"
	local cOptionLong="option"
}

gskHelp() {
	# Exibição de ajuda (-h ou --help)
}

gskValidate() {
	# Validação de parâmetros
}

gskExec() {
	# Lógica principal
}
```

### Retornos Padronizados
```bash
# 0 = sucesso
exit 0

# 1 = erro padrão
exit 1

# 2 = erro com tipo específico
exit 2
```

### Escopo de Variáveis
```bash
# SEMPRE usar 'local' para variáveis internas
gskExec() {
	local startTime=$(date +%s)           # CORRETO
	local currentBranchName="develop"     # CORRETO
	
	startTime=$(date +%s)                 # INCORRETO - sem local
}
```

### Strings com Variáveis de Ambiente
```bash
# Usar 'eval' para garantir re-interpretação correta
eval "GSK_CONTEXT_ASSUME_CURRENT_BRANCH_REMOTE_EXISTS=true gsk_comando $params"
```

## Checklist de Revisão

- [ ] Código (variáveis, funções, parâmetros) em **inglês**
- [ ] Comentários em **português**
- [ ] Nomes descritivos, sem abreviações desnecessárias
- [ ] Funções pequenas (máx. 20-30 linhas ideal)
- [ ] Cada função tem uma única responsabilidade clara
- [ ] Nenhum magic number - todos extraídos em constantes
- [ ] Variáveis internas usam escopo `local`
- [ ] Retornos padronizados (0, 1, 2)
- [ ] Estrutura GSK completa (4 funções)

## Exemplo Completo

```bash
#!/bin/bash

# Script: gsk_check_branch_status
# Verifica o status sincronização de um branch com seu remoto

gskConstants() {
	# Padrão de saída para branch não encontrado
	GSK_BRANCH_STATUS_UNKNOWN="unknown"
	GSK_BRANCH_STATUS_AHEAD="ahead"
	GSK_BRANCH_STATUS_BEHIND="behind"
	GSK_BRANCH_STATUS_SYNCED="synced"
	
	# Timeout para operações de rede
	GSK_NETWORK_TIMEOUT_SECONDS=30
}

gskHelp() {
	if [ $# -gt 0 ] && [ "$(show_help $*)" == true ]; then
		get_help "$*" -r "Verifica o status de sincronização do branch atual com o remoto."
		get_help "$*" -l "$0"
		get_help "$*" -p
		get_help "$*" --p-opc "--branch, nome do branch (padrão: branch atual)."
		exit 1
	fi
}

gskValidate() {
	if [ $# -gt 0 ]; then
		validate_parameters "$(basename $0)" "$@"
		[ $? -eq 0 ] || { exit 1; }
	fi
}

gskExec() {
	local startTime=$(date +%s)
	log_gsk_start "$0" "$*"
	
	# Obtém o nome do branch (parâmetro ou atual)
	local targetBranchName=""
	if [ -n "$pBranchValue" ]; then
		targetBranchName="$pBranchValue"
	else
		targetBranchName=$(gsk_show_current_branch_name 2>&1)
		if [ $? -ne 0 ]; then
			log_preserve_formatting "$targetBranchName"
			log_gsk_finish "$0" "$startTime"
			exit 1
		fi
	fi
	
	# Verifica existência do branch remoto
	local remoteBranchName="origin/${targetBranchName}"
	local remoteBranchExists=$(gsk_branch_exists "$remoteBranchName" 2>&1)
	
	if [ $? -ne 0 ] || [ "$remoteBranchExists" != "true" ]; then
		echo "$GSK_BRANCH_STATUS_UNKNOWN"
		log_gsk_finish "$0" "$startTime"
		exit 0
	fi
	
	# Obtém contagem de commits ahead/behind
	local aheadBehindCount=$(git rev-list --left-right --count \
		"${targetBranchName}...${remoteBranchName}" 2>/dev/null)
	
	if [ $? -ne 0 ]; then
		echo "$GSK_BRANCH_STATUS_UNKNOWN"
		log_gsk_finish "$0" "$startTime"
		exit 0
	fi
	
	local aheadCount=$(echo "$aheadBehindCount" | awk '{print $1}')
	local behindCount=$(echo "$aheadBehindCount" | awk '{print $2}')
	
	# Determina e retorna o status
	local finalStatus="$GSK_BRANCH_STATUS_SYNCED"
	if [ "$aheadCount" -gt 0 ]; then
		finalStatus="$GSK_BRANCH_STATUS_AHEAD"
	elif [ "$behindCount" -gt 0 ]; then
		finalStatus="$GSK_BRANCH_STATUS_BEHIND"
	fi
	
	echo "$finalStatus"
	log_gsk_finish "$0" "$startTime"
}

gskConstants
gskHelp "$@"
gskValidate "$@"
gskExec "$@"
```
