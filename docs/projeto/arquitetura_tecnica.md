## Arquitetura Técnica

Esta é uma página nova e fundamental que substitui a genérica "Ferramentas Utilizadas". Ela serve para documentar as decisões de engenharia que formam a espinha dorsal do projeto. A documentação da estrutura de pastas é particularmente importante, pois, como afirmado no guia de modernização, "a estrutura de pastas de um projeto é sua documentação mais imediata". Ela revela a filosofia de separação de responsabilidades do projeto: dados brutos são isolados dos dados processados, a configuração (esquemas) é separada da lógica (scripts), e o código da aplicação é desacoplado do pipeline de dados.


# Arquitetura Técnica

Este projeto foi construído sobre uma pilha de tecnologia moderna e uma arquitetura de pastas orientada a domínio, projetada para clareza, manutenibilidade e escalabilidade.

## Pilha de Tecnologia (Tech Stack)

A seleção de ferramentas foi guiada por princípios de código aberto, robustez e alinhamento com as melhores práticas da indústria de dados.
•	Python: A linguagem principal para todo o processamento de dados e para a aplicação web.
•	Poetry: Para gerenciamento de dependências e pacotes. Garante ambientes de desenvolvimento e produção determinísticos e reprodutíveis.
•	Frictionless Data: Para definir "contratos de dados" que descrevem e validam a estrutura dos nossos conjuntos de dados, garantindo qualidade e interoperabilidade.
•	Streamlit: Para a construção rápida do dashboard interativo, permitindo que os usuários explorem os dados de forma intuitiva.
•	Docker: Para conteinerizar a aplicação, garantindo uma implantação consistente, segura e eficiente em qualquer ambiente.
•	MkDocs + Material: Para a criação deste site de documentação.

## Estrutura de Diretórios do Projeto

A organização do repositório segue um padrão lógico que separa claramente as responsabilidades de cada componente do projeto.

| Diretório      | Propósito                                                                                                   | Justificativa de Design                                                                                                   |
|:-------------- |:-----------------------------------------------------------------------------------------------------------|:--------------------------------------------------------------------------------------------------------------------------|
| `/data-raw/`   | Armazena os arquivos CSV originais, baixados das fontes oficiais.                                          | **Imutabilidade da Fonte:** Garante que os dados originais nunca sejam modificados, permitindo reexecução do pipeline.    |
| `/data/`       | Destino dos dados limpos, processados e validados, prontos para o dashboard.                               | **Separação de Estados:** Mantém distinção clara entre o estado inicial (bruto) e final (processado) dos dados.           |
| `/scripts/`    | Contém todos os scripts Python do pipeline de ETL, incluindo o orquestrador `build.py`.                    | **Centralização da Lógica:** Agrupa todo o código de processamento em um único local, facilitando manutenção e entendimento.|
| `/schemas/`    | Armazena todos os "contratos" de dados do projeto, como esquemas e mapeamentos de transformação.           | **Configuração como Código:** Trata definições de dados como artefatos versionáveis e desacoplados da lógica do pipeline.  |
| `/dashboard/`  | Contém o código da aplicação Streamlit (`app.py`) que constitui a interface do usuário final.               | **Desacoplamento da UI:** Isola a camada de apresentação da de processamento, permitindo evolução independente.            |