# Roteiro do Vídeo de Demonstração (3-5 min)

**Formato:** gravação de tela do Colab, focada exclusivamente na célula da seção 5
("Demonstração em Três Cenários de Falha") do `sprint4_pln_rag.ipynb`, já executada.
Narração ao vivo, sem cortes.

---

## 0:00 – 0:15 | Abertura

*(Mostre a célula da seção 5 antes de rolar para a saída)*

> "Este é o assistente conversacional de troubleshooting do digital-twin de motores elétricos
> industriais. Ele usa RAG sobre manuais técnicos, combinado com o contexto operacional do
> equipamento. Vou mostrar os três cenários de falha testados."

---

## 0:15 – 1:35 | Cenário 1: Anomalia Elétrica (MT-042)

*(Role até o bloco de saída "=== Cenário: Anomalia elétrica (MT-042) ===")*

> "Primeiro cenário: anomalia elétrica no motor MT-042. A pergunta é: 'A temperatura do
> enrolamento subiu bastante acima do baseline, o que devo fazer?'"

*(Aponte para "Contexto operacional injetado")*

> "Antes de responder, o assistente recebe o histórico recente de alertas desse motor — aqui,
> um alerta moderado e um crítico de temperatura do enrolamento, vindos do processamento do
> Sprint 3."

*(Aponte para "Resposta do assistente")*

> "A resposta vem embasada no manual técnico do motor, cita a fonte exata — a seção de
> Especificações Gerais — e indica o nível de confiança, que aqui é alto."

---

## 1:35 – 2:55 | Cenário 2: Anomalia Mecânica (MT-017)

*(Role até o bloco "=== Cenário: Anomalia mecânica (MT-017) ===")*

> "Segundo cenário: anomalia mecânica no motor MT-017, com vibração radial crítica. A pergunta
> é sobre quais passos de inspeção seguir."

*(Aponte para a resposta)*

> "O assistente recupera o procedimento de inspeção de vibração do manual — os passos de
> medição, verificação de alinhamento e inspeção de rolamentos — citando a seção exata como
> fonte, novamente com confiança alta."

---

## 2:55 – 4:15 | Cenário 3: Manutenção Preventiva (MT-005)

*(Role até o bloco "=== Cenário: Manutenção preventiva (MT-005) ===")*

> "Terceiro cenário: consulta a procedimento de manutenção preventiva do motor MT-005 — o
> cronograma recomendado e a frequência de medição do isolamento."

*(Aponte para a resposta)*

> "A resposta traz o cronograma mensal, trimestral e anual descrito no manual técnico,
> novamente citando a seção de origem — mostrando que o assistente recupera e organiza
> informação real, sem inventar nada."

---

## 4:15 – 4:45 | Fechamento

> "Esses três cenários mostram o assistente respondendo com base na documentação técnica real,
> sempre citando a fonte e o nível de confiança, e considerando o estado operacional atual do
> equipamento. Essa é a demonstração do assistente de troubleshooting do digital-twin. Obrigado!"

---

## Dicas práticas de gravação

- **Antes de gravar**: garanta que a célula da seção 5 já foi executada no Colab, para rolar
  direto pela saída pronta, sem esperar processamento durante a gravação.
- **Ferramenta**: Windows + `Win G` (Xbox Game Bar) grava tela + microfone; ou OBS Studio para
  mais controle.
- **Zoom**: aumente o zoom do navegador (Ctrl + "+") antes de gravar, para o texto da saída
  ficar legível em vídeo.
- **Tempo total**: os blocos somam ~4:45min, dentro do limite de 3 a 5 minutos.
- **Ritmo**: role a tela lentamente enquanto fala, dando tempo para quem assiste acompanhar o
  texto da saída — não precisa ler tudo em voz alta, só apontar os pontos-chave (pergunta,
  contexto injetado, fonte citada, confiança).
