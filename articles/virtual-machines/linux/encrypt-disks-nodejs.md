---
title: "a Linux virtuális gép az Azure CLI 1.0 hello aaaEncrypt lemezek |} Microsoft Docs"
description: "Hogyan Linux virtuális gép lemezeinek tooencrypt hello Azure CLI 1.0 és hello Resource Manager telepítési modell"
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
ms.openlocfilehash: 68a0394d366c3c6941e2c6db0d4263123f951946
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-disks-on-a-linux-vm-using-hello-azure-cli-10"></a><span data-ttu-id="a5bd6-103">A Linux virtuális gépet az Azure CLI 1.0 hello lemezzel titkosítása</span><span class="sxs-lookup"><span data-stu-id="a5bd6-103">Encrypt disks on a Linux VM using hello Azure CLI 1.0</span></span>
<span data-ttu-id="a5bd6-104">A bővített virtuális gép (VM) biztonsági és megfelelőségi az Azure-ban virtuális lemezek aktívan titkosíthatók.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-104">For enhanced virtual machine (VM) security and compliance, virtual disks in Azure can be encrypted at rest.</span></span> <span data-ttu-id="a5bd6-105">Lemezek titkosítása egy Azure Key Vault a titkosított titkosítási kulcsok használatával.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-105">Disks are encrypted using cryptographic keys that are secured in an Azure Key Vault.</span></span> <span data-ttu-id="a5bd6-106">Szabályozhatja a titkosítási kulcsokat, és a használatukat naplózhatók.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-106">You control these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="a5bd6-107">Ez a cikk részletesen, hogyan Linux virtuális gép virtuális lemezein tooencrypt hello Azure CLI 1.0 és hello Resource Manager üzembe helyezési modellben.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-107">This article details how tooencrypt virtual disks on a Linux VM using hello Azure CLI 1.0 and hello Resource Manager deployment model.</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="a5bd6-108">Parancssori felület verziók toocomplete hello feladat</span><span class="sxs-lookup"><span data-stu-id="a5bd6-108">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="a5bd6-109">Hello feladat a következő parancssori felület verziók hello egyikével hajthatja végre:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-109">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="a5bd6-110">[Az Azure CLI 1.0](#quick-commands) – a parancssori felületen hello klasszikus és resource management üzembe helyezési modellel (a cikk)</span><span class="sxs-lookup"><span data-stu-id="a5bd6-110">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="a5bd6-111">[Az Azure CLI 2.0](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -a következő generációs CLI hello erőforrás felügyeleti telepítési modell</span><span class="sxs-lookup"><span data-stu-id="a5bd6-111">[Azure CLI 2.0](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="quick-commands"></a><span data-ttu-id="a5bd6-112">Gyors parancsok</span><span class="sxs-lookup"><span data-stu-id="a5bd6-112">Quick commands</span></span>
<span data-ttu-id="a5bd6-113">Ha tooquickly kell hello feladatnak, a következő szakasz részletek hello alapszintű hello parancsok tooencrypt virtuális lemezek, a virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-113">If you need tooquickly accomplish hello task, hello following section details hello base commands tooencrypt virtual disks on your VM.</span></span> <span data-ttu-id="a5bd6-114">Részletes információkat és a környezetben az egyes lépések található hello dokumentum többi részén hello, [itt indítása](#overview-of-disk-encryption).</span><span class="sxs-lookup"><span data-stu-id="a5bd6-114">More detailed information and context for each step can be found hello rest of hello document, [starting here](#overview-of-disk-encryption).</span></span>

<span data-ttu-id="a5bd6-115">Hello kell [Azure CLI legújabb 1.0](../../xplat-cli-install.md) telepítve, és bejelentkezett használatával hello Resource Manager módra az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-115">You need hello [latest Azure CLI 1.0](../../xplat-cli-install.md) installed and logged in using hello Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="a5bd6-116">Hello alábbi példák, cserélje le például paraméterek nevei a saját értékeit.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-116">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="a5bd6-117">Példa paraméter nevek a következők `myResourceGroup`, `myKeyVault`, és `myVM`.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-117">Example parameter names include `myResourceGroup`, `myKeyVault`, and `myVM`.</span></span>

<span data-ttu-id="a5bd6-118">Először engedélyezése hello Azure Key Vault szolgáltató belül az Azure-előfizetéshez, és hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-118">First, enable hello Azure Key Vault provider within your Azure subscription and create a resource group.</span></span> <span data-ttu-id="a5bd6-119">hello alábbi példa létrehoz egy erőforráscsoport neve `myResourceGroup` a hello `WestUS` helye:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-119">hello following example creates a resource group name `myResourceGroup` in hello `WestUS` location:</span></span>

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

