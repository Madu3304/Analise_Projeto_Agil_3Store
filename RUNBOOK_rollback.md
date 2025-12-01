# RUNBOOK — Rollback de Release (Canário) — 3 Store

## Quando executar (gatilhos)
- Falhas após atualização de funcionalidades de estoque (APIs)
- Erro 5xx ≥ 2% por 5 min
- p95 latência ≥ 600ms por 5 min
- Sucesso E2E smoke < 80%
- Feedback imediato do cliente

## Pré-requisitos
- Notificar o PO e o cliente
- Acesso ao banco PostgreSQL
- Acesso ao orquestrador (K8s/ECS) e observabilidade (logs/métricas)
- Última versão estável (tag `v*`) identificada

## Passos
1. **Congelar tráfego canário**
   - Reduza peso do canário para 0%
   - Suspender o uso da nova funcionalidade
2. **Reverter versão**
   - Restaurar backup automático mais recente
   - Reimplantar a versão anterior do back-end e front-end
   - Verificar se o banco de dados esteja coerente com o DER anterior ao update.
3. **Verificar saúde**
   - Monitore 10 min: erros 5xx < 1%, p95 < 300ms
   - Testar, junto ao cliente
   - Registro de entrada e saída
   - Monitoramento do estoque
4. **Comunicação**
   - Notificar o PO e o cliente sobre a reversão
   - Registrar no relatório interno
5. **Follow-up (PDCA)**
   - Registrar causa provável e ações: hotfix
   - Atualizar documentação (Historico)
   - Definir ações de melhoria

## Notas
- Migrações de banco devem seguir estratégia *expand/contract* com compatibilidade reversa.
- Nunca executar *roll forward* sem restaurar estabilidade primeiro.
- A reversão deve garantir que o estoque permaneça consistente.
- Jamais seguir com uma nova versão até que o cliente (PO) valide totalmente.
*(Documento atualizado em 2025-11-29.)*
