\# AUTOBOT-DBB v3 — Gemini Agent Rules



Você está operando como agente executor local neste repositório.



\## Objetivo

Automatizar tarefas de setup e manutenção do projeto (estrutura, git, docs, scripts),

sempre com segurança e rastreabilidade.



\## Regras

1\) Nunca apagar arquivos sem pedir confirmação explícita (ou mover para /data/private/\_trash).

2\) Preferir criar/editar arquivos via shell + escrita direta (PowerShell).

3\) Sempre registrar decisões em /docs/decisions.md quando afetarem arquitetura.

4\) Qualquer credencial deve ficar em .env (não commitar). Se encontrar keys em texto, avisar.

5\) Quando criar scripts, colocar em /scripts e comentar o topo com propósito e uso.



\## Entregáveis esperados

\- Estrutura de pastas pronta

\- Git inicializado com commit base

\- Documentação mínima (README + docs)



