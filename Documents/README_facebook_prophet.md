## 1. Preparação dos dados
A fonte de dados utilizada neste projeto é do Kaggle, que pode ser encontrada [aqui](https://www.kaggle.com/competitions/rossmann-store-sales/data?select=train.csv).
### Disposição dos dados
#### df_sale
* Store - ID único das lojas (total de 1.115 filiais)
* DayOfWeek - Representa o dia da semana, onde:
	- 1: Segunda-feira
	- 2: Terça-feira
	- 3: Quarta-feira
	- 4: Quinta-feira
	- 5: Sexta-feira
	- 6: Sábado
	- 7: Domingo
* Date - Data das vendas
* Sales[€] - Faturamento diário (em euros)
* Customers - Quantidade de clientes que entraram na loja
* Open - Representa se a loja está aberta ou fechada, onde:  
	- 0: Fechada
	- 1: Aberta
* Promo - Representa se no dia a loja estava em promoção, onde:
	- 0: Sem promoção
	- 1: Com promoção
* StateHoliday - Representam os feriados estaduais. Algumas observações são que, normalmente todas as lojas ficam fechadas em feriados estaduais. Há apenas algumas exceções. Todas as escolas fica fechadasem feriados públicos e finais de semana. Onde:
    - a: Feriado público
    - b: Páscoa
    - c: Natal
    - 0: Dia normal
* SchoolHoliday - Representa se há um feriado escolar na data, onde:
    - 0: Sem feriado escolar
    - 1: Com feriado escolar  
    
#### df_store
* Store - ID único das lojas (total de 1.115 filiais)
* StoreType - Representam diferentes tipos de loja, que variam entre {'a', 'b', 'c', 'd'}. Neste dataset não há explicações específicas sobre o segmento, ou coisa do tipo, de cada loja.
* Assortment - Representa algo como a "categoria" da loja, onde:
    - a: Básica
    - b: Extra
    - c: Extendida
* CompetitionDistance [m] - Representa a distância até o concorrente mais próximo (em metros)
* CompetitionOpenSinceMonth - Mês aproximado em que este cliente começou sua operação
* CompetitionOpenSinceYear - Ano aproximado em que este cliente começou sua operação
* Promo 2 - Representa uma promoção adicional além da promoção específica da loja (descrita no df_sale). Onde:
	- 0: Sem promoção
	- 1: Com promoção
* Promo2SinceWeek - Semana em que a loja começou a participar da Promo2 (lembrando que dentro de um ano temos 52 semanas)
* Promo2SinceYear - Ano em que a loja começou a participar da Promo2
* PromoInterval - Representa o intervalo de meses que a loja está em promoção dentro do ano corrente
---

## 2. Análise de Dados
### df_sale
* Não há dados faltantes em nenhuma das colunas
* A média de vendas é em torno de 5,8 mil euros, com máxima de 41 mil euros em um único dia
* A quantidade média de clientes diários é por volta de 640, com uma quantidade máxima de 7,3 mil clientes num único dia
* Neste dataset existe uma distribuição uniforme entre os dias da semana (como pode ser melhor observado no histograma da coluna 'DayOfWeek')

Dos dias analisados, 61,85% estavam sem promoção e 38,15% com alguma promoção.  
Os dias em que as lojas funcionaram representam 83% do total. Durante este período, cada filial ficou, em média, 155 dias fechada, totalizando 172,8 mil dias sem funcionamento.  

Após desconsiderar o dias em que as lojas estavam fechadas (pois não existe faturamento), tem-se as seguintes informações:
* A média de vendas, agora, é em torno de 6,9 mil euros. Uma diferença de mil euros
* A quantidade média de clientes diários é por volta de 760. Uma diferença de 120 pessoas  

### df_store
* Há alguns dados faltantes nas seguintes colunas: 'CompetitionOpenSince[Month/Year]','Promo2Since[Week/Year]' e 'PromoInterval'
---
*Tratando esses dados faltantes:*  
Ao isolar o dataframe onde a coluna 'Promo2SinceWeek' é nula, pode ser observado que se quer existem promoções naqueles dias. Com isso essa "falta de informação" é totalmente irrelevante para continuar a análise. Após confirmar que o índice das linhas são iguais para os dados faltantes das colunas 'Promo2Since[Week/Year]' e 'PromoInterval', os dados faltantes foram substituídos pelo valor 0 para continuar a análise.  

Com relação as colunas 'CompetitionOpenSince[Month/Year]' em um mundo real poderia pesquisar individualmente quando cada loja foi inaugurada para preencher individualmente. Pois esse erro pode ter sido causado por uma falta de atenção durante seu preenchimento. Neste caso, não é possível descobrir essa informação.  
Após confirmação que os índice das linhas que possuem dados faltantes nessas colunas são iguais, também será preenchido com o valor 0 para continuar a análise.  

Para a coluna 'CompetitionDistance' a falta de informação também pode ter sido causada por uma falta de atenção durante o preenchimento, quando considerado um mundo real. Neste caso para dar continuidade as análises, os dados faltantes serão preenchidos pela distância média dos concorrentes.

---

* O concorrente mais próximo está a 20 metros de distância de uma filial, enquanto o mais distante está a 75 quilômetros. A distância média é de 5,4 quilômetros

Dos dias analisados, 48,79% estavam sem 'promoção 2' e 51,21% com alguma 'promoção 2'.

### Gráficos
![Matriz Correlação](https://drive.google.com/uc?id=1HnuUBg0dsyGJq-ir0nBWVcq6xFMwMTjb "Matriz Correlação")  
A partir da matriz de correlação, é possível analisar se a variáveis 'Promo' e 'Promo2' estão diretamente ligadas ao faturamento diário 'Sale'. A 'Promo' tem uma correlação fraca a moderada, no valor de 0,37 enquanto a 'Promo2' não tem qualquer influência, pois é uma correlação fraca negativa, no valor de -0,13. E isso também pode ser observado ao comparar com a coluna 'Customers', em que na 'Promo' tem valor de 0,82 e 'Promo2' -0,20.  


![Média venda diária/mês](https://drive.google.com/uc?id=1-NKcwTNdA_q7EV5wFTMq6M5Uc4EVfEST "Média venda diária/mês")  
É possível observar que nos meses de novembro e dezembro ocorrem as maiores médias de vendas diárias.

![Média venda diária](https://drive.google.com/uc?id=1w8kEsk2OyKiMerRIQhtGdnI3ZuqKrARr "Média venda diária")  
Ao analisar dia a dia existem duas tendências de alta, que ocorrem entre os dias 10 a 15 e posteriormente entre os dias 24 a 31.  

![Média venda semanal](https://drive.google.com/uc?id=1Tp6nsq3Wfu9Zwl7wclqZyHcetwQhrElz "Média venda semanal")  
Os finais de semana, representados pelos números 1 e 7 que significam sábado e domingo, respectivamente, mostram as maiores médias de vendas. Enquanto as sexta-feira as vendas atingem o menor patamar.  

![Média cliente semanal](https://drive.google.com/uc?id=1-Ba_2Hu0gdlQJWZdrCczNYCH-ty5zR8e "Média cliente semanal")  
Aos domingos as lojas recebem a maior quantidade de clientes durante a semana. Vale notar que apesar de domingo receber por volta de 600 clientes a menos que as segundas-feira, seu faturamento são bastante parecidos (como pode ser notado no gráfico anterior).

---

### Tratamento para biblioteca
**First things first!**  
Essa biblioteca, em específico, precisa que algumas colunas sejam nomeadas com um exato nome, como por exemplo:
- Coluna de data: renomear para 'ds'
- Coluna a ser feita a previsão: 'y'
- Coluna com as datas comemorativas e feriados: 'holiday'

Após isso basta utilizar a biblioteca que retornara a previsão feita dentro do período especificado.


---

## 3. Conclusões

Neste projeto foram feitos 2 cenários, um otimista e um pessimista. Onde:
- Otimista:
	+ Modelo considera sazonalidades para cálculo de projeções provenientes de férias escolares e feriados
	+ Em períodos anteriores, agosto apresenta menor faturamento quando comparado a julho
	+ Agosto é um período de férias escolares
	+ Projeção de aumento no faturamento mais segura, evitando estoques parado e seus custos envolvidos
- Pessimista:
	+ Modelo não considera sazonalidades para cálculo de projeções proveniente de férias escolares e feriados
	+ Projeção de aumento no faturamento arriscada, podendo ter chances de gerar estoque parado e seus custos envolvidos como de armazenamento (por exemplo: aluguel, impostos, depreciação e maquinários)

---

[![Linkedin](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/jean-torre-44a27914b/)
[![Instagram](https://img.shields.io/badge/Instagram-E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://www.instagram.com/torrej/)
[![Gmail](https://img.shields.io/badge/Gmail-D14836?style=for-the-badge&logo=gmail&logoColor=white)](mailto:jean.torre21@gmail.com)
