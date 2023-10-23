# BD-STORED-PROCEDURES
Atividades realizadas na materia Banco de Dados.
Realizando a criação de banco de dados e alguns comandos.
## ETAPA 1:
1-Crie um banco de dados para armazenar alunos, cursos e professores de uma
universidade:
```
create table alunos (
id int not null primary key auto_increment,
nome varchar(50) not null,
email varchar(50) not null,
alunos_cursos int,

Foreign key(alunos_cursos) references cursos(id)

);

create table professores (
id int not null primary key auto_increment,
nome varchar(10.2)not null,
disciplina varchar(10.2)not null,

Foreign key (disciplina) references cursos(id)
);

create table cursos (
id int not null primary key auto_increment,
nome varchar(10.2) not null
);

alter table alunos add column sobrenome varchar(50);

insert into alunos (id,nome, sobrenome, alunos_cursos) values (01,'Igor','ferreira',1);
insert into alunos (id,nome, sobrenome, alunos_cursos) values (02,'Camila','bertaia',2);

insert into professores (id,nome, disciplina) values (01,'Rafael','Matematica');
insert into professores (id,nome, disciplina) values (02,'Joao','Direito Penal');
insert into professores (id,nome, disciplina) values (03,'Pedor','Filosofia');
insert into professores (id,nome, disciplina) values (04,'Carlos','POO');


insert into cursos (id,nome) values (01,'Engenharia de Software');
insert into cursos (id,nome) values (02,'Direito');
insert into cursos (id,nome) values (03,'Filosofia');
insert into cursos (id,nome) values (04,'Analise Desenvolvimento Sistemas');


```
2-Faça a modelagem do banco e identifique as entidades, seus atributos e relacionamentos:

![Etapa 1-2](https://github.com/Ig0rFA/BD-STORED-PROCEDURES/blob/main/BD-STORED-PRODUCERES/MODELAGEM-UNIVERSIDADE.png)

3-Utilize Stored Procedures para automatizar a inserção e seleção dos cursos:
```
DELIMITER //

create procedure insercao_cursos(
in nome varchar(50)
)
begin
	insert into cursos (nome) values (nome);
	
END//

DELIMITER ;

CALL insercao_cursos('Mecatronica');

select * from cursos;

DELIMITER //

create procedure selecao_cursos()
begin
	
    SELECT * from cursos;
	
END//

DELIMITER ;

CALL selecao_cursos();

DELIMITER //
```
4-O aluno possui um email que deve ter seu endereço gerado automaticamente no seguinte formato:
nome.sobrenome@dominio.com:
```
DELIMITER //

CREATE PROCEDURE email_padrao(
    in p_id int
)
BEGIN
    declare v_generated_email varchar(50);

    
    select concat(nome, '.', sobrenome, '@dominio.com') into v_generated_email from alunos where id = p_id;
   
    
    update alunos set email = v_generated_email where id = p_id;
END//

DELIMITER ;

call email_padrao(1);

```
5-Como fica o email se duas pessoas tiverem o mesmo nome e sobrenome?:
```
DELIMITER //

CREATE PROCEDURE email_padrao_verificacao()
BEGIN
    UPDATE alunos a
    JOIN (
        SELECT id, nome, sobrenome, 
               ROW_NUMBER() OVER (PARTITION BY nome, sobrenome ORDER BY id) as row_num
        FROM alunos
    ) b ON a.id = b.id
    SET a.email = CONCAT(b.nome, '.', b.sobrenome, 
                         CASE WHEN b.row_num > 1 THEN b.row_num ELSE '' END, 
                         '@dominio.com');
END//

DELIMITER ;

```
## Link completo SQL:

![Etapa 1-6](https://github.com/Ig0rFA/BD-STORED-PROCEDURES/blob/main/BD-STORED-PRODUCERES/SQL-ALUNOS-PROFESSORES-CURSOS.sql)

