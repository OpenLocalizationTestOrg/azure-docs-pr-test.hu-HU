---
title: "Az egyéni Azure CDN-tartomány HTTPS engedélyezése |} Microsoft Docs"
description: "Útmutató a HTTPS engedélyezése az Azure CDN-végpont egy egyéni tartomány."
services: cdn
documentationcenter: 
author: camsoper
manager: erikre
editor: 
ms.assetid: 10337468-7015-4598-9586-0b66591d939b
ms.service: cdn
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: casoper
ms.openlocfilehash: b334ba6bbec1d0a7e23a514174bffae01c7fff05
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enable-https-on-an-azure-cdn-custom-domain"></a><span data-ttu-id="a3c8b-103">Az egyéni Azure CDN-tartomány HTTPS engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a3c8b-103">Enable HTTPS on an Azure CDN custom domain</span></span>

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

<span data-ttu-id="a3c8b-104">Egyéni Azure CDN-tartomány HTTPS támogatása lehetővé teszi, hogy a tartalmak biztonságos saját tartománynév használatával, melyekkel biztonságosabbá teheti az átvitel során az adatok SSL használatával.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-104">HTTPS support for Azure CDN custom domains enables you to deliver secure content via SSL using your own domain name to improve the security of data while in transit.</span></span> <span data-ttu-id="a3c8b-105">A végpontok közötti munkafolyamat HTTPS engedélyezése az egyéni tartomány egyszerűsödött, egy kattintással engedélyezését, teljes és minden további költség nélkül.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-105">The end-to-end workflow to enable HTTPS for your custom domain is simplified via one-click enablement, complete certificate management, and all with no additional cost.</span></span>

