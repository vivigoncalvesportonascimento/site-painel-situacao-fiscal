# **A Arquitetura Fundamental: Scripts Centrais da Aplicação**

Esta seção aborda os scripts que são a espinha dorsal do projeto. Eles lidam com a inicialização da aplicação, o carregamento de dados e as funções utilitárias compartilhadas, garantindo que o dashboard funcione de maneira coesa e eficiente.

## **1. Página Inicial (dashboard/Pagina_Inicial.py)**

Este é o primeiro script executado pelo Streamlit e serve como a página de boas-vindas do dashboard. Ele é responsável por configurar aspectos globais da aplicação e apresentar o conteúdo introdutório.


```python
# dashboard/Pagina_Inicial.py

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

# --- TÍTULO PRINCIPAL ---
st.title('Painel de Controle Fiscal de Minas Gerais')

# --- TEXTO INTRODUTÓRIO ---
# Utiliza st.markdown para renderizar texto com formatação.
st.markdown(
    """
    Bem-vindo ao Painel de Controle Fiscal de Minas Gerais, uma ferramenta interativa 
    desenvolvida para proporcionar uma visão clara e detalhada sobre as finanças do estado. 
    Através de visualizações de dados e análises, este painel busca facilitar o entendimento 
    sobre a arrecadação, os gastos públicos, os resultados fiscais e previdenciários, 
    e a trajetória da dívida pública.

    **Como Navegar:**

    - Utilize o menu na barra lateral à esquerda para selecionar o indicador que deseja analisar.
    - Cada página contém gráficos e dados específicos sobre o tema escolhido.
    - Passe o mouse sobre os gráficos para obter informações detalhadas.

    Este projeto tem como objetivo promover a transparência e o acesso à informação, 
    permitindo que cidadãos, pesquisadores e gestores públicos acompanhem de perto 
    a saúde fiscal do estado.
    """
)

```

??? note "Clique aqui para ver a explicação para cada linha de código"
    -  `import streamlit as st`: Importa a biblioteca principal do Streamlit, que é a base para a criação de toda a interface do usuário. O alias st é uma convenção padrão.
    -  `import pandas as pd`: Importa a biblioteca Pandas, essencial para a manipulação e análise de dados em formato de tabelas (DataFrames). O alias pd também é uma convenção.
    -  `from pathlib import Path`: Importa a classe Path do módulo pathlib. Esta classe permite criar objetos que representam caminhos de arquivos de uma forma que funciona de maneira consistente em diferentes sistemas operacionais (Windows, macOS, Linux).
    -  `st.set_page_config(...)`: Esta é a primeira e única chamada do Streamlit que deve ser executada para configurar os metadados e o layout de toda a aplicação.
    -  `page_title`: Define o texto que aparece na aba do navegador.
    -  `page_icon`: Define o ícone (favicon) que aparece ao lado do título na aba. Pode ser um emoji ou a URL de uma imagem.
    -  `layout="wide"`: Configura a página para usar a largura total da tela do navegador, o que é ideal para dashboards que exibem múltiplos gráficos e tabelas.
    -  `st.title(...)`: Renderiza o texto fornecido como o título principal na página, com uma formatação de destaque.
    -  `st.markdown(...)`: Renderiza texto usando a sintaxe Markdown. Isso permite formatar o texto de maneira simples, como usar **...** para negrito, criar listas com hifens e organizar o conteúdo em parágrafos. É usado aqui para exibir a mensagem de boas-vindas e as instruções de navegação.

## **2. Carregamento de Dados (dashboard/load.py)**

Este módulo centraliza toda a lógica de acesso aos dados. Sua responsabilidade é ler os arquivos CSV da pasta data-raw e disponibilizá-los para as outras partes da aplicação, utilizando um sistema de cache para otimizar o desempenho.


```python

# dashboard/load.py

import streamlit as st
import pandas as pd
from pathlib import Path

@st.cache_data
def get_data(file):
    """
    Função genérica para carregar dados de um arquivo CSV.
    Utiliza o cache do Streamlit para evitar releituras desnecessárias do disco.
    """
    project_root = Path(__file__).parents
    file_path = project_root / 'data-raw' / file
    return pd.read_csv(file_path, sep=';', encoding='latin-1')

# Funções específicas para cada arquivo de dados
def load_resultado_primario():
    return get_data('resultado_primario.csv')

def load_divida_liquida():
    return get_data('divida_liquida.csv')

def load_divida_bruta():
    return get_data('divida_bruta.csv')

def load_resultado_previdenciario():
    return get_data('resultado_previdenciario.csv')

```

