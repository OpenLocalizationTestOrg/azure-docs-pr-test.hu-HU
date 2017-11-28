---
title: "Biztonsági mentése és helyreállítása Azure Linux virtuális géphez az Oracle-adatbázishoz 12c adatbázis |} Microsoft Docs"
description: "Útmutató: biztonsági mentése és helyreállítása az Oracle-adatbázishoz 12c adatbázis az Azure környezetben."
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
ms.openlocfilehash: 9a2293f13b90e9a4cb11b4169fad969dd622a9a6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-and-recover-an-oracle-database-12c-database-on-an-azure-linux-virtual-machine"></a><span data-ttu-id="54733-103">Készítsen biztonsági másolatot, és az Azure Linux virtuális gép Oracle-adatbázishoz 12c-adatbázis helyreállítása</span><span class="sxs-lookup"><span data-stu-id="54733-103">Back up and recover an Oracle Database 12c database on an Azure Linux virtual machine</span></span>

<span data-ttu-id="54733-104">Azure CLI segítségével létrehozása és kezelése az Azure-erőforrások parancsot a parancssorba, vagy parancsfájlok használata.</span><span class="sxs-lookup"><span data-stu-id="54733-104">You can use Azure CLI to create and manage Azure resources at a command prompt, or use scripts.</span></span> <span data-ttu-id="54733-105">Ebben a cikkben az Azure parancssori felület parancsfájlok használjuk az Oracle-adatbázishoz 12c adatbázis Azure piactér gyűjtemény lemezkép telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="54733-105">In this article, we use Azure CLI scripts to deploy an Oracle Database 12c database from an Azure Marketplace gallery image.</span></span>

