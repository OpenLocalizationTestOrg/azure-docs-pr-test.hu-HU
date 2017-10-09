---
title: "aaaProtect személyes adatokat átvitel közben a titkosítás az Azure-ban |} Microsoft Docs"
description: "Az Azure tooprotect személyes adatok titkosítása"
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
ms.openlocfilehash: 218ad3f49326e8dec299a6d92b18116da65eae71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-in-transit-with-encryption"></a><span data-ttu-id="3135c-103">Az Azure titkosítási technológiák: személyes adatok védelmére átvitel titkosítással</span><span class="sxs-lookup"><span data-stu-id="3135c-103">Azure encryption technologies: Protect personal data in transit with encryption</span></span>

<span data-ttu-id="3135c-104">Ez a cikk segít megérteni, és használja az Azure titkosítási technológiák toosecure adatokat átvitel közben.</span><span class="sxs-lookup"><span data-stu-id="3135c-104">This article will help you understand and use Azure encryption technologies toosecure data in transit.</span></span> 

<span data-ttu-id="3135c-105">Személyes adatok védelmet nyújtó hello adatvédelmi hello hálózaton keresztül többrétegű védelemmel az olyan jellegű biztonsági stratégia nagyon fontos részét képezik.</span><span class="sxs-lookup"><span data-stu-id="3135c-105">Protecting hello privacy of personal data as it travels across hello network is an essential part of a multi-layered defense-in-depth security strategy.</span></span> <span data-ttu-id="3135c-106">Az átvitel során titkosítás tervezett tooprevent egy támadó elfogja nem tud tooview használata hello adatok vagy az átvitelt.</span><span class="sxs-lookup"><span data-stu-id="3135c-106">Encryption in transit is designed tooprevent an attacker who intercepts transmissions from being able tooview or use hello data.</span></span>

## <a name="scenario"></a><span data-ttu-id="3135c-107">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="3135c-107">Scenario</span></span>

<span data-ttu-id="3135c-108">A nagy körutazás vállalati telephelyének hello az Amerikai Egyesült Államokban, a műveletek toooffer útvonalak hello mediterrán, Adriai, és Balti tengerek, valamint hello Brit-szigetekre növekszik.</span><span class="sxs-lookup"><span data-stu-id="3135c-108">A large cruise company, headquartered in hello United States, is expanding its operations toooffer itineraries in hello Mediterranean, Adriatic, and Baltic seas, as well as hello British Isles.</span></span> <span data-ttu-id="3135c-109">toosupport e erőfeszítéseket szerzett több kisebb körutazás sorok Olaszország, német, Dánia és hello brit</span><span class="sxs-lookup"><span data-stu-id="3135c-109">toosupport those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark and hello U.K.</span></span> 

<span data-ttu-id="3135c-110">hello vállalati hello felhő Microsoft Azure toostore vállalati adatait használja.</span><span class="sxs-lookup"><span data-stu-id="3135c-110">hello company uses Microsoft Azure toostore corporate data in hello cloud.</span></span> <span data-ttu-id="3135c-111">Ez magában foglalja a személyes azonosításra alkalmas adatokat, például neveket, címeket, telefonszámokat, és a globális felhasználói bázis hitelkártya adatait.</span><span class="sxs-lookup"><span data-stu-id="3135c-111">This includes personal identifiable information such as names, addresses, phone numbers, and credit card information of its global customer base.</span></span> <span data-ttu-id="3135c-112">Minden helyen hagyományos emberi erőforrás adatokat, például a címet, telefonszámot, azonosító számokat és a vállalat alkalmazottai orvosi információt is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="3135c-112">It also includes traditional Human Resource information such as addresses, phone numbers, tax identification numbers and medical information about company employees in all locations.</span></span> <span data-ttu-id="3135c-113">hello körutazás sor is fenntartja, nagy adatbázis ellenszolgáltatás és hűség program tagok, amely tartalmazza az aktuális és korábbi ügyfelek tootrack kapcsolatokkal személyes adatokat.</span><span class="sxs-lookup"><span data-stu-id="3135c-113">hello cruise line also maintains a large database of reward and loyalty program members that includes personal information tootrack relationships with current and past customers.</span></span>

