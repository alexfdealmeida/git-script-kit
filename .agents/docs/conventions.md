# Convenções e Referências - GSK

## Context flags (GSK_CONTEXT_*)

Usadas para otimizar validações, evitando chamadas desnecessárias ao Git/Azure.
Sempre passadas via `eval` para garantir re-interpretação correta:

```bash
eval "GSK_CONTEXT_ASSUME_CURRENT_BRANCH_REMOTE_EXISTS=true gsk_comando $params"
```

| Flag | Significado |
|------|-------------|
| `GSK_CONTEXT_ASSUME_CURRENT_BRANCH_REMOTE_EXISTS` | Branch atual existe no remoto |
| `GSK_CONTEXT_ASSUME_REFERENCE_BRANCH_REMOTE_EXISTS` | Branch de referência existe no remoto |
| `GSK_CONTEXT_ASSUME_SOURCE_BRANCH_EXISTS` | Branch de origem existe |
| `GSK_CONTEXT_ASSUME_BASE_BRANCH_EXISTS` | Branch base existe |
| `GSK_CONTEXT_ASSUME_REMOTE_BRANCH_EXISTS` | Branch remoto existe |
| `GSK_CONTEXT_ASSUME_LOCAL_TAG_EXISTS` | Tag local existe |

## Constantes globais relevantes

### Branches
```bash
GSK_BRANCH_NAME_DEVELOP="develop"
GSK_BRANCH_NAME_PRODUCTION="production"
GSK_BRANCH_NAME_MAIN="main"
GSK_BRANCH_NAME_MAIN_RC="main-rc"

GSK_BRANCH_TYPE_SINGLE="single"
GSK_BRANCH_TYPE_FEATURE="feature"
GSK_BRANCH_TYPE_BUGFIX="bugfix"
GSK_BRANCH_TYPE_EPIC="epic"
GSK_BRANCH_TYPE_RELEASE="release"
GSK_BRANCH_TYPE_HOTFIX="hotfix"
GSK_BRANCH_TYPE_HOTFIX_RC="hotfix-rc"
GSK_BRANCH_TYPE_HOTFIX_PRODUCTION="hotfix-production"
GSK_BRANCH_TYPE_HOTFIX_VERSION="hotfix-version"
GSK_BRANCH_TYPE_HOTFIX_PRODUCTION_VERSION="hotfix-production-version"
GSK_BRANCH_TYPE_TRAINING="training"
```

### Git
```bash
GSK_GIT_MERGE_STRATEGY_FAST_FORWARD="fast-forward"
GSK_GIT_MERGE_STRATEGY_THREE_WAY="three-way"
GSK_GIT_PARAMETER_NO_FAST_FORWARD="--no-ff"
GSK_GIT_COMMAND_NO_OPERATION="noop"
GSK_GIT_ENVIRONMENT_SEQUENCE_EDITOR="GIT_SEQUENCE_EDITOR"
GSK_GIT_ENVIRONMENT_LFS_SKIP_SMUDGE="GIT_LFS_SKIP_SMUDGE"
```

### Sistema
```bash
GSK_SYSTEM_FOLDER_GSK=".gsk"
GSK_SYSTEM_USER_DIR="${HOME}/.gsk"
GSK_SYSTEM_USER_LOG_DIR="${HOME}/.gsk/log"
GSK_SYSTEM_USER_TEMP_DIR="${HOME}/.gsk/.temp"
GSK_SYSTEM_RESPONSE_YES="s"
GSK_SYSTEM_RESPONSE_NO="n"
GSK_SYSTEM_PROFILE_DEV="[dev]"
GSK_SYSTEM_PROFILE_SCM="[scm]"
```

### Status messages
```bash
GSK_SYSTEM_STATUS_MESSAGE_1="is up to date"
GSK_SYSTEM_STATUS_MESSAGE_2="nothing to commit"
GSK_SYSTEM_STATUS_MESSAGE_3="changes not staged for commit"
GSK_SYSTEM_STATUS_MESSAGE_4="untracked files"
GSK_SYSTEM_STATUS_MESSAGE_5="changes to be committed"
GSK_SYSTEM_STATUS_MESSAGE_6="branch is ahead"
GSK_SYSTEM_STATUS_MESSAGE_7="branch is behind"
GSK_SYSTEM_STATUS_MESSAGE_8="head detached"
GSK_SYSTEM_STATUS_MESSAGE_9="rebase in progress"
GSK_SYSTEM_STATUS_MESSAGE_10="unmerged"
GSK_SYSTEM_STATUS_MESSAGE_11="cherry-picking" # cherry-pick in progress
GSK_SYSTEM_STATUS_MESSAGE_12="merging" # merge in progress
GSK_SYSTEM_STATUS_MESSAGE_13="have diverged"
GSK_SYSTEM_STATUS_MESSAGE_14="rebasing" # rebase in progreess: You are currently editing a commit while rebasing branch 'feature-1106' on '4b9ba2a9e'
GSK_SYSTEM_STATUS_MESSAGE_15="merge" # merge in progress: unmerged and merge
```

## Estado de rebase em andamento

Arquivos salvos em `$HOME/.gsk/git-rebase/`:

```
base-branch-name.txt
current-branch-name.txt
current-branch-locked.txt
base-branch-locked.txt
merge-conflicts.txt
merge-conflicts-temp.txt
<numero>/          # Uma subpasta por conflito
```

## show_rebase_info - formato de saída

```
(nomeBranch|REBASE commitAtual/totalCommits)
```

Lê de `.git/rebase-merge/`: `msgnum` (atual), `end` (total), `head-name` (branch).

## Convenção de mensagem dos commits

```
Branch: <nomeBranchCorrente>; #<taskId> - <descrição em português>
```

Exemplos:
```
Branch: feature-20978; #20978 - gsk_rebase: Corrigido exitCode do método "abortRebase".
Branch: feature-21040; #21040 - load_gsk_constants_system.sh: Adicionadas constantes X e Y.
```

### Commit de merge

```
Branch: <nomeBranchCorrente>; Merge branch '<origin/branch>' into '<currentBranch>' --no-verify-policies-hook

Merged files: <quantidade>
<lista de arquivos modificados>
```
