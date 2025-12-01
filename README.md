# Sistema de Gestão Hospitalar – Scripts SQL

Este projeto representa a modelagem e normalização de um banco de dados hospitalar, incluindo pacientes, consultas, médicos, medicamentos e prescrições. O projeto foi dividido em três etapas (EP1, EP2, EP3), resultando em um banco normalizado em 3FN.

---

## Estrutura do Banco (principais tabelas)

- Paciente
- Medico
- Consulta
- Medicamento
- medicamtno_consulta (tabela associativa N:N)

---

## Executando os Scripts

### 1) Criar as tabelas
Execute primeiro o script de criação das tabelas (modelo lógico da EP2).

### 2) Inserir dados de exemplo
Rodar o script:

Este passo preenche o banco com registros fictícios realistas.

### 3) Consultar dados
Executar os comandos SELECT incluídos:

- listagem de pacientes  
- consultas por paciente  
- medicamentos prescritos  
- quantidades em estoque  

### 4) Atualizar dados
Os comandos UPDATE alteram registros existentes, incluindo:

- correção de telefone  
- ajuste de estoque  
- atualização de nome  

### 5) Remover dados
Os comandos DELETE removem registros, simulando:

- exclusão de dados por LGPD  
- remoção de medicamentos vencidos  

---

## Observações Técnicas

- O banco está normalizado até a 3ª Forma Normal.
- Foram evitados atributos multivalorados.
- Relações N:N foram resolvidas com tabelas associativas.
- Todos os registros de amostra são fictícios.
-- Inserir pacientes
INSERT INTO Paciente (nome, telefone) VALUES ('João Silva', '11977882211');
INSERT INTO Paciente (nome, telefone) VALUES ('Maria Santos', '21955331122');
INSERT INTO Paciente (nome, telefone) VALUES ('Carlos Pereira', '31988994455');

-- Inserir médicos
INSERT INTO Medico (nome, crm) VALUES ('Dr. Ricardo Mattos', 'CRM12345');
INSERT INTO Medico (nome, crm) VALUES ('Dra. Ana Campos', 'CRM67890');

-- Inserir consultas
INSERT INTO Consulta (nome, paciente, idade, id_paciente) VALUES ('Consulta Cardiológica', 'João Silva', 50, 1);
INSERT INTO Consulta (nome, paciente, idade, id_paciente) VALUES ('Consulta Geral', 'Maria Santos', 35, 2);

-- Inserir medicamentos
INSERT INTO Medicamento (nome, quantidade, validade_medicamento) VALUES ('Dipirona', 200, '2026-12-30');
INSERT INTO Medicamento (nome, quantidade, validade_medicamento) VALUES ('Amoxicilina', 150, '2025-06-15');

-- Inserir relação Prescrição–Medicamento
INSERT INTO medicamtno_consulta (id_consulta, id_medicamento, dose_aplicada)
VALUES (1, 1, '20 gotas');

INSERT INTO medicamtno_consulta (id_consulta, id_medicamento, dose_aplicada)
VALUES (2, 2, '500mg a cada 8h');
-- 1) buscar todos pacientes
SELECT * FROM Paciente;

-- 2) listar consultas com nome do paciente
SELECT c.id_consulta, c.nome, p.nome
FROM Consulta c
JOIN Paciente p ON c.id_paciente = p.id_paciente;

-- 3) listar medicamentos prescritos por consulta
SELECT p.nome AS paciente, m.nome AS medicamento, mc.dose_aplicada
FROM medicamtno_consulta mc
JOIN Consulta c ON mc.id_consulta = c.id_consulta
JOIN Paciente p ON c.id_paciente = p.id_paciente
JOIN Medicamento m ON mc.id_medicamento = m.id_medicamento;

-- 4) mostrar medicamentos em baixa quantidade
SELECT * FROM Medicamento
WHERE quantidade < 180
ORDER BY quantidade ASC;

-- 5) mostrar pacientes ordenados alfabeticamente
SELECT * FROM Paciente
ORDER BY nome ASC LIMIT 10;
-- atualizar telefone do paciente
UPDATE Paciente
SET telefone = '11999999999'
WHERE nome = 'João Silva';

-- atualizar quantidade de medicamento após uso
UPDATE Medicamento
SET quantidade = quantidade - 1
WHERE nome = 'Dipirona';

-- corrigir nome da consulta
UPDATE Consulta
SET nome = 'Consulta Clínica Geral'
WHERE id_consulta = 2;
-- excluir prescrição de uma consulta
DELETE FROM medicamtno_consulta
WHERE id_consulta = 2;

-- remover medicamento que venceu
DELETE FROM Medicamento
WHERE validade_medicamento < '2025-01-01';

-- remover paciente que solicitou exclusão de dados
DELETE FROM Paciente
WHERE id_paciente = 3;

