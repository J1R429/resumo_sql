- baixar o postgreSQL

# CRIAR BANCO DE DADOS
NO PG ADMIN
    - em POSTGRESQL (Nº DA VERSÃO) -> DATABASE -> clicar com o botão direito, create -> database ->


NO TERMINAL
    - comando 
        CREATE DATABASE NOME_BANCO;
    - visualizar os bancos de dados existente
        \l
    - comando para deletar
        DROP DATABASE nome_databade

# CRIANDO UMA TABELA
- tipos de dados mais utilizados no sql
    - integer - numero inteiro
    - real - numero com casas de decimais, ate 6 digitos
    - serial - numero inteiro positivo, com soma automatica utilizado para id
    - numeric - definição de precisão por casas decimal
    - varchar - texto com tamanho pré-determinado
    - char - sabe exatamente o valor do campo, exemplo cpf que sempre terá 11 caracteres
    - text - não tem ideia do tamanho do texto
    - boolean
    - date - YYYY-MM-DD
    - time - HH24:MM:SS
    - timestamp - DATA E HORA

- comando criar tabela
```sql
CREATE TABLE aluno (
	id SERIAL,
	nome VARCHAR (255),
	cpf CHAR(11),
	observacao text,
	idade INTEGER,
	dinheiro NUMERIC (10,2),
	altura REAL,
	ativo boolean,
	data_nascimento date,
	hora_aula time,
	matricula_em timestamp
);
```

# VERIFICAR TABELA
```sql
select *
from aluno
```

# inserir valores na tabela 
```sql
insert into nome_tabela apelido_se_tiver nome_coluna
    VALUES
-- não é necessário avisar os campos pq já se sabe a ordem
-- ou

insert into nome_tabela (nome_campo_dos _calores,)
    VALUES (cada_valor,)
-- é informado a ordem dos campos
```

```sql
-- exemplo
insert into aluno (nome, 
				   cpf, 
				   observacao, 
				   idade, 
				   dinheiro, 
				   altura, 
				   ativo, 
				   data_nascimento, 
				   hora_aula,
				  matricula_em)
VALUES ('diogo', 
		'12345678901', 
		'aluno possuí diabetes', 
		35, 
		100.50, 
		1.81, 
		true, 
		'1984-08-27',
	   '17:30:00',
	   '2020-02-08 12:32:45')
```

# UPDATE
```sql
update nome_tabela
    set nome_coluna
    where condição
```

```sql
UPDATE aluno
    SET nome = 'Nico',
    cpf = '10987654321',
    observacao = 'Teste',
    idade = 38,
    dinheiro = 15.23,
    altura = 1.90,
    ativo = FALSE,
    data_nascimento = '1980-01-15',
    hora_aula = '13:00:00',
    matricula_em = '2020-01-02 15:00:00'
WHERE id = 1;
-- dessa forma o digo será substituido pelo nico
```

# EXCLUSÃO DE REGISTRO
```sql
DELETE 
    FROM nome_tabela
    where condição
```

```sql
-- neste exemplo inclui novamente o diogo
delete
	FROM ALUNO
	WHERE NOME = 'diogo'
-- necessário entender que depois do = se tem um flitro; por isso se for necessário mais de um comando necessário colocar em ()
```

# CONSULTAS COM FILTRO
- o * faz a consulta de toda a tabela
- ou pode substituir o * pelo nome da coluna

```sql
select *
    from aluno

-- ou
select nome
    from aluno
```
- se pode mudar de forma temporaria o nome das colunas por um apelido incluindo <as apelido> 

- o filtro se baseia no where
    - é utilizado com o 
        where nome_campo = valor
```sql 
SELECT *
    FROM aluno
 WHERE nome = 'diogo';
```

- <> e != são sinais para busca de diferente
- LIKE utilizado como parecido ele é utilizaod com o _ e o %
    - o _ é utilizado num caracter faltante
    - o % é utilizado no inicio ou no final da palavra, pesquisando ate aquele ponto 