<span data-ttu-id="3135c-114">Személyes ügyfelek adatok hello adatbázis hello vállalati távoli iroda és utazás ügynökök hello világ található.</span><span class="sxs-lookup"><span data-stu-id="3135c-114">Personal data of customers is entered in hello database from hello company’s remote offices and from travel agents located around hello world.</span></span> <span data-ttu-id="3135c-115">Felhasználói adatokat tartalmazó dokumentumok keresztül hello hálózati tooAzure tárolási továbbítása.</span><span class="sxs-lookup"><span data-stu-id="3135c-115">Documents containing customer information are transferred across hello network tooAzure storage.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="3135c-116">Probléma leírása</span><span class="sxs-lookup"><span data-stu-id="3135c-116">Problem statement</span></span>

<span data-ttu-id="3135c-117">hello vállalati védelmét kell beállítani, hogy az ügyfelek hello adatvédelmi és közben az alkalmazottak a személyes adatok átvitel közben tooand az Azure-szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="3135c-117">hello company must protect hello privacy of customers’ and employees’ personal data while it is in transit tooand from Azure services.</span></span>

## <a name="company-goal"></a><span data-ttu-id="3135c-118">Vállalati cél</span><span class="sxs-lookup"><span data-stu-id="3135c-118">Company goal</span></span>

<span data-ttu-id="3135c-119">Vállalati cél tooensure hello lemez ki személyes adatok titkosítását.</span><span class="sxs-lookup"><span data-stu-id="3135c-119">hello company goal tooensure that personal data is encrypted when off disk.</span></span> <span data-ttu-id="3135c-120">Jogosulatlan személyek intercept hello lemez személyes adatokat, ha megjelenítéséhez, nem olvasható formátumban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="3135c-120">If unauthorized persons intercept hello off-disk personal data, it must be in a form that will render it unreadable.</span></span> <span data-ttu-id="3135c-121">Titkosítás alkalmazásakor kell lennie, vagy az teljesen átlátható, felhasználók és rendszergazdák könnyen.</span><span class="sxs-lookup"><span data-stu-id="3135c-121">Applying encryption should be easy, or completely transparent, for users and administrators.</span></span>

## <a name="solutions"></a><span data-ttu-id="3135c-122">Megoldások</span><span class="sxs-lookup"><span data-stu-id="3135c-122">Solutions</span></span>

<span data-ttu-id="3135c-123">Azure-szolgáltatásokhoz biztosít több eszközök és technológiák toohelp meg személyes adatok védelmére átvitel.</span><span class="sxs-lookup"><span data-stu-id="3135c-123">Azure services provide multiple tools and technologies toohelp you protect personal data in transit.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="3135c-124">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="3135c-124">Azure Storage</span></span>

<span data-ttu-id="3135c-125">Hello felhőben tárolt adatok hello ügyfélről, amely fizikailag bárhol hello world, Azure-adatközpont toohello kell keresnie.</span><span class="sxs-lookup"><span data-stu-id="3135c-125">Data that is stored in hello cloud must travel from hello client, which can be physically located anywhere in hello world, toohello Azure data center.</span></span> <span data-ttu-id="3135c-126">Felhasználók lekéri az adatokat, amikor adatcsere ebben az esetben a hello ellenkező irányban.</span><span class="sxs-lookup"><span data-stu-id="3135c-126">When that data is retrieved by users, it travels again, in hello opposite direction.</span></span> <span data-ttu-id="3135c-127">Adatok átvitel közben keresztüli hello nyilvános Internet mindig az támadók hozzáférés veszélyben van.</span><span class="sxs-lookup"><span data-stu-id="3135c-127">Data that is in transit over hello public Internet is always at risk of interception by attackers.</span></span> <span data-ttu-id="3135c-128">Fontos tooprotect hello adatvédelmi személyes adatok, mert átvitel során helyek közötti átviteli szintű titkosítást toosecure használatával is.</span><span class="sxs-lookup"><span data-stu-id="3135c-128">It is important tooprotect hello privacy of personal data by using transport-level encryption toosecure it as it moves between locations.</span></span>

