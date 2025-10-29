# **A Arquitetura Fundamental: Scripts Centrais da Aplicação**

Esta seção detalha o código que constitui a espinha dorsal do dashboard. O script Pagina_Inicial.py funciona como o motor central e compartilhado que alimenta todas as páginas de análise, sendo responsável pela configuração, carregamento de dados e navegação, garantindo uma experiência de utilizador consistente e um fluxo de dados eficiente.

## **1. Estrutura do Projeto e Configuração Inicial**

A funcionalidade de múltiplas páginas do Streamlit é ativada por uma estrutura de diretórios específica que seu projeto já utiliza corretamente.

 - **Pagina_Inicial.py**: Este é o script principal e o ponto de entrada da aplicação. Ele contém o código para os elementos globais que aparecem em todas as páginas, como a barra lateral, o widget de upload de arquivos e a configuração inicial.

- **pages/**: Este diretório, localizado no mesmo nível que `Pagina_Inicial.py`, armazena os scripts de cada página de análise individual (`1_Resultado_Fiscal.py`, `2_Resultado_Previdenciario.py`, etc.). O Streamlit os detecta automaticamente e cria a navegação.

O primeiro comando executado em `Pagina_Inicial.py` é `st.set_page_config()`, que define atributos globais da aplicação exibidos no navegador.

```python
# Pagina_Inicial.py
import streamlit as st

# Configuração da página (deve ser o primeiro comando Streamlit)
st.set_page_config(
    page_title="Dashboard Financeiro Municipal",
    page_icon="📊",
    layout="wide"  # Utiliza a largura total da página
)
```
??? note "Clique aqui para ver a explicação para cada linha de código"
    -  `import streamlit as st`: Importa a biblioteca principal do Streamlit, convencionalmente com o alias st.
    -  `st.set_page_config(...)`: Esta é a primeira chamada do Streamlit no script e deve ser executada apenas uma vez. Ela configura metadados e o layout da página.
    -  `page_title`: Define o título que aparece na aba do navegador.
    -  `page_icon`: Define o favicon que aparece na aba do navegador. Pode ser um emoji ou a URL de uma imagem.
    -  `layout="wide"`: Configura a página para usar a largura total do navegador, o que é ideal para dashboards com múltiplos gráficos e tabelas.


### 2. O Hub Central: Barra Lateral e Ingestão de Dados

O script `Pagina_Inicial.py` gerencia os componentes persistentes da interface e o ponto de entrada para os dados. A barra lateral, criada com `st.sidebar`, serve como o painel de controle principal.

```python

# Pagina_Inicial.py (continuação)
import pandas as pd

# Título e descrição na barra lateral
st.sidebar.title("Painel de Controle")
st.sidebar.info(
    "Este dashboard apresenta análises sobre as finanças municipais..."
)

# Widget para upload de arquivos
st.sidebar.header("Carregar Dados")
uploaded_file = st.sidebar.file_uploader(
    "Selecione o arquivo Excel com os dados financeiros",
    type=["xlsx", "xls"]
)

```

O widget `st.file_uploader` permite que o utilizador carregue dinamicamente uma planilha do Excel, tornando o dashboard flexível e reutilizável para diferentes conjuntos de dados.

## 3. Gestão de Estado e Otimização com Cache


O Streamlit reexecuta o script a cada interação. Para evitar que o utilizador tenha que recarregar o arquivo de dados a cada mudança de página, é crucial gerir o estado da aplicação. Seu script implementa isso de forma eficiente através de duas ferramentas principais: cache de dados e estado da sessão.

### Carregamento Eficiente com @st.cache_data

O decorador `@st.cache_data` é aplicado a uma função que lê o arquivo Excel. Isso instrui o Streamlit a executar a função apenas uma vez para um determinado arquivo. Em todas as interações subsequentes (como navegar para outra página), o resultado (o DataFrame do Pandas) é recuperado instantaneamente do cache, em vez de ser lido do disco novamente. Isso melhora drasticamente o desempenho e a experiência do utilizador.

### Persistência de Dados com st.session_state

Após o carregamento, o DataFrame é armazenado em `st.session_state`. Este é um objeto semelhante a um dicionário que persiste durante toda a sessão de um utilizador.

```python

# Pagina_Inicial.py (continuação)

@st.cache_data
def load_data(file):
    #... lógica para ler o arquivo Excel com pd.read_excel(file)...
    return df

if uploaded_file is not None:
    # Carrega os dados usando a função com cache
    df_global = load_data(uploaded_file)

    # Armazena o DataFrame no estado da sessão para acesso em outras páginas
    st.session_state['df_global'] = df_global
    
    st.sidebar.success("Arquivo carregado com sucesso!")

```

Ao atribuir o DataFrame a `st.session_state['df_global']`, ele se torna uma variável global acessível a partir de qualquer script no diretório `pages/`, formando a base para todas as análises subsequentes.
