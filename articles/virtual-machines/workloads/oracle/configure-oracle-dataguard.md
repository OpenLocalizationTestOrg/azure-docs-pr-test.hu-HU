---
title: "egy Azure Linux virtuális gép Oracle Data Guard aaaImplement |} Microsoft Docs"
description: "Gyorsan karban lehessen Oracle Data Guard mentése és az Azure környezetben futna."
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
ms.date: 05/10/2017
ms.author: rclaus
ms.openlocfilehash: 6bb530098737e3ca7dd8bab3f4306ecbb620f3f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="implement-oracle-data-guard-on-an-azure-linux-virtual-machine"></a><span data-ttu-id="91919-103">Oracle Data Guard valósítja meg az Azure Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="91919-103">Implement Oracle Data Guard on an Azure Linux virtual machine</span></span> 

<span data-ttu-id="91919-104">Az Azure CLI használt toocreate és hello parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="91919-104">Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="91919-105">Ez a cikk ismerteti, hogyan toouse Azure CLI toodeploy Oracle-adatbázishoz 12c adatbázist hello Azure Piactéri lemezképhez.</span><span class="sxs-lookup"><span data-stu-id="91919-105">This article describes how toouse Azure CLI toodeploy an Oracle Database 12c database from hello Azure Marketplace image.</span></span> <span data-ttu-id="91919-106">Ez a cikk megjeleníti a, lépésről lépésre, hogyan tooinstall és Data Guard konfigurálása az Azure virtuális gépen (VM).</span><span class="sxs-lookup"><span data-stu-id="91919-106">This article then shows you, step by step, how tooinstall and configure Data Guard on an Azure virtual machine (VM).</span></span>

