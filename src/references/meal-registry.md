# Cadastro de alimentos e refeições conhecidas — meal-registry

Aplicar ao consultar, criar, corrigir ou renomear alimentos, produtos e receitas
reutilizáveis, de uma fruta a uma marmita personalizada. O Core decide a operação
e esclarece dúvidas com o usuário; meal-analyzer fornece a composição e suas
fontes; data-writer confere e efetiva a persistência.

`meal-registry` designa esse cadastro. Na organização atual, seu destino é
`food-registry.gsheet`, identificado pelo recurso real na pasta do usuário.
Não criar outra planilha apenas pela diferença de nome. Estas regras não fixam
o modelo completo do arquivo nem incluem uma base nutricional pré-preenchida.

## Alimentar o cadastro

- Consultar o cadastro pertinente antes de pesquisar ou propor outro registro.
  Reutilizar uma referência identificada e compatível com alimento, preparo,
  variante e porção; igualdade de calorias não comprova igualdade de composição.
- Na rotina de persistência autorizada, cadastrar referências reutilizáveis
  identificadas na análise: alimentos simples, produtos com rótulo ou receitas
  e marmitas descritas pelo usuário. Um pedido explícito para guardar uma receita
  também pode alimentar o cadastro sem registrar consumo.
- Não exigir repetição do alimento ou confirmação a cada fruta. Uma referência
  genérica pode ser cadastrada como tal, com a porção e a origem estimadas claras;
  não convertê-la em produto, variedade ou receita específica por inferência.
  Uma foto isolada de prato não estabelece uma receita habitual permanente.
- Preservar composição, porção de referência, unidades, preparo, origem dos dados
  e incertezas disponíveis. Identificar referências genéricas e pendências; dado
  desconhecido não é zero. Não exigir todos os nutrientes para cadastrar.
- Cadastrar uma referência não lança uma refeição. Registrar um consumo não
  exige criar outro cadastro. Pedidos de análise sem salvar não autorizam
  alimentar o cadastro; respeitar as autorizações do onboarding e da plataforma.
- Se o cadastro estiver inacessível, analisar a refeição com as fontes possíveis,
  mas deixar a criação ou renomeação pendente: não há como conferir unicidade.

## Identidade, nomes e apelidos

Cada cadastro tem **identidade estável e nome único na pasta de trabalho do
usuário**. O nome serve à conversa; não é a identidade do registro. Preservar o
identificador existente ou acrescentar uma referência estável na estrutura
disponível, sem impor um esquema completo nem usar somente número de linha
mutável. Renomear não troca a identidade nem rompe os vínculos existentes.

Para conferir nomes e apelidos:

1. Comparar desconsiderando maiúsculas/minúsculas, acentos, espaços excedentes
   e diferenças meramente gráficas de separadores, como hífen versus espaço.
   Preservar números, medidas e distinções como `1,5` e `15`. Nome sem conteúdo
   identificável não é válido. Esta comparação não exige código do usuário.
2. Conferir o nome proposto contra **todos os nomes e apelidos** já existentes,
   inclusive nomes anteriores preservados em renomeações e versões fora de uso
   habitual. Não usar diferenças de caixa ou pontuação para contornar colisões
   nem reaproveitar um nome antigo para outra composição.
3. Conferir também candidatos próximos no significado, abreviações e grafias
   parecidas. Semelhança ajuda a localizar candidatos; não autoriza fundi-los nem
   concluir que produtos, fornecedores ou receitas diferentes são o mesmo item.
4. Cada apelido deve apontar para uma única identidade e não pode colidir com
   nome ou apelido de outro cadastro. Um nome de família que abrange várias
   receitas é uma referência contextual a esclarecer, não um apelido compartilhado.

Um nome exato é um candidato forte, mas não autoriza ignorar uma mudança de
composição informada na mesma mensagem. Se o nome já existe e a referência é a
mesma, reutilizar o cadastro. Se representa outra composição, preservar o
anterior e escolher um nome distinto. Se faltar informação para distinguir,
perguntar antes de vincular o consumo ou gravar o cadastro.

Preferir nomes curtos, reconhecíveis e com diferença útil: alimento/produto ou
fornecedor, seguido de variante, preparo ou composição que o distingue. Manter
o nome informado pelo usuário quando for válido e livre. Para uma nova versão,
propor um nome como `Marmita Fornecedor Exemplo — arroz integral`.

