---
title: "egy Azure Linux virtuális gép adatbázis aaaBack mentése és helyreállítása 12c Oracle-adatbázishoz |} Microsoft Docs"
description: "Ismerje meg, hogyan tooback mentése és helyreállítása az Oracle-adatbázis 12c adatbázis az Azure környezetben."
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
ms.date: 5/17/2017
ms.author: rclaus
ms.openlocfilehash: 68846f4efce5eabdb71cd71772e003838154e93b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-and-recover-an-oracle-database-12c-database-on-an-azure-linux-virtual-machine"></a>Készítsen biztonsági másolatot, és az Azure Linux virtuális gép Oracle-adatbázishoz 12c-adatbázis helyreállítása

Használja az Azure parancssori felület toocreate és parancsot a parancssorba az Azure erőforrások kezeléséhez, vagy parancsfájlok használata. Ebben a cikkben az Azure parancssori felület parancsfájlok toodeploy az Oracle-adatbázishoz 12c adatbázis Azure piactér gyűjtemény lemezképből használjuk.

Mielőtt elkezdené, győződjön meg arról, hogy telepítve van-e az Azure parancssori felület. További információkért lásd: hello [Azure parancssori felület telepítési útmutató](https://docs.microsoft.com/cli/azure/install-azure-cli).

## <a name="prepare-hello-environment"></a>Hello környezet előkészítése

### <a name="step-1-prerequisites"></a>1. lépés: Előfeltételek

*   tooperform hello biztonsági mentési és helyreállítási folyamata, akkor először létre kell hoznia egy Linux virtuális Gépet, amely rendelkezik az Oracle-adatbázishoz 12c telepített példánya. VM nevű toocreate hello használata hello Piactéri lemezképhez *Oracle: Oracle-adatbázis-Ee:12.1.0.2:latest*.

    toolearn hogyan toocreate, Oracle-adatbázishoz: hello [Oracle adatbázis gyors üzembe helyezés létrehozása](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).


### <a name="step-2-connect-toohello-vm"></a>2. lépés: Csatlakozás toohello méretű VM

*   a virtuális gép, a következő parancs használata hello hello Secure Shell (SSH) munkamenet toocreate. Hello IP-cím és a hello állomás neve kombinációja lecserélése hello `publicIpAddress` értéket a virtuális gép számára.

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-3-prepare-hello-database"></a>3. lépés: Hello adatbázis előkészítése

1.  Ez a lépés feltételezi, hogy rendelkezik-e egy egy nevű virtuális gépen futó Oracle-példányok (cdb1) *myVM*.

    Futtassa a hello *oracle* felügyelő gyökérszintű, majd inicializálása hello figyelő:

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    Copyright (c) 1991, 2014, Oracle.  All rights reserved.

    Starting /u01/app/oracle/product/12.1.0/dbhome_1/bin/tnslsnr: please wait...

    TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Log messages written too/u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))

    Connecting too(ADDRESS=(PROTOCOL=tcp)(HOST=)(PORT=1521))
    STATUS of hello LISTENER
    ------------------------
    Alias                     LISTENER
    Version                   TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Start Date                23-MAR-2017 15:32:08
    Uptime                    0 days 0 hr. 0 min. 0 sec
    Trace Level               off
    Security                  ON: Local OS Authentication
    SNMP                      OFF
    Listener Log File         /u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening Endpoints Summary...
    (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))
    hello listener supports no services
    hello command completed successfully
    ```

2.  (Választható) Ellenőrizze, hogy hello adatbázis archív napló módban van:

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
    SQL> ALTER SYSTEM SWITCH LOGFILE;
    ```
3.  (Választható) Hozzon létre egy tábla tootest hello véglegesítést:

    ```bash
    SQL> alter session set "_ORACLE_SCRIPT"=true ;
    Session altered.
    SQL> create user scott identified by tiger;
    User created.
    SQL> grant create session tooscott;
    Grant succeeded.
    SQL> grant create table tooscott;
    Grant succeeded.
    SQL> alter user scott quota 100M on users;
    User altered.
    SQL> connect scott/tiger
    SQL> create table scott_table(col1 number, col2 varchar2(50));
    Table created.
    SQL> insert into scott_Table VALUES(1,'Line 1');
    1 row created.
    SQL> commit;
    Commit complete.
    ```
