# **Desenvolvendo o Painel** 
 
A escolha do **Streamlit** como framework foi estratégica, pois ele transforma scripts Python em aplicações web interativas com pouco código. Isso permite que a equipe foque na análise e visualização dos dados, em vez de se preocupar com a complexidade do desenvolvimento web tradicional.

Esta página é um guia para ajudar os desenvolvedores a entender, replicar e contribuir com esta ferramenta de transparência.

O painel foi arquitetado como uma aplicação multi-página para ser mais claro, organizado e fácil de manter, seguindo o princípio de "separação de responsabilidades". O **Streamlit** facilita isso, pois a própria estrutura de arquivos no diretório `/dashboard/` define a navegação do site.

```
dashboard/
├── Pagina_inicial.py       # A página principal (Home)
├── data_loader.py       # A página principal (Home)
├── utils.py       # A página principal (Home)
└── pages/
    ├── 1_resultado_fiscal.py
    └── 2_resultado_previdenciario.py
```


??? note "Clique aqui para ver a explicação para a lógica da estruturação das pastas e arquivos."
    -  **Página Principal**: O script executado pelo comando `streamlit run` (neste caso, `Pagina_inicial.py`) é automaticamente designado como a página inicial ou "Home" da aplicação.
    -  **Páginas Adicionais**: Qualquer arquivo Python (`.py`) colocado dentro de um subdiretório chamado `pages/` é automaticamente detectado pelo Streamlit e adicionado como um item de navegação em uma barra lateral.
    -  **Ordenação da Navegação**: O Streamlit ordena as páginas na barra de navegação alfabeticamente pelo nome do arquivo. Para forçar uma ordem específica, pode-se prefixar os nomes dos arquivos com números e um sublinhado (ex: 1_, 2_,...), como demonstrado na estrutura acima.

Essa arquitetura oferece vantagens significativas. Cada página se torna um módulo analítico autocontido, focado em um aspecto específico dos dados fiscais (uma visão geral, a análise fiscal detalhada, o resultado previdenciário, etc.). 
Isso não apenas facilita a navegação para o usuário final, mas também simplifica o desenvolvimento e a manutenção. Para adicionar uma nova seção de análise ao painel, um desenvolvedor precisa apenas criar um novo arquivo Python no diretório `pages/`, sem a necessidade de modificar o código das páginas existentes.

A arquitetura do dashboard assenta em três pilares essenciais, projetados para garantir robustez, escalabilidade e facilidade de manutenção a longo prazo:

- **Separação de Responsabilidades**: A lógica da aplicação é rigorosamente dividida. O acesso, limpeza e transformação dos dados estão confinados a `data_loader.py`. As funções de formatação de moeda e estilização de tabelas residem em `utils.py`. A interface do utilizador e a lógica de visualização estão nos scripts de página individuais. Esta abordagem evita scripts monolíticos e torna a depuração, os testes e a adição de novas funcionalidades tarefas significativamente mais simples.
- **Modularidade e Escalabilidade**: O projeto aproveita a estrutura nativa de aplicações multipágina do Streamlit. O diretório `dashboard/pages/` é o epicentro da escalabilidade. A adição de um novo indicador é tão simples quanto criar um novo ficheiro Python nesse diretório, seguindo o padrão estabelecido, como `3_Novo_Indicador.py`. Este modelo acomoda o crescimento futuro de forma eficiente.
- **Não se Repita**: Toda a lógica que pode ser partilhada é centralizada. O carregamento de dados e a gestão de cache estão em `data_loader.py`, enquanto a formatação de números e a estilização de tabelas são geridas por `utils.py`. Este princípio minimiza a duplicação de código, garantindo consistência e reduzindo o esforço de manutenção.

Os scripts `data_loader.py` e `utils.py` atuam como fornecedores, exportando funcionalidades (carregamento de dados, formatação) para serem consumidas pelos scripts em `dashboard/pages/`. Esta dependência unidirecional é uma característica de uma arquitetura em camadas robusta, que isola a lógica de negócio da interface do utilizador, simplificando a manutenção.

A escalabilidade é tratada como uma funcionalidade intrínseca. Ao adotar o mecanismo de multipáginas do Streamlit, para adicionar um novo indicador, um desenvolvedor simplesmente cria um novo ficheiro em `pages/`, importa as funções necessárias de `data_loader.py` e `utils.py`, e escreve o código da UI, replicando o padrão existente.