# Update Git

## 1. Atualizar instalador do Git (remover antigo e adicionar novo)
`siagrigsk-setup/setup/installer`

## 2. Atualizar versão nos scripts de instalação do Git
`siagrigsk-setup/setup/Git.ps1`
`siagrigsk-setup/setup/installer/git-install.cmd`

## 3. Instalar a nova versão do Git
`siagrigsk-setup/setup/installer/git-install.cmd`

## 4. Executar os testes unitarios para verificar a compatibilidade entre as versoes do Git e do GSK
`gsk unit_tests_git_compatibility`

## 5. Atualizar a versão do Git e do GitLFS (`git lfs version`) no arquivo `tools.properties`
`gsk change_version --tools`

## 6. Fazer commit/push
`gsk commit_all --message="Atualizada versão do Git"`