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
