# Modelo de resposta — acompanhamento alimentar

Este modelo padroniza respostas de registro e avaliação no chat. Sempre inclua
feedback alimentar breve e a parcial com barra e saldo quando houver dados
suficientes, inclusive em respostas curtas, complementos e correções. O usuário
não precisa solicitar esses elementos. Adapte detalhes e extensão às preferências,
sem omitir o essencial nem inventar dados. Perguntas simples sem registro ou
avaliação não exigem o modelo completo. As regras de análise, avaliação,
contexto temporal e gravação continuam prevalecendo.

Seja o mais enxuto possível: apresente a nota e o motivo concreto em uma frase
curta. Não mostre análises adicionais, tabelas nutricionais detalhadas ou
explicações extensas sem solicitação. Informações necessárias à compreensão ou
à segurança devem ser comunicadas de forma sucinta, com fundamento disponível.
Isso não dispensa a análise interna necessária à avaliação e ao registro.

## Estrutura

1. **Refeição e data**, com horário quando conhecido e útil.
2. Composição resumida e total de calorias em uma linha. Não apresente tabela
   detalhada por alimento ou nutriente sem pedido; mostre outros nutrientes
   somente quando pedidos ou necessários para decisão, compreensão ou segurança.
3. **Nota da refeição**, com farol, conceito e justificativa breve fornecidos
   pela avaliação. Identifique o escopo e não invente nota quando faltar base.
4. **Parcial do dia** ou, somente quando pedido ou confirmado, **fechamento**:
   total conhecido de energia, meta aplicável, barra de progresso, saldo ou
   excesso e, quando necessário para uma decisão, nutriente prioritário e sua situação.
   Identifique dados incompletos e estimativas relevantes. Se a parcial também
   tiver nota, identifique-a separadamente da nota da refeição.
5. Ressalva relevante, confirmação real do registro ou informação sobre falha
   e, quando útil e fundamentado, lembrete breve da próxima refeição.

## Barra de progresso energético

Em cada resposta de registro ou avaliação de refeição, apresente a parcial do
dia, inclusive quando a gravação estiver pendente ou falhar, identificando a
situação. Na parcial e no fechamento, inclua sempre uma barra quando houver total
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
Todo o preenchimento tem uma única cor, definida pelo farol da parcial ou do dia.
O trecho ainda não preenchido é apenas fundo neutro; nunca use uma segunda cor
de farol para indicar saldo, ultrapassagem ou faixas de calorias. O excesso é
mostrado em texto, com a barra cheia na única cor correspondente à avaliação.
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
avaliação **da parcial** 🟢 Bom e gravação confirmada.

> 🍽️ **Almoço — 14/09**
>
> Arroz, feijão e frango · ~400 kcal.
>
> 🟢 **Bom — refeição:** combina proteína do frango e fibras do feijão.
>
> **Parcial de 14/09:** ~1.000 kcal · Meta: 2.000 kcal
>
> 🟩🟩🟩🟩🟩⬜⬜⬜⬜⬜
>
> 🟢 **Bom — parcial do dia** · Restam ~1.000 kcal.
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
| Consumo acima da meta, parcial avaliada como Ruim | Barra inteira vermelha; não desenhar trecho verde até a meta e vermelho para o excesso. |
| Preferência por resposta curta ou primeiro registro do dia | Incluir feedback breve e parcial com barra e saldo quando houver dados, sem esperar pedido. |
| Nota da refeição verde e parcial sem avaliação | Barra neutra; não transferir a nota da refeição ao dia. |
| Nutriente prioritário relevante para uma decisão | Informar apenas esse nutriente e sua situação quando houver base, sem tabela detalhada. |
| Usuário pede detalhamento nutricional | Apresentar os nutrientes solicitados com valores disponíveis e incertezas pertinentes. |
| Meta ausente, zero ou em faixa | Mostrar dados conhecidos e eventual limitação, sem barra proporcional ou meta inventada. |
| Total incompleto por item sem calorias | Identificar subtotal e item excluído; barra, se usada, é provisória e não há saldo definitivo. |
| Histórico indisponível | Não apresentar subtotal da conversa como total diário conferido nem desenhar barra diária proporcional. |
| Registro falhou ou resultado é incerto | Preservar análise possível e informar a situação real, sem alegar salvamento. |
