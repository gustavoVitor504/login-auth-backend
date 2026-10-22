# Login Auth API

API REST de autenticação com Spring Boot e Spring Security, usando tokens JWT. Projeto de estudo desenvolvido para servir de backend à [Login Page](https://github.com/gustavoVitor504/login-page).

## Funcionalidades

- Cadastro de usuário com senha criptografada (BCrypt)
- Login que devolve um token JWT com validade de 2 horas
- Rota protegida que só responde com um token válido

## Tecnologias

- Java 17 e Spring Boot 3
- Spring Security
- Spring Data JPA
- Auth0 Java JWT
- H2 (banco em memória)
- Lombok e Maven

## Endpoints

| Método | Rota | Acesso | Corpo |
|---|---|---|---|
| POST | `/auth/register` | público | `{ "name", "email", "password" }` |
| POST | `/auth/login` | público | `{ "email", "password" }` |
| GET | `/user` | requer token | — |

Resposta do login e do cadastro:

```json
{ "name": "Gustavo", "token": "eyJhbGciOiJIUzI1NiJ9..." }
```

Nas rotas protegidas, envie o header `Authorization: Bearer SEU_TOKEN`.

## Como funciona a autenticação

1. `SecurityConfig` define as rotas públicas, desativa a sessão (API stateless) e registra o filtro de segurança.
2. `SecurityFilter` intercepta cada requisição, lê o token do header `Authorization`, valida e autentica o usuário no contexto do Spring Security.
3. `TokenService` gera e valida os tokens JWT, assinados com HMAC256.
4. `CustomUserDetailsService` carrega o usuário do banco pelo e-mail.

## Estrutura

```
src/main/java/com/example/login_auth_api/
├── controllers/      # AuthController, UserController
├── domain/user/      # entidade User
├── dto/              # LoginRequest, RegisterRequest, Response
├── infra/cors/       # configuração de CORS
├── infra/security/   # SecurityConfig, SecurityFilter, TokenService
└── repositories/     # UserRepository
```

## Como rodar localmente

Pré-requisitos: Java 17+.

```bash
git clone https://github.com/gustavoVitor504/login-auth-backend.git
cd login-auth-backend
```

Defina a chave usada para assinar os tokens:

```bash
# Linux/macOS
export JWT_SECRET=uma_chave_secreta_forte
# Windows (PowerShell)
$env:JWT_SECRET="uma_chave_secreta_forte"
```

Inicie a aplicação:

```bash
./mvnw spring-boot:run
```

A API sobe em `http://localhost:8080`. O banco é em memória (H2), então os dados são apagados ao reiniciar.

## Autor

**Gustavo Vitor Ferreira Nunes** — [LinkedIn](https://www.linkedin.com/in/gustavovitorferreira/) · [GitHub](https://github.com/gustavoVitor504)
