---
title: "a Linux virtuális gép az Azure-ban aaaEncrypt lemezek |} Microsoft Docs"
description: "Hogyan tooencrypt virtuális lemezek, a Linux virtuális gép a fokozott biztonság használatára vonatkozó hello Azure CLI 2.0"
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
ms.openlocfilehash: d6197742bc8562630e8395588c072093fc01d614
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooencrypt-virtual-disks-on-a-linux-vm"></a><span data-ttu-id="964ef-103">Hogyan tooencrypt virtuális lemezek, a Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="964ef-103">How tooencrypt virtual disks on a Linux VM</span></span>
<span data-ttu-id="964ef-104">A bővített virtuális gép (VM) biztonsági és megfelelőségi az Azure-ban virtuális lemezek titkosíthatók.</span><span class="sxs-lookup"><span data-stu-id="964ef-104">For enhanced virtual machine (VM) security and compliance, virtual disks in Azure can be encrypted.</span></span> <span data-ttu-id="964ef-105">Lemezek titkosítása egy Azure Key Vault a titkosított titkosítási kulcsok használatával.</span><span class="sxs-lookup"><span data-stu-id="964ef-105">Disks are encrypted using cryptographic keys that are secured in an Azure Key Vault.</span></span> <span data-ttu-id="964ef-106">Szabályozhatja a titkosítási kulcsokat, és a használatukat naplózhatók.</span><span class="sxs-lookup"><span data-stu-id="964ef-106">You control these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="964ef-107">Ez a cikk részletesen, hogyan Linux virtuális gép virtuális lemezein tooencrypt hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="964ef-107">This article details how tooencrypt virtual disks on a Linux VM using hello Azure CLI 2.0.</span></span> <span data-ttu-id="964ef-108">Is elvégezheti ezeket a lépéseket hello [Azure CLI 1.0](encrypt-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="964ef-108">You can also perform these steps with hello [Azure CLI 1.0](encrypt-disks-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="quick-commands"></a><span data-ttu-id="964ef-109">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="964ef-109">Quick commands</span></span>
<span data-ttu-id="964ef-110">Ha tooquickly kell hello feladatnak, a következő szakasz részletek hello alapszintű hello parancsok tooencrypt virtuális lemezek, a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="964ef-110">If you need tooquickly accomplish hello task, hello following section details hello base commands tooencrypt virtual disks on your VM.</span></span> <span data-ttu-id="964ef-111">Részletes információkat és a környezetben az egyes lépések található hello dokumentum többi részén hello, [itt indítása](#overview-of-disk-encryption).</span><span class="sxs-lookup"><span data-stu-id="964ef-111">More detailed information and context for each step can be found hello rest of hello document, [starting here](#overview-of-disk-encryption).</span></span>

<span data-ttu-id="964ef-112">Hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="964ef-112">You need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="964ef-113">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="964ef-113">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="964ef-114">Példa paraméter nevek a következők *myResourceGroup*, *SajátKulcs*, és *myVM*.</span><span class="sxs-lookup"><span data-stu-id="964ef-114">Example parameter names include *myResourceGroup*, *myKey*, and *myVM*.</span></span>

<span data-ttu-id="964ef-115">Először, engedélyezése hello Azure Key Vault szolgáltató az Azure-előfizetése belül [az szolgáltató regisztrálása](/cli/azure/provider#register) és hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="964ef-115">First, enable hello Azure Key Vault provider within your Azure subscription with [az provider register](/cli/azure/provider#register) and create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="964ef-116">hello alábbi példa létrehoz egy erőforráscsoport neve *myResourceGroup* a hello *eastus* helye:</span><span class="sxs-lookup"><span data-stu-id="964ef-116">hello following example creates a resource group name *myResourceGroup* in hello *eastus* location:</span></span>

```azurecli
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="964ef-117">Hozzon létre egy Azure Key Vault a [az keyvault létrehozása](/cli/azure/keyvault#create) , és engedélyezze a Key Vault hello lemeztitkosítás való használatra.</span><span class="sxs-lookup"><span data-stu-id="964ef-117">Create an Azure Key Vault with [az keyvault create](/cli/azure/keyvault#create) and enable hello Key Vault for use with disk encryption.</span></span> <span data-ttu-id="964ef-118">Adjon meg egy egyedi kulcstároló nevet *keyvault_name* az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="964ef-118">Specify a unique Key Vault name for *keyvault_name* as follows:</span></span>

```azurecli
keyvault_name=mykeyvaultikf
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

<span data-ttu-id="964ef-119">A kulcstároló, a titkosítási kulcs létrehozására [az keyvault kulcs létrehozása](/cli/azure/keyvault/key#create).</span><span class="sxs-lookup"><span data-stu-id="964ef-119">Create a cryptographic key in your Key Vault with [az keyvault key create](/cli/azure/keyvault/key#create).</span></span> <span data-ttu-id="964ef-120">hello alábbi példa létrehoz egy nevű kulcsot *SajátKulcs*:</span><span class="sxs-lookup"><span data-stu-id="964ef-120">hello following example creates a key named *myKey*:</span></span>

```azurecli
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```

<span data-ttu-id="964ef-121">Az Azure Active Directoryval az egyszerű szolgáltatás létrehozása [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac).</span><span class="sxs-lookup"><span data-stu-id="964ef-121">Create a service principal using Azure Active Directory with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac).</span></span> <span data-ttu-id="964ef-122">hello szolgáltatás egyszerű kezeli a hitelesítési és titkosítási kulcsok a Key Vault exchange hello.</span><span class="sxs-lookup"><span data-stu-id="964ef-122">hello service principal handles hello authentication and exchange of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="964ef-123">a következő példa hello beolvassa hello értékeinek hello szolgáltatás egyszerű azonosító és jelszó használható újabb parancsok:</span><span class="sxs-lookup"><span data-stu-id="964ef-123">hello following example reads in hello values for hello service principal Id and password for use in later commands:</span></span>

```azurecli
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

<span data-ttu-id="964ef-124">hello jelszó csak kimeneti hello szolgáltatás egyszerű létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="964ef-124">hello password is only output when you create hello service principal.</span></span> <span data-ttu-id="964ef-125">Ha szükséges, megtekintése és rekord hello jelszó (`echo $sp_password`).</span><span class="sxs-lookup"><span data-stu-id="964ef-125">If desired, view and record hello password (`echo $sp_password`).</span></span> <span data-ttu-id="964ef-126">A szolgáltatás elsődleges listázhatja [az sp hirdetéslista](/cli/azure/ad/sp#list) és egyszerű, az adott szolgáltatás további információkat tekinthet [az ad sp megjelenítése](/cli/azure/ad/sp#show).</span><span class="sxs-lookup"><span data-stu-id="964ef-126">You can list your service principals with [az ad sp list](/cli/azure/ad/sp#list) and view additional information about a specific service principal with [az ad sp show](/cli/azure/ad/sp#show).</span></span>

<span data-ttu-id="964ef-127">A kulcstároló, az engedélyeket [az keyvault-szabály beállítása](/cli/azure/keyvault#set-policy).</span><span class="sxs-lookup"><span data-stu-id="964ef-127">Set permissions on your Key Vault with [az keyvault set-policy](/cli/azure/keyvault#set-policy).</span></span> <span data-ttu-id="964ef-128">A következő példa hello hello résztvevő-azonosító van megadva a fenti parancs hello:</span><span class="sxs-lookup"><span data-stu-id="964ef-128">In hello following example, hello service principal ID is supplied from hello preceding command:</span></span>

```azurecli
az keyvault set-policy --name $keyvault_name --spn $sp_id \
    --key-permissions wrapKey \
    --secret-permissions set
```

<span data-ttu-id="964ef-129">A virtuális gép létrehozása [az virtuális gép létrehozása](/cli/azure/vm#create) és 5 Gb adatlemezt csatolni.</span><span class="sxs-lookup"><span data-stu-id="964ef-129">Create a VM with [az vm create](/cli/azure/vm#create) and attach a 5Gb data disk.</span></span> <span data-ttu-id="964ef-130">Csak bizonyos piactéren elérhető rendszerkép lemeztitkosítás támogatja.</span><span class="sxs-lookup"><span data-stu-id="964ef-130">Only certain marketplace images support disk encryption.</span></span> <span data-ttu-id="964ef-131">hello alábbi példakód létrehozza a virtuális gépek nevű `myVM` használatával egy **CentOS 7.2n** lemezképet:</span><span class="sxs-lookup"><span data-stu-id="964ef-131">hello following example creates a VM named `myVM` using a **CentOS 7.2n** image:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image OpenLogic:CentOS:7.2n:7.2.20160629 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

<span data-ttu-id="964ef-132">SSH tooyour VM használatával hello `publicIpAddress` hello megelőző parancs kimenetét hello látható.</span><span class="sxs-lookup"><span data-stu-id="964ef-132">SSH tooyour VM using hello `publicIpAddress` shown in hello output of hello preceding command.</span></span> <span data-ttu-id="964ef-133">Hozzon létre egy partíciót és filesystem, majd hello adatlemez csatlakoztatása.</span><span class="sxs-lookup"><span data-stu-id="964ef-133">Create a partition and filesystem, then mount hello data disk.</span></span> <span data-ttu-id="964ef-134">További információkért lásd: [tooa Linux virtuális gép toomount hello új lemez csatlakozás](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span><span class="sxs-lookup"><span data-stu-id="964ef-134">For more information, see [Connect tooa Linux VM toomount hello new disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span></span> <span data-ttu-id="964ef-135">Zárja be az SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="964ef-135">Close your SSH session.</span></span>

<span data-ttu-id="964ef-136">Az a virtuális gép titkosítása [az vm-titkosítás engedélyezése](/cli/azure/vm/encryption#enable).</span><span class="sxs-lookup"><span data-stu-id="964ef-136">Encrypt your VM with [az vm encryption enable](/cli/azure/vm/encryption#enable).</span></span> <span data-ttu-id="964ef-137">hello alábbi példában hello `$sp_id` és `$sp_password` változók az előző hello `ad sp create-for-rbac` parancs:</span><span class="sxs-lookup"><span data-stu-id="964ef-137">hello following example uses hello `$sp_id` and `$sp_password` variables from hello preceding `ad sp create-for-rbac` command:</span></span>

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

<span data-ttu-id="964ef-138">Hello lemez titkosítási folyamat toocomplete némi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="964ef-138">It takes some time for hello disk encryption process toocomplete.</span></span> <span data-ttu-id="964ef-139">Hello folyamat hello állapotának figyelése [az vm titkosítási megjelenítése](/cli/azure/vm/encryption#show):</span><span class="sxs-lookup"><span data-stu-id="964ef-139">Monitor hello status of hello process with [az vm encryption show](/cli/azure/vm/encryption#show):</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="964ef-140">hello állapota látható szövegrészt **EncryptionInProgress**.</span><span class="sxs-lookup"><span data-stu-id="964ef-140">hello status shows **EncryptionInProgress**.</span></span> <span data-ttu-id="964ef-141">Várja meg, amíg hello állapot hello OS lemez jelentések **VMRestartPending**, majd indítsa újra a virtuális Gépet a [az virtuális gép újraindítása](/cli/azure/vm#restart):</span><span class="sxs-lookup"><span data-stu-id="964ef-141">Wait until hello status for hello OS disk reports **VMRestartPending**, then restart your VM with [az vm restart](/cli/azure/vm#restart):</span></span>

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="964ef-142">hello lemez titkosítási folyamat akkor fejeződik be hello rendszerindítási folyamat során, így Várjon néhány percet, majd újra titkosítási hello állapotának ellenőrzése **az vm titkosítási megjelenítése**:</span><span class="sxs-lookup"><span data-stu-id="964ef-142">hello disk encryption process is finalized during hello boot process, so wait a few minutes before checking hello status of encryption again with **az vm encryption show**:</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="964ef-143">hello állapota most jelentse hello operációsrendszer-lemez, mind az adatok lemezre **titkosított**.</span><span class="sxs-lookup"><span data-stu-id="964ef-143">hello status should now report both hello OS disk and data disk as **Encrypted**.</span></span>

## <a name="overview-of-disk-encryption"></a><span data-ttu-id="964ef-144">Lemeztitkosítás áttekintése</span><span class="sxs-lookup"><span data-stu-id="964ef-144">Overview of disk encryption</span></span>
<span data-ttu-id="964ef-145">A Linux virtuális gépek virtuális lemezzel titkosított rest az [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span><span class="sxs-lookup"><span data-stu-id="964ef-145">Virtual disks on Linux VMs are encrypted at rest using [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span></span> <span data-ttu-id="964ef-146">Nincs nem kell fizetni az Azure-ban virtuális lemezek titkosítására.</span><span class="sxs-lookup"><span data-stu-id="964ef-146">There is no charge for encrypting virtual disks in Azure.</span></span> <span data-ttu-id="964ef-147">Titkosítási kulcsok Azure Key Vault szoftver-védelemmel vannak tárolva, vagy importálhat vagy Kulcslétrehozási a hardveres biztonsági modulokkal (HSM) a hitelesített tooFIPS 140-2 2. szint szabványoknak.</span><span class="sxs-lookup"><span data-stu-id="964ef-147">Cryptographic keys are stored in Azure Key Vault using software-protection, or you can import or generate your keys in Hardware Security Modules (HSMs) certified tooFIPS 140-2 level 2 standards.</span></span> <span data-ttu-id="964ef-148">A titkosítási kulcsokat a felügyeletet, és naplózhatja a használatukat.</span><span class="sxs-lookup"><span data-stu-id="964ef-148">You retain control of these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="964ef-149">A kriptográfiai kulcsokat használt tooencrypt és virtuális lemezek csatolt tooyour VM visszafejtéséhez.</span><span class="sxs-lookup"><span data-stu-id="964ef-149">These cryptographic keys are used tooencrypt and decrypt virtual disks attached tooyour VM.</span></span> <span data-ttu-id="964ef-150">Egy egyszerű Azure Active Directory szolgáltatás lehetővé teszi a biztonságos kiadása a titkosítási kulcsokat, a virtuális gépek vannak kapcsolva, és ki.</span><span class="sxs-lookup"><span data-stu-id="964ef-150">An Azure Active Directory service principal provides a secure mechanism for issuing these cryptographic keys as VMs are powered on and off.</span></span>

<span data-ttu-id="964ef-151">a virtuális gépek titkosításához hello folyamat a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="964ef-151">hello process for encrypting a VM is as follows:</span></span>

1. <span data-ttu-id="964ef-152">Hozzon létre egy Azure Key Vault egy titkosítási kulcsot.</span><span class="sxs-lookup"><span data-stu-id="964ef-152">Create a cryptographic key in an Azure Key Vault.</span></span>
2. <span data-ttu-id="964ef-153">Konfigurálja a hello kriptográfiai kulcs toobe lemezek titkosítására használható.</span><span class="sxs-lookup"><span data-stu-id="964ef-153">Configure hello cryptographic key toobe usable for encrypting disks.</span></span>
3. <span data-ttu-id="964ef-154">tooread hello titkosítási kulcsot az Azure Key Vault hello hozzon létre egy Azure Active Directory szolgáltatás egyszerű hello megfelelő engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="964ef-154">tooread hello cryptographic key from hello Azure Key Vault, create an Azure Active Directory service principal with hello appropriate permissions.</span></span>
4. <span data-ttu-id="964ef-155">Hello parancs tooencrypt ki a virtuális lemezek, megadva hello Azure Active Directory szolgáltatás egyszerű és a megfelelő titkosítási kulcs toobe használt.</span><span class="sxs-lookup"><span data-stu-id="964ef-155">Issue hello command tooencrypt your virtual disks, specifying hello Azure Active Directory service principal and appropriate cryptographic key toobe used.</span></span>
5. <span data-ttu-id="964ef-156">hello Azure Active Directory szolgáltatás egyszerű kérelmek hello szükséges titkosítási kulcsot az Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="964ef-156">hello Azure Active Directory service principal requests hello required cryptographic key from Azure Key Vault.</span></span>
6. <span data-ttu-id="964ef-157">virtuális lemezek hello hello megadott titkosítási kulccsal titkosított.</span><span class="sxs-lookup"><span data-stu-id="964ef-157">hello virtual disks are encrypted using hello provided cryptographic key.</span></span>

## <a name="encryption-process"></a><span data-ttu-id="964ef-158">Titkosítási folyamat</span><span class="sxs-lookup"><span data-stu-id="964ef-158">Encryption process</span></span>
<span data-ttu-id="964ef-159">Adatok titkosítása a következő további összetevők hello támaszkodik:</span><span class="sxs-lookup"><span data-stu-id="964ef-159">Disk encryption relies on hello following additional components:</span></span>

* <span data-ttu-id="964ef-160">**Az Azure Key Vault** -használt toosafeguard titkosítási kulcsok és titkos hello lemez titkosítási/visszafejtési folyamat használja.</span><span class="sxs-lookup"><span data-stu-id="964ef-160">**Azure Key Vault** - used toosafeguard cryptographic keys and secrets used for hello disk encryption/decryption process.</span></span>
  * <span data-ttu-id="964ef-161">Ha van ilyen, használhat egy meglévő Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="964ef-161">If one exists, you can use an existing Azure Key Vault.</span></span> <span data-ttu-id="964ef-162">Nincs toodedicate a Key Vault tooencrypting lemezek.</span><span class="sxs-lookup"><span data-stu-id="964ef-162">You do not have toodedicate a Key Vault tooencrypting disks.</span></span>
  * <span data-ttu-id="964ef-163">tooseparate felügyeleti határokat és kulcs látható, a dedikált kulcstároló is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="964ef-163">tooseparate administrative boundaries and key visibility, you can create a dedicated Key Vault.</span></span>
* <span data-ttu-id="964ef-164">**Az Azure Active Directory** - leírók hello szükséges titkosítási kulcsok biztonságos cseréjét, és hitelesítési a kért műveleteket.</span><span class="sxs-lookup"><span data-stu-id="964ef-164">**Azure Active Directory** - handles hello secure exchanging of required cryptographic keys and authentication for requested actions.</span></span>
  * <span data-ttu-id="964ef-165">Az alkalmazás elhelyezésére szolgáló használhat meglévő Azure Active Directory-példány általában.</span><span class="sxs-lookup"><span data-stu-id="964ef-165">You can typically use an existing Azure Active Directory instance for housing your application.</span></span>
  * <span data-ttu-id="964ef-166">egyszerű hello szolgáltatás egy biztonságos mechanizmus toorequest és hello megfelelő titkosítási kulcsok adja ki.</span><span class="sxs-lookup"><span data-stu-id="964ef-166">hello service principal provides a secure mechanism toorequest and be issued hello appropriate cryptographic keys.</span></span> <span data-ttu-id="964ef-167">Nem fejleszt, amely az Azure Active Directory tényleges kérelmet.</span><span class="sxs-lookup"><span data-stu-id="964ef-167">You are not developing an actual application that integrates with Azure Active Directory.</span></span>

## <a name="requirements-and-limitations"></a><span data-ttu-id="964ef-168">Követelmények és korlátozások</span><span class="sxs-lookup"><span data-stu-id="964ef-168">Requirements and limitations</span></span>
<span data-ttu-id="964ef-169">Támogatott esetek és lemez titkosítására vonatkozó követelményekkel kapcsolatos:</span><span class="sxs-lookup"><span data-stu-id="964ef-169">Supported scenarios and requirements for disk encryption:</span></span>

* <span data-ttu-id="964ef-170">a következő Linux server SKU - Ubuntu, CentOS, SUSE és SUSE Linux Enterprise Server (SLES) és Red Hat Enterprise Linux hello.</span><span class="sxs-lookup"><span data-stu-id="964ef-170">hello following Linux server SKUs - Ubuntu, CentOS, SUSE and SUSE Linux Enterprise Server (SLES), and Red Hat Enterprise Linux.</span></span>
* <span data-ttu-id="964ef-171">Minden erőforrások (például a Key Vault, a tárfiók és a virtuális gép) kell lennie a hello azonos Azure-régió, és az előfizetés.</span><span class="sxs-lookup"><span data-stu-id="964ef-171">All resources (such as Key Vault, Storage account, and VM) must be in hello same Azure region and subscription.</span></span>
* <span data-ttu-id="964ef-172">Standard A, a D, a DS-ben, a G és a GS adatsorozat virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="964ef-172">Standard A, D, DS, G, and GS series VMs.</span></span>

<span data-ttu-id="964ef-173">Lemeztitkosítás jelenleg nem támogatott a következő forgatókönyvek hello:</span><span class="sxs-lookup"><span data-stu-id="964ef-173">Disk encryption is not currently supported in hello following scenarios:</span></span>

* <span data-ttu-id="964ef-174">Az alapszintű csomag virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="964ef-174">Basic tier VMs.</span></span>
* <span data-ttu-id="964ef-175">A virtuális gépek hello klasszikus telepítési modellel készült.</span><span class="sxs-lookup"><span data-stu-id="964ef-175">VMs created using hello Classic deployment model.</span></span>
* <span data-ttu-id="964ef-176">Az operációs rendszer lemeztitkosítás Linux virtuális gépeken letiltása.</span><span class="sxs-lookup"><span data-stu-id="964ef-176">Disabling OS disk encryption on Linux VMs.</span></span>
* <span data-ttu-id="964ef-177">Titkosítási kulcsok hello az már titkosított Linux virtuális gép frissítése.</span><span class="sxs-lookup"><span data-stu-id="964ef-177">Updating hello cryptographic keys on an already encrypted Linux VM.</span></span>

## <a name="create-azure-key-vault-and-keys"></a><span data-ttu-id="964ef-178">Az Azure Key Vault és kulcsok létrehozása</span><span class="sxs-lookup"><span data-stu-id="964ef-178">Create Azure Key Vault and keys</span></span>
<span data-ttu-id="964ef-179">Hello legújabb kell [Azure CLI 2.0](/cli/azure/install-az-cli2) telepítve, és bejelentkezett tooan Azure-fiók használatával [az bejelentkezési](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="964ef-179">You need hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) installed and logged in tooan Azure account using [az login](/cli/azure/#login).</span></span> <span data-ttu-id="964ef-180">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="964ef-180">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="964ef-181">Példa paraméter nevek a következők *myResourceGroup*, *SajátKulcs*, és *myVM*.</span><span class="sxs-lookup"><span data-stu-id="964ef-181">Example parameter names include *myResourceGroup*, *myKey*, and *myVM*.</span></span>

<span data-ttu-id="964ef-182">első lépés hello van egy Azure Key Vault toostore toocreate a titkosítási kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="964ef-182">hello first step is toocreate an Azure Key Vault toostore your cryptographic keys.</span></span> <span data-ttu-id="964ef-183">Az Azure Key Vault tárolhatja a kulcsokat, a titkos kulcsok, vagy a jelszavak, amelyek lehetővé teszik toosecurely végrehajtja az alkalmazások és szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="964ef-183">Azure Key Vault can store keys, secrets, or passwords that allow you toosecurely implement them in your applications and services.</span></span> <span data-ttu-id="964ef-184">A virtuális lemez titkosításához használja a Key Vault toostore használt tooencrypt egy titkosítási kulcsot, illetve visszafejteni a virtuális lemezek.</span><span class="sxs-lookup"><span data-stu-id="964ef-184">For virtual disk encryption, you use Key Vault toostore a cryptographic key that is used tooencrypt or decrypt your virtual disks.</span></span>

<span data-ttu-id="964ef-185">Engedélyezi az Azure-előfizetése belül hello Azure Key Vault szolgáltatót [az szolgáltató regisztrálása](/cli/azure/provider#register) és hozzon létre egy erőforráscsoportot a [az csoport létrehozása](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="964ef-185">Enable hello Azure Key Vault provider within your Azure subscription with [az provider register](/cli/azure/provider#register) and create a resource group with [az group create](/cli/azure/group#create).</span></span> <span data-ttu-id="964ef-186">hello alábbi példa létrehoz egy erőforráscsoport neve *myResourceGroup* a hello `eastus` helye:</span><span class="sxs-lookup"><span data-stu-id="964ef-186">hello following example creates a resource group name *myResourceGroup* in hello `eastus` location:</span></span>

```azurecli
az provider register -n Microsoft.KeyVault
az group create --name myResourceGroup --location eastus
```

<span data-ttu-id="964ef-187">hello Azure Key Vault tartalmazó hello kriptográfiai kulcsokat és társított számítási erőforrások, például a tárolási és hello virtuális gépért kell lennie, hello ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="964ef-187">hello Azure Key Vault containing hello cryptographic keys and associated compute resources such as storage and hello VM itself must reside in hello same region.</span></span> <span data-ttu-id="964ef-188">Hozzon létre egy Azure Key Vault a [az keyvault létrehozása](/cli/azure/keyvault#create) , és engedélyezze a Key Vault hello lemeztitkosítás való használatra.</span><span class="sxs-lookup"><span data-stu-id="964ef-188">Create an Azure Key Vault with [az keyvault create](/cli/azure/keyvault#create) and enable hello Key Vault for use with disk encryption.</span></span> <span data-ttu-id="964ef-189">Adjon meg egy egyedi kulcstároló nevet *keyvault_name* az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="964ef-189">Specify a unique Key Vault name for *keyvault_name* as follows:</span></span>

```azurecli
keyvault_name=myUniqueKeyVaultName
az keyvault create \
    --name $keyvault_name \
    --resource-group myResourceGroup \
    --location eastus \
    --enabled-for-disk-encryption True
```

<span data-ttu-id="964ef-190">A szoftver vagy a biztonsági modell (HSM) védelmi kriptográfiai kulcsokat is tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="964ef-190">You can store cryptographic keys using software or Hardware Security Model (HSM) protection.</span></span> <span data-ttu-id="964ef-191">A Key Vault prémium HSM használata szükséges.</span><span class="sxs-lookup"><span data-stu-id="964ef-191">Using an HSM requires a premium Key Vault.</span></span> <span data-ttu-id="964ef-192">Van egy további költség nélkül toocreating szabványos Key Vault szoftveresen védett tároló helyett a Key Vault támogatás.</span><span class="sxs-lookup"><span data-stu-id="964ef-192">There is an additional cost toocreating a premium Key Vault rather than standard Key Vault that stores software-protected keys.</span></span> <span data-ttu-id="964ef-193">a prémium kulcstároló, megelőző lépés hello a hozzáadása toocreate `--sku Premium` toohello parancsot.</span><span class="sxs-lookup"><span data-stu-id="964ef-193">toocreate a premium Key Vault, in hello preceding step add `--sku Premium` toohello command.</span></span> <span data-ttu-id="964ef-194">hello alábbi példa szoftveresen védett azt a szabványos kulcstároló létrehozása óta.</span><span class="sxs-lookup"><span data-stu-id="964ef-194">hello following example uses software-protected keys since we created a standard Key Vault.</span></span>

<span data-ttu-id="964ef-195">Mindkét védelmi modellek hello Azure platformon kell toobe kap hozzáférést toorequest hello titkosítási kulcsok toodecrypt hello virtuális lemezek hello virtuális gép indításakor.</span><span class="sxs-lookup"><span data-stu-id="964ef-195">For both protection models, hello Azure platform needs toobe granted access toorequest hello cryptographic keys when hello VM boots toodecrypt hello virtual disks.</span></span> <span data-ttu-id="964ef-196">A kulcstároló, a titkosítási kulcs létrehozására [az keyvault kulcs létrehozása](/cli/azure/keyvault/key#create).</span><span class="sxs-lookup"><span data-stu-id="964ef-196">Create a cryptographic key in your Key Vault with [az keyvault key create](/cli/azure/keyvault/key#create).</span></span> <span data-ttu-id="964ef-197">hello alábbi példa létrehoz egy nevű kulcsot *SajátKulcs*:</span><span class="sxs-lookup"><span data-stu-id="964ef-197">hello following example creates a key named *myKey*:</span></span>

```azurecli
az keyvault key create --vault-name $keyvault_name --name myKey --protection software
```


## <a name="create-hello-azure-active-directory-service-principal"></a><span data-ttu-id="964ef-198">Hello Azure Active Directory szolgáltatás egyszerű létrehozása</span><span class="sxs-lookup"><span data-stu-id="964ef-198">Create hello Azure Active Directory service principal</span></span>
<span data-ttu-id="964ef-199">Amikor a virtuális lemezek vannak titkosított vagy visszafejtett, megadhatja egy toohandle hello hitelesítése és a Key Vault a titkosítási kulcsok cseréjét.</span><span class="sxs-lookup"><span data-stu-id="964ef-199">When virtual disks are encrypted or decrypted, you specify an account toohandle hello authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="964ef-200">Ezt a fiókot, egy Azure Active Directory szolgáltatás egyszerű, lehetővé teszi, hogy hello Azure platformon toorequest hello megfelelő titkosítási kulcsok hello virtuális gép nevében.</span><span class="sxs-lookup"><span data-stu-id="964ef-200">This account, an Azure Active Directory service principal, allows hello Azure platform toorequest hello appropriate cryptographic keys on behalf of hello VM.</span></span> <span data-ttu-id="964ef-201">Azure Active Directory alapértelmezett példányán érhető előfizetését, bár számos szervezet Azure Active Directory-könyvtárak dedikált.</span><span class="sxs-lookup"><span data-stu-id="964ef-201">A default Azure Active Directory instance is available in your subscription, though many organizations have dedicated Azure Active Directory directories.</span></span>

<span data-ttu-id="964ef-202">Az Azure Active Directoryval az egyszerű szolgáltatás létrehozása [az ad sp létrehozása-az-rbac](/cli/azure/ad/sp#create-for-rbac).</span><span class="sxs-lookup"><span data-stu-id="964ef-202">Create a service principal using Azure Active Directory with [az ad sp create-for-rbac](/cli/azure/ad/sp#create-for-rbac).</span></span> <span data-ttu-id="964ef-203">a következő példa hello beolvassa hello értékeinek hello szolgáltatás egyszerű azonosító és jelszó használható újabb parancsok:</span><span class="sxs-lookup"><span data-stu-id="964ef-203">hello following example reads in hello values for hello service principal Id and password for use in later commands:</span></span>

```azurecli
read sp_id sp_password <<< $(az ad sp create-for-rbac --query [appId,password] -o tsv)
```

<span data-ttu-id="964ef-204">hello jelszó csak akkor jelenik meg, amikor hello szolgáltatás egyszerű létrehozása.</span><span class="sxs-lookup"><span data-stu-id="964ef-204">hello password is only displayed when you create hello service principal.</span></span> <span data-ttu-id="964ef-205">Ha szükséges, megtekintése és rekord hello jelszó (`echo $sp_password`).</span><span class="sxs-lookup"><span data-stu-id="964ef-205">If desired, view and record hello password (`echo $sp_password`).</span></span> <span data-ttu-id="964ef-206">A szolgáltatás elsődleges listázhatja [az sp hirdetéslista](/cli/azure/ad/sp#list) és egyszerű, az adott szolgáltatás további információkat tekinthet [az ad sp megjelenítése](/cli/azure/ad/sp#show).</span><span class="sxs-lookup"><span data-stu-id="964ef-206">You can list your service principals with [az ad sp list](/cli/azure/ad/sp#list) and view additional information about a specific service principal with [az ad sp show](/cli/azure/ad/sp#show).</span></span>

<span data-ttu-id="964ef-207">toosuccessfully vagy titkosítására vagy visszafejtésére virtuális lemezek, a Key Vault tárolt titkosítási kulcs hello engedélyeinek beállítása toopermit hello Azure Active Directory szolgáltatás egyszerű tooread hello kulcsok kell lennie.</span><span class="sxs-lookup"><span data-stu-id="964ef-207">toosuccessfully encrypt or decrypt virtual disks, permissions on hello cryptographic key stored in Key Vault must be set toopermit hello Azure Active Directory service principal tooread hello keys.</span></span> <span data-ttu-id="964ef-208">A kulcstároló, az engedélyeket [az keyvault-szabály beállítása](/cli/azure/keyvault#set-policy).</span><span class="sxs-lookup"><span data-stu-id="964ef-208">Set permissions on your Key Vault with [az keyvault set-policy](/cli/azure/keyvault#set-policy).</span></span> <span data-ttu-id="964ef-209">A következő példa hello hello résztvevő-azonosító van megadva a fenti parancs hello:</span><span class="sxs-lookup"><span data-stu-id="964ef-209">In hello following example, hello service principal ID is supplied from hello preceding command:</span></span>

```azurecli
az keyvault set-policy --name $keyvault_name --spn $sp_id \
  --key-permissions wrapKey \
  --secret-permissions set
```


## <a name="create-virtual-machine"></a><span data-ttu-id="964ef-210">Virtuális gép létrehozása</span><span class="sxs-lookup"><span data-stu-id="964ef-210">Create virtual machine</span></span>
<span data-ttu-id="964ef-211">tooactually titkosítani az egyes virtuális lemezek, lehetővé teszi, hogy a virtuális gép létrehozása és a hozzá adatlemezt.</span><span class="sxs-lookup"><span data-stu-id="964ef-211">tooactually encrypt some virtual disks, lets create a VM and add a data disk.</span></span> <span data-ttu-id="964ef-212">Hozzon létre egy virtuális gép tooencrypt rendelkező [az virtuális gép létrehozása](/cli/azure/vm#create) és 5 Gb adatlemezt csatolni.</span><span class="sxs-lookup"><span data-stu-id="964ef-212">Create a VM tooencrypt with [az vm create](/cli/azure/vm#create) and attach a 5Gb data disk.</span></span> <span data-ttu-id="964ef-213">Csak bizonyos piactéren elérhető rendszerkép lemeztitkosítás támogatja.</span><span class="sxs-lookup"><span data-stu-id="964ef-213">Only certain marketplace images support disk encryption.</span></span> <span data-ttu-id="964ef-214">hello alábbi példakód létrehozza a virtuális gépek nevű *myVM* használatával egy **CentOS 7.2n** lemezképet:</span><span class="sxs-lookup"><span data-stu-id="964ef-214">hello following example creates a VM named *myVM* using a **CentOS 7.2n** image:</span></span>

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM \
    --image OpenLogic:CentOS:7.2n:7.2.20160629 \
    --admin-username azureuser \
    --generate-ssh-keys \
    --data-disk-sizes-gb 5
```

<span data-ttu-id="964ef-215">SSH tooyour VM használatával hello `publicIpAddress` hello megelőző parancs kimenetét hello látható.</span><span class="sxs-lookup"><span data-stu-id="964ef-215">SSH tooyour VM using hello `publicIpAddress` shown in hello output of hello preceding command.</span></span> <span data-ttu-id="964ef-216">Hozzon létre egy partíciót és filesystem, majd hello adatlemez csatlakoztatása.</span><span class="sxs-lookup"><span data-stu-id="964ef-216">Create a partition and filesystem, then mount hello data disk.</span></span> <span data-ttu-id="964ef-217">További információkért lásd: [tooa Linux virtuális gép toomount hello új lemez csatlakozás](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span><span class="sxs-lookup"><span data-stu-id="964ef-217">For more information, see [Connect tooa Linux VM toomount hello new disk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#connect-to-the-linux-vm-to-mount-the-new-disk).</span></span> <span data-ttu-id="964ef-218">Zárja be az SSH-munkamenetet.</span><span class="sxs-lookup"><span data-stu-id="964ef-218">Close your SSH session.</span></span>


## <a name="encrypt-virtual-machine"></a><span data-ttu-id="964ef-219">Virtuális gép titkosítása</span><span class="sxs-lookup"><span data-stu-id="964ef-219">Encrypt virtual machine</span></span>
<span data-ttu-id="964ef-220">tooencrypt hello virtuális lemezeket, akkor egyesítik összes hello az előző összetevők működését:</span><span class="sxs-lookup"><span data-stu-id="964ef-220">tooencrypt hello virtual disks, you bring together all hello previous components:</span></span>

1. <span data-ttu-id="964ef-221">Adja meg a hello Azure Active Directory szolgáltatás egyszerű és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="964ef-221">Specify hello Azure Active Directory service principal and password.</span></span>
2. <span data-ttu-id="964ef-222">Adja meg a hello Key Vault toostore hello metaadatait a titkosított lemezek.</span><span class="sxs-lookup"><span data-stu-id="964ef-222">Specify hello Key Vault toostore hello metadata for your encrypted disks.</span></span>
3. <span data-ttu-id="964ef-223">Adja meg a hello titkosítási kulcsok toouse hello tényleges titkosításához és visszafejtéséhez.</span><span class="sxs-lookup"><span data-stu-id="964ef-223">Specify hello cryptographic keys toouse for hello actual encryption and decryption.</span></span>
4. <span data-ttu-id="964ef-224">Adja meg, hogy tooencrypt hello operációsrendszer-lemez, hello adatlemezek vagy az összes.</span><span class="sxs-lookup"><span data-stu-id="964ef-224">Specify whether you want tooencrypt hello OS disk, hello data disks, or all.</span></span>

<span data-ttu-id="964ef-225">Az a virtuális gép titkosítása [az vm-titkosítás engedélyezése](/cli/azure/vm/encryption#enable).</span><span class="sxs-lookup"><span data-stu-id="964ef-225">Encrypt your VM with [az vm encryption enable](/cli/azure/vm/encryption#enable).</span></span> <span data-ttu-id="964ef-226">hello alábbi példában hello `$sp_id` és `$sp_password` változók az előző hello `ad sp create-for-rbac` parancs:</span><span class="sxs-lookup"><span data-stu-id="964ef-226">hello following example uses hello `$sp_id` and `$sp_password` variables from hello preceding `ad sp create-for-rbac` command:</span></span>

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

<span data-ttu-id="964ef-227">Hello lemez titkosítási folyamat toocomplete némi időt vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="964ef-227">It takes some time for hello disk encryption process toocomplete.</span></span> <span data-ttu-id="964ef-228">Hello folyamat hello állapotának figyelése [az vm titkosítási megjelenítése](/cli/azure/vm/encryption#show):</span><span class="sxs-lookup"><span data-stu-id="964ef-228">Monitor hello status of hello process with [az vm encryption show](/cli/azure/vm/encryption#show):</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="964ef-229">hello van a hasonló toohello következő csonkolt példa a kimenetre:</span><span class="sxs-lookup"><span data-stu-id="964ef-229">hello output is similar toohello following truncated example:</span></span>

```json
[
  "dataDisk": "EncryptionInProgress",
  "osDisk": "EncryptionInProgress",
]
```

<span data-ttu-id="964ef-230">Várja meg, amíg hello állapot hello OS lemez jelentések **VMRestartPending**, majd indítsa újra a virtuális Gépet a [az virtuális gép újraindítása](/cli/azure/vm#restart):</span><span class="sxs-lookup"><span data-stu-id="964ef-230">Wait until hello status for hello OS disk reports **VMRestartPending**, then restart your VM with [az vm restart](/cli/azure/vm#restart):</span></span>

```azurecli
az vm restart --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="964ef-231">hello lemez titkosítási folyamat akkor fejeződik be hello rendszerindítási folyamat során, így Várjon néhány percet, majd újra titkosítási hello állapotának ellenőrzése **az vm titkosítási megjelenítése**:</span><span class="sxs-lookup"><span data-stu-id="964ef-231">hello disk encryption process is finalized during hello boot process, so wait a few minutes before checking hello status of encryption again with **az vm encryption show**:</span></span>

```azurecli
az vm encryption show --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="964ef-232">hello állapota most jelentse hello operációsrendszer-lemez, mind az adatok lemezre **titkosított**.</span><span class="sxs-lookup"><span data-stu-id="964ef-232">hello status should now report both hello OS disk and data disk as **Encrypted**.</span></span>


## <a name="add-additional-data-disks"></a><span data-ttu-id="964ef-233">További adatok lemezek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="964ef-233">Add additional data disks</span></span>
<span data-ttu-id="964ef-234">Az adatlemezek titkosított, ha később hozzáadhat további virtuális lemezek tooyour VM és is titkosítani.</span><span class="sxs-lookup"><span data-stu-id="964ef-234">Once you have encrypted your data disks, you can later add additional virtual disks tooyour VM and also encrypt them.</span></span> <span data-ttu-id="964ef-235">Például lehetővé teszi, hogy az alábbiak szerint adja hozzá a második virtuális lemez tooyour VM:</span><span class="sxs-lookup"><span data-stu-id="964ef-235">For example, lets add a second virtual disk tooyour VM as follows:</span></span>

```azurecli
az vm disk attach-new --resource-group myResourceGroup --vm-name myVM --size-in-gb 5
```

<span data-ttu-id="964ef-236">Futtassa újra hello parancs tooencrypt hello virtuális lemezek az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="964ef-236">Re-run hello command tooencrypt hello virtual disks as follows:</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="964ef-237">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="964ef-237">Next steps</span></span>
* <span data-ttu-id="964ef-238">Az Azure Key Vault, beleértve a titkosítási kulcsok és a tárolók kezelésével kapcsolatos további információért lásd: [kezelése Key Vault parancssori felület használatával](../../key-vault/key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="964ef-238">For more information about managing Azure Key Vault, including deleting cryptographic keys and vaults, see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md).</span></span>
* <span data-ttu-id="964ef-239">További információ a lemez titkosítása például egy titkosított egyéni VM tooupload tooAzure, előkészítése: [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="964ef-239">For more information about disk encryption, such as preparing an encrypted custom VM tooupload tooAzure, see [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span></span>
