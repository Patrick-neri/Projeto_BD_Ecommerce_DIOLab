# 📦 Projeto Banco de Dados Ecommerce - DIOLab  

Este projeto consiste na construção de um banco de dados para um sistema de **e-commerce**, com o objetivo de replicar a modelagem lógica, criar o esquema SQL e desenvolver consultas complexas. Abaixo está a estrutura completa das tabelas e seus relacionamentos.

---

## 🛠️ Criação do Banco de Dados  

```sql
CREATE DATABASE ecommerce;
USE ecommerce;

📋 Estrutura do Banco de Dados

1️⃣ Tabela clients
Armazena informações pessoais dos clientes.

CREATE TABLE clients (
    idClients INT AUTO_INCREMENT PRIMARY KEY,
    Pname VARCHAR(10),
    Mname CHAR(3),
    Lname VARCHAR(20),
    CPF CHAR(11) NOT NULL,
    Adress VARCHAR(30),
    CONSTRAINT unique_cpf_client UNIQUE (CPF)
);

2️⃣ Tabela Product
Gerencia os produtos disponíveis no sistema.

CREATE TABLE Product (
    idProduct INT AUTO_INCREMENT PRIMARY KEY,
    Pname VARCHAR(10),
    Classification_kids BOOL DEFAULT FALSE,
    Category ENUM('Eletrônico','Alimentos','Roupas','Brinquedos') NOT NULL,
    Avaliação FLOAT DEFAULT 0,
    Size VARCHAR(18)
);

3️⃣ Tabela Payments
Registra os métodos de pagamento dos clientes.

CREATE TABLE Payments (
    idPayment INT,
    idClient INT,
    TypePayment ENUM('Boleto', 'Cartão', 'Dois Cartões'),
    LimitAvailable FLOAT,
    PRIMARY KEY (idClient, idPayment)
);

4️⃣ Tabela Orders
Armazena os pedidos realizados pelos clientes.

CREATE TABLE Orders (
    idOrder INT AUTO_INCREMENT PRIMARY KEY,
    idOrderClient INT,
    OrderStatus ENUM('Cancelado', 'Confirmado', 'Em processamento') DEFAULT 'Em Processamento',
    OrderDescription VARCHAR(255),
    SendValue FLOAT DEFAULT 10,
    PaymentCash BOOL DEFAULT FALSE,
    CONSTRAINT fk_Orders_Client FOREIGN KEY (idOrderClient) REFERENCES clients (idClients)
);


5️⃣ Tabela clientsPJ
Armazena informações de clientes Pessoa Jurídica (PJ).

CREATE TABLE clientsPJ (
    idClientsPJ INT AUTO_INCREMENT PRIMARY KEY,
    CNPJ CHAR(15) NOT NULL,
    SocialName VARCHAR(45),
    Email VARCHAR(20),
    Contact CHAR(11),
    Adress VARCHAR(30),
    CONSTRAINT unique_cnpj_client UNIQUE (CNPJ),
    CONSTRAINT fk_clientsPJ_order FOREIGN KEY (idClientsPJ) REFERENCES Orders (idOrder)
);

6️⃣ Tabela ProductStorage
Controla o armazenamento de produtos.

CREATE TABLE ProductStorage (
    idStorage INT AUTO_INCREMENT PRIMARY KEY,
    StorageLocation VARCHAR(255),
    Quantity INT DEFAULT 0
);

7️⃣ Tabela Supplier
Armazena informações sobre os fornecedores.

CREATE TABLE Supplier (
    idSupplier INT AUTO_INCREMENT PRIMARY KEY,
    SocialName VARCHAR(255) NOT NULL,
    CNPJ CHAR(15) NOT NULL,
    Contact CHAR(11) NOT NULL,
    CONSTRAINT unique_Supplier UNIQUE (CNPJ)
);

8️⃣ Tabela Seller
Registra informações dos vendedores.

CREATE TABLE Seller (
    idSeller INT AUTO_INCREMENT PRIMARY KEY,
    SocialName VARCHAR(255) NOT NULL,
    AbstName VARCHAR(255),
    CNPJ CHAR(15) NOT NULL,
    CPF CHAR(9),
    Contact CHAR(11) NOT NULL,
    CONSTRAINT unique_CNPJ_Seller UNIQUE (CNPJ),
    CONSTRAINT unique_CPF_Seller UNIQUE (CPF)
);

9️⃣ Tabela ProductSeller
Relaciona produtos com vendedores.

CREATE TABLE ProductSeller (
    idPseller INT,
    idProduct INT,
    ProdQuantity INT DEFAULT 1,
    PRIMARY KEY (idPseller, idProduct),
    CONSTRAINT fk_Product_Seller FOREIGN KEY (idPseller) REFERENCES Seller (idSeller),
    CONSTRAINT fk_Product_Product FOREIGN KEY (idProduct) REFERENCES Product (idProduct)
);

🔟 Tabela ProductOrder
Relaciona produtos com pedidos.

CREATE TABLE ProductOrder (
    idPOorder INT,
    idPOproduct INT,
    PoQuantity INT DEFAULT 1,
    poStatus ENUM('Disponível', 'Sem estoque') DEFAULT 'Disponível',
    PRIMARY KEY (idPOproduct, idPOorder),
    CONSTRAINT fk_Product_order FOREIGN KEY (idPOproduct) REFERENCES Product (idProduct),
    CONSTRAINT fk_ProductOrder_Product FOREIGN KEY (idPOorder) REFERENCES Orders (idOrder)
);

1️⃣1️⃣ Tabela ProductSupplier
Relaciona produtos com fornecedores.


CREATE TABLE ProductSupplier (
    idPsSupplier INT,
    idPsProduct INT,
    Quantity INT NOT NULL,
    PRIMARY KEY (idPsSupplier, idPsProduct),
    CONSTRAINT fk_product_supplier_supplier FOREIGN KEY (idPsSupplier) REFERENCES Supplier (idSupplier),
    CONSTRAINT fk_product_supplier_product FOREIGN KEY (idPsProduct) REFERENCES Product (idProduct)
);

