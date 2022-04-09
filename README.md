# RBMS
Retail Business Management System

Using PL/SQL and JDBC to
Implement the Retail Business Management System

The honesty statement: “We have done this assignment completely on our own. We have not copied it, nor have we given our solution to anyone else. We understand that if we are involved in plagiarism or cheating we will have to sign an official form that we have cheated and that this form will be stored in our official university records. We also understand that we will receive a grade of 0 for the involved assignment and our grades will be reduced by one level (e.g., from A to A- or from B+ to B) for our first offense, and that we will receive a grade of “F” for the course for any additional offense of any kind.”
Priyanka Lalge, Shruti Shukla & Amit Kumar Srivastava

Introduction
The project 2 for DATA-504 requires us to write a couple of sequences and triggers along with a package containing some procedures and a function within it. Also, a jdbc code was written to give us a flavor of the application layer connectivity with the database. 
Purpose of this document
The aim of this document is to introduce the pl/sql objects and compilation steps related to the PL/SQL. In addition, a jdbc code is introduced to showcase how the application/business layer interacts with the database. 
PL/SQL
The details of each pl/sql object created are provided below. In addition, code and installation steps are provided for each object separately after the description of the objects involved.
Sequences:
seqpur#
This is the sequence object created for the attribute pur# of purchases table in order to automatically generate unique values when new records are inserted into the purchases table. The sequence starts with 100001 and increases by 1. This sequence is used when a new purchase has been made by a customer and add_purchase procedure is executed in the backend.
Usage: 
<sequence_name>.nextval
Example:
SQL> seqpur#.nextval

seqlog#
This is the sequence object created for the attribute log# of logs table in order to automatically generate unique values when new records are inserted into the logs table. The sequence starts with 1001 and increases by 1. This sequence is used when the triggers are executed in the backend.
Usage: 
<sequence_name>.nextval
Example: 
SQL> seqlog#.nextval
Triggers:
purchases_add
This trigger is created to update the qoh value in products table, visits_made and last_visit_date in customers table, and display relevant messages during the trigger execution, after a tuple is added to purchases table. 
After adding a tuple to the Purchases table, the trigger is generated and the qoh of the product is reduced by the quantity purchased. If the purchase causes qoh to be below qoh_threshold, then qoh is updated to a value equivalent to qoh_threshold + 10. Also, visits_made of the involved customer is increased by 1 if the purchase is made on a new date and last_visit_date is updated accordingly.
insert_customers
This trigger is created to insert an audit entry in the logs table. When a tuple is added to the customers table due to the first visit of a customer, the trigger is generated. The table_name attribute of the logs table is updated as “customers” signifying a change in the customers table, the operation attribute is updated as “insert” signifying the addition of a new customer and the tuple_pkey attribute is updated as “cid” of the newly inserted customer.
update_lastvisitdate_customers
This trigger is created to insert an audit entry in the logs table. A tuple is added to the logs table due to update of the last_visit_date attribute of the customers table. The table_name attribute of the logs table is updated as “customers” signifying a change in the customers table, the operation  attribute is updated as “update” signifying the recent visit date of a customer and the tuple_pkey attribute is updated as “cid” of the affected customer.
update_visitsmade_customers
This trigger is created to insert an audit entry in the logs table. A tuple is added to the logs table due to update of the visits_made attribute of the customers table. The table_name attribute of the logs table is updated as “customers” signifying a change in the customers table, the operation  attribute is updated as “update” signifying the updated number of visits made by a customer and the tuple_pkey attribute is updated as “cid” of the affected customer.
insert_purchases
This trigger is created to insert an audit entry in the logs table. A tuple is added to the logs table when a new purchase tuple has been added in the purchases table. The table_name attribute of the logs table is updated as “purchases” signifying a new purchase made by a customer, the operation attribute is updated as “insert” signifying the insertion of a new tuple and the tuple_pkey attribute is updated as “pur#” of the newly inserted purchase.
update_products
This trigger is created to insert an audit entry in the logs table. A tuple is added to the logs table when the qoh attribute has been updated with a new value in the products table. The table_name attribute of the logs table is updated as “products” signifying an update of the products table, the operation attribute is updated as “update” signifying an update in the products table and the tuple_pkey attribute is updated as “pid” of the affected product. 
 
