# My-First-Repo
My First GIT Repository

This is my first line trying to document this software.

## Índice

[Cargo atual do usuário](#Cargo-atual-do-usuario)  
[Data de início, término e horas contratadas do Projeto](#Data de início, término e horas contratadas do Projeto)  
[Status atual do projeto e última data de mudança](#Status atual do projeto e última data de mudança)  

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
