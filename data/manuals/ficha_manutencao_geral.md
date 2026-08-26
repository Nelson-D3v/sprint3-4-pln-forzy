# Ficha de Manutenção — Frota de Motores Elétricos Industriais (MT-005, MT-011, MT-017, MT-029, MT-042)

## 1. Visão Geral da Frota

A frota monitorada é composta por 5 motores de indução trifásicos de potências entre 45kW e
90kW, todos equipados com sensores de temperatura de enrolamento, temperatura de rolamento,
vibração (radial e axial), corrente de fase e resistência de isolamento, com telemetria em
tempo real integrada à plataforma de digital-twin.

| Motor | Potência | Aplicação | Baseline Temp. Enrolamento | Baseline Vibração |
|---|---|---|---|---|
| MT-005 | 55 kW | Bomba centrífuga | 60°C | 1.0 mm/s |
| MT-011 | 45 kW | Compressor | 58°C | 1.1 mm/s |
| MT-017 | 90 kW | Ventilador industrial | 62°C | 1.0 mm/s |
| MT-029 | 55 kW | Esteira transportadora | 59°C | 1.2 mm/s |
| MT-042 | 75 kW | Bomba de processo | 60°C | 1.0 mm/s |

## 2. Classificação de Eventos (Taxonomia para Classificação Textual)

Para fins de categorização automática de eventos registrados no sistema, utilizam-se as
seguintes categorias, mutuamente exclusivas:

1. **manutenção corretiva** — intervenção após falha ou alerta crítico já manifestado
   (ex.: substituição de rolamento danificado, rebobinamento por falha de isolamento).
2. **manutenção preventiva** — intervenção programada ou motivada por desvio leve/moderado
   antes de falha (ex.: lubrificação, limpeza, ajuste de alinhamento).
3. **anomalia elétrica** — desvio em parâmetros elétricos: temperatura de enrolamento,
   corrente de fase, resistência de isolamento.
4. **anomalia mecânica** — desvio em parâmetros mecânicos: vibração (radial/axial),
   temperatura de rolamento.
5. **operação normal** — parâmetros dentro da faixa esperada (desvio leve, sem ação requerida
   além de monitoramento).

## 3. Procedimento Padrão de Resposta a Alertas

### 3.1 Alerta Leve
Registrar em log de monitoramento. Nenhuma ação de campo é necessária, exceto reforço da
frequência de leitura do sensor afetado nas próximas 24h.

### 3.2 Alerta Moderado
Gerar ordem de serviço de verificação (não corretiva) a ser executada em até 48h. Consultar o
manual específico do motor (ex.: motor_mt042_manual.md) para o procedimento de inspeção do
subsistema afetado (refrigeração, isolamento, vibração).

### 3.3 Alerta Crítico
Gerar ordem de serviço de manutenção corretiva com prioridade máxima. Avaliar necessidade de
parada imediata do equipamento conforme os limites de segurança descritos no manual técnico
específico do motor. Notificar o responsável de manutenção e registrar o evento no relatório
semanal de equipamentos em risco.

## 4. Rastreabilidade

Todo relatório de estado operacional gerado automaticamente deve referenciar o `alert_id` e o
`sensor` de origem de cada afirmação, permitindo auditoria e correlação com os dados brutos de
telemetria armazenados no histórico do digital-twin.
