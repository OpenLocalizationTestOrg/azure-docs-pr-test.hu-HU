---
title: "Oracle-adatbázishoz egy Azure virtuális gép létrehozása |} Microsoft Docs"
description: "Gyorsan karban lehessen az Oracle-adatbázishoz 12c adatbázis mentése és az Azure környezetben futó."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/17/2017
ms.author: rclaus
ms.openlocfilehash: 8683b016c4db2c66fb1dd994405b70c3d137a7fc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="create-an-oracle-database-in-an-azure-vm"></a><span data-ttu-id="c3132-103">Oracle-adatbázishoz egy Azure virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="c3132-103">Create an Oracle Database in an Azure VM</span></span>

<span data-ttu-id="c3132-104">Ez az útmutató részletek központi telepítése az Azure virtuális gépként az Azure parancssori felület használatával a [Oracle piactér gyűjtemény kép](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) létrehozásához 12 c Oracle-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="c3132-104">This guide details using the Azure CLI to deploy an Azure virtual machine from the [Oracle marketplace gallery image](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) in order to create an Oracle 12c database.</span></span> <span data-ttu-id="c3132-105">Ha a kiszolgáló van telepítve, kapcsolódni fog SSH-kapcsolaton keresztül az Oracle-adatbázishoz való konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="c3132-105">Once the server is deployed, you will connect via SSH in order to configure the Oracle database.</span></span> 

