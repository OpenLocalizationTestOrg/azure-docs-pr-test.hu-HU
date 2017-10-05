---
title: "Oracle Golden kapu valósítja meg az Azure Linux virtuális gép |} Microsoft Docs"
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
ms.openlocfilehash: a05711357d345267647c02e42336fd37c09e1bff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="implement-oracle-golden-gate-on-an-azure-linux-vm"></a><span data-ttu-id="bbeb9-103">Oracle Golden kapu valósítja meg az Azure Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="bbeb9-103">Implement Oracle Golden Gate on an Azure Linux VM</span></span> 

<span data-ttu-id="bbeb9-104">Az Azure CLI az Azure-erőforrások parancssorból vagy szkriptekkel történő létrehozására és kezelésére használható.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-104">The Azure CLI is used to create and manage Azure resources from the command line or in scripts.</span></span> <span data-ttu-id="bbeb9-105">Ez az útmutató részletesen használata az Azure parancssori felület telepítése az Azure piactér gyűjtemény lemezképről való 12c Oracle-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-105">This guide details how to use the Azure CLI to deploy an Oracle 12c database from the Azure Marketplace gallery image.</span></span> 

<span data-ttu-id="bbeb9-106">Ez a dokumentum lépésről lépésre bemutatja létrehozása, telepítése és konfigurálása az Oracle Golden kapu egy Azure virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-106">This document shows you step-by-step how to create, install, and configure Oracle Golden Gate on an Azure VM.</span></span>

<span data-ttu-id="bbeb9-107">A kezdés előtt győződjön meg arról, hogy az Azure CLI telepítve van.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-107">Before you start, make sure that the Azure CLI has been installed.</span></span> <span data-ttu-id="bbeb9-108">További információért lásd az [Azure CLI telepítési útmutatóját](https://docs.microsoft.com/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="bbeb9-108">For more information, see [Azure CLI installation guide](https://docs.microsoft.com/cli/azure/install-azure-cli).</span></span>

## <a name="prepare-the-environment"></a><span data-ttu-id="bbeb9-109">A környezet előkészítése</span><span class="sxs-lookup"><span data-stu-id="bbeb9-109">Prepare the environment</span></span>

<span data-ttu-id="bbeb9-110">Az Oracle Golden kapu telepítés elvégzéséhez szüksége az azonos rendelkezésre állási készlet két Azure virtuális gépek létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-110">To perform the Oracle Golden Gate installation, you need to create two Azure VMs on the same availability set.</span></span> <span data-ttu-id="bbeb9-111">A Piactéri lemezképet hozhat létre a virtuális gépek **Oracle: Oracle-adatbázis-Ee:12.1.0.2:latest**.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-111">The Marketplace image you use to create the VMs is **Oracle:Oracle-Database-Ee:12.1.0.2:latest**.</span></span>

<span data-ttu-id="bbeb9-112">Szükség Unix szerkesztő vi ismernie kell, és megismerheti a x11 (X Windows).</span><span class="sxs-lookup"><span data-stu-id="bbeb9-112">You also need to be familiar with Unix editor vi and have a basic understanding of x11 (X Windows).</span></span>

<span data-ttu-id="bbeb9-113">Az alábbiakban áttekintjük a környezet konfigurációjának összefoglalása:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-113">The following is a summary of the environment configuration:</span></span>
> 
> |  | <span data-ttu-id="bbeb9-114">**Elsődleges hely**</span><span class="sxs-lookup"><span data-stu-id="bbeb9-114">**Primary site**</span></span> | <span data-ttu-id="bbeb9-115">**Hely replikálása**</span><span class="sxs-lookup"><span data-stu-id="bbeb9-115">**Replicate site**</span></span> |
> | --- | --- | --- |
> | <span data-ttu-id="bbeb9-116">**Oracle-kiadás**</span><span class="sxs-lookup"><span data-stu-id="bbeb9-116">**Oracle release**</span></span> |<span data-ttu-id="bbeb9-117">Oracle 12c kiadás 2 – (12.1.0.2)</span><span class="sxs-lookup"><span data-stu-id="bbeb9-117">Oracle 12c Release 2 – (12.1.0.2)</span></span> |<span data-ttu-id="bbeb9-118">Oracle 12c kiadás 2 – (12.1.0.2)</span><span class="sxs-lookup"><span data-stu-id="bbeb9-118">Oracle 12c Release 2 – (12.1.0.2)</span></span>|
> | <span data-ttu-id="bbeb9-119">**Számítógép neve**</span><span class="sxs-lookup"><span data-stu-id="bbeb9-119">**Machine name**</span></span> |<span data-ttu-id="bbeb9-120">myVM1</span><span class="sxs-lookup"><span data-stu-id="bbeb9-120">myVM1</span></span> |<span data-ttu-id="bbeb9-121">myVM2</span><span class="sxs-lookup"><span data-stu-id="bbeb9-121">myVM2</span></span> |
> | <span data-ttu-id="bbeb9-122">**Operációs rendszer**</span><span class="sxs-lookup"><span data-stu-id="bbeb9-122">**Operating system**</span></span> |<span data-ttu-id="bbeb9-123">Oracle Linux 6.x</span><span class="sxs-lookup"><span data-stu-id="bbeb9-123">Oracle Linux 6.x</span></span> |<span data-ttu-id="bbeb9-124">Oracle Linux 6.x</span><span class="sxs-lookup"><span data-stu-id="bbeb9-124">Oracle Linux 6.x</span></span> |
> | <span data-ttu-id="bbeb9-125">**Oracle SID**</span><span class="sxs-lookup"><span data-stu-id="bbeb9-125">**Oracle SID**</span></span> |<span data-ttu-id="bbeb9-126">CDB1</span><span class="sxs-lookup"><span data-stu-id="bbeb9-126">CDB1</span></span> |<span data-ttu-id="bbeb9-127">CDB1</span><span class="sxs-lookup"><span data-stu-id="bbeb9-127">CDB1</span></span> |
> | <span data-ttu-id="bbeb9-128">**Replikációs séma**</span><span class="sxs-lookup"><span data-stu-id="bbeb9-128">**Replication schema**</span></span> |<span data-ttu-id="bbeb9-129">TESZT</span><span class="sxs-lookup"><span data-stu-id="bbeb9-129">TEST</span></span>|<span data-ttu-id="bbeb9-130">TESZT</span><span class="sxs-lookup"><span data-stu-id="bbeb9-130">TEST</span></span> |
> | <span data-ttu-id="bbeb9-131">**Tulajdonos vagy replicate Golden kapu**</span><span class="sxs-lookup"><span data-stu-id="bbeb9-131">**Golden Gate owner/replicate**</span></span> |<span data-ttu-id="bbeb9-132">C ##GGADMIN</span><span class="sxs-lookup"><span data-stu-id="bbeb9-132">C##GGADMIN</span></span> |<span data-ttu-id="bbeb9-133">REPUSER</span><span class="sxs-lookup"><span data-stu-id="bbeb9-133">REPUSER</span></span> |
> | <span data-ttu-id="bbeb9-134">**Golden kapu folyamat**</span><span class="sxs-lookup"><span data-stu-id="bbeb9-134">**Golden Gate process**</span></span> |<span data-ttu-id="bbeb9-135">EXTORA</span><span class="sxs-lookup"><span data-stu-id="bbeb9-135">EXTORA</span></span> |<span data-ttu-id="bbeb9-136">REPORA</span><span class="sxs-lookup"><span data-stu-id="bbeb9-136">REPORA</span></span>|


