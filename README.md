# Building a Training & Certification Data Platform with Microsoft Fabric
---
![Empresa]()
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
-- Criar schema
CREATE SCHEMA training;
GO

-- Tabela País
CREATE TABLE training.Pais (
    id_pais INT PRIMARY KEY,
    nome_pais VARCHAR(100)
);

-- Tabela Cliente (empresa)
CREATE TABLE training.Cliente (
    id_cliente INT PRIMARY KEY,
    nome_cliente VARCHAR(150),
    setor VARCHAR(100),
    id_pais INT,
    FOREIGN KEY (id_pais) REFERENCES training.Pais(id_pais)
);

-- Tabela Instrutor
CREATE TABLE training.Instrutor (
    id_instrutor INT PRIMARY KEY,
    nome_instrutor VARCHAR(150),
    especialidade VARCHAR(100)
);

-- Tabela Treinamento
CREATE TABLE training.Treinamento (
    id_treinamento INT PRIMARY KEY,
    nome_treinamento VARCHAR(150),
    tecnologia VARCHAR(100),
    nivel VARCHAR(50),
    duracao_horas INT
);

-- Tabela Aluno
CREATE TABLE training.Aluno (
    id_aluno INT PRIMARY KEY,
    nome_aluno VARCHAR(150),
    email VARCHAR(150),
    id_cliente INT,
    FOREIGN KEY (id_cliente) REFERENCES training.Cliente(id_cliente)
);

-- Tabela Certificação
CREATE TABLE training.Certificacao (
    id_certificacao INT PRIMARY KEY,
    nome_certificacao VARCHAR(150),
    tecnologia VARCHAR(100)
);

-- Tabela Inscrição (Fato)
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
    FOREIGN KEY (id_aluno) REFERENCES training.Aluno(id_aluno),
    FOREIGN KEY (id_treinamento) REFERENCES training.Treinamento(id_treinamento),
    FOREIGN KEY (id_instrutor) REFERENCES training.Instrutor(id_instrutor),
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
INSERT INTO training.Pais VALUES (1, 'Angola');
INSERT INTO training.Pais VALUES (2, 'Moçambique');


/* =========================
   2. CLIENTES 
========================= */
INSERT INTO training.Cliente VALUES (1, 'Banco BAI', 'Banca', 1);
INSERT INTO training.Cliente VALUES (2, 'Unitel', 'Telecom', 1);
INSERT INTO training.Cliente VALUES (3, 'Sonangol', 'Energia', 1);
INSERT INTO training.Cliente VALUES (4, 'TAAG', 'Aviação', 1);
INSERT INTO training.Cliente VALUES (5, 'Banco BIC', 'Banca', 1);
INSERT INTO training.Cliente VALUES (6, 'Movicel', 'Telecom', 1);
INSERT INTO training.Cliente VALUES (7, 'Refriango', 'Indústria', 1);
INSERT INTO training.Cliente VALUES (8, 'Angola Telecom', 'Telecom', 1);
INSERT INTO training.Cliente VALUES (9, 'Banco Sol', 'Banca', 1);
INSERT INTO training.Cliente VALUES (10, 'ENSUL', 'Energia', 1);

INSERT INTO training.Cliente VALUES (11, 'Vodacom Moçambique', 'Telecom', 2);
INSERT INTO training.Cliente VALUES (12, 'Banco Millennium Moçambique', 'Banca', 2);
INSERT INTO training.Cliente VALUES (13, 'EDM', 'Energia', 2);
INSERT INTO training.Cliente VALUES (14, 'HCB', 'Energia', 2);
INSERT INTO training.Cliente VALUES (15, 'Movitel', 'Telecom', 2);
INSERT INTO training.Cliente VALUES (16, 'LAM', 'Aviação', 2);
INSERT INTO training.Cliente VALUES (17, 'Cervejas de Moçambique', 'Indústria', 2);
INSERT INTO training.Cliente VALUES (18, 'Banco Moza', 'Banca', 2);
INSERT INTO training.Cliente VALUES (19, 'Portos e Caminhos de Ferro', 'Logística', 2);
INSERT INTO training.Cliente VALUES (20, 'CMAM', 'Saúde', 2);

