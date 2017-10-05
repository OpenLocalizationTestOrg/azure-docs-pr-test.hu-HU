---
title: "Állítson be egy egyéni tartománynevet a felhőalapú szolgáltatások |} Microsoft Docs"
description: "Megtudhatja, hogyan teszi közzé az Azure alkalmazás vagy adatokat az interneten egyéni tartomány DNS-beállítások konfigurálásával.  Ezekben a példákban az Azure-portálon."
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
ms.openlocfilehash: cf43d86dddc3a68573e1ba1b09118c54f0b16bc5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a><span data-ttu-id="d3843-104">Egy egyéni tartománynevet, az Azure-felhőszolgáltatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="d3843-104">Configuring a custom domain name for an Azure cloud service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d3843-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="d3843-105">Azure portal</span></span>](cloud-services-custom-domain-name-portal.md)
> * [<span data-ttu-id="d3843-106">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="d3843-106">Azure classic portal</span></span>](cloud-services-custom-domain-name.md)
> 
> 

<span data-ttu-id="d3843-107">Amikor létrehoz egy felhőalapú szolgáltatás, Azure rendeli altartománya **cloudapp.net**.</span><span class="sxs-lookup"><span data-stu-id="d3843-107">When you create a Cloud Service, Azure assigns it to a subdomain of **cloudapp.net**.</span></span> <span data-ttu-id="d3843-108">Például ha a Felhőszolgáltatás neve "contoso", a felhasználók tudnak az URL-cím http://contoso.cloudapp.net például az alkalmazás eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="d3843-108">For example, if your Cloud Service is named "contoso", your users will be able to access your application on a URL like http://contoso.cloudapp.net.</span></span> <span data-ttu-id="d3843-109">Azure is hozzárendel egy virtuális IP-címet.</span><span class="sxs-lookup"><span data-stu-id="d3843-109">Azure also assigns a virtual IP address.</span></span>

<span data-ttu-id="d3843-110">Azonban Ön is is elérhetővé teheti az alkalmazás a saját tartománynevét, például a **contoso.com**. Ez a cikk ismerteti, hogyan állítson be egy egyéni tartománynevet a felhőalapú szolgáltatás webes szerepkör, illetve a.</span><span class="sxs-lookup"><span data-stu-id="d3843-110">However, you can also expose your application on your own domain name, such as **contoso.com**. This article explains how to reserve or configure a custom domain name for Cloud Service web roles.</span></span>

