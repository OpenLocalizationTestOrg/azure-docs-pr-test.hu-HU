---
title: "Az Azure Linux virtuális gép lemezeinek titkosítása |} Microsoft Docs"
description: "A fokozott biztonság, az Azure CLI 2.0 használatával Linux virtuális gép virtuális lemezein titkosítása"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 2a23b6fa-6941-4998-9804-8efe93b647b3
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/05/2017
ms.author: iainfou
ms.openlocfilehash: 172b4c8f5c098d776cb689543f5d8f163b8895b4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-encrypt-virtual-disks-on-a-linux-vm"></a><span data-ttu-id="5f0f4-103">Virtuális lemezek, a Linux virtuális gép titkosítása</span><span class="sxs-lookup"><span data-stu-id="5f0f4-103">How to encrypt virtual disks on a Linux VM</span></span>
<span data-ttu-id="5f0f4-104">A bővített virtuális gép (VM) biztonsági és megfelelőségi az Azure-ban virtuális lemezek titkosíthatók.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-104">For enhanced virtual machine (VM) security and compliance, virtual disks in Azure can be encrypted.</span></span> <span data-ttu-id="5f0f4-105">Lemezek titkosítása egy Azure Key Vault a titkosított titkosítási kulcsok használatával.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-105">Disks are encrypted using cryptographic keys that are secured in an Azure Key Vault.</span></span> <span data-ttu-id="5f0f4-106">Szabályozhatja a titkosítási kulcsokat, és a használatukat naplózhatók.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-106">You control these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="5f0f4-107">Ez a cikk részletesen titkosítása a Linux virtuális gépet az Azure CLI 2.0 virtuális lemezzel.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-107">This article details how to encrypt virtual disks on a Linux VM using the Azure CLI 2.0.</span></span> <span data-ttu-id="5f0f4-108">Az [Azure CLI 1.0-s](encrypt-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) verziójával is elvégezheti ezeket a lépéseket.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-108">You can also perform these steps with the [Azure CLI 1.0](encrypt-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="5f0f4-109">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="5f0f4-109">Quick commands</span></span>
<span data-ttu-id="5f0f4-110">Ha szeretné gyorsan a feladatnak a, a következő szakasz részleteit a következő parancsokat a virtuális Gépen lévő virtuális lemezek titkosításához.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-110">If you need to quickly accomplish the task, the following section details the base commands to encrypt virtual disks on your VM.</span></span> <span data-ttu-id="5f0f4-111">Részletes információkat és a környezetben az egyes lépések a dokumentum többi részén található [itt indítása](#overview-of-disk-encryption).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-111">More detailed information and context for each step can be found the rest of the document, [starting here](#overview-of-disk-encryption).</span></span>

<span data-ttu-id="5f0f4-112">A legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett az Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-112">You need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="5f0f4-113">A következő példákban cserélje le a saját értékeit példa paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-113">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="5f0f4-114">Példa paraméter nevek a következők *myResourceGroup*, *SajátKulcs*, és *myVM*.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-114">Example parameter names include *myResourceGroup*, *myKey*, and *myVM*.</span></span>

<span data-ttu-id="5f0f4-115">Először, engedélyezése az Azure Key Vault-szolgáltató az Azure-előfizetése belül [az szolgáltató regisztrálása](/cli/azure/provider#register) és hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-115">First, enable the Azure Key Vault provider within your Azure subscription with [az provider register](/cli/azure/provider#register) and create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="5f0f4-116">Az alábbi példa létrehoz egy erőforráscsoport neve *myResourceGroup* a a *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-116">The following example creates a resource group name *myResourceGroup* in the *eastus* location:</span></span>

```azurecli
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="5f0f4-117">Hozzon létre egy Azure Key Vault a [az keyvault létrehozása](/cli/azure/keyvault#create) , és engedélyezze a Key Vault lemeztitkosítás való használatra.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-117">Create an Azure Key Vault with [az keyvault create](/cli/azure/keyvault#create) and enable the Key Vault for use with disk encryption.</span></span> <span data-ttu-id="5f0f4-118">Adjon meg egy egyedi kulcstároló nevet *keyvault_name* az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-118">Specify a unique Key Vault name for *keyvault_name* as follows:</span></span>

```azurecli
keyvault_name=mykeyvaultikf
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

<span data-ttu-id="5f0f4-119">A kulcstároló, a titkosítási kulcs létrehozására [az keyvault kulcs létrehozása](/cli/azure/keyvault/key#create).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-119">Create a cryptographic key in your Key Vault with [az keyvault key create](/cli/azure/keyvault/key#create).</span></span> <span data-ttu-id="5f0f4-120">Az alábbi példakód létrehozza nevű kulcs *SajátKulcs*:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-120">The following example creates a key named *myKey*:</span></span>

```azurecli
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```

<span data-ttu-id="5f0f4-121">Az Azure Active Directoryval az egyszerű szolgáltatás létrehozása [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-121">Create a service principal using Azure Active Directory with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac).</span></span> <span data-ttu-id="5f0f4-122">A szolgáltatás egyszerű hitelesítési és titkosítási kulcsok a Key Vault exchange kezeli.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-122">The service principal handles the authentication and exchange of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="5f0f4-123">Az alábbi példa beolvassa az értékek a szolgáltatás egyszerű azonosító és jelszó használható újabb parancsokat:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-123">The following example reads in the values for the service principal Id and password for use in later commands:</span></span>

```azurecli
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

<span data-ttu-id="5f0f4-124">A jelszó csak kimeneti, az egyszerű szolgáltatás létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-124">The password is only output when you create the service principal.</span></span> <span data-ttu-id="5f0f4-125">Szükség esetén megtekintheti, és jegyezze fel a jelszót (`echo $sp_password`).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-125">If desired, view and record the password (`echo $sp_password`).</span></span> <span data-ttu-id="5f0f4-126">A szolgáltatás elsődleges listázhatja [az sp hirdetéslista](/cli/azure/ad/sp#list) és egyszerű, az adott szolgáltatás további információkat tekinthet [az ad sp megjelenítése](/cli/azure/ad/sp#show).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-126">You can list your service principals with [az ad sp list](/cli/azure/ad/sp#list) and view additional information about a specific service principal with [az ad sp show](/cli/azure/ad/sp#show).</span></span>

<span data-ttu-id="5f0f4-127">A kulcstároló, az engedélyeket [az keyvault-szabály beállítása](/cli/azure/keyvault#set-policy).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-127">Set permissions on your Key Vault with [az keyvault set-policy](/cli/azure/keyvault#set-policy).</span></span> <span data-ttu-id="5f0f4-128">A következő példában a résztvevő-azonosító az előző parancs van megadva:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-128">In the following example, the service principal ID is supplied from the preceding command:</span></span>

```azurecli
az keyvault set-policy --name $keyvault_name --spn $sp_id \
    --key-permissions wrapKey \
    --secret-permissions set
```

<span data-ttu-id="5f0f4-129">A virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) és 5 Gb adatlemezt csatolni.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-129">Create a VM with [az vm create](/cli/azure/vm#create) and attach a 5Gb data disk.</span></span> <span data-ttu-id="5f0f4-130">Csak bizonyos piactéren elérhető rendszerkép lemeztitkosítás támogatja.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-130">Only certain marketplace images support disk encryption.</span></span> <span data-ttu-id="5f0f4-131">Az alábbi példakód létrehozza a virtuális gépek nevű `myVM` használatával egy **CentOS 7.2n** lemezképet:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-131">The following example creates a VM named `myVM` using a **CentOS 7.2n** image:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image OpenLogic:CentOS:7.2n:7.2.20160629 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

<span data-ttu-id="5f0f4-132">SSH-kapcsolatot a virtuális gép használata a `publicIpAddress` látható a fenti parancs kimenetét.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-132">SSH to your VM using the `publicIpAddress` shown in the output of the preceding command.</span></span> <span data-ttu-id="5f0f4-133">Hozzon létre egy partíciót és filesystem, majd csatlakoztassa az adatok lemezt.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-133">Create a partition and filesystem, then mount the data disk.</span></span> <span data-ttu-id="5f0f4-134">További információkért lásd: [csatlakozás egy Linux virtuális Gépet az új lemez csatlakoztatásához](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-134">For more information, see [Connect to a Linux VM to mount the new disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span></span> <span data-ttu-id="5f0f4-135">Zárja be az SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-135">Close your SSH session.</span></span>

<span data-ttu-id="5f0f4-136">Az a virtuális gép titkosítása [az vm-titkosítás engedélyezése](/cli/azure/vm/encryption#enable).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-136">Encrypt your VM with [az vm encryption enable](/cli/azure/vm/encryption#enable).</span></span> <span data-ttu-id="5f0f4-137">Az alábbi példában a `$sp_id` és `$sp_password` változókat az előző `ad sp create-for-rbac` parancs:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-137">The following example uses the `$sp_id` and `$sp_password` variables from the preceding `ad sp create-for-rbac` command:</span></span>

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

<span data-ttu-id="5f0f4-138">A titkosítási folyamat befejezéséhez sok időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-138">It takes some time for the disk encryption process to complete.</span></span> <span data-ttu-id="5f0f4-139">A folyamat állapotának figyelése [az vm titkosítási megjelenítése](/cli/azure/vm/encryption#show):</span><span class="sxs-lookup"><span data-stu-id="5f0f4-139">Monitor the status of the process with [az vm encryption show](/cli/azure/vm/encryption#show):</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="5f0f4-140">Az állapot látható **EncryptionInProgress**.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-140">The status shows **EncryptionInProgress**.</span></span> <span data-ttu-id="5f0f4-141">Várjon, amíg az operációs rendszer állapota lemez jelentések **VMRestartPending**, majd indítsa újra a virtuális Gépet a [az virtuális gép újraindítása](/cli/azure/vm#restart):</span><span class="sxs-lookup"><span data-stu-id="5f0f4-141">Wait until the status for the OS disk reports **VMRestartPending**, then restart your VM with [az vm restart](/cli/azure/vm#restart):</span></span>

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="5f0f4-142">A titkosítási folyamat akkor fejeződik be, a rendszerindítási folyamat során, így Várjon néhány percet, majd újra titkosítási állapotának ellenőrzésekor **az vm titkosítási megjelenítése**:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-142">The disk encryption process is finalized during the boot process, so wait a few minutes before checking the status of encryption again with **az vm encryption show**:</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="5f0f4-143">Az állapot most jelentse az operációsrendszer-lemez és az adatok lemezre **titkosított**.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-143">The status should now report both the OS disk and data disk as **Encrypted**.</span></span>

## <a name="overview-of-disk-encryption"></a><span data-ttu-id="5f0f4-144">Lemeztitkosítás áttekintése</span><span class="sxs-lookup"><span data-stu-id="5f0f4-144">Overview of disk encryption</span></span>
<span data-ttu-id="5f0f4-145">A Linux virtuális gépek virtuális lemezzel titkosított rest az [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-145">Virtual disks on Linux VMs are encrypted at rest using [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span></span> <span data-ttu-id="5f0f4-146">Nincs nem kell fizetni az Azure-ban virtuális lemezek titkosítására.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-146">There is no charge for encrypting virtual disks in Azure.</span></span> <span data-ttu-id="5f0f4-147">Titkosítási kulcsok Azure Key Vault szoftver-védelemmel vannak tárolva, vagy importálhat vagy a tanúsított FIPS 140-2 2. szint szabványok hardveres biztonsági modulokkal (HSM) a kulcsok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-147">Cryptographic keys are stored in Azure Key Vault using software-protection, or you can import or generate your keys in Hardware Security Modules (HSMs) certified to FIPS 140-2 level 2 standards.</span></span> <span data-ttu-id="5f0f4-148">A titkosítási kulcsokat a felügyeletet, és naplózhatja a használatukat.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-148">You retain control of these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="5f0f4-149">Ezek a titkosítási kulcsok titkosítására és visszafejtésére a virtuális Géphez csatolt virtuális lemezek segítségével.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-149">These cryptographic keys are used to encrypt and decrypt virtual disks attached to your VM.</span></span> <span data-ttu-id="5f0f4-150">Egy egyszerű Azure Active Directory szolgáltatás lehetővé teszi a biztonságos kiadása a titkosítási kulcsokat, a virtuális gépek vannak kapcsolva, és ki.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-150">An Azure Active Directory service principal provides a secure mechanism for issuing these cryptographic keys as VMs are powered on and off.</span></span>

<span data-ttu-id="5f0f4-151">A virtuális gépek titkosításához a folyamat a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-151">The process for encrypting a VM is as follows:</span></span>

1. <span data-ttu-id="5f0f4-152">Hozzon létre egy Azure Key Vault egy titkosítási kulcsot.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-152">Create a cryptographic key in an Azure Key Vault.</span></span>
2. <span data-ttu-id="5f0f4-153">Állítsa be a titkosítási kulccsal, hogy a lemezek titkosítására használható.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-153">Configure the cryptographic key to be usable for encrypting disks.</span></span>
3. <span data-ttu-id="5f0f4-154">Az Azure Key Vault beolvasni a titkosítási kulcsot, hozzon létre egy Azure Active Directory szolgáltatás egyszerű a megfelelő engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-154">To read the cryptographic key from the Azure Key Vault, create an Azure Active Directory service principal with the appropriate permissions.</span></span>
4. <span data-ttu-id="5f0f4-155">Adja ki a parancsot, a virtuális lemezek, adja meg az Azure Active Directory szolgáltatás egyszerű és megfelelő titkosítási használandó kulcs titkosításához.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-155">Issue the command to encrypt your virtual disks, specifying the Azure Active Directory service principal and appropriate cryptographic key to be used.</span></span>
5. <span data-ttu-id="5f0f4-156">Az Azure Active Directory szolgáltatás egyszerű kér az Azure Key Vault a szükséges titkosítási kulcs.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-156">The Azure Active Directory service principal requests the required cryptographic key from Azure Key Vault.</span></span>
6. <span data-ttu-id="5f0f4-157">A virtuális lemezek vannak titkosítva, a megadott titkosítási kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-157">The virtual disks are encrypted using the provided cryptographic key.</span></span>

## <a name="encryption-process"></a><span data-ttu-id="5f0f4-158">Titkosítási folyamat</span><span class="sxs-lookup"><span data-stu-id="5f0f4-158">Encryption process</span></span>
<span data-ttu-id="5f0f4-159">Adatok titkosítása a következő további összetevők támaszkodik:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-159">Disk encryption relies on the following additional components:</span></span>

* <span data-ttu-id="5f0f4-160">**Az Azure Key Vault** – a lemez titkosítási/visszafejtési folyamathoz használt titkosítási kulcsok és titkos biztosításához használt.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-160">**Azure Key Vault** - used to safeguard cryptographic keys and secrets used for the disk encryption/decryption process.</span></span>
  * <span data-ttu-id="5f0f4-161">Ha van ilyen, használhat egy meglévő Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-161">If one exists, you can use an existing Azure Key Vault.</span></span> <span data-ttu-id="5f0f4-162">Nem jelölt ki a kulcstároló, a lemezek titkosító rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-162">You do not have to dedicate a Key Vault to encrypting disks.</span></span>
  * <span data-ttu-id="5f0f4-163">Külön felügyeleti határokat és a kulcs látható, a dedikált kulcstároló is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-163">To separate administrative boundaries and key visibility, you can create a dedicated Key Vault.</span></span>
* <span data-ttu-id="5f0f4-164">**Az Azure Active Directory** -kezeli a szükséges titkosítási kulcsokat és a kért műveletek hitelesítési biztonságos cseréjét.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-164">**Azure Active Directory** - handles the secure exchanging of required cryptographic keys and authentication for requested actions.</span></span>
  * <span data-ttu-id="5f0f4-165">Az alkalmazás elhelyezésére szolgáló használhat meglévő Azure Active Directory-példány általában.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-165">You can typically use an existing Azure Active Directory instance for housing your application.</span></span>
  * <span data-ttu-id="5f0f4-166">Az egyszerű szolgáltatás lehetővé teszi a biztonságos le tudja kérni, és adja ki a megfelelő titkosítási kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-166">The service principal provides a secure mechanism to request and be issued the appropriate cryptographic keys.</span></span> <span data-ttu-id="5f0f4-167">Nem fejleszt, amely az Azure Active Directory tényleges kérelmet.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-167">You are not developing an actual application that integrates with Azure Active Directory.</span></span>

## <a name="requirements-and-limitations"></a><span data-ttu-id="5f0f4-168">Követelmények és korlátozások</span><span class="sxs-lookup"><span data-stu-id="5f0f4-168">Requirements and limitations</span></span>
<span data-ttu-id="5f0f4-169">Támogatott esetek és lemez titkosítására vonatkozó követelményekkel kapcsolatos:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-169">Supported scenarios and requirements for disk encryption:</span></span>

* <span data-ttu-id="5f0f4-170">A következő Linux-kiszolgálóra SKU - Ubuntu, CentOS, SUSE és SUSE Linux Enterprise Server (SLES) és Red Hat Enterprise Linux.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-170">The following Linux server SKUs - Ubuntu, CentOS, SUSE and SUSE Linux Enterprise Server (SLES), and Red Hat Enterprise Linux.</span></span>
* <span data-ttu-id="5f0f4-171">Az azonos Azure-régió, és az előfizetés összes erőforrások (például a Key Vault, a tárfiók és a virtuális gép) kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-171">All resources (such as Key Vault, Storage account, and VM) must be in the same Azure region and subscription.</span></span>
* <span data-ttu-id="5f0f4-172">Standard A, a D, a DS-ben, a G és a GS adatsorozat virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-172">Standard A, D, DS, G, and GS series VMs.</span></span>

<span data-ttu-id="5f0f4-173">Lemeztitkosítás jelenleg nem támogatott a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-173">Disk encryption is not currently supported in the following scenarios:</span></span>

* <span data-ttu-id="5f0f4-174">Az alapszintű csomag virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-174">Basic tier VMs.</span></span>
* <span data-ttu-id="5f0f4-175">A virtuális gépek létrehozása a klasszikus telepítési modell használatával.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-175">VMs created using the Classic deployment model.</span></span>
* <span data-ttu-id="5f0f4-176">Az operációs rendszer lemeztitkosítás Linux virtuális gépeken letiltása.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-176">Disabling OS disk encryption on Linux VMs.</span></span>
* <span data-ttu-id="5f0f4-177">A titkosítási kulcsok egy már titkosított Linux virtuális gép frissítése.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-177">Updating the cryptographic keys on an already encrypted Linux VM.</span></span>

## <a name="create-azure-key-vault-and-keys"></a><span data-ttu-id="5f0f4-178">Az Azure Key Vault és kulcsok létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f0f4-178">Create Azure Key Vault and keys</span></span>
<span data-ttu-id="5f0f4-179">A legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett az Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-179">You need the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in to an Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="5f0f4-180">A következő példákban cserélje le a saját értékeit példa paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-180">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="5f0f4-181">Példa paraméter nevek a következők *myResourceGroup*, *SajátKulcs*, és *myVM*.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-181">Example parameter names include *myResourceGroup*, *myKey*, and *myVM*.</span></span>

<span data-ttu-id="5f0f4-182">Az első lépés, ha az Azure Key Vault a kriptográfiai kulcsok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-182">The first step is to create an Azure Key Vault to store your cryptographic keys.</span></span> <span data-ttu-id="5f0f4-183">Az Azure Key Vault kulcsok, a titkos kulcsok és a jelszót, amely engedélyezi, hogy biztonságosan végrehajtja az alkalmazások és szolgáltatások a tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-183">Azure Key Vault can store keys, secrets, or passwords that allow you to securely implement them in your applications and services.</span></span> <span data-ttu-id="5f0f4-184">A virtuális lemez titkosításához segítségével Key Vault titkosítására vagy visszafejtésére a virtuális lemezek használt kriptográfiai kulcs tárolása.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-184">For virtual disk encryption, you use Key Vault to store a cryptographic key that is used to encrypt or decrypt your virtual disks.</span></span>

<span data-ttu-id="5f0f4-185">Engedélyezze az Azure Key Vault-szolgáltató az Azure-előfizetése belül [az szolgáltató regisztrálása](/cli/azure/provider#register) és hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-185">Enable the Azure Key Vault provider within your Azure subscription with [az provider register](/cli/azure/provider#register) and create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="5f0f4-186">Az alábbi példa létrehoz egy erőforráscsoport neve *myResourceGroup* a a `eastus` helye:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-186">The following example creates a resource group name *myResourceGroup* in the `eastus` location:</span></span>

```azurecli
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="5f0f4-187">Az Azure Key Vault a kriptográfiai kulcsokat és tartozó számítási erőforrásokat, például a tároló és a virtuális gépért tartalmazó ugyanabban a régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-187">The Azure Key Vault containing the cryptographic keys and associated compute resources such as storage and the VM itself must reside in the same region.</span></span> <span data-ttu-id="5f0f4-188">Hozzon létre egy Azure Key Vault a [az keyvault létrehozása](/cli/azure/keyvault#create) , és engedélyezze a Key Vault lemeztitkosítás való használatra.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-188">Create an Azure Key Vault with [az keyvault create](/cli/azure/keyvault#create) and enable the Key Vault for use with disk encryption.</span></span> <span data-ttu-id="5f0f4-189">Adjon meg egy egyedi kulcstároló nevet *keyvault_name* az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-189">Specify a unique Key Vault name for *keyvault_name* as follows:</span></span>

```azurecli
keyvault_name=myUniqueKeyVaultName
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

<span data-ttu-id="5f0f4-190">A szoftver vagy a biztonsági modell (HSM) védelmi kriptográfiai kulcsokat is tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-190">You can store cryptographic keys using software or Hardware Security Model (HSM) protection.</span></span> <span data-ttu-id="5f0f4-191">A Key Vault prémium HSM használata szükséges.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-191">Using an HSM requires a premium Key Vault.</span></span> <span data-ttu-id="5f0f4-192">Nincs a prémium szabványos Key Vault szoftveresen védett tároló helyett kulcstároló létrehozásához további költségek.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-192">There is an additional cost to creating a premium Key Vault rather than standard Key Vault that stores software-protected keys.</span></span> <span data-ttu-id="5f0f4-193">A prémium kulcstároló létrehozásához az előző lépésben hozzáadása `--sku Premium` a parancshoz.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-193">To create a premium Key Vault, in the preceding step add `--sku Premium` to the command.</span></span> <span data-ttu-id="5f0f4-194">Az alábbi példában szoftveresen védett azt a szabványos kulcstároló létrehozása óta.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-194">The following example uses software-protected keys since we created a standard Key Vault.</span></span>

<span data-ttu-id="5f0f4-195">Mindkét védelmi modellek esetében az Azure platformon kell hozzáférést kérni a titkosítási kulcsokat a virtuális lemezek visszafejtése a virtuális gép indításakor.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-195">For both protection models, the Azure platform needs to be granted access to request the cryptographic keys when the VM boots to decrypt the virtual disks.</span></span> <span data-ttu-id="5f0f4-196">A kulcstároló, a titkosítási kulcs létrehozására [az keyvault kulcs létrehozása](/cli/azure/keyvault/key#create).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-196">Create a cryptographic key in your Key Vault with [az keyvault key create](/cli/azure/keyvault/key#create).</span></span> <span data-ttu-id="5f0f4-197">Az alábbi példakód létrehozza nevű kulcs *SajátKulcs*:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-197">The following example creates a key named *myKey*:</span></span>

```azurecli
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```


## <a name="create-the-azure-active-directory-service-principal"></a><span data-ttu-id="5f0f4-198">Az Azure Active Directory szolgáltatás egyszerű létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f0f4-198">Create the Azure Active Directory service principal</span></span>
<span data-ttu-id="5f0f4-199">Ha a virtuális lemezek vannak titkosított vagy visszafejtett, meg kell adnia egy fiókot a hitelesítési és titkosítási kulcsok a Key Vault cseréjét.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-199">When virtual disks are encrypted or decrypted, you specify an account to handle the authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="5f0f4-200">Ezt a fiókot, egy Azure Active Directory szolgáltatás egyszerű, lehetővé teszi, hogy az Azure platformon, kérje a megfelelő titkosítási kulcsokat a virtuális gép nevében.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-200">This account, an Azure Active Directory service principal, allows the Azure platform to request the appropriate cryptographic keys on behalf of the VM.</span></span> <span data-ttu-id="5f0f4-201">Azure Active Directory alapértelmezett példányán érhető előfizetését, bár számos szervezet Azure Active Directory-könyvtárak dedikált.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-201">A default Azure Active Directory instance is available in your subscription, though many organizations have dedicated Azure Active Directory directories.</span></span>

<span data-ttu-id="5f0f4-202">Az Azure Active Directoryval az egyszerű szolgáltatás létrehozása [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-202">Create a service principal using Azure Active Directory with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac).</span></span> <span data-ttu-id="5f0f4-203">Az alábbi példa beolvassa az értékek a szolgáltatás egyszerű azonosító és jelszó használható újabb parancsokat:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-203">The following example reads in the values for the service principal Id and password for use in later commands:</span></span>

```azurecli
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

<span data-ttu-id="5f0f4-204">A jelszó csak akkor jelenik meg, ha a szolgáltatás egyszerű létrehozása.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-204">The password is only displayed when you create the service principal.</span></span> <span data-ttu-id="5f0f4-205">Szükség esetén megtekintheti, és jegyezze fel a jelszót (`echo $sp_password`).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-205">If desired, view and record the password (`echo $sp_password`).</span></span> <span data-ttu-id="5f0f4-206">A szolgáltatás elsődleges listázhatja [az sp hirdetéslista](/cli/azure/ad/sp#list) és egyszerű, az adott szolgáltatás további információkat tekinthet [az ad sp megjelenítése](/cli/azure/ad/sp#show).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-206">You can list your service principals with [az ad sp list](/cli/azure/ad/sp#list) and view additional information about a specific service principal with [az ad sp show](/cli/azure/ad/sp#show).</span></span>

<span data-ttu-id="5f0f4-207">Sikeresen vagy titkosítására vagy visszafejtésére virtuális lemezek, a Key Vault tárolt titkosítási kulcs engedélyei lehetővé teszik az Azure Active Directory szolgáltatás egyszerű az a kulcsok beolvasása a értékre kell állítani.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-207">To successfully encrypt or decrypt virtual disks, permissions on the cryptographic key stored in Key Vault must be set to permit the Azure Active Directory service principal to read the keys.</span></span> <span data-ttu-id="5f0f4-208">A kulcstároló, az engedélyeket [az keyvault-szabály beállítása](/cli/azure/keyvault#set-policy).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-208">Set permissions on your Key Vault with [az keyvault set-policy](/cli/azure/keyvault#set-policy).</span></span> <span data-ttu-id="5f0f4-209">A következő példában a résztvevő-azonosító az előző parancs van megadva:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-209">In the following example, the service principal ID is supplied from the preceding command:</span></span>

```azurecli
az keyvault set-policy --name $keyvault_name --spn $sp_id \
  --key-permissions wrapKey \
  --secret-permissions set
```


## <a name="create-virtual-machine"></a><span data-ttu-id="5f0f4-210">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="5f0f4-210">Create virtual machine</span></span>
<span data-ttu-id="5f0f4-211">Ténylegesen titkosíthatja az egyes virtuális lemezek, lehetővé teszi, hogy a virtuális gép létrehozása és a hozzá adatlemezt.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-211">To actually encrypt some virtual disks, lets create a VM and add a data disk.</span></span> <span data-ttu-id="5f0f4-212">Hozzon létre egy virtuális Gépet titkosítást a [az virtuális gép létrehozása](/cli/azure/vm#create) és 5 Gb adatlemezt csatolni.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-212">Create a VM to encrypt with [az vm create](/cli/azure/vm#create) and attach a 5Gb data disk.</span></span> <span data-ttu-id="5f0f4-213">Csak bizonyos piactéren elérhető rendszerkép lemeztitkosítás támogatja.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-213">Only certain marketplace images support disk encryption.</span></span> <span data-ttu-id="5f0f4-214">Az alábbi példakód létrehozza a virtuális gépek nevű *myVM* használatával egy **CentOS 7.2n** lemezképet:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-214">The following example creates a VM named *myVM* using a **CentOS 7.2n** image:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image OpenLogic:CentOS:7.2n:7.2.20160629 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

<span data-ttu-id="5f0f4-215">SSH-kapcsolatot a virtuális gép használata a `publicIpAddress` látható a fenti parancs kimenetét.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-215">SSH to your VM using the `publicIpAddress` shown in the output of the preceding command.</span></span> <span data-ttu-id="5f0f4-216">Hozzon létre egy partíciót és filesystem, majd csatlakoztassa az adatok lemezt.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-216">Create a partition and filesystem, then mount the data disk.</span></span> <span data-ttu-id="5f0f4-217">További információkért lásd: [csatlakozás egy Linux virtuális Gépet az új lemez csatlakoztatásához](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-217">For more information, see [Connect to a Linux VM to mount the new disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span></span> <span data-ttu-id="5f0f4-218">Zárja be az SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-218">Close your SSH session.</span></span>


## <a name="encrypt-virtual-machine"></a><span data-ttu-id="5f0f4-219">Virtuális gép titkosítása</span><span class="sxs-lookup"><span data-stu-id="5f0f4-219">Encrypt virtual machine</span></span>
<span data-ttu-id="5f0f4-220">A virtuális lemez titkosításához, kapcsolása együtt minden az előző összetevők működését:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-220">To encrypt the virtual disks, you bring together all the previous components:</span></span>

1. <span data-ttu-id="5f0f4-221">Adja meg az Azure Active Directory szolgáltatás egyszerű és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-221">Specify the Azure Active Directory service principal and password.</span></span>
2. <span data-ttu-id="5f0f4-222">Adja meg a Key Vault a titkosított lemezek metaadatait tárolja.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-222">Specify the Key Vault to store the metadata for your encrypted disks.</span></span>
3. <span data-ttu-id="5f0f4-223">Adja meg a titkosítási kulcsok használandó tényleges titkosításához és visszafejtéséhez.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-223">Specify the cryptographic keys to use for the actual encryption and decryption.</span></span>
4. <span data-ttu-id="5f0f4-224">Adja meg, hogy az operációsrendszer-lemezképet, az adatlemezek vagy az összes titkosításához.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-224">Specify whether you want to encrypt the OS disk, the data disks, or all.</span></span>

<span data-ttu-id="5f0f4-225">Az a virtuális gép titkosítása [az vm-titkosítás engedélyezése](/cli/azure/vm/encryption#enable).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-225">Encrypt your VM with [az vm encryption enable](/cli/azure/vm/encryption#enable).</span></span> <span data-ttu-id="5f0f4-226">Az alábbi példában a `$sp_id` és `$sp_password` változókat az előző `ad sp create-for-rbac` parancs:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-226">The following example uses the `$sp_id` and `$sp_password` variables from the preceding `ad sp create-for-rbac` command:</span></span>

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```

<span data-ttu-id="5f0f4-227">A titkosítási folyamat befejezéséhez sok időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-227">It takes some time for the disk encryption process to complete.</span></span> <span data-ttu-id="5f0f4-228">A folyamat állapotának figyelése [az vm titkosítási megjelenítése](/cli/azure/vm/encryption#show):</span><span class="sxs-lookup"><span data-stu-id="5f0f4-228">Monitor the status of the process with [az vm encryption show](/cli/azure/vm/encryption#show):</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="5f0f4-229">A kimenete a következő csonkolt példához hasonló:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-229">The output is similar to the following truncated example:</span></span>

```json
[
  "dataDisk": "EncryptionInProgress",
  "osDisk": "EncryptionInProgress",
]
```

<span data-ttu-id="5f0f4-230">Várjon, amíg az operációs rendszer állapota lemez jelentések **VMRestartPending**, majd indítsa újra a virtuális Gépet a [az virtuális gép újraindítása](/cli/azure/vm#restart):</span><span class="sxs-lookup"><span data-stu-id="5f0f4-230">Wait until the status for the OS disk reports **VMRestartPending**, then restart your VM with [az vm restart](/cli/azure/vm#restart):</span></span>

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="5f0f4-231">A titkosítási folyamat akkor fejeződik be, a rendszerindítási folyamat során, így Várjon néhány percet, majd újra titkosítási állapotának ellenőrzésekor **az vm titkosítási megjelenítése**:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-231">The disk encryption process is finalized during the boot process, so wait a few minutes before checking the status of encryption again with **az vm encryption show**:</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="5f0f4-232">Az állapot most jelentse az operációsrendszer-lemez és az adatok lemezre **titkosított**.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-232">The status should now report both the OS disk and data disk as **Encrypted**.</span></span>


## <a name="add-additional-data-disks"></a><span data-ttu-id="5f0f4-233">További adatok lemezek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="5f0f4-233">Add additional data disks</span></span>
<span data-ttu-id="5f0f4-234">Az adatlemezek titkosított, ha később további virtuális lemezek hozzáadása a virtuális Gépet, és is titkosítani.</span><span class="sxs-lookup"><span data-stu-id="5f0f4-234">Once you have encrypted your data disks, you can later add additional virtual disks to your VM and also encrypt them.</span></span> <span data-ttu-id="5f0f4-235">Például lehetővé teszi, hogy a második virtuális lemez hozzáadása a virtuális Gépet az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-235">For example, lets add a second virtual disk to your VM as follows:</span></span>

```azurecli
az vm disk attach-new --resource-group myResourceGroup --vm-name myVM --size-in-gb 5
```

<span data-ttu-id="5f0f4-236">Futtassa újra a parancsot a virtuális lemezek titkosítani az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5f0f4-236">Re-run the command to encrypt the virtual disks as follows:</span></span>

```azurecli
az vm encryption enable \
    --resource-group myResourceGroup \
    --name myVM \
    --aad-client-id $sp_id \
    --aad-client-secret $sp_password \
    --disk-encryption-keyvault $keyvault_name \
    --key-encryption-key myKey \
    --volume-type all
```


## <a name="next-steps"></a><span data-ttu-id="5f0f4-237">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="5f0f4-237">Next steps</span></span>
* <span data-ttu-id="5f0f4-238">Az Azure Key Vault, beleértve a titkosítási kulcsok és a tárolók kezelésével kapcsolatos további információért lásd: [kezelése Key Vault parancssori felület használatával](../../key-vault/key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-238">For more information about managing Azure Key Vault, including deleting cryptographic keys and vaults, see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md).</span></span>
* <span data-ttu-id="5f0f4-239">Lemeztitkosítás, például egy titkosított egyéni virtuális Gépre az Azure-ba, a feltöltendő előkészítése kapcsolatos további információk: [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="5f0f4-239">For more information about disk encryption, such as preparing an encrypted custom VM to upload to Azure, see [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span></span>