<span data-ttu-id="a3c8b-106">Nagyon fontos adatvédelmi és a webes alkalmazások bizalmas adatok az átvitel alatt adatok integritásának biztosításához.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-106">It's critical to ensure the privacy and data integrity of all of your web applications sensitive data while in transit.</span></span> <span data-ttu-id="a3c8b-107">A HTTPS protokoll használatával biztosítja, hogy a bizalmas adatok titkosítva van, amikor az interneten keresztül továbbítja azokat.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-107">Using the HTTPS protocol ensures that your sensitive data is encrypted when it is sent across the internet.</span></span> <span data-ttu-id="a3c8b-108">Megbízható, hitelesítés és a webes alkalmazások megvédi a támadások biztosít.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-108">It provides trust, authentication and protects your web applications from attacks.</span></span> <span data-ttu-id="a3c8b-109">Azure CDN jelenleg a CDN-végpont támogatja a HTTPS PROTOKOLLT.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-109">Currently, Azure CDN supports HTTPS on a CDN endpoint.</span></span> <span data-ttu-id="a3c8b-110">Például, ha a CDN-végpont létrehozása az Azure CDN (pl. https://contoso.azureedge.net), HTTPS alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-110">For example, if you create a CDN endpoint from Azure CDN (e.g. https://contoso.azureedge.net), HTTPS is enabled by default.</span></span> <span data-ttu-id="a3c8b-111">Most az egyéni tartomány HTTPS, engedélyezheti a biztonságos kézbesítés egy egyéni tartomány (pl. https://www.contoso.com) is.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-111">Now, with custom domain HTTPS, you can enable secure delivery for a custom domain (e.g. https://www.contoso.com) as well.</span></span> 

<span data-ttu-id="a3c8b-112">A legfőbb attribútumai a HTTPS-szolgáltatás a következők:</span><span class="sxs-lookup"><span data-stu-id="a3c8b-112">Some of the key attributes of HTTPS feature are:</span></span>

- <span data-ttu-id="a3c8b-113">További költség nélkül: a tanúsítvány-beszerzéssel vagy megújítása nem költségekkel és a HTTPS-forgalmat további költség nélkül.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-113">No additional cost: There are no costs for certificate acquisition or renewal and no additional cost for HTTPS traffic.</span></span> <span data-ttu-id="a3c8b-114">Ugyanúgy kell fizetnie GB kilépő a CDN.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-114">You just pay for GB egress from the CDN.</span></span>

- <span data-ttu-id="a3c8b-115">Egyszerű engedélyezése: egy kattintson kiépítés érhető el a [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a3c8b-115">Simple enablement: One click provisioning is available from the [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="a3c8b-116">REST API vagy egyéb fejlesztői eszközök segítségével is engedélyezze a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-116">You can also use REST API or other developer tools to enable the feature.</span></span>

- <span data-ttu-id="a3c8b-117">Fejezze be a Tanúsítványkezelő: az összes tanúsítvány beszerzésének és kezelik a kulcsokat meg.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-117">Complete certificate management: All certificate procurement and management is handled for you.</span></span> <span data-ttu-id="a3c8b-118">A tanúsítványok automatikusan kiépítése és megújítani lejárta előtt.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-118">Certificates are automatically provisioned and renewed prior to expiration.</span></span> <span data-ttu-id="a3c8b-119">Ez teljesen eltávolítja a szolgáltatás megszakítás miatt a tanúsítvány lejárati ideje kockázatát.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-119">This completely removes the risks of service interruption as a result of a certificate expiring.</span></span>

>[!NOTE] 
><span data-ttu-id="a3c8b-120">Mielőtt engedélyezné a HTTPS PROTOKOLLT támogatja, akkor kell már létrehozott egy [egyéni Azure CDN-tartomány](./cdn-map-content-to-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="a3c8b-120">Prior to enabling HTTPS support, you must have already established an [Azure CDN custom domain](./cdn-map-content-to-custom-domain.md).</span></span>

## <a name="step-1-enabling-the-feature"></a><span data-ttu-id="a3c8b-121">1. lépés: A funkció engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a3c8b-121">Step 1: Enabling the feature</span></span> 

1. <span data-ttu-id="a3c8b-122">Az a [Azure-portálon](https://portal.azure.com), tallózással keresse meg a Verizon standard vagy prémium szintű CDN-profilt.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-122">In the [Azure portal](https://portal.azure.com), browse to your Verizon standard or premium CDN profile.</span></span>

2. <span data-ttu-id="a3c8b-123">A végpontok, kattintson az egyéni tartomány tartalmazó végpont.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-123">In the list of endpoints, click the endpoint containing your custom domain.</span></span>

3. <span data-ttu-id="a3c8b-124">Kattintson az egyéni tartománynak a HTTPS engedélyezéséhez.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-124">Click the custom domain for which you want to enable HTTPS.</span></span>

    ![Végpont panel](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. <span data-ttu-id="a3c8b-126">Kattintson a **a** HTTPS engedélyezése és a módosítás mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-126">Click **On** to enable HTTPS and save the change.</span></span>

    ![Egyéni HTTPS párbeszédpanel](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


## <a name="step-2-domain-validation"></a><span data-ttu-id="a3c8b-128">2. lépés: Tartomány ellenőrzéséhez</span><span class="sxs-lookup"><span data-stu-id="a3c8b-128">Step 2: Domain validation</span></span>

>[!IMPORTANT] 
><span data-ttu-id="a3c8b-129">Tartomány ellenőrzéséhez kell végeznie, mielőtt a saját egyéni tartománynévre HTTPS aktív lesz.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-129">You must complete domain validation before HTTPS will be active on your custom domain.</span></span> <span data-ttu-id="a3c8b-130">6 üzleti napja hagyja jóvá a tartományban van.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-130">You have 6 business days to approve the domain.</span></span> <span data-ttu-id="a3c8b-131">Kérelem 6 munkanapon belül nem jóváhagyással végrehajtások leállnak.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-131">Request will be canceled with no approval within 6 business days.</span></span>  

<span data-ttu-id="a3c8b-132">Miután engedélyezte az egyéni tartomány a HTTPS, a HTTPS-tanúsítvány-szolgáltatója DigiCert fogja ellenőrizni a tulajdonjogát, a tartomány kapcsolatba lépve a bejegyzés a tartományba, WHOIS bejegyzés információk, e-mailben (alapértelmezés) vagy telefonos alapján.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-132">After enabling HTTPS on your custom domain, our HTTPS certificate provider DigiCert will validate ownership of your domain by contacting the registrant for your domain, based on WHOIS registrant information, via email (by default) or phone.</span></span> <span data-ttu-id="a3c8b-133">DigiCert is el fogja küldeni az ellenőrző e-mailt az alábbi címek.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-133">DigiCert will also send the verification email to the below addresses.</span></span> <span data-ttu-id="a3c8b-134">Ha WHOIS bejegyzés információk személyes, győződjön meg arról jóváhagyhatja közvetlenül az egyik ezeknél a címeknél.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-134">If WHOIS registrant information is private, make sure you can approve directly from one of these addresses.</span></span>

><span data-ttu-id="a3c8b-135">felügyeleti @< your-tartományi-name.com > rendszergazda @< your-tartományi-name.com ></span><span class="sxs-lookup"><span data-stu-id="a3c8b-135">admin@<your-domain-name.com> administrator@<your-domain-name.com></span></span>  
><span data-ttu-id="a3c8b-136">gazdáját @< your-tartományi-name.com ></span><span class="sxs-lookup"><span data-stu-id="a3c8b-136">webmaster@<your-domain-name.com></span></span>  
><span data-ttu-id="a3c8b-137">hostmaster @< your-tartományi-name.com ></span><span class="sxs-lookup"><span data-stu-id="a3c8b-137">hostmaster@<your-domain-name.com></span></span>  
><span data-ttu-id="a3c8b-138">postamester @< your-tartományi-name.com ></span><span class="sxs-lookup"><span data-stu-id="a3c8b-138">postmaster@<your-domain-name.com></span></span>


<span data-ttu-id="a3c8b-139">E-mail fogadása után ellenőrzési két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="a3c8b-139">Upon receiving the email, you have two verification options:</span></span>

1. <span data-ttu-id="a3c8b-140">Ugyanazt a fiókot a legfelső szintű tartománynak, pl. consoto.com keresztül az összes jövőbeni megrendelések jóváhagyhatja.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-140">You can approve all future orders placed through the same account for the same root domain, e.g. consoto.com.</span></span> <span data-ttu-id="a3c8b-141">Ez az ajánlott módszer, abban az esetben, ha azt tervezi, hogy további egyéni tartományokat a jövőben ugyanazon gyökértartományának hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-141">This is a recommended approach if you are planning to add additional custom domains in the future for the same root domain.</span></span>
 
2. <span data-ttu-id="a3c8b-142">A megadott állomásnév, amelyet a kérés ugyanúgy jóváhagyhatja.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-142">You can just approve the specific host name used in this request.</span></span> <span data-ttu-id="a3c8b-143">További jóváhagyás lesz szükséges további kérelmeknél.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-143">Additional approval will be required for subsequent requests.</span></span>

    <span data-ttu-id="a3c8b-144">Példa e-mailek:</span><span class="sxs-lookup"><span data-stu-id="a3c8b-144">Example email:</span></span>
    
    ![Egyéni HTTPS párbeszédpanel](./media/cdn-custom-ssl/domain-validation-email-example.png)

<span data-ttu-id="a3c8b-146">A jóváhagyást követően DigiCert felveszi az egyéni tartománynevet a SAN-tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-146">After approval, DigiCert will add your custom domain name to the SAN certificate.</span></span> <span data-ttu-id="a3c8b-147">Az egy évet a tanúsítvány érvényes lesz, és automatikusan megújul lejárta előtt.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-147">The certificate will be valid for one year and will be auto renewed before it's expired.</span></span>

## <a name="step-3-wait-for-the-propagation-then-start-using-your-feature"></a><span data-ttu-id="a3c8b-148">3. lépés: A propagálás várja meg, majd indítsa el, a szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="a3c8b-148">Step 3: Wait for the propagation then start using your feature</span></span>

<span data-ttu-id="a3c8b-149">A tartománynév érvényesítését követően az egyéni tartomány HTTPS funkció aktív akár 6 – 8 óráig fog tartani.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-149">After the domain name is validated it will take up to 6-8 hours for the custom domain HTTPS feature to be active.</span></span> <span data-ttu-id="a3c8b-150">A folyamat befejezése után, az Azure portálon "egyéni HTTPS" állapotát állítja be "Engedélyezett".</span><span class="sxs-lookup"><span data-stu-id="a3c8b-150">After the process is complete, the "custom HTTPS" status in the Azure portal will be set to "Enabled".</span></span> <span data-ttu-id="a3c8b-151">HTTPS-FORGALOMHOZ pedig az egyéni tartomány már készen áll a használatra.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-151">HTTPS with your custom domain is now ready for your use.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="a3c8b-152">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="a3c8b-152">Frequently asked questions</span></span>

1. <span data-ttu-id="a3c8b-153">*A tanúsítványszolgáltató és használt tanúsítvány típusa?*</span><span class="sxs-lookup"><span data-stu-id="a3c8b-153">*Who is the certificate provider and what type of certificate is used?*</span></span>

    <span data-ttu-id="a3c8b-154">Tulajdonos alternatív neve (SAN) tanúsítvány DigiCert által biztosított használjuk.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-154">We use Subject Alternative Names (SAN) certificate provided by DigiCert.</span></span> <span data-ttu-id="a3c8b-155">SAN-tanúsítványnak biztonságossá teheti az egy tanúsítvánnyal rendelkező több teljesen minősített tartománynév.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-155">A SAN certificate can secure multiple fully qualifIed domain names with one certificate.</span></span>

2. <span data-ttu-id="a3c8b-156">*A dedikált tanúsítvány is használható?*</span><span class="sxs-lookup"><span data-stu-id="a3c8b-156">*Can I use my dedicated certificate?*</span></span>
    
    <span data-ttu-id="a3c8b-157">Jelenleg nem de a programba.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-157">Not currently, but it's on the roadmap.</span></span>

3. <span data-ttu-id="a3c8b-158">*Mi történik, ha a tartomány megerősítési e-mailt nem jelenik meg a DigiCert?*</span><span class="sxs-lookup"><span data-stu-id="a3c8b-158">*What if I don't receive the domain verification email from DigiCert?*</span></span>

    <span data-ttu-id="a3c8b-159">Lépjen kapcsolatba a Microsofttal, ha 24 órán belül nem kap egy e-mailt.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-159">Please contact Microsoft if you don't receive an email within 24 hours.</span></span>

4. <span data-ttu-id="a3c8b-160">*Kevésbé biztonságos, mint egy dedikált tanúsítvány SAN tanúsítványt használ?*</span><span class="sxs-lookup"><span data-stu-id="a3c8b-160">*Is using a SAN certificate less secure than a dedicated certificate?*</span></span>
    
    <span data-ttu-id="a3c8b-161">Egy SAN-tanúsítvány, egy dedikált cert azonos titkosítási és biztonsági szabványok követi.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-161">A SAN cert follows the same encryption and security standards as a dedicated cert.</span></span> <span data-ttu-id="a3c8b-162">Összes kiállított SSL-tanúsítvány SHA-256 algoritmust használ a fokozott biztonság.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-162">All issued SSL certificates are using SHA-256 for enhanced server security.</span></span>

5. <span data-ttu-id="a3c8b-163">*Használható-e az egyéni tartomány HTTPS Akamai Azure CDN?*</span><span class="sxs-lookup"><span data-stu-id="a3c8b-163">*Can I use custom domain HTTPS with Azure CDN from Akamai?*</span></span>

    <span data-ttu-id="a3c8b-164">Ez a funkció jelenleg csak Azure CDN Verizon érhető el.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-164">Currently, this feature is only available with Azure CDN from Verizon.</span></span> <span data-ttu-id="a3c8b-165">Ez a szolgáltatás Akamai Azure CDN támogatása az elkövetkező hónapokban dolgozunk.</span><span class="sxs-lookup"><span data-stu-id="a3c8b-165">We are working on supporting this feature with Azure CDN from Akamai in the coming months.</span></span>


## <a name="next-steps"></a><span data-ttu-id="a3c8b-166">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a3c8b-166">Next steps</span></span>

- <span data-ttu-id="a3c8b-167">Ismerje meg, hogyan állíthat be egy [Azure CDN-végpont egyéni tartományhoz](./cdn-map-content-to-custom-domain.md)</span><span class="sxs-lookup"><span data-stu-id="a3c8b-167">Learn how to set up a [custom domain on your Azure CDN endpoint](./cdn-map-content-to-custom-domain.md)</span></span>


