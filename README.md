# Building a Training & Certification Data Platform with Microsoft Fabric
---
![Empresa](techskills.png)
## 🏢 Business Context

A empresa **TechSkills Africa** fornece:

- Treinamentos oficiais Microsoft  
- Certificações  
- Consultoria tecnológica  

Atua principalmente em:

- Angola  
- Moçambique  

---

## ⚠️ Desafios atuais

- Dados dispersos (Excel, emails, sistemas diferentes)  
- Falta de visibilidade sobre performance dos treinamentos  
- Dificuldade em medir impacto por país  
- Baixa rastreabilidade de certificações  

---

## 🎯 Business Goals

- Centralizar dados num único sistema (Fabric SQL)  
- Monitorar treinamentos por país  
- Acompanhar alunos e certificações  
- Melhorar decisões comerciais e operacionais  

---

## 🧱 Data Model (Visão Geral)

Principais entidades do modelo:

- Clientes (empresas)  
- Alunos  
- Treinamentos  
- Instrutores  
- Países  
- Certificações  
- Inscrições (fact table)  

---

## 🗄️ SQL Script – Criação da Base de Dados

```
/* =========================
   CRIAÇÃO DO SCHEMA
========================= */
CREATE SCHEMA training;
GO

/* =========================
   TABELA PAÍS
========================= */
CREATE TABLE training.Pais (
    id_pais INT PRIMARY KEY,
    nome_pais VARCHAR(100)
);

/* =========================
   TABELA CLIENTE
========================= */
CREATE TABLE training.Cliente (
    id_cliente INT PRIMARY KEY,
    nome_cliente VARCHAR(150),
    setor VARCHAR(100),
    id_pais INT,
    CONSTRAINT FK_Cliente_Pais
        FOREIGN KEY (id_pais) REFERENCES training.Pais(id_pais)
);

/* =========================
   TABELA INSTRUTOR
========================= */
CREATE TABLE training.Instrutor (
    id_instrutor INT PRIMARY KEY,
    nome_instrutor VARCHAR(150),
    especialidade VARCHAR(100)
);

/* =========================
   TABELA TREINAMENTO
========================= */
CREATE TABLE training.Treinamento (
    id_treinamento INT PRIMARY KEY,
    nome_treinamento VARCHAR(150),
    tecnologia VARCHAR(100),
    nivel VARCHAR(50),
    duracao_horas INT
);

/* =========================
   TABELA ALUNO
========================= */
CREATE TABLE training.Aluno (
    id_aluno INT PRIMARY KEY,
    nome_aluno VARCHAR(150),
    email VARCHAR(150),
    id_cliente INT,
    CONSTRAINT FK_Aluno_Cliente
        FOREIGN KEY (id_cliente) REFERENCES training.Cliente(id_cliente)
);

/* =========================
   TABELA CERTIFICAÇÃO
========================= */
CREATE TABLE training.Certificacao (
    id_certificacao INT PRIMARY KEY,
    nome_certificacao VARCHAR(150),
    tecnologia VARCHAR(100)
);

/* =========================
   TABELA INSCRIÇÃO (FATO)
========================= */
CREATE TABLE training.Inscricao (
    id_inscricao INT PRIMARY KEY,
    id_aluno INT,
    id_treinamento INT,
    id_instrutor INT,
    data_inicio DATE,
    data_fim DATE,
    status VARCHAR(50),
    aprovado BIT,
    id_certificacao INT,
    CONSTRAINT FK_Inscricao_Aluno
        FOREIGN KEY (id_aluno) REFERENCES training.Aluno(id_aluno),
    CONSTRAINT FK_Inscricao_Treinamento
        FOREIGN KEY (id_treinamento) REFERENCES training.Treinamento(id_treinamento),
    CONSTRAINT FK_Inscricao_Instrutor
        FOREIGN KEY (id_instrutor) REFERENCES training.Instrutor(id_instrutor),
    CONSTRAINT FK_Inscricao_Certificacao
        FOREIGN KEY (id_certificacao) REFERENCES training.Certificacao(id_certificacao)
);
```

