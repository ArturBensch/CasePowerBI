# Case PowerBI - Análise de Dados
Case de uma empresa x para Analista de Dados/BI - Utilizando PowerBI<br/>

 [**Link para o Dashboard**](https://app.powerbi.com/view?r=eyJrIjoiMzhmZWUxNGUtMGExMi00NzE0LTgwYWUtZjk2YTEyZjAyNGFjIiwidCI6ImU3ZWJhMzMwLWNlM2ItNGQzNy1hNDI4LTRjMWY4ZDEwYTNmZCJ9)

## Introdução
A empresa “CASE x”, trata-se de uma loja virtual que vende produtos de material de escritório e eletrônicos para o Brasil e para Fora do Brasil, e para que seja possível gerenciar o funcionamento dessa loja, precisamos que seja criado dashboards que ajude a organização a entender a performance da loja para os níveis de Análise estratégica e Análises Operacionais.
Para isso você deverá utilizar as fontes de dados: “Pedidos”, “Devoluções” e “Pessoas”. Essa base possui dados de 2020 a 2023 

Construa dashboards que responda as seguintes perguntas:<br/>
1) Com base nos dados, faça uma análise crítica da performance de venda da loja<br/>
2) Dê visibilidade na variação de vendas entre mês e Ano<br/>
3) Dê visibilidade a margem de lucro das vendas<br/> 
4) Dê visibilidade do impacto da devolução nas vendas e nos lucros<br/>
5) Criar uma medida que calcule a taxa de devolução por produto<br/>
6) Comparar o total de vendas com devoluções acumuladas ao longo do tempo<br/> 
7) Desafio: Criar um gráfico de linha que exiba Vendas x Devoluções YTD para monitorar o impacto das devoluções mês a mês.<br/>
8) Calcular o custo financeiro das devoluções considerando o valor das vendas devolvidas e possíveis descontos. Mostrando o impacto financeiro das devoluções em comparação com o faturamento bruto.<br/>
9) Dê visibilidade a indicadores e comportamentos das devoluções.<br/>
10 )Há algum produto com potencial saída de nosso portifólio? Se sim, justifique.<br/>

Para construir esse case é importante construir um bom storytelling de dados, pois nosso cliente analisará a narrativa dos dashs, bem como a capacidade de análise da base e geração de insights para fins de acompanhamento estratégicos e operacionais;
Construa as análises usando o Power BI, utilizando técnicas de storytelling, você pode usar o power point para fazer a apresentação das conclusões com prints de cada dash ou apresentá-las no próprio power BI, mas é importante apresentar as conclusões dos números e responder as respostas listadas acima. 
 Além de criar os dashs é importante analisar os resultados e nos apresentar as suas conclusões. 

**Importante**: Sinta-se à vontade para explorar novas análises, você não precisa limitar-se apenas às perguntas listadas.

## Extração, Tratamento e Limpeza 
- Antes de iniciar a construção do dashboard, tive que juntar as 3 fontes em uma unica base, assim tranzendo as colunas necessárias. A primeira foi a tabela "Pedidos" e "Devolução", por meio de um `left join` com a coluna *id_pedido*. Por último a tabela de Pessoas, a qual so possuia o campo de gerentes, também com um `left join` por meio da coluna *regiao*.
-  Além disso `eliminei as linhas duplicadas`, removi o cabeçalho que estava na linha 1, alterei a tipagem de alguns dados para trazer o resultado correto.

## Primeira Análise - Overview

- Iniciei minha análise construindo um dashboard que fosse um "overview" das regiões pertencentes ao dataset, e com isso fazendo um drill down para os países e os estados. Esse dash tem como objetivo identificar as regioes de maior e menor faturamento, com os principais KPIs em cima. 
- Ao criar essa visualizaçao, tive que trabalhar primeiramente na base de dados, a qual possuia regiões e países com nomeclaturas erradas ou fora do padrão. Além de corrigir alguns dados que não estavam corretos geográficamente.
- Aqui eu ja exploro a opção me dada no case de criar novas análises!
  
  ![image](https://github.com/user-attachments/assets/13865549-3c5c-4e69-bd50-3e7ef99fa22b)


 ## Respondendo as necessidas do usuário - Performance de Vendas 
- Para poder trazer os objetivos do case e analisar as vendas, fiz um **dax** que me trouxesse os resultados apenas *Faturado*, assim :
  `Faturamento Total = 
CALCULATE(
    SUM('Tabela Sales'[Vendas]), 
    'Tabela Sales'[Devolvido] <> "Sim" || ISBLANK('Tabela Sales'[Devolvido])`
- Fiz uma visualização de Ano-Mês para a variaçao de vendas com o impacto das devoluçoes e o quanto isso afetava a margem de lucro. Assim fazendo o desafio e respondendo as perguntas 2, 3 e 4  da empresa.
  ![image](https://github.com/user-attachments/assets/5e8bcc4c-dfe2-47a0-aaa6-13b53a6c4432)
- Além disso, conseguimos identificar os principais produtos de maior e menor venda, as cidades que mais faturam e a participação dos segmentos. 

 ## Aprofundando o impacto da Devolução 
- Com o objetivo de identificar as variáveis que influenciam na devolução e o resultado dela na empresa, foi feito um dash apenas para isso.
- Sendo assim, foi criado  cálculos, assim resolvendo o objetivo 8 :
  
I) `Valor em Devolução = 
CALCULATE(
    SUM('Tabela Sales'[Vendas]), 
    'Tabela Sales'[Devolvido] = "Sim"
)`               

II) `Impacto Financeiro = 
DIVIDE(
    ('Tabela Sales'[Custo Devoluções]), 
    SUM('Tabela Sales'[Vendas])
)`   

III) `Taxa de Devolução = 
VAR TotalPedidos = COUNT('Tabela Sales'[ID do pedido])
VAR PedidosDevolvidos = CALCULATE(COUNT('Tabela Sales'[ID do pedido]), 'Tabela Sales'[Devolvido] = "Sim")
RETURN DIVIDE(PedidosDevolvidos, TotalPedidos, 0)` 

- Para entender a relação de margem de lucro com o a taxa de devolução, foi feita um grafico de área Ano-Mês, assim respondendo o objetivo 6
  ![image](https://github.com/user-attachments/assets/d3daac7e-a52a-4c11-95ba-1bf6fe609ec8)
  
-Agora para responder a pergunta 10, foi feito dois gráficos, o qual buscam aprofundar o custo da devolução com a taxa, abrindo para sub-categorias e finalmente produtos:
![image](https://github.com/user-attachments/assets/4852dfc3-0ace-447e-86be-176750d2c1fd)
*Com isso vemos que existem alguns produtos com potencias para saída de catálogo ou reformulação deste, por exemplo o Barricks (Mesa para Computador Retangular) o qual tem um custo elevado e possui 100% de taxa de devolução.**

## Análise de Grande Impacto - Descoberta de Outlier
- Tive o interesse de entender possiveis váriaveis que influenciassem na devolução. Com isso, utilizei as datas de pedido e de envio a fim de ver se o tempo de entrega desde a compra poderia fazer com que o cliente desistisse.<br/>
  fiz um cáculo de (data do envio) - (data do pedido), e criei um gráfico de dispersão. Com isso, obtive uma grande surpresa, percebi um **Outlier** na minha base como mostra a imagem:
  
  ![image](https://github.com/user-attachments/assets/9a53ce2b-eeab-4fb7-b482-0639dac24959)
  
     Existiam linhas com datas de envio inferiores a do pedido. Assim, fiz um novo tratamento na base, eliminando essas linhas e me trazendo apenas o resultado coeso.