<span data-ttu-id="a5bd6-120">Hozzon létre egy Azure-tárolóban.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-120">Create an Azure Key Vault.</span></span> <span data-ttu-id="a5bd6-121">hello alábbi példakód létrehozza a kulcstároló nevű `myKeyVault`:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-121">hello following example creates a Key Vault named `myKeyVault`:</span></span>

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

<span data-ttu-id="a5bd6-122">Titkosítási kulcs létrehozására a kulcstároló, majd engedélyezze a lemez titkosításához.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-122">Create a cryptographic key in your Key Vault and enable it for disk encryption.</span></span> <span data-ttu-id="a5bd6-123">hello alábbi példa létrehoz egy nevű kulcsot `myKey`:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-123">hello following example creates a key named `myKey`:</span></span>

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```

<span data-ttu-id="a5bd6-124">Hozzon létre egy Azure Active Directory használatával hello hitelesítési kezelésével és a Key Vault a titkosítási kulcsok cseréjét végpontot.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-124">Create an endpoint using Azure Active Directory for handling hello authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="a5bd6-125">Hello `--home-page` és `--identifier-uris` toobe tényleges irányítható cím nem szükséges.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-125">hello `--home-page` and `--identifier-uris` do not need toobe actual routable address.</span></span> <span data-ttu-id="a5bd6-126">Hello legmagasabb szintű biztonság jelszavak helyett ügyfélkulcs kell használni.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-126">For hello highest level of security, client secrets should be used instead of passwords.</span></span> <span data-ttu-id="a5bd6-127">hello Azure parancssori felület jelenleg nem hozható létre ügyfélkulcs.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-127">hello Azure CLI cannot currently generate client secrets.</span></span> <span data-ttu-id="a5bd6-128">Titkos ügyfélkulccsal csak az Azure-portálon hello hozható létre.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-128">Client secrets can only be generated in hello Azure portal.</span></span> <span data-ttu-id="a5bd6-129">hello alábbi példa létrehoz egy Azure Active Directory-végpontot nevű `myAADApp` és egy jelszót használja `myPassword`.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-129">hello following example creates an Azure Active Directory endpoint named `myAADApp` and uses a password of `myPassword`.</span></span> <span data-ttu-id="a5bd6-130">Adja meg a saját jelszavát a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-130">Specify your own password as follows:</span></span>

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

<span data-ttu-id="a5bd6-131">Megjegyzés: hello `applicationId` hello hello megelőző parancs kimenetében megjelennek.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-131">Note hello `applicationId` shown in hello output from hello preceding command.</span></span> <span data-ttu-id="a5bd6-132">Az alkalmazás-azonosító a következő lépéseket hello olyan:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-132">This application ID is used in hello following steps:</span></span>

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```

<span data-ttu-id="a5bd6-133">Adjon hozzá egy meglévő virtuális gép adatok lemez tooan.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-133">Add a data disk tooan existing VM.</span></span> <span data-ttu-id="a5bd6-134">hello következő példakóddal felveheti egy adatok lemez tooa nevű virtuális gép `myVM`:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-134">hello following example adds a data disk tooa VM named `myVM`:</span></span>

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

<span data-ttu-id="a5bd6-135">A Key Vault és hello kulcs létrehozott hello részletes adatok áttekintésére.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-135">Review hello details for your Key Vault and hello key you created.</span></span> <span data-ttu-id="a5bd6-136">Key Vault azonosító URI és kulcs kell hello hello utolsó lépésként URL-címet.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-136">You need hello Key Vault ID, URI, and key URL in hello final step.</span></span> <span data-ttu-id="a5bd6-137">hello alábbi példa ellenőrzi, hogy hello részleteit a Key vault nevű `myKeyVault` és nevű kulcs `myKey`:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-137">hello following example reviews hello details for a Key Vault named `myKeyVault` and key named `myKey`:</span></span>

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

<span data-ttu-id="a5bd6-138">Titkosítása következő, a lemezek megadása egész saját paraméterek nevei:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-138">Encrypt your disks as follows, entering your own parameter names throughout:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

<span data-ttu-id="a5bd6-139">hello Azure parancssori felület nem biztosít részletes hibák hello titkosítási folyamat során.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-139">hello Azure CLI doesn't provide verbose errors during hello encryption process.</span></span> <span data-ttu-id="a5bd6-140">További hibaelhárítási információért tekintse át `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-140">For additional troubleshooting information, review `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log`.</span></span> <span data-ttu-id="a5bd6-141">Hello, mert a parancs megelőző rendelkezik sok változók és sok arra utal, hogy toowhy hello folyamat sikertelen lesz, amelyekhez nem tarozik, a teljes parancs például a következőképpen nézne ki:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-141">As hello preceding command has many variables and you may not get much indication as toowhy hello process fails, a complete command example would be as follows:</span></span>

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

