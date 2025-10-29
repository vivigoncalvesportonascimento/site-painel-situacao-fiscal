# **Motivações e Objetivos do Painel**

A criação do Painel Fiscal representa uma evolução estratégica na forma como os dados fiscais do Estado são analisados, apresentados e consumidos. A principal motivação para este projeto é a transição de um processo manual e fragmentado para uma solução centralizada, automatizada e confiável, que busca organizar, padronizar e facilitar o acesso aos principais indicadores da situação fiscal.

## **O Desafio: As Limitações do Processo Atual**

Atualmente, a elaboração de relatórios e análises sobre a situação fiscal depende de um fluxo de trabalho, baseado na extração descentralizada de dados de múltiplas fontes e na manipulação manual em planilhas (como o Excel).

Este processo é repetitivo, consome tempo e apresenta limitações:

- **Falta de Padronização**: A ausência de um padrão visual resulta em gráficos e tabelas com estilos inconsistentes entre diferentes apresentações, a depender de quem elabora.

- **Risco à Integridade dos Dados**: A manipulação manual aumenta o risco de erros humanos, comprometendo a consistência, a rastreabilidade e a segurança das informações.

- **Baixa Agilidade**: O modelo limita a agilidade da equipe e dificulta a verificação dos dados, cruciais para a tomada de decisão.

## **A Solução: Um Painel Centralizado, Confiável e Interativo**

O desenvolvimento do painel aborda diretamente esses desafios, estabelecendo uma fonte única da verdade para os indicadores fiscais. Os objetivos centrais do projeto são:

- **Centralizar e Estruturar**: Criar um repositório único e interativo para todos os visuais, gráficos e tabelas recorrentemente utilizados, acessível à equipe técnica e institucional.

- **Padronizar a Visualização**: Implementar uma identidade visual e metodológica consistente para todos os indicadores, garantindo que as apresentações sejam profissionais, claras, uniformes e sempre atualizadas.

- **Garantir Integridade e Confiabilidade**: Conectar o painel diretamente a fontes de dados já tratadas e normalizadas por scripts automatizados (pipeline de ETL). Isso minimiza a intervenção manual, elimina o retrabalho e reduz drasticamente o risco de erros.

- **Otimizar a Comunicação Estratégica**: Fornecer uma ferramenta robusta para subsidiar apresentações recorrentes e de alto impacto, como as realizadas para agências de classificação de risco (Moody’s, S&P), Assembleia Legislativa (Fiscaliza), audiências públicas e outras reuniões técnicas.

O uso de uma plataforma como o **Streamlit** permite ainda que o painel seja facilmente implantado e compartilhado via web, com filtros interativos e visualizações dinâmicas que se adaptam às necessidades de análise.

## **Visuais e Indicadores Planejados**

Inicialmente, o painel foi projetado para abrigar um conjunto essencial de indicadores fiscais que refletem a saúde financeira do Estado. A estrutura modular permite que novos visuais sejam adicionados ou ajustados conforme a necessidade.

Os principais componentes visuais planejados são:

- **Resultados Gerais**: Resultado Orçamentário, Resultado Primário e Resultado Previdenciário.

- **Indicadores Macro**: Dívida Consolidada Líquida (DCL) e Receita Corrente Líquida (RCL).

- **Despesa Total de Pessoal (DTP)**

- **Detalhamentos da Despesa**:

    - Obrigatória vs. Discricionária

    - Poder Executivo vs. Outros Poderes

    - Por Grupo de Despesa

    - Por Função (Saúde, Educação, Segurança, etc.)

    - Por Unidade Orçamentária (UO)

    - Investimentos: que transitam vs. não transitam no Tesouro

- **Detalhamento da Receita**:

    - Por Natureza (Tributária, Contribuição, Patrimonial, etc.)

    - Vinculação da Receita do ICMS

- **Indicadores e Limites Constitucionais**:

    - Manutenção e Desenvolvimento do Ensino (MDE)

    - Ações e Serviços Públicos de Saúde (ASPS)

    - FAPEMIG

- **Acompanhamento (PROPAG)**:

    - Limite de crescimento das despesas

    - Investimentos Diretos e FEF por UO (Previsão vs. Liquidado)

Em suma, este painel transcende a função de uma simples ferramenta de visualização, estabelecendo-se como uma plataforma estratégica que promove eficiência, segurança e clareza na gestão e comunicação dos dados fiscais do Estado.