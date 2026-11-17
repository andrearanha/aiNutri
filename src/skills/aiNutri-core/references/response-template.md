# Modelo de resposta — acompanhamento alimentar

Este modelo padroniza respostas de registro e avaliação no chat. Adapte os
blocos aos dados, ao pedido e às preferências do usuário; não preencha campos
ausentes nem imponha o modelo completo a perguntas simples. As regras de
análise, avaliação, contexto temporal e gravação continuam prevalecendo.

## Estrutura

1. **Refeição e data**, com horário quando conhecido e útil.
2. Para múltiplos itens, tabela compacta de alimento, porção e calorias, com
   total; para um único item, uma linha basta. Mostre outros nutrientes somente
   quando pertinentes ao plano, objetivo ou pedido e houver dados sustentados.
3. **Nota da refeição**, com farol, conceito e justificativa breve fornecidos
   pela avaliação. Identifique o escopo e não invente nota quando faltar base.
4. **Parcial do dia** ou, somente quando pedido ou confirmado, **fechamento**:
   total conhecido de energia, meta aplicável, barra de progresso, saldo ou
   excesso e nutrientes prioritários com metas e situação quando disponíveis.
   Identifique dados incompletos e estimativas relevantes. Se a parcial também
   tiver nota, identifique-a separadamente da nota da refeição.
5. Ressalva relevante, confirmação real do registro ou informação sobre falha
   e, quando útil e fundamentado, lembrete breve da próxima refeição.

## Barra de progresso energético

Ao apresentar a parcial ou o fechamento, inclua uma barra quando houver total
calórico diário conhecido e meta energética diária pontual, positiva e
aplicável no plano disponível. O preenchimento representa consumo ÷ meta,
limitado visualmente a 100%; arredonde a representação visual, não os valores
usados nos cálculos. Informe sempre consumo e meta em texto e, se houver excesso,
o valor acima da meta: barra cheia não significa que o consumo parou na meta.
Não trate esse progresso como nota numérica ou percentual de saúde ou aderência.

A cor do preenchimento acompanha o farol da **avaliação da parcial ou do dia**
para o mesmo escopo e estado dos dados: verde para Bom/Ótimo, amarelo para
Alerta, vermelho para Ruim e a representação de BOFETADA quando aplicável. Não
use automaticamente a nota da refeição como nota diária nem determine a cor
somente pelo percentual consumido ou por ultrapassar a meta. Uma parcial não é
um dia completo. Se não houver avaliação diária sustentada e atual, use barra
neutra e não atribua conceito. Indique o conceito por texto quando houver;
cor sozinha não comunica a avaliação.

Use 10 segmentos: 🟩, 🟨 ou 🟥 preenchidos conforme o farol, e ⬜ para o
restante; para BOFETADA, use 🟥 e identifique 👋 **BOFETADA** em texto. Sem nota
diária, use ◼ para preenchimento neutro e ◻ para o restante. Arredonde ao
segmento mais próximo; um consumo positivo pode aparecer sem preenchimento
quando muito pequeno. Os números em texto são exatos ou estimados conforme sua origem.
Quando a interface permitir, transforme a linha de segmentos em uma barra de progresso contínua, proporcional e colorida, sem expor HTML ou código ao usuário.

Sem meta pontual positiva, com meta apenas em faixa, ou sem total diário
confiável, não construa uma barra proporcional nem invente denominador. Se a
leitura dos registros do dia estiver incompleta, mostre o subtotal conhecido e a limitação,
sem representá-lo como total completo. Se apenas alguns itens ou nutrientes
estiverem pendentes e o subtotal diário for conhecido, uma barra pode representar
explicitamente esse subtotal provisório, com a exclusão indicada; não apresente
saldo definitivo. Em simulações, qualquer barra deve estar identificada como
projeção e separada do consumo confirmado.

## Exemplo fictício

Contexto: consumo diário aproximado de 1.000 kcal, meta aplicável de 2.000 kcal,
avaliação **da parcial** 🟢 Bom, mínimo diário de 100 g de proteínas e 70 g
conhecidos; gravação confirmada. Proteínas são apenas o nutriente prioritário
deste exemplo, não um padrão para todos os usuários.

> 🍽️ **Almoço — 14/09**
>
> | Alimento | Porção estimada | Calorias |
> | --- | --- | ---: |
> | Arroz, feijão e frango | porções estimadas | ~400 kcal |
> | **Total da refeição** | | **~400 kcal** |
>
> 🟢 **Bom — refeição:** composição favorável ao plano.
>
> **Parcial de 14/09:** ~1.000 kcal · Meta: 2.000 kcal
>
> 🟩🟩🟩🟩🟩⬜⬜⬜⬜⬜
>
> 🟢 **Bom — parcial do dia** · Restam ~1.000 kcal.
>
> **Proteínas:** ~70 g · Faltam ~30 g para o mínimo diário.
>
> ❗ **Atenção:** as porções foram estimadas pela foto.
>
> Refeição salva no seu Drive.

O exemplo é fictício; não reutilize seus valores como referência nutricional ou
meta. A nota da parcial deve ser obtida para esse escopo, não copiada da refeição.

## Cenários de revisão

| Situação | Resultado esperado |
| --- | --- |
| Consumo 1.354 kcal, meta 1.700 kcal, parcial avaliada como Bom | Barra verde com cerca de 80% preenchido; consumo, meta e 346 kcal restantes em texto. |
| Consumo 1.774 kcal, meta 1.700 kcal, dia avaliado como Alerta | Barra amarela cheia e +74 kcal acima da meta; não assumir fechamento só por ser jantar. |
| Nota da refeição verde e parcial sem avaliação | Barra neutra; não transferir a nota da refeição ao dia. |
| Macronutriente prioritário é carboidrato, não proteína | Mostrar o carboidrato pertinente e sua situação quando houver base; não incluir proteína por padrão. |
| Meta ausente, zero ou em faixa | Mostrar dados conhecidos e eventual limitação, sem barra proporcional ou meta inventada. |
| Total incompleto por item sem calorias | Identificar subtotal e item excluído; barra, se usada, é provisória e não há saldo definitivo. |
| Histórico indisponível | Não apresentar subtotal da conversa como total diário conferido nem desenhar barra diária proporcional. |
| Registro falhou ou resultado é incerto | Preservar análise possível e informar a situação real, sem alegar salvamento. |
