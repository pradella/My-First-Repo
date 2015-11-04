# My-First-Repo
My First GIT Repository

This is my first line trying to document this software.

#### Cargo atual do usuário

A partir de novembro/2015, foi criada uma tabela para controle do histórico dos cargos dos usuários (USUARIOCARGO_HISTORICO). Para obter o cargo atual do usuário, considero o campo DT_CARGO_TERMINO IS NULL (ou seja, não há data de término do cargo, significa que ele está trabalhando ainda na SOU, e neste cargo). Essa informação do cargo atual pode ser das seguintes formas:

```sql
-- Diretamente na view vUSUARIO
SELECT CS_USUARIO_NOME, CARGO FROM vUSUARIO
```

```sql
-- Ou filtrando na tabela USUARIOCARGO_HISTORICO onde DT_CARGO_TERMINO IS NULL
-- (ou seja, NULL indica que ele ainda está naquele cargo)
SELECT * FROM USUARIOCARGO_HISTORICO WHERE DT_CARGO_TERMINO IS NULL
```


#### Data de início e término do Projeto

Para obter a data de início e término de um projeto, obtenha pelo seguinte comando:

```sql
SELECT ID_PROJETO, DT_INICIO_CONTRATO_OU_CRONOGRAMA, DT_TERMINO_CONTRATO_OU_CRONOGRAMA
FROM vPROJETO
```
