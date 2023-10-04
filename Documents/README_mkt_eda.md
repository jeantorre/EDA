## 1. Descrição
Quem nunca ouviu aquela famosa frase de marketing "Quem não é visto, não é lembrado", não é mesmo?  
No início da vida de uma empresa, produto ou serviço, demora tempos até que você seja reconhecido no mercado. Seja por qualidade do produto vendido e/ou serviço prestado, por uma logo ou até mesmo um bordão que se torne único.  
Existem algumas maneiras de diminuir esse tempo (que eu não tenho capacidade técnica para me aprofundar).  

Abrindo um grande parênteses aqui...  
Por exemplo, a muitos anos atrás quando a internet era menos popular (de difícil acesso mesmo) e a televisão era nossa principal interação com esse meio mais digitalizado, nós víamos um ator/atriz - até então desconhecido - em todas as programações da emissora sem, se quer, ter lançado a novela.  
* De manhã estava estampado o rostinho dele/dela naquele café da manhã farto com a apresentadora  
* De tarde aparecia uma notícia sobre a cidade em que ele/ela nasceu e/ou cresceu  
* De noite, no horário mais nobre da televisão, falavam sobre toda a história de vida, seus feitos quando criança, sua relação com a família etc  

Lançada a novela, em pouquíssimo tempo ele/ela era amado/odiado (dependendo do seu papel) por toda uma audiência. Tudo isso graças ao marketing feito com a imagem dele/dela pré-lançamento da novela!  
Fechando esse parênteses...  

Uma das etapas principais é saber quem é seu público alvo (persona) para podermos direcionar nossa campanha de marketing de forma efetiva! Podemos fazer isso com base em pesquisas de mercado e posteriormente, a medida que vamos ganhando clientes, com nossa própria base!  

Dado esse pequeno apanhado geral - mega superficial - **vamos ao estudo de hoje!**  

Neste estudo iremos entender sobre o público de um banco americano para podermos separá-los em grupos, a partir da técnica de "clusters" e podermos direcionar campanhas de marketing cada vez mais eficazes e específicas a cada um deles.

---

