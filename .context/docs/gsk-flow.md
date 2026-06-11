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
><font color="orange">São excluídos de forma automática assim que o PR (_Pull Request_), para a sua respectiva _baseline_, é aprovado/completado.</font>

- feature
Utilizado para implementar uma tarefa de "Evolução" ou "Legislação" no branch `develop`.

- bugfix
Utilizado para implementar uma tarefa de "Correção" no branch `develop`.

- epic
Utilizado para implementar mais de uma tarefa de "Evolução", "Legislação" e/ou "Correção" no branch `develop`.

- release
Utilizado para implementar tarefas de "Correção" referentes a erros identificados durante a homologação interna do branch `develop`. 
Assim que a homologação é finalizada, é realizado o merge no branch `production` e a exclusão do branch `release`.

- hotfix-production
Utilizado para implementar uma tarefa de "Evolução", "Legislação" ou "Correção" no branch `production`.

- hotfix-rc
Utilizado para implementar uma tarefa de "Legislação" ou "Correção" no branch `main-rc`.

- hotfix
Utilizado para implementar uma tarefa de "Legislação" ou "Correção" no branch `main`.

## Estrutura de branches permanentes
- Obrigatório: `develop` e `main`
- Opcional: `production` e `main-rc`

## Fluxos entre os branches permanentes
- Fluxo de manutenção (merge - subindo as linhas): `main → (main-rc) → (production) → (release) → develop`
- Fluxo de sprints (rebase - descendo as linhas): `develop → (release) → (production) → (main-rc) → main`
