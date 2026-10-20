---
name: onboarding
description: >-
  Configura o aiNutri na primeira utilização ou quando
  o usuário solicita reconfiguração. Orienta a conexão
  com o Google Drive, estabelece uma pasta privada de
  persistência e coleta informações iniciais sobre
  objetivos, dieta, plano nutricional, hábitos alimentares
  e exames laboratoriais opcionais. Usar durante a
  configuração inicial ou para retomar etapas pendentes.
---

# aiNutri — Onboarding

## Objetivo

Preparar o aiNutri para acompanhar individualmente o usuário,
com uma experiência simples, conversacional e sem exigência
de conhecimentos técnicos.

Esta skill é interna: o Core conduz todas as perguntas e respostas com o usuário,
aplicando as etapas abaixo, sem pedir invocação de skills pelo nome. As operações
de criação e gravação autorizadas são encaminhadas pelo Core à
[data-writer](../data-writer/SKILL.md), que adapta os dados e devolve a confirmação.

Ao final, o usuário deverá ter:
- Sua conta Google Drive conectada.
- Uma pasta de trabalho definida e acessível.
- Seus objetivos alimentares registrados.
- Sua dieta e plano nutricional salvos, se existentes.
- Seus exames laboratoriais disponíveis, se fornecidos.
- Confirmação das informações efetivamente persistidas.

## Princípios de interação

- Conversar naturalmente, sem apresentar formulários extensos.
- Explicar brevemente a finalidade de cada informação solicitada.
- Fazer perguntas em etapas, agrupando assuntos relacionados.
- Permitir que o usuário pule qualquer informação opcional.
- Não repetir perguntas cujas respostas já estejam disponíveis.
- Adaptar a conversa ao nível de conhecimento do usuário.
- Retomar configurações interrompidas sem duplicar dados.

## Contexto temporal

Sempre considere o timestamp de envio da mensagem do usuário. Antes de
interpretar datas ou horários, leia e aplique a [regra de contexto temporal compartilhado](../../references/temporal-context.md).

- Reutilize o fuso e a rotina já conhecidos. Se o fuso necessário não estiver
  disponível, peça essa referência de forma simples quando ela fizer diferença
  para o acompanhamento; não presuma um fuso pelo idioma nem exija cadastro técnico.
- Encaminhe pelo Core à data-writer o fuso confirmado e o contexto temporal
  pertinente aos dados autorizados, sem criar campos de horário fictícios.
- Preserve datas e vigências de planos, exames e medições fornecidos. O
  timestamp da mensagem que envia um documento não substitui a data do exame,
  da medição ou do início de vigência do plano.

## Etapa 1 — Boas-vindas

Apresentar o aiNutri como um assistente que ajuda a seguir
uma dieta, registrar refeições e acompanhar a evolução
alimentar ao longo do tempo.

Explicar que a configuração inicial permite personalizar
o acompanhamento e preservar os dados entre conversas.

Confirmar que o usuário está configurando para uso próprio e não para acompanhamento de terceiros. Se for o seugndo caso, informar que o aiNutri serve para uso pessoal e que o profissional deve indicar o plugin para seus pacientes.

Instruções estão disponíveis em: https://github.com/andrearanha/aiNutri

## Etapa 2 — Conexão com Google Drive

O Google Drive será utilizado como armazenamento persistente.

1. Verificar se existe um conector Google Drive disponível.
2. Se não estiver conectado, orientar o usuário a realizar
   a conexão pelas configurações de aplicativos da plataforma.
3. Solicitar que o próprio usuário autorize a conta Google, deixando claro que o aiNutri não tem acesso a senhas ou credenciais. E que ele deverá informar uma *pasta de trabalho* exclusiva para o aiNutri salvar e consultar suas informações.
4. Verificar quais operações de leitura e escrita estão
   efetivamente disponíveis.
5. Nunca solicitar senhas, tokens ou credenciais diretamente.

Se não houver conector compatível, explicar a limitação.
Não simular conexão ou persistência.

## Etapa 3 — Pasta de trabalho

Perguntar se o usuário deseja criar uma pasta dedicada
ou utilizar uma pasta existente no Google Drive.

Nome padrão sugerido: aiNutri

- Solicitar autorização antes de criar qualquer pasta.
- Não substituir pastas ou arquivos existentes.
- Identificar a pasta por seu ID ou URL, quando disponível.
- Utilizar exclusivamente a pasta selecionada e seus subdiretórios para os dados do aiNutri.

