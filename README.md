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
('Açúcar 1kg', 4.50, 60);
```
3. ### Venda
```sql
INSERT INTO Venda (ClienteID, DataVenda, TotalVenda) VALUES
(1, '2025-02-10 14:30:00', 42.30),
(2, '2025-02-10 15:00:00', 33.80),
(3, '2025-02-11 10:15:00', 25.90);
```
4. ### Venda
```sql
INSERT INTO ItemVenda (VendaID, ProdutoID, Quantidade, Subtotal) VALUES
(1, 1, 1, 25.90),  -- João comprou 1 arroz
(1, 2, 2, 16.40),  -- João comprou 2 feijões
(2, 3, 2, 15.80),  -- Maria comprou 2 óleos
(2, 4, 4, 18.00),  -- Maria comprou 4 açúcares
(3, 1, 1, 25.90);  -- Carlos comprou 1 arroz
```


