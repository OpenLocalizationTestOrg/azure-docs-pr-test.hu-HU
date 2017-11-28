---
title: "Az Azure személyes adatok védelme a titkosítás aktívan |} Microsoft Docs"
description: "Ez a cikk segít a személyes adatok védelme az Azure használatával több része"
services: security
documentationcenter: na
author: Barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/22/2017
ms.author: barclayn
ms.custom: 
ms.openlocfilehash: d0ef3ca8d48f5ba11a89c787ff59df39eeaf673b
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-at-rest-with-encryption"></a><span data-ttu-id="cd1cd-103">Az Azure titkosítási technológiák: a titkosítás aktívan személyes adatok védelme</span><span class="sxs-lookup"><span data-stu-id="cd1cd-103">Azure encryption technologies: Protect personal data at rest with encryption</span></span>

<span data-ttu-id="cd1cd-104">Ez a cikk segít megértéséhez, valamint Azure titkosítási technológiák segítségével inaktív adatok védelmét.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-104">This article helps you understand and use Azure encryption technologies to secure data at rest.</span></span>

<span data-ttu-id="cd1cd-105">Az inaktív adatok titkosítása elengedhetetlen az ajánlott eljárás bizalmas vagy személyes adatok védelme és a megfelelőség és az adatok adatvédelmi követelményeinek megfelelően.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-105">Encryption of data at rest is essential as a best practice to protect sensitive or personal data and to meet compliance and data privacy requirements.</span></span>
<span data-ttu-id="cd1cd-106">Titkosítását az célja, hogy megakadályozható, hogy a támadó a titkosítatlan hozzáférjenek az adatok biztosításával adattitkosítás a lemezen.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-106">Encryption at rest is designed to prevent the attacker from accessing the unencrypted data by ensuring the data is encrypted when on disk.</span></span>

## <a name="scenario"></a><span data-ttu-id="cd1cd-107">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="cd1cd-107">Scenario</span></span> 

<span data-ttu-id="cd1cd-108">Egy nagy körutazás cég, az Amerikai Egyesült Államokban telephelyének bővíti a mediterrán és Balti tengerek, valamint a Brit-szigetekre útvonalak biztosítani a műveleteket.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-108">A large cruise company, headquartered in the United States, is expanding its operations to offer itineraries in the Mediterranean, and Baltic seas, as well as the British Isles.</span></span> <span data-ttu-id="cd1cd-109">Támogatás ezeket, több kisebb körutazás sorok Olaszország, német, Dánia és az Egyesült szerzett</span><span class="sxs-lookup"><span data-stu-id="cd1cd-109">To support those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark, and the U.K.</span></span>

<span data-ttu-id="cd1cd-110">A vállalat a Microsoft Azure használ a felhőben tárolt vállalati adatokat.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-110">The company uses Microsoft Azure to store corporate data in the cloud.</span></span> <span data-ttu-id="cd1cd-111">Ide tartozhat alkalmazott és/vagy ügyféladatok, mint:</span><span class="sxs-lookup"><span data-stu-id="cd1cd-111">This may include employee and/or customer information such as:</span></span>

- <span data-ttu-id="cd1cd-112">címek</span><span class="sxs-lookup"><span data-stu-id="cd1cd-112">addresses</span></span>
- <span data-ttu-id="cd1cd-113">telefonszámok</span><span class="sxs-lookup"><span data-stu-id="cd1cd-113">phone numbers</span></span>
- <span data-ttu-id="cd1cd-114">azonosító számokat</span><span class="sxs-lookup"><span data-stu-id="cd1cd-114">tax identification numbers</span></span>
- <span data-ttu-id="cd1cd-115">orvosi adatok</span><span class="sxs-lookup"><span data-stu-id="cd1cd-115">medical information</span></span>
- <span data-ttu-id="cd1cd-116">Hitelkártya-adatokhoz</span><span class="sxs-lookup"><span data-stu-id="cd1cd-116">credit card information</span></span>