### <a name="sign-in-to-azure"></a><span data-ttu-id="bbeb9-137">Bejelentkezés az Azure-ba</span><span class="sxs-lookup"><span data-stu-id="bbeb9-137">Sign in to Azure</span></span> 

<span data-ttu-id="bbeb9-138">Jelentkezzen be Azure előfizetés a [az bejelentkezési](/cli/azure/#login) parancsot.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-138">Sign in to your Azure subscription with the [az login](/cli/azure/#login) command.</span></span> <span data-ttu-id="bbeb9-139">Ezután kövesse a képernyőn megjelenő utasításokat.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-139">Then follow the on-screen directions.</span></span>

```azurecli
az login
```

### <a name="create-a-resource-group"></a><span data-ttu-id="bbeb9-140">Hozzon létre egy erőforráscsoportot</span><span class="sxs-lookup"><span data-stu-id="bbeb9-140">Create a resource group</span></span>

<span data-ttu-id="bbeb9-141">Hozzon létre egy erőforráscsoportot az [az group create](/cli/azure/group#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-141">Create a resource group with the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="bbeb9-142">Egy Azure erőforráscsoport egy olyan logikai tároló mely Azure-erőforrások vannak telepítve és onnan, ami által kezelhető legyen.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-142">An Azure resource group is a logical container into which Azure resources are deployed and from which they can be managed.</span></span> 

<span data-ttu-id="bbeb9-143">A következő példában létrehozunk egy `westus` nevű erőforráscsoportot a `myResourceGroup` helyen.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-143">The following example creates a resource group named `myResourceGroup` in the `westus` location.</span></span>

```azurecli
az group create --name myResourceGroup --location westus
```

### <a name="create-an-availability-set"></a><span data-ttu-id="bbeb9-144">Rendelkezésre állási csoport létrehozása</span><span class="sxs-lookup"><span data-stu-id="bbeb9-144">Create an availability set</span></span>

<span data-ttu-id="bbeb9-145">A következő lépés az opcionális de javasolt.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-145">The following step is optional but recommended.</span></span> <span data-ttu-id="bbeb9-146">További információkért lásd: [Azure rendelkezésre állási készletek útmutató](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).</span><span class="sxs-lookup"><span data-stu-id="bbeb9-146">For more information, see [Azure availability sets guide](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines).</span></span>

```azurecli
az vm availability-set create \
    --resource-group myResourceGroup \
    --name myAvailabilitySet \
    --platform-fault-domain-count 2 \
    --platform-update-domain-count 2
```

### <a name="create-a-virtual-machine"></a><span data-ttu-id="bbeb9-147">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="bbeb9-147">Create a virtual machine</span></span>

<span data-ttu-id="bbeb9-148">Hozzon létre egy virtuális gépet az [az vm create](/cli/azure/vm#create) paranccsal.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-148">Create a VM with the [az vm create](/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="bbeb9-149">Az alábbi példakód létrehozza nevű két virtuális gép `myVM1` és `myVM2`.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-149">The following example creates two VMs named `myVM1` and `myVM2`.</span></span> <span data-ttu-id="bbeb9-150">SSH-kulcsok létrehozása, ha még nem léteznek a kulcs alapértelmezett helye.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-150">Create SSH keys if they do not already exist in a default key location.</span></span> <span data-ttu-id="bbeb9-151">Ha konkrét kulcsokat szeretné használni, használja az `--ssh-key-value` beállítást.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-151">To use a specific set of keys, use the `--ssh-key-value` option.</span></span>

#### <a name="create-myvm1-primary"></a><span data-ttu-id="bbeb9-152">Hozzon létre myVM1 (elsődleges):</span><span class="sxs-lookup"><span data-stu-id="bbeb9-152">Create myVM1 (primary):</span></span>
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM1 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

<span data-ttu-id="bbeb9-153">A virtuális gép létrehozása után az Azure parancssori felület információit mutatja meg az alábbi példához hasonló.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-153">After the VM has been created, the Azure CLI shows information similar to the following example.</span></span> <span data-ttu-id="bbeb9-154">(Vegye figyelembe a `publicIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-154">(Take note of the `publicIpAddress`.</span></span> <span data-ttu-id="bbeb9-155">Ez a cím eléréséhez használt virtuális gép.)</span><span class="sxs-lookup"><span data-stu-id="bbeb9-155">This address is used to access the VM.)</span></span>

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

#### <a name="create-myvm2-replicate"></a><span data-ttu-id="bbeb9-156">Hozzon létre myVM2 (replikáció):</span><span class="sxs-lookup"><span data-stu-id="bbeb9-156">Create myVM2 (replicate):</span></span>
```azurecli
az vm create \
     --resource-group myResourceGroup \
     --name myVM2 \
     --availability-set myAvailabilitySet \
     --image Oracle:Oracle-Database-Ee:12.1.0.2:latest \
     --size Standard_DS1_v2  \
     --generate-ssh-keys \
```

<span data-ttu-id="bbeb9-157">Vegye figyelembe a `publicIpAddress` , valamint a létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-157">Take note of the `publicIpAddress` as well after it has been created.</span></span>

### <a name="open-the-tcp-port-for-connectivity"></a><span data-ttu-id="bbeb9-158">Nyissa meg a TCP-portot a kapcsolatot</span><span class="sxs-lookup"><span data-stu-id="bbeb9-158">Open the TCP port for connectivity</span></span>

<span data-ttu-id="bbeb9-159">A következő lépés, hogy konfigurálja a külső végpontok száma, amelyek lehetővé teszik, hogy távolról fér hozzá az Oracle-adatbázishoz.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-159">The next step is to configure external endpoints,  which enable you to access the Oracle database remotely.</span></span> <span data-ttu-id="bbeb9-160">A külső végpontok száma konfigurálásához futtassa a következő parancsokat.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-160">To configure the external endpoints, run the following commands.</span></span>

#### <a name="open-the-port-for-myvm1"></a><span data-ttu-id="bbeb9-161">Nyissa meg a myVM1 portja:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-161">Open the port for myVM1:</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm1NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

<span data-ttu-id="bbeb9-162">Az eredményeket a következő válasz hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-162">The results should look similar to the following response:</span></span>

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

#### <a name="open-the-port-for-myvm2"></a><span data-ttu-id="bbeb9-163">Nyissa meg a myVM2 portja:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-163">Open the port for myVM2:</span></span>

```azurecli
az network nsg rule create --resource-group myResourceGroup\
    --nsg-name myVm2NSG --name allow-oracle\
    --protocol tcp --direction inbound --priority 999 \
    --source-address-prefix '*' --source-port-range '*' \
    --destination-address-prefix '*' --destination-port-range 1521 --access allow
```

### <a name="connect-to-the-virtual-machine"></a><span data-ttu-id="bbeb9-164">Csatlakozás a virtuális géphez</span><span class="sxs-lookup"><span data-stu-id="bbeb9-164">Connect to the virtual machine</span></span>

<span data-ttu-id="bbeb9-165">Használja az alábbi parancsot egy SSH-munkamenet létrehozásához a virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-165">Use the following command to create an SSH session with the virtual machine.</span></span> <span data-ttu-id="bbeb9-166">Cserélje le az IP-címet a virtuális gépe `publicIpAddress` címére.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-166">Replace the IP address with the `publicIpAddress` of your virtual machine.</span></span>

```bash 
ssh <publicIpAddress>
```

### <a name="create-the-database-on-myvm1-primary"></a><span data-ttu-id="bbeb9-167">Létrehozza az adatbázist az myVM1 (elsődleges)</span><span class="sxs-lookup"><span data-stu-id="bbeb9-167">Create the database on myVM1 (primary)</span></span>

<span data-ttu-id="bbeb9-168">Az Oracle szoftver már telepítve van a Piactéri lemezképhez, így a következő lépés az adatbázis telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-168">The Oracle software is already installed on the Marketplace image, so the next step is to install the database.</span></span> 

<span data-ttu-id="bbeb9-169">Futtassa a szoftvert az "oracle" felügyelő:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-169">Run the software as the 'oracle' superuser:</span></span>

```bash
sudo su - oracle
```

<span data-ttu-id="bbeb9-170">Az adatbázis létrehozása:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-170">Create the database:</span></span>

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
<span data-ttu-id="bbeb9-171">Kimenetének létrehozása a következő válasz hasonlóan kell kinéznie:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-171">Outputs should look similar to the following response:</span></span>

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
Look at the log file "/u01/app/oracle/cfgtoollogs/dbca/cdb1/cdb1.log" for more details.
```

<span data-ttu-id="bbeb9-172">A ORACLE_SID és ORACLE_HOME változók megadása.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-172">Set the ORACLE_SID and ORACLE_HOME variables.</span></span>

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=gg1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

<span data-ttu-id="bbeb9-173">Szükség esetén adhat hozzá ORACLE_HOME és ORACLE_SID .bashrc fájlt úgy, hogy ezek a beállítások a későbbi bejelentkezések lesznek mentve:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-173">Optionally, you can add ORACLE_HOME and ORACLE_SID to the .bashrc file, so that these settings are saved for future sign-ins:</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=gg1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a><span data-ttu-id="bbeb9-174">Indítsa el az Oracle-figyelő</span><span class="sxs-lookup"><span data-stu-id="bbeb9-174">Start Oracle listener</span></span>
```bash
$ sudo su - oracle
$ lsnrctl start
```

### <a name="create-the-database-on-myvm2-replicate"></a><span data-ttu-id="bbeb9-175">Az adatbázis létrehozása a következőn myVM2 (replikáció)</span><span class="sxs-lookup"><span data-stu-id="bbeb9-175">Create the database on myVM2 (replicate)</span></span>

```bash
sudo su - oracle
```
<span data-ttu-id="bbeb9-176">Az adatbázis létrehozása:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-176">Create the database:</span></span>

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
<span data-ttu-id="bbeb9-177">A ORACLE_SID és ORACLE_HOME változók megadása.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-177">Set the ORACLE_SID and ORACLE_HOME variables.</span></span>

```bash
$ ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1; export ORACLE_HOME
$ ORACLE_SID=cdb1; export ORACLE_SID
$ LD_LIBRARY_PATH=ORACLE_HOME/lib; export LD_LIBRARY_PATH
```

<span data-ttu-id="bbeb9-178">Szükség esetén is hozzáadott ORACLE_HOME és ORACLE_SID .bashrc fájlt úgy, hogy ezek a beállítások lesznek mentve a későbbi bejelentkezések.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-178">Optionally, you can added ORACLE_HOME and ORACLE_SID to the .bashrc file, so that these settings are saved for future sign-ins.</span></span>

```bash
# add oracle home
export ORACLE_HOME=/u01/app/oracle/product/12.1.0/dbhome_1
# add oracle sid
export ORACLE_SID=cdb1
# add Oracle library path
export LD_LIBRARY_PATH=$ORACLE_HOME/lib
```

### <a name="start-oracle-listener"></a><span data-ttu-id="bbeb9-179">Indítsa el az Oracle-figyelő</span><span class="sxs-lookup"><span data-stu-id="bbeb9-179">Start Oracle listener</span></span>
```bash
$ sudo su - oracle
$ lsnrctl start
```

## <a name="configure-golden-gate"></a><span data-ttu-id="bbeb9-180">Golden kapu beállítása</span><span class="sxs-lookup"><span data-stu-id="bbeb9-180">Configure Golden Gate</span></span> 
<span data-ttu-id="bbeb9-181">Golden kapu konfigurálásához lépéseket az ebben a szakaszban.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-181">To configure Golden Gate, take the steps in this section.</span></span>

### <a name="enable-archive-log-mode-on-myvm1-primary"></a><span data-ttu-id="bbeb9-182">Archív napló mód a myVM1 (elsődleges) engedélyezése</span><span class="sxs-lookup"><span data-stu-id="bbeb9-182">Enable archive log mode on myVM1 (primary)</span></span>

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
<span data-ttu-id="bbeb9-183">Kényszerített naplózását, és ellenőrizze, hogy legalább egy naplófájlt.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-183">Enable force logging, and make sure at least one log file is present.</span></span>

```bash
SQL> ALTER DATABASE FORCE LOGGING;
SQL> ALTER SYSTEM SWITCH LOGFILE;
SQL> ALTER SYSTEM set enable_goldengate_replication=true;
SQL> ALTER PLUGGABLE DATABASE PDB1 OPEN;
SQL> ALTER SESSION SET CONTAINER=PDB1;
SQL> ALTER DATABASE ADD SUPPLEMENTAL LOG DATA;
SQL> EXIT;
```

### <a name="download-golden-gate-software"></a><span data-ttu-id="bbeb9-184">Golden kapu szoftver letöltése</span><span class="sxs-lookup"><span data-stu-id="bbeb9-184">Download Golden Gate software</span></span>
<span data-ttu-id="bbeb9-185">Töltse le, és az Oracle Golden kapu szoftver előkészítése, végezze el az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-185">To download and prepare the Oracle Golden Gate software, complete the following steps:</span></span>

1. <span data-ttu-id="bbeb9-186">Töltse le a **fbo_ggs_Linux_x64_shiphome.zip** fájlt a [Oracle Golden kapu letöltési oldala](http://www.oracle.com/technetwork/middleware/goldengate/downloads/index.html).</span><span class="sxs-lookup"><span data-stu-id="bbeb9-186">Download the **fbo_ggs_Linux_x64_shiphome.zip** file from the [Oracle Golden Gate download page](http://www.oracle.com/technetwork/middleware/goldengate/downloads/index.html).</span></span> <span data-ttu-id="bbeb9-187">A letöltési címben **Oracle GoldenGate 12.x.x.x az Oracle Linux x86-64**, kell lennie a .zip fájlokat letölteni.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-187">Under the download title **Oracle GoldenGate 12.x.x.x for Oracle Linux x86-64**, there should be a set of .zip files to download.</span></span>

2. <span data-ttu-id="bbeb9-188">Miután letöltötte a .zip fájlokat az ügyfélszámítógépre, használja a biztonságos másolási protokoll (SCP) a fájlok másolása a virtuális Gépet:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-188">After you download the .zip files to your client computer, use Secure Copy Protocol (SCP) to copy the files to your VM:</span></span>

  ```bash
  $ scp fbo_ggs_Linux_x64_shiphome.zip <publicIpAddress>:<folder>
  ```

3. <span data-ttu-id="bbeb9-189">Helyezze át a .zip fájlokat a **/ opt** mappát.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-189">Move the .zip files to the **/opt** folder.</span></span> <span data-ttu-id="bbeb9-190">Az alábbiak szerint módosítsa a fájlok tulajdonosa:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-190">Then change the owner of the files as follows:</span></span>

  ```bash
  $ sudo su -
  # mv <folder>/*.zip /opt
  ```

4. <span data-ttu-id="bbeb9-191">Bontsa ki a fájlokat (telepítés a Linux csomagolja ki segédprogram Ha még nincs telepítve van):</span><span class="sxs-lookup"><span data-stu-id="bbeb9-191">Unzip the files (install the Linux unzip utility if it's not already installed):</span></span>

  ```bash
  # yum install unzip
  # cd /opt
  # unzip fbo_ggs_Linux_x64_shiphome.zip
  ```

5. <span data-ttu-id="bbeb9-192">Engedély módosítása:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-192">Change permission:</span></span>

  ```bash
  # chown -R oracle:oinstall /opt/fbo_ggs_Linux_x64_shiphome
  ```

### <a name="prepare-the-client-and-vm-to-run-x11-for-windows-clients-only"></a><span data-ttu-id="bbeb9-193">Az ügyfél és a virtuális gép számára való futás x11 (csak Windows-ügyfelek) előkészítése</span><span class="sxs-lookup"><span data-stu-id="bbeb9-193">Prepare the client and VM to run x11 (for Windows clients only)</span></span>
<span data-ttu-id="bbeb9-194">Ez az egy opcionális lépés.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-194">This is an optional step.</span></span> <span data-ttu-id="bbeb9-195">Ezt a lépést kihagyhatja, ha a Linux ügyfelet kell használnia, vagy már rendelkezik x11 beállítása.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-195">You can skip this step if you are using a Linux client or already have x11 setup.</span></span>

1. <span data-ttu-id="bbeb9-196">Töltse le a PuTTY és Xming a Windows rendszerű számítógépen:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-196">Download PuTTY and Xming to your Windows computer:</span></span>

  * [<span data-ttu-id="bbeb9-197">Töltse le a PuTTY</span><span class="sxs-lookup"><span data-stu-id="bbeb9-197">Download PuTTY</span></span>](http://www.putty.org/)
  * [<span data-ttu-id="bbeb9-198">Töltse le a Xming</span><span class="sxs-lookup"><span data-stu-id="bbeb9-198">Download Xming</span></span>](https://xming.en.softonic.com/)

2.  <span data-ttu-id="bbeb9-199">Miután telepítette a PuTTY, a PuTTY mappában (például, C:\Program Files\PuTTY), futtassa a puttygen.exe (PuTTY kulcs generátor).</span><span class="sxs-lookup"><span data-stu-id="bbeb9-199">After you install PuTTY, in the PuTTY folder (for example, C:\Program Files\PuTTY), run puttygen.exe (PuTTY Key Generator).</span></span>

3.  <span data-ttu-id="bbeb9-200">A PuTTY Megosztottelérésikulcs-készítő:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-200">In PuTTY Key Generator:</span></span>

  - <span data-ttu-id="bbeb9-201">A kulcs létrehozásához válassza ki a **Generate** gombra.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-201">To generate a key, select the **Generate** button.</span></span>
  - <span data-ttu-id="bbeb9-202">Másolja a kulcs tartalmát (**Ctrl + C**).</span><span class="sxs-lookup"><span data-stu-id="bbeb9-202">Copy the contents of the key (**Ctrl+C**).</span></span>
  - <span data-ttu-id="bbeb9-203">Válassza ki a **mentés titkos kulcs** gombra.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-203">Select the **Save private key** button.</span></span>
  - <span data-ttu-id="bbeb9-204">Figyelmen kívül hagyhatja a figyelmeztetést, amely akkor jelenik meg, majd válassza ki **OK**.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-204">Ignore the warning that appears, and then select **OK**.</span></span>

    ![A PuTTY megosztottelérésikulcs-készítő oldalát bemutató képernyőkép](./media/oracle-golden-gate/puttykeygen.png)

4.  <span data-ttu-id="bbeb9-206">A virtuális gépen, futtassa az alábbi parancsokat:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-206">In your VM, run these commands:</span></span>

  ```bash
  # sudo su - oracle
  $ mkdir .ssh (if not already created)
  $ cd .ssh
  ```

5. <span data-ttu-id="bbeb9-207">Hozzon létre egy fájlt **authorized_keys**.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-207">Create a file named **authorized_keys**.</span></span> <span data-ttu-id="bbeb9-208">A kulcs tartalmának beillesztése a fájlban, és mentse a fájlt.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-208">Paste the contents of the key in this file, and then save the file.</span></span>

  > [!NOTE]
  > <span data-ttu-id="bbeb9-209">A kulcs hossza csak a karakterlánc `ssh-rsa`.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-209">The key must contain the string `ssh-rsa`.</span></span> <span data-ttu-id="bbeb9-210">A kulcs tartalmát is, egy egyszerű szövegsor kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-210">Also, the contents of the key must be a single line of text.</span></span>
  >  

6. <span data-ttu-id="bbeb9-211">Indítsa el a PuTTY alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-211">Start PuTTY.</span></span> <span data-ttu-id="bbeb9-212">Az a **kategória** ablaktáblán válassza előbb **kapcsolat** > **SSH** > **Auth**.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-212">In the **Category** pane, select **Connection** > **SSH** > **Auth**.</span></span> <span data-ttu-id="bbeb9-213">Az a **hitelesítéshez titkos kulcsfájl** mezőben tallózással keresse meg a korábban létrehozott kulcsot.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-213">In the **Private key file for authentication** box, browse to the key that you generated earlier.</span></span>

  ![A titkos kulcs beállítása oldalát bemutató képernyőkép](./media/oracle-golden-gate/setprivatekey.png)

7. <span data-ttu-id="bbeb9-215">Az a **kategória** ablaktáblán válassza előbb **kapcsolat** > **SSH** > **X11**.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-215">In the **Category** pane, select **Connection** > **SSH** > **X11**.</span></span> <span data-ttu-id="bbeb9-216">Válassza ki a **engedélyezése X11 továbbítási** mezőbe.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-216">Then select the **Enable X11 forwarding** box.</span></span>

  ![Az engedélyezés X11 oldalát bemutató képernyőkép](./media/oracle-golden-gate/enablex11.png)

8. <span data-ttu-id="bbeb9-218">Az a **kategória** ablaktáblában lépjen **munkamenet**.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-218">In the **Category** pane, go to **Session**.</span></span> <span data-ttu-id="bbeb9-219">Adja meg a gazdagép adatokat, majd válassza ki **nyitott**.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-219">Enter the host information, and then select **Open**.</span></span>

  ![A munkamenet oldalát bemutató képernyőkép](./media/oracle-golden-gate/puttysession.png)

### <a name="install-golden-gate-software"></a><span data-ttu-id="bbeb9-221">Golden kapu szoftver telepítése</span><span class="sxs-lookup"><span data-stu-id="bbeb9-221">Install Golden Gate software</span></span>

<span data-ttu-id="bbeb9-222">Oracle Golden kapu telepítéséhez kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-222">To install Oracle Golden Gate, complete the following steps:</span></span>

1. <span data-ttu-id="bbeb9-223">Jelentkezzen be a oracle felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-223">Sign in as oracle.</span></span> <span data-ttu-id="bbeb9-224">(Kell használva jelentkezhet be a jelszó megadása nélkül.) Győződjön meg arról, hogy fut-e Xming, a telepítés megkezdése előtt.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-224">(You should be able to sign in without being prompted for a password.) Make sure that Xming is running before you begin the installation.</span></span>
 
  ```bash
  $ cd /opt/fbo_ggs_Linux_x64_shiphome/Disk1
  $ ./runInstaller
  ```
2. <span data-ttu-id="bbeb9-225">Válassza ki az "Oracle adatbázis 12c Oracle GoldenGate".</span><span class="sxs-lookup"><span data-stu-id="bbeb9-225">Select 'Oracle GoldenGate for Oracle Database 12c'.</span></span> <span data-ttu-id="bbeb9-226">Válassza ki **tovább** a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-226">Then select **Next** to continue.</span></span>

  ![A telepítő telepítési válasszon oldalát bemutató képernyőkép](./media/oracle-golden-gate/golden_gate_install_01.png)

3. <span data-ttu-id="bbeb9-228">Módosíthatja a szoftver helyét.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-228">Change the software location.</span></span> <span data-ttu-id="bbeb9-229">Válassza ki a **Start Manager** mezőbe, majd adja meg az adatbázis helyén.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-229">Then select  the **Start Manager** box and enter the database location.</span></span> <span data-ttu-id="bbeb9-230">Válassza ki **tovább** a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-230">Select **Next** to continue.</span></span>

  ![A telepítés válasszon oldalát bemutató képernyőkép](./media/oracle-golden-gate/golden_gate_install_02.png)

4. <span data-ttu-id="bbeb9-232">Módosítsa a készlet könyvtárat, majd válassza ki **tovább** a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-232">Change the inventory directory, and then select **Next** to continue.</span></span>

  ![A telepítés válasszon oldalát bemutató képernyőkép](./media/oracle-golden-gate/golden_gate_install_03.png)

5. <span data-ttu-id="bbeb9-234">Az a **összegzés** képernyőn válassza ki **telepítése** folytatja.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-234">On the **Summary** screen, select **Install** to continue.</span></span>

  ![A telepítő telepítési válasszon oldalát bemutató képernyőkép](./media/oracle-golden-gate/golden_gate_install_04.png)

6. <span data-ttu-id="bbeb9-236">"Legfelső szintű" parancsfájllal kérheti.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-236">You might be prompted to run a script as 'root'.</span></span> <span data-ttu-id="bbeb9-237">Ha igen, nyisson meg egy külön munkamenet ssh a virtuális gépre, a sudo legfelső szintű, és futtassa a parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-237">If so, open a separate session, ssh to the VM, sudo to root, and then run the script.</span></span> <span data-ttu-id="bbeb9-238">Válassza ki **OK** továbbra is.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-238">Select **OK** continue.</span></span>

  ![A telepítés válasszon oldalát bemutató képernyőkép](./media/oracle-golden-gate/golden_gate_install_05.png)

7. <span data-ttu-id="bbeb9-240">Ha a telepítés befejeződött, válassza ki a **Bezárás** a folyamat befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-240">When the installation has finished, select **Close** to complete the process.</span></span>

  ![A telepítés válasszon oldalát bemutató képernyőkép](./media/oracle-golden-gate/golden_gate_install_06.png)

### <a name="set-up-service-on-myvm1-primary"></a><span data-ttu-id="bbeb9-242">Állítsa be a szolgáltatást a myVM1 (elsődleges)</span><span class="sxs-lookup"><span data-stu-id="bbeb9-242">Set up service on myVM1 (primary)</span></span>

1. <span data-ttu-id="bbeb9-243">Hozzon létre, vagy frissítse a tnsnames.ora fájlt:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-243">Create or update the tnsnames.ora file:</span></span>

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

2. <span data-ttu-id="bbeb9-244">A Golden kapu tulajdonosi és felhasználói fiókokat hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-244">Create the Golden Gate owner and user accounts.</span></span>

  > [!NOTE]
  > <span data-ttu-id="bbeb9-245">A tulajdonos fióknak C ## előtaggal kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-245">The owner account must have C## prefix.</span></span>
  >

    ```bash
    $ sqlplus / as sysdba
    SQL> CREATE USER C##GGADMIN identified by ggadmin;
    SQL> EXEC dbms_goldengate_auth.grant_admin_privilege('C##GGADMIN',container=>'ALL');
    SQL> GRANT DBA to C##GGADMIN container=all;
    SQL> connect C##GGADMIN/ggadmin
    SQL> ALTER SESSION SET CONTAINER=PDB1;
    SQL> EXIT;
    ```

3. <span data-ttu-id="bbeb9-246">Hozza létre a Golden kapu tesztfelhasználói fiókja:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-246">Create the Golden Gate test user account:</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ sqlplus system/OraPasswd1@pdb1
  SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
  SQL> GRANT connect, resource, dba TO test;
  SQL> ALTER USER test QUOTA 100M on USERS;
  SQL> connect test/test@pdb1
  SQL> @demo_ora_create
  SQL> @demo_ora_insert
  SQL> EXIT;
  ```

4. <span data-ttu-id="bbeb9-247">A kivonat paraméterfájl konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-247">Configure the extract parameter file.</span></span>

 <span data-ttu-id="bbeb9-248">Indítsa el az arany kapu parancssori felület (ggsci):</span><span class="sxs-lookup"><span data-stu-id="bbeb9-248">Start the Golden gate command-line interface (ggsci):</span></span>

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
5. <span data-ttu-id="bbeb9-249">Adja hozzá a következőket a KIVONATOT paraméterfájl (vi parancsok segítségével).</span><span class="sxs-lookup"><span data-stu-id="bbeb9-249">Add the following to the EXTRACT parameter file (by using vi commands).</span></span> <span data-ttu-id="bbeb9-250">Nyomja meg az Esc billentyűt, ': wq! "</span><span class="sxs-lookup"><span data-stu-id="bbeb9-250">Press Esc key, ':wq!'</span></span> <span data-ttu-id="bbeb9-251">sikerült menteni a fájlt.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-251">to save file.</span></span> 

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
6. <span data-ttu-id="bbeb9-252">Regisztráció nyerje ki – integrált kivonat:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-252">Register extract--integrated extract:</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci

  GGSCI> dblogin userid C##GGADMIN, password ggadmin
  Successfully logged into database CDB$ROOT.

  GGSCI> REGISTER EXTRACT EXTORA DATABASE CONTAINER(pdb1)

  2017-05-23 15:58:34  INFO    OGG-02003  Extract EXTORA successfully registered with database at SCN 1821260.

  GGSCI> exit
  ```
7. <span data-ttu-id="bbeb9-253">Kinyerési ellenőrzőpontok felállítása, és indítsa el a valós idejű kivonat:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-253">Set up extract checkpoints and start real-time extract:</span></span>

  ```bash
  $ ./ggsci
  GGSCI>  ADD EXTRACT EXTORA, INTEGRATED TRANLOG, BEGIN NOW
  EXTRACT (Integrated) added.

  GGSCI>  ADD RMTTRAIL ./dirdat/rt, EXTRACT EXTORA, MEGABYTES 10
  RMTTRAIL added.

  GGSCI>  START EXTRACT EXTORA

  Sending START request to MANAGER ...
  EXTRACT EXTORA starting

  GGSCI > info all

  Program     Status      Group       Lag at Chkpt  Time Since Chkpt

  MANAGER     RUNNING
  EXTRACT     RUNNING     EXTORA      00:00:11      00:00:04
  ```
<span data-ttu-id="bbeb9-254">Ebben a lépésben a kiindulási Állapotváltozás, később, a másik szakaszban használt keresése:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-254">In this step, you find the starting SCN, which will be used later, in a different section:</span></span>

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

### <a name="set-up-service-on-myvm2-replicate"></a><span data-ttu-id="bbeb9-255">A myVM2 szolgáltatás beállítása (replikáció)</span><span class="sxs-lookup"><span data-stu-id="bbeb9-255">Set up service on myVM2 (replicate)</span></span>


1. <span data-ttu-id="bbeb9-256">Hozzon létre, vagy frissítse a tnsnames.ora fájlt:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-256">Create or update the tnsnames.ora file:</span></span>

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

2. <span data-ttu-id="bbeb9-257">Hozzon létre egy párhuzamos fiókot:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-257">Create a replicate account:</span></span>

  ```bash
  $ sqlplus / as sysdba
  SQL> alter session set container = pdb1;
  SQL> create user repuser identified by rep_pass container=current;
  SQL> grant dba to repuser;
  SQL> exec dbms_goldengate_auth.grant_admin_privilege('REPUSER',container=>'PDB1');
  SQL> connect repuser/rep_pass@pdb1 
  SQL> EXIT;
  ```

3. <span data-ttu-id="bbeb9-258">Golden kapu teszt felhasználói fiók létrehozása:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-258">Create a Golden Gate test user account:</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ sqlplus system/OraPasswd1@pdb1
  SQL> CREATE USER test identified by test DEFAULT TABLESPACE USERS TEMPORARY TABLESPACE TEMP;
  SQL> GRANT connect, resource, dba TO test;
  SQL> ALTER USER test QUOTA 100M on USERS;
  SQL> connect test/test@pdb1
  SQL> @demo_ora_create
  SQL> EXIT;
  ```

4. <span data-ttu-id="bbeb9-259">A paraméterfájl REPLICAT replikálni a módosításokat:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-259">REPLICAT parameter file to replicate changes:</span></span> 

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS REPORA  
  ```
  <span data-ttu-id="bbeb9-260">REPORA paraméter fájl tartalma:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-260">Content of REPORA parameter file:</span></span>

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

5. <span data-ttu-id="bbeb9-261">Replicat ellenőrzőpont beállítása:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-261">Set up a replicat checkpoint:</span></span>

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

### <a name="set-up-the-replication-myvm1-and-myvm2"></a><span data-ttu-id="bbeb9-262">A replikáció (myVM1 és myVM2) beállítása</span><span class="sxs-lookup"><span data-stu-id="bbeb9-262">Set up the replication (myVM1 and myVM2)</span></span>

#### <a name="1-set-up-the-replication-on-myvm2-replicate"></a><span data-ttu-id="bbeb9-263">1. A replikáció myVM2 beállítása (replikáció)</span><span class="sxs-lookup"><span data-stu-id="bbeb9-263">1. Set up the replication on myVM2 (replicate)</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  GGSCI> EDIT PARAMS MGR
  ```
<span data-ttu-id="bbeb9-264">Frissítse a fájlt a következő:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-264">Update the file with the following:</span></span>

  ```bash
  PORT 7809
  ACCESSRULE, PROG *, IPADDR *, ALLOW
  ```
<span data-ttu-id="bbeb9-265">Indítsa újra a kezelő szolgáltatás:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-265">Then restart the Manager service:</span></span>

  ```bash
  GGSCI> STOP MGR
  GGSCI> START MGR
  GGSCI> EXIT
  ```

#### <a name="2-set-up-the-replication-on-myvm1-primary"></a><span data-ttu-id="bbeb9-266">2. Állítsa be a replikáció myVM1 (elsődleges)</span><span class="sxs-lookup"><span data-stu-id="bbeb9-266">2. Set up the replication on myVM1 (primary)</span></span>

<span data-ttu-id="bbeb9-267">Indítsa el a kezdeti betöltés és a hibák:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-267">Start the initial load and check for errors:</span></span>

```bash
$ cd /u01/app/oracle/product/12.1.0/oggcore_1
$ ./ggsci
GGSCI> START EXTRACT INITEXT
GGSCI> VIEW REPORT INITEXT
```
#### <a name="3-set-up-the-replication-on-myvm2-replicate"></a><span data-ttu-id="bbeb9-268">3. A replikáció myVM2 beállítása (replikáció)</span><span class="sxs-lookup"><span data-stu-id="bbeb9-268">3. Set up the replication on myVM2 (replicate)</span></span>

<span data-ttu-id="bbeb9-269">A módosítás előtt beszerzett számot száma az Állapotváltozás:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-269">Change the SCN number with the number you obtained before:</span></span>

  ```bash
  $ cd /u01/app/oracle/product/12.1.0/oggcore_1
  $ ./ggsci
  START REPLICAT REPORA, AFTERCSN 1857887
  ```
<span data-ttu-id="bbeb9-270">A replikáció már megkezdődött, és új rekordok teszt táblák beszúrásával tesztelheti.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-270">The replication has begun, and you can test it by inserting new records to TEST tables.</span></span>


### <a name="view-job-status-and-troubleshooting"></a><span data-ttu-id="bbeb9-271">Feladat állapotának megtekintése és hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="bbeb9-271">View job status and troubleshooting</span></span>

#### <a name="view-reports"></a><span data-ttu-id="bbeb9-272">Jelentések megtekintése</span><span class="sxs-lookup"><span data-stu-id="bbeb9-272">View reports</span></span>
<span data-ttu-id="bbeb9-273">MyVM1 jelentések megtekintéséhez futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-273">To view reports on myVM1, run the following commands:</span></span>

  ```bash
  GGSCI> VIEW REPORT EXTORA 
  ```
 
<span data-ttu-id="bbeb9-274">MyVM2 jelentések megtekintéséhez futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-274">To view reports on myVM2, run the following commands:</span></span>

  ```bash
  GGSCI> VIEW REPORT REPORA
  ```

#### <a name="view-status-and-history"></a><span data-ttu-id="bbeb9-275">Nézet állapota és előzményei</span><span class="sxs-lookup"><span data-stu-id="bbeb9-275">View status and history</span></span>
<span data-ttu-id="bbeb9-276">A myVM1 állapota és előzményei megtekintéséhez futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-276">To view status and history on myVM1, run the following commands:</span></span>

  ```bash
  GGSCI> dblogin userid c##ggadmin, password ggadmin 
  GGSCI> INFO EXTRACT EXTORA, DETAIL
  ```

<span data-ttu-id="bbeb9-277">A myVM2 állapota és előzményei megtekintéséhez futtassa a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="bbeb9-277">To view status and history on myVM2, run the following commands:</span></span>

  ```bash
  GGSCI> dblogin userid repuser@pdb1 password rep_pass 
  GGSCI> INFO REP REPORA, DETAIL
  ```
<span data-ttu-id="bbeb9-278">Ezzel befejezte a telepítési és konfigurációs Golden kapu Oracle Linux.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-278">This completes the installation and configuration of Golden Gate on Oracle linux.</span></span>


## <a name="delete-the-virtual-machine"></a><span data-ttu-id="bbeb9-279">A virtuális gép törlése</span><span class="sxs-lookup"><span data-stu-id="bbeb9-279">Delete the virtual machine</span></span>

<span data-ttu-id="bbeb9-280">Ha már nincs szükség, az alábbi parancs segítségével távolítsa el az erőforráscsoportot, virtuális gép és az összes kapcsolódó erőforrások.</span><span class="sxs-lookup"><span data-stu-id="bbeb9-280">When it's no longer needed, the following command can be used to remove the resource group, VM, and all related resources.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="bbeb9-281">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bbeb9-281">Next steps</span></span>

[<span data-ttu-id="bbeb9-282">Magas rendelkezésre állású virtuális gép létrehozása oktatóanyag</span><span class="sxs-lookup"><span data-stu-id="bbeb9-282">Create highly available virtual machines tutorial</span></span>](../../linux/create-cli-complete.md)

[<span data-ttu-id="bbeb9-283">A virtuális gépek parancssori felületen való üzembe helyezését ismertető minták megismerése</span><span class="sxs-lookup"><span data-stu-id="bbeb9-283">Explore VM deployment CLI samples</span></span>](../../linux/cli-samples.md)