<span data-ttu-id="91919-107">Mielőtt elkezdené, győződjön meg arról, hogy telepítve van-e az Azure parancssori felület.</span><span class="sxs-lookup"><span data-stu-id="91919-107">Before you start, make sure that Azure CLI is installed.</span></span> <span data-ttu-id="91919-108">További információkért lásd: hello [Azure parancssori felület telepítési útmutató](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="91919-108">For more information, see hello [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-hello-environment"></a><span data-ttu-id="91919-109">Hello környezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="91919-109">Prepare hello environment</span></span>
### <a name="assumptions"></a><span data-ttu-id="91919-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="91919-110">Assumptions</span></span>

<span data-ttu-id="91919-111">Oracle Data Guard tooinstall, kell toocreate két Azure-alapú virtuális gépek hello ugyanahhoz a rendelkezésre állási csoporthoz:</span><span class="sxs-lookup"><span data-stu-id="91919-111">tooinstall Oracle Data Guard, you need toocreate two Azure VMs on hello same availability set:</span></span>

- <span data-ttu-id="91919-112">hello elsődleges virtuális gép (myVM1) rendelkezik egy futó Oracle-példány.</span><span class="sxs-lookup"><span data-stu-id="91919-112">hello primary VM (myVM1) has a running Oracle instance.</span></span>
- <span data-ttu-id="91919-113">készenléti állapotban lévő virtuális gép (myVM2) rendelkezik hello Oracle szoftvereket csak hello.</span><span class="sxs-lookup"><span data-stu-id="91919-113">hello standby VM (myVM2) has hello Oracle software installed only.</span></span>

<span data-ttu-id="91919-114">hello használata virtuális gépek toocreate hello Piactéri lemezképhez Oracle: Oracle-adatbázis-Ee:12.1.0.2:latest.</span><span class="sxs-lookup"><span data-stu-id="91919-114">hello Marketplace image that you use toocreate hello VMs is Oracle:Oracle-Database-Ee:12.1.0.2:latest.</span></span>

### <a name="sign-in-tooazure"></a><span data-ttu-id="91919-115">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="91919-115">Sign in tooAzure</span></span> 

<span data-ttu-id="91919-116">Jelentkezzen be Azure-előfizetés tooyour hello [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="91919-116">Sign in tooyour Azure subscription by using hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span>

```azurecli
az login
```

### <a name="create-a-resource-group"></a><span data-ttu-id="91919-117">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="91919-117">Create a resource group</span></span>

<span data-ttu-id="91919-118">Hozzon létre egy erőforráscsoportot hello segítségével [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="91919-118">Create a resource group by using hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="91919-119">Egy Azure erőforráscsoport egy olyan logikai tároló, amelyre erőforrások telepítése és kezelése.</span><span class="sxs-lookup"><span data-stu-id="91919-119">An Azure resource group is a logical container in which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="91919-120">hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `westus` helye:</span><span class="sxs-lookup"><span data-stu-id="91919-120">hello following example creates a resource group named `myResourceGroup` in hello `westus` location:</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-an-availability-set"></a><span data-ttu-id="91919-121">Rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="91919-121">Create an availability set</span></span>

<span data-ttu-id="91919-122">Rendelkezésre állási csoport létrehozása nem kötelező, de ajánlott.</span><span class="sxs-lookup"><span data-stu-id="91919-122">Creating an availability set is optional, but we recommend it.</span></span> <span data-ttu-id="91919-123">További információkért lásd: [Azure rendelkezésre állási készletek irányelvek](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).</span><span class="sxs-lookup"><span data-stu-id="91919-123">For more information, see [Azure availability sets guidelines](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-a-virtual-machine"></a><span data-ttu-id="91919-124">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="91919-124">Create a virtual machine</span></span>

<span data-ttu-id="91919-125">Hozzon létre egy virtuális gép hello [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="91919-125">Create a VM by using hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="91919-126">hello alábbi példa létrehoz két virtuális gépek nevű `myVM1` és `myVM2`.</span><span class="sxs-lookup"><span data-stu-id="91919-126">hello following example creates two VMs named `myVM1` and `myVM2`.</span></span> <span data-ttu-id="91919-127">SSH-kulcsok, azt is hoz létre, ha még nem léteznek a kulcs alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="91919-127">It also creates SSH keys, if they do not already exist in a default key location.</span></span> <span data-ttu-id="91919-128">toouse egy adott kulcsok beállításához használja a hello `--ssh-key-value` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="91919-128">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>

<span data-ttu-id="91919-129">Hozzon létre myVM1 (elsődleges):</span><span class="sxs-lookup"><span data-stu-id="91919-129">Create myVM1 (primary):</span></span>
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM1 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --admin-username azureuser \
     --generate-ssh-keys \
```

<span data-ttu-id="91919-130">Hello virtuális gép létrehozása után az Azure CLI információkat a következő példa hasonló toohello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="91919-130">After you create hello VM, Azure CLI shows information similar toohello following example.</span></span> <span data-ttu-id="91919-131">Vegye figyelembe a hello értékének `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="91919-131">Note hello value of `publicIpAddress`.</span></span> <span data-ttu-id="91919-132">A cím tooaccess hello virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="91919-132">You use this address tooaccess hello VM.</span></span>

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

<span data-ttu-id="91919-133">Hozzon létre myVM2 (készenléti):</span><span class="sxs-lookup"><span data-stu-id="91919-133">Create myVM2 (standby):</span></span>
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM2 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --admin-username azureuser \
     --generate-ssh-keys \
```

<span data-ttu-id="91919-134">Vegye figyelembe a hello értékének `publicIpAddress` myVM2 létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="91919-134">Note hello value of `publicIpAddress` after you create myVM2.</span></span>

### <a name="open-hello-tcp-port-for-connectivity"></a><span data-ttu-id="91919-135">Nyissa meg a TCP-portját hello kapcsolat</span><span class="sxs-lookup"><span data-stu-id="91919-135">Open hello TCP port for connectivity</span></span>

<span data-ttu-id="91919-136">Ez a lépés konfigurál külső végpontok száma, amelyek lehetővé teszik a távelérés toohello Oracle-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="91919-136">This step configures external endpoints, which allow remote access toohello Oracle database.</span></span>

<span data-ttu-id="91919-137">Nyissa meg a hello port myVM1:</span><span class="sxs-lookup"><span data-stu-id="91919-137">Open hello port for myVM1:</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVM1NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

<span data-ttu-id="91919-138">hello eredménye a következő válasz hasonló toohello kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="91919-138">hello result should look similar toohello following response:</span></span>

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

<span data-ttu-id="91919-139">Nyissa meg a hello port myVM2:</span><span class="sxs-lookup"><span data-stu-id="91919-139">Open hello port for myVM2:</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVM2NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-toohello-virtual-machine"></a><span data-ttu-id="91919-140">Csatlakoztassa toohello virtuális gépet</span><span class="sxs-lookup"><span data-stu-id="91919-140">Connect toohello virtual machine</span></span>

<span data-ttu-id="91919-141">Használjon hello következő parancsot a toocreate egy hello virtuális gép SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="91919-141">Use hello following command toocreate an SSH session with hello virtual machine.</span></span> <span data-ttu-id="91919-142">Cserélje le hello IP-cím hello `publicIpAddress` értéket a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="91919-142">Replace hello IP address with hello `publicIpAddress` value for your virtual machine.</span></span>

```bash 
$ ssh azureuser@<publicIpAddress>
```

### <a name="create-hello-database-on-myvm1-primary"></a><span data-ttu-id="91919-143">Adatbázis létrehozásához. hello myVM1 (elsődleges)</span><span class="sxs-lookup"><span data-stu-id="91919-143">Create hello database on myVM1 (primary)</span></span>

<span data-ttu-id="91919-144">hello Oracle szoftver már telepítve van hello Piactéri lemezképhez, így a következő lépésre hello tooinstall hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="91919-144">hello Oracle software is already installed on hello Marketplace image, so hello next step is tooinstall hello database.</span></span> 

<span data-ttu-id="91919-145">Kapcsoló toohello Oracle felügyelő:</span><span class="sxs-lookup"><span data-stu-id="91919-145">Switch toohello Oracle superuser:</span></span>

```bash
$ sudo su - oracle
```

<span data-ttu-id="91919-146">hello adatbázis létrehozása:</span><span class="sxs-lookup"><span data-stu-id="91919-146">Create hello database:</span></span>

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
<span data-ttu-id="91919-147">Kimenetek alábbihoz hasonló toohello válasz a következő:</span><span class="sxs-lookup"><span data-stu-id="91919-147">Outputs should look similar toohello following response:</span></span>

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
Look at hello log file "/u01/app/oracle/cfgtoollogs/dbca/cdb1/cdb1.log" for further details.
```

<span data-ttu-id="91919-148">Hello ORACLE_SID és ORACLE_HOME változók megadása:</span><span class="sxs-lookup"><span data-stu-id="91919-148">Set hello ORACLE_SID and ORACLE_HOME variables:</span></span>

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
```

<span data-ttu-id="91919-149">Másik lehetőségként ORACLE_HOME és ORACLE_SID toohello /home/oracle/.bashrc fájl, adhat hozzá, hogy ezek a beállítások lesznek mentve a későbbi bejelentkezések során:</span><span class="sxs-lookup"><span data-stu-id="91919-149">Optionally, you can add ORACLE_HOME and ORACLE_SID toohello /home/oracle/.bashrc file, so that these settings are saved for future logins:</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
```

## <a name="configure-data-guard"></a><span data-ttu-id="91919-150">Data Guard konfigurálása</span><span class="sxs-lookup"><span data-stu-id="91919-150">Configure Data Guard</span></span>

### <a name="enable-archive-log-mode-on-myvm1-primary"></a><span data-ttu-id="91919-151">Archív napló mód a myVM1 (elsődleges) engedélyezése</span><span class="sxs-lookup"><span data-stu-id="91919-151">Enable archive log mode on myVM1 (primary)</span></span>

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
<span data-ttu-id="91919-152">Kényszerített naplózását, és ellenőrizze, hogy legalább egy naplófájlban:</span><span class="sxs-lookup"><span data-stu-id="91919-152">Enable force logging, and make sure at least one log file is present:</span></span>

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
```

<span data-ttu-id="91919-153">Készenléti ismét: naplók létrehozása:</span><span class="sxs-lookup"><span data-stu-id="91919-153">Create standby redo logs:</span></span>

```bash
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo01.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo02.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo03.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo04.log') SIZE 50M;
```

<span data-ttu-id="91919-154">Kapcsolja be a Flashback (így helyreállítási sokkal könnyebben), és készenléti\_fájl\_felügyeleti tooauto.</span><span class="sxs-lookup"><span data-stu-id="91919-154">Turn on Flashback (which makes recovery a lot easier) and set STANDBY\_FILE\_MANAGEMENT tooauto.</span></span> <span data-ttu-id="91919-155">Lépjen ki az SQL * Plus ezt követően.</span><span class="sxs-lookup"><span data-stu-id="91919-155">Exit SQL*Plus after that.</span></span>

```bash
SQL> ALTER DATABASE FLASHBACK ON;
SQL> ALTER SYSTEM SET STANDBY_FILE_MANAGEMENT=AUTO;
SQL> EXIT;
```

### <a name="set-up-service-on-myvm1-primary"></a><span data-ttu-id="91919-156">Állítsa be a szolgáltatást a myVM1 (elsődleges)</span><span class="sxs-lookup"><span data-stu-id="91919-156">Set up service on myVM1 (primary)</span></span>

<span data-ttu-id="91919-157">Szerkessze, vagy hozzon létre hello tnsnames.ora fájlt, amely hello $ORACLE_HOME\network\admin mappában.</span><span class="sxs-lookup"><span data-stu-id="91919-157">Edit or create hello tnsnames.ora file, which is in hello $ORACLE_HOME\network\admin folder.</span></span>

<span data-ttu-id="91919-158">Adja hozzá a következő tételek hello:</span><span class="sxs-lookup"><span data-stu-id="91919-158">Add hello following entries:</span></span>

```bash
cdb1 =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM1)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )

cdb1_stby =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM2)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )
```

<span data-ttu-id="91919-159">Szerkessze, vagy hozzon létre hello listener.ora fájlt, amely hello $ORACLE_HOME\network\admin mappában.</span><span class="sxs-lookup"><span data-stu-id="91919-159">Edit or create hello listener.ora file, which is in hello $ORACLE_HOME\network\admin folder.</span></span>

<span data-ttu-id="91919-160">Adja hozzá a következő tételek hello:</span><span class="sxs-lookup"><span data-stu-id="91919-160">Add hello following entries:</span></span>

```bash
LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM1)(PORT = 1521))
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
    )
  )

SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (GLOBAL_DBNAME = cdb1_DGMGRL)
      (ORACLE_HOME = /u01/app/oracle/product/12.1.0/dbhome_1)
      (SID_NAME = cdb1)
    )
  )

ADR_BASE_LISTENER = /u01/app/oracle
```

<span data-ttu-id="91919-161">Data Guard Broker engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="91919-161">Enable Data Guard Broker:</span></span>
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```
<span data-ttu-id="91919-162">Indítsa el a hello figyelő:</span><span class="sxs-lookup"><span data-stu-id="91919-162">Start hello listener:</span></span>

```bash
$ lsnrctl stop
$ lsnrctl start
```

### <a name="set-up-service-on-myvm2-standby"></a><span data-ttu-id="91919-163">A myVM2 szolgáltatás beállítása (készenléti)</span><span class="sxs-lookup"><span data-stu-id="91919-163">Set up service on myVM2 (standby)</span></span>

<span data-ttu-id="91919-164">SSH toomyVM2:</span><span class="sxs-lookup"><span data-stu-id="91919-164">SSH toomyVM2:</span></span>

```bash 
$ ssh azureuser@<publicIpAddress>
```

<span data-ttu-id="91919-165">Jelentkezzen be Oracle:</span><span class="sxs-lookup"><span data-stu-id="91919-165">Log in as Oracle:</span></span>