<span data-ttu-id="a5bd6-142">Hello titkosítási állapotának ellenőrzését, végül újra tooconfirm, hogy a virtuális lemezek most lett titkosítva.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-142">Finally, review hello encryption status again tooconfirm that your virtual disks have now been encrypted.</span></span> <span data-ttu-id="a5bd6-143">hello alábbi példa hello állapotát ellenőrzi a virtuális gépek nevű `myVM` a hello `myResourceGroup` erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-143">hello following example checks hello status of a VM named `myVM` in hello `myResourceGroup` resource group:</span></span>

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```

## <a name="overview-of-disk-encryption"></a><span data-ttu-id="a5bd6-144">Lemeztitkosítás áttekintése</span><span class="sxs-lookup"><span data-stu-id="a5bd6-144">Overview of disk encryption</span></span>
<span data-ttu-id="a5bd6-145">A Linux virtuális gépek virtuális lemezzel titkosított rest az [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span><span class="sxs-lookup"><span data-stu-id="a5bd6-145">Virtual disks on Linux VMs are encrypted at rest using [dm-crypt](https://wikipedia.org/wiki/Dm-crypt).</span></span> <span data-ttu-id="a5bd6-146">Nincs nem kell fizetni az Azure-ban virtuális lemezek titkosítására.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-146">There is no charge for encrypting virtual disks in Azure.</span></span> <span data-ttu-id="a5bd6-147">Titkosítási kulcsok Azure Key Vault szoftver-védelemmel vannak tárolva, vagy importálhat vagy Kulcslétrehozási a hardveres biztonsági modulokkal (HSM) a hitelesített tooFIPS 140-2 2. szint szabványoknak.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-147">Cryptographic keys are stored in Azure Key Vault using software-protection, or you can import or generate your keys in Hardware Security Modules (HSMs) certified tooFIPS 140-2 level 2 standards.</span></span> <span data-ttu-id="a5bd6-148">A titkosítási kulcsokat a felügyeletet, és naplózhatja a használatukat.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-148">You retain control of these cryptographic keys and can audit their use.</span></span> <span data-ttu-id="a5bd6-149">A kriptográfiai kulcsokat használt tooencrypt és virtuális lemezek csatolt tooyour VM visszafejtéséhez.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-149">These cryptographic keys are used tooencrypt and decrypt virtual disks attached tooyour VM.</span></span> <span data-ttu-id="a5bd6-150">Egy Azure Active Directory végpontján lehetővé teszi a biztonságos kiadása a titkosítási kulcsokat, a virtuális gépek vannak kapcsolva, és ki.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-150">An Azure Active Directory endpoint provides a secure mechanism for issuing these cryptographic keys as VMs are powered on and off.</span></span>

<span data-ttu-id="a5bd6-151">a virtuális gépek titkosításához hello folyamat a következőképpen történik:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-151">hello process for encrypting a VM is as follows:</span></span>

1. <span data-ttu-id="a5bd6-152">Hozzon létre egy Azure Key Vault egy titkosítási kulcsot.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-152">Create a cryptographic key in an Azure Key Vault.</span></span>
2. <span data-ttu-id="a5bd6-153">Konfigurálja a hello kriptográfiai kulcs toobe lemezek titkosítására használható.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-153">Configure hello cryptographic key toobe usable for encrypting disks.</span></span>
3. <span data-ttu-id="a5bd6-154">tooread hello titkosítási kulcsot az Azure Key Vault hello hozzon létre egy végpontot, az Azure Active Directoryval hello megfelelő engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-154">tooread hello cryptographic key from hello Azure Key Vault, create an endpoint using Azure Active Directory with hello appropriate permissions.</span></span>
4. <span data-ttu-id="a5bd6-155">Hello parancs tooencrypt ki a virtuális lemezek, megadva hello Azure Active Directory végpontján és a megfelelő titkosítási kulcs toobe használt.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-155">Issue hello command tooencrypt your virtual disks, specifying hello Azure Active Directory endpoint and appropriate cryptographic key toobe used.</span></span>
5. <span data-ttu-id="a5bd6-156">hello Azure Active Directory végpontján kér az Azure Key Vault hello szükséges titkosítási kulcs.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-156">hello Azure Active Directory endpoint requests hello required cryptographic key from Azure Key Vault.</span></span>
6. <span data-ttu-id="a5bd6-157">virtuális lemezek hello hello megadott titkosítási kulccsal titkosított.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-157">hello virtual disks are encrypted using hello provided cryptographic key.</span></span>

## <a name="supporting-services-and-encryption-process"></a><span data-ttu-id="a5bd6-158">Szolgáltatások és a titkosítási folyamat támogatása</span><span class="sxs-lookup"><span data-stu-id="a5bd6-158">Supporting services and encryption process</span></span>
<span data-ttu-id="a5bd6-159">Adatok titkosítása a következő további összetevők hello támaszkodik:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-159">Disk encryption relies on hello following additional components:</span></span>

* <span data-ttu-id="a5bd6-160">**Az Azure Key Vault** -használt toosafeguard titkosítási kulcsok és titkos hello lemez titkosítási/visszafejtési folyamat használja.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-160">**Azure Key Vault** - used toosafeguard cryptographic keys and secrets used for hello disk encryption/decryption process.</span></span>
  * <span data-ttu-id="a5bd6-161">Ha van ilyen, használhat egy meglévő Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-161">If one exists, you can use an existing Azure Key Vault.</span></span> <span data-ttu-id="a5bd6-162">Nincs toodedicate a Key Vault tooencrypting lemezek.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-162">You do not have toodedicate a Key Vault tooencrypting disks.</span></span>
  * <span data-ttu-id="a5bd6-163">tooseparate felügyeleti határokat és kulcs látható, a dedikált kulcstároló is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-163">tooseparate administrative boundaries and key visibility, you can create a dedicated Key Vault.</span></span>
* <span data-ttu-id="a5bd6-164">**Az Azure Active Directory** - leírók hello szükséges titkosítási kulcsok biztonságos cseréjét, és hitelesítési a kért műveleteket.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-164">**Azure Active Directory** - handles hello secure exchanging of required cryptographic keys and authentication for requested actions.</span></span>
  * <span data-ttu-id="a5bd6-165">Az alkalmazás elhelyezésére szolgáló használhat meglévő Azure Active Directory-példány általában.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-165">You can typically use an existing Azure Active Directory instance for housing your application.</span></span>
  * <span data-ttu-id="a5bd6-166">hello alkalmazás több egy végpont a Key Vault hello és a virtuális gép szolgáltatások toorequest, és lekérése kiadott hello megfelelő titkosítási kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-166">hello application is more of an endpoint for hello Key Vault and Virtual Machine services toorequest and get issued hello appropriate cryptographic keys.</span></span> <span data-ttu-id="a5bd6-167">Nem fejleszt, amely az Azure Active Directory tényleges kérelmet.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-167">You are not developing an actual application that integrates with Azure Active Directory.</span></span>

## <a name="requirements-and-limitations"></a><span data-ttu-id="a5bd6-168">Követelmények és korlátozások</span><span class="sxs-lookup"><span data-stu-id="a5bd6-168">Requirements and limitations</span></span>
<span data-ttu-id="a5bd6-169">Támogatott esetek és lemez titkosítására vonatkozó követelményekkel kapcsolatos:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-169">Supported scenarios and requirements for disk encryption:</span></span>

* <span data-ttu-id="a5bd6-170">a következő Linux server SKU - Ubuntu, CentOS, SUSE és SUSE Linux Enterprise Server (SLES) és Red Hat Enterprise Linux hello.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-170">hello following Linux server SKUs - Ubuntu, CentOS, SUSE and SUSE Linux Enterprise Server (SLES), and Red Hat Enterprise Linux.</span></span>
* <span data-ttu-id="a5bd6-171">Minden erőforrások (például a Key Vault, a tárfiók és a virtuális gép) kell lennie a hello azonos Azure-régió, és az előfizetés.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-171">All resources (such as Key Vault, Storage account, and VM) must be in hello same Azure region and subscription.</span></span>
* <span data-ttu-id="a5bd6-172">Standard A, a D, a DS-ben, a G és a GS adatsorozat virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-172">Standard A, D, DS, G, and GS series VMs.</span></span>

<span data-ttu-id="a5bd6-173">Lemeztitkosítás jelenleg nem támogatott a következő forgatókönyvek hello:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-173">Disk encryption is not currently supported in hello following scenarios:</span></span>

* <span data-ttu-id="a5bd6-174">Az alapszintű csomag virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-174">Basic tier VMs.</span></span>
* <span data-ttu-id="a5bd6-175">A virtuális gépek hello klasszikus telepítési modellel készült.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-175">VMs created using hello Classic deployment model.</span></span>
* <span data-ttu-id="a5bd6-176">Az operációs rendszer lemeztitkosítás Linux virtuális gépeken letiltása.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-176">Disabling OS disk encryption on Linux VMs.</span></span>
* <span data-ttu-id="a5bd6-177">Titkosítási kulcsok hello az már titkosított Linux virtuális gép frissítése.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-177">Updating hello cryptographic keys on an already encrypted Linux VM.</span></span>

## <a name="create-hello-azure-key-vault-and-keys"></a><span data-ttu-id="a5bd6-178">Hozzon létre az Azure Key Vault és kulcsok hello</span><span class="sxs-lookup"><span data-stu-id="a5bd6-178">Create hello Azure Key Vault and keys</span></span>
<span data-ttu-id="a5bd6-179">Ez az útmutató toocomplete hello részében szüksége hello [Azure CLI legújabb 1.0](../../xplat-cli-install.md) telepítve, és bejelentkezett használatával hello Resource Manager módra az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-179">toocomplete hello remainder of this guide, you need hello [latest Azure CLI 1.0](../../xplat-cli-install.md) installed and logged in using hello Resource Manager mode as follows:</span></span>

```azurecli
azure config mode arm
```

<span data-ttu-id="a5bd6-180">Teljes hello parancspéldákban cserélje le minden példa paraméter a saját nevét, helyét és kulcsértékek.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-180">Throughout hello command examples, replace all example parameters with your own names, location, and key values.</span></span> <span data-ttu-id="a5bd6-181">hello alábbi példák szabályt használ a `myResourceGroup`, `myKeyVault`, `myAADApp`stb.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-181">hello following examples use a convention of `myResourceGroup`, `myKeyVault`, `myAADApp`, etc.</span></span>

<span data-ttu-id="a5bd6-182">első lépés hello van egy Azure Key Vault toostore toocreate a titkosítási kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-182">hello first step is toocreate an Azure Key Vault toostore your cryptographic keys.</span></span> <span data-ttu-id="a5bd6-183">Az Azure Key Vault tárolhatja a kulcsokat, a titkos kulcsok, vagy a jelszavak, amelyek lehetővé teszik toosecurely végrehajtja az alkalmazások és szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-183">Azure Key Vault can store keys, secrets, or passwords that allow you toosecurely implement them in your applications and services.</span></span> <span data-ttu-id="a5bd6-184">A virtuális lemez titkosításához használja a Key Vault toostore használt tooencrypt egy titkosítási kulcsot, illetve visszafejteni a virtuális lemezek.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-184">For virtual disk encryption, you use Key Vault toostore a cryptographic key that is used tooencrypt or decrypt your virtual disks.</span></span>

<span data-ttu-id="a5bd6-185">Engedélyezi az Azure-előfizetése hello Azure Key Vault szolgáltatót, majd hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-185">Enable hello Azure Key Vault provider in your Azure subscription, then create a resource group.</span></span> <span data-ttu-id="a5bd6-186">hello alábbi példa létrehoz egy erőforráscsoportot `myResourceGroup` a hello `WestUS` helye:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-186">hello following example creates a resource group named `myResourceGroup` in hello `WestUS` location:</span></span>

```azurecli
azure provider register Microsoft.KeyVault
azure group create myResourceGroup --location WestUS
```

<span data-ttu-id="a5bd6-187">hello Azure Key Vault tartalmazó hello kriptográfiai kulcsokat és társított számítási erőforrások, például a tárolási és hello virtuális gépért kell lennie, hello ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-187">hello Azure Key Vault containing hello cryptographic keys and associated compute resources such as storage and hello VM itself must reside in hello same region.</span></span> <span data-ttu-id="a5bd6-188">hello alábbi példa létrehoz egy Azure Key Vault nevű `myKeyVault`:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-188">hello following example creates an Azure Key Vault named `myKeyVault`:</span></span>

```azurecli
azure keyvault create --vault-name myKeyVault --resource-group myResourceGroup \
  --location WestUS
