# 1) Criação de tabelas
## Regras:
1. Criação de Table
2. Criação de View
3. Criação de Trigger
4. Criação do Sinônimo
### Supor a criação de uma tabela para Empréstimos
#### Padrôes de tabelas, views, triggers, SP: 
1. Prefixo da tabela: M4T_(NOME_TABLE)
2. Prefixo da View: M4_(NOME_TABLE)
3. Prefixo da Trigger: m4tg_upd_(NOME_TABLE)
4. Préfixo da Store Procedure(Se necessário): m4pr_(NOME)
#### Padrões de constraints 
1. Primary Key Préfixo: pk_(NOME_TABLE)
2. Foreign Key Prefixo: fk_(TABLE_ORIGEM)(TABLE_DESTINO)
#### Campos obrigatórios (Toda nova tabela deve conter)
```sql
  coment                          VARCHAR2(2000),
  id_utiliz                       VARCHAR2(40) default USER,
  data_ult_actual                 DATE default SYSDATE
```
#### Não existe chave auto-icrement
> Importante saber que toda relação com a tabela m4t_empregad devem ser feitas FKs referenciando os seguintes campos:
```sql
  id_sociedad                     VARCHAR2(2) NOT NULL,
  id_empregad                     VARCHAR2(10) NOT NULL,
  dat_ini_act_emp                 DATE NOT NULL,  
```
> Com isso, é importante ter o conhecimento básico de três tabelas:
```sql
  m4t_sociedades
  m4t_empregad
  m4t_fases_act
```
#### Abaixo temos um exemplo ilustrativo utilizando database ORACLE:
```sql

 CREATE TABLE m4t_emprestimos
(
  id_sociedad                     VARCHAR2(2) NOT NULL,
  id_empregad                     VARCHAR2(10) NOT NULL,
  dat_ini_act_emp                 DATE NOT NULL,  
  dat_inicio                      DATE NOT NULL,
  id_tipo_emp                     VARCHAR2(10) NOT NULL,
  dat_fim                         DATE,
  --valor                           NUMERIC(14,4),  
  --prazo                           INTEGER,       
  coment                          VARCHAR2(2000),
  id_utiliz                       VARCHAR2(40) default USER,
  data_ult_actual                 DATE default SYSDATE,
  
  CONSTRAINT pk_emprestimos PRIMARY KEY(id_sociedad, id_empregad, dat_ini_act_emp, dat_inicio),
  CONSTRAINT fk_tipoemprestimo_emprestimo FOREIGN KEY(id_tipo_emp) 
             REFERENCES m4t_tipos_emprestimos(id_tipo_emp), --ON DELETE CASCADE
  CONSTRAINT fk_fasesatv_emprestimo FOREIGN KEY(id_sociedad, id_empregad, dat_ini_act_emp) 
             REFERENCES m4t_fases_act(id_sociedad, id_empregad, dat_ini_act_emp)           
);
--ALTER TABLE m4t_emprestimos ADD CONSTRAINT fk_fasesatv_emprestimo FOREIGN KEY(id_sociedad, id_empregad, dat_ini_act_emp) 
--             REFERENCES m4t_fases_act(id_sociedad, id_empregad, dat_ini_act_emp) 
--cria view
CREATE OR REPLACE VIEW m4_emprestimos AS SELECT * FROM m4t_emprestimos; 
/

-- criar trigger
CREATE OR REPLACE TRIGGER m4tg_upd_emprestimos
BEFORE UPDATE OR INSERT ON m4t_emprestimos FOR EACH ROW
BEGIN
  :new.data_ult_actual := SYSDATE;
  :new.id_utiliz := USER;
END;
/
--criar sinonimo
CREATE PUBLIC SYNONYM m4_emprestimos FOR pfabre.m4_emprestimos;
/ 

```
> No caso dos sinônimos, no exemplo acima tivemos o nome ***pfabre.m4_emprestimos***.
> ***pfabre***, equivale ao nome do usuário logado no banco e ***m4_emprestimos***, o nome da ***VIEW*** criada.