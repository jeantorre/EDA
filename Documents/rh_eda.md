## 1. Descrição
Manter um funcionário engajado durante o dia a dia de trabalho não é uma tarefa simples para as empresas e isso com o decorrer de meses, e anos, pode tornar essa convivência mais delicada para este funcionário. Muitas das vezes salários não compatíveis com mercado, falta de benefícios, não reconhecimento do trabalho e outros podem enfraquecer essa relação entre empregador e empregado, mas dada as necessidades um funcionário, o mesmo pode se submeter a coisa que de fora falaríamos "Não acredito!".  
Fazendo uma análise extremamente fria, dentro do capitalismo para (muitas) uma indústria(s) somos apenas uma peça que pode ser substituída quando, e se, necessário. Muitas das vezes até mesmo sem entendimento mútuo entre as partes.  
Dentro dessa frieza eles mesmo não enxergam os prós/contras sobre a retenção desse funcionário. A perda de um funcionário gera impacto econômico e de produtividade que, aparentemente, não é calculado. Acredito que muitas pessoas já receberam aquela resposta meio atravessada, para não dizer mal criada, quando foi pedir um pequeno aumento. Ou até mesmo as respostas genéricas como "mês que vem vemos isso", "ta ruim para todo mundo" etc.  
Contratar e reter funcionários são tarefas extremamente complexas que exigem capital, tempo e habilidades. Vamos a algumas notícias que fortalecem esse cuidado extremo que empresas deveria levar muito em consideração.

### Notícias
* De acordo com o Toogl Blog, em uma publicação feita em 02 de fevereiro de 2023, nos mostra que a maioria de pequenos empresários gastam em torno de 40% das horas de trabalho em tarefas que não geram receitas, como por exemplo a etapa de contratação. Nessa mesma notícia, estudos mostram que empresas gastam de 15 a 25% do salário anual dos funcionários para recrutar um novo candidato
* De acordo com o site Glassdoor, em uma notícia de 5 de julho de 2019, uma empresa média nos Estados Unidos gasta 4 mil dólares para contratar um novo empregado, demorando até 52 dias para ele ocupar, de fato, sua nova posição
* De acordo com a SHRM (*Society for Human Resource Management*), em uma matéria do dia 12 de julho de 2023, o tempo de contratação está aumentando, com empresas demorando 44 dias, em média, para realizar o processo.
* Em uma diferente matéria da SHRM, do dia 11 de abril de 2022, indica que o custo médio para contratação de um novo funcionário é de 4.700 dólares, mas muitos empresários estimam que o custo total pode alcançar de 3 a 4 vezes o salário para aquela determinada posição.

Tudo isso reforça a necessidade de antecipar um possível pedido de demissão, iniciar uma mudança organizacional para reconhecer e incentivar mais o funcionário ou coisas do tipo.

### Premissas
Determinada empresa de RH durante um certo período fez pesquisas de satisfação e coletou os resultados dos funcionários e solicitou que fosse feito o seguinte estudo:
* É possível criar associações entre esse resultados e a saída dos funcionários?
* Por acaso há como prever os pedidos de demissão?

---

## 2. Preparação dos dados
A fonte de dados utilizada neste projeto é do Kaggle, que pode ser encontrada [aqui](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset).
### Disposição dos dados
No próprio site há uma explicação completa do que significa cada coluna, porém vou trazer, aqui, as mais importantes para darmos continuidade ao estudo.  
A coluna 'Attrition' significa, em livre tradução, atrito e isso quer dizer, dentro do contexto de Recursos Humanos, se determinado colaborador saiu ou permaneceu na empresa. Essa será a principal coluna a ser estudada, afinal é nela que está a causa.  
Diferentemente dos empregos brasileiros onde normalmente temos o salário mensal, nos EUA o salário normalmente é dito em base anual ou também convertido para diário. Logo, a coluna 'DailyRate' quer dizer sobre o pagamento diário de determinado funcionário.  
Essa base de dados possui pouco mais de 50 mil dados o que, talvez, torne ela algo próximo a uma realidade profissional. Não há dados faltantes e o "tipo" das colunas estão coerentes, porém existem algumas colunas que não alteram esse estudo, tais como: 'EmployeeCount', que é a contagem de funcionário, ou 'EmployeeNumber', que é algo como a matrícula do mesmo. Por isso essas, e mais algumas outras, serão excluídas. Seguiremos com a análise e se necessário serão feitas mudanças durante o desenvolvimento.

