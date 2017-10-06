---
title: "Cloud Services egy egyéni tartománynevet aaaConfigure |} Microsoft Docs"
description: "Megtudhatja, hogyan tooexpose az Azure alkalmazás vagy adatok toohello internetes DNS-beállítások konfigurálásával olyan egyéni tartományhoz.  Ezekben a példákban hello Azure-portálon."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 5783a246-a151-4fb1-b488-441bfb29ee44
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: a0f3186b6022fbc4570ef1ce4b921426842bde76
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a><span data-ttu-id="b7ee0-104">Egy egyéni tartománynevet, az Azure-felhőszolgáltatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b7ee0-104">Configuring a custom domain name for an Azure cloud service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b7ee0-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="b7ee0-105">Azure portal</span></span>](cloud-services-custom-domain-name-portal.md)
> * [<span data-ttu-id="b7ee0-106">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="b7ee0-106">Azure classic portal</span></span>](cloud-services-custom-domain-name.md)
> 
> 

<span data-ttu-id="b7ee0-107">Amikor létrehoz egy felhőalapú szolgáltatás, Azure rendeli tooa altartománya **cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-107">When you create a Cloud Service, Azure assigns it tooa subdomain of **cloudapp.net**.</span></span> <span data-ttu-id="b7ee0-108">Például, ha a Felhőszolgáltatás neve "contoso", a felhasználók fog kell tudni tooaccess http://contoso.cloudapp.net URL-az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-108">For example, if your Cloud Service is named "contoso", your users will be able tooaccess your application on a URL like http://contoso.cloudapp.net.</span></span> <span data-ttu-id="b7ee0-109">Azure is hozzárendel egy virtuális IP-címet.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-109">Azure also assigns a virtual IP address.</span></span>

<span data-ttu-id="b7ee0-110">Azonban Ön is is elérhetővé teheti az alkalmazás a saját tartománynevét, például a **contoso.com**. Ez a cikk azt ismerteti, hogyan tooreserve vagy állítson be egy egyéni tartománynevet a felhőalapú szolgáltatás webes szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-110">However, you can also expose your application on your own domain name, such as **contoso.com**. This article explains how tooreserve or configure a custom domain name for Cloud Service web roles.</span></span>

