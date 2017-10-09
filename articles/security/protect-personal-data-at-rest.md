---
title: "személyes adatok védelme a titkosítás aktívan aaaAzure |} Microsoft Docs"
description: "Ez a cikk segít az Azure tooprotect személyes adatok felhasználásával több része"
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
ms.openlocfilehash: 9af182b4897f1d04f5f519e6671f53b85073bae1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-at-rest-with-encryption"></a><span data-ttu-id="128b8-103">Az Azure titkosítási technológiák: a titkosítás aktívan személyes adatok védelme</span><span class="sxs-lookup"><span data-stu-id="128b8-103">Azure encryption technologies: Protect personal data at rest with encryption</span></span>

<span data-ttu-id="128b8-104">Ez a cikk segít megismerje és hatékonyabban használhassa az Azure titkosítási technológiák toosecure adatokat aktívan.</span><span class="sxs-lookup"><span data-stu-id="128b8-104">This article helps you understand and use Azure encryption technologies toosecure data at rest.</span></span>

<span data-ttu-id="128b8-105">Az inaktív adatok titkosítása elengedhetetlen a bevált gyakorlat tooprotect bizalmas vagy személyes adatok és a toomeet megfelelőségi és az adatvédelmi követelményeit.</span><span class="sxs-lookup"><span data-stu-id="128b8-105">Encryption of data at rest is essential as a best practice tooprotect sensitive or personal data and toomeet compliance and data privacy requirements.</span></span>
<span data-ttu-id="128b8-106">Aktívan nem adattitkosítás tervezett tooprevent hello támadó titkosítatlanul hello adatok hozzáférését az adatok titkosítása a lemezen hello biztosításával.</span><span class="sxs-lookup"><span data-stu-id="128b8-106">Encryption at rest is designed tooprevent hello attacker from accessing hello unencrypted data by ensuring hello data is encrypted when on disk.</span></span>

## <a name="scenario"></a><span data-ttu-id="128b8-107">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="128b8-107">Scenario</span></span> 

<span data-ttu-id="128b8-108">A nagy körutazás vállalat, az Amerikai Egyesült Államokban, hello telephelyének bővíti a műveletek toooffer útvonalak hello mediterrán, és Balti tengerek, valamint hello Brit-szigetekre.</span><span class="sxs-lookup"><span data-stu-id="128b8-108">A large cruise company, headquartered in hello United States, is expanding its operations toooffer itineraries in hello Mediterranean, and Baltic seas, as well as hello British Isles.</span></span> <span data-ttu-id="128b8-109">toosupport e erőfeszítéseket szerzett több kisebb körutazás sorok Olaszország, német, Dánia és hello brit</span><span class="sxs-lookup"><span data-stu-id="128b8-109">toosupport those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark, and hello U.K.</span></span>

<span data-ttu-id="128b8-110">hello vállalati hello felhő Microsoft Azure toostore vállalati adatait használja.</span><span class="sxs-lookup"><span data-stu-id="128b8-110">hello company uses Microsoft Azure toostore corporate data in hello cloud.</span></span> <span data-ttu-id="128b8-111">Ide tartozhat alkalmazott és/vagy ügyféladatok, mint:</span><span class="sxs-lookup"><span data-stu-id="128b8-111">This may include employee and/or customer information such as:</span></span>

- <span data-ttu-id="128b8-112">címek</span><span class="sxs-lookup"><span data-stu-id="128b8-112">addresses</span></span>
- <span data-ttu-id="128b8-113">telefonszámok</span><span class="sxs-lookup"><span data-stu-id="128b8-113">phone numbers</span></span>
- <span data-ttu-id="128b8-114">azonosító számokat</span><span class="sxs-lookup"><span data-stu-id="128b8-114">tax identification numbers</span></span>
- <span data-ttu-id="128b8-115">orvosi adatok</span><span class="sxs-lookup"><span data-stu-id="128b8-115">medical information</span></span>
- <span data-ttu-id="128b8-116">Hitelkártya-adatokhoz</span><span class="sxs-lookup"><span data-stu-id="128b8-116">credit card information</span></span>

