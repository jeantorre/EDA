## 1. Descrição
A mundialmente conhecida Amazon, uma empresa fundada em 1994 por Jeff Bezos, é uma das empresas com maior valor de mercado da atualidade. De acordo com a Forbes, em uma matéria publicada em 01 de julho de 2023, o seleto e exclusivo grupo das empresas "trilhionárias" são:
* Apple - US$3,02 trilhões
* Microsoft - US$2,5 trilhões
* Saudi Aramco - US$2,1 trilhões
* Alphabet - US$1,5 trilhão
* Amazon - US$1,3 trilhão
* Nvidia - US$1 trilhão

Hoje sabemos que a Amazon possui diversas frentes, oferecendo serviços como lojas físicas e *online*, serviços "na nuvem" - *cloud computing* -, *streaming* entre outros... e por acaso sabiam que inicialmente, em sua fundação, ela comercializava apenas livros para serem vendidos *online*? Ela não foi a pioneira, pois em 1991 a *Computer Literacy*, uma loja de livros do Vale do Silício, já oferecia esse tipo de serviço, mas a Amazon prometia ser diferente sendo capaz de entregar qualquer livro, a qualquer pessoa em qualquer lugar.  
Em 2007 iniciaram as vendas do Kindle, um dos *e-readers* mais utilizados atualmente, onde estima-se que hoje represente 2/3 dos dispositivos deste segmento. Em 2011 foram introduzidos os primeiros aparelhos com tela sensíveis ao toque e no ano seguinte modelos com iluminação da própria tela.  
Hoje trago uma base de dados que consiste nos 50 livros mais vendidos dos anos 2009 a 2022 pela Amazon.


