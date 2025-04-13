# 📊 Consultas SQL - Banco de Dados Ecommerce  

Este conjunto de consultas SQL foi desenvolvido para extrair informações relevantes do banco de dados do sistema de **e-commerce**. Abaixo estão exemplos de queries que exploram diferentes aspectos dos dados, como clientes, métodos de pagamento, status de pedidos e produtos cancelados.  

---

## 🧑‍💻 **1. Relação de Clientes, Status e Valores de Pedidos**  
Esta consulta retorna a lista de clientes, seus endereços, o status de seus pedidos e os valores correspondentes.  
- Os resultados são ordenados por valor de envio.  

```sql
SELECT c.Pname, c.Adress, o.OrderStatus, o.SendValue 
FROM clients c 
JOIN orders o ON (c.idClients = o.idOrderClient)
ORDER BY o.SendValue;

💳 2. Métodos de Pagamento dos Clientes
Consulta que apresenta os clientes juntamente com seus endereços
e métodos de pagamento cadastrados.

Os resultados são ordenados pelo tipo de pagamento.

SELECT c.Pname, c.Adress, p.TypePayment 
FROM clients c 
JOIN payments p ON (c.idClients = p.idClient)
ORDER BY p.TypePayment;

📦 3. Status dos Pedidos dos Clientes
Retorna uma lista dos clientes e o status de seus pedidos.

Utiliza um relacionamento direto entre as tabelas clients e orders.

SELECT c.Pname, o.OrderStatus 
FROM clients c, orders o 
WHERE c.idClients = o.idOrderClient;

❌ 4. Pedidos Cancelados e Produtos Relacionados
Esta consulta identifica os produtos associados a pedidos cancelados,
destacando o status do pedido e a quantidade de produtos envolvidos.

Os resultados são ordenados pela quantidade de produtos enviados.

SELECT p.Pname AS Product_Name, o.OrderStatus, o.SendValue AS Quantity_Product 
FROM Product p
JOIN ProductSeller pc ON p.idProduct = pc.idProduct
JOIN Orders o ON pc.idPseller = o.idOrder
WHERE o.OrderStatus = "Cancelado"
ORDER BY o.SendValue;