```bash
$ sudo su - oracle
```

<span data-ttu-id="91919-166">Szerkessze, vagy hozzon létre hello tnsnames.ora fájlt, amely hello $ORACLE_HOME\network\admin mappában.</span><span class="sxs-lookup"><span data-stu-id="91919-166">Edit or create hello tnsnames.ora file, which is in hello $ORACLE_HOME\network\admin folder.</span></span>

<span data-ttu-id="91919-167">Adja hozzá a következő tételek hello:</span><span class="sxs-lookup"><span data-stu-id="91919-167">Add hello following entries:</span></span>

```bash
cdb1 =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM1)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )

cdb1_stby =
  (DESCRIPTION =
    (ADDRESS_LIST =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM2)(PORT = 1521))
    )
    (CONNECT_DATA =
      (SID = cdb1)
    )
  )
```

<span data-ttu-id="91919-168">Szerkessze, vagy hozzon létre hello listener.ora fájlt, amely hello $ORACLE_HOME\network\admin mappában.</span><span class="sxs-lookup"><span data-stu-id="91919-168">Edit or create hello listener.ora file, which is in hello $ORACLE_HOME\network\admin folder.</span></span>

<span data-ttu-id="91919-169">Adja hozzá a következő tételek hello:</span><span class="sxs-lookup"><span data-stu-id="91919-169">Add hello following entries:</span></span>

