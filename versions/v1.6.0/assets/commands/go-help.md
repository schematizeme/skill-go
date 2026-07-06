---
description: schematize-go — lista todos os comandos disponíveis e o que cada um faz
---

Mostre ao usuário a lista de comandos do conjunto **schematize-go**, em formato
de tabela legível, exatamente com este conteúdo (ajuste se houver comandos novos
instalados em `.claude/commands/`):

| Comando | O que faz |
|---|---|
| `/go-help` | Lista todos os comandos do schematize-go (este). |
| `/go-load` | **Carrega à força TODO o corpo normativo** (DDD/arquitetura, clean code, segurança, dados, testes, operação) no contexto e passa a aplicá-lo no projeto como regra inegociável. |
| `/go-claude` | Cria ou **atualiza (sobrescreve)** o `CLAUDE.md` da raiz com a versão atual da skill (backup se houver customização local). |
| `/go-cc` | Context compact: gera `context.md` + `checklist.md` em `<projeto>_archive/context/` e roda `/compact`. |
| `/go-handoff` | Gera o handoff (`context.md` + `checklist.md`) **sem** compactar — ideal pra fim de sessão ou troca de tarefa. |
| `/go-qa` | Fluxo de Q.A. plan-first (§22.9): planeja tudo, gera MD de passo a passo, pede aprovação, e roda faseado/assistido ou de uma vez. |
| `/go-review` | Roda o gate da Definition of Done e dos anti-padrões (§35, §37): arquivos >300 linhas, função sem doc-comment, índice desatualizado, macaquices de segurança. |
| `/go-index` | (Re)gera o índice de microfunções (§39) a partir dos doc-comments das funções. |
| `/go-ops` | Audita/scaffolda o `<projeto>_ops` (interface única): fluxo de ambientes, instalação paralela (`nproc`), independência dos serviços. |

Depois da tabela, diga em uma linha que o detalhe normativo está na skill
`schematize-go` (referências em `references/`) e que o site é `skills.schematize.me/go`.