Se o discriminador não bastar ou ainda não for conhecido, usar um sufixo de
versão legível, como `v2`, conferindo o próximo nome livre; não presumir que
`v2` está disponível. Composição e vínculo com a versão anterior devem continuar
registrados, mesmo quando o nome use somente um número. O usuário não precisa
inventar um nome técnico: escolher um nome livre e informar qual foi usado.
Se ele exigir um nome ocupado para outro item, explicar a colisão e propor a
alternativa antes de gravar, sem substituir ou renomear o cadastro anterior.

## Composição nova, porção diferente ou correção

| Situação | Decisão |
| --- | --- |
| Mesmo alimento/receita e consumo proporcional de metade, duas unidades ou outra quantidade | Reutilizar o cadastro e escalar a porção consumida; não criar outra versão. |
| Acréscimo, retirada ou sobra somente nesta refeição | Ajustar o consumo específico, mantendo a receita habitual. Não criar outra receita permanente por padrão. |
| Nova composição habitual: ingredientes, proporções ou preparo diferentes | Criar outro cadastro com identidade e nome distintos, relacionando-o à mesma família quando pertinente. Preservar o anterior. |
| Nova variante de produto ou reformulação documentada | Distinguir a nova referência; não substituir silenciosamente a composição usada anteriormente. |
| Correção explícita de erro de digitação, transcrição ou fonte da mesma referência | Corrigir o alvo identificado, preservando a origem e a alteração. Não tratar uma receita realmente nova como correção. |
| Mesma composição já cadastrada sob outro nome | Reutilizar a identidade encontrada; se útil, acrescentar um apelido livre. Não duplicar só porque o relato usou outro nome. |

Se “agora veio mais frango” puder representar consumo pontual ou novo padrão,
perguntar: “Mudou só nesta refeição ou essa passa a ser a composição habitual?”.
Se o usuário disser claramente “mudei minha marmita de sempre; a nova composição
é...”, criar a nova versão sem pedir novamente se a mudança é habitual.
Pedir somente os componentes ou diferenças necessários que ainda faltarem.

Estimativas de quantidades não obrigam a interromper a análise do consumo.
Entretanto, uma estimativa visual não pode decidir sozinha qual receita
permanente substituir nem resolver uma ambiguidade entre cadastros.

## Encontrar a referência na conversa

- Reutilizar nome ou apelido inequívoco quando compatível com o relato. Para
  referências curtas como “a marmita”, usar a família e o contexto já confirmado.
- Quando o usuário declarar um novo padrão habitual, guardar essa preferência
  para a família correspondente, junto da nova versão. Só usar como padrão após
  persistência confirmada; criar uma alternativa não a torna automaticamente
  o padrão. Não escolher pela maior versão, pela última linha ou pela data mais
  recente de criação.
- “A de sempre” pode usar o padrão confirmado e compatível com o período do
  consumo. Havendo várias opções sem preferência clara, perguntar com os nomes
  e diferenças úteis: “A de arroz integral ou a de purê?”. Não pedir IDs.
- Nome explícito de versão anterior continua identificando aquela composição,
  mesmo que outra seja habitual. Em relatos retroativos, usar o cadastro ligado
  ao consumo ou o padrão vigente conhecido para aquele período; se não houver
  base, esclarecer. Não aplicar automaticamente o padrão atual ao passado.
- Se faltou a escolha, manter o vínculo pendente, sem usar silenciosamente uma
  das opções nos cálculos. Analisar os componentes já descritos e registrar o
  consumo autorizado quando identificável, com essa limitação; nunca vincular
  ou calcular uma marmita só por compartilhar o fornecedor com outra.

## Preservar o histórico e conferir a gravação

Antes de criar ou renomear, data-writer deve conferir os nomes, nomes anteriores
e apelidos no estado atual do cadastro e confirmar que a operação ainda não foi
aplicada. Consultar somente as informações necessárias à conferência, sem
carregar composições nutricionais de todo o catálogo em cada reutilização.
Mapear identidade, nome, composição e vínculos à estrutura encontrada, criando
somente os campos ou observações necessários para preservar essas informações.

- Uma receita nova não sobrescreve a anterior, não troca sua identidade e não
  atualiza refeições passadas. O diário mantém os valores e a referência usados
  no consumo. Não ligar o histórico a um nome que pode ser reatribuído nem a
  uma fórmula que passe a recalcular o passado usando a composição atual.
- Renomeações e correções preservam a identidade e o nome anterior como
  referência histórica; um apelido anterior só pode ser mantido se for livre.
  Corrigir o cadastro não autoriza recalcular todo o diário: uma revisão de
  consumos anteriores exige identificar o período e os alvos pertinentes.