/* =========================
   3. ALUNOS 
========================= */
INSERT INTO training.Aluno VALUES (1, 'José Manuel', 'jose1@mail.com', 1);
INSERT INTO training.Aluno VALUES (2, 'Maria Silva', 'maria2@mail.com', 2);
INSERT INTO training.Aluno VALUES (3, 'Carlos Alberto', 'carlos3@mail.com', 3);
INSERT INTO training.Aluno VALUES (4, 'Ana Paula', 'ana4@mail.com', 4);
INSERT INTO training.Aluno VALUES (5, 'Pedro Miguel', 'pedro5@mail.com', 5);
INSERT INTO training.Aluno VALUES (6, 'João Francisco', 'joao6@mail.com', 6);
INSERT INTO training.Aluno VALUES (7, 'Filomena Jorge', 'filomena7@mail.com', 7);
INSERT INTO training.Aluno VALUES (8, 'Paulo Gomes', 'paulo8@mail.com', 8);
INSERT INTO training.Aluno VALUES (9, 'Helena Domingos', 'helena9@mail.com', 9);
INSERT INTO training.Aluno VALUES (10, 'António Pedro', 'antonio10@mail.com', 10);

INSERT INTO training.Aluno VALUES (11, 'Joana Nhampossa', 'joana11@mail.com', 11);
INSERT INTO training.Aluno VALUES (12, 'Manuel Chipande', 'manuel12@mail.com', 12);
INSERT INTO training.Aluno VALUES (13, 'Celina Mucavele', 'celina13@mail.com', 13);
INSERT INTO training.Aluno VALUES (14, 'Paulo Machava', 'paulo14@mail.com', 14);
INSERT INTO training.Aluno VALUES (15, 'Isabel Mungoi', 'isabel15@mail.com', 15);
INSERT INTO training.Aluno VALUES (16, 'Eduardo Nhantumbo', 'eduardo16@mail.com', 16);
INSERT INTO training.Aluno VALUES (17, 'Rosa Mussa', 'rosa17@mail.com', 17);
INSERT INTO training.Aluno VALUES (18, 'Filipe Zimba', 'filipe18@mail.com', 18);
INSERT INTO training.Aluno VALUES (19, 'Teresa Macamo', 'teresa19@mail.com', 19);
INSERT INTO training.Aluno VALUES (20, 'José Nhantumbo', 'jose20@mail.com', 20);

/* =========================
   4. INSTRUTORES (50)
========================= */
INSERT INTO training.Instrutor VALUES (1, 'Eng. Carlos Manuel', 'Azure');
INSERT INTO training.Instrutor VALUES (2, 'Eng. João Pedro', 'Power BI');
INSERT INTO training.Instrutor VALUES (3, 'Dra. Ana Paula', 'Data');
INSERT INTO training.Instrutor VALUES (4, 'Eng. Miguel Santos', 'AI');
INSERT INTO training.Instrutor VALUES (5, 'Eng. Paulo Dias', 'Security');

INSERT INTO training.Instrutor VALUES (6, 'Eng. José Domingos', 'Azure');
INSERT INTO training.Instrutor VALUES (7, 'Dra. Filomena Costa', 'Power BI');
INSERT INTO training.Instrutor VALUES (8, 'Eng. Eduardo Nhantumbo', 'Data');
INSERT INTO training.Instrutor VALUES (9, 'Eng. Manuel Chipande', 'AI');
INSERT INTO training.Instrutor VALUES (10, 'Eng. António Joaquim', 'Security');

/* =========================
   5. TREINAMENTOS (50)
========================= */
INSERT INTO training.Treinamento VALUES (1, 'Azure Basics', 'Azure', 'Iniciante', 8);
INSERT INTO training.Treinamento VALUES (2, 'Azure Admin', 'Azure', 'Intermediário', 16);
INSERT INTO training.Treinamento VALUES (3, 'Azure Architect', 'Azure', 'Avançado', 24);
INSERT INTO training.Treinamento VALUES (4, 'Power BI Basics', 'Power BI', 'Iniciante', 8);
INSERT INTO training.Treinamento VALUES (5, 'Power BI Dashboards', 'Power BI', 'Intermediário', 16);
INSERT INTO training.Treinamento VALUES (6, 'Power BI Advanced', 'Power BI', 'Avançado', 24);
INSERT INTO training.Treinamento VALUES (7, 'SQL Basics', 'Data', 'Iniciante', 8);
INSERT INTO training.Treinamento VALUES (8, 'SQL Advanced', 'Data', 'Avançado', 16);
INSERT INTO training.Treinamento VALUES (9, 'AI Basics', 'AI', 'Iniciante', 8);
INSERT INTO training.Treinamento VALUES (10, 'Machine Learning', 'AI', 'Intermediário', 16);

