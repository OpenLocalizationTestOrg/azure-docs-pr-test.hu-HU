---
title: "AAA \"egyéni Azure CDN-tartományt a HTTPS engedélyezése |} Microsoft dokumentumok\""
description: "Megtudhatja, hogyan tooenable HTTPS az Azure CDN-végpont egy egyéni tartomány."
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
ms.openlocfilehash: 93746222616c9ed7977ec3b22c38ac1d43b118f8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-https-on-an-azure-cdn-custom-domain"></a><span data-ttu-id="d2b22-103">Az egyéni Azure CDN-tartomány HTTPS engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d2b22-103">Enable HTTPS on an Azure CDN custom domain</span></span>

[!INCLUDE [cdn-verizon-only](../../includes/cdn-verizon-only.md)]

<span data-ttu-id="d2b22-104">Egyéni Azure CDN-tartomány HTTPS támogatása lehetővé teszi toodeliver biztonságos SSL használatával saját tartomány neve tooimprove hello biztonsági adatok az átvitel alatt a tartalomhoz.</span><span class="sxs-lookup"><span data-stu-id="d2b22-104">HTTPS support for Azure CDN custom domains enables you toodeliver secure content via SSL using your own domain name tooimprove hello security of data while in transit.</span></span> <span data-ttu-id="d2b22-105">az egyéni tartomány hello végpontok közötti munkafolyamat tooenable HTTPS egyszerűsített keresztül egy kattintással engedélyezését, teljes tanúsítványkezelés, és minden további költség nélkül.</span><span class="sxs-lookup"><span data-stu-id="d2b22-105">hello end-to-end workflow tooenable HTTPS for your custom domain is simplified via one-click enablement, complete certificate management, and all with no additional cost.</span></span>

<span data-ttu-id="d2b22-106">Kritikus tooensure hello adatvédelmi és adatok integritását a webes alkalmazások a bizalmas adatok az átvitel alatt.</span><span class="sxs-lookup"><span data-stu-id="d2b22-106">It's critical tooensure hello privacy and data integrity of all of your web applications sensitive data while in transit.</span></span> <span data-ttu-id="d2b22-107">Az internet használatával a HTTPS protokoll biztosítja, hogy a bizalmas adatok titkosítva van, amikor keresztül továbbítja azokat hello hello.</span><span class="sxs-lookup"><span data-stu-id="d2b22-107">Using hello HTTPS protocol ensures that your sensitive data is encrypted when it is sent across hello internet.</span></span> <span data-ttu-id="d2b22-108">Megbízható, hitelesítés és a webes alkalmazások megvédi a támadások biztosít.</span><span class="sxs-lookup"><span data-stu-id="d2b22-108">It provides trust, authentication and protects your web applications from attacks.</span></span> <span data-ttu-id="d2b22-109">Azure CDN jelenleg a CDN-végpont támogatja a HTTPS PROTOKOLLT.</span><span class="sxs-lookup"><span data-stu-id="d2b22-109">Currently, Azure CDN supports HTTPS on a CDN endpoint.</span></span> <span data-ttu-id="d2b22-110">Például, ha a CDN-végpont létrehozása az Azure CDN (pl. https://contoso.azureedge.net), HTTPS alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="d2b22-110">For example, if you create a CDN endpoint from Azure CDN (e.g. https://contoso.azureedge.net), HTTPS is enabled by default.</span></span> <span data-ttu-id="d2b22-111">Most az egyéni tartomány HTTPS, engedélyezheti a biztonságos kézbesítés egy egyéni tartomány (pl. https://www.contoso.com) is.</span><span class="sxs-lookup"><span data-stu-id="d2b22-111">Now, with custom domain HTTPS, you can enable secure delivery for a custom domain (e.g. https://www.contoso.com) as well.</span></span> 

