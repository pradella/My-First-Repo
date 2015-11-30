# My-First-Repo
My First GIT Repository

This is my first line trying to document this software.

## Índice

[Cargo atual do usuário](https://github.com/pradella/My-First-Repo#cargo-atual-do-usuario)  
[Data de início, término e horas contratadas do Projeto](https://github.com/pradella/My-First-Repo#data-de-início-término-e-horas-contratadas-do-projeto)  
[Status atual do projeto e última data de mudança](https://github.com/pradella/My-First-Repo#status-atual-do-projeto-e-última-data-de-mudança)  
[Horas contratadas](https://github.com/pradella/My-First-Repo/#horas-contratadas)  

#### Cargo atual do usuario

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


#### Data de início, término e horas contratadas do Projeto

Para obter a data de início, término e horas contratadas de um projeto, foi criada uma view que centraliza tais infromações: vPROJETO_HORASCONTRATADAS_RESUMO. Veja abaixo alguns exemplos:

```sql
-- obtém dados do contrato (inputados na tela de edição do projeto)
SELECT ID_PROJETO, NU_CONTRATO_HORA, DT_CONTRATO_INICIO, DT_CONTRATO_TERMINO
FROM vPROJETO_HORASCONTRATADAS_RESUMO

-- obtém dados do cronograma (obtidos nas tarefas do cronograma criado para o projeto)
SELECT ID_PROJETO, NU_CRONOGRAMA_HORA, DT_CRONOGRAMA_INICIO, DT_CRONOGRAMA_TERMINO
FROM vPROJETO_HORASCONTRATADAS_RESUMO

-- obtém os dados de uma das duas fontes, priorizando contrato; se não achar, pega do cronograma
SELECT ID_PROJETO, NU_HORACONTRATADA_CONTRATO_OU_CRONOGRAMA, DT_INICIO_CONTRATO_OU_CRONOGRAMA, DT_TERMINO_CONTRATO_OU_CRONOGRAMA
FROM vPROJETO_HORASCONTRATADAS_RESUMO
```


#### Status atual do projeto e última data de mudança

O status do projeto pode ser obtido na tabela PROJETOSTATUS.
Foi criada uma tabela para logar as mudanças de status: PROJETOSTATUS_LOG.
Assim, para obter o status atual do projeto, bem como a ultima data de mudança, utilize o comando abaixo:

```sql
SELECT ID_PROJETO, ID_PROJETOSTATUS, CS_PROJETOSTATUS_NOME, DT_PROJETOSTATUS_CHANGED, DT_PROJETOSTATUS_CHANGED_DAYSAGO
FROM vPROJETO
```

Exemplo: 
- Filtrando apenas os projetos finalizados
```sql
SELECT ID_PROJETO, ID_PROJETOSTATUS, CS_PROJETOSTATUS_NOME, DT_PROJETOSTATUS_CHANGED, DT_PROJETOSTATUS_CHANGED_DAYSAGO 
FROM vPROJETO WHERE ID_PROJETOSTATUS = 4
```


#### Horas contratadas

O controle de horas contratadas é feito de duas maneiras:
- via cronograma (obtido da somatória do campo NU_TAREFA_TRABALHO*8 - 1 dia = 8 horas)
- via input manual (obtido da somatória do campo NU_HORASCONTRATADAS da tabela PROJETO_HORASCONTRATADAS)

Para facilitar a obtenção dessa informação, foi criada a view vPROJETO_HORASCONTRATADAS_RESUMO, pois quando o projeto possui essas duas informações cadastradas (cronograma e input manual), essa view automaticamente traz uma informação, priorizando o input manual. 

```sql
SELECT
NU_CONTRATO_HORA,
DT_CONTRATO_INICIO,
DT_CONTRATO_TERMINO, 
NU_CRONOGRAMA_HORA,
DT_CRONOGRAMA_INICIO,
DT_CRONOGRAMA_TERMINO,
NU_HORACONTRATADA_CONTRATO_OU_CRONOGRAMA,
DT_INICIO_CONTRATO_OU_CRONOGRAMA,
DT_TERMINO_CONTRATO_OU_CRONOGRAMA
FROM vPROJETO_HORASCONTRATADAS_RESUMO
```

Para finalidade de consolidação de dados, também foram criadas duas funções que trazem essa informação quebrada por mês e/ou dia:

```sql
SELECT 
ID_PROJETO, 
AREA, 
SUBAREA, 
ANO, 
MES, 
DATA, 
HORAS_CONTRATADAS 
FROM fHORAS_CONTRATADAS('2015-01-01', '2015-12-31') 
OPTION (MAXRECURSION 0)
```

Também foi criada uma função que traz o consolidado mês a mês de horas contratadas informando o ID do projeto como parâmetro. Repare que ele traz as colunas de hora contratada de input manual (NU_CONTRATO_HORA) e crongorama (NU_CRONOGRAMA_HORA), onde é papel da aplicação definir qual delas irá utilizar.

```sql
SELECT 
ID_PROJETO, 
ANO, 
MES, 
NU_CONTRATO_HORA, 
NU_CRONOGRAMA_HORA, 
NU_ENTREGUE_HORA 
FROM fHORAS_CONTRATADAS_MESAMES(1583) 
```
