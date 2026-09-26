# Adão

**LLM open source pequena, eficiente e barata — com foco em reduzir alucinação em domínio específico.**

> Nome provisório. Pode mudar.

## Visão

A inovação não está no modelo — está na combinação. Modelos pequenos e baratos já existem prontos. O que ninguém resolveu de forma confiável é fazer um modelo pequeno **não alucinar** em um domínio específico.

O Adão é um projeto open source que parte de um modelo open-weight existente, aplica fine-tuning com **LoRA** em cima dos pesos congelados usando dados do nicho, e força **saída estruturada** — o modelo escolhe entre opções predefinidas em vez de gerar texto livre. Isso mata a maior parte da alucinação.

## Decisões já tomadas

| Decisão | Escolha | Por quê |
|---|---|---|
| Modelo base | **Qwen 3.5 9B** | Melhor all-rounder pequeno: ~82% no GPQA Diamond, roda com 6 GB de RAM, Apache 2.0, maior variedade de tamanhos da família (0.8B → 27B) |
| Alternativa | Gemma 4 12B | Mais forte na faixa 8–16 GB; Gemma 4 migrou para Apache 2.0 em abril/2026, então a diferença de licença com o Qwen sumiu |
| Hardware mínimo | 6–8 GB VRAM/RAM | Qwen 3.5 9B em Q4 cabe em notebook comum |
| Licença do projeto | Apache 2.0 | Permissiva: uso comercial, modificação, redistribuição sem restrição |
| Fine-tuning | LoRA | Treina só adaptadores pequenos sobre pesos congelados — barato, rápido, não apaga o conhecimento original |
| Redução de alucinação | Saída estruturada + RAG | Modelo escolhe entre opções em vez de texto livre; RAG restringe respostas a documentos fornecidos |

## Por que não criar do zero

Treinar uma LLM do zero custa milhões em GPU e anos de engenharia. O caminho realista é pegar um modelo open source como base e fazer fine-tuning. O Adão não reinventa a roda — ele resolve o problema que a roda ainda não resolveu bem.

## Plano de execução (rascunho)

### Fase 0 — Definição (agora)
- [ ] Escrever em uma frase o problema que o Adão resolve
- [ ] Definir o domínio/nicho de aplicação
- [ ] Definir o que é "acerto" e "alucinação" nesse domínio (métrica de avaliação)

### Fase 1 — Baseline
- [ ] Subir o Qwen 3.5 9B via Ollama/vLLM
- [ ] Montar um conjunto de prompts de teste do domínio (golden set)
- [ ] Medir taxa de alucinação do modelo base sem nenhuma adaptação

### Fase 2 — Dados e fine-tuning
- [ ] Coletar/gerar dados do nicho (pares pergunta → resposta correta)
- [ ] Fine-tune com LoRA (Hugging Face PEFT / Axolotl / Unsloth)
- [ ] Exportar o modelo adaptado (GGUF para Ollama, se necessário)

### Fase 3 — Saída estruturada
- [ ] Definir o schema de saída (JSON com campos fixos ou enum de opções)
- [ ] Implementar validação pós-geração (rejeitar saída fora do schema)
- [ ] Medir ganho de alucinação vs. baseline

### Fase 4 — RAG (opcional, se o domínio tiver documentos)
- [ ] Indexar documentos de referência (embeddings + vector store)
- [ ] Injetar contexto recuperado no prompt
- [ ] Medir se RAG reduz alucinação além do fine-tuning

### Fase 5 — Empacotamento open source
- [ ] README com instruções de reprodução
- [ ] Licença Apache 2.0
- [ ] Exemplos de uso e benchmarks públicos

## Stack prevista

- **Inferência local:** Ollama / vLLM / llama.cpp
- **Fine-tuning:** Hugging Face Transformers + PEFT (LoRA), ou Unsloth para velocidade
- **Avaliação:** golden set próprio + métricas de factualidade
- **RAG (fase 4):** sentence-transformers + ChromaDB/FAISS

## Status

🚧 Rascunho inicial — 26/09/2026. Nada implementado ainda.

## Licença

Apache 2.0 (a confirmar no momento do release).