/* =========================
   6. INSCRIÇÕES 
========================= */
INSERT INTO training.Inscricao VALUES (1, 1, 1, 1, '2024-01-01', '2024-01-03', 'Concluído', 1, 1);
INSERT INTO training.Inscricao VALUES (2, 2, 2, 2, '2024-01-02', '2024-01-04', 'Concluído', 1, 2);
INSERT INTO training.Inscricao VALUES (3, 3, 3, 3, '2024-01-03', '2024-01-05', 'Em andamento', 0, 3);
INSERT INTO training.Inscricao VALUES (4, 4, 4, 4, '2024-01-04', '2024-01-06', 'Concluído', 1, 4);
INSERT INTO training.Inscricao VALUES (5, 5, 5, 5, '2024-01-05', '2024-01-07', 'Cancelado', 0, 5);
INSERT INTO training.Inscricao VALUES (6, 6, 6, 1, '2024-01-06', '2024-01-08', 'Concluído', 1, 6);
INSERT INTO training.Inscricao VALUES (7, 7, 7, 2, '2024-01-07', '2024-01-09', 'Em andamento', 0, 7);
INSERT INTO training.Inscricao VALUES (8, 8, 8, 3, '2024-01-08', '2024-01-10', 'Concluído', 1, 8);
INSERT INTO training.Inscricao VALUES (9, 9, 9, 4, '2024-01-09', '2024-01-11', 'Cancelado', 0, 9);
INSERT INTO training.Inscricao VALUES (10, 10, 10, 5, '2024-01-10', '2024-01-12', 'Concluído', 1, 10);

INSERT INTO training.Inscricao VALUES (11, 11, 11, 6, '2024-01-11', '2024-01-13', 'Concluído', 1, 11);
INSERT INTO training.Inscricao VALUES (12, 12, 12, 7, '2024-01-12', '2024-01-14', 'Em andamento', 0, 12);
INSERT INTO training.Inscricao VALUES (13, 13, 13, 8, '2024-01-13', '2024-01-15', 'Concluído', 1, 13);
INSERT INTO training.Inscricao VALUES (14, 14, 14, 9, '2024-01-14', '2024-01-16', 'Cancelado', 0, 14);
INSERT INTO training.Inscricao VALUES (15, 15, 15, 10, '2024-01-15', '2024-01-17', 'Concluído', 1, 15);

INSERT INTO training.Inscricao VALUES (16, 16, 16, 1, '2024-01-16', '2024-01-18', 'Concluído', 1, 16);
INSERT INTO training.Inscricao VALUES (17, 17, 17, 2, '2024-01-17', '2024-01-19', 'Em andamento', 0, 17);
INSERT INTO training.Inscricao VALUES (18, 18, 18, 3, '2024-01-18', '2024-01-20', 'Concluído', 1, 18);
INSERT INTO training.Inscricao VALUES (19, 19, 19, 4, '2024-01-19', '2024-01-21', 'Cancelado', 0, 19);
INSERT INTO training.Inscricao VALUES (20, 20, 20, 5, '2024-01-20', '2024-01-22', 'Concluído', 1, 20);

INSERT INTO training.Inscricao VALUES (21, 21, 21, 6, '2024-01-21', '2024-01-23', 'Concluído', 1, 21);
INSERT INTO training.Inscricao VALUES (22, 22, 22, 7, '2024-01-22', '2024-01-24', 'Em andamento', 0, 22);
INSERT INTO training.Inscricao VALUES (23, 23, 23, 8, '2024-01-23', '2024-01-25', 'Concluído', 1, 23);
INSERT INTO training.Inscricao VALUES (24, 24, 24, 9, '2024-01-24', '2024-01-26', 'Cancelado', 0, 24);
INSERT INTO training.Inscricao VALUES (25, 25, 25, 10, '2024-01-25', '2024-01-27', 'Concluído', 1, 25);

INSERT INTO training.Inscricao VALUES (26, 26, 26, 1, '2024-01-26', '2024-01-28', 'Concluído', 1, 26);
INSERT INTO training.Inscricao VALUES (27, 27, 27, 2, '2024-01-27', '2024-01-29', 'Em andamento', 0, 27);
INSERT INTO training.Inscricao VALUES (28, 28, 28, 3, '2024-01-28', '2024-01-30', 'Concluído', 1, 28);
INSERT INTO training.Inscricao VALUES (29, 29, 29, 4, '2024-01-29', '2024-01-31', 'Cancelado', 0, 29);
INSERT INTO training.Inscricao VALUES (30, 30, 30, 5, '2024-01-30', '2024-02-01', 'Concluído', 1, 30);

