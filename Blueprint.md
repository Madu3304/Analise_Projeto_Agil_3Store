# Blueprint do Downstream — Acme Books API

**Objetivo:** garantir **qualidade** e **previsibilidade de entrega** conectando *PDCA/Kaizen* e **maturidade leve** (repetibilidade).

---

## 1) Contexto do Projeto
- **Nome:** Acme Books API
- **Stack:** TypeScript + Node.js (Express), Jest, ESLint, Docker
- **Repositório:** `acme-books-api` (fictício)
- **Produto:** API de catálogo e pedidos de livros
- **Stakeholders:** PO, Eng. Líder, Time Dev, QA/Qualidade, SRE

## 2) Fluxo & Git (Previsibilidade do trabalho)
- **Trunk-Based leve** com *branches* curtas:
  - `main` (produção) · `staging` (homologação) · `dev` (integração)
  - *feature branches:* `feat/<slug>`, `fix/<slug>`, `chore/<slug>`
- **Commits:** Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:` …)
- **Pull Requests:**
  - **DoR (Definition of Ready):**
    - ✔️ item no backlog com objetivo claro e critérios de aceitação
    - ✔️ impacto/risco declarado
  - **DoD (Definition of Done):**
    - ✔️ testes unitários cobrindo regra central
    - ✔️ *lint* sem erros; cobertura ≥ **70%** (fase inicial)
    - ✔️ documentação curta (README/ADR ou comentários)
    - ✔️ CI verde e SAST sem **HIGH/CRITICAL**
- **Quality Gates (bloqueio de merge):**
  - CI *checks* verdes (lint + test + coverage)
  - cobertura global ≥ 70% (ajustável com maturidade)
  - SAST sem `HIGH/CRITICAL` pendente
  - 1 reviewer técnico + PO quando impacta comportamento

## 3) Qualidade & Testes (Precisão e estabilidade)
- **Pirâmide de testes:**
  - **Unitários (70–80%)** com Jest + supertest (quando aplicável)
  - **Integração (15–25%)** com testcontainers/docker
  - **E2E smoke (5–10%)** no *staging*
- **Cobertura mínima:** 70% global e 80% nos módulos críticos (`/src/domain`)
- **Lint:** ESLint + Prettier (bloqueante)
- **SAST:** CodeQL (GH) ou Semgrep OSS (bloqueante para HIGH/CRITICAL)
- **Quando bloqueiam:** CI falhou, SAST apontou HIGH/CRITICAL → sem merge

## 4) CI/CD & Ambientes (Fluxo visível)
- **Pipeline mínimo:** `install → lint → test → coverage → SAST → build/artifacts`
- **Artefatos:** pacote Docker `acme-books-api:<git-sha>`
- **Critérios de promoção:**
  - **dev → staging:** DoD ok; CI verde; tag `rc-*` criada
  - **staging → prod:** canário aprovado; erro 5xx < 1% e p95 latência < 300ms nos 15 min iniciais
- **Versionamento:** SemVer (`vX.Y.Z`); `main` é produção; `rc-*` em `staging`

## 5) Releases & Operação (Risco controlado)
- **Estratégia:** *Canary Release* (10% → 50% → 100%) com *feature flags*
- **Gatilhos de rollback (qualquer um):**
  - erro 5xx ≥ **2%** por 5 min
  - p95 latência ≥ **600ms** por 5 min
  - taxa de sucesso E2E smoke < **95%**
- **Rollback:** voltar para *image* anterior; desligar *flags*; comunicar status

## 6) Observabilidade & DORA (Repetibilidade)
- **Logs essenciais:**
  - *request log* estruturado (método, rota, status, latência)
  - *error log* com *stacktrace* e *correlation-id*
- **Métricas essenciais:**
  - p95 latência HTTP; taxa de erros 5xx/min
- **DORA mínima (coleta mensal):**
  - *Lead time for changes* (PR aberto → *merge*)
  - *Change failure rate* (deploys com rollback / total)
- **Rotina PDCA/Kaizen (quinzenal):**
  - *Check:* revisar DORA e incidentes
  - *Act:* 1–2 *action items* padronizados (ex.: novo gate, ajuste de alarme)

## 7) Riscos e Mitigações
- **Migração de schema:** *expand/contract*, *backward compat*, *flags*
- **Dependências externas:** *timeouts*; *circuit breaker*
- **Segurança:** SAST, dependabot/renovate; varredura de *secrets*

---

## Apêndice A — Checklist de Entrega
- [ ] PR Template referenciando DoD e status do pipeline
- [ ] Gates: lint ok, SAST ok, cobertura ≥ 70%
- [ ] Critérios dev→stg→prod objetivos
- [ ] Estratégia de release definida + gatilhos de rollback
- [ ] 2 logs + 2 métricas definidos; DORA mínima & rotina PDCA

*(Documento gerado em 2025-11-10.)*