<span data-ttu-id="54733-106">Mielőtt elkezdené, győződjön meg arról, hogy telepítve van-e az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="54733-106">Before you begin, make sure that Azure CLI is installed.</span></span> <span data-ttu-id="54733-107">További információkért lásd: a [Azure parancssori felület telepítési útmutató](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="54733-107">For more information, see the [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-the-environment"></a><span data-ttu-id="54733-108">A környezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="54733-108">Prepare the environment</span></span>

### <a name="step-1-prerequisites"></a><span data-ttu-id="54733-109">1. lépés: Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="54733-109">Step 1: Prerequisites</span></span>

*   <span data-ttu-id="54733-110">A biztonsági mentési és helyreállítási folyamat végrehajtásához először létre kell hoznia egy Linux virtuális Gépet, amely rendelkezik az Oracle-adatbázishoz 12c telepített példánya.</span><span class="sxs-lookup"><span data-stu-id="54733-110">To perform the backup and recovery process, you must first create a Linux VM that has an installed instance of Oracle Database 12c.</span></span> <span data-ttu-id="54733-111">A Piactéri lemezképet hozhat létre a virtuális gép neve *Oracle: Oracle-adatbázis-Ee:12.1.0.2:latest*.</span><span class="sxs-lookup"><span data-stu-id="54733-111">The Marketplace image you use to create the VM is named *Oracle:Oracle-Database-Ee:12.1.0.2:latest*.</span></span>

    <span data-ttu-id="54733-112">Az Oracle-adatbázis létrehozásához, lásd: a [Oracle adatbázis gyors üzembe helyezés létrehozása](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).</span><span class="sxs-lookup"><span data-stu-id="54733-112">To learn how to create an Oracle database, see the [Oracle create database quickstart](https://docs.microsoft.com/azure/virtual-machines/workloads/oracle/oracle-database-quick-create).</span></span>


### <a name="step-2-connect-to-the-vm"></a><span data-ttu-id="54733-113">2. lépés: Csatlakoztassa a virtuális Gépet</span><span class="sxs-lookup"><span data-stu-id="54733-113">Step 2: Connect to the VM</span></span>

*   <span data-ttu-id="54733-114">Secure Shell (SSH) munkamenetet létrehozni a virtuális gép, használja a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="54733-114">To create a Secure Shell (SSH) session with the VM, use the following command.</span></span> <span data-ttu-id="54733-115">Az IP-cím és a gazdagép neve együtt cserélje le a `publicIpAddress` értéket a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="54733-115">Replace the IP address and the host name combination with the `publicIpAddress` value for your VM.</span></span>

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-3-prepare-the-database"></a><span data-ttu-id="54733-116">3. lépés: Az adatbázis előkészítése</span><span class="sxs-lookup"><span data-stu-id="54733-116">Step 3: Prepare the database</span></span>

1.  <span data-ttu-id="54733-117">Ez a lépés feltételezi, hogy rendelkezik-e egy egy nevű virtuális gépen futó Oracle-példányok (cdb1) *myVM*.</span><span class="sxs-lookup"><span data-stu-id="54733-117">This step assumes that you have an Oracle instance (cdb1) that is running on a VM named *myVM*.</span></span>

    <span data-ttu-id="54733-118">Futtassa a *oracle* felügyelő legfelső szintű és a figyelő majd inicializálása:</span><span class="sxs-lookup"><span data-stu-id="54733-118">Run the *oracle* superuser root, and then initialize the listener:</span></span>

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    Copyright (c) 1991, 2014, Oracle.  All rights reserved.

    Starting /u01/app/oracle/product/12.1.0/dbhome_1/bin/tnslsnr: please wait...

    TNSLSNR for Linux: Version 12.1.0.2.0 - Production
    Log messages written to /u01/app/oracle/diag/tnslsnr/myVM/listener/alert/log.xml
    Listening on: (DESCRIPTION=(ADDRESS=(PROTOCOL=tcp)(HOST=myVM.twltkue3xvsujaz1bvlrhfuiwf.dx.internal.cloudapp.net)(PORT=1521)))

    Connecting to (ADDRESS=(PROTOCOL=tcp)(HOST=)(PORT=1521))
    STATUS of the LISTENER
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
    The listener supports no services
    The command completed successfully
    ```

2.  <span data-ttu-id="54733-119">(Választható) Ellenőrizze, hogy az adatbázis archív napló módban van:</span><span class="sxs-lookup"><span data-stu-id="54733-119">(Optional) Make sure the database is in archive log mode:</span></span>

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
3.  <span data-ttu-id="54733-120">(Választható) Hozzon létre egy táblát, a véglegesítés teszteléséhez:</span><span class="sxs-lookup"><span data-stu-id="54733-120">(Optional) Create a table to test the commit:</span></span>

    ```bash
    SQL> alter session set "_ORACLE_SCRIPT"=true ;
    Session altered.
    SQL> create user scott identified by tiger;
    User created.
    SQL> grant create session to scott;
    Grant succeeded.
    SQL> grant create table to scott;
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
4.  <span data-ttu-id="54733-121">Győződjön meg arról, vagy módosítsa a biztonságimásolat-fájl helye és mérete:</span><span class="sxs-lookup"><span data-stu-id="54733-121">Verify or change the backup file location and size:</span></span>

    ```bash
    $ sqlplus / as sysdba
    SQL> show parameter db_recovery
    NAME                                 TYPE        VALUE
    ------------------------------------ ----------- ------------------------------
    db_recovery_file_dest                string      /u01/app/oracle/fast_recovery_area
    db_recovery_file_dest_size           big integer 4560M
    ```
5. <span data-ttu-id="54733-122">Oracle Recovery Manager (RMAN) segítségével készítsen biztonsági másolatot az adatbázisról:</span><span class="sxs-lookup"><span data-stu-id="54733-122">Use Oracle Recovery Manager (RMAN) to back up the database:</span></span>

    ```bash
    $ rman target /
    RMAN> backup database plus archivelog;
    ```

### <a name="step-4-application-consistent-backup-for-linux-vms"></a><span data-ttu-id="54733-123">4. lépés: A Linux virtuális gépek alkalmazáskonzisztens biztonsági mentését</span><span class="sxs-lookup"><span data-stu-id="54733-123">Step 4: Application-consistent backup for Linux VMs</span></span>

<span data-ttu-id="54733-124">Alkalmazáskonzisztens biztonsági mentést az Azure biztonsági mentés új szolgáltatása.</span><span class="sxs-lookup"><span data-stu-id="54733-124">Application-consistent backups is a new feature in Azure Backup.</span></span> <span data-ttu-id="54733-125">Hozzon létre, és válassza ki a parancsfájlok végrehajtása előtt és után a virtuális gép pillanatkép (pillanatkép előtti és utáni pillanatkép).</span><span class="sxs-lookup"><span data-stu-id="54733-125">You can create and select scripts to execute before and after the VM snapshot (pre-snapshot and post-snapshot).</span></span>

1. <span data-ttu-id="54733-126">A JSON-fájl letöltésére.</span><span class="sxs-lookup"><span data-stu-id="54733-126">Download the JSON file.</span></span>

    <span data-ttu-id="54733-127">Https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig VMSnapshotScriptPluginConfig.json le.</span><span class="sxs-lookup"><span data-stu-id="54733-127">Download VMSnapshotScriptPluginConfig.json from https://github.com/MicrosoftAzureBackup/VMSnapshotPluginConfig.</span></span> <span data-ttu-id="54733-128">A fájl tartalmának keressen a következőhöz hasonló:</span><span class="sxs-lookup"><span data-stu-id="54733-128">The file contents look similar to the following:</span></span>

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

2. <span data-ttu-id="54733-129">A /etc/azure mappa létrehozása a virtuális Gépen:</span><span class="sxs-lookup"><span data-stu-id="54733-129">Create the /etc/azure folder on the VM:</span></span>

    ```bash
    $ sudo su -
    # mkdir /etc/azure
    # cd /etc/azure
    ```

3. <span data-ttu-id="54733-130">A JSON-fájl másolása.</span><span class="sxs-lookup"><span data-stu-id="54733-130">Copy the JSON file.</span></span>

    <span data-ttu-id="54733-131">VMSnapshotScriptPluginConfig.json másolása a /etc/azure mappába.</span><span class="sxs-lookup"><span data-stu-id="54733-131">Copy VMSnapshotScriptPluginConfig.json to the /etc/azure folder.</span></span>

4. <span data-ttu-id="54733-132">Szerkessze a JSON-fájlt.</span><span class="sxs-lookup"><span data-stu-id="54733-132">Edit the JSON file.</span></span>

    <span data-ttu-id="54733-133">A VMSnapshotScriptPluginConfig.json-fájl szerkesztése a `PreScriptLocation` és `PostScriptlocation` paraméterek.</span><span class="sxs-lookup"><span data-stu-id="54733-133">Edit the VMSnapshotScriptPluginConfig.json file to include the `PreScriptLocation` and `PostScriptlocation` parameters.</span></span> <span data-ttu-id="54733-134">Példa:</span><span class="sxs-lookup"><span data-stu-id="54733-134">For example:</span></span>

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

5. <span data-ttu-id="54733-135">A pillanatkép előtti és a pillanatkép utáni parancsfájlok létrehozása.</span><span class="sxs-lookup"><span data-stu-id="54733-135">Create the pre-snapshot and post-snapshot script files.</span></span>

    <span data-ttu-id="54733-136">Íme egy példa a "cold biztonsági mentéshez" pillanatkép előtti és a pillanatkép utáni parancsfájlok (offline biztonsági másolat, leállítása és újraindítása):</span><span class="sxs-lookup"><span data-stu-id="54733-136">Here's an example of pre-snapshot and post-snapshot scripts for a "cold backup" (an offline backup, with shutdown and restart):</span></span>

    <span data-ttu-id="54733-137">A /etc/azure/pre_script.sh:</span><span class="sxs-lookup"><span data-stu-id="54733-137">For /etc/azure/pre_script.sh:</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" > /etc/azure/pre_script_$v_date.log
    ```

    <span data-ttu-id="54733-138">A /etc/azure/post_script.sh:</span><span class="sxs-lookup"><span data-stu-id="54733-138">For /etc/azure/post_script.sh:</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" > /etc/azure/post_script_$v_date.log
    ```

    <span data-ttu-id="54733-139">Íme egy példa a "gyors biztonsági mentéshez" pillanatkép előtti és a pillanatkép utáni parancsfájlok (online biztonsági mentés):</span><span class="sxs-lookup"><span data-stu-id="54733-139">Here's an example of pre-snapshot and post-snapshot scripts for a "hot backup" (an online backup):</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/pre_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    <span data-ttu-id="54733-140">A /etc/azure/post_script.sh:</span><span class="sxs-lookup"><span data-stu-id="54733-140">For /etc/azure/post_script.sh:</span></span>

    ```bash
    v_date=`date +%Y%m%d%H%M`
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle
    su - $ORA_OWNER -c "sqlplus / as sysdba @/etc/azure/post_script.sql" > /etc/azure/pre_script_$v_date.log
    ```

    <span data-ttu-id="54733-141">A /etc/azure/pre_script.sql módosítsa a / a követelmények a fájl tartalmát:</span><span class="sxs-lookup"><span data-stu-id="54733-141">For /etc/azure/pre_script.sql, modify the contents of the file per your requirements:</span></span>

    ```bash
    alter tablespace SYSTEM begin backup;
    ...
    ...
    alter system switch logfile;
    alter system archive log stop;
    ```

    <span data-ttu-id="54733-142">A /etc/azure/post_script.sql módosítsa a / a követelmények a fájl tartalmát:</span><span class="sxs-lookup"><span data-stu-id="54733-142">For /etc/azure/post_script.sql, modify the contents of the file per your requirements:</span></span>

    ```bash
    alter tablespace SYSTEM end backup;
    ...
    ...
    alter system archive log start;
    ```

6. <span data-ttu-id="54733-143">Fájl engedélyeinek módosítása:</span><span class="sxs-lookup"><span data-stu-id="54733-143">Change file permissions:</span></span>

    ```bash
    # chmod 600 /etc/azure/VMSnapshotScriptPluginConfig.json
    # chmod 700 /etc/azure/pre_script.sh
    # chmod 700 /etc/azure/post_script.sh
    ```

7. <span data-ttu-id="54733-144">Tesztelje a parancsfájlokat.</span><span class="sxs-lookup"><span data-stu-id="54733-144">Test the scripts.</span></span>

    <span data-ttu-id="54733-145">Tesztelje a parancsfájlokat, először jelentkezzen be rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="54733-145">To test the scripts, first, sign in as root.</span></span> <span data-ttu-id="54733-146">Majd győződjön meg arról, hogy nincsenek-e hibák:</span><span class="sxs-lookup"><span data-stu-id="54733-146">Then, ensure that there are no errors:</span></span>

    ```bash
    # /etc/azure/pre_script.sh
    # /etc/azure/post_script.sh
    ```

<span data-ttu-id="54733-147">További információkért lásd: [Linux virtuális gépek alkalmazáskonzisztens biztonsági mentését](https://azure.microsoft.com/en-us/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).</span><span class="sxs-lookup"><span data-stu-id="54733-147">For more information, see [Application-consistent backup for Linux VMs](https://azure.microsoft.com/en-us/blog/announcing-application-consistent-backup-for-linux-vms-using-azure-backup/).</span></span>


### <a name="step-5-use-azure-recovery-services-vaults-to-back-up-the-vm"></a><span data-ttu-id="54733-148">5. lépés: Használata Azure Recovery Services-tárolók biztonsági mentése a virtuális gép</span><span class="sxs-lookup"><span data-stu-id="54733-148">Step 5: Use Azure Recovery Services vaults to back up the VM</span></span>

1.  <span data-ttu-id="54733-149">Az Azure-portálon a keresse meg a **Recovery Services-tárolók**.</span><span class="sxs-lookup"><span data-stu-id="54733-149">In the Azure portal, search for **Recovery Services vaults**.</span></span>

    ![Helyreállítási szolgáltatások tárolók lap](./media/oracle-backup-recovery/recovery_service_01.png)

2.  <span data-ttu-id="54733-151">Az a **Recovery Services-tárolók** panelt, adja hozzá egy új tárolót, kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="54733-151">On the **Recovery Services vaults** blade, to add a new vault, click **Add**.</span></span>

    ![Helyreállítási szolgáltatások tárolók-weblap hozzáadása](./media/oracle-backup-recovery/recovery_service_02.png)

3.  <span data-ttu-id="54733-153">A folytatáshoz kattintson a **myVault**.</span><span class="sxs-lookup"><span data-stu-id="54733-153">To continue, click **myVault**.</span></span>

    ![Helyreállítási szolgáltatások tárolók részletességi lap](./media/oracle-backup-recovery/recovery_service_03.png)

4.  <span data-ttu-id="54733-155">Az a **myVault** panelen kattintson a **biztonsági mentés**.</span><span class="sxs-lookup"><span data-stu-id="54733-155">On the **myVault** blade, click **Backup**.</span></span>

    ![Helyreállítási szolgáltatások tárolók lap biztonsági mentése](./media/oracle-backup-recovery/recovery_service_04.png)

5.  <span data-ttu-id="54733-157">Az a **biztonsági mentési cél** panelen, használja az alapértelmezett értékekkel rendelkező **Azure** és **virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="54733-157">On the **Backup Goal** blade, use the default values of **Azure** and **Virtual machine**.</span></span> <span data-ttu-id="54733-158">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="54733-158">Click **OK**.</span></span>

    ![Helyreállítási szolgáltatások tárolók részletességi lap](./media/oracle-backup-recovery/recovery_service_05.png)

6.  <span data-ttu-id="54733-160">A **biztonsági mentési házirend**, használjon **DefaultPolicy**, vagy válasszon **hozzon létre új házirend**.</span><span class="sxs-lookup"><span data-stu-id="54733-160">For **Backup policy**, use **DefaultPolicy**, or select **Create New policy**.</span></span> <span data-ttu-id="54733-161">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="54733-161">Click **OK**.</span></span>

    ![Helyreállítási szolgáltatások tárolók biztonsági mentési házirend Részletek lap](./media/oracle-backup-recovery/recovery_service_06.png)

7.  <span data-ttu-id="54733-163">Az a **válassza ki a virtuális gépek** panelen válassza ki a **myVM1** jelölőnégyzetet, majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="54733-163">On the **Select virtual machines** blade, select the **myVM1** check box, and then click **OK**.</span></span> <span data-ttu-id="54733-164">Kattintson a **biztonságimásolat-készítés engedélyezése** gombra.</span><span class="sxs-lookup"><span data-stu-id="54733-164">Click the **Enable backup** button.</span></span>

    ![Helyreállítási szolgáltatások tárolók elemeket a biztonsági mentési Részletek lap](./media/oracle-backup-recovery/recovery_service_07.png)

    > [!IMPORTANT]
    > <span data-ttu-id="54733-166">Miután rákattintott **biztonságimásolat-készítés engedélyezése**, a biztonsági mentés nem indul el, amíg az ütemezett időpontban lejár.</span><span class="sxs-lookup"><span data-stu-id="54733-166">After you click **Enable backup**, the backup process doesn't start until the scheduled time expires.</span></span> <span data-ttu-id="54733-167">Az azonnali biztonsági másolat beállítása, a következő lépés végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="54733-167">To set up an immediate backup, complete the next step.</span></span>

8.  <span data-ttu-id="54733-168">A a **myVault - biztonsági mentés elemek** panel alatt **biztonsági MENTÉSI ELEMSZÁMNÁL**, válassza ki a biztonsági mentési elemek száma.</span><span class="sxs-lookup"><span data-stu-id="54733-168">On the **myVault - Backup items** blade, under **BACKUP ITEM COUNT**, select the backup item count.</span></span>

    ![Helyreállítási szolgáltatások tárolók myVault Részletek lap](./media/oracle-backup-recovery/recovery_service_08.png)

9.  <span data-ttu-id="54733-170">Az a **(Azure virtuális gép) biztonsági mentés elemek** panelen, a lap jobb oldalán kattintson a három pont (**...** ) gombra, majd **biztonsági mentés most**.</span><span class="sxs-lookup"><span data-stu-id="54733-170">On the **Backup Items (Azure Virtual Machine)** blade, on the right side of the page, click the ellipsis (**...**) button, and then click **Backup now**.</span></span>

    ![Helyreállítási szolgáltatások tárolók biztonsági mentés most parancsot.](./media/oracle-backup-recovery/recovery_service_09.png)

10. <span data-ttu-id="54733-172">Kattintson a **biztonsági mentés** gombra.</span><span class="sxs-lookup"><span data-stu-id="54733-172">Click the **Backup** button.</span></span> <span data-ttu-id="54733-173">Várja meg a biztonsági mentési folyamat befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="54733-173">Wait for the backup process to finish.</span></span> <span data-ttu-id="54733-174">Folytassa a [6. lépés: távolítsa el az adatbázisfájlok](#step-6-remove-the-database-files).</span><span class="sxs-lookup"><span data-stu-id="54733-174">Then, go to [Step 6: Remove the database files](#step-6-remove-the-database-files).</span></span>

    <span data-ttu-id="54733-175">A biztonsági mentési feladat állapotának megtekintéséhez kattintson **feladatok**.</span><span class="sxs-lookup"><span data-stu-id="54733-175">To view the status of the backup job, click **Jobs**.</span></span>

    ![Helyreállítási szolgáltatások tárolók feladat lap](./media/oracle-backup-recovery/recovery_service_10.png)

    <span data-ttu-id="54733-177">A biztonsági mentési feladat állapotát a következő kép jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="54733-177">The status of the backup job appears in the following image:</span></span>

    ![Helyreállítási szolgáltatások tárolók feladat állapotú lap](./media/oracle-backup-recovery/recovery_service_11.png)

11. <span data-ttu-id="54733-179">Az alkalmazáskonzisztens biztonsági mentését címek a naplófájl-e hibák.</span><span class="sxs-lookup"><span data-stu-id="54733-179">For an application-consistent backup, address any errors in the log file.</span></span> <span data-ttu-id="54733-180">A naplófájl /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0 helyezkedik el.</span><span class="sxs-lookup"><span data-stu-id="54733-180">The log file is located at /var/log/azure/Microsoft.Azure.RecoveryServices.VMSnapshotLinux/1.0.9114.0.</span></span>

### <a name="step-6-remove-the-database-files"></a><span data-ttu-id="54733-181">6. lépés: Távolítsa el az adatbázis fájljai</span><span class="sxs-lookup"><span data-stu-id="54733-181">Step 6: Remove the database files</span></span> 
<span data-ttu-id="54733-182">A cikk későbbi részében megtudhatja, hogyan a helyreállítási folyamat ellenőrzéséhez.</span><span class="sxs-lookup"><span data-stu-id="54733-182">Later in this article, you'll learn how to test the recovery process.</span></span> <span data-ttu-id="54733-183">A helyreállítási folyamat teszteléséhez előtt el kell távolítania az adatbázisfájlokat.</span><span class="sxs-lookup"><span data-stu-id="54733-183">Before you can test the recovery process, you have to remove the database files.</span></span>

1.  <span data-ttu-id="54733-184">Távolítsa el a táblaterületen és a biztonsági mentési fájlokat:</span><span class="sxs-lookup"><span data-stu-id="54733-184">Remove the tablespace and backup files:</span></span>

    ```bash
    $ sudo su - oracle
    $ cd /u01/app/oracle/oradata/cdb1
    $ rm -f *.dbf
    $ cd /u01/app/oracle/fast_recovery_area/CDB1/backupset
    $ rm -rf *
    ```
    
2.  <span data-ttu-id="54733-185">(Választható) Állítsa le az Oracle-példány:</span><span class="sxs-lookup"><span data-stu-id="54733-185">(Optional) Shut down the Oracle instance:</span></span>

    ```bash
    $ sqlplus / as sysdba
    SQL> shutdown abort
    ORACLE instance shut down.
    ```

## <a name="restore-the-deleted-files-from-the-recovery-services-vaults"></a><span data-ttu-id="54733-186">A Recovery Services-tárolók a törölt fájlok visszaállítása</span><span class="sxs-lookup"><span data-stu-id="54733-186">Restore the deleted files from the Recovery Services vaults</span></span>
<span data-ttu-id="54733-187">A törölt fájlok visszaállításához kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="54733-187">To restore the deleted files, complete the following steps:</span></span>

1. <span data-ttu-id="54733-188">Az Azure portálon keresse meg a *myVault* Recovery Services-tárolók elemet.</span><span class="sxs-lookup"><span data-stu-id="54733-188">In the Azure portal, search for the *myVault* Recovery Services vaults item.</span></span> <span data-ttu-id="54733-189">A a **áttekintése** panel alatt **elem biztonsági mentését**, válassza ki az elemek száma.</span><span class="sxs-lookup"><span data-stu-id="54733-189">On the **Overview** blade, under **Backup items**, select the number of items.</span></span>

    ![Helyreállítási szolgáltatások tárolók myVault biztonsági másolati elemei](./media/oracle-backup-recovery/recovery_service_12.png)

2. <span data-ttu-id="54733-191">A **biztonsági MENTÉS ELEMSZÁMNÁL**, válassza ki az elemek száma.</span><span class="sxs-lookup"><span data-stu-id="54733-191">Under **BACKUP ITEM COUNT**, select the number of items.</span></span>

    ![Recovery Services-tárolók Azure virtuális gép biztonsági mentési elemek száma](./media/oracle-backup-recovery/recovery_service_13.png)

3. <span data-ttu-id="54733-193">Az a **myvm1** panelen kattintson a **fájlhelyreállítás (előzetes verzió)**.</span><span class="sxs-lookup"><span data-stu-id="54733-193">On the **myvm1** blade, click **File Recovery (Preview)**.</span></span>

    ![Képernyőfelvétel a Recovery Services-tárolók helyreállítási lapja](./media/oracle-backup-recovery/recovery_service_14.png)

4. <span data-ttu-id="54733-195">Az a **fájlhelyreállítás (előzetes verzió)** ablaktáblán kattintson a **parancsfájl letöltése**.</span><span class="sxs-lookup"><span data-stu-id="54733-195">On the **File Recovery (Preview)** pane, click **Download Script**.</span></span> <span data-ttu-id="54733-196">Az ügyfélszámítógépen, majd menti a letöltési (.sh) fájlt egy mappába.</span><span class="sxs-lookup"><span data-stu-id="54733-196">Then, save the download (.sh) file to a folder on the client computer.</span></span>

    ![Töltse le a parancsfájl mentése beállításai](./media/oracle-backup-recovery/recovery_service_15.png)

5. <span data-ttu-id="54733-198">Másolja a .sh fájlt a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="54733-198">Copy the .sh file to the VM.</span></span>

    <span data-ttu-id="54733-199">A következő példa bemutatja, hogyan biztonságos másolatot (scp) használatát a fájl áthelyezéséhez a virtuális gép parancsot.</span><span class="sxs-lookup"><span data-stu-id="54733-199">The following example shows how you to use a secure copy (scp) command to move the file to the VM.</span></span> <span data-ttu-id="54733-200">Akkor is is tartalmának másolása a vágólapra, majd illessze be a tartalom egy új fájlba, amely a virtuális Gépre van beállítva.</span><span class="sxs-lookup"><span data-stu-id="54733-200">You also can copy the contents to the clipboard, and then paste the contents in a new file that is set up on the VM.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="54733-201">A következő példában győződjön meg arról, hogy frissíti a IP-cím és a mappa értékek.</span><span class="sxs-lookup"><span data-stu-id="54733-201">In the following example, ensure that you update the IP address and folder values.</span></span> <span data-ttu-id="54733-202">Az értékek a mappába, a fájl menteni kell társítani.</span><span class="sxs-lookup"><span data-stu-id="54733-202">The values must map to the folder where the file is saved.</span></span>

    ```bash
    $ scp Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh <publicIpAddress>:/<folder>
    ```
6. <span data-ttu-id="54733-203">Módosítsa a fájlt, hogy a legfelső szintű tulajdonában van.</span><span class="sxs-lookup"><span data-stu-id="54733-203">Change the file, so that it's owned by the root.</span></span>

    <span data-ttu-id="54733-204">A következő példában módosítja a fájlt, hogy a legfelső szintű tulajdonában van.</span><span class="sxs-lookup"><span data-stu-id="54733-204">In the following example, change the file so that it's owned by the root.</span></span> <span data-ttu-id="54733-205">Ezután módosítsa az engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="54733-205">Then, change permissions.</span></span>

    ```bash 
    $ ssh <publicIpAddress>
    $ sudo su -
    # chown root:root /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # chmod 755 /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    # /<folder>/Linux_myvm1_xx-xx-2017 xx-xx-xx PM.sh
    ```
    <span data-ttu-id="54733-206">A következő példa bemutatja, lásd az előző parancsfájl futtatása után.</span><span class="sxs-lookup"><span data-stu-id="54733-206">The following example shows what you should see after you run the preceding script.</span></span> <span data-ttu-id="54733-207">Ha van, a folytatás megerősítését kéri, írja be **Y**.</span><span class="sxs-lookup"><span data-stu-id="54733-207">When you're prompted to continue, enter **Y**.</span></span>

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
    The script requires 'open-iscsi' and 'lshw' to run.
    Do you want us to install 'open-iscsi' and 'lshw' on this machine?
    Please press 'Y' to continue with installation, 'N' to abort the operation. : Y
    Installing 'open-iscsi'....
    Installing 'lshw'....

    Connecting to recovery point using ISCSI service...

    Connection succeeded!

    Please wait while we attach volumes of the recovery point to this machine...

    ************ Volumes of the recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath

    1)  | /dev/sde  |  /dev/sde1  |  /root/myVM-20170517093913/Volume1

    2)  | /dev/sde  |  /dev/sde2  |  /root/myVM-20170517093913/Volume2

    ************ Open File Explorer to browse for files. ************

    After recovery, to remove the disks and close the connection to the recovery point, please click 'Unmount Disks' in step 3 of the portal.

    Please enter 'q/Q' to exit...
    ```

7. <span data-ttu-id="54733-208">A csatlakoztatott kötetek való hozzáférés ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="54733-208">Access to the mounted volumes is confirmed.</span></span>

    <span data-ttu-id="54733-209">Lépjen ki, írja be a következőt **q**, majd keresse meg a csatlakoztatott kötetek.</span><span class="sxs-lookup"><span data-stu-id="54733-209">To exit, enter **q**, and then search for the mounted volumes.</span></span> <span data-ttu-id="54733-210">A felvett kötetek listájának létrehozásához parancsot a parancssorba írja be **df -k**.</span><span class="sxs-lookup"><span data-stu-id="54733-210">To create a list of the added volumes, at a command prompt, enter **df -k**.</span></span>

    ![A df -k parancs](./media/oracle-backup-recovery/recovery_service_16.png)

8. <span data-ttu-id="54733-212">A következő parancsfájl használata a hiányzó fájlokat másolja vissza a mappák:</span><span class="sxs-lookup"><span data-stu-id="54733-212">Use the following script to copy the missing files back to the folders:</span></span>

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
9. <span data-ttu-id="54733-213">A következő parancsfájlban RMAN használatával állítsa helyre az adatbázist:</span><span class="sxs-lookup"><span data-stu-id="54733-213">In the following script, use RMAN to recover the database:</span></span>

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```
    
10. <span data-ttu-id="54733-214">A lemez leválasztása.</span><span class="sxs-lookup"><span data-stu-id="54733-214">Unmount the disk.</span></span>

    <span data-ttu-id="54733-215">Az Azure portálon a a **fájlhelyreállítás (előzetes verzió)** panelen kattintson a **lemez leválasztása**.</span><span class="sxs-lookup"><span data-stu-id="54733-215">In the Azure portal, on the **File Recovery (Preview)** blade, click **Unmount Disks**.</span></span>

    ![Válassza le a lemezek parancs](./media/oracle-backup-recovery/recovery_service_17.png)

## <a name="restore-the-entire-vm"></a><span data-ttu-id="54733-217">Állítsa vissza a teljes méretű VM</span><span class="sxs-lookup"><span data-stu-id="54733-217">Restore the entire VM</span></span>

<span data-ttu-id="54733-218">Helyett a törölt fájlok visszaállítást végezni a Recovery Services-tárolók, állítsa vissza a teljes virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="54733-218">Instead of restoring the deleted files from the Recovery Services vaults, you can restore the entire VM.</span></span>

### <a name="step-1-delete-myvm"></a><span data-ttu-id="54733-219">1. lépés: Delete myVM</span><span class="sxs-lookup"><span data-stu-id="54733-219">Step 1: Delete myVM</span></span>

*   <span data-ttu-id="54733-220">Az Azure-portálon lépjen a **myVM1** tároló, és válassza ki **törlése**.</span><span class="sxs-lookup"><span data-stu-id="54733-220">In the Azure portal, go to the **myVM1** vault, and then select **Delete**.</span></span>

    ![Tároló delete parancs esetében](./media/oracle-backup-recovery/recover_vm_01.png)

### <a name="step-2-recover-the-vm"></a><span data-ttu-id="54733-222">2. lépés: A virtuális gép helyreállítása</span><span class="sxs-lookup"><span data-stu-id="54733-222">Step 2: Recover the VM</span></span>

1.  <span data-ttu-id="54733-223">Ugrás a **Recovery Services-tárolók**, majd válassza ki **myVault**.</span><span class="sxs-lookup"><span data-stu-id="54733-223">Go to **Recovery Services vaults**, and then select **myVault**.</span></span>

    ![myVault bejegyzés](./media/oracle-backup-recovery/recover_vm_02.png)

2.  <span data-ttu-id="54733-225">A a **áttekintése** panel alatt **elem biztonsági mentését**, válassza ki az elemek száma.</span><span class="sxs-lookup"><span data-stu-id="54733-225">On the **Overview** blade, under **Backup items**, select the number of items.</span></span>

    ![Készítsen biztonsági másolatot elemek myVault](./media/oracle-backup-recovery/recover_vm_03.png)

3.  <span data-ttu-id="54733-227">Az a **(Azure virtuális gép) biztonsági mentés elemek** panelen válassza **myvm1**.</span><span class="sxs-lookup"><span data-stu-id="54733-227">On the **Backup Items (Azure Virtual Machine)** blade, select **myvm1**.</span></span>

    ![A helyreállítási virtuális gép lap](./media/oracle-backup-recovery/recover_vm_04.png)

4.  <span data-ttu-id="54733-229">Az a **myvm1** panelen kattintson a három pont (**...** ) gombra, majd **visszaállítani a virtuális gép**.</span><span class="sxs-lookup"><span data-stu-id="54733-229">On the **myvm1** blade, click the ellipsis (**...**) button,  and then click **Restore VM**.</span></span>

    ![A restore parancs méretű VM](./media/oracle-backup-recovery/recover_vm_05.png)

5.  <span data-ttu-id="54733-231">Az a **visszaállítási pont kiválasztása** panelen, válassza ki a visszaállítani kívánt cikket, és végül **OK**.</span><span class="sxs-lookup"><span data-stu-id="54733-231">On the **Select restore point** blade, select the item that you want to restore, and then click **OK**.</span></span>

    ![Válassza ki a visszaállítási pont](./media/oracle-backup-recovery/recover_vm_06.png)

    <span data-ttu-id="54733-233">Ha engedélyezte az alkalmazáskonzisztens biztonsági mentését, egy függőleges kék színű sáv jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="54733-233">If you have enabled application-consistent backup, a vertical blue bar appears.</span></span>

6.  <span data-ttu-id="54733-234">Az a **konfiguráció visszaállítása** panelen válassza ki a virtuális gép nevét, válassza ki az erőforráscsoportot, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="54733-234">On the **Restore configuration** blade, select the virtual machine name, select the resource group, and then click **OK**.</span></span>

    ![Állítsa vissza a konfigurációs értékek](./media/oracle-backup-recovery/recover_vm_07.png)

7.  <span data-ttu-id="54733-236">Állítsa helyre a virtuális Gépet, kattintson a **visszaállítása** gombra.</span><span class="sxs-lookup"><span data-stu-id="54733-236">To restore the VM, click the **Restore** button.</span></span>

8.  <span data-ttu-id="54733-237">A visszaállítási folyamat állapotának megtekintéséhez kattintson **feladatok**, és kattintson a **biztonsági mentési feladatok**.</span><span class="sxs-lookup"><span data-stu-id="54733-237">To view the status of the restore process, click **Jobs**, and then click **Backup Jobs**.</span></span>

    ![Biztonsági mentési feladatok állapota parancs](./media/oracle-backup-recovery/recover_vm_08.png)

    <span data-ttu-id="54733-239">Az alábbi ábra a visszaállítási folyamat állapotának megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="54733-239">The following figure shows the status of the restore process:</span></span>

    ![A visszaállítási folyamat állapota](./media/oracle-backup-recovery/recover_vm_09.png)

### <a name="step-3-set-the-public-ip-address"></a><span data-ttu-id="54733-241">3. lépés: A nyilvános IP-cím beállítása</span><span class="sxs-lookup"><span data-stu-id="54733-241">Step 3: Set the public IP address</span></span>
<span data-ttu-id="54733-242">Miután visszaállította a virtuális gép, állítsa be a nyilvános IP-cím.</span><span class="sxs-lookup"><span data-stu-id="54733-242">After the VM is restored, set up the public IP address.</span></span>

1.  <span data-ttu-id="54733-243">A keresési mezőbe, írja be a **nyilvános IP-cím**.</span><span class="sxs-lookup"><span data-stu-id="54733-243">In the search box, enter **public IP address**.</span></span>

    ![Nyilvános IP-címek listája](./media/oracle-backup-recovery/create_ip_00.png)

2.  <span data-ttu-id="54733-245">Az a **nyilvános IP-címek** panelen kattintson a **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="54733-245">On the **Public IP addresses** blade, click **Add**.</span></span> <span data-ttu-id="54733-246">Az a **nyilvános IP-cím létrehozása** panelen a **neve**, válassza ki a nyilvános IP-név.</span><span class="sxs-lookup"><span data-stu-id="54733-246">On the **Create public IP address** blade, for **Name**, select the public IP name.</span></span> <span data-ttu-id="54733-247">Az **Erőforráscsoport** területen válassza a **Meglévő használata** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="54733-247">For **Resource group**, select **Use existing**.</span></span> <span data-ttu-id="54733-248">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="54733-248">Then, click **Create**.</span></span>

    ![IP-cím létrehozása](./media/oracle-backup-recovery/create_ip_01.png)

3.  <span data-ttu-id="54733-250">A nyilvános IP-címet társítani a hálózati illesztő a virtuális gép számára, és adja meg **myVMip**.</span><span class="sxs-lookup"><span data-stu-id="54733-250">To associate the public IP address with the network interface for the VM, search for and select **myVMip**.</span></span> <span data-ttu-id="54733-251">Kattintson a **társítása**.</span><span class="sxs-lookup"><span data-stu-id="54733-251">Then, click **Associate**.</span></span>

    ![IP-cím hozzárendelése](./media/oracle-backup-recovery/create_ip_02.png)

4.  <span data-ttu-id="54733-253">A **erőforrástípus**, jelölje be **hálózati illesztő**.</span><span class="sxs-lookup"><span data-stu-id="54733-253">For **Resource type**, select **Network interface**.</span></span> <span data-ttu-id="54733-254">Válassza ki, hogy az myVM példány által használt hálózati kapcsolat, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="54733-254">Select the network interface that is used by the myVM instance, and then click **OK**.</span></span>

    ![Válassza ki az erőforrás típusa és a hálózati adapter értékek](./media/oracle-backup-recovery/create_ip_03.png)

5.  <span data-ttu-id="54733-256">Keresse meg, és nyissa meg az, hogy van-e legelterjedtebb myVM példányát a portálról.</span><span class="sxs-lookup"><span data-stu-id="54733-256">Search for and open the instance of myVM that is ported from the portal.</span></span> <span data-ttu-id="54733-257">A virtuális Géphez társított IP-cím jelenik meg a myVM **áttekintése** panelen.</span><span class="sxs-lookup"><span data-stu-id="54733-257">The IP address that is associated with the VM appears on the myVM **Overview** blade.</span></span>

    ![IP-cím értéke](./media/oracle-backup-recovery/create_ip_04.png)

### <a name="step-4-connect-to-the-vm"></a><span data-ttu-id="54733-259">4. lépés: Csatlakoztassa a virtuális Gépet</span><span class="sxs-lookup"><span data-stu-id="54733-259">Step 4: Connect to the VM</span></span>

*   <span data-ttu-id="54733-260">Csatlakoztassa a virtuális Gépet, használja a következő parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="54733-260">To connect to the VM, use the following script:</span></span>

    ```bash 
    ssh <publicIpAddress>
    ```

### <a name="step-5-test-whether-the-database-is-accessible"></a><span data-ttu-id="54733-261">5. lépés: Az adatbázis elérhető-e tesztelése</span><span class="sxs-lookup"><span data-stu-id="54733-261">Step 5: Test whether the database is accessible</span></span>
*   <span data-ttu-id="54733-262">Kisegítő lehetőségek teszteléséhez a következő parancsprogram használata:</span><span class="sxs-lookup"><span data-stu-id="54733-262">To test accessibility, use the following script:</span></span>

    ```bash 
    $ sudo su - oracle
    $ sqlplus / as sysdba
    SQL> startup
    ```

    > [!IMPORTANT]
    > <span data-ttu-id="54733-263">Ha az adatbázis **indítási** parancs hibát generál, állítsa helyre az adatbázist, lásd: [6. lépés: az adatbázis használata RMAN](#step-6-optional-use-rman-to-recover-the-database).</span><span class="sxs-lookup"><span data-stu-id="54733-263">If the database **startup** command generates an error, to recover the database, see [Step 6: Use RMAN to recover the database](#step-6-optional-use-rman-to-recover-the-database).</span></span>

### <a name="step-6-optional-use-rman-to-recover-the-database"></a><span data-ttu-id="54733-264">6. lépés: (Nem kötelező) használata RMAN az adatbázis helyreállítása</span><span class="sxs-lookup"><span data-stu-id="54733-264">Step 6: (Optional) Use RMAN to recover the database</span></span>
*   <span data-ttu-id="54733-265">Állítsa helyre az adatbázist, használja a következő parancsfájlt:</span><span class="sxs-lookup"><span data-stu-id="54733-265">To recover the database, use the following script:</span></span>

    ```bash
    # sudo su - oracle
    $ rman target /
    RMAN> startup mount;
    RMAN> restore database;
    RMAN> recover database;
    RMAN> alter database open resetlogs;
    RMAN> SELECT * FROM scott.scott_table;
    ```

<span data-ttu-id="54733-266">A biztonsági mentési és helyreállítási az Azure Linux virtuális gép Oracle-adatbázishoz 12c adatbázis most már befejeződött.</span><span class="sxs-lookup"><span data-stu-id="54733-266">The backup and recovery of the Oracle Database 12c database on an Azure Linux VM is now finished.</span></span>

## <a name="delete-the-vm"></a><span data-ttu-id="54733-267">A virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="54733-267">Delete the VM</span></span>

<span data-ttu-id="54733-268">Ha már nincs szüksége a virtuális Gépet, a következő paranccsal távolítsa el az erőforráscsoportot, a virtuális gép és az összes kapcsolódó erőforrások:</span><span class="sxs-lookup"><span data-stu-id="54733-268">When you no longer need the VM, you can use the following command to remove the resource group, the VM, and all related resources:</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="54733-269">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="54733-269">Next steps</span></span>

[<span data-ttu-id="54733-270">Oktatóanyag: Hozzon létre magas rendelkezésre állású virtuális gépek</span><span class="sxs-lookup"><span data-stu-id="54733-270">Tutorial: Create highly available VMs</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="54733-271">Virtuális gép telepítése az Azure parancssori felület minták felfedezése</span><span class="sxs-lookup"><span data-stu-id="54733-271">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)