<span data-ttu-id="b7ee0-111">Tegye meg már megismeréséhez CNAME és A rekordok?</span><span class="sxs-lookup"><span data-stu-id="b7ee0-111">Do you already understand what CNAME and A records are?</span></span> <span data-ttu-id="b7ee0-112">[Ugrás túli hello magyarázat](#add-a-cname-record-for-your-custom-domain).</span><span class="sxs-lookup"><span data-stu-id="b7ee0-112">[Jump past hello explanation](#add-a-cname-record-for-your-custom-domain).</span></span>

> [!NOTE]
> <span data-ttu-id="b7ee0-113">Ebben a feladatban hello eljárások tooAzure Felhőszolgáltatások alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-113">hello procedures in this task apply tooAzure Cloud Services.</span></span> <span data-ttu-id="b7ee0-114">Alkalmazásszolgáltatások, lásd: [ez](../app-service-web/web-sites-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="b7ee0-114">For App Services, see [this](../app-service-web/web-sites-custom-domain-name.md).</span></span> <span data-ttu-id="b7ee0-115">Storage-fiókok, lásd: [ez](../storage/blobs/storage-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="b7ee0-115">For storage accounts, see [this](../storage/blobs/storage-custom-domain-name.md).</span></span>
> 
> 

<p/>

> [!TIP]
> <span data-ttu-id="b7ee0-116">Gyorsabb--induláshoz használható hello új Azure [forgatókönyv interaktív](http://support.microsoft.com/kb/2990804)!</span><span class="sxs-lookup"><span data-stu-id="b7ee0-116">Get going faster--use hello NEW Azure [guided walkthrough](http://support.microsoft.com/kb/2990804)!</span></span>  <span data-ttu-id="b7ee0-117">Így a egy egyéni tartománynevet és biztonságossá tétele kommunikációhoz (SSL) társítása Azure Cloud Services vagy az Azure Websitesra egy beépülő.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-117">It makes associating a custom domain name AND securing communication (SSL) with Azure Cloud Services or Azure Websites a snap.</span></span>
> 
> 

## <a name="understand-cname-and-a-records"></a><span data-ttu-id="b7ee0-118">CNAME és az A rekordok ismertetése</span><span class="sxs-lookup"><span data-stu-id="b7ee0-118">Understand CNAME and A records</span></span>
<span data-ttu-id="b7ee0-119">CNAME (vagy alias rekordokat) és A rekordok is lehetővé teszik a tartomány nevét, egy adott kiszolgálóval tooassociate (vagy szolgáltatás ebben az esetben) azonban ezek eltérően működik.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-119">CNAME (or alias records) and A records both allow you tooassociate a domain name with a specific server (or service in this case,) however they work differently.</span></span> <span data-ttu-id="b7ee0-120">Nincsenek is figyelembe kell venni néhány bizonyos Azure Cloud serviceshez, mely toouse dönt előtt figyelembe kell venni A rekordok használatával.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-120">There are also some specific considerations when using A records with Azure Cloud services that you should consider before deciding which toouse.</span></span>

### <a name="cname-or-alias-record"></a><span data-ttu-id="b7ee0-121">CNAME- vagy aliasnév rekord</span><span class="sxs-lookup"><span data-stu-id="b7ee0-121">CNAME or Alias record</span></span>
<span data-ttu-id="b7ee0-122">A CNAME rekord rendeli hozzá egy *adott* tartomány, például a **contoso.com** vagy **www.contoso.com**, tooa kanonikus tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-122">A CNAME record maps a *specific* domain, such as **contoso.com** or **www.contoso.com**, tooa canonical domain name.</span></span> <span data-ttu-id="b7ee0-123">Ebben az esetben hello kanonikus tartománynév hello **[myapp] .cloudapp .net** tartománynevét, az Azure futó alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-123">In this case, hello canonical domain name is hello **[myapp].cloudapp.net** domain name of your Azure hosted application.</span></span> <span data-ttu-id="b7ee0-124">Létrehozása után hello CNAME létrehoz egy aliast hello **[myapp] .cloudapp .net**.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-124">Once created, hello CNAME creates an alias for hello **[myapp].cloudapp.net**.</span></span> <span data-ttu-id="b7ee0-125">hello CNAME bejegyzés megoldja toohello IP-címét a **[myapp] .cloudapp .net** szolgáltatás automatikusan, így ha megváltozik a hello IP-cím hello felhőalapú szolgáltatás, nem kell tootake bármilyen műveletet.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-125">hello CNAME entry will resolve toohello IP address of your **[myapp].cloudapp.net** service automatically, so if hello IP address of hello cloud service changes, you do not have tootake any action.</span></span>

> [!NOTE]
> <span data-ttu-id="b7ee0-126">Néhány tartomány regisztráló szervezetek csak teszi toomap altartományok egy olyan CNAME rekordot, például www.contoso.com és a nem gyökér neve, például contoso.com használatakor. A regisztráló által biztosított hello dokumentációjában talál további információt a CNAME-rekordot, [Wikipedia bejegyzés a CNAME rekord hello](http://en.wikipedia.org/wiki/CNAME_record), vagy hello [IETF tartománynév - megvalósítása és meghatározása](http://tools.ietf.org/html/rfc1035) a dokumentum.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-126">Some domain registrars only allow you toomap subdomains when using a CNAME record, such as www.contoso.com, and not root names, such as contoso.com. For more information on CNAME records, see hello documentation provided by your registrar, [hello Wikipedia entry on CNAME record](http://en.wikipedia.org/wiki/CNAME_record), or hello [IETF Domain Names - Implementation and Specification](http://tools.ietf.org/html/rfc1035) document.</span></span>
> 
> 

### <a name="a-record"></a><span data-ttu-id="b7ee0-127">Egy rekord</span><span class="sxs-lookup"><span data-stu-id="b7ee0-127">A record</span></span>
<span data-ttu-id="b7ee0-128">Egy *A* rekord rendeli hozzá a tartományhoz, például a **contoso.com** vagy **www.contoso.com**, *vagy helyettesítő tartomány* például  **\*. contoso.com**, tooan IP-címet.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-128">An *A* record maps a domain, such as **contoso.com** or **www.contoso.com**, *or a wildcard domain* such as **\*.contoso.com**, tooan IP address.</span></span> <span data-ttu-id="b7ee0-129">Azure Cloud Service hello esetben hello hello szolgáltatás virtuális IP-cím.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-129">In hello case of an Azure Cloud Service, hello virtual IP of hello service.</span></span> <span data-ttu-id="b7ee0-130">Így hello fő előnye, hogy az A rekord keresztül egy olyan CNAME rekordot, akkor használja, mint a helyettesítő karakter, egy vagy több bejegyzése \* **. contoso.com**, amely volna tanúsítványigénylések több altartományok például  **mail.contoso.com**, **login.contoso.com**, vagy **www.contso.com**.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-130">So hello main benefit of an A record over a CNAME record is that you can have one entry that uses a wildcard, such as \***.contoso.com**, which would handle requests for multiple sub-domains such as **mail.contoso.com**, **login.contoso.com**, or **www.contso.com**.</span></span>

> [!NOTE]
> <span data-ttu-id="b7ee0-131">Mivel az A rekord le van képezve tooa statikus IP-cím, nem tudják automatikusan feloldani a felhőalapú szolgáltatás módosítások toohello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-131">Since an A record is mapped tooa static IP address, it cannot automatically resolve changes toohello IP address of your Cloud Service.</span></span> <span data-ttu-id="b7ee0-132">a felhőalapú szolgáltatás által használt hello IP-cím van lefoglalva a hello először telepít tooan üres helyre (éles vagy átmeneti.) Hello tárolóhely hello központi telepítésének törlésekor Azure által kiadott hello IP-címet, és a jövőben a központi telepítésekre toohello tárolóhely új IP-cím is megadható.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-132">hello IP address used by your Cloud Service is allocated hello first time you deploy tooan empty slot (either production or staging.) If you delete hello deployment for hello slot, hello IP address is released by Azure and any future deployments toohello slot may be given a new IP address.</span></span>
> 
> <span data-ttu-id="b7ee0-133">Állandó kényelmesen, egy adott üzembe helyezési pontjának (üzemi és átmeneti) hello IP-címét a csere közötti átmeneti és üzemi környezetek vagy egy meglévő központi telepítési helyben történő frissítése során.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-133">Conveniently, hello IP address of a given deployment slot (production or staging) is persisted when swapping between staging and production deployments or performing an in-place upgrade of an existing deployment.</span></span> <span data-ttu-id="b7ee0-134">Ezek a műveletek végrehajtásával további információkért lásd: [hogyan toomanage felhőalapú szolgáltatások](cloud-services-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="b7ee0-134">For more information on performing these actions, see [How toomanage cloud services](cloud-services-how-to-manage.md).</span></span>
> 
> 

## <a name="add-a-cname-record-for-your-custom-domain"></a><span data-ttu-id="b7ee0-135">Adjon hozzá egy CNAME rekordot, az egyéni tartományhoz</span><span class="sxs-lookup"><span data-stu-id="b7ee0-135">Add a CNAME record for your custom domain</span></span>
<span data-ttu-id="b7ee0-136">toocreate egy olyan CNAME rekordot, hozzá kell adnia egy új bejegyzést hello DNS-tábla az egyéni tartomány a regisztráló által biztosított hello eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-136">toocreate a CNAME record, you must add a new entry in hello DNS table for your custom domain by using hello tools provided by your registrar.</span></span> <span data-ttu-id="b7ee0-137">Minden tartományregisztráló egy CNAME rekordot a telepítésükhöz hasonló, csak metódust tartalmaz, de koncepciók hello hello azonos.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-137">Each registrar has a similar but slightly different method of specifying a CNAME record, but hello concepts are hello same.</span></span>

1. <span data-ttu-id="b7ee0-138">Ezen módszerek toofind hello valamelyikével **. cloudapp.net** hozzárendelt tooyour felhőalapú szolgáltatás, a tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-138">Use one of these methods toofind hello **.cloudapp.net** domain name assigned tooyour cloud service.</span></span>
   
   * <span data-ttu-id="b7ee0-139">Bejelentkezési toohello [Azure-portálon], válassza ki a felhőalapú szolgáltatás, nézze meg hello **Essentials** szakaszt, és keresse meg hello **webhely URL-címe** bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-139">Login toohello [Azure portal], select your cloud service, look at hello **Essentials** section and then find hello **Site URL** entry.</span></span>
     
       ![gyors áttekintő szakasz megjelenítő hello webhely URL-címe][csurl]
     
       <span data-ttu-id="b7ee0-141">**VAGY**</span><span class="sxs-lookup"><span data-stu-id="b7ee0-141">**OR**</span></span>
   * <span data-ttu-id="b7ee0-142">Telepítse és konfigurálja [Azure Powershell](/powershell/azure/overview), és majd a hello használata a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="b7ee0-142">Install and configure [Azure Powershell](/powershell/azure/overview), and then use hello following command:</span></span>
     
       ```powershell
       Get-AzureDeployment -ServiceName yourservicename | Select Url
       ```
     
     <span data-ttu-id="b7ee0-143">Mentse a hello tartománynevet hello URL-címet, vagy a metódus által visszaadott, szüksége lesz rá egy olyan CNAME rekordot létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-143">Save hello domain name used in hello URL returned by either method, as you will need it when creating a CNAME record.</span></span>
2. <span data-ttu-id="b7ee0-144">Jelentkezzen be tooyour DNS regisztráló webhelyén, és lépjen toohello lapra DNS kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-144">Log on tooyour DNS registrar's website and go toohello page for managing DNS.</span></span> <span data-ttu-id="b7ee0-145">Keressen hivatkozásokat vagy hello hely területek címkézve **tartománynév**, **DNS**, vagy **neve kezelő**.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-145">Look for links or areas of hello site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>
3. <span data-ttu-id="b7ee0-146">Most már található, ahol válassza ki vagy adja meg a CNAME meg.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-146">Now find where you can select or enter CNAME's.</span></span> <span data-ttu-id="b7ee0-147">Előfordulhat, hogy egy legördülő menüben írja be, vagy válassza a Speciális beállítások lap tooan tooselect hello rekord.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-147">You may have tooselect hello record type from a drop down, or go tooan advanced settings page.</span></span> <span data-ttu-id="b7ee0-148">Hello szavak kell keresnie **CNAME**, **Alias**, vagy **altartományok**.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-148">You should look for hello words **CNAME**, **Alias**, or **Subdomains**.</span></span>
4. <span data-ttu-id="b7ee0-149">Meg kell adni hello tartomány és altartomány alias a hello CNAME, például a **www** Ha azt szeretné, toocreate alias **www.customdomain.com**. Ha szeretne toocreate alias hello gyökértartomány, akkor elérhetőként hello "**@**" szimbólumot lát, az a regisztráló DNS-eszközök.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-149">You must also provide hello domain or subdomain alias for hello CNAME, such as **www** if you want toocreate an alias for **www.customdomain.com**. If you want toocreate an alias for hello root domain, it may be listed as hello '**@**' symbol in your registrar's DNS tools.</span></span>
5. <span data-ttu-id="b7ee0-150">Ezt követően meg kell adnia egy kanonikus nevet, amely az alkalmazás **cloudapp.net** tartomány ebben az esetben.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-150">Then, you must provide a canonical host name, which is your application's **cloudapp.net** domain in this case.</span></span>

<span data-ttu-id="b7ee0-151">Például a CNAME rekordot a következő hello származó összes forgalmat továbbítja **www.contoso.com** túl**contoso.cloudapp.net**, a telepített alkalmazás hello egyéni tartomány nevét:</span><span class="sxs-lookup"><span data-stu-id="b7ee0-151">For example, hello following CNAME record forwards all traffic from **www.contoso.com** too**contoso.cloudapp.net**, hello custom domain name of your deployed application:</span></span>

| <span data-ttu-id="b7ee0-152">Alias és a gazdagép neve/altartomány</span><span class="sxs-lookup"><span data-stu-id="b7ee0-152">Alias/Host name/Subdomain</span></span> | <span data-ttu-id="b7ee0-153">Canonical tartomány</span><span class="sxs-lookup"><span data-stu-id="b7ee0-153">Canonical domain</span></span> |
| --- | --- |
| <span data-ttu-id="b7ee0-154">www</span><span class="sxs-lookup"><span data-stu-id="b7ee0-154">www</span></span> |<span data-ttu-id="b7ee0-155">contoso.cloudapp.NET</span><span class="sxs-lookup"><span data-stu-id="b7ee0-155">contoso.cloudapp.net</span></span> |

> [!NOTE]
> <span data-ttu-id="b7ee0-156">A látogatói **www.contoso.com** nem veszi észre hello igaz állomás (contoso.cloudapp.net), így folyamat továbbítási hello láthatatlan toothe végfelhasználói.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-156">A visitor of **www.contoso.com** will never see hello true host (contoso.cloudapp.net), so hello forwarding process is invisible toothe end user.</span></span>
> 
> <span data-ttu-id="b7ee0-157">hello példa a fenti csak érvényes tootraffic: hello **www** altartomány.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-157">hello example above only applies tootraffic at hello **www** subdomain.</span></span> <span data-ttu-id="b7ee0-158">Helyettesítő karakterek nem használhatja a CNAME-rekordot, mert minden tartomány/altartomány egy CNAME REKORDOT kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-158">Since you cannot use wildcards with CNAME records, you must create one CNAME for each domain/subdomain.</span></span> <span data-ttu-id="b7ee0-159">Toodirect forgalmát altartományt, például szeretné *. contoso.com, tooyour cloudapp.net cím, konfigurálhat egy **átirányítási URL-cím** vagy **előre URL-cím** bejegyzést a DNS-beállításokat, vagy hozzon létre egy A rekordot.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-159">If you want toodirect  traffic from subdomains, such as *.contoso.com, tooyour cloudapp.net address, you can configure a **URL Redirect** or **URL Forward** entry in your DNS settings, or create an A record.</span></span>
> 
> 

## <a name="add-an-a-record-for-your-custom-domain"></a><span data-ttu-id="b7ee0-160">Az egyéni tartomány az A rekord hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b7ee0-160">Add an A record for your custom domain</span></span>
<span data-ttu-id="b7ee0-161">toocreate egy A rekordot, először keresse meg a felhőalapú szolgáltatás hello virtuális IP-címét.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-161">toocreate an A record, you must first find hello virtual IP address of your cloud service.</span></span> <span data-ttu-id="b7ee0-162">Majd új bejegyzés hozzáadása az egyéni tartomány hello DNS táblázatban a regisztráló által biztosított hello eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-162">Then add a new entry in hello DNS table for your custom domain by using hello tools provided by your registrar.</span></span> <span data-ttu-id="b7ee0-163">Minden tartományregisztráló egy A rekordot telepítésükhöz hasonló, csak metódust tartalmaz, de koncepciók hello hello azonos.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-163">Each registrar has a similar but slightly different method of specifying an A record, but hello concepts are hello same.</span></span>

1. <span data-ttu-id="b7ee0-164">A következő módszerek tooget hello IP-címet a felhőszolgáltatás hello egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-164">Use one of hello following methods tooget hello IP address of your cloud service.</span></span>
   
   * <span data-ttu-id="b7ee0-165">Bejelentkezési toohello [Azure-portálon], válassza ki a felhőalapú szolgáltatás, nézze meg hello **Essentials** szakaszt, és keresse meg hello **nyilvános IP-címek** bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-165">Login toohello [Azure portal], select your cloud service, look at hello **Essentials** section and then find hello **Public IP addresses** entry.</span></span>
     
       ![hello VIP megjelenítő gyors áttekintő szakasz][vip]
     
       <span data-ttu-id="b7ee0-167">**VAGY**</span><span class="sxs-lookup"><span data-stu-id="b7ee0-167">**OR**</span></span>
   * <span data-ttu-id="b7ee0-168">Telepítse és konfigurálja [Azure Powershell](/powershell/azure/overview), és majd a hello használata a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="b7ee0-168">Install and configure [Azure Powershell](/powershell/azure/overview), and then use hello following command:</span></span>
     
       ```powershell
       get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
       ```
     
     <span data-ttu-id="b7ee0-169">Hello IP-cím mentése szüksége lesz rá egy A rekordot létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-169">Save hello IP address, as you will need it when creating an A record.</span></span>
2. <span data-ttu-id="b7ee0-170">Jelentkezzen be tooyour DNS regisztráló webhelyén, és lépjen toohello lapra DNS kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-170">Log on tooyour DNS registrar's website and go toohello page for managing DNS.</span></span> <span data-ttu-id="b7ee0-171">Keressen hivatkozásokat vagy hello hely területek címkézve **tartománynév**, **DNS**, vagy **neve kezelő**.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-171">Look for links or areas of hello site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>
3. <span data-ttu-id="b7ee0-172">Most már található, ahol válasszon vagy adjon meg egy olyan rekordot.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-172">Now find where you can select or enter A record's.</span></span> <span data-ttu-id="b7ee0-173">Előfordulhat, hogy egy legördülő menüben írja be, vagy válassza a Speciális beállítások lap tooan tooselect hello rekord.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-173">You may have tooselect hello record type from a drop down, or go tooan advanced settings page.</span></span>
4. <span data-ttu-id="b7ee0-174">Válassza ki, vagy adja meg a hello tartomány vagy altartományt, amelyet ez A rekord fog használni.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-174">Select or enter hello domain or subdomain that will use this A record.</span></span> <span data-ttu-id="b7ee0-175">Válassza például **www** Ha azt szeretné, toocreate alias **www.customdomain.com**. Ha egy helyettesítő karakteres bejegyzés toocreate altartományokkal szeretne, adjon meg "x".</span><span class="sxs-lookup"><span data-stu-id="b7ee0-175">For example, select **www** if you want toocreate an alias for **www.customdomain.com**. If you want toocreate a wildcard entry for all subdomains, enter '*****'.</span></span> <span data-ttu-id="b7ee0-176">Ez például lefedi altartományokkal **mail.customdomain.com**, **login.customdomain.com**, és **www.customdomain.com**.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-176">This will cover all sub-domains such as **mail.customdomain.com**, **login.customdomain.com**, and **www.customdomain.com**.</span></span>
   
    <span data-ttu-id="b7ee0-177">Hello gyökértartomány toocreate egy A rekordot szeretne, ha azt elérhetőként hello "**@**" szimbólumot lát, az a regisztráló DNS-eszközök.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-177">If you want toocreate an A record for hello root domain, it may be listed as hello '**@**' symbol in your registrar's DNS tools.</span></span>
5. <span data-ttu-id="b7ee0-178">Adja meg a felhőszolgáltatás hello IP-cím mezőben megadott hello.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-178">Enter hello IP address of your cloud service in hello provided field.</span></span> <span data-ttu-id="b7ee0-179">Ez társít hello tartomány bejegyzés hello A rekordban a cloud service-környezet hello IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-179">This associates hello domain entry used in hello A record with hello IP address of your cloud service deployment.</span></span>

<span data-ttu-id="b7ee0-180">Például a rekord a következő hello származó összes forgalmat továbbítja **contoso.com** túl**137.135.70.239**, IP-címét a telepített alkalmazás hello:</span><span class="sxs-lookup"><span data-stu-id="b7ee0-180">For example, hello following A record forwards all traffic from **contoso.com** too**137.135.70.239**, hello IP address of your deployed application:</span></span>

| <span data-ttu-id="b7ee0-181">Gazdagép neve/altartomány</span><span class="sxs-lookup"><span data-stu-id="b7ee0-181">Host name/Subdomain</span></span> | <span data-ttu-id="b7ee0-182">IP-cím</span><span class="sxs-lookup"><span data-stu-id="b7ee0-182">IP address</span></span> |
| --- | --- |
| @ |<span data-ttu-id="b7ee0-183">137.135.70.239</span><span class="sxs-lookup"><span data-stu-id="b7ee0-183">137.135.70.239</span></span> |

<span data-ttu-id="b7ee0-184">Ez a példa azt mutatja be, hello legfelső szintű tartomány az A rekord létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-184">This example demonstrates creating an A record for hello root domain.</span></span> <span data-ttu-id="b7ee0-185">Ha egy helyettesítő karakteres bejegyzés toocover kívánja toocreate altartományokkal, adjon meg "x", hello altartomány.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-185">If you wish toocreate a wildcard entry toocover all subdomains, you would enter '*****' as hello subdomain.</span></span>

> [!WARNING]
> <span data-ttu-id="b7ee0-186">IP-címek az Azure-ban alapértelmezés szerint dinamikus.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-186">IP addresses in Azure are dynamic by default.</span></span> <span data-ttu-id="b7ee0-187">Toouse érdemes egy [fenntartott IP-cím](../virtual-network/virtual-networks-reserved-public-ip.md) tooensure, amely az IP-címe nem változik.</span><span class="sxs-lookup"><span data-stu-id="b7ee0-187">You will probably want toouse a [reserved IP address](../virtual-network/virtual-networks-reserved-public-ip.md) tooensure that your IP address does not change.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="b7ee0-188">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b7ee0-188">Next steps</span></span>
* [<span data-ttu-id="b7ee0-189">TooManage Cloud Services</span><span class="sxs-lookup"><span data-stu-id="b7ee0-189">How tooManage Cloud Services</span></span>](cloud-services-how-to-manage.md)
* [<span data-ttu-id="b7ee0-190">Hogyan tooMap CDN tartalom tooa egyéni tartományhoz</span><span class="sxs-lookup"><span data-stu-id="b7ee0-190">How tooMap CDN Content tooa Custom Domain</span></span>](../cdn/cdn-map-content-to-custom-domain.md)
* <span data-ttu-id="b7ee0-191">[A felhőalapú szolgáltatás általános konfigurációs](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b7ee0-191">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="b7ee0-192">Ismerje meg, hogyan túl[felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b7ee0-192">Learn how too[deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="b7ee0-193">Konfigurálása [ssl-tanúsítványok](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="b7ee0-193">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
[Create a CNAME record that associates hello subdomain with hello storage account]: #create-cname
[Azure-portálon]: https://portal.azure.com
[vip]: ./media/cloud-services-custom-domain-name-portal/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name-portal/csurl.png
