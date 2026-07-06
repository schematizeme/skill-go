# Changelog — schematize-go

Formato: [Keep a Changelog]; versionamento: SemVer.

## [1.6.0] — 2026-07-06
Deploy destrutivo por seed + isolamento por usuário (ops).

### Adicionado
- references/ops.md §2: layout /<app>/ + repos clonados dentro; /<app>/.env como seeder global (fonte única); redeploy destrutivo na aplicação (apaga e recria clone zerado do seed), preservando DADOS (migration reversível; ops reset de dados só dev/hml).
- references/ops.md §3: isolamento por usuário — user Linux + systemd unit hardened por serviço (blast radius mínimo); tudo automatizado pelo ops.
- Piso de seed/isolamento no CLAUDE.md; anti-padrões (patch in-place/redeploy fora do seed; config/repos fora de /<app>/; apagar dados no destrutivo; serviços sem isolamento de user); /go-ops audita layout/seed/isolamento.

## [1.5.0] — 2026-07-05
Control plane <projeto>_ops: fluxo de ambientes, ops interface única, instalação paralela, independência invariante.

### Adicionado
- references/ops.md: fluxo dev→local→github→hml→prd (nada direto no servidor; servidor imutável por edição manual), ops como interface única (100%, autônomo — usuário provisiona do zero sem IA), instalação paralela=nproc, independência invariante (falha no paralelo = serviços não independentes → prioridade máxima; nunca serializar pra mascarar).
- Comando /go-ops; pisos de ambientes/ops no CLAUDE.md; anti-padrões (editar no servidor, pular pra hml/prd, operar fora do ops, instalar serial, serializar pra mascarar); operacao.md §21 estendido; /go-load carrega ops.md.

## [1.4.0] — 2026-07-05
Todo MD gerado no archive, root limpo.

### Corrigido
- MAPA/índice saíam no root → agora `<projeto>_archive/index/` (padroes-codigo §4, MAPA.md, /go-index, build-index.mjs, CLAUDE.md, SKILL.md).

### Adicionado
- §28.0 (operacao.md): layout canônico do archive — todo MD gerado (MAPA, índices, planos, relatórios, handoffs) em `<projeto>_archive/<área>/`, NUNCA no root.

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
