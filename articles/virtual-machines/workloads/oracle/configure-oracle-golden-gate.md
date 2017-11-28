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
# <a name="implement-oracle-golden-gate-on-an-azure-linux-vm"></a><span data-ttu-id="2c063-103">Oracle Golden kapu valósítja meg az Azure Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="2c063-103">Implement Oracle Golden Gate on an Azure Linux VM</span></span> 

<span data-ttu-id="2c063-104">hello Azure CLI használt toocreate és hello parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="2c063-104">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="2c063-105">Ez az útmutató adatokat hogyan toouse hello Azure CLI toodeploy egy 12c Oracle adatbázis-hello Azure piactér gyűjtemény lemezképből.</span><span class="sxs-lookup"><span data-stu-id="2c063-105">This guide details how toouse hello Azure CLI toodeploy an Oracle 12c database from hello Azure Marketplace gallery image.</span></span> 

<span data-ttu-id="2c063-106">Ez a dokumentum lépésről lépésre bemutatja hogyan toocreate, telepíteni és beállítani az Oracle Golden kapu egy Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="2c063-106">This document shows you step-by-step how toocreate, install, and configure Oracle Golden Gate on an Azure VM.</span></span>

<span data-ttu-id="2c063-107">Mielőtt elkezdené, győződjön meg arról, hogy telepítve van az Azure CLI hello listájában.</span><span class="sxs-lookup"><span data-stu-id="2c063-107">Before you start, make sure that hello Azure CLI has been installed.</span></span> <span data-ttu-id="2c063-108">További információért lásd az [Azure CLI telepítési útmutatóját](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="2c063-108">For more information, see [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-hello-environment"></a><span data-ttu-id="2c063-109">Hello környezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="2c063-109">Prepare hello environment</span></span>

<span data-ttu-id="2c063-110">tooperform hello a Golden kapu Oracle telepítés kell toocreate két Azure-alapú virtuális gépek hello azonos rendelkezésre állási csoportot.</span><span class="sxs-lookup"><span data-stu-id="2c063-110">tooperform hello Oracle Golden Gate installation, you need toocreate two Azure VMs on hello same availability set.</span></span> <span data-ttu-id="2c063-111">hello Piactéri rendszerkép toocreate hello virtuális gépek használata **Oracle: Oracle-adatbázis-Ee:12.1.0.2:latest**.</span><span class="sxs-lookup"><span data-stu-id="2c063-111">hello Marketplace image you use toocreate hello VMs is **Oracle:Oracle-Database-Ee:12.1.0.2:latest**.</span></span>

<span data-ttu-id="2c063-112">Is toobe Unix szerkesztő vi ismernie kell, és megismerheti a x11 (X Windows).</span><span class="sxs-lookup"><span data-stu-id="2c063-112">You also need toobe familiar with Unix editor vi and have a basic understanding of x11 (X Windows).</span></span>

<span data-ttu-id="2c063-113">hello az alábbiakban látható hello környezet konfigurációjának összefoglalása:</span><span class="sxs-lookup"><span data-stu-id="2c063-113">hello following is a summary of hello environment configuration:</span></span>
> 
> |  | <span data-ttu-id="2c063-114">**Elsődleges hely**</span><span class="sxs-lookup"><span data-stu-id="2c063-114">**Primary site**</span></span> | <span data-ttu-id="2c063-115">**Hely replikálása**</span><span class="sxs-lookup"><span data-stu-id="2c063-115">**Replicate site**</span></span> |
> | --- | --- | --- |
> | <span data-ttu-id="2c063-116">**Oracle-kiadás**</span><span class="sxs-lookup"><span data-stu-id="2c063-116">**Oracle release**</span></span> |<span data-ttu-id="2c063-117">Oracle 12c kiadás 2 – (12.1.0.2)</span><span class="sxs-lookup"><span data-stu-id="2c063-117">Oracle 12c Release 2 – (12.1.0.2)</span></span> |<span data-ttu-id="2c063-118">Oracle 12c kiadás 2 – (12.1.0.2)</span><span class="sxs-lookup"><span data-stu-id="2c063-118">Oracle 12c Release 2 – (12.1.0.2)</span></span>|
> | <span data-ttu-id="2c063-119">**Számítógép neve**</span><span class="sxs-lookup"><span data-stu-id="2c063-119">**Machine name**</span></span> |<span data-ttu-id="2c063-120">myVM1</span><span class="sxs-lookup"><span data-stu-id="2c063-120">myVM1</span></span> |<span data-ttu-id="2c063-121">myVM2</span><span class="sxs-lookup"><span data-stu-id="2c063-121">myVM2</span></span> |
> | <span data-ttu-id="2c063-122">**Operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="2c063-122">**Operating system**</span></span> |<span data-ttu-id="2c063-123">Oracle Linux 6.x</span><span class="sxs-lookup"><span data-stu-id="2c063-123">Oracle Linux 6.x</span></span> |<span data-ttu-id="2c063-124">Oracle Linux 6.x</span><span class="sxs-lookup"><span data-stu-id="2c063-124">Oracle Linux 6.x</span></span> |
> | <span data-ttu-id="2c063-125">**Oracle SID**</span><span class="sxs-lookup"><span data-stu-id="2c063-125">**Oracle SID**</span></span> |<span data-ttu-id="2c063-126">CDB1</span><span class="sxs-lookup"><span data-stu-id="2c063-126">CDB1</span></span> |<span data-ttu-id="2c063-127">CDB1</span><span class="sxs-lookup"><span data-stu-id="2c063-127">CDB1</span></span> |
> | <span data-ttu-id="2c063-128">**Replikációs séma**</span><span class="sxs-lookup"><span data-stu-id="2c063-128">**Replication schema**</span></span> |<span data-ttu-id="2c063-129">TESZT</span><span class="sxs-lookup"><span data-stu-id="2c063-129">TEST</span></span>|<span data-ttu-id="2c063-130">TESZT</span><span class="sxs-lookup"><span data-stu-id="2c063-130">TEST</span></span> |
> | <span data-ttu-id="2c063-131">**Tulajdonos vagy replicate Golden kapu**</span><span class="sxs-lookup"><span data-stu-id="2c063-131">**Golden Gate owner/replicate**</span></span> |<span data-ttu-id="2c063-132">C ##GGADMIN</span><span class="sxs-lookup"><span data-stu-id="2c063-132">C##GGADMIN</span></span> |<span data-ttu-id="2c063-133">REPUSER</span><span class="sxs-lookup"><span data-stu-id="2c063-133">REPUSER</span></span> |
> | <span data-ttu-id="2c063-134">**Golden kapu folyamat**</span><span class="sxs-lookup"><span data-stu-id="2c063-134">**Golden Gate process**</span></span> |<span data-ttu-id="2c063-135">EXTORA</span><span class="sxs-lookup"><span data-stu-id="2c063-135">EXTORA</span></span> |<span data-ttu-id="2c063-136">REPORA</span><span class="sxs-lookup"><span data-stu-id="2c063-136">REPORA</span></span>|


### <a name="sign-in-tooazure"></a><span data-ttu-id="2c063-137">Jelentkezzen be tooAzure</span><span class="sxs-lookup"><span data-stu-id="2c063-137">Sign in tooAzure</span></span> 

<span data-ttu-id="2c063-138">Jelentkezzen be Azure előfizetés hello tooyour [az bejelentkezési](/cli/azure/#login) parancsot.</span><span class="sxs-lookup"><span data-stu-id="2c063-138">Sign in tooyour Azure subscription with hello [az login](/cli/azure/#login) command.</span></span> <span data-ttu-id="2c063-139">Ezután kövesse hello képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="2c063-139">Then follow hello on-screen directions.</span></span>

```azurecli
az login
```

### <a name="create-a-resource-group"></a><span data-ttu-id="2c063-140">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="2c063-140">Create a resource group</span></span>

<span data-ttu-id="2c063-141">Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="2c063-141">Create a resource group with hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="2c063-142">Egy Azure erőforráscsoport egy olyan logikai tároló mely Azure-erőforrások vannak telepítve és onnan, ami által kezelhető legyen.</span><span class="sxs-lookup"><span data-stu-id="2c063-142">An Azure resource group is a logical container into which Azure resources are deployed and from which they can be managed.</span></span> 

<span data-ttu-id="2c063-143">hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `westus` helyét.</span><span class="sxs-lookup"><span data-stu-id="2c063-143">hello following example creates a resource group named `myResourceGroup` in hello `westus` location.</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-an-availability-set"></a><span data-ttu-id="2c063-144">Rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="2c063-144">Create an availability set</span></span>

<span data-ttu-id="2c063-145">a következő lépés hello, kötelező, de ajánlott.</span><span class="sxs-lookup"><span data-stu-id="2c063-145">hello following step is optional but recommended.</span></span> <span data-ttu-id="2c063-146">További információkért lásd: [Azure rendelkezésre állási készletek útmutató](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).</span><span class="sxs-lookup"><span data-stu-id="2c063-146">For more information, see [Azure availability sets guide](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-a-virtual-machine"></a><span data-ttu-id="2c063-147">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="2c063-147">Create a virtual machine</span></span>

<span data-ttu-id="2c063-148">Hozzon létre egy virtuális gép hello [az virtuális gép létrehozása](/cli/azure/vm#create) parancsot.</span><span class="sxs-lookup"><span data-stu-id="2c063-148">Create a VM with hello [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="2c063-149">hello alábbi példa létrehoz két virtuális gépek nevű `myVM1` és `myVM2`.</span><span class="sxs-lookup"><span data-stu-id="2c063-149">hello following example creates two VMs named `myVM1` and `myVM2`.</span></span> <span data-ttu-id="2c063-150">SSH-kulcsok létrehozása, ha még nem léteznek a kulcs alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="2c063-150">Create SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="2c063-151">toouse egy adott kulcsok beállításához használja a hello `--ssh-key-value` lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="2c063-151">toouse a specific set of keys, use hello `--ssh-key-value` option.</span></span>

#### <a name="create-myvm1-primary"></a><span data-ttu-id="2c063-152">Hozzon létre myVM1 (elsődleges):</span><span class="sxs-lookup"><span data-stu-id="2c063-152">Create myVM1 (primary):</span></span>
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM1 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

<span data-ttu-id="2c063-153">Hello a virtuális gép létrehozása, miután hello Azure CLI információkat a következő példa hasonló toohello jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="2c063-153">After hello VM has been created, hello Azure CLI shows information similar toohello following example.</span></span> <span data-ttu-id="2c063-154">(Jegyezze fel a hello `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="2c063-154">(Take note of hello `publicIpAddress`.</span></span> <span data-ttu-id="2c063-155">Ez a cím az használt tooaccess hello VM.)</span><span class="sxs-lookup"><span data-stu-id="2c063-155">This address is used tooaccess hello VM.)</span></span>

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

#### <a name="create-myvm2-replicate"></a><span data-ttu-id="2c063-156">Hozzon létre myVM2 (replikáció):</span><span class="sxs-lookup"><span data-stu-id="2c063-156">Create myVM2 (replicate):</span></span>
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM2 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

<span data-ttu-id="2c063-157">Jegyezze fel a hello `publicIpAddress` , valamint a létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="2c063-157">Take note of hello `publicIpAddress` as well after it has been created.</span></span>

### <a name="open-hello-tcp-port-for-connectivity"></a><span data-ttu-id="2c063-158">Nyissa meg a TCP-portját hello kapcsolat</span><span class="sxs-lookup"><span data-stu-id="2c063-158">Open hello TCP port for connectivity</span></span>

<span data-ttu-id="2c063-159">következő lépés hello tooconfigure külső végpontok száma, amelyek lehetővé teszik a távoli tooaccess hello Oracle-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="2c063-159">hello next step is tooconfigure external endpoints,  which enable you tooaccess hello Oracle database remotely.</span></span> <span data-ttu-id="2c063-160">tooconfigure hello külső végpontok száma, futtassa a következő parancsok hello.</span><span class="sxs-lookup"><span data-stu-id="2c063-160">tooconfigure hello external endpoints, run hello following commands.</span></span>

#### <a name="open-hello-port-for-myvm1"></a><span data-ttu-id="2c063-161">Nyissa meg a hello port myVM1:</span><span class="sxs-lookup"><span data-stu-id="2c063-161">Open hello port for myVM1:</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm1NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

<span data-ttu-id="2c063-162">hello eredmények alábbihoz hasonló toohello válasz a következő:</span><span class="sxs-lookup"><span data-stu-id="2c063-162">hello results should look similar toohello following response:</span></span>

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

#### <a name="open-hello-port-for-myvm2"></a><span data-ttu-id="2c063-163">Nyissa meg a hello port myVM2:</span><span class="sxs-lookup"><span data-stu-id="2c063-163">Open hello port for myVM2:</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm2NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-toohello-virtual-machine"></a><span data-ttu-id="2c063-164">Csatlakoztassa toohello virtuális gépet</span><span class="sxs-lookup"><span data-stu-id="2c063-164">Connect toohello virtual machine</span></span>

<span data-ttu-id="2c063-165">Használjon hello következő parancsot a toocreate egy hello virtuális gép SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="2c063-165">Use hello following command toocreate an SSH session with hello virtual machine.</span></span> <span data-ttu-id="2c063-166">Cserélje le hello IP-cím hello `publicIpAddress` a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="2c063-166">Replace hello IP address with hello `publicIpAddress` of your virtual machine.</span></span>

```bash 
ssh <publicIpAddress>
```

### <a name="create-hello-database-on-myvm1-primary"></a><span data-ttu-id="2c063-167">Adatbázis létrehozásához. hello myVM1 (elsődleges)</span><span class="sxs-lookup"><span data-stu-id="2c063-167">Create hello database on myVM1 (primary)</span></span>

<span data-ttu-id="2c063-168">hello Oracle szoftver már telepítve van hello Piactéri lemezképhez, így a következő lépésre hello tooinstall hello adatbázis.</span><span class="sxs-lookup"><span data-stu-id="2c063-168">hello Oracle software is already installed on hello Marketplace image, so hello next step is tooinstall hello database.</span></span> 

<span data-ttu-id="2c063-169">Hello szoftvert futtató hello "oracle" felügyelő:</span><span class="sxs-lookup"><span data-stu-id="2c063-169">Run hello software as hello 'oracle' superuser:</span></span>

```bash
sudo su - oracle
```

<span data-ttu-id="2c063-170">hello adatbázis létrehozása:</span><span class="sxs-lookup"><span data-stu-id="2c063-170">Create hello database:</span></span>

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
<span data-ttu-id="2c063-171">Kimenetek alábbihoz hasonló toohello válasz a következő:</span><span class="sxs-lookup"><span data-stu-id="2c063-171">Outputs should look similar toohello following response:</span></span>

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

<span data-ttu-id="2c063-172">Hello ORACLE_SID és ORACLE_HOME változók megadása.</span><span class="sxs-lookup"><span data-stu-id="2c063-172">Set hello ORACLE_SID and ORACLE_HOME variables.</span></span>

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=gg1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

<span data-ttu-id="2c063-173">Szükség esetén ORACLE_HOME és ORACLE_SID toohello .bashrc fájl, adhat hozzá, hogy ezek a beállítások a későbbi bejelentkezések lesznek mentve:</span><span class="sxs-lookup"><span data-stu-id="2c063-173">Optionally, you can add ORACLE_HOME and ORACLE_SID toohello .bashrc file, so that these settings are saved for future sign-ins:</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=gg1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a><span data-ttu-id="2c063-174">Indítsa el az Oracle-figyelő</span><span class="sxs-lookup"><span data-stu-id="2c063-174">Start Oracle listener</span></span>
```bash
$ sudo su - oracle
$ lsnrctl start
```

### <a name="create-hello-database-on-myvm2-replicate"></a><span data-ttu-id="2c063-175">Hello adatbázis létrehozása a következőn myVM2 (replikáció)</span><span class="sxs-lookup"><span data-stu-id="2c063-175">Create hello database on myVM2 (replicate)</span></span>

```bash
sudo su - oracle
```
<span data-ttu-id="2c063-176">hello adatbázis létrehozása:</span><span class="sxs-lookup"><span data-stu-id="2c063-176">Create hello database:</span></span>

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
<span data-ttu-id="2c063-177">Hello ORACLE_SID és ORACLE_HOME változók megadása.</span><span class="sxs-lookup"><span data-stu-id="2c063-177">Set hello ORACLE_SID and ORACLE_HOME variables.</span></span>

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

<span data-ttu-id="2c063-178">Szükség esetén is felvett ORACLE_HOME és ORACLE_SID toohello .bashrc fájlt, így ezek a beállítások lesznek mentve a későbbi bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="2c063-178">Optionally, you can added ORACLE_HOME and ORACLE_SID toohello .bashrc file, so that these settings are saved for future sign-ins.</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a><span data-ttu-id="2c063-179">Indítsa el az Oracle-figyelő</span><span class="sxs-lookup"><span data-stu-id="2c063-179">Start Oracle listener</span></span>
```bash
$ sudo su - oracle
$ lsnrctl start
```

## <a name="configure-golden-gate"></a><span data-ttu-id="2c063-180">Golden kapu beállítása</span><span class="sxs-lookup"><span data-stu-id="2c063-180">Configure Golden Gate</span></span> 
<span data-ttu-id="2c063-181">tooconfigure Golden kapu, ebben a szakaszban hello lépéseket.</span><span class="sxs-lookup"><span data-stu-id="2c063-181">tooconfigure Golden Gate, take hello steps in this section.</span></span>

### <a name="enable-archive-log-mode-on-myvm1-primary"></a><span data-ttu-id="2c063-182">Archív napló mód a myVM1 (elsődleges) engedélyezése</span><span class="sxs-lookup"><span data-stu-id="2c063-182">Enable archive log mode on myVM1 (primary)</span></span>

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
<span data-ttu-id="2c063-183">Kényszerített naplózását, és ellenőrizze, hogy legalább egy naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="2c063-183">Enable force logging, and make sure at least one log file is present.</span></span>

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
SQL> ALTER SYSTEM set enable_goldengate_replication=true;
SQL> ALTER PLUGGABLE DATABASE PDB1 OPEN;
SQL> ALTER SESSION SET CONTAINER=PDB1;
SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA;
SQL> EXIT;
```

### <a name="download-golden-gate-software"></a><span data-ttu-id="2c063-184">Golden kapu szoftver letöltése</span><span class="sxs-lookup"><span data-stu-id="2c063-184">Download Golden Gate software</span></span>
<span data-ttu-id="2c063-185">toodownload és hello Oracle Golden kapu szoftver, a következő lépéseket teljes hello előkészítése:</span><span class="sxs-lookup"><span data-stu-id="2c063-185">toodownload and prepare hello Oracle Golden Gate software, complete hello following steps:</span></span>

1. <span data-ttu-id="2c063-186">Töltse le a hello **fbo_ggs_Linux_x64_shiphome.zip** hello fájlt [Oracle Golden kapu letöltési oldala](http://www.oracle.com/technetwork/middleware/goldengate/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="2c063-186">Download hello **fbo_ggs_Linux_x64_shiphome.zip** file from hello [Oracle Golden Gate download page](http://www.oracle.com/technetwork/middleware/goldengate/downloads/index.html).</span></span> <span data-ttu-id="2c063-187">Hello alatt töltse le a cím **Oracle GoldenGate 12.x.x.x az Oracle Linux x86-64**, .zip fájlokat toodownload készlete kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2c063-187">Under hello download title **Oracle GoldenGate 12.x.x.x for Oracle Linux x86-64**, there should be a set of .zip files toodownload.</span></span>

2. <span data-ttu-id="2c063-188">Hello .zip fájlokat tooyour ügyfélszámítógép letöltése után használja a biztonságos másolási protokoll (SCP) toocopy hello fájlok tooyour VM:</span><span class="sxs-lookup"><span data-stu-id="2c063-188">After you download hello .zip files tooyour client computer, use Secure Copy Protocol (SCP) toocopy hello files tooyour VM:</span></span>

  ```bash
  $ scp fbo_ggs_Linux_x64_shiphome.zip <publicIpAddress>:<folder>
  ```

3. <span data-ttu-id="2c063-189">Helyezze át a hello .zip fájlokat toohello **/ opt** mappát.</span><span class="sxs-lookup"><span data-stu-id="2c063-189">Move hello .zip files toohello **/opt** folder.</span></span> <span data-ttu-id="2c063-190">Az alábbiak szerint módosítsa a hello fájlok hello tulajdonosa:</span><span class="sxs-lookup"><span data-stu-id="2c063-190">Then change hello owner of hello files as follows:</span></span>

  ```bash
  $ sudo su -
  # mv <folder>/*.zip /opt
  ```

4. <span data-ttu-id="2c063-191">Bontsa ki a hello fájlok (telepítés hello Linux csomagolja ki segédprogram Ha még nincs telepítve van):</span><span class="sxs-lookup"><span data-stu-id="2c063-191">Unzip hello files (install hello Linux unzip utility if it's not already installed):</span></span>

  ```bash
  # yum install unzip
  # cd /opt
  # unzip fbo_ggs_Linux_x64_shiphome.zip
  ```

5. <span data-ttu-id="2c063-192">Engedély módosítása:</span><span class="sxs-lookup"><span data-stu-id="2c063-192">Change permission:</span></span>

  ```bash
  # chown -R oracle:oinstall /opt/fbo_ggs_Linux_x64_shiphome
  ```

### <a name="prepare-hello-client-and-vm-toorun-x11-for-windows-clients-only"></a><span data-ttu-id="2c063-193">Előkészítése hello ügyfél és a virtuális gép toorun x11 (csak Windows-ügyfelek)</span><span class="sxs-lookup"><span data-stu-id="2c063-193">Prepare hello client and VM toorun x11 (for Windows clients only)</span></span>
<span data-ttu-id="2c063-194">Ez az egy opcionális lépés.</span><span class="sxs-lookup"><span data-stu-id="2c063-194">This is an optional step.</span></span> <span data-ttu-id="2c063-195">Ezt a lépést kihagyhatja, ha a Linux ügyfelet kell használnia, vagy már rendelkezik x11 beállítása.</span><span class="sxs-lookup"><span data-stu-id="2c063-195">You can skip this step if you are using a Linux client or already have x11 setup.</span></span>

1. <span data-ttu-id="2c063-196">Töltse le a PuTTY és Xming tooyour Windows-számítógép:</span><span class="sxs-lookup"><span data-stu-id="2c063-196">Download PuTTY and Xming tooyour Windows computer:</span></span>

  * [<span data-ttu-id="2c063-197">Töltse le a PuTTY</span><span class="sxs-lookup"><span data-stu-id="2c063-197">Download PuTTY</span></span>](http://www.putty.org/)
  * [<span data-ttu-id="2c063-198">Töltse le a Xming</span><span class="sxs-lookup"><span data-stu-id="2c063-198">Download Xming</span></span>](https://xming.en.softonic.com/)

2.  <span data-ttu-id="2c063-199">PuTTY telepítése után a PuTTY mappát (például, C:\Program Files\PuTTY) hello, puttygen.exe (PuTTY kulcs generátor) futtatásához.</span><span class="sxs-lookup"><span data-stu-id="2c063-199">After you install PuTTY, in hello PuTTY folder (for example, C:\Program Files\PuTTY), run puttygen.exe (PuTTY Key Generator).</span></span>

3.  <span data-ttu-id="2c063-200">A PuTTY Megosztottelérésikulcs-készítő:</span><span class="sxs-lookup"><span data-stu-id="2c063-200">In PuTTY Key Generator:</span></span>

  - <span data-ttu-id="2c063-201">a kulcs, jelölje be hello toogenerate **Generate** gombra.</span><span class="sxs-lookup"><span data-stu-id="2c063-201">toogenerate a key, select hello **Generate** button.</span></span>
  - <span data-ttu-id="2c063-202">Hello kulcs hello tartalom másolása (**Ctrl + C**).</span><span class="sxs-lookup"><span data-stu-id="2c063-202">Copy hello contents of hello key (**Ctrl+C**).</span></span>
  - <span data-ttu-id="2c063-203">Jelölje be hello **mentés titkos kulcs** gombra.</span><span class="sxs-lookup"><span data-stu-id="2c063-203">Select hello **Save private key** button.</span></span>
  - <span data-ttu-id="2c063-204">Figyelmen kívül, amely akkor jelenik meg, majd válassza ki a hello figyelmeztetést **OK**.</span><span class="sxs-lookup"><span data-stu-id="2c063-204">Ignore hello warning that appears, and then select **OK**.</span></span>

    ![Hello PuTTY megosztottelérésikulcs-készítő oldalát bemutató képernyőkép](./media/oracle-golden-gate/puttykeygen.png)

4.  <span data-ttu-id="2c063-206">A virtuális gépen, futtassa az alábbi parancsokat:</span><span class="sxs-lookup"><span data-stu-id="2c063-206">In your VM, run these commands:</span></span>

  ```bash
  # sudo su - oracle
  $ mkdir .ssh (if not already created)
  $ cd .ssh
  ```

5. <span data-ttu-id="2c063-207">Hozzon létre egy fájlt **authorized_keys**.</span><span class="sxs-lookup"><span data-stu-id="2c063-207">Create a file named **authorized_keys**.</span></span> <span data-ttu-id="2c063-208">Hello kulcs tartalmát hello illessze be ezt a fájlt, és mentse hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="2c063-208">Paste hello contents of hello key in this file, and then save hello file.</span></span>

  > [!NOTE]
  > <span data-ttu-id="2c063-209">hello kulcs tartalmaznia kell a hello karakterlánc `ssh-rsa`.</span><span class="sxs-lookup"><span data-stu-id="2c063-209">hello key must contain hello string `ssh-rsa`.</span></span> <span data-ttu-id="2c063-210">Hello hello kulcs tartalmát is, egy egyszerű szövegsor kell lennie.</span><span class="sxs-lookup"><span data-stu-id="2c063-210">Also, hello contents of hello key must be a single line of text.</span></span>
  >  

6. <span data-ttu-id="2c063-211">Indítsa el a PuTTY alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="2c063-211">Start PuTTY.</span></span> <span data-ttu-id="2c063-212">A hello **kategória** ablaktáblán válassza előbb **kapcsolat** > **SSH** > **Auth**. A hello **hitelesítéshez titkos kulcsfájl** mezőbe keresse meg a korábban létrehozott toohello kulcs.</span><span class="sxs-lookup"><span data-stu-id="2c063-212">In hello **Category** pane, select **Connection** > **SSH** > **Auth**. In hello **Private key file for authentication** box, browse toohello key that you generated earlier.</span></span>

  ![Hello titkos kulcs beállítása oldalát bemutató képernyőkép](./media/oracle-golden-gate/setprivatekey.png)

7. <span data-ttu-id="2c063-214">A hello **kategória** ablaktáblán válassza előbb **kapcsolat** > **SSH** > **X11**.</span><span class="sxs-lookup"><span data-stu-id="2c063-214">In hello **Category** pane, select **Connection** > **SSH** > **X11**.</span></span> <span data-ttu-id="2c063-215">Válassza ki hello **engedélyezése X11 továbbítási** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="2c063-215">Then select hello **Enable X11 forwarding** box.</span></span>

  ![Hello engedélyezése X11 oldalát bemutató képernyőkép](./media/oracle-golden-gate/enablex11.png)

8. <span data-ttu-id="2c063-217">A hello **kategória** ablaktáblában lépjen túl**munkamenet**.</span><span class="sxs-lookup"><span data-stu-id="2c063-217">In hello **Category** pane, go too**Session**.</span></span> <span data-ttu-id="2c063-218">Adja meg a hello állomásadatai, majd válassza ki **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="2c063-218">Enter hello host information, and then select **Open**.</span></span>

  ![Hello munkamenet oldalát bemutató képernyőkép](./media/oracle-golden-gate/puttysession.png)

### <a name="install-golden-gate-software"></a><span data-ttu-id="2c063-220">Golden kapu szoftver telepítése</span><span class="sxs-lookup"><span data-stu-id="2c063-220">Install Golden Gate software</span></span>

<span data-ttu-id="2c063-221">tooinstall Oracle Golden kapu, teljes hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="2c063-221">tooinstall Oracle Golden Gate, complete hello following steps:</span></span>

1. <span data-ttu-id="2c063-222">Jelentkezzen be a oracle felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="2c063-222">Sign in as oracle.</span></span> <span data-ttu-id="2c063-223">(A jelszó megadása nélkül képes toosign kell lennie.) Győződjön meg arról, hogy fut-e Xming, hello telepítés megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="2c063-223">(You should be able toosign in without being prompted for a password.) Make sure that Xming is running before you begin hello installation.</span></span>
 
  ```bash
  $ cd /opt/fbo_ggs_Linux_x64_shiphome/Disk1
  $ ./runInstaller
  ```
2. <span data-ttu-id="2c063-224">Válassza ki az "Oracle adatbázis 12c Oracle GoldenGate".</span><span class="sxs-lookup"><span data-stu-id="2c063-224">Select 'Oracle GoldenGate for Oracle Database 12c'.</span></span> <span data-ttu-id="2c063-225">Válassza ki **következő** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="2c063-225">Then select **Next** toocontinue.</span></span>

  ![Képernyőfelvétel a hello telepítő telepítési kiválasztása lap](./media/oracle-golden-gate/golden_gate_install_01.png)

3. <span data-ttu-id="2c063-227">Hello szoftver helyének módosítása.</span><span class="sxs-lookup"><span data-stu-id="2c063-227">Change hello software location.</span></span> <span data-ttu-id="2c063-228">Válassza ki hello **Start Manager** mezőben, és írja be a hello adatbázis helye.</span><span class="sxs-lookup"><span data-stu-id="2c063-228">Then select  hello **Start Manager** box and enter hello database location.</span></span> <span data-ttu-id="2c063-229">Válassza ki **következő** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="2c063-229">Select **Next** toocontinue.</span></span>

  ![Képernyőfelvétel a hello telepítési kiválasztása lap](./media/oracle-golden-gate/golden_gate_install_02.png)

4. <span data-ttu-id="2c063-231">Módosítsa hello készlet könyvtárat, majd válassza ki **következő** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="2c063-231">Change hello inventory directory, and then select **Next** toocontinue.</span></span>

  ![Képernyőfelvétel a hello telepítési kiválasztása lap](./media/oracle-golden-gate/golden_gate_install_03.png)

5. <span data-ttu-id="2c063-233">A hello **összegzés** képernyőn válassza ki **telepítése** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="2c063-233">On hello **Summary** screen, select **Install** toocontinue.</span></span>

  ![Képernyőfelvétel a hello telepítő telepítési kiválasztása lap](./media/oracle-golden-gate/golden_gate_install_04.png)

6. <span data-ttu-id="2c063-235">Előfordulhat, hogy a kért toorun parancsfájl "legfelső szintű".</span><span class="sxs-lookup"><span data-stu-id="2c063-235">You might be prompted toorun a script as 'root'.</span></span> <span data-ttu-id="2c063-236">Ha igen, nyisson meg egy külön munkamenet ssh toohello VM, sudo tooroot, és futtassa hello parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="2c063-236">If so, open a separate session, ssh toohello VM, sudo tooroot, and then run hello script.</span></span> <span data-ttu-id="2c063-237">Válassza ki **OK** továbbra is.</span><span class="sxs-lookup"><span data-stu-id="2c063-237">Select **OK** continue.</span></span>

  ![Képernyőfelvétel a hello telepítési kiválasztása lap](./media/oracle-golden-gate/golden_gate_install_05.png)

7. <span data-ttu-id="2c063-239">Hello telepítésének befejezését, válassza ki a **Bezárás** toocomplete hello folyamat.</span><span class="sxs-lookup"><span data-stu-id="2c063-239">When hello installation has finished, select **Close** toocomplete hello process.</span></span>

  ![Képernyőfelvétel a hello telepítési kiválasztása lap](./media/oracle-golden-gate/golden_gate_install_06.png)

### <a name="set-up-service-on-myvm1-primary"></a><span data-ttu-id="2c063-241">Állítsa be a szolgáltatást a myVM1 (elsődleges)</span><span class="sxs-lookup"><span data-stu-id="2c063-241">Set up service on myVM1 (primary)</span></span>

1. <span data-ttu-id="2c063-242">Hozzon létre vagy hello tnsnames.ora fájl frissítése:</span><span class="sxs-lookup"><span data-stu-id="2c063-242">Create or update hello tnsnames.ora file:</span></span>

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

2. <span data-ttu-id="2c063-243">Hello Golden kapu tulajdonosi és felhasználói fiókokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="2c063-243">Create hello Golden Gate owner and user accounts.</span></span>

  > [!NOTE]
  > <span data-ttu-id="2c063-244">hello tulajdonos fióknak rendelkeznie kell a C ## előtag.</span><span class="sxs-lookup"><span data-stu-id="2c063-244">hello owner account must have C## prefix.</span></span>
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

3. <span data-ttu-id="2c063-245">Hello Golden kapu teszt felhasználói fiók létrehozása:</span><span class="sxs-lookup"><span data-stu-id="2c063-245">Create hello Golden Gate test user account:</span></span>

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

4. <span data-ttu-id="2c063-246">Hello kivonat paraméterfájl konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="2c063-246">Configure hello extract parameter file.</span></span>

 <span data-ttu-id="2c063-247">Indítsa el a hello arany kapu parancssori felület (ggsci):</span><span class="sxs-lookup"><span data-stu-id="2c063-247">Start hello Golden gate command-line interface (ggsci):</span></span>

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
5. <span data-ttu-id="2c063-248">Adja hozzá a következő toohello kivonat paraméter fájl (parancsokkal vi) hello.</span><span class="sxs-lookup"><span data-stu-id="2c063-248">Add hello following toohello EXTRACT parameter file (by using vi commands).</span></span> <span data-ttu-id="2c063-249">Nyomja meg az Esc billentyűt, ': wq! "</span><span class="sxs-lookup"><span data-stu-id="2c063-249">Press Esc key, ':wq!'</span></span> <span data-ttu-id="2c063-250">toosave fájlt.</span><span class="sxs-lookup"><span data-stu-id="2c063-250">toosave file.</span></span> 

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
6. <span data-ttu-id="2c063-251">Regisztráció nyerje ki – integrált kivonat:</span><span class="sxs-lookup"><span data-stu-id="2c063-251">Register extract--integrated extract:</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci

  GGSCI> dblogin userid C##GGADMIN, password ggadmin
  Successfully logged into database CDB$ROOT.

  GGSCI> REGISTER EXTRACT EXTORA DATABASE CONTAINER(pdb1)

  2017-05-23 15:58:34  INFO    OGG-02003  Extract EXTORA successfully registered with database at SCN 1821260.

  GGSCI> exit
  ```
7. <span data-ttu-id="2c063-252">Kinyerési ellenőrzőpontok felállítása, és indítsa el a valós idejű kivonat:</span><span class="sxs-lookup"><span data-stu-id="2c063-252">Set up extract checkpoints and start real-time extract:</span></span>

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
<span data-ttu-id="2c063-253">Ebben a lépésben található hello Állapotváltozás, később, a másik szakaszban használt indítása:</span><span class="sxs-lookup"><span data-stu-id="2c063-253">In this step, you find hello starting SCN, which will be used later, in a different section:</span></span>

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

### <a name="set-up-service-on-myvm2-replicate"></a><span data-ttu-id="2c063-254">A myVM2 szolgáltatás beállítása (replikáció)</span><span class="sxs-lookup"><span data-stu-id="2c063-254">Set up service on myVM2 (replicate)</span></span>


1. <span data-ttu-id="2c063-255">Hozzon létre vagy hello tnsnames.ora fájl frissítése:</span><span class="sxs-lookup"><span data-stu-id="2c063-255">Create or update hello tnsnames.ora file:</span></span>

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

2. <span data-ttu-id="2c063-256">Hozzon létre egy párhuzamos fiókot:</span><span class="sxs-lookup"><span data-stu-id="2c063-256">Create a replicate account:</span></span>

  ```bash
  $ sqlplus / as sysdba
  SQL> alter session set container = pdb1;
  SQL> create user repuser identified by rep_pass container=current;
  SQL> grant dba toorepuser;
  SQL> exec dbms_goldengate_auth.grant_admin_privilege('REPUSER',container=>'PDB1');
  SQL> connect repuser/rep_pass@pdb1 
  SQL> EXIT;
  ```

3. <span data-ttu-id="2c063-257">Golden kapu teszt felhasználói fiók létrehozása:</span><span class="sxs-lookup"><span data-stu-id="2c063-257">Create a Golden Gate test user account:</span></span>

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

4. <span data-ttu-id="2c063-258">REPLICAT paraméter fájl tooreplicate változásai:</span><span class="sxs-lookup"><span data-stu-id="2c063-258">REPLICAT parameter file tooreplicate changes:</span></span> 

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS REPORA  
  ```
  <span data-ttu-id="2c063-259">REPORA paraméter fájl tartalma:</span><span class="sxs-lookup"><span data-stu-id="2c063-259">Content of REPORA parameter file:</span></span>

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

5. <span data-ttu-id="2c063-260">Replicat ellenőrzőpont beállítása:</span><span class="sxs-lookup"><span data-stu-id="2c063-260">Set up a replicat checkpoint:</span></span>

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

### <a name="set-up-hello-replication-myvm1-and-myvm2"></a><span data-ttu-id="2c063-261">Hello replikációjának (myVM1 és myVM2)</span><span class="sxs-lookup"><span data-stu-id="2c063-261">Set up hello replication (myVM1 and myVM2)</span></span>

#### <a name="1-set-up-hello-replication-on-myvm2-replicate"></a><span data-ttu-id="2c063-262">1. A replikáció myVM2 hello beállítása (replikáció)</span><span class="sxs-lookup"><span data-stu-id="2c063-262">1. Set up hello replication on myVM2 (replicate)</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS MGR
  ```
<span data-ttu-id="2c063-263">Hello fájl frissítése hello alábbira:</span><span class="sxs-lookup"><span data-stu-id="2c063-263">Update hello file with hello following:</span></span>

  ```bash
  PORT 7809
  ACCESSRULE, PROG *, IPADDR *, ALLOW
  ```
<span data-ttu-id="2c063-264">Indítsa újra a hello kezelő szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="2c063-264">Then restart hello Manager service:</span></span>

  ```bash
  GGSCI> STOP MGR
  GGSCI> START MGR
  GGSCI> EXIT
  ```

#### <a name="2-set-up-hello-replication-on-myvm1-primary"></a><span data-ttu-id="2c063-265">2. (Elsődleges) myVM1 hello a replikáció beállítása</span><span class="sxs-lookup"><span data-stu-id="2c063-265">2. Set up hello replication on myVM1 (primary)</span></span>

<span data-ttu-id="2c063-266">Indítsa el a kezdeti betöltés hello és a hibák:</span><span class="sxs-lookup"><span data-stu-id="2c063-266">Start hello initial load and check for errors:</span></span>

```bash
$ cd /u01/app/oracle/product/12.1.0/oggcore_1
$ ./ggsci
GGSCI> START EXTRACT INITEXT
GGSCI> VIEW REPORT INITEXT
```
#### <a name="3-set-up-hello-replication-on-myvm2-replicate"></a><span data-ttu-id="2c063-267">3. A replikáció myVM2 hello beállítása (replikáció)</span><span class="sxs-lookup"><span data-stu-id="2c063-267">3. Set up hello replication on myVM2 (replicate)</span></span>

<span data-ttu-id="2c063-268">Hello Állapotváltozás számot hello előtt beszerzett módosítása:</span><span class="sxs-lookup"><span data-stu-id="2c063-268">Change hello SCN number with hello number you obtained before:</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  START REPLICAT REPORA, AFTERCSN 1857887
  ```
<span data-ttu-id="2c063-269">hello replikációs megkezdődött, és a teszteléshez le új rekordok tooTEST táblák beszúrva.</span><span class="sxs-lookup"><span data-stu-id="2c063-269">hello replication has begun, and you can test it by inserting new records tooTEST tables.</span></span>


### <a name="view-job-status-and-troubleshooting"></a><span data-ttu-id="2c063-270">Feladat állapotának megtekintése és hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="2c063-270">View job status and troubleshooting</span></span>

#### <a name="view-reports"></a><span data-ttu-id="2c063-271">Jelentések megtekintése</span><span class="sxs-lookup"><span data-stu-id="2c063-271">View reports</span></span>
<span data-ttu-id="2c063-272">tooview myVM1, futtassa a következő parancsok hello jelentések:</span><span class="sxs-lookup"><span data-stu-id="2c063-272">tooview reports on myVM1, run hello following commands:</span></span>

  ```bash
  GGSCI> VIEW REPORT EXTORA 
  ```
 
<span data-ttu-id="2c063-273">tooview myVM2, futtassa a következő parancsok hello jelentések:</span><span class="sxs-lookup"><span data-stu-id="2c063-273">tooview reports on myVM2, run hello following commands:</span></span>

  ```bash
  GGSCI> VIEW REPORT REPORA
  ```

#### <a name="view-status-and-history"></a><span data-ttu-id="2c063-274">Nézet állapota és előzményei</span><span class="sxs-lookup"><span data-stu-id="2c063-274">View status and history</span></span>
<span data-ttu-id="2c063-275">tooview állapota és előzményei a myVM1, futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="2c063-275">tooview status and history on myVM1, run hello following commands:</span></span>

  ```bash
  GGSCI> dblogin userid c##ggadmin, password ggadmin 
  GGSCI> INFO EXTRACT EXTORA, DETAIL
  ```

<span data-ttu-id="2c063-276">tooview állapota és előzményei a myVM2, futtassa a következő parancsok hello:</span><span class="sxs-lookup"><span data-stu-id="2c063-276">tooview status and history on myVM2, run hello following commands:</span></span>

  ```bash
  GGSCI> dblogin userid repuser@pdb1 password rep_pass 
  GGSCI> INFO REP REPORA, DETAIL
  ```
<span data-ttu-id="2c063-277">Ezzel befejezte a hello telepítés és konfigurálás Golden kapu Oracle linux rendszeren.</span><span class="sxs-lookup"><span data-stu-id="2c063-277">This completes hello installation and configuration of Golden Gate on Oracle linux.</span></span>


## <a name="delete-hello-virtual-machine"></a><span data-ttu-id="2c063-278">Hello virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="2c063-278">Delete hello virtual machine</span></span>

<span data-ttu-id="2c063-279">Már nincs szükség, ha a következő parancs hello lehet használt tooremove hello erőforráscsoport, virtuális gép és minden kapcsolódó erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="2c063-279">When it's no longer needed, hello following command can be used tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="2c063-280">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2c063-280">Next steps</span></span>

[<span data-ttu-id="2c063-281">Magas rendelkezésre állású virtuális gép létrehozása oktatóanyag</span><span class="sxs-lookup"><span data-stu-id="2c063-281">Create highly available virtual machines tutorial</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="2c063-282">A virtuális gépek parancssori felületen való üzembe helyezését ismertető minták megismerése</span><span class="sxs-lookup"><span data-stu-id="2c063-282">Explore VM deployment CLI samples</span></span>](../../linux/cli-samples.md)
