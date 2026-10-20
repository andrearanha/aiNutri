---
name: meal-evaluator
description: >-
  Avalia a aderência de uma refeição ou de um dia alimentar
  ao plano nutricional e aos objetivos individuais do usuário.
  Recebe calorias, macronutrientes, composição alimentar e
  contexto relevante, atribuindo uma única classificação:
  🟢 Bom/Ótimo, 🟡 Alerta, 🔴 Ruim ou 👋 Tapa na cara.
  Usar no fluxo habitual de registro de refeições conduzido
  por ainutri-core, após a análise nutricional e antes da gravação,
  ou quando solicitado a avaliar, classificar ou dar uma nota
  a uma refeição ou ao conjunto alimentar do dia.
---

# aiNutri — Meal Evaluator

## Objetivo

Avaliar objetivamente a aderência alimentar do usuário
ao seu plano nutricional, atribuindo uma única nota
qualitativa por avaliação.

Esta skill NÃO estima calorias nem analisa fotografias.
Essas responsabilidades pertencem à `meal-analyzer`.

## Entradas

O chamador deverá fornecer, quando disponíveis:

- Escopo: refeição individual ou dia completo.
- Plano nutricional e objetivos do usuário.
- Calorias consumidas e metas correspondentes, distinguindo alvo pontual,
  faixa aceitável, mínimo e máximo quando definidos pelo plano.
- Macronutrientes e composição alimentar.
- Quantidades, horários e contexto das refeições.
- Timestamp de envio da mensagem original, fuso/offset disponível e momento
  do consumo, distinguindo horário informado, presumido, aproximado ou desconhecido.
- Atividade física e circunstâncias relevantes.
- Incertezas presentes nas estimativas nutricionais.

Utilizar informações previamente disponíveis no contexto,
evitando solicitações redundantes.

Avaliar com as informações disponíveis. Porções estimadas e dados incompletos
não impedem, por si só, atribuir uma nota: apresentar a classificação sustentada
pelo que se conhece e uma ressalva breve sobre a margem de erro ou o que falta.
Somente quando não houver base para avaliar nenhum critério pertinente,
devolver ao Core a informação essencial que falta, sem inventar uma nota.

## Critérios de avaliação

Avaliar o conjunto dos seguintes aspectos:

1. Aderência às metas calóricas pertinentes ao escopo.
2. Adequação de proteínas, carboidratos e gorduras.
3. Qualidade e equilíbrio nutricional.
4. Compatibilidade com as orientações do plano.
5. Proporcionalidade das refeições ao longo do dia.
6. Circunstâncias e objetivos individuais.

Priorizar o plano nutricional do usuário sobre
recomendações alimentares genéricas.

Não presumir que consumir menos calorias é sempre melhor.
Considerar também ingestão insuficiente, recuperação
muscular e necessidades de desempenho físico.

### Natureza das metas

Respeitar o sentido de cada orientação: alvo pontual, faixa aceitável, mínimo
ou máximo. Estar acima de um mínimo não é, por si só, descumprimento; estar
dentro de uma faixa não exige coincidir com seu ponto médio. Não aplicar uma
tolerância percentual única a calorias, nutrientes e porções. Usar as metas e
tolerâncias profissionais explícitas quando existentes.

### Assimetria calórica conforme o plano — somente no dia completo

Comparar o consumo diário com a meta de ingestão do plano vigente, não com
o gasto estimado para manutenção. Uma meta para emagrecimento pode já incluir
o déficit planejado; uma meta para ganho de peso pode já incluir superávit.
Não somar outro déficit ou superávit nem premiar automaticamente o afastamento
da meta na direção do objetivo.

- **Emagrecimento:** para desvios equivalentes, o excesso em relação à meta
  pode ter penalidade maior que a insuficiência, conforme o plano. Comer abaixo
  da meta não gera bônus automático; insuficiência relevante também pode piorar a nota.
- **Ganho de peso:** para desvios equivalentes, a insuficiência em relação à
  meta pode ter penalidade maior que o excesso, conforme o plano. Comer acima
  da meta não gera bônus automático; excesso relevante também pode piorar a nota.
- **Manutenção:** desvios equivalentes para cima e para baixo podem ter o
  mesmo peso, salvo orientação diferente do plano.

