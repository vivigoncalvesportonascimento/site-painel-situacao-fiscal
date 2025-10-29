# **Scripts das Páginas de Indicadores**

Esta seção detalha os scripts localizados na pasta dashboard/pages/. Cada arquivo corresponde a uma página específica no dashboard e utiliza os scripts gerais para buscar dados e construir as visualizações.

## **1. Resultado Fiscal (dashboard/pages/resultado_fiscal.py)**


Este script constrói a página que exibe os indicadores de resultado fiscal, como o resultado primário e a dívida pública. Ele demonstra como uma página de indicador consome os serviços dos módulos load e utils para apresentar suas análises.

Código-Fonte e Explicação Detalhada

```python

# dashboard/pages/resultado_fiscal.py

import streamlit as st
import pandas as pd
import altair as alt
from load import load_resultado_primario, load_divida_liquida, load_divida_bruta
from utils import create_chart

st.set_page_config(page_title="Resultado Fiscal")
st.title('Análise do Resultado Fiscal')

# --- Carregamento e Preparação dos Dados ---
df_resultado_primario = load_resultado_primario()
df_divida_liquida = load_divida_liquida()
df_divida_bruta = load_divida_bruta()

# Transformação do DataFrame de Resultado Primário para formato longo
df_resultado_primario_long = df_resultado_primario.melt(
    id_vars=['data'],
    value_vars=['resultado_primario', 'resultado_nominal'],
    var_name='Tipo de Resultado',
    value_name='Valor (R$ bi)'
)

# --- Renderização dos Gráficos ---
st.subheader('Resultado Primário e Nominal')
st.markdown('O resultado primário representa a diferença entre as receitas e despesas do governo, excluindo os juros da dívida. O resultado nominal inclui os juros.')

chart_primario = create_chart(
    data=df_resultado_primario_long,
    x_axis='data:T',
    y_axis='Valor (R$ bi):Q',
    x_title='Data',
    y_title='Valor (Bilhões R$)',
    chart_title='Resultado Primário e Nominal Acumulado em 12 Meses',
    tooltip=
)
st.altair_chart(chart_primario, use_container_width=True)

st.subheader('Dívida Líquida do Setor Público')
chart_divida_liquida = create_chart(
    data=df_divida_liquida,
    x_axis='data:T',
    y_axis='divida_liquida_pib:Q',
    x_title='Data',
    y_title='% do PIB',
    chart_title='Dívida Líquida (% do PIB)',
    tooltip=
)
st.altair_chart(chart_divida_liquida, use_container_width=True)

st.subheader('Dívida Bruta do Governo Geral')
chart_divida_bruta = create_chart(
    data=df_divida_bruta,
    x_axis='data:T',
    y_axis='divida_bruta_pib:Q',
    x_title='Data',
    y_title='% do PIB',
    chart_title='Dívida Bruta (% do PIB)',
    tooltip=
)
st.altair_chart(chart_divida_bruta, use_container_width=True)

```

??? note "Clique aqui para ver a explicação para cada linha de código"
    -  `from load import... e from utils import...`: Importa as funções necessárias dos módulos de infraestrutura.
    -  `st.set_page_config(...)` e `st.title(...)`: Configura o título da aba do navegador e o título principal da página.
    -  `df_resultado_primario = load_resultado_primario()`: Chama a função do módulo load para carregar os dados. O mesmo padrão é seguido para os outros DataFrames.
    -  `df_resultado_primario.melt(...)`: Transforma o DataFrame de um formato "largo" para um "longo", que é o ideal para plotagem com Altair, permitindo que 'resultado_primario' e 'resultado_nominal' sejam plotados no mesmo gráfico com uma legenda automática.
    -  `st.subheader(...)` e `st.markdown(...)`: Adicionam um subtítulo e um texto explicativo antes de cada gráfico.
    -  `chart_primario = create_chart(...)`: Chama a função utilitária para criar o gráfico, passando o DataFrame e os mapeamentos de eixos.
    -  `st.altair_chart(chart_primario, use_container_width=True)`: Renderiza o gráfico criado na página. use_container_width=True faz com que o gráfico se ajuste à largura da tela.


## **2. Resultado Previdenciário (dashboard/pages/resultado_previdenciario.py)**


Este script constrói a página que exibe os indicadores relacionados à previdência social, seguindo o mesmo padrão de arquitetura.

```python

# dashboard/pages/resultado_previdenciario.py

import streamlit as st
import pandas as pd
import altair as alt
from load import load_resultado_previdenciario
from utils import create_chart

st.set_page_config(page_title="Resultado Previdenciário")
st.title('Análise do Resultado Previdenciário')

# --- Carregamento dos Dados ---
df_previdencia = load_resultado_previdenciario()

# --- Renderização do Gráfico ---
st.subheader('Déficit Previdenciário (RGPS)')
st.markdown('Representa a diferença entre a arrecadação líquida e a despesa com benefícios do Regime Geral de Previdência Social (RGPS).')

chart_previdencia = create_chart(
    data=df_previdencia,
    x_axis='data:T',
    y_axis='deficit_rgps_pib:Q',
    x_title='Data',
    y_title='% do PIB',
    chart_title='Déficit do RGPS Acumulado em 12 Meses (% do PIB)',
    tooltip=
)
st.altair_chart(chart_previdencia, use_container_width=True)

```

??? note "Clique aqui para ver a explicação para cada linha de código"
    -  `from load import load_resultado_previdenciario`: Importa a função específica para carregar os dados da previdência.
    -  `df_previdencia = load_resultado_previdenciario()`: Carrega os dados em um DataFrame.
    -  `st.subheader(...)` e `st.markdown(...)`: Adicionam o título e a descrição para o gráfico.
    -  `chart_previdencia = create_chart(...)`: Utiliza a mesma função create_chart do módulo utils para gerar o gráfico.
    -  `st.altair_chart(...)`: Renderiza o gráfico na página.