<span data-ttu-id="c3132-106">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="c3132-106">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="c3132-107">Ha a CLI helyi telepítését és használatát választja, akkor ehhez a gyorsútmutatóhoz az Azure CLI 2.0.4-es vagy újabb verziójára lesz szükség.</span><span class="sxs-lookup"><span data-stu-id="c3132-107">If you choose to install and use the CLI locally, this quickstart requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="c3132-108">A verzió azonosításához futtassa a következőt: `az --version`.</span><span class="sxs-lookup"><span data-stu-id="c3132-108">Run `az --version` to find the version.</span></span> <span data-ttu-id="c3132-109">Ha telepíteni vagy frissíteni szeretne: [Az Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="c3132-109">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="c3132-110">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="c3132-110">Create a resource group</span></span>

<span data-ttu-id="c3132-111">Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="c3132-111">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="c3132-112">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="c3132-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="c3132-113">A következő példában létrehozunk egy *myResourceGroup* nevű erőforráscsoportot az *eastus* helyen.</span><span class="sxs-lookup"><span data-stu-id="c3132-113">The following example creates a resource group named *myResourceGroup* in the *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
## <a name="create-virtual-machine"></a><span data-ttu-id="c3132-114">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="c3132-114">Create virtual machine</span></span>

<span data-ttu-id="c3132-115">Hozzon létre egy virtuális gépet (VM), használja a [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="c3132-115">To create a virtual machine (VM), use the [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="c3132-116">Az alábbi példa egy `myVM` nevű virtuális gépet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c3132-116">The following example creates a VM named `myVM`.</span></span> <span data-ttu-id="c3132-117">SSH-kulcsok, azt is hoz létre, ha még nem léteznek a kulcs alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="c3132-117">It also creates SSH keys, if they do not already exist in a default key location.</span></span> <span data-ttu-id="c3132-118">Ha konkrét kulcsokat szeretné használni, használja az `--ssh-key-value` beállítást.</span><span class="sxs-lookup"><span data-stu-id="c3132-118">To use a specific set of keys, use the `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="c3132-119">A virtuális gép létrehozása után a Azure CLI-t az alábbi példához hasonló információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="c3132-119">After you create the VM, Azure CLI displays information similar to the following example.</span></span> <span data-ttu-id="c3132-120">Vegye figyelembe a következő `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="c3132-120">Note the value for `publicIpAddress`.</span></span> <span data-ttu-id="c3132-121">Ez a cím a virtuális gép elérésére használhat.</span><span class="sxs-lookup"><span data-stu-id="c3132-121">You use this address to access the VM.</span></span>

```azurecli
{
  "fqdns": "",
  "id": "/subscriptions/{snip}/resourceGroups/myResourceGroup/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "westus",
  "macAddress": "00-0D-3A-36-2F-56",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "13.64.104.241",
  "resourceGroup": "myResourceGroup"
}
```

## <a name="connect-to-the-vm"></a><span data-ttu-id="c3132-122">Kapcsolódás a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="c3132-122">Connect to the VM</span></span>

<span data-ttu-id="c3132-123">A virtuális gép SSH-munkamenetet létrehozni, használja a következő parancsot.</span><span class="sxs-lookup"><span data-stu-id="c3132-123">To create an SSH session with the VM, use the following command.</span></span> <span data-ttu-id="c3132-124">Cserélje le az IP-cím a `publicIpAddress` értéket a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="c3132-124">Replace the IP address with the `publicIpAddress` value for your VM.</span></span>

```bash 
ssh <publicIpAddress>
```

## <a name="create-the-database"></a><span data-ttu-id="c3132-125">Az adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="c3132-125">Create the database</span></span>

<span data-ttu-id="c3132-126">Az Oracle szoftver már telepítve van a Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="c3132-126">The Oracle software is already installed on the Marketplace image.</span></span> <span data-ttu-id="c3132-127">Hozzon létre egy adatbázist az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="c3132-127">Create a sample database as follows.</span></span> 

1.  <span data-ttu-id="c3132-128">Váltás a *oracle* felügyelő, majd a naplózás a figyelő inicializálása:</span><span class="sxs-lookup"><span data-stu-id="c3132-128">Switch to the *oracle* superuser, then initialize the listener for logging:</span></span>

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    ```

    <span data-ttu-id="c3132-129">A kimenet a következő példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="c3132-129">The output is similar to the following:</span></span>

    ```bash
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

2.  <span data-ttu-id="c3132-130">Az adatbázis létrehozása:</span><span class="sxs-lookup"><span data-stu-id="c3132-130">Create the database:</span></span>

    ```bash
    dbca -silent \
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

    <span data-ttu-id="c3132-131">Az adatbázis létrehozása néhány percet vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="c3132-131">It takes a few minutes to create the database.</span></span>

3. <span data-ttu-id="c3132-132">Oracle változók megadása</span><span class="sxs-lookup"><span data-stu-id="c3132-132">Set Oracle variables</span></span>

<span data-ttu-id="c3132-133">Csatlakozás előtt be kell állítani a két környezeti változók: *ORACLE_HOME* és *ORACLE_SID*.</span><span class="sxs-lookup"><span data-stu-id="c3132-133">Before you connect, you need to set two environment variables: *ORACLE_HOME* and *ORACLE_SID*.</span></span>

```bash
ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
ORACLE_SID=cdb1; export ORACLE_SID
```
<span data-ttu-id="c3132-134">ORACLE_HOME és ORACLE_SID változók .bashrc fájl adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="c3132-134">You also can add ORACLE_HOME and ORACLE_SID variables to the .bashrc file.</span></span> <span data-ttu-id="c3132-135">Ez szeretné menteni a környezeti változók a későbbi bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="c3132-135">This would save the environment variables for future sign-ins.</span></span> <span data-ttu-id="c3132-136">Győződjön meg arról, a következő utasításokat lettek hozzáadva a `~/.bashrc` a választott szerkesztővel fájlt.</span><span class="sxs-lookup"><span data-stu-id="c3132-136">Confirm the following statements have been added to the `~/.bashrc` file using editor of your choice.</span></span>

```bash
# Add ORACLE_HOME. 
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1 
# Add ORACLE_SID. 
export ORACLE_SID=cdb1 
```

## <a name="oracle-em-express-connectivity"></a><span data-ttu-id="c3132-137">Oracle EM Express kapcsolat</span><span class="sxs-lookup"><span data-stu-id="c3132-137">Oracle EM Express connectivity</span></span>

<span data-ttu-id="c3132-138">A grafikus felhasználói felület segítségével megismerkedhet az adatbázist, és állítsa be az Oracle EM Express felügyeleti eszköz.</span><span class="sxs-lookup"><span data-stu-id="c3132-138">For a GUI management tool that you can use to explore the database, set up Oracle EM Express.</span></span> <span data-ttu-id="c3132-139">Szeretne csatlakozni az Oracle EM Express, az Oracle port először meg kell adnia.</span><span class="sxs-lookup"><span data-stu-id="c3132-139">To connect to Oracle EM Express, you must first set up the port in Oracle.</span></span> 

1. <span data-ttu-id="c3132-140">Kapcsolódás saját adatbázishoz sqlplus használatával:</span><span class="sxs-lookup"><span data-stu-id="c3132-140">Connect to your database using sqlplus:</span></span>

    ```bash
    sqlplus / as sysdba
    ```

2. <span data-ttu-id="c3132-141">A csatlakozás után állítsa be a port 5502 EM Express</span><span class="sxs-lookup"><span data-stu-id="c3132-141">Once connected, set the port 5502 for EM Express</span></span>

    ```bash
    exec DBMS_XDB_CONFIG.SETHTTPSPORT(5502);
    ```

3. <span data-ttu-id="c3132-142">Nyissa meg a tároló PDB1, ha nem már megnyitott, de az első ellenőrizze az állapotát:</span><span class="sxs-lookup"><span data-stu-id="c3132-142">Open the container PDB1 if not already opened, but first check the status:</span></span>

    ```bash
    select con_id, name, open_mode from v$pdbs;
    ```

    <span data-ttu-id="c3132-143">A kimenet a következő példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="c3132-143">The output is similar to the following:</span></span>

    ```bash
      CON_ID NAME                           OPEN_MODE 
      ----------- ------------------------- ---------- 
      2           PDB$SEED                  READ ONLY 
      3           PDB1                      MOUNT
    ```

4. <span data-ttu-id="c3132-144">Ha a OPEN_MODE `PDB1` nincs OLVASÁSI ÍRNI, majd a PDB1 nyissa meg a következőket parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="c3132-144">If the OPEN_MODE for `PDB1` is not READ WRITE, then run the followings commands to open PDB1:</span></span>

   ```bash
    alter session set container=pdb1;
    alter database open;
   ```

<span data-ttu-id="c3132-145">Önnek kell beírnia `quit` sqlplus munkamenet és típus befejezéséhez `exit` oracle felhasználó kijelentkezik.</span><span class="sxs-lookup"><span data-stu-id="c3132-145">You need to type `quit` to end the sqlplus session and type `exit` to logout of the oracle user.</span></span>

## <a name="automate-database-startup-and-shutdown"></a><span data-ttu-id="c3132-146">Adatbázis indítási és leállítási automatizálásához</span><span class="sxs-lookup"><span data-stu-id="c3132-146">Automate database startup and shutdown</span></span>

<span data-ttu-id="c3132-147">Alapértelmezés szerint az Oracle-adatbázishoz nem indul el automatikusan a virtuális gép újraindításakor.</span><span class="sxs-lookup"><span data-stu-id="c3132-147">The Oracle database by default doesn't automatically start when you restart the VM.</span></span> <span data-ttu-id="c3132-148">Az Oracle-adatbázishoz be automatikus indításra, először jelentkezzen be rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="c3132-148">To set up the Oracle database to start automatically, first sign in as root.</span></span> <span data-ttu-id="c3132-149">Ezt követően létrehozása, és egyes rendszer fájlok frissítése.</span><span class="sxs-lookup"><span data-stu-id="c3132-149">Then, create and update some system files.</span></span>

1. <span data-ttu-id="c3132-150">Jelentkezzen a legfelső szintű</span><span class="sxs-lookup"><span data-stu-id="c3132-150">Sign on as root</span></span>
    ```bash
    sudo su -
    ```

2.  <span data-ttu-id="c3132-151">A kedvenc szerkesztővel szerkessze a fájlt `/etc/oratab` , és módosítsa az alapértelmezett `N` való `Y`:</span><span class="sxs-lookup"><span data-stu-id="c3132-151">Using your favorite editor, edit the file `/etc/oratab` and change the default `N` to `Y`:</span></span>

    ```bash
    cdb1:/u01/app/oracle/product/12.1.0/dbhome_1:Y
    ```

3.  <span data-ttu-id="c3132-152">Hozzon létre egy fájlt `/etc/init.d/dbora` , majd illessze be az alábbiakat:</span><span class="sxs-lookup"><span data-stu-id="c3132-152">Create a file named `/etc/init.d/dbora` and paste the following contents:</span></span>

    ```
    #!/bin/sh
    # chkconfig: 345 99 10
    # Description: Oracle auto start-stop script.
    #
    # Set ORA_HOME to be equivalent to $ORACLE_HOME.
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle

    case "$1" in
    'start')
        # Start the Oracle databases:
        # The following command assumes that the Oracle sign-in
        # will not prompt the user for any values.
        # Remove "&" if you don't want startup as a background process.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" &
        touch /var/lock/subsys/dbora
        ;;

    'stop')
        # Stop the Oracle databases:
        # The following command assumes that the Oracle sign-in
        # will not prompt the user for any values.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" &
        rm -f /var/lock/subsys/dbora
        ;;
    esac
    ```

4.  <span data-ttu-id="c3132-153">A fájlok engedélyeinek módosítása *chmod* az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c3132-153">Change permissions on files with *chmod* as follows:</span></span>

    ```bash
    chgrp dba /etc/init.d/dbora
    chmod 750 /etc/init.d/dbora
    ```

5.  <span data-ttu-id="c3132-154">Indítási és leállítási szimbolikus hivatkozásokat hoz létre a a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="c3132-154">Create symbolic links for startup and shutdown as follows:</span></span>

    ```bash
    ln -s /etc/init.d/dbora /etc/rc.d/rc0.d/K01dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc3.d/S99dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc5.d/S99dbora
    ```

6.  <span data-ttu-id="c3132-155">A módosítások ellenőrzéséhez indítsa újra a virtuális Gépet:</span><span class="sxs-lookup"><span data-stu-id="c3132-155">To test your changes, restart the VM:</span></span>

    ```bash
    reboot
    ```

## <a name="open-ports-for-connectivity"></a><span data-ttu-id="c3132-156">A hálózati kapcsolatot megnyitott portok</span><span class="sxs-lookup"><span data-stu-id="c3132-156">Open ports for connectivity</span></span>

<span data-ttu-id="c3132-157">Az utolsó feladat be nem konfigurálhatja az egyes külső végpontok száma.</span><span class="sxs-lookup"><span data-stu-id="c3132-157">The final task is to configure some external endpoints.</span></span> <span data-ttu-id="c3132-158">Az Azure hálózati biztonsági csoport, amely védi a virtuális gép beállításához lépjen ki az SSH-munkamenetet a virtuális gép (kell rendelkeznie lett kezdődött SSH kívül ha újraindítás az előző lépésben).</span><span class="sxs-lookup"><span data-stu-id="c3132-158">To set up the Azure Network Security Group that protects the VM, first exit your SSH session in the VM (should have been kicked out of SSH when rebooting in previous step).</span></span> 

1.  <span data-ttu-id="c3132-159">Nyissa meg a végpont, amelyekkel távolról fér hozzá az Oracle-adatbázishoz, hozzon létre egy hálózati biztonsági csoport szabály [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c3132-159">To open the endpoint that you use to access the Oracle database remotely, create a Network Security Group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) as follows:</span></span> 

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup\
        --nsg-name myVmNSG \
        --name allow-oracle \
        --protocol tcp \
        --priority 1001 \
        --destination-port-range 1521
    ```

2.  <span data-ttu-id="c3132-160">Nyissa meg a végpont Oracle EM Express távoli eléréséhez használt, hozzon létre egy hálózati biztonsági csoport szabály [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c3132-160">To open the endpoint that you use to access Oracle EM Express remotely, create a Network Security Group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) as follows:</span></span>

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup \
        --nsg-name myVmNSG \
        --name allow-oracle-EM \
        --protocol tcp \
        --priority 1002 \
        --destination-port-range 5502
    ```

3. <span data-ttu-id="c3132-161">Ha szükséges, szerezze be a nyilvános IP-címet a virtuális gép újra [az hálózati nyilvános ip-megjelenítése](/cli/azure/network/public-ip#show) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="c3132-161">If needed, obtain the public IP address of your VM again with [az network public-ip show](/cli/azure/network/public-ip#show) as follows:</span></span>

    ```azurecli-interactive
    az network public-ip show \
        --resource-group myResourceGroup \
        --name myVMPublicIP \
        --query [ipAddress] \
        --output tsv
    ```

4.  <span data-ttu-id="c3132-162">Csatlakoztassa a böngésző EM Express.</span><span class="sxs-lookup"><span data-stu-id="c3132-162">Connect EM Express from your browser.</span></span> <span data-ttu-id="c3132-163">Ügyeljen arra, hogy a böngésző kompatibilis EM expressz (Flash esetén):</span><span class="sxs-lookup"><span data-stu-id="c3132-163">Make sure your browser is compatible with EM Express (Flash install is required):</span></span> 

    ```
    https://<VM ip address or hostname>:5502/em
    ```

<span data-ttu-id="c3132-164">A bejelentkezhet a **SYS** fiókot, és ellenőrizze a **SYSDBA csoport szerint** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="c3132-164">You can log in by using the **SYS** account, and check the **as sysdba** checkbox.</span></span> <span data-ttu-id="c3132-165">A jelszó használata **OraPasswd1** telepítése során beállított.</span><span class="sxs-lookup"><span data-stu-id="c3132-165">Use the password **OraPasswd1** that you set during installation.</span></span> 

![Az Oracle OEM Express bejelentkezési oldalát bemutató képernyőkép](./media/oracle-quick-start/oracle_oem_express_login.png)

## <a name="clean-up-resources"></a><span data-ttu-id="c3132-167">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="c3132-167">Clean up resources</span></span>

<span data-ttu-id="c3132-168">Miután befejezte az első Oracle adatbázis Azure felfedezése és a virtuális gép már nem szükséges, használhatja a [az csoport törlése](/cli/azure/group#delete) parancsot a távolítsa el az erőforráscsoportot, a virtuális gép, és minden kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="c3132-168">Once you have finished exploring your first Oracle database on Azure and the VM is no longer needed, you can use the [az group delete](/cli/azure/group#delete) command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="c3132-169">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c3132-169">Next steps</span></span>

<span data-ttu-id="c3132-170">Tudnivalók más [Oracle megoldások Azure](oracle-considerations.md).</span><span class="sxs-lookup"><span data-stu-id="c3132-170">Learn about other [Oracle solutions on Azure](oracle-considerations.md).</span></span> 

<span data-ttu-id="c3132-171">Próbálja meg a [telepítése és konfigurálása Oracle automatikus tárhely-kezelés](configure-oracle-asm.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="c3132-171">Try the [Installing and Configuring Oracle Automated Storage Management](configure-oracle-asm.md) tutorial.</span></span>