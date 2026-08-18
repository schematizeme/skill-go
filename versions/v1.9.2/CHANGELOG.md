# Changelog — schematize-go

## [1.9.2] — 2026-08-18
Correção da contradição do muro pré-login de IAM (alinha ao `iam.md` da schematize-engineering).
### Mudado
- **/go-iam**: removido o "2º fator forte obrigatório antes do acesso pleno" e o "força 2º fator no 1º login" — o muro pré-login / deadlock de bootstrap VETADO pela norma. Agora senha+Email OTP = 2FA baseline; fator forte é nudge + step-up just-in-time.


Formato: [Keep a Changelog]; versionamento: SemVer.

## [1.9.1] — 2026-08-18
Q.A. repointado para a skill dedicada **schematize-qa**.
### Mudado
- **`/go-qa` virou wrapper fino** da **schematize-qa** (`/qa-plan` → `/qa-run`) no recorte Go (`go test -race`). Referências ao antigo **§22.9** (Q.A. plan-first, agora extraído para a schematize-qa) removidas de `SKILL.md`, `references/testes*.md`, `assets/CLAUDE.md` e `/go-help`.

## [1.9.0] — 2026-08-15
Correção de desenho no IAM backend — **senha + Email OTP já é 2FA baseline** (o middleware/PEP não barra mais o login) + **risk engine adaptativo robusto**.

### Mudado (correção de piso)
- **`references/iam.md` §3/§4/§7 + roadmap + checklist:** senha + Email OTP conta como **2FA baseline** desde o cadastro; o **middleware/PEP libera o acesso baseline** e exige AAL alto **só por rota sensível** (step-up just-in-time, `403 step_up_required`), **nunca barra o login** por falta de fator forte — fim do **círculo infinito**. Fator forte é **nudge + just-in-time**. Migração ativa o Email OTP always-on como 2º fator baseline (sem muro).

### Adicionado
- **Autenticação adaptativa por risco (robusta)** (`references/iam.md` §9): log de sessões/tentativas + **score** (IP/ASN, device novo, geovelocidade, velocity, honeypot); **escalonamento 2FA→3FA** sob risco (senha → email → app/chave); **negação deceptiva/tarpit** (falso negativo sob suspeita — resposta idêntica ao erro real em **tempo constante**, estado "próxima passa" curto/escopado a conta+IP+device, soma-se ao 3FA, nunca trava o legítimo); **honeypot**; notifica login suspeito + "não fui eu".

## [1.8.0] — 2026-07-11
IAM por desenho (recorte Go) — identidade + autorização robustas, auth como microserviço Go separado.

### Adicionado
- **`references/iam.md`** — piso de IAM da casa no recorte **backend/Go**: **auth é app SEPARADA** — **microserviço Go** `<projeto>_auth_go` + front próprio (`<projeto>_authfront`) em `auth.<domain>`, isolados; nunca monolith (nem `internal/auth`); apps delegam por OIDC/OAuth2.1 + PKCE e validam por **JWKS público** (assinatura só no auth). **ID interno imutável (ULID/UUIDv7) — email/telefone nunca é ID** (N emails por usuário como entidades filhas com `VerifiedAt`; SSO com recuperação local forçada; nudge de email secundário com detecção de provedor + tooltip). **≥2 fatores sempre** (AAL/NIST 800-63B): **passkey/WebAuthn no núcleo** (`go-webauthn`), TOTP (`pquerna/otp`)/push, **email OTP Resend always-on inclusive HML**, **Twilio** p/ telefone — providers como **interfaces Go plugáveis** (`EmailProvider`/`SmsProvider`/`PushProvider`); senha por padrão (**argon2id** via `x/crypto/argon2` + HIBP) mas opcional no seletor; invariante de troca "fator Y≠X no maior AAL"; **recuperação ≥ força do login**. **Multi-tenant RBAC/ABAC granular via ReBAC** (OpenFGA/SpiceDB via client): deny-default, PDP=Check API / **PEP=middleware Go**, enforcement server-side, token fino, decisão auditada, escrita de tupla via outbox. **Multi-dispositivo** + view de remover (session store Postgres+Redis, `refresh_family_id`/`device_id`/`jti`); **sessão 7d/90d** (fim do "15 min e é chutado") com refresh rotativo silencioso; **logout irreversível** (revoga refresh+família, apaga sessão server-side, `jti` na denylist consultada em toda request). **Migração de auth legado = prioridade 0** (strangler-fig, re-hash preguiçoso → argon2id, re-deriva authz). Rotina agressiva de testes cross-tenant/priv-esc (schematize-pentest).
- **Comando `/go-iam`** (plan-first): força/audita/scaffolda o IAM num projeto (bootstrap) ou porta um auth legado (migrate).
- **Piso 16** no `CLAUDE.md`; bullet + linha na tabela de references do `SKILL.md`; anti-padrões **43–46** (auth monolith; email como ID / 1 fator; authz hand-rolled/no cliente; logout que só apaga cookie); `/go-load` carrega `iam.md`; `/go-help` lista `/go-iam`.

## [1.7.0] — 2026-07-11
Limite de arquivo em camadas — teto de 750 (≤500 úteis + ~250 comentário) + flag em >300 úteis.

### Alterado
- **`references/padroes-codigo.md` §1/§2:** o limite rígido de **300 linhas/arquivo** vira regra **em camadas**. **Teto DURO: 750 linhas** (das quais **~250 reservadas a comentário/doc** e **até ~500 de código útil**) — acima bloqueia. **FLAG (não bloqueia, mas SEMPRE sinaliza) em > 300 linhas de código útil:** indício de que a função está **muito extensa** / **precisa de mais abstração** — registra como dívida e **revê quando as prioridades forem resolvidas**. **Observabilidade tem folga natural (~400 úteis).** Função com >300 úteis dispara o mesmo flag; "uma função por arquivo" mantida.
- **`scripts/check-diff.sh`:** o gate de tamanho passa a contar **código útil** (exclui comentário/branco): `total > 750` **bloqueia**, `útil > 500` **bloqueia**, `útil > 300` (ou `> 400` em arquivo de observabilidade) **flagueia** (`warn`, não trava).
- Propagado no piso do `CLAUDE.md`, `SKILL.md`, `references/entrega.md` (DoD), `references/arquitetura.md` (§6) e comandos `/go-load` `/go-help` `/go-review`.

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