A base de dados foi estruturada no Microsoft Fabric utilizando tabelas relacionais:

- País  
- Cliente  
- Aluno  
- Instrutor  
- Treinamento  
- Certificação  
- Inscrição

Dados de exemplo:

```
/* =========================
   1. PAÍSES
========================= */
INSERT INTO training.Pais (id_pais, nome_pais)
VALUES
(1, 'Angola'),
(2, 'Moçambique');

/* =========================
   2. CLIENTES
========================= */
INSERT INTO training.Cliente (id_cliente, nome_cliente, setor, id_pais)
VALUES
(1, 'Banco BAI', 'Banca', 1),
(2, 'Unitel', 'Telecom', 1),
(3, 'Sonangol', 'Energia', 1),
(4, 'TAAG', 'Aviação', 1),
(5, 'Banco BIC', 'Banca', 1),
(6, 'Movicel', 'Telecom', 1),
(7, 'Refriango', 'Indústria', 1),
(8, 'Angola Telecom', 'Telecom', 1),
(9, 'Banco Sol', 'Banca', 1),
(10, 'ENSUL', 'Energia', 1),
(11, 'Vodacom Moçambique', 'Telecom', 2),
(12, 'Banco Millennium Moçambique', 'Banca', 2),
(13, 'EDM', 'Energia', 2),
(14, 'HCB', 'Energia', 2),
(15, 'Movitel', 'Telecom', 2),
(16, 'LAM', 'Aviação', 2),
(17, 'Cervejas de Moçambique', 'Indústria', 2),
(18, 'Banco Moza', 'Banca', 2),
(19, 'Portos e Caminhos de Ferro', 'Logística', 2),
(20, 'CMAM', 'Saúde', 2);

/* =========================
   3. ALUNOS
========================= */
INSERT INTO training.Aluno (id_aluno, nome_aluno, email, id_cliente)
VALUES
(1,'José Manuel','jose1@mail.com',1),
(2,'Maria Silva','maria2@mail.com',2),
(3,'Carlos Alberto','carlos3@mail.com',3),
(4,'Ana Paula','ana4@mail.com',4),
(5,'Pedro Miguel','pedro5@mail.com',5),
(6,'João Francisco','joao6@mail.com',6),
(7,'Filomena Jorge','filomena7@mail.com',7),
(8,'Paulo Gomes','paulo8@mail.com',8),
(9,'Helena Domingos','helena9@mail.com',9),
(10,'António Pedro','antonio10@mail.com',10),
(11,'Joana Nhampossa','joana11@mail.com',11),
(12,'Manuel Chipande','manuel12@mail.com',12),
(13,'Celina Mucavele','celina13@mail.com',13),
(14,'Paulo Machava','paulo14@mail.com',14),
(15,'Isabel Mungoi','isabel15@mail.com',15),
(16,'Eduardo Nhantumbo','eduardo16@mail.com',16),
(17,'Rosa Mussa','rosa17@mail.com',17),
(18,'Filipe Zimba','filipe18@mail.com',18),
(19,'Teresa Macamo','teresa19@mail.com',19),
(20,'José Nhantumbo','jose20@mail.com',20);

/* =========================
   4. INSTRUTORES
========================= */
INSERT INTO training.Instrutor (id_instrutor, nome_instrutor, especialidade)
VALUES
(1,'Eng. Carlos Manuel','Azure'),
(2,'Eng. João Pedro','Power BI'),
(3,'Dra. Ana Paula','Data'),
(4,'Eng. Miguel Santos','AI'),
(5,'Eng. Paulo Dias','Security'),
(6,'Eng. José Domingos','Azure'),
(7,'Dra. Filomena Costa','Power BI'),
(8,'Eng. Eduardo Nhantumbo','Data'),
(9,'Eng. Manuel Chipande','AI'),
(10,'Eng. António Joaquim','Security');

/* =========================
   5. TREINAMENTOS
========================= */
INSERT INTO training.Treinamento (id_treinamento, nome_treinamento, tecnologia, nivel, duracao_horas)
VALUES
(1,'Azure Basics','Azure','Iniciante',8),
(2,'Azure Admin','Azure','Intermediário',16),
(3,'Azure Architect','Azure','Avançado',24),
(4,'Power BI Basics','Power BI','Iniciante',8),
(5,'Power BI Dashboards','Power BI','Intermediário',16),
(6,'Power BI Advanced','Power BI','Avançado',24),
(7,'SQL Basics','Data','Iniciante',8),
(8,'SQL Advanced','Data','Avançado',16),
(9,'AI Basics','AI','Iniciante',8),
(10,'Machine Learning','AI','Intermediário',16);

/* =========================
   6. CERTIFICAÇÕES
========================= */
INSERT INTO training.Certificacao (id_certificacao, nome_certificacao, tecnologia)
VALUES
(1,'Azure Fundamentals','Azure'),
(2,'Azure Administrator','Azure'),
(3,'Azure Architect Expert','Azure'),
(4,'Power BI Data Analyst','Power BI'),
(5,'Power BI Advanced','Power BI'),
(6,'SQL Fundamentals','Data'),
(7,'SQL Advanced','Data'),
(8,'AI Fundamentals','AI'),
(9,'Machine Learning','AI'),
(10,'Security Basics','Security'),
(11,'Cloud Fundamentals','Azure'),
(12,'Power BI Service','Power BI'),
(13,'Data Engineering','Data'),
(14,'Advanced Analytics','Data'),
(15,'Azure Security','Security');

/* =========================
   7. INSCRIÇÕES
========================= */
INSERT INTO training.Inscricao
(id_inscricao,id_aluno,id_treinamento,id_instrutor,data_inicio,data_fim,status,aprovado,id_certificacao)
VALUES
(1,1,1,1,'2024-01-01','2024-01-03','Concluído',1,1),
(2,2,2,2,'2024-01-02','2024-01-04','Concluído',1,2),
(3,3,3,3,'2024-01-03','2024-01-05','Em andamento',0,3),
(4,4,4,4,'2024-01-04','2024-01-06','Concluído',1,4),
(5,5,5,5,'2024-01-05','2024-01-07','Cancelado',0,5),
(6,6,6,6,'2024-01-06','2024-01-08','Concluído',1,6),
(7,7,7,7,'2024-01-07','2024-01-09','Em andamento',0,7),
(8,8,8,8,'2024-01-08','2024-01-10','Concluído',1,8),
(9,9,9,9,'2024-01-09','2024-01-11','Cancelado',0,9),
(10,10,10,10,'2024-01-10','2024-01-12','Concluído',1,10),
(11,11,1,1,'2024-01-11','2024-01-13','Concluído',1,11),
(12,12,2,2,'2024-01-12','2024-01-14','Em andamento',0,12),
(13,13,3,3,'2024-01-13','2024-01-15','Concluído',1,13),
(14,14,4,4,'2024-01-14','2024-01-16','Cancelado',0,14),
(15,15,5,5,'2024-01-15','2024-01-17','Concluído',1,15);

```

---

## 📊 Objetivo Analítico

Com esta base de dados:

- Quais treinamentos foram realizados em Angola?
- Quais treinamentos foram realizados em Moçambique?
- Quantos alunos participaram por treinamento?
- Lista de alunos por cliente
- Quantos treinamentos cada instrutor ministrou?
- Número de certificações obtidas por país
- Top 5 treinamentos mais frequentados
- Quantidade de alunos por nível (iniciante, intermediário, avançado)

---

## 🚀 Resultado Esperado

Após implementação:

- Dados centralizados e confiáveis  
- Melhor tomada de decisão  
- Visibilidade total do negócio de formação  
- Suporte à estratégia de crescimento em África  

---

## 🔮 Futuras Melhorias

- Uso no Power BI
- Modelos preditivos de aprovação de alunos  
- Análise de ROI dos treinamentos  

---

## 👨‍💻 Autor

Shalom André
Microsoft Student Ambassador
