# Git Update History

Esse arquivo detalha os impactos para manter a compatibilidade entre as versões do Git e do GSK.

## 2.54.0.1 - 22/06/26

- O Git 2.54+ alterou o comportamento de `bash.exe --cd=...`, fazendo com que a variável `PWD` fosse inicializada como `C:/...` em alguns cenários.
  - Nesses casos, foi necessário substituir `bash.exe --cd=...` por `bash.exe -c "cd ..."`.

> A integração com o menu de contexto do Windows `git-bash.exe --cd=%V`permanece compatível, e, portanto, foi preservada.
> Os scripts de instalação do Git LFS foram removidos, pois o Git LFS já está incluído no instalador do Git.