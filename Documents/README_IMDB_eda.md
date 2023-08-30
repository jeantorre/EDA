## 1. Descrição
Quando falamos em dados a maioria das vezes pensamos em números, não é mesmo? Particularmente não tenho qualquer lembrança de um professor de português falando algo como: "Considerando o dado acima, retire o substantivo", por exemplo.  
Porém na área de análise de dados qualquer informação, qualquer mesmo, pode ser considerada um dado! Uma imagem, um número, uma palavra... qualquer coisa, basta apenas nos adequarmos as necessidades do negócio.  
Com isso resolvi aprender uma nova ferramenta e fazer uma "Análise de Sentimento" a partir de textos! Ou seja, a partir de uma avaliação escrita do público, ser capaz de distinguir se tiveram informações boas ou ruins naquela avaliação.  
Para isso faremos a análise das avaliações do site IMDb (Internet Movie Database), que é de propriedade da Amazon. Nele são encontradas informações como atores/participantes, sinopse, gênero, avaliações escritas, e por pontuações, de críticos e usuários, de séries, filmes e muito mais. Ou seja, um ótimo guia para nos orientarmos naquele final de semana com tempo livre!


## 2. Preparação dos dados
A fonte de dados utilizada neste projeto é do Kaggle, que pode ser encontrada [aqui](https://www.kaggle.com/datasets/lakshmi25npathi/imdb-dataset-of-50k-movie-reviews).

### Bibliotecas utilizadas
``` Python
import pandas as pd
import numpy as np
import re
import nltk
from nltk.corpus import stopwords
from nltk.tokenize import word_tokenize
from nltk.stem import SnowballStemmer
from sklearn.feature_extraction.text import CountVectorizer
from sklearn.model_selection import train_test_split
from sklearn.naive_bayes import GaussianNB, MultinomialNB, BernoulliNB
from sklearn.metrics import accuracy_score
from bs4 import BeautifulSoup
import os

import warnings
warnings.filterwarnings("ignore")
```

### Disposição dos dados
Primeiro é necessário entender o comportamento dos dados coletados, descobrindo seu tipo e se há valores faltantes.
```Python
df_imdb.info()
df_imdb['sentiment'].value_counts()
```

Não há dados ausentes no *dataset* e também foi observado que para essa amostragem há 25 mil avaliações positivas e 25 mil negativas. Dessa forma não haverá tendências durante a predição, dentro desta amostragem.

```Python
df_imdb['review'][0]
```

Por se tratar de informações retiradas diretamente do site IMDb, as avaliações vieram com *tags* do HTML da página, ou seja, cheias de marcações como por exemplo: < br>[...]</ br>. Além disso existem caracteres especiais, ou seja, tudo que não é alfanumérico e também deixaremos tudo em letra minúscula.  
Para seguir com todo este tratamento descrito foram criadas as seguintes funções:
```Python
def retirando_tag(texto):
    soup = BeautifulSoup(texto, "html.parser")
    return soup.get_text()

def remover_caracter_especial(texto):
    remover = ''
    for i in texto:
        if i.isalnum():
            remover = remover + i
        else:
            remover = remover + ' '
    
    return remover

def tudo_minuscula(texto):
    return texto.lower()

def limpando_todo_texto(texto):
    texto = retirando_tag(texto)
    texto = remover_caracter_especial(texto)
    texto = tudo_minuscula(texto)

    return texto
```

Após esse pré-tratamento dos dados coletados, agora iniciaremos a utilização da biblioteca NLTK. Como é a primeira vez que eu ouvi falar dela, vale a pena uma pequena explicação...  
Sua tradução é 'Ferramenta de linguagem natural' ('Natural Language Toolkit') que, de acordo com o próprio site, é a plataforma líder para programas em Python que trabalham com dados de linguagem humana. Se pensarmos nos seguintes conjuntos:
* Ciência de dados
* Linguística
* Inteligência artificial

A NLP é, literalmente, a união dos três. Agora vamos as etapas deste processo.  
1. Primeiro é feita a segmentação das frases, por exemplo: 'Eu amo tomar café assim que acordo. Logo em seguida me arrumo e vou pedalando para o parque.'  
Ao segmentarmos essa frase temos:
	* 'Eu amo tomar café assim que acordo.'
	* 'Logo em seguida me arrumo e vou pedalando para o parque.' 

2. Segundo a ""tokenização"" - desculpem, não faço ideia de como traduzir essa palavra haha. Dentro de cada segmentação serão separadas palavra a palavra, dessa forma teríamos:
	* '[Eu] [amo] [tomar] [café] [assim] [que] [acordo]'

3. Terceiro são removidas as palavras que não são essenciais para determinar o sentimento da frase, como por exemplo os artigos do português (o, e, a, as entre outras palavras). Talvez, dentro do nosso exemplo, a frase ficaria assim:
	* '[Logo] [seguida] [arrumo] [vou] [pedalando] [parque].'

4. 'Stemming' que significa algo como a origem da palavra. É responsável por pegar as palavras que foram conjugadas e voltar para o verbo, como por exemplo:
	* '[Logo] [seguida] [arrumar] [ir] [pedalar] [parque].'

Dada essa pequena explicação, foram feitos os seguintes tratamentos:
```Python
nltk.download('stopwords')
nltk.download('punkt')

def removendo_stopword(text):
    stop_words = set(stopwords.words('english'))
    words = word_tokenize(text)

    return [w for w in words if w not in stop_words]
    
def stemming_texto(texto):
    ss = SnowballStemmer('english')

    return ' '.join([ss.stem(w) for w in texto])
```


### Criando o modelo preditivo

```Python
x = np.array(df_imdb['review'])
y = np.array(df_imdb['sentiment'])
cv = CountVectorizer(max_features = 1000)
x = cv.fit_transform(df_imdb['review']).toarray()
trainx, testx, trainy, testy = train_test_split(x, y, test_size = 0.2, random_state = 9)
gnb, mnb, bnb = GaussianNB(), MultinomialNB(alpha = 1.0, fit_prior = True), BernoulliNB(alpha = 1.0, fit_prior = True)
```
No eixo X estão as avaliações e cada palavra, de cada avaliação, se torna uma variável. Serão testados os modelos Gaussiano, Multinomial e Bernoulli.

```Python
trainx, testx, trainy, testy = train_test_split(x, y, test_size = 0.2, random_state = 9)
gnb, mnb, bnb = GaussianNB(), MultinomialNB(alpha = 1.0, fit_prior = True), BernoulliNB(alpha = 1.0, fit_prior = True)
```
80% das amostras foram para treinamento e 20% para teste, como comumente feito.

```Python
gnb.fit(trainx, trainy)
mnb.fit(trainx, trainy)
bnb.fit(trainx, trainy)

ypg = gnb.predict(testx)
ypm = mnb.predict(testx)
ypb = bnb.predict(testx)

print('Gaussian: ', accuracy_score(testy, ypg))
print('Multinomial: ', accuracy_score(testy, ypm))
print('Bernoulli: ', accuracy_score(testy, ypb))

Output:
Gaussian:  0.7843
Multinomial:  0.8309
Bernoulli:  0.8387
```
Os resultados foram satisfatórios chegando a 83,9% de acerto quando utilizado o modelo de Bernoulli.


## 3. Análise dos dados
Para testarmos o modelo recém criado, foi retirado, também do site IMDb, uma avaliação aleatória e testado.
```Python
review = '''
But the movie has plot as well. 
It has characters that I cared about.
From Keanu Reeves' excellent portrayal of Neo, the man trying to come to grips with his own identity, to Lawrence Fishburne's mysterious Morpheus, and even the creepy Agents, everyone does a stellar job of making their characters more than just the usual action "hero that kicks butt" and "cannon fodder" roles.
I cared about each and every one of the heroes, and hated the villains with a passion.
It has a plot, and it has a meaning...and lo and behold, a plot does help the fight scenes!
Just try it, if you haven't seen the movie before.  Watch one of the fight scenes.
Then watch the whole movie. 
There's a big difference in the feeling and excitement of the scenes- sure, they're great as standalones, but the whole thing put together is an experience unlike just about everything else that's come to the theaters.
Think about it next time you're watching one of the more brainless action flicks...think how much better it COULD be.
'''
```

Foram repetidos os mesmos passos de tratamento para esta análise:
```Python
passo_1 = limpando_todo_texto(review)
passo_2 = removendo_stopword(passo_1)
passo_3 = stemming_texto(passo_2)

bow, palavras = [], word_tokenize(passo_3)
for palavra in palavras:
    bow.append(palavras.count(palavra))

dic_palavra = cv.vocabulary_
inp = []
for i in dic_palavra:
    inp.append(passo_3.count(i[0]))
y_pred = bnb.predict(np.array(inp).reshape(1, 1000))
print(y_pred)

Output:
['positive']
```

## 4. Conclusões
Foi criado um modelo de predição com alta taxa de acerto, sendo superior a 80%. Ao ser testado em algumas avaliações de diferentes sites podemos comprovar a eficácia do modelo.


## 5. Referências
* IMDb. **Wikipedia**, 2023. Disponível em: <https://en.wikipedia.org/wiki/IMDb>. Acesso em 29 de ago. de 2023.
* Documentation. **NLTK**, 2023. Disponível em: <https://www.nltk.org/>. Acesso em 29 de ago. de 2023.
* NATURAL LANGUAGE PROCESSING | NLP for Beginners with Python Exercise using NLTK. **Youtube**, 2023. Disponível em: <https://www.youtube.com/watch?v=MpIagClRELI>. Acesso em 29 de ago. de 2023.
* NLTK stop words. **Python Tutorials**, 2023. Disponível em: <https://pythonspot.com/nltk-stop-words/#:~:text=Stop%20words%20are%20words%20that,most%20common%20words%20in%20data>. Acesso em 29 de ago. de 2023.
* BALLISTIC: ECKS VS. SEVER REVIEWS. **Rotten Tomatoes**, 2023. Disponível em: <https://www.rottentomatoes.com/m/ballistic_ecks_vs_sever/reviews?type=user>. Acesso em 29 de ago. de 2023.