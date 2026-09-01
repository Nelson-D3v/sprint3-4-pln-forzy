# Sprints 3 e 4 — PLN | Challenge FIAP Forzy

Digital-twin para manutenção preditiva de motores elétricos industriais.

## Como executar

1. Abra os notebooks no Google Colab ou Jupyter local (pasta `notebooks/`), executando
   `sprint3_pln_alertas.ipynb` primeiro (gera `data/alerts_processed.csv`, consumido pelo
   Sprint 4) e depois `sprint4_pln_rag.ipynb`.
2. Instale as dependências indicadas na primeira célula de cada notebook (`pip install`).
3. (Opcional) Para respostas geradas por LLM real no assistente do Sprint 4, defina a variável
   de ambiente `OPENAI_API_KEY` ou habilite `USE_OLLAMA=True` com um modelo local rodando. Sem
   isso, o notebook usa um gerador extrativo de fallback, mantendo o pipeline 100% executável.

## Estrutura

- `data/` — alertas simulados (`alerts_raw.csv`), saída processada do Sprint 3
  (`alerts_processed.csv`), resumos de referência para ROUGE, manuais técnicos (base do RAG) e o
  gabarito de 20 perguntas de troubleshooting.
- `notebooks/` — os dois notebooks Colab da entrega.
- `docs/` — documento final unificado, relatório operacional gerado e o vídeo de demonstração
  (link).

Ver [`docs/documento_final.md`](docs/documento_final.md) para arquitetura, métricas obtidas e
limites do sistema.

## Resultados obtidos

| Métrica | Resultado |
|---|---|
| F1-macro (classificação de eventos, zero-shot) | 0,120 |
| ROUGE-1 / ROUGE-2 / ROUGE-L (resumos de alerta) | 0,646 / 0,485 / 0,614 |
| Context precision (retriever RAG, top-3) | 94,7% |
| Answer relevancy (assistente conversacional) | 0,580 |
| Faithfulness (assistente conversacional) | 0,809 |

## Vídeo de demonstração

[youtu.be/CRiJkBjhDLg](https://youtu.be/CRiJkBjhDLg) — demonstração do assistente de
troubleshooting nos três cenários de falha (anomalia elétrica, anomalia mecânica e manutenção
preventiva).
