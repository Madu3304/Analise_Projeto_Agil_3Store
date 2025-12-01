# Pull Request — 3 Store

## Descrição
Foi definida uma estrutura simples de versionamento para garantir clareza e promover versões entre ambientes: duas branches de trabalho: dev-product-registration e dev-integracao (Com APIs) foram criadas além da main (produção). Cada branch representava um escopo específico e só podia ser integrada à main após cumprir o DoD definido. Ao final, as duas branches foram unificadas na main por meio de dois pull requests aprovados, seguindo critérios objetivos e repetíveis de revisão e qualidade.

## Checklist — Definition of Ready (DoR)
- [ ] História/issue com objetivo e critérios de aceitação claros
- [ ] Impacto/risco declarado (funcional / não-funcional)
- [ ] O sistema deve permitir cadastrar, editar e remover produtos
- [ ] O sistema deve monitorar o estoque, mostrando a quantidade disponível
- [ ] O sistema deve emitir alertas de níveis baixos de estoque
- [ ] Deve registrar entrada e saída de produtos em tempo real
- [ ] Deve gerenciar fornecedores
- [ ] Deve gerar relatórios detalhados de produtos
- [ ] Deve automatizar sugestões de reposição
- [ ] interface deve ser intuitiva
- [ ] dados devem ser seguros
- [ ] deve haver backup automático
- [ ] desempenho rápido mesmo com grande volume de dados
## Checklist — Definition of Done (DoD)
- [ ] testes unitários cobrindo
- [ ] testes unitários integração
- [ ] cadastro e edição de produtos funcionando
- [ ] monitoramento de estoque ativo
- [ ] movimentação de entrada/saída registrada corretamente
- [ ] relatórios gerados
- [ ] dados de fornecedores funcionando
- [ ] *lint* sem erros; cobertura ≥ **80%** 
- [ ] Protótipo do Figma compatível
- [ ] segurança básica dos dados
- [ ] Validação com o público beneficiado
- [ ] Cobertura global ≥ 80% (CI)
- [ ] Documentação curta (README/ADR/comentários)
- [ ] CI sinal verde e SAST sem HIGH/CRITICAL

## Quality Gates (bloqueio)
- [ ] Frequência de deploy 
- [ ] CI passou verde (lint + test + coverage)
- [ ] Consistência dos dados verificada com o modelo DER
- [ ] SAST sem HIGH/CRITICAL pendente
- [ ] 2 reviewer técnica de aprovação
- [ ] Funcionalidades revisadas pelo PO e aprovada 
- [ ] Interface conferida com o protótipo

## Risco e rollback
- [ ] Mudança possui plano de rollback simples
- [ ] Ajustes podem ser revertidos rapidamente por meio da restauração de dados

## Screenshots/Logs