```bash
LISTENER =
  (DESCRIPTION_LIST =
    (DESCRIPTION =
      (ADDRESS = (PROTOCOL = TCP)(HOST = myVM2)(PORT = 1521))
      (ADDRESS = (PROTOCOL = IPC)(KEY = EXTPROC1521))
    )
  )

SID_LIST_LISTENER =
  (SID_LIST =
    (SID_DESC =
      (GLOBAL_DBNAME = cdb1_DGMGRL)
      (ORACLE_HOME = /u01/app/oracle/product/12.1.0/dbhome_1)
      (SID_NAME = cdb1)
    )
  )

ADR_BASE_LISTENER = /u01/app/oracle
```

<span data-ttu-id="91919-170">Indítsa el a hello figyelő:</span><span class="sxs-lookup"><span data-stu-id="91919-170">Start hello listener:</span></span>

```bash
$ lsnrctl stop
$ lsnrctl start
```


### <a name="restore-hello-database-toomyvm2-standby"></a><span data-ttu-id="91919-171">Állítsa vissza a hello adatbázis toomyVM2 (készenléti)</span><span class="sxs-lookup"><span data-stu-id="91919-171">Restore hello database toomyVM2 (standby)</span></span>

<span data-ttu-id="91919-172">Hozzon létre hello paraméter fájl /tmp/initcdb1_stby.ora hello tartalma a következő:</span><span class="sxs-lookup"><span data-stu-id="91919-172">Create hello parameter file /tmp/initcdb1_stby.ora with hello following contents:</span></span>
```bash
*.db_name='cdb1'
```

<span data-ttu-id="91919-173">Mappák létrehozása:</span><span class="sxs-lookup"><span data-stu-id="91919-173">Create folders:</span></span>

```bash
mkdir -p /u01/app/oracle/oradata/cdb1/pdbseed
mkdir -p /u01/app/oracle/oradata/cdb1/pdb1
mkdir -p /u01/app/oracle/fast_recovery_area/cdb1
mkdir -p /u01/app/oracle/admin/cdb1/adump
```

<span data-ttu-id="91919-174">Hozzon létre egy jelszót fájlt:</span><span class="sxs-lookup"><span data-stu-id="91919-174">Create a password file:</span></span>

