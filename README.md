# ABI - Automated Bot for Inquiries
Repositório completo do projeto ABI

Documentos:

- ABI.ipynb: notebook com o código completo do projeto;
- Bases (documentação abaixo):
  - base_treino_100.csv
  - base_produto.csv
  - base_teste_perguntas.csv
- imagens: diretório com as figurar utilizadas no notebook.

__ATENÇÃO: para correto funcionamento da interface, baixe todos os arquivos (inclusive a pasta "imagens"), e rode o notebook completo diretamente no ambiente jupyter. A interface gráfica é a última célula do notebook, e ela apenas aparecerá na tela quando a última célula for exectuada. Mas, no corpo do notebook, além dos códigos, há muita informação relevante sobre nossa solução.__
  
# Documentação das bases

A seguir, descreveremos cada uma das bases utilizadas no projeto.

## Base de Perguntas (base_treino_100.csv):

A base de perguntas contém os dados que utilizamos para treinar nosso modelo de machine learning, ou seja, ela é usada para construir a componente inteligente do modelo. 

O objetivo é que o modelo possa determinar o intent de uma pergunta. Temos, então, um problema supervisionado de classificação.

Para resolver o problema, precisamos de uma base de treino supervisionada, isto é, temos que fornecer não somente as perguntas como também seu respectivo intent previamente classificado. Assim, o algoritmo é capaz de "aprender" a identificar o intent de determinada pergunta.

A base de treino é componente fundamental de todo algoritmo de machine learning. Sendo assim, dedicamos especial atenção à sua construção, que foi feita da seguinte maneira:

- Primeiramente, definimos que intents gostaríamos de classificar. Para isso, fizemos um estudo de mercado, com o fim de descobrir quais são as perguntas mais comuns feitas nos marketplaces. De modo a garantir maior generalidade ao algoritmo, priorizamos intents mais genéricos, sobre os quais possam surgir perguntas em diferentes classes de produtos (eletrônicos, eletrodomésticos, vestuário, etc.). No fim, definimos 12 intents diferentes:
	- Cor: as cores disponíveis do produto;
	- Dimensão: as dimensões do produto ou de sua embalagem;
	- Entrega: informações sobre frete, prazo e locais de entrega;
	- Voltagem: informações sobre a tensão de funcionamento do aparelho;	
	- Meios de pagamento: quais as formas de pagamento aceitas;
	- Capacidade: informações sobre especificações de capacidade (volumétrica, de armazenamento, etc.);
	- Garantia: informações sobre as garantias oferecidas para o produto;
	- Nota fiscal: informações sobre a disponibilidade de envio de nota fiscal do produto;
	- Bateria: informações sobre a bateria e/ou forma de alimentação do produto;
	- Acessórios: informações sobre itens que acompanham o produto na embalagem;
	- Estado: se o produto é novo ou usado;
	- Disponibilidade: se o produto está em estoque, e quantas unidades há disponíveis.

- Após definidos os intents, passamos a coletar perguntas relativas a cada um deles. Para isso, fomos às páginas de marketplace, e coletamos perguntas relativas a cada intent, de modo a obter uma base de treino variada e correspondente com a realidade. Para alguns intents, não conseguimos encontrar um número suficiente de perguntas nas páginas de marketplace. Nestes casos, elaboramos algumas perguntas, bem como coletamos possíveis perguntas de amigos e familiares, sempre mantendo a variabilidade e naturalidade da língua.

Um ponto importante ao qual prestamos atenção foi garantir que a base final tivesse um número balanceado de perguntas relativas a cada intent: 100 perguntas para cada um, totalizando 1200 perguntas no total. Tal balanceamento é muito importante para que o modelo não seja enviesado na direção dos intents em maior quantidade na base de treino.

A base, portanto, tem duas colunas e 1200 linhas, onde cada linha é relativa a uma pergunta, e as colunas são:

- "intent" : contém um entre os 12 intents possíveis. Essa é a coluna de target do modelo;

- "pergunta" : contém uma pergunta cuja intenção é dada pelo respectivo intent. Essa é a coluna de features do modelo (que deverá passar por um pré-processamento através de técnicas de NLP).


## Base de produtos (base_produto.csv)

A base de produtos contém os dados que utilizamos para construir a resposta do robô às perguntas, após a determinação do intent relativo à pergunta. Ou seja, esta base é utilizada na construção da componente de consulta do modelo.

Após a determinação do intent de uma pergunta sobre determinado produto pela componente inteligente do modelo, temos de consultar na base de cadastro do produto quais são as informações cadastradas para o intent em questão, para então construir a resposta do robô, que deve conter estas informações cadastradas.

A base de produtos é fundamental para que respostas correta sejam retornadas pelo robô, afinal, não é suficiente o robô dizer que entendeu sobre o que a pergunta se trata: ele deve retornar a resposta completa! 

Certamente a Olist tem uma base de cadastro para cada produto, que, conforme investigamos, são cadastrados no formato json. Como não tivemos acesso a este cadastro, nós mesmos construímos a base de produtos, que tivesse um formato facilmente adaptável a um json. Para isso, coletamos 10 produtos diferentes, e cadastramos em um dicionário seus atributos de acordo com cada um dos 12 intents que definimos anteriormente. Naturalmente, há intents que não estão definidos para alguns produtos, em cujo caso o campo respectivo foi cadastrado como vazio.

A base, portanto, tem 13 colunas (uma para o nome do produto e 12 para os intents) e 10 linhas, onde cada linha se refere a um produto, e cada coluna contém um dicionário com as informações relativas àquele intente do respectivo produto. Como exemplo da estrutura de dicionários, mostramos a seguir as cores disponíveis do produto "Samsung Galaxy J2":

{"azul": 0 ,"dourado":1,"lavanda":26,"preto":104,"púrpura":0},

O que segue o formato: {"nome da cor" : quantidade de produtos disponíveis naquela cor}.

O formato é consistente em todos os registros de determinada coluna, mas varia entre as diferentes colunas (pois cada intent tem informações específicas e variável entre os intents).

## Base de produtos (base_teste_perguntas.csv)

Esta base é bem similar à base de perguntas, em estrutura: temos uma coluna para as perguntas, e outra coluna para os respectivos intents. Sua construção, no entanto, foi um pouco mais livre, pois não nos preocupamos em balancear as classes: simplesmente fomos coletando as perguntas que mais aparecem nos marketplaces, e as classificando.

Uma distinção importante é que, nesta base de testes também temos perguntas com mais de um intent, justamente para testar a habilidade do modelo de classificar perguntas deste tipo.