<span data-ttu-id="d3843-111">Tegye meg már megismeréséhez CNAME és A rekordok?</span><span class="sxs-lookup"><span data-stu-id="d3843-111">Do you already understand what CNAME and A records are?</span></span> <span data-ttu-id="d3843-112">[Ugrás a magyarázat túli](#add-a-cname-record-for-your-custom-domain).</span><span class="sxs-lookup"><span data-stu-id="d3843-112">[Jump past the explanation](#add-a-cname-record-for-your-custom-domain).</span></span>

> [!NOTE]
> <span data-ttu-id="d3843-113">Ebben a feladatban eljárások Azure Felhőszolgáltatások vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="d3843-113">The procedures in this task apply to Azure Cloud Services.</span></span> <span data-ttu-id="d3843-114">Alkalmazásszolgáltatások, lásd: [ez](../app-service-web/web-sites-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="d3843-114">For App Services, see [this](../app-service-web/web-sites-custom-domain-name.md).</span></span> <span data-ttu-id="d3843-115">Storage-fiókok, lásd: [ez](../storage/blobs/storage-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="d3843-115">For storage accounts, see [this](../storage/blobs/storage-custom-domain-name.md).</span></span>
> 
> 

<p/>

> [!TIP]
> <span data-ttu-id="d3843-116">Gyorsabb--induláshoz használja az új Azure [forgatókönyv interaktív](http://support.microsoft.com/kb/2990804)!</span><span class="sxs-lookup"><span data-stu-id="d3843-116">Get going faster--use the NEW Azure [guided walkthrough](http://support.microsoft.com/kb/2990804)!</span></span>  <span data-ttu-id="d3843-117">Így a egy egyéni tartománynevet és biztonságossá tétele kommunikációhoz (SSL) társítása Azure Cloud Services vagy az Azure Websitesra egy beépülő.</span><span class="sxs-lookup"><span data-stu-id="d3843-117">It makes associating a custom domain name AND securing communication (SSL) with Azure Cloud Services or Azure Websites a snap.</span></span>
> 
> 

## <a name="understand-cname-and-a-records"></a><span data-ttu-id="d3843-118">CNAME és az A rekordok ismertetése</span><span class="sxs-lookup"><span data-stu-id="d3843-118">Understand CNAME and A records</span></span>
<span data-ttu-id="d3843-119">CNAME (vagy alias rekordokat) és a rekordok mindkét lehetővé teszi tartománynév társítani a kiszolgálóhoz (vagy szolgáltatás ebben az esetben) azonban ezek eltérően működik.</span><span class="sxs-lookup"><span data-stu-id="d3843-119">CNAME (or alias records) and A records both allow you to associate a domain name with a specific server (or service in this case,) however they work differently.</span></span> <span data-ttu-id="d3843-120">Nincsenek is figyelembe kell venni néhány bizonyos Azure felhőszolgáltatások is dönt használandó előtt figyelembe kell vennie A rekordok használatával.</span><span class="sxs-lookup"><span data-stu-id="d3843-120">There are also some specific considerations when using A records with Azure Cloud services that you should consider before deciding which to use.</span></span>

### <a name="cname-or-alias-record"></a><span data-ttu-id="d3843-121">CNAME- vagy aliasnév rekord</span><span class="sxs-lookup"><span data-stu-id="d3843-121">CNAME or Alias record</span></span>
<span data-ttu-id="d3843-122">A CNAME rekord rendeli hozzá a *adott* tartomány, például **contoso.com** vagy **www.contoso.com**, kanonikus tartománynévvel.</span><span class="sxs-lookup"><span data-stu-id="d3843-122">A CNAME record maps a *specific* domain, such as **contoso.com** or **www.contoso.com**, to a canonical domain name.</span></span> <span data-ttu-id="d3843-123">A kanonikus tartománynév van ebben az esetben a **[myapp] .cloudapp .net** tartománynevét, az Azure futó alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="d3843-123">In this case, the canonical domain name is the **[myapp].cloudapp.net** domain name of your Azure hosted application.</span></span> <span data-ttu-id="d3843-124">Létrehozása után a CNAME REKORDOT hoz létre egy aliast a **[myapp] .cloudapp .net**.</span><span class="sxs-lookup"><span data-stu-id="d3843-124">Once created, the CNAME creates an alias for the **[myapp].cloudapp.net**.</span></span> <span data-ttu-id="d3843-125">A CNAME-bejegyzés feloldja az IP-címére a **[myapp] .cloudapp .net** szolgáltatás automatikusan, így ha megváltozik az IP-címet a felhőszolgáltatás, nincs teendője.</span><span class="sxs-lookup"><span data-stu-id="d3843-125">The CNAME entry will resolve to the IP address of your **[myapp].cloudapp.net** service automatically, so if the IP address of the cloud service changes, you do not have to take any action.</span></span>

> [!NOTE]
> <span data-ttu-id="d3843-126">Néhány tartomány regisztráló szervezetek altartományok csatlakoztatható egy olyan CNAME rekordot, például www.contoso.com és a nem gyökér neve, például contoso.com használata esetén csak engedélyezése. További információ a CNAME-rekordot, a regisztráló által dokumentációjában talál [CNAME rekordot a Wikipedia bevitelének](http://en.wikipedia.org/wiki/CNAME_record), vagy a [IETF tartománynév - megvalósítása és meghatározása](http://tools.ietf.org/html/rfc1035) dokumentum.</span><span class="sxs-lookup"><span data-stu-id="d3843-126">Some domain registrars only allow you to map subdomains when using a CNAME record, such as www.contoso.com, and not root names, such as contoso.com. For more information on CNAME records, see the documentation provided by your registrar, [the Wikipedia entry on CNAME record](http://en.wikipedia.org/wiki/CNAME_record), or the [IETF Domain Names - Implementation and Specification](http://tools.ietf.org/html/rfc1035) document.</span></span>
> 
> 

### <a name="a-record"></a><span data-ttu-id="d3843-127">Egy rekord</span><span class="sxs-lookup"><span data-stu-id="d3843-127">A record</span></span>
<span data-ttu-id="d3843-128">Egy *A* rekord rendeli hozzá a tartományhoz, például a **contoso.com** vagy **www.contoso.com**, *vagy helyettesítő tartomány* például  **\*. contoso.com**, egy IP-címre.</span><span class="sxs-lookup"><span data-stu-id="d3843-128">An *A* record maps a domain, such as **contoso.com** or **www.contoso.com**, *or a wildcard domain* such as **\*.contoso.com**, to an IP address.</span></span> <span data-ttu-id="d3843-129">Ha egy Azure Cloud Service, a virtuális IP-címe a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="d3843-129">In the case of an Azure Cloud Service, the virtual IP of the service.</span></span> <span data-ttu-id="d3843-130">Így a fő előnye, hogy az A rekord keresztül egy olyan CNAME rekordot, akkor használja, mint a helyettesítő karakter, egy vagy több bejegyzése \* **. contoso.com**, amely volna tanúsítványigénylések több altartományok például  **mail.contoso.com**, **login.contoso.com**, vagy **www.contso.com**.</span><span class="sxs-lookup"><span data-stu-id="d3843-130">So the main benefit of an A record over a CNAME record is that you can have one entry that uses a wildcard, such as \***.contoso.com**, which would handle requests for multiple sub-domains such as **mail.contoso.com**, **login.contoso.com**, or **www.contso.com**.</span></span>

> [!NOTE]
> <span data-ttu-id="d3843-131">Mivel az A rekord egy statikus IP-cím van leképezve, automatikusan nem képes feloldani módosítások a felhőalapú szolgáltatás IP-címre.</span><span class="sxs-lookup"><span data-stu-id="d3843-131">Since an A record is mapped to a static IP address, it cannot automatically resolve changes to the IP address of your Cloud Service.</span></span> <span data-ttu-id="d3843-132">A a felhőalapú szolgáltatás által használt IP-cím van lefoglalva az első alkalommal telepít egy üres helyre (éles vagy átmeneti.) Ha törli a tárhely központi telepítését, az IP-cím Azure által kiadott, és a jövőbeli telepítések a kivett új IP-cím is megadható.</span><span class="sxs-lookup"><span data-stu-id="d3843-132">The IP address used by your Cloud Service is allocated the first time you deploy to an empty slot (either production or staging.) If you delete the deployment for the slot, the IP address is released by Azure and any future deployments to the slot may be given a new IP address.</span></span>
> 
> <span data-ttu-id="d3843-133">Állandó kényelmesen, egy adott üzembe helyezési pontjának (üzemi és átmeneti) IP-címét a csere közötti átmeneti és üzemi környezetek vagy egy meglévő központi telepítési helyben történő frissítése során.</span><span class="sxs-lookup"><span data-stu-id="d3843-133">Conveniently, the IP address of a given deployment slot (production or staging) is persisted when swapping between staging and production deployments or performing an in-place upgrade of an existing deployment.</span></span> <span data-ttu-id="d3843-134">Ezek a műveletek végrehajtásával további információkért lásd: [felhőszolgáltatások kezelése](cloud-services-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="d3843-134">For more information on performing these actions, see [How to manage cloud services](cloud-services-how-to-manage.md).</span></span>
> 
> 

## <a name="add-a-cname-record-for-your-custom-domain"></a><span data-ttu-id="d3843-135">Adjon hozzá egy CNAME rekordot, az egyéni tartományhoz</span><span class="sxs-lookup"><span data-stu-id="d3843-135">Add a CNAME record for your custom domain</span></span>
<span data-ttu-id="d3843-136">A CNAME rekordot kell létrehozni, hozzá kell adnia egy új bejegyzést a DNS-tábla az egyéni tartomány a regisztráló által biztosított eszközök segítségével.</span><span class="sxs-lookup"><span data-stu-id="d3843-136">To create a CNAME record, you must add a new entry in the DNS table for your custom domain by using the tools provided by your registrar.</span></span> <span data-ttu-id="d3843-137">Minden tartományregisztráló egy CNAME rekordot a telepítésükhöz hasonló, csak metódust tartalmaz, de a koncepciók.</span><span class="sxs-lookup"><span data-stu-id="d3843-137">Each registrar has a similar but slightly different method of specifying a CNAME record, but the concepts are the same.</span></span>

1. <span data-ttu-id="d3843-138">Ezen módszerek egyikét kereséséhez használja a **. cloudapp.net** tartománynevet a felhőalapú szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="d3843-138">Use one of these methods to find the **.cloudapp.net** domain name assigned to your cloud service.</span></span>
   
   * <span data-ttu-id="d3843-139">Jelentkezzen be a [Azure-portálon], válassza ki a felhőalapú szolgáltatás, tekintse meg a **Essentials** szakaszt, és keresse meg a **webhely URL-címe** bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="d3843-139">Login to the [Azure portal], select your cloud service, look at the **Essentials** section and then find the **Site URL** entry.</span></span>
     
       ![Megjeleníti a hely URL-cím gyors áttekintő szakasz][csurl]
     
       <span data-ttu-id="d3843-141">**VAGY**</span><span class="sxs-lookup"><span data-stu-id="d3843-141">**OR**</span></span>
   * <span data-ttu-id="d3843-142">Telepítse és konfigurálja [Azure Powershell](/powershell/azure/overview), és a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="d3843-142">Install and configure [Azure Powershell](/powershell/azure/overview), and then use the following command:</span></span>
     
       ```powershell
       Get-AzureDeployment -ServiceName yourservicename | Select Url
       ```
     
     <span data-ttu-id="d3843-143">Mentse a tartomány nevét, szüksége lesz rá egy CNAME rekordot a létrehozásakor vagy metódus által visszaadott URL-címét használják.</span><span class="sxs-lookup"><span data-stu-id="d3843-143">Save the domain name used in the URL returned by either method, as you will need it when creating a CNAME record.</span></span>
2. <span data-ttu-id="d3843-144">Jelentkezzen be a DNS-regisztráló webhelyén, és a DNS kezelése lapon.</span><span class="sxs-lookup"><span data-stu-id="d3843-144">Log on to your DNS registrar's website and go to the page for managing DNS.</span></span> <span data-ttu-id="d3843-145">Keressen hivatkozásokat vagy a hely címkézve területéhez **tartománynév**, **DNS**, vagy **neve kezelő**.</span><span class="sxs-lookup"><span data-stu-id="d3843-145">Look for links or areas of the site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>
3. <span data-ttu-id="d3843-146">Most már található, ahol válassza ki vagy adja meg a CNAME meg.</span><span class="sxs-lookup"><span data-stu-id="d3843-146">Now find where you can select or enter CNAME's.</span></span> <span data-ttu-id="d3843-147">Előfordulhat, hogy a rekord típusát a legördülő menüből válassza le, vagy egy speciális beállítások lapot.</span><span class="sxs-lookup"><span data-stu-id="d3843-147">You may have to select the record type from a drop down, or go to an advanced settings page.</span></span> <span data-ttu-id="d3843-148">A szavakat kell keresnie **CNAME**, **Alias**, vagy **altartományok**.</span><span class="sxs-lookup"><span data-stu-id="d3843-148">You should look for the words **CNAME**, **Alias**, or **Subdomains**.</span></span>
4. <span data-ttu-id="d3843-149">Meg kell adni a tartomány és altartomány alias a CNAME, például a **www** Ha létre szeretné **www.customdomain.com**. Ha azt szeretné, hozzon létre egy aliast a legfelső szintű tartomány, akkor elérhetőként a "**@**" szimbólumot lát, az a regisztráló DNS-eszközök.</span><span class="sxs-lookup"><span data-stu-id="d3843-149">You must also provide the domain or subdomain alias for the CNAME, such as **www** if you want to create an alias for **www.customdomain.com**. If you want to create an alias for the root domain, it may be listed as the '**@**' symbol in your registrar's DNS tools.</span></span>
5. <span data-ttu-id="d3843-150">Ezt követően meg kell adnia egy kanonikus nevet, amely az alkalmazás **cloudapp.net** tartomány ebben az esetben.</span><span class="sxs-lookup"><span data-stu-id="d3843-150">Then, you must provide a canonical host name, which is your application's **cloudapp.net** domain in this case.</span></span>

<span data-ttu-id="d3843-151">Például a következő CNAME-rekordot továbbítja származó összes forgalmat **www.contoso.com** való **contoso.cloudapp.net**, az egyéni tartománynév a központilag telepített alkalmazás:</span><span class="sxs-lookup"><span data-stu-id="d3843-151">For example, the following CNAME record forwards all traffic from **www.contoso.com** to **contoso.cloudapp.net**, the custom domain name of your deployed application:</span></span>

| <span data-ttu-id="d3843-152">Alias és a gazdagép neve/altartomány</span><span class="sxs-lookup"><span data-stu-id="d3843-152">Alias/Host name/Subdomain</span></span> | <span data-ttu-id="d3843-153">Canonical tartomány</span><span class="sxs-lookup"><span data-stu-id="d3843-153">Canonical domain</span></span> |
| --- | --- |
| <span data-ttu-id="d3843-154">www</span><span class="sxs-lookup"><span data-stu-id="d3843-154">www</span></span> |<span data-ttu-id="d3843-155">contoso.cloudapp.NET</span><span class="sxs-lookup"><span data-stu-id="d3843-155">contoso.cloudapp.net</span></span> |

> [!NOTE]
> <span data-ttu-id="d3843-156">A látogatói **www.contoso.com** nem érhető el a valódi állomás (contoso.cloudapp.net), így a továbbítási folyamatban a felhasználók nem látják.</span><span class="sxs-lookup"><span data-stu-id="d3843-156">A visitor of **www.contoso.com** will never see the true host (contoso.cloudapp.net), so the forwarding process is invisible to the end user.</span></span>
> 
> <span data-ttu-id="d3843-157">A fenti példa csak vonatkozik-forgalom zárása a **www** altartomány.</span><span class="sxs-lookup"><span data-stu-id="d3843-157">The example above only applies to traffic at the **www** subdomain.</span></span> <span data-ttu-id="d3843-158">Helyettesítő karakterek nem használhatja a CNAME-rekordot, mert minden tartomány/altartomány egy CNAME REKORDOT kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="d3843-158">Since you cannot use wildcards with CNAME records, you must create one CNAME for each domain/subdomain.</span></span> <span data-ttu-id="d3843-159">Ha szeretné-e a forgalom altartományt, például a közvetlen *. contoso.com, a cloudapp.net címet is konfigurálhat egy **átirányítási URL-cím** vagy **URL-cím előre** bejegyzést a DNS-beállításokat, vagy hozzon létre egy A rekordot.</span><span class="sxs-lookup"><span data-stu-id="d3843-159">If you want to direct  traffic from subdomains, such as *.contoso.com, to your cloudapp.net address, you can configure a **URL Redirect** or **URL Forward** entry in your DNS settings, or create an A record.</span></span>
> 
> 

## <a name="add-an-a-record-for-your-custom-domain"></a><span data-ttu-id="d3843-160">Az egyéni tartomány az A rekord hozzáadása</span><span class="sxs-lookup"><span data-stu-id="d3843-160">Add an A record for your custom domain</span></span>
<span data-ttu-id="d3843-161">Az A rekord létrehozásához először keresse meg a virtuális IP-cím a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d3843-161">To create an A record, you must first find the virtual IP address of your cloud service.</span></span> <span data-ttu-id="d3843-162">Majd adjon hozzá egy új bejegyzést a DNS-tábla az egyéni tartomány a regisztráló által biztosított eszközök segítségével.</span><span class="sxs-lookup"><span data-stu-id="d3843-162">Then add a new entry in the DNS table for your custom domain by using the tools provided by your registrar.</span></span> <span data-ttu-id="d3843-163">Minden tartományregisztráló egy A rekordot telepítésükhöz hasonló, csak metódust tartalmaz, de a koncepciók.</span><span class="sxs-lookup"><span data-stu-id="d3843-163">Each registrar has a similar but slightly different method of specifying an A record, but the concepts are the same.</span></span>

1. <span data-ttu-id="d3843-164">Az alábbi módszerek valamelyikével tud az IP-címet a felhőalapú szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="d3843-164">Use one of the following methods to get the IP address of your cloud service.</span></span>
   
   * <span data-ttu-id="d3843-165">Jelentkezzen be a [Azure-portálon], válassza ki a felhőalapú szolgáltatás, tekintse meg a **Essentials** szakaszt, és keresse meg a **nyilvános IP-címek** bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="d3843-165">Login to the [Azure portal], select your cloud service, look at the **Essentials** section and then find the **Public IP addresses** entry.</span></span>
     
       ![a VIP megjelenítő gyors áttekintő szakasz][vip]
     
       <span data-ttu-id="d3843-167">**VAGY**</span><span class="sxs-lookup"><span data-stu-id="d3843-167">**OR**</span></span>
   * <span data-ttu-id="d3843-168">Telepítse és konfigurálja [Azure Powershell](/powershell/azure/overview), és a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="d3843-168">Install and configure [Azure Powershell](/powershell/azure/overview), and then use the following command:</span></span>
     
       ```powershell
       get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
       ```
     
     <span data-ttu-id="d3843-169">Az IP-cím mentése szüksége lesz rá egy A rekordot létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="d3843-169">Save the IP address, as you will need it when creating an A record.</span></span>
2. <span data-ttu-id="d3843-170">Jelentkezzen be a DNS-regisztráló webhelyén, és a DNS kezelése lapon.</span><span class="sxs-lookup"><span data-stu-id="d3843-170">Log on to your DNS registrar's website and go to the page for managing DNS.</span></span> <span data-ttu-id="d3843-171">Keressen hivatkozásokat vagy a hely címkézve területéhez **tartománynév**, **DNS**, vagy **neve kezelő**.</span><span class="sxs-lookup"><span data-stu-id="d3843-171">Look for links or areas of the site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>
3. <span data-ttu-id="d3843-172">Most már található, ahol válasszon vagy adjon meg egy olyan rekordot.</span><span class="sxs-lookup"><span data-stu-id="d3843-172">Now find where you can select or enter A record's.</span></span> <span data-ttu-id="d3843-173">Előfordulhat, hogy a rekord típusát a legördülő menüből válassza le, vagy egy speciális beállítások lapot.</span><span class="sxs-lookup"><span data-stu-id="d3843-173">You may have to select the record type from a drop down, or go to an advanced settings page.</span></span>
4. <span data-ttu-id="d3843-174">Válassza ki vagy írja be a tartomány vagy altartományt, amelyet ez A rekord fog használni.</span><span class="sxs-lookup"><span data-stu-id="d3843-174">Select or enter the domain or subdomain that will use this A record.</span></span> <span data-ttu-id="d3843-175">Válassza például **www** Ha létre szeretné **www.customdomain.com**. Ha szeretne létrehozni egy helyettesítő karakteres bejegyzést altartományokkal, adja meg az "x".</span><span class="sxs-lookup"><span data-stu-id="d3843-175">For example, select **www** if you want to create an alias for **www.customdomain.com**. If you want to create a wildcard entry for all subdomains, enter '*****'.</span></span> <span data-ttu-id="d3843-176">Ez például lefedi altartományokkal **mail.customdomain.com**, **login.customdomain.com**, és **www.customdomain.com**.</span><span class="sxs-lookup"><span data-stu-id="d3843-176">This will cover all sub-domains such as **mail.customdomain.com**, **login.customdomain.com**, and **www.customdomain.com**.</span></span>
   
    <span data-ttu-id="d3843-177">Ha szeretne létrehozni egy A rekordot, a legfelső szintű tartomány, akkor elérhetőként a "**@**" szimbólumot lát, az a regisztráló DNS-eszközök.</span><span class="sxs-lookup"><span data-stu-id="d3843-177">If you want to create an A record for the root domain, it may be listed as the '**@**' symbol in your registrar's DNS tools.</span></span>
5. <span data-ttu-id="d3843-178">A megadott mezőben adja meg a felhőalapú szolgáltatás IP-címét.</span><span class="sxs-lookup"><span data-stu-id="d3843-178">Enter the IP address of your cloud service in the provided field.</span></span> <span data-ttu-id="d3843-179">Ez társít a tartomány bejegyzést, az A rekord a cloud service-környezet az IP-cím szerepel.</span><span class="sxs-lookup"><span data-stu-id="d3843-179">This associates the domain entry used in the A record with the IP address of your cloud service deployment.</span></span>

<span data-ttu-id="d3843-180">Például a következő rekord származó összes forgalmat továbbítja **contoso.com** való **137.135.70.239**, a telepített alkalmazás IP-címe:</span><span class="sxs-lookup"><span data-stu-id="d3843-180">For example, the following A record forwards all traffic from **contoso.com** to **137.135.70.239**, the IP address of your deployed application:</span></span>

| <span data-ttu-id="d3843-181">Gazdagép neve/altartomány</span><span class="sxs-lookup"><span data-stu-id="d3843-181">Host name/Subdomain</span></span> | <span data-ttu-id="d3843-182">IP-cím</span><span class="sxs-lookup"><span data-stu-id="d3843-182">IP address</span></span> |
| --- | --- |
| @ |<span data-ttu-id="d3843-183">137.135.70.239</span><span class="sxs-lookup"><span data-stu-id="d3843-183">137.135.70.239</span></span> |

<span data-ttu-id="d3843-184">Ez a példa azt mutatja be, a legfelső szintű tartomány az A rekord létrehozása.</span><span class="sxs-lookup"><span data-stu-id="d3843-184">This example demonstrates creating an A record for the root domain.</span></span> <span data-ttu-id="d3843-185">Ha létre szeretne hozni egy helyettesítő karakteres bejegyzés fedik le az összes altartomány, "x", az altartomány.</span><span class="sxs-lookup"><span data-stu-id="d3843-185">If you wish to create a wildcard entry to cover all subdomains, you would enter '*****' as the subdomain.</span></span>

> [!WARNING]
> <span data-ttu-id="d3843-186">IP-címek az Azure-ban alapértelmezés szerint dinamikus.</span><span class="sxs-lookup"><span data-stu-id="d3843-186">IP addresses in Azure are dynamic by default.</span></span> <span data-ttu-id="d3843-187">Valószínűleg érdemes használni a [fenntartott IP-cím](../virtual-network/virtual-networks-reserved-public-ip.md) annak érdekében, hogy az IP-címe nem változik.</span><span class="sxs-lookup"><span data-stu-id="d3843-187">You will probably want to use a [reserved IP address](../virtual-network/virtual-networks-reserved-public-ip.md) to ensure that your IP address does not change.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="d3843-188">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d3843-188">Next steps</span></span>
* [<span data-ttu-id="d3843-189">A Cloud Services felügyelete</span><span class="sxs-lookup"><span data-stu-id="d3843-189">How to Manage Cloud Services</span></span>](cloud-services-how-to-manage.md)
* [<span data-ttu-id="d3843-190">CDN-tartalom leképezése egyéni tartományra</span><span class="sxs-lookup"><span data-stu-id="d3843-190">How to Map CDN Content to a Custom Domain</span></span>](../cdn/cdn-map-content-to-custom-domain.md)
* <span data-ttu-id="d3843-191">[A felhőalapú szolgáltatás általános konfigurációs](cloud-services-how-to-configure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d3843-191">[General configuration of your cloud service](cloud-services-how-to-configure-portal.md).</span></span>
* <span data-ttu-id="d3843-192">Megtudhatja, hogyan [felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d3843-192">Learn how to [deploy a cloud service](cloud-services-how-to-create-deploy-portal.md).</span></span>
* <span data-ttu-id="d3843-193">Konfigurálása [ssl-tanúsítványok](cloud-services-configure-ssl-certificate-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d3843-193">Configure [ssl certificates](cloud-services-configure-ssl-certificate-portal.md).</span></span>

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: cloud-services-how-to-manage-portal.md#how-to-swap-deployments-to-promote-a-staged-deployment-to-production
[Create a CNAME record that associates the subdomain with the storage account]: #create-cname
[Azure-portálon]: https://portal.azure.com
[vip]: ./media/cloud-services-custom-domain-name-portal/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name-portal/csurl.png
