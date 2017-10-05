---
title: "Személyes adatok védelmére átvitel a titkosítás az Azure-ban |} Microsoft Docs"
description: "Azure titkosítással személyes adatok védelme"
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
ms.openlocfilehash: 99e40b8a09a2f151e7f83fbb58bdfc00ae4b1268
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="azure-encryption-technologies-protect-personal-data-in-transit-with-encryption"></a><span data-ttu-id="e0d1e-103">Az Azure titkosítási technológiák: személyes adatok védelmére átvitel titkosítással</span><span class="sxs-lookup"><span data-stu-id="e0d1e-103">Azure encryption technologies: Protect personal data in transit with encryption</span></span>

<span data-ttu-id="e0d1e-104">Ez a cikk segít megértéséhez, valamint a védett adatok Azure titkosítási technológiák használata az átvitel során.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-104">This article will help you understand and use Azure encryption technologies to secure data in transit.</span></span> 

<span data-ttu-id="e0d1e-105">Fontos része a többrétegű védelemmel az olyan jellegű biztonsági stratégia védelme a személyes adatokat a hálózaton keresztül.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-105">Protecting the privacy of personal data as it travels across the network is an essential part of a multi-layered defense-in-depth security strategy.</span></span> <span data-ttu-id="e0d1e-106">Titkosítási átvitel célja egy támadó elfogja az átvitelt nem tudja megtekinteni, illetve az adatok felhasználásával megelőzése érdekében.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-106">Encryption in transit is designed to prevent an attacker who intercepts transmissions from being able to view or use the data.</span></span>

## <a name="scenario"></a><span data-ttu-id="e0d1e-107">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="e0d1e-107">Scenario</span></span>

<span data-ttu-id="e0d1e-108">Egy nagy körutazás cég, az Amerikai Egyesült Államokban telephelyének bővíti a műveleteket a mediterrán, Adriai, és Balti tengerek, valamint a Brit-szigetekre útvonalak biztosítani.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-108">A large cruise company, headquartered in the United States, is expanding its operations to offer itineraries in the Mediterranean, Adriatic, and Baltic seas, as well as the British Isles.</span></span> <span data-ttu-id="e0d1e-109">Támogatás ezeket, több kisebb körutazás sorok Olaszország, Németországban, Dánia és az Egyesült szerzett</span><span class="sxs-lookup"><span data-stu-id="e0d1e-109">To support those efforts, it has acquired several smaller cruise lines based in Italy, Germany, Denmark and the U.K.</span></span> 

<span data-ttu-id="e0d1e-110">A vállalat a Microsoft Azure használ a felhőben tárolt vállalati adatokat.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-110">The company uses Microsoft Azure to store corporate data in the cloud.</span></span> <span data-ttu-id="e0d1e-111">Ez magában foglalja a személyes azonosításra alkalmas adatokat, például neveket, címeket, telefonszámokat, és a globális felhasználói bázis hitelkártya adatait.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-111">This includes personal identifiable information such as names, addresses, phone numbers, and credit card information of its global customer base.</span></span> <span data-ttu-id="e0d1e-112">Minden helyen hagyományos emberi erőforrás adatokat, például a címet, telefonszámot, azonosító számokat és a vállalat alkalmazottai orvosi információt is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-112">It also includes traditional Human Resource information such as addresses, phone numbers, tax identification numbers and medical information about company employees in all locations.</span></span> <span data-ttu-id="e0d1e-113">A körutazás sor is fenntartja, nagy adatbázis ellenszolgáltatás és hűség program tagok nyomon követéséhez a kapcsolatokat az aktuális és korábbi ügyfelek személyes információkat is tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-113">The cruise line also maintains a large database of reward and loyalty program members that includes personal information to track relationships with current and past customers.</span></span>

<span data-ttu-id="e0d1e-114">Személyes ügyfelek adatok az adatbázis távoli iroda a vállalat és a világ utazás ügynökök.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-114">Personal data of customers is entered in the database from the company’s remote offices and from travel agents located around the world.</span></span> <span data-ttu-id="e0d1e-115">Felhasználói adatokat tartalmazó dokumentumok továbbítja a hálózaton az Azure Storage.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-115">Documents containing customer information are transferred across the network to Azure storage.</span></span>

