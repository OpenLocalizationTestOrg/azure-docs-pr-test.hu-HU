---
title: "aaaHow toogenerate és átviteli HSM által védett kulcsok Azure Key vault |} Microsoft Docs"
description: "Ez a cikk toohelp tervezése, létrehozása és majd a saját HSM által védett kulcsokat toouse az Azure Key Vault átviteléhez használja. Más néven BYOK vagy a saját kulcs."
services: key-vault
documentationcenter: 
author: cabailey
manager: mbaldwin
tags: azure-resource-manager
ms.assetid: 51abafa1-812b-460f-a129-d714fdc391da
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/09/2017
ms.author: ambapat
ms.openlocfilehash: 3bb234bd1c4b81770542ccf7110e256385ca3309
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toogenerate-and-transfer-hsm-protected-keys-for-azure-key-vault"></a><span data-ttu-id="9b576-104">Hogyan toogenerate és átviteli HSM által védett kulcsok Azure Key vault</span><span class="sxs-lookup"><span data-stu-id="9b576-104">How toogenerate and transfer HSM-protected keys for Azure Key Vault</span></span>
## <a name="introduction"></a><span data-ttu-id="9b576-105">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="9b576-105">Introduction</span></span>
<span data-ttu-id="9b576-106">A nagyobb az Azure Key Vault használatakor importálhatja és a hardveres biztonsági modulokkal (HSM), amely sosem hagyják el a HSM határait hello kulcsok létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="9b576-106">For added assurance, when you use Azure Key Vault, you can import or generate keys in hardware security modules (HSMs) that never leave hello HSM boundary.</span></span> <span data-ttu-id="9b576-107">Ebben a forgatókönyvben legtöbbször hivatkozott tooas *a saját kulcs*, vagy BYOK.</span><span class="sxs-lookup"><span data-stu-id="9b576-107">This scenario is often referred tooas *bring your own key*, or BYOK.</span></span> <span data-ttu-id="9b576-108">hello hardveres biztonsági modulok FIPS 140-2 2. szintű érvényesítve.</span><span class="sxs-lookup"><span data-stu-id="9b576-108">hello HSMs are FIPS 140-2 Level 2 validated.</span></span> <span data-ttu-id="9b576-109">Az Azure Key Vault használja a Thales nShield hardveres biztonsági modulok tooprotect-család a kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="9b576-109">Azure Key Vault uses Thales nShield family of HSMs tooprotect your keys.</span></span>

<span data-ttu-id="9b576-110">Ez a témakör toohelp tervezése, létrehozása és a saját HSM által védett kulcsokat toouse az Azure Key Vault majd át használja fel hello adatokat.</span><span class="sxs-lookup"><span data-stu-id="9b576-110">Use hello information in this topic toohelp you plan for, generate, and then transfer your own HSM-protected keys toouse with Azure Key Vault.</span></span>

<span data-ttu-id="9b576-111">Ez a funkció nem érhető el Azure Kínában.</span><span class="sxs-lookup"><span data-stu-id="9b576-111">This functionality is not available for Azure China.</span></span>

