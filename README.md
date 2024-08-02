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


    