# **Desenvolvendo o Painel** 
 
A escolha do **Streamlit** como framework foi estratégica, pois ele transforma scripts Python em aplicações web interativas com pouco código. Isso permite que a equipe foque na análise e visualização dos dados, em vez de se preocupar com a complexidade do desenvolvimento web tradicional.

Esta página é um guia para ajudar os desenvolvedores a entender, replicar e contribuir com esta ferramenta de transparência.

O painel foi arquitetado como uma aplicação multi-página para ser mais claro, organizado e fácil de manter, seguindo o princípio de "separação de responsabilidades". O **Streamlit** facilita isso, pois a própria estrutura de arquivos no diretório `/dashboard/` define a navegação do site.

```
dashboard/
├── pagina_inicial.py       # A página principal (Home)
└── pages/
    ├── 1_resultado_fiscal.py
    └── 2_resultado_previdenciario.py
```


??? note "Clique aqui para ver a explicação para a lógica da estruturação das pastas e arquivos."
    -  **Página Principal**: O script executado pelo comando `streamlit run` (neste caso, `Pagina_inicial.py`) é automaticamente designado como a página inicial ou "Home" da aplicação.
    -  **Páginas Adicionais**: Qualquer arquivo Python (`.py`) colocado dentro de um subdiretório chamado `pages/` é automaticamente detectado pelo Streamlit e adicionado como um item de navegação em uma barra lateral.
    -  **Ordenação da Navegação**: O Streamlit ordena as páginas na barra de navegação alfabeticamente pelo nome do arquivo. Para forçar uma ordem específica, pode-se prefixar os nomes dos arquivos com números e um sublinhado (ex: 1_, 2_,...), como demonstrado na estrutura acima.

Essa arquitetura oferece vantagens significativas. Cada página se torna um módulo analítico autocontido, focado em um aspecto específico dos dados fiscais (uma visão geral, a análise fiscal detalhada, o resultado previdenciário, etc.). Isso não apenas facilita a navegação para o usuário final, mas também simplifica o desenvolvimento e a manutenção. Para adicionar uma nova seção de análise ao painel, um desenvolvedor precisa apenas criar um novo arquivo Python no diretório `pages/`, sem a necessidade de modificar o código das páginas existentes.

A arquitetura aqui proposta divide a aplicação em duas categorias principais:

- **Arquitetura Fundamental**: Compreende os scripts centrais que formam o esqueleto da aplicação. Estes componentes são responsáveis pela configuração global, navegação, ingestão de dados e gestão de estado. Eles são agnósticos em relação ao conteúdo analítico específico de cada página e servem como a base sobre a qual todos os módulos de análise operam.

- **Módulos de Análise**: São scripts individuais, cada um correspondendo a uma página específica do dashboard (por exemplo, "Resultado Fiscal", "Resultado Previdenciário"). Cada módulo é responsável por sua própria lógica de manipulação de dados, cálculos e visualizações, operando de forma independente dos outros módulos de análise.