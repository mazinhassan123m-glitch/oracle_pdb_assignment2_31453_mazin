# Individual Assignment II: Oracle Pluggable Database (PDB) Administration

**Student Name:** Mazin  
**Student ID:** 31453  
**Course:** Database Programming (CI1665 - DPR400210)  

---

## 1. Assignment Overview
The objective of this practical assignment is to implement and manage an Oracle Multitenant Architecture. This includes creating and managing Pluggable Databases (PDBs), configuring database users, managing temporary database resources, and configuring Oracle Enterprise Manager (OEM) for structural administration.

---

## 2. Oracle Environment
*   **Oracle Version Used:** Oracle Database 21c Express Edition (XE)
*   **Operating System/Environment:** Windows 11 / Command Prompt (SQL*Plus)
*   **Tools Used:** SQL*Plus, Oracle Enterprise Manager (OEM) Express

---

## 3. Task Documentation

### A. PDB Creation Process
*   **Steps:** Connected to the Container Database (CDB) as `SYSDBA` and executed the SQL command to create a new pluggable database named `ma_pdb_31453`. Opened the newly created PDB and saved its operational state.
*   **SQL Commands:**
    ```sql
    CONNECT / AS SYSDBA;
    CREATE PLUGGABLE DATABASE ma_pdb_31453 ADMIN USER pdb_admin IDENTIFIED BY AdminPassword123 FILE_NAME_CONVERT=('pdbseed', 'ma_pdb_31453');
    ALTER PLUGGABLE DATABASE ma_pdb_31453 OPEN;
    ALTER PLUGGABLE DATABASE ma_pdb_31453 SAVE STATE;
    ```
*   **Evidence:**  
    ![PDB Creation](screenshots/pdb_creation.png)

### B. User Creation Process
*   **Steps:** Switched the session context to the newly created pluggable database `ma_pdb_31453`, created a local database user named `mazin_plsqlauca_31453`, and granted administrative roles (`CONNECT`, `RESOURCE`, `DBA`).
*   **SQL Commands:**
    ```sql
    ALTER SESSION SET CONTAINER = ma_pdb_31453;
    CREATE USER mazin_plsqlauca_31453 IDENTIFIED BY MazinPass2026;
    GRANT CONNECT, RESOURCE, DBA TO mazin_plsqlauca_31453;
    ```
*   **Evidence:**  
    ![User Creation](screenshots/user_creation.png)

### C. User Login Verification
*   **Steps:** Confirmed the successful configuration by establishing a new session connection to the specific PDB using the newly created local user credentials.
*   **SQL Commands:**
    ```sql
    CONNECT mazin_plsqlauca_31453/MazinPass2026@//localhost:1521/ma_pdb_31453
    SHOW USER;
    ```
*   **Evidence:**  
    ![User Login Verification](screenshots/user_login.png)

### D. Temporary PDB Creation and Deletion
*   **Steps:** Created a temporary pluggable database `ma_to_delete_pdb_31453` to test transient administrative capabilities. Verified its existence via `SHOW PDBS`, closed it, and dropped it entirely including datafiles.
*   **SQL Commands:**
    ```sql
    -- Creation and Verification
    CREATE PLUGGABLE DATABASE ma_to_delete_pdb_31453 ADMIN USER temp_admin IDENTIFIED BY TempPassword123 FILE_NAME_CONVERT=('pdbseed', 'ma_to_delete_pdb_31453');
    ALTER PLUGGABLE DATABASE ma_to_delete_pdb_31453 OPEN;
    SHOW PDBS;
    
    -- Deletion and Confirmation
    ALTER PLUGGABLE DATABASE ma_to_delete_pdb_31453 CLOSE IMMEDIATE;
    DROP PLUGGABLE DATABASE ma_to_delete_pdb_31453 INCLUDING DATAFILES;
    SHOW PDBS;
    ```
*   **Evidence (Creation):**  
    ![Temporary PDB Creation](screenshots/temporary_pdb_creation.png)
*   **Evidence (Deletion):**  
    ![Temporary PDB Deletion](screenshots/temporary_pdb_deletion.png)

### E. OEM Configuration
*   **Steps:** Configured and verified the HTTPS port allocation for Oracle Enterprise Manager to allow browser-based visualization of the multitenant architecture.
*   **SQL Commands:**
    ```sql
    CONNECT / AS SYSDBA;
    EXEC DBMS_XDB_CONFIG.SETHTTPSPORT(5500);
    ```
*   **Evidence:**  
    ![OEM Dashboard](screenshots/oem_dashboard.png)

---

## 4. Challenges and Solutions
1.  **Challenge:** PDB closed automatically when the CDB restarted.  
    **Solution:** Executed `ALTER PLUGGABLE DATABASE ma_pdb_31453 SAVE STATE;` to ensure persistence across database lifecycles.
2.  **Challenge:** Formatting limitations on database names preventing the use of standard slashes from student identifiers.  
    **Solution:** Normalized the database container suffix to standard numerical digits `31453` to avoid syntax compilation restrictions.

---

## 5. Lessons Learned
*   Gained practical knowledge in managing tenant resource virtualization using Oracle Multitenant tools.
*   Mastered operational workflow boundaries between global root containers (CDB) and localized database containers (PDB).
*   Learned the absolute critical importance of structure verification, command isolation, and environment asset separation.

---

## 6. Integrity Statement
"I confirm that this assignment represents my own practical work, screenshots, and documentation. All external resources consulted have been properly acknowledged."