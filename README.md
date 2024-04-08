# Projeto SQL: Análise de Dados de Livros

## Objetivos do Estudo
O objetivo deste estudo é realizar uma análise dos dados fornecidos sobre livros, editoras, autores e avaliações de clientes. As principais tarefas incluem encontrar o número de livros lançados após 1 de janeiro de 2000, identificar a editora com o maior número de livros com mais de 50 páginas, encontrar o autor com a média mais alta de classificação de livros, e calcular o número médio de avaliações entre usuários que classificaram mais de 50 livros.

## Consultas SQL

1. **Número de livros lançados após 1 de janeiro de 2000:**
```sql
SELECT COUNT(*)
FROM books
WHERE publication_date > '2000-01-01';
```

2. **Número de avaliações e classificação média para cada livro:**
```sql
SELECT b.title, COUNT(r.rating_id) AS num_reviews, AVG(r.rating) AS avg_rating
FROM books AS b
LEFT JOIN ratings AS r ON b.book_id = r.book_id
GROUP BY b.book_id, b.title;
```

3. **Editora que lançou o maior número de livros com mais de 50 páginas:**
```sql
SELECT p.publisher, COUNT(b.book_id) AS num_books
FROM books AS b
JOIN publishers AS p ON b.publisher_id = p.publisher_id
WHERE b.num_pages > 50
GROUP BY p.publisher_id
ORDER BY num_books DESC
LIMIT 1;
```

4. **Autor com a média mais alta de classificação de livros (com pelo menos 50 classificações):**
```sql
SELECT a.author, AVG(r.rating) AS avg_rating
FROM authors AS a
JOIN books AS b ON a.author_id = b.author_id
JOIN ratings AS r ON b.book_id = r.book_id
GROUP BY a.author_id
HAVING COUNT(r.rating_id) >= 50
ORDER BY avg_rating DESC
LIMIT 1;
```

5. **Número médio de avaliações entre usuários que classificaram mais de 50 livros:**
```sql
SELECT AVG(sub.num_reviews) AS avg_reviews
FROM (
    SELECT username, COUNT(DISTINCT book_id) AS num_reviews
    FROM ratings
    GROUP BY username
    HAVING COUNT(DISTINCT book_id) > 50
) AS sub;
```

## Conclusões

1. O número de livros lançados após 1 de janeiro de 2000 é X.
2. Para cada livro, encontramos o número de avaliações e a classificação média.
3. A editora que lançou o maior número de livros com mais de 50 páginas é Y.
4. O autor com a média mais alta de classificação de livros, com pelo menos 50 classificações, é Z.
5. O número médio de avaliações entre usuários que classificaram mais de 50 livros é W.

Estas informações fornecem insights valiosos para entender os padrões de publicação, avaliação e classificação de livros pelos usuários. Isso pode ajudar na tomada de decisões estratégicas para o desenvolvimento de novos produtos ou melhorias no serviço existente.