<span data-ttu-id="cd1cd-117">A vállalati adatok azokat, hogy szükség lenne rá részlegek számára elérhető tétele közben kell védeni az alkalmazottak és a felhasználói adatok védelme.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-117">The company must protect the privacy of employee and customer data while making data accessible to those departments that need it.</span></span> <span data-ttu-id="cd1cd-118">(például a bérszámfejtési és foglalások részlegek)</span><span class="sxs-lookup"><span data-stu-id="cd1cd-118">(such as payroll and reservations departments)</span></span>

<span data-ttu-id="cd1cd-119">A körutazás sor is fenntartja, nagy adatbázis ellenszolgáltatás és hűség program tagok nyomon követéséhez a kapcsolatokat az aktuális és korábbi ügyfelek személyes információkat is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-119">The cruise line also maintains a large database of reward and loyalty program members that includes personal information to track relationships with current and past customers.</span></span>

### <a name="problem-statement"></a><span data-ttu-id="cd1cd-120">Probléma leírása</span><span class="sxs-lookup"><span data-stu-id="cd1cd-120">Problem statement</span></span>

<span data-ttu-id="cd1cd-121">A vállalati adatok ezen osztályok (például a bérszámfejtési és foglalások részlegek) igénylő elérhetővé tétele közben kell védeni az alkalmazottak és a felhasználók személyes adatok védelme.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-121">The company must protect the privacy of employees’ and customers’ personal data while making data accessible to those departments that need it (such as payroll and reservations departments).</span></span> <span data-ttu-id="cd1cd-122">A személyes adatok kívül a vállalat által felügyelt adatközpont tárolja, és nem a vállalat fizikai ellenőrzés alatt áll.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-122">This personal data is stored outside of the corporate-controlled data center and is not under the company’s physical control.</span></span>

### <a name="company-goal"></a><span data-ttu-id="cd1cd-123">Vállalati cél</span><span class="sxs-lookup"><span data-stu-id="cd1cd-123">Company goal</span></span>

<span data-ttu-id="cd1cd-124">Többrétegű védelemmel az olyan jellegű biztonsági stratégia részeként akkor a vállalat célja, hogy győződjön meg arról, hogy minden adatforrás, amely tartalmazza a személyes adatok titkosítva legyenek, beleértve a felhőben található.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-124">As part of a multi-layered defense-in-depth security strategy, it is a company goal to ensure that all data sources that contain personal data are encrypted, including those residing in cloud storage.</span></span> <span data-ttu-id="cd1cd-125">Ha illetéktelen személyek a személyes adatok eléréséhez, megjelenítéséhez, nem olvasható formátumban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-125">If unauthorized persons gain access to the personal data, it must be in a form that will render it unreadable.</span></span> <span data-ttu-id="cd1cd-126">Titkosítás alkalmazásakor kell lennie, vagy az átlátszó – felhasználók és rendszergazdák könnyen.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-126">Applying encryption should be easy, or transparent – for users and administrators.</span></span>

## <a name="solutions"></a><span data-ttu-id="cd1cd-127">Megoldások</span><span class="sxs-lookup"><span data-stu-id="cd1cd-127">Solutions</span></span>

<span data-ttu-id="cd1cd-128">Azure-szolgáltatások és több inaktív személyes adatok védelméhez titkosítással technológiák biztosítása.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-128">Azure services provide multiple tools and technologies to help you protect personal data at rest by encrypting it.</span></span>

### <a name="azure-key-vault"></a><span data-ttu-id="cd1cd-129">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="cd1cd-129">Azure Key Vault</span></span>

<span data-ttu-id="cd1cd-130">[Az Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) biztonságitár biztosít az Azure-szolgáltatásokat az inaktív adatok titkosításához használt kulcsok és a javasolt kulcs tárolása és kezelése megoldás.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-130">[Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) provides secure storage for the keys used to encrypt data at rest in Azure services and is the recommended key storage and management solution.</span></span> <span data-ttu-id="cd1cd-131">Titkosítási kulcsok kezelése fontos tárolt adatok védelme.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-131">Encryption key management is essential to securing stored data.</span></span>

#### <a name="how-do-i-use-azure-key-vault-to-protect-keys-that-encrypt-personal-data"></a><span data-ttu-id="cd1cd-132">Miként használható az Azure Key Vault kulcsok titkosító személyes adatok védelme?</span><span class="sxs-lookup"><span data-stu-id="cd1cd-132">How do I use Azure Key Vault to protect keys that encrypt personal data?</span></span>