<span data-ttu-id="128b8-117">hello vállalati kell védelmében hello az alkalmazottak és a felhasználói adatok szükség lenne rá adatok elérhető toothose osztályai tétele közben.</span><span class="sxs-lookup"><span data-stu-id="128b8-117">hello company must protect hello privacy of employee and customer data while making data accessible toothose departments that need it.</span></span> <span data-ttu-id="128b8-118">(például a bérszámfejtési és foglalások részlegek)</span><span class="sxs-lookup"><span data-stu-id="128b8-118">(such as payroll and reservations departments)</span></span>

<span data-ttu-id="128b8-119">hello körutazás sor is fenntartja, nagy adatbázis ellenszolgáltatás és hűség program tagok, amely tartalmazza az aktuális és korábbi ügyfelek tootrack kapcsolatokkal személyes adatokat.</span><span class="sxs-lookup"><span data-stu-id="128b8-119">hello cruise line also maintains a large database of reward and loyalty program members that includes personal information tootrack relationships with current and past customers.</span></span>

### <a name="problem-statement"></a><span data-ttu-id="128b8-120">Probléma leírása</span><span class="sxs-lookup"><span data-stu-id="128b8-120">Problem statement</span></span>

<span data-ttu-id="128b8-121">hello vállalati kell védelmében hello alkalmazottak és a felhasználók a személyes adatok (például a bérszámfejtési és foglalások részlegek) igénylő adatok elérhető toothose részlegek tétele közben.</span><span class="sxs-lookup"><span data-stu-id="128b8-121">hello company must protect hello privacy of employees’ and customers’ personal data while making data accessible toothose departments that need it (such as payroll and reservations departments).</span></span> <span data-ttu-id="128b8-122">A személyes adatok hello vállalati vezérelt adatközpont kívül van tárolva, és nem hello vállalat fizikai ellenőrzés alatt áll.</span><span class="sxs-lookup"><span data-stu-id="128b8-122">This personal data is stored outside of hello corporate-controlled data center and is not under hello company’s physical control.</span></span>

### <a name="company-goal"></a><span data-ttu-id="128b8-123">Vállalati cél</span><span class="sxs-lookup"><span data-stu-id="128b8-123">Company goal</span></span>

<span data-ttu-id="128b8-124">Többrétegű védelemmel az olyan jellegű biztonsági stratégia részeként akkor egy vállalati cél tooensure, hogy minden adatforrás, amely tartalmazza a személyes adatok titkosítva legyenek, beleértve a felhőben található.</span><span class="sxs-lookup"><span data-stu-id="128b8-124">As part of a multi-layered defense-in-depth security strategy, it is a company goal tooensure that all data sources that contain personal data are encrypted, including those residing in cloud storage.</span></span> <span data-ttu-id="128b8-125">Nem jogosult személyek nyereség access toohello személyes adatokat, ha megjelenítéséhez, nem olvasható formátumban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="128b8-125">If unauthorized persons gain access toohello personal data, it must be in a form that will render it unreadable.</span></span> <span data-ttu-id="128b8-126">Titkosítás alkalmazásakor kell lennie, vagy az átlátszó – felhasználók és rendszergazdák könnyen.</span><span class="sxs-lookup"><span data-stu-id="128b8-126">Applying encryption should be easy, or transparent – for users and administrators.</span></span>

## <a name="solutions"></a><span data-ttu-id="128b8-127">Megoldások</span><span class="sxs-lookup"><span data-stu-id="128b8-127">Solutions</span></span>

<span data-ttu-id="128b8-128">Azure-szolgáltatásokhoz biztosít több eszközök és technológiák toohelp titkosítással inaktív személyes adatok védelme.</span><span class="sxs-lookup"><span data-stu-id="128b8-128">Azure services provide multiple tools and technologies toohelp you protect personal data at rest by encrypting it.</span></span>

### <a name="azure-key-vault"></a><span data-ttu-id="128b8-129">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="128b8-129">Azure Key Vault</span></span>

