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
# <a name="back-up-and-recover-an-oracle-database-12c-database-on-an-azure-linux-virtual-machine"></a><span data-ttu-id="6bde5-103">Készítsen biztonsági másolatot, és az Azure Linux virtuális gép Oracle-adatbázishoz 12c-adatbázis helyreállítása</span><span class="sxs-lookup"><span data-stu-id="6bde5-103">Back up and recover an Oracle Database 12c database on an Azure Linux virtual machine</span></span>

<span data-ttu-id="6bde5-104">Használja az Azure parancssori felület toocreate és parancsot a parancssorba az Azure erőforrások kezeléséhez, vagy parancsfájlok használata.</span><span class="sxs-lookup"><span data-stu-id="6bde5-104">You can use Azure CLI toocreate and manage Azure resources at a command prompt, or use scripts.</span></span> <span data-ttu-id="6bde5-105">Ebben a cikkben az Azure parancssori felület parancsfájlok toodeploy az Oracle-adatbázishoz 12c adatbázis Azure piactér gyűjtemény lemezképből használjuk.</span><span class="sxs-lookup"><span data-stu-id="6bde5-105">In this article, we use Azure CLI scripts toodeploy an Oracle Database 12c database from an Azure Marketplace gallery image.</span></span>

<span data-ttu-id="6bde5-106">Mielőtt elkezdené, győződjön meg arról, hogy telepítve van-e az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="6bde5-106">Before you begin, make sure that Azure CLI is installed.</span></span> <span data-ttu-id="6bde5-107">További információkért lásd: hello [Azure parancssori felület telepítési útmutató](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="6bde5-107">For more information, see hello [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-hello-environment"></a><span data-ttu-id="6bde5-108">Hello környezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="6bde5-108">Prepare hello environment</span></span>

### <a name="step-1-prerequisites"></a><span data-ttu-id="6bde5-109">1. lépés: Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6bde5-109">Step 1: Prerequisites</span></span>

*   <span data-ttu-id="6bde5-110">tooperform hello biztonsági mentési és helyreállítási folyamata, akkor először létre kell hoznia egy Linux virtuális Gépet, amely rendelkezik az Oracle-adatbázishoz 12c telepített példánya.</span><span class="sxs-lookup"><span data-stu-id="6bde5-110">tooperform hello backup and recovery process, you must first create a Linux VM that has an installed instance of Oracle Database 12c.</span></span> <span data-ttu-id="6bde5-111">VM nevű toocreate hello használata hello Piactéri lemezképhez *Oracle: Oracle-adatbázis-Ee:12.1.0.2:latest*.</span><span class="sxs-lookup"><span data-stu-id="6bde5-111">hello Marketplace image you use toocreate hello VM is named *Oracle:Oracle-Database-Ee:12.1.0.2:latest*.</span></span>

    <span data-ttu-id="6bde5-112">toolearn hogyan toocreate, Oracle-adatbázishoz: hello [Oracle adatbázis gyors üzembe helyezés létrehozása](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).</span><span class="sxs-lookup"><span data-stu-id="6bde5-112">toolearn how toocreate an Oracle database, see hello [Oracle create database quickstart](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).</span></span>


### <a name="step-2-connect-toohello-vm"></a><span data-ttu-id="6bde5-113">2. lépés: Csatlakozás toohello méretű VM</span><span class="sxs-lookup"><span data-stu-id="6bde5-113">Step 2: Connect toohello VM</span></span>

*   <span data-ttu-id="6bde5-114">a virtuális gép, a következő parancs használata hello hello Secure Shell (SSH) munkamenet toocreate.</span><span class="sxs-lookup"><span data-stu-id="6bde5-114">toocreate a Secure Shell (SSH) session with hello VM, use hello following command.</span></span> <span data-ttu-id="6bde5-115">Hello IP-cím és a hello állomás neve kombinációja lecserélése hello `publicIpAddress` értéket a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="6bde5-115">Replace hello IP address and hello host name combination with hello `publicIpAddress` value for your VM.</span></span>

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-3-prepare-hello-database"></a><span data-ttu-id="6bde5-116">3. lépés: Hello adatbázis előkészítése</span><span class="sxs-lookup"><span data-stu-id="6bde5-116">Step 3: Prepare hello database</span></span>

1.  <span data-ttu-id="6bde5-117">Ez a lépés feltételezi, hogy rendelkezik-e egy egy nevű virtuális gépen futó Oracle-példányok (cdb1) *myVM*.</span><span class="sxs-lookup"><span data-stu-id="6bde5-117">This step assumes that you have an Oracle instance (cdb1) that is running on a VM named *myVM*.</span></span>

    <span data-ttu-id="6bde5-118">Futtassa a hello *oracle* felügyelő gyökérszintű, majd inicializálása hello figyelő:</span><span class="sxs-lookup"><span data-stu-id="6bde5-118">Run hello *oracle* superuser root, and then initialize hello listener:</span></span>

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

2.  <span data-ttu-id="6bde5-119">(Választható) Ellenőrizze, hogy hello adatbázis archív napló módban van:</span><span class="sxs-lookup"><span data-stu-id="6bde5-119">(Optional) Make sure hello database is in archive log mode:</span></span>

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
3.  <span data-ttu-id="6bde5-120">(Választható) Hozzon létre egy tábla tootest hello véglegesítést:</span><span class="sxs-lookup"><span data-stu-id="6bde5-120">(Optional) Create a table tootest hello commit:</span></span>

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
4.  <span data-ttu-id="6bde5-121">Győződjön meg arról, vagy módosítsa a hello biztonságimásolat-fájl helye és mérete:</span><span class="sxs-lookup"><span data-stu-id="6bde5-121">Verify or change hello backup file location and size:</span></span>

    ```bash
    $ sqlplus / as sysdba
    SQL> show parameter db_recovery
    NAME                                 TYPE        VALUE
    ------------------------------------ ----------- ------------------------------
    db_recovery_file_dest                string      /u01/app/oracle/fast_recovery_area
    db_recovery_file_dest_size           big integer 4560M
    ```
5. <span data-ttu-id="6bde5-122">Hello adatbázis tooback használata Oracle helyreállítás-kezelő (RMAN):</span><span class="sxs-lookup"><span data-stu-id="6bde5-122">Use Oracle Recovery Manager (RMAN) tooback up hello database:</span></span>

    ```bash
    $ rman target /
    RMAN> backup database plus archivelog;
    ```

### <a name="step-4-application-consistent-backup-for-linux-vms"></a><span data-ttu-id="6bde5-123">4. lépés: A Linux virtuális gépek alkalmazáskonzisztens biztonsági mentését</span><span class="sxs-lookup"><span data-stu-id="6bde5-123">Step 4: Application-consistent backup for Linux VMs</span></span>

<span data-ttu-id="6bde5-124">Alkalmazáskonzisztens biztonsági mentést az Azure biztonsági mentés új szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="6bde5-124">Application-consistent backups is a new feature in Azure Backup.</span></span> <span data-ttu-id="6bde5-125">Hozzon létre, és válassza ki a parancsfájlok tooexecute előtt és után hello gép pillanatképével (pillanatkép előtti és utáni pillanatkép).</span><span class="sxs-lookup"><span data-stu-id="6bde5-125">You can create and select scripts tooexecute before and after hello VM snapshot (pre-snapshot and post-snapshot).</span></span>

1. <span data-ttu-id="6bde5-126">Hello JSON-fájl letöltésére.</span><span class="sxs-lookup"><span data-stu-id="6bde5-126">Download hello JSON file.</span></span>

    <span data-ttu-id="6bde5-127">Https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig VMSnapshotScriptPluginConfig.json le.</span><span class="sxs-lookup"><span data-stu-id="6bde5-127">Download VMSnapshotScriptPluginConfig.json from https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig.</span></span> <span data-ttu-id="6bde5-128">hello fájl tartalmának hasonló toohello következő keresése:</span><span class="sxs-lookup"><span data-stu-id="6bde5-128">hello file contents look similar toohello following:</span></span>

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

2. <span data-ttu-id="6bde5-129">Hello /etc/azure mappa létrehozása a virtuális gép hello:</span><span class="sxs-lookup"><span data-stu-id="6bde5-129">Create hello /etc/azure folder on hello VM:</span></span>

    ```bash
    $ sudo su -
    # mkdir /etc/azure
    # cd /etc/azure
    ```

3. <span data-ttu-id="6bde5-130">Hello JSON-fájl másolása.</span><span class="sxs-lookup"><span data-stu-id="6bde5-130">Copy hello JSON file.</span></span>

    <span data-ttu-id="6bde5-131">VMSnapshotScriptPluginConfig.json toohello /etc/azure mappába másolja.</span><span class="sxs-lookup"><span data-stu-id="6bde5-131">Copy VMSnapshotScriptPluginConfig.json toohello /etc/azure folder.</span></span>

4. <span data-ttu-id="6bde5-132">Hello JSON-fájl szerkesztése.</span><span class="sxs-lookup"><span data-stu-id="6bde5-132">Edit hello JSON file.</span></span>

    <span data-ttu-id="6bde5-133">Hello VMSnapshotScriptPluginConfig.json fájl tooinclude hello szerkesztése `PreScriptLocation` és `PostScriptlocation` paraméterek.</span><span class="sxs-lookup"><span data-stu-id="6bde5-133">Edit hello VMSnapshotScriptPluginConfig.json file tooinclude hello `PreScriptLocation` and `PostScriptlocation` parameters.</span></span> <span data-ttu-id="6bde5-134">Példa:</span><span class="sxs-lookup"><span data-stu-id="6bde5-134">For example:</span></span>

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

5. <span data-ttu-id="6bde5-135">Hello pillanatkép előtti és a pillanatkép utáni parancsfájl-fájlok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6bde5-135">Create hello pre-snapshot and post-snapshot script files.</span></span>

    <span data-ttu-id="6bde5-136">Íme egy példa a "cold biztonsági mentéshez" pillanatkép előtti és a pillanatkép utáni parancsfájlok (offline biztonsági másolat, leállítása és újraindítása):</span><span class="sxs-lookup"><span data-stu-id="6bde5-136">Here's an example of pre-snapshot and post-snapshot scripts for a "cold backup" (an offline backup, with shutdown and restart):</span></span>

    <span data-ttu-id="6bde5-137">A /etc/azure/pre_script.sh:</span><span class="sxs-lookup"><span data-stu-id="6bde5-137">For /etc/azure/pre_script.sh:</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" > /etc/azure/pre_script_$v_date.log
    ```

    <span data-ttu-id="6bde5-138">A /etc/azure/post_script.sh:</span><span class="sxs-lookup"><span data-stu-id="6bde5-138">For /etc/azure/post_script.sh:</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" > /etc/azure/post_script_$v_date.log
    ```

    <span data-ttu-id="6bde5-139">Íme egy példa a "gyors biztonsági mentéshez" pillanatkép előtti és a pillanatkép utáni parancsfájlok (online biztonsági mentés):</span><span class="sxs-lookup"><span data-stu-id="6bde5-139">Here's an example of pre-snapshot and post-snapshot scripts for a "hot backup" (an online backup):</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/pre_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    <span data-ttu-id="6bde5-140">A /etc/azure/post_script.sh:</span><span class="sxs-lookup"><span data-stu-id="6bde5-140">For /etc/azure/post_script.sh:</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/post_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    <span data-ttu-id="6bde5-141">A /etc/azure/pre_script.sql módosítsa a hello fájl a követelmények / hello tartalma:</span><span class="sxs-lookup"><span data-stu-id="6bde5-141">For /etc/azure/pre_script.sql, modify hello contents of hello file per your requirements:</span></span>

    ```bash
    alter tablespace SYSTEM begin backup;
    ...
    ...
    alter system switch logfile;
    alter system archive log stop;
    ```

    <span data-ttu-id="6bde5-142">A /etc/azure/post_script.sql módosítsa a hello fájl a követelmények / hello tartalma:</span><span class="sxs-lookup"><span data-stu-id="6bde5-142">For /etc/azure/post_script.sql, modify hello contents of hello file per your requirements:</span></span>

    ```bash
    alter tablespace SYSTEM end backup;
    ...
    ...
    alter system archive log start;
    ```

6. <span data-ttu-id="6bde5-143">Fájl engedélyeinek módosítása:</span><span class="sxs-lookup"><span data-stu-id="6bde5-143">Change file permissions:</span></span>

    ```bash
    # chmod 600 /etc/azure/VMSnapshotScriptPluginConfig.json
    # chmod 700 /etc/azure/pre_script.sh
    # chmod 700 /etc/azure/post_script.sh
    ```

7. <span data-ttu-id="6bde5-144">Hello parancsfájlok tesztelése.</span><span class="sxs-lookup"><span data-stu-id="6bde5-144">Test hello scripts.</span></span>

    <span data-ttu-id="6bde5-145">tootest hello parancsfájlok, először jelentkezzen be rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="6bde5-145">tootest hello scripts, first, sign in as root.</span></span> <span data-ttu-id="6bde5-146">Majd győződjön meg arról, hogy nincsenek-e hibák:</span><span class="sxs-lookup"><span data-stu-id="6bde5-146">Then, ensure that there are no errors:</span></span>

    ```bash
    # /etc/azure/pre_script.sh
    # /etc/azure/post_script.sh
    ```

<span data-ttu-id="6bde5-147">További információkért lásd: [Linux virtuális gépek alkalmazáskonzisztens biztonsági mentését](https://azure.microsoft.com/en-us/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).</span><span class="sxs-lookup"><span data-stu-id="6bde5-147">For more information, see [Application-consistent backup for Linux VMs](https://azure.microsoft.com/en-us/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).</span></span>


### <a name="step-5-use-azure-recovery-services-vaults-tooback-up-hello-vm"></a><span data-ttu-id="6bde5-148">5. lépés: Használata Azure Recovery Services-tárolók tooback hello virtuális gép mentése</span><span class="sxs-lookup"><span data-stu-id="6bde5-148">Step 5: Use Azure Recovery Services vaults tooback up hello VM</span></span>

1.  <span data-ttu-id="6bde5-149">Az Azure-portálon hello, keressen **Recovery Services-tárolók**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-149">In hello Azure portal, search for **Recovery Services vaults**.</span></span>

    ![Helyreállítási szolgáltatások tárolók lap](./media/oracle-backup-recovery/recovery_service_01.png)

2.  <span data-ttu-id="6bde5-151">A hello **Recovery Services-tárolók** tooadd egy új tárolót panelen kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-151">On hello **Recovery Services vaults** blade, tooadd a new vault, click **Add**.</span></span>

    ![Helyreállítási szolgáltatások tárolók-weblap hozzáadása](./media/oracle-backup-recovery/recovery_service_02.png)

3.  <span data-ttu-id="6bde5-153">toocontinue, kattintson a **myVault**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-153">toocontinue, click **myVault**.</span></span>

    ![Helyreállítási szolgáltatások tárolók részletességi lap](./media/oracle-backup-recovery/recovery_service_03.png)

4.  <span data-ttu-id="6bde5-155">A hello **myVault** panelen kattintson a **biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-155">On hello **myVault** blade, click **Backup**.</span></span>

    ![Helyreállítási szolgáltatások tárolók lap biztonsági mentése](./media/oracle-backup-recovery/recovery_service_04.png)

5.  <span data-ttu-id="6bde5-157">A hello **biztonsági mentési cél** panelen hello az alapértelmezett értékek használata **Azure** és **virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-157">On hello **Backup Goal** blade, use hello default values of **Azure** and **Virtual machine**.</span></span> <span data-ttu-id="6bde5-158">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="6bde5-158">Click **OK**.</span></span>

    ![Helyreállítási szolgáltatások tárolók részletességi lap](./media/oracle-backup-recovery/recovery_service_05.png)

6.  <span data-ttu-id="6bde5-160">A **biztonsági mentési házirend**, használjon **DefaultPolicy**, vagy válasszon **hozzon létre új házirend**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-160">For **Backup policy**, use **DefaultPolicy**, or select **Create New policy**.</span></span> <span data-ttu-id="6bde5-161">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="6bde5-161">Click **OK**.</span></span>

    ![Helyreállítási szolgáltatások tárolók biztonsági mentési házirend Részletek lap](./media/oracle-backup-recovery/recovery_service_06.png)

7.  <span data-ttu-id="6bde5-163">A hello **válassza ki a virtuális gépek** panelen, jelölje be hello **myVM1** jelölőnégyzetet, majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-163">On hello **Select virtual machines** blade, select hello **myVM1** check box, and then click **OK**.</span></span> <span data-ttu-id="6bde5-164">Kattintson a hello **biztonságimásolat-készítés engedélyezése** gombra.</span><span class="sxs-lookup"><span data-stu-id="6bde5-164">Click hello **Enable backup** button.</span></span>

    ![Helyreállítási szolgáltatások tárolók elemek toohello biztonsági mentési Részletek lap](./media/oracle-backup-recovery/recovery_service_07.png)

    > [!IMPORTANT]
    > <span data-ttu-id="6bde5-166">Miután rákattintott **biztonságimásolat-készítés engedélyezése**, hello biztonsági mentési folyamat nem indul el, amíg hello ütemezett időpontban lejár.</span><span class="sxs-lookup"><span data-stu-id="6bde5-166">After you click **Enable backup**, hello backup process doesn't start until hello scheduled time expires.</span></span> <span data-ttu-id="6bde5-167">egy azonnali, teljes biztonsági mentést tooset hello a következő lépésre.</span><span class="sxs-lookup"><span data-stu-id="6bde5-167">tooset up an immediate backup, complete hello next step.</span></span>

8.  <span data-ttu-id="6bde5-168">A hello **myVault - biztonsági mentés elemek** panelen, a **biztonsági MENTÉSI ELEMSZÁMNÁL**, válassza ki a biztonsági mentési elemszámnál hello.</span><span class="sxs-lookup"><span data-stu-id="6bde5-168">On hello **myVault - Backup items** blade, under **BACKUP ITEM COUNT**, select hello backup item count.</span></span>

    ![Helyreállítási szolgáltatások tárolók myVault Részletek lap](./media/oracle-backup-recovery/recovery_service_08.png)

9.  <span data-ttu-id="6bde5-170">A hello **(Azure virtuális gép) biztonsági mentés elemek** panelen hello jobb oldalán található hello, kattintson a hello három pont (**...** ) gombra, majd **biztonsági mentés most**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-170">On hello **Backup Items (Azure Virtual Machine)** blade, on hello right side of hello page, click hello ellipsis (**...**) button, and then click **Backup now**.</span></span>

    ![Helyreállítási szolgáltatások tárolók biztonsági mentés most parancsot.](./media/oracle-backup-recovery/recovery_service_09.png)

10. <span data-ttu-id="6bde5-172">Kattintson a hello **biztonsági mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="6bde5-172">Click hello **Backup** button.</span></span> <span data-ttu-id="6bde5-173">Várjon, amíg hello biztonsági mentési folyamat toofinish.</span><span class="sxs-lookup"><span data-stu-id="6bde5-173">Wait for hello backup process toofinish.</span></span> <span data-ttu-id="6bde5-174">Ezután térjen túl[6. lépés: távolítsa el a hello adatbázisfájlok](#step-6-remove-the-database-files).</span><span class="sxs-lookup"><span data-stu-id="6bde5-174">Then, go too[Step 6: Remove hello database files](#step-6-remove-the-database-files).</span></span>

    <span data-ttu-id="6bde5-175">tooview hello állapotát hello biztonsági mentési feladat, kattintson az **feladatok**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-175">tooview hello status of hello backup job, click **Jobs**.</span></span>

    ![Helyreállítási szolgáltatások tárolók feladat lap](./media/oracle-backup-recovery/recovery_service_10.png)

    <span data-ttu-id="6bde5-177">hello hello biztonsági mentési feladat állapota a következő kép hello jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="6bde5-177">hello status of hello backup job appears in hello following image:</span></span>

    ![Helyreállítási szolgáltatások tárolók feladat állapotú lap](./media/oracle-backup-recovery/recovery_service_11.png)

11. <span data-ttu-id="6bde5-179">Az alkalmazáskonzisztens biztonsági mentését címek hello naplófájl-e hibák.</span><span class="sxs-lookup"><span data-stu-id="6bde5-179">For an application-consistent backup, address any errors in hello log file.</span></span> <span data-ttu-id="6bde5-180">hello naplófájl /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0 helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="6bde5-180">hello log file is located at /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0.</span></span>

### <a name="step-6-remove-hello-database-files"></a><span data-ttu-id="6bde5-181">6. lépés: Távolítsa el a hello adatbázisfájlok</span><span class="sxs-lookup"><span data-stu-id="6bde5-181">Step 6: Remove hello database files</span></span> 
<span data-ttu-id="6bde5-182">A cikk későbbi részében megtudhatja, hogyan tootest hello helyreállítási folyamatot.</span><span class="sxs-lookup"><span data-stu-id="6bde5-182">Later in this article, you'll learn how tootest hello recovery process.</span></span> <span data-ttu-id="6bde5-183">Mielőtt hello helyreállítási folyamat teszteléséhez tooremove hello adatbázisfájlok rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="6bde5-183">Before you can test hello recovery process, you have tooremove hello database files.</span></span>

1.  <span data-ttu-id="6bde5-184">Hello táblaterületen és a biztonsági mentés fájljának eltávolítása:</span><span class="sxs-lookup"><span data-stu-id="6bde5-184">Remove hello tablespace and backup files:</span></span>

    ```bash
    $ sudo su - oracle
    $ cd /u01/app/oracle/oradata/cdb1
    $ rm -f *.dbf
    $ cd /u01/app/oracle/fast_recovery_area/CDB1/backupset
    $ rm -rf *
    ```
    
2.  <span data-ttu-id="6bde5-185">(Választható) Állítsa le a hello Oracle-példány:</span><span class="sxs-lookup"><span data-stu-id="6bde5-185">(Optional) Shut down hello Oracle instance:</span></span>

    ```bash
    $ sqlplus / as sysdba
    SQL> shutdown abort
    ORACLE instance shut down.
    ```

## <a name="restore-hello-deleted-files-from-hello-recovery-services-vaults"></a><span data-ttu-id="6bde5-186">Recovery Services-tárolók hello hello törölt fájlok visszaállítása</span><span class="sxs-lookup"><span data-stu-id="6bde5-186">Restore hello deleted files from hello Recovery Services vaults</span></span>
<span data-ttu-id="6bde5-187">toorestore hello törölt fájlok, teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6bde5-187">toorestore hello deleted files, complete hello following steps:</span></span>

1. <span data-ttu-id="6bde5-188">A hello Azure-portálon, keresse meg a hello *myVault* Recovery Services-tárolók elemet.</span><span class="sxs-lookup"><span data-stu-id="6bde5-188">In hello Azure portal, search for hello *myVault* Recovery Services vaults item.</span></span> <span data-ttu-id="6bde5-189">A hello **áttekintése** panelen, a **elem biztonsági mentését**, válassza ki a hello elemek száma.</span><span class="sxs-lookup"><span data-stu-id="6bde5-189">On hello **Overview** blade, under **Backup items**, select hello number of items.</span></span>

    ![Helyreállítási szolgáltatások tárolók myVault biztonsági másolati elemei](./media/oracle-backup-recovery/recovery_service_12.png)

2. <span data-ttu-id="6bde5-191">A **biztonsági MENTÉS elemek száma**, válassza ki a hello elemek száma.</span><span class="sxs-lookup"><span data-stu-id="6bde5-191">Under **BACKUP ITEM COUNT**, select hello number of items.</span></span>

    ![Recovery Services-tárolók Azure virtuális gép biztonsági mentési elemek száma](./media/oracle-backup-recovery/recovery_service_13.png)

3. <span data-ttu-id="6bde5-193">A hello **myvm1** panelen kattintson a **fájlhelyreállítás (előzetes verzió)**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-193">On hello **myvm1** blade, click **File Recovery (Preview)**.</span></span>

    ![Képernyőfelvétel a hello Recovery Services-tárolók helyreállítási lapja](./media/oracle-backup-recovery/recovery_service_14.png)

4. <span data-ttu-id="6bde5-195">A hello **fájlhelyreállítás (előzetes verzió)** ablaktáblán kattintson a **parancsfájl letöltése**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-195">On hello **File Recovery (Preview)** pane, click **Download Script**.</span></span> <span data-ttu-id="6bde5-196">Ezt követően mentse hello letöltési (.sh) fájl tooa mappában hello ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="6bde5-196">Then, save hello download (.sh) file tooa folder on hello client computer.</span></span>

    ![Töltse le a parancsfájl mentése beállításai](./media/oracle-backup-recovery/recovery_service_15.png)

5. <span data-ttu-id="6bde5-198">Hello .sh fájl toohello VM másolja.</span><span class="sxs-lookup"><span data-stu-id="6bde5-198">Copy hello .sh file toohello VM.</span></span>

    <span data-ttu-id="6bde5-199">hello a következő példa bemutatja, hogyan egy biztonságos másolása (scp) parancs toomove toouse hello fájl toohello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="6bde5-199">hello following example shows how you toouse a secure copy (scp) command toomove hello file toohello VM.</span></span> <span data-ttu-id="6bde5-200">Is másolhatja hello tartalma toohello vágólapra, majd a Beillesztés hello tartalmát egy új fájlba, a virtuális gép hello be van állítva.</span><span class="sxs-lookup"><span data-stu-id="6bde5-200">You also can copy hello contents toohello clipboard, and then paste hello contents in a new file that is set up on hello VM.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="6bde5-201">A következő példa hello győződjön meg arról, hogy a hello IP cím és a mappa értékek frissíteni.</span><span class="sxs-lookup"><span data-stu-id="6bde5-201">In hello following example, ensure that you update hello IP address and folder values.</span></span> <span data-ttu-id="6bde5-202">hello értékek hello fájl mentési helyének toohello mappát kell társítani.</span><span class="sxs-lookup"><span data-stu-id="6bde5-202">hello values must map toohello folder where hello file is saved.</span></span>

    ```bash
    $ scp Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh <publicIpAddress>:/<folder>
    ```
6. <span data-ttu-id="6bde5-203">Hello fájl módosítása a hello legfelső szintű tulajdonában van.</span><span class="sxs-lookup"><span data-stu-id="6bde5-203">Change hello file, so that it's owned by hello root.</span></span>

    <span data-ttu-id="6bde5-204">A következő példa hello módosítsa hello fájlt úgy, hogy a tulajdonosa hello legfelső szintű.</span><span class="sxs-lookup"><span data-stu-id="6bde5-204">In hello following example, change hello file so that it's owned by hello root.</span></span> <span data-ttu-id="6bde5-205">Ezután módosítsa az engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="6bde5-205">Then, change permissions.</span></span>

    ```bash 
    $ ssh <publicIpAddress>
    $ sudo su -
    # chown root:root /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # chmod 755 /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    ```
    <span data-ttu-id="6bde5-206">hello következő példát talál parancsfájlt hello futtatása után.</span><span class="sxs-lookup"><span data-stu-id="6bde5-206">hello following example shows what you should see after you run hello preceding script.</span></span> <span data-ttu-id="6bde5-207">Amikor felszólítja toocontinue, adja meg **Y**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-207">When you're prompted toocontinue, enter **Y**.</span></span>

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

7. <span data-ttu-id="6bde5-208">Hozzáférés toohello csatlakoztatott kötetek ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="6bde5-208">Access toohello mounted volumes is confirmed.</span></span>

    <span data-ttu-id="6bde5-209">tooexit, adja meg **q**, majd keresse meg a hello csatlakoztatott kötetek.</span><span class="sxs-lookup"><span data-stu-id="6bde5-209">tooexit, enter **q**, and then search for hello mounted volumes.</span></span> <span data-ttu-id="6bde5-210">hello listája hozzáadott kötetek, a parancssorba írja be toocreate **df -k**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-210">toocreate a list of hello added volumes, at a command prompt, enter **df -k**.</span></span>

    ![hello df -k parancs](./media/oracle-backup-recovery/recovery_service_16.png)

8. <span data-ttu-id="6bde5-212">A következő parancsfájl toocopy hello hiányzó használata hello fájlok hátsó toohello mappák:</span><span class="sxs-lookup"><span data-stu-id="6bde5-212">Use hello following script toocopy hello missing files back toohello folders:</span></span>

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
9. <span data-ttu-id="6bde5-213">A következő parancsfájl hello RMAN toorecover hello adatbázis használata:</span><span class="sxs-lookup"><span data-stu-id="6bde5-213">In hello following script, use RMAN toorecover hello database:</span></span>

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```
    
10. <span data-ttu-id="6bde5-214">Hello lemez leválasztása.</span><span class="sxs-lookup"><span data-stu-id="6bde5-214">Unmount hello disk.</span></span>

    <span data-ttu-id="6bde5-215">Az Azure portál, a hello hello **fájlhelyreállítás (előzetes verzió)** panelen kattintson a **lemez leválasztása**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-215">In hello Azure portal, on hello **File Recovery (Preview)** blade, click **Unmount Disks**.</span></span>

    ![Válassza le a lemezek parancs](./media/oracle-backup-recovery/recovery_service_17.png)

## <a name="restore-hello-entire-vm"></a><span data-ttu-id="6bde5-217">Visszaállítás hello teljes méretű VM</span><span class="sxs-lookup"><span data-stu-id="6bde5-217">Restore hello entire VM</span></span>

<span data-ttu-id="6bde5-218">Helyett a Recovery Services-tárolók hello hello törölt fájlok visszaállítása során, hogy visszaállíthassa a teljes virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="6bde5-218">Instead of restoring hello deleted files from hello Recovery Services vaults, you can restore hello entire VM.</span></span>

### <a name="step-1-delete-myvm"></a><span data-ttu-id="6bde5-219">1. lépés: Delete myVM</span><span class="sxs-lookup"><span data-stu-id="6bde5-219">Step 1: Delete myVM</span></span>

*   <span data-ttu-id="6bde5-220">A hello Azure-portálon, válassza a toohello **myVM1** tároló, és válassza ki **törlése**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-220">In hello Azure portal, go toohello **myVM1** vault, and then select **Delete**.</span></span>

    ![Tároló delete parancs esetében](./media/oracle-backup-recovery/recover_vm_01.png)

### <a name="step-2-recover-hello-vm"></a><span data-ttu-id="6bde5-222">2. lépés: Hello virtuális gép helyreállítása</span><span class="sxs-lookup"><span data-stu-id="6bde5-222">Step 2: Recover hello VM</span></span>

1.  <span data-ttu-id="6bde5-223">Nyissa meg túl**Recovery Services-tárolók**, majd válassza ki **myVault**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-223">Go too**Recovery Services vaults**, and then select **myVault**.</span></span>

    ![myVault bejegyzés](./media/oracle-backup-recovery/recover_vm_02.png)

2.  <span data-ttu-id="6bde5-225">A hello **áttekintése** panelen, a **elem biztonsági mentését**, válassza ki a hello elemek száma.</span><span class="sxs-lookup"><span data-stu-id="6bde5-225">On hello **Overview** blade, under **Backup items**, select hello number of items.</span></span>

    ![Készítsen biztonsági másolatot elemek myVault](./media/oracle-backup-recovery/recover_vm_03.png)

3.  <span data-ttu-id="6bde5-227">A hello **(Azure virtuális gép) biztonsági mentés elemek** panelen válassza **myvm1**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-227">On hello **Backup Items (Azure Virtual Machine)** blade, select **myvm1**.</span></span>

    ![A helyreállítási virtuális gép lap](./media/oracle-backup-recovery/recover_vm_04.png)

4.  <span data-ttu-id="6bde5-229">A hello **myvm1** paneljén kattintson hello három pont (**...** ) gombra, majd **visszaállítani a virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-229">On hello **myvm1** blade, click hello ellipsis (**...**) button,  and then click **Restore VM**.</span></span>

    ![A restore parancs méretű VM](./media/oracle-backup-recovery/recover_vm_05.png)

5.  <span data-ttu-id="6bde5-231">A hello **visszaállítási pont kiválasztása** panelen, jelölje be hello elem toorestore szeretne, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-231">On hello **Select restore point** blade, select hello item that you want toorestore, and then click **OK**.</span></span>

    ![Jelölje be hello visszaállítási pont](./media/oracle-backup-recovery/recover_vm_06.png)

    <span data-ttu-id="6bde5-233">Ha engedélyezte az alkalmazáskonzisztens biztonsági mentését, egy függőleges kék színű sáv jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="6bde5-233">If you have enabled application-consistent backup, a vertical blue bar appears.</span></span>

6.  <span data-ttu-id="6bde5-234">A hello **konfiguráció visszaállítása** panelen, jelölje be hello virtuális gép nevét, jelölje ki a hello erőforráscsoport, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-234">On hello **Restore configuration** blade, select hello virtual machine name, select hello resource group, and then click **OK**.</span></span>

    ![Állítsa vissza a konfigurációs értékek](./media/oracle-backup-recovery/recover_vm_07.png)

7.  <span data-ttu-id="6bde5-236">toorestore hello VM, kattintson a hello **visszaállítása** gombra.</span><span class="sxs-lookup"><span data-stu-id="6bde5-236">toorestore hello VM, click hello **Restore** button.</span></span>

8.  <span data-ttu-id="6bde5-237">tooview hello állapotát hello visszaállítási folyamat, kattintson az **feladatok**, és kattintson a **biztonsági mentési feladatok**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-237">tooview hello status of hello restore process, click **Jobs**, and then click **Backup Jobs**.</span></span>

    ![Biztonsági mentési feladatok állapota parancs](./media/oracle-backup-recovery/recover_vm_08.png)

    <span data-ttu-id="6bde5-239">hello következő ábrán látható hello visszaállítási folyamat hello állapotát:</span><span class="sxs-lookup"><span data-stu-id="6bde5-239">hello following figure shows hello status of hello restore process:</span></span>

    ![Hello visszaállítási folyamat állapota](./media/oracle-backup-recovery/recover_vm_09.png)

### <a name="step-3-set-hello-public-ip-address"></a><span data-ttu-id="6bde5-241">3. lépés: Hello nyilvános IP-cím beállítása</span><span class="sxs-lookup"><span data-stu-id="6bde5-241">Step 3: Set hello public IP address</span></span>
<span data-ttu-id="6bde5-242">Hello VM helyreáll, után állítsa be hello nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="6bde5-242">After hello VM is restored, set up hello public IP address.</span></span>

1.  <span data-ttu-id="6bde5-243">Hello keresési mezőbe, írja be a **nyilvános IP-cím**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-243">In hello search box, enter **public IP address**.</span></span>

    ![Nyilvános IP-címek listája](./media/oracle-backup-recovery/create_ip_00.png)

2.  <span data-ttu-id="6bde5-245">A hello **nyilvános IP-címek** panelen kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-245">On hello **Public IP addresses** blade, click **Add**.</span></span> <span data-ttu-id="6bde5-246">A hello **nyilvános IP-cím létrehozása** panelen a **neve**, jelölje be hello nyilvános IP-név.</span><span class="sxs-lookup"><span data-stu-id="6bde5-246">On hello **Create public IP address** blade, for **Name**, select hello public IP name.</span></span> <span data-ttu-id="6bde5-247">Az **Erőforráscsoport** területen válassza a **Meglévő használata** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="6bde5-247">For **Resource group**, select **Use existing**.</span></span> <span data-ttu-id="6bde5-248">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="6bde5-248">Then, click **Create**.</span></span>

    ![IP-cím létrehozása](./media/oracle-backup-recovery/create_ip_01.png)

3.  <span data-ttu-id="6bde5-250">tooassociate hello nyilvános IP-cím hello hálózati csatoló a virtuális gép, hello keresse és jelölje be a **myVMip**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-250">tooassociate hello public IP address with hello network interface for hello VM, search for and select **myVMip**.</span></span> <span data-ttu-id="6bde5-251">Kattintson a **társítása**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-251">Then, click **Associate**.</span></span>

    ![IP-cím hozzárendelése](./media/oracle-backup-recovery/create_ip_02.png)

4.  <span data-ttu-id="6bde5-253">A **erőforrástípus**, jelölje be **hálózati illesztő**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-253">For **Resource type**, select **Network interface**.</span></span> <span data-ttu-id="6bde5-254">Jelölje be, amely hello myVM példány által használt hello hálózati adaptert, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="6bde5-254">Select hello network interface that is used by hello myVM instance, and then click **OK**.</span></span>

    ![Válassza ki az erőforrás típusa és a hálózati adapter értékek](./media/oracle-backup-recovery/create_ip_03.png)

5.  <span data-ttu-id="6bde5-256">Keresse meg, és nyissa meg a hello példánya, amely van legelterjedtebb myVM hello portálról.</span><span class="sxs-lookup"><span data-stu-id="6bde5-256">Search for and open hello instance of myVM that is ported from hello portal.</span></span> <span data-ttu-id="6bde5-257">hello hello VM társított IP-cím jelenik meg hello myVM **áttekintése** panelen.</span><span class="sxs-lookup"><span data-stu-id="6bde5-257">hello IP address that is associated with hello VM appears on hello myVM **Overview** blade.</span></span>

    ![IP-cím értéke](./media/oracle-backup-recovery/create_ip_04.png)

### <a name="step-4-connect-toohello-vm"></a><span data-ttu-id="6bde5-259">4. lépés: Csatlakozás toohello méretű VM</span><span class="sxs-lookup"><span data-stu-id="6bde5-259">Step 4: Connect toohello VM</span></span>

*   <span data-ttu-id="6bde5-260">tooconnect toohello VM, a következő parancsfájl hello használata:</span><span class="sxs-lookup"><span data-stu-id="6bde5-260">tooconnect toohello VM, use hello following script:</span></span>

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-5-test-whether-hello-database-is-accessible"></a><span data-ttu-id="6bde5-261">5. lépés: Hello adatbázis elérhető-e tesztelése</span><span class="sxs-lookup"><span data-stu-id="6bde5-261">Step 5: Test whether hello database is accessible</span></span>
*   <span data-ttu-id="6bde5-262">tootest kisegítő, a következő parancsfájl használata hello:</span><span class="sxs-lookup"><span data-stu-id="6bde5-262">tootest accessibility, use hello following script:</span></span>

    ```bash 
    $ sudo su - oracle
    $ sqlplus / as sysdba
    SQL> startup
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="6bde5-263">Ha hello adatbázis **indítási** parancs létrehoz egy hiba, toorecover hello adatbázis című [6. lépés: használja RMAN toorecover hello adatbázis](#step-6-optional-use-rman-to-recover-the-database).</span><span class="sxs-lookup"><span data-stu-id="6bde5-263">If hello database **startup** command generates an error, toorecover hello database, see [Step 6: Use RMAN toorecover hello database](#step-6-optional-use-rman-to-recover-the-database).</span></span>

### <a name="step-6-optional-use-rman-toorecover-hello-database"></a><span data-ttu-id="6bde5-264">6. lépés: (Nem kötelező) használata RMAN toorecover hello adatbázis</span><span class="sxs-lookup"><span data-stu-id="6bde5-264">Step 6: (Optional) Use RMAN toorecover hello database</span></span>
*   <span data-ttu-id="6bde5-265">toorecover hello adatbázis, a következő parancsfájl használata hello:</span><span class="sxs-lookup"><span data-stu-id="6bde5-265">toorecover hello database, use hello following script:</span></span>

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```

<span data-ttu-id="6bde5-266">hello biztonsági mentési és helyreállítási hello Oracle-adatbázishoz 12c adatbázis az Azure Linux virtuális gép most már befejeződött.</span><span class="sxs-lookup"><span data-stu-id="6bde5-266">hello backup and recovery of hello Oracle Database 12c database on an Azure Linux VM is now finished.</span></span>

## <a name="delete-hello-vm"></a><span data-ttu-id="6bde5-267">Hello virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="6bde5-267">Delete hello VM</span></span>

<span data-ttu-id="6bde5-268">Ha a virtuális gép már nem kell hello, használhatja a következő parancs tooremove hello erőforráscsoport, hello VM és minden kapcsolódó erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="6bde5-268">When you no longer need hello VM, you can use hello following command tooremove hello resource group, hello VM, and all related resources:</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="6bde5-269">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6bde5-269">Next steps</span></span>

[<span data-ttu-id="6bde5-270">Oktatóanyag: Hozzon létre magas rendelkezésre állású virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="6bde5-270">Tutorial: Create highly available VMs</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="6bde5-271">Virtuális gép telepítése az Azure parancssori felület minták felfedezése</span><span class="sxs-lookup"><span data-stu-id="6bde5-271">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)