```bash
$ orapwd file=/u01/app/oracle/product/12.1.0/dbhome_1/dbs/orapwcdb1 password=OraPasswd1 entries=10
```
<span data-ttu-id="91919-175">Indítsa el a hello adatbázis a myVM2:</span><span class="sxs-lookup"><span data-stu-id="91919-175">Start hello database on myVM2:</span></span>

```bash
$ export ORACLE_SID=cdb1
$ sqlplus / as sysdba

SQL> STARTUP NOMOUNT PFILE='/tmp/initcdb1_stby.ora';
SQL> EXIT;
```

<span data-ttu-id="91919-176">Hello adatbázis visszaállítása hello RMAN eszköz használatával:</span><span class="sxs-lookup"><span data-stu-id="91919-176">Restore hello database by using hello RMAN tool:</span></span>

```bash
$ rman TARGET sys/OraPasswd1@cdb1 AUXILIARY sys/OraPasswd1@cdb1_stby
```

<span data-ttu-id="91919-177">Futtassa a következő parancsok RMAN hello:</span><span class="sxs-lookup"><span data-stu-id="91919-177">Run hello following commands in RMAN:</span></span>
```bash
DUPLICATE TARGET DATABASE
  FOR STANDBY
  FROM ACTIVE DATABASE
  DORECOVER
  SPFILE
    SET db_unique_name='CDB1_STBY' COMMENT 'Is standby'
  NOFILENAMECHECK;
```

<span data-ttu-id="91919-178">Megtekintheti az üzenetek hasonló toohello következő hello parancs végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="91919-178">You should see messages similar toohello following when hello command is completed.</span></span> <span data-ttu-id="91919-179">Kilépés RMAN.</span><span class="sxs-lookup"><span data-stu-id="91919-179">Exit RMAN.</span></span>
```bash
media recovery complete, elapsed time: 00:00:00
Finished recover at 29-JUN-17
Finished Duplicate Db at 29-JUN-17

RMAN> EXIT;
```

<span data-ttu-id="91919-180">Másik lehetőségként ORACLE_HOME és ORACLE_SID toohello /home/oracle/.bashrc fájl, adhat hozzá, hogy ezek a beállítások lesznek mentve a későbbi bejelentkezések során:</span><span class="sxs-lookup"><span data-stu-id="91919-180">Optionally, you can add ORACLE_HOME and ORACLE_SID toohello /home/oracle/.bashrc file, so that these settings are saved for future logins:</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
```

<span data-ttu-id="91919-181">Data Guard Broker engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="91919-181">Enable Data Guard Broker:</span></span>
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```

### <a name="configure-data-guard-broker-on-myvm1-primary"></a><span data-ttu-id="91919-182">Data Guard Broker myVM1 (elsődleges) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="91919-182">Configure Data Guard Broker on myVM1 (primary)</span></span>

<span data-ttu-id="91919-183">Indítsa el a Data Guard kezelőjét, és jelentkezzen be SYS és jelszó használatával.</span><span class="sxs-lookup"><span data-stu-id="91919-183">Start Data Guard Manager and log in by using SYS and a password.</span></span> <span data-ttu-id="91919-184">(Ne használja az operációs rendszer hitelesítési.) Hello következőket hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="91919-184">(Do not use OS authentication.) Perform hello following:</span></span>

```bash
$ dgmgrl sys/OraPasswd1@cdb1
DGMGRL for Linux: Version 12.1.0.2.0 - 64bit Production

Copyright (c) 2000, 2013, Oracle. All rights reserved.

Welcome tooDGMGRL, type "help" for information.
Connected as SYSDBA.
DGMGRL> CREATE CONFIGURATION my_dg_config AS PRIMARY DATABASE IS cdb1 CONNECT IDENTIFIER IS cdb1;
Configuration "my_dg_config" created with primary database "cdb1"
DGMGRL> ADD DATABASE cdb1_stby AS CONNECT IDENTIFIER IS cdb1_stby MAINTAINED AS PHYSICAL;
Database "cdb1_stby" added
DGMGRL> ENABLE CONFIGURATION;
Enabled.
```

