# Teste-de-Qualificacao
Teste de Qualificao PLD 2022

Questões
A – Com suas palavras explique o que é lavagem de dinheiro.

R: É a introdução no Sistema Financeiro Nacional de recursos ilícitos, originados da prática de crimes como tráfico de drogas, armas, etc, para transformar em recursos aparentemente legais e dificultar a sua origem ou rastreamento.

B – O que é Cartão de Pagamento do Governo Federal (CPGF), e qual a sua finalidade.

R: É um meio de pagamento do Governo Federal similar a um cartão de crédito, com limites e regras específicas, é utilizado para pagamento de despesas próprias que possam ser enquadradas como suprimento de fundos.

C – Quem pode utilizar o CPGF? 

R: O servidor autorizado pelo orgão responsável pelas despesas.

D – Qual a URL onde é possível fazer o download dos arquivos do CPGF?

R: https://www.portaltransparencia.gov.br/download-de-dados

E – Qual a URL da paǵina com a descrição dos campos (ou dicionário de dados) da CPGF?

R: https://www.portaldatransparencia.gov.br/pagina-interna/603393-dicionario-de-dados-cpgf

F – É possível identificar o nome e o documento do portador do CPGF, em todas as movimentações ou há movimentações onde não é possível identificar o portador?

R: Nas movimentações de caráter sigiloso não é possível identificar nem o nome do portador, documento, devido a característica da transação

G – É possível identificar o Órgão do portador do CPGF?

R: Sim.

H - Qual o nome do Órgão cujo código é 20402? 

R: Agência Espacial Brasileira.

I - É possível identificar o Nome e Documento (CNPJ) dos favorecidos pela utilização do CPGF? 

R: Sim.

J – É possível identificar a data e o valor das movimentações financeiras do CPGF, em todas as movimentações? Ou há movimentações onde não é possível identificar as datas e
ou valores? 

R: Não. Nas movimentações de caráter sigiloso somente é possível identificar os valores, a data não.

K - (código) – Qual a soma total das movimentações utilizando o CPGF?

R: CREATE TABLE CPGFOUTSOMA
(
	ID		INT,
	TRANSACAO	DECIMAL(8,2)
);

select sum(valortransacao) from CPGFOUT;

L (código) – Qual a soma das movimentações sigilosas ?

R: CREATE TABLE CPGFOUTSOMASIGILOSA
(
	ID		INT,
	NOMEFAVORECIDO	VARCHAR(200),
	TRANSACAO	DECIMAL (8,2)
);

SELECT NOMEFAVORECIDO, SUM(TRANSACAO) FROM CPGFOUTSOMASIGILOSA WHERE NOMEFAVORECIDO = 'Sigiloso' GROUP BY NOMEFAVORECIDO

M (código) – Qual o Órgão que mais realizou movimentações sigilosas no período e qual o valor (somado)?

R: SELECT TOP 1 NOMEORGAO, count(NOMEFAVORECIDO) as Sigilosas, SUM(VALORTRANSACAO) as Total 
FROM SIGILOSASOMA
WHERE NOMEFAVORECIDO = 'Sigiloso'
GROUP BY NOMEORGAO 
ORDER BY Total DESC

N (código) – Qual o nome do portador que mais realizou saques no período? Qual a soma dos saques realizada por ele? Qual o nome do Órgão desse portador?

R: SELECT TOP 1 NOMEPORTADOR, COUNT(TRANSACAO), SUM(VALOR_TRANSACAO) as Total, NOMEORGAO
FROM TRANSACOES
WHERE TRANSACAO LIKE %saque%
GROUP BY NOMEPORTADOR
ORDER BY Total DESC

O (código) – Qual o nome do favorecido que mais recebeu compras realizadas utilizando o CPGF?

R: SELECT TOP 1 NOMEFAVORECIDO, count(TRANSACAO)
FROM TRANSACOES
WHERE TRANSACAO LIKE %COMPRA%
GROUP BY NOMEFAVORECIDO
ORDER BY NOMEFAVORECIDO DESC

P - Descreva qual a abordagem utilizada para desenvolver o código para os ítens de K a O.

R: Foi pensado em utilizar o Query para realizar a busca de dados já inseridos em um banco de dados. Na letra K, foi pensado em pegar a coluna referente aos valores das transações e somar todas. Na letra L, foi pensado em definir primeiramente dentro da query, os resultados somente com a identificação de sigilosos e realizado a soma dos valores das movimentações. Na letra M, foi pensado em realizar a filtragem sobre o nome do favorecido para sigiloso e o de mais repetição, colocando o TOP 1 para que retornasse o resultado com mais repetições, assim, somando o valor da transação somente dos campos com essa definição e também, informando o nome do órgão responsável. Na letra N, foi pensado em filtrar pelo nome do portador, com a contagem de repertições de saque e com o nome do órgão, juntamente com o %LIKE%, na qual, busca no campo transação, a palavra saque. Na letra O, foi pensado em filtrar pelo nome do favorecido, juntamente com o count para a contagem de compras realizadas e o %LIKE% para a coluna TRANSAÇÕES ser procurada somente valores com compra.