```sql 
SELECT *
    FROM aluno
 WHERE nome LIKE '_iogo';

 -- ou

SELECT *
    FROM aluno
 WHERE nome = '%o';
```

- o NOT LIKE - é para ignorar o item nformado

- NULL
    - campos não preenchidos, nulos
    - IS NULL ou IS NOT NULL 
- =
- > ou >=
- < ou <=

- OPERADORES LÓGICOS
- Usados para unir expressões simples em uma composta

- AND: Verifica se duas comparações são simultaneamente verdadeiras
- OR: Verifica se uma ou outra comparação é verdadeiras
- BETWEEN: Verifica quais valores estão dentro do range definido
- IN: Funciona como multiplos ORs
- LIKE e ILIKE: Comparam textos e são sempre utilizados em conjunto com o operador %, que funciona como um coringa, indicando que qualquer texto pode aparecer no lugar do campo
- ILIKE ignora se o campo tem letras maiúsculas ou minúsculas na comparação
- IS NULL: Verifica se o campo é nulo

```sql
SELECT *
    FROM aluno
  WHERE nome LIKE 'D%'
    AND cpf IS NOT NULL;
```

```sql
SELECT *
    FROM aluno
  WHERE nome LIKE 'Diogo'
    OR nome LIKE 'Rodrigo';
```

```sql
SELECT *
    FROM aluno
  WHERE nome LIKE 'Diogo'
    AND nome LIKE 'Nico%';
```

# CHAVES PRIMÁRIAS 
- "uma coluna, ou grupo de colunas, que pode ser usada para identificar uma linha da tabela"

```sql
CREATE TABLE curso (
    id INTEGER,
        nome VARCHAR(255)
-- criar tabela para teste
)
```


```sql
-- Na nova tabela, informaremos que os campos não podem receber o valor nulo com o comando NOT NULL 
CREATE TABLE curso (
    id INTEGER NOT NULL,
        nome VARCHAR(255) NOT NULL
);
```
- Se tentarmos executar novamente o comando INSERT INTO curso (id, nome) VALUES (NULL, NULL); , receberemos uma mensagem de falha, informando que não pode haver valores nulos nestes campos. Com essa limitação que impomos, a mensagem de sucesso na inclusão dos dados só aparecerá se preenchermos os dois campos.


```sql
-- verificar tabela criada

select * from curso
```


```sql
-- criando valores tabela curso
INSERT INTO curso (id, nome) VALUES (1, 'HTML');
INSERT INTO curso (id, nome) VALUES (2, 'Javascript');
```

- Ao executarmos o código acima, percebemos a possibilidade de adicionar no nosso banco de dados um novo curso com o mesmo "id", o que também é ruim. Para impedir que isso aconteça, quando criarmos uma tabela, temos que informar que cada "id" precisa ser único. Faremos isso com o comando UNIQUE .
- Então determinamos que "id" do curso não pode ser nulo e será um número único, que são as mesmas características de uma chave primária. Portanto podemos trocar NOT NULL UNIQUE, que identifica o campo "id", por PRIMARY KEY e teremos as mesmas funções.
```sql
CREATE TABLE curso (
    id INTEGER PRIMARY KEY,
    nome VARCHAR(255) NOT NULL
)
```

- Com o "id" sendo uma chave primária, ele terá as mesmas características de antes, então se tentarmos colocar o mesmo "id" para os dois cursos aparecerá a mensagem de erro que vimos anteriormente. Só conseguimos adicionar os cursos de "HTML" e "Javascript" ao usarmos id's diferentes.


# CHAVES ESTRANGEIRAS
- "uma limitação para especificar que o valor de uma coluna (ou múltiplas colunas) precisa corresponder a alguma linha de outra tabela".

```sql
-- criar uma tabela de teste e incluir valores
CREATE TABLE alunx (
    id SERIAL PRIMARY KEY,
    nome VARCHAR(255) NOT NULL
);


INSERT INTO alunx (nome) VALUES ('Diogo');
INSERT INTO alunx (nome) VALUES ('Vinícius');
```

