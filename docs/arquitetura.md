# Arquitetura do Adão (rascunho)

## Visão geral

```
[Usuário] → [API/CLI] → [Adão: Qwen 3.5 9B + LoRA]
                              ↓
                    [Validador de schema]
                              ↓
              [Resposta estruturada ou rejeição]
                              ↓
              [RAG (fase 4): contexto de documentos]
```

## Componentes

1. **Modelo base:** Qwen 3.5 9B, quantizado em 4 bits (Q4_K_M) para caber em ~6 GB.
2. **Adaptadores LoRA:** treinados por domínio; os pesos base ficam congelados e intocados.
3. **Validador de saída:** rejeita qualquer resposta fora do schema definido (JSON Schema / enum). Sem texto livre fora do contrato.
4. **RAG (fase 4):** embeddings locais + vector store; injeta só trechos recuperados dos documentos de referência.

## O que NÃO faz parte (por enquanto)

- Treinamento do zero
- Modelos fechados (API paga) como dependência
- Fine-tuning full-parameter

## Decisões em aberto

- [ ] Domínio/nicho de aplicação
- [ ] Formato exato do schema de saída
- [ ] Se RAG entra na v1 ou fica para v2
- [ ] Interface: CLI, API REST ou biblioteca Python
