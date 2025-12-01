# Blueprint do Downstream — 3 Store

**Objetivo:** garantir **qualidade** e **previsibilidade de entrega** conectando *PDCA/Kaizen* e **maturidade leve** (repetibilidade).

## 1) Contexto do Projeto
- **Nome:** 3 Store
- **Stack:** JavaScript + Node.js (Express), Jest, ESLint
- **Repositório:** `3 Store`
- **Produto:** Software de controle de Estoque
- **Stakeholders:** PO, Eng. Líder, Time Dev, QA/Qualidade

## 2) Fluxo & Git (Previsibilidade do trabalho)
- **Trunk-Based leve** com *branches* curtas:
  - `main` (produção) · `dev` (PRODUCT_REGISTRATION) · `dev` (integração)
  - *feature branches:* `feat/<slug>`, `fix/<slug>`, `chore/<slug>`
- **Commits:** Conventional Commits (`feat:`, `fix:`, `docs:`, `chore:` …)
- **Pull Requests:**
  - **DoR (Definition of Ready):**
    - ✔️ Desenvolver um software de controle de estoque inteligente, com recursos administrativos e integração com APIs externas
    - ✔️ O sistema deve permitir cadastrar, editar e remover produtos
    - ✔️ O sistema deve monitorar o estoque, mostrando a quantidade disponível
    - ✔️ O sistema deve emitir alertas de níveis baixos de estoque
    - ✔️ Deve registrar entrada e saída de produtos em tempo real
    - ✔️ Deve gerenciar fornecedores
    - ✔️ Deve gerar relatórios detalhados de produtos
    - ✔️ Deve automatizar sugestões de reposição
    - ✔️ interface deve ser intuitiva
    - ✔️ dados devem ser seguros
    - ✔️ deve haver backup automático
    - ✔️ desempenho rápido mesmo com grande volume de dados
  - **DoD (Definition of Done):**
    - ✔️ testes unitários cobrindo
    - ✔️ testes unitários integração
    - ✔️ cadastro e edição de produtos funcionando
    - ✔️ monitoramento de estoque ativo
    - ✔️ movimentação de entrada/saída registrada corretamente
    - ✔️ relatórios gerados
    - ✔️ dados de fornecedores funcionando
    - ✔️ *lint* sem erros; cobertura ≥ **80%** 
    - ✔️ Protótipo do Figma compatível
    - ✔️ segurança básica dos dados
    - ✔️ documentação curta
    - ✔️ Validação com o público beneficiado
    - ✔️ CI verde e SAST sem **HIGH/CRITICAL**
- **Quality Gates (bloqueio de merge):**
  - CI *checks* verdes (lint + test + coverage)
  - cobertura global ≥ 80%
  - SAST sem `HIGH/CRITICAL` pendente
  - 1 reviewer técnico + PO quando impacta comportamento

## 3) Qualidade & Testes (Precisão e estabilidade)
- **Pirâmide de testes:**
  - **Unitários (70–80%)** com Jest 
  - **Integração (15–25%)** com testcontainers
  - **E2E smoke (5–10%)** no *staging*
- **Cobertura mínima:** 70% global e 80% nos módulos críticos (`/src/domain`)
- **Lint:** ESLint + Prettier
- **SAST:** CodeQL (GH) ou Semgrep OSS (bloqueante para HIGH/CRITICAL)
- **Quando bloqueiam:** CI falhou, SAST apontou HIGH/CRITICAL → sem merge

## 4) CI/CD & Ambientes (Fluxo visível)
- **Pipeline mínimo:** `install → lint → build → test manual funcional → integração com banco → revisão pelo PO → build/artifacts`
- **Artefatos:** Back-end Node.js/Express; Front-end React; scripts SQL do PostgreSQL; modelos e diagramas (C4 + DER) usados como artefatos de documentação
- **Critérios de promoção:**
  - **dev → staging:** DoD atendido; protótipo do Figma respeitado; testes manuais validados pelo time; CI verde; aprovação do PO
  - **staging → prod:** canário validado; confirmação de que as funcionalidades atendem às necessidades; erro 5xx < 1% e p95 latência < 300ms nos 15 min iniciais; interface funcionando conforme esperado
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
  - *request log* estruturado (produto, tipo de operação, quantidade, data/hora)
  - *error log* com *stacktrace* e *correlation-id*
- **Métricas essenciais:**
  - p95 latência HTTP; volume diário de entradas e saídas de produtos
- **DORA mínima (coleta mensal):**
  - *Lead time for changes* (tempo entre a definição de uma necessidade na reunião com o cliente → entrega validada)
  - *Change failure rate* (deploys com rollback / total de entregas)
- **Rotina PDCA/Kaizen (quinzenal):**
  - *Check:* revisar com o cliente e o time os problemas ocorridos, dados dos logs e necessidades identificadas
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