- como relacionar essa nova tabela ALUNX com a tabela de CURSO?
    1- criar uma terceira tabela que terá os dados relacionados adicionando uma chave estrangeira
    ```sql
    CREATE TABLE alunx_curso (
        alunx_id INTEGER,
        curso_id INTEGER,
        PRIMARY KEY (alunx_id, curso_id),
        
        FOREIGN KEY (alunx_id) REFERENCES alunx (id),

        FOREIGN KEY (curso_id) REFERENCES curso (id)

        --FOREIGN KEY (campo_na_tabela_origem)
        --REFERENCES tabela_destino (campo_tabela_destino)
        --REFERENCES tabela_destino (campo_tabela_destino)
    );
    ```
    - a chave estrangeira serve para impedir a inclusão de dados ids que não existem nas tabelas de mesclagem

    2- adicionar o cursos dos alunos pelos id do aluno e do curso
    ```sql
    INSERT INTO alunx_curso (aluno_id, curso_id) VALUES (1,1);
    INSERT INTO alunx_curso (aluno_id, curso_id) VALUES (2,1);
    -- os alunos 1 e 2 estão no curso 1 que é de HTML
    ```

# juntar as tabelas
- utilizar o join - comando que une os dados existentes na tabela "A" com os dados existentes na tabela "B"

- Até o momento, para sabermos em qual o curso o aluno está matriculado, usamos o SELECT * FROM nome_da_tabela WHERE id = para consultar individualmente o id do aluno e do curso em cada tabela. Contudo, essa não é uma boa forma para visualizarmos os dados, então aprenderemos como fazer essa consulta em uma única query.

```sql
SELECT *
  FROM alunx
  JOIN alunx_curso ON alunx_curso.alunx_id = alunx.id
  JOIN curso ON curso.id = alunx_curso.curso_id
```
- Ao executarmos esse código, percebemos que o comando JOIN aluno_curso ON aluno_curso.aluno_id = aluno.id concatenou as informações das tabelas "aluno" e "aluno_curso" de forma correspondente, na qual o registro do aluno com "id = 1" está na mesma linha que "aluno_id = 1" e o mesmo acontece com o aluno com o "id = 2".
- Para adicionarmos a tabela "curso", complementaremos nosso código de busca com o comando JOIN curso ON curso.id = aluno_curso.curso_id . Ao executá-lo novamente, teremos um resultado com mais clareza, no qual veremos que o Diogo está fazendo o curso de HTML, conforme a linha 1, e o Vinícius também está cursando HTML, conforme a linha 2.

- FROM alunx: Aqui estamos dizendo de onde queremos buscar informações, nesse caso na tabela chamada "alunx".
- JOIN alunx_curso ON alunx_curso.alunx_id = alunx.id: Isso significa que queremos juntar (ou combinar) informações da tabela "alunx" com a tabela "alunx_curso". Estamos unindo essas tabelas onde o "alunx_id" na tabela "alunx_curso" é igual ao "id" na tabela "alunx".
- JOIN curso ON curso.id = alunx_curso.curso_id: Aqui estamos fazendo outra junção, agora entre a tabela "curso" e a tabela "alunx_curso". Estamos combinando essas tabelas onde o "id" na tabela "curso" é igual ao "curso_id" na tabela "alunx_curso".


- aluno que não esta matriculado em nenhum curso irá desaparecer na junção das tabelas
    - a criação de curso sem alunos tambem irá criar a mesma situação
- para realizar a junção de todos os campos mesmo com a informação NULL necessário usar o:
    - left join
    - right join
    - full join

```sql
-- vai tarzer todos os alunos mesmo com null
SELECT aluno.nome as "Nome do Aluno",
        curso.nome as "Nome do Curso"
    FROM aluno
LEFT JOIN aluno_curso ON aluno_curso.aluno_id = aluno.id
LEFT JOIN curso ON curso.id = aluno_curso.curso_id
```

```sql
-- vai trazer todos os cursos mesmo com null
SELECT aluno.nome as "Nome do Aluno",
        curso.nome as "Nome do Curso"
    FROM aluno
RIGHT JOIN aluno_curso ON aluno_curso.aluno_id = aluno.id
RIGHT JOIN curso ON curso.id = aluno_curso.curso_id
```

