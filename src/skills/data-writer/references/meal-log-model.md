# Modelo mínimo do diário de refeições

Este contrato é para `meal-log.gsheet`, recurso nativo na pasta autorizada. Para
criação inicial, use uma planilha vazia com as cinco abas e apenas os cabeçalhos
abaixo, uma linha de cabeçalho por aba e nenhuma linha de exemplo. Crie e leia
abas e células com as ferramentas disponíveis; se não suportarem essas operações,
a criação fica pendente. Os nomes são canônicos para novos diários; equivalentes
legados podem ser mapeados conforme [data-writer](../SKILL.md).

## Convenções

IDs são textos únicos e estáveis, gerados antes da primeira tentativa, sem dados
pessoais, conferidos contra o diário e reutilizados em retomadas. Não dependem de
número de linha. Em conflito ou dúvida sobre identidade, pare e esclareça. Datas
locais usam `AAAA-MM-DD`; horas locais usam `HH`, `HH:MM` ou texto fiel como
`por volta das 22h`, sem acrescentar precisão. Fuso é identificador IANA ou offset
informado/conhecido; ausência fica vazia. Não converter horário sem base.

Célula vazia significa desconhecido ou não aplicável, nunca zero. A pendência e
seu estado explicam o que falta; zero numérico só representa valor conhecido zero.
Campos numéricos são números, sem unidade no valor. Uma faixa usa limites mínimo
e máximo; não preencher valor central sem fonte. Textos de origem e incerteza
acompanham o dado correspondente. Identificadores e textos são gravados como
texto literal, sem interpretar conteúdo do usuário como fórmula.

## Abas e cabeçalhos

Cada linha representa uma entidade; referências apontam para IDs, não posições.
Nenhuma linha de cabeçalho é consumo. Um consumo é contado uma vez, mesmo com
vários nutrientes ou alterações. A refeição pode agrupar consumos de dias distintos.

### `Refeicoes`

| Campo | Tipo e significado |
| --- | --- |
| refeicao_id | Texto, identidade estável da refeição. |
| nome | Texto, nome ou tipo informado, como almoço; pode estar vazio. |
| observacoes | Texto, contexto e pendências da refeição. |

### `Consumos`

Uma linha por item ou evento consumido; um complemento novo recebe outro ID e
aponta para a mesma refeição. Correção mantém o ID do item afetado.

| Campo | Tipo e significado |
| --- | --- |
| consumo_id | Texto, identidade estável do consumo. |
| refeicao_id | Texto, vínculo obrigatório com `Refeicoes`. |
| data_consumo | Data local do consumo, obrigatória para registro no diário. |
| hora_consumo | Hora local ou expressão aproximada; vazia se desconhecida após pergunta. |
| precisao_temporal | Texto: `informado`, `aproximado`, `resolvido_relativo`, `inferido_confirmado` ou `pendente`; registrar também aproximação de expressão relativa em origem_temporal. |
| origem_temporal | Texto: relato, expressão relativa ou inferência confirmada e referência confiável usada, com limitações; não exige timestamp da mensagem. |
| fuso | Texto, IANA ou offset quando disponível e necessário. |
| alimento | Texto, descrição identificada; distinguir descrição inferida. |
| preparo | Texto, preparo conhecido ou estimado. |
| quantidade, quantidade_min, quantidade_max | Números, quantidade central ou limites da faixa; vazios quando desconhecidos. |
| unidade_quantidade | Texto, unidade da quantidade, por exemplo `g`, `ml` ou `unidade`. |
| origem_descricao_quantidade | Texto, informado, documentado ou estimado, fonte e incerteza de alimento, preparo e porção. |
| cadastro_id | Texto opcional, vínculo ao cadastro reutilizável; preservar aqui descrição e valores usados no consumo. |
| observacoes | Texto, contexto e pendências não cobertas por outros campos. |

### `Nutrientes`

Uma linha por nutriente e consumo, inclusive energia: múltiplos nutrientes não
são múltiplos consumos. Não repetir o total de uma refeição como valor de item.

| Campo | Tipo e significado |
| --- | --- |
| consumo_id | Texto, vínculo com `Consumos`. |
| nutriente | Texto identificável, por exemplo `energia`, `proteína`, `carboidrato`, `gordura`, `fibra` ou `sódio`. |
| valor, minimo, maximo | Números, valor ou limites da faixa; vazios quando pendente. |
| unidade | Texto explícito: `kcal` para energia, `g` ou `mg` conforme nutriente. |
| origem | Texto: `informado`, `documentado` ou `estimado`, com fonte identificável quando disponível. |
| incerteza | Texto, hipótese, limitação ou aproximação; vazio se não informada. |
| estado | `disponivel` ou `pendente`; pendente não significa zero. |

