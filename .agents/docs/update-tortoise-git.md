# Update TortoiseGit

## 1. Atualizar instalador do TortoiseGit (remover antigo e adicionar novo)
`siagrigsk-setup/setup/installer`

## 2. Atualizar versão no script de instalação do TortoiseGit
`siagrigsk-setup/setup/TortoiseGit.ps1`

## 3. Instalar a nova versão do TortoiseGit
`siagrigsk-setup/setup/TortoiseGit.cmd`

## 4. Atualizar a versão do TortoiseGit no arquivo `tools.properties`
`gsk change_version --tools`

## 5. Fazer commit/push
`gsk commit_all --message="Atualizada versão do TortoiseGit"`