> [!NOTE]
> <span data-ttu-id="9b576-112">Az Azure Key Vault kapcsolatos további információkért lásd: [Mi az Azure Key Vault?](key-vault-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="9b576-112">For more information about Azure Key Vault, see [What is Azure Key Vault?](key-vault-whatis.md)</span></span>  
>
> <span data-ttu-id="9b576-113">Egy alapszintű bemutató, amely tartalmazza a HSM által védett kulcsok kulcstároló létrehozása, lásd: [Ismerkedés az Azure Key Vault](key-vault-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9b576-113">For a getting started tutorial, which includes creating a key vault for HSM-protected keys, see [Get started with Azure Key Vault](key-vault-get-started.md).</span></span>
>
>

<span data-ttu-id="9b576-114">További információ létrehozása és egy HSM által védett kulcs átvitele hello Internet:</span><span class="sxs-lookup"><span data-stu-id="9b576-114">More information about generating and transferring an HSM-protected key over hello Internet:</span></span>

* <span data-ttu-id="9b576-115">Egy kapcsolat nélküli munkaállomást, amely csökkenti a támadási felület hello hello kulcs generálása.</span><span class="sxs-lookup"><span data-stu-id="9b576-115">You generate hello key from an offline workstation, which reduces hello attack surface.</span></span>
* <span data-ttu-id="9b576-116">hello kulcs és egy kulcs kulcscserekulcs (KEK), amely átvitelig titkosítva marad, amíg az Azure Key Vault HSM átvitt toohello nem titkosított.</span><span class="sxs-lookup"><span data-stu-id="9b576-116">hello key is encrypted with a Key Exchange Key (KEK), which stays encrypted until it is transferred toohello Azure Key Vault HSMs.</span></span> <span data-ttu-id="9b576-117">Csak hello titkosított verziót az Ön kulcsának hello eredeti munkaállomást hagyja.</span><span class="sxs-lookup"><span data-stu-id="9b576-117">Only hello encrypted version of your key leaves hello original workstation.</span></span>
* <span data-ttu-id="9b576-118">hello eszközkészlet tulajdonságainak megadása a bérlőkulcsot, amelyhez a fő toohello Azure Key Vault biztonságivilág van.</span><span class="sxs-lookup"><span data-stu-id="9b576-118">hello toolset sets properties on your tenant key that binds your key toohello Azure Key Vault security world.</span></span> <span data-ttu-id="9b576-119">Ezért után hello Azure Key Vault HSM kap, és fejti vissza a kulcsot, csak a hardveres biztonsági modulok használható.</span><span class="sxs-lookup"><span data-stu-id="9b576-119">So after hello Azure Key Vault HSMs receive and decrypt your key, only these HSMs can use it.</span></span> <span data-ttu-id="9b576-120">A kulcs nem exportálható.</span><span class="sxs-lookup"><span data-stu-id="9b576-120">Your key cannot be exported.</span></span> <span data-ttu-id="9b576-121">Ez a Thales HSM-ekről hello kötést.</span><span class="sxs-lookup"><span data-stu-id="9b576-121">This binding is enforced by hello Thales HSMs.</span></span>
* <span data-ttu-id="9b576-122">hello kulcscserekulcs (KEK), amely használt tooencrypt a kulcs hello Azure Key Vault HSM belül jön létre, és nem exportálható.</span><span class="sxs-lookup"><span data-stu-id="9b576-122">hello Key Exchange Key (KEK) that is used tooencrypt your key is generated inside hello Azure Key Vault HSMs and is not exportable.</span></span> <span data-ttu-id="9b576-123">hello HSM-EK biztosítják, hogy az nem hello KEK kívül hello HSM titkosítatlan verziója lehet.</span><span class="sxs-lookup"><span data-stu-id="9b576-123">hello HSMs enforce that there can be no clear version of hello KEK outside hello HSMs.</span></span> <span data-ttu-id="9b576-124">Ezenkívül hello eszközkészlet által kiadott igazolást tartalmaz a Thales e hello KEK nem exportálható, és egy eredeti HSM-en, a Thales által gyártott rendszer belül jött létre.</span><span class="sxs-lookup"><span data-stu-id="9b576-124">In addition, hello toolset includes attestation from Thales that hello KEK is not exportable and was generated inside a genuine HSM that was manufactured by Thales.</span></span>
* <span data-ttu-id="9b576-125">hello eszközkészlet igazolást tartalmaz arról, hogy hello biztonsági világ szintén egy eredeti Thales által gyártott HSM jön létre az Azure Key Vault Thales is.</span><span class="sxs-lookup"><span data-stu-id="9b576-125">hello toolset includes attestation from Thales that hello Azure Key Vault security world was also generated on a genuine HSM manufactured by Thales.</span></span> <span data-ttu-id="9b576-126">Az igazolás igazolja, hogy a Microsoft eredeti hardvereket használ tooyou.</span><span class="sxs-lookup"><span data-stu-id="9b576-126">This attestation proves tooyou that Microsoft is using genuine hardware.</span></span>
* <span data-ttu-id="9b576-127">A Microsoft külön kek használ, és biztonsági világot minden egyes földrajzi régióban.</span><span class="sxs-lookup"><span data-stu-id="9b576-127">Microsoft uses separate KEKs and separate Security Worlds in each geographical region.</span></span> <span data-ttu-id="9b576-128">Ez az elkülönítés biztosítja, hogy használható-e a kulcsot csak az adatközpontokban hello régióban, ahol titkosítva lett.</span><span class="sxs-lookup"><span data-stu-id="9b576-128">This separation ensures that your key can be used only in data centers in hello region in which you encrypted it.</span></span> <span data-ttu-id="9b576-129">Például egy Európai ügyfél kulcs nem használható az Észak-amerikai vagy ázsiai adatközpontokban.</span><span class="sxs-lookup"><span data-stu-id="9b576-129">For example, a key from a European customer cannot be used in data centers in North American or Asia.</span></span>

## <a name="more-information-about-thales-hsms-and-microsoft-services"></a><span data-ttu-id="9b576-130">További információ a Thales HSM-ekről és a Microsoft-szolgáltatások</span><span class="sxs-lookup"><span data-stu-id="9b576-130">More information about Thales HSMs and Microsoft services</span></span>
<span data-ttu-id="9b576-131">A Thales e-Security vezető globális szolgáltató az adattitkosítás és a számítógépes biztonsági megoldások toohello pénzügyi szolgáltatásokat, a csúcstechnológiai, a gyártási, a kormányzati és a technológiai szektorok részére.</span><span class="sxs-lookup"><span data-stu-id="9b576-131">Thales e-Security is a leading global provider of data encryption and cyber security solutions toohello financial services, high technology, manufacturing, government, and technology sectors.</span></span> <span data-ttu-id="9b576-132">Egy 40 éves tapasztalattal bíró védelmének vállalati és kormányzati információk Thales megoldásait négy hello öt legnagyobb energetikai és űrtechnikai vállalatok által használt.</span><span class="sxs-lookup"><span data-stu-id="9b576-132">With a 40-year track record of protecting corporate and government information, Thales solutions are used by four of hello five largest energy and aerospace companies.</span></span> <span data-ttu-id="9b576-133">A megoldások 22 NATO országok is használják, és biztonságos világszerte a pénzügyi tranzakciók több mint 80 %-át.</span><span class="sxs-lookup"><span data-stu-id="9b576-133">Their solutions are also used by 22 NATO countries, and secure more than 80 per cent of worldwide payment transactions.</span></span>

<span data-ttu-id="9b576-134">Microsoft állapotú Thales tooenhance hello argentin van közvetlenül az HSM-EK számára.</span><span class="sxs-lookup"><span data-stu-id="9b576-134">Microsoft has collaborated with Thales tooenhance hello state of art for HSMs.</span></span> <span data-ttu-id="9b576-135">Az ilyen fejlesztések lehetővé teszik a tooget hello tipikus üzemeltetett szolgáltatások előnyei lemondana szabályozható a kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="9b576-135">These enhancements enable you tooget hello typical benefits of hosted services without relinquishing control over your keys.</span></span> <span data-ttu-id="9b576-136">Pontosabban a fejlesztéseknek köszönhetően a Microsoft hello HSM-EK kezelése, így nem kell.</span><span class="sxs-lookup"><span data-stu-id="9b576-136">Specifically, these enhancements let Microsoft manage hello HSMs so that you do not have to.</span></span> <span data-ttu-id="9b576-137">Egy felhőalapú szolgáltatás, az Azure Key Vault rendkívül gyorsan igazodik toomeet, a szervezete használati napra.</span><span class="sxs-lookup"><span data-stu-id="9b576-137">As a cloud service, Azure Key Vault scales up at short notice toomeet your organization’s usage spikes.</span></span> <span data-ttu-id="9b576-138">At hello azonos időben, az Ön kulcsának védelmét a Microsoft HSM-jei: mivel hello kulcs létrehozása, és vigye tooMicrosoft a HSM-EK ellenőrzése alatt tartja a hello kulcs életciklusához kapcsolódó megőrzi.</span><span class="sxs-lookup"><span data-stu-id="9b576-138">At hello same time, your key is protected inside Microsoft’s HSMs: You retain control over hello key lifecycle because you generate hello key and transfer it tooMicrosoft’s HSMs.</span></span>

## <a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a><span data-ttu-id="9b576-139">Az Azure Key Vault a saját kulcs (használatának BYOK) megvalósítása kapcsolása</span><span class="sxs-lookup"><span data-stu-id="9b576-139">Implementing bring your own key (BYOK) for Azure Key Vault</span></span>
<span data-ttu-id="9b576-140">Használjon hello követő információkat és eljárásokat, ha a saját HSM által védett kulcs létrehozása és tooAzure Key Vault át fogja – hello kapcsolja a saját kulcs (használatának BYOK) forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="9b576-140">Use hello following information and procedures if you will generate your own HSM-protected key and then transfer it tooAzure Key Vault—hello bring your own key (BYOK) scenario.</span></span>

## <a name="prerequisites-for-byok"></a><span data-ttu-id="9b576-141">A BYOK előfeltételei</span><span class="sxs-lookup"><span data-stu-id="9b576-141">Prerequisites for BYOK</span></span>
<span data-ttu-id="9b576-142">Tekintse meg a következő táblázat az Előfeltételek listáját hello Azure Key Vault teheti a saját kulcs (használatának BYOK).</span><span class="sxs-lookup"><span data-stu-id="9b576-142">See hello following table for a list of prerequisites for bring your own key (BYOK) for Azure Key Vault.</span></span>

| <span data-ttu-id="9b576-143">Követelmény</span><span class="sxs-lookup"><span data-stu-id="9b576-143">Requirement</span></span> | <span data-ttu-id="9b576-144">További információ</span><span class="sxs-lookup"><span data-stu-id="9b576-144">More information</span></span> |
| --- | --- |
| <span data-ttu-id="9b576-145">Egy előfizetés tooAzure</span><span class="sxs-lookup"><span data-stu-id="9b576-145">A subscription tooAzure</span></span> |<span data-ttu-id="9b576-146">egy Azure Key Vault toocreate, Azure-előfizetés szükséges: [regisztráljon az ingyenes próbaverzióhoz](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="9b576-146">toocreate an Azure Key Vault, you need an Azure subscription: [Sign up for free trial](https://azure.microsoft.com/pricing/free-trial/)</span></span> |
| <span data-ttu-id="9b576-147">hello Key Vault prémium szintű szolgáltatási réteg toosupport HSM által védett kulcsokat</span><span class="sxs-lookup"><span data-stu-id="9b576-147">hello Azure Key Vault Premium service tier toosupport HSM-protected keys</span></span> |<span data-ttu-id="9b576-148">Az Azure Key Vault hello szolgáltatási szinteket és képességeire vonatkozó további információkért lásd: hello [Azure Key Vault díjszabása](https://azure.microsoft.com/pricing/details/key-vault/) webhelyet.</span><span class="sxs-lookup"><span data-stu-id="9b576-148">For more information about hello service tiers and capabilities for Azure Key Vault, see hello [Azure Key Vault Pricing](https://azure.microsoft.com/pricing/details/key-vault/) website.</span></span> |
| <span data-ttu-id="9b576-149">A Thales HSM, intelligens kártyák és a szoftvert</span><span class="sxs-lookup"><span data-stu-id="9b576-149">Thales HSM, smartcards, and support software</span></span> |<span data-ttu-id="9b576-150">Rendelkeznie kell tooa Thales hardveres biztonsági modul és a Thales HSM-ekről alapvető ismeretekre.</span><span class="sxs-lookup"><span data-stu-id="9b576-150">You must have access tooa Thales Hardware Security Module and basic operational knowledge of Thales HSMs.</span></span> <span data-ttu-id="9b576-151">Lásd: [Thales hardveres biztonsági modul](https://www.thales-esecurity.com/msrms/buy) kompatibilis modellek, vagy egy hardveres biztonsági MODULT, ha még nem rendelkezik ilyennel toopurchase hello listáját.</span><span class="sxs-lookup"><span data-stu-id="9b576-151">See [Thales Hardware Security Module](https://www.thales-esecurity.com/msrms/buy) for hello list of compatible models, or toopurchase an HSM if you do not have one.</span></span> |
| <span data-ttu-id="9b576-152">hello következő hardver- és:</span><span class="sxs-lookup"><span data-stu-id="9b576-152">hello following hardware and software:</span></span><ol><li><span data-ttu-id="9b576-153">Egy kapcsolat nélküli x64 munkaállomás, amelyen a Windows 7 és a Thales nShield szoftver legalább egy minimális Windows operációs rendszer verziója 11.50.</span><span class="sxs-lookup"><span data-stu-id="9b576-153">An offline x64 workstation with a minimum Windows operation system of Windows 7 and Thales nShield software that is at least version 11.50.</span></span><br/><br/><span data-ttu-id="9b576-154">Ha ez a munkaállomás Windows 7 rendszeren fut kell [Microsoft .NET-keretrendszer 4.5-ös verziójának telepítése](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</span><span class="sxs-lookup"><span data-stu-id="9b576-154">If this workstation runs Windows 7, you must [install Microsoft .NET Framework 4.5](http://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe).</span></span></li><li><span data-ttu-id="9b576-155">A munkaállomás csatlakoztatott toohello Internet és a Windows 7 minimális Windows operációs rendszert és [Azure PowerShell](/powershell/azure/overview) **minimális verziója 1.1.0-ás** telepítve.</span><span class="sxs-lookup"><span data-stu-id="9b576-155">A workstation that is connected toohello Internet and has a minimum Windows operating system of Windows 7 and [Azure PowerShell](/powershell/azure/overview) **minimum version 1.1.0** installed.</span></span></li><li><span data-ttu-id="9b576-156">Egy USB-meghajtó vagy más hordozható tárolóeszköz, amelyen legalább 16 MB szabad tárhely.</span><span class="sxs-lookup"><span data-stu-id="9b576-156">A USB drive or other portable storage device that has at least 16 MB free space.</span></span></li></ol> |<span data-ttu-id="9b576-157">Biztonsági okokból azt javasoljuk, hogy hello első munkaállomás nincs csatlakoztatott tooa hálózati.</span><span class="sxs-lookup"><span data-stu-id="9b576-157">For security reasons, we recommend that hello first workstation is not connected tooa network.</span></span> <span data-ttu-id="9b576-158">Azonban ez a javaslat nem szoftveres követelmény.</span><span class="sxs-lookup"><span data-stu-id="9b576-158">However, this recommendation is not programmatically enforced.</span></span><br/><br/><span data-ttu-id="9b576-159">Vegye figyelembe, hogy a hello utasításokban, ennek a munkaállomásnak hivatkozott tooas leválasztása hello munkaállomásra.</span><span class="sxs-lookup"><span data-stu-id="9b576-159">Note that in hello instructions that follow, this workstation is referred tooas hello disconnected workstation.</span></span></p></blockquote><br/><span data-ttu-id="9b576-160">Emellett ha a bérlőkulcsot egy üzemi hálózat, azt javasoljuk, hogy egy második, különálló munkaállomást toodownload hello eszközkészlet, valamint a feltöltési hello bérlői kulcsot használ.</span><span class="sxs-lookup"><span data-stu-id="9b576-160">In addition, if your tenant key is for a production network, we recommend that you use a second, separate workstation toodownload hello toolset and upload hello tenant key.</span></span> <span data-ttu-id="9b576-161">Tesztelési célokra használható, de hello munkaállomást, hello elsőt.</span><span class="sxs-lookup"><span data-stu-id="9b576-161">But for testing purposes, you can use hello same workstation as hello first one.</span></span><br/><br/><span data-ttu-id="9b576-162">Vegye figyelembe, hogy a hello utasításokban, erre a második munkaállomásra hivatkozott tooas hello internetre kapcsolódó munkaállomásra.</span><span class="sxs-lookup"><span data-stu-id="9b576-162">Note that in hello instructions that follow, this second workstation is referred tooas hello Internet-connected workstation.</span></span></p></blockquote><br/> |

## <a name="generate-and-transfer-your-key-tooazure-key-vault-hsm"></a><span data-ttu-id="9b576-163">Generate and transfer a kulcs tooAzure Key Vault HSM és</span><span class="sxs-lookup"><span data-stu-id="9b576-163">Generate and transfer your key tooAzure Key Vault HSM</span></span>
<span data-ttu-id="9b576-164">Akkor használja a következő öt lépéseket toogenerate hello, és a kulcs tooan Azure Key Vault HSM átviteli:</span><span class="sxs-lookup"><span data-stu-id="9b576-164">You will use hello following five steps toogenerate and transfer your key tooan Azure Key Vault HSM:</span></span>

* [<span data-ttu-id="9b576-165">1. lépés: Az internetre kapcsolódó munkaállomás előkészítése</span><span class="sxs-lookup"><span data-stu-id="9b576-165">Step 1: Prepare your Internet-connected workstation</span></span>](#step-1-prepare-your-internet-connected-workstation)
* [<span data-ttu-id="9b576-166">2. lépés: A kapcsolat nélküli munkaállomás előkészítése</span><span class="sxs-lookup"><span data-stu-id="9b576-166">Step 2: Prepare your disconnected workstation</span></span>](#step-2-prepare-your-disconnected-workstation)
* [<span data-ttu-id="9b576-167">3. lépés: A kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b576-167">Step 3: Generate your key</span></span>](#step-3-generate-your-key)
* [<span data-ttu-id="9b576-168">4. lépés: A kulcs Áthelyezés előkészítése</span><span class="sxs-lookup"><span data-stu-id="9b576-168">Step 4: Prepare your key for transfer</span></span>](#step-4-prepare-your-key-for-transfer)
* [<span data-ttu-id="9b576-169">5. lépés: A kulcs tooAzure Key Vault átvitele</span><span class="sxs-lookup"><span data-stu-id="9b576-169">Step 5: Transfer your key tooAzure Key Vault</span></span>](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a><span data-ttu-id="9b576-170">1. lépés: Az internetre kapcsolódó munkaállomás előkészítése</span><span class="sxs-lookup"><span data-stu-id="9b576-170">Step 1: Prepare your Internet-connected workstation</span></span>
<span data-ttu-id="9b576-171">Az hello az első lépés az alábbi eljárásokat az internethez csatlakoztatott toohello munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="9b576-171">For this first step, do hello following procedures on your workstation that is connected toohello Internet.</span></span>

### <a name="step-11-install-azure-powershell"></a><span data-ttu-id="9b576-172">1.1. lépés: Az Azure PowerShell telepítése</span><span class="sxs-lookup"><span data-stu-id="9b576-172">Step 1.1: Install Azure PowerShell</span></span>
<span data-ttu-id="9b576-173">Hello internethez csatlakoztatott munkaállomáson töltse le és telepítse hello Azure PowerShell-modult, amely tartalmazza az hello parancsmagok toomanage Azure Key Vault.</span><span class="sxs-lookup"><span data-stu-id="9b576-173">From hello Internet-connected workstation, download and install hello Azure PowerShell module that includes hello cmdlets toomanage Azure Key Vault.</span></span> <span data-ttu-id="9b576-174">Ehhez a 0.8.13 verzióját.</span><span class="sxs-lookup"><span data-stu-id="9b576-174">This requires a minimum version of 0.8.13.</span></span>

<span data-ttu-id="9b576-175">A telepítési utasításokért lásd: [hogyan tooinstall és konfigurálja az Azure Powershellt](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="9b576-175">For installation instructions, see [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

### <a name="step-12-get-your-azure-subscription-id"></a><span data-ttu-id="9b576-176">1.2. lépés: Az Azure előfizetés-azonosító lekérése</span><span class="sxs-lookup"><span data-stu-id="9b576-176">Step 1.2: Get your Azure subscription ID</span></span>
<span data-ttu-id="9b576-177">Indítson el egy Azure PowerShell-munkamenetet, és jelentkezzen be Azure-fiók tooyour hello a következő parancs használatával:</span><span class="sxs-lookup"><span data-stu-id="9b576-177">Start an Azure PowerShell session and sign in tooyour Azure account by using hello following command:</span></span>

        Add-AzureAccount
<span data-ttu-id="9b576-178">Hello előugró böngészőablakban adja meg a Azure-fiók felhasználói nevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="9b576-178">In hello pop-up browser window, enter your Azure account user name and password.</span></span> <span data-ttu-id="9b576-179">Ezután használja az hello [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) parancs:</span><span class="sxs-lookup"><span data-stu-id="9b576-179">Then, use hello [Get-AzureSubscription](/powershell/module/azure/get-azuresubscription?view=azuresmps-3.7.0) command:</span></span>

        Get-AzureSubscription
<span data-ttu-id="9b576-180">Hello kimenetből keresse meg a hello Azonosítóját használja-e az Azure Key Vault hello az előfizetéshez.</span><span class="sxs-lookup"><span data-stu-id="9b576-180">From hello output, locate hello ID for hello subscription you will use for Azure Key Vault.</span></span> <span data-ttu-id="9b576-181">Később szüksége lesz az előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="9b576-181">You will need this subscription ID later.</span></span>

<span data-ttu-id="9b576-182">Ne zárja be a hello Azure PowerShell-ablakot.</span><span class="sxs-lookup"><span data-stu-id="9b576-182">Do not close hello Azure PowerShell window.</span></span>

### <a name="step-13-download-hello-byok-toolset-for-azure-key-vault"></a><span data-ttu-id="9b576-183">1.3. lépés: Az Azure Key Vault hello BYOK eszközkészlet letöltése</span><span class="sxs-lookup"><span data-stu-id="9b576-183">Step 1.3: Download hello BYOK toolset for Azure Key Vault</span></span>
<span data-ttu-id="9b576-184">Nyissa meg a Microsoft Download Center toohello és [hello Azure Key Vault BYOK eszközkészlet letöltése](http://www.microsoft.com/download/details.aspx?id=45345) a földrajzi régióban vagy Azure példányát.</span><span class="sxs-lookup"><span data-stu-id="9b576-184">Go toohello Microsoft Download Center and [download hello Azure Key Vault BYOK toolset](http://www.microsoft.com/download/details.aspx?id=45345) for your geographic region or instance of Azure.</span></span> <span data-ttu-id="9b576-185">Hello alábbi információkat tooidentify hello csomag neve toodownload és a megfelelő SHA-256 kivonatoló csomag:</span><span class="sxs-lookup"><span data-stu-id="9b576-185">Use hello following information tooidentify hello package name toodownload and its corresponding SHA-256 package hash:</span></span>

- - -
<span data-ttu-id="9b576-186">**Egyesült Államok:**</span><span class="sxs-lookup"><span data-stu-id="9b576-186">**United States:**</span></span>

<span data-ttu-id="9b576-187">KeyVault-BYOK-eszközök – Egyesült States.zip</span><span class="sxs-lookup"><span data-stu-id="9b576-187">KeyVault-BYOK-Tools-UnitedStates.zip</span></span>

<span data-ttu-id="9b576-188">760EE9BD6445C87CFF0E8B032577118704B3BEAA045AA55977C10EF68BC67E2B</span><span class="sxs-lookup"><span data-stu-id="9b576-188">760EE9BD6445C87CFF0E8B032577118704B3BEAA045AA55977C10EF68BC67E2B</span></span>

- - -
<span data-ttu-id="9b576-189">**Európa:**</span><span class="sxs-lookup"><span data-stu-id="9b576-189">**Europe:**</span></span>

<span data-ttu-id="9b576-190">KeyVault-BYOK-eszközök – Europe.zip</span><span class="sxs-lookup"><span data-stu-id="9b576-190">KeyVault-BYOK-Tools-Europe.zip</span></span>

<span data-ttu-id="9b576-191">7A64B94225F59B847C5C27C2200BAD7D16C901E1687767EDBBB8B09BB285011D</span><span class="sxs-lookup"><span data-stu-id="9b576-191">7A64B94225F59B847C5C27C2200BAD7D16C901E1687767EDBBB8B09BB285011D</span></span>

- - -
<span data-ttu-id="9b576-192">**Ázsia:**</span><span class="sxs-lookup"><span data-stu-id="9b576-192">**Asia:**</span></span>

<span data-ttu-id="9b576-193">KeyVault-BYOK-eszközök – AsiaPacific.zip</span><span class="sxs-lookup"><span data-stu-id="9b576-193">KeyVault-BYOK-Tools-AsiaPacific.zip</span></span>

<span data-ttu-id="9b576-194">813DC94B23079CF7A5CEA71D5B444E86B292F463C53EE47AED25D4F7CD58E7D8</span><span class="sxs-lookup"><span data-stu-id="9b576-194">813DC94B23079CF7A5CEA71D5B444E86B292F463C53EE47AED25D4F7CD58E7D8</span></span>

- - -
<span data-ttu-id="9b576-195">**Latin-Amerika:**</span><span class="sxs-lookup"><span data-stu-id="9b576-195">**Latin America:**</span></span>

<span data-ttu-id="9b576-196">KeyVault-BYOK-eszközök – LatinAmerica.zip</span><span class="sxs-lookup"><span data-stu-id="9b576-196">KeyVault-BYOK-Tools-LatinAmerica.zip</span></span>

<span data-ttu-id="9b576-197">3F29069E3500F95C0E156F4B8914E1DC60C20FB64B464306A299EA5145D755C0</span><span class="sxs-lookup"><span data-stu-id="9b576-197">3F29069E3500F95C0E156F4B8914E1DC60C20FB64B464306A299EA5145D755C0</span></span>

- - -
<span data-ttu-id="9b576-198">**Japán:**</span><span class="sxs-lookup"><span data-stu-id="9b576-198">**Japan:**</span></span>

<span data-ttu-id="9b576-199">KeyVault-BYOK-eszközök – Japan.zip</span><span class="sxs-lookup"><span data-stu-id="9b576-199">KeyVault-BYOK-Tools-Japan.zip</span></span>

<span data-ttu-id="9b576-200">453FFEA2F8F410720B68B8BAC4CF79135A7F37F4E491FF840BE9E69E88A98C90</span><span class="sxs-lookup"><span data-stu-id="9b576-200">453FFEA2F8F410720B68B8BAC4CF79135A7F37F4E491FF840BE9E69E88A98C90</span></span>

- - -
<span data-ttu-id="9b576-201">**Korea:**</span><span class="sxs-lookup"><span data-stu-id="9b576-201">**Korea:**</span></span>

<span data-ttu-id="9b576-202">KeyVault-BYOK-eszközök – Korea.zip</span><span class="sxs-lookup"><span data-stu-id="9b576-202">KeyVault-BYOK-Tools-Korea.zip</span></span>

<span data-ttu-id="9b576-203">C17B7E93224DA80F5668E09CF7DAE2F92527E8226179995BBE2E43DA4323595A</span><span class="sxs-lookup"><span data-stu-id="9b576-203">C17B7E93224DA80F5668E09CF7DAE2F92527E8226179995BBE2E43DA4323595A</span></span>

- - -
<span data-ttu-id="9b576-204">**Ausztrália:**</span><span class="sxs-lookup"><span data-stu-id="9b576-204">**Australia:**</span></span>

<span data-ttu-id="9b576-205">KeyVault-BYOK-eszközök – Australia.zip</span><span class="sxs-lookup"><span data-stu-id="9b576-205">KeyVault-BYOK-Tools-Australia.zip</span></span>

<span data-ttu-id="9b576-206">4AD893396E86F2D2A71682876A6A8EA59E3C7895BEAD2F7E7C8516682582C34B</span><span class="sxs-lookup"><span data-stu-id="9b576-206">4AD893396E86F2D2A71682876A6A8EA59E3C7895BEAD2F7E7C8516682582C34B</span></span>

- - -
[<span data-ttu-id="9b576-207">**Az Azure Government:**</span><span class="sxs-lookup"><span data-stu-id="9b576-207">**Azure Government:**</span></span>](https://azure.microsoft.com/features/gov/)

<span data-ttu-id="9b576-208">KeyVault-BYOK-eszközök – USGovCloud.zip</span><span class="sxs-lookup"><span data-stu-id="9b576-208">KeyVault-BYOK-Tools-USGovCloud.zip</span></span>

<span data-ttu-id="9b576-209">3AAE1A96B9D15B899B8126CFC0380719EB54FDF2EA94489B43FAD21ECC745F64</span><span class="sxs-lookup"><span data-stu-id="9b576-209">3AAE1A96B9D15B899B8126CFC0380719EB54FDF2EA94489B43FAD21ECC745F64</span></span>

- - -
<span data-ttu-id="9b576-210">**Egyesült Államok kormánya DOD:**</span><span class="sxs-lookup"><span data-stu-id="9b576-210">**US Government DOD:**</span></span>

<span data-ttu-id="9b576-211">KeyVault-BYOK-eszközök – USGovernmentDoD.zip</span><span class="sxs-lookup"><span data-stu-id="9b576-211">KeyVault-BYOK-Tools-USGovernmentDoD.zip</span></span>

<span data-ttu-id="9b576-212">A61E78297B0732DF2682FDE63D7B572CE4D23B0BC27CC48AFF620BD060BB9E9D</span><span class="sxs-lookup"><span data-stu-id="9b576-212">A61E78297B0732DF2682FDE63D7B572CE4D23B0BC27CC48AFF620BD060BB9E9D</span></span>

- - -
<span data-ttu-id="9b576-213">**Kanada:**</span><span class="sxs-lookup"><span data-stu-id="9b576-213">**Canada:**</span></span>

<span data-ttu-id="9b576-214">KeyVault-BYOK-eszközök – Canada.zip</span><span class="sxs-lookup"><span data-stu-id="9b576-214">KeyVault-BYOK-Tools-Canada.zip</span></span>

<span data-ttu-id="9b576-215">30B87A0BA8208F6B7241C30C794FED1C370D7445ACA179685816E4E156CD2AF7</span><span class="sxs-lookup"><span data-stu-id="9b576-215">30B87A0BA8208F6B7241C30C794FED1C370D7445ACA179685816E4E156CD2AF7</span></span>

- - -
<span data-ttu-id="9b576-216">**Németország:**</span><span class="sxs-lookup"><span data-stu-id="9b576-216">**Germany:**</span></span>

<span data-ttu-id="9b576-217">KeyVault-BYOK-eszközök – Germany.zip</span><span class="sxs-lookup"><span data-stu-id="9b576-217">KeyVault-BYOK-Tools-Germany.zip</span></span>

<span data-ttu-id="9b576-218">5E3E4AA54715E4F93C3C145035B18275B7C6815A06D7ABB212E7FADBF2929261</span><span class="sxs-lookup"><span data-stu-id="9b576-218">5E3E4AA54715E4F93C3C145035B18275B7C6815A06D7ABB212E7FADBF2929261</span></span>

- - -
<span data-ttu-id="9b576-219">**India:**</span><span class="sxs-lookup"><span data-stu-id="9b576-219">**India:**</span></span>

<span data-ttu-id="9b576-220">KeyVault-BYOK-eszközök – India.zip</span><span class="sxs-lookup"><span data-stu-id="9b576-220">KeyVault-BYOK-Tools-India.zip</span></span>

<span data-ttu-id="9b576-221">136733A6C6A71D75571BB80819B3D55A9B83CCAD5C996C686BC5682A3F369BF7</span><span class="sxs-lookup"><span data-stu-id="9b576-221">136733A6C6A71D75571BB80819B3D55A9B83CCAD5C996C686BC5682A3F369BF7</span></span>

- - -
<span data-ttu-id="9b576-222">**Egyesült Királyság:**</span><span class="sxs-lookup"><span data-stu-id="9b576-222">**United Kingdom:**</span></span>

<span data-ttu-id="9b576-223">KeyVault-BYOK-eszközök – UnitedKingdom.zip</span><span class="sxs-lookup"><span data-stu-id="9b576-223">KeyVault-BYOK-Tools-UnitedKingdom.zip</span></span>

<span data-ttu-id="9b576-224">ED331A6F1D34A402317D3F27D5396046AF0E5C2D44B5D10CCCE293472942D268</span><span class="sxs-lookup"><span data-stu-id="9b576-224">ED331A6F1D34A402317D3F27D5396046AF0E5C2D44B5D10CCCE293472942D268</span></span>

- - -

<span data-ttu-id="9b576-225">a letöltött BYOK eszközkészlet, az Azure PowerShell-munkamenetben, használjon hello toovalidate hello sértetlenségének [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="9b576-225">toovalidate hello integrity of your downloaded BYOK toolset, from your Azure PowerShell session, use hello [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) cmdlet.</span></span>

    Get-FileHash KeyVault-BYOK-Tools-*.zip

<span data-ttu-id="9b576-226">hello eszközkészlet hello következőket tartalmazza:</span><span class="sxs-lookup"><span data-stu-id="9b576-226">hello toolset includes hello following:</span></span>

* <span data-ttu-id="9b576-227">Egy kulcscserekulcs (KEK) csomag, amelynek a neve a **BYOK-KEK - pkg-.**</span><span class="sxs-lookup"><span data-stu-id="9b576-227">A Key Exchange Key (KEK) package that has a name beginning with **BYOK-KEK-pkg-.**</span></span>
* <span data-ttu-id="9b576-228">Egy Biztonságivilág-csomag, amelynek a neve a **BYOK-SecurityWorld - pkg-.**</span><span class="sxs-lookup"><span data-stu-id="9b576-228">A Security World package that has a name beginning with **BYOK-SecurityWorld-pkg-.**</span></span>
* <span data-ttu-id="9b576-229">Nevű python-parancsfájl **verifykeypackage.py.**</span><span class="sxs-lookup"><span data-stu-id="9b576-229">A python script named **verifykeypackage.py.**</span></span>
* <span data-ttu-id="9b576-230">Egy parancssori végrehajtható fájl **KeyTransferRemote.exe** és kapcsolódó dll-fájlok.</span><span class="sxs-lookup"><span data-stu-id="9b576-230">A command-line executable file named **KeyTransferRemote.exe** and associated DLLs.</span></span>
* <span data-ttu-id="9b576-231">A Visual C++ terjeszthető csomag **vcredist_x64.exe.**</span><span class="sxs-lookup"><span data-stu-id="9b576-231">A Visual C++ Redistributable Package, named **vcredist_x64.exe.**</span></span>

<span data-ttu-id="9b576-232">Másolja a hello csomag tooa USB-meghajtó vagy más hordozható tárolóeszközre.</span><span class="sxs-lookup"><span data-stu-id="9b576-232">Copy hello package tooa USB drive or other portable storage.</span></span>

## <a name="step-2-prepare-your-disconnected-workstation"></a><span data-ttu-id="9b576-233">2. lépés: A kapcsolat nélküli munkaállomás előkészítése</span><span class="sxs-lookup"><span data-stu-id="9b576-233">Step 2: Prepare your disconnected workstation</span></span>
<span data-ttu-id="9b576-234">Az hello ebben a második lépésben az alábbi eljárásokat hello munkaállomáson, amely nem csatlakoztatott tooa hálózat (hello Internet vagy a belső hálózaton).</span><span class="sxs-lookup"><span data-stu-id="9b576-234">For this second step, do hello following procedures on hello workstation that is not connected tooa network (either hello Internet or your internal network).</span></span>

### <a name="step-21-prepare-hello-disconnected-workstation-with-thales-hsm"></a><span data-ttu-id="9b576-235">2.1. lépés: A Thales HSM-mel leválasztása hello munkaállomás előkészítése</span><span class="sxs-lookup"><span data-stu-id="9b576-235">Step 2.1: Prepare hello disconnected workstation with Thales HSM</span></span>
<span data-ttu-id="9b576-236">Hello nCipher (Thales) támogatószoftvert egy Windows számítógépre telepítse, majd csatolja a Thales HSM-en toothat számítógép.</span><span class="sxs-lookup"><span data-stu-id="9b576-236">Install hello nCipher (Thales) support software on a Windows computer, and then attach a Thales HSM toothat computer.</span></span>

<span data-ttu-id="9b576-237">Győződjön meg arról, hogy a Thales-eszközök hello az elérési úthoz (**%nfast_home%\bin**).</span><span class="sxs-lookup"><span data-stu-id="9b576-237">Ensure that hello Thales tools are in your path (**%nfast_home%\bin**).</span></span> <span data-ttu-id="9b576-238">Írja be például a következő hello:</span><span class="sxs-lookup"><span data-stu-id="9b576-238">For example, type hello following:</span></span>

        set PATH=%PATH%;"%nfast_home%\bin"

<span data-ttu-id="9b576-239">További információkért lásd: hello Thales HSM-en mellékelt hello felhasználói útmutatót.</span><span class="sxs-lookup"><span data-stu-id="9b576-239">For more information, see hello user guide included with hello Thales HSM.</span></span>

### <a name="step-22-install-hello-byok-toolset-on-hello-disconnected-workstation"></a><span data-ttu-id="9b576-240">2.2. lépés: Telepítse hello BYOK eszközkészlet leválasztása hello munkaállomáson</span><span class="sxs-lookup"><span data-stu-id="9b576-240">Step 2.2: Install hello BYOK toolset on hello disconnected workstation</span></span>
<span data-ttu-id="9b576-241">Hello BYOK eszközkészletcsomagot másolása hello USB-meghajtó vagy más hordozható tárolóeszköz, és ezután hello a következő:</span><span class="sxs-lookup"><span data-stu-id="9b576-241">Copy hello BYOK toolset package from hello USB drive or other portable storage, and then do hello following:</span></span>

1. <span data-ttu-id="9b576-242">Csomagolja ki hello fájlokat hello letöltött csomag egy tetszőleges mappába.</span><span class="sxs-lookup"><span data-stu-id="9b576-242">Extract hello files from hello downloaded package into any folder.</span></span>
2. <span data-ttu-id="9b576-243">Ebből a könyvtárból futtassa a vcredist_x64.exe.</span><span class="sxs-lookup"><span data-stu-id="9b576-243">From that folder, run vcredist_x64.exe.</span></span>
3. <span data-ttu-id="9b576-244">Útmutatás alapján hello toohello hello Visual C++ futtatókörnyezeti összetevők telepítése a Visual Studio 2013.</span><span class="sxs-lookup"><span data-stu-id="9b576-244">Follow hello instructions toohello install hello Visual C++ runtime components for Visual Studio 2013.</span></span>

## <a name="step-3-generate-your-key"></a><span data-ttu-id="9b576-245">3. lépés: A kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b576-245">Step 3: Generate your key</span></span>
<span data-ttu-id="9b576-246">Az hello harmadik ezt a lépést, az alábbi eljárásokat leválasztva hello munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="9b576-246">For this third step, do hello following procedures on hello disconnected workstation.</span></span> <span data-ttu-id="9b576-247">toocomplete ebben a lépésben a HSM-ből módban kell lennie az inicializálás.</span><span class="sxs-lookup"><span data-stu-id="9b576-247">toocomplete this step your HSM must be in initialization mode.</span></span> 


### <a name="step-31-change-hello-hsm-mode-tooi"></a><span data-ttu-id="9b576-248">3.1. lépés: Hello HSM mód too'I módosítása "</span><span class="sxs-lookup"><span data-stu-id="9b576-248">Step 3.1: Change hello HSM mode too'I'</span></span>
<span data-ttu-id="9b576-249">A Thales nShield él, toochange hello mód használata: 1.</span><span class="sxs-lookup"><span data-stu-id="9b576-249">If you are using Thales nShield Edge, toochange hello mode: 1.</span></span> <span data-ttu-id="9b576-250">Hello mód gomb toohighlight hello szükséges módot használja.</span><span class="sxs-lookup"><span data-stu-id="9b576-250">Use hello Mode button toohighlight hello required mode.</span></span> <span data-ttu-id="9b576-251">2.</span><span class="sxs-lookup"><span data-stu-id="9b576-251">2.</span></span> <span data-ttu-id="9b576-252">Néhány másodpercen belül tartsa lenyomva a hello világos gomb néhány másodpercig.</span><span class="sxs-lookup"><span data-stu-id="9b576-252">Within a few seconds, press and hold hello Clear button for a couple of seconds.</span></span> <span data-ttu-id="9b576-253">Ha hello mód hello új mód villogó LED leáll, és bekapcsolásával marad.</span><span class="sxs-lookup"><span data-stu-id="9b576-253">If hello mode changes, hello new mode’s LED stops flashing and remains lit.</span></span> <span data-ttu-id="9b576-254">hello állapot LED szabálytalan flash előfordulhat, hogy néhány másodpercig, és rendszeresen, amikor a hello eszköz készen áll majd tokenkódot.</span><span class="sxs-lookup"><span data-stu-id="9b576-254">hello Status LED might flash irregularly for a few seconds and then flashes regularly when hello device is ready.</span></span> <span data-ttu-id="9b576-255">Ellenkező esetben hello eszköz marad hello aktuális módban, hello megfelelő módot LED bekapcsolásával.</span><span class="sxs-lookup"><span data-stu-id="9b576-255">Otherwise, hello device remains in hello current mode, with hello appropriate mode LED lit.</span></span>

### <a name="step-32-create-a-security-world"></a><span data-ttu-id="9b576-256">3.2. lépés: Biztonsági világ létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b576-256">Step 3.2: Create a security world</span></span>
<span data-ttu-id="9b576-257">Nyisson meg egy parancsablakot, és futtassa a Thales új világot létrehozó programját hello.</span><span class="sxs-lookup"><span data-stu-id="9b576-257">Start a command prompt and run hello Thales new-world program.</span></span>

    new-world.exe --initialize --cipher-suite=DLf1024s160mRijndael --module=1 --acs-quorum=2/3

<span data-ttu-id="9b576-258">Ez a program létrehoz egy **Biztonságivilág** fájl: % NFAST_KMDATA%\local\world, amely toohello C:\ProgramData\nCipher\Key Management Data\local mappának felel meg.</span><span class="sxs-lookup"><span data-stu-id="9b576-258">This program creates a **Security World** file at %NFAST_KMDATA%\local\world, which corresponds toohello C:\ProgramData\nCipher\Key Management Data\local folder.</span></span> <span data-ttu-id="9b576-259">Hello kvóruma eltérő értékeket is használhat, de ebben a példában, tehát felszólító tooenter három üres kártyát és mindegyikhez PIN-kód.</span><span class="sxs-lookup"><span data-stu-id="9b576-259">You can use different values for hello quorum but in our example, you’re prompted tooenter three blank cards and pins for each one.</span></span> <span data-ttu-id="9b576-260">Bármelyik két kártyának adjon teljes hozzáférés toohello biztonságivilág.</span><span class="sxs-lookup"><span data-stu-id="9b576-260">Then, any two cards give full access toohello security world.</span></span> <span data-ttu-id="9b576-261">Ezek a kártyák lesznek hello **rendszergazdai Kártyakészlete** hello új biztonsági világ számára.</span><span class="sxs-lookup"><span data-stu-id="9b576-261">These cards become hello **Administrator Card Set** for hello new security world.</span></span>

<span data-ttu-id="9b576-262">Majd hello a következő:</span><span class="sxs-lookup"><span data-stu-id="9b576-262">Then do hello following:</span></span>

* <span data-ttu-id="9b576-263">Készítsen biztonsági másolatot a világfájlról hello.</span><span class="sxs-lookup"><span data-stu-id="9b576-263">Back up hello world file.</span></span> <span data-ttu-id="9b576-264">Biztonságos és hello world fájl, hello rendszergazdai kártyák és a PIN-kódok védelmében, és győződjön meg arról, hogy egy személy önmagában nem rendelkezik-e a hozzáférés toomore, mint egy kártya.</span><span class="sxs-lookup"><span data-stu-id="9b576-264">Secure and protect hello world file, hello Administrator Cards, and their pins, and make sure that no single person has access toomore than one card.</span></span>

### <a name="step-33-change-hello-hsm-mode-tooo"></a><span data-ttu-id="9b576-265">3.3. lépés: Hello HSM mód too'O módosítása "</span><span class="sxs-lookup"><span data-stu-id="9b576-265">Step 3.3: Change hello HSM mode too'O'</span></span>
<span data-ttu-id="9b576-266">A Thales nShield él, toochange hello mód használata: 1.</span><span class="sxs-lookup"><span data-stu-id="9b576-266">If you are using Thales nShield Edge, toochange hello mode: 1.</span></span> <span data-ttu-id="9b576-267">Hello mód gomb toohighlight hello szükséges módot használja.</span><span class="sxs-lookup"><span data-stu-id="9b576-267">Use hello Mode button toohighlight hello required mode.</span></span> <span data-ttu-id="9b576-268">2.</span><span class="sxs-lookup"><span data-stu-id="9b576-268">2.</span></span> <span data-ttu-id="9b576-269">Néhány másodpercen belül tartsa lenyomva a hello világos gomb néhány másodpercig.</span><span class="sxs-lookup"><span data-stu-id="9b576-269">Within a few seconds, press and hold hello Clear button for a couple of seconds.</span></span> <span data-ttu-id="9b576-270">Ha hello mód hello új mód villogó LED leáll, és bekapcsolásával marad.</span><span class="sxs-lookup"><span data-stu-id="9b576-270">If hello mode changes, hello new mode’s LED stops flashing and remains lit.</span></span> <span data-ttu-id="9b576-271">hello állapot LED szabálytalan flash előfordulhat, hogy néhány másodpercig, és rendszeresen, amikor a hello eszköz készen áll majd tokenkódot.</span><span class="sxs-lookup"><span data-stu-id="9b576-271">hello Status LED might flash irregularly for a few seconds and then flashes regularly when hello device is ready.</span></span> <span data-ttu-id="9b576-272">Ellenkező esetben hello eszköz marad hello aktuális módban, hello megfelelő módot LED bekapcsolásával.</span><span class="sxs-lookup"><span data-stu-id="9b576-272">Otherwise, hello device remains in hello current mode, with hello appropriate mode LED lit.</span></span>


### <a name="step-34-validate-hello-downloaded-package"></a><span data-ttu-id="9b576-273">3.4. lépés: Hello letöltött csomag ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="9b576-273">Step 3.4: Validate hello downloaded package</span></span>
<span data-ttu-id="9b576-274">Ez a lépés nem kötelező, de ajánlott hello következő ellenőrizheti:</span><span class="sxs-lookup"><span data-stu-id="9b576-274">This step is optional but recommended so that you can validate hello following:</span></span>

* <span data-ttu-id="9b576-275">hello hello eszközkészletben található kulcscserekulcs egy eredeti Thales HSM-en lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="9b576-275">hello Key Exchange Key that is included in hello toolset has been generated from a genuine Thales HSM.</span></span>
* <span data-ttu-id="9b576-276">hello hello biztonsági világának hello eszközkészletben található kivonata egy eredeti Thales HSM-en lett létrehozva.</span><span class="sxs-lookup"><span data-stu-id="9b576-276">hello hash of hello Security World that is included in hello toolset has been generated in a genuine Thales HSM.</span></span>
* <span data-ttu-id="9b576-277">hello kulcscserekulcs nem exportálható.</span><span class="sxs-lookup"><span data-stu-id="9b576-277">hello Key Exchange Key is non-exportable.</span></span>

> [!NOTE]
> <span data-ttu-id="9b576-278">toovalidate hello letöltött csomag, hello HSM csatlakoztatva kell lennie, az alkalmazás bekapcsolja, és rendelkeznie kell egy biztonsági világgal (például a most létrehozott egy hello).</span><span class="sxs-lookup"><span data-stu-id="9b576-278">toovalidate hello downloaded package, hello HSM must be connected, powered on, and must have a security world on it (such as hello one you’ve just created).</span></span>
>
>

<span data-ttu-id="9b576-279">toovalidate hello letöltött csomag:</span><span class="sxs-lookup"><span data-stu-id="9b576-279">toovalidate hello downloaded package:</span></span>

1. <span data-ttu-id="9b576-280">Hello verifykeypackage.py parancsfájlt futtassa hello következők egyike, attól függően, hogy a földrajzi régióban vagy Azure példánya beírásával:</span><span class="sxs-lookup"><span data-stu-id="9b576-280">Run hello verifykeypackage.py script by typing one of hello following, depending on your geographic region or instance of Azure:</span></span>

   * <span data-ttu-id="9b576-281">Észak-amerikai:</span><span class="sxs-lookup"><span data-stu-id="9b576-281">For North America:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
   * <span data-ttu-id="9b576-282">Európa esetében:</span><span class="sxs-lookup"><span data-stu-id="9b576-282">For Europe:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
   * <span data-ttu-id="9b576-283">Ázsia esetében:</span><span class="sxs-lookup"><span data-stu-id="9b576-283">For Asia:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
   * <span data-ttu-id="9b576-284">Latin-Amerika: a</span><span class="sxs-lookup"><span data-stu-id="9b576-284">For Latin America:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
   * <span data-ttu-id="9b576-285">A japán:</span><span class="sxs-lookup"><span data-stu-id="9b576-285">For Japan:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
   * <span data-ttu-id="9b576-286">Koreai:</span><span class="sxs-lookup"><span data-stu-id="9b576-286">For Korea:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-KOREA-1 -w BYOK-SecurityWorld-pkg-KOREA-1
   * <span data-ttu-id="9b576-287">Ausztráliában:</span><span class="sxs-lookup"><span data-stu-id="9b576-287">For Australia:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
   * <span data-ttu-id="9b576-288">A [Azure Government](https://azure.microsoft.com/features/gov/), használó Azure hello Egyesült Államokbeli kormányzati példánya:</span><span class="sxs-lookup"><span data-stu-id="9b576-288">For [Azure Government](https://azure.microsoft.com/features/gov/), which uses hello US government instance of Azure:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
   * <span data-ttu-id="9b576-289">Az Amerikai Egyesült Államok kormánya DOD:</span><span class="sxs-lookup"><span data-stu-id="9b576-289">For US Government DOD:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USDOD-1 -w BYOK-SecurityWorld-pkg-USDOD-1
   * <span data-ttu-id="9b576-290">Kanada esetében:</span><span class="sxs-lookup"><span data-stu-id="9b576-290">For Canada:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
   * <span data-ttu-id="9b576-291">Németország:</span><span class="sxs-lookup"><span data-stu-id="9b576-291">For Germany:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
   * <span data-ttu-id="9b576-292">A India:</span><span class="sxs-lookup"><span data-stu-id="9b576-292">For India:</span></span>

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
     > [!TIP]
     > <span data-ttu-id="9b576-293">hello Thales szoftver tartalmaz python %NFAST_HOME%\python\bin címen</span><span class="sxs-lookup"><span data-stu-id="9b576-293">hello Thales software includes python at %NFAST_HOME%\python\bin</span></span>
     >
     >
2. <span data-ttu-id="9b576-294">Ellenőrizze, hogy látható-e hello következő, az ellenőrzés sikerességét jelző: **Result: SUCCESS**</span><span class="sxs-lookup"><span data-stu-id="9b576-294">Confirm that you see hello following, which indicates successful validation: **Result: SUCCESS**</span></span>

<span data-ttu-id="9b576-295">A parancsfájl ellenőrzi az aláírói láncot hello a Thales-gyökérkulcsig toohello fel.</span><span class="sxs-lookup"><span data-stu-id="9b576-295">This script validates hello signer chain up toohello Thales root key.</span></span> <span data-ttu-id="9b576-296">a legfelső szintű kulcs hello kivonatoló beágyazott hello parancsfájl és az értéket kell **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**.</span><span class="sxs-lookup"><span data-stu-id="9b576-296">hello hash of this root key is embedded in hello script and its value should be **59178a47 de508c3f 291277ee 184f46c4 f1d9c639**.</span></span> <span data-ttu-id="9b576-297">Azt is ellenőrizheti ezt az értéket külön-külön hello felkeresésével [Thales webhelyén](http://www.thalesesec.com/).</span><span class="sxs-lookup"><span data-stu-id="9b576-297">You can also confirm this value separately by visiting hello [Thales website](http://www.thalesesec.com/).</span></span>

<span data-ttu-id="9b576-298">Most már készen áll a toocreate egy új kulcsot.</span><span class="sxs-lookup"><span data-stu-id="9b576-298">You’re now ready toocreate a new key.</span></span>

### <a name="step-35-create-a-new-key"></a><span data-ttu-id="9b576-299">3.5. lépés: Új kulcs létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b576-299">Step 3.5: Create a new key</span></span>
<span data-ttu-id="9b576-300">Hozzon létre egy kulcsot a hello Thales **generatekey** program.</span><span class="sxs-lookup"><span data-stu-id="9b576-300">Generate a key by using hello Thales **generatekey** program.</span></span>

<span data-ttu-id="9b576-301">Futtassa a következő parancs toogenerate hello kulcs hello:</span><span class="sxs-lookup"><span data-stu-id="9b576-301">Run hello following command toogenerate hello key:</span></span>

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

<span data-ttu-id="9b576-302">Ez a parancs futtatásakor használja ezeket az utasításokat:</span><span class="sxs-lookup"><span data-stu-id="9b576-302">When you run this command, use these instructions:</span></span>

* <span data-ttu-id="9b576-303">hello paraméter *védelme* toohello értéket meg kell **modul**látható módon.</span><span class="sxs-lookup"><span data-stu-id="9b576-303">hello parameter *protect* must be set toohello value **module**, as shown.</span></span> <span data-ttu-id="9b576-304">Ez a modul által védett kulcsot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="9b576-304">This creates a module-protected key.</span></span> <span data-ttu-id="9b576-305">hello BYOK eszközkészlet nem támogatja az OCS által védett kulcsokat.</span><span class="sxs-lookup"><span data-stu-id="9b576-305">hello BYOK toolset does not support OCS-protected keys.</span></span>
* <span data-ttu-id="9b576-306">Cserélje le a hello értékének *contosokey* a hello **ident** és **plainname** bármilyen karakterlánc-értékkel.</span><span class="sxs-lookup"><span data-stu-id="9b576-306">Replace hello value of *contosokey* for hello **ident** and **plainname** with any string value.</span></span> <span data-ttu-id="9b576-307">az adminisztratív terhek toominimize hello kockázatok csökkentése és a hibák, azt javasoljuk, hogy hello értékeként is azonos értéket használja.</span><span class="sxs-lookup"><span data-stu-id="9b576-307">toominimize administrative overheads and reduce hello risk of errors, we recommend that you use hello same value for both.</span></span> <span data-ttu-id="9b576-308">Hello **ident** érték csak számokat, kötőjeleket és kisbetűket tartalmazhat.</span><span class="sxs-lookup"><span data-stu-id="9b576-308">hello **ident** value must contain only numbers, dashes, and lower case letters.</span></span>
* <span data-ttu-id="9b576-309">hello pubexp üresen maradt (alapértelmezett) ebben a példában, de az adott értékeket adhat meg.</span><span class="sxs-lookup"><span data-stu-id="9b576-309">hello pubexp is left blank (default) in this example, but you can specify specific values.</span></span> <span data-ttu-id="9b576-310">További információkért lásd: hello a Thales dokumentációjában talál.</span><span class="sxs-lookup"><span data-stu-id="9b576-310">For more information, see hello Thales documentation.</span></span>

<span data-ttu-id="9b576-311">Ez a parancs egy tokenekre bontott kulcsfájlt hoz létre az %NFAST_KMDATA%\local mappában, amelynek a neve a **key_simple_**, utána pedig hello **ident** hello parancsban megadott.</span><span class="sxs-lookup"><span data-stu-id="9b576-311">This command creates a Tokenized Key file in your %NFAST_KMDATA%\local folder with a name starting with **key_simple_**, followed by hello **ident** that was specified in hello command.</span></span> <span data-ttu-id="9b576-312">Például: **key_simple_contosokey**.</span><span class="sxs-lookup"><span data-stu-id="9b576-312">For example: **key_simple_contosokey**.</span></span> <span data-ttu-id="9b576-313">Ez a fájl egy titkosított kulcsot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="9b576-313">This file contains an encrypted key.</span></span>

<span data-ttu-id="9b576-314">Készítsen biztonsági másolatot a tokenekre bontott Kulcsfájlról egy biztonságos helyre.</span><span class="sxs-lookup"><span data-stu-id="9b576-314">Back up this Tokenized Key File in a safe location.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9b576-315">Amikor később átviszi a kulcs tooAzure kulcstároló, a Microsoft nem exportálható a kulcs hátsó tooyou, ezért rendkívül fontos, hogy biztonsági másolatot készíteni a kulcs és a biztonsági világáról biztonságosan.</span><span class="sxs-lookup"><span data-stu-id="9b576-315">When you later transfer your key tooAzure Key Vault, Microsoft cannot export this key back tooyou so it becomes extremely important that you back up your key and security world safely.</span></span> <span data-ttu-id="9b576-316">Lépjen kapcsolatba a Thales útmutatás és ajánlott eljárások a kulcs biztonsági mentéséről.</span><span class="sxs-lookup"><span data-stu-id="9b576-316">Contact Thales for guidance and best practices for backing up your key.</span></span>
>
>

<span data-ttu-id="9b576-317">Meg vannak tootransfer most már készen áll a kulcs tooAzure kulcstároló.</span><span class="sxs-lookup"><span data-stu-id="9b576-317">You are now ready tootransfer your key tooAzure Key Vault.</span></span>

## <a name="step-4-prepare-your-key-for-transfer"></a><span data-ttu-id="9b576-318">4. lépés: A kulcs Áthelyezés előkészítése</span><span class="sxs-lookup"><span data-stu-id="9b576-318">Step 4: Prepare your key for transfer</span></span>
<span data-ttu-id="9b576-319">Az hello a negyedik lépése az alábbi eljárásokat leválasztva hello munkaállomáson.</span><span class="sxs-lookup"><span data-stu-id="9b576-319">For this fourth step, do hello following procedures on hello disconnected workstation.</span></span>

### <a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a><span data-ttu-id="9b576-320">4.1. lépés: A korlátozott engedélyekkel rendelkező másolata a létrehozása</span><span class="sxs-lookup"><span data-stu-id="9b576-320">Step 4.1: Create a copy of your key with reduced permissions</span></span>

<span data-ttu-id="9b576-321">Nyisson meg egy új parancssort, és hello aktuális directory toohello helyének módosításához ahol unzipped a hello BYOK zip-fájl.</span><span class="sxs-lookup"><span data-stu-id="9b576-321">Open a new command prompt and change hello current directory toohello location where you unzipped hello BYOK zip file.</span></span> <span data-ttu-id="9b576-322">tooreduce hello engedélyeit a kulcsot egy parancssorból futtassa hello következők egyike, attól függően, hogy a földrajzi régióban vagy Azure példánya:</span><span class="sxs-lookup"><span data-stu-id="9b576-322">tooreduce hello permissions on your key, from a command prompt, run one of hello following, depending on your geographic region or instance of Azure:</span></span>

* <span data-ttu-id="9b576-323">Észak-amerikai:</span><span class="sxs-lookup"><span data-stu-id="9b576-323">For North America:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
* <span data-ttu-id="9b576-324">Európa esetében:</span><span class="sxs-lookup"><span data-stu-id="9b576-324">For Europe:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
* <span data-ttu-id="9b576-325">Ázsia esetében:</span><span class="sxs-lookup"><span data-stu-id="9b576-325">For Asia:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
* <span data-ttu-id="9b576-326">Latin-Amerika: a</span><span class="sxs-lookup"><span data-stu-id="9b576-326">For Latin America:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
* <span data-ttu-id="9b576-327">A japán:</span><span class="sxs-lookup"><span data-stu-id="9b576-327">For Japan:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
* <span data-ttu-id="9b576-328">Koreai:</span><span class="sxs-lookup"><span data-stu-id="9b576-328">For Korea:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1
* <span data-ttu-id="9b576-329">Ausztráliában:</span><span class="sxs-lookup"><span data-stu-id="9b576-329">For Australia:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
* <span data-ttu-id="9b576-330">A [Azure Government](https://azure.microsoft.com/features/gov/), használó Azure hello Egyesült Államokbeli kormányzati példánya:</span><span class="sxs-lookup"><span data-stu-id="9b576-330">For [Azure Government](https://azure.microsoft.com/features/gov/), which uses hello US government instance of Azure:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
* <span data-ttu-id="9b576-331">Az Amerikai Egyesült Államok kormánya DOD:</span><span class="sxs-lookup"><span data-stu-id="9b576-331">For US Government DOD:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1
* <span data-ttu-id="9b576-332">Kanada esetében:</span><span class="sxs-lookup"><span data-stu-id="9b576-332">For Canada:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
* <span data-ttu-id="9b576-333">Németország:</span><span class="sxs-lookup"><span data-stu-id="9b576-333">For Germany:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
* <span data-ttu-id="9b576-334">A India:</span><span class="sxs-lookup"><span data-stu-id="9b576-334">For India:</span></span>

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1

<span data-ttu-id="9b576-335">Ez a parancs futtatásakor cserélje le *contosokey* hello azonos értékre a **lépés 3.5: hozzon létre egy új kulcsot** a hello [a kulcs létrehozása](#step-3-generate-your-key) lépés.</span><span class="sxs-lookup"><span data-stu-id="9b576-335">When you run this command, replace *contosokey* with hello same value you specified in **Step 3.5: Create a new key** from hello [Generate your key](#step-3-generate-your-key) step.</span></span>

<span data-ttu-id="9b576-336">A biztonsági globális rendszergazdai kártyák tooplug kell adnia.</span><span class="sxs-lookup"><span data-stu-id="9b576-336">You are asked tooplug in your security world admin cards.</span></span>

<span data-ttu-id="9b576-337">Hello parancs befejeződésekor megjelenik **eredmény: sikeres** hello másolat készítése a kulcsról korlátozott engedélyekkel rendelkező key_xferacId_ nevű hello fájlban, és<contosokey>.</span><span class="sxs-lookup"><span data-stu-id="9b576-337">When hello command completes, you see **Result: SUCCESS** and hello copy of your key with reduced permissions are in hello file named key_xferacId_<contosokey>.</span></span>

<span data-ttu-id="9b576-338">Hello hozzáférés-vezérlési LISTÁKAT, előfordulhat, hogy megvizsgálja a Thales segédprogramjai használatával a következő parancsokat a hello:</span><span class="sxs-lookup"><span data-stu-id="9b576-338">You may inspects hello ACLS using following commands using hello Thales utilities:</span></span>

* <span data-ttu-id="9b576-339">aclprint.PY:</span><span class="sxs-lookup"><span data-stu-id="9b576-339">aclprint.py:</span></span>

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
* <span data-ttu-id="9b576-340">kmfile-dump.exe:</span><span class="sxs-lookup"><span data-stu-id="9b576-340">kmfile-dump.exe:</span></span>

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
  <span data-ttu-id="9b576-341">Ezek a parancsok futtatásakor cserélje le contosokey ugyanaz az érték hello **lépés 3.5: hozzon létre egy új kulcsot** a hello [a kulcs létrehozása](#step-3-generate-your-key) lépés.</span><span class="sxs-lookup"><span data-stu-id="9b576-341">When you run these commands, replace contosokey with hello same value you specified in **Step 3.5: Create a new key** from hello [Generate your key](#step-3-generate-your-key) step.</span></span>

### <a name="step-42-encrypt-your-key-by-using-microsofts-key-exchange-key"></a><span data-ttu-id="9b576-342">4.2. lépés: A kulcs titkosítása a Microsoft kulcscserekulcs használatával</span><span class="sxs-lookup"><span data-stu-id="9b576-342">Step 4.2: Encrypt your key by using Microsoft’s Key Exchange Key</span></span>
<span data-ttu-id="9b576-343">Futtassa a következő parancsokat, attól függően, hogy a földrajzi régióban vagy Azure példánya hello egyikét:</span><span class="sxs-lookup"><span data-stu-id="9b576-343">Run one of hello following commands, depending on your geographic region or instance of Azure:</span></span>

* <span data-ttu-id="9b576-344">Észak-amerikai:</span><span class="sxs-lookup"><span data-stu-id="9b576-344">For North America:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="9b576-345">Európa esetében:</span><span class="sxs-lookup"><span data-stu-id="9b576-345">For Europe:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="9b576-346">Ázsia esetében:</span><span class="sxs-lookup"><span data-stu-id="9b576-346">For Asia:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="9b576-347">Latin-Amerika: a</span><span class="sxs-lookup"><span data-stu-id="9b576-347">For Latin America:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="9b576-348">A japán:</span><span class="sxs-lookup"><span data-stu-id="9b576-348">For Japan:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="9b576-349">Koreai:</span><span class="sxs-lookup"><span data-stu-id="9b576-349">For Korea:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="9b576-350">Ausztráliában:</span><span class="sxs-lookup"><span data-stu-id="9b576-350">For Australia:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="9b576-351">A [Azure Government](https://azure.microsoft.com/features/gov/), használó Azure hello Egyesült Államokbeli kormányzati példánya:</span><span class="sxs-lookup"><span data-stu-id="9b576-351">For [Azure Government](https://azure.microsoft.com/features/gov/), which uses hello US government instance of Azure:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="9b576-352">Az Amerikai Egyesült Államok kormánya DOD:</span><span class="sxs-lookup"><span data-stu-id="9b576-352">For US Government DOD:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="9b576-353">Kanada esetében:</span><span class="sxs-lookup"><span data-stu-id="9b576-353">For Canada:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="9b576-354">Németország:</span><span class="sxs-lookup"><span data-stu-id="9b576-354">For Germany:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* <span data-ttu-id="9b576-355">A India:</span><span class="sxs-lookup"><span data-stu-id="9b576-355">For India:</span></span>

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey

<span data-ttu-id="9b576-356">Ez a parancs futtatásakor használja ezeket az utasításokat:</span><span class="sxs-lookup"><span data-stu-id="9b576-356">When you run this command, use these instructions:</span></span>

* <span data-ttu-id="9b576-357">Cserélje le *contosokey* toogenerate hello kulcsában használt hello azonosítójú **lépés 3.5: hozzon létre egy új kulcsot** a hello [a kulcs létrehozása](#step-3-generate-your-key) lépés.</span><span class="sxs-lookup"><span data-stu-id="9b576-357">Replace *contosokey* with hello identifier that you used toogenerate hello key in **Step 3.5: Create a new key** from hello [Generate your key](#step-3-generate-your-key) step.</span></span>
* <span data-ttu-id="9b576-358">Cserélje le *SubscriptionID* hello azonosítójú, hello Azure-előfizetéssel, amely a kulcstartót tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="9b576-358">Replace *SubscriptionID* with hello ID of hello Azure subscription that contains your key vault.</span></span> <span data-ttu-id="9b576-359">Ez az érték lekért korábban a **lépés 1.2: az Azure-előfizetés beszerzése** a hello [az internetre kapcsolódó munkaállomás előkészítése](#step-1-prepare-your-internet-connected-workstation) lépés.</span><span class="sxs-lookup"><span data-stu-id="9b576-359">You retrieved this value previously, in **Step 1.2: Get your Azure subscription ID** from hello [Prepare your Internet-connected workstation](#step-1-prepare-your-internet-connected-workstation) step.</span></span>
* <span data-ttu-id="9b576-360">Cserélje le *ContosoFirstHSMKey* a címkére, amelyet a kimeneti fájl neveként használatos.</span><span class="sxs-lookup"><span data-stu-id="9b576-360">Replace *ContosoFirstHSMKey* with a label that is used for your output file name.</span></span>

<span data-ttu-id="9b576-361">Amikor ez befejeződik, megjeleníti **eredmény: sikeres** és egy új fájlt hello aktuális mappában, amelynek neve a következő hello: KeyTransferPackage -*ContosoFirstHSMkey*.byok</span><span class="sxs-lookup"><span data-stu-id="9b576-361">When this completes successfully, it displays **Result: SUCCESS** and there is a new file in hello current folder that has hello following name: KeyTransferPackage-*ContosoFirstHSMkey*.byok</span></span>

### <a name="step-43-copy-your-key-transfer-package-toohello-internet-connected-workstation"></a><span data-ttu-id="9b576-362">4.3. lépés: A kulcsátviteli csomag toohello internetre kapcsolódó munkaállomás másolása</span><span class="sxs-lookup"><span data-stu-id="9b576-362">Step 4.3: Copy your key transfer package toohello Internet-connected workstation</span></span>
<span data-ttu-id="9b576-363">Hello előző lépést (KeyTransferPackage-ContosoFirstHSMkey.byok) tooyour internetre kapcsolódó munkaállomás egy USB-meghajtó vagy más hordozható tárolóeszköz toocopy hello kimeneti fájlt használja.</span><span class="sxs-lookup"><span data-stu-id="9b576-363">Use a USB drive or other portable storage toocopy hello output file from hello previous step (KeyTransferPackage-ContosoFirstHSMkey.byok) tooyour Internet-connected workstation.</span></span>

## <a name="step-5-transfer-your-key-tooazure-key-vault"></a><span data-ttu-id="9b576-364">5. lépés: A kulcs tooAzure Key Vault átvitele</span><span class="sxs-lookup"><span data-stu-id="9b576-364">Step 5: Transfer your key tooAzure Key Vault</span></span>
<span data-ttu-id="9b576-365">A végső lépés hello internetkapcsolattal rendelkező munkaállomáson, használja a hello [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) parancsmag tooupload hello kulcsátviteli csomag hello fájlból másolt csatlakoztatva munkaállomás toohello Azure Key Vault hardveres biztonsági MODULT:</span><span class="sxs-lookup"><span data-stu-id="9b576-365">For this final step, on hello Internet-connected workstation, use hello [Add-AzureKeyVaultKey](/powershell/module/azurerm.keyvault/add-azurermkeyvaultkey) cmdlet tooupload hello key transfer package that you copied from hello disconnected workstation toohello Azure Key Vault HSM:</span></span>

    Add-AzureKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\KeyTransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'

<span data-ttu-id="9b576-366">Sikeres hello feltöltés esetén, amelyek az előzőekben adott hozzá hello kulcs megjelenített hello tulajdonságai jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="9b576-366">If hello upload is successful, you see displayed hello properties of hello key that you just added.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9b576-367">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9b576-367">Next steps</span></span>
<span data-ttu-id="9b576-368">A HSM által védett kulcs key vaultban lévő most már használhatja.</span><span class="sxs-lookup"><span data-stu-id="9b576-368">You can now use this HSM-protected key in your key vault.</span></span> <span data-ttu-id="9b576-369">További információkért lásd: hello **Ha azt szeretné, a hardveres biztonsági modul (HSM) toouse** hello szakasz [Ismerkedés az Azure Key Vault](key-vault-get-started.md) oktatóanyag.</span><span class="sxs-lookup"><span data-stu-id="9b576-369">For more information, see hello **If you want toouse a hardware security module (HSM)** section in hello [Getting started with Azure Key Vault](key-vault-get-started.md) tutorial.</span></span>
