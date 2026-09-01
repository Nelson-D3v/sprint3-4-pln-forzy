# Documento Final — Sprints 3 e 4: Inteligência Operacional e Integração da Solução PLN

**Challenge FIAP — Forzy | Digital-twin para manutenção preditiva de motores elétricos industriais**

## 1. Visão Geral

Este documento consolida os dois módulos de PLN entregues:

- **Sprint 3**: geração automática de resumos de alertas, classificação textual de eventos e
  relatório operacional em linguagem natural com rastreabilidade.
- **Sprint 4**: pipeline RAG sobre a documentação técnica do ativo, integrado a um assistente
  conversacional de troubleshooting que usa o contexto operacional do Sprint 3.

Notebooks: [`sprint3_pln_alertas.ipynb`](../notebooks/sprint3_pln_alertas.ipynb) e
[`sprint4_pln_rag.ipynb`](../notebooks/sprint4_pln_rag.ipynb).

## 2. Templates de Narrativa de Alertas

Três templates parametrizados por nível de severidade (`leve`, `moderado`, `crítico`), com
vocabulário e tom escalonados:

| Nível | Tom | Ação sugerida no texto |
|---|---|---|
| Leve | informativo, neutro | monitoramento contínuo |
| Moderado | atenção, recomendação | inspeção em até 48h |
| Crítico | urgente, imperativo | prioridade máxima, avaliar parada imediata |

Cada resumo é composto a partir dos campos estruturados do alerta: motor, sensor, valor medido,
baseline, desvio, timestamp e recomendação associada ao subsistema (mapeada por tipo de sensor).

## 3. Critérios de Classificação de Eventos

Taxonomia (5 categorias mutuamente exclusivas): `manutenção corretiva`, `manutenção preventiva`,
`anomalia elétrica`, `anomalia mecânica`, `operação normal` — detalhada em
[`ficha_manutencao_geral.md`](../data/manuals/ficha_manutencao_geral.md), seção 2.

Duas implementações são fornecidas no notebook: classificação zero-shot com
`facebook/bart-large-mnli` (quando há acesso a rede/GPU) e um classificador de regras
determinístico como fallback offline, permitindo o cálculo de F1-score por categoria em
qualquer ambiente de execução.

## 4. Arquitetura do Assistente RAG

```
[Manuais técnicos .md] --chunking por seção--> [chunks + metadados]
        --> [SentenceTransformer multilingual] --> [embeddings]
        --> [FAISS IndexFlatIP] --> [índice vetorial]

Pergunta do operador --embedding--> busca top-k --> re-ranking (score semântico +
   overlap lexical + bônus por motor_id do estado operacional atual) --> top-k final

[Contexto operacional (Sprint 3)] + [Histórico da conversa] + [Chunks recuperados]
        --> prompt --> LLM (API ou local via Ollama; fallback extrativo se ausente)
        --> resposta com citação de fonte + nível de confiança
```

**Persona do sistema**: assistente técnico especialista em motores elétricos, instruído a
responder apenas com base nos documentos recuperados, citar fonte, indicar confiança e
reconhecer perguntas fora de escopo (prompt completo no notebook, seção 4).

**Memória de curto prazo**: histórico da conversa (últimos turnos) é injetado no prompt a cada
nova pergunta, permitindo diálogos de troubleshooting com múltiplos turnos.

## 5. Métricas Obtidas

Execução realizada no Google Colab (CPU, sem GPU dedicada), sobre os dados sintéticos fornecidos.

| Métrica | Onde é calculada | Resultado |
|---|---|---|
| F1-macro (classificação, zero-shot `bart-large-mnli`) | `sprint3_pln_alertas.ipynb`, seção 2 | **0,120** |
| ROUGE-1 / ROUGE-2 / ROUGE-L (resumos de alerta) | `sprint3_pln_alertas.ipynb`, seção 4.1 | **0,646 / 0,485 / 0,614** |
| Context precision (retriever, top-3, nível documento) | `sprint4_pln_rag.ipynb`, seção 3.1 | **94,7%** |
| Answer relevancy (heurística de similaridade) | `sprint4_pln_rag.ipynb`, seção 6 | **0,580** |
| Faithfulness (heurística de overlap lexical) | `sprint4_pln_rag.ipynb`, seção 6 | **0,809** |

**Classificação de eventos — detalhe por categoria (zero-shot, 20 alertas):**

| Categoria | Precision | Recall | F1-score | Suporte |
|---|---|---|---|---|
| manutenção corretiva | 0,00 | 0,00 | 0,00 | 0 |
| manutenção preventiva | 0,21 | 1,00 | 0,35 | 4 |
| anomalia elétrica | 1,00 | 0,14 | 0,25 | 7 |
| anomalia mecânica | 0,00 | 0,00 | 0,00 | 4 |
| operação normal | 0,00 | 0,00 | 0,00 | 5 |
| **Acurácia geral** | | | **0,25** | 20 |