<span data-ttu-id="3135c-129">hello HTTPS protokollon keresztül hello Internet biztosítja a biztonságos, titkosított kommunikációs csatornát.</span><span class="sxs-lookup"><span data-stu-id="3135c-129">hello HTTPS protocol provides a secure, encrypted communications channel over hello Internet.</span></span> <span data-ttu-id="3135c-130">HTTPS kell használt tooaccess objektumok és Azure Storage REST API-k hívásakor.</span><span class="sxs-lookup"><span data-stu-id="3135c-130">HTTPS should be used tooaccess objects in Azure Storage and when calling REST APIs.</span></span> <span data-ttu-id="3135c-131">Hello HTTPS protokoll használatát kényszeríti használatakor [megosztott hozzáférési aláírásokkal](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) (SAS) toodelegate access tooAzure tároló objektumok.</span><span class="sxs-lookup"><span data-stu-id="3135c-131">You enforce use of hello HTTPS protocol when using [Shared Access Signatures](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) (SAS) toodelegate access tooAzure Storage objects.</span></span> <span data-ttu-id="3135c-132">SAS két típusa van: szolgáltatás SAS és a fiók SAS.</span><span class="sxs-lookup"><span data-stu-id="3135c-132">There are two types of SAS: Service SAS and Account SAS.</span></span>

#### <a name="how-do-i-construct-a-service-sas"></a><span data-ttu-id="3135c-133">Hogyan hozható létre a szolgáltatás SAS?</span><span class="sxs-lookup"><span data-stu-id="3135c-133">How do I construct a Service SAS?</span></span>

<span data-ttu-id="3135c-134">Szolgáltatás SAS delegáltak hozzáférés tooa erőforrás csak egy hello tárolási szolgáltatásokhoz (blob, a várólista, a tábla vagy a fájl szolgáltatás).</span><span class="sxs-lookup"><span data-stu-id="3135c-134">A Service SAS delegates access tooa resource in just one of hello storage services (blob, queue, table or file service).</span></span> <span data-ttu-id="3135c-135">a szolgáltatás SAS tooconstruct hello a következő:</span><span class="sxs-lookup"><span data-stu-id="3135c-135">tooconstruct a Service SAS, do hello following:</span></span>

1. <span data-ttu-id="3135c-136">Adja meg az aláírt Verziómezőjének hello</span><span class="sxs-lookup"><span data-stu-id="3135c-136">Specify hello Signed Version Field</span></span>

2. <span data-ttu-id="3135c-137">Adja meg a hello aláírt erőforrás (Blob, és csak a szolgáltatás fájl)</span><span class="sxs-lookup"><span data-stu-id="3135c-137">Specify hello Signed Resource (Blob and File Service Only)</span></span>

3. <span data-ttu-id="3135c-138">Adja meg a lekérdezési paraméterek tooOverride válaszfejlécek (Blob szolgáltatás és a szolgáltatás csak fájl)</span><span class="sxs-lookup"><span data-stu-id="3135c-138">Specify Query Parameters tooOverride Response Headers (Blob Service and File Service Only)</span></span>

4. <span data-ttu-id="3135c-139">Adja meg a hello (Table szolgáltatás csak) tábla neve</span><span class="sxs-lookup"><span data-stu-id="3135c-139">Specify hello Table Name (Table Service Only)</span></span>

5. <span data-ttu-id="3135c-140">Adja meg a hello hozzáférési házirend</span><span class="sxs-lookup"><span data-stu-id="3135c-140">Specify hello Access Policy</span></span>

6. <span data-ttu-id="3135c-141">Adja meg az aláírás érvényességi időszakának hello</span><span class="sxs-lookup"><span data-stu-id="3135c-141">Specify hello Signature Validity Interval</span></span>

8. <span data-ttu-id="3135c-142">Engedélyek megadása</span><span class="sxs-lookup"><span data-stu-id="3135c-142">Specify Permissions</span></span>

9. <span data-ttu-id="3135c-143">Adja meg az IP-cím vagy IP-címtartomány</span><span class="sxs-lookup"><span data-stu-id="3135c-143">Specify IP Address or IP Range</span></span>

10. <span data-ttu-id="3135c-144">Adja meg a HTTP protokoll hello</span><span class="sxs-lookup"><span data-stu-id="3135c-144">Specify hello HTTP Protocol</span></span>

11. <span data-ttu-id="3135c-145">Adja meg a hozzáférés tartomány</span><span class="sxs-lookup"><span data-stu-id="3135c-145">Specify Table Access Ranges</span></span>

12. <span data-ttu-id="3135c-146">Adja meg a hello aláírt azonosítója</span><span class="sxs-lookup"><span data-stu-id="3135c-146">Specify hello Signed Identifier</span></span>

13. <span data-ttu-id="3135c-147">Adja meg a hello aláírás</span><span class="sxs-lookup"><span data-stu-id="3135c-147">Specify hello Signature</span></span>