4.  Győződjön meg arról, vagy módosítsa a hello biztonságimásolat-fájl helye és mérete:

    ```bash
    $ sqlplus / as sysdba
    SQL> show parameter db_recovery
    NAME                                 TYPE        VALUE
    ------------------------------------ ----------- ------------------------------
    db_recovery_file_dest                string      /u01/app/oracle/fast_recovery_area
    db_recovery_file_dest_size           big integer 4560M
    ```
5. Hello adatbázis tooback használata Oracle helyreállítás-kezelő (RMAN):

    ```bash
    $ rman target /
    RMAN> backup database plus archivelog;
    ```

### <a name="step-4-application-consistent-backup-for-linux-vms"></a>4. lépés: A Linux virtuális gépek alkalmazáskonzisztens biztonsági mentését

Alkalmazáskonzisztens biztonsági mentést az Azure biztonsági mentés új szolgáltatása. Hozzon létre, és válassza ki a parancsfájlok tooexecute előtt és után hello gép pillanatképével (pillanatkép előtti és utáni pillanatkép).

1. Hello JSON-fájl letöltésére.

    Https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig VMSnapshotScriptPluginConfig.json le. hello fájl tartalmának hasonló toohello következő keresése:

    ```azurecli
    {
        "pluginName" : "ScriptRunner",
        "preScriptLocation" : "",
        "postScriptLocation" : "",
        "preScriptParams" : ["", ""],
        "postScriptParams" : ["", ""],
        "preScriptNoOfRetries" : 0,
        "postScriptNoOfRetries" : 0,
        "timeoutInSeconds" : 30,
        "continueBackupOnFailure" : true,
        "fsFreezeEnabled" : true
    }
    ```

2. Hello /etc/azure mappa létrehozása a virtuális gép hello:

    ```bash
    $ sudo su -
    # mkdir /etc/azure
    # cd /etc/azure
    ```

3. Hello JSON-fájl másolása.

    VMSnapshotScriptPluginConfig.json toohello /etc/azure mappába másolja.

4. Hello JSON-fájl szerkesztése.

    Hello VMSnapshotScriptPluginConfig.json fájl tooinclude hello szerkesztése `PreScriptLocation` és `PostScriptlocation` paraméterek. Példa:

    ```azurecli
    {
        "pluginName" : "ScriptRunner",
        "preScriptLocation" : "/etc/azure/pre_script.sh",
        "postScriptLocation" : "/etc/azure/post_script.sh",
        "preScriptParams" : ["", ""],
        "postScriptParams" : ["", ""],
        "preScriptNoOfRetries" : 0,
        "postScriptNoOfRetries" : 0,
        "timeoutInSeconds" : 30,
        "continueBackupOnFailure" : true,
        "fsFreezeEnabled" : true
    }
    ```

5. Hello pillanatkép előtti és a pillanatkép utáni parancsfájl-fájlok létrehozása.

    Íme egy példa a "cold biztonsági mentéshez" pillanatkép előtti és a pillanatkép utáni parancsfájlok (offline biztonsági másolat, leállítása és újraindítása):

    A /etc/azure/pre_script.sh:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" > /etc/azure/pre_script_$v_date.log
    ```

    A /etc/azure/post_script.sh:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" > /etc/azure/post_script_$v_date.log
    ```

    Íme egy példa a "gyors biztonsági mentéshez" pillanatkép előtti és a pillanatkép utáni parancsfájlok (online biztonsági mentés):

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/pre_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    A /etc/azure/post_script.sh:

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/post_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    A /etc/azure/pre_script.sql módosítsa a hello fájl a követelmények / hello tartalma:

    ```bash
    alter tablespace SYSTEM begin backup;
    ...
    ...
    alter system switch logfile;
    alter system archive log stop;
    ```

    A /etc/azure/post_script.sql módosítsa a hello fájl a követelmények / hello tartalma:

    ```bash
    alter tablespace SYSTEM end backup;
    ...
    ...
    alter system archive log start;
    ```