- Se já existirem nomes duplicados, não fundir, excluir ou renomear registros
  automaticamente. Devolver ao Core os candidatos e pedir a distinção necessária
  para resolver o caso. Não usar arbitrariamente a primeira ocorrência.
- Antes da escrita, conferir novamente colisões quando houver mudança de estado.
  Depois, conferir identidade, nome, composição, apelidos e padrão que foram
  efetivamente gravados. Se a ferramenta não confirmar a unicidade final,
  reler os candidatos pertinentes, inclusive fora do primeiro lote de resultados.
- Repetir um pedido ou retomar uma escrita incerta não cria outra versão.
  Conferir a identidade e a alteração já aplicadas antes de tentar novamente.
  Se houver conflito concorrente ou não for possível verificar a unicidade,
  devolver a pendência; não declarar sucesso nem prometer proteção transacional
  que o conector não oferece.
- Separar gravação do cadastro, definição do padrão e registro do consumo. Se
  só parte tiver sucesso, confirmar apenas essa parte; não relançar a refeição
  para completar o cadastro e não presumir que um padrão pendente foi salvo.

## Exemplos de conversa e cenários de revisão

Exemplos fictícios de comportamento, sem dados nutricionais nem registros reais
do usuário. Não cadastrar esses nomes ou receitas no onboarding.

**Novo padrão:** existe `Marmita Fornecedor Exemplo — arroz integral`.
Usuário: “Minha marmita de sempre agora leva purê no lugar do arroz; o resto
continua igual.” A composição anterior está acessível e o nome proposto está
livre. Após a confirmação real das gravações, o Core pode responder:

> Cadastrei **Marmita Fornecedor Exemplo — purê** como sua nova marmita habitual.
> A versão com arroz integral continua disponível. As refeições anteriores foram preservadas.

Isso não registra um almoço: a mensagem mudou o padrão, sem informar consumo.

**Nome ocupado:** usuário pede “Salve a versão com purê como Marmita Fornecedor
Exemplo”, mas esse nome já identifica a versão com arroz. Antes de gravar:

> Esse nome já identifica a versão com arroz. Posso chamar a nova de
> **Marmita Fornecedor Exemplo — purê**?

**Referência ambígua:** há duas marmitas do mesmo fornecedor e nenhum padrão
confirmado. Usuário: “Almocei a marmita.”

> Foi a marmita com arroz integral ou a com purê?

| Cenário | Resultado esperado |
| --- | --- |
| `Maçã` existente; proposta ` maçã ` ou `MACA` | Detectar colisão normalizada; reutilizar se for a mesma referência, distinguir se for outra. |
| Novo nome igual ao apelido de outra versão, inclusive fora de uso habitual | Rejeitar a colisão; não reatribuir o apelido. |
| Grafias próximas do fornecedor, com receitas diferentes | Localizar candidatos e esclarecer; não fundir automaticamente. |
| Nova banana genérica sem variedade conhecida | Usar referência genérica sustentada, com origem e porção estimadas; não inventar variedade nem exigir perfil completo. |
| “Comi metade da marmita habitual” | Reutilizar a versão identificada, escalando somente o consumo. |
| “Só hoje troquei o arroz por purê” | Ajustar esse consumo; manter a receita e o padrão habituais. |
| “Agora minha marmita leva purê” sem clareza sobre recorrência | Perguntar se é pontual ou novo padrão antes de alterar o cadastro habitual. |
| Nova receita nomeada; `v2` e `v3` já ocupados | Escolher outro nome livre, sem sobrescrever as versões existentes. |
| Receita já existente relatada com outro nome | Reutilizar a identidade; apelido só se não colidir. |
| Novo padrão confirmado, retomado em outro chat | Recuperar a preferência persistida; não depender da memória desta thread. |
| Nome explícito antigo ou consumo anterior à troca | Usar a referência identificada para aquele consumo, esclarecendo se necessário. |
| Renomeação, correção do cadastro ou nova composição | Preservar vínculos e valores passados; não recalcular o diário silenciosamente. |
| Escrita incerta, concorrência ou duplicatas legadas | Conferir o estado; deixar conflito pendente em vez de criar cópia ou declarar unicidade sem base. |
| Cadastro salvo, definição do novo padrão falha | Confirmar o cadastro e indicar a preferência pendente, sem afirmar que o novo padrão foi persistido. |