<span data-ttu-id="3135c-148">Részletes utasítások, lásd: [hozhat létre, egy szolgáltatás SAS](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).</span><span class="sxs-lookup"><span data-stu-id="3135c-148">For more detailed instructions, see [Constructing a Service SAS](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).</span></span>

#### <a name="how-do-i-construct-an-account-sas"></a><span data-ttu-id="3135c-149">Hogyan hozható létre egy fiókot SAS?</span><span class="sxs-lookup"><span data-stu-id="3135c-149">How do I construct an Account SAS?</span></span>

<span data-ttu-id="3135c-150">Egy fiók SAS egy vagy több hello tárolószolgáltatások hozzáférés tooresources delegálja.</span><span class="sxs-lookup"><span data-stu-id="3135c-150">An Account SAS delegates access tooresources in one or more of hello storage services.</span></span> <span data-ttu-id="3135c-151">Delegálhatja hozzáférés tooread, írási és törlési műveletek a blobtárolók, táblák, üzenetsorok és fájlmegosztások a szolgáltatásalapú SAS nem engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="3135c-151">You can also delegate access tooread, write, and delete operations on blob containers, tables, queues, and file shares that are not permitted with a service SAS.</span></span> <span data-ttu-id="3135c-152">Egy fiók SAS építése egy szolgáltatás SAS hasonló toothat.</span><span class="sxs-lookup"><span data-stu-id="3135c-152">Construction of an Account SAS is similar toothat of a Service SAS.</span></span> <span data-ttu-id="3135c-153">Részletes útmutatásért lásd: [hozhat létre egy fiókot SAS.](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN)</span><span class="sxs-lookup"><span data-stu-id="3135c-153">For detailed instructions, see [Constructing an Account SAS.](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN)</span></span>

#### <a name="how-do-i-enforce-https-when-calling-rest-apis"></a><span data-ttu-id="3135c-154">Hogyan kényszerítése meg HTTPS, amikor a program meghívja a REST API-k?</span><span class="sxs-lookup"><span data-stu-id="3135c-154">How do I enforce HTTPS when calling REST APIs?</span></span>

<span data-ttu-id="3135c-155">a storage-fiókok REST API-k tooaccess objektumok meghívásakor HTTPS tooenforce hello használata, engedélyezheti a biztonságos átviteléhez szükséges hello tárfiók.</span><span class="sxs-lookup"><span data-stu-id="3135c-155">tooenforce hello use of HTTPS when calling REST APIs tooaccess objects in storage accounts, you can enable Secure Transfer Required for hello storage account.</span></span> 

1. <span data-ttu-id="3135c-156">Hello Azure-portálon, válassza ki **Storage-fiók létrehozása**, vagy válassza a meglévő tárfiókot, **beállítások** , majd **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="3135c-156">In hello Azure portal, select **Create Storage Account**, or for an existing storage account, select **Settings** and then **Configuration**.</span></span>

2. <span data-ttu-id="3135c-157">A **biztonságos átviteléhez szükséges**, jelölje be **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="3135c-157">Under **Secure Transfer Required**, select **Enabled**.</span></span>

![A storage-fiók létrehozása](media/protect-personal-data-in-transit-encryption/create-storage-account.png)

<span data-ttu-id="3135c-159">Részletesebb leírását, beleértve a tooenable biztonságos átviteléhez szükséges programozott módon, lásd: hogyan [szükséges biztonságos átviteli](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).</span><span class="sxs-lookup"><span data-stu-id="3135c-159">For more detailed instructions, including how tooenable Secure Transfer Required programmatically, see [Require Secure Transfer](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).</span></span>

#### <a name="how-do-i-encrypt-data-in-azure-file-storage"></a><span data-ttu-id="3135c-160">Hogyan titkosítják az adatokat az Azure File Storage?</span><span class="sxs-lookup"><span data-stu-id="3135c-160">How do I encrypt data in Azure File Storage?</span></span>

