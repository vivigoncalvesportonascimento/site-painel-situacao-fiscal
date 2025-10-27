# Metodologia de Dados

Esta seção é o coração da documentação, detalhando o percurso dos dados desde sua fonte original até se tornarem informações validadas e prontas para análise.

## Fontes de dados

Esta página deve listar, de forma transparente, todos os conjuntos de dados utilizados no projeto. Para cada conjunto de dados, é essencial fornecer o nome, uma breve descrição e um link direto para a página correspondente no portal de dados abertos oficial. Isso não apenas confere credibilidade ao projeto, mas também capacita os usuários a realizar suas próprias verificações e análises.

### Contratos de Dados (Frictonless)

Esta página explica uma das inovações mais importantes da arquitetura modernizada: o uso de contratos de dados explícitos e modulares. A separação do esquema de dados do descritor do pacote é uma mudança fundamental que eleva o esquema de um mero detalhe de implementação para um artefato de governança de dados reutilizável.
Essa abordagem tem implicações que vão além do projeto individual. Um esquema bem definido como table-schema.yaml pode se tornar a "fonte única da verdade" para a estrutura de dados fiscais dentro de uma organização. Outras equipes ou sistemas podem referenciar este mesmo arquivo para garantir consistência em diferentes aplicações, prevenindo a proliferação de silos de dados e definições conflitantes. A documentação deve destacar essa visão estratégica.

#### Contrato de Dados Frictionless Data

Para garantir a qualidade, consistência e confiabilidade dos dados, nosso pipeline é governado por "contratos de dados" explícitos. Utilizamos o padrão Frictionless Data para definir e validar a estrutura dos dados em cada etapa.
Esta abordagem trata as definições de dados como artefatos de primeira classe, separados da lógica de programação, o que melhora a governança e permite a reutilização desses contratos por outros sistemas.
1. O Contrato de Transformação (schemas/transformations.yml)
Este arquivo YAML funciona como uma "receita" para a limpeza inicial dos dados. Ele mapeia os nomes das colunas dos arquivos brutos para nomes padronizados e define os tipos de dados de destino.yaml
schemas/transformations.yml
•	original_name: 'ANO_EXERCICIO' target_name: 'ano_lancamento' type: 'integer'
•	original_name: 'VALOR_EMPENHADO' target_name: 'valor_empenhado' type: 'number'
•	original_name: 'ORGAO_NOME' target_name: 'orgao_responsavel' type: 'string'
3. O Manifesto do Pacote (datapackage.yaml)
Com o esquema de qualidade extraído, o arquivo datapackage.yaml atua como um manifesto de alto nível. Ele descreve o conjunto de dados como um todo e, crucialmente, referencia o contrato de qualidade externo, em vez de contê-lo.
Esta arquitetura modular, com um manifesto que aponta para um contrato de qualidade, cria um sistema de governança de dados mais limpo, robusto e sustentável.
Componentes do Pipeline
Todo o código de processamento está localizado no diretório /scripts, com cada módulo tendo uma responsabilidade única:
•	load_data.py: Responsável por carregar os dados brutos de /data-raw/.
•	transform_data.py: Aplica as transformações definidas em schemas/transformations.yml.
•	validate_data.py: Valida o dataframe transformado contra o contrato de qualidade schemas/table-schema.yaml.
•	save_data.py: Salva os dados validados no diretório /data/.
•	build.py: O orquestrador que importa e executa os módulos acima na sequência correta.
Execução Orquestrada com Poetry
A execução de todo o pipeline foi abstraída para um único comando, definido em nosso arquivo pyproject.toml. Isso desacopla o comando de execução da implementação interna do script.
Definição no pyproject.toml:
Esta configuração cria um ponto de entrada chamado build-data. Para executar todo o pipeline de ponta a ponta, basta executar o seguinte comando na raiz do projeto:
O comando poetry run garante que o script seja executado dentro do ambiente virtual correto, com acesso a todas as dependências do projeto na versão exata definida no poetry.lock.