```sql
-- vai trazer todas as informações
SELECT aluno.nome as "Nome do Aluno",
        curso.nome as "Nome do Curso"
    FROM aluno
FULL JOIN aluno_curso ON aluno_curso.aluno_id = aluno.id
FULL JOIN curso ON curso.id = aluno_curso.curso_id
``` 

- Existe outro tipo de junção que relaciona todos os dados da tabela "A" com todos os dados da tabela "B". Para essa junção, escreveremos o select sem passar pela tabela "aluno_curso".
- O CROSS JOIN , que colocamos no nosso código, não precisa de um campo para realizar o vínculo, poque ele vai extrair os dados da tabela "aluno" (FROM aluno) e mesclar à tabela curso (CROSS JOIN curso), como se cada aluno estivesse matriculado em todos os cursos, ou seja, esse comando multiplica os dados da tabela "A" pelos dados da tabela "B".
```sql
SELECT aluno.nome as "Nome do Aluno",
       curso.nome as "Nome do Curso"
    FROM aluno
CROSS JOIN curso
```

# DELETE DE ELEMENTO VINCULADO A CHAVE ESTRANGEIRA

```sql
-- NECESSÁRIO NO MOMENTO DA CRIAÇÃO DA TABELA DE JUNÇÃO MENCIONAR A EXCLUSÃO POR CASCADE
-- SE NÃO QUISER O DELETE PRECISAR COLOCAR RESTRICT
CREATE TABLE aluno_curso (
    aluno_id INTEGER,
    curso_id INTEGER,
    PRIMARY KEY (aluno_id, curso_id),

    FOREIGN KEY (aluno_id)
     REFERENCES aluno (id)
     ON DELETE CASCADE,

    FOREIGN KEY (curso_id)
     REFERENCES curso (id)

);
```

# UPDATE DE ELEMENTO VINCULADO A CHAVE ESTRANGEIRA

```sql
CREATE TABLE aluno_curso (
    aluno_id INTEGER,
        curso_id INTEGER,
        PRIMARY KEY (aluno_id, curso_id),

        FOREIGN KEY (aluno_id)
         REFERENCES aluno (id)
         ON DELETE CASCADE
         ON  UPDATE CASCADE,

        FOREIGN KEY (curso_id)
         REFERENCES curso (id)

);
```


# ORDENAR UMA CONSULTA
- criando a tabela e incluindo valores para o teste

```sql
CREATE TABLE funcionarios(
    id SERIAL PRIMARY KEY,
    matricula VARCHAR(10),
    nome VARCHAR(255),
    sobrenome VARCHAR(255)
);

INSERT INTO funcionarios (matricula, nome, sobrenome) VALUES ('M001', 'Diogo', 'Mascarenhas');
INSERT INTO funcionarios (matricula, nome, sobrenome) VALUES ('M002', 'Vinícius', 'Dias');
INSERT INTO funcionarios (matricula, nome, sobrenome) VALUES ('M003', 'Nico', 'Steppat');
INSERT INTO funcionarios (matricula, nome, sobrenome) VALUES ('M004', 'João', 'Roberto');
INSERT INTO funcionarios (matricula, nome, sobrenome) VALUES ('M005', 'Diogo', 'Mascarenhas');
INSERT INTO funcionarios (matricula, nome, sobrenome) VALUES ('M006', 'Alberto', 'Martins');
```

- a clausula ORDER BY realiza a ordenação pela coluna apontada (nome ou numero de posição) de forma crescente
    a inclusão do DESC no final realiza decrescente 
```sql
SELECT * 
    FROM funcionarios
    ORDER BY nome;
-- ou

SELECT * 
    FROM funcionarios
    ORDER BY 4; -- sobrenome é a 4ª coluna

```

# LIMITANDO DADOS DE CONSULTA
```sql
SELECT *
  FROM funcionarios
  ORDER BY nome
LIMIT 5;
```

