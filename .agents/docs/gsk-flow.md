# GSK Flow

## Branches permanentes
Branches principais (baselines), que servem como base para a criação dos branches temporários e/ou para a geração das versões.
Ou seja, são branches que nunca serão excluídos do repositório.

- develop
Ambiente de desenvolvimento, utilizado como base para criação dos branches temporários `bugfix`, `feature`, `epic` e `release`.

- production
Ambiente de produção, utilizado como base para criação de branches temporários `hotfix-production` e para geração das Versões Evolutivas (LTE - Long Term Evolution).

- main-rc
Ambiente de produção, utilizado como base base para criação de branches temporários `hotfix-rc` e para geração das Versões Pré Comerciais (LTS-RC - Release Candidate).

- main
Ambiente de produção, utilizado como base base para criação de branches temporários `hotfix` e para geração das Versões Comerciais (LTS - Long Term Support).

## Branches temporários
Branches criados temporariamente para implementar novas funcionalidades ou corrigir erros.
>São excluídos de forma automática assim que o PR (_Pull Request_), para a sua respectiva _baseline_, é aprovado/completado.

- feature (`^feature-[0-9]+$|^temp\/develop\/feature\/[0-9]+$`)
Utilizado para implementar uma tarefa de "Evolução" ou "Legislação" no branch `develop`.

- bugfix (`^bugfix-[0-9]+$|^temp\/develop\/bugfix\/[0-9]+$`)
Utilizado para implementar uma tarefa de "Correção" no branch `develop`.

- epic (`^epic-[0-9]+$|^temp\/develop\/epic\/[0-9]+$`)
Utilizado para implementar mais de uma tarefa de "Evolução", "Legislação" e/ou "Correção" no branch `develop`.

- release (`^release-[0-9]+-[0-9]+-[0-9]+$|^release-[0-9]+\.[0-9]+\.[0-9]+$`)
Utilizado para implementar tarefas de "Correção" referentes a erros identificados durante a homologação interna do branch `develop`. 
Assim que a homologação é finalizada, é realizado o merge no branch `production` e a exclusão do branch `release`.

- hotfix-production (`^hotfix-production-[0-9]+$|^temp\/production\/hotfix\/[0-9]+$`)
Utilizado para implementar uma tarefa de "Evolução", "Legislação" ou "Correção" no branch `production`.

- hotfix-rc (`^temp\/main-rc\/hotfix\/[0-9]+$`)
Utilizado para implementar uma tarefa de "Legislação" ou "Correção" no branch `main-rc`.

- hotfix (`^hotfix-[0-9]+$|^temp\/main\/hotfix\/[0-9]+$`)
Utilizado para implementar uma tarefa de "Legislação" ou "Correção" no branch `main`.

## Estrutura de branches permanentes
- Obrigatório: `develop` e `main`
- Opcional: `production` e `main-rc`

## Fluxos entre os branches permanentes
- Fluxo de manutenção (merge - subindo as linhas): `main → (main-rc) → (production) → (release) → develop`
- Fluxo de sprints (rebase - descendo as linhas): `develop → (release) → (production) → (main-rc) → main`

## Restrições

- Branch `release`: não permite rebase
- Branch `permanent`: não permite rebase (exceto com `--allow-permanent-branch`)
- Push force: sempre precedido de backup local do branch no repositório auxiliar (.gsk-aux)
