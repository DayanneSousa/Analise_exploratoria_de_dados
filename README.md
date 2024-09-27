# Analise_exploratoria_de_dados
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import datetime as dt

pip install seaborn

import seaborn as sns

# Carrega o dataset
df_dsa = pd.read_csv("C:/Users/SEPLAG_SPL-06/DSA/23-Cap13/dados/dataset.csv")

df_dsa.shape
df_dsa.head()
df_dsa.columns
df_dsa['Valor_Venda'].describe()

# Verificando se há registros duplicados
df_dsa[df_dsa.duplicated()]

# Verificando de há valores ausentes
df_dsa.isnull().sum()

## Pergunta de Negócio 1:

### Qual Cidade com Maior Valor de Venda de Produtos da Categoria 'Office Supplies'?
df_office_supplies = df_dsa[df_dsa['Categoria'] == 'Office Supplies']
query1 = df_office_supplies.groupby('Cidade')['Valor_Venda'].sum()
cidade_com_mais_vendas = query1.idxmax()
valor_mais_vendas = query1.max()
print(cidade_com_mais_vendas, valor_mais_vendas)

## Pergunta de Negócio 2:

### Qual o Total de Vendas Por Data do Pedido?

total_de_vendas_data = df_dsa.groupby('Data_Pedido')['Valor_Venda'].sum()
print(total_de_vendas_data)

import matplotlib.pyplot as plt

# Plot
plt.figure(figsize = (20, 6))
total_de_vendas_data.plot(x = 'Data_Pedido', y = 'Valor_Venda', color = 'green')
plt.title('Total de Vendas Por Data do Pedido')
plt.show()

# Pergunta de Negócio 3:

### Qual o Total de Vendas por Estado?

### Demonstre o resultado através de um gráfico de barras.

df_total_de_vendas_estado = df_dsa.groupby('Estado')['Valor_Venda'].sum().reset_index()
print(total_de_vendas_estado)

# Plot
plt.figure(figsize = (16, 6))
sns.barplot(data = df_total_de_vendas_estado, 
            y = 'Valor_Venda', 
            x = 'Estado').set(title = 'Vendas Por Estado')
plt.xticks(rotation = 80)
plt.show()

## Pergunta de Negócio 4:

### Quais São as 10 Cidades com Maior Total de Vendas?
df_ten_cities = df_dsa.groupby('Cidade')['Valor_Venda'].sum().reset_index().sort_values(by= 'Valor_Venda', ascending= False).head(10)

plt.figure(figsize = (16, 6))
sns.set_palette('coolwarm')
sns.barplot(data = df_ten_cities, 
            y = 'Valor_Venda', 
            x = 'Cidade').set(title = 'As 10 Cidades com Maior Total de Vendas')
plt.show()

## Pergunta de Negócio 5:

### Qual Segmento Teve o Maior Total de Vendas?
df_segmentos = df_dsa.groupby('Segmento')['Valor_Venda'].sum().reset_index().sort_values(by= 'Valor_Venda', ascending= False)

df_segmentos.head()

# Função para converter os dados em valor absoluto
def autopct_format(values): 
    def my_format(pct): 
        total = sum(values) 
        val = int(round(pct * total / 100.0))
        return ' $ {v:d}'.format(v = val)
    return my_format

# Plot

# Tamanho da figura
plt.figure(figsize = (16, 6))

# Gráfico de pizza
plt.pie(df_segmentos['Valor_Venda'], 
        labels = df_segmentos['Segmento'],
        autopct = autopct_format(df_segmentos['Valor_Venda']),
        startangle = 90)

# Limpa o círculo central
centre_circle = plt.Circle((0, 0), 0.82, fc = 'white')
fig = plt.gcf()
fig.gca().add_artist(centre_circle)

# Labels e anotações
plt.annotate(text = 'Total de Vendas: ' + '$ ' + str(int(sum(df_segmentos['Valor_Venda']))), xy = (-0.25, 0))
plt.title('Total de Vendas Por Segmento')
plt.show()

## Pergunta de Negócio 6 (Desafio Nível Baby):

### Qual o Total de Vendas Por Segmento e Por Ano?
df_vendasea = df_dsa.groupby(['Segmento', 'Data_Pedido'])['Valor_Venda'].sum().reset_index()
print(df_vendasea)