- Se precisarmos do retorno de dados que não estão no começo da tabela, ou seja, exibir o resultado após avançar algumas linhas, codamos o comando OFFSET . Essa cláusula pula a quantidade de linhas que estipularmos antes de exibir a busca
- Ao executarmos o código com OFFSET 1 , percebemos que o registro do "id=1" não aparece, ou seja, com esse comando informamos ao programa que queríamos a exibição de cinco resultados, com LIMIT 5 , que estão após a linha "1" do nosso banco de dados, com OFFSET 1 .
```sql
SELECT *
  FROM funcionarios
  ORDER BY id
 LIMIT 5
OFFSET 1;
```

# FUNÇÕES DE AGREGAÇÃO
- para integrar registros em um único resultado.

-- COUNT - Retorna a quantidade de registros
-- SUM -   Retorna a soma dos registros
-- MAX -   Retorna o maior valor dos registros
-- MIN -   Retorna o menor valor dos registros
-- AVG -   Retorna a média dos registros

```sql
SELECT COUNT (id)
  FROM funcionarios;
```

```sql
SELECT COUNT (id),
       SUM(id), -- soma os valores
       MAX(id), -- maior valor
       MIN(id), -- menor valor
       ROUND(AVG(id),0) -- media
  FROM funcionarios;
```


- agrupamento se repetição
- clausual distinct dados não se repetem
```sql
SELECT DISTINCT
       nome,
       sobrenome,
       COUNT(*)
  FROM funcionarios
  ORDER BY nome;
```

- mas quando precisar utilizar alguma função de agregação é necessário utilizar o GROUP BY e não pode utilizar o DISTINCT 
```sql
SELECT
       nome,
       sobrenome,
       COUNT(*)
  FROM funcionarios
  GROUP BY nome, sobrenome
  ORDER BY nome;
```

# FILTROS EM CONSULTA AGRUPADAS
- EXEMPLO, tentaremos descobrir quais cursos não têm alunos matriculados. Essa informação pode ser utilizada para, por exemplo, um relatório ou a exclusão do curso.
```sql
SELECT *
    FROM curso
    LEFT JOIN aluno_curso ON aluno.curso_id = curso.id
    LEFT JOIN aluno ON aluno.id = aluno_curso.aluno_id
```


-  após o GROUP BY , adicionaremos o HAVING , que é um comando de filtro assim como o WHERE . A diferença da filtragem com HAVING é a possibilidade de utilizar as funções de agrupamento que aprendemos, enquanto o WHERE filtra a partir dos campos.
```sql
SELECT *
    FROM curso
    LEFT JOIN aluno_curso ON aluno.curso_id = curso.id
    LEFT JOIN aluno ON aluno.id = aluno_curso.aluno_id
    --WHERE curso.nome = 'Javascritp'
GROUP BY 1
    HAVING COUNT (aluno.id) = 0
```


```sql
```

Em SQL, os parênteses podem ser usados em diferentes contextos para diferentes propósitos:

    - Funções: Muitos comandos SQL têm funções embutidas que realizam operações específicas, como calcular valores, manipular strings, datas, etc. Essas funções frequentemente têm argumentos que são passados entre parênteses. Por exemplo, SUM(coluna) usa a função SUM para somar os valores de uma coluna específica.

    - Cláusulas e expressões: Em algumas cláusulas, como na cláusula WHERE ou HAVING, você pode ter expressões condicionais ou lógicas dentro dos parênteses para definir condições mais complexas. Por exemplo, (idade > 18 AND cidade = 'São Paulo') é uma expressão condicional que usa parênteses para agrupar as condições.

    - Definição de tabelas temporárias ou colunas: Em certos bancos de dados, ao criar tabelas temporárias ou definir colunas com tipos de dados específicos, os parênteses são usados para envolver informações como nomes de colunas, tipos de dados e tamanhos. Por exemplo, ao criar uma tabela temporária, pode-se fazer algo como CREATE TEMPORARY TABLE temp_table (id INT, nome VARCHAR(50)).