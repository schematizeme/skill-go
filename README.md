# schematize-go

> Padrões normativos de engenharia da casa — **Go principal, Rust auxiliar**. Arquitetura, segurança, testes/pentest, dados, observabilidade, deploy e archive.

Pacote de **skill normativa para [Claude Code](https://claude.com/claude-code)**.
Parte do catálogo **schematize skills** ([skills.schematize.me](https://skills.schematize.me)).

## Instalar

### Última versão (recomendado)

A partir de um clone do repositório:

```bash
git clone https://github.com/schematizeme/skill-go.git
cd skill-go && ./install.sh            # instala no projeto atual (diretório corrente)
# ./install.sh /caminho/do/projeto        # ou aponte para outro projeto
```

Ou baixe o `.zip` da última release e descompacte direto em `.claude/skills/`:

```bash
curl -L -o schematize-go.zip \
  https://github.com/schematizeme/skill-go/releases/latest/download/skill-go.zip
unzip schematize-go.zip -d .claude/skills/
```

### Uma versão específica

Cada versão tem três formas de obter: **(1)** um Release com `.zip` para baixar,
**(2)** uma pasta navegável em `versions/`, e **(3)** uma tag git.

| Versão | Data | Download (.zip) | Pasta navegável | Notas |
|---|---|---|---|---|
| **1.0.0** | 2026-06-20 | [release](https://github.com/schematizeme/skill-go/releases/download/v1.0.0/skill-go.zip) | [versions/v1.0.0/](versions/v1.0.0) | [CHANGELOG](CHANGELOG.md) |

```bash
# clonar uma versão exata pela tag:
git clone --branch v1.0.0 https://github.com/schematizeme/skill-go.git
```

> Todas as versões aparecem na página de **[Releases](https://github.com/schematizeme/skill-go/releases)**.

## Comandos

Todos prefixados por `go-` — **sem conflito** com as outras skills na mesma máquina.

| Comando | O que faz |
|---|---|
| `/go-cc` | comando `go-cc` |
| `/go-handoff` | comando `go-handoff` |
| `/go-help` | comando `go-help` |
| `/go-index` | comando `go-index` |
| `/go-qa` | comando `go-qa` |
| `/go-review` | comando `go-review` |

Digite `/go-help` dentro do Claude Code para ver a lista completa.

## Conteúdo da skill

- `SKILL.md` — porta de entrada e pisos inegociáveis.
- `references/` — corpo normativo fatiado por domínio (leia o que casa com a tarefa).
- `assets/` — templates (ADR/TASK/RUNBOOK/…), comandos, `CLAUDE.md`, CI, lint, hooks.
- `scripts/` — andaime de testes, índice e gestão de contexto.
- `skill.toml` — manifesto da skill (slug, nome, versão, descrições).

## Skills irmãs

- [skill-go](https://github.com/schematizeme/skill-go) — backend Go principal.
- [skill-rust](https://github.com/schematizeme/skill-rust) — backend Rust principal.
- [skill-web](https://github.com/schematizeme/skill-web) — frontend / SEO / performance.

As três podem ficar habilitadas ao mesmo tempo: os comandos são namespaced por skill
(`go-*`, `rust-*`, `web-*`).

## Licença

[MIT](LICENSE) © 2026 schematizeme.
