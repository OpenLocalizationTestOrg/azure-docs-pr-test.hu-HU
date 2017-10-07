---
title: "Oracle Data Guard Azure Linux virtuális gépen aaaImplement |} Microsoft Docs"
description: "Gyorsan karban lehessen az Oracle Data Guard fel, és az Azure környezetben futó."
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
ms.openlocfilehash: 101196b2f50dfca64d3eb1b4be56ff0c108693e9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="implement-oracle-data-guard-on-azure-linux-vm"></a><span data-ttu-id="2e11a-103">Oracle Data Guard megvalósítása az Azure Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="2e11a-103">Implement Oracle Data Guard on Azure Linux VM</span></span> 

<span data-ttu-id="2e11a-104">hello Azure CLI használt toocreate és hello parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="2e11a-104">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="2e11a-105">Ez az útmutató adatokat hello Azure CLI toodeploy az Oracle hello piactér gyűjtemény lemezképből 12c adatbázis használatával.</span><span class="sxs-lookup"><span data-stu-id="2e11a-105">This guide details using hello Azure CLI toodeploy an Oracle 12c Database from hello Marketplace gallery image.</span></span> <span data-ttu-id="2e11a-106">Hello Oracle-adatbázis létrehozása után ez a dokumentum bemutatja, hogyan részletes tooinstall és Data Guard konfigurálása az Azure virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="2e11a-106">Once hello Oracle database is created, this document shows you step-by-step how tooinstall and configure Data Guard on Azure VM.</span></span>

