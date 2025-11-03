-- PostgreSQL Script
-- Adaptado de MySQL Workbench
-- Data: 03 de Novembro de 2025

-- Criar schema
CREATE SCHEMA IF NOT EXISTS mydb;
SET search_path TO mydb;

-- -----------------------------------------------------
-- Tabela mydb.endereco
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS endereco (
  idendereco SERIAL PRIMARY KEY,
  rua VARCHAR(45) NOT NULL,
  numero INT NOT NULL,
  cidade VARCHAR(45) NOT NULL,
  estado VARCHAR(45) NOT NULL,
  cep VARCHAR(45) NOT NULL
);

-- -----------------------------------------------------
-- Tabela mydb.empresa
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS empresa (
  idempresa SERIAL PRIMARY KEY,
  nome VARCHAR(99) NOT NULL,
  cnpj VARCHAR(45) UNIQUE
);

-- -----------------------------------------------------
-- Tabela mydb.banco
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS banco (
  idbanco SERIAL PRIMARY KEY,
  nome VARCHAR(45) NOT NULL,
  codigo_banco INT NOT NULL,
  empresa_idempresa INT NOT NULL,
  CONSTRAINT fk_banco_empresa
    FOREIGN KEY (empresa_idempresa)
    REFERENCES empresa (idempresa)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION
);

CREATE INDEX IF NOT EXISTS idx_banco_empresa ON banco(empresa_idempresa);

-- -----------------------------------------------------
-- Tabela mydb.funcionario
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS funcionario (
  idfuncionario SERIAL PRIMARY KEY,
  nome VARCHAR(45) NOT NULL,
  cargo VARCHAR(45) NOT NULL,
  empresa_idempresa INT NOT NULL,
  endereco_idendereco INT NOT NULL,
  banco_idbanco INT NOT NULL,
  CONSTRAINT fk_funcionario_empresa
    FOREIGN KEY (empresa_idempresa)
    REFERENCES empresa (idempresa)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT fk_funcionario_endereco
    FOREIGN KEY (endereco_idendereco)
    REFERENCES endereco (idendereco)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT fk_funcionario_banco
    FOREIGN KEY (banco_idbanco)
    REFERENCES banco (idbanco)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION
);

CREATE INDEX IF NOT EXISTS idx_funcionario_empresa ON funcionario(empresa_idempresa);
CREATE INDEX IF NOT EXISTS idx_funcionario_endereco ON funcionario(endereco_idendereco);
CREATE INDEX IF NOT EXISTS idx_funcionario_banco ON funcionario(banco_idbanco);

-- -----------------------------------------------------
-- Tabela mydb.mensalidade
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS mensalidade (
  idmensalidade SERIAL PRIMARY KEY,
  valor DECIMAL(10,2) NOT NULL,
  data_vencimento DATE NOT NULL,
  status_pagamento VARCHAR(45) NOT NULL
);

-- -----------------------------------------------------
-- Tabela mydb.transacao
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS transacao (
  idtransacao SERIAL PRIMARY KEY,
  banco_idbanco INT NOT NULL,
  mensalidade_idmensalidade INT NOT NULL,
  valor DECIMAL(10,2) NOT NULL,
  data DATE NOT NULL,
  funcionario_idfuncionario INT NOT NULL,
  CONSTRAINT fk_transacao_banco
    FOREIGN KEY (banco_idbanco)
    REFERENCES banco (idbanco)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT fk_transacao_mensalidade
    FOREIGN KEY (mensalidade_idmensalidade)
    REFERENCES mensalidade (idmensalidade)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT fk_transacao_funcionario
    FOREIGN KEY (funcionario_idfuncionario)
    REFERENCES funcionario (idfuncionario)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION
);

CREATE INDEX IF NOT EXISTS idx_transacao_banco ON transacao(banco_idbanco);
CREATE INDEX IF NOT EXISTS idx_transacao_mensalidade ON transacao(mensalidade_idmensalidade);
CREATE INDEX IF NOT EXISTS idx_transacao_funcionario ON transacao(funcionario_idfuncionario);

-- -----------------------------------------------------
-- Tabela mydb.auditor
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS auditor (
  idauditor SERIAL PRIMARY KEY,
  nome VARCHAR(45) NOT NULL,
  registro VARCHAR(45) NOT NULL UNIQUE
);

-- -----------------------------------------------------
-- Tabela mydb.auditoria
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS auditoria (
  idauditoria SERIAL PRIMARY KEY,
  data_auditoria DATE NOT NULL,
  auditor_idauditor INT NOT NULL,
  transacao_idtransacao INT NOT NULL,
  CONSTRAINT fk_auditoria_auditor
    FOREIGN KEY (auditor_idauditor)
    REFERENCES auditor (idauditor)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION,
  CONSTRAINT fk_auditoria_transacao
    FOREIGN KEY (transacao_idtransacao)
    REFERENCES transacao (idtransacao)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION
);

CREATE INDEX IF NOT EXISTS idx_auditoria_auditor ON auditoria(auditor_idauditor);
CREATE INDEX IF NOT EXISTS idx_auditoria_transacao ON auditoria(transacao_idtransacao);

-- -----------------------------------------------------
-- Tabela mydb.membro_associado
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS membro_associado (
  idmembro SERIAL PRIMARY KEY,
  nome VARCHAR(99) NOT NULL,
  cpf VARCHAR(45) NOT NULL UNIQUE,
  email VARCHAR(99) NOT NULL UNIQUE,
  telefone VARCHAR(45) NOT NULL UNIQUE,
  empresa_idempresa INT NOT NULL,
  CONSTRAINT fk_membro_associado_empresa
    FOREIGN KEY (empresa_idempresa)
    REFERENCES empresa (idempresa)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION
);

CREATE INDEX IF NOT EXISTS idx_membro_associado_empresa ON membro_associado(empresa_idempresa);

-- -----------------------------------------------------
-- Tabela mydb.documento
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS documento (
  iddocumento SERIAL PRIMARY KEY,
  arquivo_digitalizado TEXT NOT NULL UNIQUE,
  documentocol VARCHAR(45),
  membro_associado_idmembro INT NOT NULL,
  CONSTRAINT fk_documento_membro_associado
    FOREIGN KEY (membro_associado_idmembro)
    REFERENCES membro_associado (idmembro)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION
);

CREATE INDEX IF NOT EXISTS idx_documento_membro ON documento(membro_associado_idmembro);

-- -----------------------------------------------------
-- Tabela mydb.jogador
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS jogador (
  idjogador SERIAL PRIMARY KEY,
  endereco_idendereco INT NOT NULL,
  posicao VARCHAR(45) NOT NULL,
  CONSTRAINT fk_jogador_endereco
    FOREIGN KEY (endereco_idendereco)
    REFERENCES endereco (idendereco)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION
);

CREATE INDEX IF NOT EXISTS idx_jogador_endereco ON jogador(endereco_idendereco);