<span data-ttu-id="cd1cd-133">Az Azure Key Vault használatához az Azure-fiók előfizetés kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-133">To use Azure Key Vault, you need a subscription to an Azure account.</span></span> <span data-ttu-id="cd1cd-134">Szükség Azure PowerShell telepítése.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-134">You also need Azure PowerShell installed.</span></span> <span data-ttu-id="cd1cd-135">A lépések az alábbiak PowerShell-parancsmagok használatával a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="cd1cd-135">Steps include using PowerShell cmdlets to do the following:</span></span>

1. <span data-ttu-id="cd1cd-136">Csatlakozás az előfizetésekhez</span><span class="sxs-lookup"><span data-stu-id="cd1cd-136">Connect to your subscriptions</span></span>

2. <span data-ttu-id="cd1cd-137">Kulcstartó létrehozása</span><span class="sxs-lookup"><span data-stu-id="cd1cd-137">Create a key vault</span></span>

3. <span data-ttu-id="cd1cd-138">Kulcs vagy titkos kód hozzáadása a kulcstartóhoz</span><span class="sxs-lookup"><span data-stu-id="cd1cd-138">Add a key or secret to the key vault</span></span>

4. <span data-ttu-id="cd1cd-139">Regisztrálja az Azure Active Directoryban a kulcstartót használó alkalmazások</span><span class="sxs-lookup"><span data-stu-id="cd1cd-139">Register applications that will use the key vault with Azure Active Directory</span></span>

5. <span data-ttu-id="cd1cd-140">Engedélyezi az alkalmazások, a kulcsok vagy titkos kulcsok használata</span><span class="sxs-lookup"><span data-stu-id="cd1cd-140">Authorize the applications to use the key or secret</span></span>

<span data-ttu-id="cd1cd-141">Hozzon létre egy kulcstartót, használja a New-AzureRmKeyVault PowerShell-parancsmagot is.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-141">To create a key vault, use the New-AzureRmKeyVault PowerShell CmDlt.</span></span> <span data-ttu-id="cd1cd-142">A tároló nevére, erőforráscsoport-név és földrajzi helyet rendeli.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-142">You will assign a vault name, resource group name, and geographic location.</span></span> <span data-ttu-id="cd1cd-143">A tároló neve fogjuk más parancsmagokkal keresztül kulcsok kezelése.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-143">You’ll use the vault name when managing keys via other Cmdlets.</span></span> <span data-ttu-id="cd1cd-144">A tárolót a REST API-n keresztül használó alkalmazások a tároló URI fogja használni.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-144">Applications that use the vault through the REST API will use the vault URI.</span></span>

<span data-ttu-id="cd1cd-145">Az Azure Key Vault biztosítható egy szoftveres védelemmel ellátott kulcs, vagy importálhatja a meglévő kulcs egy. PFX-fájlt.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-145">Azure Key Vault can provide a software-protected key for you, or you can import an existing key in a .PFX file.</span></span> <span data-ttu-id="cd1cd-146">A tárolóban lévő titkos kulcsokat (jelszó) is tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-146">You can also store secrets (passwords) in the vault.</span></span>

<span data-ttu-id="cd1cd-147">Hozzon létre egy kulcsot a helyi HSM-ben is, és vigye át a HSM a Key Vault szolgáltatásban nélkül a kulcs elhagyná a HSM határait.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-147">You can also generate a key in your local HSM and transfer it to HSMs in the Key Vault service, without the key leaving the HSM boundary.</span></span>

<span data-ttu-id="cd1cd-148">Az Azure Key Vault használatával részletes utasításokat kövesse a [Ismerkedés az Azure Key Vault.](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-get-started)</span><span class="sxs-lookup"><span data-stu-id="cd1cd-148">For detailed instructions on using Azure Key Vault, follow the steps in [Get Started with Azure Key Vault.](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-get-started)</span></span>