---

## 3. Análise dos dados
![Histograma](https://drive.google.com/uc?id=1Arcyz9AEBewYzkEXo8Ziq81ItKh3Oi8a "Histograma")

Nesses histogramas iniciais podemos tirar algumas conclusões:
* A maior parte dos funcionários possuem idade próxima a 40 anos.
* O atrito - as pessoas que pediram demissão - está em torno de 13%. Apesar de não ser um número alarmante o ideal é manter menor que 10%.
* A grande maioria dos funcionários moram até 10 milhas de distância.
* Mais da metade dos funcionários possuem, pelo menos, ensino superior completo.
* Boa parte dos funcionários recebem até 5 mil dólares por mês.
* Quase 30% dos funcionários fazem hora extra.

Isso é apenas uma análise superficial e qualitativa, posteriormente iremos, cada vez mais, nos aprofundar trazendo valores mais precisos.  
Ainda de forma ampla, vamos descobrir algumas informações entre as pessoas que permaneceram na empresa e das que saíram. Para isso foi separado em 2 DataFrames, agrupados pela coluna 'Attrition' e utilizado o método 'describe()'.
* Os colaboradores que saíram da empresa possuem uma média de idade menor quando comparado aos que ficaram na empresa
* Os funcionários que permaneceram na empresa moram mais perto da empresa
* Os colaboradores que saíram passaram, em média, menos tempo trabalhando por essa empresa
* O salário dos funcionários que ficam na empresa é, em média, maior que dos que saíram (Daily Rate)
* O nível de educação das pessoas que saíram da empresa é um pouco menor das que ficaram

![Matriz Correlação](https://drive.google.com/uc?id=1A2BXrxcbQC4EUvo7DKpUzQfIuRJ0YhTf "Matriz Correlação")

A matriz de correlação é possível entendermos algumas tendências diretamente proporcionais e inversamente proporcionais. Quando o valor apresentado é positivo que dizer que a coluna X linha estão diretamente associados - quando um aumenta o outro aumenta. O contrário também vale, se são valores negativos estão inversamente associados - quando um aumenta o outro diminui. Os valores possíveis variam de -1 a 1.  
Algumas conclusões, ainda, iniciais:
* As linhas 'JobLevel' e 'MonthlyIncome' se associam a coluna 'Age', afinal, em teoria, com o passar do tempo vamos evoluindo dentro de um trabalho, por meio de promoções de salário e cargo.
* A linha 'TotalWorkingYears' se associa com as colunas 'JobLevel' e 'MonthlyIncome' pelos mesmos motivos justificados anteriormente.
* As linhas 'YearsInCurrentRole', 'YearsSinceLastPromotion' e 'YearsWithCurrManager' se associam com a coluna 'YearsAtCompany', o que mostra que quanto mais tempo permanecem na empresa eles permanecem exatamente na mesma posição e não continuam a receber muitas promoções, ficando estagnado naquela posição.  

Dentro da nossa vivência e noção do mundo possuímos algumas teorias sobre o que faz alguém permanecer, ou não, dentro de um trabalho. Que tal utilizarmos nosso conhecimento intrínseco para essa análise?  
**Sim**, análise de dados não são apenas números. É o que eles nos dizem dentro de um contexto!  

![Atrito Idade](https://drive.google.com/uc?id=1IsMusq7hLbrqHnkfON-17P9ffcvR4IDR "Atrito Idade")

Como nenhum pouco suspeito, os mais novos possuem mais tempo para se arriscar, pedindo demissão, correndo atrás de novas oportunidade, errando e acertando cada vez mais. Este histograma mostra que a partir de 24 anos a quantidade de pessoas  que pedem demissão começam a aumentar, "constantemente", e caem depois dos 35 anos.  

Continuando com nosso conhecimento intrínseco... Vamos analisar agora a (não)permanência dos profissionais com as seguintes informações: 'cargo', 'estado civil', 'envolvimento no trabalho' e 'nível de trabalho'.

![Atrito comparações 1](https://drive.google.com/uc?id=1opbmd2mfjLW2dnM7J8uYdnB8RVHAAPYI "Atrito comparações 1")

Um detalhe antes de seguirmos com as análises, a legenda 0 significa "permaneceu na empresa" e 1 significa "saiu da empresa". Sem mais delongas, vamos analisar em ordem:
* Cargo - estatisticamente, em percentual, o cargo que mais possui evasão é o de "Representante de Vendas" (39.76%).  
**Obs**.: Vai ser interessante descobrir alguns dados específico desse cargo para analisarmos mais profundamente o que acontece.
* Estado civil: os solteiros são os que mais se arriscam por ""ter menos responsabilidade"" quando comparado a alguém casado (25,53%).
* Envolvimento no trabalho - também proporcionalmente aos que "saem vs ficam", os que possuem menos envolvimento com a empresa é onde está a maior parte de pedidos de demissão (33,73%).
* Nível de trabalho - os que ainda não tiveram evolução de cargo dentro da empresa são os que mais pedem demissão (26,33%).  

![Atrito comparações 2](https://drive.google.com/uc?id=1GzJEWkUhDbdCmlhSfQGBwM2flk05JwB3 "Atrito comparações 2")

* Distância - sem surpresas os que moram mais longe tendem a pedir demissão dos seus trabalhos.  

Agora vamos afunilar e analisar, mais profundamente, os "Representante de Vendas".
Situação|Idade|Salário diário|Distância de casa|Satisfação no trabalho
-|-|-|-|-
Saiu|27,87|734,09|8,15|2,48
Permaneceu|32,06|862,34|9,00|2,90

Como podemos observar o salário é substancialmente menor, inclusive menor que a média de todos os funcionários que saíram. Além disso possuem uma menor satisfação com o emprego e como visto anteriormente, são mais jovens e tendem a correr mais riscos.

![KDE salário](https://drive.google.com/uc?id=1hfP1Nr4YweIJ8XLXhkH43aUIaCEUaA7- "KDE salário")

Como dito anteriormente, o salário influencia na tomada de decisão de um funcionário entre pedir demissão ou permanecer no emprego.  

Poderíamos acabar por aqui a nossa análise, afinal com o decorrer deste relatório fomos descrevendo informações relevantes e chegamos a conclusões sobre a premissa inicial. Porém vamos continuar a exploração de dados.

![Boxplot salário gênero 1](https://drive.google.com/uc?id=1akna1lH3okAhQDDUAO3iLMULTx4p0WCX "Boxplot salário gênero 1")

Nessa amostragem o salário mínimo, mensal, feminino é um pouco maior que o masculino. A diferença maior ocorro no salário máximo entre os gêneros.

![Boxplot salário gênero 2](https://drive.google.com/uc?id=1-xIjZ8PcH7NGRlEMDVevvtjPCizCRfhJ "Boxplot salário gênero 2")

Com essa visualização gráfica fica bem nítido a diferença de salário para o cargo "Representante de vendas". Além de ser um dos menos salários, mensais, iniciais, é o que tem menor variação entre mínimo e máximo recebido neste cargo, o que, aparentemente, mostra como não há evolução neste cargo dentro da empresa.  

---

### ML para previsão
Para complementar a análise, vamos tentar prever a probabilidade de quando um funcionário deseja sair, ou não, para a partir dessa pesquisa de satisfação a novos funcionários tentar contornar essa possível decisão oferecendo benefícios para o mesmo, pois como visto é bastante prejudicial para a empresa perder talentos.  
Para isso usaremos três metodologias:
* Regressão logística - utilizada para prever saídas binárias, nesse caso permanece/sai.
* Random forest - é utilizado um conjunto de algoritmos onde cria-se decisões baseadas em atributos randômicos.
* Rede neurais artificiais - uma metodologia de *deep learning* que tenta reproduzir a rede neural de um humano para tomar decisões.

Para descobrir qual o melhor modelo utilizaremos os seguintes KPI: *accuracy*, *precision* e *recall*. É importante fazer o uso deles, de forma associada, para de fato encontrar o melhor modelo de previsão. Ao utilizar de forma isolada pode levar a tomadas de decisão inconsistentes. Além desses faremos o cálculo de *f1 score* que faz um balanceamento entre *precision* e *recall*.

Como o foco deste estudo é a EDA, seguem, de forma direta, os resultados a partir dos modelos de ML:

Modelo|Precision|Recall|F1-Score (macro)|Accuracy
-|-|-|-|-
Regressão logística|86%|45%|76%|88%
Random forest|92%|20%|62%|85%
Rede neural|56%|40%|68%|83%

O resultados mostrados acima para *precision* e *recall* são referentes a quantidade de vezes que o modelo acerta que alguém vai sair. Por ser uma base de dados desbalanceada, visto que neste modelo há muito mais pessoas que permanecem no seu emprego invés de pedir demissão, o KPI que decidirá o modelo ideal é o F1-score (macro). Com isso concluímos que foi criado um modelo de regressão logística que acerta 76% das vezes.

---

## 4. Conclusões
Este estudo mostra que mesmo sem utilizar o *Machine Learning* ao nos aprofundarmos no contexto e nos dados, podemos tirar conclusões importantes para tomada de decisões.
* Salário é uma das maiores formas de reconhecimento do profissional - afinal todos precisamos dele para sobreviver nos dias atuais. Cargo de salário mais baixo possui maior rotatividade de funcionários.
* Ainda num mundo onde, preferencialmente, há o trabalho presencial a distância entre o trajeto casa-emprego com o decorrer do tempo vai se tornando massante e um bom motivo para permanecer, ou não, no emprego.
* Se possui um jovem e brilhante talento na empresa busque meios de aumentar a satisfação dele, pois o mesmo está propenso a correr mais riscos.
* Aumento o envolvimento do funcionário com a empresa para que o mesmo se mantenha interessado em permanecer no local.

Essa conclusões não são totalmente novidade, afinal todos já imaginávamos esses mesmos motivos. Porém este estudo comprova, por meio de dados, nosso achismo.

---

## 5. Referências
* The True Cost of Hiring an Employee in 2023. **toggl blog**, 2023. Disponível em: <https://toggl.com/blog/cost-of-hiring-an-employee>. Acesso em 11 de setembro de 2023.
* How To Calculate Cost-Per-Hire. **Glassdoor for Employers**, 2019. Disponível em: <https://www.glassdoor.com/employers/blog/calculate-cost-per-hire/>. Acesso em 11 de setembro de 2023.
* Employers Across Industries Are Taking Longer to Hire. **SHRM**, 2023. Disponível em: <https://www.shrm.org/resourcesandtools/hr-topics/talent-acquisition/pages/employers-across-industries-are-taking-longer-to-hire.aspx>. Acesso em 11 de setembro de 2023.
* Attrition. **Gartner**, 2023. Disponível em: <https://www.gartner.com/en/human-resources/glossary/attrition#:~:text=Attrition%20is%20the%20departure%20of,%2C%20termination%2C%20death%20or%20retirement>. Acesso em 12 de setembro de 2023.
* Day Rate: Flat Fee For a Day of Work, Considerations. **Investopedia**, 2022. Disponível em: <https://www.investopedia.com/terms/d/dayrate.asp#:~:text=The%20day%20rate%2C%20or%20per,per%2Dproject%20or%20seasonal%20basis>. Acesso em 12 de setembro de 2023.
* The Real Costs of Recruitment. **SHRM**, 2022. Disponível em: <https://www.shrm.org/resourcesandtools/hr-topics/talent-acquisition/pages/the-real-costs-of-recruitment.aspx>. Acesso em 13 de setembro de 2023.