6. Fájl engedélyeinek módosítása:

    ```bash
    # chmod 600 /etc/azure/VMSnapshotScriptPluginConfig.json
    # chmod 700 /etc/azure/pre_script.sh
    # chmod 700 /etc/azure/post_script.sh
    ```

7. Hello parancsfájlok tesztelése.

    tootest hello parancsfájlok, először jelentkezzen be rendszergazdaként. Majd győződjön meg arról, hogy nincsenek-e hibák:

    ```bash
    # /etc/azure/pre_script.sh
    # /etc/azure/post_script.sh
    ```

További információkért lásd: [Linux virtuális gépek alkalmazáskonzisztens biztonsági mentését](https://azure.microsoft.com/en-us/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).


### <a name="step-5-use-azure-recovery-services-vaults-tooback-up-hello-vm"></a>5. lépés: Használata Azure Recovery Services-tárolók tooback hello virtuális gép mentése

1.  Az Azure-portálon hello, keressen **Recovery Services-tárolók**.

    ![Helyreállítási szolgáltatások tárolók lap](./media/oracle-backup-recovery/recovery_service_01.png)

2.  A hello **Recovery Services-tárolók** tooadd egy új tárolót panelen kattintson a **Hozzáadás**.

    ![Helyreállítási szolgáltatások tárolók-weblap hozzáadása](./media/oracle-backup-recovery/recovery_service_02.png)

3.  toocontinue, kattintson a **myVault**.

    ![Helyreállítási szolgáltatások tárolók részletességi lap](./media/oracle-backup-recovery/recovery_service_03.png)

4.  A hello **myVault** panelen kattintson a **biztonsági mentés**.

    ![Helyreállítási szolgáltatások tárolók lap biztonsági mentése](./media/oracle-backup-recovery/recovery_service_04.png)

5.  A hello **biztonsági mentési cél** panelen hello az alapértelmezett értékek használata **Azure** és **virtuális gép**. Kattintson az **OK** gombra.

    ![Helyreállítási szolgáltatások tárolók részletességi lap](./media/oracle-backup-recovery/recovery_service_05.png)

6.  A **biztonsági mentési házirend**, használjon **DefaultPolicy**, vagy válasszon **hozzon létre új házirend**. Kattintson az **OK** gombra.

    ![Helyreállítási szolgáltatások tárolók biztonsági mentési házirend Részletek lap](./media/oracle-backup-recovery/recovery_service_06.png)

7.  A hello **válassza ki a virtuális gépek** panelen, jelölje be hello **myVM1** jelölőnégyzetet, majd kattintson a **OK**. Kattintson a hello **biztonságimásolat-készítés engedélyezése** gombra.

    ![Helyreállítási szolgáltatások tárolók elemek toohello biztonsági mentési Részletek lap](./media/oracle-backup-recovery/recovery_service_07.png)

    > [!IMPORTANT]
    > Miután rákattintott **biztonságimásolat-készítés engedélyezése**, hello biztonsági mentési folyamat nem indul el, amíg hello ütemezett időpontban lejár. egy azonnali, teljes biztonsági mentést tooset hello a következő lépésre.

8.  A hello **myVault - biztonsági mentés elemek** panelen, a **biztonsági MENTÉSI ELEMSZÁMNÁL**, válassza ki a biztonsági mentési elemszámnál hello.

    ![Helyreállítási szolgáltatások tárolók myVault Részletek lap](./media/oracle-backup-recovery/recovery_service_08.png)

9.  A hello **(Azure virtuális gép) biztonsági mentés elemek** panelen hello jobb oldalán található hello, kattintson a hello három pont (**...** ) gombra, majd **biztonsági mentés most**.

    ![Helyreállítási szolgáltatások tárolók biztonsági mentés most parancsot.](./media/oracle-backup-recovery/recovery_service_09.png)

10. Kattintson a hello **biztonsági mentés** gombra. Várjon, amíg hello biztonsági mentési folyamat toofinish. Ezután térjen túl[6. lépés: távolítsa el a hello adatbázisfájlok](#step-6-remove-the-database-files).

    tooview hello állapotát hello biztonsági mentési feladat, kattintson az **feladatok**.

    ![Helyreállítási szolgáltatások tárolók feladat lap](./media/oracle-backup-recovery/recovery_service_10.png)

    hello hello biztonsági mentési feladat állapota a következő kép hello jelenik meg:

    ![Helyreállítási szolgáltatások tárolók feladat állapotú lap](./media/oracle-backup-recovery/recovery_service_11.png)

11. Az alkalmazáskonzisztens biztonsági mentését címek hello naplófájl-e hibák. hello naplófájl /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0 helyezkedik el.

### <a name="step-6-remove-hello-database-files"></a>6. lépés: Távolítsa el a hello adatbázisfájlok 
A cikk későbbi részében megtudhatja, hogyan tootest hello helyreállítási folyamatot. Mielőtt hello helyreállítási folyamat teszteléséhez tooremove hello adatbázisfájlok rendelkezik.

1.  Hello táblaterületen és a biztonsági mentés fájljának eltávolítása:

    ```bash
    $ sudo su - oracle
    $ cd /u01/app/oracle/oradata/cdb1
    $ rm -f *.dbf
    $ cd /u01/app/oracle/fast_recovery_area/CDB1/backupset
    $ rm -rf *
    ```
    
2.  (Választható) Állítsa le a hello Oracle-példány:

    ```bash
    $ sqlplus / as sysdba
    SQL> shutdown abort
    ORACLE instance shut down.
    ```

## <a name="restore-hello-deleted-files-from-hello-recovery-services-vaults"></a>Recovery Services-tárolók hello hello törölt fájlok visszaállítása
toorestore hello törölt fájlok, teljes hello a következő lépéseket:

1. A hello Azure-portálon, keresse meg a hello *myVault* Recovery Services-tárolók elemet. A hello **áttekintése** panelen, a **elem biztonsági mentését**, válassza ki a hello elemek száma.

    ![Helyreállítási szolgáltatások tárolók myVault biztonsági másolati elemei](./media/oracle-backup-recovery/recovery_service_12.png)

2. A **biztonsági MENTÉS elemek száma**, válassza ki a hello elemek száma.

    ![Recovery Services-tárolók Azure virtuális gép biztonsági mentési elemek száma](./media/oracle-backup-recovery/recovery_service_13.png)

3. A hello **myvm1** panelen kattintson a **fájlhelyreállítás (előzetes verzió)**.

    ![Képernyőfelvétel a hello Recovery Services-tárolók helyreállítási lapja](./media/oracle-backup-recovery/recovery_service_14.png)

4. A hello **fájlhelyreállítás (előzetes verzió)** ablaktáblán kattintson a **parancsfájl letöltése**. Ezt követően mentse hello letöltési (.sh) fájl tooa mappában hello ügyfélszámítógépen.

    ![Töltse le a parancsfájl mentése beállításai](./media/oracle-backup-recovery/recovery_service_15.png)

5. Hello .sh fájl toohello VM másolja.

    hello a következő példa bemutatja, hogyan egy biztonságos másolása (scp) parancs toomove toouse hello fájl toohello virtuális gép. Is másolhatja hello tartalma toohello vágólapra, majd a Beillesztés hello tartalmát egy új fájlba, a virtuális gép hello be van állítva.

    > [!IMPORTANT]
    > A következő példa hello győződjön meg arról, hogy a hello IP cím és a mappa értékek frissíteni. hello értékek hello fájl mentési helyének toohello mappát kell társítani.

    ```bash
    $ scp Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh <publicIpAddress>:/<folder>
    ```
6. Hello fájl módosítása a hello legfelső szintű tulajdonában van.

    A következő példa hello módosítsa hello fájlt úgy, hogy a tulajdonosa hello legfelső szintű. Ezután módosítsa az engedélyeket.

    ```bash 
    $ ssh <publicIpAddress>
    $ sudo su -
    # chown root:root /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # chmod 755 /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    ```
    hello következő példát talál parancsfájlt hello futtatása után. Amikor felszólítja toocontinue, adja meg **Y**.

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
    hello script requires 'open-iscsi' and 'lshw' toorun.
    Do you want us tooinstall 'open-iscsi' and 'lshw' on this machine?
    Please press 'Y' toocontinue with installation, 'N' tooabort hello operation. : Y
    Installing 'open-iscsi'....
    Installing 'lshw'....

    Connecting toorecovery point using ISCSI service...

    Connection succeeded!

    Please wait while we attach volumes of hello recovery point toothis machine...

    ************ Volumes of hello recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath

    1)  | /dev/sde  |  /dev/sde1  |  /root/myVM-20170517093913/Volume1

    2)  | /dev/sde  |  /dev/sde2  |  /root/myVM-20170517093913/Volume2

    ************ Open File Explorer toobrowse for files. ************

    After recovery, tooremove hello disks and close hello connection toohello recovery point, please click 'Unmount Disks' in step 3 of hello portal.

    Please enter 'q/Q' tooexit...
    ```

7. Hozzáférés toohello csatlakoztatott kötetek ellenőrzése.

    tooexit, adja meg **q**, majd keresse meg a hello csatlakoztatott kötetek. hello listája hozzáadott kötetek, a parancssorba írja be toocreate **df -k**.

    ![hello df -k parancs](./media/oracle-backup-recovery/recovery_service_16.png)

8. A következő parancsfájl toocopy hello hiányzó használata hello fájlok hátsó toohello mappák:

    ```bash
    # cd /root/myVM-2017XXXXXXX/Volume2/u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # cp *.bkp /u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # cd /u01/app/oracle/fast_recovery_area/CDB1/backupset/2017_xx_xx
    # chown oracle:oinstall *.bkp
    # cd /root/myVM-2017XXXXXXX/Volume2/u01/app/oracle/oradata/cdb1
    # cp *.dbf /u01/app/oracle/oradata/cdb1
    # cd /u01/app/oracle/oradata/cdb1
    # chown oracle:oinstall *.dbf
    ```
9. A következő parancsfájl hello RMAN toorecover hello adatbázis használata:

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```
    