<span data-ttu-id="91919-185">Hello konfiguráció áttekintése:</span><span class="sxs-lookup"><span data-stu-id="91919-185">Review hello configuration:</span></span>
```bash
DGMGRL> SHOW CONFIGURATION;

Configuration - my_dg_config

  Protection Mode: MaxPerformance
  Members:
  cdb1      - Primary database
    cdb1_stby - Physical standby database

Fast-Start Failover: DISABLED

Configuration Status:
SUCCESS   (status updated 26 seconds ago)
```

<span data-ttu-id="91919-186">Hello Oracle Data Guard beállítása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="91919-186">You've completed hello Oracle Data Guard setup.</span></span> <span data-ttu-id="91919-187">hello következő szakasz bemutatja, hogyan tootest kapcsolat hello és vált át.</span><span class="sxs-lookup"><span data-stu-id="91919-187">hello next section shows you how tootest hello connectivity and switch over.</span></span>

### <a name="connect-hello-database-from-hello-client-machine"></a><span data-ttu-id="91919-188">Hello adatbázis csatlakoztatja a hello ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="91919-188">Connect hello database from hello client machine</span></span>

<span data-ttu-id="91919-189">Frissítés, vagy az ügyfélszámítógép hello tnsnames.ora fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="91919-189">Update or create hello tnsnames.ora file on your client machine.</span></span> <span data-ttu-id="91919-190">A fájl általában $ORACLE_HOME\network\admin van.</span><span class="sxs-lookup"><span data-stu-id="91919-190">This file is usually in $ORACLE_HOME\network\admin.</span></span>

<span data-ttu-id="91919-191">Cserélje le a hello IP-címet a `publicIpAddress` myVM1 és myVM2 értékeit:</span><span class="sxs-lookup"><span data-stu-id="91919-191">Replace hello IP addresses with your `publicIpAddress` values for myVM1 and myVM2:</span></span>

```bash
cdb1=
  (DESCRIPTION=
    (ADDRESS=
      (PROTOCOL=TCP)
      (HOST=<myVM1 IP address>)
      (PORT=1521)
    )
    (CONNECT_DATA=
      (SERVER=dedicated)
      (SERVICE_NAME=cdb1)
    )
  )

cdb1_stby=
  (DESCRIPTION=
    (ADDRESS=
      (PROTOCOL=TCP)
      (HOST=<myVM2 IP address>)
      (PORT=1521)
    )
    (CONNECT_DATA=
      (SERVER=dedicated)
      (SERVICE_NAME=cdb1_stby)
    )
  )
```

<span data-ttu-id="91919-192">Indítsa el az SQL * Plus:</span><span class="sxs-lookup"><span data-stu-id="91919-192">Start SQL*Plus:</span></span>

```bash
$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```
## <a name="test-hello-data-guard-configuration"></a><span data-ttu-id="91919-193">Hello Data Guard tesztkonfiguráció</span><span class="sxs-lookup"><span data-stu-id="91919-193">Test hello Data Guard configuration</span></span>

### <a name="switch-over-hello-database-on-myvm1-primary"></a><span data-ttu-id="91919-194">Vált át hello adatbázis myVM1 (elsődleges)</span><span class="sxs-lookup"><span data-stu-id="91919-194">Switch over hello database on myVM1 (primary)</span></span>

<span data-ttu-id="91919-195">az elsődleges toostandby (cdb1 toocdb1_stby) tooswitch:</span><span class="sxs-lookup"><span data-stu-id="91919-195">tooswitch from primary toostandby (cdb1 toocdb1_stby):</span></span>

