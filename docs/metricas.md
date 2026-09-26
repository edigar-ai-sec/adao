# Métricas do Adão (rascunho)

Como medir se o Adão realmente reduz alucinação:

## Métrica principal

**Taxa de alucinação** = respostas factualmente incorretas / total de respostas, em um golden set fixo do domínio.

## Métricas secundárias

- **Cobertura:** % de perguntas do golden set que recebem resposta (em vez de recusa)
- **Latência:** tokens/segundo em hardware de referência (ex.: RTX 4060 8 GB)
- **Custo de treino:** horas de GPU para o LoRA convergir
- **Tamanho do artefato:** GB do modelo adaptado em Q4

## Baseline

Antes de qualquer adaptação, rodar o Qwen 3.5 9B puro no golden set e registrar a taxa de alucinação. Tudo que vier depois é ganho em cima desse número.

## Golden set

- [ ] Definir 50–100 perguntas do domínio com resposta correta conhecida
- [ ] Versionar o golden set no repositório (JSON)
- [ ] Atualizar só com justificativa (mudança de domínio = novo conjunto)