10. Hello lemez leválasztása.

    Az Azure portál, a hello hello **fájlhelyreállítás (előzetes verzió)** panelen kattintson a **lemez leválasztása**.

    ![Válassza le a lemezek parancs](./media/oracle-backup-recovery/recovery_service_17.png)

## <a name="restore-hello-entire-vm"></a>Visszaállítás hello teljes méretű VM

Helyett a Recovery Services-tárolók hello hello törölt fájlok visszaállítása során, hogy visszaállíthassa a teljes virtuális gép hello.

### <a name="step-1-delete-myvm"></a>1. lépés: Delete myVM

*   A hello Azure-portálon, válassza a toohello **myVM1** tároló, és válassza ki **törlése**.

    ![Tároló delete parancs esetében](./media/oracle-backup-recovery/recover_vm_01.png)

### <a name="step-2-recover-hello-vm"></a>2. lépés: Hello virtuális gép helyreállítása

1.  Nyissa meg túl**Recovery Services-tárolók**, majd válassza ki **myVault**.

    ![myVault bejegyzés](./media/oracle-backup-recovery/recover_vm_02.png)

2.  A hello **áttekintése** panelen, a **elem biztonsági mentését**, válassza ki a hello elemek száma.

    ![Készítsen biztonsági másolatot elemek myVault](./media/oracle-backup-recovery/recover_vm_03.png)