## 2. Preparação dos dados
A fonte de dados utilizada neste projeto é do Kaggle, que pode ser encontrada [aqui](https://www.kaggle.com/datasets/arjunbhasin2013/ccdata).
### Disposição dos dados
Esta base apresenta, aproximadamente, 160 mil dados. Possui 8.950 de base de clientes e são classificados em 17 características.  
No próprio site há uma explicação completa do que significa cada coluna! Mas vou citar aqui algumas que serão importantes logo no início da análise:
* "BALANCE" - é o montante de dinheiro que o cliente deixa em sua conta
* "ONEOFF_PURCHASES" - quantidade de compras à vista de cada cliente
* "PURCHASE_FREQUENCY" - mostra a frequência que um consumidor realiza uma compra, onde 1 é a frequência máxima de compras
* "CASH_ADVANCE" - é o valor de empréstimo (que é retirado do valor do cartão de crédito), de cada cliente

---

## 3. Análise de Dados
![Histograma](https://drive.google.com/uc?id=1tkEEjNMtS0pXlKZgbCVJ3l840_9htT29 "Histograma")  
Inicialmente vamos avaliar, de forma gráfica, como se comportam os valores de cada coluna:  
* A maior parte dos clientes deixam até 5 mil dólares em suas contas
* A maioria faz compras de até uns 5 mil dólares, à vista
* Temos, de cara, 3 grupos de consumidores:
	- Aqueles que raramente fazem compras utilizando o cartão (variando entre 0 e 0,1 em "PURCHASE_FREQUENCY")
	- Os que usam de forma razoável o cartão (variando entre 0,2 e 0,8 em "PURCHASE_FREQUENCY")
	- Os usuários mais assíduos (entre 0,9 e 1 de "PURCHASE_FREQUENCY")

Para aprofundarmos, é possível fazer análise de cada grupo a fim de tirarmos melhores conclusões da base de clientes e entendermos melhor suas características.  
Para isso deixei lá no GitHub, onde tem o código completo, uma forma de plotar o histogramas de forma individual com a linha de tendência!  

As colunas "CREDIT_LIMIT" e "MINIMUM_PAYMENTS" possuem 1 e 313 valores nulos, respectivamente. Como visto no histograma, elas não possuem uma distribuição normal e por isso utilizaremos o valor de mediana para substituir os valores nulos e dar prosseguimento na análise.  

Esta decisão foi embasada nesta premissa:  

1. Mean imputation is often used when the missing values are numerical and the distribution of the variable is approximately normal.
2. Median imputation is preferred when the distribution is skewed, as the median is less sensitive to outliers than the mean.
3. Mode imputation is suitable for categorical variables or numerical variables with a small number of unique values.

Ela foi retirada do site "Analytics Yogi" e serve como base para uma análise inicial.  

Como o foco desta análise é identificar os grupos de clientes que fazem parte do banco de dados (não velos de forma individual), foi confirmado na coluna "CUST_ID" se há valores duplicados e após essa verificação, confirmando que não há, a mesma foi retirada!  

![Mapa de calor](https://drive.google.com/uc?id=1I07uxJOq3PPYGGmjbP9zqyqLITv8ZFLq "Mapa de calor")  

Após a visualização do gráfico "mapa de calor", é possível constatar colunas fortemente relacionadas e termos algumas observações, tais como:
* "ONEOFF_PUCHASES" e "PURCHASES" > "INSTALLMENTS_PURCHASES" e "PURCHASES"
* "PURCHASE_INSTALLMENTS_FREQUENCY" e "PURCHASE_FREQUENCY" > "ONEOFF_PUCHASES_FREQUENCY" e "PURCHASE_FREQUENCY"
* "CASH_ADVANCE_TRX" e "CASH_ADVANCE_FREQUENCY"

Apenas como contexto, é muito comum no Brasil as compras parceladas (quem não gosta do 12x sem juros, né), enquanto nos EUA essa prática não é usual e por isso que observamos uma relação maior entre quantidade de compras feitas a vista do que parceladas!  

---

### Tratamento para ML
**First things first!**  
Primeiro é necessário normalizar os dados, pois como visto na função .describre() do dataframe há uma diferença muito grande nas escalas. Por exemplo, na coluna "BALANCE" os valores máximo e mínimo são, respectivamente, 19.043 e 0 enquanto na "BALANCE_FREQUENCY" varia entre 1 e 0. Sem o tratamento, o algoritmo pode considerar valores mais altos sendo os mais importantes  
Para isso utilizaremos a função "StandardScaler" da biblioteca scikit-learn. Essa técnica é mais utilizada em bases que possuem mais valores *outliers*, pois em sua fórmula é utilizado valores como média e desvio padrão.  
Após normalizarmos os valores, e separarmos em grupo, lembre de voltar aos valores iniciais para podermos realizar as análises

#### Clusters
"Clusters" significa (no bom e velho português) conjuntos/grupos. Dessa forma quando ouvir falar sobre "clusterização" de algo querem dizer que desejam separar uma determinada quantidade de dados em grupos. Veja na imagem a seguir:  
![Clusters 1](https://drive.google.com/uc?id=1GX55evyfx0IlYppSBKRahbh6mWmoa2Bm "Clusters 1")  
Fonte: https://analise-estatistica.pt/wp-content/uploads/2012/10/diagrama-de-dispersao-analise-de-clusters.png  

Cada grupo neste gráfico (representado por cores diferentes) mostra um "cluster". Neste exemplo o cálculo do KMeans foi feito utilizando 3 clusters!  
Mas como defini-los? Bom, inicialmente o modelo cria centroides nos gráficos de forma aleatória e calcula a distância entre os pontos e esses centroides. Para ter uma melhor separação entre os grupos a distância entre pontos-centroides deve ser o menor possível.  

![Clusters 2](https://drive.google.com/uc?id=14gknBxd9VvGv64_E7Bjc6kUwn0uzMbZe "Clusters 2")  
Fonte: https://media.geeksforgeeks.org/wp-content/uploads/20190812011808/Screenshot-2019-08-12-at-1.13.15-AM.png  

Por exemplo, esses pontos pretos seriam os centroides iniciais, criados de forma aleatória.

![Clusters 3](https://drive.google.com/uc?id=1zn41fba0X1sFBFVu9igcfBwlQouBIiXV "Clusters 3")  
Fonte: https://media.geeksforgeeks.org/wp-content/uploads/20190812011831/Screenshot-2019-08-12-at-1.09.42-AM.png  

Após o modelo recalcular a distância entre os pontos e os centroides sua divisão ficará mais precisa, conforme visto na imagem acima.  

Como a quantidade de clusters é uma variável em que o usuário define, vamos aprender a calcular a quantidade ideal.  

#### Calculando o WCSS e o Método do Cotovelo
WCSS vem do inglês "within-clusters sum-of-squares", onde sua tradução significa "soma dos quadrados dos intra-clusters".  
O resultado do WCSS que mostrará a quantidade ideal de grupos.  
![Met Cotovelo](https://drive.google.com/uc?id=1yHHBpG020C2exW6CUOccUHa0awXoze2n "Met Cotovelo")  
Fonte: https://media.geeksforgeeks.org/wp-content/uploads/20210615172943/wcssplot.png  

A quantidade ideal de "clusters" é quando a diferença entre dois grupos é muito pequena. Na imagem acima, podemos perceber que a partir de 5 grupos a curva vai se tornando menos acentuada até chegar um momento que ela é quase linear. Mas pensando bem... aparentemente com 6 grupos se torna menor a diferença... ou será que 7?  

Bom, achei um artigo da Jessica Temporal que mostra uma forma de calcularmos esse valor e faremos das três maneiras:
* A partir do gráfico de cotovelo - apenas visual
* A partir dos valores calculados de WCSS - analisando valor a valor
* A partir da fórmula que calcula a distância entre um ponto e uma reta

#### Gráfico cotovelo
![Análise 1](https://drive.google.com/uc?id=166FxQ0Z9A2nIMhER3qTCMnvxOYfu57CD "Análise 1")  
Aparentemente em 8 "clusters" a variação da diferença começa bem baixa. A partir disso vamos dividir os clientes em 8 conjuntos e fazermos uma análise:  
* O grupo 2 é o que possui o maior valor, em média, disponível de crédito ($15.570) e também o que paga o valor total da parcela com mais frequência (47,84%)
* O grupo 4 é o que pega o maior valor, em média, de empréstimo ($5.224) e o que menos pagam o valor total da fatura (3,89%). Caso os bancos americanos atuem como os brasileiros, com a taxa de juros para empréstimo sendo bastante alta, tornando este grupo um dos mais rentáveis ao banco (e de maior risco). Coincidentemente o valor do empréstimo é bem próximo ao valor em conta corrente (U$5.098), dando alguma garantia que o banco terá esse valor recebido (de alguma forma)
* O grupo 0 é o que possui o menor valor, em média, em conta corrente (U$104,92) e o segundo grupo que menos compra utilizando o cartão (26,69%)
* O grupo 6 são onde ficam os clientes novos (7,22 anos)

#### WCSS
Temos os seguintes valores para WCSS:
* Cluster: 1 Valor WCSS: 152150.0
* Cluster: 2 Valor WCSS: 127784.45
* Cluster: 3 Valor WCSS: 111973.97
* Cluster: 4 Valor WCSS: 99061.94
* Cluster: 5 Valor WCSS: 91501.61
* Cluster: 6 Valor WCSS: 84826.62
* Cluster: 7 Valor WCSS: 79740.16
* Cluster: 8 Valor WCSS: 74598.75
* Cluster: 9 Valor WCSS: 69959.28
* Cluster: 10 Valor WCSS: 66461.15
* Cluster: 11 Valor WCSS: 63627.11
* Cluster: 12 Valor WCSS: 61365.42
* Cluster: 13 Valor WCSS: 59123.39
* Cluster: 14 Valor WCSS: 57466.99
* Cluster: 15 Valor WCSS: 55817.92
* Cluster: 16 Valor WCSS: 54582.74
* Cluster: 17 Valor WCSS: 52979.21
* Cluster: 18 Valor WCSS: 51931.32
* Cluster: 19 Valor WCSS: 50653.2
* Cluster: 20 Valor WCSS: 49623.84

A partir de 6 grupos as diferenças começa a ficar em torno de 5 mil, então faremos a análise dividindo em 6 "clusters". Algumas conclusões:
* O grupo 0 possui o maior valor, em média, disponível de crédito ($12.493) e também o que paga o valor total da parcela com mais frequência (39,47%)
* O grupo 4 pega o maior valor, em média, de empréstimo ($5.052) e o que menos pagam o valor total da fatura (3,89%)
* Grupo 1 possui o menor valor, em média, em conta corrente (U$112,01) e o segundo grupo que menos compra utilizando o cartão (26,59%)
* Não há um grupo bem definido com os clientes novos. Todos variam, em média, com 11 anos de posse do cartão

#### Quantidade ótima de "clusters" (por fórmula)
O resultado, a partir do cálculo, foi 6. Como na análise anterior, vendo visualmente os valores de WCSS, já fizemos essa análise, prosseguiremos com o estudo.

---

## 4. Conclusões
É de suma importância nesse tipo de aplicação (de "clustering") a participação de alguém mais voltada ao setor de negócios, pois para um Analista de Dados (onde sua principal função é determinar *insights* de forma rápida) entendermos melhor cada grupo.  
Além disso também ter a participação do grupo de Marketing para ter campanhas voltadas para cada segmentação!  
Apesar de ter utilizado 6 grupos neste estudos, particularmente consegui separar e entender um pouquinho mais a fundo 3 grupos:
* O grupo "VIP", são aqueles que possuem um maior limite e também pagam a totalidade da fatura com mais frequência. Uma possível ação a ser tomada pelo banco é aumentar o limite do cartão deste grupo para estimular, ainda mais, as compras visto que é um grupo de baixo risco de inadimplência.
* O grupo de "alto risco", são aqueles que pegam muitos empréstimos e quase não pagam os valores totais de suas parcelas. Mesmo que o saldo na conta seja próximo ao valor do empréstimo, dando alguma garantia ao banco, é necessário permanecer atento a esses clientes para que a dívida fique cada vez maior e, ao final, o banco não receba os valores
* O grupo que não faz tanta movimentação e também não deixa dinheiro em conta corrente. Uma possível explicação para não deixar dinheiro parado é porque preferem tê-lo investido, pois há um maior rendimento. Uma possível ação do banco é mostrar os benefícios a esses usuários ao utilizar o cartão, visto que o banco também fatura por transação.

---

## 5. Referências
* Análise de Clusters. **Análise Estatística**, 2012. Disponível em: <https://analise-estatistica.pt/2012/10/analise-de-clusters.html>. Acesso em 29 de set. de 2023.
* ML | K-means++ Algorithm. **Geeks for Geeks**, 2023. Disponível em: <https://www.geeksforgeeks.org/ml-k-means-algorithm/>. Acesso em 29 de set. de 2023.
* Determinando o número de clusters em mineração de dados. **Acervo Lima**, 2022. Disponível em: <https://acervolima.com/determinando-o-numero-de-clusters-em-mineracao-de-dados/>. Acesso em 29 de set. de 2023.
* Como definir o número de clusters para o seu KMeans. **Medium**, 2019. Disponível em: <https://medium.com/pizzadedados/kmeans-e-metodo-do-cotovelo-94ded9fdf3a9#:~:text=Matematicamente%20falando%2C%20n%C3%B3s%20estamos%20buscando,sendo%20zero%20o%20resultado%20%C3%B3timo.>. Acesso em 30 de set. de 2023.

---

[![Linkedin](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/jean-torre-44a27914b/)
[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://www.instagram.com/torrej/)
[![Gmail](https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:jean.torre21@gmail.com)
