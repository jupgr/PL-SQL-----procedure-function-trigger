# PL-SQL-----PROCEDURE-FUNCTION-E-TRIGGER

Banco de Dados – Mackenzie

Este repositório contém apenas um README com os materiais estudados em Banco de Dados, abordando programação interna do SGBD com PL/SQL, incluindo blocos, functions, procedures e triggers.


### Conteúdo:
Bloco PL/SQL - 
Uso de variáveis -
SELECT INTO - 
 IF / ELSE 



###  Atualização de dados via programação : 
DBMS_OUTPUT - 
Function - 
Procedure - 
Trigger 



## Bloco PL/SQL – Aumento Salarial Professor

```text
DECLARE
    v_cod_prof   Professor.Cod_Professor%TYPE;
    v_qtd_orient INTEGER;
    v_sal_ant    Professor.Salario%TYPE;
BEGIN
    -- Descobrir o código da Camila
    SELECT Cod_Professor INTO v_cod_prof
    FROM Professor
    WHERE Nome_Professor = 'Camila';

    -- Contar quantos alunos ela orienta
    SELECT COUNT(*) INTO v_qtd_orient
    FROM Aluno
    WHERE Cod_Professor_Orientador = v_cod_prof;

    -- Verificar condição e atualizar salário
    IF v_qtd_orient > 1 THEN
        SELECT Salario INTO v_sal_ant FROM Professor WHERE Cod_Professor = v_cod_prof;

        UPDATE Professor
           SET Salario = Salario * 1.10
         WHERE Cod_Professor = v_cod_prof;

        DBMS_OUTPUT.PUT_LINE(
            'Aumento aplicado (10%). Salário anterior: ' || v_sal_ant ||
            ' | Novo: ' || (v_sal_ant * 1.10)
        );
    ELSE
        DBMS_OUTPUT.PUT_LINE('Não teve aumento, pois não orientou mais de um aluno.');
    END IF;
END;
/

```


## Function — retorna nome do professor pelo código

```text

CREATE OR REPLACE FUNCTION fn_nome_professor(p_cod NUMBER)
RETURN VARCHAR2
IS
    v_nome Professor.Nome_Professor%TYPE;
BEGIN
    SELECT Nome_Professor
      INTO v_nome
      FROM Professor
     WHERE Cod_Professor = p_cod;

    RETURN v_nome;
END;
/

```


## Procedure — cadastra aluno

```text
CREATE OR REPLACE PROCEDURE sp_cadastra_aluno (
    p_nome     IN VARCHAR2,
    p_email    IN VARCHAR2,
    p_prof_cod IN NUMBER
)
AS
BEGIN
    INSERT INTO Aluno (Nome_Aluno, Email_Aluno, Cod_Professor_Orientador)
    VALUES (p_nome, p_email, p_prof_cod);

    COMMIT;

    DBMS_OUTPUT.PUT_LINE('Aluno cadastrado com sucesso!');
END;
/
```

## Trigger — registra data de alteração no Professor
```text
CREATE OR REPLACE TRIGGER trg_professor_update
BEFORE UPDATE ON Professor
FOR EACH ROW
BEGIN
    :NEW.Data_Alteracao := SYSDATE;
END;
/
```
