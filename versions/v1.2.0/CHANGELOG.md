# Changelog — schematize-go

Formato: [Keep a Changelog]; versionamento: SemVer.

## [1.2.0] — 2026-07-03

### Adicionado
- **Contenção no workspace** (§2 / anti-padrões §37 / `CLAUDE.md`): aplicação/repo novo nasce **dentro da pasta do projeto atual** (`./<projeto>_<contexto>/`). Veto a começar largando arquivos no root e depois **subir de diretório** (`cd ..`, `../`) pra criar repos irmãos fora, ou espalhar arquivos em `~`/`Documents`/`Downloads`/`/tmp`/Área de Trabalho. O agente **não sai da pasta do projeto** (ler ou escrever) sem o usuário pedir.

## [1.1.1] — 2026-06-27

### Alterado
- `/go-claude` passa a **mesclar** o `CLAUDE.md` em repo multi-linguagem (Node/Go/Rust/Web) — não sobrescreve blocos de outras skills; nota de convivência no `CLAUDE.md`.

## [1.1.0] — 2026-06-27

### Adicionado
- **Convenção de nome de repositório** `<projeto>_<contexto>[_<lang>]` e o repo **`<projeto>_ops`** (control plane de desenvolvimento) — arquitetura §2.
- **Independência de runtime** (cada serviço sobe e funciona sozinho; nunca crash em cascata) + **resiliência store-and-forward** (persiste/loga/alerta/retoma na falha de notificação a outro serviço) — §2/§18; anti-padrões 32/33.
- **Observabilidade LGTM+** integrada em toda ferramenta/serviço (OpenTelemetry → Grafana Alloy → Loki/Tempo/Prometheus/Mimir → Grafana, + Helm chart) — §16.
- **Doc-comment com fluxo do dado** (de onde vem → o que faz → pra onde vai, inclusive cross-aplicação) e "todas as funcionalidades mapeadas" — §3/§39.
- Comandos: **`/go-load`** (carrega à força o corpo normativo) e **`/go-claude`** (cria/atualiza o `CLAUDE.md` da raiz).
- `CLAUDE.md` sempre-on atualizado (pisos 10 e 11).

## [1.0.0] — 2026-06-20
Primeira release sob o nome **schematize-go** — padrões normativos de engenharia da casa.

### Adicionado
- Conhecimento normativo completo fatiado em `references/` (arquitetura, segurança,
  dados/eventos, testes/pentest, observabilidade, operação, anti-padrões, contexto).
- Comandos: `/go-help`, `/go-cc`, `/go-handoff`, `/go-qa`, `/go-review`, `/go-index`.
- Scripts: `lib.sh`, `test-skeleton.sh`, `smoke-selfcheck.sh`, `simulated/run.py`,
  `build-index.mjs`, `check-diff.sh`, `archive-secret-scan.sh`, hooks de contexto.
- Assets: `CLAUDE.md`, templates (ADR/TASK/CHAT/PR/RUNBOOK/INDEX_*), `settings.claude.example.json`,
  CI (`ci/`), guard-tests de import (`lint/`), pre-commit (`hooks/`).
- Site `skills.schematize.me/go` (multi-idioma, AI-friendly) + instalador.

### Normativo coberto
- Backend só Go/Rust; Node backend proibido (migração por funcionalidade, §3.1);
  PHP proibido (§3.2); frontend Node 100% permitido (§3).
- Arquivos ≤300 linhas + micro-funções; toda função documentada (§6).
- Índice de funcionalidades como fonte da verdade (§39).
- Q.A. plan-first (§22.9); handoff de contexto (§34.1); archive obrigatório (§28).
- Anti-padrões vetados (§37); testes/pentest com gate de "verde de verdade" (§22).
