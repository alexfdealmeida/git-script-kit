# Update WinMerge

## 1. Atualizar instalador do WinMerge (remover antigo e adicionar novo)
`siagrigsk-setup/setup/installer`

## 2. Atualizar versão nos scripts de instalação do WinMerge
`siagrigsk-setup/setup/WinMerge.ps1`
`siagrigsk-setup/setup/installer/winmerge-install.cmd`

## 3. Instalar a nova versão do WinMerge
`siagrigsk-setup/setup/installer/winmerge-install.cmd`

## 4. Atualizar a versão do WinMerge no arquivo `tools.properties`
`gsk change_version --tools`

## 5. Fazer commit/push
`gsk commit_all --message="Atualizada versão do WinMerge"`