## <a name="problem-statement"></a><span data-ttu-id="e0d1e-116">Probléma leírása</span><span class="sxs-lookup"><span data-stu-id="e0d1e-116">Problem statement</span></span>

<span data-ttu-id="e0d1e-117">A vállalat kell az ügyfelek és az alkalmazottak személyes adatok védelme, amikor az Azure-szolgáltatásokhoz érkező vagy oda irányuló útközben.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-117">The company must protect the privacy of customers’ and employees’ personal data while it is in transit to and from Azure services.</span></span>

## <a name="company-goal"></a><span data-ttu-id="e0d1e-118">Vállalati cél</span><span class="sxs-lookup"><span data-stu-id="e0d1e-118">Company goal</span></span>

<span data-ttu-id="e0d1e-119">A vállalat célja, hogy a személyes adatok lemez ki legyen titkosítva.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-119">The company goal to ensure that personal data is encrypted when off disk.</span></span> <span data-ttu-id="e0d1e-120">Jogosulatlan személyek intercept a lemez személyes adatokat, ha megjelenítéséhez, nem olvasható formátumban kell lennie.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-120">If unauthorized persons intercept the off-disk personal data, it must be in a form that will render it unreadable.</span></span> <span data-ttu-id="e0d1e-121">Titkosítás alkalmazásakor kell lennie, vagy az teljesen átlátható, felhasználók és rendszergazdák könnyen.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-121">Applying encryption should be easy, or completely transparent, for users and administrators.</span></span>

## <a name="solutions"></a><span data-ttu-id="e0d1e-122">Megoldások</span><span class="sxs-lookup"><span data-stu-id="e0d1e-122">Solutions</span></span>

<span data-ttu-id="e0d1e-123">Azure-szolgáltatások és több segítséget a személyes adatok védelmére átvitel technológiák biztosítása.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-123">Azure services provide multiple tools and technologies to help you protect personal data in transit.</span></span>

### <a name="azure-storage"></a><span data-ttu-id="e0d1e-124">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="e0d1e-124">Azure Storage</span></span>

<span data-ttu-id="e0d1e-125">A felhőben tárolt adatokat az Azure-adatközpontot az ügyfélről, amely fizikailag bárhol a világon, kell keresnie.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-125">Data that is stored in the cloud must travel from the client, which can be physically located anywhere in the world, to the Azure data center.</span></span> <span data-ttu-id="e0d1e-126">Amikor a felhasználók lekéri az adatokat, akkor újra, az ellenkező irányba választ.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-126">When that data is retrieved by users, it travels again, in the opposite direction.</span></span> <span data-ttu-id="e0d1e-127">Adatok az átvitel során a nyilvános interneten keresztül mindig az támadók hozzáférés veszélyben van.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-127">Data that is in transit over the public Internet is always at risk of interception by attackers.</span></span> <span data-ttu-id="e0d1e-128">Fontos az személyes adatok védelmét átviteli szintű titkosítást biztonságossá tételéhez, helyek közötti átvitel során.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-128">It is important to protect the privacy of personal data by using transport-level encryption to secure it as it moves between locations.</span></span>

<span data-ttu-id="e0d1e-129">A HTTPS protokoll egy biztonságos, titkosított kommunikációs csatornát biztosít az interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-129">The HTTPS protocol provides a secure, encrypted communications channel over the Internet.</span></span> <span data-ttu-id="e0d1e-130">HTTPS használatával érhető el az Azure Storage és objektumok, amikor a REST API-k hívása.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-130">HTTPS should be used to access objects in Azure Storage and when calling REST APIs.</span></span> <span data-ttu-id="e0d1e-131">Ha kényszeríti a HTTPS protokoll használatát [megosztott hozzáférési aláírásokkal](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) (SAS) delegálása az Azure Storage-objektumokhoz való hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-131">You enforce use of the HTTPS protocol when using [Shared Access Signatures](https://docs.microsoft.com/azure/storage/storage-dotnet-shared-access-signature-part-1) (SAS) to delegate access to Azure Storage objects.</span></span> <span data-ttu-id="e0d1e-132">SAS két típusa van: szolgáltatás SAS és a fiók SAS.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-132">There are two types of SAS: Service SAS and Account SAS.</span></span>