3.  A hello **(Azure virtuális gép) biztonsági mentés elemek** panelen válassza **myvm1**.

    ![A helyreállítási virtuális gép lap](./media/oracle-backup-recovery/recover_vm_04.png)

4.  A hello **myvm1** paneljén kattintson hello három pont (**...** ) gombra, majd **visszaállítani a virtuális gép**.

    ![A restore parancs méretű VM](./media/oracle-backup-recovery/recover_vm_05.png)

5.  A hello **visszaállítási pont kiválasztása** panelen, jelölje be hello elem toorestore szeretne, és kattintson **OK**.

    ![Jelölje be hello visszaállítási pont](./media/oracle-backup-recovery/recover_vm_06.png)

    Ha engedélyezte az alkalmazáskonzisztens biztonsági mentését, egy függőleges kék színű sáv jelenik meg.

6.  A hello **konfiguráció visszaállítása** panelen, jelölje be hello virtuális gép nevét, jelölje ki a hello erőforráscsoport, és kattintson **OK**.

    ![Állítsa vissza a konfigurációs értékek](./media/oracle-backup-recovery/recover_vm_07.png)

7.  toorestore hello VM, kattintson a hello **visszaállítása** gombra.

8.  tooview hello állapotát hello visszaállítási folyamat, kattintson az **feladatok**, és kattintson a **biztonsági mentési feladatok**.

    ![Biztonsági mentési feladatok állapota parancs](./media/oracle-backup-recovery/recover_vm_08.png)

    hello következő ábrán látható hello visszaállítási folyamat hello állapotát:

    ![Hello visszaállítási folyamat állapota](./media/oracle-backup-recovery/recover_vm_09.png)

