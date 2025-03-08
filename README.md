# desafio_dio

-- Tabela Cliente PF
CREATE TABLE ClientePF (
    CPF VARCHAR(11) PRIMARY KEY,
    Nome VARCHAR(255),
    Endereco VARCHAR(255),
    Telefone VARCHAR(20)
);

-- Tabela Cliente PJ
CREATE TABLE ClientePJ (
    CNPJ VARCHAR(14) PRIMARY KEY,
    RazaoSocial VARCHAR(255),
    Endereco VARCHAR(255),
    Telefone VARCHAR(20)
);

-- Tabela Conta
CREATE TABLE Conta (
    IDConta INT PRIMARY KEY AUTO_INCREMENT,
    TipoCliente ENUM('PF', 'PJ'),
    CPF_ClientePF VARCHAR(11),
    CNPJ_ClientePJ VARCHAR(14),
    FOREIGN KEY (CPF_ClientePF) REFERENCES ClientePF(CPF),
    FOREIGN KEY (CNPJ_ClientePJ) REFERENCES ClientePJ(CNPJ),
    CONSTRAINT chk_Cliente CHECK ((TipoCliente = 'PF' AND CPF_ClientePF IS NOT NULL AND CNPJ_ClientePJ IS NULL) OR (TipoCliente = 'PJ' AND CNPJ_ClientePJ IS NOT NULL AND CPF_ClientePF IS NULL))
);

-- Tabela Pagamento
CREATE TABLE Pagamento (
    IDPagamento INT PRIMARY KEY AUTO_INCREMENT,
    IDConta INT,
    Descricao VARCHAR(255),
    FOREIGN KEY (IDConta) REFERENCES Conta(IDConta)
);

-- Tabela Pedido
CREATE TABLE Pedido (
    IDPedido INT PRIMARY KEY AUTO_INCREMENT,
    IDConta INT,
    IDPagamento INT,
    FOREIGN KEY (IDConta) REFERENCES Conta(IDConta),
    FOREIGN KEY (IDPagamento) REFERENCES Pagamento(IDPagamento)
);

-- Tabela Entrega
CREATE TABLE Entrega (
    IDEntrega INT PRIMARY KEY AUTO_INCREMENT,
    IDPedido INT,
    Status VARCHAR(50),
    CodigoRastreio VARCHAR(50),
    FOREIGN KEY (IDPedido) REFERENCES Pedido(IDPedido)
);

-- Tabela Produto
CREATE TABLE Produto (
    IdProduto INT PRIMARY KEY AUTO_INCREMENT,
    Descricao VARCHAR(255),
    Valor DECIMAL(10, 2)
);

-- Tabela Item do Pedido
CREATE TABLE ItemPedido (
    IdItemPedido INT PRIMARY KEY AUTO_INCREMENT,
    IDPedido INT,
    IdProduto INT,
    Quantidade INT,
    FOREIGN KEY (IDPedido) REFERENCES Pedido(IDPedido),
    FOREIGN KEY (IdProduto) REFERENCES Produto(IdProduto)
);