<span data-ttu-id="2e11a-107">Mielőtt elkezdené, győződjön meg arról, hogy telepítve van az Azure CLI hello listájában.</span><span class="sxs-lookup"><span data-stu-id="2e11a-107">Before you start, make sure that hello Azure CLI has been installed.</span></span> <span data-ttu-id="2e11a-108">További információért lásd az [Azure CLI telepítési útmutatóját](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="2e11a-108">For more information, see [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-hello-environment"></a><span data-ttu-id="2e11a-109">Hello környezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="2e11a-109">Prepare hello environment</span></span>
### <a name="assumptions"></a><span data-ttu-id="2e11a-110">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2e11a-110">Assumptions</span></span>

<span data-ttu-id="2e11a-111">toocreate két Azure-alapú virtuális gépek hello szüksége tooperform hello Oracle Data Guard telepítse, azonos rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="2e11a-111">tooperform hello Oracle Data Guard install, you need toocreate two Azure VMs on hello same availability set.</span></span> <span data-ttu-id="2e11a-112">hello Piactéri lemezképhez toocreate hello virtuális gépek használja az "Oracle: Oracle-adatbázis-Ee:12.1.0.2:latest".</span><span class="sxs-lookup"><span data-stu-id="2e11a-112">hello Marketplace image you use toocreate hello VMs is "Oracle:Oracle-Database-Ee:12.1.0.2:latest".</span></span>

<span data-ttu-id="2e11a-113">hello elsődleges virtuális gép (myVM1) rendelkezik egy futó Oracle-példány.</span><span class="sxs-lookup"><span data-stu-id="2e11a-113">hello primary VM (myVM1) has a running Oracle instance.</span></span>

<span data-ttu-id="2e11a-114">készenléti állapotban lévő virtuális gép (myVM2) rendelkezik hello Oracle szoftvereket csak hello.</span><span class="sxs-lookup"><span data-stu-id="2e11a-114">hello standby VM (myVM2) has hello Oracle software installed only.</span></span>

### <a name="log-in-tooazure"></a><span data-ttu-id="2e11a-115">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="2e11a-115">Log in tooAzure</span></span> 

<span data-ttu-id="2e11a-116">Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot, és kövesse hello képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="2e11a-116">Log in tooyour Azure subscription with hello [az login](/cli/azure/#login) command and follow hello on-screen directions.</span></span>

```azurecli
az login
```

### <a name="create-a-resource-group"></a><span data-ttu-id="2e11a-117">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="2e11a-117">Create a resource group</span></span>

<span data-ttu-id="2e11a-118">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="2e11a-118">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="2e11a-119">Az Azure-erőforráscsoport olyan logikai tároló, amelybe a rendszer üzembe helyezi és kezeli az Azure-erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="2e11a-119">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> 

<span data-ttu-id="2e11a-120">hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `westus` helyét.</span><span class="sxs-lookup"><span data-stu-id="2e11a-120">hello following example creates a resource group named `myResourceGroup` in hello `westus` location.</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-availability-set"></a><span data-ttu-id="2e11a-121">Rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="2e11a-121">Create availability set</span></span>

<span data-ttu-id="2e11a-122">Ez a lépés nem kötelező, de ajánlott.</span><span class="sxs-lookup"><span data-stu-id="2e11a-122">This step is optional, but is recommended.</span></span> <span data-ttu-id="2e11a-123">Lásd: [Azure rendelkezésre állási készletek útmutató](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) további információt.</span><span class="sxs-lookup"><span data-stu-id="2e11a-123">see [Azure availability sets guide](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) for more information.</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-virtual-machine"></a><span data-ttu-id="2e11a-124">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="2e11a-124">Create virtual machine</span></span>

<span data-ttu-id="2e11a-125">Hozzon létre egy virtuális gép hello [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="2e11a-125">Create a VM with hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="2e11a-126">hello alábbi példakód létrehozza nevű 2 virtuális gép `myVM1` és `myVM2`.</span><span class="sxs-lookup"><span data-stu-id="2e11a-126">hello following example creates 2 VMs named `myVM1` and `myVM2`.</span></span> <span data-ttu-id="2e11a-127">SSH-kulcsok létrehozása, ha még nem léteznek a kulcs alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="2e11a-127">Creates SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="2e11a-128">toouse egy adott kulcsok beállításához használja a hello `--ssh-key-value` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2e11a-128">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>

<span data-ttu-id="2e11a-129">Hozzon létre myVM1 (elsődleges)</span><span class="sxs-lookup"><span data-stu-id="2e11a-129">Create myVM1 (primary)</span></span>
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

<span data-ttu-id="2e11a-130">Egyszer hello a virtuális gép létrehozása, hello Azure CLI látható információt a következő példa hasonló toohello: hello hajtsa végre a megfelelő tudomásul `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="2e11a-130">Once hello VM has been created, hello Azure CLI shows information similar toohello following example: Take note of hello `publicIpAddress`.</span></span> <span data-ttu-id="2e11a-131">Ez a cím használt tooaccess hello virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="2e11a-131">This address is used tooaccess hello VM.</span></span>

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

<span data-ttu-id="2e11a-132">Hozzon létre myVM2 (készenléti)</span><span class="sxs-lookup"><span data-stu-id="2e11a-132">Create myVM2 (standby)</span></span>
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

<span data-ttu-id="2e11a-133">Jegyezze fel a hello `publicIpAddress` azt a létrehozása után is.</span><span class="sxs-lookup"><span data-stu-id="2e11a-133">Take note of hello `publicIpAddress` as well once it created.</span></span>

### <a name="open-hello-tcp-port-for-connectivity"></a><span data-ttu-id="2e11a-134">Nyissa meg a TCP-portját hello kapcsolat</span><span class="sxs-lookup"><span data-stu-id="2e11a-134">Open hello TCP port for connectivity</span></span>

<span data-ttu-id="2e11a-135">hello lépés tooconfigure külső végpontok száma, amely lehetővé teszi, hogy hello Oracle DB távelérése, hello a következő parancs végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="2e11a-135">hello step is tooconfigure external endpoints, which allows accessing hello Oracle DB remotely, you execute hello following command.</span></span>

<span data-ttu-id="2e11a-136">MyVM1 nyitott port</span><span class="sxs-lookup"><span data-stu-id="2e11a-136">Open port for myVM1</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVmN1SG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

<span data-ttu-id="2e11a-137">Eredménye a következő válasz hasonló toohello kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="2e11a-137">Result should look similar toohello following response:</span></span>

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

<span data-ttu-id="2e11a-138">MyVM2 nyitott port</span><span class="sxs-lookup"><span data-stu-id="2e11a-138">Open port for myVM2</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm2N1SG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-toovirtual-machine"></a><span data-ttu-id="2e11a-139">Csatlakoztassa a gépet toovirtual</span><span class="sxs-lookup"><span data-stu-id="2e11a-139">Connect toovirtual machine</span></span>

<span data-ttu-id="2e11a-140">Használjon hello következő parancsot a toocreate egy hello virtuális gép SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="2e11a-140">Use hello following command toocreate an SSH session with hello virtual machine.</span></span> <span data-ttu-id="2e11a-141">Cserélje le hello IP-cím hello `publicIpAddress` a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="2e11a-141">Replace hello IP address with hello `publicIpAddress` of your virtual machine.</span></span>

```bash 
ssh azureuser@<publicIpAddress>
```

### <a name="create-database-on-myvm1-primary"></a><span data-ttu-id="2e11a-142">Adatbázis létrehozása a következőn myVM1 (elsődleges)</span><span class="sxs-lookup"><span data-stu-id="2e11a-142">Create Database on myVM1 (Primary)</span></span>

<span data-ttu-id="2e11a-143">hello Oracle szoftver már telepítve van hello Piactéri lemezképhez, így a következő lépésre hello tooinstall hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="2e11a-143">hello Oracle software is already installed on hello Marketplace image, so hello next step is tooinstall hello database.</span></span> <span data-ttu-id="2e11a-144">első lépés hello hello "oracle" felügyelő futtató.</span><span class="sxs-lookup"><span data-stu-id="2e11a-144">hello first step is running as hello 'oracle' superuser.</span></span>

```bash
sudo su - oracle
```

<span data-ttu-id="2e11a-145">hello adatbázis létrehozása:</span><span class="sxs-lookup"><span data-stu-id="2e11a-145">create hello database:</span></span>

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
<span data-ttu-id="2e11a-146">Kimenetek alábbihoz hasonló toohello válasz a következő:</span><span class="sxs-lookup"><span data-stu-id="2e11a-146">Outputs should look similar toohello following response:</span></span>

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

<span data-ttu-id="2e11a-147">Hello ORACLE_SID és ORACLE_HOME változók megadása</span><span class="sxs-lookup"><span data-stu-id="2e11a-147">Set hello ORACLE_SID and ORACLE_HOME variables</span></span>

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
```

<span data-ttu-id="2e11a-148">Szükség esetén is felvett ORACLE_HOME és ORACLE_SID toohello .bashrc fájlt, így ezek a beállítások lesznek mentve a későbbi bejelentkezések során.</span><span class="sxs-lookup"><span data-stu-id="2e11a-148">Optionally, You can added ORACLE_HOME and ORACLE_SID toohello .bashrc file, so that these settings are saved for future logins.</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
```

## <a name="data-guard-configurations"></a><span data-ttu-id="2e11a-149">Data Guard konfigurációk</span><span class="sxs-lookup"><span data-stu-id="2e11a-149">Data Guard Configurations</span></span>

### <a name="enable-archive-log-mode-on-myvm1-primary"></a><span data-ttu-id="2e11a-150">Archív napló mód a myVM1 (elsődleges) engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2e11a-150">Enable Archive log mode on myVM1 (primary)</span></span>

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
<span data-ttu-id="2e11a-151">Kényszerített naplózását, és ellenőrizze, hogy legalább egy naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="2e11a-151">Enable force logging, and make sure at least one logfile is present.</span></span>

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
```

<span data-ttu-id="2e11a-152">Készenléti ismét: naplók létrehozása</span><span class="sxs-lookup"><span data-stu-id="2e11a-152">Create standby redo logs</span></span>

```bash
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo01.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo02.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo03.log') SIZE 50M;
SQL> ALTER DATABASE ADD STANDBY LOGFILE ('/u01/app/oracle/oradata/cdb1/standby_redo04.log') SIZE 50M;
```

<span data-ttu-id="2e11a-153">Kapcsolja be a Flashback (ami hello helyreállítási sokkal korábban) és STANDBY_FILE_MANAGEMENT tooauto beállítása</span><span class="sxs-lookup"><span data-stu-id="2e11a-153">Turn on Flashback (which made hello recovery a lot earlier) and set STANDBY_FILE_MANAGEMENT tooauto</span></span>

```bash
SQL> ALTER DATABASE FLASHBACK ON;
SQL> ALTER SYSTEM SET STANDBY_FILE_MANAGEMENT=AUTO;
```

### <a name="service-setup-on-myvm1-primary"></a><span data-ttu-id="2e11a-154">A myVM1 (elsődleges) telepítése</span><span class="sxs-lookup"><span data-stu-id="2e11a-154">Service setup on myVM1 (primary)</span></span>

<span data-ttu-id="2e11a-155">Szerkeszteni vagy létrehozni hello tnsnames.ora fájl, amely $ORACLE_HOME\network\admin mappához</span><span class="sxs-lookup"><span data-stu-id="2e11a-155">Edit or create hello tnsnames.ora file, which is located at $ORACLE_HOME\network\admin folder</span></span>

<span data-ttu-id="2e11a-156">A következő tételek hello hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2e11a-156">Add hello following entries</span></span>

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

<span data-ttu-id="2e11a-157">Szerkeszteni vagy létrehozni hello listener.ora fájl, amely $ORACLE_HOME\network\admin mappához</span><span class="sxs-lookup"><span data-stu-id="2e11a-157">Edit or create hello listener.ora file, which is located at $ORACLE_HOME\network\admin folder</span></span>

<span data-ttu-id="2e11a-158">A következő tételek hello hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2e11a-158">Add hello following entries</span></span>

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

<span data-ttu-id="2e11a-159">Hello figyelő elindítása</span><span class="sxs-lookup"><span data-stu-id="2e11a-159">Start hello listener</span></span>

```bash
$ lsnrctl stop
$ lsnrctl start
```

### <a name="service-setup-on-myvm2-standby"></a><span data-ttu-id="2e11a-160">A myVM2 telepítése (készenléti)</span><span class="sxs-lookup"><span data-stu-id="2e11a-160">Service setup on myVM2 (Standby)</span></span>

<span data-ttu-id="2e11a-161">Szerkeszteni vagy létrehozni hello tnsnames.ora fájl, amely $ORACLE_HOME\network\admin mappához</span><span class="sxs-lookup"><span data-stu-id="2e11a-161">Edit or create hello tnsnames.ora file, which is located at $ORACLE_HOME\network\admin folder</span></span>

<span data-ttu-id="2e11a-162">A következő tételek hello hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2e11a-162">Add hello following entries</span></span>

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

<span data-ttu-id="2e11a-163">Szerkeszteni vagy létrehozni hello listener.ora fájl, amely $ORACLE_HOME\network\admin mappához</span><span class="sxs-lookup"><span data-stu-id="2e11a-163">Edit or create hello listener.ora file, which is located at $ORACLE_HOME\network\admin folder</span></span>

<span data-ttu-id="2e11a-164">A következő tételek hello hozzáadása</span><span class="sxs-lookup"><span data-stu-id="2e11a-164">Add hello following entries</span></span>

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

<span data-ttu-id="2e11a-165">Hello figyelő elindítása</span><span class="sxs-lookup"><span data-stu-id="2e11a-165">Start hello listener</span></span>

```bash
$ lsnrctl stop
$ lsnrctl start
```

<span data-ttu-id="2e11a-166">Data Guard Broker engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2e11a-166">Enable Data Guard Broker</span></span>
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```

### <a name="restore-database-toomyvm2-standby"></a><span data-ttu-id="2e11a-167">Állítsa vissza az adatbázis toomyVM2 (készenléti)</span><span class="sxs-lookup"><span data-stu-id="2e11a-167">Restore database toomyVM2 (Standby)</span></span>

<span data-ttu-id="2e11a-168">Hozzon létre egy paraméterfájl "/ tmp/initcdb1_stby.ora" hello következőket tartalma</span><span class="sxs-lookup"><span data-stu-id="2e11a-168">Create a parameter file '/tmp/initcdb1_stby.ora' with hello followings contents</span></span>
```bash
*.db_name='cdb1'
```

<span data-ttu-id="2e11a-169">Mappák létrehozása</span><span class="sxs-lookup"><span data-stu-id="2e11a-169">Create folders</span></span>

```bash
mkdir -p /u01/app/oracle/oradata/cdb1/pdbseed
mkdir -p /u01/app/oracle/oradata/cdb1/pdb1
mkdir -p /u01/app/oracle/fast_recovery_area/cdb1
mkdir -p /u01/app/oracle/admin/cdb1/adump
```

<span data-ttu-id="2e11a-170">Jelszó-fájl létrehozása</span><span class="sxs-lookup"><span data-stu-id="2e11a-170">Create password file</span></span>

```bash
$ orapwd file=/u01/app/oracle/product/12.1.0.2/db_1/dbs/orapwcdb1 password=OraPasswd1 entries=10
```
<span data-ttu-id="2e11a-171">Indítsa el a myVM2 adatbázis</span><span class="sxs-lookup"><span data-stu-id="2e11a-171">Start up database on myVM2</span></span>

```bash
$ export ORACLE_SID=cdb1
$ sqlplus / as sysdba

SQL> STARTUP NOMOUNT PFILE='/tmp/initcdb1_stby.ora';
SQL> EXIT;
```

<span data-ttu-id="2e11a-172">RMAN segédprogrammal adatbázis visszaállítása</span><span class="sxs-lookup"><span data-stu-id="2e11a-172">Restore database using RMAN utility</span></span>

```bash
$ rman TARGET sys/OraPasswd1@cdb1 AUXILIARY sys/OraPasswd1@cdb1_stby
```

<span data-ttu-id="2e11a-173">A következő parancsok RMAN hello végrehajtása</span><span class="sxs-lookup"><span data-stu-id="2e11a-173">Execute hello following commands in RMAN</span></span>
```bash
DUPLICATE TARGET DATABASE
  FOR STANDBY
  FROM ACTIVE DATABASE
  DORECOVER
  SPFILE
    SET db_unique_name='CDB1_STBY' COMMENT 'Is standby'
  NOFILENAMECHECK;
```

<span data-ttu-id="2e11a-174">Data Guard Broker engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2e11a-174">Enable Data Guard Broker</span></span>
```bash
$ sqlplus / as sysdba
SQL> ALTER SYSTEM SET dg_broker_start=true;
SQL> EXIT;
```

### <a name="configure-data-guard-broker-on-myvm1-primary"></a><span data-ttu-id="2e11a-175">Data Guard broker myVM1 (elsődleges) konfigurálása</span><span class="sxs-lookup"><span data-stu-id="2e11a-175">Configure Data Guard broker on myVM1 (primary)</span></span>

<span data-ttu-id="2e11a-176">Indítsa el a hello Data Guard manager és a bejelentkezési SYS és jelszóval (ne nem használ az operációs rendszer hitelesítést).</span><span class="sxs-lookup"><span data-stu-id="2e11a-176">Start hello Data Guard manager and login using SYS and password (do not using OS authentication).</span></span> <span data-ttu-id="2e11a-177">Hajtsa végre a következőket hello</span><span class="sxs-lookup"><span data-stu-id="2e11a-177">Perform hello followings</span></span>

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

<span data-ttu-id="2e11a-178">Hello konfiguráció áttekintése</span><span class="sxs-lookup"><span data-stu-id="2e11a-178">Review hello configuration</span></span>
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

<span data-ttu-id="2e11a-179">Ez a hello Oracle Data Guard beállítása befejeződött.</span><span class="sxs-lookup"><span data-stu-id="2e11a-179">This completed hello Oracle Data Guard setup.</span></span> <span data-ttu-id="2e11a-180">hello következő szakasz bemutatja, hogyan tootest hello kapcsolatot, valamint váltás</span><span class="sxs-lookup"><span data-stu-id="2e11a-180">hello next section shows you how tootest hello connectivity and switching over</span></span>

### <a name="connect-database-from-client-machine"></a><span data-ttu-id="2e11a-181">Csatlakozás az adatbázis az ügyfélszámítógépen.</span><span class="sxs-lookup"><span data-stu-id="2e11a-181">Connect database from client machine</span></span>

<span data-ttu-id="2e11a-182">Frissítés, vagy az ügyfélszámítógép, amely általában a következő helyen található: $ORACLE_HOME\network\admin hello tnsnames.ora fájl létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2e11a-182">Update or create hello tnsnames.ora file on your client machine which usually is located at $ORACLE_HOME\network\admin.</span></span>

<span data-ttu-id="2e11a-183">Cserélje le a hello IP-cím a `publicIpAddress` myVM1 és myVM2</span><span class="sxs-lookup"><span data-stu-id="2e11a-183">Replace hello IP with your `publicIpAddress` for myVM1 and myVM2</span></span>

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

<span data-ttu-id="2e11a-184">Indítsa el a sqlplus</span><span class="sxs-lookup"><span data-stu-id="2e11a-184">Start sqlplus</span></span>

```bash
$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```
## <a name="test-data-guard-configuration"></a><span data-ttu-id="2e11a-185">Data Guard tesztkonfiguráció</span><span class="sxs-lookup"><span data-stu-id="2e11a-185">Test Data Guard configuration</span></span>

### <a name="database-switchover-on-myvm1-primary"></a><span data-ttu-id="2e11a-186">Adatbázis-Váltás a myVM1 (elsődleges)</span><span class="sxs-lookup"><span data-stu-id="2e11a-186">Database switchover on myVM1 (primary)</span></span>

<span data-ttu-id="2e11a-187">az elsődleges toostandby (cdb1 toocdb1_stby) tooswitch</span><span class="sxs-lookup"><span data-stu-id="2e11a-187">tooswitch from primary toostandby (cdb1 toocdb1_stby)</span></span>

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

<span data-ttu-id="2e11a-188">Meg kell tudni tooconnect toohello készenléti adatbázis</span><span class="sxs-lookup"><span data-stu-id="2e11a-188">You should now be able tooconnect toohello standby database</span></span>

<span data-ttu-id="2e11a-189">Indítsa el a sqlplus</span><span class="sxs-lookup"><span data-stu-id="2e11a-189">Start sqlplus</span></span>

```bash

$ sqlplus sys/OraPasswd1@cdb1_stby
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

### <a name="database-switch-back-on-myvm2-standby"></a><span data-ttu-id="2e11a-190">Adatbázis-kapcsoló myVM2 be újra (készenléti)</span><span class="sxs-lookup"><span data-stu-id="2e11a-190">Database switch back on myVM2 (standby)</span></span>

<span data-ttu-id="2e11a-191">vissza tooswitch hello következőket futtatnak myVM2</span><span class="sxs-lookup"><span data-stu-id="2e11a-191">tooswitch back, run hello followings on myVM2</span></span>
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

<span data-ttu-id="2e11a-192">Ebben az esetben kell tudni tooconnect toohello elsődleges adatbázis</span><span class="sxs-lookup"><span data-stu-id="2e11a-192">Once again, You should now be able tooconnect toohello primary database</span></span>

<span data-ttu-id="2e11a-193">Indítsa el a sqlplus</span><span class="sxs-lookup"><span data-stu-id="2e11a-193">Start sqlplus</span></span>

```bash

$ sqlplus sys/OraPasswd1@cdb1
SQL*Plus: Release 12.2.0.1.0 Production on Wed May 10 14:18:31 2017

Copyright (c) 1982, 2016, Oracle.  All rights reserved.

Connected to:
Oracle Database 12c Enterprise Edition Release 12.1.0.2.0 - 64bit Production
With hello Partitioning, OLAP, Advanced Analytics and Real Application Testing options

SQL>
```

<span data-ttu-id="2e11a-194">Ez befejeződött, hello telepítésének és konfigurálásának Data Guard Oracle linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="2e11a-194">This completed hello installation and configuration of Data Guard on Oracle linux.</span></span>


## <a name="delete-virtual-machine"></a><span data-ttu-id="2e11a-195">Virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="2e11a-195">Delete virtual machine</span></span>

<span data-ttu-id="2e11a-196">Már nincs szükség, ha a következő parancs hello használt tooremove hello erőforráscsoportot, a virtuális gép, és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="2e11a-196">When no longer needed, hello following command can be used tooremove hello Resource Group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="2e11a-197">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2e11a-197">Next steps</span></span>

[<span data-ttu-id="2e11a-198">Magas rendelkezésre állású virtuális gép létrehozása oktatóanyag</span><span class="sxs-lookup"><span data-stu-id="2e11a-198">Create highly available virtual machines tutorial</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="2e11a-199">A virtuális gépek parancssori felületen való üzembe helyezését ismertető minták megismerése</span><span class="sxs-lookup"><span data-stu-id="2e11a-199">Explore VM deployment CLI samples</span></span>](../../linux/cli-samples.md)
