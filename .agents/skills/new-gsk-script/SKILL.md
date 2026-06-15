---
name: new-script-gsk
description: Criar um novo script GSK seguindo a estrutura padrão com gskConstants, gskHelp, gskValidate e gskExec
---

# Criar Novo Script GSK

Cria um script GSK seguindo os padrões do projeto.

## Estrutura Padrão

Todo script GSK deve ter estas quatro funções:

```bash
#!/bin/bash

gskConstants () {
	# Definir constantes específicas do script
	local cOptionShort="o"
	local cOptionLong="option"
}

gskHelp () {
	# Exibir ajuda (-h ou --help)
	if [ $# -gt 0 ] && [ "$(show_help $*)" == true ]; then
		get_help "$*" -r "Descrição do script."
		get_help "$*" -l "$0"
		get_help "$*" -p
		get_help "$*" --p-obg "parametro obrigatorio" "1"
		get_help "$*" --p-opc "-o|--option, descrição da opção."
		exit 1
	fi
}

gskValidate () {
	# Validar parâmetros
	if [ -z "$1" ]; then
		log_error "Mensagem de erro!"
		gskHelp -h
	fi
	
	# Processar opções com getopt
	if [ $# -gt 0 ]; then
		validate_parameters "$(basename $0)" "$@"
		[ $? -eq 0 ] || { exit 1; }
		
		local options=""
		options="$(getopt -o "$cOptionShort" -l "$cOptionLong" -- "$@" 2>&1)"
		[ $? -eq 0 ] || {
			log_error_invalid_parameter "$(basename $0)" "$(printf "%s" "$options" | head -n1)"
			exit 1
		}
		
		eval set -- "$options"
		while true; do
			case "$1" in
			-$cOptionShort|--$cOptionLong)
				# Processar opção
				;;
			--)
				shift
				break
				;;
			esac
			shift
		done
	fi
}

gskExec () {
	# Nota: O logging abaixo é obrigatório apenas para scripts em /dev e /scm
	local startTime=$(date +%s)
	log_gsk_start "$0" "$*"
	
	# Lógica principal do script
	# ... implementação ...
	
	log_gsk_finish "$0" "$startTime"
}

# Execução
gskConstants
gskHelp "$@"
gskValidate "$@"
gskExec "$@"
```

## Convenções

### Retornos
- `0` = sucesso
- `1` = erro padrão
- `2` = erro com tipo específico

### Variáveis
- Sempre usar escopo `local` para variáveis internas
- Strings com variáveis de ambiente: usar `eval`

### Logging
```bash
log_gsk_start "$0" "$*"          # Início da execução
log_gsk_finish "$0" "$startTime" # Fim da execução
log_error "mensagem"             # Log de erro
log_info "mensagem"              # Log informativo
log_alert "mensagem"             # Alerta ao usuário
```

> **Nota:** O logging de início/fim (`log_gsk_start` / `log_gsk_finish`) é **obrigatório apenas para scripts das pastas `/dev` e `/scm`** (comandos executados diretamente pelos usuários). Scripts de suporte (`system/`, `functions/`, `manager/`, `drafts/`, `deprecated/`) não precisam implementar este logging.

## Exemplo de Uso

```bash
# Criar script simples
gsk-new-command gsk_meu_script

# Estrutura gerada:
# dev/gsk_meu_script
```

## Passos de Implementação

1. Criar arquivo no diretório apropriado (`dev/` ou `scm/` ou nas demais pastas do projeto)
2. Implementar as 4 funções padrão
3. Definir constantes específicas em `gskConstants`
4. Documentar parâmetros em `gskHelp`
5. Validar entrada em `gskValidate`
6. Implementar lógica em `gskExec` (com logging se em `/dev` ou `/scm`)
7. Testar o script com parâmetros válidos e inválidos

> **Importante:** Se o script estiver sendo criado em `system/`, `functions/`, `manager/`, `drafts/` ou `deprecated/`, **não é necessário** implementar o logging de início/fim (`log_gsk_start` / `log_gsk_finish`).