### <a name="step-3-set-hello-public-ip-address"></a>3. lépés: Hello nyilvános IP-cím beállítása
Hello VM helyreáll, után állítsa be hello nyilvános IP-cím.

1.  Hello keresési mezőbe, írja be a **nyilvános IP-cím**.

    ![Nyilvános IP-címek listája](./media/oracle-backup-recovery/create_ip_00.png)

2.  A hello **nyilvános IP-címek** panelen kattintson a **Hozzáadás**. A hello **nyilvános IP-cím létrehozása** panelen a **neve**, jelölje be hello nyilvános IP-név. Az **Erőforráscsoport** területen válassza a **Meglévő használata** lehetőséget. Ezt követően kattintson a **Create** (Létrehozás) gombra.

    ![IP-cím létrehozása](./media/oracle-backup-recovery/create_ip_01.png)

3.  tooassociate hello nyilvános IP-cím hello hálózati csatoló a virtuális gép, hello keresse és jelölje be a **myVMip**. Kattintson a **társítása**.

    ![IP-cím hozzárendelése](./media/oracle-backup-recovery/create_ip_02.png)

4.  A **erőforrástípus**, jelölje be **hálózati illesztő**. Jelölje be, amely hello myVM példány által használt hello hálózati adaptert, és kattintson **OK**.

    ![Válassza ki az erőforrás típusa és a hálózati adapter értékek](./media/oracle-backup-recovery/create_ip_03.png)

5.  Keresse meg, és nyissa meg a hello példánya, amely van legelterjedtebb myVM hello portálról. hello hello VM társított IP-cím jelenik meg hello myVM **áttekintése** panelen.

    ![IP-cím értéke](./media/oracle-backup-recovery/create_ip_04.png)

### <a name="step-4-connect-toohello-vm"></a>4. lépés: Csatlakozás toohello méretű VM

*   tooconnect toohello VM, a következő parancsfájl hello használata:

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-5-test-whether-hello-database-is-accessible"></a>5. lépés: Hello adatbázis elérhető-e tesztelése
*   tootest kisegítő, a következő parancsfájl használata hello:

    ```bash 
    $ sudo su - oracle
    $ sqlplus / as sysdba
    SQL> startup
    ```

    > [!IMPORTANT]
    > Ha hello adatbázis **indítási** parancs létrehoz egy hiba, toorecover hello adatbázis című [6. lépés: használja RMAN toorecover hello adatbázis](#step-6-optional-use-rman-to-recover-the-database).

### <a name="step-6-optional-use-rman-toorecover-hello-database"></a>6. lépés: (Nem kötelező) használata RMAN toorecover hello adatbázis
*   toorecover hello adatbázis, a következő parancsfájl használata hello:

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```

hello biztonsági mentési és helyreállítási hello Oracle-adatbázishoz 12c adatbázis az Azure Linux virtuális gép most már befejeződött.

## <a name="delete-hello-vm"></a>Hello virtuális gép törlése

Ha a virtuális gép már nem kell hello, használhatja a következő parancs tooremove hello erőforráscsoport, hello VM és minden kapcsolódó erőforrások hello:

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a>Következő lépések

[Oktatóanyag: Hozzon létre magas rendelkezésre állású virtuális gépek](../../linux/create-cli-complete.md)

[Virtuális gép telepítése az Azure parancssori felület minták felfedezése](../../linux/cli-samples.md)



