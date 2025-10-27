# Desenvolvendo o Painel 
 
O script irá carregar os dados limpos, criar filtros interativos na barra lateral e exibir gráficos e 
tabelas que se atualizam dinamicamente. 

A escolha do Streamlit como framework para esta camada de interface foi estratégica. Sua capacidade de transformar scripts Python em aplicações web interativas com um mínimo de código boilerplate permite que a equipe de desenvolvimento foque na lógica de análise e visualização dos dados, em vez de se preocupar com a complexidade do desenvolvimento web tradicional. O resultado é um ciclo de desenvolvimento rápido que acelera a entrega de valor para o usuário final: o cidadão, o jornalista, o pesquisador e o gestor público.

Esta página, portanto, serve como um guia definitivo para o código localizado no diretório /dashboard/ do projeto, capacitando desenvolvedores a entender, replicar e contribuir para a evolução desta ferramenta essencial de transparência pública.

Configuração do Ambiente e Bibliotecas Essenciais
Para garantir a total reprodutibilidade do ambiente de desenvolvimento e execução do painel, o projeto adota uma abordagem de gerenciamento de dependências determinística. Em vez de uma instalação manual de pacotes, utiliza-se um sistema que garante que cada desenvolvedor, em qualquer máquina, opere com as versões exatas das bibliotecas definidas no projeto. A base tecnológica do painel é composta por um conjunto de bibliotecas Python de código aberto, cada uma selecionada por sua robustez e alinhamento com as melhores práticas da indústria.   

As instruções para replicação do ambiente estão detalhadas na seção "Comunidade" deste site, utilizando o gerenciador de pacotes Poetry para automatizar a instalação a partir dos arquivos pyproject.toml e poetry.lock. As bibliotecas fundamentais que compõem a aplicação são detalhadas abaixo.   

Arquitetura da Aplicação: Navegação Multi-Página
O Painel Fiscal não é um script monolítico. Ele foi arquitetado como uma aplicação multi-página para promover a clareza, a manutenibilidade e uma experiência de usuário organizada. Essa estrutura modular é uma manifestação do princípio de engenharia de software de "separação de responsabilidades", uma filosofia que permeia todo o projeto, desde a organização dos scripts do pipeline de dados até a interface do usuário.   

O Streamlit adota uma abordagem de "convenção sobre configuração" para criar aplicações multi-página, o que simplifica enormemente a implementação. A estrutura de arquivos do diretório `/dashboard/` dita diretamente a navegação do site:

```
dashboard/
├── pagina_inicial.py       # A página principal (Home)
└── pages/
    ├── 1_resultado_fiscal.py
    └── 2_resultado_previdenciario.py
```

A lógica é a seguinte:

Página Principal: O script executado pelo comando `streamlit run` (neste caso, `Pagina_inicial.py`) é automaticamente designado como a página inicial ou "Home" da aplicação.

Páginas Adicionais: Qualquer arquivo Python (`.py`) colocado dentro de um subdiretório chamado `pages/` é automaticamente detectado pelo Streamlit e adicionado como um item de navegação em uma barra lateral.

Ordenação da Navegação: O Streamlit ordena as páginas na barra de navegação alfabeticamente pelo nome do arquivo. Para forçar uma ordem específica, pode-se prefixar os nomes dos arquivos com números e um sublinhado (ex: 1_, 2_,...), como demonstrado na estrutura acima.

Essa arquitetura oferece vantagens significativas. Cada página se torna um módulo analítico autocontido, focado em um aspecto específico dos dados fiscais (uma visão geral, a análise fiscal detalhada, o resultado previdenciário, etc.). Isso não apenas facilita a navegação para o usuário final, mas também simplifica o desenvolvimento e a manutenção. Para adicionar uma nova seção de análise ao painel, um desenvolvedor precisa apenas criar um novo arquivo Python no diretório `pages/`, sem a necessidade de modificar o código das páginas existentes.

## Análise Detalhada: Pagina_inicial.py (A Porta de Entrada)

O script `pagina_inicial.py` serve como a landing page do Painel Fiscal. Sua principal função é apresentar o projeto, fornecer contexto e exibir métricas de alto nível que oferecem um panorama imediato da situação fiscal. A análise a seguir detalha cada bloco de código deste script.

Bloco 1: Importações e Configuração da Página
O script começa com a importação das bibliotecas necessárias e a configuração inicial da página.

```python
# dashboard/pagina_inicial.py

import streamlit as st
import pandas as pd
from pathlib import Path

# --- CONFIGURAÇÃO DA PÁGINA ---
# Define o título da página, o ícone e o layout.
# layout="wide" utiliza toda a largura da tela para o conteúdo.
st.set_page_config(
    page_title="Painel Fiscal MG | Início",
    page_icon="📊",
    layout="wide"
)
```

??? note "Clique aqui para ver a explicação para cada linha de código"
    -  `import streamlit as st`: Importa a biblioteca principal do Streamlit, convencionalmente com o alias st.
    -  `import pandas as pd`: Importa a biblioteca Pandas para manipulação de dados, com o alias pd.
    -  `from pathlib import Path`: Importa a classe Path para uma manipulação mais robusta e independente de sistema operacional dos caminhos de arquivo.
    -  `st.set_page_config(...)`: Esta é a primeira chamada do Streamlit no script e deve ser executada apenas uma vez. Ela configura metadados e o layout da página.
    -  `page_title`: Define o título que aparece na aba do navegador.
    -  `page_icon`: Define o favicon que aparece na aba do navegador. Pode ser um emoji ou a URL de uma imagem.
    -  `layout="wide"`: Configura a página para usar a largura total do navegador, o que é ideal para dashboards com múltiplos gráficos e tabelas.