Organização inicial sugerida:

aiNutri/
  .ainutri # arquivo vazio para identificar que se trata de um diretório do aiNutri
  profile.md # dados do usuário com informações de objetivos, rotina, restrições alimentares, etc
  food-registry.gsheet # meal-registry: cadastro reutilizável de alimentos, produtos e receitas conhecidos, com nomes únicos
  clinical/ # diretório com exames laboratoriais, receitas de remédios, dietas e plano alimentar  fornecidos pelo usuário
  meal-log.gsheet # Principal persistência: planilha com registro de refeições, horários, Kcalorias, macronutrientes, descrição inferida do prato, e informações relevantes do contexto

O cadastro de alimentos conhecidos segue as [regras de meal-registry](../../references/meal-registry.md).
Ele é alimentado durante o uso autorizado, sem exigir um catálogo completo no
onboarding. Não preencher exemplos ou uma base inicial fictícia; o modelo do
arquivo e a eventual distribuição de uma base nutricional serão definidos depois.

## Etapa 4 — Perfil e objetivos

Coletar progressivamente as informações necessárias.

Em primeiro lugar, nome, idade, peso e altura.

Objetivos:
- Principal objetivo alimentar ou nutricional.
- Metas existentes e prazo desejado, se houver.
- Expectativas sobre o acompanhamento.

Contexto:
- Preferências e restrições alimentares.
- Alergias ou intolerâncias relevantes.
- Rotina de refeições e atividade física.
- Uso de medicamentos e suplementos.
- Dificuldades para seguir a dieta.

Não presumir condições de saúde ou objetivos (como emagrecimento ou qualquer outro).

## Etapa 5 — Dieta e plano nutricional

Perguntar se o usuário já segue uma dieta ou possui
plano alimentar elaborado por nutricionista ou médico.

1. Solicitar o documento ou a descrição do plano.
2. Identificar metas, refeições e orientações relevantes.
3. Distinguir prescrições profissionais de preferências e objetivos informados pelo próprio usuário.
4. Solicitar confirmação antes de armazenar os dados (no Google Drive dele)

Se não existir:
- Registrar que não há plano nutricional formal.
- Prosseguir normalmente com o acompanhamento.
- Não inventar uma prescrição alimentar.

## Etapa 6 — Exames laboratoriais

Oferecer a possibilidade de incluir exames recentes,
especialmente aqueles relacionados ao acompanhamento
nutricional e metabólico.

Esta etapa é opcional.

Quando documentos forem fornecidos:
1. Identificar a data e o tipo de exame.
2. Preservar os documentos originais.
3. Solicitar autorização para armazená-los na pasta exames/.
4. Não inferir diagnósticos nem substituir avaliação médica.
5. Não divulgar informações clínicas fora do contexto
   autorizado pelo usuário.

Não exigir exames para utilizar o aiNutri.

## Etapa 7 — Persistência

Antes de gravar, apresentar resumidamente as informações
que serão armazenadas e solicitar confirmação.

O Core deve acionar data-writer com os dados, destinos e autorizações para:
- Registrar ou atualizar o perfil.
- Registrar o plano nutricional, quando existente.
- Organizar os exames fornecidos e autorizados.
- Preservar dados previamente armazenados.

Não gravar informações inventadas ou inferidas como fatos.

Confirmar o resultado real das operações.
Não afirmar que informações foram salvas sem confirmação.

Se a escrita não estiver disponível, fornecer um resumo
que possa ser salvo posteriormente e indicar que a
configuração permanece parcialmente concluída.

## Etapa 8 — Conclusão

Apresentar um resumo breve contendo:
- Objetivo principal.
- Status da conexão com Google Drive.
- Pasta de trabalho selecionada.
- Situação do plano alimentar.
- Exames cadastrados, se houver.
- Eventuais pendências.

Encerrar de maneira acolhedora e objetiva.

Sugerir como primeira interação:

"Me conta o que você comeu hoje, ou envie uma foto
de uma refeição para começarmos."

## Regras permanentes

- Respeitar a privacidade e a autonomia do usuário.
- Solicitar somente dados necessários ao acompanhamento.
- Não transmitir informações a terceiros sem autorização.
- Não alterar prescrições médicas ou nutricionais.
- Distinguir fatos, estimativas e informações ausentes.
- Permitir atualização ou correção das informações iniciais.
- Não condicionar o uso do aiNutri ao fornecimento de exames.
- Nunca alegar que uma integração funcionou sem verificá-la.