<span data-ttu-id="d2b22-112">Hello legfőbb attribútumai a HTTPS-szolgáltatás a következők:</span><span class="sxs-lookup"><span data-stu-id="d2b22-112">Some of hello key attributes of HTTPS feature are:</span></span>

- <span data-ttu-id="d2b22-113">További költség nélkül: a tanúsítvány-beszerzéssel vagy megújítása nem költségekkel és a HTTPS-forgalmat további költség nélkül.</span><span class="sxs-lookup"><span data-stu-id="d2b22-113">No additional cost: There are no costs for certificate acquisition or renewal and no additional cost for HTTPS traffic.</span></span> <span data-ttu-id="d2b22-114">Ugyanúgy kell fizetnie a hello CDN GB kilépő.</span><span class="sxs-lookup"><span data-stu-id="d2b22-114">You just pay for GB egress from hello CDN.</span></span>

- <span data-ttu-id="d2b22-115">Egyszerű engedélyezése: egy kattintson kiépítés rendszerhez elérhető a hello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d2b22-115">Simple enablement: One click provisioning is available from hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="d2b22-116">REST API vagy egyéb fejlesztői eszközök tooenable hello szolgáltatást is használhatja.</span><span class="sxs-lookup"><span data-stu-id="d2b22-116">You can also use REST API or other developer tools tooenable hello feature.</span></span>

- <span data-ttu-id="d2b22-117">Fejezze be a Tanúsítványkezelő: az összes tanúsítvány beszerzésének és kezelik a kulcsokat meg.</span><span class="sxs-lookup"><span data-stu-id="d2b22-117">Complete certificate management: All certificate procurement and management is handled for you.</span></span> <span data-ttu-id="d2b22-118">Tanúsítványok automatikusan törlődnek, és a korábbi tooexpiration megújítani.</span><span class="sxs-lookup"><span data-stu-id="d2b22-118">Certificates are automatically provisioned and renewed prior tooexpiration.</span></span> <span data-ttu-id="d2b22-119">Ez teljesen eltávolítja a szolgáltatás megszakítás miatt a tanúsítvány lejárati ideje hello kockázatát.</span><span class="sxs-lookup"><span data-stu-id="d2b22-119">This completely removes hello risks of service interruption as a result of a certificate expiring.</span></span>

>[!NOTE] 
><span data-ttu-id="d2b22-120">Előzetes tooenabling HTTPS PROTOKOLLT támogatja, meg kell már létrehozott egy [egyéni Azure CDN-tartomány](./cdn-map-content-to-custom-domain.md).</span><span class="sxs-lookup"><span data-stu-id="d2b22-120">Prior tooenabling HTTPS support, you must have already established an [Azure CDN custom domain](./cdn-map-content-to-custom-domain.md).</span></span>

## <a name="step-1-enabling-hello-feature"></a><span data-ttu-id="d2b22-121">1. lépés: Hello szolgáltatás engedélyezése</span><span class="sxs-lookup"><span data-stu-id="d2b22-121">Step 1: Enabling hello feature</span></span> 