<span data-ttu-id="cd1cd-149">Az Azure Key Vault használt PowerShell-parancsmagok listáját lásd: [AzureRM.KeyVault](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span><span class="sxs-lookup"><span data-stu-id="cd1cd-149">For a list of PowerShell Cmdlets used with Azure Key Vault, see [AzureRM.KeyVault](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span></span>

### <a name="azure-disk-encryption-for-windows"></a><span data-ttu-id="cd1cd-150">A Windows Azure lemez titkosítása</span><span class="sxs-lookup"><span data-stu-id="cd1cd-150">Azure Disk Encryption for Windows</span></span>

<span data-ttu-id="cd1cd-151">[Az Azure lemez titkosítása a Windows és Linux IaaS virtuális gépeket](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) Azure virtuális gépeken futó aktívan személyes adatok védelmét, és jól integrálható az Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-151">[Azure Disk Encryption for Windows and Linux IaaS VMs](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) protects personal data at rest on Azure virtual machines and integrates with Azure Key Vault.</span></span> <span data-ttu-id="cd1cd-152">Használja az Azure Disk Encryption [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) a Windows és [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) lévő Linux mind az operációs rendszer és az adatlemezek titkosítására.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-152">Azure Disk Encryption uses [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) in Windows and [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) in Linux to encrypt both the OS and the data disks.</span></span> <span data-ttu-id="cd1cd-153">Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016-os, és a Windows 8 és Windows 10-ügyfelek az Azure Disk Encryption támogatott.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-153">Azure Disk Encryption is supported on Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, and on Windows 8 and Windows 10 clients.</span></span>

#### <a name="how-do-i-use-azure-disk-encryption-to-protect-personal-data"></a><span data-ttu-id="cd1cd-154">Miként használható az Azure Disk Encryption személyes adatok védelme?</span><span class="sxs-lookup"><span data-stu-id="cd1cd-154">How do I use Azure Disk Encryption to protect personal data?</span></span>

<span data-ttu-id="cd1cd-155">Azure Disk Encryption használatához az Azure-fiók előfizetés kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-155">To use Azure Disk Encryption, you need a subscription to an Azure account.</span></span> <span data-ttu-id="cd1cd-156">Engedélyezi a lemez titkosítása a Windows Azure és a Linux virtuális gépek, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="cd1cd-156">To enable Azure Disk Encryption for Windows and Linux VMs, do the following:</span></span>

1. <span data-ttu-id="cd1cd-157">Az Azure lemez titkosítási Resource Manager-sablon, a PowerShell vagy a parancssori felület (CLI) használatával lemeztitkosítás engedélyezze és adja meg a titkosítási konfiguráció.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-157">Use the Azure Disk Encryption Resource Manager template, PowerShell, or the command line interface (CLI) to enable disk encryption and specify the  encryption configuration.</span></span> 

2. <span data-ttu-id="cd1cd-158">Hozzáférést biztosít az Azure platformon, a titkosítási anyagot olvasni a kulcstartót.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-158">Grant access to the Azure platform to read the encryption material from your key vault.</span></span>

3. <span data-ttu-id="cd1cd-159">Adjon meg egy Azure Active Directory (AAD) alkalmazás azonosítóját a kulcstartót a titkosítási kulcsokat tárol írni.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-159">Provide an Azure Active Directory (AAD) application identity to write the encryption key material to your key vault.</span></span>

<span data-ttu-id="cd1cd-160">Azure frissíti a virtuális gép és a kulcstároló konfigurációs, és a titkosított virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-160">Azure will update the VM and the key vault configuration, and set up your encrypted VM.</span></span>

<span data-ttu-id="cd1cd-161">Beállításakor a kulcstartót Azure Disk Encryption támogatásához, hozzáadhat egy fő titkosítási kulcs-(KEK), a nagyobb biztonság, és támogatja a titkosított virtuális gépek biztonsági mentése.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-161">When you set up your key vault to support Azure Disk Encryption, you can add a key encryption key (KEK) for added security and to support backup of encrypted virtual machines.</span></span>

![](media/protect-personal-data-at-rest/create-key.png)

<span data-ttu-id="cd1cd-162">Részletes utasítások az adott telepítési forgatókönyvek és a felhasználói élmény szerepelnek [lemez titkosítás a Windows Azure és a Linux IaaS virtuális gépeket.](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption)</span><span class="sxs-lookup"><span data-stu-id="cd1cd-162">Detailed instructions for specific deployment scenarios and user experiences are included in [Azure Disk Encryption for Windows and Linux IaaS VMs.](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption)</span></span>

### <a name="azure-storage-service-encryption"></a><span data-ttu-id="cd1cd-163">Azure Storage Service Encryption</span><span class="sxs-lookup"><span data-stu-id="cd1cd-163">Azure Storage Service Encryption</span></span>

<span data-ttu-id="cd1cd-164">[Az Azure Storage szolgáltatás titkosítási (SSE) inaktív adatok](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) segítségével, és az adatokat, hogy megfeleljen a szervezeti biztonsági és megfelelőségi kötelezettségvállalások megvédeni.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-164">[Azure Storage Service Encryption (SSE) for Data at Rest](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) helps you protect and safeguard your data to meet your organizational security and compliance commitments.</span></span> <span data-ttu-id="cd1cd-165">Az Azure Storage automatikusan titkosítja az adatokat, 256 bites AES titkosítással előtt Storage megőrzése, és lekérése előtt visszafejti.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-165">Azure Storage automatically encrypts your data using 256-bit AES encryption prior to persisting to storage, and decrypts it prior to retrieval.</span></span> <span data-ttu-id="cd1cd-166">A szolgáltatás az Azure-BLOB és a fájlok érhető el.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-166">This service is available for Azure Blobs and Files.</span></span>

#### <a name="how-do-i-use-storage-service-encryption-to-protect-personal-data"></a><span data-ttu-id="cd1cd-167">Hogyan használja a Storage szolgáltatás titkosítási személyes adatok védelme?</span><span class="sxs-lookup"><span data-stu-id="cd1cd-167">How do I use Storage Service Encryption to protect personal data?</span></span>

<span data-ttu-id="cd1cd-168">Ahhoz, hogy a Storage szolgáltatás titkosítási, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="cd1cd-168">To enable Storage Service Encryption, do the following:</span></span>

1. <span data-ttu-id="cd1cd-169">Jelentkezzen be az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-169">Log into the Azure portal.</span></span>

2. <span data-ttu-id="cd1cd-170">Válasszon egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-170">Select a storage account.</span></span>

3. <span data-ttu-id="cd1cd-171">Beállítások a Blob szolgáltatás szakaszban válassza ki a titkosítás.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-171">In Settings, under the Blob Service section, select Encryption.</span></span>

4. <span data-ttu-id="cd1cd-172">A Fájlszolgáltatás szakaszban jelölje be a titkosítás.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-172">Under the File Service section, select Encryption.</span></span>

<span data-ttu-id="cd1cd-173">A titkosítási beállítás gombra kattintva engedélyezheti vagy letilthatja a Storage szolgáltatás titkosítási.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-173">After you click the Encryption setting, you can enable or disable Storage Service Encryption.</span></span>

![](media/protect-personal-data-at-rest/storage-service-encryption.png)

<span data-ttu-id="cd1cd-174">Új adatok lesznek titkosítva.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-174">New data will be encrypted.</span></span> <span data-ttu-id="cd1cd-175">A meglévő fájlokat a tárfiókban lévő adatokat nem titkosított marad.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-175">Data in existing files in this storage account will remain unencrypted.</span></span>

<span data-ttu-id="cd1cd-176">Miután engedélyezte a titkosítás, adatok másolása a tárfiókot, a következő módszerek valamelyikével:</span><span class="sxs-lookup"><span data-stu-id="cd1cd-176">After enabling encryption, copy data to the storage account using one of the following methods:</span></span>

1. <span data-ttu-id="cd1cd-177">Blobok vagy a fájlok másolása a [AzCopy parancssori segédprogram](https://docs.microsoft.com/en-us/azure/storage/storage-use-azcopy).</span><span class="sxs-lookup"><span data-stu-id="cd1cd-177">Copy blobs or files with the [AzCopy Command Line utility](https://docs.microsoft.com/en-us/azure/storage/storage-use-azcopy).</span></span>

2. <span data-ttu-id="cd1cd-178">[Az SMB-fájlmegosztás csatlakoztatása](https://docs.microsoft.com/en-us/azure/storage/storage-file-how-to-use-files-windows) , például Robocopy segédprogram segítségével másolja a fájlokat.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-178">[Mount a file share using SMB](https://docs.microsoft.com/en-us/azure/storage/storage-file-how-to-use-files-windows) so you can use a utility such as Robocopy to copy files.</span></span>

3. <span data-ttu-id="cd1cd-179">Adatok másolása a blob vagy a fájl a és a blob-tároló vagy közötti tárolási fiókok [Storage Ügyfélkódtáraival például .NET](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="cd1cd-179">Copy blob or file data to and from blob storage or between storage accounts using [Storage Client Libraries such as .NET](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-blobs).</span></span>

4.  <span data-ttu-id="cd1cd-180">Használja a [Tártallózó](https://docs.microsoft.com/en-us/azure/storage/storage-explorers) blobok feltöltése a tárfiókhoz a titkosítás engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-180">Use a [Storage Explorer](https://docs.microsoft.com/en-us/azure/storage/storage-explorers) to upload blobs to your storage account with encryption enabled.</span></span>

### <a name="transparent-data-encryption"></a><span data-ttu-id="cd1cd-181">Transzparens adattitkosítás</span><span class="sxs-lookup"><span data-stu-id="cd1cd-181">Transparent Data Encryption</span></span>

<span data-ttu-id="cd1cd-182">Transzparens Data Encryption (TDE) egy olyan szolgáltatás, amellyel az adatbázis és a kiszolgáló szintjén az adatokat titkosíthatja az SQL Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-182">Transparent Data Encryption (TDE) is a feature in SQL Azure by which you can encrypt data at both the database and server levels.</span></span> <span data-ttu-id="cd1cd-183">TDE engedélyezve van az újonnan létrehozott összes adatbázisra alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-183">TDE is now enabled by default on all newly created databases.</span></span> <span data-ttu-id="cd1cd-184">TDE hajt végre, valós idejű i/o-titkosítási és visszafejtési adatainak és naplókönyvtárainak fájlt.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-184">TDE performs real-time I/O encryption and decryption of the data and log files.</span></span>

#### <a name="how-do-i-use-tde-to-protect-personal-data"></a><span data-ttu-id="cd1cd-185">Hogyan használjam TDE személyes adatok védelme?</span><span class="sxs-lookup"><span data-stu-id="cd1cd-185">How do I use TDE to protect personal data?</span></span>

<span data-ttu-id="cd1cd-186">TDE konfigurálhatja az Azure portálon keresztül, a REST API használatával, vagy a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-186">You can configure TDE through the Azure portal, by using the REST API, or by using PowerShell.</span></span> <span data-ttu-id="cd1cd-187">Egy meglévő adatbázist, az Azure portál használatával TDE engedélyezéséhez a következőkre:</span><span class="sxs-lookup"><span data-stu-id="cd1cd-187">To enable TDE on an existing database using the Azure Portal, do the following:</span></span>

1. <span data-ttu-id="cd1cd-188">Látogasson el az Azure portálon, a <https://portal.azure.com> és bejelentkezés az Azure rendszergazdai vagy közreműködői fiókját.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-188">Visit the Azure portal at <https://portal.azure.com> and sign-in with your Azure Administrator or Contributor account.</span></span>

2. <span data-ttu-id="cd1cd-189">A bal oldali szalagcím kattintson a Tallózás gombra, és kattintson az SQL-adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-189">On the left banner, click to BROWSE, and then click SQL databases.</span></span>

3. <span data-ttu-id="cd1cd-190">A bal oldali ablaktáblán kijelölt SQL adatbázisok és kattintson a felhasználói adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-190">With SQL databases selected in the left pane, click your user database.</span></span>

4. <span data-ttu-id="cd1cd-191">Az adatbázis paneljén kattintson az összes beállítást.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-191">In the database blade, click All settings.</span></span>

5. <span data-ttu-id="cd1cd-192">Kattintson a beállítások panelről átlátható titkosítási rész átlátható titkosítási panel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-192">In the Settings blade, click Transparent data encryption part to open the Transparent data encryption blade.</span></span>

6. <span data-ttu-id="cd1cd-193">Az adatok titkosítása panelen áthelyeznie az adatokat titkosítási gombra, és kattintson Mentés (el a lap tetején) beállítás alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-193">In the Data encryption blade, move the Data encryption button to On, and then click Save (at the top of the page) to apply the setting.</span></span> <span data-ttu-id="cd1cd-194">A titkosítási állapottal fog hozzávetőleges az átlátható adattitkosítást előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-194">The Encryption status will approximate the progress of the transparent data encryption.</span></span>

![Adatok titkosítás engedélyezése](media/protect-personal-data-at-rest/turn-data-encryption-on.png)

<span data-ttu-id="cd1cd-196">A cikk található útmutatást ahhoz, hogy TDE és visszafejtése TDE védett adatbázisok és egyéb információkat [átlátható adattitkosítást az Azure SQL Database.](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database)</span><span class="sxs-lookup"><span data-stu-id="cd1cd-196">Instructions on how to enable TDE and information on decrypting TDE-protected databases and more can be found in the article [Transparent Data Encryption with Azure SQL Database.](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database)</span></span>

## <a name="summary"></a><span data-ttu-id="cd1cd-197">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="cd1cd-197">Summary</span></span>

<span data-ttu-id="cd1cd-198">A vállalat az Azure felhőben tárolt személyes adatok célját érhető el.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-198">The company can accomplish its goal of encrypting personal data stored in the Azure cloud.</span></span> <span data-ttu-id="cd1cd-199">Ehhez teljes kötetek védelméhez az Azure Disk Encryption használatával.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-199">They can do this by using Azure Disk Encryption to  protect entire volumes.</span></span> <span data-ttu-id="cd1cd-200">Ebbe beletartozik az operációs rendszer fájljait és az adatfájlokat, amely személyes azonosításra alkalmas adatokat és más bizalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-200">This may include both the operating system files and data files that hold personal identifiable information and other sensitive data.</span></span> <span data-ttu-id="cd1cd-201">Az Azure Storage szolgáltatás titkosítási blobokat és fájlok tárolt személyes adatok védelmére használható.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-201">Azure Storage Service encryption can be used to protect personal data that is stored in blobs and files.</span></span> <span data-ttu-id="cd1cd-202">Az Azure SQL-adatbázisban tárolt adatokat az átlátható adattitkosítási védelmet nyújt az illetéktelen elérhetővé tegyék személyes információkat.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-202">For data that is stored in Azure SQL databases, Transparent Data Encryption provides protection from unauthorized exposure of personal information.</span></span>

<span data-ttu-id="cd1cd-203">A az Azure-adatok titkosítására használt kulcs védelme érdekében a vállalat az Azure Key Vault használja.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-203">To protect the keys that are used to encrypt data in Azure, the company can use Azure Key Vault.</span></span> <span data-ttu-id="cd1cd-204">Ez leegyszerűsíti a kulcskezelési folyamatot, és lehetővé teszi, hogy a vállalat kulcsok feletti személyes adatok titkosítása az irányítást tarthat fenn.</span><span class="sxs-lookup"><span data-stu-id="cd1cd-204">This streamlines the key management process and enables the company to maintain control of keys that access and encrypt personal data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cd1cd-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cd1cd-205">Next steps</span></span>

- [<span data-ttu-id="cd1cd-206">Azure Disk Encryption hibaelhárítási útmutatója</span><span class="sxs-lookup"><span data-stu-id="cd1cd-206">Azure Disk Encryption Troubleshooting Guide</span></span>](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption-tsg)

- [<span data-ttu-id="cd1cd-207">Azure virtuális gép titkosítása</span><span class="sxs-lookup"><span data-stu-id="cd1cd-207">Encrypt an Azure Virtual Machine</span></span>](https://docs.microsoft.com/en-us/azure/security-center/security-center-disk-encryption?toc=%2fazure%2fsecurity%2ftoc.json)

- [<span data-ttu-id="cd1cd-208">Az Azure Data Lake Store az adatok titkosítása</span><span class="sxs-lookup"><span data-stu-id="cd1cd-208">Encryption of data in Azure Data Lake Store</span></span>](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-encryption)

- [<span data-ttu-id="cd1cd-209">Az Azure Cosmos DB adatbázis titkosítását</span><span class="sxs-lookup"><span data-stu-id="cd1cd-209">Azure Cosmos DB database encryption at rest</span></span>](https://docs.microsoft.com/en-us/azure/cosmos-db/database-encryption-at-rest)