```

<span data-ttu-id="a5bd6-189">A szoftver vagy a biztonsági modell (HSM) védelmi kriptográfiai kulcsokat is tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-189">You can store cryptographic keys using software or Hardware Security Model (HSM) protection.</span></span> <span data-ttu-id="a5bd6-190">A Key Vault prémium HSM használata szükséges.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-190">Using an HSM requires a premium Key Vault.</span></span> <span data-ttu-id="a5bd6-191">Van egy további költség nélkül toocreating szabványos Key Vault szoftveresen védett tároló helyett a Key Vault támogatás.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-191">There is an additional cost toocreating a premium Key Vault rather than standard Key Vault that stores software-protected keys.</span></span> <span data-ttu-id="a5bd6-192">a prémium kulcstároló, megelőző lépés hello a hozzáadása toocreate `--sku Premium` toohello parancsot.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-192">toocreate a premium Key Vault, in hello preceding step add `--sku Premium` toohello command.</span></span> <span data-ttu-id="a5bd6-193">hello alábbi példa szoftveresen védett azt a szabványos kulcstároló létrehozása óta.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-193">hello following example uses software-protected keys since we created a standard Key Vault.</span></span>

<span data-ttu-id="a5bd6-194">Mindkét védelmi modellek hello Azure platformon kell toobe kap hozzáférést toorequest hello titkosítási kulcsok toodecrypt hello virtuális lemezek hello virtuális gép indításakor.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-194">For both protection models, hello Azure platform needs toobe granted access toorequest hello cryptographic keys when hello VM boots toodecrypt hello virtual disks.</span></span> <span data-ttu-id="a5bd6-195">Hozzon létre egy titkosítási kulcsot a Key Vault belül, majd a virtuális lemez titkosítás használatának engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-195">Create an encryption key within your Key Vault, then enable it for use with virtual disk encryption.</span></span> <span data-ttu-id="a5bd6-196">hello alábbi példa létrehoz egy nevű kulcsot `myKey` és majd lehetővé teszi az adatok titkosítása:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-196">hello following example creates a key named `myKey` and then enables it for disk encryption:</span></span>

```azurecli
azure keyvault key create --vault-name myKeyVault --key-name myKey \
  --destination software