Valores disponíveis apenas para a refeição inteira não devem ser atribuídos a
itens arbitrários: preserve-os em `Resumos` com escopo da refeição e descrição
de nutriente, unidade, origem e incerteza.

### `Resumos`

Uma linha por escopo e tipo de resumo. A identidade lógica é a combinação de
`escopo`, `alvo` e `tipo`; não misturar nota de refeição e de dia.

| Campo | Tipo e significado |
| --- | --- |
| escopo | `refeicao` ou `dia`. |
| alvo | `refeicao_id` para refeição ou data `AAAA-MM-DD` para dia. |
| tipo | `nota` ou nome do total nutricional, por exemplo `energia`. |
| estado | `atual`, `desatualizado` ou `dados_insuficientes`. |
| situacao_dia | Para dia: `parcial`, `fechado` ou `revisao`; vazio para refeição. Fechamento exige pedido ou confirmação. |
| conceito | Somente para nota: `Ótimo`, `Bom`, `Alerta`, `Ruim` ou `BOFETADA`; vazio se insuficiente. |
| farol | Somente para nota: `🟢`, `🟡`, `🔴` ou `👋`, correspondente ao conceito. |
| justificativa | Texto da avaliação e suas limitações; vazio se não houver. |
| valor, minimo, maximo, unidade | Número ou faixa e unidade para total nutricional; vazios para nota ou dado desconhecido. |
| origem_incerteza | Texto, fontes, hipóteses, lacunas e escopo dos dados considerados. |

Mapeamento do farol: Ótimo e Bom → 🟢; Alerta → 🟡; Ruim → 🔴;
BOFETADA → 👋. Nota não é número. Ausência de avaliação pode ser representada
por linha `dados_insuficientes` com conceito e farol vazios ou pela ausência de
linha; leitura deve distingui-la de uma nota válida. Não atribuir nota por
preenchimento da estrutura. Uma nota diária não substitui a nota de refeição.

### `Alteracoes`

| Campo | Tipo e significado |
| --- | --- |
| operacao_id | Texto único e estável para a mesma operação e suas retomadas. |
| tipo | `registro`, `complemento`, `correcao` ou `pendencia`. |
| refeicao_id, consumo_id | Textos de vínculo ao alvo; consumo_id pode ficar vazio para pendência de refeição. |
| descricao | Texto com alteração solicitada, valores anteriores e novos pertinentes e motivo/fonte conhecidos. |
| estado | `pendente` ou `concluida`; não registrar conclusão sem confirmação. |
| dias_afetados | Texto com datas locais anterior e nova, quando pertinente. |

Use a mesma `operacao_id` nas linhas necessárias de uma operação que afete
vários consumos, distinguindo cada alvo; o par operação/alvo identifica a ação.
Mantenha pendências e histórico de correções sem copiar uma linha de consumo.
Em falha parcial, o estado real dos dados e das ações confirmadas prevalece sobre
o marcador: confira cada alvo antes de concluir. Não é necessária data de escrita.

## Recuperação e consistência

Leia todos os lotes relevantes e una por IDs; selecione dias pela data de cada
consumo, nunca pela data da conversa. Para operações retomadas, procure o ID da
operação e confira os alvos e campos efetivos; marcador isolado não comprova a
escrita. Se os IDs da tentativa não puderem ser recuperados em outra conversa,
procure candidatos por data, refeição e conteúdo e esclareça ambiguidade com o
usuário antes de gravar. Itens iguais podem ser consumos diferentes. Não prometa
idempotência transacional: verificação concorrente depende do conector.

Antes de novo consumo, correção, complemento ou conclusão de pendência, identifique resumos afetados e marque-os como
desatualizados, inclusive dia antigo e novo ao mudar a data. Se não for possível
confirmar essa marcação, não altere os consumos que deixariam resumos falsamente
atuais. Se a operação falhar depois, mantenha a marcação conservadora. Recalcule
com base apenas no estado confirmado e atualize ou mantenha `desatualizado`;
para dia fechado use `revisao` até confirmação de novo fechamento. Se uma
fórmula existente se recalcula, confira abrangência e resultado, sem sobrescrever
sua lógica. Um relatório lê registros persistidos de todos os dias solicitados;
uma nota calculada apenas para consulta não é escrita automaticamente.
