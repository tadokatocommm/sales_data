## Pandas

# Importar
    import pandas as pd

# Ler o dataset
    df = pd.read_csv('caminho/dataset.csv')

# Ler as 5 primeiras linhas ou colocar a quantidade de linhas que queira ler

    df.head()
    df.head(30)

# Informações do dataset

    df.info()


# Nomes das colunas do dataset

    df.columns

# Contagem de numeros nulos

    df.isna().sum()

# Informaçoes de media, min, max, count...

    df.describe()

# Renomear colunas (Cria uma view, não modifica o dataset principal)

df.renomeado = df.rename(columns={
    'OS':'Operacional System'
})

# Agora se quiser alterar o dataset principal (inplace=True)
    df.rename(columns={
    'OS':'Operacional System'
}, inplace=True)


# Selecionando uma coluna inteira

    df['Brand']

# Mostrar todas os registros UNICOS em uma coluna

    df['Brand'].unique()

# Selecionando uma linha do data set

    df.iloc[1]


# Removendo uma coluna

    del df['Unnamed: 0']


# Removendo mais de uma coluna

    pl.drop(columns=['Fluxo', 'Total de séries', 'Training Stress Score®'], inplace=True)
    pl.head()

 
# Salvando um dataset (Para não vim com o 'Unnamed: 0', coloque index=False)

    df.to_csv('Laptop_pre.csv', index=False)


# Agora filtrando uma coluna, vou guardar em uma variavel uma coluna e que nessa coluna me retorne todos os valores booleanos TRUE que estou comparando (==). Resumindo, cria uma seleção baseada em uma coluna e guarda numa variavel. Dessa forma, obtém todas as linhas do DataFrame onde a coluna 'Hard_Disk_Capacity' é igual a '256 GB SSD'. E se alguma coluna tem algum acento, o metodo query() não funciona devido a sintaxe

    selecao = df['Hard_Disk_Capacity'] == '256 GB SSD'
    df[selecao] 

    OU

    OS = df.query('OS == "Windows 11 Home"')
   
# Quando dar o comando "selecao2 = df.query('Hard_Disk_Capacity == "256 GB SSD"')" ele vem pelo o index da tabela original, agora caso queira que venha com os index resetados, utilize o método .reset_index() e caso queira remover o index original que vem na tabela, já que cria coloque drop=True

    OS.reset_index(drop=True)


# Se quiser fazer duas comparaçoes ou mais, utilize o operador and

    os = df.query('Hard_Disk_Capacity == "256 GB SSD" and Rating >= 4.3')
    os.head(30).reset_index(drop=True)

# Utilizando o operador | (OU)

    od = df.query('Hard_Disk_Capacity == "256 GB SSD" or Hard_Disk_Capacity == "512 GB SSD"')
    od.head(50).reset_index(drop=True)

# Caso queira fazer 3 comparacoes em duas colunas, compare a condicao da primeira coluna entre parenteses e depois a outra comparacao. O que acontece se não colocar os parenteses: A expressão Hard_Disk_Capacity == "512 GB SSD" and Rating > "4.3" será avaliada primeiro devido à precedência do operador and sobre o operador or. Isso pode levar a resultados incorretos, porque você está misturando condições de forma não desejada.

    od = df.query('(Hard_Disk_Capacity == "256 GB SSD" or Hard_Disk_Capacity == "512 GB SSD") and Rating > 4.7')
    od.head().reset_index(drop=True)

# Caso queira agrupar duas colunas e somar uma delas para ver quais categorias vendeu mais                               variavel = dataset.groupby('Nome da tabela')['Nome da outra tabela'].sum()


    categoria_preco = df.groupby('Product Category')['Total Revenue'].sum()

# Colocando em ordem descrescente

    categoria_preco_asc = categoria_preco.sort_values(ascending=False)

# Resetando o index 
    categoria_df = categoria_preco_asc.reset_index()

# Renomeando as colunas da variavel (não é o dataset)
    categoria_df.columns = ['Categoria dos produtos', 'Total de vendas']


# Para descobrir o maior e o menor indice de uma coluna                                                           variavel = v_query.loc[v_query['Nome da coluna'].idxmax()] 

    melhor_regiao = regiao_df.loc[regiao_df['Total de vendas'].idxmax()]
    pior_regiao = regiao_df.loc[regiao_df['Total de vendas'].idxmin()]
    

# Concatenando para mostrar o indice maior e menor de uma coluna

    desempenho_extremos = pd.concat([melhor_regiao.to_frame().T, pior_regiao.to_frame().T], ignore_index=True)

# Transformando o date time 

    df['Date'] = pd.to_datetime(df['Date'])

# Injetando o dia, mes e ano no dataset 

    df['Year'] = df['Date'].dt.year
    df['Month'] = df['Date'].dt.month
    df['Day'] = df['Date'].dt.day

# Como mostrar as vendas do dia (date)

    vendas_diarias = df.groupby(df['Date'].dt.date)['Total Revenue'].sum()

# Como mostrar as vendas semanais (week)

    vendas_semanais = df.groupby(df['Date'].dt.to_period('W'))['Total Revenue'].sum()
    vendas_semanais

 # Como mostrar as vendas mensais (month)

    vendas_mensais = df.groupby(df['Date'].dt.to_period('M'))['Total Revenue'].sum()
    vendas_mensais


# Como tirar a media

    media_diaria = vendas_diarias.mean()
    media_diaria


# Como tirar o desvio padrao

    desvio_padrao_vendas_diarias = vendas_diarias.std()
    desvio_padrao_vendas_diarias


# Para descobrir picos de vendas. Primeiro, calcula a media de vendas e o desvio padrao. Segundo, coloca na forma do limiar (media + 2 × desvio padrao). Se as vendas diárias forem maiores que o limiar, essas vendas serão consideradas picos de vendas.

    picos_vendas = vendas_diarias[vendas_diarias > (media_diaria + 2 * desvio_padrao_vendas_diarias)].reset_index()


# Caso queira saber quanto representa o valor total das vendas por método de pagamento em porcentagem. Em resumo, está pegando o valor total das vendas de cada método de pagamento e dividindo pelo valor total de todas as vendas de todos os métodos de pagamento, multiplicando o resultado por 100 para converter para porcentagem.

    valor_total_metodos_pagamento_percent = (valor_total_metodos_pagamento / valor_total_metodos_pagamento.sum()) * 100


# Caso queira consultar nessa coluna, duas outras colunas. Por exemplo, tenho uma Regiao e nessa regiao quero consultar as categorias que mais vendeu. Aqui estamos selecionando apenas as linhas do DataFrame original df que correspondem à região da Europa.

    df_europe = df[df['Region'] == 'Asia']

# E em seguida voce agrupa as colunas que voce quer consultar

    europe_category = df_europe.groupby('Product Category')['Total Revenue'].sum().reset_index()

# Se quiser consultar quanto um produto, regiao, categoria vendeu em um determinado periodo

    df_american = df[df['Region'] == 'North America']
    america_category_month = df_american.groupby(df['Date'].dt.to_period('W'))['Total Revenue'].sum().reset_index()
    american_asc = america_category_month.sort_values(by= 'Total Revenue', ascending = False).reset_index(drop=True)
    american_asc