INSERT INTO training.Inscricao VALUES (31, 31, 31, 6, '2024-01-31', '2024-02-02', 'Concluído', 1, 31);
INSERT INTO training.Inscricao VALUES (32, 32, 32, 7, '2024-02-01', '2024-02-03', 'Em andamento', 0, 32);
INSERT INTO training.Inscricao VALUES (33, 33, 33, 8, '2024-02-02', '2024-02-04', 'Concluído', 1, 33);
INSERT INTO training.Inscricao VALUES (34, 34, 34, 9, '2024-02-03', '2024-02-05', 'Cancelado', 0, 34);
INSERT INTO training.Inscricao VALUES (35, 35, 35, 10, '2024-02-04', '2024-02-06', 'Concluído', 1, 35);

INSERT INTO training.Inscricao VALUES (36, 36, 36, 1, '2024-02-05', '2024-02-07', 'Concluído', 1, 36);
INSERT INTO training.Inscricao VALUES (37, 37, 37, 2, '2024-02-06', '2024-02-08', 'Em andamento', 0, 37);
INSERT INTO training.Inscricao VALUES (38, 38, 38, 3, '2024-02-07', '2024-02-09', 'Concluído', 1, 38);
INSERT INTO training.Inscricao VALUES (39, 39, 39, 4, '2024-02-08', '2024-02-10', 'Cancelado', 0, 39);
INSERT INTO training.Inscricao VALUES (40, 40, 40, 5, '2024-02-09', '2024-02-11', 'Concluído', 1, 40);

INSERT INTO training.Inscricao VALUES (41, 41, 41, 6, '2024-02-10', '2024-02-12', 'Concluído', 1, 41);
INSERT INTO training.Inscricao VALUES (42, 42, 42, 7, '2024-02-11', '2024-02-13', 'Em andamento', 0, 42);
INSERT INTO training.Inscricao VALUES (43, 43, 43, 8, '2024-02-12', '2024-02-14', 'Concluído', 1, 43);
INSERT INTO training.Inscricao VALUES (44, 44, 44, 9, '2024-02-13', '2024-02-15', 'Cancelado', 0, 44);
INSERT INTO training.Inscricao VALUES (45, 45, 45, 10, '2024-02-14', '2024-02-16', 'Concluído', 1, 45);

INSERT INTO training.Inscricao VALUES (46, 46, 46, 1, '2024-02-15', '2024-02-17', 'Concluído', 1, 46);
INSERT INTO training.Inscricao VALUES (47, 47, 47, 2, '2024-02-16', '2024-02-18', 'Em andamento', 0, 47);
INSERT INTO training.Inscricao VALUES (48, 48, 48, 3, '2024-02-17', '2024-02-19', 'Concluído', 1, 48);
INSERT INTO training.Inscricao VALUES (49, 49, 49, 4, '2024-02-18', '2024-02-20', 'Cancelado', 0, 49);
INSERT INTO training.Inscricao VALUES (50, 50, 50, 5, '2024-02-19', '2024-02-21', 'Concluído', 1, 50);
INSERT INTO training.Inscricao VALUES (51, 1, 2, 6, '2024-02-20', '2024-02-22', 'Concluído', 1, 1);
INSERT INTO training.Inscricao VALUES (52, 2, 3, 7, '2024-02-21', '2024-02-23', 'Em andamento', 0, 2);
INSERT INTO training.Inscricao VALUES (53, 3, 4, 8, '2024-02-22', '2024-02-24', 'Concluído', 1, 3);
INSERT INTO training.Inscricao VALUES (54, 4, 5, 9, '2024-02-23', '2024-02-25', 'Cancelado', 0, 4);
INSERT INTO training.Inscricao VALUES (55, 5, 6, 10, '2024-02-24', '2024-02-26', 'Concluído', 1, 5);
```

---

## 📊 Objetivo Analítico

Com esta base de dados é possível:

- Analisar desempenho de treinamentos  
- Medir taxa de certificação  
- Comparar Angola vs Moçambique  
- Avaliar performance de instrutores  
- Identificar clientes mais ativos  

---

## 📈 Uso no Power BI

A base de dados será conectada ao Power BI para criação de dashboards com:

- KPIs de formação  
- Taxa de aprovação  
- Volume de alunos por país  
- Treinamentos mais procurados  
- Receita estimada por cliente  

---

## 🚀 Resultado Esperado

Após implementação:

- Dados centralizados e confiáveis  
- Melhor tomada de decisão  
- Visibilidade total do negócio de formação  
- Suporte à estratégia de crescimento em África  

---

## 🔮 Futuras Melhorias

- Modelos preditivos de aprovação de alunos  
- Análise de ROI dos treinamentos  

---

## 👨‍💻 Autor

Shalom André
Microsoft Student Ambassador
