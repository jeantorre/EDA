## 1. Descrição
Udemy é uma plataforma digital para ensino e aprendizado que, de acordo com eles, é o *marketplace* líder global do ramo.  
Os dados são de 2011 até 2017, época que ninguém pensaria numa pandemia como a COVID-19 acometeu o mundo em 2020. Hoje, pós pandemia, para nós se tornou algo muito comum o ensino EaD, certificações *online* e também trabalhos de forma remota, sendo cada vez mais normal, e até preferível por muito, esta modalidade a distância.

## 2. Preparação dos dados
A fonte de dados utilizada neste projeto é do Kaggle, que pode ser encontrada [aqui](https://www.kaggle.com/datasets/andrewmvd/udemy-courses).
### Bibliotecas utilizadas
``` Python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import matplotlib.ticker as mtick
import seaborn as sns
import os
import mplcyberpunk

plt.style.use("cyberpunk")

import warnings
warnings.filterwarnings("ignore")
```

### Disposição dos dados
Primeiro é necessário entender o comportamento dos dados coletados, descobrindo seu tipo, se há valores faltantes.
```Python
df_udemy.info()
df_udemy.isnull().sum()
```
Como, em teoria, cada curso deve ter apenas um ID é necessário confirmar se há dados duplicados:
```Python
df_udemy['course_id'].duplicated().any()
Output: True
```
Para confirmar individualmente se todos os valores de todas as colunas são semelhantes:
```Python
df_udemy[df_udemy.duplicated(keep = False)]
```
Foi possível checar que os ID's repetidos de fato possuem todos os dados duplicados. Então foi retirado esses valores duplicados.
```Python
df_udemy = df_udemy.drop_duplicates(subset = 'course_id')
```

### Limpeza e tratamento dos dados

#### Considerações
* A unidade da coluna 'price' é dólar ($).
* A unidade da coluna 'content_duration' é hora.  

Para ser visualizado o crescimento mensal e anual dos dados, foi separada a coluna 'published_timestamp' em 'Year' e 'Month' da seguinte forma:
```Python
df_udemy['Year'] = df_udemy['published_timestamp'].dt.year
df_udemy['Month'] = df_udemy['published_timestamp'].dt.month
```
E logo em seguida retirada as colunas que não serão necessárias para as análises de dados:
```Python
df_udemy = df_udemy.drop(columns = ['url', 'published_timestamp'], axis = 1)
```

## 3. Análise dos dados
Dentro da base de dados utilizados, os dados variam entre 2011 à 2017. Estes foram divididos em 4 assuntos, que são eles: "finanças empresariais", "design gráfico", "intrumento musical" e "desenvolvimento web".
Os temas com os maiores números de cursos são de "Finanças" e "Desenvolvimento Web", ambos com aproximadamente 1,2 mil cursos. Nós, brasileiros, temos o EUA como referência em muitos assuntos, principalmente quando falamos de tecnologia e a educação financeira, pois para nós o que está se tornando tendência hoje já é conhecido "lá fora" a alguns anos atrás e estes dados confirmam isso.  
Fazendo uma rápida pesquisa no próprio site da Udemy, em julho de 2023, com os temas "desenvolvimento web" e "finanças", colocando um filtro apenas para língua portuguesa, encontramos 913 e 247 cursos para estes assuntos, respectivamente. Isso mostra como eles são mais inteirados desses assuntos desde alguns anos atrás.  
No geral os cursos normalmente são "do básico ao avançado", como visto até os dias atuais. Talvez isso seja para alcançar um público maior, e aumentar o faturamento de seus professores.  
Seus preços são baixos, pois como o meio de veiculação é a internet os custos de produção, em linhas gerais, são extremamente inferiores a algo que seria presencial, o que torna possível a "popularização" do ensino de qualidade. A grande maioria dos cursos custam, aproximadamente, U$25. E mesmo assim o *ticket* médio dos assinantes pagantes é de U$72,19, o que gerou uma receita média dos cursos pagos de quase U$180 mil para cada curso. Isso equivale a quase U$600 milhões que a Udemy movimentou, nos anos analisados.  
90% dos cursos com o maior número de inscritos são da área de tecnologia.

## 4. Conclusões
O que presenciamos nos últimos anos no Brasil, principalmente com a pandemia do COVID-19, com aulas remotas, o grande número de plataformas de ensino digital entre outros, pode ser visto - numa escala pequena - nos dados analisados.  
O crescimento exponencial da quantidade de cursos de 2011 a 2017, o baixo preço de venda já que o principal diferencial da internet é a escalabilidade e também a preferência dos cursos voltados a tecnologia onde, do último ano para cá, vivenciamos esse estouro com muita gente, inclusive eu, fazendo a transição de carreira.

## 5. Referências:
* Udemy Courses. **Kaggle**, 2023. Disponível em: <https://www.kaggle.com/datasets/andrewmvd/udemy-courses>. Acesso em 21 de jul. de 2023.
* Melhore vidas por meio do aprendizado. **Udemy**, 2023. Disponível em: <https://about.udemy.com/pt-br/>. Acesso em: 21 de jul. de 2023.
* 913 resultados para "desenvolvimento web". **Udemy**, 2023. Disponível em: <https://www.udemy.com/courses/search/?lang=pt&q=desenvolvimento+web&sort=relevance&src=ukw>. Acesso em 23 de jul. de 2023.
* 247 resultados para "finanças". **Udemy**, 2023. Disponível em: <https://www.udemy.com/courses/search/?lang=pt&q=finan%C3%A7as&sort=relevance&src=ukw>. Acesso em 23 de jul. de 2023.