Package:
RetailBusiness_Package
The package consists of two parts namely package specification and package body. In the specification we declare the cursors, procedures and functions that can be referenced from outside the package. The body contains the definitions for the objects mentioned in the specification. This package has been created with the below listed procedures and function to perform necessary activities according to the requirements document.
Procedures:
show_customers
This procedure has no input parameters and displays all the tuples from the customers table whenever it is called or executed from the package.
Usage:
execute <package_name>.<procedure_name>;
Example:
SQL> execute RetailBusiness_Package.show_customers;
show_employees
This procedure has no input parameters and displays all the tuples from the employees table whenever it is called or executed from the package.
Usage:
execute <package_name>.<procedure_name>;
Example:
SQL> execute RetailBusiness_Package.show_employees;
show_products
This procedure has no input parameters and displays all the tuples from the products table whenever it is called or executed from the package.
Usage:
execute <package_name>.<procedure_name>;
Example:
SQL> execute RetailBusiness_Package.show_products;
show_purchases
This procedure has no input parameters and displays all the tuples from the purchases table whenever it is called or executed from the package.
Usage:
execute <package_name>.<procedure_name>;
Example:
SQL> execute RetailBusiness_Package.show_purchases;
show_logs
This procedure has no input parameters and displays all the tuples from the logs table whenever it is called or executed from the package.
Usage:
execute <package_name>.<procedure_name>;
Example:
SQL> execute RetailBusiness_Package.show_logs;
purchases_made
Objective of this procedure is to return the name of the customer as well as every purchase (output pid, pur_date, qty, unit_price, and total) the customer has made. This procedure has to be called by providing the cid of the customer as the input parameter.
Usage:
execute <package_name>.<procedure_name>(<c_id>);
Example:
SQL> execute RetailBusiness_Package.purchases_made('c001');
add_customer
Objective of this procedure is to add a tuple into the customers table. This procedure has to be called by providing the cid, name and telephone# of the new customer as the input parameter. For the attributes visits_made and last_visit_date of any newly added customer, a default value of  1 and sysdate is considered respectively.
Usage:
execute <package_name>.<procedure_name>(<c_id>,<c_name>,<c_telephone#>);
Example:
SQL> execute RetailBusiness_Package.add_customer('c009','Mr. Meng','777-888-9900');
add_purchase
Objective of this procedure is to add a tuple into the purchases table. This procedure has to be called by providing the eid, pid, cid, qty and unit_price of the new purchase as the input parameter. The attribute pur_# is generated by the sequence. The attribute pur_date is considered by the sysdate. The attributes total and saving are calculated on the go by the procedure. 
Usage:
execute <package_name>.<procedure_name>(<e_id>, <p_id>, <c_id>, <pur_qty>, <pur_unit_price>);
Example:
SQL> execute RetailBusiness_Package.add_purchase('e01', 'p004', 'c007', 4, 0.6);
Function:
number_customers
Objective of this function is to report the number of customers who have purchased the product identified by the pid. This function has to be called by providing the pid of the product as the input parameter.
Usage:
declare
   result number;
begin
   -- Call the function
   result := package_name.function_name(<pid>);
   DBMS_OUTPUT.PUT_LINE(result);
End;/

OR

select package_name.function_name(<pid>) from dual;

Example:
SQL> select RetailBusiness_Package.number_customers('p009') from dual;
SQL> declare
  result number;
begin
  -- Call the function
  result := RetailBusiness_Package.number_customers('p009');
  DBMS_OUTPUT.PUT_LINE(result);
End;/


Compilation steps:
After the creation of the required tables below, the following steps should be performed: 
 
Employees(eid, name, telephone#, email)
Customers(cid, name, telephone#, visits_made, last_visit_date)
Products(pid, name, qoh, qoh_threshold, regular_price, discnt_rate)
Purchases(pur#, eid, pid, cid, pur_date, qty, unit_price, total, saving)
Logs(log#, user_name, operation, op_time, table_name, tuple_pkey)
 
Login to the database with a username/password in the sqlplus environment or any compatible sql client connected to the oracle database. 
Step 1: Compile sequences
For a clean compilation we first compile the sequences. In order to compile the sequences the below code snippet should be executed.
 
/* sequence for pur# */
CREATE SEQUENCE seqpur#
   INCREMENT BY 1
   START WITH 100001
   ORDER;
  
  
/* sequence for log# */  
CREATE SEQUENCE seqlog#
   INCREMENT BY 1
   START WITH 1001
   ORDER;
 
 
Step 2: Compile triggers
The second step involves the compilation of the triggers. The below set of codes should be executed in order to compile the necessary triggers as per the requirements document. 
 
 
SET SERVEROUTPUT ON
 
/* question 6: trigger for inserting a tuple into the Purchases table */
create or replace trigger purchases_add
after insert on purchases
for each row
declare
qty products.qoh%type;
threshold products.qoh_threshold%type;
today customers.last_visit_date%type;
qoh_updated products.qoh%type;
 
begin
   update products
       set qoh = qoh - :new.qty where pid = :new.pid;
 
   select last_visit_date into today from customers where cid = :new.cid;
   if (to_char(today, 'YYYY') = to_char(sysdate, 'YYYY') and to_char(today, 'MON') = to_char(sysdate, 'MON') and to_char(today, 'DD') = to_char(sysdate, 'DD'))
   then
       NULL;
   else
       update customers
           set visits_made = visits_made + 1 where cid = :new.cid;
       update customers
           set last_visit_date = sysdate where cid = :new.cid;
   end if;
   select qoh, qoh_threshold into qty, threshold from products where pid = :new.pid;
   if(qty < threshold) then
       dbms_output.put_line('The current qoh of the product is below the required threshold and new supply is required');
       update products
           set qoh = threshold + 10 where pid = :new.pid;
          
           select qoh into qoh_updated from products where pid= :new.pid;           
           dbms_output.put_line('The new value of qoh after supply is: ' || qoh_updated );
   end if;
end;
/
 
 
 
/* question 7.1: trigger for inserting a tuple into the Customers table */
create or replace trigger insert_customers
after insert on customers
for each row
begin
insert into logs values (seqlog#.nextval, user, 'insert', sysdate, 'customers', :new.cid);
end;
/
show errors
 
/* question 7.2 trigger for updating the last_visit_date attribute of the Customers table */
create or replace trigger update_lastvisitdate_customers
after update of last_visit_date on customers
for each row
begin
insert into logs values (seqlog#.nextval, user, 'update', sysdate, 'customers', :old.cid);
end;
/
show errors
 
/* question 7.3: trigger for updating the visits_made attribute of the Customers table */
create or replace trigger update_visitsmade_customers
after update of visits_made on customers
for each row
begin
insert into logs values (seqlog#.nextval, user, 'update', sysdate, 'customers', :old.cid);
end;
/
show errors
 
/* question 7.4: trigger for inserting a tuple into the Purchases table */
create or replace trigger insert_purchases
after insert on purchases
for each row
begin
insert into logs values (seqlog#.nextval, user, 'insert', sysdate, 'purchases', :new.pur#);
end;
/
show errors
 
/* question 7.5: trigger for updating the qoh attribute of the Products table */
create or replace trigger update_products
after update of qoh on products
for each row
begin
insert into logs values (seqlog#.nextval, user, 'update', sysdate, 'products', :old.pid);
end;
/
show errors
 
 
Step 3: Compile package
The third step involves the compilation of the package which includes the subprograms (procedures and functions). The below set of codes should be executed in order to compile the package as per the requirements document. 
 
 
SET SERVEROUTPUT ON
 
/* package name specification: RetailBusiness_Package */
CREATE OR REPLACE PACKAGE RetailBusiness_Package AS
 
type cursor1 is ref cursor;
 
/* question 2: procedures to show the tuples in each table */
procedure show_customers;
procedure show_employees;
procedure show_logs;
procedure show_products;
procedure show_purchases;
 
/* question 3: procedure to return the name of the customer as well as every purchase */
procedure purchases_made(c_id in customers.cid%type);
 
/* question 4: function to report the number of customers who have purchased the product identified by the pid */
function number_customers(cust_pid IN purchases.pid%type) return NUMBER;
 
/* question 5: procedure for adding tuples to the Customers table.*/
procedure add_customer(c_id in customers.cid%type, c_name in customers.name%type, c_telephone# in customers.telephone#%type);
 
/* question 6: procedure for adding tuples to the Purchases table.*/
procedure add_purchase(e_id in purchases.eid%type, p_id in purchases.pid%type, c_id in purchases.cid%type, pur_qty in purchases.qty%type, pur_unit_price in purchases.unit_price%type);
 
end;
/
 
 
/* package body */
create or replace package body RetailBusiness_Package as
 
/* question 2: procedure to show the tuples in employees */
PROCEDURE show_employees
AS
   CURSOR c_emp IS
   SELECT * FROM employees;
   c_emp_rec c_emp%rowtype;
 
BEGIN
dbms_output.put_line( 'EID' || ',' || 'NAME' || ',' || 'TELEPHONE#' || ',' || 'EMAIL');
OPEN c_emp;
   LOOP
       FETCH c_emp INTO c_emp_rec;
       EXIT WHEN c_emp%notfound;
        dbms_output.put_line(c_emp_rec.eid || ',' || c_emp_rec.name || ',' || c_emp_rec.telephone# || ',' ||c_emp_rec.email);
   END LOOP;
CLOSE c_emp;
END show_employees;
 
/* question 2: procedure to show the tuples in customers */
PROCEDURE show_customers
AS
   CURSOR c_cust IS
   SELECT * FROM customers;
   c_cust_rec c_cust%rowtype;
 
BEGIN
dbms_output.put_line('CID' || ',' || 'NAME' || ',' || 'TELEPHONE#' || ',' ||'VISITS_MADE' || ',' || 'LAST_VISIT_DATE');
OPEN c_cust;
   LOOP
       FETCH c_cust INTO c_cust_rec;
       EXIT WHEN c_cust%notfound;
        dbms_output.put_line(c_cust_rec.cid || ',' || c_cust_rec.name || ',' || c_cust_rec.telephone# || ',' ||c_cust_rec.visits_made || ',' || c_cust_rec.last_visit_date);
   END LOOP;
CLOSE c_cust;
END show_customers;
    
/* question 2: procedure to show the tuples in products */
PROCEDURE show_products
AS
   CURSOR c_pr IS
   SELECT * FROM products;
   c_pr_rec c_pr%rowtype;
 
BEGIN
dbms_output.put_line('PID' || ',' || 'NAME' || ',' || 'QOH' || ',' || 'QOH_THRESHOLD' || ',' || 'REGULAR_PRICE' || ',' || 'DISCNT_RATE');
OPEN c_pr;
   LOOP
       FETCH c_pr INTO c_pr_rec;
       EXIT WHEN c_pr%notfound;
        dbms_output.put_line(c_pr_rec.pid || ',' || c_pr_rec.name || ',' || c_pr_rec.qoh || ',' ||c_pr_rec.qoh_threshold || ',' || c_pr_rec.regular_price || ',' || c_pr_rec.discnt_rate);
   END LOOP;
CLOSE c_pr;
END show_products;
    
/* question 2: procedure to show the tuples in purchases */
PROCEDURE show_purchases
AS
   CURSOR c_pur IS
   SELECT * FROM purchases;
   c_pur_rec c_pur%rowtype;
 
BEGIN
dbms_output.put_line('PUR#' || ',' || 'EID' || ',' || 'PID' || ',' || 'CID' || ',' || 'PUR_DATE' || ',' || 'QTY' || ',' || 'UNIT_PRICE' || ',' || 'TOTAL' ||','|| 'SAVING');
OPEN c_pur;
   LOOP
       FETCH c_pur INTO c_pur_rec;
       EXIT WHEN c_pur%notfound;
        dbms_output.put_line(c_pur_rec.pur# || ',' || c_pur_rec.eid || ',' || c_pur_rec.pid || ',' ||c_pur_rec.cid || ',' || c_pur_rec.pur_date || ',' || c_pur_rec.qty || ',' ||
        c_pur_rec.unit_price || ',' || c_pur_rec.total ||','|| c_pur_rec.saving);
   END LOOP;
CLOSE c_pur;
END show_purchases;
    
/* question 2: procedure to show the tuples in logs */
PROCEDURE show_logs
AS
   CURSOR c_log IS
   SELECT * FROM logs;
   c_log_rec c_log%rowtype;
 
BEGIN
dbms_output.put_line('LOG#' || ',' || 'USER_NAME' || ',' || 'OPERATION' || ',' || 'OP_TIME' || ',' || 'TABLE_NAME' || ',' || 'TUPLE_PKEY');
OPEN c_log;
   LOOP
       FETCH c_log INTO c_log_rec;
       EXIT WHEN c_log%notfound;
        dbms_output.put_line(c_log_rec.log# || ',' || c_log_rec.user_name || ',' || c_log_rec.operation || ',' ||c_log_rec.op_time || ',' || c_log_rec.table_name || ',' || c_log_rec.tuple_pkey);
   END LOOP;
CLOSE c_log;
END show_logs;
 
 
/* question 3,: procedure to return the name of the customer as well as every purchase */
PROCEDURE purchases_made(c_id in customers.cid%type)
     IS
         cursor cursor1 is
           select * from
               (select name from customers where cid = c_id) a,
               (select * from
                   (select pid, pur_date, qty, unit_price, total from purchases where cid = c_id)) b;
                  
         c_cid NUMBER;
         invalid_cid EXCEPTION;
     BEGIN
       c_cid:= 0;
       SELECT count(cid) INTO c_cid FROM CUSTOMERS WHERE cid = c_id;
       IF(c_cid = 0) THEN
           RAISE invalid_cid;
       ELSE     
           dbms_output.put_line( 'name' || ',' || 'pid' || ',' || 'pur_date' || ',' || 'qty' || ',' || 'unit_price' || ',' || 'total');
           for c1 in cursor1
               loop
                   dbms_output.put_line( c1.name || ',' || c1.pid || ',' || c1.pur_date || ',' || c1.qty || ',' || c1.unit_price || ',' || c1.total );
               end loop;
       END IF;
      
    EXCEPTION
       WHEN invalid_cid  THEN
           dbms_output.put_line('Invalid Customer ID');
                      
END purchases_made;
 
 
/* question 4: function to report the number of customers who have purchased the product identified by the pid */
FUNCTION number_customers(cust_pid IN purchases.pid%type)
   RETURN NUMBER IS
       num_of_customers NUMBER;
       c_pid NUMBER;
       invalid_pid EXCEPTION;
      
   BEGIN     
       c_pid:= 0;
       SELECT count(pid) INTO c_pid FROM PURCHASES WHERE pid = cust_pid;
       IF(c_pid = 0) THEN
           RAISE invalid_pid;
       ELSE
           SELECT count(DISTINCT cid) into num_of_customers FROM purchases WHERE pid = cust_pid;      
           return num_of_customers; 
      
       END IF;
   EXCEPTION
       WHEN invalid_pid  THEN
       dbms_output.put_line('Invalid purchase ID.');
       RETURN 0;
END;
 
/* question 5: procedure for adding tuples to the Customers table*/
PROCEDURE add_customer(c_id IN CUSTOMERS.CID%TYPE, c_name IN CUSTOMERS.NAME%TYPE, c_telephone# IN CUSTOMERS.TELEPHONE#%TYPE)
     IS
     BEGIN
         INSERT INTO CUSTOMERS VALUES (c_id, c_name, c_telephone#, 1, sysdate);
         commit;
     END;
 
 
/* question 6: procedure for adding tuples to the Purchases table */
 
PROCEDURE add_purchase(
e_id in purchases.eid%type,
p_id in purchases.pid%type,
c_id in purchases.cid%type,
pur_qty in purchases.qty%type,
pur_unit_price in purchases.unit_price%type
)
IS
   pur_total PURCHASES.TOTAL%TYPE;
   pur_saving PURCHASES.SAVING%TYPE;
   reg_price PRODUCTS.REGULAR_PRICE%TYPE;
   pr_qoh PRODUCTS.QOH%TYPE;
   insuf_qty exception;
        
BEGIN
   pur_total :=  pur_qty * pur_unit_price;
   SELECT REGULAR_PRICE INTO reg_price FROM PRODUCTS WHERE PID = p_id;
   pur_saving := (reg_price * pur_qty) - (pur_qty * pur_unit_price);
   SELECT QOH INTO pr_qoh FROM PRODUCTS WHERE PID = p_id;
 
   IF(pur_qty <= pr_qoh) THEN
   /*Insert new purchase*/
     INSERT INTO PURCHASES VALUES (seqpur#.nextval, e_id, p_id, c_id, sysdate, pur_qty, pur_unit_price, pur_total, pur_saving);
     commit;
   ELSE
     RAISE insuf_qty;
   END IF;
  
   EXCEPTION
     WHEN insuf_qty THEN
     dbms_output.put_line('Insufficient quantity in stock.');
        
 END add_purchase;
 
 
END;
 
 
Alternative method: Compile objects via .sql files
The alternative of the above steps is to save different objects as different .sql files in the current worksheet of oracle. In our case, we would require 3 files. 
Save the sequence code of step 1 as “retailBusiness_sequence.sql”.
Save the trigger code of step 2 as “retailBusiness_triggers.sql”.
Save the package code of step 3 as “retailBusiness_package.sql”.
Once the .sql files are saved, the below set of codes should be executed from the worksheet in order to compile the PL/SQL objects. 
start <Sequences_file_name>
start <Triggers_file_name>
start <Package_file_name>
Note:
All the above mentioned objects should be created after the creation of required tables.
As per the recommendation we shall compile the objects as: Sequences →  Triggers → Package.
Whenever we drop any table and recreate, we should recreate the corresponding triggers and sequences which were created on it.

Java Database Connectivity (JDBC)
Java Database Connectivity (JDBC) is an application programming interface (API) for the programming language Java, which defines how a client may access a database. The Java Database Connectivity (JDBC) API provides universal data access from the Java programming language.
Below the jdbc coding is provided which has two-fold purpose as per the requirements document.
Retrieve all tuples of the Customers table: This is achieved by the getAllCust() method defined in the jdbc. No input parameters are required. 
Retrieve the tuple of any given “cid” from the Customers table: This is achieved by the getOneCust() method defined in the jdbc in which the “cid” of the customer is passed as an input by the end user through the keyboard. 
The JDBC requires it to be deployed in conjunction with the application coding logic in the middleware, however, we don't have an application for the jdbc in question. Hence here we skip the installation process. For our purpose, we will directly execute the jdbc file. The connection string defined in the code helps to authenticate the credentials and connect to the database. Once the connection is established, the code then performs the above defined operations as per the requirements document. 
Java code:

 
import java.sql.*;
import java.io.*;
import oracle.jdbc.pool.OracleDataSource;
 
public class getCust {
 
   public static void main(String args[]) throws SQLException {
      
       // Method for Part1 of JDBC - get all customers
       getAllCust();
      
       // Wait for user for Part2
       System.out.println();
       System.out.println("Press enter to continue...");try{        System.in.read();}catch(Exception e){  e.printStackTrace();}
      
       // Method for Part2 of JDBC - get one customer for a cid
       getOneCust();
   }
 
   private static void getAllCust() {
       try {
           // Connection to Oracle server.
           OracleDataSource ds = new oracle.jdbc.pool.OracleDataSource();
           ds.setURL("jdbc:oracle:thin:@castor.cc.binghamton.edu:1521:acad111");
           Connection conn = ds.getConnection("asrivas3", "sr927641");
 
           // Query
           Statement stmt = conn.createStatement();
 
           // Save result
           ResultSet rset;
           rset = stmt.executeQuery("SELECT * FROM customers");
 
           System.out.println("CID, Name, Telephone#, Visits Made, Last Visit Date ");
           // Print
           while (rset.next()) {
               System.out.print(rset.getString(1) + ", ");
               System.out.print(rset.getString(2) + ", ");
               System.out.print(rset.getString(3) + ", ");
               System.out.print(rset.getString(4) + ", ");
               System.out.println(rset.getString(5) + ", ");
           }
 
           // close the result set, statement, and the connection
           rset.close();
           stmt.close();
           conn.close();
       } catch (SQLException ex) {
           System.out.println(
                   "\n*** SQLException caught 1st***\n" + ex.getStackTrace() + ex.getMessage()
                           + ex.getLocalizedMessage());
       } catch (Exception e) {
           System.out.println(
                   "\n*** other Exception caught 2nd***\n" + e.getStackTrace() + e.getMessage()
                           + e.getLocalizedMessage());
       }
   }
 
   private static void getOneCust() {
       try {
 
           // Connection to Oracle server.
           OracleDataSource ds = new oracle.jdbc.pool.OracleDataSource();
           ds.setURL("jdbc:oracle:thin:@castor.cc.binghamton.edu:1521:acad111");
           Connection conn = ds.getConnection("asrivas3", "sr927641");
 
           // Input CID from keyboard
           BufferedReader readKeyBoard;
           String cid;
           readKeyBoard = new BufferedReader(new InputStreamReader(System.in));
           System.out.print("Please enter customer's CID:");
           cid = readKeyBoard.readLine();
 
           // Prepare statement and save in resultset
           PreparedStatement select1 = conn.prepareStatement("SELECT * FROM customers where cid = '" + cid + "'");
           ResultSet rset;
           select1.executeUpdate();
           rset = select1.executeQuery();
 
           // Check if CID exists or not and then display desired output
           if (!rset.isBeforeFirst()) {
               System.out.println("Customer doesn't exist");
           } else {
               System.out.println("CID, Name, Telephone#, Visits Made, Last Visit Date ");
               while (rset.next()) {
                   System.out.print(rset.getString(1) + ", ");
                   System.out.print(rset.getString(2) + ", ");
                   System.out.print(rset.getString(3) + ", ");
                   System.out.print(rset.getString(4) + ", ");
                   System.out.println(rset.getString(5) + ", ");
               }
           }
           // close the resultset, statement, and the connection
           rset.close();
           select1.close();
           conn.close();
       } catch (SQLException ex) {
           System.out.println("\n*** SQLException caught 1st***\n" + ex.getStackTrace() + ex.getMessage()
                   + ex.getLocalizedMessage());
       } catch (Exception e) {
           System.out.println("\n*** other Exception caught 2nd***\n" + e.getStackTrace() + e.getMessage()
                   + e.getLocalizedMessage());
       }
   }
}
 