azure keyvault set-policy --vault-name myKeyVault --resource-group myResourceGroup \
  --enabled-for-disk-encryption true
```


## <a name="create-hello-azure-active-directory-application"></a><span data-ttu-id="a5bd6-197">Hello Azure Active Directory-alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a5bd6-197">Create hello Azure Active Directory application</span></span>
<span data-ttu-id="a5bd6-198">Ha a virtuális lemezek vannak titkosított vagy visszafejtett, használhat egy végpont toohandle hello hitelesítési és titkosítási kulcsok a Key Vault cseréjét.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-198">When virtual disks are encrypted or decrypted, you use an endpoint toohandle hello authentication and exchanging of cryptographic keys from Key Vault.</span></span> <span data-ttu-id="a5bd6-199">Ehhez a végponthoz, egy Azure Active Directory-alkalmazás, lehetővé teszi, hogy hello Azure platformon toorequest hello megfelelő titkosítási kulcsok hello virtuális gép nevében.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-199">This endpoint, an Azure Active Directory application, allows hello Azure platform toorequest hello appropriate cryptographic keys on behalf of hello VM.</span></span> <span data-ttu-id="a5bd6-200">Azure Active Directory alapértelmezett példányán érhető előfizetését, bár számos szervezet Azure Active Directory-könyvtárak dedikált.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-200">A default Azure Active Directory instance is available in your subscription, though many organizations have dedicated Azure Active Directory directories.</span></span>

<span data-ttu-id="a5bd6-201">Nem hoz létre a teljes Azure Active Directory-alkalmazást, mert hello `--home-page` és `--identifier-uris` hello a következő példa a paraméterek nem kell toobe tényleges irányítható cím.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-201">As you are not creating a full Azure Active Directory application, hello `--home-page` and `--identifier-uris` parameters in hello following example do not need toobe actual routable address.</span></span> <span data-ttu-id="a5bd6-202">hello alábbi példa is meghatározza, hogy hello Azure-portálon belül előállítása kulcsokat helyett jelszóalapú titkos kulcs.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-202">hello following example also specifies a password-based secret rather than generating keys from within hello Azure portal.</span></span> <span data-ttu-id="a5bd6-203">Most kulcs létrehozásakor nem végezhető el az Azure parancssori felület hello.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-203">As this time, generating keys cannot be done from hello Azure CLI.</span></span>

<span data-ttu-id="a5bd6-204">Az Azure Active Directory-alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-204">Create your Azure Active Directory application.</span></span> <span data-ttu-id="a5bd6-205">hello alábbi példa létrehoz egy alkalmazást `myAADApp` és egy jelszót használja `myPassword`.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-205">hello following example creates an application named `myAADApp` and uses a password of `myPassword`.</span></span> <span data-ttu-id="a5bd6-206">Adja meg a saját jelszavát a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-206">Specify your own password as follows:</span></span>

```azurecli
azure ad app create --name myAADApp \
  --home-page http://testencrypt.contoso.com \
  --identifier-uris http://testencrypt.contoso.com \
  --password myPassword