<span data-ttu-id="128b8-130">[Az Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) nyújt biztonságos tárolási hello kulcsok tooencrypt adatok használt Azure-szolgáltatások aktívan és ajánlott hello tárolási és kezelési megoldást.</span><span class="sxs-lookup"><span data-stu-id="128b8-130">[Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis) provides secure storage for hello keys used tooencrypt data at rest in Azure services and is hello recommended key storage and management solution.</span></span> <span data-ttu-id="128b8-131">Titkosítási kulcs felügyeleti az alapvető toosecuring tárolt adatai.</span><span class="sxs-lookup"><span data-stu-id="128b8-131">Encryption key management is essential toosecuring stored data.</span></span>

#### <a name="how-do-i-use-azure-key-vault-tooprotect-keys-that-encrypt-personal-data"></a><span data-ttu-id="128b8-132">Miként használható az Azure Key Vault tooprotect kulcsok, amelyek személyes adatok titkosítása?</span><span class="sxs-lookup"><span data-stu-id="128b8-132">How do I use Azure Key Vault tooprotect keys that encrypt personal data?</span></span>

<span data-ttu-id="128b8-133">az Azure Key Vault toouse, kell egy előfizetési tooan Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="128b8-133">toouse Azure Key Vault, you need a subscription tooan Azure account.</span></span> <span data-ttu-id="128b8-134">Szükség Azure PowerShell telepítése.</span><span class="sxs-lookup"><span data-stu-id="128b8-134">You also need Azure PowerShell installed.</span></span> <span data-ttu-id="128b8-135">A lépések az alábbiak PowerShell parancsmagok toodo hello alábbi:</span><span class="sxs-lookup"><span data-stu-id="128b8-135">Steps include using PowerShell cmdlets toodo hello following:</span></span>

1. <span data-ttu-id="128b8-136">Csatlakozás tooyour előfizetések</span><span class="sxs-lookup"><span data-stu-id="128b8-136">Connect tooyour subscriptions</span></span>

2. <span data-ttu-id="128b8-137">Kulcstartó létrehozása</span><span class="sxs-lookup"><span data-stu-id="128b8-137">Create a key vault</span></span>

3. <span data-ttu-id="128b8-138">Kulcs vagy titkos toohello kulcstároló hozzáadása</span><span class="sxs-lookup"><span data-stu-id="128b8-138">Add a key or secret toohello key vault</span></span>

4. <span data-ttu-id="128b8-139">Regisztrálja az Azure Active Directoryval hello kulcstartót használó alkalmazások</span><span class="sxs-lookup"><span data-stu-id="128b8-139">Register applications that will use hello key vault with Azure Active Directory</span></span>

5. <span data-ttu-id="128b8-140">Hello alkalmazások toouse hello kulcs vagy titkos kulcs engedélyezése</span><span class="sxs-lookup"><span data-stu-id="128b8-140">Authorize hello applications toouse hello key or secret</span></span>

<span data-ttu-id="128b8-141">Kulcstároló, toocreate hello New-AzureRmKeyVault PowerShell-parancsmagot is használja.</span><span class="sxs-lookup"><span data-stu-id="128b8-141">toocreate a key vault, use hello New-AzureRmKeyVault PowerShell CmDlt.</span></span> <span data-ttu-id="128b8-142">A tároló nevére, erőforráscsoport-név és földrajzi helyet rendeli.</span><span class="sxs-lookup"><span data-stu-id="128b8-142">You will assign a vault name, resource group name, and geographic location.</span></span> <span data-ttu-id="128b8-143">Hello tároló neve fog használni, más parancsmagok segítségével kulcsok kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="128b8-143">You’ll use hello vault name when managing keys via other Cmdlets.</span></span> <span data-ttu-id="128b8-144">Hello tároló hello REST API-n keresztül használó alkalmazások hello tároló URI fogja használni.</span><span class="sxs-lookup"><span data-stu-id="128b8-144">Applications that use hello vault through hello REST API will use hello vault URI.</span></span>

<span data-ttu-id="128b8-145">Az Azure Key Vault biztosítható egy szoftveres védelemmel ellátott kulcs, vagy importálhatja a meglévő kulcs egy. PFX-fájlt.</span><span class="sxs-lookup"><span data-stu-id="128b8-145">Azure Key Vault can provide a software-protected key for you, or you can import an existing key in a .PFX file.</span></span> <span data-ttu-id="128b8-146">Hello tárolóban lévő titkos kulcsokat (jelszó) is tárolhatja.</span><span class="sxs-lookup"><span data-stu-id="128b8-146">You can also store secrets (passwords) in hello vault.</span></span>

