# Oracle PL/SQL Comprehensive Notes

This document provides a complete, structured, and professional guide to
Oracle PL/SQL. It covers foundational concepts, anonymous blocks, named
blocks (procedures, functions, packages, triggers), cursors, exception
handling, collections, dynamic SQL, performance tuning, and best practices.
These notes are suitable for beginners, intermediate learners, and as a
reference for professional developers.

## Table of Contents

- [1. Introduction to PL/SQL](#1-introduction-to-plsql)
  - [1.1 What is PL/SQL?](#11-what-is-plsql)
  - [1.2 PL/SQL Architecture and Context Switching](#12-plsql-architecture-and-context-switching)
  - [1.3 Benefits of PL/SQL](#13-benefits-of-plsql)
  - [1.4 PL/SQL Block Structure](#14-plsql-block-structure)
- [2. Anonymous Blocks](#2-anonymous-blocks)
  - [2.1 Basic Anonymous Block](#21-basic-anonymous-block)
  - [2.2 Setting Up Output (DBMS_OUTPUT)](#22-setting-up-output-dbs_output)
  - [2.3 Working with Variables](#23-working-with-variables)
  - [2.4 Variable Qualifiers: CONSTANT and NOT NULL](#24-variable-qualifiers-constant-and-not-null)
  - [2.5 SELECT INTO in PL/SQL](#25-select-into-in-plsql)
  - [2.6 DML Operations in PL/SQL](#26-dml-operations-in-plsql)
  - [2.7 Transactions and Savepoints](#27-transactions-and-savepoints)
  - [2.8 %TYPE and %ROWTYPE Attributes](#28-type-and-rowtype-attributes)
- [3. Conditional and Iterative Control](#3-conditional-and-iterative-control)
  - [3.1 IF Statement](#31-if-statement)
  - [3.2 CASE Statement](#32-case-statement)
  - [3.3 Loops: Basic LOOP, WHILE, FOR, Nested](#33-loops-basic-loop-while-for-nested)
- [4. Cursors](#4-cursors)
  - [4.1 What is a Cursor?](#41-what-is-a-cursor)
  - [4.2 Implicit Cursors](#42-implicit-cursors)
  - [4.3 Explicit Cursors](#43-explicit-cursors)
  - [4.4 Cursor FOR Loops](#44-cursor-for-loops)
  - [4.5 Cursors with Parameters](#45-cursors-with-parameters)
  - [4.6 REF Cursors (Dynamic Cursors)](#46-ref-cursors-dynamic-cursors)
  - [4.7 Cursor Attributes](#47-cursor-attributes)
- [5. Stored Procedures](#5-stored-procedures)
  - [5.1 Creating and Executing Procedures](#51-creating-and-executing-procedures)
  - [5.2 Procedure Parameters: IN, OUT, IN OUT](#52-procedure-parameters-in-out-in-out)
  - [5.3 Examples of Stored Procedures](#53-examples-of-stored-procedures)
- [6. Functions](#6-functions)
  - [6.1 Creating and Executing Functions](#61-creating-and-executing-functions)
  - [6.2 Function Examples](#62-function-examples)
  - [6.3 Function Overloading](#63-function-overloading)
- [7. Packages](#7-packages)
  - [7.1 Package Specification and Body](#71-package-specification-and-body)
  - [7.2 Public and Private Constructs](#72-public-and-private-constructs)
  - [7.3 Advantages of Packages](#73-advantages-of-packages)
- [8. Exception Handling](#8-exception-handling)
  - [8.1 Predefined Exceptions](#81-predefined-exceptions)
  - [8.2 User-Defined Exceptions](#82-user-defined-exceptions)
  - [8.3 SQLCODE and SQLERRM](#83-sqlcode-and-sqlerrm)
  - [8.4 RAISE_APPLICATION_ERROR](#84-raise_application_error)
- [9. Triggers](#9-triggers)
  - [9.1 DML Triggers (BEFORE/AFTER, ROW/STATEMENT)](#91-dml-triggers-beforeafter-rowstatement)
  - [9.2 :OLD and :NEW Qualifiers](#92-old-and-new-qualifiers)
  - [9.3 INSTEAD OF Triggers for Views](#93-instead-of-triggers-for-views)
  - [9.4 DDL and System Triggers](#94-ddl-and-system-triggers)
  - [9.5 The Mutating Table Error and How to Fix It](#95-the-mutating-table-error-and-how-to-fix-it)
- [10. Autonomous Transactions](#10-autonomous-transactions)
  - [10.1 Pragma AUTONOMOUS_TRANSACTION](#101-pragma-autonomous_transaction)
  - [10.2 Use Cases for Autonomous Transactions](#102-use-cases-for-autonomous-transactions)
- [11. Collections and Records](#11-collections-and-records)
  - [11.1 PL/SQL Records (TYPE RECORD)](#111-plsql-records-type-record)
  - [11.2 Associative Arrays (Index-By Tables)](#112-associative-arrays-index-by-tables)
  - [11.3 Nested Tables](#113-nested-tables)
  - [11.4 VARRAYs (Variable-Size Arrays)](#114-varrays-variable-size-arrays)
  - [11.5 Collection Methods](#115-collection-methods)
- [12. Bulk Operations for Performance](#12-bulk-operations-for-performance)
  - [12.1 BULK COLLECT for Fast Data Retrieval](#121-bulk-collect-for-fast-data-retrieval)
  - [12.2 FORALL for Bulk DML Operations](#122-forall-for-bulk-dml-operations)
  - [12.3 LIMIT Clause for Managing Memory](#123-limit-clause-for-managing-memory)
- [13. Dynamic SQL](#13-dynamic-sql)
  - [13.1 EXECUTE IMMEDIATE for DDL and DML](#131-execute-immediate-for-ddl-and-dml)
  - [13.2 Using Bind Variables with EXECUTE IMMEDIATE](#132-using-bind-variables-with-execute-immediate)
- [14. Global Temporary Tables](#14-global-temporary-tables)
- [15. Database Setup Scripts for Practice](#15-database-setup-scripts-for-practice)
  - [15.1 Create and Populate Employees Table](#151-create-and-populate-employees-table)
  - [15.2 Create and Populate Departments Table](#152-create-and-populate-departments-table)
  - [15.3 Create and Populate Customer Table](#153-create-and-populate-customer-table)
- [16. Core PL/SQL Best Practices and Summary](#16-core-plsql-best-practices-and-summary)

---

## 1. Introduction to PL/SQL

### 1.1 What is PL/SQL?

PL/SQL stands for **Procedural Language extension to SQL**.
It is Oracle Corporation's proprietary programming language
embedded within the Oracle Database. It adds procedural
constructs (like loops, conditions, and variables) to standard SQL,
allowing you to build complex, business-logic-driven operations
directly inside the database.

**Key Difference Between SQL and PL/SQL:**

- **SQL** is a declarative language. You tell the database *what*
  data you want (e.g., `SELECT ... FROM ... WHERE ...`), but you
  cannot use conditional logic (IF-THEN-ELSE), loops, or variables.
- **PL/SQL** is a procedural language. You tell the database *how*
  to process data step-by-step. You can use variables, IF statements,
  loops, error handling, and more. PL/SQL is to Oracle what T-SQL is
  to SQL Server.

### 1.2 PL/SQL Architecture and Context Switching

When you execute a PL/SQL block, the Oracle Database server uses a
**PL/SQL engine**. This engine separates the procedural code
(e.g., loops, IF statements) from the SQL code
(e.g., SELECT, INSERT, UPDATE, DELETE).

- **Procedural statements** are handled by the Procedural Statement
  Executor.
- **SQL statements** are sent to the SQL Statement Executor.

**Context Switching** refers to the overhead of moving back and
forth between these two executors. For example, a loop that executes
a single SQL statement 10,000 times will cause 10,000 context
switches, which significantly impacts performance. Techniques like
`BULK COLLECT` and `FORALL` (covered later) minimize context switching.

### 1.3 Benefits of PL/SQL

- **Procedural Control**: Use variables, loops, and conditional logic.
- **Improved Performance**: Reduce network traffic by sending entire
  blocks of code to the database instead of individual SQL statements.
- **Modularity**: Group related logic into procedures, functions,
  and packages.
- **Error Handling**: Gracefully handle runtime errors with exceptions.
- **Portability**: PL/SQL code written for Oracle can run on any
  Oracle Database platform.

### 1.4 PL/SQL Block Structure

Every PL/SQL code is organized into blocks. A block consists of up
to three sections:

1.  **Declaration Section (Optional)**: `DECLARE` – Define variables,
    constants, cursors, etc.
2.  **Execution Section (Mandatory)**: `BEGIN` ... `END;` – Contains
    the main logic (SQL statements, assignments, procedure calls).
3.  **Exception Handling Section (Optional)**: `EXCEPTION` – Code to
    handle runtime errors.

There are two types of PL/SQL blocks:

- **Anonymous Block**: Has no name. It is compiled and executed each
  time it is run. It is not stored in the database.
- **Named Block**: Has a name and is stored as a database object
  (e.g., Procedure, Function, Package, Trigger). It is compiled once
  and can be executed many times.

---

## 2. Anonymous Blocks

Anonymous blocks are ideal for ad-hoc scripts, testing, or one-time
operations.

### 2.1 Basic Anonymous Block

This is the simplest PL/SQL block. It contains only the mandatory
`BEGIN` and `END;` sections.

**Example: Simple Anonymous Block**

```sql
BEGIN
    -- No declaration, no exception handling. Just a simple statement.
    NULL;  -- NULL is a valid executable statement that does nothing.
END;
/
```

**Expected Output:**

```
PL/SQL procedure successfully completed.
```

### 2.2 Setting Up Output (DBMS_OUTPUT)

`DBMS_OUTPUT` is a built-in Oracle package that allows you to display
text from PL/SQL blocks. To see the output in SQL Developer or
SQL*Plus, you must enable it.

- `SET SERVEROUTPUT ON;` – Enables output for the current session.
- `DBMS_OUTPUT.PUT_LINE('Your text here');` – Prints a line of text.

**Example: Hello World**

```sql
SET SERVEROUTPUT ON;

BEGIN
    DBMS_OUTPUT.PUT_LINE('Hello, welcome to the PL/SQL session.');
END;
/
```

**Expected Output:**

```
Hello, welcome to the PL/SQL session.
```

### 2.3 Working with Variables

Variables must be declared in the `DECLARE` section. Use the
assignment operator `:=` to assign a value.

**Example: Adding Two Numbers**

```sql
SET SERVEROUTPUT ON;

DECLARE
    num1 NUMBER(10,2) := 10.23;
    num2 NUMBER(10,2) := 20.97;
    num3 NUMBER(10,2);
BEGIN
    num3 := num1 + num2;

    DBMS_OUTPUT.PUT_LINE('Equation: ' || num1 || ' + ' || num2 ||
                         ' = ' || num3);
END;
/
```

**Expected Output:**

```
Equation: 10.23 + 20.97 = 31.2
```

### 2.4 Variable Qualifiers: CONSTANT and NOT NULL

- `CONSTANT`: The variable's value cannot change after initialization.
- `NOT NULL`: The variable cannot hold a `NULL` value. It must be
  initialized.

**Example: Using CONSTANT and NOT NULL**

```sql
SET SERVEROUTPUT ON;

DECLARE
    num1 CONSTANT NUMBER(12,3) := 10;
    num2 CONSTANT NUMBER(12,3) := 20;
    num3 NUMBER(12,3) NOT NULL := 0;  -- Must have a default value
BEGIN
    num3 := num1 + num2;
    DBMS_OUTPUT.PUT_LINE('Total: ' || num3);
END;
/
```

**Expected Output:**

```
Total: 30
```

### 2.5 SELECT INTO in PL/SQL

Use `SELECT ... INTO ...` to fetch a single row from a table and
store its values into PL/SQL variables.

**Important:** If the query returns no rows, Oracle raises
`NO_DATA_FOUND`. If it returns more than one row, it raises
`TOO_MANY_ROWS`. Use proper exception handling (see Section 8) to
manage these cases.

**Example: Selecting a Single Value**

```sql
SET SERVEROUTPUT ON;

DECLARE
    v_phone_number EMPLOYEE.PHONE_NUMBER%TYPE;
BEGIN
    SELECT PHONE_NUMBER
    INTO v_phone_number
    FROM EMPLOYEE
    WHERE EMPLOYEE_ID = 2;

    DBMS_OUTPUT.PUT_LINE('Phone Number: ' || v_phone_number);
END;
/
```

**Expected Output (based on sample data):**

```
Phone Number: 0722222222
```

### 2.6 DML Operations in PL/SQL

You can execute `INSERT`, `UPDATE`, and `DELETE` statements directly
inside a PL/SQL block. Remember to `COMMIT` or `ROLLBACK` your
transactions.

**Example: Inserting a New Customer**

```sql
SET SERVEROUTPUT ON;

BEGIN
    INSERT INTO CUSTOMER (CUST_ID, CUST_NAME, MOBILE_NO, AGE, CITY,
                          CITY_ID)
    VALUES (10, 'John Smith', '0760000007', 28, 'Braamfontein', 106);

    COMMIT;  -- Make the change permanent
    DBMS_OUTPUT.PUT_LINE('Customer inserted successfully.');
END;
/
```

**Expected Output:**

```
Customer inserted successfully.
```

### 2.7 Transactions and Savepoints

A transaction is a logical unit of work. It begins with the first
DML statement and ends with a `COMMIT` (makes all changes permanent)
or `ROLLBACK` (undoes all changes).

- **`SAVEPOINT <name>`**: Creates a marker within a transaction.
- **`ROLLBACK TO <savepoint_name>`**: Undoes changes only up to that
  savepoint.

**Example: Using Savepoints**

```sql
BEGIN
    INSERT INTO CUSTOMER (CUST_ID, CUST_NAME) VALUES (100, 'Alice');
    SAVEPOINT after_alice;

    INSERT INTO CUSTOMER (CUST_ID, CUST_NAME) VALUES (101, 'Bob');
    SAVEPOINT after_bob;

    INSERT INTO CUSTOMER (CUST_ID, CUST_NAME) VALUES (102, 'Charlie');

    -- Undo the last two inserts (Bob and Charlie)
    ROLLBACK TO after_alice;

    COMMIT;  -- Only Alice is permanently saved.
END;
/
```

### 2.8 %TYPE and %ROWTYPE Attributes

These attributes provide **anchor declarations**, making your code
more robust and easier to maintain. They automatically inherit the
data type of a database column or a whole row.

- **`%TYPE`**: Declares a variable to have the same data type as a
  specific table column.
- **`%ROWTYPE`**: Declares a record variable that can hold an entire
  row from a table or view.

**Example: %TYPE and %ROWTYPE**

```sql
SET SERVEROUTPUT ON;

DECLARE
    -- %TYPE: Variable inherits the data type of the MOBILE_NO column
    v_mobile_num  CUSTOMER.MOBILE_NO%TYPE;

    -- %ROWTYPE: Record variable that can hold a full CUSTOMER row
    v_customer_rec CUSTOMER%ROWTYPE;
BEGIN
    -- Using %TYPE
    SELECT MOBILE_NO INTO v_mobile_num
    FROM CUSTOMER
    WHERE CUST_ID = 2;

    DBMS_OUTPUT.PUT_LINE('Mobile Number: ' || v_mobile_num);

    -- Using %ROWTYPE
    SELECT * INTO v_customer_rec
    FROM CUSTOMER
    WHERE CUST_ID = 3;

    DBMS_OUTPUT.PUT_LINE('Customer Name: ' || v_customer_rec.CUST_NAME);
    DBMS_OUTPUT.PUT_LINE('Customer City: ' || v_customer_rec.CITY);
END;
/
```

---

## 3. Conditional and Iterative Control

### 3.1 IF Statement

The `IF` statement allows you to execute code based on conditions.
Oracle supports `IF-THEN`, `IF-THEN-ELSE`, and `IF-THEN-ELSIF`
(note the spelling, not `ELSEIF`).

**Syntax:**

```sql
IF condition1 THEN
    statements;
ELSIF condition2 THEN
    statements;
ELSE
    statements;
END IF;
```

**Example: IF-THEN-ELSIF**

```sql
SET SERVEROUTPUT ON;

DECLARE
    a NUMBER := 10;
    b NUMBER := 20;
BEGIN
    IF a > b THEN
        DBMS_OUTPUT.PUT_LINE('a is greater than b');
    ELSIF a < b THEN
        DBMS_OUTPUT.PUT_LINE('a is less than b');
    ELSE
        DBMS_OUTPUT.PUT_LINE('a equals b');
    END IF;
END;
/
```

**Expected Output:**

```
a is less than b
```

### 3.2 CASE Statement

The `CASE` statement is an alternative to `IF-THEN-ELSIF` that can be
more readable, especially with many equality checks.

**Syntax:**

```sql
CASE
    WHEN condition1 THEN
        statements;
    WHEN condition2 THEN
        statements;
    ELSE
        statements;
END CASE;
```

**Example: CASE Statement**

```sql
SET SERVEROUTPUT ON;

DECLARE
    v_grade CHAR(1) := 'B';
    v_comment VARCHAR2(20);
BEGIN
    CASE v_grade
        WHEN 'A' THEN v_comment := 'Excellent';
        WHEN 'B' THEN v_comment := 'Very Good';
        WHEN 'C' THEN v_comment := 'Good';
        ELSE v_comment := 'Needs Improvement';
    END CASE;

    DBMS_OUTPUT.PUT_LINE('Comment: ' || v_comment);
END;
/
```

**Expected Output:**

```
Comment: Very Good
```

### 3.3 Loops: Basic LOOP, WHILE, FOR, Nested

PL/SQL provides three main types of loops.

**1. Basic LOOP (Infinite Loop)**
Executes at least once. You must include an `EXIT` or `EXIT WHEN`
condition to avoid an infinite loop.

```sql
SET SERVEROUTPUT ON;

DECLARE
    counter NUMBER := 1;
BEGIN
    LOOP
        DBMS_OUTPUT.PUT_LINE('Counter = ' || counter);
        counter := counter + 1;
        EXIT WHEN counter > 5;
    END LOOP;
END;
/
```

**2. WHILE LOOP**
Checks the condition before each iteration. If the condition is false
initially, the loop body never executes.

```sql
SET SERVEROUTPUT ON;

DECLARE
    counter NUMBER := 1;
BEGIN
    WHILE counter <= 5
    LOOP
        DBMS_OUTPUT.PUT_LINE('Counter = ' || counter);
        counter := counter + 1;
    END LOOP;
END;
/
```

**3. FOR LOOP**
Iterates a specific number of times. The loop index is automatically
declared and incremented. Use `REVERSE` to count backward.

```sql
SET SERVEROUTPUT ON;

BEGIN
    -- Forward loop
    FOR i IN 1..5
    LOOP
        DBMS_OUTPUT.PUT_LINE('Forward: i = ' || i);
    END LOOP;

    -- Reverse loop
    FOR i IN REVERSE 1..5
    LOOP
        DBMS_OUTPUT.PUT_LINE('Reverse: i = ' || i);
    END LOOP;
END;
/
```

**4. CONTINUE Statement**
Skips the current iteration and moves to the next.

```sql
SET SERVEROUTPUT ON;

BEGIN
    FOR i IN 1..5
    LOOP
        CONTINUE WHEN i = 3;  -- Skip when i is 3
        DBMS_OUTPUT.PUT_LINE('i = ' || i);
    END LOOP;
END;
/
```

**Expected Output:**

```
i = 1
i = 2
i = 4
i = 5
```

---

## 4. Cursors

### 4.1 What is a Cursor?

A cursor is a private memory area (work area) that Oracle uses to
execute SQL statements and process the results. It is essential for
handling queries that return multiple rows.

- **Implicit Cursor**: Automatically created by Oracle for all DML
  and single-row `SELECT` statements.
- **Explicit Cursor**: Defined by the programmer to handle queries
  that return multiple rows.

### 4.2 Implicit Cursors

For every `INSERT`, `UPDATE`, `DELETE`, or `SELECT ... INTO`
statement, Oracle creates an implicit cursor named `SQL`. You can use
its attributes (`%FOUND`, `%NOTFOUND`, `%ROWCOUNT`, `%ISOPEN`) to get
information about the operation.

**Example: Using Implicit Cursor Attributes**

```sql
SET SERVEROUTPUT ON;

BEGIN
    UPDATE CUSTOMER
    SET MOBILE_NO = '+1-' || MOBILE_NO
    WHERE CUST_ID = 99;  -- Assume this ID does not exist

    IF SQL%NOTFOUND THEN
        DBMS_OUTPUT.PUT_LINE('Note: No records found to update.');
    ELSE
        DBMS_OUTPUT.PUT_LINE('Note: ' || SQL%ROWCOUNT ||
                             ' rows updated.');
    END IF;
END;
/
```

**Expected Output:**

```
Note: No records found to update.
```

### 4.3 Explicit Cursors

Explicit cursors give you fine-grained control over multi-row queries.
Use these four steps:

1.  **DECLARE**: Define the cursor with a `SELECT` statement.
2.  **OPEN**: Execute the query and populate the work area.
3.  **FETCH**: Retrieve one row at a time into local variables.
4.  **CLOSE**: Release the work area.

**Example: Basic Explicit Cursor**

```sql
SET SERVEROUTPUT ON;

DECLARE
    v_emp_first_name EMPLOYEE.FIRST_NAME%TYPE;
    v_emp_salary     EMPLOYEE.SALARY%TYPE;

    CURSOR cur_1 IS
        SELECT FIRST_NAME, SALARY
        FROM EMPLOYEE;
BEGIN
    OPEN cur_1;

    LOOP
        FETCH cur_1 INTO v_emp_first_name, v_emp_salary;
        EXIT WHEN cur_1%NOTFOUND;  -- Exit when no more rows

        DBMS_OUTPUT.PUT_LINE('Name: ' || v_emp_first_name ||
                             ', Salary: ' || v_emp_salary);
    END LOOP;

    CLOSE cur_1;
END;
/
```

### 4.4 Cursor FOR Loops

The `CURSOR FOR LOOP` is the simplest and most efficient way to
process all rows of a query. It implicitly opens, fetches, and closes
the cursor. You do not need to declare a fetch variable; the loop
record (`rec` in the example) acts as a `%ROWTYPE` variable.

**Example: Cursor FOR Loop**

```sql
SET SERVEROUTPUT ON;

DECLARE
    CURSOR cl IS
        SELECT FIRST_NAME, SALARY
        FROM EMPLOYEE;
BEGIN
    FOR rec IN cl
    LOOP
        DBMS_OUTPUT.PUT_LINE('First Name: ' || rec.FIRST_NAME);
        DBMS_OUTPUT.PUT_LINE('Salary: ' || rec.SALARY);
    END LOOP;
END;
/
```

### 4.5 Cursors with Parameters

You can pass parameters to a cursor to make it dynamic and reusable.
This is more efficient than declaring multiple separate cursors.

**Example: Cursor with Parameter**

```sql
SET SERVEROUTPUT ON;

DECLARE
    CURSOR c1 (p_dept_num NUMBER) IS
        SELECT FIRST_NAME, SALARY
        FROM EMPLOYEE
        WHERE DEPARTMENT_ID = p_dept_num;
BEGIN
    DBMS_OUTPUT.PUT_LINE('--- Department 30 ---');
    FOR rec IN c1(30)
    LOOP
        DBMS_OUTPUT.PUT_LINE('Name: ' || rec.FIRST_NAME ||
                             ', Salary: ' || rec.SALARY);
    END LOOP;

    DBMS_OUTPUT.PUT_LINE('--- Department 60 ---');
    FOR rec IN c1(60)
    LOOP
        DBMS_OUTPUT.PUT_LINE('Name: ' || rec.FIRST_NAME ||
                             ', Salary: ' || rec.SALARY);
    END LOOP;
END;
/
```

### 4.6 REF Cursors (Dynamic Cursors)

A `REF CURSOR` is a pointer or a handle to a result set. It is a data
type that allows you to associate a different query with the same
cursor variable at runtime. This is useful for building dynamic
queries or passing result sets between subprograms.

- **Strongly Typed REF CURSOR**: Uses a `RETURN` clause, defining the
  exact record structure it will return.
- **Weakly Typed REF CURSOR**: No `RETURN` clause. Can be used with
  any query, offering maximum flexibility.

**Example: Weakly Typed REF CURSOR**

```sql
SET SERVEROUTPUT ON;

DECLARE
    -- Define a weak REF CURSOR type
    TYPE t_refcur IS REF CURSOR;
    l_cursor t_refcur;

    l_emp_id   EMPLOYEE.EMPLOYEE_ID%TYPE;
    l_name     EMPLOYEE.FIRST_NAME%TYPE;
BEGIN
    -- Dynamically open the cursor for a specific query
    OPEN l_cursor FOR
        SELECT EMPLOYEE_ID, FIRST_NAME
        FROM EMPLOYEE
        WHERE DEPARTMENT_ID = 10;

    LOOP
        FETCH l_cursor INTO l_emp_id, l_name;
        EXIT WHEN l_cursor%NOTFOUND;
        DBMS_OUTPUT.PUT_LINE('ID: ' || l_emp_id || ', Name: ' ||
                             l_name);
    END LOOP;

    CLOSE l_cursor;
END;
/
```

### 4.7 Cursor Attributes

These attributes return information about the state of a cursor.

| Attribute    | Description                                            |
|--------------|--------------------------------------------------------|
| `%FOUND`     | Returns `TRUE` if the last `FETCH` returned a row,    |
|              | `NULL` before the first fetch, `FALSE` otherwise.     |
| `%NOTFOUND`  | Returns `TRUE` if the last `FETCH` failed to return   |
|              | a row, `NULL` before the first fetch.                 |
| `%ISOPEN`    | Returns `TRUE` if the cursor is open.                 |
| `%ROWCOUNT`  | Returns the total number of rows fetched so far.      |

---

## 5. Stored Procedures

### 5.1 Creating and Executing Procedures

A **stored procedure** is a named PL/SQL block that performs one or
more specific actions. It is stored in the database, compiled once,
and can be executed many times. Procedures may or may not return
values (through `OUT` parameters).

**Syntax:**

```sql
CREATE [OR REPLACE] PROCEDURE procedure_name
    [ (parameter_list) ]
AS | IS
    -- Declaration section (variables, cursors, etc.)
BEGIN
    -- Execution section (main logic)
EXCEPTION
    -- Exception handling section (optional)
END [procedure_name];
/
```

**Example: A Simple Procedure**

```sql
CREATE OR REPLACE PROCEDURE Greetings
AS
BEGIN
    DBMS_OUTPUT.PUT_LINE('Hello. Welcome to the PL/SQL session.');
END Greetings;
/

-- Execute the procedure
EXECUTE Greetings;
```

### 5.2 Procedure Parameters: IN, OUT, IN OUT

Parameters define the data passed to and from a procedure.

- `IN` (Default): A read-only value passed into the procedure.
- `OUT`: A write-only value returned from the procedure. The calling
  program provides a variable to hold the result.
- `IN OUT`: A read-write parameter. The procedure can read the
  incoming value and modify it before returning.

**Example: Procedure with IN and OUT Parameters**

```sql
CREATE OR REPLACE PROCEDURE Add_Numbers (
    p_num1 IN NUMBER,
    p_num2 IN NUMBER,
    p_result OUT NUMBER
)
AS
BEGIN
    p_result := p_num1 + p_num2;
END Add_Numbers;
/

-- Calling the procedure from an anonymous block
DECLARE
    v_sum NUMBER;
BEGIN
    Add_Numbers(10, 20, v_sum);
    DBMS_OUTPUT.PUT_LINE('The sum is: ' || v_sum);
END;
/
```

### 5.3 Examples of Stored Procedures

**Example: Procedure with IN Parameter and Cursor**

```sql
CREATE OR REPLACE PROCEDURE Get_Employees_By_Dept (
    p_department_id IN EMPLOYEE.DEPARTMENT_ID%TYPE
)
IS
    CURSOR cur_emp IS
        SELECT FIRST_NAME, SALARY
        FROM EMPLOYEE
        WHERE DEPARTMENT_ID = p_department_id;
BEGIN
    DBMS_OUTPUT.PUT_LINE('Employees in Department ' ||
                         p_department_id);
    DBMS_OUTPUT.PUT_LINE('==================================');

    FOR rec IN cur_emp
    LOOP
        DBMS_OUTPUT.PUT_LINE('Name: ' || rec.FIRST_NAME ||
                             ', Salary: ' || rec.SALARY);
    END LOOP;
END Get_Employees_By_Dept;
/

-- Execute
EXECUTE Get_Employees_By_Dept(30);
```

---

## 6. Functions

### 6.1 Creating and Executing Functions

A **function** is a named PL/SQL block that computes and **must
return a single value**. It is used for computations and can be
called in SQL statements (unlike procedures).

**Key Differences from Procedures:**

| Feature           | Procedure                               | Function                                  |
|-------------------|-----------------------------------------|-------------------------------------------|
| Return Value      | May return 0 or many values (via OUT).  | Must return exactly one value (via RETURN). |
| Call in SQL       | Cannot be called in a SQL statement.    | Can be called in a SQL statement.          |
| Main Purpose      | Performs an action.                     | Computes and returns a value.               |
| `RETURN` Keyword  | Exits the procedure early.              | Returns the function's result and exits.    |

**Syntax:**

```sql
CREATE [OR REPLACE] FUNCTION function_name
    [ (parameter_list) ]
    RETURN return_data_type
AS | IS
    -- Declaration section
BEGIN
    -- Execution section
    RETURN return_value;
EXCEPTION
    -- Exception handling section
END [function_name];
/
```

**Example: A Simple Function**

```sql
CREATE OR REPLACE FUNCTION Get_Employee_Count
    RETURN NUMBER
IS
    v_count NUMBER;
BEGIN
    SELECT COUNT(*) INTO v_count FROM EMPLOYEE;
    RETURN v_count;
END Get_Employee_Count;
/

-- Call the function from a SQL statement
SELECT Get_Employee_Count() FROM DUAL;
```

### 6.2 Function Examples

**Example: Function with IN Parameter (Salary Hike)**

```sql
CREATE OR REPLACE FUNCTION Calculate_Salary_Hike (
    p_emp_id IN EMPLOYEE.EMPLOYEE_ID%TYPE
)
RETURN NUMBER
IS
    v_current_salary EMPLOYEE.SALARY%TYPE;
    v_hike NUMBER(3,2) := 0;
    v_new_salary NUMBER;
BEGIN
    SELECT SALARY INTO v_current_salary
    FROM EMPLOYEE
    WHERE EMPLOYEE_ID = p_emp_id;

    -- Business logic: 10% raise if salary < 50000, else 5%
    IF v_current_salary < 50000 THEN
        v_hike := 0.10;
    ELSE
        v_hike := 0.05;
    END IF;

    v_new_salary := v_current_salary + (v_current_salary * v_hike);
    RETURN ROUND(v_new_salary, 2);
END Calculate_Salary_Hike;
/

-- Use the function in a query
SELECT
    EMPLOYEE_ID,
    FIRST_NAME,
    SALARY,
    Calculate_Salary_Hike(EMPLOYEE_ID) AS "Salary_After_Hike"
FROM EMPLOYEE;
```

### 6.3 Function Overloading

Function overloading allows you to create multiple functions with the
same name but different parameters (number, data type, or order).
This is only possible within a **package** (see Section 7).

**Example: Overloading in a Package (Conceptual)**

```sql
CREATE OR REPLACE PACKAGE Math_Pkg IS
    FUNCTION Add_Numbers (a NUMBER, b NUMBER) RETURN NUMBER;
    FUNCTION Add_Numbers (a NUMBER, b NUMBER, c NUMBER)
        RETURN NUMBER;
END Math_Pkg;
/
```

---

## 7. Packages

### 7.1 Package Specification and Body

A **package** is a container that groups related procedures,
functions, variables, constants, cursors, and exceptions. It has two
mandatory parts:

1.  **Package Specification (Public Interface)**: Declares the objects
    that are visible and callable from outside the package.
2.  **Package Body (Private Implementation)**: Contains the actual
    code for the procedures and functions declared in the
    specification. It can also contain private (hidden) objects.

**Basic Structure Example:**

```sql
-- Package Specification
CREATE OR REPLACE PACKAGE Employee_Pkg IS
    -- Public variable
    g_company_name CONSTANT VARCHAR2(50) := 'ABC Corp';

    -- Public procedure and function declarations
    PROCEDURE Hire_Employee (p_name VARCHAR2, p_salary NUMBER);
    FUNCTION Get_Employee_Salary (p_id NUMBER) RETURN NUMBER;
END Employee_Pkg;
/

-- Package Body
CREATE OR REPLACE PACKAGE BODY Employee_Pkg IS
    -- Private variable (only visible inside the body)
    v_internal_counter NUMBER := 0;

    -- Implementation of public procedure
    PROCEDURE Hire_Employee (p_name VARCHAR2, p_salary NUMBER) IS
    BEGIN
        INSERT INTO EMPLOYEE (EMPLOYEE_ID, FIRST_NAME, SALARY)
        VALUES (emp_seq.NEXTVAL, p_name, p_salary);
        COMMIT;
    END Hire_Employee;

    -- Implementation of public function
    FUNCTION Get_Employee_Salary (p_id NUMBER) RETURN NUMBER IS
        v_sal EMPLOYEE.SALARY%TYPE;
    BEGIN
        SELECT SALARY INTO v_sal
        FROM EMPLOYEE
        WHERE EMPLOYEE_ID = p_id;
        RETURN v_sal;
    END Get_Employee_Salary;

BEGIN
    -- Package initialization block
    -- (executes once when package is first referenced)
    DBMS_OUTPUT.PUT_LINE('Employee Package initialized.');
END Employee_Pkg;
/

-- Calling a public procedure from the package
EXECUTE Employee_Pkg.Hire_Employee('Jane Doe', 60000);
```

### 7.2 Public and Private Constructs

- **Public**: Declared in the specification. Accessible anywhere via
  `package_name.object_name`.
- **Private**: Declared only in the body. Not accessible outside the
  package. They help enforce encapsulation.

### 7.3 Advantages of Packages

- **Modularity**: Logically groups related functionality.
- **Encapsulation**: Hides implementation details.
- **Performance**: When any object in a package is called, the entire
  package is loaded into memory. Subsequent calls to other objects in
  the same package incur no additional disk I/O.
- **Overloading**: Enables overloading of subprogram names.
- **Initialization**: The optional initialization block (at the bottom
  of the body) runs once per session, perfect for setting up global
  state.

---

## 8. Exception Handling

### 8.1 Predefined Exceptions

Oracle provides many predefined exceptions for common errors. Some of
the most common include:

| Exception Name          | SQLCODE | Description                                    |
|-------------------------|---------|------------------------------------------------|
| `NO_DATA_FOUND`         | +100    | A `SELECT INTO` returned no rows.              |
| `TOO_MANY_ROWS`         | -1422   | A `SELECT INTO` returned more than one row.    |
| `DUP_VAL_ON_INDEX`      | -1      | Duplicate value in a unique index.             |
| `ZERO_DIVIDE`           | -1476   | Attempted to divide a number by zero.          |
| `INVALID_CURSOR`        | -1001   | Operation on an invalid cursor.                |
| `OTHERS`                |         | Handles any exception not explicitly named.    |

**Example: Handling Predefined Exceptions**

```sql
SET SERVEROUTPUT ON;

DECLARE
    v_salary EMPLOYEE.SALARY%TYPE;
BEGIN
    SELECT SALARY INTO v_salary
    FROM EMPLOYEE
    WHERE EMPLOYEE_ID = 999;  -- This ID does not exist

    DBMS_OUTPUT.PUT_LINE('Salary: ' || v_salary);
EXCEPTION
    WHEN NO_DATA_FOUND THEN
        DBMS_OUTPUT.PUT_LINE('Error: Employee not found.');
    WHEN TOO_MANY_ROWS THEN
        DBMS_OUTPUT.PUT_LINE('Error: Multiple employees found.');
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Unexpected error: ' || SQLERRM);
END;
/
```

### 8.2 User-Defined Exceptions

You can declare and raise your own exceptions for business rule
violations.

**Example: User-Defined Exception**

```sql
SET SERVEROUTPUT ON;

DECLARE
    v_salary EMPLOYEE.SALARY%TYPE;
    e_high_salary EXCEPTION;  -- Declare user-defined exception
    PRAGMA EXCEPTION_INIT(e_high_salary, -20001);
BEGIN
    SELECT SALARY INTO v_salary FROM EMPLOYEE WHERE EMPLOYEE_ID = 101;

    IF v_salary > 50000 THEN
        RAISE e_high_salary;  -- Raise the exception
    END IF;

    DBMS_OUTPUT.PUT_LINE('Salary is acceptable.');
EXCEPTION
    WHEN e_high_salary THEN
        DBMS_OUTPUT.PUT_LINE('Error: Salary exceeds maximum limit.');
END;
/
```

### 8.3 SQLCODE and SQLERRM

- `SQLCODE`: Returns the numeric code of the last encountered exception.
- `SQLERRM`: Returns the error message associated with `SQLCODE`.

**Example: Using SQLCODE and SQLERRM**

```sql
EXCEPTION
    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('Error Code: ' || SQLCODE);
        DBMS_OUTPUT.PUT_LINE('Error Message: ' || SQLERRM);
END;
```

### 8.4 RAISE_APPLICATION_ERROR

`RAISE_APPLICATION_ERROR` returns a user-defined error message to the
calling application. The error number must be between -20000 and
-20999.

**Syntax:** `RAISE_APPLICATION_ERROR(error_number, error_message);`

**Example: RAISE_APPLICATION_ERROR**

```sql
CREATE OR REPLACE PROCEDURE Update_Salary (
    p_emp_id NUMBER,
    p_new_salary NUMBER
) AS
BEGIN
    IF p_new_salary < 0 THEN
        RAISE_APPLICATION_ERROR(-20001, 'Salary cannot be negative.');
    ELSIF p_new_salary > 100000 THEN
        RAISE_APPLICATION_ERROR(-20002,
            'Salary exceeds management approval limit.');
    ELSE
        UPDATE EMPLOYEE SET SALARY = p_new_salary
        WHERE EMPLOYEE_ID = p_emp_id;
        COMMIT;
    END IF;
END Update_Salary;
/
```

---

## 9. Triggers

### 9.1 DML Triggers (BEFORE/AFTER, ROW/STATEMENT)

A trigger is a stored procedure that automatically fires (executes)
when a specific event (INSERT, UPDATE, DELETE) occurs on a table or
view.

**Types of DML Triggers:**

- **Timing**: `BEFORE` (action before DML) or `AFTER`
  (action after DML).
- **Level**: `FOR EACH ROW` (row-level trigger, fires once per row
  modified) or `FOR EACH STATEMENT` (statement-level trigger, fires
  once for the entire DML statement, which is the default).

**Syntax:**

```sql
CREATE [OR REPLACE] TRIGGER trigger_name
    {BEFORE | AFTER}
    {INSERT | UPDATE | DELETE} [OF column]
    ON table_name
    [FOR EACH ROW]
    [WHEN (condition)]
DECLARE
    -- Declaration section
BEGIN
    -- Trigger body (PL/SQL code)
END trigger_name;
/
```

**Example: Row-Level BEFORE DELETE Trigger (Audit Log)**

This trigger logs every deleted row into a backup table, including
who deleted it and when.

```sql
-- Create backup table for deleted rows
CREATE TABLE CUSTOMER_BKP_TRIG AS SELECT * FROM CUSTOMER WHERE 1 = 0;

ALTER TABLE CUSTOMER_BKP_TRIG ADD (DATE_OF_DELETION DATE);
ALTER TABLE CUSTOMER_BKP_TRIG ADD (WHO_DELETED VARCHAR2(30));

-- Create the trigger
CREATE OR REPLACE TRIGGER Customer_Delete_Trigger
    BEFORE DELETE ON CUSTOMER
    FOR EACH ROW
BEGIN
    INSERT INTO CUSTOMER_BKP_TRIG (
        CUST_ID, CUST_NAME, MOBILE_NO, AGE, CITY, CITY_ID,
        DATE_OF_DELETION, WHO_DELETED
    )
    VALUES (
        :OLD.CUST_ID, :OLD.CUST_NAME, :OLD.MOBILE_NO, :OLD.AGE,
        :OLD.CITY, :OLD.CITY_ID, SYSDATE, USER
    );
END Customer_Delete_Trigger;
/

-- Now any DELETE on the CUSTOMER table will be logged
DELETE FROM CUSTOMER WHERE CUST_ID = 2;
```

### 9.2 :OLD and :NEW Qualifiers

These qualifiers allow you to access the column values of the row
being processed.

- **`INSERT`**: Only `:NEW` exists.
- **`UPDATE`**: Both `:OLD` (value before update) and `:NEW`
  (value after update) exist.
- **`DELETE`**: Only `:OLD` exists.

**Example: Trigger to Enforce Business Rule on UPDATE**

```sql
CREATE OR REPLACE TRIGGER Salary_Update_Check
    BEFORE UPDATE OF SALARY ON EMPLOYEE
    FOR EACH ROW
BEGIN
    -- Do not allow a salary decrease of more than 10%
    IF :NEW.SALARY < :OLD.SALARY * 0.9 THEN
        RAISE_APPLICATION_ERROR(-20010,
            'Salary cannot be reduced by more than 10%. Old: ' ||
            :OLD.SALARY || ', New: ' || :NEW.SALARY);
    END IF;
END Salary_Update_Check;
/
```

### 9.3 INSTEAD OF Triggers for Views

`INSTEAD OF` triggers are used on **non-updatable views**
(e.g., views based on multiple tables). They tell the database
*what to do* instead of the default (which would be to reject the
DML operation).

**Example: INSTEAD OF INSERT on a Complex View**

```sql
CREATE OR REPLACE VIEW Emp_Dept_View AS
SELECT e.EMPLOYEE_ID, e.FIRST_NAME, e.SALARY,
       d.DEPARTMENT_ID, d.DEPARTMENT_NAME
FROM EMPLOYEE e, DEPARTMENTS d
WHERE e.DEPARTMENT_ID = d.DEPARTMENT_ID;

-- This trigger will handle inserts into the view
CREATE OR REPLACE TRIGGER Insert_Emp_Dept_View
    INSTEAD OF INSERT ON Emp_Dept_View
    FOR EACH ROW
DECLARE
    v_dept_id DEPARTMENTS.DEPARTMENT_ID%TYPE;
BEGIN
    -- Check if the department exists; if not, insert it first
    SELECT COUNT(*) INTO v_dept_id
    FROM DEPARTMENTS
    WHERE DEPARTMENT_ID = :NEW.DEPARTMENT_ID;

    IF v_dept_id = 0 THEN
        INSERT INTO DEPARTMENTS (DEPARTMENT_ID, DEPARTMENT_NAME)
        VALUES (:NEW.DEPARTMENT_ID, :NEW.DEPARTMENT_NAME);
    END IF;

    -- Insert the employee record
    INSERT INTO EMPLOYEE (EMPLOYEE_ID, FIRST_NAME, SALARY,
                          DEPARTMENT_ID)
    VALUES (:NEW.EMPLOYEE_ID, :NEW.FIRST_NAME, :NEW.SALARY,
            :NEW.DEPARTMENT_ID);
END Insert_Emp_Dept_View;
/
```

### 9.4 DDL and System Triggers

DDL triggers fire on DDL events (`CREATE`, `ALTER`, `DROP`,
`TRUNCATE`). System triggers fire on database events like `LOGON`,
`LOGOFF`, `STARTUP`, `SHUTDOWN`.

**Example: Logging DDL Operations on a Schema**

```sql
-- Create log table
CREATE TABLE DDL_EVENT_LOG (
    EVENT_DATE DATE,
    USERNAME VARCHAR2(30),
    DDL_EVENT VARCHAR2(30),
    OBJECT_NAME VARCHAR2(30),
    OBJECT_TYPE VARCHAR2(30)
);

-- Create the trigger on the SCHEMA
CREATE OR REPLACE TRIGGER Log_DDL_Changes
    AFTER DDL ON SCHEMA
BEGIN
    INSERT INTO DDL_EVENT_LOG
    VALUES (
        SYSDATE,
        ORA_LOGIN_USER,
        ORA_SYSEVENT,
        ORA_DICT_OBJ_NAME,
        ORA_DICT_OBJ_TYPE
    );
END Log_DDL_Changes;
/
```

### 9.5 The Mutating Table Error and How to Fix It

The **mutating table error** (`ORA-04091: table is mutating`) occurs
when a row-level trigger reads or modifies the same table on which it
is defined. This is because the table is in an inconsistent, changing
state.

**Solution**: Use an **Autonomous Transaction** (covered in the next
section) to separate the problematic operation. This makes the
trigger's action independent of the main transaction.

**Example: Fixing a Mutating Table Error**

```sql
CREATE OR REPLACE TRIGGER Fix_Mutating_Trigger
    AFTER INSERT ON CUSTOMER_2
    FOR EACH ROW
DECLARE
    PRAGMA AUTONOMOUS_TRANSACTION;
BEGIN
    UPDATE CUSTOMER_1
    SET CUSTOMER_ID = (SELECT MAX(CUSTOMER_ID) FROM CUSTOMER_2);
    COMMIT;  -- Required for autonomous transaction
END Fix_Mutating_Trigger;
/
```

---

## 10. Autonomous Transactions

### 10.1 Pragma AUTONOMOUS_TRANSACTION

An **autonomous transaction** is an independent transaction started
by another (parent) transaction. It does not depend on the parent.
The autonomous transaction must be committed or rolled back before
the parent resumes.

**Key Features:**

- It is a child transaction.
- It can `COMMIT` or `ROLLBACK` without affecting the parent
  transaction.
- The parent can also commit or roll back independently.

**Syntax:** `PRAGMA AUTONOMOUS_TRANSACTION;` placed in the declaration
section.

**Example: Autonomous Transaction Inside a Procedure**

```sql
CREATE OR REPLACE PROCEDURE Log_Error (
    p_error_msg VARCHAR2
) IS
    PRAGMA AUTONOMOUS_TRANSACTION;
BEGIN
    INSERT INTO ERROR_LOG (LOG_ID, LOG_DATE, ERROR_MESSAGE)
    VALUES (ERROR_SEQ.NEXTVAL, SYSDATE, p_error_msg);
    COMMIT;  -- This commit does not affect the calling transaction
END Log_Error;
/
```

### 10.2 Use Cases for Autonomous Transactions

- **Error Logging**: Log an error even if the main transaction rolls
  back.
- **Auditing**: Record an audit trail independently of the main
  business transaction.
- **Fixing Mutating Table Errors**: As shown in Section 9.5.
- **Calling DDL in Functions**: Since DDL performs auto-commit,
  isolate it in an autonomous transaction inside a function.

---

## 11. Collections and Records

Collections and records are composite data types, meaning they can
hold multiple pieces of data.

- **Record**: Holds a single row of data with fields of potentially
  different data types.
- **Collection**: Holds multiple elements (like an array) of the
  *same* data type.

### 11.1 PL/SQL Records (TYPE RECORD)

A record is used to treat related, but potentially different, data as
a single unit.

**Example: Declaring and Using a Record**

```sql
SET SERVEROUTPUT ON;

DECLARE
    -- Define the record type
    TYPE t_employee_rec IS RECORD (
        emp_name   EMPLOYEE.FIRST_NAME%TYPE,
        emp_salary EMPLOYEE.SALARY%TYPE
    );

    -- Declare a variable of that record type
    v_emp_rec t_employee_rec;
BEGIN
    SELECT FIRST_NAME, SALARY
    INTO v_emp_rec
    FROM EMPLOYEE
    WHERE EMPLOYEE_ID = 120;

    DBMS_OUTPUT.PUT_LINE('Name: ' || v_emp_rec.emp_name);
    DBMS_OUTPUT.PUT_LINE('Salary: ' || v_emp_rec.emp_salary);
END;
/
```

### 11.2 Associative Arrays (Index-By Tables)

Associative arrays are key-value pairs. The key (index) can be
integer or string. They are temporary and exist only in the PL/SQL
block.

**Example: Associative Array with Integer Index**

```sql
SET SERVEROUTPUT ON;

DECLARE
    TYPE t_salary_array IS TABLE OF EMPLOYEE.SALARY%TYPE
        INDEX BY PLS_INTEGER;  -- Index by integer

    v_salaries t_salary_array;
    v_idx PLS_INTEGER;
BEGIN
    -- Populate the array
    v_salaries(1) := 50000;
    v_salaries(2) := 60000;
    v_salaries(3) := 55000;

    -- Iterate and print
    v_idx := v_salaries.FIRST;
    WHILE v_idx IS NOT NULL
    LOOP
        DBMS_OUTPUT.PUT_LINE('Salary at index ' || v_idx ||
                             ': ' || v_salaries(v_idx));
        v_idx := v_salaries.NEXT(v_idx);
    END LOOP;
END;
/
```

### 11.3 Nested Tables

Nested tables are collections that can be stored in the database.
They do not have a predefined maximum size and are initially dense
but can become sparse if you delete elements.

### 11.4 VARRAYs (Variable-Size Arrays)

VARRAYs have a maximum size defined when the type is created. They
are always dense (no gaps in index numbers).

### 11.5 Collection Methods

These are built-in functions and procedures to manipulate collections.

| Method        | Type       | Description                                            |
|---------------|------------|--------------------------------------------------------|
| `COUNT`       | Function   | Returns the current number of elements.               |
| `EXISTS(index)`| Function   | Returns `TRUE` if the specified index exists.         |
| `FIRST`, `LAST`| Function   | Returns the first and last index value, respectively. |
| `PRIOR(index)`, `NEXT(index)` | Function | Returns the previous/next index value.|
| `LIMIT`       | Function   | Returns the maximum size of a VARRAY; `NULL` for nested tables. |
| `EXTEND`      | Procedure  | Appends one or more null elements.                    |
| `TRIM`        | Procedure  | Removes one or more elements from the end.            |
| `DELETE`      | Procedure  | Removes all elements, or an element at a specific index.|

---

## 12. Bulk Operations for Performance

### 12.1 BULK COLLECT for Fast Data Retrieval

`BULK COLLECT` fetches multiple rows from a query into a collection
in a single operation, drastically reducing context switches.

**Example: BULK COLLECT with SELECT**

```sql
SET SERVEROUTPUT ON;

DECLARE
    TYPE t_salary_table IS TABLE OF EMPLOYEE.SALARY%TYPE;
    v_salaries t_salary_table;
BEGIN
    -- Fetch all salaries in one go
    SELECT SALARY BULK COLLECT INTO v_salaries
    FROM EMPLOYEE;

    FOR i IN 1..v_salaries.COUNT
    LOOP
        DBMS_OUTPUT.PUT_LINE('Salary: ' || v_salaries(i));
    END LOOP;
END;
/
```

### 12.2 FORALL for Bulk DML Operations

`FORALL` performs the same DML operation (INSERT, UPDATE, DELETE) for
all elements in a collection, again minimizing context switches.

**Example: FORALL with INSERT**

```sql
DECLARE
    TYPE t_id_table IS TABLE OF EMPLOYEE.EMPLOYEE_ID%TYPE;
    TYPE t_name_table IS TABLE OF EMPLOYEE.FIRST_NAME%TYPE;

    v_ids   t_id_table := t_id_table(101, 102, 103);
    v_names t_name_table := t_name_table('New Emp A', 'New Emp B',
                                         'New Emp C');
BEGIN
    FORALL i IN v_ids.FIRST..v_ids.LAST
        INSERT INTO EMPLOYEE_TEMP (EMPLOYEE_ID, FIRST_NAME)
        VALUES (v_ids(i), v_names(i));

    COMMIT;
END;
/
```

### 12.3 LIMIT Clause for Managing Memory

When fetching a very large result set with `BULK COLLECT`, using
`LIMIT` fetches rows in batches, preventing out-of-memory errors.

**Example: LIMIT with a Cursor**

```sql
DECLARE
    TYPE t_emp_table IS TABLE OF EMPLOYEE%ROWTYPE;
    v_employees t_emp_table;

    CURSOR c_emp IS SELECT * FROM EMPLOYEE;
BEGIN
    OPEN c_emp;
    LOOP
        FETCH c_emp BULK COLLECT INTO v_employees LIMIT 100;
        EXIT WHEN v_employees.COUNT = 0;

        FOR i IN 1..v_employees.COUNT
        LOOP
            -- Process each row
            DBMS_OUTPUT.PUT_LINE(v_employees(i).FIRST_NAME);
        END LOOP;
    END LOOP;
    CLOSE c_emp;
END;
/
```

---

## 13. Dynamic SQL

Dynamic SQL allows you to build and execute SQL statements at
runtime, which is useful when you don't know the exact structure
(e.g., table names, column names, or WHERE clauses) at compile time.

### 13.1 EXECUTE IMMEDIATE for DDL and DML

`EXECUTE IMMEDIATE` is the simplest way to execute dynamic SQL. It is
ideal for DDL (CREATE, DROP, TRUNCATE) and single DML statements.

**Example: Dropping a Table Dynamically**

```sql
DECLARE
    v_table_name VARCHAR2(30) := 'CUSTOMER_OLD';
BEGIN
    EXECUTE IMMEDIATE 'DROP TABLE ' || v_table_name || ' PURGE';
EXCEPTION
    WHEN OTHERS THEN
        IF SQLCODE != -942 THEN  -- Ignore if table does not exist
            RAISE;
        END IF;
END;
/
```

### 13.2 Using Bind Variables with EXECUTE IMMEDIATE

Using **bind variables** (placeholders preceded by a colon,
e.g., `:salary`) in dynamic SQL is a best practice. It improves
performance by allowing Oracle to reuse SQL area and prevents
SQL injection.

**Example: Dynamic UPDATE with Bind Variables**

```sql
CREATE OR REPLACE PROCEDURE Dynamic_Update (
    p_table_name VARCHAR2,
    p_column_name VARCHAR2,
    p_new_value NUMBER,
    p_emp_id NUMBER
) IS
    v_sql VARCHAR2(1000);
BEGIN
    v_sql := 'UPDATE ' || p_table_name || ' SET ' ||
             p_column_name || ' = :1 WHERE EMPLOYEE_ID = :2';
    EXECUTE IMMEDIATE v_sql USING p_new_value, p_emp_id;
    COMMIT;
END Dynamic_Update;
/

-- Usage
EXECUTE Dynamic_Update('EMPLOYEE', 'SALARY', 75000, 101);
```

---

## 14. Global Temporary Tables

Global Temporary Tables (GTTs) store session-private data. Data
inserted into a GTT is visible only to the current session and is
automatically dropped at the end of the transaction or session.

**Example: Creating a Global Temporary Table**

```sql
-- Transaction-level GTT: Data is deleted after COMMIT
CREATE GLOBAL TEMPORARY TABLE Temp_Employee_Data (
    EMP_ID NUMBER,
    CALC_SALARY NUMBER
) ON COMMIT DELETE ROWS;

-- Session-level GTT: Data persists across commits but is deleted
-- at the end of the session
CREATE GLOBAL TEMPORARY TABLE Temp_Session_Data (
    USER_ID VARCHAR2(30),
    LOGIN_TIME DATE
) ON COMMIT PRESERVE ROWS;
```

---

## 15. Database Setup Scripts for Practice

Run the following scripts in your Oracle schema to create and
populate the tables used in the examples. This script creates tables
for `EMPLOYEES`, `DEPARTMENTS`, and `CUSTOMER`, and inserts a sample
of 12 to 15 rows into each.

**15.1 Create and Populate Employees Table**

```sql
-- =============================================================================
-- DROP EXISTING TABLES (CLEAN UP)
-- =============================================================================
BEGIN
    EXECUTE IMMEDIATE 'DROP TABLE DEPARTMENTS CASCADE CONSTRAINTS PURGE';
    EXCEPTION WHEN OTHERS THEN NULL;
END;
/

BEGIN
    EXECUTE IMMEDIATE 'DROP TABLE EMPLOYEES CASCADE CONSTRAINTS PURGE';
    EXCEPTION WHEN OTHERS THEN NULL;
END;
/

BEGIN
    EXECUTE IMMEDIATE 'DROP TABLE CUSTOMER PURGE';
    EXCEPTION WHEN OTHERS THEN NULL;
END;
/

-- =============================================================================
-- CREATE DEPARTMENTS TABLE (MASTER)
-- =============================================================================
CREATE TABLE DEPARTMENTS (
    DEPARTMENT_ID   NUMBER(4) PRIMARY KEY,
    DEPARTMENT_NAME VARCHAR2(30) NOT NULL,
    MANAGER_ID      NUMBER(6),
    LOCATION_ID     NUMBER(4)
);

INSERT INTO DEPARTMENTS (DEPARTMENT_ID, DEPARTMENT_NAME)
VALUES (10, 'Administration');
INSERT INTO DEPARTMENTS (DEPARTMENT_ID, DEPARTMENT_NAME)
VALUES (20, 'Marketing');
INSERT INTO DEPARTMENTS (DEPARTMENT_ID, DEPARTMENT_NAME)
VALUES (30, 'Purchasing');
INSERT INTO DEPARTMENTS (DEPARTMENT_ID, DEPARTMENT_NAME)
VALUES (40, 'Human Resources');
INSERT INTO DEPARTMENTS (DEPARTMENT_ID, DEPARTMENT_NAME)
VALUES (50, 'Shipping');
INSERT INTO DEPARTMENTS (DEPARTMENT_ID, DEPARTMENT_NAME)
VALUES (60, 'IT');
INSERT INTO DEPARTMENTS (DEPARTMENT_ID, DEPARTMENT_NAME)
VALUES (70, 'Public Relations');
INSERT INTO DEPARTMENTS (DEPARTMENT_ID, DEPARTMENT_NAME)
VALUES (80, 'Sales');
INSERT INTO DEPARTMENTS (DEPARTMENT_ID, DEPARTMENT_NAME)
VALUES (90, 'Executive');
INSERT INTO DEPARTMENTS (DEPARTMENT_ID, DEPARTMENT_NAME)
VALUES (100, 'Finance');
INSERT INTO DEPARTMENTS (DEPARTMENT_ID, DEPARTMENT_NAME)
VALUES (110, 'Accounting');
INSERT INTO DEPARTMENTS (DEPARTMENT_ID, DEPARTMENT_NAME)
VALUES (120, 'Treasury');
INSERT INTO DEPARTMENTS (DEPARTMENT_ID, DEPARTMENT_NAME)
VALUES (130, 'Corporate Tax');
INSERT INTO DEPARTMENTS (DEPARTMENT_ID, DEPARTMENT_NAME)
VALUES (140, 'Control And Credit');
COMMIT;

-- =============================================================================
-- CREATE EMPLOYEES TABLE
-- =============================================================================
CREATE TABLE EMPLOYEES (
    EMPLOYEE_ID      NUMBER(6) PRIMARY KEY,
    FIRST_NAME       VARCHAR2(20),
    LAST_NAME        VARCHAR2(25) NOT NULL,
    EMAIL            VARCHAR2(25) NOT NULL,
    PHONE_NUMBER     VARCHAR2(20),
    HIRE_DATE        DATE NOT NULL,
    JOB_ID           VARCHAR2(10) NOT NULL,
    SALARY           NUMBER(8,2),
    COMMISSION_PCT   NUMBER(2,2),
    MANAGER_ID       NUMBER(6),
    DEPARTMENT_ID    NUMBER(4),
    CONSTRAINT fk_emp_dept
        FOREIGN KEY (DEPARTMENT_ID) REFERENCES DEPARTMENTS(DEPARTMENT_ID)
);

BEGIN
    INSERT INTO EMPLOYEES VALUES
        (100, 'Steven', 'King', 'SKING', '515.123.4567',
         DATE '2003-06-17', 'AD_PRES', 24000, NULL, NULL, 90);
    INSERT INTO EMPLOYEES VALUES
        (101, 'Neena', 'Kochhar', 'NKOCHHAR', '515.123.4568',
         DATE '2005-09-21', 'AD_VP', 17000, NULL, 100, 90);
    INSERT INTO EMPLOYEES VALUES
        (102, 'Lex', 'De Haan', 'LDEHAAN', '515.123.4569',
         DATE '2001-01-13', 'AD_VP', 17000, NULL, 100, 90);
    INSERT INTO EMPLOYEES VALUES
        (103, 'Alexander', 'Hunold', 'AHUNOLD', '590.423.4567',
         DATE '2006-01-03', 'IT_PROG', 9000, NULL, 102, 60);
    INSERT INTO EMPLOYEES VALUES
        (104, 'Bruce', 'Ernst', 'BERNST', '590.423.4568',
         DATE '2007-05-21', 'IT_PROG', 6000, NULL, 103, 60);
    INSERT INTO EMPLOYEES VALUES
        (105, 'David', 'Austin', 'DAUSTIN', '590.423.4569',
         DATE '2005-06-25', 'IT_PROG', 4800, NULL, 103, 60);
    INSERT INTO EMPLOYEES VALUES
        (106, 'Valli', 'Pataballa', 'VPATABAL', '590.423.4560',
         DATE '2006-02-05', 'IT_PROG', 4800, NULL, 103, 60);
    INSERT INTO EMPLOYEES VALUES
        (107, 'Diana', 'Lorentz', 'DLORENTZ', '590.423.5567',
         DATE '2007-02-07', 'IT_PROG', 4200, NULL, 103, 60);
    INSERT INTO EMPLOYEES VALUES
        (108, 'Nancy', 'Greenberg', 'NGREENBE', '515.124.4569',
         DATE '2002-08-17', 'FI_MGR', 12008, NULL, 101, 100);
    INSERT INTO EMPLOYEES VALUES
        (109, 'Daniel', 'Faviet', 'DFAVIET', '515.124.4169',
         DATE '2002-08-16', 'FI_ACCOUNT', 9000, NULL, 108, 100);
    INSERT INTO EMPLOYEES VALUES
        (110, 'John', 'Chen', 'JCHEN', '515.124.4269',
         DATE '2005-09-28', 'FI_ACCOUNT', 8200, NULL, 108, 100);
    INSERT INTO EMPLOYEES VALUES
        (111, 'Ismael', 'Sciarra', 'ISCIARRA', '515.124.4369',
         DATE '2005-09-30', 'FI_ACCOUNT', 7700, NULL, 108, 100);
    INSERT INTO EMPLOYEES VALUES
        (112, 'Jose Manuel', 'Urman', 'JMURMAN', '515.124.4469',
         DATE '2006-03-07', 'FI_ACCOUNT', 7800, NULL, 108, 100);
    INSERT INTO EMPLOYEES VALUES
        (113, 'Luis', 'Popp', 'LPOPP', '515.124.4567',
         DATE '2007-12-07', 'FI_ACCOUNT', 6900, NULL, 108, 100);
    COMMIT;
END;
/

-- =============================================================================
-- CREATE CUSTOMER TABLE
-- =============================================================================
CREATE TABLE CUSTOMER (
    CUST_ID     NUMBER(10) PRIMARY KEY,
    CUST_NAME   VARCHAR2(100) NOT NULL,
    MOBILE_NO   VARCHAR2(15) NOT NULL,
    AGE         NUMBER(3),
    CITY        VARCHAR2(50),
    CITY_ID     NUMBER(10)
);

BEGIN
    INSERT INTO CUSTOMER VALUES
        (1, 'Ayesha Khan', '0711111111', 29, 'Johannesburg', 101);
    INSERT INTO CUSTOMER VALUES
        (2, 'Thabo Mokoena', '0722222222', 35, 'Pretoria', 102);
    INSERT INTO CUSTOMER VALUES
        (3, 'Priya Naidoo', '0733333333', 26, 'Durban', 103);
    INSERT INTO CUSTOMER VALUES
        (4, 'Michael Smith', '0744444444', 41, 'Cape Town', 104);
    INSERT INTO CUSTOMER VALUES
        (5, 'Naledi Dlamini', '0755555555', 32, 'Bloemfontein', 105);
    INSERT INTO CUSTOMER VALUES
        (6, 'John Smiths', '0760000007', 22, 'Braamfontein', 106);
    INSERT INTO CUSTOMER VALUES
        (7, 'Sarah Johnson', '0761111111', 28, 'Sandton', 107);
    INSERT INTO CUSTOMER VALUES
        (8, 'Chris Evans', '0762222222', 39, 'Midrand', 108);
    INSERT INTO CUSTOMER VALUES
        (9, 'Emma Watson', '0763333333', 27, 'Centurion', 109);
    INSERT INTO CUSTOMER VALUES
        (10, 'Liam Miller', '0764444444', 45, 'Roodepoort', 110);
    INSERT INTO CUSTOMER VALUES
        (11, 'Olivia Brown', '0765555555', 31, 'Benoni', 111);
    INSERT INTO CUSTOMER VALUES
        (12, 'Noah Taylor', '0766666666', 24, 'Boksburg', 112);
    INSERT INTO CUSTOMER VALUES
        (13, 'Mia Johnson', '0767777777', 38, 'Krugersdorp', 113);
    INSERT INTO CUSTOMER VALUES
        (14, 'James Wilson', '0768888888', 33, 'Randburg', 114);
    INSERT INTO CUSTOMER VALUES
        (15, 'Isabella Moore', '0769999999', 29, 'Rivonia', 115);
    COMMIT;
END;
/

-- Verify data
SELECT 'Employees Count: ' || COUNT(*) FROM EMPLOYEES
UNION ALL
SELECT 'Departments Count: ' || COUNT(*) FROM DEPARTMENTS
UNION ALL
SELECT 'Customer Count: ' || COUNT(*) FROM CUSTOMER;
```

**Expected Output for Count Verification:**

```
Employees Count: 14
Departments Count: 14
Customer Count: 15
```

---

## 16. Core PL/SQL Best Practices and Summary

1.  **Always use `%TYPE` and `%ROWTYPE`** instead of hardcoding data
    types. This makes your code adaptable to table changes.
2.  **Handle exceptions gracefully**. At a minimum, have an `OTHERS`
    handler to log errors.
3.  **Use `BULK COLLECT` and `FORALL`** for high-volume data
    processing to minimize context switching.
4.  **Use bind variables in dynamic SQL** to improve performance and
    security.
5.  **Avoid large, monolithic blocks**. Break code into procedures,
    functions, and packages for modularity and reusability.
6.  **Explicitly close cursors** when done with explicit cursors to
    free resources.
7.  **Use `COMMIT` and `ROLLBACK` judiciously**. Ensure transactions
    are complete before committing.
8.  **Use packages** to group related logic and hide implementation
    details.
9.  **Document your code** using comments, especially for complex
    business logic.
10. **Test your exception handlers** to ensure they cover both
    expected and unexpected errors.

---

*END OF DOCUMENT*

---