```bash
$ dgmgrl sys/OraPasswd1@cdb1
DGMGRL for Linux: Version 12.1.0.2.0 - 64bit Production

Copyright (c) 2000, 2013, Oracle. All rights reserved.

Welcome tooDGMGRL, type "help" for information.
Connected as SYSDBA.
DGMGRL> SWITCHOVER toocdb1_stby;
Performing switchover NOW, please wait...
Operation requires a connection tooinstance "cdb1" on database "cdb1_stby"
Connecting tooinstance "cdb1"...
Connected as SYSDBA.
New primary database "cdb1_stby" is opening...
Operation requires start up of instance "cdb1" on database "cdb1"
Starting instance "cdb1"...
ORACLE instance started.
Database mounted.
Switchover succeeded, new primary is "cdb1_stby"
DGMGRL>
```

<span data-ttu-id="91919-196">Csatlakoztathatja toohello készenléti adatbázis.</span><span class="sxs-lookup"><span data-stu-id="91919-196">You can now connect toohello standby database.</span></span>

<span data-ttu-id="91919-197">Indítsa el az SQL * Plus:</span><span class="sxs-lookup"><span data-stu-id="91919-197">Start SQL*Plus:</span></span>

```bash

$ sqlplus sys/OraPasswd1@cdb1_stby
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

### <a name="switch-over-hello-database-on-myvm2-standby"></a><span data-ttu-id="91919-198">Vált át hello database myVM2 (készenléti)</span><span class="sxs-lookup"><span data-stu-id="91919-198">Switch over hello database on myVM2 (standby)</span></span>

<span data-ttu-id="91919-199">tooswitch keresztül, hello következő myVM2 futtassa:</span><span class="sxs-lookup"><span data-stu-id="91919-199">tooswitch over, run hello following on myVM2:</span></span>
```bash
$ dgmgrl sys/OraPasswd1@cdb1_stby
DGMGRL for Linux: Version 12.1.0.2.0 - 64bit Production

Copyright (c) 2000, 2013, Oracle. All rights reserved.

Welcome tooDGMGRL, type "help" for information.
Connected as SYSDBA.
DGMGRL> SWITCHOVER toocdb1;
Performing switchover NOW, please wait...
Operation requires a connection tooinstance "cdb1" on database "cdb1"
Connecting tooinstance "cdb1"...
Connected as SYSDBA.
New primary database "cdb1" is opening...
Operation requires start up of instance "cdb1" on database "cdb1_stby"
Starting instance "cdb1"...
ORACLE instance started.
Database mounted.
Switchover succeeded, new primary is "cdb1"
```

<span data-ttu-id="91919-200">Ebben az esetben kell tudni tooconnect toohello elsődleges adatbázis.</span><span class="sxs-lookup"><span data-stu-id="91919-200">Once again, you should now be able tooconnect toohello primary database.</span></span>

<span data-ttu-id="91919-201">Indítsa el az SQL * Plus:</span><span class="sxs-lookup"><span data-stu-id="91919-201">Start SQL*Plus:</span></span>

```bash

$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

<span data-ttu-id="91919-202">Hello telepítésének és konfigurálásának Data Guard Oracle Linux befejezése után.</span><span class="sxs-lookup"><span data-stu-id="91919-202">You've finished hello installation and configuration of Data Guard on Oracle Linux.</span></span>


## <a name="delete-hello-virtual-machine"></a><span data-ttu-id="91919-203">Hello virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="91919-203">Delete hello virtual machine</span></span>

<span data-ttu-id="91919-204">Ha a virtuális gép már nem kell hello, használhatja a következő parancs tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrások hello:</span><span class="sxs-lookup"><span data-stu-id="91919-204">When you no longer need hello VM, you can use hello following command tooremove hello resource group, VM, and all related resources:</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="91919-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="91919-205">Next steps</span></span>

[<span data-ttu-id="91919-206">Oktatóanyag: Magas rendelkezésre állású virtuális gépek létrehozása</span><span class="sxs-lookup"><span data-stu-id="91919-206">Tutorial: Create highly available virtual machines</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="91919-207">Virtuális gép telepítése az Azure parancssori felület minták felfedezése</span><span class="sxs-lookup"><span data-stu-id="91919-207">Explore VM deployment Azure CLI samples</span></span>](../../linux/cli-samples.md)
