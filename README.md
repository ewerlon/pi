# Projeto integrador Banco de Dados
DER (Diagrama Entidade-Relacionamento) para um sistema de PDV (Ponto de Venda) com pelo menos quatro entidades. O cenário escolhido envolve a venda de produtos, relacionando clientes, vendas e itens de venda.
## Entidades e Relacionamentos
1.	### Cliente (ClienteID, Nome, CPF, Telefone, Email)
       o Um cliente pode realizar várias compras.
2.	### Venda (VendaID, ClienteID, DataVenda, TotalVenda)
       o Uma venda pertence a um cliente.
3.	### Produto (ProdutoID, Nome, Preço, Estoque)
       o Produtos são vendidos em múltiplas vendas.
4.	### ItemVenda (ItemVendaID, VendaID, ProdutoID, Quantidade, Subtotal)
       o Representa os produtos vendidos em cada venda.
## Relacionamentos
Modelo relacional das tabelas normalizadas até a Terceira Forma Normal (3FN), garantindo que não há redundância e que todas as dependências funcionais estão devidamente organizadas.
•	Um Cliente pode ter muitas Vendas (1:N).