## 2. Preparação dos dados
A fonte de dados utilizada neste projeto é do Kaggle, que pode ser encontrada [aqui](https://www.kaggle.com/datasets/chriskachmar/amazon-top-50-bestselling-books-2009-2022).
### Bibliotecas utilizadas
``` Python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.ticker as mtick
import seaborn as sns
import os
import mplcyberpunk
import dataframe_image as dfi

plt.style.use("cyberpunk")
```

### Disposição dos dados
Primeiro é necessário entender o comportamento dos dados coletados, descobrindo seu tipo e se há valores faltantes.
```Python
df_amzn.info()
df_amzn.isnull().sum()
```

Não há dados ausentes. Agora é necessário ver se possuem dados duplicados:
```Python
df_udemy['course_id'].duplicated().any()
Output: False
```

Como esta base de dados está totalmente preenchida e sem informações duplicadas, podemos dar prosseguimento da análise.


## 3. Análise dos dados
Inicialmente vamos retirar as colunas que estão no formato de *string* para observar se há correlação entre os números.

```Python
df_amzn_sem_string = df_amzn.drop(columns = ['Name', 'Author', 'Genre'], axis = 1)

fig, ax = plt.subplots()
ax = sns.heatmap(data = df_amzn_sem_string.corr(), annot = True, linewidths = .3, cmap = "RdYlGn", fmt = '.2f')
plt.show()

```
Com o passar dos anos é possível ver correlações positivas entre as colunas 'Year' x 'User Rating' e também 'Year' x 'Review', com os valores de 0,29 e 0,49, respectivamente. Isso, talvez, se dê pelo fato de que com o passar dos anos nós, consumidores, estamos criando o hábito de fazermos uma avaliação sobre os produtos que compramos, dando notas e escrevendo uma avaliação sobre. Afinal... atire a primeira pedra quem na hora de comprar algo novo - e sem recomendação de algum conhecido - não foi pesquisar na internet sobre, não é mesmo?  
Em contra partida é possível observar uma correlação negativa entre as colunas 'Year' x 'Price' com o valor de -0,16. Provavelmente isso é reflexo da inserção dos *e-readers*, como o Kindle, no mercado, o que tornou o custo de fabricação de livros, incluindo os eletrônicos, mais barata e consequentemente com um menor preço para o consumidor final.  

```Python
df_amzn_genre_counts = df_amzn.groupby(['Genre', 'Year']).size().reset_index(name = 'Counts')

fig, ax = plt.subplots(figsize = (10, 9))
ax = sns.barplot(data = df_amzn_genre_counts, x = 'Year', y = 'Counts', hue = 'Genre')
plt.title("Quantidade de livros de Ficção vs Não-Ficção 'Amazon Top 50 livros 2009-2022'")
plt.legend()
plt.show()
```
Ao plotar um gráfico de barras é possível ver que apenas nos anos de 2014 e 2022 os livros de ficção estavam em maior quantidade na lista de "Top 50".

```Python
df_amzn_genre_counts.groupby(['Genre']).sum().plot(kind = 'pie', y = 'Counts', autopct = '%.2f%%', textprops={'color':"black"})
```
No total os livros de Ficção e Não-Ficção representam, respectivamente, 44,57 e 55,43% desta base de dados.  

```Python
df_top_10_author = df_amzn.groupby('Author').size().reset_index(name = 'Counts').sort_values(by = 'Counts', ascending = False, ignore_index = True).head(10)
fig, ax = plt.subplots()
ax = sns.barplot(data = df_top_10_author, x = 'Counts', y = 'Author')
plt.title('Top 10 autores Livros Amazon 2009-2022')
plt.show()
```
Os autores Gary Chapman e Jeff Kinney estiveram na lista de "Top 50" em todos os anos.  
Agora vamos verificar se entre os "Top 10 autores" os mesmos possuem as melhores médias de avaliações, feitas por clientes, no decorrer dos anos.

```Python
df_top_10_author_mean_rating = df_amzn.groupby('Author')['User Rating'].mean().reset_index(name = 'User Rating').sort_values(by = 'User Rating', ascending = False, ignore_index = True).head(10)
df_merge_1 = pd.merge(df_top_10_author, df_top_10_author_mean_rating, on = 'Author', how = 'inner')
```
Os autores Eric Carle e Dav Pilkey apareceram, respectivamente, 10 e 9 vezes entre os "Top 10" e possuem uma média de notas de avaliação de 4,9, de 5,0 máximo. Isso mostra a consistências dos livros publicados por eles dois e como seus públicos gostam de suas obras.  
Será que esses mesmo autores também são os que possuem maior número de avaliações - em números absolutos?

```Python
df_top_10_author_sum_review = df_amzn.groupby('Author')['Reviews'].sum().reset_index(name = 'Sum Reviews').sort_values(by = 'Sum Reviews', ignore_index = True, ascending = False).head(10)
df_merge_2 = pd.merge(df_top_10_author_sum_review, df_top_10_author_mean_rating, on = 'Author', how = 'inner')
```
Novamente o autor Eric Carle aparece no DataFrame unificado entre os "Top 10 autores" e os autores com maior número de avaliações, alcançando a marca de 278 mil avaliações.


## 4. Conclusões
Dentro desta base de dados foi possível constatar que o autor Eric Carle se destaca ao entrar na lista dos "Top 50 livros vendidos na Amazon (2009-2022)" em 71% das vezes, possuindo a incrível nota de 4,9, feitas por leitores, com uma média de 27,8 mil avaliações escritas por clientes a cada ano que ele entrou no "Top 50".


## 5. Referências
* Apple vale US$ 3 trilhões no mercado e pode subir outros US$ 800 bilhões. **Forbes**, 2023. Disponível em: <https://forbes.com.br/forbes-money/2023/07/apple-vale-us-3-trilhoes-no-mercado-e-pode-subir-outros-us-800-bilhoes/>. Acesso em 29 de jul. de 2023.
* Amazon.com. **Britannica**, 2023. Disponível em: <https://www.britannica.com/technology/Kindle>. Acesso em 29 de julh. de 2023.