??? note "Clique aqui para ver a explicação para cada linha de código"
    -  `@st.cache_data`: Este é um "decorador" do Streamlit e é a chave para o bom desempenho do dashboard. Quando aplicado a uma função, ele instrui o Streamlit a armazenar o resultado dessa função em um cache na memória. Se a função for chamada novamente com os mesmos argumentos de entrada (neste caso, o mesmo file), o Streamlit retornará o resultado do cache instantaneamente, em vez de executar a função novamente. Isso evita a releitura lenta dos arquivos CSV do disco a cada interação do usuário.
    -  `def get_data(file)`:: Define uma função genérica e reutilizável para carregar um arquivo CSV.
    -  `project_root = Path(__file__).parents`: Esta linha determina o caminho para o diretório raiz do projeto de forma dinâmica. Path(__file__).parents refere-se ao diretório pai do diretório atual, que neste caso aponta para a pasta raiz do projeto.
    -  `file_path = project_root / 'data-raw' / file`: Constrói o caminho completo para o arquivo CSV desejado, juntando o caminho da raiz, o diretório data-raw e o nome do arquivo passado como argumento.
    -  `return pd.read_csv(...)`: Lê o arquivo CSV usando o Pandas e retorna um DataFrame.
    -  `sep=';'`: Especifica que as colunas no arquivo CSV são separadas por ponto e vírgula.
    -  `encoding='latin-1'`: Especifica a codificação de caracteres do arquivo, importante para dados em português que possam conter acentuação.
    -  `def load_resultado_primario()`: (e funções similares): São funções "wrapper" específicas para cada conjunto de dados. Elas chamam a função genérica get_data com o nome do arquivo correto, tornando o código nas páginas de indicadores mais limpo e legível.

## **3. Funções Utilitárias (dashboard/utils.py)**

Este módulo serve como uma "caixa de ferramentas" para o projeto. Ele contém funções auxiliares reutilizáveis, principalmente para a criação de gráficos padronizados com a biblioteca Altair e para a formatação de números.


```python

# dashboard/utils.py

import altair as alt
import pandas as pd

def format_number(num):
    """Formata um número para o padrão brasileiro (milhar com ponto, decimal com vírgula)."""
    if num % 1 == 0:
        return f"{int(num):,}".replace(",", ".")
    else:
        return f"{num:,.2f}".replace(",", "X").replace(".", ",").replace("X", ".")

def create_chart(data, x_axis, y_axis, x_title, y_title, chart_title, tooltip):
    """Cria um gráfico de linha padronizado com Altair."""
    chart = alt.Chart(data).mark_line(
        point=alt.OverlayMarkDef(color="blue", size=40)
    ).encode(
        x=alt.X(x_axis, type='temporal', title=x_title),
        y=alt.Y(y_axis, type='quantitative', title=y_title),
        tooltip=tooltip
    ).properties(
        title=chart_title
    ).interactive()

    return chart.configure_axis(
        labelFontSize=12,
        titleFontSize=14
    ).configure_title(
        fontSize=16
    )

```

??? note "Clique aqui para ver a explicação para cada linha de código"
    -  `import altair as alt`: Importa a biblioteca Altair para visualização de dados, com o alias alt.
    -  `def format_number(num):`: Define uma função para formatar números no padrão brasileiro.
    -  `if num % 1 == 0:`: Verifica se o número é inteiro.
    -  `return f"{int(num):,}".replace(",", ".")`: Se for inteiro, formata com separador de milhar e depois substitui a vírgula padrão por ponto.
    -  `else:`: Se não for inteiro.
    -  `return f"{num:,.2f}".replace(...)`: Formata com duas casas decimais e separador de milhar. A cadeia de .replace() inverte os separadores para o padrão brasileiro.
    -  `def create_chart(...)`: Define uma função centralizada para criar gráficos de linha com um estilo consistente.
    -  `chart = alt.Chart(data).mark_line(...)`: Inicia a criação do gráfico, especificando o DataFrame e o tipo de marcação (linha). point adiciona um marcador em cada ponto de dados para facilitar a interação.
    -  `.encode(...)`: Mapeia as colunas do DataFrame para as propriedades visuais do gráfico (eixos x, y, e a caixa de dicas).
    -  `.properties(title=chart_title)`: Define o título principal do gráfico.
    -  `.interactive()`: Habilita funcionalidades interativas como zoom e pan.
    -  `return chart.configure_axis(...).configure_title(...)`: Aplica configurações de estilo, como o tamanho da fonte dos rótulos e títulos, antes de retornar o objeto do gráfico.