```

<span data-ttu-id="a5bd6-207">Jegyezze fel a hello `applicationId` , visszaadott hello kimenet hello megelőző parancsot.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-207">Make a note of hello `applicationId` that is returned in hello output from hello preceding command.</span></span> <span data-ttu-id="a5bd6-208">Az alkalmazás azonosító olyan egyes hello hátralévő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-208">This application ID is used in some of hello remaining steps.</span></span> <span data-ttu-id="a5bd6-209">Ezután hozzon létre egy egyszerű szolgáltatásnév (SPN), így a hello alkalmazás nem érhető el a környezetben.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-209">Next, create a service principal name (SPN) so that hello application is accessible within your environment.</span></span> <span data-ttu-id="a5bd6-210">toosuccessfully vagy titkosítására vagy visszafejtésére virtuális lemezek, a Key Vault tárolt titkosítási kulcs hello engedélyeinek beállítása toopermit hello Azure Active Directory application tooread hello kulcsok kell lennie.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-210">toosuccessfully encrypt or decrypt virtual disks, permissions on hello cryptographic key stored in Key Vault must be set toopermit hello Azure Active Directory application tooread hello keys.</span></span>

<span data-ttu-id="a5bd6-211">Hello egyszerű szolgáltatásnév létrehozása, és hello megfelelő engedélyek beállítása az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-211">Create hello SPN and set hello appropriate permissions as follows:</span></span>

```azurecli
azure ad sp create --applicationId myApplicationID
azure keyvault set-policy --vault-name myKeyVault --spn myApplicationID \
  --perms-to-keys [\"all\"] --perms-to-secrets [\"all\"]
```


## <a name="add-a-virtual-disk-and-review-encryption-status"></a><span data-ttu-id="a5bd6-212">Adjon hozzá egy virtuális lemezt, és tekintse át a titkosítás állapotát</span><span class="sxs-lookup"><span data-stu-id="a5bd6-212">Add a virtual disk and review encryption status</span></span>
<span data-ttu-id="a5bd6-213">tooactually titkosítani az egyes virtuális lemezek, lehetővé teszi, hogy a virtuális gép meglévő lemez tooan hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-213">tooactually encrypt some virtual disks, lets add a disk tooan existing VM.</span></span> <span data-ttu-id="a5bd6-214">Adja hozzá a következőképpen meglévő virtuális gép 5Gb adat lemez tooan:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-214">Add a 5Gb data disk tooan existing VM as follows:</span></span>

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

<span data-ttu-id="a5bd6-215">virtuális lemezek hello jelenleg nincs titkosítva.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-215">hello virtual disks are not currently encrypted.</span></span> <span data-ttu-id="a5bd6-216">Tekintse át a virtuális gép hello aktuális titkosítás állapotát az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-216">Review hello current encryption status of your VM as follows:</span></span>

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="encrypt-virtual-disks"></a><span data-ttu-id="a5bd6-217">Virtuális lemezek titkosítása</span><span class="sxs-lookup"><span data-stu-id="a5bd6-217">Encrypt virtual disks</span></span>
<span data-ttu-id="a5bd6-218">toonow titkosítása hello virtuális lemezek, akkor egyesítik összes hello az előző összetevők működését:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-218">toonow encrypt hello virtual disks, you bring together all hello previous components:</span></span>

1. <span data-ttu-id="a5bd6-219">Adja meg a hello Azure Active Directory-alkalmazás és a jelszót.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-219">Specify hello Azure Active Directory application and password.</span></span>
2. <span data-ttu-id="a5bd6-220">Adja meg a hello Key Vault toostore hello metaadatait a titkosított lemezek.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-220">Specify hello Key Vault toostore hello metadata for your encrypted disks.</span></span>
3. <span data-ttu-id="a5bd6-221">Adja meg a hello titkosítási kulcsok toouse hello tényleges titkosításához és visszafejtéséhez.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-221">Specify hello cryptographic keys toouse for hello actual encryption and decryption.</span></span>
4. <span data-ttu-id="a5bd6-222">Adja meg, hogy tooencrypt hello operációsrendszer-lemez, hello adatlemezek vagy az összes.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-222">Specify whether you want tooencrypt hello OS disk, hello data disks, or all.</span></span>

<span data-ttu-id="a5bd6-223">Lehetővé teszi, hogy az Azure Key Vault és hello létrehozott kulccsal, hello tároló Kulcsazonosító, URI, és majd kulcs URL-címet a végső lépés hello hello részletes adatok áttekintésére:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-223">Lets review hello details for your Azure Key Vault and hello key you created, as you need hello Key Vault ID, URI, and then key URL in hello final step:</span></span>

```azurecli
azure keyvault show myKeyVault
azure keyvault key show myKeyVault myKey
```

<span data-ttu-id="a5bd6-224">A virtuális lemezek használatával hello hello kimenete titkosítása `azure keyvault show` és `azure keyvault key show` parancsok az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-224">Encrypt your virtual disks using hello output from hello `azure keyvault show` and `azure keyvault key show` commands as follows:</span></span>

```azurecli
azure vm enable-disk-encryption --resource-group myResourceGroup --name myVM \
  --aad-client-id myApplicationID --aad-client-secret myApplicationPassword \
  --disk-encryption-key-vault-url myKeyVaultVaultURI \
  --disk-encryption-key-vault-id myKeyVaultID \
  --key-encryption-key-url myKeyKID \
  --key-encryption-key-vault-id myKeyVaultID \
  --volume-type Data
```

<span data-ttu-id="a5bd6-225">Hello előző parancs esetében van sok változók, hello következő példája hello teljes parancs referenciaként:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-225">As hello preceding command has many variables, hello following example is hello complete command for reference:</span></span>

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

<span data-ttu-id="a5bd6-226">hello Azure parancssori felület nem biztosít részletes hibák hello titkosítási folyamat során.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-226">hello Azure CLI doesn't provide verbose errors during hello encryption process.</span></span> <span data-ttu-id="a5bd6-227">További hibaelhárítási információért tekintse át a `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` a virtuális gép titkosít hello.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-227">For additional troubleshooting information, review `/var/log/azure/Microsoft.OSTCExtensions.AzureDiskEncryptionForLinux/0.x.x.x/extension.log` on hello VM you are encrypting.</span></span>

<span data-ttu-id="a5bd6-228">Végezetül lehetővé teszi, hogy tekintse át a hello titkosítási állapot újra, hogy a virtuális lemezek most már titkosítva tooconfirm:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-228">Finally, lets review hello encryption status again tooconfirm that your virtual disks have now been encrypted:</span></span>

```azurecli
azure vm show-disk-encryption-status --resource-group myResourceGroup --name myVM
```


## <a name="add-additional-data-disks"></a><span data-ttu-id="a5bd6-229">További adatok lemezek hozzáadása</span><span class="sxs-lookup"><span data-stu-id="a5bd6-229">Add additional data disks</span></span>
<span data-ttu-id="a5bd6-230">Az adatlemezek titkosított, ha később hozzáadhat további virtuális lemezek tooyour VM és is titkosítani.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-230">Once you have encrypted your data disks, you can later add additional virtual disks tooyour VM and also encrypt them.</span></span> <span data-ttu-id="a5bd6-231">Hello futtatásakor `azure vm enable-disk-encryption` parancs, a növelést hello feladatütemezési verzió hello segítségével `--sequence-version` paraméter.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-231">When you run hello `azure vm enable-disk-encryption` command, increment hello sequence version using hello `--sequence-version` parameter.</span></span> <span data-ttu-id="a5bd6-232">A feladatütemezési verzió paraméter lehetővé teszi a tooperform történő műveletek hello azonos virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="a5bd6-232">This sequence version parameter allows you tooperform repeated operations on hello same VM.</span></span>

<span data-ttu-id="a5bd6-233">Például lehetővé teszi, hogy az alábbiak szerint adja hozzá a második virtuális lemez tooyour VM:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-233">For example, lets add a second virtual disk tooyour VM as follows:</span></span>

```azurecli
azure vm disk attach-new --resource-group myResourceGroup --vm-name myVM \
  --size-in-gb 5
```

<span data-ttu-id="a5bd6-234">Futtassa újra a hello parancs tooencrypt hello virtuális lemezek, ezúttal hello hozzáadása `--sequence-version` paraméter, és növekvő hello értéket az első futtassa a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="a5bd6-234">Rerun hello command tooencrypt hello virtual disks, this time adding hello `--sequence-version` parameter, and incrementing hello value from our first run as follows:</span></span>

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


## <a name="next-steps"></a><span data-ttu-id="a5bd6-235">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a5bd6-235">Next steps</span></span>
* <span data-ttu-id="a5bd6-236">Az Azure Key Vault, beleértve a titkosítási kulcsok és a tárolók kezelésével kapcsolatos további információért lásd: [kezelése Key Vault parancssori felület használatával](../../key-vault/key-vault-manage-with-cli2.md).</span><span class="sxs-lookup"><span data-stu-id="a5bd6-236">For more information about managing Azure Key Vault, including deleting cryptographic keys and vaults, see [Manage Key Vault using CLI](../../key-vault/key-vault-manage-with-cli2.md).</span></span>
* <span data-ttu-id="a5bd6-237">További információ a lemez titkosítása például egy titkosított egyéni VM tooupload tooAzure, előkészítése: [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span><span class="sxs-lookup"><span data-stu-id="a5bd6-237">For more information about disk encryption, such as preparing an encrypted custom VM tooupload tooAzure, see [Azure Disk Encryption](../../security/azure-security-disk-encryption.md).</span></span>