<span data-ttu-id="3135c-161">az átvitt adatokat tooencrypt [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), használhatja az SMB Windows 8, 8.1 és 10 és Windows Server 2012 R2 és Windows Server 2016 3.x.</span><span class="sxs-lookup"><span data-stu-id="3135c-161">tooencrypt data in transit with [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), you can use SMB 3.x with Windows 8, 8.1, and 10 and with Windows Server 2012 R2 and Windows Server 2016.</span></span> <span data-ttu-id="3135c-162">Hello Azure fájlok szolgáltatást használja, a titkosítás nélküli kapcsolat sikertelen lesz, ha engedélyezve van a "Biztonságos szükséges átviteli".</span><span class="sxs-lookup"><span data-stu-id="3135c-162">When you are using hello Azure Files service, any connection without encryption fails when "Secure transfer required" is enabled.</span></span> <span data-ttu-id="3135c-163">Ez magában foglalja a forgatókönyveket SMB 2.1, SMB 3.0-titkosítás nélkül, és néhány változatban is elkészíti a hello Linux SMB-ügyfél használatával.</span><span class="sxs-lookup"><span data-stu-id="3135c-163">This includes scenarios using SMB 2.1, SMB 3.0 without encryption, and some flavors of hello Linux SMB client.</span></span>

#### <a name="azure-client-side-encryption"></a><span data-ttu-id="3135c-164">Az Azure ügyféloldali titkosítás</span><span class="sxs-lookup"><span data-stu-id="3135c-164">Azure Client-Side Encryption</span></span>

<span data-ttu-id="3135c-165">Másik lehetőségként a személyes adatok védelme alatt álló ügyfélalkalmazást és az Azure Storage közötti átvitel közben [ügyféloldali titkosítás](https://docs.microsoft.com/azure/storage/storage-client-side-encryption).</span><span class="sxs-lookup"><span data-stu-id="3135c-165">Another option for protecting personal data while it’s being transferred between a client application and Azure Storage is [Client-side Encryption](https://docs.microsoft.com/azure/storage/storage-client-side-encryption).</span></span> <span data-ttu-id="3135c-166">hello adattitkosítás előtt Azure Storage történő átvitel során, és amikor hello adatok Azure Storage-ból, hello adatok visszafejtése hello ügyféloldalon fogadását követően.</span><span class="sxs-lookup"><span data-stu-id="3135c-166">hello data is encrypted before being transferred into Azure Storage and when you retrieve hello data from Azure Storage, hello data is decrypted after it is received on hello client side.</span></span>

### <a name="azure-site-to-site-vpn"></a><span data-ttu-id="3135c-167">Az Azure--webhelyek közötti VPN</span><span class="sxs-lookup"><span data-stu-id="3135c-167">Azure Site-to-Site VPN</span></span>

<span data-ttu-id="3135c-168">Egy hatékony módot tooprotect személyes adatok átvitele történik a vállalati hálózaton vagy a felhasználó és a hello Azure-beli virtuális hálózat között toouse egy [pont-pont](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) vagy [pont-pont](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) virtuális magánhálózati (VPN).</span><span class="sxs-lookup"><span data-stu-id="3135c-168">An effective way tooprotect personal data in transit between a corporate network or user and hello Azure virtual network is toouse a [site-to-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) or [point-to-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) Virtual Private Network (VPN).</span></span> <span data-ttu-id="3135c-169">VPN-kapcsolat egy biztonságos, titkosított csatornán keresztül hello Internet hoz létre.</span><span class="sxs-lookup"><span data-stu-id="3135c-169">A VPN connection creates a secure encrypted tunnel across hello Internet.</span></span>

#### <a name="how-do-i-create-a-site-to-site-vpn-connection"></a><span data-ttu-id="3135c-170">Hogyan hozható létre a pont-pont VPN-kapcsolatot?</span><span class="sxs-lookup"><span data-stu-id="3135c-170">How do I create a site-to-site VPN connection?</span></span>

<span data-ttu-id="3135c-171">A telephelyek közötti VPN hello vállalati hálózat tooAzure több felhasználó csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="3135c-171">A site-to-site VPN connects multiple users on hello corporate network tooAzure.</span></span> <span data-ttu-id="3135c-172">hello Azure-portálon a pont-pont kapcsolat toocreate hello a következő:</span><span class="sxs-lookup"><span data-stu-id="3135c-172">toocreate a site-to-site connection in hello Azure portal, do hello following:</span></span>

1. <span data-ttu-id="3135c-173">Hozzon létre egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="3135c-173">Create a virtual network.</span></span>

2. <span data-ttu-id="3135c-174">Adjon meg egy DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="3135c-174">Specify a DNS server.</span></span>

3. <span data-ttu-id="3135c-175">Hozzon létre hello átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="3135c-175">Create hello gateway subnet.</span></span>

4. <span data-ttu-id="3135c-176">Hello VPN-átjáró létrehozásához.</span><span class="sxs-lookup"><span data-stu-id="3135c-176">Create hello VPN gateway.</span></span> 

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-01.png)

