# Exemplos de gravação interna

Dados e valores fictícios, usados para revisar decisões. Não são registros do
usuário, composições nutricionais ou um modelo obrigatório de planilha.
Em todos os casos, a Core reúne o pedido e fala com o usuário; data-writer
recebe a operação já contextualizada e retorna o resultado à Core.

## 1. Cabeçalhos existentes e coluna adicional pertinente

**Contexto:** diário com `Data`, `Refeição`, `Descrição`, `Porção`,
`Energia (kcal)` e `Observações`. A análise retorna arroz e feijão, porções
estimadas, 300 kcal no total e 8 g de fibras. O acompanhamento inclui fibras.
O registro cotidiano está autorizado.

**Condução:** mapear kcal para `Energia (kcal)`, sem criar `Calorias`.
Preservar os componentes e as porções na granularidade existente. Acrescentar
`Fibras (g)` em espaço livre, manter as demais colunas e registrar a origem
estimada em `Observações`. Não acrescentar proteínas sem necessidade nem
perguntar ao usuário se pode criar cada coluna. Conferir a gravação.

**Retorno interno possível:** situação salvo; intervalo efetivamente confirmado;
refeição de 300 kcal estimadas; coluna `Fibras (g)` criada; 8 g registrados;
nenhuma pendência. IDs e intervalos reais vêm da ferramenta, nunca do exemplo.

## 2. Correção e complemento na mesma mensagem

**Contexto:** a Core identificou um jantar com 400 kcal e encaminhou duas
alterações já analisadas: corrigir o frango de 200 para 160 kcal e acrescentar
meio filé de 100 kcal. Novo total conferido pela Core: 460 kcal.

**Condução:** atualizar o item anterior e acrescentar apenas o meio filé,
mantendo ambos no jantar correto. Preservar fórmulas; se houver total gravado
como valor, atualizá-lo para 460 kcal. Não somar 160 kcal às 400 anteriores
nem lançar outra cópia de todo o jantar. Retornar o que foi confirmado.

Se a correção for confirmada e o complemento falhar, retornar parcial com as
duas situações separadas. O total de 460 kcal não pode constar como persistido
antes de o complemento ser confirmado.

## 3. Calorias ainda desconhecidas e faixa estimada

**Contexto:** a Core encaminha um suplemento consumido com kcal desconhecidas
e outro item analisado em uma faixa de 100–150 kcal, sem valor central.

**Condução:** registrar os consumos identificados, mantendo as calorias do
suplemento pendentes. Guardar a faixa do segundo item na representação
compatível, com sua origem estimada; não escolher 125 kcal nem preencher zero.
Se o modelo numérico precisar de campos adicionais para preservar a faixa,
criá-los sem alterar a semântica das colunas existentes.

Quando a análise posterior trouxer as kcal do suplemento, completar o registro
existente. Não registrar novamente sua ingestão. Informar à Core quais resumos
precisam ser atualizados a partir dos novos dados.

## 4. Escrita sem confirmação e repetição do pedido

**Contexto:** o envio de um novo lanche retorna resultado incerto. A releitura
confirma exatamente o lançamento correspondente à operação.

**Condução:** retornar salvo, com a evidência da releitura. Não repetir o envio.
Se o mesmo pedido for retomado depois, conferir o estado existente antes de
decidir entre operação já concluída e novo consumo efetivamente confirmado.

Se a releitura também falhar, retornar incerto, incluindo destino e dados da
tentativa. Não dizer que nada foi salvo nem criar outro lançamento por segurança.

## 5. Coluna criada, registro não gravado

**Contexto:** a criação de `Sódio (mg)` é confirmada, mas o preenchimento da
refeição falha de forma conhecida e não há condição de repetir a operação.

**Condução:** retornar parcial: coluna criada, dados da refeição não salvos.
Preservar a coluna e os registros anteriores. Na retomada, reutilizar o cabeçalho
e verificar se há lançamento antes de preencher apenas o que falta.

## 6. Fórmulas, colunas personalizadas e mudança concorrente

**Contexto:** a planilha tem campos personalizados e fórmulas de resumo. Entre
a leitura da Core e a gravação, o usuário altera a porção do registro alvo.

**Condução:** conferir o estado atual e devolver o conflito à Core antes de
aplicar a correção preparada com dados antigos. Não apagar colunas extras,
sobrescrever fórmulas ou substituir a planilha para restaurar a versão anterior.

## 7. Intenção, hábito e data do consumo

**Contexto:** a mensagem com timestamp `2026-09-15T00:10:00-03:00`, no fuso conhecido, contém:
“Comi uma banana há 40 minutos, costumo beber água e talvez tome iogurte”.

**Condução da Core:** resolver a banana como consumo de 14/09, aproximadamente
23h30; identificar o hábito e a intenção separadamente.

**Gravação:** registrar somente a banana como consumo em 14/09. Atualizar o
hábito no perfil apenas se isso fizer parte da operação autorizada recebida.
Não lançar água hoje nem iogurte. Preservar o timestamp da mensagem e o horário
aproximado do consumo em campos distintos ou observações compatíveis. Se a
gravação ocorrer em 16/09, manter a banana em 14/09 e distinguir a data da escrita.
Não trocar a data pelo instante da gravação nem de uma tentativa posterior.

## 8. Resumo desatualizado e falta de escrita

**Contexto:** uma correção nutricional está pronta, mas falta avaliação atualizada.
O diário armazena um farol anterior da refeição.

**Condução:** se a escrita estiver disponível, aplicar a correção autorizada e
identificar a avaliação armazenada como pendente de revisão, sem inventar outro
farol. Devolver à Core a necessidade de reavaliar e salvar o resultado.

Se não houver escrita, retornar não salvo com os dados preparados. A Core pode
apresentar a análise na conversa, deixando explícito que não houve gravação.