#### <a name="how-do-i-construct-a-service-sas"></a><span data-ttu-id="e0d1e-133">Hogyan hozható létre a szolgáltatás SAS?</span><span class="sxs-lookup"><span data-stu-id="e0d1e-133">How do I construct a Service SAS?</span></span>

<span data-ttu-id="e0d1e-134">A szolgáltatás SAS delegálja a tároló szolgáltatást (blob, a várólista, a tábla vagy a fájl szolgáltatás) csak egy erőforráshoz való hozzáférés.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-134">A Service SAS delegates access to a resource in just one of the storage services (blob, queue, table or file service).</span></span> <span data-ttu-id="e0d1e-135">A szolgáltatás SAS összeállításához, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e0d1e-135">To construct a Service SAS, do the following:</span></span>

1. <span data-ttu-id="e0d1e-136">Adja meg az aláírt verzió mező</span><span class="sxs-lookup"><span data-stu-id="e0d1e-136">Specify the Signed Version Field</span></span>

2. <span data-ttu-id="e0d1e-137">Adja meg az aláírt erőforrás (Blob és csak a File szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="e0d1e-137">Specify the Signed Resource (Blob and File Service Only)</span></span>

3. <span data-ttu-id="e0d1e-138">Válaszfejlécek (Blob és csak a fájl szolgáltatás) felülbírálása a lekérdezési paraméterek megadása</span><span class="sxs-lookup"><span data-stu-id="e0d1e-138">Specify Query Parameters to Override Response Headers (Blob Service and File Service Only)</span></span>

4. <span data-ttu-id="e0d1e-139">Adja meg a táblanevet (csak a Table szolgáltatás)</span><span class="sxs-lookup"><span data-stu-id="e0d1e-139">Specify the Table Name (Table Service Only)</span></span>

5. <span data-ttu-id="e0d1e-140">Adja meg a hozzáférési házirend</span><span class="sxs-lookup"><span data-stu-id="e0d1e-140">Specify the Access Policy</span></span>

6. <span data-ttu-id="e0d1e-141">Adja meg az aláírás érvényességi időszakának</span><span class="sxs-lookup"><span data-stu-id="e0d1e-141">Specify the Signature Validity Interval</span></span>

8. <span data-ttu-id="e0d1e-142">Engedélyek megadása</span><span class="sxs-lookup"><span data-stu-id="e0d1e-142">Specify Permissions</span></span>

9. <span data-ttu-id="e0d1e-143">Adja meg az IP-cím vagy IP-címtartomány</span><span class="sxs-lookup"><span data-stu-id="e0d1e-143">Specify IP Address or IP Range</span></span>

10. <span data-ttu-id="e0d1e-144">Adja meg a HTTP protokoll</span><span class="sxs-lookup"><span data-stu-id="e0d1e-144">Specify the HTTP Protocol</span></span>

11. <span data-ttu-id="e0d1e-145">Adja meg a hozzáférés tartomány</span><span class="sxs-lookup"><span data-stu-id="e0d1e-145">Specify Table Access Ranges</span></span>

12. <span data-ttu-id="e0d1e-146">Adja meg az aláírt azonosítója</span><span class="sxs-lookup"><span data-stu-id="e0d1e-146">Specify the Signed Identifier</span></span>

13. <span data-ttu-id="e0d1e-147">Adja meg az aláírás</span><span class="sxs-lookup"><span data-stu-id="e0d1e-147">Specify the Signature</span></span>

<span data-ttu-id="e0d1e-148">Részletes utasítások, lásd: [hozhat létre, egy szolgáltatás SAS](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).</span><span class="sxs-lookup"><span data-stu-id="e0d1e-148">For more detailed instructions, see [Constructing a Service SAS](https://docs.microsoft.com/rest/api/storageservices/Constructing-a-Service-SAS?redirectedfrom=MSDN).</span></span>

#### <a name="how-do-i-construct-an-account-sas"></a><span data-ttu-id="e0d1e-149">Hogyan hozható létre egy fiókot SAS?</span><span class="sxs-lookup"><span data-stu-id="e0d1e-149">How do I construct an Account SAS?</span></span>

<span data-ttu-id="e0d1e-150">Egy fiók SAS egy vagy több, a tárolási szolgáltatások erőforrásokhoz való hozzáférést biztosít.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-150">An Account SAS delegates access to resources in one or more of the storage services.</span></span> <span data-ttu-id="e0d1e-151">A blobtárolók, táblák, üzenetsorok és fájlmegosztások olvasási, írási és törlési műveleteihez is hozzáférést biztosíthat, amelyeket a szolgáltatásalapú SAS nem engedélyez.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-151">You can also delegate access to read, write, and delete operations on blob containers, tables, queues, and file shares that are not permitted with a service SAS.</span></span> <span data-ttu-id="e0d1e-152">Egy fiók SAS építése hasonlít a szolgáltatás SAS.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-152">Construction of an Account SAS is similar to that of a Service SAS.</span></span> <span data-ttu-id="e0d1e-153">Részletes útmutatásért lásd: [hozhat létre egy fiókot SAS.](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN)</span><span class="sxs-lookup"><span data-stu-id="e0d1e-153">For detailed instructions, see [Constructing an Account SAS.](https://docs.microsoft.com/rest/api/storageservices/Constructing-an-Account-SAS?redirectedfrom=MSDN)</span></span>

#### <a name="how-do-i-enforce-https-when-calling-rest-apis"></a><span data-ttu-id="e0d1e-154">Hogyan kényszerítése meg HTTPS, amikor a program meghívja a REST API-k?</span><span class="sxs-lookup"><span data-stu-id="e0d1e-154">How do I enforce HTTPS when calling REST APIs?</span></span>

<span data-ttu-id="e0d1e-155">A HTTPS protokoll használatát a storage-fiókok hozzáférést objektumokhoz REST API-k meghívásakor kényszerítéséhez engedélyezheti biztonságos átviteléhez szükséges a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-155">To enforce the use of HTTPS when calling REST APIs to access objects in storage accounts, you can enable Secure Transfer Required for the storage account.</span></span> 

1. <span data-ttu-id="e0d1e-156">Válassza ki az Azure-portálon **Storage-fiók létrehozása**, vagy meglévő tárfiókot, válassza a **beállítások** , majd **konfigurációs**.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-156">In the Azure portal, select **Create Storage Account**, or for an existing storage account, select **Settings** and then **Configuration**.</span></span>

2. <span data-ttu-id="e0d1e-157">A **biztonságos átviteléhez szükséges**, jelölje be **engedélyezve**.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-157">Under **Secure Transfer Required**, select **Enabled**.</span></span>

![A storage-fiók létrehozása](media/protect-personal-data-in-transit-encryption/create-storage-account.png)

<span data-ttu-id="e0d1e-159">Részletesebb leírását, beleértve a programozott módon engedélyezze a biztonságos átviteléhez szükséges tudnivalókat [szükséges biztonságos átviteli](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).</span><span class="sxs-lookup"><span data-stu-id="e0d1e-159">For more detailed instructions, including how to enable Secure Transfer Required programmatically, see [Require Secure Transfer](https://docs.microsoft.com/azure/storage/storage-require-secure-transfer).</span></span>

#### <a name="how-do-i-encrypt-data-in-azure-file-storage"></a><span data-ttu-id="e0d1e-160">Hogyan titkosítják az adatokat az Azure File Storage?</span><span class="sxs-lookup"><span data-stu-id="e0d1e-160">How do I encrypt data in Azure File Storage?</span></span>

<span data-ttu-id="e0d1e-161">Az átvitt adatokat titkosítani [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), használhatja az SMB Windows 8, 8.1 és 10 és Windows Server 2012 R2 és Windows Server 2016 3.x.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-161">To encrypt data in transit with [Azure File Storage](https://docs.microsoft.com/azure/storage/storage-file-how-to-use-files-portal), you can use SMB 3.x with Windows 8, 8.1, and 10 and with Windows Server 2012 R2 and Windows Server 2016.</span></span> <span data-ttu-id="e0d1e-162">Az Azure Fileshoz szolgáltatást használja, bármilyen titkosítás nélküli kapcsolatot sikertelen lesz, ha engedélyezve van a "Biztonságos szükséges átviteli".</span><span class="sxs-lookup"><span data-stu-id="e0d1e-162">When you are using the Azure Files service, any connection without encryption fails when "Secure transfer required" is enabled.</span></span> <span data-ttu-id="e0d1e-163">Ez magában foglalja az SMB 2.1, az SMB 3.0-titkosítás nélkül és az egyes változatban is elkészíti a Linux SMB-ügyfél a forgatókönyveket.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-163">This includes scenarios using SMB 2.1, SMB 3.0 without encryption, and some flavors of the Linux SMB client.</span></span>

#### <a name="azure-client-side-encryption"></a><span data-ttu-id="e0d1e-164">Az Azure ügyféloldali titkosítás</span><span class="sxs-lookup"><span data-stu-id="e0d1e-164">Azure Client-Side Encryption</span></span>

<span data-ttu-id="e0d1e-165">Másik lehetőségként a személyes adatok védelme alatt álló ügyfélalkalmazást és az Azure Storage közötti átvitel közben [ügyféloldali titkosítás](https://docs.microsoft.com/azure/storage/storage-client-side-encryption).</span><span class="sxs-lookup"><span data-stu-id="e0d1e-165">Another option for protecting personal data while it’s being transferred between a client application and Azure Storage is [Client-side Encryption](https://docs.microsoft.com/azure/storage/storage-client-side-encryption).</span></span> <span data-ttu-id="e0d1e-166">Az Azure Storage átvitele előtt titkosítja az adatokat, és amikor az adatok Azure Storage-ból, az adatok visszafejtése ügyféloldali fogadását követően.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-166">The data is encrypted before being transferred into Azure Storage and when you retrieve the data from Azure Storage, the data is decrypted after it is received on the client side.</span></span>

### <a name="azure-site-to-site-vpn"></a><span data-ttu-id="e0d1e-167">Az Azure--webhelyek közötti VPN</span><span class="sxs-lookup"><span data-stu-id="e0d1e-167">Azure Site-to-Site VPN</span></span>

<span data-ttu-id="e0d1e-168">Hatékony módot személyes adatok védelmére átvitel a vállalati hálózathoz vagy a felhasználó és az Azure virtuális hálózat között, hogy használja a [pont-pont](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) vagy [pont-pont](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) virtuális magánhálózati (VPN).</span><span class="sxs-lookup"><span data-stu-id="e0d1e-168">An effective way to protect personal data in transit between a corporate network or user and the Azure virtual network is to use a [site-to-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) or [point-to-site](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal) Virtual Private Network (VPN).</span></span> <span data-ttu-id="e0d1e-169">VPN-kapcsolatot hoz létre a titkosított biztonságos alagutat az interneten keresztül.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-169">A VPN connection creates a secure encrypted tunnel across the Internet.</span></span>

#### <a name="how-do-i-create-a-site-to-site-vpn-connection"></a><span data-ttu-id="e0d1e-170">Hogyan hozható létre a pont-pont VPN-kapcsolatot?</span><span class="sxs-lookup"><span data-stu-id="e0d1e-170">How do I create a site-to-site VPN connection?</span></span>

<span data-ttu-id="e0d1e-171">A telephelyek közötti VPN a vállalati hálózaton lévő több felhasználó csatlakozik Azure.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-171">A site-to-site VPN connects multiple users on the corporate network to Azure.</span></span> <span data-ttu-id="e0d1e-172">Pont-pont kapcsolat létrehozása az Azure portálon, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="e0d1e-172">To create a site-to-site connection in the Azure portal, do the following:</span></span>

1. <span data-ttu-id="e0d1e-173">Hozzon létre egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-173">Create a virtual network.</span></span>

2. <span data-ttu-id="e0d1e-174">Adjon meg egy DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-174">Specify a DNS server.</span></span>

3. <span data-ttu-id="e0d1e-175">Hozza létre az átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-175">Create the gateway subnet.</span></span>

4. <span data-ttu-id="e0d1e-176">Hozza létre a VPN-átjárót.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-176">Create the VPN gateway.</span></span> 

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-01.png)

5. <span data-ttu-id="e0d1e-177">A helyi hálózati átjáró létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-177">Create the local network gateway.</span></span>

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-02.png)

6. <span data-ttu-id="e0d1e-178">Konfigurálja a VPN-eszköz.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-178">Configure your VPN device.</span></span>

7. <span data-ttu-id="e0d1e-179">A VPN-kapcsolat létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-179">Create the VPN connection.</span></span>

    ![](media/protect-personal-data-in-transit-encryption/vpn-step-03.png)

8. <span data-ttu-id="e0d1e-180">A VPN-kapcsolat ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-180">Verify the VPN connection.</span></span>

<span data-ttu-id="e0d1e-181">A helyek kapcsolat létrehozása az Azure portálon részletes utasításokért lásd: [hozzon létre egy webhelyek kapcsolatot az Azure portálon.] (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)</span><span class="sxs-lookup"><span data-stu-id="e0d1e-181">For more detailed instructions on how to create a site-to-site connection in the Azure portal, see [Create a Site-to-Site  connection in the Azure Portal.] (https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal)</span></span>

#### <a name="how-do-i-create-a-point-to-site-vpn-connection"></a><span data-ttu-id="e0d1e-182">Hogyan hozható létre egy pont – hely VPN-kapcsolatot?</span><span class="sxs-lookup"><span data-stu-id="e0d1e-182">How do I create a point-to-site VPN connection?</span></span>

<span data-ttu-id="e0d1e-183">A pont-pont VPN biztonságos kapcsolatot hoz létre egyéni ügyfél-számítógépről egy virtuális hálózathoz.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-183">A Point-to-Site VPN creates a secure connection from an individual client computer to a virtual network.</span></span> <span data-ttu-id="e0d1e-184">Ez akkor hasznos, ha, amelyhez csatlakozni az Azure-bA egy távoli helyről, például otthoni vagy a szállodák vagy konferencia központból.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-184">This is useful when you  want to connect to Azure from a remote location, such as from home or a hotel or conference center.</span></span> <span data-ttu-id="e0d1e-185">Pont – hely kapcsolat létrehozása az Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="e0d1e-185">To create a point-to-site  connection in the Azure portal,</span></span>

1. <span data-ttu-id="e0d1e-186">Hozzon létre egy virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-186">Create a virtual network.</span></span>

2. <span data-ttu-id="e0d1e-187">Adjon hozzá egy átjáró-alhálózatot.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-187">Add a gateway subnet.</span></span>

3. <span data-ttu-id="e0d1e-188">Adjon meg egy DNS-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-188">Specify a DNS server.</span></span> <span data-ttu-id="e0d1e-189">(nem kötelező)</span><span class="sxs-lookup"><span data-stu-id="e0d1e-189">(optional)</span></span>

4. <span data-ttu-id="e0d1e-190">A virtuális hálózati átjáró létrehozása.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-190">Create a virtual network gateway.</span></span>

5. <span data-ttu-id="e0d1e-191">Tanúsítványok előállításához.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-191">Generate certificates.</span></span>

6. <span data-ttu-id="e0d1e-192">Adja hozzá az ügyfél-címkészlet.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-192">Add the client address pool.</span></span>

7. <span data-ttu-id="e0d1e-193">Töltse fel a legfelső szintű tanúsítványának nyilvános tanúsítvány adatait.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-193">Upload the root certificate public certificate data.</span></span>

8. <span data-ttu-id="e0d1e-194">Létrehozni, és a VPN-ügyfélcsomag konfigurációs telepítse.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-194">Generate and install the VPN client configuration package.</span></span>

9. <span data-ttu-id="e0d1e-195">Telepíti az exportált ügyféltanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-195">Install an exported client certificate.</span></span>

10. <span data-ttu-id="e0d1e-196">Csatlakozás az Azure-bA.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-196">Connect to Azure.</span></span>

11. <span data-ttu-id="e0d1e-197">Ellenőrizze a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-197">Verify your connection.</span></span>

<span data-ttu-id="e0d1e-198">Részletes utasítások, lásd: [egy Vnetet tanúsítvány hitelesítése a pont-pont kapcsolat: Azure-portálon.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)</span><span class="sxs-lookup"><span data-stu-id="e0d1e-198">For more detailed instructions, see [Configure a Point-to-Site connection to a VNet using certificate authentication: Azure Portal.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-point-to-site-resource-manager-portal)</span></span>

### <a name="ssltls"></a><span data-ttu-id="e0d1e-199">SSL/TLS</span><span class="sxs-lookup"><span data-stu-id="e0d1e-199">SSL/TLS</span></span>

<span data-ttu-id="e0d1e-200">A Microsoft azt javasolja, hogy mindig SSL/TLS protokollokat használ az exchange-adatok különböző helyek között.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-200">Microsoft recommends that you always use SSL/TLS protocols to exchange data across different locations.</span></span> <span data-ttu-id="e0d1e-201">A szervezetek, amelyek [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) nagy adatkészletek áthelyezése egy dedikált nagy sebességű WAN-KAPCSOLATON keresztül hivatkozásra is az adatokat titkosíthatja az alkalmazás szintjén SSL/TLS-vagy egyéb protokollok, a hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-201">Organizations that choose to use [ExpressRoute](https://docs.microsoft.com/azure/expressroute/) to move large data sets over a dedicated high-speed WAN link can also encrypt the data at the application-level using SSL/TLS or other protocols for added protection.</span></span>

### <a name="encryption-by-default"></a><span data-ttu-id="e0d1e-202">Alapértelmezés szerint titkosítás</span><span class="sxs-lookup"><span data-stu-id="e0d1e-202">Encryption by default</span></span>

<span data-ttu-id="e0d1e-203">Microsoft-titkosítást használ az adatok védelmére átvitel ügyfelek és az Azure felhőszolgáltatások között.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-203">Microsoft uses encryption to protect data in transit between customers and Azure cloud services.</span></span> <span data-ttu-id="e0d1e-204">Az Azure Storage az Azure-portálon való interakció, minden tranzakció történik meg HTTPS használatával.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-204">If you are interacting with Azure Storage through the Azure Portal, all transactions occur via HTTPS.</span></span>

<span data-ttu-id="e0d1e-205">[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) a protokoll, amelyet a Microsoft azon adatközpontjainak megkísérli egyeztetni az ügyfélszámítógépeken, a Microsoft felhőalapú szolgáltatásokhoz.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-205">[Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security) (TLS) is the protocol that Microsoft data centers will attempt to negotiate with client systems that connect to Microsoft cloud services.</span></span> <span data-ttu-id="e0d1e-206">A TLS nyújt erős hitelesítés, üzenetvédelem, és integritását (lehetővé teszi, hogy a üzenet illetéktelen hozzáférés és hamisítására észlelése), együttműködés, az algoritmusok rugalmassága, könnyű telepítése és használata.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-206">TLS provides strong authentication, message privacy, and integrity (enables detection of message tampering, interception, and forgery), interoperability, algorithm flexibility, ease of deployment and use.</span></span>

<span data-ttu-id="e0d1e-207">[Továbbítási titkosítása tökéletes](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) is, hogy az egyes ügyfelek ügyfél és a Microsoft olyan felhőszolgáltatásai közötti kapcsolat egyedi kulcsok használata alkalmazzák.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-207">[Perfect Forward Secrecy](https://en.wikipedia.org/wiki/Forward_secrecy) (PFS) is also employed so that each connection between customers’ client systems and Microsoft’s cloud services use unique keys.</span></span> <span data-ttu-id="e0d1e-208">Kapcsolatok a Microsoft felhőszolgáltatásai kihasználhatják alapú RSA 2048 bites titkosítást kulcshossza.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-208">Connections to Microsoft cloud services also take advantage of RSA based 2,048-bit encryption key lengths.</span></span> <span data-ttu-id="e0d1e-209">A TLS, RSA 2048 bit, kulcshosszok kombinációja, így a PFS jelentős mértékben megnehezíti mások intercept és a hozzáférési adatok, amely a Microsoft más felhőszolgáltatásaival és az ügyfelek közötti átvitel közben.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-209">The combination of TLS, RSA 2,048-bit key lengths, and PFS makes it much  more difficult for someone to intercept and access data that is in transit between Microsoft cloud services and customers.</span></span>

<span data-ttu-id="e0d1e-210">Adatok az átvitel során mindig titkosítja az [Data Lake Store] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview).</span><span class="sxs-lookup"><span data-stu-id="e0d1e-210">Data in transit is always encrypted in [Data Lake Store] (https://docs.microsoft.com/azure/data-lake-store/data-lake-store-security-overview).</span></span> <span data-ttu-id="e0d1e-211">Amellett, hogy az adatok titkosítása az állandó adathordozón való tárolás előtt történik meg, az átvitt adatok is mindig titkosítva vannak HTTPS segítségével.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-211">In addition to encrypting data prior to storing to persistent media, the data is also always secured in transit by using HTTPS.</span></span> <span data-ttu-id="e0d1e-212">A HTTPS az egyetlen olyan protokoll, amely támogatott a Data Lake Store REST-felületeihez.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-212">HTTPS is the only protocol that is supported for the Data Lake Store REST interfaces.</span></span>

## <a name="summary"></a><span data-ttu-id="e0d1e-213">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="e0d1e-213">Summary</span></span>

<span data-ttu-id="e0d1e-214">A vállalat személyes adatok és az ilyen adatok védelmének azáltal, Azure Storage HTTPS-kapcsolatok, megosztott hozzáférési aláírásokkal használatával biztonságos átviteléhez szükséges engedélyezését a storage-fiókok a cél megvalósításához.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-214">The company can accomplish its goal of protecting personal data and the privacy of such data by enforcing HTTPS connections to Azure Storage, using Shared Access Signatures and enabling Secure Transfer Required on the storage accounts.</span></span> <span data-ttu-id="e0d1e-215">Személyes adatok védelme az SMB 3.0-kapcsolatot is használ, és ügyféloldali titkosítás végrehajtásával is.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-215">They can also protect personal data by using SMB 3.0 connections and implementing client-side encryption.</span></span> <span data-ttu-id="e0d1e-216">Pont-pont VPN-kapcsolatok az Azure virtuális hálózat számára a vállalati hálózatról és a pont-pont VPN-kapcsolatok az egyes felhasználók létrehoz egy biztonságos csatornán keresztül, amelyek személyes adatokat is biztonságosan változatlan marad.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-216">Site-to-site VPN connections from the corporate network to the Azure virtual network and point-to-site VPN connections from individual users will create a secure tunnel through which personal data can securely travel.</span></span> <span data-ttu-id="e0d1e-217">A Microsoft alapértelmezett titkosítási eljárásokat további megvédi az személyes adatok védelme.</span><span class="sxs-lookup"><span data-stu-id="e0d1e-217">Microsoft’s default encryption practices will further protect the privacy of personal data.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e0d1e-218">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e0d1e-218">Next steps</span></span>

- [<span data-ttu-id="e0d1e-219">Az Azure Data biztonsági és a titkosítás gyakorlati tanácsok</span><span class="sxs-lookup"><span data-stu-id="e0d1e-219">Azure Data Security and Encryption Best Practices</span></span>](https://docs.microsoft.com/azure/security/azure-security-data-encryption-best-practices)

- [<span data-ttu-id="e0d1e-220">A VPN Gateway tervezése és kialakítása</span><span class="sxs-lookup"><span data-stu-id="e0d1e-220">Planning and design for VPN Gateway</span></span>](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design)

- [<span data-ttu-id="e0d1e-221">VPN Gateway – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="e0d1e-221">VPN Gateway FAQ</span></span>](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vpn-faq)

- [<span data-ttu-id="e0d1e-222">Vásároljon és SSL-tanúsítvány konfigurálása az Azure App Service</span><span class="sxs-lookup"><span data-stu-id="e0d1e-222">Buy and configure an SSL Certificate for your Azure App Service</span></span>](https://docs.microsoft.com/azure/app-service-web/web-sites-purchase-ssl-web-site)
