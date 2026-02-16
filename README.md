# Oracle Pluggable Database (PDB) Management

## Individual Assignment II – INSY 8311

**Student Name:** Alain
**Student ID:** 22967
**Repository Name:** oracle_pdb_ass_II_22967_alain

---

# 1️ Overview of Tasks

This assignment demonstrates practical understanding of Oracle Multitenant Architecture by performing the following:

1. Creating a new Pluggable Database (PDB)
2. Creating a user inside the PDB with required privileges
3. Creating and deleting a temporary PDB
4. Configuring and accessing Oracle Enterprise Manager (OEM)
5. Documenting the full process with screenshots

This README serves as a step-by-step guide of the actions performed.

---

# 2️ Oracle Environment Used

* **Database:** Oracle Database 21c XE
* **Architecture:** Multitenant (CDB + PDB)
* **Tools:** SQL*Plus, Oracle SQL Developer
* **OEM Access:** Oracle Enterprise Manager Express
* **Host:** localhost
* **Port:** 1521

---

# 3️ Task 1 – Create Main PDB and User

## Step 1: Connect as SYSDBA

Opened Command Prompt and connected using:

```
sqlplus / as sysdba
```

Checked existing PDBs:

```
SHOW PDBS;
```
![PDB Before Creation](screenshots/01_show_pdbs_before.png)


* Existing PDB list before creation

---

## Step 2: Create Main PDB

Created the required PDB using the correct naming format:

```
CREATE PLUGGABLE DATABASE AL_PDB_22967
ADMIN USER pdbadmin IDENTIFIED BY <password>
FILE_NAME_CONVERT = ('pdbseed','AL_PDB_22967');
```

**Explanation:**
This command creates a new pluggable database by copying the template (PDB$SEED) and assigning a new name.
![PDB Before Creation](screenshots/02_create_main_pdb.png)

* PDB creation command
* “Pluggable database created” message

---

## Step 3: Open the PDB

After creation, the PDB was in MOUNTED state. It was opened using:

```
ALTER PLUGGABLE DATABASE AL_PDB_22967 OPEN;
```

Verified status:

```
SHOW PDBS;
```

**Explanation:**
The PDB must be in READ WRITE mode to allow user creation and access.

![PDB Before Creation](screenshots/03_open_main_pdb.png)

* SHOW PDBS displaying AL_PDB_22967 in READ WRITE

---

## Step 4: Create User Inside the PDB

Switched to the PDB:

```
ALTER SESSION SET CONTAINER = AL_PDB_22967;
```

Created the required user:

```
CREATE USER ALAIN_PLSQLAUCA_22967 IDENTIFIED BY root;
```

Granted privileges:

```
GRANT CREATE SESSION TO ALAIN_PLSQLAUCA_22967;
GRANT CONNECT, RESOURCE TO ALAIN_PLSQLAUCA_22967;
```

**Explanation:**
The user is created inside the PDB and granted privileges required to log in and perform database operations.

Verified user existence:

```
SELECT username FROM dba_users;
```

![PDB Before Creation](screenshots/04_user_created.png)

* User creation and grants
* Query showing ALAIN_PLSQLAUCA_22967

---

# 4️ Task 2 – Create and Delete Temporary PDB

## Step 1: Create Temporary PDB

Returned to root container and created:

```
CREATE PLUGGABLE DATABASE AL_TO_DELETE_PDB_22967
ADMIN USER tempadmin IDENTIFIED BY <password>
FILE_NAME_CONVERT = ('pdbseed','AL_TO_DELETE_PDB_22967');
```

![PDB Before Creation](screenshots/04_user_created.png)

* Temporary PDB creation confirmation

---

## Step 2: Verify Temporary PDB

Opened and verified:

```
ALTER PLUGGABLE DATABASE AL_TO_DELETE_PDB_22967 OPEN;
SHOW PDBS;
```

* SHOW PDBS displaying temporary PDB

---

## Step 3: Delete Temporary PDB

Closed the PDB:

```
ALTER PLUGGABLE DATABASE AL_TO_DELETE_PDB_22967 CLOSE IMMEDIATE;
```

Deleted permanently:

```
DROP PLUGGABLE DATABASE AL_TO_DELETE_PDB_22967 INCLUDING DATAFILES;
```

Confirmed removal:

```
SHOW PDBS;
```

**Explanation:**
The PDB was removed completely along with its datafiles to ensure full deletion.

![PDB Before Creation](screenshots/06_temp_pdb_deleted.png)


* Drop command success
* SHOW PDBS confirming deletion

---

# 5️ Task 3 – Oracle Enterprise Manager (OEM)

Checked HTTPS port:

```
SELECT DBMS_XDB_CONFIG.GETHTTPSPORT() FROM dual;
```

Configured alternative port when necessary:

```
EXEC DBMS_XDB_CONFIG.SETHTTPSPORT(5501);
```

Accessed OEM through:

```
https://localhost:5501/em
```

Logged in as:

* Username: sys
* Role: SYSDBA

Verified:

* Database environment visible
* Main PDB (AL_PDB_22967) visible
* Created user visible under Security → Users

![PDB Before Creation](screenshots/08_oem_dashboard.png)


* OEM dashboard showing PDB
* OEM page showing ALAIN_PLSQLAUCA_22967

---

# 6️ Challenges and Solutions

* **PDB initially MOUNTED:** Opened using `ALTER PLUGGABLE DATABASE ... OPEN;`
* **Invalid login credentials:** Ensured correct container and granted `CREATE SESSION`
* **OEM port conflict:** Configured alternative free HTTPS port

All issues were resolved successfully.

---

# 7️ Integrity Statement

I confirm that this assignment was completed independently in my own Oracle environment.
All commands were executed personally, and all screenshots represent my own work.

---
