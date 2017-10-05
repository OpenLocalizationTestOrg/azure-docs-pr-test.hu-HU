---
title: "A Linux virtuális gép és az Azure CLI 1.0 lemezeket titkosítása |} Microsoft Docs"
description: "A Linux virtuális gépet az Azure CLI 1.0 és a Resource Manager üzembe helyezési modellel lemezzel titkosítása"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 03/06/2017
ms.author: iainfou
ms.openlocfilehash: b436f2d43c41000f4385889edb3fa3983d4a8c66
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="encrypt-disks-on-a-linux-vm-using-the-azure-cli-10"></a><span data-ttu-id="256c1-103">A Linux virtuális gépet az Azure CLI 1.0 lemezeket titkosítása</span><span class="sxs-lookup"><span data-stu-id="256c1-103">Encrypt disks on a Linux VM using the Azure CLI 1.0</span></span>
<span data-ttu-id="256c1-104">A bővített virtuális gép (VM) biztonsági és megfelelőségi az Azure-ban virtuális lemezek aktívan titkosíthatók.</span><span class="sxs-lookup"><span data-stu-id="256c1-104">For enhanced virtual machine (VM) security and compliance, virtual disks in Azure can be encrypted at rest.</span></span> <span data-ttu-id="256c1-105">Lemezek titkosítása egy Azure Key Vault a titkosított titkosítási kulcsok használatával.</span><span class="sxs-lookup"><span data-stu-id="256c1-105">Disks are encrypted using cryptographic keys that are secured in an Azure Key Vault.</span></span> <span data-ttu-id="256c1-106">Szabályozhatja a titkosítási kulcsokat, és a használatukat naplózhatók.</span><span class="sxs-lookup"><span data-stu-id="256c1-106">You control these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="256c1-107">Ez a cikk részletesen titkosítása a Linux virtuális gépet az Azure CLI 1.0 és a Resource Manager üzembe helyezési modellel virtuális lemezzel.</span><span class="sxs-lookup"><span data-stu-id="256c1-107">This article details how to encrypt virtual disks on a Linux VM using the Azure CLI 1.0 and the Resource Manager deployment model.</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="256c1-108">A feladat befejezéséhez használható CLI-verziók</span><span class="sxs-lookup"><span data-stu-id="256c1-108">CLI versions to complete the task</span></span>
<span data-ttu-id="256c1-109">A következő CLI-verziók egyikével elvégezheti a feladatot:</span><span class="sxs-lookup"><span data-stu-id="256c1-109">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="256c1-110">[Az Azure CLI 1.0](#quick-commands) – a parancssori felületen a klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="256c1-110">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="256c1-111">[Azure CLI 2.0](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – a Resource Management üzemi modellhez tartozó parancssori felületek következő generációját képviseli.</span><span class="sxs-lookup"><span data-stu-id="256c1-111">[Azure CLI 2.0](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>

## <a name="quick-commands"></a><span data-ttu-id="256c1-112">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="256c1-112">Quick commands</span></span>
<span data-ttu-id="256c1-113">Ha szeretné gyorsan a feladatnak a, a következő szakasz részleteit a következő parancsokat a virtuális Gépen lévő virtuális lemezek titkosításához.</span><span class="sxs-lookup"><span data-stu-id="256c1-113">If you need to quickly accomplish the task, the following section details the base commands to encrypt virtual disks on your VM.</span></span> <span data-ttu-id="256c1-114">Részletes információkat és a környezetben az egyes lépések a dokumentum többi részén található [itt indítása](#overview-of-disk-encryption).</span><span class="sxs-lookup"><span data-stu-id="256c1-114">More detailed information and context for each step can be found the rest of the document, [starting here](#overview-of-disk-encryption).</span></span>

<span data-ttu-id="256c1-115">Van szüksége a [Azure CLI legújabb 1.0](../../xplat-cli-install.md) telepítve, és bejelentkezett a Resource Manager módra használatával az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="256c1-115">You need the [latest Azure CLI 1.0](../../xplat-cli-install.md) installed and logged in using the Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="256c1-116">A következő példákban cserélje le a saját értékeit példa paraméterek nevei.</span><span class="sxs-lookup"><span data-stu-id="256c1-116">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="256c1-117">Példa paraméter nevek a következők `myResourceGroup`, `myKeyVault`, és `myVM`.</span><span class="sxs-lookup"><span data-stu-id="256c1-117">Example parameter names include `myResourceGroup`, `myKeyVault`, and `myVM`.</span></span>

<span data-ttu-id="256c1-118">Először engedélyezése az Azure Key Vault-szolgáltató az Azure-előfizetéshez belül, és hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="256c1-118">First, enable the Azure Key Vault provider within your Azure subscription and create a resource group.</span></span> <span data-ttu-id="256c1-119">Az alábbi példa létrehoz egy erőforráscsoport neve `myResourceGroup` a a `WestUS` helye:</span><span class="sxs-lookup"><span data-stu-id="256c1-119">The following example creates a resource group name `myResourceGroup` in the `WestUS` location:</span></span>

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

<span data-ttu-id="256c1-120">Hozzon létre egy Azure-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="256c1-120">Create an Azure Key Vault.</span></span> <span data-ttu-id="256c1-121">Az alábbi példakód létrehozza a kulcstároló nevű `myKeyVault`:</span><span class="sxs-lookup"><span data-stu-id="256c1-121">The following example creates a Key Vault named `myKeyVault`:</span></span>

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

<span data-ttu-id="256c1-122">Titkosítási kulcs létrehozására a kulcstároló, majd engedélyezze a lemez titkosításához.</span><span class="sxs-lookup"><span data-stu-id="256c1-122">Create a cryptographic key in your Key Vault and enable it for disk encryption.</span></span> <span data-ttu-id="256c1-123">Az alábbi példakód létrehozza nevű kulcs `myKey`:</span><span class="sxs-lookup"><span data-stu-id="256c1-123">The following example creates a key named `myKey`:</span></span>

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

<span data-ttu-id="256c1-124">Hozzon létre egy Azure Active Directory használatával kezeli a hitelesítési és titkosítási kulcsok a Key Vault cseréjét végpontot.</span><span class="sxs-lookup"><span data-stu-id="256c1-124">Create an endpoint using Azure Active Directory for handling the authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="256c1-125">A `--home-page` és `--identifier-uris` tényleges irányítható címnek nem kell.</span><span class="sxs-lookup"><span data-stu-id="256c1-125">The `--home-page` and `--identifier-uris` do not need to be actual routable address.</span></span> <span data-ttu-id="256c1-126">A legmagasabb szintű biztonság jelszavak helyett ügyfélkulcs kell használni.</span><span class="sxs-lookup"><span data-stu-id="256c1-126">For the highest level of security, client secrets should be used instead of passwords.</span></span> <span data-ttu-id="256c1-127">Az Azure parancssori felület jelenleg nem hozható létre ügyfélkulcs.</span><span class="sxs-lookup"><span data-stu-id="256c1-127">The Azure CLI cannot currently generate client secrets.</span></span> <span data-ttu-id="256c1-128">Titkos ügyfélkulccsal csak az Azure portálon hozható létre.</span><span class="sxs-lookup"><span data-stu-id="256c1-128">Client secrets can only be generated in the Azure portal.</span></span> <span data-ttu-id="256c1-129">Az alábbi példa létrehoz egy Azure Active Directory végpontján nevű `myAADApp` és egy jelszót használja `myPassword`.</span><span class="sxs-lookup"><span data-stu-id="256c1-129">The following example creates an Azure Active Directory endpoint named `myAADApp` and uses a password of `myPassword`.</span></span> <span data-ttu-id="256c1-130">Adja meg a saját jelszavát a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="256c1-130">Specify your own password as follows:</span></span>

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

<span data-ttu-id="256c1-131">Megjegyzés: a `applicationId` az előző parancs kimenetében megjelennek.</span><span class="sxs-lookup"><span data-stu-id="256c1-131">Note the `applicationId` shown in the output from the preceding command.</span></span> <span data-ttu-id="256c1-132">Az alkalmazás azonosítója szerepel a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="256c1-132">This application ID is used in the following steps:</span></span>

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

<span data-ttu-id="256c1-133">Adatlemez hozzáadása egy meglévő virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="256c1-133">Add a data disk to an existing VM.</span></span> <span data-ttu-id="256c1-134">A következő példa adatlemezt ad egy nevű virtuális gép `myVM`:</span><span class="sxs-lookup"><span data-stu-id="256c1-134">The following example adds a data disk to a VM named `myVM`:</span></span>

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

<span data-ttu-id="256c1-135">Tekintse át a Key Vault és a létrehozott kulcsot.</span><span class="sxs-lookup"><span data-stu-id="256c1-135">Review the details for your Key Vault and the key you created.</span></span> <span data-ttu-id="256c1-136">A Key Vault azonosító URI és kulcs van szüksége az utolsó lépésben URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="256c1-136">You need the Key Vault ID, URI, and key URL in the final step.</span></span> <span data-ttu-id="256c1-137">A következő példa a részletek ellenőrzi, hogy a Key vault nevű `myKeyVault` és nevű kulcs `myKey`:</span><span class="sxs-lookup"><span data-stu-id="256c1-137">The following example reviews the details for a Key Vault named `myKeyVault` and key named `myKey`:</span></span>

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

<span data-ttu-id="256c1-138">Titkosítása következő, a lemezek megadása egész saját paraméterek nevei:</span><span class="sxs-lookup"><span data-stu-id="256c1-138">Encrypt your disks as follows, entering your own parameter names throughout:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

<span data-ttu-id="256c1-139">Az Azure parancssori felület nem biztosít részletes hibák a titkosítási folyamat során.</span><span class="sxs-lookup"><span data-stu-id="256c1-139">The Azure CLI doesn't provide verbose errors during the encryption process.</span></span> <span data-ttu-id="256c1-140">További hibaelhárítási információért tekintse át `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`.</span><span class="sxs-lookup"><span data-stu-id="256c1-140">For additional troubleshooting information, review `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`.</span></span> <span data-ttu-id="256c1-141">Az előző parancs sok változók rendelkezik, és amelyekhez nem tarozik sok jelzi, miért a sikertelen, a teljes parancs példa lehet az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="256c1-141">As the preceding command has many variables and you may not get much indication as to why the process fails, a complete command example would be as follows:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

<span data-ttu-id="256c1-142">Végül tekintse át a titkosítási állapot újra megerősítéséhez, hogy a virtuális lemezek most lett titkosítva.</span><span class="sxs-lookup"><span data-stu-id="256c1-142">Finally, review the encryption status again to confirm that your virtual disks have now been encrypted.</span></span> <span data-ttu-id="256c1-143">A következő példa egy nevű virtuális gép állapotát ellenőrzi `myVM` a a `myResourceGroup` erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="256c1-143">The following example checks the status of a VM named `myVM` in the `myResourceGroup` resource group:</span></span>

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a><span data-ttu-id="256c1-144">Lemeztitkosítás áttekintése</span><span class="sxs-lookup"><span data-stu-id="256c1-144">Overview of disk encryption</span></span>
<span data-ttu-id="256c1-145">A Linux virtuális gépek virtuális lemezzel titkosított rest az [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span><span class="sxs-lookup"><span data-stu-id="256c1-145">Virtual disks on Linux VMs are encrypted at rest using [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span></span> <span data-ttu-id="256c1-146">Nincs nem kell fizetni az Azure-ban virtuális lemezek titkosítására.</span><span class="sxs-lookup"><span data-stu-id="256c1-146">There is no charge for encrypting virtual disks in Azure.</span></span> <span data-ttu-id="256c1-147">Titkosítási kulcsok Azure Key Vault szoftver-védelemmel vannak tárolva, vagy importálhat vagy a tanúsított FIPS 140-2 2. szint szabványok hardveres biztonsági modulokkal (HSM) a kulcsok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="256c1-147">Cryptographic keys are stored in Azure Key Vault using software-protection, or you can import or generate your keys in Hardware Security Modules (HSMs) certified to FIPS 140-2 level 2 standards.</span></span> <span data-ttu-id="256c1-148">A titkosítási kulcsokat a felügyeletet, és naplózhatja a használatukat.</span><span class="sxs-lookup"><span data-stu-id="256c1-148">You retain control of these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="256c1-149">Ezek a titkosítási kulcsok titkosítására és visszafejtésére a virtuális Géphez csatolt virtuális lemezek segítségével.</span><span class="sxs-lookup"><span data-stu-id="256c1-149">These cryptographic keys are used to encrypt and decrypt virtual disks attached to your VM.</span></span> <span data-ttu-id="256c1-150">Egy Azure Active Directory végpontján lehetővé teszi a biztonságos kiadása a titkosítási kulcsokat, a virtuális gépek vannak kapcsolva, és ki.</span><span class="sxs-lookup"><span data-stu-id="256c1-150">An Azure Active Directory endpoint provides a secure mechanism for issuing these cryptographic keys as VMs are powered on and off.</span></span>

<span data-ttu-id="256c1-151">A virtuális gépek titkosításához a folyamat a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="256c1-151">The process for encrypting a VM is as follows:</span></span>

1. <span data-ttu-id="256c1-152">Hozzon létre egy Azure Key Vault egy titkosítási kulcsot.</span><span class="sxs-lookup"><span data-stu-id="256c1-152">Create a cryptographic key in an Azure Key Vault.</span></span>
2. <span data-ttu-id="256c1-153">Állítsa be a titkosítási kulccsal, hogy a lemezek titkosítására használható.</span><span class="sxs-lookup"><span data-stu-id="256c1-153">Configure the cryptographic key to be usable for encrypting disks.</span></span>
3. <span data-ttu-id="256c1-154">Az Azure Key Vault beolvasni a titkosítási kulcsot, hozzon létre egy végpontot, az Azure Active Directoryval a megfelelő engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="256c1-154">To read the cryptographic key from the Azure Key Vault, create an endpoint using Azure Active Directory with the appropriate permissions.</span></span>
4. <span data-ttu-id="256c1-155">Adja ki a parancsot, a virtuális lemezek, Azure Active Directory végpontján és egyéb használandó megfelelő titkosítási kulcs titkosításához.</span><span class="sxs-lookup"><span data-stu-id="256c1-155">Issue the command to encrypt your virtual disks, specifying the Azure Active Directory endpoint and appropriate cryptographic key to be used.</span></span>
5. <span data-ttu-id="256c1-156">Az Azure Active Directory végpontján kér az Azure Key Vault a szükséges titkosítási kulcs.</span><span class="sxs-lookup"><span data-stu-id="256c1-156">The Azure Active Directory endpoint requests the required cryptographic key from Azure Key Vault.</span></span>
6. <span data-ttu-id="256c1-157">A virtuális lemezek vannak titkosítva, a megadott titkosítási kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="256c1-157">The virtual disks are encrypted using the provided cryptographic key.</span></span>

## <a name="supporting-services-and-encryption-process"></a><span data-ttu-id="256c1-158">Szolgáltatások és a titkosítási folyamat támogatása</span><span class="sxs-lookup"><span data-stu-id="256c1-158">Supporting services and encryption process</span></span>
<span data-ttu-id="256c1-159">Adatok titkosítása a következő további összetevők támaszkodik:</span><span class="sxs-lookup"><span data-stu-id="256c1-159">Disk encryption relies on the following additional components:</span></span>

* <span data-ttu-id="256c1-160">**Az Azure Key Vault** – a lemez titkosítási/visszafejtési folyamathoz használt titkosítási kulcsok és titkos biztosításához használt.</span><span class="sxs-lookup"><span data-stu-id="256c1-160">**Azure Key Vault** - used to safeguard cryptographic keys and secrets used for the disk encryption/decryption process.</span></span>
  * <span data-ttu-id="256c1-161">Ha van ilyen, használhat egy meglévő Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="256c1-161">If one exists, you can use an existing Azure Key Vault.</span></span> <span data-ttu-id="256c1-162">Nem jelölt ki a kulcstároló, a lemezek titkosító rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="256c1-162">You do not have to dedicate a Key Vault to encrypting disks.</span></span>
  * <span data-ttu-id="256c1-163">Külön felügyeleti határokat és a kulcs látható, a dedikált kulcstároló is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="256c1-163">To separate administrative boundaries and key visibility, you can create a dedicated Key Vault.</span></span>
* <span data-ttu-id="256c1-164">**Az Azure Active Directory** -kezeli a szükséges titkosítási kulcsokat és a kért műveletek hitelesítési biztonságos cseréjét.</span><span class="sxs-lookup"><span data-stu-id="256c1-164">**Azure Active Directory** - handles the secure exchanging of required cryptographic keys and authentication for requested actions.</span></span>
  * <span data-ttu-id="256c1-165">Az alkalmazás elhelyezésére szolgáló használhat meglévő Azure Active Directory-példány általában.</span><span class="sxs-lookup"><span data-stu-id="256c1-165">You can typically use an existing Azure Active Directory instance for housing your application.</span></span>
  * <span data-ttu-id="256c1-166">Az alkalmazás több végpont le tudja kérni és a megfelelő titkosítási kulcsok beszerzése kiadott Kulcstárolónak, valamint a virtuális gép számára.</span><span class="sxs-lookup"><span data-stu-id="256c1-166">The application is more of an endpoint for the Key Vault and Virtual Machine services to request and get issued the appropriate cryptographic keys.</span></span> <span data-ttu-id="256c1-167">Nem fejleszt, amely az Azure Active Directory tényleges kérelmet.</span><span class="sxs-lookup"><span data-stu-id="256c1-167">You are not developing an actual application that integrates with Azure Active Directory.</span></span>

## <a name="requirements-and-limitations"></a><span data-ttu-id="256c1-168">Követelmények és korlátozások</span><span class="sxs-lookup"><span data-stu-id="256c1-168">Requirements and limitations</span></span>
<span data-ttu-id="256c1-169">Támogatott esetek és lemez titkosítására vonatkozó követelményekkel kapcsolatos:</span><span class="sxs-lookup"><span data-stu-id="256c1-169">Supported scenarios and requirements for disk encryption:</span></span>

* <span data-ttu-id="256c1-170">A következő Linux-kiszolgálóra SKU - Ubuntu, CentOS, SUSE és SUSE Linux Enterprise Server (SLES) és Red Hat Enterprise Linux.</span><span class="sxs-lookup"><span data-stu-id="256c1-170">The following Linux server SKUs - Ubuntu, CentOS, SUSE and SUSE Linux Enterprise Server (SLES), and Red Hat Enterprise Linux.</span></span>
* <span data-ttu-id="256c1-171">Az azonos Azure-régió, és az előfizetés összes erőforrások (például a Key Vault, a tárfiók és a virtuális gép) kell lennie.</span><span class="sxs-lookup"><span data-stu-id="256c1-171">All resources (such as Key Vault, Storage account, and VM) must be in the same Azure region and subscription.</span></span>
* <span data-ttu-id="256c1-172">Standard A, a D, a DS-ben, a G és a GS adatsorozat virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="256c1-172">Standard A, D, DS, G, and GS series VMs.</span></span>

<span data-ttu-id="256c1-173">Lemeztitkosítás jelenleg nem támogatott a következő esetekben:</span><span class="sxs-lookup"><span data-stu-id="256c1-173">Disk encryption is not currently supported in the following scenarios:</span></span>

* <span data-ttu-id="256c1-174">Az alapszintű csomag virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="256c1-174">Basic tier VMs.</span></span>
* <span data-ttu-id="256c1-175">A virtuális gépek létrehozása a klasszikus telepítési modell használatával.</span><span class="sxs-lookup"><span data-stu-id="256c1-175">VMs created using the Classic deployment model.</span></span>
* <span data-ttu-id="256c1-176">Az operációs rendszer lemeztitkosítás Linux virtuális gépeken letiltása.</span><span class="sxs-lookup"><span data-stu-id="256c1-176">Disabling OS disk encryption on Linux VMs.</span></span>
* <span data-ttu-id="256c1-177">A titkosítási kulcsok egy már titkosított Linux virtuális gép frissítése.</span><span class="sxs-lookup"><span data-stu-id="256c1-177">Updating the cryptographic keys on an already encrypted Linux VM.</span></span>

## <a name="create-the-azure-key-vault-and-keys"></a><span data-ttu-id="256c1-178">Az Azure Key Vault és a kulcsok létrehozása</span><span class="sxs-lookup"><span data-stu-id="256c1-178">Create the Azure Key Vault and keys</span></span>
<span data-ttu-id="256c1-179">Ez az útmutató többi végrehajtásához, rendelkeznie kell a [Azure CLI legújabb 1.0](../../xplat-cli-install.md) telepítve, és bejelentkezett a Resource Manager módra használatával az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="256c1-179">To complete the remainder of this guide, you need the [latest Azure CLI 1.0](../../xplat-cli-install.md) installed and logged in using the Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="256c1-180">Egész a parancspéldákban cserélje le minden példa paraméter a saját nevét, helyét és kulcsértékek.</span><span class="sxs-lookup"><span data-stu-id="256c1-180">Throughout the command examples, replace all example parameters with your own names, location, and key values.</span></span> <span data-ttu-id="256c1-181">Az alábbi példák a szabályt használ `myResourceGroup`, `myKeyVault`, `myAADApp`stb.</span><span class="sxs-lookup"><span data-stu-id="256c1-181">The following examples use a convention of `myResourceGroup`, `myKeyVault`, `myAADApp`, etc.</span></span>

<span data-ttu-id="256c1-182">Az első lépés, ha az Azure Key Vault a kriptográfiai kulcsok tárolásához.</span><span class="sxs-lookup"><span data-stu-id="256c1-182">The first step is to create an Azure Key Vault to store your cryptographic keys.</span></span> <span data-ttu-id="256c1-183">Az Azure Key Vault kulcsok, a titkos kulcsok és a jelszót, amely engedélyezi, hogy biztonságosan végrehajtja az alkalmazások és szolgáltatások a tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="256c1-183">Azure Key Vault can store keys, secrets, or passwords that allow you to securely implement them in your applications and services.</span></span> <span data-ttu-id="256c1-184">A virtuális lemez titkosításához segítségével Key Vault titkosítására vagy visszafejtésére a virtuális lemezek használt kriptográfiai kulcs tárolása.</span><span class="sxs-lookup"><span data-stu-id="256c1-184">For virtual disk encryption, you use Key Vault to store a cryptographic key that is used to encrypt or decrypt your virtual disks.</span></span>

<span data-ttu-id="256c1-185">Engedélyezze az Azure Key Vault-szolgáltató az Azure-előfizetése, majd hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="256c1-185">Enable the Azure Key Vault provider in your Azure subscription, then create a resource group.</span></span> <span data-ttu-id="256c1-186">Az alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a a `WestUS` helye:</span><span class="sxs-lookup"><span data-stu-id="256c1-186">The following example creates a resource group named `myResourceGroup` in the `WestUS` location:</span></span>

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

<span data-ttu-id="256c1-187">Az Azure Key Vault a kriptográfiai kulcsokat és tartozó számítási erőforrásokat, például a tároló és a virtuális gépért tartalmazó ugyanabban a régióban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="256c1-187">The Azure Key Vault containing the cryptographic keys and associated compute resources such as storage and the VM itself must reside in the same region.</span></span> <span data-ttu-id="256c1-188">Az alábbi példa létrehoz egy Azure Key Vault nevű `myKeyVault`:</span><span class="sxs-lookup"><span data-stu-id="256c1-188">The following example creates an Azure Key Vault named `myKeyVault`:</span></span>

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

<span data-ttu-id="256c1-189">A szoftver vagy a biztonsági modell (HSM) védelmi kriptográfiai kulcsokat is tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="256c1-189">You can store cryptographic keys using software or Hardware Security Model (HSM) protection.</span></span> <span data-ttu-id="256c1-190">A Key Vault prémium HSM használata szükséges.</span><span class="sxs-lookup"><span data-stu-id="256c1-190">Using an HSM requires a premium Key Vault.</span></span> <span data-ttu-id="256c1-191">Nincs a prémium szabványos Key Vault szoftveresen védett tároló helyett kulcstároló létrehozásához további költségek.</span><span class="sxs-lookup"><span data-stu-id="256c1-191">There is an additional cost to creating a premium Key Vault rather than standard Key Vault that stores software-protected keys.</span></span> <span data-ttu-id="256c1-192">A prémium kulcstároló létrehozásához az előző lépésben hozzáadása `--sku Premium` a parancshoz.</span><span class="sxs-lookup"><span data-stu-id="256c1-192">To create a premium Key Vault, in the preceding step add `--sku Premium` to the command.</span></span> <span data-ttu-id="256c1-193">Az alábbi példában szoftveresen védett azt a szabványos kulcstároló létrehozása óta.</span><span class="sxs-lookup"><span data-stu-id="256c1-193">The following example uses software-protected keys since we created a standard Key Vault.</span></span>

<span data-ttu-id="256c1-194">Mindkét védelmi modellek esetében az Azure platformon kell hozzáférést kérni a titkosítási kulcsokat a virtuális lemezek visszafejtése a virtuális gép indításakor.</span><span class="sxs-lookup"><span data-stu-id="256c1-194">For both protection models, the Azure platform needs to be granted access to request the cryptographic keys when the VM boots to decrypt the virtual disks.</span></span> <span data-ttu-id="256c1-195">Hozzon létre egy titkosítási kulcsot a Key Vault belül, majd a virtuális lemez titkosítás használatának engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="256c1-195">Create an encryption key within your Key Vault, then enable it for use with virtual disk encryption.</span></span> <span data-ttu-id="256c1-196">Az alábbi példakód létrehozza nevű kulcs `myKey` és majd lehetővé teszi az adatok titkosítása:</span><span class="sxs-lookup"><span data-stu-id="256c1-196">The following example creates a key named `myKey` and then enables it for disk encryption:</span></span>

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-the-azure-active-directory-application"></a><span data-ttu-id="256c1-197">Az Azure Active Directory-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="256c1-197">Create the Azure Active Directory application</span></span>
<span data-ttu-id="256c1-198">Amikor a virtuális lemezek vannak titkosított vagy visszafejtett, a végpont használatával kezeli a hitelesítési és titkosítási kulcsok a Key Vault cseréjét.</span><span class="sxs-lookup"><span data-stu-id="256c1-198">When virtual disks are encrypted or decrypted, you use an endpoint to handle the authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="256c1-199">Ehhez a végponthoz, egy Azure Active Directory-alkalmazás, lehetővé teszi, hogy az Azure platformon, kérje a megfelelő titkosítási kulcsokat a virtuális gép nevében.</span><span class="sxs-lookup"><span data-stu-id="256c1-199">This endpoint, an Azure Active Directory application, allows the Azure platform to request the appropriate cryptographic keys on behalf of the VM.</span></span> <span data-ttu-id="256c1-200">Azure Active Directory alapértelmezett példányán érhető előfizetését, bár számos szervezet Azure Active Directory-könyvtárak dedikált.</span><span class="sxs-lookup"><span data-stu-id="256c1-200">A default Azure Active Directory instance is available in your subscription, though many organizations have dedicated Azure Active Directory directories.</span></span>

<span data-ttu-id="256c1-201">Nem a teljes Azure Active Directory-alkalmazás hozza létre a `--home-page` és `--identifier-uris` az alábbi példában szereplő paraméterek nem kell tényleges irányítható cím lehet.</span><span class="sxs-lookup"><span data-stu-id="256c1-201">As you are not creating a full Azure Active Directory application, the `--home-page` and `--identifier-uris` parameters in the following example do not need to be actual routable address.</span></span> <span data-ttu-id="256c1-202">Az alábbi példa is itt adhatja meg, az Azure-portálon belül előállítása kulcsokat helyett jelszóalapú titkos kulcs.</span><span class="sxs-lookup"><span data-stu-id="256c1-202">The following example also specifies a password-based secret rather than generating keys from within the Azure portal.</span></span> <span data-ttu-id="256c1-203">Most kulcs létrehozásakor nem hajtható végre az Azure parancssori felületen.</span><span class="sxs-lookup"><span data-stu-id="256c1-203">As this time, generating keys cannot be done from the Azure CLI.</span></span>

<span data-ttu-id="256c1-204">Az Azure Active Directory-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="256c1-204">Create your Azure Active Directory application.</span></span> <span data-ttu-id="256c1-205">Az alábbi példakód létrehozza egy alkalmazás nevű `myAADApp` és egy jelszót használja `myPassword`.</span><span class="sxs-lookup"><span data-stu-id="256c1-205">The following example creates an application named `myAADApp` and uses a password of `myPassword`.</span></span> <span data-ttu-id="256c1-206">Adja meg a saját jelszavát a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="256c1-206">Specify your own password as follows:</span></span>

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

<span data-ttu-id="256c1-207">Jegyezze fel a `applicationId` , amely küld vissza a kimenet a fenti paranccsal.</span><span class="sxs-lookup"><span data-stu-id="256c1-207">Make a note of the `applicationId` that is returned in the output from the preceding command.</span></span> <span data-ttu-id="256c1-208">Az alkalmazás azonosító olyan egyes fennmaradó lépéseit.</span><span class="sxs-lookup"><span data-stu-id="256c1-208">This application ID is used in some of the remaining steps.</span></span> <span data-ttu-id="256c1-209">Ezután hozzon létre egy egyszerű szolgáltatásnév (SPN), így az alkalmazás nem érhető el a környezetben.</span><span class="sxs-lookup"><span data-stu-id="256c1-209">Next, create a service principal name (SPN) so that the application is accessible within your environment.</span></span> <span data-ttu-id="256c1-210">Sikeresen vagy titkosítására vagy visszafejtésére virtuális lemezek, a Key Vault tárolt titkosítási kulcs engedélyei lehetővé teszik az Azure Active Directory-alkalmazás az a kulcsok beolvasása a értékre kell állítani.</span><span class="sxs-lookup"><span data-stu-id="256c1-210">To successfully encrypt or decrypt virtual disks, permissions on the cryptographic key stored in Key Vault must be set to permit the Azure Active Directory application to read the keys.</span></span>

<span data-ttu-id="256c1-211">Az egyszerű szolgáltatásnév létrehozásához, és állítsa be a megfelelő engedélyeket az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="256c1-211">Create the SPN and set the appropriate permissions as follows:</span></span>

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a><span data-ttu-id="256c1-212">Adjon hozzá egy virtuális lemezt, és tekintse át a titkosítás állapotát</span><span class="sxs-lookup"><span data-stu-id="256c1-212">Add a virtual disk and review encryption status</span></span>
<span data-ttu-id="256c1-213">Ténylegesen titkosíthatja az egyes virtuális lemezek, lehetővé teszi, hogy a lemez hozzáadása egy meglévő virtuális gépre.</span><span class="sxs-lookup"><span data-stu-id="256c1-213">To actually encrypt some virtual disks, lets add a disk to an existing VM.</span></span> <span data-ttu-id="256c1-214">Egy 5Gb adatlemez hozzáadása egy meglévő virtuális gépre az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="256c1-214">Add a 5Gb data disk to an existing VM as follows:</span></span>

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

<span data-ttu-id="256c1-215">A virtuális lemezek jelenleg nincs titkosítva.</span><span class="sxs-lookup"><span data-stu-id="256c1-215">The virtual disks are not currently encrypted.</span></span> <span data-ttu-id="256c1-216">Tekintse át a virtuális gép aktuális titkosítási állapotát az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="256c1-216">Review the current encryption status of your VM as follows:</span></span>

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a><span data-ttu-id="256c1-217">Virtuális lemezek titkosítása</span><span class="sxs-lookup"><span data-stu-id="256c1-217">Encrypt virtual disks</span></span>
<span data-ttu-id="256c1-218">A virtuális lemezek most titkosításához, kapcsolása együtt minden az előző összetevők működését:</span><span class="sxs-lookup"><span data-stu-id="256c1-218">To now encrypt the virtual disks, you bring together all the previous components:</span></span>

1. <span data-ttu-id="256c1-219">Adja meg az Azure Active Directory-alkalmazás és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="256c1-219">Specify the Azure Active Directory application and password.</span></span>
2. <span data-ttu-id="256c1-220">Adja meg a Key Vault a titkosított lemezek metaadatait tárolja.</span><span class="sxs-lookup"><span data-stu-id="256c1-220">Specify the Key Vault to store the metadata for your encrypted disks.</span></span>
3. <span data-ttu-id="256c1-221">Adja meg a titkosítási kulcsok használandó tényleges titkosításához és visszafejtéséhez.</span><span class="sxs-lookup"><span data-stu-id="256c1-221">Specify the cryptographic keys to use for the actual encryption and decryption.</span></span>
4. <span data-ttu-id="256c1-222">Adja meg, hogy az operációsrendszer-lemezképet, az adatlemezek vagy az összes titkosításához.</span><span class="sxs-lookup"><span data-stu-id="256c1-222">Specify whether you want to encrypt the OS disk, the data disks, or all.</span></span>

<span data-ttu-id="256c1-223">Lehetővé teszi, hogy tekintse át a részletes adatait az Azure Key Vault és a létrehozott kulccsal, a tároló Kulcsazonosító, URI, és majd kulcs az utolsó lépésben URL-címe:</span><span class="sxs-lookup"><span data-stu-id="256c1-223">Lets review the details for your Azure Key Vault and the key you created, as you need the Key Vault ID, URI, and then key URL in the final step:</span></span>

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

<span data-ttu-id="256c1-224">A virtuális lemezek használatával kimenete titkosítása a `azure keyvault show` és `azure keyvault key show` parancsok az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="256c1-224">Encrypt your virtual disks using the output from the `azure keyvault show` and `azure keyvault key show` commands as follows:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

<span data-ttu-id="256c1-225">A fenti paranccsal sok változók rendelkezik, az alábbi példa referenciaként a teljes parancs:</span><span class="sxs-lookup"><span data-stu-id="256c1-225">As the preceding command has many variables, the following example is the complete command for reference:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id 147bc426-595d-4bad-b267-58a7cbd8e0b6 \
  --aad-client-secret P@ssw0rd! \
  --disk-encryption-key-vault-url https://myKeyVault.vault.azure.net/ \
  --disk-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResourceGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --key-encryption-key-url https://myKeyVault.vault.azure.net/keys/myKey/6f5fe9383f4e42d0a41553ebc6a82dd1 \
  --key-encryption-key-vault-id /subscriptions/guid/resourceGroups/myResoureGroup/providers/Microsoft.KeyVault/vaults/myKeyVault \
  --volume-type Data
```

<span data-ttu-id="256c1-226">Az Azure parancssori felület nem biztosít részletes hibák a titkosítási folyamat során.</span><span class="sxs-lookup"><span data-stu-id="256c1-226">The Azure CLI doesn't provide verbose errors during the encryption process.</span></span> <span data-ttu-id="256c1-227">További hibaelhárítási információért tekintse át `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` titkosít a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="256c1-227">For additional troubleshooting information, review `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` on the VM you are encrypting.</span></span>

<span data-ttu-id="256c1-228">Végezetül lehetővé teszi, hogy tekintse át a titkosítás állapotát, hogy a virtuális lemezek most már titkosítva megerősítésként:</span><span class="sxs-lookup"><span data-stu-id="256c1-228">Finally, lets review the encryption status again to confirm that your virtual disks have now been encrypted:</span></span>

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a><span data-ttu-id="256c1-229">További adatok lemezek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="256c1-229">Add additional data disks</span></span>
<span data-ttu-id="256c1-230">Az adatlemezek titkosított, ha később további virtuális lemezek hozzáadása a virtuális Gépet, és is titkosítani.</span><span class="sxs-lookup"><span data-stu-id="256c1-230">Once you have encrypted your data disks, you can later add additional virtual disks to your VM and also encrypt them.</span></span> <span data-ttu-id="256c1-231">Amikor futtatja a `azure vm enable-disk-encryption` paranccsal, növeli a feladatütemezési verzióját használja a `--sequence-version` paraméter.</span><span class="sxs-lookup"><span data-stu-id="256c1-231">When you run the `azure vm enable-disk-encryption` command, increment the sequence version using the `--sequence-version` parameter.</span></span> <span data-ttu-id="256c1-232">A feladatütemezési verzió paraméter lehetővé teszi az azonos virtuális gépen végzett műveletek végrehajtását.</span><span class="sxs-lookup"><span data-stu-id="256c1-232">This sequence version parameter allows you to perform repeated operations on the same VM.</span></span>

<span data-ttu-id="256c1-233">Például lehetővé teszi, hogy a második virtuális lemez hozzáadása a virtuális Gépet az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="256c1-233">For example, lets add a second virtual disk to your VM as follows:</span></span>

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

<span data-ttu-id="256c1-234">Futtassa újra a parancsot a virtuális lemezek titkosításához a idő hozzáadása a `--sequence-version` paraméter, és az értéket az első alkalommal történő futtatásakor az alábbiak szerint növekvő:</span><span class="sxs-lookup"><span data-stu-id="256c1-234">Rerun the command to encrypt the virtual disks, this time adding the `--sequence-version` parameter, and incrementing the value from our first run as follows:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
  --sequence-version 2
```


## <a name="next-steps"></a><span data-ttu-id="256c1-235">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="256c1-235">Next steps</span></span>
* <span data-ttu-id="256c1-236">Az Azure Key Vault, beleértve a titkosítási kulcsok és a tárolók kezelésével kapcsolatos további információért lásd: [kezelése Key Vault parancssori felület használatával](../../key-vault/key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="256c1-236">For more information about managing Azure Key Vault, including deleting cryptographic keys and vaults, see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md).</span></span>
* <span data-ttu-id="256c1-237">Lemeztitkosítás, például egy titkosított egyéni virtuális Gépre az Azure-ba, a feltöltendő előkészítése kapcsolatos további információk: [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="256c1-237">For more information about disk encryption, such as preparing an encrypted custom VM to upload to Azure, see [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span></span>