<span data-ttu-id="128b8-147">Hozzon létre egy kulcsot a helyi HSM-ben is, és hello kulcs elhagyná a HSM határait hello nélkül a Key Vault szolgáltatás hello tooHSMs vigye.</span><span class="sxs-lookup"><span data-stu-id="128b8-147">You can also generate a key in your local HSM and transfer it tooHSMs in hello Key Vault service, without hello key leaving hello HSM boundary.</span></span>

<span data-ttu-id="128b8-148">Az Azure Key Vault használatával részletes utasításokat kövesse hello lépései [Ismerkedés az Azure Key Vault.](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-get-started)</span><span class="sxs-lookup"><span data-stu-id="128b8-148">For detailed instructions on using Azure Key Vault, follow hello steps in [Get Started with Azure Key Vault.](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-get-started)</span></span>

<span data-ttu-id="128b8-149">Az Azure Key Vault használt PowerShell-parancsmagok listáját lásd: [AzureRM.KeyVault](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span><span class="sxs-lookup"><span data-stu-id="128b8-149">For a list of PowerShell Cmdlets used with Azure Key Vault, see [AzureRM.KeyVault](https://docs.microsoft.com/en-us/powershell/module/azurerm.keyvault/?view=azurermps-4.2.0).</span></span>

### <a name="azure-disk-encryption-for-windows"></a><span data-ttu-id="128b8-150">A Windows Azure lemez titkosítása</span><span class="sxs-lookup"><span data-stu-id="128b8-150">Azure Disk Encryption for Windows</span></span>

<span data-ttu-id="128b8-151">[Az Azure lemez titkosítása a Windows és Linux IaaS virtuális gépeket](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) Azure virtuális gépeken futó aktívan személyes adatok védelmét, és jól integrálható az Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="128b8-151">[Azure Disk Encryption for Windows and Linux IaaS VMs](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption) protects personal data at rest on Azure virtual machines and integrates with Azure Key Vault.</span></span> <span data-ttu-id="128b8-152">Használja az Azure Disk Encryption [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) a Windows és [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) Linux tooencrypt mind az operációs rendszer és hello adatlemezek hello.</span><span class="sxs-lookup"><span data-stu-id="128b8-152">Azure Disk Encryption uses [BitLocker](https://technet.microsoft.com/library/cc732774.aspx) in Windows and [DM-Crypt](https://en.wikipedia.org/wiki/Dm-crypt) in Linux tooencrypt both hello OS and hello data disks.</span></span> <span data-ttu-id="128b8-153">Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016-os, és a Windows 8 és Windows 10-ügyfelek az Azure Disk Encryption támogatott.</span><span class="sxs-lookup"><span data-stu-id="128b8-153">Azure Disk Encryption is supported on Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, and on Windows 8 and Windows 10 clients.</span></span>

#### <a name="how-do-i-use-azure-disk-encryption-tooprotect-personal-data"></a><span data-ttu-id="128b8-154">Miként használható az Azure Disk Encryption tooprotect személyes adatokat?</span><span class="sxs-lookup"><span data-stu-id="128b8-154">How do I use Azure Disk Encryption tooprotect personal data?</span></span>

<span data-ttu-id="128b8-155">Azure Disk Encryption toouse, egy előfizetés tooan Azure-fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="128b8-155">toouse Azure Disk Encryption, you need a subscription tooan Azure account.</span></span> <span data-ttu-id="128b8-156">tooenable Azure lemez titkosítása a Windows és Linux virtuális gépek, a hello, a következő:</span><span class="sxs-lookup"><span data-stu-id="128b8-156">tooenable Azure Disk Encryption for Windows and Linux VMs, do hello following:</span></span>

1. <span data-ttu-id="128b8-157">Hello Azure lemez titkosítási Resource Manager-sablon, a PowerShell vagy a parancssori felület (CLI) tooenable lemeztitkosítás hello használata, és adja meg a titkosítási konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="128b8-157">Use hello Azure Disk Encryption Resource Manager template, PowerShell, or hello command line interface (CLI) tooenable disk encryption and specify the  encryption configuration.</span></span> 

2. <span data-ttu-id="128b8-158">Adjon hozzáférést toohello Azure platformon tooread hello titkosítási anyagok a kulcstartót.</span><span class="sxs-lookup"><span data-stu-id="128b8-158">Grant access toohello Azure platform tooread hello encryption material from your key vault.</span></span>

3. <span data-ttu-id="128b8-159">Adjon meg egy Azure Active Directory (AAD) alkalmazás identitását toowrite hello titkosítási kulcs jelentős tooyour kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="128b8-159">Provide an Azure Active Directory (AAD) application identity toowrite hello encryption key material tooyour key vault.</span></span>

<span data-ttu-id="128b8-160">Azure hello VM és hello kulcstároló konfiguráció frissítése, és a titkosított virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="128b8-160">Azure will update hello VM and hello key vault configuration, and set up your encrypted VM.</span></span>

<span data-ttu-id="128b8-161">A kulcstároló toosupport Azure Disk Encryption beállításakor a nagyobb biztonság és toosupport titkosított virtuális gépek biztonsági mentését egy fő titkosítási kulcs-(KEK) adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="128b8-161">When you set up your key vault toosupport Azure Disk Encryption, you can add a key encryption key (KEK) for added security and toosupport backup of encrypted virtual machines.</span></span>

![](media/protect-personal-data-at-rest/create-key.png)

<span data-ttu-id="128b8-162">Részletes utasítások az adott telepítési forgatókönyvek és a felhasználói élmény szerepelnek [lemez titkosítás a Windows Azure és a Linux IaaS virtuális gépeket.](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption)</span><span class="sxs-lookup"><span data-stu-id="128b8-162">Detailed instructions for specific deployment scenarios and user experiences are included in [Azure Disk Encryption for Windows and Linux IaaS VMs.](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption)</span></span>

### <a name="azure-storage-service-encryption"></a><span data-ttu-id="128b8-163">Azure Storage Service Encryption</span><span class="sxs-lookup"><span data-stu-id="128b8-163">Azure Storage Service Encryption</span></span>

<span data-ttu-id="128b8-164">[Az Azure Storage szolgáltatás titkosítási (SSE) inaktív adatok](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) segít és megvédeni az adatok toomeet a szervezeti biztonsági és megfelelőségi jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="128b8-164">[Azure Storage Service Encryption (SSE) for Data at Rest](https://docs.microsoft.com/en-us/azure/storage/storage-service-encryption) helps you protect and safeguard your data toomeet your organizational security and compliance commitments.</span></span> <span data-ttu-id="128b8-165">Az Azure Storage automatikusan titkosítja az adatokat, 256 bites AES titkosítást előzetes toopersisting toostorage használatával, és a korábbi tooretrieval visszafejti.</span><span class="sxs-lookup"><span data-stu-id="128b8-165">Azure Storage automatically encrypts your data using 256-bit AES encryption prior toopersisting toostorage, and decrypts it prior tooretrieval.</span></span> <span data-ttu-id="128b8-166">A szolgáltatás az Azure-BLOB és a fájlok érhető el.</span><span class="sxs-lookup"><span data-stu-id="128b8-166">This service is available for Azure Blobs and Files.</span></span>

#### <a name="how-do-i-use-storage-service-encryption-tooprotect-personal-data"></a><span data-ttu-id="128b8-167">Hogyan használja a Storage szolgáltatás titkosítási tooprotect személyes adatokat?</span><span class="sxs-lookup"><span data-stu-id="128b8-167">How do I use Storage Service Encryption tooprotect personal data?</span></span>

<span data-ttu-id="128b8-168">Storage szolgáltatás titkosítási tooenable hello a következő:</span><span class="sxs-lookup"><span data-stu-id="128b8-168">tooenable Storage Service Encryption, do hello following:</span></span>

1. <span data-ttu-id="128b8-169">Jelentkezzen be hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="128b8-169">Log into hello Azure portal.</span></span>

2. <span data-ttu-id="128b8-170">Válasszon egy tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="128b8-170">Select a storage account.</span></span>

3. <span data-ttu-id="128b8-171">A beállítások hello Blob szolgáltatás szakaszban, válassza ki a titkosítási.</span><span class="sxs-lookup"><span data-stu-id="128b8-171">In Settings, under hello Blob Service section, select Encryption.</span></span>

4. <span data-ttu-id="128b8-172">Csoportban hello Fájlszolgáltatás szakaszban, a titkosítás.</span><span class="sxs-lookup"><span data-stu-id="128b8-172">Under hello File Service section, select Encryption.</span></span>

<span data-ttu-id="128b8-173">Hello titkosítási beállítás gombra kattintva engedélyezheti vagy letilthatja a Storage szolgáltatás titkosítási.</span><span class="sxs-lookup"><span data-stu-id="128b8-173">After you click hello Encryption setting, you can enable or disable Storage Service Encryption.</span></span>

![](media/protect-personal-data-at-rest/storage-service-encryption.png)

<span data-ttu-id="128b8-174">Új adatok lesznek titkosítva.</span><span class="sxs-lookup"><span data-stu-id="128b8-174">New data will be encrypted.</span></span> <span data-ttu-id="128b8-175">A meglévő fájlokat a tárfiókban lévő adatokat nem titkosított marad.</span><span class="sxs-lookup"><span data-stu-id="128b8-175">Data in existing files in this storage account will remain unencrypted.</span></span>

<span data-ttu-id="128b8-176">Miután engedélyezte a titkosítás, másolja a toohello tárfiók hello a következő módszerek egyikével:</span><span class="sxs-lookup"><span data-stu-id="128b8-176">After enabling encryption, copy data toohello storage account using one of hello following methods:</span></span>

1. <span data-ttu-id="128b8-177">Blobok vagy hello a fájlok másolása [AzCopy parancssori segédprogram](https://docs.microsoft.com/en-us/azure/storage/storage-use-azcopy).</span><span class="sxs-lookup"><span data-stu-id="128b8-177">Copy blobs or files with hello [AzCopy Command Line utility](https://docs.microsoft.com/en-us/azure/storage/storage-use-azcopy).</span></span>

2. <span data-ttu-id="128b8-178">[Az SMB-fájlmegosztás csatlakoztatása](https://docs.microsoft.com/en-us/azure/storage/storage-file-how-to-use-files-windows) , hogy használhassa a segédprogram például Robocopy toocopy fájlokat.</span><span class="sxs-lookup"><span data-stu-id="128b8-178">[Mount a file share using SMB](https://docs.microsoft.com/en-us/azure/storage/storage-file-how-to-use-files-windows) so you can use a utility such as Robocopy toocopy files.</span></span>

3. <span data-ttu-id="128b8-179">Másolja a blob vagy a fájl adatainak tooand között vagy a blob storage tárolási fiókok [Storage Ügyfélkódtáraival például .NET](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-blobs).</span><span class="sxs-lookup"><span data-stu-id="128b8-179">Copy blob or file data tooand from blob storage or between storage accounts using [Storage Client Libraries such as .NET](https://docs.microsoft.com/en-us/azure/storage/storage-dotnet-how-to-use-blobs).</span></span>

4.  <span data-ttu-id="128b8-180">Használja a [Tártallózó](https://docs.microsoft.com/en-us/azure/storage/storage-explorers) tooupload tooyour tárfiók blobok a titkosítás engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="128b8-180">Use a [Storage Explorer](https://docs.microsoft.com/en-us/azure/storage/storage-explorers) tooupload blobs tooyour storage account with encryption enabled.</span></span>

### <a name="transparent-data-encryption"></a><span data-ttu-id="128b8-181">Transzparens adattitkosítás</span><span class="sxs-lookup"><span data-stu-id="128b8-181">Transparent Data Encryption</span></span>

<span data-ttu-id="128b8-182">Transzparens Data Encryption (TDE) egy olyan szolgáltatás, amely mindkét hello adatbázis és a kiszolgáló szintjén az adatokat titkosíthatja az SQL Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="128b8-182">Transparent Data Encryption (TDE) is a feature in SQL Azure by which you can encrypt data at both hello database and server levels.</span></span> <span data-ttu-id="128b8-183">TDE engedélyezve van az újonnan létrehozott összes adatbázisra alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="128b8-183">TDE is now enabled by default on all newly created databases.</span></span> <span data-ttu-id="128b8-184">TDE valós idejű i/o-titkosítási és visszafejtési hello adatainak és naplókönyvtárainak fájlok hajt végre.</span><span class="sxs-lookup"><span data-stu-id="128b8-184">TDE performs real-time I/O encryption and decryption of hello data and log files.</span></span>

#### <a name="how-do-i-use-tde-tooprotect-personal-data"></a><span data-ttu-id="128b8-185">Hogyan használja a TDE tooprotect személyes adatokat?</span><span class="sxs-lookup"><span data-stu-id="128b8-185">How do I use TDE tooprotect personal data?</span></span>

<span data-ttu-id="128b8-186">Konfigurálhat TDE hello Azure-portálon keresztül, hello REST API használatával, vagy a PowerShell használatával.</span><span class="sxs-lookup"><span data-stu-id="128b8-186">You can configure TDE through hello Azure portal, by using hello REST API, or by using PowerShell.</span></span> <span data-ttu-id="128b8-187">egy létező adatbázis felett hello Azure portál használatával TDE tooenable hello a következő:</span><span class="sxs-lookup"><span data-stu-id="128b8-187">tooenable TDE on an existing database using hello Azure Portal, do hello following:</span></span>

1. <span data-ttu-id="128b8-188">A Microsoft hello Azure portál, <https://portal.azure.com> és bejelentkezés az Azure rendszergazdai vagy közreműködői fiókját.</span><span class="sxs-lookup"><span data-stu-id="128b8-188">Visit hello Azure portal at <https://portal.azure.com> and sign-in with your Azure Administrator or Contributor account.</span></span>

2. <span data-ttu-id="128b8-189">Hello bal szalagcím tooBROWSE kattintson, majd SQL-adatbázisok.</span><span class="sxs-lookup"><span data-stu-id="128b8-189">On hello left banner, click tooBROWSE, and then click SQL databases.</span></span>

3. <span data-ttu-id="128b8-190">Hello bal oldali ablaktáblán kijelölt SQL adatbázisok és kattintson a felhasználói adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="128b8-190">With SQL databases selected in hello left pane, click your user database.</span></span>

4. <span data-ttu-id="128b8-191">Hello adatbázis paneljén kattintson az összes beállítás.</span><span class="sxs-lookup"><span data-stu-id="128b8-191">In hello database blade, click All settings.</span></span>

5. <span data-ttu-id="128b8-192">Hello-beállítások panelen kattintson átlátható titkosítási rész tooopen hello átlátható titkosítási panelen.</span><span class="sxs-lookup"><span data-stu-id="128b8-192">In hello Settings blade, click Transparent data encryption part tooopen hello Transparent data encryption blade.</span></span>

6. <span data-ttu-id="128b8-193">Hello adatok titkosítás panelen, helyezze át a hello adatok titkosítás gomb tooOn, és kattintson a Save (hello hello oldal tetején) tooapply hello beállítást.</span><span class="sxs-lookup"><span data-stu-id="128b8-193">In hello Data encryption blade, move hello Data encryption button tooOn, and then click Save (at hello top of hello page) tooapply hello setting.</span></span> <span data-ttu-id="128b8-194">hello titkosítási állapottal fog hozzávetőleges hello átlátható adattitkosítás hello előrehaladását.</span><span class="sxs-lookup"><span data-stu-id="128b8-194">hello Encryption status will approximate hello progress of hello transparent data encryption.</span></span>

![Adatok titkosítás engedélyezése](media/protect-personal-data-at-rest/turn-data-encryption-on.png)

<span data-ttu-id="128b8-196">Hogyan tooenable TDE és visszafejtése TDE védett adatbázisok és egyéb információk megtalálhatók hello cikkben lévő utasítások [átlátható adattitkosítást az Azure SQL Database.](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database)</span><span class="sxs-lookup"><span data-stu-id="128b8-196">Instructions on how tooenable TDE and information on decrypting TDE-protected databases and more can be found in hello article [Transparent Data Encryption with Azure SQL Database.](https://docs.microsoft.com/en-us/sql/relational-databases/security/encryption/transparent-data-encryption-with-azure-sql-database)</span></span>

## <a name="summary"></a><span data-ttu-id="128b8-197">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="128b8-197">Summary</span></span>

<span data-ttu-id="128b8-198">hello vállalat is elérheti személyes hello Azure felhőben tárolt adatok titkosítása az a célja.</span><span class="sxs-lookup"><span data-stu-id="128b8-198">hello company can accomplish its goal of encrypting personal data stored in hello Azure cloud.</span></span> <span data-ttu-id="128b8-199">Ehhez a Azure Disk Encryption túl a teljes kötetek védelméhez.</span><span class="sxs-lookup"><span data-stu-id="128b8-199">They can do this by using Azure Disk Encryption too protect entire volumes.</span></span> <span data-ttu-id="128b8-200">Ide tartozhat hello operációs rendszer fájljait és a adatfájlok, amely személyes azonosításra alkalmas adatokat és más bizalmas adatokat.</span><span class="sxs-lookup"><span data-stu-id="128b8-200">This may include both hello operating system files and data files that hold personal identifiable information and other sensitive data.</span></span> <span data-ttu-id="128b8-201">Az Azure Storage szolgáltatás titkosítási használt tooprotect blobok és fájlok tárolt személyes adatok is lehet.</span><span class="sxs-lookup"><span data-stu-id="128b8-201">Azure Storage Service encryption can be used tooprotect personal data that is stored in blobs and files.</span></span> <span data-ttu-id="128b8-202">Az Azure SQL-adatbázisban tárolt adatokat az átlátható adattitkosítási védelmet nyújt az illetéktelen elérhetővé tegyék személyes információkat.</span><span class="sxs-lookup"><span data-stu-id="128b8-202">For data that is stored in Azure SQL databases, Transparent Data Encryption provides protection from unauthorized exposure of personal information.</span></span>

<span data-ttu-id="128b8-203">hello vállalati tooprotect hello kulcsokat is használt tooencrypt adatok Azure-ban, használhatja az Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="128b8-203">tooprotect hello keys that are used tooencrypt data in Azure, hello company can use Azure Key Vault.</span></span> <span data-ttu-id="128b8-204">Ez leegyszerűsíti hello kulcskezelési folyamatot, és lehetővé teszi, hogy hello vállalati toomaintain irányítását kulcsok feletti személyes adatok titkosítása.</span><span class="sxs-lookup"><span data-stu-id="128b8-204">This streamlines hello key management process and enables hello company toomaintain control of keys that access and encrypt personal data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="128b8-205">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="128b8-205">Next steps</span></span>

- [<span data-ttu-id="128b8-206">Azure Disk Encryption hibaelhárítási útmutatója</span><span class="sxs-lookup"><span data-stu-id="128b8-206">Azure Disk Encryption Troubleshooting Guide</span></span>](https://docs.microsoft.com/en-us/azure/security/azure-security-disk-encryption-tsg)

- [<span data-ttu-id="128b8-207">Azure virtuális gép titkosítása</span><span class="sxs-lookup"><span data-stu-id="128b8-207">Encrypt an Azure Virtual Machine</span></span>](https://docs.microsoft.com/en-us/azure/security-center/security-center-disk-encryption?toc=%2fazure%2fsecurity%2ftoc.json)

- [<span data-ttu-id="128b8-208">Az Azure Data Lake Store az adatok titkosítása</span><span class="sxs-lookup"><span data-stu-id="128b8-208">Encryption of data in Azure Data Lake Store</span></span>](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-encryption)

- [<span data-ttu-id="128b8-209">Az Azure Cosmos DB adatbázis titkosítását</span><span class="sxs-lookup"><span data-stu-id="128b8-209">Azure Cosmos DB database encryption at rest</span></span>](https://docs.microsoft.com/en-us/azure/cosmos-db/database-encryption-at-rest)
