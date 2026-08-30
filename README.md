# 🛒 Product Management System

A full-stack **Spring Boot MVC** web application for managing products with complete CRUD operations, containerized with **Docker** and orchestrated with **Docker Compose**.

![Java](https://img.shields.io/badge/Java-17-orange?style=flat-square&logo=java)
![Spring Boot](https://img.shields.io/badge/Spring%20Boot-3.0.5-green?style=flat-square&logo=spring)
![MySQL](https://img.shields.io/badge/MySQL-8.0-blue?style=flat-square&logo=mysql)
![Docker](https://img.shields.io/badge/Docker-Containerized-blue?style=flat-square&logo=docker)
![Thymeleaf](https://img.shields.io/badge/Thymeleaf-Template%20Engine-green?style=flat-square)

---

## 📸 Screenshots

| Home Page | Product List | Add Product |
|-----------|-------------|-------------|
| Welcome page with navigation | View all products in table | Form to add new product |

---

## ✨ Features

- ✅ **Add** new products
- ✅ **View** all products in a table
- ✅ **Edit** existing products
- ✅ **Delete** products with confirmation
- ✅ **Form Validation** with error messages
- ✅ **Responsive UI** with Bootstrap 4
- ✅ **Dockerized** for easy deployment
- ✅ **Persistent Database** with Docker volumes

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| **Backend** | Java 17, Spring Boot 3.0.5 |
| **Web Framework** | Spring MVC |
| **Template Engine** | Thymeleaf |
| **Database** | MySQL 8.0 |
| **ORM** | Spring Data JPA / Hibernate |
| **Frontend** | Bootstrap 4, Font Awesome |
| **Containerization** | Docker, Docker Compose |
| **Build Tool** | Maven |

---

## 🗂️ Project Structure

```
product-management/
├── src/
│   ├── main/
│   │   ├── java/com/example/productmanagement/
│   │   │   ├── ProductManagementApplication.java   # Main class
│   │   │   ├── controller/
│   │   │   │   └── ProductController.java          # MVC Controller
│   │   │   ├── entity/
│   │   │   │   └── Product.java                    # JPA Entity
│   │   │   └── repository/
│   │   │       └── ProductRepository.java          # JPA Repository
│   │   └── resources/
│   │       ├── application.properties              # App config
│   │       ├── static/
│   │       │   └── index.html                      # Home page
│   │       └── templates/
│   │           ├── list.html                       # Product list
│   │           ├── insert.html                     # Add product form
│   │           └── update.html                     # Edit product form
├── Dockerfile                                       # Docker image config
├── docker-compose.yml                               # Multi-container setup
├── mvnw                                             # Maven wrapper
└── pom.xml                                          # Maven dependencies
```

---

## 🐳 Run with Docker (Recommended)

### Prerequisites
- [Docker](https://docs.docker.com/get-docker/) installed
- [Docker Compose](https://docs.docker.com/compose/install/) installed

### Steps

```bash
# 1. Clone the repository
git clone https://github.com/agha177/Product-Management.git
cd Product-Management/product-management/product-management

# 2. Build and start all containers
sudo docker compose up --build

# 3. Access the application
# Open browser: http://localhost:8080
```

### Stop the Application
```bash
# Stop containers
sudo docker compose down

# Stop and remove database (fresh start)
sudo docker compose down -v
```

---

## 💻 Run Locally (Without Docker)

### Prerequisites
- Java 17
- Maven
- MySQL 8.0

### Steps

```bash
# 1. Clone the repository
git clone https://github.com/agha177/Product-Management.git
cd Product-Management/product-management/product-management

# 2. Create MySQL database
mysql -u root -p
CREATE DATABASE productdb;
EXIT;

# 3. Update application.properties
spring.datasource.password=YOUR_PASSWORD

# 4. Run the application
./mvnw spring-boot:run

# 5. Access the application
# Open browser: http://localhost:8080
```

---

## 🌐 Application Endpoints

| Method | URL | Description |
|--------|-----|-------------|
| GET | `/` | Home page |
| GET | `/products/list` | List all products |
| GET | `/products/insert` | Show add product form |
| POST | `/products/add` | Add new product |
| GET | `/products/edit/{id}` | Show edit product form |
| POST | `/products/update/{id}` | Update product |
| GET | `/products/delete/{id}` | Delete product |

---

## 🐳 Docker Architecture

```
┌─────────────────────────────────────────────┐
│              Docker Environment              │
│                                             │
│  ┌─────────────────┐  ┌─────────────────┐  │
│  │   product-app   │  │  product-mysql  │  │
│  │                 │  │                 │  │
│  │  Spring Boot    │──│   MySQL 8.0     │  │
│  │  Port: 8080     │  │   Port: 3306    │  │
│  └─────────────────┘  └────────┬────────┘  │
│           │                    │           │
│    product-network        mysql_data       │
│      (Bridge)              (Volume)        │
└───────────┼────────────────────────────────┘
            │
     http://localhost:8080
         (Browser)
```

### Docker Components:

| Component | Description |
|-----------|-------------|
| **product-app** | Spring Boot application container |
| **product-mysql** | MySQL database container |
| **product-network** | Bridge network for container communication |
| **mysql_data** | Volume for persistent database storage |

---

## ⚙️ Configuration

### application.properties
```properties
spring.application.name=product-management
spring.datasource.url=jdbc:mysql://localhost:3306/productdb
spring.datasource.username=root
spring.datasource.password=root
spring.jpa.hibernate.ddl-auto=update
spring.jpa.show-sql=true
```

### Docker Environment Variables
Docker Compose overrides the database URL to use container name:
```yaml
SPRING_DATASOURCE_URL: jdbc:mysql://mysql:3306/productdb
SPRING_DATASOURCE_USERNAME: root
SPRING_DATASOURCE_PASSWORD: root
```

---

## 🔧 Useful Docker Commands

```bash
# View running containers
sudo docker ps

# View application logs
sudo docker logs product-app

# View database logs
sudo docker logs product-mysql

# Stop all containers
sudo docker compose down

# Rebuild after code changes
sudo docker compose up --build

# View all volumes
sudo docker volume ls

# Access MySQL inside container
sudo docker exec -it product-mysql mysql -u root -p
```

---

## 📦 Product Entity

```java
@Entity
@Table(name = "products")
public class Product {
    private Long id;          // Auto-generated ID
    private String name;      // Product name (required)
    private String description; // Product description (required)
    private Double price;     // Product price (must be positive)
    private Integer quantity; // Product quantity (must be positive)
}
```

---

## 🚀 Deployment Workflow

```
1. Make changes on local machine
         ↓
2. git add . && git commit -m "message"
         ↓
3. git push origin main
         ↓
4. On server: git pull
         ↓
5. sudo docker compose up --build
         ↓
6. Application updated! ✅
```

---

## 👨‍💻 Author

**Abdelrahman** - [@agha177](https://github.com/agha177)

---

## 📝 License

This project is for educational purposes.

---

## 🙏 Acknowledgments

- [Spring Boot Documentation](https://spring.io/projects/spring-boot)
- [Docker Documentation](https://docs.docker.com/)
- [Thymeleaf Documentation](https://www.thymeleaf.org/)
