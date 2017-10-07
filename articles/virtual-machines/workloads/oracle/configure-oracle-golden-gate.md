---
title: "az Azure Linux virtuális gép az Oracle-Golden kapu aaaImplement |} Microsoft Docs"
description: "Gyorsan karban lehessen az Oracle Golden mentése és az Azure-alapú környezetben futó."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: v-shiuma
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: rclaus
ms.openlocfilehash: 320cafd5d23ee472f0af9f92577bc6f432f65778
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="implement-oracle-golden-gate-on-an-azure-linux-vm"></a>Oracle Golden kapu valósítja meg az Azure Linux virtuális gép 

hello Azure CLI használt toocreate és hello parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez. Ez az útmutató adatokat hogyan toouse hello Azure CLI toodeploy egy 12c Oracle adatbázis-hello Azure piactér gyűjtemény lemezképből. 

Ez a dokumentum lépésről lépésre bemutatja hogyan toocreate, telepíteni és beállítani az Oracle Golden kapu egy Azure virtuális gépen.

Mielőtt elkezdené, győződjön meg arról, hogy telepítve van az Azure CLI hello listájában. További információért lásd az [Azure CLI telepítési útmutatóját](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="prepare-hello-environment"></a>Hello környezet előkészítése

tooperform hello a Golden kapu Oracle telepítés kell toocreate két Azure-alapú virtuális gépek hello azonos rendelkezésre állási csoportot. hello Piactéri rendszerkép toocreate hello virtuális gépek használata **Oracle: Oracle-adatbázis-Ee:12.1.0.2:latest**.

Is toobe Unix szerkesztő vi ismernie kell, és megismerheti a x11 (X Windows).

hello az alábbiakban látható hello környezet konfigurációjának összefoglalása:
> 
> |  | **Elsődleges hely** | **Hely replikálása** |
> | --- | --- | --- |
> | **Oracle-kiadás** |Oracle 12c kiadás 2 – (12.1.0.2) |Oracle 12c kiadás 2 – (12.1.0.2)|
> | **Számítógép neve** |myVM1 |myVM2 |
> | **Operációs rendszer** |Oracle Linux 6.x |Oracle Linux 6.x |
> | **Oracle SID** |CDB1 |CDB1 |
> | **Replikációs séma** |TESZT|TESZT |
> | **Tulajdonos vagy replicate Golden kapu** |C ##GGADMIN |REPUSER |
> | **Golden kapu folyamat** |EXTORA |REPORA|


### <a name="sign-in-tooazure"></a>Jelentkezzen be tooAzure 

Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot. Ezután kövesse hello képernyőn megjelenő utasításokat.

```azurecli
az login
```

### <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot. Egy Azure erőforráscsoport egy olyan logikai tároló mely Azure-erőforrások vannak telepítve és onnan, ami által kezelhető legyen. 

hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `westus` helyét.

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-an-availability-set"></a>Rendelkezésre állási csoport létrehozása

a következő lépés hello, kötelező, de ajánlott. További információkért lásd: [Azure rendelkezésre állási készletek útmutató](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-a-virtual-machine"></a>Virtuális gép létrehozása

Hozzon létre egy virtuális gép hello [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot. 

hello alábbi példa létrehoz két virtuális gépek nevű `myVM1` és `myVM2`. SSH-kulcsok létrehozása, ha még nem léteznek a kulcs alapértelmezett helye. toouse egy adott kulcsok beállításához használja a hello `--ssh-key-value` lehetőséget.

#### <a name="create-myvm1-primary"></a>Hozzon létre myVM1 (elsődleges):
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM1 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

Hello a virtuális gép létrehozása, miután hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg. (Jegyezze fel a hello `publicIpAddress`. Ez a cím az használt tooaccess hello VM.)

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-0D-3A-36-2F-56",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.64.104.241",
  "resourceGroup": "myResourceGroup"
}
```

#### <a name="create-myvm2-replicate"></a>Hozzon létre myVM2 (replikáció):
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM2 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

Jegyezze fel a hello `publicIpAddress` , valamint a létrehozása után.

### <a name="open-hello-tcp-port-for-connectivity"></a>Nyissa meg a TCP-portját hello kapcsolat

következő lépés hello tooconfigure külső végpontok száma, amelyek lehetővé teszik a távoli tooaccess hello Oracle-adatbázishoz. tooconfigure hello külső végpontok száma, futtassa a következő parancsok hello.

#### <a name="open-hello-port-for-myvm1"></a>Nyissa meg a hello port myVM1:

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm1NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

hello eredmények alábbihoz hasonló toohello válasz a következő:

```bash
{
  "access": "Allow",
  "description": null,
  "destinationAddressPrefix": "*",
  "destinationPortRange": "1521",
  "direction": "Inbound",
  "etag": "W/\"bd77dcae-e5fd-4bd6-a632-26045b646414\"",
  "id": "/subscriptions/<subscription-id>/resourceGroups/myResourceGroup/providers/Microsoft.Network/networkSecurityGroups/myVmNSG/securityRules/allow-oracle",
  "name": "allow-oracle",
  "priority": 999,
  "protocol": "Tcp",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sourceAddressPrefix": "*",
  "sourcePortRange": "*"
}
```

#### <a name="open-hello-port-for-myvm2"></a>Nyissa meg a hello port myVM2:

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm2NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-toohello-virtual-machine"></a>Csatlakoztassa toohello virtuális gépet

Használjon hello következő parancsot a toocreate egy hello virtuális gép SSH-munkamenetet. Cserélje le hello IP-cím hello `publicIpAddress` a virtuális gép.

```bash 
ssh <publicIpAddress>
```

### <a name="create-hello-database-on-myvm1-primary"></a>Adatbázis létrehozásához. hello myVM1 (elsődleges)

hello Oracle szoftver már telepítve van hello Piactéri lemezképhez, így a következő lépésre hello tooinstall hello adatbázis. 

Hello szoftvert futtató hello "oracle" felügyelő:

```bash
sudo su - oracle
```

hello adatbázis létrehozása:

```bash
$ dbca -silent \
   -createDatabase \
   -templateName General_Purpose.dbc \
   -gdbname cdb1 \
   -sid cdb1 \
   -responseFile NO_VALUE \
   -characterSet AL32UTF8 \
   -sysPassword OraPasswd1 \
   -systemPassword OraPasswd1 \
   -createAsContainerDatabase true \
   -numberOfPDBs 1 \
   -pdbName pdb1 \
   -pdbAdminPassword OraPasswd1 \
   -databaseType MULTIPURPOSE \
   -automaticMemoryManagement false \
   -storageType FS \
   -ignorePreReqs
```
Kimenetek alábbihoz hasonló toohello válasz a következő:

```bash
Copying database files
1% complete
2% complete
8% complete
13% complete
19% complete
27% complete
Creating and starting Oracle instance
29% complete
32% complete
33% complete
34% complete
38% complete
42% complete
43% complete
45% complete
Completing Database Creation
48% complete
51% complete
53% complete
62% complete
70% complete
72% complete
Creating Pluggable Databases
78% complete
100% complete
Look at hello log file "/u01/app/oracle/cfgtoollogs/dbca/cdb1/cdb1.log" for more details.
```

Hello ORACLE_SID és ORACLE_HOME változók megadása.

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=gg1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

Szükség esetén ORACLE_HOME és ORACLE_SID toohello .bashrc fájl, adhat hozzá, hogy ezek a beállítások a későbbi bejelentkezések lesznek mentve:

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=gg1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a>Indítsa el az Oracle-figyelő
```bash
$ sudo su - oracle
$ lsnrctl start
```

### <a name="create-hello-database-on-myvm2-replicate"></a>Hello adatbázis létrehozása a következőn myVM2 (replikáció)

```bash
sudo su - oracle
```
hello adatbázis létrehozása:

```bash
$ dbca -silent \
   -createDatabase \
   -templateName General_Purpose.dbc \
   -gdbname cdb1 \
   -sid cdb1 \
   -responseFile NO_VALUE \
   -characterSet AL32UTF8 \
   -sysPassword OraPasswd1 \
   -systemPassword OraPasswd1 \
   -createAsContainerDatabase true \
   -numberOfPDBs 1 \
   -pdbName pdb1 \
   -pdbAdminPassword OraPasswd1 \
   -databaseType MULTIPURPOSE \
   -automaticMemoryManagement false \
   -storageType FS \
   -ignorePreReqs
```
Hello ORACLE_SID és ORACLE_HOME változók megadása.

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

Szükség esetén is felvett ORACLE_HOME és ORACLE_SID toohello .bashrc fájlt, így ezek a beállítások lesznek mentve a későbbi bejelentkezések.

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a>Indítsa el az Oracle-figyelő
```bash
$ sudo su - oracle
$ lsnrctl start
```

## <a name="configure-golden-gate"></a>Golden kapu beállítása 
tooconfigure Golden kapu, ebben a szakaszban hello lépéseket.

### <a name="enable-archive-log-mode-on-myvm1-primary"></a>Archív napló mód a myVM1 (elsődleges) engedélyezése

```bash
$ sqlplus / as sysdba
SQL> SELECT log_mode FROM v$database;

LOG_MODE
------------
NOARCHIVELOG

SQL> SHUTDOWN IMMEDIATE;
SQL> STARTUP MOUNT;
SQL> ALTER DATABASE ARCHIVELOG;
SQL> ALTER DATABASE OPEN;
```
Kényszerített naplózását, és ellenőrizze, hogy legalább egy naplófájlt.

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
SQL> ALTER SYSTEM set enable_goldengate_replication=true;
SQL> ALTER PLUGGABLE DATABASE PDB1 OPEN;
SQL> ALTER SESSION SET CONTAINER=PDB1;
SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA;
SQL> EXIT;
```

### <a name="download-golden-gate-software"></a>Golden kapu szoftver letöltése
toodownload és hello Oracle Golden kapu szoftver, a következő lépéseket teljes hello előkészítése:

1. Töltse le a hello **fbo_ggs_Linux_x64_shiphome.zip** hello fájlt [Oracle Golden kapu letöltési oldala](http://www.oracle.com/technetwork/middleware/goldengate/downloads/index.html). Hello alatt töltse le a cím **Oracle GoldenGate 12.x.x.x az Oracle Linux x86-64**, .zip fájlokat toodownload készlete kell lennie.

2. Hello .zip fájlokat tooyour ügyfélszámítógép letöltése után használja a biztonságos másolási protokoll (SCP) toocopy hello fájlok tooyour VM:

  ```bash
  $ scp fbo_ggs_Linux_x64_shiphome.zip <publicIpAddress>:<folder>
  ```

3. Helyezze át a hello .zip fájlokat toohello **/ opt** mappát. Az alábbiak szerint módosítsa a hello fájlok hello tulajdonosa:

  ```bash
  $ sudo su -
  # mv <folder>/*.zip /opt
  ```

4. Bontsa ki a hello fájlok (telepítés hello Linux csomagolja ki segédprogram Ha még nincs telepítve van):

  ```bash
  # yum install unzip
  # cd /opt
  # unzip fbo_ggs_Linux_x64_shiphome.zip
  ```

5. Engedély módosítása:

  ```bash
  # chown -R oracle:oinstall /opt/fbo_ggs_Linux_x64_shiphome
  ```

### <a name="prepare-hello-client-and-vm-toorun-x11-for-windows-clients-only"></a>Előkészítése hello ügyfél és a virtuális gép toorun x11 (csak Windows-ügyfelek)
Ez az egy opcionális lépés. Ezt a lépést kihagyhatja, ha a Linux ügyfelet kell használnia, vagy már rendelkezik x11 beállítása.

1. Töltse le a PuTTY és Xming tooyour Windows-számítógép:

  * [Töltse le a PuTTY](http://www.putty.org/)
  * [Töltse le a Xming](https://xming.en.softonic.com/)

2.  PuTTY telepítése után a PuTTY mappát (például, C:\Program Files\PuTTY) hello, puttygen.exe (PuTTY kulcs generátor) futtatásához.

3.  A PuTTY Megosztottelérésikulcs-készítő:

  - a kulcs, jelölje be hello toogenerate **Generate** gombra.
  - Hello kulcs hello tartalom másolása (**Ctrl + C**).
  - Jelölje be hello **mentés titkos kulcs** gombra.
  - Figyelmen kívül, amely akkor jelenik meg, majd válassza ki a hello figyelmeztetést **OK**.

    ![Hello PuTTY megosztottelérésikulcs-készítő oldalát bemutató képernyőkép](./media/oracle-golden-gate/puttykeygen.png)

4.  A virtuális gépen, futtassa az alábbi parancsokat:

  ```bash
  # sudo su - oracle
  $ mkdir .ssh (if not already created)
  $ cd .ssh
  ```

5. Hozzon létre egy fájlt **authorized_keys**. Hello kulcs tartalmát hello illessze be ezt a fájlt, és mentse hello fájlt.

  > [!NOTE]
  > hello kulcs tartalmaznia kell a hello karakterlánc `ssh-rsa`. Hello hello kulcs tartalmát is, egy egyszerű szövegsor kell lennie.
  >  

6. Indítsa el a PuTTY alkalmazást. A hello **kategória** ablaktáblán válassza előbb **kapcsolat** > **SSH** > **Auth**. A hello **hitelesítéshez titkos kulcsfájl** mezőbe keresse meg a korábban létrehozott toohello kulcs.

  ![Hello titkos kulcs beállítása oldalát bemutató képernyőkép](./media/oracle-golden-gate/setprivatekey.png)

7. A hello **kategória** ablaktáblán válassza előbb **kapcsolat** > **SSH** > **X11**. Válassza ki hello **engedélyezése X11 továbbítási** mezőbe.

  ![Hello engedélyezése X11 oldalát bemutató képernyőkép](./media/oracle-golden-gate/enablex11.png)

8. A hello **kategória** ablaktáblában lépjen túl**munkamenet**. Adja meg a hello állomásadatai, majd válassza ki **nyitott**.

  ![Hello munkamenet oldalát bemutató képernyőkép](./media/oracle-golden-gate/puttysession.png)

### <a name="install-golden-gate-software"></a>Golden kapu szoftver telepítése

tooinstall Oracle Golden kapu, teljes hello a következő lépéseket:

1. Jelentkezzen be a oracle felhasználóként. (A jelszó megadása nélkül képes toosign kell lennie.) Győződjön meg arról, hogy fut-e Xming, hello telepítés megkezdése előtt.
 
  ```bash
  $ cd /opt/fbo_ggs_Linux_x64_shiphome/Disk1
  $ ./runInstaller
  ```
2. Válassza ki az "Oracle adatbázis 12c Oracle GoldenGate". Válassza ki **következő** toocontinue.

  ![Képernyőfelvétel a hello telepítő telepítési kiválasztása lap](./media/oracle-golden-gate/golden_gate_install_01.png)

3. Hello szoftver helyének módosítása. Válassza ki hello **Start Manager** mezőben, és írja be a hello adatbázis helye. Válassza ki **következő** toocontinue.

  ![Képernyőfelvétel a hello telepítési kiválasztása lap](./media/oracle-golden-gate/golden_gate_install_02.png)

4. Módosítsa hello készlet könyvtárat, majd válassza ki **következő** toocontinue.

  ![Képernyőfelvétel a hello telepítési kiválasztása lap](./media/oracle-golden-gate/golden_gate_install_03.png)

5. A hello **összegzés** képernyőn válassza ki **telepítése** toocontinue.

  ![Képernyőfelvétel a hello telepítő telepítési kiválasztása lap](./media/oracle-golden-gate/golden_gate_install_04.png)

6. Előfordulhat, hogy a kért toorun parancsfájl "legfelső szintű". Ha igen, nyisson meg egy külön munkamenet ssh toohello VM, sudo tooroot, és futtassa hello parancsfájl. Válassza ki **OK** továbbra is.

  ![Képernyőfelvétel a hello telepítési kiválasztása lap](./media/oracle-golden-gate/golden_gate_install_05.png)

7. Hello telepítésének befejezését, válassza ki a **Bezárás** toocomplete hello folyamat.

  ![Képernyőfelvétel a hello telepítési kiválasztása lap](./media/oracle-golden-gate/golden_gate_install_06.png)

### <a name="set-up-service-on-myvm1-primary"></a>Állítsa be a szolgáltatást a myVM1 (elsődleges)

1. Hozzon létre vagy hello tnsnames.ora fájl frissítése:

  ```bash
  $ cd $ORACLE_HOME/network/admin
  $ vi tnsnames.ora

  cdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=cdb1)
      )
    )

  pdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=pdb1)
      )
    )
  ```

2. Hello Golden kapu tulajdonosi és felhasználói fiókokat hozhat létre.

  > [!NOTE]
  > hello tulajdonos fióknak rendelkeznie kell a C ## előtag.
  >

    ```bash
    $ sqlplus / as sysdba
    SQL> CREATE USER C##GGADMIN identified by ggadmin;
    SQL> EXEC dbms_goldengate_auth.grant_admin_privilege('C##GGADMIN',container=>'ALL');
    SQL> GRANT DBA tooC##GGADMIN container=all;
    SQL> connect C##GGADMIN/ggadmin
    SQL> ALTER SESSION SET CONTAINER=PDB1;
    SQL> EXIT;
    ```

3. Hello Golden kapu teszt felhasználói fiók létrehozása:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ sqlplus system/OraPasswd1@pdb1
  SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
  SQL> GRANT connect, resource, dba tootest;
  SQL> ALTER USER test QUOTA 100M on USERS;
  SQL> connect test/test@pdb1
  SQL> @demo_ora_create
  SQL> @demo_ora_insert
  SQL> EXIT;
  ```

4. Hello kivonat paraméterfájl konfigurálása.

 Indítsa el a hello arany kapu parancssori felület (ggsci):

  ```bash
  $ sudo su - oracle
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> DBLOGIN USERID test@pdb1, PASSWORD test
  Successfully logged into database  pdb1
  GGSCI>  ADD SCHEMATRANDATA pdb1.test
  2017-05-23 15:44:25  INFO    OGG-01788  SCHEMATRANDATA has been added on schema test.
  2017-05-23 15:44:25  INFO    OGG-01976  SCHEMATRANDATA for scheduling columns has been added on schema test.

  GGSCI> EDIT PARAMS EXTORA
  ```
5. Adja hozzá a következő toohello kivonat paraméter fájl (parancsokkal vi) hello. Nyomja meg az Esc billentyűt, ': wq! " toosave fájlt. 

  ```bash
  EXTRACT EXTORA
  USERID C##GGADMIN, PASSWORD ggadmin
  RMTHOST 10.0.0.5, MGRPORT 7809
  RMTTRAIL ./dirdat/rt  
  DDL INCLUDE MAPPED
  DDLOPTIONS REPORT 
  LOGALLSUPCOLS
  UPDATERECORDFORMAT COMPACT
  TABLE pdb1.test.TCUSTMER;
  TABLE pdb1.test.TCUSTORD;
  ```
6. Regisztráció nyerje ki – integrált kivonat:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci

  GGSCI> dblogin userid C##GGADMIN, password ggadmin
  Successfully logged into database CDB$ROOT.

  GGSCI> REGISTER EXTRACT EXTORA DATABASE CONTAINER(pdb1)

  2017-05-23 15:58:34  INFO    OGG-02003  Extract EXTORA successfully registered with database at SCN 1821260.

  GGSCI> exit
  ```
7. Kinyerési ellenőrzőpontok felállítása, és indítsa el a valós idejű kivonat:

  ```bash
  $ ./ggsci
  GGSCI>  ADD EXTRACT EXTORA, INTEGRATED TRANLOG, BEGIN NOW
  EXTRACT (Integrated) added.

  GGSCI>  ADD RMTTRAIL ./dirdat/rt, EXTRACT EXTORA, MEGABYTES 10
  RMTTRAIL added.

  GGSCI>  START EXTRACT EXTORA

  Sending START request tooMANAGER ...
  EXTRACT EXTORA starting

  GGSCI > info all

  Program     Status      Group       Lag at Chkpt  Time Since Chkpt

  MANAGER     RUNNING
  EXTRACT     RUNNING     EXTORA      00:00:11      00:00:04
  ```
Ebben a lépésben található hello Állapotváltozás, később, a másik szakaszban használt indítása:

  ```bash
  $ sqlplus / as sysdba
  SQL> alter session set container = pdb1;
  SQL> SELECT current_scn from v$database;
  CURRENT_SCN
  -----------
      1857887
  SQL> EXIT;
  ```

  ```bash
  $ ./ggsci
  GGSCI> EDIT PARAMS INITEXT
  ```

  ```bash
  EXTRACT INITEXT
  USERID C##GGADMIN, PASSWORD ggadmin
  RMTHOST 10.0.0.5, MGRPORT 7809
  RMTTASK REPLICAT, GROUP INITREP
  TABLE pdb1.test.*, SQLPREDICATE 'AS OF SCN 1857887'; 
  ```

  ```bash
  GGSCI> ADD EXTRACT INITEXT, SOURCEISTABLE
  ```

### <a name="set-up-service-on-myvm2-replicate"></a>A myVM2 szolgáltatás beállítása (replikáció)


1. Hozzon létre vagy hello tnsnames.ora fájl frissítése:

  ```bash
  $ cd $ORACLE_HOME/network/admin
  $ vi tnsnames.ora

  cdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=cdb1)
      )
    )

  pdb1=
    (DESCRIPTION=
      (ADDRESS=
        (PROTOCOL=TCP)
        (HOST=localhost)
        (PORT=1521)
      )
      (CONNECT_DATA=
        (SERVER=dedicated)
        (SERVICE_NAME=pdb1)
      )
    )
  ```

2. Hozzon létre egy párhuzamos fiókot:

  ```bash
  $ sqlplus / as sysdba
  SQL> alter session set container = pdb1;
  SQL> create user repuser identified by rep_pass container=current;
  SQL> grant dba toorepuser;
  SQL> exec dbms_goldengate_auth.grant_admin_privilege('REPUSER',container=>'PDB1');
  SQL> connect repuser/rep_pass@pdb1 
  SQL> EXIT;
  ```

3. Golden kapu teszt felhasználói fiók létrehozása:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ sqlplus system/OraPasswd1@pdb1
  SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
  SQL> GRANT connect, resource, dba tootest;
  SQL> ALTER USER test QUOTA 100M on USERS;
  SQL> connect test/test@pdb1
  SQL> @demo_ora_create
  SQL> EXIT;
  ```

4. REPLICAT paraméter fájl tooreplicate változásai: 

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS REPORA  
  ```
  REPORA paraméter fájl tartalma:

  ```bash
  REPLICAT REPORA
  ASSUMETARGETDEFS
  DISCARDFILE ./dirrpt/repora.dsc, PURGE, MEGABYTES 100
  DDL INCLUDE MAPPED
  DDLOPTIONS REPORT
  DBOPTIONS INTEGRATEDPARAMS(parallelism 6)
  USERID repuser@pdb1, PASSWORD rep_pass
  MAP pdb1.test.*, TARGET pdb1.test.*;
  ```

5. Replicat ellenőrzőpont beállítása:

  ```bash
  GGSCI> ADD REPLICAT REPORA, INTEGRATED, EXTTRAIL ./dirdat/rt
  GGSCI> EDIT PARAMS INITREP

  ```

  ```bash
  REPLICAT INITREP
  ASSUMETARGETDEFS
  DISCARDFILE ./dirrpt/tcustmer.dsc, APPEND
  USERID repuser@pdb1, PASSWORD rep_pass
  MAP pdb1.test.*, TARGET pdb1.test.*;   
  ```

  ```bash
  GGSCI> ADD REPLICAT INITREP, SPECIALRUN
  ```

### <a name="set-up-hello-replication-myvm1-and-myvm2"></a>Hello replikációjának (myVM1 és myVM2)

#### <a name="1-set-up-hello-replication-on-myvm2-replicate"></a>1. A replikáció myVM2 hello beállítása (replikáció)

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS MGR
  ```
Hello fájl frissítése hello alábbira:

  ```bash
  PORT 7809
  ACCESSRULE, PROG *, IPADDR *, ALLOW
  ```
Indítsa újra a hello kezelő szolgáltatás:

  ```bash
  GGSCI> STOP MGR
  GGSCI> START MGR
  GGSCI> EXIT
  ```

#### <a name="2-set-up-hello-replication-on-myvm1-primary"></a>2. (Elsődleges) myVM1 hello a replikáció beállítása

Indítsa el a kezdeti betöltés hello és a hibák:

```bash
$ cd /u01/app/oracle/product/12.1.0/oggcore_1
$ ./ggsci
GGSCI> START EXTRACT INITEXT
GGSCI> VIEW REPORT INITEXT
```
#### <a name="3-set-up-hello-replication-on-myvm2-replicate"></a>3. A replikáció myVM2 hello beállítása (replikáció)

Hello Állapotváltozás számot hello előtt beszerzett módosítása:

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  START REPLICAT REPORA, AFTERCSN 1857887
  ```
hello replikációs megkezdődött, és a teszteléshez le új rekordok tooTEST táblák beszúrva.


### <a name="view-job-status-and-troubleshooting"></a>Feladat állapotának megtekintése és hibaelhárítása

#### <a name="view-reports"></a>Jelentések megtekintése
tooview myVM1, futtassa a következő parancsok hello jelentések:

  ```bash
  GGSCI> VIEW REPORT EXTORA 
  ```
 
tooview myVM2, futtassa a következő parancsok hello jelentések:

  ```bash
  GGSCI> VIEW REPORT REPORA
  ```

#### <a name="view-status-and-history"></a>Nézet állapota és előzményei
tooview állapota és előzményei a myVM1, futtassa a következő parancsok hello:

  ```bash
  GGSCI> dblogin userid c##ggadmin, password ggadmin 
  GGSCI> INFO EXTRACT EXTORA, DETAIL
  ```

tooview állapota és előzményei a myVM2, futtassa a következő parancsok hello:

  ```bash
  GGSCI> dblogin userid repuser@pdb1 password rep_pass 
  GGSCI> INFO REP REPORA, DETAIL
  ```
Ezzel befejezte a hello telepítés és konfigurálás Golden kapu Oracle linux rendszeren.


## <a name="delete-hello-virtual-machine"></a>Hello virtuális gép törlése

Már nincs szükség, ha a következő parancs hello lehet használt tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrásokat.

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Következő lépések

[Magas rendelkezésre állású virtuális gép létrehozása oktatóanyag](../../linux/create-cli-complete.md)

[A virtuális gépek parancssori felületen való üzembe helyezését ismertető minták megismerése](../../linux/cli-samples.md)
