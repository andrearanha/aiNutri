# Contexto temporal compartilhado

Aplicar esta regra em todas as skills que interpretam, analisam, avaliam ou
persistem informações com datas e horários. O Core reúne esse contexto e o
repassa às skills internas; cada uma preserva as referências recebidas.

## Referência da mensagem

**Sempre considere o timestamp de envio da mensagem do usuário como um
metadado relevante do contexto, quando disponibilizado pela plataforma.**
Use o timestamp da mensagem original à qual o relato pertence, inclusive ao
retomar um histórico. O instante da resposta, da análise ou da gravação não
substitui o instante de envio. A data de uma foto, arquivo ou documento também
não comprova quando um alimento foi consumido.

- Preserve no contexto o timestamp original e seu fuso ou offset quando disponíveis. Use
  o fuso conhecido do usuário para interpretar o horário local; não presuma um
  fuso pelo idioma, pela localização do servidor ou pelo horário da ferramenta.
- Se a plataforma não fornecer o timestamp ou o fuso necessário, mantenha
  essa limitação explícita. Não invente metadados nem use o relógio atual como
  se fosse o horário da mensagem. Aproveite datas e horários informados pelo
  usuário; pergunte apenas quando a informação ausente for necessária para
  identificar o dia ou resolver uma contradição relevante. A hora pode ficar pendente.

## Momento do consumo

Distinga **envio da mensagem**, **consumo** e **gravação/atualização**.
Esses momentos podem pertencer a dias diferentes.

1. Datas e horários explicitamente informados para o consumo prevalecem sobre
   a suposição de que ocorreu no envio da mensagem. Preserve aproximações como
   “por volta das 22h”; não acrescente minutos ou segundos inexistentes.
2. Resolva “agora”, “hoje”, “ontem” e “há 40 minutos” a partir do timestamp da
   respectiva mensagem, no fuso conhecido do usuário. Considere a mudança de
   dia, mês e ano. Não recalcule essas expressões pelo momento de uma releitura.
3. Quando não houver horário, intervalo relativo ou “agora” informado para o
   consumo, use o timestamp confiável da mensagem original e o fuso disponível
   para inferir consumo recente, aproximadamente no horário local da mensagem,
   sem pedir confirmação. Registre a aproximação e sua origem inferida, sem
   apresentá-la como horário informado. Se houver indício de relato retroativo,
   preserve a data informada e não copie o horário da mensagem para outro dia;
   deixe a hora desconhecida quando não houver outra referência para ela.
   Com vários consumos, considere o contexto de cada um; não atribua um mesmo
   horário quando o relato indicar momentos distintos. Pergunte somente diante
   de contradição ou ambiguidade relevante para o dia ou para uma decisão; não
   confirme por rotina um horário plausível, inclusive se o consumo foi agora.
   Sem referência confiável, mantenha a hora pendente; não use o relógio da
   ferramenta como substituto. Se o usuário corrigir a inferência, use a correção.
   Sem dia identificável, não atribua o evento a um dia nem grave no diário até
   esclarecê-lo. A falta apenas de horário não impede o registro autorizado.
4. Preserve a parte conhecida quando faltar precisão. Um horário sem data ou
   fuso suficiente não autoriza escolher silenciosamente um dia ou conversão.
   As skills internas devolvem dúvidas essenciais ao Core.

## Correções, totais e fechamento

- Uma correção posterior mantém o momento do consumo original, salvo quando
  o usuário estiver corrigindo justamente esse momento. Não é necessário persistir os instantes da mensagem ou da gravação.
- Um complemento pode pertencer à mesma refeição e ter outro horário ou dia
  de consumo. Consolide cada item no dia correspondente, sem transferir os
  itens anteriores para a data da nova mensagem.
- Use o período de consumo para recuperar o histórico e calcular parciais e
  avaliações. Se uma correção mudar o dia, atualize os dois períodos afetados.
- Horário noturno, jantar ou passagem da meia-noite não confirmam fechamento.
  Feche somente o período pedido ou confirmado pelo usuário; registros tardios
  podem atualizar um dia já fechado. Ausência de registro não comprova jejum.

## Cenários de aceitação

Dados fictícios para revisão das decisões, sem exigir redação idêntica.

| Contexto e relato | Resultado esperado |
| --- | --- |
| Mensagem em `2026-09-15T00:10:00-03:00`: “Banana há 40 minutos; iogurte agora” | Banana aproximadamente às 23h30 de 14/09 e iogurte com referência presumida às 00h10 de 15/09; preservar a origem e a aproximação de cada horário. |
| A mesma mensagem só é analisada ou gravada em 16/09 | Manter os consumos de 14 e 15/09; o instante de processamento não muda suas datas. |
| Timestamp `2026-09-15T03:10:00Z`, fuso do usuário `America/Sao_Paulo`: “agora” | Usar 15/09 às 00h10 no horário local, preservando a origem e o fuso usado. |
| Mensagem em 15/09 às 08h: “Ontem tomei café às 21h” | Avaliar o consumo em 14/09 às 21h com a rotina conhecida; não usar 08h nem atribuir farol automaticamente pelo horário. |
| Correção enviada em 16/09 para jantar de 14/09: “Era sem pele” | Manter a data do jantar; sem relançar consumo em 16/09. |
| “Esse jantar foi em 13/09, não 14/09” | Corrigir o evento existente e revisar os totais e avaliações dos dois dias afetados, sem duplicação. |
| “Comi há 40 minutos”, sem timestamp disponível e com dia indefinido | Não calcular pelo relógio atual; Core pede apenas a referência necessária, e writer não escolhe um dia arbitrário. |
| Timestamp em UTC, fuso do usuário desconhecido e conversão pode mudar o dia | Pedir o fuso ou a referência local necessária; não presumir São Paulo pelo idioma. |
| Meia-noite, jantar ou pedido “feche ontem” | Os dois primeiros não fecham o dia; no pedido explícito, resolver “ontem” pelo timestamp da mensagem e fechar apenas esse período. |
| Exame de 10/09 enviado em 15/09 | Preservar 10/09 como data do exame e 15/09 como referência do envio, sem substituir uma pela outra. |
| Mensagem local às 12h: “Almocei arroz”, sem horário informado | Registrar aproximadamente 12h, com origem inferida do timestamp, sem perguntar. Se corrigido, usar o horário informado. |
| “Almocei ontem”, mensagem às 12h | Preservar a data de ontem; deixar a hora desconhecida, sem copiar 12h nem exigir confirmação. |
| Relato de consumo recente, sem timestamp confiável, mas com dia conhecido | Registrar no dia conhecido com hora pendente, sem inventar horário nem perguntar apenas para preencher o campo. |
| Relato contraditório que impede identificar o dia | Perguntar somente a referência necessária antes de registrar. |