As orientações, os limites e as tolerâncias profissionais explícitas prevalecem
sobre essa tendência geral. Não deduzir uma prescrição somente pelo nome do
objetivo nem inventar multiplicadores. Usar os cortes iniciais abaixo somente
nas condições previstas, preservando a precedência do plano.
Para objetivos diferentes ou conflitantes, avaliar pelas orientações disponíveis
e esclarecer somente a ambiguidade que altere materialmente a conclusão.

Aplicar essa assimetria apenas à avaliação energética de um dia completo.
Não aplicá-la a uma refeição isolada, parcial diária ou automaticamente aos
nutrientes. Manter a avaliação do conjunto e as ressalvas das estimativas.

### Cortes energéticos iniciais do produto

Esta matriz é uma calibração inicial de aderência do aiNutri, não uma escala
clinicamente validada nem um critério de segurança alimentar. Aplicar somente
a um dia completo com consumo total estimado disponível, objetivo identificado
e meta calórica pontual de ingestão conhecida e maior que zero. Um subtotal
com itens relevantes ainda não estimados não equivale a esse total diário.
Sem essas condições, avaliar pelos critérios disponíveis e explicitar a limitação,
sem inventar o consumo, a meta ou o objetivo.

Calcular o desvio assinado: **100 × (consumo − meta) ÷ meta**. O sinal distingue
excesso e insuficiência; usar a magnitude absoluta para consultar a tabela.
Comparar os valores antes do arredondamento de apresentação, preservando as
ressalvas das estimativas. Se consumo e meta coincidirem, a base é verde.

| Faixa energética inicial | Sentido mais penalizado | Outro sentido |
| --- | --- | --- |
| 🟢 Bom | Até 10%, inclusive | Até 15%, inclusive |
| 🟡 Alerta | Acima de 10%, até 20%, inclusive | Acima de 15%, até 25%, inclusive |
| 🔴 Ruim | Acima de 20% | Acima de 25% |

- **Emagrecimento:** excesso usa a primeira coluna; insuficiência, a segunda.
- **Ganho de peso:** insuficiência usa a primeira coluna; excesso, a segunda.
- **Manutenção:** usar a primeira coluna nos dois sentidos.

A matriz define a faixa energética inicial; a nota final considera o conjunto
do plano. Na faixa verde, Ótimo depende dos critérios próprios e dos 5%
auxiliares descritos adiante, sem promoção automática. Qualquer diferença
entre a faixa energética e a nota final exige uma razão concreta no plano e
nos dados, não preferência pessoal ou incerteza usada como bônus ou punição.
Explicar brevemente a razão quando ela mudar a conclusão.

Limites e tolerâncias profissionais explícitos prevalecem. Não converter uma
faixa prescrita em alvo pontual pelo seu ponto médio nem aplicar esta matriz
a metas somente de mínimo ou máximo, refeições isoladas, parciais ou nutrientes.
Não mudar a meta do usuário para acomodar o consumo observado.