5. <span data-ttu-id="3135c-177">Hello helyi hálózati átjáró létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3135c-177">Create hello local network gateway.</span></span>

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-02.png)

6. <span data-ttu-id="3135c-178">Konfigurálja a VPN-eszköz.</span><span class="sxs-lookup"><span data-stu-id="3135c-178">Configure your VPN device.</span></span>

7. <span data-ttu-id="3135c-179">Hello VPN-kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3135c-179">Create hello VPN connection.</span></span>

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-03.png)

8. <span data-ttu-id="3135c-180">Hello VPN-kapcsolat ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="3135c-180">Verify hello VPN connection.</span></span>

<span data-ttu-id="3135c-181">Részletes utasítások a hogyan toocreate a pont-pont kapcsolat hello Azure portál, lásd: [hello Azure Portal kapcsolat létrehozása a pont-pont.] (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)</span><span class="sxs-lookup"><span data-stu-id="3135c-181">For more detailed instructions on how toocreate a site-to-site connection in hello Azure portal, see [Create a Site-to-Site  connection in hello Azure Portal.] (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)</span></span>

#### <a name="how-do-i-create-a-point-to-site-vpn-connection"></a><span data-ttu-id="3135c-182">Hogyan hozható létre egy pont – hely VPN-kapcsolatot?</span><span class="sxs-lookup"><span data-stu-id="3135c-182">How do I create a point-to-site VPN connection?</span></span>

<span data-ttu-id="3135c-183">A pont-pont VPN biztonságos kapcsolatot hoz létre egy egyéni ügyfél számítógép tooa virtuális hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="3135c-183">A Point-to-Site VPN creates a secure connection from an individual client computer tooa virtual network.</span></span> <span data-ttu-id="3135c-184">Ez akkor hasznos, ha azt szeretné, tooconnect tooAzure távoli helyről, például otthoni vagy a szállodák vagy konferencia központból.</span><span class="sxs-lookup"><span data-stu-id="3135c-184">This is useful when you  want tooconnect tooAzure from a remote location, such as from home or a hotel or conference center.</span></span> <span data-ttu-id="3135c-185">toocreate egy pont – hely kapcsolatot hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="3135c-185">toocreate a point-to-site  connection in hello Azure portal,</span></span>

1. <span data-ttu-id="3135c-186">Hozzon létre egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="3135c-186">Create a virtual network.</span></span>

2. <span data-ttu-id="3135c-187">Adjon hozzá egy átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="3135c-187">Add a gateway subnet.</span></span>

