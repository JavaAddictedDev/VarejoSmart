# VarejoSmart - Sistema de Gestão para Varejo

## 📋 Sobre o Projeto
VarejoSmart é uma solução moderna de gestão para o mercado varejista, construída com uma arquitetura de microserviços utilizando Spring Boot. O sistema oferece uma abordagem modular e escalável para gerenciar diferentes aspectos de operações varejistas.

## 🏗️ Arquitetura do Sistema

### Tecnologias Principais
- **Java 17**
- **Spring Boot 3.x**
- **Spring Cloud Netflix Eureka** (Service Discovery)
- **RabbitMQ** (Message Broker)
- **PostgreSQL** (Banco de Dados Principal)
- **Redis** (Cache)
- **Docker** (Containerização)
- **Kubernetes** (Orquestração)

### Microserviços

#### 1. Service Registry (eureka-server)
- Registro e descoberta de serviços
- Porta padrão: 8761

#### 2. API Gateway
- Roteamento de requisições
- Balanceamento de carga
- Segurança e autenticação centralizada
- Porta padrão: 8080

#### 3. Serviço de Autenticação (auth-service)
- Gestão de usuários e perfis
- Autenticação e autorização
- JWT Token management
- Porta padrão: 8081

**Domínios principais:**
```java
User {
    Long id
    String username
    String password
    String email
    List<Role> roles
    Boolean active
}

Role {
    Long id
    String name
    List<Permission> permissions
}
```

#### 4. Serviço de Produtos (product-service)
- Gestão de produtos e categorias
- Gestão de estoque
- Precificação
- Porta padrão: 8082

**Domínios principais:**
```java
Product {
    Long id
    String name
    String description
    BigDecimal price
    Category category
    String sku
    Integer stockQuantity
    Boolean active
}

Category {
    Long id
    String name
    String description
    Category parentCategory
}

Stock {
    Long id
    Product product
    Integer quantity
    String location
}
```

#### 5. Serviço de Vendas (sales-service)
- Processamento de pedidos
- Carrinhos de compra
- Gestão de transações
- Porta padrão: 8083

**Domínios principais:**
```java
Order {
    Long id
    String orderNumber
    Customer customer
    List<OrderItem> items
    OrderStatus status
    PaymentInfo paymentInfo
    BigDecimal totalAmount
    LocalDateTime createdAt
}

OrderItem {
    Long id
    Product product
    Integer quantity
    BigDecimal unitPrice
    BigDecimal subtotal
}

Cart {
    Long id
    Customer customer
    List<CartItem> items
    LocalDateTime createdAt
    LocalDateTime updatedAt
}
```

#### 6. Serviço de Clientes (customer-service)
- Gestão de clientes
- Programas de fidelidade
- Histórico de compras
- Porta padrão: 8084

**Domínios principais:**
```java
Customer {
    Long id
    String name
    String email
    String cpf
    Address address
    String phone
    LoyaltyProgram loyaltyProgram
}

LoyaltyProgram {
    Long id
    Customer customer
    Integer points
    Level level
    LocalDate joinDate
}
```

#### 7. Serviço de Pagamentos (payment-service)
- Processamento de pagamentos
- Integração com gateways
- Gestão de reembolsos
- Porta padrão: 8085

**Domínios principais:**
```java
Payment {
    Long id
    Order order
    BigDecimal amount
    PaymentStatus status
    PaymentMethod method
    String transactionId
    LocalDateTime processedAt
}

Refund {
    Long id
    Payment payment
    BigDecimal amount
    RefundStatus status
    String reason
    LocalDateTime requestedAt
}
```

## 📦 Regras de Negócio Principais

### Produtos e Estoque
1. Produtos não podem ter preço negativo
2. Toda alteração de estoque deve ser registrada com log de auditoria
3. Produtos com estoque abaixo do mínimo devem gerar alertas
4. Produtos inativos não podem ser vendidos

### Vendas
1. Verificar disponibilidade de estoque antes de confirmar venda
2. Calcular desconto baseado no programa de fidelidade do cliente
3. Validar formas de pagamento disponíveis
4. Atualizar pontos de fidelidade após confirmação da venda
5. Enviar notificação para cliente após processamento do pedido

### Clientes
1. CPF deve ser válido e único no sistema
2. Email deve ser único e validado
3. Programa de fidelidade deve ser atualizado automaticamente baseado em regras de pontuação
4. Cliente pode ter apenas um programa de fidelidade ativo

### Pagamentos
1. Timeout máximo de 5 minutos para processamento
2. Reembolsos devem ser aprovados por supervisor
3. Notificar cliente sobre status do pagamento
4. Manter histórico de todas as transações

## 🚀 Setup do Projeto

### Pré-requisitos
- Java 17+
- Maven 3.8+
- Docker
- Docker Compose

### Configuração do Ambiente de Desenvolvimento
1. Clone o repositório
```bash
git clone https://github.com/JavaAddictedDev/VarejoSmart.git
```

2. Configure as variáveis de ambiente em cada serviço (arquivo .env)

3. Inicie os serviços de infraestrutura
```bash
docker-compose up -d
```

4. Execute os microserviços
```bash
./mvnw spring-boot:run
```

## 📈 Monitoramento e Observabilidade

- **Prometheus**: Coleta de métricas
- **Grafana**: Visualização de métricas
- **ELK Stack**: Centralização de logs
- **Spring Boot Actuator**: Health checks e métricas

## 🔒 Segurança

- Autenticação via JWT
- Comunicação entre serviços via HTTPS
- Secrets management via Vault
- Rate limiting no API Gateway
- Auditoria de operações críticas

## 🛠️ Práticas de Desenvolvimento

- **Testes**: JUnit 5, Mockito, Testcontainers
- **Documentação**: SpringDoc OpenAPI
- **Versionamento**: Semantic Versioning
- **CI/CD**: GitHub Actions
- **Code Quality**: SonarQube

## 📝 Próximos Passos

1. Implementação de cache distribuído
2. Módulo de relatórios e analytics
3. App mobile para vendedores
4. Integração com marketplaces
5. Sistema de recomendação de produtos

## 👥 Contribuição

1. Fork o projeto
2. Crie uma branch para sua feature (`git checkout -b feature/AmazingFeature`)
3. Commit suas mudanças (`git commit -m 'Add some AmazingFeature'`)
4. Push para a branch (`git push origin feature/AmazingFeature`)
5. Abra um Pull Request

## 📄 Licença

Este projeto está licenciado sob a licença MIT - veja o arquivo [LICENSE.md](LICENSE.md) para detalhes.
