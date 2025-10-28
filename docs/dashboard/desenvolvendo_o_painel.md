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

## **Pagina Inicial**

O script `pagina_inicial.py` serve como a landing page do Painel Fiscal. Sua principal função é apresentar o projeto, fornecer contexto e exibir métricas de alto nível que oferecem um panorama imediato da situação fiscal. A análise a seguir detalha cada bloco de código deste script.

**Bloco 1: Importações e Configuração da Página**

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

**Bloco 2: Carregamento e Cache de Dados**

Para garantir a performance da aplicação, o carregamento de dados é encapsulado em uma função e otimizado com um decorador de cache do Streamlit.

```python
# --- FUNÇÕES DE APOIO ---

@st.cache_data
def carregar_dados():
    """
    Carrega os dados fiscais processados a partir de um arquivo CSV.
    Utiliza o cache do Streamlit para evitar recarregamentos desnecessários.
    """
    caminho_dados = Path(__file__).parent.parent / "data" / "dados_fiscais_processados.csv"
    df = pd.read_csv(caminho_dados)
    # Converte colunas de data para o tipo datetime para manipulação correta
    for col in df.columns:
        if 'data' in col:
            df[col] = pd.to_datetime(df[col])
    return df

# Carrega os dados na inicialização do script
df = carregar_dados()

```

??? note "Clique aqui para ver a explicação para cada linha de código"
    -  `@st.cache_data`: Este decorador é um dos recursos mais poderosos do Streamlit para otimização de performance. Quando a função carregar_dados é chamada pela primeira vez, ela executa normalmente (lendo o CSV do disco) e o resultado (o DataFrame) é armazenado na memória cache. Em todas as chamadas subsequentes (por exemplo, quando um usuário interage com um widget em outra página), o Streamlit verifica se o código da função ou seus parâmetros de entrada mudaram. Se não mudaram, ele retorna instantaneamente o resultado do cache, em vez de ler o arquivo do disco novamente. Isso torna a navegação entre páginas e as interações subsequentes extremamente rápidas.   
    -  `caminho_dados` = Path(...): O uso de pathlib e __file__ cria um caminho relativo robusto para o arquivo de dados. Path(__file__).parent.parent navega dois níveis acima do diretório do script atual (de dashboard/ para a raiz do projeto), garantindo que o caminho para data/dados_fiscais_processados.csv funcione independentemente de onde o script for executado.
    -  `pd.to_datetime(...)`: Uma etapa crucial de pré-processamento para garantir que as colunas de data sejam tratadas como objetos de data/hora, permitindo filtros e agregações baseados em tempo.

**Bloco 3: Renderização do Conteúdo da Página**

Esta seção utiliza as funções do Streamlit para renderizar o título, textos descritivos e outros elementos visuais que compõem o corpo da página inicial.

```python
# --- LAYOUT DA PÁGINA INICIAL ---

st.title("Painel de Transparência Fiscal de Minas Gerais")
st.markdown("---") # Cria uma linha divisória horizontal

st.markdown(
    """
    Bem-vindo(a) ao portal de documentação e análise do Painel de Transparência Fiscal de Minas Gerais.
    Este projeto visa transformar dados fiscais complexos em um dashboard interativo e compreensível,
    permitindo que qualquer cidadão explore e entenda como os recursos públicos são gerenciados.

    **Navegue pelas seções na barra lateral para explorar as análises detalhadas.**
    """
)
st.markdown("---")

```

??? note "Clique aqui para ver a explicação para cada linha de código"
    -  `st.title(...)`: Exibe o texto como o título principal da página.
    -  `st.markdown(...)`: Renderiza texto usando a sintaxe Markdown. Isso permite formatação rica como negrito (**texto**), itálico, listas e, como visto aqui, a criação de uma linha divisória horizontal (---)

**Bloco 4: Exibição de Métricas Chave (KPIs)**


Esta seção utiliza as funções do Streamlit para renderizar o título, textos descritivos e outros elementos visuais que compõem o corpo da página inicial.

```python
# --- EXIBIÇÃO DE KPIs ---

st.header("Visão Geral dos Dados")

# Calcula as métricas a partir do DataFrame completo
valor_total_empenhado = df['valor_empenhado'].sum()
num_orgaos = df['orgao_responsavel'].nunique()
data_inicio = df['data_emissao'].min().strftime('%d/%m/%Y')
data_fim = df['data_emissao'].max().strftime('%d/%m/%Y')

# Cria um layout de colunas para organizar os KPIs
col1, col2, col3 = st.columns(3)

with col1:
    st.metric(
        label="Valor Total Empenhado",
        value=f"R$ {valor_total_empenhado:,.2f}"
    )

with col2:
    st.metric(
        label="Órgãos Analisados",
        value=num_orgaos
    )

with col3:
    st.metric(
        label="Período de Análise",
        value=f"{data_inicio} a {data_fim}"
    )

```

??? note "Clique aqui para ver a explicação para cada linha de código"
    -  `st.header(...)`: Renderiza um cabeçalho de seção.
    -  **Cálculo das Métricas**: A lógica do Pandas é usada para agregar os dados do DataFrame e calcular os valores dos KPIs. `df['valor_empenhado'].sum()` soma todos os valores na coluna, e `df['orgao_responsavel'].nunique()` conta o número de órgãos únicos.
    -  `st.columns(3)`: Cria um layout de grade com três colunas de largura igual. O código subsequente dentro dos blocos `with col1:, with col2:`, etc., será renderizado dentro da coluna correspondente. Esta é a maneira padrão do Streamlit de criar layouts de dashboard mais complexos.   
    -  `st.metric(...)`: Um componente de UI especializado para exibir KPIs. Ele renderiza um `label` (título da métrica) e um value (o valor da métrica) em um formato visualmente destacado. A formatação do valor (ex: `f"R$ {valor_total_empenhado:,.2f}"`) é feita usando f-strings do Python para garantir a exibição correta de moeda e separadores de milhar.