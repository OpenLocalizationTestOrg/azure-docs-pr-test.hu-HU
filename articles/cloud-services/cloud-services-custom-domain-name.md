---
title: "Cloud Services egy egyéni tartománynevet aaaConfigure |} Microsoft Docs"
description: "Megtudhatja, hogyan tooexpose az Azure alkalmazás vagy egy egyéni tartomány DNS-beállítások konfigurálásával meg adatok."
services: cloud-services
documentationcenter: .net
author: Thraka
manager: timlt
editor: 
ms.assetid: 6a62c2b7-ea47-4cce-9d6a-0cca38357f42
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: adegeo
ms.openlocfilehash: 71e553a73b40a8d0512b4d40173500561841772c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configuring-a-custom-domain-name-for-an-azure-cloud-service"></a><span data-ttu-id="0aecb-103">Egy egyéni tartománynevet, az Azure-felhőszolgáltatás konfigurálása</span><span class="sxs-lookup"><span data-stu-id="0aecb-103">Configuring a custom domain name for an Azure cloud service</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="0aecb-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="0aecb-104">Azure portal</span></span>](cloud-services-custom-domain-name-portal.md)
> * [<span data-ttu-id="0aecb-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="0aecb-105">Azure classic portal</span></span>](cloud-services-custom-domain-name.md)
> 
> 

<span data-ttu-id="0aecb-106">Ha létrehoz egy felhőalapú szolgáltatás, Azure hozzárendel cloudapp.net tooa altartománya.</span><span class="sxs-lookup"><span data-stu-id="0aecb-106">When you create a Cloud Service, Azure assigns it tooa subdomain of cloudapp.net.</span></span> <span data-ttu-id="0aecb-107">Például, ha a Felhőszolgáltatás neve "contoso", a felhasználók fog kell tudni tooaccess http://contoso.cloudapp.net URL-az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="0aecb-107">For example, if your Cloud Service is named "contoso", your users will be able tooaccess your application on a URL like http://contoso.cloudapp.net.</span></span> <span data-ttu-id="0aecb-108">Azure is hozzárendel egy virtuális IP-címet.</span><span class="sxs-lookup"><span data-stu-id="0aecb-108">Azure also assigns a virtual IP address.</span></span>

<span data-ttu-id="0aecb-109">Azonban Ön is is elérhetővé teheti az alkalmazás a saját tartománynevét, például contoso.com. Ez a cikk azt ismerteti, hogyan tooreserve vagy állítson be egy egyéni tartománynevet a felhőalapú szolgáltatás webes szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="0aecb-109">However, you can also expose your application on your own domain name, such as contoso.com. This article explains how tooreserve or configure a custom domain name for Cloud Service web roles.</span></span>

<span data-ttu-id="0aecb-110">Tegye meg már megismeréséhez CNAME és A rekordok?</span><span class="sxs-lookup"><span data-stu-id="0aecb-110">Do you already understand what CNAME and A records are?</span></span> <span data-ttu-id="0aecb-111">[Ugrás túli hello magyarázat](#add-a-cname-record-for-your-custom-domain).</span><span class="sxs-lookup"><span data-stu-id="0aecb-111">[Jump past hello explanation](#add-a-cname-record-for-your-custom-domain).</span></span>

> [!NOTE]
> <span data-ttu-id="0aecb-112">Első gyorsabban fog!</span><span class="sxs-lookup"><span data-stu-id="0aecb-112">Get going faster!</span></span> <span data-ttu-id="0aecb-113">Használjon hello Azure [forgatókönyv interaktív](http://support.microsoft.com/kb/2990804).</span><span class="sxs-lookup"><span data-stu-id="0aecb-113">Use hello Azure [guided walkthrough](http://support.microsoft.com/kb/2990804).</span></span> <span data-ttu-id="0aecb-114">Így társítása egy egyéni tartománynevet és egy beépülő kommunikáció (SSL) Azure Cloud Services vagy az Azure-webhelyek biztonságossá tételéhez.</span><span class="sxs-lookup"><span data-stu-id="0aecb-114">It makes associating a custom domain name and securing communication (SSL) with Azure Cloud Services or Azure Websites a snap.</span></span>
> 
> 

<p/>

> [!NOTE]
> <span data-ttu-id="0aecb-115">Ebben a feladatban hello eljárások tooAzure Felhőszolgáltatások alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="0aecb-115">hello procedures in this task apply tooAzure Cloud Services.</span></span> <span data-ttu-id="0aecb-116">Alkalmazásszolgáltatások, lásd: [ez](../app-service-web/web-sites-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="0aecb-116">For App Services, see [this](../app-service-web/web-sites-custom-domain-name.md).</span></span> <span data-ttu-id="0aecb-117">Storage-fiókok, lásd: [ez](../storage/blobs/storage-custom-domain-name.md).</span><span class="sxs-lookup"><span data-stu-id="0aecb-117">For storage accounts, see [this](../storage/blobs/storage-custom-domain-name.md).</span></span>
> 
> 

## <a name="understand-cname-and-a-records"></a><span data-ttu-id="0aecb-118">CNAME és az A rekordok ismertetése</span><span class="sxs-lookup"><span data-stu-id="0aecb-118">Understand CNAME and A records</span></span>
<span data-ttu-id="0aecb-119">CNAME (vagy alias rekordokat) és A rekordok is lehetővé teszik a tartomány nevét, egy adott kiszolgálóval tooassociate (vagy szolgáltatás ebben az esetben) azonban ezek eltérően működik.</span><span class="sxs-lookup"><span data-stu-id="0aecb-119">CNAME (or alias records) and A records both allow you tooassociate a domain name with a specific server (or service in this case,) however they work differently.</span></span> <span data-ttu-id="0aecb-120">Nincsenek is figyelembe kell venni néhány bizonyos Azure Cloud serviceshez, mely toouse dönt előtt figyelembe kell venni A rekordok használatával.</span><span class="sxs-lookup"><span data-stu-id="0aecb-120">There are also some specific considerations when using A records with Azure Cloud services that you should consider before deciding which toouse.</span></span>

### <a name="cname-or-alias-record"></a><span data-ttu-id="0aecb-121">CNAME- vagy aliasnév rekord</span><span class="sxs-lookup"><span data-stu-id="0aecb-121">CNAME or Alias record</span></span>
<span data-ttu-id="0aecb-122">A CNAME rekord rendeli hozzá egy *adott* tartomány, például a **contoso.com** vagy **www.contoso.com**, tooa kanonikus tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="0aecb-122">A CNAME record maps a *specific* domain, such as **contoso.com** or **www.contoso.com**, tooa canonical domain name.</span></span> <span data-ttu-id="0aecb-123">Ebben az esetben hello kanonikus tartománynév hello **[myapp] .cloudapp .net** tartománynevét, az Azure futó alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="0aecb-123">In this case, hello canonical domain name is hello **[myapp].cloudapp.net** domain name of your Azure hosted application.</span></span> <span data-ttu-id="0aecb-124">Létrehozása után hello CNAME létrehoz egy aliast hello **[myapp] .cloudapp .net**.</span><span class="sxs-lookup"><span data-stu-id="0aecb-124">Once created, hello CNAME creates an alias for hello **[myapp].cloudapp.net**.</span></span> <span data-ttu-id="0aecb-125">hello CNAME bejegyzés megoldja toohello IP-címét a **[myapp] .cloudapp .net** szolgáltatás automatikusan, így ha megváltozik a hello IP-cím hello felhőalapú szolgáltatás, nem kell tootake bármilyen műveletet.</span><span class="sxs-lookup"><span data-stu-id="0aecb-125">hello CNAME entry will resolve toohello IP address of your **[myapp].cloudapp.net** service automatically, so if hello IP address of hello cloud service changes, you do not have tootake any action.</span></span>

> [!NOTE]
> <span data-ttu-id="0aecb-126">Néhány tartomány regisztráló szervezetek csak teszi toomap altartományok egy olyan CNAME rekordot, például www.contoso.com és a nem gyökér neve, például contoso.com használatakor. A regisztráló által biztosított hello dokumentációjában talál további információt a CNAME-rekordot, [Wikipedia bejegyzés a CNAME rekord hello](http://en.wikipedia.org/wiki/CNAME_record), vagy hello [IETF tartománynév - megvalósítása és meghatározása](http://tools.ietf.org/html/rfc1035) a dokumentum.</span><span class="sxs-lookup"><span data-stu-id="0aecb-126">Some domain registrars only allow you toomap subdomains when using a CNAME record, such as www.contoso.com, and not root names, such as contoso.com. For more information on CNAME records, see hello documentation provided by your registrar, [hello Wikipedia entry on CNAME record](http://en.wikipedia.org/wiki/CNAME_record), or hello [IETF Domain Names - Implementation and Specification](http://tools.ietf.org/html/rfc1035) document.</span></span>
> 
> 

### <a name="a-record"></a><span data-ttu-id="0aecb-127">Egy rekord</span><span class="sxs-lookup"><span data-stu-id="0aecb-127">A record</span></span>
<span data-ttu-id="0aecb-128">Az A rekord rendeli hozzá egy tartományhoz, például a **contoso.com** vagy **www.contoso.com**, *vagy helyettesítő tartomány* például  **\*. contoso.com**, tooan IP-címet.</span><span class="sxs-lookup"><span data-stu-id="0aecb-128">An A record maps a domain, such as **contoso.com** or **www.contoso.com**, *or a wildcard domain* such as **\*.contoso.com**, tooan IP address.</span></span> <span data-ttu-id="0aecb-129">Azure Cloud Service hello esetben hello hello szolgáltatás virtuális IP-cím.</span><span class="sxs-lookup"><span data-stu-id="0aecb-129">In hello case of an Azure Cloud Service, hello virtual IP of hello service.</span></span> <span data-ttu-id="0aecb-130">Így hello fő előnye, hogy az A rekord keresztül egy olyan CNAME rekordot, akkor használja, mint a helyettesítő karakter, egy vagy több bejegyzése \* **. contoso.com**, amely volna tanúsítványigénylések több altartományok például  **mail.contoso.com**, **login.contoso.com**, vagy **www.contso.com**.</span><span class="sxs-lookup"><span data-stu-id="0aecb-130">So hello main benefit of an A record over a CNAME record is that you can have one entry that uses a wildcard, such as \***.contoso.com**, which would handle requests for multiple sub-domains such as **mail.contoso.com**, **login.contoso.com**, or **www.contso.com**.</span></span>

> [!NOTE]
> <span data-ttu-id="0aecb-131">Mivel az A rekord le van képezve tooa statikus IP-cím, nem tudják automatikusan feloldani a felhőalapú szolgáltatás módosítások toohello IP-címét.</span><span class="sxs-lookup"><span data-stu-id="0aecb-131">Since an A record is mapped tooa static IP address, it cannot automatically resolve changes toohello IP address of your Cloud Service.</span></span> <span data-ttu-id="0aecb-132">a felhőalapú szolgáltatás által használt hello IP-cím van lefoglalva a hello először telepít tooan üres helyre (éles vagy átmeneti.) Hello tárolóhely hello központi telepítésének törlésekor Azure által kiadott hello IP-címet, és a jövőben a központi telepítésekre toohello tárolóhely új IP-cím is megadható.</span><span class="sxs-lookup"><span data-stu-id="0aecb-132">hello IP address used by your Cloud Service is allocated hello first time you deploy tooan empty slot (either production or staging.) If you delete hello deployment for hello slot, hello IP address is released by Azure and any future deployments toohello slot may be given a new IP address.</span></span>
> 
> <span data-ttu-id="0aecb-133">Állandó kényelmesen, egy adott üzembe helyezési pontjának (üzemi és átmeneti) hello IP-címét a csere közötti átmeneti és üzemi környezetek vagy egy meglévő központi telepítési helyben történő frissítése során.</span><span class="sxs-lookup"><span data-stu-id="0aecb-133">Conveniently, hello IP address of a given deployment slot (production or staging) is persisted when swapping between staging and production deployments or performing an in-place upgrade of an existing deployment.</span></span> <span data-ttu-id="0aecb-134">Ezek a műveletek végrehajtásával további információkért lásd: [hogyan toomanage felhőalapú szolgáltatások](cloud-services-how-to-manage.md).</span><span class="sxs-lookup"><span data-stu-id="0aecb-134">For more information on performing these actions, see [How toomanage cloud services](cloud-services-how-to-manage.md).</span></span>
> 
> 

## <a name="add-a-cname-record-for-your-custom-domain"></a><span data-ttu-id="0aecb-135">Adjon hozzá egy CNAME rekordot, az egyéni tartományhoz</span><span class="sxs-lookup"><span data-stu-id="0aecb-135">Add a CNAME record for your custom domain</span></span>
<span data-ttu-id="0aecb-136">toocreate egy olyan CNAME rekordot, hozzá kell adnia egy új bejegyzést hello DNS-tábla az egyéni tartomány a regisztráló által biztosított hello eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="0aecb-136">toocreate a CNAME record, you must add a new entry in hello DNS table for your custom domain by using hello tools provided by your registrar.</span></span> <span data-ttu-id="0aecb-137">Minden tartományregisztráló egy CNAME rekordot a telepítésükhöz hasonló, csak metódust tartalmaz, de koncepciók hello hello azonos.</span><span class="sxs-lookup"><span data-stu-id="0aecb-137">Each registrar has a similar but slightly different method of specifying a CNAME record, but hello concepts are hello same.</span></span>

1. <span data-ttu-id="0aecb-138">Ezen módszerek toofind hello valamelyikével **. cloudapp.net** hozzárendelt tooyour felhőalapú szolgáltatás, a tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="0aecb-138">Use one of these methods toofind hello **.cloudapp.net** domain name assigned tooyour cloud service.</span></span>
   
   * <span data-ttu-id="0aecb-139">Bejelentkezési toohello [a klasszikus Azure portálon], válassza ki a felhőalapú szolgáltatás, válassza ki **irányítópult**, majd keresse meg hello **webhely URL-címe** hello bejegyzést **gyors áttekintő**  szakasz.</span><span class="sxs-lookup"><span data-stu-id="0aecb-139">Login toohello [Azure classic portal], select your cloud service, select **Dashboard**, and then find hello **Site URL** entry in hello **quick glance** section.</span></span>
     
       ![gyors áttekintő szakasz megjelenítő hello webhely URL-címe][csurl]
     
       <span data-ttu-id="0aecb-141">**VAGY**</span><span class="sxs-lookup"><span data-stu-id="0aecb-141">**OR**</span></span>  
   * <span data-ttu-id="0aecb-142">Telepítse és konfigurálja [Azure Powershell](/powershell/azure/overview), és majd a hello használata a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="0aecb-142">Install and configure [Azure Powershell](/powershell/azure/overview), and then use hello following command:</span></span>
     
       ```powershell
       Get-AzureDeployment -ServiceName yourservicename | Select Url
       ```
     
     <span data-ttu-id="0aecb-143">Mentse a hello tartománynevet hello URL-címet, vagy a metódus által visszaadott, szüksége lesz rá egy olyan CNAME rekordot létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="0aecb-143">Save hello domain name used in hello URL returned by either method, as you will need it when creating a CNAME record.</span></span>
2. <span data-ttu-id="0aecb-144">Jelentkezzen be tooyour DNS regisztráló webhelyén, és lépjen toohello lapra DNS kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="0aecb-144">Log on tooyour DNS registrar's website and go toohello page for managing DNS.</span></span> <span data-ttu-id="0aecb-145">Keressen hivatkozásokat vagy hello hely területek címkézve **tartománynév**, **DNS**, vagy **neve kezelő**.</span><span class="sxs-lookup"><span data-stu-id="0aecb-145">Look for links or areas of hello site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>
3. <span data-ttu-id="0aecb-146">Most már található, ahol válassza ki vagy adja meg a CNAME meg.</span><span class="sxs-lookup"><span data-stu-id="0aecb-146">Now find where you can select or enter CNAME's.</span></span> <span data-ttu-id="0aecb-147">Előfordulhat, hogy egy legördülő menüben írja be, vagy válassza a Speciális beállítások lap tooan tooselect hello rekord.</span><span class="sxs-lookup"><span data-stu-id="0aecb-147">You may have tooselect hello record type from a drop down, or go tooan advanced settings page.</span></span> <span data-ttu-id="0aecb-148">Hello szavak kell keresnie **CNAME**, **Alias**, vagy **altartományok**.</span><span class="sxs-lookup"><span data-stu-id="0aecb-148">You should look for hello words **CNAME**, **Alias**, or **Subdomains**.</span></span>
4. <span data-ttu-id="0aecb-149">Meg kell adni hello tartomány és altartomány alias a hello CNAME, például a **www** Ha azt szeretné, toocreate alias **www.customdomain.com**. Ha szeretne toocreate alias hello gyökértartomány, akkor elérhetőként hello "**@**" szimbólumot lát, az a regisztráló DNS-eszközök.</span><span class="sxs-lookup"><span data-stu-id="0aecb-149">You must also provide hello domain or subdomain alias for hello CNAME, such as **www** if you want toocreate an alias for **www.customdomain.com**. If you want toocreate an alias for hello root domain, it may be listed as hello '**@**' symbol in your registrar's DNS tools.</span></span>
5. <span data-ttu-id="0aecb-150">Ezt követően meg kell adnia egy kanonikus nevet, amely az alkalmazás **cloudapp.net** tartomány ebben az esetben.</span><span class="sxs-lookup"><span data-stu-id="0aecb-150">Then, you must provide a canonical host name, which is your application's **cloudapp.net** domain in this case.</span></span>

<span data-ttu-id="0aecb-151">Például a CNAME rekordot a következő hello származó összes forgalmat továbbítja **www.contoso.com** túl**contoso.cloudapp.net**, a telepített alkalmazás hello egyéni tartomány nevét:</span><span class="sxs-lookup"><span data-stu-id="0aecb-151">For example, hello following CNAME record forwards all traffic from **www.contoso.com** too**contoso.cloudapp.net**, hello custom domain name of your deployed application:</span></span>

| <span data-ttu-id="0aecb-152">Alias és a gazdagép neve/altartomány</span><span class="sxs-lookup"><span data-stu-id="0aecb-152">Alias/Host name/Subdomain</span></span> | <span data-ttu-id="0aecb-153">Canonical tartomány</span><span class="sxs-lookup"><span data-stu-id="0aecb-153">Canonical domain</span></span> |
| --- | --- |
| <span data-ttu-id="0aecb-154">www</span><span class="sxs-lookup"><span data-stu-id="0aecb-154">www</span></span> |<span data-ttu-id="0aecb-155">contoso.cloudapp.NET</span><span class="sxs-lookup"><span data-stu-id="0aecb-155">contoso.cloudapp.net</span></span> |

<span data-ttu-id="0aecb-156">A látogatói **www.contoso.com** nem veszi észre hello igaz állomás (contoso.cloudapp.net), így folyamat továbbítási hello láthatatlan toothe végfelhasználói.</span><span class="sxs-lookup"><span data-stu-id="0aecb-156">A visitor of **www.contoso.com** will never see hello true host (contoso.cloudapp.net), so hello forwarding process is invisible toothe end user.</span></span>

> [!NOTE]
> <span data-ttu-id="0aecb-157">hello példa a fenti csak érvényes tootraffic: hello **www** altartomány.</span><span class="sxs-lookup"><span data-stu-id="0aecb-157">hello example above only applies tootraffic at hello **www** subdomain.</span></span> <span data-ttu-id="0aecb-158">Helyettesítő karakterek nem használhatja a CNAME-rekordot, mert minden tartomány/altartomány egy CNAME REKORDOT kell létrehoznia.</span><span class="sxs-lookup"><span data-stu-id="0aecb-158">Since you cannot use wildcards with CNAME records, you must create one CNAME for each domain/subdomain.</span></span> <span data-ttu-id="0aecb-159">Toodirect forgalmát altartományt, például a kívánt \*. contoso.com, tooyour cloudapp.net cím, konfigurálhat egy **átirányítási URL-cím** vagy **előre URL-cím** bejegyzést a DNS-beállításokat, vagy az A rekord létrehozására.</span><span class="sxs-lookup"><span data-stu-id="0aecb-159">If you want toodirect  traffic from subdomains, such as \*.contoso.com, tooyour cloudapp.net address, you can configure a **URL Redirect** or **URL Forward** entry in your DNS settings, or create an A record.</span></span>
> 
> 

## <a name="add-an-a-record-for-your-custom-domain"></a><span data-ttu-id="0aecb-160">Az egyéni tartomány az A rekord hozzáadása</span><span class="sxs-lookup"><span data-stu-id="0aecb-160">Add an A record for your custom domain</span></span>
<span data-ttu-id="0aecb-161">toocreate egy A rekordot, először keresse meg a felhőalapú szolgáltatás hello virtuális IP-címét.</span><span class="sxs-lookup"><span data-stu-id="0aecb-161">toocreate an A record, you must first find hello virtual IP address of your cloud service.</span></span> <span data-ttu-id="0aecb-162">Majd új bejegyzés hozzáadása az egyéni tartomány hello DNS táblázatban a regisztráló által biztosított hello eszközökkel.</span><span class="sxs-lookup"><span data-stu-id="0aecb-162">Then add a new entry in hello DNS table for your custom domain by using hello tools provided by your registrar.</span></span> <span data-ttu-id="0aecb-163">Minden tartományregisztráló egy A rekordot telepítésükhöz hasonló, csak metódust tartalmaz, de koncepciók hello hello azonos.</span><span class="sxs-lookup"><span data-stu-id="0aecb-163">Each registrar has a similar but slightly different method of specifying an A record, but hello concepts are hello same.</span></span>

1. <span data-ttu-id="0aecb-164">A következő módszerek tooget hello IP-címet a felhőszolgáltatás hello egyikét használhatja.</span><span class="sxs-lookup"><span data-stu-id="0aecb-164">Use one of hello following methods tooget hello IP address of your cloud service.</span></span>
   
   * <span data-ttu-id="0aecb-165">bejelentkezési toohello [a klasszikus Azure portálon], válassza ki a felhőalapú szolgáltatás, válassza ki **irányítópult**, majd keresse meg hello **nyilvános virtuális IP-cím (VIP)** hello bejegyzést**gyors áttekintő** szakasz.</span><span class="sxs-lookup"><span data-stu-id="0aecb-165">login toohello [Azure classic portal], select your cloud service, select **Dashboard**, and then find hello **Public Virtual IP (VIP) address** entry in hello **quick glance** section.</span></span>
     
       ![hello VIP megjelenítő gyors áttekintő szakasz][vip]
     
       <span data-ttu-id="0aecb-167">**VAGY**</span><span class="sxs-lookup"><span data-stu-id="0aecb-167">**OR**</span></span>  
   * <span data-ttu-id="0aecb-168">Telepítse és konfigurálja [Azure Powershell](/powershell/azure/overview), és majd a hello használata a következő parancsot:</span><span class="sxs-lookup"><span data-stu-id="0aecb-168">Install and configure [Azure Powershell](/powershell/azure/overview), and then use hello following command:</span></span>
     
       ```powershell
       get-azurevm -servicename yourservicename | get-azureendpoint -VM {$_.VM} | select Vip
       ```
     
     <span data-ttu-id="0aecb-169">Ha több, a felhőalapú szolgáltatáshoz társított végpont, több sort tartalmazó hello IP-címet kap, de megjelenjen-e az összes hello azonos cím.</span><span class="sxs-lookup"><span data-stu-id="0aecb-169">If you have multiple endpoints associated with your cloud service, you will receive multiple lines containing hello IP address, but all should display hello same address.</span></span>
     
     <span data-ttu-id="0aecb-170">Hello IP-cím mentése szüksége lesz rá egy A rekordot létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="0aecb-170">Save hello IP address, as you will need it when creating an A record.</span></span>
2. <span data-ttu-id="0aecb-171">Jelentkezzen be tooyour DNS regisztráló webhelyén, és lépjen toohello lapra DNS kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="0aecb-171">Log on tooyour DNS registrar's website and go toohello page for managing DNS.</span></span> <span data-ttu-id="0aecb-172">Keressen hivatkozásokat vagy hello hely területek címkézve **tartománynév**, **DNS**, vagy **neve kezelő**.</span><span class="sxs-lookup"><span data-stu-id="0aecb-172">Look for links or areas of hello site labeled as **Domain Name**, **DNS**, or **Name Server Management**.</span></span>
3. <span data-ttu-id="0aecb-173">Most már található, ahol válasszon vagy adjon meg egy olyan rekordot.</span><span class="sxs-lookup"><span data-stu-id="0aecb-173">Now find where you can select or enter A record's.</span></span> <span data-ttu-id="0aecb-174">Előfordulhat, hogy egy legördülő menüben írja be, vagy válassza a Speciális beállítások lap tooan tooselect hello rekord.</span><span class="sxs-lookup"><span data-stu-id="0aecb-174">You may have tooselect hello record type from a drop down, or go tooan advanced settings page.</span></span>
4. <span data-ttu-id="0aecb-175">Válassza ki, vagy adja meg a hello tartomány vagy altartományt, amelyet ez A rekord fog használni.</span><span class="sxs-lookup"><span data-stu-id="0aecb-175">Select or enter hello domain or subdomain that will use this A record.</span></span> <span data-ttu-id="0aecb-176">Válassza például **www** Ha azt szeretné, toocreate alias **www.customdomain.com**. Ha egy helyettesítő karakteres bejegyzés toocreate altartományokkal szeretne, adjon meg "__*__".</span><span class="sxs-lookup"><span data-stu-id="0aecb-176">For example, select **www** if you want toocreate an alias for **www.customdomain.com**. If you want toocreate a wildcard entry for all subdomains, enter '__*__'.</span></span> <span data-ttu-id="0aecb-177">Ez például lefedi altartományokkal **mail.customdomain.com**, **login.customdomain.com**, és **www.customdomain.com**.</span><span class="sxs-lookup"><span data-stu-id="0aecb-177">This will cover all sub-domains such as **mail.customdomain.com**, **login.customdomain.com**, and **www.customdomain.com**.</span></span>
   
    <span data-ttu-id="0aecb-178">Hello gyökértartomány toocreate egy A rekordot szeretne, ha azt elérhetőként hello "**@**" szimbólumot lát, az a regisztráló DNS-eszközök.</span><span class="sxs-lookup"><span data-stu-id="0aecb-178">If you want toocreate an A record for hello root domain, it may be listed as hello '**@**' symbol in your registrar's DNS tools.</span></span>
5. <span data-ttu-id="0aecb-179">Adja meg a felhőszolgáltatás hello IP-cím mezőben megadott hello.</span><span class="sxs-lookup"><span data-stu-id="0aecb-179">Enter hello IP address of your cloud service in hello provided field.</span></span> <span data-ttu-id="0aecb-180">Ez társít hello tartomány bejegyzés hello A rekordban a cloud service-környezet hello IP-címmel.</span><span class="sxs-lookup"><span data-stu-id="0aecb-180">This associates hello domain entry used in hello A record with hello IP address of your cloud service deployment.</span></span>

<span data-ttu-id="0aecb-181">Például a rekord a következő hello származó összes forgalmat továbbítja **contoso.com** túl**137.135.70.239**, IP-címét a telepített alkalmazás hello:</span><span class="sxs-lookup"><span data-stu-id="0aecb-181">For example, hello following A record forwards all traffic from **contoso.com** too**137.135.70.239**, hello IP address of your deployed application:</span></span>

| <span data-ttu-id="0aecb-182">Gazdagép neve/altartomány</span><span class="sxs-lookup"><span data-stu-id="0aecb-182">Host name/Subdomain</span></span> | <span data-ttu-id="0aecb-183">IP-cím</span><span class="sxs-lookup"><span data-stu-id="0aecb-183">IP address</span></span> |
| --- | --- |
| @ |<span data-ttu-id="0aecb-184">137.135.70.239</span><span class="sxs-lookup"><span data-stu-id="0aecb-184">137.135.70.239</span></span> |

<span data-ttu-id="0aecb-185">Ez a példa azt mutatja be, hello legfelső szintű tartomány az A rekord létrehozása.</span><span class="sxs-lookup"><span data-stu-id="0aecb-185">This example demonstrates creating an A record for hello root domain.</span></span> <span data-ttu-id="0aecb-186">Ha egy helyettesítő karakteres bejegyzés toocover kívánja toocreate altartományokkal, adjon meg "__*__" hello altartomány szerint.</span><span class="sxs-lookup"><span data-stu-id="0aecb-186">If you wish toocreate a wildcard entry toocover all subdomains, you would enter '__*__' as hello subdomain.</span></span>

> [!WARNING]
> <span data-ttu-id="0aecb-187">IP-címek az Azure-ban alapértelmezés szerint dinamikus.</span><span class="sxs-lookup"><span data-stu-id="0aecb-187">IP addresses in Azure are dynamic by default.</span></span> <span data-ttu-id="0aecb-188">Toouse érdemes egy [fenntartott IP-cím](../virtual-network/virtual-networks-reserved-public-ip.md) tooensure, amely az IP-címe nem változik.</span><span class="sxs-lookup"><span data-stu-id="0aecb-188">You will probably want toouse a [reserved IP address](../virtual-network/virtual-networks-reserved-public-ip.md) tooensure that your IP address does not change.</span></span>
> 
> 

## <a name="next-steps"></a><span data-ttu-id="0aecb-189">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="0aecb-189">Next steps</span></span>
* [<span data-ttu-id="0aecb-190">TooManage Cloud Services</span><span class="sxs-lookup"><span data-stu-id="0aecb-190">How tooManage Cloud Services</span></span>](cloud-services-how-to-manage.md)
* [<span data-ttu-id="0aecb-191">Hogyan tooMap CDN tartalom tooa egyéni tartományhoz</span><span class="sxs-lookup"><span data-stu-id="0aecb-191">How tooMap CDN Content tooa Custom Domain</span></span>](../cdn/cdn-map-content-to-custom-domain.md)
* <span data-ttu-id="0aecb-192">[A felhőalapú szolgáltatás általános konfigurációs](cloud-services-how-to-configure.md).</span><span class="sxs-lookup"><span data-stu-id="0aecb-192">[General configuration of your cloud service](cloud-services-how-to-configure.md).</span></span>
* <span data-ttu-id="0aecb-193">Ismerje meg, hogyan túl[felhőalapú szolgáltatás telepítése](cloud-services-how-to-create-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="0aecb-193">Learn how too[deploy a cloud service](cloud-services-how-to-create-deploy.md).</span></span>
* <span data-ttu-id="0aecb-194">Konfigurálása [ssl-tanúsítványok](cloud-services-configure-ssl-certificate.md).</span><span class="sxs-lookup"><span data-stu-id="0aecb-194">Configure [ssl certificates](cloud-services-configure-ssl-certificate.md).</span></span>

[Expose Your Application on a Custom Domain]: #access-app
[Add a CNAME Record for Your Custom Domain]: #add-cname
[Expose Your Data on a Custom Domain]: #access-data
[VIP swaps]: http://msdn.microsoft.com/library/ee517253.aspx
[Create a CNAME record that associates hello subdomain with hello storage account]: #create-cname
[a klasszikus Azure portálon]: https://manage.windowsazure.com
[Validate Custom Domain dialog box]: http://i.msdn.microsoft.com/dynimg/IC544437.jpg
[vip]: ./media/cloud-services-custom-domain-name/csvip.png
[csurl]: ./media/cloud-services-custom-domain-name/csurl.png
