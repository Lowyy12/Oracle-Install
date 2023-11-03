The firts thing we have to do is update the packages:

``` bash
sudo apt-get update
```

https://raw.githubusercontent.com/Lowyy12/Oracle-Install/blob/main/fotos/Screenshot_20231102_134917.png

The second command is: 
``` bash
sudo apt-get install bc ksh libaio1 gawk
```

The next thing we have to do is configure the oracle users:

```bash
groupadd dba
useradd -m -s /bin/bash -g dba oracle
```

![[Screenshot_20231102_135136.png]]

### oracle installation

File

https://drive.google.com/uc?id=1ZZtnFNpeFtiRnCguUQM5NAjpt1EGLHlX&export=download

The next step:

```bash
dpkg -i oracle-database-xe-18c_1.0-2_amd64.deb
```

![[Screenshot_20231102_141919.png]]

We will start configuring

__IPV4 Settings__
![[Screenshot_20231102_142144.png]]

**Oracle Configuring**

```bash
/etc/init.d/oracle-xe-18c configure
```

![[Screenshot_20231102_143021.png]]

We will confirm the password will be used for SYS, SYSTEM and PDBADMIN accounts.

![[Screenshot_20231102_144752.png]]

_FATAL DBT-50000, Unable to check for available memory, Oracle 18c XE_

File:

/etc/init.d/oracle-xe-18c
![[Screenshot_20231102_145023.png]]

![[Screenshot_20231102_145109.png]]
-J-Doracle.assistants.dbca.validate.ConfigurationParams=false

Finish
![[Screenshot_20231102_145407.png]]

Change the **oracle** uiser password

![[Screenshot_20231102_145611.png]]

We will export the path to be able to have sqlplus

![[Screenshot_20231102_145936.png]]

To make it stay permanently, we copy this into the .bashrc and close and open the terminal.

``` bash
export ORACLE_HOME=/opt/oracle/product/18c/dbhomeXE/  
export ORACLE_SID=XE  
export PATH=$ORACLE_HOME/bin/:$PATH
```

Solution error (ORA-12547: TNS:lost contact) 

https://rene-ace.com/tratas-de-conectarte-como-sysdba-y-no-puedes-por-el-error-ora-12547/

![[Screenshot_20231102_235801.png]]

To mount the database:

![[Screenshot_20231103_003922.png]]

Now we are going to add an extra and it is to have a user in this case "lowy" as a user in Oracle and not only in our operating system and we will also create our own role:

ORA-6509 Solution
![[Screenshot_20231103_005024.png]]

Create role:

```sqlñ
CREATE ROLE rol_ini;

CREATE ROLE rol_ini NOT IDENTIFIED;

GRANT CREATE SESSION, CREATE TABLE, CREATE VIEW TO rol_ini;
```

Create user:

```sql
CREATE USER lowy IDENTIFIED BY uno

DEFAULT TABLESPACE USERS

TEMPORARY TABLESPACE TEMP

QUOTA 100K ON USERS;
```

Assign role:

```sql
grant rol_ini to lowy;
```

ORA-65096 Solution:

```sql
alter system set "_ORACLE_SCRIPT"=true SCOPE=SPFILE;
```


Change password from user:

```sql
alter user lowy identified by 1234;
```

#### This would be one of the most correct ways to start with databases, I hope you liked it.
