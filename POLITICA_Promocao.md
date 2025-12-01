# POLITICA_Promocao — dev → staging → prod

## Objetivo
Definir critérios claros e repetíveis para promover versões do sistema de controle de estoque entre ambientes, garantindo que cada entrega esteja validada pelo time e pelo cliente antes de chegar à produção.

## Critérios — dev → staging
- CI verde (lint, test, coverage ≥ 70%, SAST sem HIGH/CRITICAL)
- Funcionalidade implementada conforme os requisitos
- PR aprovado (2 reviewer técnico)
- Dados funcionando corretamente segundo o DER
- Interface validada com base no protótipo
- Validação do PO durante as reuniões semanais
- Tag de release candidate criada (`rc-<data>-<slug>`)
- Documentação atualizada

## Critérios — staging → prod
- Canary em staging/sandbox aprovado (smoke E2E ok)
- Erros 5xx < 1% e p95 < 300ms por 15 min
- Fluxos essenciais testados manualmente
    - cadastro/edição de produtos;
    - movimentação de entrada e saída;
    - alertas de estoque;
    - geração de relatórios.
- Confirmação de que não há inconsistências de dados
- Aprovação do PO
- Janela de mudança observada (fora de *freeze*)
- Plano de rollback baseado em backup automático revisado
- Versão considerada estável

## Janela / Freeze
- Publicações em produção preferencialmente entre**08h–12h** (dias úteis)
- *Change freeze* em períodos de alto navegação/solicitação conforme cada API retornar. 

## Evidências
- Registro das validações feitas com o PO nas reuniões semanais
- Anotações sobre ajustes feitos na modelagem (DER/C4) quando aplicável
- Links para execução da pipeline, dashboards de métricas/log
- Registro de aprovação da versão e decisão de promoção (issue/ADR).
