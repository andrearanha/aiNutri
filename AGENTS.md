# Instruções para agentes de desenvolvimento — aiNutri

## Objetivo

Desenvolver e manter o **aiNutri**, um assistente conversacional de acompanhamento alimentar e de saúde. 

Deve ajudar a registrar refeições, interpretar o histórico e acompanhar objetivos individuais, com linguagem acessível a leigos, atletas, nutricionistas e médicos.

Funcionar preferencialmente com base num plano alimentar ou dieta fornecido por profissional habilitado, a ser submetido pelo usuário no onboarding.

## Princípios

- **Prompts e Skills, não código:** implementar funcionalidades por meio de instruções em Markdown e configurações declarativas estritamente necessárias. Não introduzir scripts, backends ou aplicações próprias sem uma mudança explícita de escopo.
- **Experiência simples:** orientar usuários sem conhecimentos técnicos em linguagem clara, com onboarding guiado e perguntas feitas somente quando necessárias.
- **Uso individual e privado:** cada usuário conecta suas próprias contas e mantém seus dados separados. Nunca incluir credenciais, exames, históricos pessoais ou informações identificáveis no repositório ou no pacote público.
- **Persistência explícita em conta do usuário:** priorizar conectores autorizados e armazenamento do próprio usuário (como Google Drive). Não afirmar que um registro foi salvo ou lido sem confirmação da ferramenta.
- **Responsabilidade em saúde:** distinguir dados informados de estimativas e interpretações; indicar incertezas, não inventar medições nem substituir diagnóstico ou acompanhamento profissional.
- **Projeto aberto e portável:** manter instruções compreensíveis, modulares e reutilizáveis por colaboradores e usuários de diferentes perfis.

## Organização e manutenção

- `src/` contém o material editável do Plugin; cada capacidade deve ter sua própria Skill, com `SKILL.md` como entrada.
- A personalidade e os comportamentos de uso pertencem às Skills, **não a este `AGENTS.md`**, que orienta apenas o desenvolvimento.
- `dist/` contém exclusivamente artefatos de distribuição gerados a partir de `src/`; não editar manualmente nem incluir este arquivo no pacote final.
- Preferir mudanças pequenas e documentadas. Antes de adicionar uma Skill, verificar se a capacidade pode ser incorporada a uma existente sem perder clareza.
- Descrever e revisar exemplos reais de interação, sobretudo onboarding, registro de refeições, análise de histórico e leitura responsável de exames.

## Critério de qualidade

Cada alteração deve preservar uma experiência utilizável integralmente pelo ChatGPT online, sem exigir terminal, GitHub ou configuração de código do usuário final.