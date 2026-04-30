# **Pesquisa DAO + JDBC + MySQL**

- ## **1. O que é DAO?**

O padrão DAO (Data Access Object) é um padrão de projeto estrutural 
que abstrai e encapsula todas as interações com o banco de dados em 
uma camada dedicada. Ou seja, ele separa em classes DAO tudo aquilo 
que pertence ao banco, como as operações CRUD, e permite acessá-las 
de forma organizada.

- ### **Por que usar DAO?**

Em um sistema sem DAO, como o projeto Teatro, todas as alterações 
feitas durante a execução do programa eram perdidas ao fechar, pois 
ficavam apenas na memória RAM. Uma conexão com banco de dados resolve 
esse problema, pois permite que os dados sejam salvos e manipulados 
de forma persistente. O DAO encapsula esse acesso, adicionando 
segurança e organização ao código.

### **Benefícios do DAO**

- **Desacoplamento:** a lógica do código e dos processos dentro do 
sistema não muda mesmo mudando a base de dados. Os processos 
encapsulados continuam funcionando normalmente.

- **Reutilização:** os métodos do DAO podem ser utilizados durante 
todo o projeto de formas variadas. Por exemplo, um método 
`buscarPorId()` pode ser chamado sempre que necessário, sem 
gerar código extra.

- **Manutenção:** tudo o que envolve o banco e seus processos fica 
na classe DAO, o que facilita a manutenção, adição de conteúdo e 
correção de problemas, já que tudo fica em um lugar específico 
e organizado.

## **2. Ciclo de vida da conexão JDBC**

O JDBC (Java Database Connectivity) é uma API do Java que possibilita 
que uma aplicação consiga acessar um banco de dados local ou remoto. 
Ele é composto pelos pacotes `java.sql` e `javax.sql`, que contêm as 
classes e interfaces que padronizam a comunicação entre o Java e o banco.

Para fazer essa comunicação funcionar, o JDBC utiliza drivers — 
componentes de software (arquivos JAR) que permitem à aplicação se 
conectar e interagir com bancos relacionais como MySQL, PostgreSQL e Oracle.

O ciclo de vida de uma conexão JDBC segue 6 etapas:

1. **Carregar o driver:** registra o driver do banco no sistema
2. **Estabelecer conexão:** abre a conexão via `DriverManager.getConnection()`, 
passando URL, usuário e senha
3. **Criar Statement:** prepara o objeto que vai enviar comandos SQL
4. **Executar SQL:** roda comandos como `SELECT`, `INSERT`, `UPDATE`, `DELETE`
5. **Processar ResultSet:** percorre os resultados retornados pelo `SELECT`
6. **Fechar conexão:** libera os recursos para não vazar memória

## **3. SQL Injection e PreparedStatement**

SQL Injection é uma técnica de ataque onde o invasor injeta comandos 
SQL por meio de formulários ou campos de entrada, tentando executar 
comandos no banco de dados para obter vantagens, como acessar dados 
sigilosos, destruir e manipular informações, ou simplismente adquirir
controle ADM sobre o sistema.

---

Por exemplo, num sistema como o Teatro, um atacante poderia usar o 
cadastro de salas ou um formulário de login para rodar comandos 
maliciosos no banco.

---

### **Como o PreparedStatement resolve isso?**

Quando o SQL é montado juntando strings, o usuário pode digitar 
caracteres especiais como `'` e fazer o banco interpretar o que 
digitou como um comando SQL.

Com o PreparedStatement, tudo que o usuário digita é tratado como 
texto simples e transformado em String, fazendo com que o banco não
interprete o que o usuario digitou como um comando. O banco
de dados recebe os parâmetros separados do SQL, tornando a injeção 
impossível.

** Exemplo da diferença: **

--- 

```java
// ERRADO - vulnerável a SQL Injection
String sql = "SELECT * FROM salas WHERE nome = '" + nome + "'";
```
---

```java
// CORRETO - seguro com PreparedStatement
String sql = "SELECT * FROM salas WHERE nome = ?";
PreparedStatement stmt = conn.prepareStatement(sql);
stmt.setString(1, nome);
```

---

```

projeto-dao-base/
├─ pom.xml
├─ README.md
└─ src/
   ├─ main/
   │  ├─ java/
   │  │  └─ br/escola/dao_base/
   │  │     ├─ app/
   │  │     │  └─ Main.java
   │  │     ├─ model/
   │  │     │  └─ (entidades do tema entram depois)
   │  │     ├─ dao/
   │  │     │  └─ (interfaces entram depois)
   │  │     ├─ dao/impl/
   │  │     │  └─ (implementações JDBC entram depois)
   │  │     └─ db/
   │  │        ├─ ConnectionFactory.java
   │  │        └─ DbException.java
   │  └─ resources/
   │     ├─ db.properties
   │     └─ sql/
   │        ├─ schema.sql
   │        └─ seed.sql
   └─ test/
      └─ java/ (opcional)


```


## **5. Fontes**

1. JavaThinking — O que é DAO em Java  
   - https://www.javathinking.com/

2. DIO — O que é DAO  
   - https://www.dio.me/articles/o-que-e-dao-ba9c73921265

3. DevMedia — Baixo acoplamento com DAO  
   - https://www.devmedia.com.br/baixo-acoplamento-com-dao/32696

4. Alura — Conhecendo o JDBC  
   - https://www.alura.com.br/artigos/conhecendo-o-jdbc

5. DIO — JDBC: uma visão geral  
   - https://www.dio.me/articles/jdbc-java-database-connectivity-uma-visao-geral-57b6b447ec8d

6. DIO — O papel do PreparedStatement  
   - https://www.dio.me/articles/o-papel-do-preparedstatement-o-agente-infiltrado-do-banco-de-dados

7. Fortinet — O que é SQL Injection  
   - https://www.fortinet.com/br/resources/cyberglossary/sql-injection

8. DevMedia — SQL Injection  
   - https://www.devmedia.com.br/sql-injection/6102

- Miguel Souza França