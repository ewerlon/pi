# Projeto integrador Banco de Dados
DER (Diagrama Entidade-Relacionamento) para um sistema de PDV (Ponto de Venda) com pelo menos quatro entidades. O cenário escolhido envolve a venda de produtos, relacionando clientes, vendas e itens de venda.
## Entidades
1.	### Cliente: Armazena informações dos clientes.
	• Cliente (ClienteID, Nome, CPF, Telefone, Email) Um cliente pode realizar várias compras.
2.	### Venda: Registra as transações de vendas.
	• Venda (VendaID, ClienteID, DataVenda, TotalVenda) Registra as transações de vendas.
3.	### Produto: Contém os produtos disponíveis.
	• Produto (ProdutoID, Nome, Preço, Estoque) Contém os produtos disponíveis.
4.	### ItemVenda: Relaciona produtos vendidos dentro de cada venda.
	• ItemVenda (ItemVendaID, VendaID, ProdutoID, Quantidade, Subtotal) Relaciona produtos vendidos dentro de cada venda.
## Relacionamentos
Modelo relacional das tabelas normalizadas até a Terceira Forma Normal (3FN), garantindo que não há redundância e que todas as dependências funcionais estão devidamente organizadas.

• Um Cliente pode ter muitas Vendas (1:N).

• Uma Venda pode ter muitos ItensVenda (1:N).

• Um Produto pode estar em muitos ItensVenda (1:N).
## Estrutura e Querys para criação das Tabelas
1. ### Cliente
```sql
CREATE TABLE Cliente (
    ClienteID INT IDENTITY(1,1) PRIMARY KEY,
    Nome NVARCHAR(100) NOT NULL,
    CPF CHAR(11) UNIQUE NOT NULL,
    Telefone NVARCHAR(15),
    Email NVARCHAR(100) UNIQUE
);
```
2. ### Venda
```sql
CREATE TABLE Venda (
    VendaID INT IDENTITY(1,1) PRIMARY KEY,
    ClienteID INT NOT NULL,
    DataVenda DATETIME DEFAULT GETDATE(),
    TotalVenda DECIMAL(10,2) NOT NULL,
    CONSTRAINT FK_Venda_Cliente FOREIGN KEY (ClienteID)
        REFERENCES Cliente(ClienteID) ON DELETE CASCADE
);
```
3. ### Produto
```sql
   CREATE TABLE Produto (
    ProdutoID INT IDENTITY(1,1) PRIMARY KEY,
    Nome NVARCHAR(100) NOT NULL,
    Preco DECIMAL(10,2) NOT NULL,
    Estoque INT NOT NULL
);
```
4. ### ItemVenda
```sql
CREATE TABLE ItemVenda (
    ItemVendaID INT IDENTITY(1,1) PRIMARY KEY,
    VendaID INT NOT NULL,
    ProdutoID INT NOT NULL,
    Quantidade INT NOT NULL,
    Subtotal DECIMAL(10,2) NOT NULL,
    CONSTRAINT FK_ItemVenda_Venda FOREIGN KEY (VendaID)
        REFERENCES Venda(VendaID) ON DELETE CASCADE,
    CONSTRAINT FK_ItemVenda_Produto FOREIGN KEY (ProdutoID)
        REFERENCES Produto(ProdutoID) ON DELETE CASCADE
);
```
## Querys para inserção de dados fictícios
1. ### Cliente
```sql
INSERT INTO Cliente (Nome, CPF, Telefone, Email) VALUES
('Alexandre Afonso', '11111111111', '11111111111', 'alexandre@email.com'),
('Carlos Henrique', '22222222222', '22222222222', 'carlos@email.com'),
('Ewerlon Silva', '33333333333', '33333333333', 'ewerlon@email.com'),
('Paulo Jefferson', '44444444444', '44444444444, 'paulo@email.com'),
('Tomás Kangaza', '55555555555', '55555555555', 'tomas@email.com');
```
2. ### Produtos
```sql
INSERT INTO Produto (Nome, Preco, Estoque) VALUES
('Arroz 5kg', 25.90, 50),
('Feijão 1kg', 8.50, 100),
('Óleo de Soja 900ml', 7.90, 80),
('Açúcar 1kg', 4.50, 60).
('Coca-Cola 2L', 10.00, 40),
('Fanta Laranja 2L', 10.00, 45);
```
3. ### Venda
```sql
INSERT INTO Venda (ClienteID, DataVenda, TotalVenda) VALUES
(1, '2025-02-10 14:30:00', 42.30),
(2, '2025-02-10 15:00:00', 33.80),
(3, '2025-02-11 10:15:00', 25.90),
(4, '2025-02-12 12:15:00', 10.00),
(5, '2025-02-12 13:15:00', 10.00);
```
4. ### Itens das Vendas
```sql
INSERT INTO ItemVenda (VendaID, ProdutoID, Quantidade, Subtotal) VALUES
(1, 1, 1, 25.90),  -- Alexandre Afonso comprou 1 Arroz
(1, 2, 2, 16.40),  -- Alexandre Afonso comprou 2 Feijões
(2, 3, 2, 15.80),  -- Carlos Henrique comprou 2 Óleos
(2, 4, 4, 18.00),  -- Carlos Henrique comprou 4 Açúcares
(3, 1, 1, 25.90),  -- Ewerlon Silva comprou 1 Arroz
(4, 5, 1, 10.00).  -- Paulo Jefferson comprou 1 Coca-Cola 2L
(5, 6, 1, 10.00);  -- Tomás Kangaza comprou 1 Fanta Laranja 2L
```
## Querys para verificação dos Dados
Após executar as inserções, você pode visualizar os registros usando os comandos abaixo:
```sql
SELECT * FROM Cliente;
SELECT * FROM Produto;
SELECT * FROM Venda;
SELECT * FROM ItemVenda;
```
## Consultas SQL
Aqui estão alguns exemplos de consultas SQL para obter dados combinando informações de múltiplas tabelas no SQL Server.
### Listar todas as vendas com os nomes dos clientes
Essa consulta Exibe todas as vendas e o nome do cliente que realizou cada uma.
```sql
SELECT V.VendaID, C.Nome AS Cliente, V.DataVenda, V.TotalVenda
FROM Venda V
JOIN Cliente C ON V.ClienteID = C.ClienteID;
```
### Listar os itens de cada venda com detalhes do produto e do cliente
Essa consulta exibe os produtos comprados em cada venda, junto com o nome do cliente e os valores.
```sql
SELECT V.VendaID, C.Nome AS Cliente, P.Nome AS Produto, IV.Quantidade, IV.Subtotal
FROM ItemVenda IV
JOIN Venda V ON IV.VendaID = V.VendaID
JOIN Cliente C ON V.ClienteID = C.ClienteID
JOIN Produto P ON IV.ProdutoID = P.ProdutoID;
```
### Total gasto por cada cliente em compras
Essa consulta mostra o total gasto por cada cliente, ordenado do maior para o menor.
```sql
SELECT C.Nome AS Cliente, SUM(V.TotalVenda) AS TotalGasto
FROM Venda V
JOIN Cliente C ON V.ClienteID = C.ClienteID
GROUP BY C.Nome
ORDER BY TotalGasto DESC;
```
### Estoque atual dos produtos mais vendidos
Exibe os produtos mais vendidos e o estoque atual disponível.
```sql
SELECT P.Nome AS Produto, SUM(IV.Quantidade) AS TotalVendido, P.Estoque
FROM ItemVenda IV
JOIN Produto P ON IV.ProdutoID = P.ProdutoID
GROUP BY P.Nome, P.Estoque
ORDER BY TotalVendido DESC;
```





