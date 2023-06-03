# Case de Análise de Dados no SQL

## Tecnologias utilizadas
- [MySQL](https://www.mysql.com/)

**Descrição**: Projeto de análise de dados feito com base no vídeo da [Hashtag Treinamentos](https://www.youtube.com/watch?v=hprufLQkS2M), a fim de adquirir e aprimorar conhecimentos na linguagem de consulta SQL.
O CASE exposto no vídeo traz como background uma locadora de filmes (HASHTAG MOVIES), na qual guarda informações sobre seus filmes e clientes em um banco de dados MYSQL. Onde iremos fazer uma breve análise com objetivo de descobrir a média de preço e desempenho dos alugueis.

## Indo diretamente a análise.

-- CASE 1. Descobrir o preço médio de aluguel dos filmes.

```SQL
SELECT ROUND(AVG(preco_aluguel),2) FROM filmes;
```
[](/imagens/mediaPrecoAluguel.PNG)

-- CASE 2. Descobrir qual é o preço médio para cada gênero de filme.

```SQL
SELECT
	genero,
    round((preco_aluguel),2) AS preco_medio,
    COUNT(*) as qtd_filmes
FROM filmes
GROUP BY genero;
```
![](/imagens/mediaPrecoGenero.PNG)

-- CASE 3: Fazer a mesma análise do CASE 2, mas considerando apenas os filmes com ANO_LANCAMENTO igual a 2011.

```SQL
SELECT
	genero,
    round((preco_aluguel),2) AS preco_medio,
    COUNT(*) as qtd_filmes
FROM filmes
WHERE ano_lancamento = 2011
GROUP BY genero;
```
![](/imagens/mediaPrecoGenero2011.PNG)

-- CASE 4. Análise de desempenho dos alugueis. Identificar quais aluguéis tiveram nota acima da média.

```SQL
SELECT ROUND(AVG(nota),2) FROM alugueis; -- 7,94

SELECT * FROM alugueis WHERE nota >= (SELECT ROUND(AVG(nota),2) FROM alugueis);
```
![](/imagens/mediaDesempenhoAlugueis.PNG)

-- CASE 5. Crie uma view para guardar o resultado do SELECT abaixo.

```SQL
CREATE VIEW resultado AS
SELECT
	genero,
	ROUND(AVG(preco_aluguel), 2) AS media_preco,
    COUNT(*) AS qtd_filmes
FROM filmes
GROUP BY genero;

SELECT * FROM resultado;
```
![](/imagens/criandoView.PNG)

## Perguntas complementares

- Qual a RECEITA DA EMPRESA?
```SQL
SELECT sum(preco_aluguel) AS receita_total
FROM filmes;
-- R$ 156.89
```
![](/imagens/receitaTotal.PNG)

- Qual o nível de satisfação dos clientes?
```SQL
SELECT ROUND(AVG(nota),2) FROM alugueis;
-- 7.94 Podemos considerar que os clientes estão satisfeitos. Porém devemos olhar mais a fundo quais genêros de filmes mais atraem clientes, além de pensar na possibilidade de trazer filmes recém-lançados mais rapidamente e talvez criar algum beneficio de fidelidade.
```
![](/imagens/satisfacaoClientes.PNG)

- Em quais gêneros de filme investir com base em sua popularidade?
```SQL
SELECT genero, round(avg(nota),2) FROM filmes
INNER JOIN alugueis
ON filmes.id_filme = alugueis.id_filme
GROUP BY genero;
-- Apesar da média de avaliação ser parelha entre os gêneros de filmes, vemos que há uma prefêrencia maior entre os gêneros de Ação, Aventura e Arte. Sendo assim podemos ficar atentos ao mercado e dar uma atenção maior ao público que se enquadra nesta média.
```
![](/imagens/PopularidadeFilmes.PNG)

- Quais filmes tem nota acima da média ?
```SQL
SELECT distinct titulo FROM filmes
	INNER JOIN alugueis
    ON filmes.id_filme = alugueis.id_filme
WHERE nota > (SELECT AVG(nota) FROM alugueis);

-- Temos muitos filmes com notas acima da média, principalmente a saga do HARRY POTTER, além de RIO 2, MOTOQUEIRO FANTASMA E HAPPY FEET.
```
![](/imagens/filmesMaioresMedias.PNG)