1. <span data-ttu-id="d2b22-122">A hello [Azure-portálon](https://portal.azure.com), keresse meg a tooyour Verizon standard vagy prémium szintű CDN-profilt.</span><span class="sxs-lookup"><span data-stu-id="d2b22-122">In hello [Azure portal](https://portal.azure.com), browse tooyour Verizon standard or premium CDN profile.</span></span>

2. <span data-ttu-id="d2b22-123">Hello végpontok, kattintson az egyéni tartomány tartalmazó hello végpont.</span><span class="sxs-lookup"><span data-stu-id="d2b22-123">In hello list of endpoints, click hello endpoint containing your custom domain.</span></span>

3. <span data-ttu-id="d2b22-124">Kattintson a kívánt tooenable HTTPS hello egyéni tartományt.</span><span class="sxs-lookup"><span data-stu-id="d2b22-124">Click hello custom domain for which you want tooenable HTTPS.</span></span>

    ![Végpont panel](./media/cdn-custom-ssl/cdn-custom-domain.png)

4. <span data-ttu-id="d2b22-126">Kattintson a **a** tooenable HTTPS és hello módosítás mentéséhez.</span><span class="sxs-lookup"><span data-stu-id="d2b22-126">Click **On** tooenable HTTPS and save hello change.</span></span>

    ![Egyéni HTTPS párbeszédpanel](./media/cdn-custom-ssl/cdn-enable-custom-ssl.png)


## <a name="step-2-domain-validation"></a><span data-ttu-id="d2b22-128">2. lépés: Tartomány ellenőrzéséhez</span><span class="sxs-lookup"><span data-stu-id="d2b22-128">Step 2: Domain validation</span></span>

>[!IMPORTANT] 
><span data-ttu-id="d2b22-129">Tartomány ellenőrzéséhez kell végeznie, mielőtt a saját egyéni tartománynévre HTTPS aktív lesz.</span><span class="sxs-lookup"><span data-stu-id="d2b22-129">You must complete domain validation before HTTPS will be active on your custom domain.</span></span> <span data-ttu-id="d2b22-130">6 üzleti nap tooapprove hello tartományban van.</span><span class="sxs-lookup"><span data-stu-id="d2b22-130">You have 6 business days tooapprove hello domain.</span></span> <span data-ttu-id="d2b22-131">Kérelem 6 munkanapon belül nem jóváhagyással végrehajtások leállnak.</span><span class="sxs-lookup"><span data-stu-id="d2b22-131">Request will be canceled with no approval within 6 business days.</span></span>  

<span data-ttu-id="d2b22-132">Miután engedélyezte az egyéni tartomány a HTTPS, a HTTPS-tanúsítvány-szolgáltatója DigiCert fogja ellenőrizni a tulajdonjogát, a tartomány lépjen kapcsolatba az hello a bejegyzés a tartományba, WHOIS bejegyzés információk, e-mailben (alapértelmezés) vagy telefonos alapján.</span><span class="sxs-lookup"><span data-stu-id="d2b22-132">After enabling HTTPS on your custom domain, our HTTPS certificate provider DigiCert will validate ownership of your domain by contacting hello registrant for your domain, based on WHOIS registrant information, via email (by default) or phone.</span></span> <span data-ttu-id="d2b22-133">DigiCert hello ellenőrző e-mailek toohello címek alatt is el fogja küldeni.</span><span class="sxs-lookup"><span data-stu-id="d2b22-133">DigiCert will also send hello verification email toohello below addresses.</span></span> <span data-ttu-id="d2b22-134">Ha WHOIS bejegyzés információk személyes, győződjön meg arról jóváhagyhatja közvetlenül az egyik ezeknél a címeknél.</span><span class="sxs-lookup"><span data-stu-id="d2b22-134">If WHOIS registrant information is private, make sure you can approve directly from one of these addresses.</span></span>

><span data-ttu-id="d2b22-135">felügyeleti @< your-tartományi-name.com > rendszergazda @< your-tartományi-name.com ></span><span class="sxs-lookup"><span data-stu-id="d2b22-135">admin@<your-domain-name.com> administrator@<your-domain-name.com></span></span>  
><span data-ttu-id="d2b22-136">gazdáját @< your-tartományi-name.com ></span><span class="sxs-lookup"><span data-stu-id="d2b22-136">webmaster@<your-domain-name.com></span></span>  
><span data-ttu-id="d2b22-137">hostmaster @< your-tartományi-name.com ></span><span class="sxs-lookup"><span data-stu-id="d2b22-137">hostmaster@<your-domain-name.com></span></span>  
><span data-ttu-id="d2b22-138">postamester @< your-tartományi-name.com ></span><span class="sxs-lookup"><span data-stu-id="d2b22-138">postmaster@<your-domain-name.com></span></span>


<span data-ttu-id="d2b22-139">Hello e-mail fogadásakor ellenőrzési két lehetőség közül választhat:</span><span class="sxs-lookup"><span data-stu-id="d2b22-139">Upon receiving hello email, you have two verification options:</span></span>

1. <span data-ttu-id="d2b22-140">Minden további megrendelések keresztül hello hello fiókot jóváhagyhatja azonos gyökértartomány, pl. consoto.com. Ez az ajánlott módszer, ha azt tervezi, hogy további egyéni tartományok tooadd hello jövőbeli hello a legfelső szintű tartománynak.</span><span class="sxs-lookup"><span data-stu-id="d2b22-140">You can approve all future orders placed through hello same account for hello same root domain, e.g. consoto.com. This is a recommended approach if you are planning tooadd additional custom domains in hello future for hello same root domain.</span></span>
 
2. <span data-ttu-id="d2b22-141">Jóváhagyja hello adott állomás nevét, amelyet a kérés.</span><span class="sxs-lookup"><span data-stu-id="d2b22-141">You can just approve hello specific host name used in this request.</span></span> <span data-ttu-id="d2b22-142">További jóváhagyás lesz szükséges további kérelmeknél.</span><span class="sxs-lookup"><span data-stu-id="d2b22-142">Additional approval will be required for subsequent requests.</span></span>

    <span data-ttu-id="d2b22-143">Példa e-mailek:</span><span class="sxs-lookup"><span data-stu-id="d2b22-143">Example email:</span></span>
    
    ![Egyéni HTTPS párbeszédpanel](./media/cdn-custom-ssl/domain-validation-email-example.png)

<span data-ttu-id="d2b22-145">A jóváhagyást követően DigiCert adja hozzá az egyéni tartomány nevét toohello SAN-tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="d2b22-145">After approval, DigiCert will add your custom domain name toohello SAN certificate.</span></span> <span data-ttu-id="d2b22-146">egy évig hello tanúsítvány érvényes lesz, és automatikusan megújul lejárta előtt.</span><span class="sxs-lookup"><span data-stu-id="d2b22-146">hello certificate will be valid for one year and will be auto renewed before it's expired.</span></span>

## <a name="step-3-wait-for-hello-propagation-then-start-using-your-feature"></a><span data-ttu-id="d2b22-147">3. lépés: Hello propagálás várja meg, majd indítsa el, a szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="d2b22-147">Step 3: Wait for hello propagation then start using your feature</span></span>

<span data-ttu-id="d2b22-148">Hello tartománynév érvényesítését követően hello egyéni tartomány HTTPS szolgáltatás toobe aktív too6-8 órát másolatot tart.</span><span class="sxs-lookup"><span data-stu-id="d2b22-148">After hello domain name is validated it will take up too6-8 hours for hello custom domain HTTPS feature toobe active.</span></span> <span data-ttu-id="d2b22-149">Hello folyamat befejezése után, túl "Enabled" hello "egyéni HTTPS" hello Azure-portálon állapota lesz beállítva.</span><span class="sxs-lookup"><span data-stu-id="d2b22-149">After hello process is complete, hello "custom HTTPS" status in hello Azure portal will be set too"Enabled".</span></span> <span data-ttu-id="d2b22-150">HTTPS-FORGALOMHOZ pedig az egyéni tartomány már készen áll a használatra.</span><span class="sxs-lookup"><span data-stu-id="d2b22-150">HTTPS with your custom domain is now ready for your use.</span></span>

## <a name="frequently-asked-questions"></a><span data-ttu-id="d2b22-151">Gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="d2b22-151">Frequently asked questions</span></span>

1. <span data-ttu-id="d2b22-152">*Ki hello tanúsítványszolgáltató és a használt tanúsítvány típusa?*</span><span class="sxs-lookup"><span data-stu-id="d2b22-152">*Who is hello certificate provider and what type of certificate is used?*</span></span>

    <span data-ttu-id="d2b22-153">Tulajdonos alternatív neve (SAN) tanúsítvány DigiCert által biztosított használjuk.</span><span class="sxs-lookup"><span data-stu-id="d2b22-153">We use Subject Alternative Names (SAN) certificate provided by DigiCert.</span></span> <span data-ttu-id="d2b22-154">SAN-tanúsítványnak biztonságossá teheti az egy tanúsítvánnyal rendelkező több teljesen minősített tartománynév.</span><span class="sxs-lookup"><span data-stu-id="d2b22-154">A SAN certificate can secure multiple fully qualifIed domain names with one certificate.</span></span>

2. <span data-ttu-id="d2b22-155">*A dedikált tanúsítvány is használható?*</span><span class="sxs-lookup"><span data-stu-id="d2b22-155">*Can I use my dedicated certificate?*</span></span>
    
    <span data-ttu-id="d2b22-156">Az on hello terv azonban jelenleg nem.</span><span class="sxs-lookup"><span data-stu-id="d2b22-156">Not currently, but it's on hello roadmap.</span></span>

3. <span data-ttu-id="d2b22-157">*Mi történik, ha a DigiCert hello tartomány megerősítési e-mailt nem érkezik?*</span><span class="sxs-lookup"><span data-stu-id="d2b22-157">*What if I don't receive hello domain verification email from DigiCert?*</span></span>

    <span data-ttu-id="d2b22-158">Lépjen kapcsolatba a Microsofttal, ha 24 órán belül nem kap egy e-mailt.</span><span class="sxs-lookup"><span data-stu-id="d2b22-158">Please contact Microsoft if you don't receive an email within 24 hours.</span></span>

4. <span data-ttu-id="d2b22-159">*Kevésbé biztonságos, mint egy dedikált tanúsítvány SAN tanúsítványt használ?*</span><span class="sxs-lookup"><span data-stu-id="d2b22-159">*Is using a SAN certificate less secure than a dedicated certificate?*</span></span>
    
    <span data-ttu-id="d2b22-160">A következő hello azonos titkosítási biztonsági szabványok gyűjteménye, egy dedikált cert SAN cert. Összes kiállított SSL-tanúsítvány SHA-256 algoritmust használ a fokozott biztonság.</span><span class="sxs-lookup"><span data-stu-id="d2b22-160">A SAN cert follows hello same encryption and security standards as a dedicated cert. All issued SSL certificates are using SHA-256 for enhanced server security.</span></span>

5. <span data-ttu-id="d2b22-161">*Használható-e az egyéni tartomány HTTPS Akamai Azure CDN?*</span><span class="sxs-lookup"><span data-stu-id="d2b22-161">*Can I use custom domain HTTPS with Azure CDN from Akamai?*</span></span>

    <span data-ttu-id="d2b22-162">Ez a funkció jelenleg csak Azure CDN Verizon érhető el.</span><span class="sxs-lookup"><span data-stu-id="d2b22-162">Currently, this feature is only available with Azure CDN from Verizon.</span></span> <span data-ttu-id="d2b22-163">Ez a szolgáltatás Akamai Azure CDN támogató hello elkövetkező hónapokban dolgozunk.</span><span class="sxs-lookup"><span data-stu-id="d2b22-163">We are working on supporting this feature with Azure CDN from Akamai in hello coming months.</span></span>


## <a name="next-steps"></a><span data-ttu-id="d2b22-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d2b22-164">Next steps</span></span>

- <span data-ttu-id="d2b22-165">Megtudhatja, hogyan tooset be egy [Azure CDN-végpont egyéni tartományhoz](./cdn-map-content-to-custom-domain.md)</span><span class="sxs-lookup"><span data-stu-id="d2b22-165">Learn how tooset up a [custom domain on your Azure CDN endpoint](./cdn-map-content-to-custom-domain.md)</span></span>