O F1-macro de 0,120 é baixo e esperado: o `bart-large-mnli` não foi treinado no domínio de
manutenção industrial e, sem GPU, foi avaliado apenas com o hypothesis template genérico
("Este evento é um caso de {}"), sem calibração adicional. O modelo tende fortemente a rotular
tudo como "manutenção preventiva" (recall de 1,00 nessa classe, mas precision de apenas 0,21),
evidenciando baixa capacidade discriminativa entre as 5 categorias nesse cenário zero-shot.
Como mitigação documentada no notebook, o classificador de regras determinístico (fallback)
serve de baseline mais confiável para este domínio específico — recomenda-se, como evolução,
fine-tuning supervisionado ou poucos exemplos rotulados (few-shot) em vez de zero-shot puro.

Já o ROUGE dos resumos textuais (9 alertas críticos/moderados avaliados contra referências
manuais) mostra forte sobreposição de conteúdo com o texto de referência (ROUGE-1 = 0,646,
ROUGE-L = 0,614), indicando que os templates de narrativa capturam corretamente os elementos
essenciais (motor, sensor, valores, desvio e recomendação) na mesma estrutura frasal usada nas
referências — resultado esperado, já que os resumos são gerados por template determinístico
(não por um LLM livre), o que favorece alta sobreposição lexical com textos de referência
escritos no mesmo padrão.

**Assistente conversacional — detalhe por categoria (20 perguntas de troubleshooting):**

| Categoria | Answer relevancy | Faithfulness |
|---|---|---|
| anomalia_eletrica | 0,614 | 0,801 |
| anomalia_mecanica | 0,557 | 0,835 |
| manutencao_preventiva | 0,539 | 0,797 |
| geral | 0,732 | 0,797 |
| fora_de_escopo (Q20) | 0,123 | 0,821 |

A faithfulness ficou consistentemente alta (~0,80 em todas as categorias, inclusive fora de
escopo), confirmando que o assistente ancora suas respostas nos chunks recuperados e não
"inventa" termos fora do contexto retornado — mesmo no fallback extrativo (sem LLM real). A
answer relevancy geral (0,580) é razoável mas variável por categoria: mais alta em perguntas
"gerais" (0,732, respostas mais diretas e curtas) e mais baixa em "fora_de_escopo" (0,123, Q20 —
esperado, já que a resposta de referência fala sobre preço de mercado, informação ausente nos
manuais, então a resposta do assistente diverge semanticamente da referência por definição, não
por falha). O resultado confirma o comportamento desejado: o assistente reconhece a pergunta
fora de escopo e não tenta respondê-la com valores inventados, ainda que isso penalize a métrica
de similaridade textual com a referência.

Recomenda-se, como evolução do projeto, substituir as heurísticas de faithfulness/relevancy por
avaliação com **RAGAS** usando um LLM-juiz, quando houver orçamento de API disponível — o que
tende a separar melhor "resposta correta mas fora de escopo reconhecido" de "resposta errada".

## 6. Limites Identificados e Estratégias de Mitigação

- **Alucinação**: mitigada por prompt restritivo (responder só com base no contexto recuperado),
  exigência de citação de fonte e auditoria pós-hoc via heurística de faithfulness.
- **Perguntas fora de escopo** (ex.: preço de mercado do motor): reconhecidas via score
  semântico baixo do retriever, sinalizando confiança "baixa" e resposta explícita de limite.
- **Cobertura documental desigual**: manual detalhado apenas para o MT-042; demais motores
  cobertos pela ficha geral da frota — mitigação futura: um manual por motor.
- **Re-ranking heurístico**: combinação simples de score semântico + overlap lexical + bônus por
  motor; evolução recomendada: cross-encoder dedicado para re-ranking.

## 7. Cenários de Demonstração

Três cenários demonstrados em `sprint4_pln_rag.ipynb`, seção 5, e no vídeo de demonstração:

1. **Anomalia elétrica** — MT-042, desvio de temperatura do enrolamento.
2. **Anomalia mecânica** — MT-017, vibração radial crítica.
3. **Consulta a procedimento de manutenção preventiva** — MT-005, cronograma e frequência de
   inspeção de isolamento.

## 8. Estrutura de Entrega

```
Processamento/
├── data/
│   ├── alerts_raw.csv                    # alertas simulados (entrada Sprint 3)
│   ├── alerts_processed.csv              # saída Sprint 3 (resumos + classificação)
│   ├── alerts_reference_summaries.csv    # referências para ROUGE
│   ├── troubleshooting_qa.json           # 20 perguntas com resposta de referência
│   └── manuals/                          # documentação técnica (base do RAG)
├── notebooks/
│   ├── sprint3_pln_alertas.ipynb
│   └── sprint4_pln_rag.ipynb
└── docs/
    ├── documento_final.md                # este documento
    └── relatorio_operacional_semanal.md  # gerado pelo notebook Sprint 3
```

## 9. Vídeo de Demonstração

[youtu.be/CRiJkBjhDLg](https://youtu.be/CRiJkBjhDLg) (YouTube, não listado) — demonstração do
assistente conversacional nos três cenários de falha descritos na seção 7.
