---
title: "Oracle-adatbázishoz egy Azure virtuális gép aaaCreate |} Microsoft Docs"
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
ms.openlocfilehash: 83205154c3275d5f57b46c8acfb0cb4e5c68a412
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-oracle-database-in-an-azure-vm"></a><span data-ttu-id="356df-103">Oracle-adatbázishoz egy Azure virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="356df-103">Create an Oracle Database in an Azure VM</span></span>

<span data-ttu-id="356df-104">Ez az útmutató részletek hello Azure CLI toodeploy hello az Azure virtuális gép használata [Oracle piactér gyűjtemény kép](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) a rendezés toocreate 12 c Oracle-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="356df-104">This guide details using hello Azure CLI toodeploy an Azure virtual machine from hello [Oracle marketplace gallery image](https://azuremarketplace.microsoft.com/marketplace/apps/Oracle.OracleDatabase12102EnterpriseEdition?tab=Overview) in order toocreate an Oracle 12c database.</span></span> <span data-ttu-id="356df-105">Miután hello kiszolgáló van telepítve, kapcsolódni fog SSH-kapcsolaton keresztül a rendelés tooconfigure hello Oracle-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="356df-105">Once hello server is deployed, you will connect via SSH in order tooconfigure hello Oracle database.</span></span> 

<span data-ttu-id="356df-106">Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="356df-106">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="356df-107">Ha Ön tooinstall kiválasztása és hello CLI helyileg, a gyors üzembe helyezés van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="356df-107">If you choose tooinstall and use hello CLI locally, this quickstart requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="356df-108">Futtatás `az --version` toofind hello verziója.</span><span class="sxs-lookup"><span data-stu-id="356df-108">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="356df-109">Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="356df-109">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span>

## <a name="create-a-resource-group"></a><span data-ttu-id="356df-110">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="356df-110">Create a resource group</span></span>

<span data-ttu-id="356df-111">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="356df-111">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="356df-112">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="356df-112">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="356df-113">hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helyét.</span><span class="sxs-lookup"><span data-stu-id="356df-113">hello following example creates a resource group named *myResourceGroup* in hello *eastus* location.</span></span>

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```
## <a name="create-virtual-machine"></a><span data-ttu-id="356df-114">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="356df-114">Create virtual machine</span></span>

<span data-ttu-id="356df-115">egy virtuális gép (VM) toocreate hello használata [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="356df-115">toocreate a virtual machine (VM), use hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="356df-116">hello alábbi példakód létrehozza a virtuális gépek nevű `myVM`.</span><span class="sxs-lookup"><span data-stu-id="356df-116">hello following example creates a VM named `myVM`.</span></span> <span data-ttu-id="356df-117">SSH-kulcsok, azt is hoz létre, ha még nem léteznek a kulcs alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="356df-117">It also creates SSH keys, if they do not already exist in a default key location.</span></span> <span data-ttu-id="356df-118">toouse egy adott kulcsok beállításához használja a hello `--ssh-key-value` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="356df-118">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>  

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
    --size Standard_DS2_v2 \
    --admin-username azureuser \
    --generate-ssh-keys
```

<span data-ttu-id="356df-119">Hello virtuális gép létrehozása után az Azure CLI információkat a következő példa hasonló toohello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="356df-119">After you create hello VM, Azure CLI displays information similar toohello following example.</span></span> <span data-ttu-id="356df-120">Jegyezze fel a hello értékét `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="356df-120">Note hello value for `publicIpAddress`.</span></span> <span data-ttu-id="356df-121">A cím tooaccess hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="356df-121">You use this address tooaccess hello VM.</span></span>

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

## <a name="connect-toohello-vm"></a><span data-ttu-id="356df-122">Csatlakozás a virtuális gép toohello</span><span class="sxs-lookup"><span data-stu-id="356df-122">Connect toohello VM</span></span>

<span data-ttu-id="356df-123">az SSH-munkamenetet a virtuális gép, hello toocreate hello a következő parancsot használja.</span><span class="sxs-lookup"><span data-stu-id="356df-123">toocreate an SSH session with hello VM, use hello following command.</span></span> <span data-ttu-id="356df-124">Cserélje le hello IP-cím hello `publicIpAddress` értéket a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="356df-124">Replace hello IP address with hello `publicIpAddress` value for your VM.</span></span>

```bash 
ssh <publicIpAddress>
```

## <a name="create-hello-database"></a><span data-ttu-id="356df-125">Hello adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="356df-125">Create hello database</span></span>

<span data-ttu-id="356df-126">hello Piactéri lemezképhez hello Oracle szoftver már telepítve van.</span><span class="sxs-lookup"><span data-stu-id="356df-126">hello Oracle software is already installed on hello Marketplace image.</span></span> <span data-ttu-id="356df-127">Hozzon létre egy adatbázist az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="356df-127">Create a sample database as follows.</span></span> 

1.  <span data-ttu-id="356df-128">Váltás toohello *oracle* felügyelő, majd a naplózás inicializálásakor hello figyelő:</span><span class="sxs-lookup"><span data-stu-id="356df-128">Switch toohello *oracle* superuser, then initialize hello listener for logging:</span></span>

    ```bash
    $ sudo su - oracle
    $ lsnrctl start
    ```

    <span data-ttu-id="356df-129">hello hasonló toohello következő kimenete:</span><span class="sxs-lookup"><span data-stu-id="356df-129">hello output is similar toohello following:</span></span>

    ```bash
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

2.  <span data-ttu-id="356df-130">hello adatbázis létrehozása:</span><span class="sxs-lookup"><span data-stu-id="356df-130">Create hello database:</span></span>

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

    <span data-ttu-id="356df-131">Néhány perc toocreate hello adatbázis vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="356df-131">It takes a few minutes toocreate hello database.</span></span>

3. <span data-ttu-id="356df-132">Oracle változók megadása</span><span class="sxs-lookup"><span data-stu-id="356df-132">Set Oracle variables</span></span>

<span data-ttu-id="356df-133">Csatlakozás előtt van szüksége a két tooset környezeti változók: *ORACLE_HOME* és *ORACLE_SID*.</span><span class="sxs-lookup"><span data-stu-id="356df-133">Before you connect, you need tooset two environment variables: *ORACLE_HOME* and *ORACLE_SID*.</span></span>

```bash
ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
ORACLE_SID=cdb1; export ORACLE_SID
```
<span data-ttu-id="356df-134">ORACLE_HOME és ORACLE_SID változók toohello .bashrc fájl is adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="356df-134">You also can add ORACLE_HOME and ORACLE_SID variables toohello .bashrc file.</span></span> <span data-ttu-id="356df-135">A környezeti változók hello a későbbi bejelentkezések volna mentse. Erősítse meg a következő hello utasítás hozzá vannak adva toohello `~/.bashrc` a választott szerkesztővel fájlt.</span><span class="sxs-lookup"><span data-stu-id="356df-135">This would save hello environment variables for future sign-ins. Confirm hello following statements have been added toohello `~/.bashrc` file using editor of your choice.</span></span>

```bash
# Add ORACLE_HOME. 
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1 
# Add ORACLE_SID. 
export ORACLE_SID=cdb1 
```

## <a name="oracle-em-express-connectivity"></a><span data-ttu-id="356df-136">Oracle EM Express kapcsolat</span><span class="sxs-lookup"><span data-stu-id="356df-136">Oracle EM Express connectivity</span></span>

<span data-ttu-id="356df-137">Grafikus felügyeleti eszköz használható tooexplore hello adatbázis, az Oracle EM Express beállítása.</span><span class="sxs-lookup"><span data-stu-id="356df-137">For a GUI management tool that you can use tooexplore hello database, set up Oracle EM Express.</span></span> <span data-ttu-id="356df-138">tooconnect tooOracle EM Express, először be kell állítania az Oracle hello port.</span><span class="sxs-lookup"><span data-stu-id="356df-138">tooconnect tooOracle EM Express, you must first set up hello port in Oracle.</span></span> 

1. <span data-ttu-id="356df-139">Csatlakozás tooyour adatbázist sqlplus használatával:</span><span class="sxs-lookup"><span data-stu-id="356df-139">Connect tooyour database using sqlplus:</span></span>

    ```bash
    sqlplus / as sysdba
    ```

2. <span data-ttu-id="356df-140">A csatlakozás után a EM expressz hello port 5502 beállítása</span><span class="sxs-lookup"><span data-stu-id="356df-140">Once connected, set hello port 5502 for EM Express</span></span>

    ```bash
    exec DBMS_XDB_CONFIG.SETHTTPSPORT(5502);
    ```

3. <span data-ttu-id="356df-141">Nyissa meg hello tároló PDB1, ha nem már megnyitott, de az első hello állapotát:</span><span class="sxs-lookup"><span data-stu-id="356df-141">Open hello container PDB1 if not already opened, but first check hello status:</span></span>

    ```bash
    select con_id, name, open_mode from v$pdbs;
    ```

    <span data-ttu-id="356df-142">hello hasonló toohello következő kimenete:</span><span class="sxs-lookup"><span data-stu-id="356df-142">hello output is similar toohello following:</span></span>

    ```bash
      CON_ID NAME                           OPEN_MODE 
      ----------- ------------------------- ---------- 
      2           PDB$SEED                  READ ONLY 
      3           PDB1                      MOUNT
    ```

4. <span data-ttu-id="356df-143">Ha a OPEN_MODE hello `PDB1` nincs OLVASÁSI ÍRNI, majd futtassa a hello következőket parancsok tooopen PDB1:</span><span class="sxs-lookup"><span data-stu-id="356df-143">If hello OPEN_MODE for `PDB1` is not READ WRITE, then run hello followings commands tooopen PDB1:</span></span>

   ```bash
    alter session set container=pdb1;
    alter database open;
   ```

<span data-ttu-id="356df-144">Tootype kell `quit` tooend hello sqlplus munkamenet és típus `exit` toologout hello oracle felhasználó.</span><span class="sxs-lookup"><span data-stu-id="356df-144">You need tootype `quit` tooend hello sqlplus session and type `exit` toologout of hello oracle user.</span></span>

## <a name="automate-database-startup-and-shutdown"></a><span data-ttu-id="356df-145">Adatbázis indítási és leállítási automatizálásához</span><span class="sxs-lookup"><span data-stu-id="356df-145">Automate database startup and shutdown</span></span>

<span data-ttu-id="356df-146">Oracle-adatbázishoz hello alapértelmezés szerint automatikusan hello VM újraindításakor nem indul el.</span><span class="sxs-lookup"><span data-stu-id="356df-146">hello Oracle database by default doesn't automatically start when you restart hello VM.</span></span> <span data-ttu-id="356df-147">tooset hello Oracle adatbázis toostart fel automatikusan, először bejelentkezik rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="356df-147">tooset up hello Oracle database toostart automatically, first sign in as root.</span></span> <span data-ttu-id="356df-148">Ezt követően létrehozása, és egyes rendszer fájlok frissítése.</span><span class="sxs-lookup"><span data-stu-id="356df-148">Then, create and update some system files.</span></span>

1. <span data-ttu-id="356df-149">Jelentkezzen a legfelső szintű</span><span class="sxs-lookup"><span data-stu-id="356df-149">Sign on as root</span></span>
    ```bash
    sudo su -
    ```

2.  <span data-ttu-id="356df-150">A kedvenc szerkesztővel szerkessze a hello fájlt `/etc/oratab` , és módosítsa a hello alapértelmezett `N` túl`Y`:</span><span class="sxs-lookup"><span data-stu-id="356df-150">Using your favorite editor, edit hello file `/etc/oratab` and change hello default `N` too`Y`:</span></span>

    ```bash
    cdb1:/u01/app/oracle/product/12.1.0/dbhome_1:Y
    ```

3.  <span data-ttu-id="356df-151">Hozzon létre egy fájlt `/etc/init.d/dbora` és a Beillesztés hello a következő tartalmát:</span><span class="sxs-lookup"><span data-stu-id="356df-151">Create a file named `/etc/init.d/dbora` and paste hello following contents:</span></span>

    ```
    #!/bin/sh
    # chkconfig: 345 99 10
    # Description: Oracle auto start-stop script.
    #
    # Set ORA_HOME toobe equivalent too$ORACLE_HOME.
    ORA_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
    ORA_OWNER=oracle

    case "$1" in
    'start')
        # Start hello Oracle databases:
        # hello following command assumes that hello Oracle sign-in
        # will not prompt hello user for any values.
        # Remove "&" if you don't want startup as a background process.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbstart $ORA_HOME" &
        touch /var/lock/subsys/dbora
        ;;

    'stop')
        # Stop hello Oracle databases:
        # hello following command assumes that hello Oracle sign-in
        # will not prompt hello user for any values.
        su - $ORA_OWNER -c "$ORA_HOME/bin/dbshut $ORA_HOME" &
        rm -f /var/lock/subsys/dbora
        ;;
    esac
    ```

4.  <span data-ttu-id="356df-152">A fájlok engedélyeinek módosítása *chmod* az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="356df-152">Change permissions on files with *chmod* as follows:</span></span>

    ```bash
    chgrp dba /etc/init.d/dbora
    chmod 750 /etc/init.d/dbora
    ```

5.  <span data-ttu-id="356df-153">Indítási és leállítási szimbolikus hivatkozásokat hoz létre a a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="356df-153">Create symbolic links for startup and shutdown as follows:</span></span>

    ```bash
    ln -s /etc/init.d/dbora /etc/rc.d/rc0.d/K01dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc3.d/S99dbora
    ln -s /etc/init.d/dbora /etc/rc.d/rc5.d/S99dbora
    ```

6.  <span data-ttu-id="356df-154">tootest a módosításokat, indítsa újra a virtuális gép hello:</span><span class="sxs-lookup"><span data-stu-id="356df-154">tootest your changes, restart hello VM:</span></span>

    ```bash
    reboot
    ```

## <a name="open-ports-for-connectivity"></a><span data-ttu-id="356df-155">A hálózati kapcsolatot megnyitott portok</span><span class="sxs-lookup"><span data-stu-id="356df-155">Open ports for connectivity</span></span>

<span data-ttu-id="356df-156">hello végső feladat tooconfigure van néhány külső végpontok száma.</span><span class="sxs-lookup"><span data-stu-id="356df-156">hello final task is tooconfigure some external endpoints.</span></span> <span data-ttu-id="356df-157">másolatot tooset hello Azure hálózati biztonsági csoportot, amely védi a virtuális gép hello, először lépjen ki a virtuális gép (kell rendelkeznie lett kezdődött SSH kívül ha újraindítás az előző lépésben) hello az SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="356df-157">tooset up hello Azure Network Security Group that protects hello VM, first exit your SSH session in hello VM (should have been kicked out of SSH when rebooting in previous step).</span></span> 

1.  <span data-ttu-id="356df-158">tooopen hello végpont tooaccess hello Oracle-adatbázishoz távolról, használja a hálózati biztonsági csoport szabály létrehozása [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="356df-158">tooopen hello endpoint that you use tooaccess hello Oracle database remotely, create a Network Security Group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) as follows:</span></span> 

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup\
        --nsg-name myVmNSG \
        --name allow-oracle \
        --protocol tcp \
        --priority 1001 \
        --destination-port-range 1521
    ```

2.  <span data-ttu-id="356df-159">tooopen hello végpont Oracle EM Express tooaccess távolról, használja a hálózati biztonsági csoport szabály létrehozása [az hálózati nsg-szabály létrehozása](/cli/azure/network/nsg/rule#create) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="356df-159">tooopen hello endpoint that you use tooaccess Oracle EM Express remotely, create a Network Security Group rule with [az network nsg rule create](/cli/azure/network/nsg/rule#create) as follows:</span></span>

    ```azurecli-interactive
    az network nsg rule create \
        --resource-group myResourceGroup \
        --nsg-name myVmNSG \
        --name allow-oracle-EM \
        --protocol tcp \
        --priority 1002 \
        --destination-port-range 5502
    ```

3. <span data-ttu-id="356df-160">Szükség esetén hello nyilvános IP-címet a virtuális gép újra az beszerzése [az hálózati nyilvános ip-megjelenítése](/cli/azure/network/public-ip#show) az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="356df-160">If needed, obtain hello public IP address of your VM again with [az network public-ip show](/cli/azure/network/public-ip#show) as follows:</span></span>

    ```azurecli-interactive
    az network public-ip show \
        --resource-group myResourceGroup \
        --name myVMPublicIP \
        --query [ipAddress] \
        --output tsv
    ```

4.  <span data-ttu-id="356df-161">Csatlakoztassa a böngésző EM Express.</span><span class="sxs-lookup"><span data-stu-id="356df-161">Connect EM Express from your browser.</span></span> <span data-ttu-id="356df-162">Ügyeljen arra, hogy a böngésző kompatibilis EM expressz (Flash esetén):</span><span class="sxs-lookup"><span data-stu-id="356df-162">Make sure your browser is compatible with EM Express (Flash install is required):</span></span> 

    ```
    https://<VM ip address or hostname>:5502/em
    ```

<span data-ttu-id="356df-163">Hello segítségével bejelentkezhet **SYS** fiókot, és ellenőrizze a hello **SYSDBA csoport szerint** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="356df-163">You can log in by using hello **SYS** account, and check hello **as sysdba** checkbox.</span></span> <span data-ttu-id="356df-164">Használjon hello jelszó **OraPasswd1** telepítése során beállított.</span><span class="sxs-lookup"><span data-stu-id="356df-164">Use hello password **OraPasswd1** that you set during installation.</span></span> 

![Képernyőfelvétel a hello Oracle OEM Express bejelentkezési oldal](./media/oracle-quick-start/oracle_oem_express_login.png)

## <a name="clean-up-resources"></a><span data-ttu-id="356df-166">Az erőforrások eltávolítása</span><span class="sxs-lookup"><span data-stu-id="356df-166">Clean up resources</span></span>

<span data-ttu-id="356df-167">Miután befejezte az első Oracle adatbázis fel az Azure-on, és hello virtuális gép már nem szükséges, használhatja a hello [az csoport törlése](/cli/azure/group#delete) tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások parancsot.</span><span class="sxs-lookup"><span data-stu-id="356df-167">Once you have finished exploring your first Oracle database on Azure and hello VM is no longer needed, you can use hello [az group delete](/cli/azure/group#delete) command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="356df-168">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="356df-168">Next steps</span></span>

<span data-ttu-id="356df-169">Tudnivalók más [Oracle megoldások Azure](oracle-considerations.md).</span><span class="sxs-lookup"><span data-stu-id="356df-169">Learn about other [Oracle solutions on Azure](oracle-considerations.md).</span></span> 

<span data-ttu-id="356df-170">Próbálja meg hello [telepítése és konfigurálása Oracle automatikus tárhely-kezelés](configure-oracle-asm.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="356df-170">Try hello [Installing and Configuring Oracle Automated Storage Management](configure-oracle-asm.md) tutorial.</span></span>
