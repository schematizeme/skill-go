---
description: schematize-go — cria ou ATUALIZA (sobrescreve) o CLAUDE.md da raiz do repositório com a versão atual da skill; faz backup se houver customização local
---

Sincronize o **`CLAUDE.md` da raiz** deste repositório com a versão **atual** da skill `schematize-go`.

1. Localize o `assets/CLAUDE.md` da skill: `.claude/skills/schematize-go/assets/CLAUDE.md` (instalação no projeto) ou `~/.claude/skills/schematize-go/assets/CLAUDE.md` (global).
2. **Se já existe `./CLAUDE.md`:** compare com o da skill.
   - Se for uma versão antiga do template (sem customização), **sobrescreva** pela atual.
   - Se tiver customização local (seções que você/o usuário adicionaram fora do template), **preserve**: salve `./CLAUDE.md.bak` com o conteúdo antigo, aplique o template novo e reaplique as customizações por cima — avisando o que mudou.
3. **Se não existe:** crie `./CLAUDE.md` a partir do `assets/CLAUDE.md` da skill.
4. Confirme ao usuário: caminho, se **sobrescreveu** ou **mesclou**, e se gerou backup.

Este é o jeito **explícito de atualizar um `CLAUDE.md` que já existe** — rodar não pode deixar a versão antiga. Em repo full-stack, rode também o comando do lado correspondente (este é o de backend Go; o frontend vem do `schematize-web`, o Rust-first do `schematize-rust`).
