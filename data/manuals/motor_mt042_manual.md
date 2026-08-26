# Manual Técnico — Motor Elétrico Industrial MT-042 (Linha Trifásica IE3, 75 kW, 380V)

## 1. Especificações Gerais

O motor MT-042 é um motor de indução trifásico de alta eficiência (classe IE3), potência nominal
de 75 kW, tensão nominal 380V, corrente nominal 128A, rotação nominal 1780 RPM, grau de
proteção IP55, classe de isolamento F (temperatura máxima admissível de enrolamento: 155°C,
com elevação limite de 80°C sobre ambiente de 40°C). O baseline operacional de temperatura do
enrolamento em regime nominal é de aproximadamente 60°C.

## 2. Sistema de Refrigeração

O MT-042 utiliza refrigeração forçada (IC411) por meio de ventilador acoplado ao eixo. O sistema
depende de fluxo de ar desobstruído nas grades de admissão e exaustão da carcaça.

### 2.1 Procedimento de Verificação do Sistema de Refrigeração

1. Desligar o motor e aguardar resfriamento (mínimo 15 minutos).
2. Inspecionar visualmente as grades de admissão e exaustão quanto a acúmulo de poeira,
   obstruções ou danos nas palhetas do ventilador.
3. Limpar as aletas da carcaça com ar comprimido de baixa pressão (máx. 2 bar).
4. Verificar folga do rolamento do ventilador; substituir se houver ruído ou folga excessiva.
5. Religar o motor e monitorar a temperatura do enrolamento por 30 minutos, comparando com o
   baseline de 60°C. Um desvio acima de +15°C após a limpeza indica possível problema
   estrutural na refrigeração (motor subdimensionado para a carga, obstrução interna) ou
   sobrecarga elétrica — nesse caso, escalar para inspeção elétrica (seção 4).

## 3. Anomalias Térmicas — Diagnóstico e Ação

| Desvio de Temperatura do Enrolamento | Classificação | Ação Recomendada |
|---|---|---|
| até +10°C acima do baseline | leve | Monitorar; registrar em log |
| +10°C a +20°C acima do baseline | moderado | Verificar sistema de refrigeração (seção 2.1) |
| acima de +20°C acima do baseline | crítico | Parar o motor; inspecionar refrigeração E carga elétrica (sobrecarga, desbalanceamento de fases) |

**Atenção:** temperaturas de enrolamento acima de 140°C (elevação de +80°C sobre o baseline de
60°C) exigem parada IMEDIATA do motor para evitar dano permanente ao isolamento classe F.

## 4. Sistema Elétrico e Isolamento

A resistência de isolamento nominal (medida entre enrolamento e carcaça, com megômetro a 500V)
deve ser igual ou superior a 5 MΩ. Valores abaixo de 1 MΩ indicam risco iminente de curto-circuito
para a carcaça e exigem desligamento imediato do motor até nova medição após secagem/limpeza
do enrolamento.

### 4.1 Procedimento de Inspeção do Isolamento

1. Desenergizar completamente o motor e aplicar bloqueio (LOTO).
2. Desconectar os cabos de alimentação nos terminais do motor.
3. Medir a resistência de isolamento com megômetro (500V DC) entre cada fase e a carcaça
   aterrada.
4. Se resistência < 5 MΩ: verificar umidade/contaminação nos enrolamentos; secar em estufa
   a baixa temperatura (máx 90°C) se aplicável.
5. Se resistência < 1 MΩ: NÃO reenergizar. Encaminhar para rebobinamento ou substituição.
6. Registrar valores medidos no relatório de manutenção corretiva.

### 4.2 Anomalias de Corrente

Corrente de fase acima de 10% do valor nominal de forma sustentada indica sobrecarga mecânica
(carga acoplada excessiva) ou desbalanceamento de tensão de alimentação. Verificar a carga
mecânica acoplada antes de qualquer intervenção elétrica.

## 5. Sistema Mecânico — Vibração e Rolamentos

O limite de vibração aceitável para o MT-042 (conforme ISO 10816-3, classe de máquina rígida)
é de 1.0 mm/s RMS em operação nominal (baseline). Valores entre 1.0 e 2.0 mm/s indicam alerta
moderado; acima de 2.0 mm/s indicam alerta crítico, com risco de falha de rolamento ou
desalinhamento severo.

### 5.1 Procedimento de Inspeção de Vibração

1. Com o motor em operação, medir vibração radial e axial nos mancais dianteiro e traseiro.
2. Se vibração acima do baseline: verificar alinhamento do acoplamento com relógio comparador
   ou laser de alinhamento (tolerância máxima 0.05mm).
3. Verificar fixação da base do motor (parafusos de ancoragem) e desbalanceamento do rotor.
4. Inspecionar rolamentos quanto a ruído, folga radial excessiva e temperatura (limite: 45°C
   baseline, alerta acima de 70°C).
5. Se folga excessiva ou ruído metálico constante: substituir rolamento na próxima janela de
   manutenção programada; se vibração crítica (>2.0mm/s) com ruído: parada imediata.

## 6. Manutenção Preventiva — Cronograma

- **Mensal:** limpeza de grades de ventilação, inspeção visual geral, medição de vibração.
- **Trimestral:** medição de resistência de isolamento, lubrificação de rolamentos (graxa de
  lítio complexo, conforme especificação do fabricante).
- **Anual:** rebalanceamento se necessário, revisão completa de alinhamento, substituição
  preventiva de rolamentos com mais de 20.000 horas de operação.

## 7. Segurança

Todas as intervenções elétricas ou mecânicas exigem bloqueio e etiquetagem (LOTO) e uso de
EPIs adequados (luvas isolantes classe 0, óculos de proteção). Nunca realizar medições elétricas
com o motor energizado sem equipamento de proteção adequado (classe de arco elétrico
apropriada).
