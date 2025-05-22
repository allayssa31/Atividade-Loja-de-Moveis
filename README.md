# Atividade-Loja-de-Moveis

```
select * from item_venda;
select * from entrega;
select * from veiculo;
select * from motorista;
select * from cliente;
select * from venda;
select * from vendedor;
select * from produto;

-- Criando tabelas
CREATE TABLE entrega(
horaEntrega time,
dataEntrega date,
numVen integer,
placa char(7),
codMot integer,
PRIMARY KEY (horaEntrega, dataEntrega),
foreign key (numVen) references venda (numVend),
foreign key (placa) references veiculo (placa),
foreign key (codMot)references motorista(codMot)
)

CREATE TABLE veiculo(
placa char(7) primary key,
capacidade integer
)

CREATE TABLE venda(
numVend integer primary key,
valor_total numeric(11,2),
codVdd integer,
codCli integer
)

CREATE TABLE vendedor(
codVdd integer primary key,
cpf numeric(11),
v_comissao numeric(4,2),
nome varchar(50),
endereco varchar(100)
)

CREATE TABLE motorista(
codMot int primary key,
cpf int,
cnh int,
nome varchar(100),
endereco varchar(150)
)

CREATE TABLE cliente(
codCli integer primary key,
nome varchar(50),
tel char(10),
endereco varchar(100),
cpf numeric(11),
email varchar(50)
)

CREATE TABLE produto(
codPro integer primary key,
custo numeric(11,2),
descricao text,
preco numeric(11,2),
nome varchar(50)
)

CREATE TABLE item_venda(
codPro INTEGER,
numVen INTEGER,
vUnitario NUMERIC(11,2),
qtd INTEGER,
FOREIGN KEY (codPro) REFERENCES produto (codPro),
FOREIGN KEY (numVen) REFERENCES venda (numVend)
);
-- inserindo valores
insert into

entrega(horaEntrega, dataEntrega, numVen, placa, codMot)
values
('07:00:00', '2024-02-23', 1111,'ABC1234',1 ),
('05:45:32', '2024-04-24', 2222,'DEF5678',2 ),
('06:30:00', '2024-12-23', 3333, 'GHI9012',3 ),
('07:00:00', '2024-12-26', 4444, 'ABC1244',2 ),
('09:00:00', '2024-02-26', 5555,'ABC1244',1 ),
('08:00:00', '2024-02-27', 6666,'ABC1244',2 )
;

insert into
veiculo(placa, capacidade)
values
('ABC1234',40),
('DEF5678',20),
('GHI9012', 15)
;
insert into
venda(numVend, valor_total, codVdd, codCli)
values
(1111, 1250.00, 111, 11111),
(2222, 4300.00, 222, 22222),
(3333, 3200.00, 333, 33333)
;
insert into
vendedor(codVdd, cpf, v_comissao, nome, endereco)
values
(111, 10223456734, 10.00, 'José', 'Aracagi'),
(222, 55552021555, 15.00, 'Maria', 'Mari'),
(333, 55484811514, 12.00, 'Marcos', 'Aracagi')
;
insert into motorista(
codMot,
cpf,
cnh,
nome,
endereco
)
values
(1,12345,4321,'João', 'Guarabira'),
(2,98765, 96753, 'Roberto', 'Pirpirituba'),
(3, 13569, 91283, 'Pedro', 'Belém')
;

insert into
cliente(codCli, nome, tel, endereco, cpf, email)
values
(11111, 'Aldo', '96543211', 'Contendas', '532000', 'aldo@santosoliveira.com'),
(22222, 'José', '918720', 'Contendas', '532000', 'jose.silva@gmail.com'),
(33333, 'Mário', '9191827', 'Guarabira', '191726', 'mario@gmail.com')
;
INSERT INTO
produto (codPro, custo, descricao, preco, nome)
VALUES
(9053, 500.00, 'Descrição do produto 1', 600.00, 'Nome do Produto 1'),
(9054, 350.50, 'Descrição do produto 2', 450.00, 'Nome do Produto 2'),
(9055, 720.75, 'Descrição do produto 3', 850.00, 'Nome do Produto 3')
;
insert into
item_venda (codPro, numVen, vUnitario, qtd)
VALUES
(9053, 1111, 10.50, 5),
(9054, 2222, 20.75, 3),
(9055, 3333, 15.20, 8)
;
-- Atualizando o valor total de uma venda
UPDATE venda
SET valor_total = 1500.00
WHERE numVend = 1111;
-- Atualizando o nome e o endereço de um vendedor
UPDATE vendedor
SET nome = 'José Silva', endereco = 'Rua Principal, 123'
WHERE codVdd = 111;
-- Atualizando o telefone de um cliente
UPDATE cliente
SET tel = '987654321'
WHERE codCli = 11111;
-- Atualizando a descrição de um produto
UPDATE produto
SET descricao = 'Nova descrição do produto 1'
WHERE codPro = 9053;

-- Remover uma entrega específica

DELETE FROM entrega
WHERE horaEntrega = '07:00:00' AND dataEntrega = '2024-02-23';
-- Remover um veículo específico
DELETE FROM veiculo
WHERE placa = 'GHI9012';
-- Remover uma venda específica e seus itens associados
DELETE FROM venda
WHERE numVend = 2222;
-- Remover um vendedor específico
DELETE FROM vendedor
WHERE codVdd = 111;
-- Remover um cliente específico
DELETE FROM cliente
WHERE codCli = 11111;
-- Remover um produto específico e seus itens associados em vendas
DELETE FROM produto
WHERE codPro = 9053;


SELECT m.nome, count(numVen) AS qtd_pedidos
FROM entrega e
JOIN motorista m ON e.codMot = m.codMot
WHERE m.nome LIKE '%o' AND e.dataEntrega
GROUP BY m.nome
order by M.nome;

SELECT e.codMot,
COUNT(e.numVen) AS qtd_entregas,
SUM(v.capacidade) AS total_capacidade,
MAX(e.horaEntrega) AS hora_entrega_mais_tarde,
MIN(e.horaEntrega) AS hora_entrega_mais_cedida
FROM entrega e
JOIN veiculo v ON e.placa = v.placa
GROUP BY e.codMot
ORDER BY e.codMot;

SELECT v.nome AS vendedor,
AVG(vd.valor_total) AS media_valor_vendas,
COUNT(vd.numVend) AS total_vendas
FROM vendedor v
JOIN venda vd ON v.codVdd = vd.codVdd
GROUP BY v.nome
HAVING SUM(vd.valor_total) > 3000
ORDER BY v.nome;``
