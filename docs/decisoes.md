# Decisões do projeto Adão

Registro das decisões tomadas e do raciocínio por trás delas. Atualizar sempre que uma decisão mudar.

## 2026-09-26 — Escolha do modelo base: Qwen 3.5 9B

**Decisão:** partir do Qwen 3.5 9B (Alibaba) como modelo base.

**Alternativas consideradas:**
- Gemma 4 12B (Google) — mais forte em 8–16 GB; Gemma 4 (abril/2026) migrou para Apache 2.0, eliminando a restrição da licença antiga (ToU com Prohibited Use Policy, flow-down e kill switch remoto). Fica como alternativa se o hardware permitir.
- Phi-4 mini 3.8B (Microsoft, MIT) — melhor em raciocínio/matemática para hardware muito fraco; menor variedade de tamanhos.
- gpt-oss-20b (OpenAI, Apache 2.0) — MoE com só 3.6B ativos; rápido em notebook de 16 GB, mas menos maduro no ecossistema de fine-tuning.
- DeepSeek R1 Distill 7B (MIT) — forte em matemática/lógica; menos versátil para chat geral.

**Critérios:** qualidade por GB, licença permissiva (Apache 2.0), variedade de tamanhos para escalar sem trocar de família, ecossistema maduro de fine-tuning (Hugging Face, Ollama, vLLM).

**Resultado:** Qwen 3.5 9B atende todos. Gemma 4 12B é o plano B.

## 2026-09-26 — Estratégia: não treinar do zero

**Decisão:** usar modelo open-weight existente + LoRA + saída estruturada.

**Raciocínio:** treinar LLM do zero custa milhões em GPU e anos. LoRA treina só adaptadores pequenos sobre pesos congelados — barato, rápido, preserva o conhecimento original. Saída estruturada (escolha entre opções em vez de texto livre) elimina a maior parte da alucinação em domínio fechado. RAG como reforço quando houver documentos de referência.

## 2026-09-26 — Licença: Apache 2.0

**Decisão:** o projeto Adão será open source sob Apache 2.0.

**Raciocínio:** permissiva (uso comercial, modificação, redistribuição), sem limites de usuários ou receita, sem termos customizados que a Google possa alterar unilateralmente. Compatível com o Qwen 3.5 (Apache 2.0) e com o Gemma 4 (Apache 2.0 desde abril/2026).