3. <span data-ttu-id="3135c-188">Adjon meg egy DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="3135c-188">Specify a DNS server.</span></span> <span data-ttu-id="3135c-189">(nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="3135c-189">(optional)</span></span>

4. <span data-ttu-id="3135c-190">A virtuális hálózati átjáró létrehozása.</span><span class="sxs-lookup"><span data-stu-id="3135c-190">Create a virtual network gateway.</span></span>

5. <span data-ttu-id="3135c-191">Tanúsítványok előállításához.</span><span class="sxs-lookup"><span data-stu-id="3135c-191">Generate certificates.</span></span>

6. <span data-ttu-id="3135c-192">Adja hozzá a hello ügyfélcímkészlete.</span><span class="sxs-lookup"><span data-stu-id="3135c-192">Add hello client address pool.</span></span>

7. <span data-ttu-id="3135c-193">Hello legfelső szintű tanúsítvány nyilvános Tanúsítványadatok feltöltése.</span><span class="sxs-lookup"><span data-stu-id="3135c-193">Upload hello root certificate public certificate data.</span></span>

8. <span data-ttu-id="3135c-194">Készítése és hello VPN-ügyfélcsomag konfigurációs telepítése.</span><span class="sxs-lookup"><span data-stu-id="3135c-194">Generate and install hello VPN client configuration package.</span></span>

9. <span data-ttu-id="3135c-195">Telepíti az exportált ügyféltanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="3135c-195">Install an exported client certificate.</span></span>

10. <span data-ttu-id="3135c-196">Csatlakozás tooAzure.</span><span class="sxs-lookup"><span data-stu-id="3135c-196">Connect tooAzure.</span></span>

11. <span data-ttu-id="3135c-197">Ellenőrizze a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="3135c-197">Verify your connection.</span></span>

<span data-ttu-id="3135c-198">Részletes utasításokat, lásd: [konfigurálása egy pont – hely kapcsolat tooa VNet-alapú hitelesítést használó: Azure portálon.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)</span><span class="sxs-lookup"><span data-stu-id="3135c-198">For more detailed instructions, see [Configure a Point-to-Site connection tooa VNet using certificate authentication: Azure Portal.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)</span></span>

### <a name="ssltls"></a><span data-ttu-id="3135c-199">SSL/TLS</span><span class="sxs-lookup"><span data-stu-id="3135c-199">SSL/TLS</span></span>

<span data-ttu-id="3135c-200">A Microsoft azt javasolja, hogy mindig használjon SSL/TLS protokollok tooexchange adatok különböző helyek között.</span><span class="sxs-lookup"><span data-stu-id="3135c-200">Microsoft recommends that you always use SSL/TLS protocols tooexchange data across different locations.</span></span> <span data-ttu-id="3135c-201">A szervezetek, amelyek toouse [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) toomove nagy adatkészletek egy dedikált nagy sebességű WAN-kapcsolaton keresztül is titkosíthatók hello hello SSL/TLS- vagy egyéb protokollok használatával további alkalmazásszintű adatok.</span><span class="sxs-lookup"><span data-stu-id="3135c-201">Organizations that choose toouse [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) toomove large data sets over a dedicated high-speed WAN link can also encrypt hello data at hello application-level using SSL/TLS or other protocols for added protection.</span></span>

### <a name="encryption-by-default"></a><span data-ttu-id="3135c-202">Alapértelmezés szerint titkosítás</span><span class="sxs-lookup"><span data-stu-id="3135c-202">Encryption by default</span></span>

<span data-ttu-id="3135c-203">A Microsoft titkosítási tooprotect adatokat használja az ügyfelek és az Azure felhőszolgáltatások közötti átvitel során.</span><span class="sxs-lookup"><span data-stu-id="3135c-203">Microsoft uses encryption tooprotect data in transit between customers and Azure cloud services.</span></span> <span data-ttu-id="3135c-204">Interakció Azure Storage hello Azure portálon keresztül, ha minden tranzakciója történik, HTTPS használatával.</span><span class="sxs-lookup"><span data-stu-id="3135c-204">If you are interacting with Azure Storage through hello Azure Portal, all transactions occur via HTTPS.</span></span>

<span data-ttu-id="3135c-205">[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS), hogy a Microsoft azon adatközpontjainak megpróbál toonegotiate tooMicrosoft felhőszolgáltatásokhoz csatlakozó ügyfél rendszerekkel hello protokoll.</span><span class="sxs-lookup"><span data-stu-id="3135c-205">[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) is hello protocol that Microsoft data centers will attempt toonegotiate with client systems that connect tooMicrosoft cloud services.</span></span> <span data-ttu-id="3135c-206">A TLS nyújt erős hitelesítés, üzenetvédelem, és integritását (lehetővé teszi, hogy a üzenet illetéktelen hozzáférés és hamisítására észlelése), együttműködés, az algoritmusok rugalmassága, könnyű telepítése és használata.</span><span class="sxs-lookup"><span data-stu-id="3135c-206">TLS provides strong authentication, message privacy, and integrity (enables detection of message tampering, interception, and forgery), interoperability, algorithm flexibility, ease of deployment and use.</span></span>

<span data-ttu-id="3135c-207">[Továbbítási titkosítása tökéletes](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) is, hogy az egyes ügyfelek ügyfél és a Microsoft olyan felhőszolgáltatásai közötti kapcsolat egyedi kulcsok használata alkalmazzák.</span><span class="sxs-lookup"><span data-stu-id="3135c-207">[Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) is also employed so that each connection between customers’ client systems and Microsoft’s cloud services use unique keys.</span></span> <span data-ttu-id="3135c-208">Kapcsolatok tooMicrosoft felhőszolgáltatások kihasználhatják alapú RSA 2048 bites titkosítást kulcshossza.</span><span class="sxs-lookup"><span data-stu-id="3135c-208">Connections tooMicrosoft cloud services also take advantage of RSA based 2,048-bit encryption key lengths.</span></span> <span data-ttu-id="3135c-209">hello TLS, RSA 2048 bit, kulcshosszok kombinációja, így a PFS jelentős mértékben megnehezíti mások toointercept és a hozzáférési adatok, amely a Microsoft más felhőszolgáltatásaival és az ügyfelek közötti átvitel közben.</span><span class="sxs-lookup"><span data-stu-id="3135c-209">hello combination of TLS, RSA 2,048-bit key lengths, and PFS makes it much  more difficult for someone toointercept and access data that is in transit between Microsoft cloud services and customers.</span></span>

<span data-ttu-id="3135c-210">Adatok az átvitel során mindig titkosítja az [Data Lake Store] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview).</span><span class="sxs-lookup"><span data-stu-id="3135c-210">Data in transit is always encrypted in [Data Lake Store] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview).</span></span> <span data-ttu-id="3135c-211">Ezenkívül tooencrypting előzetes toostoring toopersistent adathordozók, hello adatok is minden esetben biztosítva átvitel közben HTTPS használatával.</span><span class="sxs-lookup"><span data-stu-id="3135c-211">In addition tooencrypting data prior toostoring toopersistent media, hello data is also always secured in transit by using HTTPS.</span></span> <span data-ttu-id="3135c-212">HTTPS hello egyetlen protokoll, amely Data Lake Store REST felületeihez hello támogatott.</span><span class="sxs-lookup"><span data-stu-id="3135c-212">HTTPS is hello only protocol that is supported for hello Data Lake Store REST interfaces.</span></span>