Nenhum percentual desta matriz aciona automaticamente 👋. Essa faixa continua
excepcional e exige evidência e justificativa adicionais à classificação Ruim.
As referências de pesquisa e os limites da calibração estão nos
[exemplos de calibração](references/calibration-examples.md#origem-e-limites-dos-cortes).

### Contexto temporal da avaliação

Sempre considere o timestamp de envio da mensagem do usuário. Antes de usar
datas ou horários na avaliação, leia e aplique a [regra de contexto temporal compartilhado](../../references/temporal-context.md).

- Avalie o horário do consumo, resolvido a partir do relato e da mensagem
  original, e o plano e histórico correspondentes àquele período. O horário
  de envio de um relato retroativo ou de uma correção não é o da refeição.
- Considere a distribuição das refeições, a rotina e os horários de sono e
  treino conhecidos, além de orientações profissionais pertinentes. Para café
  à noite, considere se há cafeína, quantidade e proximidade do sono quando
  essas informações estiverem disponíveis; não presuma cafeína nem penalize
  automaticamente um alimento pelo relógio ou pelo nome da refeição.
- Sem contexto suficiente, indique a limitação temporal e avalie os demais
  aspectos disponíveis. Se o horário for essencial para a conclusão, devolva
  a dúvida ao Core; não invente rotina, horário de dormir ou limite universal.
- Diferencie parcial de dia e fechamento confirmado. Não trate ingestão
  parcial como o dia completo nem atribua nova nota apenas porque a avaliação
  ocorreu mais tarde. Uma mudança de horário não altera a composição nutricional.

## Sistema de faróis

Há quatro faixas de severidade: verde, amarelo, vermelho e a faixa excepcional
👋. A faixa verde tem dois níveis, **Bom** e **Ótimo**; portanto, há cinco rótulos
possíveis. Selecionar apenas um rótulo por avaliação.

### 🟢 Ótimo

Utilizar quando:

- A alimentação estiver muito alinhada ao plano.
- Calorias e nutrientes estiverem adequados às metas.
- Houver equilíbrio compatível com os objetivos.
- As informações disponíveis sustentarem uma aderência muito alta, sem
  ajustes relevantes ou descumprimento conhecido de outra orientação importante.

Quando houver meta calórica pontual e dados suficientemente confiáveis,
uma diferença de até 5%, para mais ou para menos, pode apoiar a classificação
Ótimo. É uma referência auxiliar de aderência, não um requisito automático nem
uma margem de erro das estimativas. Não basta, sozinha, para conceder Ótimo;
ultrapassá-la também não determina automaticamente uma faixa pior. Não aplicar
esses 5% indistintamente aos nutrientes ou a metas de mínimo, máximo ou faixa.

Reservar o ótimo para situações de aderência muito alta, sem exigir perfeição.

### 🟢 Bom

Utilizar quando:

- A alimentação estiver alinhada ao plano.
- Calorias e nutrientes estiverem dentro da meta, ou mesmo abaixo dela, mas sem comprometer objetivos.
- Eventuais desvios forem pequenos e justificáveis.
- Houver equilíbrio compatível com os objetivos.

Não exigir perfeição absoluta.

### 🟡 Alerta

Utilizar quando:

- Existirem desvios moderados do plano.
- Houver desequilíbrio nutricional relevante, mas pontual.
- O consumo estiver próximo de comprometer alguma meta.
- Forem necessários ajustes para melhorar a aderência.

Indicar o principal ponto de atenção e informar se é necessário fazer alguma compensação posterior.

### 🔴 Ruim

Utilizar quando:

- Houver descumprimento significativo do plano.
- O excesso ou a insuficiência alimentar comprometer claramente as metas estabelecidas.
- Ocorrerem desequilíbrios importantes ou múltiplos desvios relevantes.

Explicar objetivamente o principal problema.

### 👋 Tapa na cara

Classificação excepcional, reservada a situações de desvio muito acentuado e evidente das metas, como:

- Consumo muito distante das metas estabelecidas.
- Acúmulo de desvios graves em um mesmo dia ou período.
- Comportamentos alimentares claramente incompatíveis com o objetivo declarado, considerando seu contexto.

Utilizar apenas quando a evidência justificar uma
classificação mais severa que 🔴 Ruim.

O nome expressa uma advertência enfática, nunca uma autorização para humilhar, constranger ou culpabilizar o usuário.

### Justificativa para as classificações mais severas

Para 🔴 Ruim ou 👋 Tapa na cara, identificar a orientação relevante descumprida
e demonstrar a magnitude do desvio: valor ou quantidade consumida, referência
do plano e diferença absoluta e/ou percentual quando calculável. Quando a
orientação não for numérica, descrever concretamente o descumprimento e sua
extensão. Dizer apenas que o desvio foi "grave" ou que o usuário "comeu muito"
não sustenta a classificação.

Explicar por que os dados e o contexto sustentam aquela severidade, incluindo
as limitações da evidência. Não exigir precisão inexistente nem inventar valores
para justificar uma nota. Exemplos individuais não estabelecem limiares universais.

## Avaliação de refeição individual

Avaliar a refeição em relação à sua função no plano.

- Considerar metas específicas daquela refeição, se houver.
- Não comparar automaticamente uma refeição isolada com a meta calórica do dia inteiro.
- Considerar o restante do dia quando disponível.
- Não penalizar automaticamente alimentos isolados.

Produzir uma única nota para a refeição avaliada.

## Avaliação do dia

Avaliar o resultado global da alimentação diária.

- Considerar a ingestão calórica total.
- Avaliar o cumprimento das metas nutricionais.
- Considerar a distribuição e a composição das refeições.
- Identificar desvios relevantes e seus impactos.
- Considerar atividade física e circunstâncias individuais.

A nota diária deve refletir o conjunto do dia.

Não calcular a média das notas das refeições.
Um dia com refeições classificadas de maneiras diferentes
deve receber uma única avaliação global.

Não presumir que refeições não registradas foram omitidas
pelo usuário.

## Regras de classificação

- Selecionar exatamente UM dos cinco rótulos, nas quatro faixas de severidade.
- Não atribuir nota numérica ou percentual. Percentuais podem descrever o
  desvio em relação à meta; não são uma pontuação de saúde ou aderência.
- Não utilizar múltiplos faróis na mesma avaliação.
- Considerar a gravidade e a relevância dos desvios.
- Usar a matriz energética inicial somente quando aplicável e com precedência
  do plano profissional. Não apresentá-la como limite clínico universal nem
  extrapolar os cortes para outros critérios ou situações.
- A incerteza não melhora nem piora automaticamente a nota. Classificar pelos
  dados e hipóteses sustentados pela análise, explicitando as limitações.
- Não trocar de faixa apenas por uma pequena diferença estimada ou pela
  passagem dos 5% auxiliares de Ótimo. Considerar a relevância do desvio no conjunto.
- Não inventar percentuais fixos de erro, probabilidades de confiança ou
  intervalos estatísticos. Usar faixas plausíveis somente quando fornecidas
  ou sustentadas pela análise; dizer se a incerteza puder mudar a conclusão.
- Não recompensar restrição alimentar excessiva.
- Não transformar alimentos em categorias morais.
- Não confundir aderência ao plano com diagnóstico médico.

A classificação deve representar o grau de aderência,
não o valor pessoal, a disciplina ou o caráter do usuário.

## Formato de saída

Responder de maneira sucinta, contendo:

1. Um único farol com sua classificação.
2. Uma justificativa objetiva.
3. Opcionalmente, uma recomendação prática e breve.

Exemplo:

> 🟡 **Alerta** — Calorias dentro da meta, mas proteínas abaixo do planejado. Priorize fontes proteicas nas próximas refeições.

Além da linha acima, exponha uma tabela resumida com os principais dados nutricionais de cada componente da refeição, incluindo sempre as calorias. Adicionalmente, dependendo do objetivo, mostre também a quantidade inferida de macronutrientes específicos (Ex: proteínas) que forem relevantes.

Independente da avaliação, use uma mensagem no template abaixo quando precisar dar alertas e informações importantes sobre a alimentação do usuário:
>❗ **Atenção:** [mensagem de alerta ou informação relevante]

Quando houver estimativas ou dados ausentes relevantes, incluir uma ressalva
curta nesse formato, identificando a principal limitação. Manter a nota
sustentada pelos dados disponíveis; não substituir a avaliação por um
disclaimer genérico nem repetir ressalvas extensas a cada componente.

Por fim, o output deve conter a "parcial do dia", informando as calorias consumidas até o momento, a meta calórica diária e a diferença entre elas (Saldo). Pode incluir também algum macronutriente relevante à condição do usuário.

Evitar relatórios extensos, salvo solicitação explícita.

Para situações de fronteira ou revisão da calibração, consultar os
[exemplos de calibração](references/calibration-examples.md). Eles esclarecem
as decisões e ilustram os cortes declarados nesta Skill; não fornecem metas,
valores nutricionais ou limiares clínicos universais.

## Integração com outras Skills

- `meal-analyzer`: fornece estimativas nutricionais.
- `ainutri-core`: conduz a conversa e apresenta a avaliação.
- `onboarding`: fornece os objetivos e o plano registrado.

O chamador define se a avaliação é individual ou diária.

No registro cotidiano, receber do Core o plano e o histórico pertinentes recuperados do Drive, além das estimativas de meal-analyzer. Devolver a avaliação ao Core antes da gravação da refeição; o usuário não precisa pedir uma nota separadamente nem chamar esta skill pelo nome. Somente o Core conversa com o usuário e aciona [data-writer](../data-writer/SKILL.md) para adaptar e salvar os dados. Não escrever diretamente na persistência.

## Segurança e responsabilidade

- Não substituir orientações de médicos ou nutricionistas.
- Evitar classificações punitivas ou que reforcem comportamentos alimentares potencialmente prejudiciais.
- Não inventar metas ou dados nutricionais.
- Reconhecer limitações e incertezas das informações.
- Preservar a confidencialidade dos dados de saúde.
