[Voltar para o menu](readme.md)
# Criação de rúbricas
 <sup>Caminho: Configuração -> Cálculo -> Ficheiro -> ***Criar rúbricas***</sup>

### Criação do campo no database
> A primeira coisa a se pensar quando se vai criar uma rúbrica, é a criação do campo que irá armazenar o valor da rúbrica no banco de dados.
1. Escolher uma tabela para criação do novo campo. Para isso temos uma lista de 4 tabelas que poderemos utilizar o bom senso de acordo com a quantidade de colunas que cada uma possua. Tender a escolher a que tiver menos campos Dentre:
```sql 
m4t_acumulado_c
m4t_acumulado_rl
m4t_acumulado_rl1
m4t_acumulado_rl2

--comando para criação do campo
ALTER TABLE m4t_acumulado_rl2 ADD (novo_campo NUMERIC(14,4));
--onde novo_campo é o nome do campo que deseja criar. 
--este nome é importante para configurar a rúbrica.
```
2. Muito importante, não esqueça de atualizar a view m4_acumulado_rlx... 
```sql
--No exemplo em questão, foi utilizada a tabela m4t_acumulado_rl2
create or replace view m4_acumulado_rl2 as
select * from m4t_acumulado_rl2
with check option;
```
3. Pronto, agora o próximo passo é acessar o meta4 para criação da rúbrica.
<sup>Caminho: Configuração -> Cálculo -> Ficheiro -> ***Criar rúbricas***</sup>

### A configurar o campo criado anteriormente
1. Na ecrã de edição de rúbricas, criar em ***novo*** para criação da rúbrica.
2. Na seção ***Tipo de saída*** temos:
 1. ***Acumula para o acumulado*** - escolha entre 
```
CORTO se o campo foi criado na tabela m4t_acumulado_c
LARGO se o campo foi criado na tabela m4t_acumulado_rl
LARGO1 se o campo foi criado na tabela m4t_acumulado_rl1
LARGO2 se o campo foi criado na tabela m4t_acumulado_rl2
```
 2. Em ***Campo no acumulado***, escolher o campo criado.

> Existem regras não obrigatórias porém, para melhor entendimento, se faz necessário por bom senso, as cumprir.
## Regras por natureza:
1. Se bonus, deve-se adotar uma numeração abaixo de 5000
2. Se Desconto, deve-se adotar uma numeração acima de 5000
## Regras de numeração:
1. xxx0 - rúbricas de unidade
2. xxx2 - rúbricas de valores 
3. xxx3 - rúbricas de totais 
### Exemplo: 
```
#1430 - unidade em dias
#1432 - preço do prêmio 
#1433 - totais #1430 * #1432
```
> Vale lembrar que não é regra sempre criar a base de 3 rúbricas. Muitas vezes só há necessidade de uma ou a combinação delas.
Algumas vezes se faz necessário a criação de mais de 3. 

### a respeito da #1430 (Rúbrica de unidade)  
1. Tipo Criação ***I***
2. Classificação ***Verificar de acordo com a natureza da unidade*** (D, U...)

### a respeito da #1432 (Rúbrica de unidade) 
1. Tipo Criação ***I***
2. Classificação ***P*** 
3. Se valor monetário, selecionar ***Monetário***

### a respeito da #1433 (Rúbrica de cálculo) estas podem ir diretamente para o recibo. Com isso segue algumas regras:
1. Se Abono, em ***Rúbricas de salário*** devemos:
  1. Tipo Criação ***I***
  2. Selecionar na opção de ***Classificação*** a opção **DV** 
  3. Tipo da rúbrica: ***NÚmero*** 
  4. Monetário (Selecionar)

2. Na ecrã: ***Histórico de Normas***
  1. Data de início: ***01/01/1994*** (Geralmente)
  2. Data de fim: Geralmente não é necessário informar
  3. ***Saída para totais*** selecionar:
    * Abonos
    * Abonos cargo Empresa
    * I.R.S (Rúbricas Tributáveis)
    * S. Social Base 1
    * S. Social Empresa
    * SS - Código de valor (CÓDIGO)
    * onde (CÓDIGO) pode ser (2, A B C D F H I N M O P R S T X)

  4. Em ***Revisão*** 
    * ***Comportamento em Recálculo*** - geralmente se mantém a opção: ***Valor calculado*** (Depende da necessidade)
    * ***Comportamento em Diferenças*** -  geralmente se mantém a opção: ***Diferença*** (Depende da necessidade)
  5. Tipo de cálculo
    * f(x) (fórmula) local onde se realizar a edição da fórmula (se necessário)

## Fazendo surgir no recibo.
> Apenas criar e configurar corretamente uma rúbrica não a faz aparecer automaticamente na ecrã de recibos após um cálculo de processamento de salários. Para isso, deverá seguir o seguinte passo:
1. Acessar: Base de Dados -> Processos -> Desenhador de linhas de cálculo
2. Clicar no ícone de ***Abrir*** e selecionar ***CALCULATION*** clicar em (Base de Dados)
  1. Unidades Informar a rúbrica de unidade (caso exista) exemplo: 1430
  2. Preço Informar a rúbrica de preço (caso exista) exemplo: 1432
  3. Rúbricas - Informar a descrição que deseja que apareça no recibo
  4. Abonos Informar a rúbrica de abono. Exemplo 1433
> OBS: Se a rúbrica for de Retenção, então deve-se informar na coluna de retenção


[Voltar para o menu](readme.md)