## <a name="summary"></a><span data-ttu-id="3135c-213">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="3135c-213">Summary</span></span>

<span data-ttu-id="3135c-214">hello vállalati megvalósításához azáltal, HTTPS-kapcsolatok tooAzure tárolási, megosztott hozzáférési aláírásokkal használatával biztonságos átviteléhez szükséges engedélyezését hello tárfiókok személyes adatok és hello adatvédelmi az ilyen adatok védelme a célja.</span><span class="sxs-lookup"><span data-stu-id="3135c-214">hello company can accomplish its goal of protecting personal data and hello privacy of such data by enforcing HTTPS connections tooAzure Storage, using Shared Access Signatures and enabling Secure Transfer Required on hello storage accounts.</span></span> <span data-ttu-id="3135c-215">Személyes adatok védelme az SMB 3.0-kapcsolatot is használ, és ügyféloldali titkosítás végrehajtásával is.</span><span class="sxs-lookup"><span data-stu-id="3135c-215">They can also protect personal data by using SMB 3.0 connections and implementing client-side encryption.</span></span> <span data-ttu-id="3135c-216">Pont-pont VPN-kapcsolatok a vállalati hálózat toohello Azure-beli virtuális hálózat hello és a pont-pont VPN-kapcsolatok az egyes felhasználó létrehoz egy biztonságos csatornán keresztül, amelyek személyes adatokat is biztonságosan változatlan marad.</span><span class="sxs-lookup"><span data-stu-id="3135c-216">Site-to-site VPN connections from hello corporate network toohello Azure virtual network and point-to-site VPN connections from individual users will create a secure tunnel through which personal data can securely travel.</span></span> <span data-ttu-id="3135c-217">A Microsoft alapértelmezett titkosítási eljárásokat további megvédi a hello adatvédelmi személyes adatok.</span><span class="sxs-lookup"><span data-stu-id="3135c-217">Microsoft’s default encryption practices will further protect hello privacy of personal data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3135c-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="3135c-218">Next steps</span></span>

- [<span data-ttu-id="3135c-219">Az Azure Data biztonsági és a titkosítás gyakorlati tanácsok</span><span class="sxs-lookup"><span data-stu-id="3135c-219">Azure Data Security and Encryption Best Practices</span></span>](https://docs.microsoft.com/azure/security/azure-security-data-encryption-best-practices)

- [<span data-ttu-id="3135c-220">A VPN Gateway tervezése és kialakítása</span><span class="sxs-lookup"><span data-stu-id="3135c-220">Planning and design for VPN Gateway</span></span>](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

- [<span data-ttu-id="3135c-221">VPN Gateway – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="3135c-221">VPN Gateway FAQ</span></span>](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq)

- [<span data-ttu-id="3135c-222">Vásároljon és SSL-tanúsítvány konfigurálása az Azure App Service</span><span class="sxs-lookup"><span data-stu-id="3135c-222">Buy and configure an SSL Certificate for your Azure App Service</span></span>](https://docs.microsoft.com/azure/app-service-web/web-sites-purchase-ssl-web-site)
