# 🚀 Backend - CRUD com Spring Boot

## 📋 Sobre o Projeto

Backend robusto desenvolvido em **Spring Boot** para um sistema de gerenciamento de produtos. API RESTful completa com operações CRUD, integração com MySQL via Docker e tratamento adequado de erros.

## 🛠️ Tecnologias Utilizadas

| Tecnologia | Versão | Finalidade |
|------------|--------|------------|
| Java | 25 LTS | Linguagem de programação |
| Spring Boot | 4.0.3 | Framework principal |
| Spring Web | - | Criação de APIs REST |
| Spring Data JPA | - | Persistência e ORM |
| MySQL Connector | - | Driver de conexão com MySQL |
| Lombok | - | Redução de código boilerplate |
| Hibernate | 7.2.4 | Mapeamento objeto-relacional |
| Docker | latest | Containerização do banco de dados |
| Gradle | 9.3.1 | Gerenciamento de dependências e build |

## 📁 Estrutura do Projeto
src/main/java/com/example/demo/
├── config/
│ └── CorsConfig.java # Configuração de CORS
├── controller/
│ └── ProdutoController.java # Endpoints da API
├── model/
│ └── Produto.java # Entidade JPA
└── repository/
└── ProdutoRepository.java # Interface de persistência


## 🔧 Dependências (build.gradle)

```gradle
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    compileOnly 'org.projectlombok:lombok'
    developmentOnly 'org.springframework.boot:spring-boot-docker-compose'
    runtimeOnly 'com.mysql:mysql-connector-j'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

📊 Modelo de Dados (Entidade Produto)
@Entity
@Data
public class Produto {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(nullable = false)
    private String nome;
    
    private String descricao;
    private Double preco;
    private Integer quantidade;
}

🌐 Endpoints da API
Método	Endpoint	Descrição	Status HTTP
GET	/api/produtos	Lista todos os produtos	200 OK
GET	/api/produtos/{id}	Busca produto por ID	200 OK / 404 Not Found
POST	/api/produtos	Cria novo produto	201 Created
PUT	/api/produtos/{id}	Atualiza produto existente	200 OK / 404 Not Found
DELETE	/api/produtos/{id}	Remove produto	204 No Content / 404 Not Found
🐳 Configuração Docker (compose.yaml)

services:
  mysql:
    image: 'mysql:8.0'
    container_name: java-mysql-1
    environment:
      - 'MYSQL_DATABASE=cruddb'
      - 'MYSQL_ROOT_PASSWORD=root'
    ports:
      - '3306:3306'
    volumes:
      - mysql-data:/var/lib/mysql

volumes:
  mysql-data:

⚙️ Configurações (application.properties)

# Banco de dados
spring.datasource.url=jdbc:mysql://localhost:3306/cruddb?useSSL=false&allowPublicKeyRetrieval=true&serverTimezone=UTC
spring.datasource.username=root
spring.datasource.password=root
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver

# JPA Configuration
spring.jpa.database=mysql
spring.jpa.database-platform=org.hibernate.dialect.MySQLDialect
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
spring.jpa.properties.hibernate.format_sql=true

# CORS Configuration
spring.web.cors.allowed-origins=http://127.0.0.1:5500,http://localhost:5500
spring.web.cors.allowed-methods=GET,POST,PUT,DELETE,OPTIONS
spring.web.cors.allowed-headers=*
spring.web.cors.allow-credentials=true

🚀 Como Executar o Backend
Pré-requisitos
☕ Java 25 LTS instalado

🐳 Docker Desktop em execução

📦 Git (opcional, para clonagem)

Passo a Passo
Clone o repositório

bash
git clone https://github.com/elizeu-lima/crud-java.git
cd crud-java
Inicie o container MySQL

bash
docker-compose up -d
Verifique se o MySQL está rodando

bash
docker ps
docker exec -it java-mysql-1 mysql -uroot -proot -e "SHOW DATABASES;"
Execute a aplicação Spring Boot

bash
# No Windows
./gradlew bootRun

# No Linux/Mac
./gradlew bootRun
Acesse a API

text
http://localhost:8080/api/produtos
📦 Comandos Gradle Úteis
bash
# Limpar builds anteriores
./gradlew clean

# Compilar o projeto
./gradlew build

# Executar testes
./gradlew test

# Executar a aplicação
./gradlew bootRun

# Pular testes durante o build
./gradlew build -x test
🐳 Comandos Docker Úteis
bash
# Iniciar MySQL
docker-compose up -d

# Parar MySQL
docker-compose down

# Ver logs do MySQL
docker logs java-mysql-1

# Acessar MySQL via terminal
docker exec -it java-mysql-1 mysql -uroot -proot

# Listar bancos de dados
docker exec -it java-mysql-1 mysql -uroot -proot -e "SHOW DATABASES;"

# Ver tabelas do banco
docker exec -it java-mysql-1 mysql -uroot -proot -e "USE cruddb; SHOW TABLES;"
🔍 Tratamento de Erros
A API retorna status HTTP apropriados:

200 OK: Requisição bem-sucedida

201 Created: Recurso criado com sucesso

204 No Content: Recurso deletado

404 Not Found: Recurso não encontrado

500 Internal Server Error: Erro no servidor

🌍 Configuração CORS
Configurado para permitir requisições de:

http://127.0.0.1:5500

http://localhost:5500

📊 Exemplos de Requisições
Criar Produto (POST)
bash
curl -X POST http://localhost:8080/api/produtos \
  -H "Content-Type: application/json" \
  -d '{
    "nome": "Notebook Dell",
    "descricao": "Intel i7, 16GB RAM, 512GB SSD",
    "preco": 4500.00,
    "quantidade": 10
  }'
Listar Produtos (GET)
bash
curl http://localhost:8080/api/produtos
Atualizar Produto (PUT)
bash
curl -X PUT http://localhost:8080/api/produtos/1 \
  -H "Content-Type: application/json" \
  -d '{
    "nome": "Notebook Dell XPS",
    "descricao": "Intel i9, 32GB RAM, 1TB SSD",
    "preco": 8500.00,
    "quantidade": 5
  }'
Deletar Produto (DELETE)
bash
curl -X DELETE http://localhost:8080/api/produtos/1
🧪 Testes
Para executar os testes automatizados:

bash
./gradlew test
📈 Possíveis Melhorias Futuras
Autenticação com JWT

Paginação nos resultados

Validações com Bean Validation

Logs estruturados

Cache com Redis

Documentação com Swagger/OpenAPI

Profiles para diferentes ambientes

Monitoramento com Actuator
