---
title: "aaaCreate és a belső terheléselosztók az Azure App Service-környezet használata"
description: "Megtudhatja, hogyan toocreate és egy internet-egymástól el vannak különítve Azure App Service-környezet használata"
services: app-service
documentationcenter: na
author: ccompy
manager: stefsch
ms.assetid: 0f4c1fa4-e344-46e7-8d24-a25e247ae138
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2017
ms.author: ccompy
ms.openlocfilehash: d019ca6f231c3acfdab4cd380db375a076802f7a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-use-an-internal-load-balancer-with-an-app-service-environment"></a><span data-ttu-id="4e88b-103">Létrehozhat és használhat belső terheléselosztót az App Service-környezet</span><span class="sxs-lookup"><span data-stu-id="4e88b-103">Create and use an internal load balancer with an App Service environment</span></span> #

 <span data-ttu-id="4e88b-104">Az Azure App Service Environment-környezet az Azure App Service egy Azure virtuális hálózatot (VNet) lévő alhálózatot történő központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="4e88b-104">Azure App Service Environment is a deployment of Azure App Service into a subnet in an Azure virtual network (VNet).</span></span> <span data-ttu-id="4e88b-105">Nincsenek két módon toodeploy az App Service-környezetek (ASE):</span><span class="sxs-lookup"><span data-stu-id="4e88b-105">There are two ways toodeploy an App Service environment (ASE):</span></span> 

- <span data-ttu-id="4e88b-106">A virtuális IP-címre a külső IP-cím, egy külső ASE gyakran nevezik.</span><span class="sxs-lookup"><span data-stu-id="4e88b-106">With a VIP on an external IP address, often called an External ASE.</span></span>
- <span data-ttu-id="4e88b-107">A virtuális IP-címre a belső IP-címet gyakran nevezik egy ILB ASE mert hello belső végpont egy belső terheléselosztón (ILB).</span><span class="sxs-lookup"><span data-stu-id="4e88b-107">With a VIP on an internal IP address, often called an ILB ASE because hello internal endpoint is an internal load balancer (ILB).</span></span> 

<span data-ttu-id="4e88b-108">Ez a cikk bemutatja, hogyan toocreate egy ILB ASE.</span><span class="sxs-lookup"><span data-stu-id="4e88b-108">This article shows you how toocreate an ILB ASE.</span></span> <span data-ttu-id="4e88b-109">A hello ASE áttekintéséért lásd: [bemutatása tooApp Service-környezetek][Intro].</span><span class="sxs-lookup"><span data-stu-id="4e88b-109">For an overview on hello ASE, see [Introduction tooApp Service environments][Intro].</span></span> <span data-ttu-id="4e88b-110">Hogyan toocreate egy külső ASE: toolearn [hozzon létre egy külső ASE][MakeExternalASE].</span><span class="sxs-lookup"><span data-stu-id="4e88b-110">toolearn how toocreate an External ASE, see [Create an External ASE][MakeExternalASE].</span></span>

## <a name="overview"></a><span data-ttu-id="4e88b-111">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="4e88b-111">Overview</span></span> ##

<span data-ttu-id="4e88b-112">A virtuális hálózat telepítheti egy ASE az internetről elérhető végpontok vagy IP-címet.</span><span class="sxs-lookup"><span data-stu-id="4e88b-112">You can deploy an ASE with an internet-accessible endpoint or with an IP address in your VNet.</span></span> <span data-ttu-id="4e88b-113">tooset hello IP-címek tooa VNet-címet, hello ASE egy ILB együtt kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="4e88b-113">tooset hello IP address tooa VNet address, hello ASE must be deployed with an ILB.</span></span> <span data-ttu-id="4e88b-114">Ha az egy ILB a ASE telepít, meg kell adnia:</span><span class="sxs-lookup"><span data-stu-id="4e88b-114">When you deploy your ASE with an ILB, you must provide:</span></span>

-   <span data-ttu-id="4e88b-115">A saját tartomány használni, amikor a alkalmazásai létrehozására.</span><span class="sxs-lookup"><span data-stu-id="4e88b-115">Your own domain that you use when you create your apps.</span></span>
-   <span data-ttu-id="4e88b-116">hello a HTTPS-hez használt tanúsítvány.</span><span class="sxs-lookup"><span data-stu-id="4e88b-116">hello certificate used for HTTPS.</span></span>
-   <span data-ttu-id="4e88b-117">A tartomány DNS-kezelés.</span><span class="sxs-lookup"><span data-stu-id="4e88b-117">DNS management for your domain.</span></span>

<span data-ttu-id="4e88b-118">Ismét műveleteket végezheti el, mint:</span><span class="sxs-lookup"><span data-stu-id="4e88b-118">In return, you can do things such as:</span></span>

-   <span data-ttu-id="4e88b-119">Intranetes alkalmazások üzemeltetését biztonságosan felhőben hello, amelyek egy-webhelyek vagy Azure ExpressRoute VPN keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="4e88b-119">Host intranet applications securely in hello cloud, which you access through a site-to-site or Azure ExpressRoute VPN.</span></span>
-   <span data-ttu-id="4e88b-120">Állomás alkalmazások hello felhőben a nyilvános DNS-kiszolgálók nem jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="4e88b-120">Host apps in hello cloud that aren't listed in public DNS servers.</span></span>
-   <span data-ttu-id="4e88b-121">Hozzon létre internet elszigetelt háttér-alkalmazásokat, az előtér-alkalmazások biztonságosan integrálható.</span><span class="sxs-lookup"><span data-stu-id="4e88b-121">Create internet-isolated back-end apps, which your front-end apps can securely integrate with.</span></span>

### <a name="disabled-functionality"></a><span data-ttu-id="4e88b-122">Letiltott funkció</span><span class="sxs-lookup"><span data-stu-id="4e88b-122">Disabled functionality</span></span> ###

<span data-ttu-id="4e88b-123">Néhány dolgot, amely egy ILB ASE használatakor nem hajtható végre:</span><span class="sxs-lookup"><span data-stu-id="4e88b-123">There are some things that you can't do when you use an ILB ASE:</span></span>

-   <span data-ttu-id="4e88b-124">IP-alapú SSL használatára.</span><span class="sxs-lookup"><span data-stu-id="4e88b-124">Use IP-based SSL.</span></span>
-   <span data-ttu-id="4e88b-125">IP-cím hozzárendelése toospecific alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="4e88b-125">Assign IP addresses toospecific apps.</span></span>
-   <span data-ttu-id="4e88b-126">Vásároljon és tanúsítványt használjon olyan alkalmazással hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="4e88b-126">Buy and use a certificate with an app through hello Azure portal.</span></span> <span data-ttu-id="4e88b-127">Tanúsítványok beszerzése hitelesítésszolgáltatótól származó közvetlenül, és használhatja azokat az alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="4e88b-127">You can obtain certificates directly from a certificate authority and use them with your apps.</span></span> <span data-ttu-id="4e88b-128">Nem lehet beszerezni azokat, hello Azure-portálon keresztül.</span><span class="sxs-lookup"><span data-stu-id="4e88b-128">You can't obtain them through hello Azure portal.</span></span>

## <a name="create-an-ilb-ase"></a><span data-ttu-id="4e88b-129">Hozzon létre egy ILB ASE</span><span class="sxs-lookup"><span data-stu-id="4e88b-129">Create an ILB ASE</span></span> ##

<span data-ttu-id="4e88b-130">egy ILB ASE toocreate:</span><span class="sxs-lookup"><span data-stu-id="4e88b-130">toocreate an ILB ASE:</span></span>

1. <span data-ttu-id="4e88b-131">Hello Azure-portálon, válassza ki **új** > **Web + mobil** > **App Service Environment-környezet**.</span><span class="sxs-lookup"><span data-stu-id="4e88b-131">In hello Azure portal, select **New** > **Web + Mobile** > **App Service Environment**.</span></span>

2. <span data-ttu-id="4e88b-132">Válassza ki előfizetését.</span><span class="sxs-lookup"><span data-stu-id="4e88b-132">Select your subscription.</span></span>

3. <span data-ttu-id="4e88b-133">Válasszon ki vagy hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="4e88b-133">Select or create a resource group.</span></span>

4. <span data-ttu-id="4e88b-134">Válassza ki, vagy hozzon létre egy Vnetet.</span><span class="sxs-lookup"><span data-stu-id="4e88b-134">Select or create a VNet.</span></span>

5. <span data-ttu-id="4e88b-135">Ha egy meglévő virtuális hálózatot választ ki, egy alhálózat toohold hello ASE toocreate kell.</span><span class="sxs-lookup"><span data-stu-id="4e88b-135">If you select an existing VNet, you need toocreate a subnet toohold hello ASE.</span></span> <span data-ttu-id="4e88b-136">Ellenőrizze, hogy tooset alhálózat méretének elég nagy tooaccommodate a ASE jövőbeli növekedésének.</span><span class="sxs-lookup"><span data-stu-id="4e88b-136">Make sure tooset a subnet size large enough tooaccommodate any future growth of your ASE.</span></span> <span data-ttu-id="4e88b-137">Azt javasoljuk, hogy a méretet `/25`, amely 128-címekkel rendelkezik, és kezelni tud a maximális méretű ASE.</span><span class="sxs-lookup"><span data-stu-id="4e88b-137">We recommend a size of `/25`, which has 128 addresses and can handle a maximum-sized ASE.</span></span> <span data-ttu-id="4e88b-138">hello minimális méret választhat egy `/28`.</span><span class="sxs-lookup"><span data-stu-id="4e88b-138">hello minimum size you can select is a `/28`.</span></span> <span data-ttu-id="4e88b-139">Után szükség van az infrastruktúra, ez a méret lehet méretezett tooa legfeljebb 11 példányok.</span><span class="sxs-lookup"><span data-stu-id="4e88b-139">After infrastructure needs, this size can be scaled tooa maximum of 11 instances.</span></span>

    * <span data-ttu-id="4e88b-140">Az App Service-csomagokról túlmutató hello alapértelmezett legfeljebb 100 példányok.</span><span class="sxs-lookup"><span data-stu-id="4e88b-140">Go beyond hello default maximum of 100 instances in your App Service plans.</span></span>

    * <span data-ttu-id="4e88b-141">Méretezhető, 100 közelében, de gyorsabb előtér-méretezés.</span><span class="sxs-lookup"><span data-stu-id="4e88b-141">Scale near 100 but with more rapid front-end scaling.</span></span>

6. <span data-ttu-id="4e88b-142">Válassza ki **virtuális hálózati/hely** > **virtuális hálózati konfiguráció**.</span><span class="sxs-lookup"><span data-stu-id="4e88b-142">Select **Virtual Network/Location** > **Virtual Network Configuration**.</span></span> <span data-ttu-id="4e88b-143">Set hello **VIP típus** túl**belső**.</span><span class="sxs-lookup"><span data-stu-id="4e88b-143">Set hello **VIP Type** too**Internal**.</span></span>

7. <span data-ttu-id="4e88b-144">Adja meg a tartomány nevét.</span><span class="sxs-lookup"><span data-stu-id="4e88b-144">Enter a domain name.</span></span> <span data-ttu-id="4e88b-145">Ebben a tartományban, egy a ASE létre alkalmazásokat a hello.</span><span class="sxs-lookup"><span data-stu-id="4e88b-145">This domain is hello one used for apps created in this ASE.</span></span> <span data-ttu-id="4e88b-146">Nincsenek bizonyos korlátozások vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="4e88b-146">There are some restrictions.</span></span> <span data-ttu-id="4e88b-147">Nem lehet:</span><span class="sxs-lookup"><span data-stu-id="4e88b-147">It can't be:</span></span>

    * <span data-ttu-id="4e88b-148">nettó</span><span class="sxs-lookup"><span data-stu-id="4e88b-148">net</span></span>   

    * <span data-ttu-id="4e88b-149">azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="4e88b-149">azurewebsites.net</span></span>

    * <span data-ttu-id="4e88b-150">p.azurewebsites.NET</span><span class="sxs-lookup"><span data-stu-id="4e88b-150">p.azurewebsites.net</span></span>

    * <span data-ttu-id="4e88b-151">&lt;asename környezet&gt;. p.azurewebsites.net</span><span class="sxs-lookup"><span data-stu-id="4e88b-151">&lt;asename&gt;.p.azurewebsites.net</span></span>

   <span data-ttu-id="4e88b-152">alkalmazások használt hello egyéni tartománynevet, és használja a ASE hello tartománynév nem lehet átfedésben.</span><span class="sxs-lookup"><span data-stu-id="4e88b-152">hello custom domain name used for apps and hello domain name used by your ASE can't overlap.</span></span> <span data-ttu-id="4e88b-153">Az egy ILB ASE hello tartománynévvel _contoso.com_, nem használható egyéni tartománynevek az alkalmazásokhoz, például:</span><span class="sxs-lookup"><span data-stu-id="4e88b-153">For an ILB ASE with hello domain name _contoso.com_, you can't use custom domain names for your apps like:</span></span>

    * <span data-ttu-id="4e88b-154">www.contoso.com</span><span class="sxs-lookup"><span data-stu-id="4e88b-154">www.contoso.com</span></span>

    * <span data-ttu-id="4e88b-155">ABCD.def.contoso.com</span><span class="sxs-lookup"><span data-stu-id="4e88b-155">abcd.def.contoso.com</span></span>

    * <span data-ttu-id="4e88b-156">ABCD.contoso.com</span><span class="sxs-lookup"><span data-stu-id="4e88b-156">abcd.contoso.com</span></span>

   <span data-ttu-id="4e88b-157">Ha ismeri az egyéni tartománynevek hello az alkalmazásokat, válassza ki egy tartományt a hello ILB ASE, amelyek nem rendelkeznek ezen egyéni tartománynevekkel ütközés.</span><span class="sxs-lookup"><span data-stu-id="4e88b-157">If you know hello custom domain names for your apps, choose a domain for hello ILB ASE that won’t have a conflict with those custom domain names.</span></span> <span data-ttu-id="4e88b-158">Ebben a példában, használhatja a következőhöz hasonlóan *contoso-internal.com* hello tartomány a hajlamosnak mert, amelyek nem ütköznek egyéni tartománynevek végződő *. contoso.com*.</span><span class="sxs-lookup"><span data-stu-id="4e88b-158">In this example, you can use something like *contoso-internal.com* for hello domain of your ASE because that won't conflict with custom domain names that end in *.contoso.com*.</span></span>

8. <span data-ttu-id="4e88b-159">Válassza ki **OK**, majd válassza ki **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="4e88b-159">Select **OK**, and then select **Create**.</span></span>

    ![ASE létrehozása][1]

<span data-ttu-id="4e88b-161">A hello **virtuális hálózati** panelen van egy **virtuális hálózati konfiguráció** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="4e88b-161">On hello **Virtual Network** blade, there is a **Virtual Network Configuration** option.</span></span> <span data-ttu-id="4e88b-162">Használat tooselect egy külső VIP vagy VIP belső.</span><span class="sxs-lookup"><span data-stu-id="4e88b-162">You can use it tooselect an External VIP or an Internal VIP.</span></span> <span data-ttu-id="4e88b-163">hello alapértelmezett érték a **külső**.</span><span class="sxs-lookup"><span data-stu-id="4e88b-163">hello default is **External**.</span></span> <span data-ttu-id="4e88b-164">Ha **külső**, a ASE egy internetről elérhető VIP használja.</span><span class="sxs-lookup"><span data-stu-id="4e88b-164">If you select **External**, your ASE uses an internet-accessible VIP.</span></span> <span data-ttu-id="4e88b-165">Ha **belső**, a ASE egy ILB a Vneten belül egy IP-cím van konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="4e88b-165">If you select **Internal**, your ASE is configured with an ILB on an IP address within your VNet.</span></span>

<span data-ttu-id="4e88b-166">Miután kiválasztotta a **belső**, hello képességét tooadd több IP-címet tooyour ASE törlődik.</span><span class="sxs-lookup"><span data-stu-id="4e88b-166">After you select **Internal**, hello ability tooadd more IP addresses tooyour ASE is removed.</span></span> <span data-ttu-id="4e88b-167">Ehelyett tooprovide hello tartományának hello ASE van szüksége.</span><span class="sxs-lookup"><span data-stu-id="4e88b-167">Instead, you need tooprovide hello domain of hello ASE.</span></span> <span data-ttu-id="4e88b-168">Service-környezetben egy külső virtuális IP-címre, ASE hello tartomány szolgál, hogy ASE létrehozott alkalmazások hello hello nevét.</span><span class="sxs-lookup"><span data-stu-id="4e88b-168">In an ASE with an External VIP, hello name of hello ASE is used in hello domain for apps created in that ASE.</span></span>

<span data-ttu-id="4e88b-169">Ha **VIP típus** túl**belső**, a ASE név nem szolgál hello tartomány hello ASE.</span><span class="sxs-lookup"><span data-stu-id="4e88b-169">If you set **VIP Type** too**Internal**, your ASE name is not used in hello domain for hello ASE.</span></span> <span data-ttu-id="4e88b-170">Hello tartomány explicit módon adja meg.</span><span class="sxs-lookup"><span data-stu-id="4e88b-170">You specify hello domain explicitly.</span></span> <span data-ttu-id="4e88b-171">Ha a tartomány *contoso.corp.net* és abban a ASE nevű alkalmazást hoz létre *timereporting*, hello URL-cím, az alkalmazás timereporting.contoso.corp.net érték.</span><span class="sxs-lookup"><span data-stu-id="4e88b-171">If your domain is *contoso.corp.net* and you create an app in that ASE named *timereporting*, hello URL for that app is timereporting.contoso.corp.net.</span></span>


## <a name="create-an-app-in-an-ilb-ase"></a><span data-ttu-id="4e88b-172">Hozzon létre egy alkalmazást-Példánynak környezetben</span><span class="sxs-lookup"><span data-stu-id="4e88b-172">Create an app in an ILB ASE</span></span> ##

<span data-ttu-id="4e88b-173">A hello-Példánynak környezetben alkalmazást hoz létre, hogy hozzon létre egy app Service-környezetben általában azonos módon.</span><span class="sxs-lookup"><span data-stu-id="4e88b-173">You create an app in an ILB ASE in hello same way that you create an app in an ASE normally.</span></span>

1. <span data-ttu-id="4e88b-174">Hello Azure-portálon, válassza ki **új** > **Web + mobil** > **webes** vagy **Mobile** vagy  **API-alkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="4e88b-174">In hello Azure portal, select **New** > **Web + Mobile** > **Web** or **Mobile** or **API App**.</span></span>

2. <span data-ttu-id="4e88b-175">Adja meg a hello alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="4e88b-175">Enter hello name of hello app.</span></span>

3. <span data-ttu-id="4e88b-176">Válassza ki a hello előfizetést.</span><span class="sxs-lookup"><span data-stu-id="4e88b-176">Select hello subscription.</span></span>

4. <span data-ttu-id="4e88b-177">Válasszon ki vagy hozzon létre egy erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="4e88b-177">Select or create a resource group.</span></span>

5. <span data-ttu-id="4e88b-178">Válassza ki, vagy hozzon létre egy App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="4e88b-178">Select or create an App Service plan.</span></span> <span data-ttu-id="4e88b-179">Ha egy új App Service-csomag toocreate, jelölje be a ASE hello helyként.</span><span class="sxs-lookup"><span data-stu-id="4e88b-179">If you want toocreate a new App Service plan, select your ASE as hello location.</span></span> <span data-ttu-id="4e88b-180">Válassza ki a létrehozott App Service-csomag toobe, ahová hello feldolgozókészletek.</span><span class="sxs-lookup"><span data-stu-id="4e88b-180">Select hello worker pool where you want your App Service plan toobe created.</span></span> <span data-ttu-id="4e88b-181">Hello App Service-csomag létrehozásakor válassza ki a ASE hello helyét és hello munkavégző készletét.</span><span class="sxs-lookup"><span data-stu-id="4e88b-181">When you create hello App Service plan, select your ASE as hello location and hello worker pool.</span></span> <span data-ttu-id="4e88b-182">Hello alkalmazás hello nevét adja meg, ha az alkalmazás neve alatt hello tartomány lép hello tartomány által a ASE.</span><span class="sxs-lookup"><span data-stu-id="4e88b-182">When you specify hello name of hello app, hello domain under your app name is replaced by hello domain for your ASE.</span></span>

6. <span data-ttu-id="4e88b-183">Kattintson a **Létrehozás** gombra.</span><span class="sxs-lookup"><span data-stu-id="4e88b-183">Select **Create**.</span></span> <span data-ttu-id="4e88b-184">Ha hello app tooappear az irányítópulton, jelölje be a **PIN-kód toodashboard** jelölőnégyzetet.</span><span class="sxs-lookup"><span data-stu-id="4e88b-184">If you want hello app tooappear on your dashboard, select the **Pin toodashboard** check box.</span></span>

    ![App Service-csomag létrehozása][2]

    <span data-ttu-id="4e88b-186">A **alkalmazásnév**, hello tartománynév megadása a ASE frissített tooreflect hello tartományán.</span><span class="sxs-lookup"><span data-stu-id="4e88b-186">Under **App name**, hello domain name is updated tooreflect hello domain of your ASE.</span></span>

## <a name="post-ilb-ase-creation-validation"></a><span data-ttu-id="4e88b-187">POST-Példánynak ASE létrehozásának ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="4e88b-187">Post-ILB ASE creation validation</span></span> ##

<span data-ttu-id="4e88b-188">Egy ILB ASE mint hello nem - Példánynak ASE némileg eltérő.</span><span class="sxs-lookup"><span data-stu-id="4e88b-188">An ILB ASE is slightly different than hello non-ILB ASE.</span></span> <span data-ttu-id="4e88b-189">Már beállításértékeket, mint a saját DNS toomanage akkor kell.</span><span class="sxs-lookup"><span data-stu-id="4e88b-189">As already noted, you need toomanage your own DNS.</span></span> <span data-ttu-id="4e88b-190">Akkor is tooprovide saját tanúsítványát a HTTPS-kapcsolatoknál.</span><span class="sxs-lookup"><span data-stu-id="4e88b-190">You also have tooprovide your own certificate for HTTPS connections.</span></span>

<span data-ttu-id="4e88b-191">Miután létrehozta a ASE, hello tartománynév megadott hello tartomány jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="4e88b-191">After you create your ASE, hello domain name shows hello domain you specified.</span></span> <span data-ttu-id="4e88b-192">Új elem megjelenik a **beállítás** nevű menü **ILB tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="4e88b-192">A new item appears in the **Setting** menu called **ILB Certificate**.</span></span> <span data-ttu-id="4e88b-193">hello ASE olyan tanúsítvány, amely nem adja meg a hello ILB ASE tartományt hozza létre.</span><span class="sxs-lookup"><span data-stu-id="4e88b-193">hello ASE is created with a certificate that doesn't specify hello ILB ASE domain.</span></span> <span data-ttu-id="4e88b-194">Hello ASE tanúsítványt használja, ha a böngésző megtudhatja, hogy érvénytelen.</span><span class="sxs-lookup"><span data-stu-id="4e88b-194">If you use hello ASE with that certificate, your browser tells you that it's invalid.</span></span> <span data-ttu-id="4e88b-195">Ez a tanúsítvány teszi, hogy könnyebben tootest HTTPS, de a saját tanúsítványban kapcsolt tooyour ILB ASE tartományt kell tooupload.</span><span class="sxs-lookup"><span data-stu-id="4e88b-195">This certificate makes it easier tootest HTTPS, but you need tooupload your own certificate that's tied tooyour ILB ASE domain.</span></span> <span data-ttu-id="4e88b-196">Ez a lépés nem szükséges, függetlenül attól, hogy a tanúsítvány önaláírt vagy a hitelesítésszolgáltatótól beszerzett.</span><span class="sxs-lookup"><span data-stu-id="4e88b-196">This step is necessary regardless of whether your certificate is self-signed or acquired from a certificate authority.</span></span>

![ILB ASE tartománynév][3]

<span data-ttu-id="4e88b-198">A ILB ASE érvényes SSL-tanúsítvány szükséges.</span><span class="sxs-lookup"><span data-stu-id="4e88b-198">Your ILB ASE needs a valid SSL certificate.</span></span> <span data-ttu-id="4e88b-199">Belső hitelesítésszolgáltatók használja, vásárolhat egy tanúsítványt egy külső kibocsátó, vagy használhat önaláírt tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="4e88b-199">Use internal certificate authorities, purchase a certificate from an external issuer, or use a self-signed certificate.</span></span> <span data-ttu-id="4e88b-200">Hello SSL-tanúsítvány hello forrását, függetlenül hello következő tanúsítvány attribútumokkal kell toobe megfelelően konfigurálva:</span><span class="sxs-lookup"><span data-stu-id="4e88b-200">Regardless of hello source of hello SSL certificate, hello following certificate attributes need toobe configured properly:</span></span>

* <span data-ttu-id="4e88b-201">**Tulajdonos**: Ez az attribútum too*.your-gyökér-tartományi-Itt állítható be.</span><span class="sxs-lookup"><span data-stu-id="4e88b-201">**Subject**: This attribute must be set too*.your-root-domain-here.</span></span>
* <span data-ttu-id="4e88b-202">**Tulajdonos alternatív neve**: ennek az attribútumnak kell tartalmaznia a **.your-gyökér-tartományi-Itt* és **.scm.your-gyökér-tartományi-Itt*.</span><span class="sxs-lookup"><span data-stu-id="4e88b-202">**Subject Alternative Name**: This attribute must include both **.your-root-domain-here* and **.scm.your-root-domain-here*.</span></span> <span data-ttu-id="4e88b-203">Minden alkalmazáshoz kapcsolódó SCM/Kudu webhely SSL-kapcsolatok toohello hello űrlap cím használata *your-app-name.scm.your-root-domain-here*.</span><span class="sxs-lookup"><span data-stu-id="4e88b-203">SSL connections toohello SCM/Kudu site associated with each app use an address of hello form *your-app-name.scm.your-root-domain-here*.</span></span>

<span data-ttu-id="4e88b-204">A konvertálás/save hello SSL-tanúsítvány egy .pfx fájlba.</span><span class="sxs-lookup"><span data-stu-id="4e88b-204">Convert/save hello SSL certificate as a .pfx file.</span></span> <span data-ttu-id="4e88b-205">hello .pfx fájl tartalmazza az összes köztes kell, és a legfelső szintű tanúsítványok.</span><span class="sxs-lookup"><span data-stu-id="4e88b-205">hello .pfx file must include all intermediate and root certificates.</span></span> <span data-ttu-id="4e88b-206">A biztonság jelszóval.</span><span class="sxs-lookup"><span data-stu-id="4e88b-206">Secure it with a password.</span></span>

<span data-ttu-id="4e88b-207">Ha azt szeretné, hogy toocreate egy önaláírt tanúsítványt, hello PowerShell-parancsok itt is használhatja.</span><span class="sxs-lookup"><span data-stu-id="4e88b-207">If you want toocreate a self-signed certificate, you can use hello PowerShell commands here.</span></span> <span data-ttu-id="4e88b-208">Lehet, hogy toouse a ILB ASE tartománynév helyett *internal.contoso.com*:</span><span class="sxs-lookup"><span data-stu-id="4e88b-208">Be sure toouse your ILB ASE domain name instead of *internal.contoso.com*:</span></span> 

    $certificate = New-SelfSignedCertificate -certstorelocation cert:\localmachine\my -dnsname "\*.internal-contoso.com","\*.scm.internal-contoso.com"
    
    $certThumbprint = "cert:\localMachine\my\" +$certificate.Thumbprint
    $password = ConvertTo-SecureString -String "CHANGETHISPASSWORD" -Force -AsPlainText
    
    $fileName = "exportedcert.pfx" 
    Export-PfxCertificate -cert $certThumbprint -FilePath $fileName -Password $password

<span data-ttu-id="4e88b-209">a PowerShell-parancsok generáló hello tanúsítvány böngésző megjelölt, mert hello tanúsítványt a hitelesítésszolgáltatótól, amely megbízhatósági lánc a böngésző nem hozta létre.</span><span class="sxs-lookup"><span data-stu-id="4e88b-209">hello certificate that these PowerShell commands generate is flagged by browsers because hello certificate wasn't created by a certificate authority that's in your browser's chain of trust.</span></span> <span data-ttu-id="4e88b-210">tooget olyan tanúsítvány, amely megbízik a böngésző, be kell szereznie, a böngésző lánc megbízhatósági kereskedelmi hitelesítésszolgáltatótól származó.</span><span class="sxs-lookup"><span data-stu-id="4e88b-210">tooget a certificate that your browser trusts, procure it from a commercial certificate authority in your browser's chain of trust.</span></span> 

![Állítsa be a ILB tanúsítványt][4]

<span data-ttu-id="4e88b-212">tooupload saját tanúsítványokat és a vizsgálati hozzáférést:</span><span class="sxs-lookup"><span data-stu-id="4e88b-212">tooupload your own certificates and test access:</span></span>

1. <span data-ttu-id="4e88b-213">Hello ASE létrehozása után nyissa meg toohello ASE felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="4e88b-213">After hello ASE is created, go toohello ASE UI.</span></span> <span data-ttu-id="4e88b-214">Válassza ki **ASE** > **beállítások** > **ILB tanúsítvány**.</span><span class="sxs-lookup"><span data-stu-id="4e88b-214">Select **ASE** > **Settings** > **ILB Certificate**.</span></span>

2. <span data-ttu-id="4e88b-215">tooset hello ILB tanúsítvány, válasszon hello .pfx fájlt, majd írja be a hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="4e88b-215">tooset hello ILB certificate, select hello certificate .pfx file and enter hello password.</span></span> <span data-ttu-id="4e88b-216">Ez a lépés néhány alkalommal tooprocess vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="4e88b-216">This step takes some time tooprocess.</span></span> <span data-ttu-id="4e88b-217">Egy üzenet jelenik meg, amely meghatározza, hogy, hogy folyamatban van egy feltöltési művelet.</span><span class="sxs-lookup"><span data-stu-id="4e88b-217">A message appears stating that an upload operation is in progress.</span></span>

3. <span data-ttu-id="4e88b-218">A ASE hello ILB cím beolvasása.</span><span class="sxs-lookup"><span data-stu-id="4e88b-218">Get hello ILB address for your ASE.</span></span> <span data-ttu-id="4e88b-219">Válassza ki **ASE** > **tulajdonságok** > **virtuális IP-cím**.</span><span class="sxs-lookup"><span data-stu-id="4e88b-219">Select **ASE** > **Properties** > **Virtual IP Address**.</span></span>

4. <span data-ttu-id="4e88b-220">Webalkalmazás létrehozása a ASE hello ASE létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="4e88b-220">Create a web app in your ASE after hello ASE is created.</span></span>

5. <span data-ttu-id="4e88b-221">Virtuális gép létrehozása, ha még nincs fiókja, hogy a Vneten belül.</span><span class="sxs-lookup"><span data-stu-id="4e88b-221">Create a VM if you don't have one in that VNet.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="4e88b-222">A virtuális gép ne toocreate hello ugyanazon az alhálózaton, hello ASE, mert sikertelen, vagy problémákhoz.</span><span class="sxs-lookup"><span data-stu-id="4e88b-222">Don't try toocreate this VM in hello same subnet as hello ASE because it will fail or cause problems.</span></span>
    >
    >

6. <span data-ttu-id="4e88b-223">A ASE tartomány DNS hello beállítása.</span><span class="sxs-lookup"><span data-stu-id="4e88b-223">Set hello DNS for your ASE domain.</span></span> <span data-ttu-id="4e88b-224">A tartomány a DNS-ben helyettesítő karakter használható.</span><span class="sxs-lookup"><span data-stu-id="4e88b-224">You can use a wildcard with your domain in your DNS.</span></span> <span data-ttu-id="4e88b-225">toodo néhány egyszerű teszteli, hello hosts fájlt a virtuális gép tooset hello web app name toohello VIP IP-cím szerkesztése:</span><span class="sxs-lookup"><span data-stu-id="4e88b-225">toodo some simple tests, edit hello hosts file on your VM tooset hello web app name toohello VIP IP address:</span></span>

    <span data-ttu-id="4e88b-226">a.</span><span class="sxs-lookup"><span data-stu-id="4e88b-226">a.</span></span> <span data-ttu-id="4e88b-227">Ha a ASE hello tartomány neve _. ilbase.com_ nevű hello-webalkalmazás létrehozása és _mytestapp_, a címzett _mytestapp.ilbase.com_. Ezután _mytestapp.ilbase.com_ tooresolve toohello ILB cím.</span><span class="sxs-lookup"><span data-stu-id="4e88b-227">If your ASE has hello domain name _.ilbase.com_ and you create hello web app named _mytestapp_, it's addressed at _mytestapp.ilbase.com_. You then set _mytestapp.ilbase.com_ tooresolve toohello ILB address.</span></span> <span data-ttu-id="4e88b-228">(A Windows hello hosts fájl jelenleg _C:\Windows\System32\drivers\etc\_.)</span><span class="sxs-lookup"><span data-stu-id="4e88b-228">(On Windows, hello hosts file is at _C:\Windows\System32\drivers\etc\_.)</span></span>

    <span data-ttu-id="4e88b-229">b.</span><span class="sxs-lookup"><span data-stu-id="4e88b-229">b.</span></span> <span data-ttu-id="4e88b-230">tootest webalkalmazás telepítési közzététel vagy a hozzáférés toohello konzol, speciális rekord létrehozása _mytestapp.scm.ilbase.com_.</span><span class="sxs-lookup"><span data-stu-id="4e88b-230">tootest web deployment publishing or access toohello advanced console, create a record for _mytestapp.scm.ilbase.com_.</span></span>

7. <span data-ttu-id="4e88b-231">Használja ezt a virtuális Gépet egy böngészőt, és navigáljon a http://mytestapp.ilbase.com. (Vagy a tartományban van a webes alkalmazás neve toowhatever.)</span><span class="sxs-lookup"><span data-stu-id="4e88b-231">Use a browser on that VM and go to http://mytestapp.ilbase.com. (Or go toowhatever your web app name is with your domain.)</span></span>

8. <span data-ttu-id="4e88b-232">Használja ezt a virtuális Gépet egy böngészőt, és navigáljon a https://mytestapp.ilbase.com. Ha egy önaláírt tanúsítványt használ, fogadja el a biztonsági hello hiánya.</span><span class="sxs-lookup"><span data-stu-id="4e88b-232">Use a browser on that VM and go to https://mytestapp.ilbase.com. If you use a self-signed certificate, accept hello lack of security.</span></span>

    <span data-ttu-id="4e88b-233">hello IP-címet a ILB megtalálható-e **IP-címek**.</span><span class="sxs-lookup"><span data-stu-id="4e88b-233">hello IP address for your ILB is listed under **IP addresses**.</span></span> <span data-ttu-id="4e88b-234">Ez a lista a hello IP-címek hello külső VIP és bejövő felügyeleti adatforgalomhoz használt is rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="4e88b-234">This list also has hello IP addresses used by hello external VIP and for inbound management traffic.</span></span>

    ![ILB IP-cím][5]

### <a name="web-jobs-functions-and-hello-ilb-ase"></a><span data-ttu-id="4e88b-236">Webalkalmazás-feladatok, funkciók és ILB ASE hello</span><span class="sxs-lookup"><span data-stu-id="4e88b-236">Web jobs, Functions and hello ILB ASE</span></span>

<span data-ttu-id="4e88b-237">Egy ILB ASE funkciók és a webes feladatok is támogatottak, de hello portál toowork velük, rendelkeznie kell hálózati hozzáférési toohello SCM helyet.</span><span class="sxs-lookup"><span data-stu-id="4e88b-237">Both Functions and web jobs are supported on an ILB ASE but for hello portal toowork with them, you must have network access toohello SCM site.</span></span>  <span data-ttu-id="4e88b-238">Ez azt jelenti, hogy a böngészőt kell lennie egy gazdagépen, vagy vagy toohello virtuális hálózathoz csatlakoztatva.</span><span class="sxs-lookup"><span data-stu-id="4e88b-238">This means your browser must either be on a host that is either in or connected toohello virtual network.</span></span>  

<span data-ttu-id="4e88b-239">Ha az Azure Functions egy ILB ASE használja, akkor előfordulhat, hogy hiba jelenik, amely szerint a "azt válnak a függvények jelenleg képes tooretrieve.</span><span class="sxs-lookup"><span data-stu-id="4e88b-239">When you use Azure Functions on an ILB ASE, you might get an error message that says "We are not able tooretrieve your functions right now.</span></span> <span data-ttu-id="4e88b-240">Próbálkozzon újra később."</span><span class="sxs-lookup"><span data-stu-id="4e88b-240">Please try again later."</span></span> <span data-ttu-id="4e88b-241">Ez a hiba akkor fordul elő, mert hello funkciók felhasználói felület hello SCM webhely használja a HTTPS-KAPCSOLATON keresztül, és hello legfelső szintű tanúsítvány nem hello böngésző lánc megbízhatósági.</span><span class="sxs-lookup"><span data-stu-id="4e88b-241">This error occurs because hello Functions UI leverages hello SCM site over HTTPS and hello root certificate is not in hello browser chain of trust.</span></span> <span data-ttu-id="4e88b-242">Webes feladatok hasonló problémát észlelt.</span><span class="sxs-lookup"><span data-stu-id="4e88b-242">Web jobs has a similar problem.</span></span> <span data-ttu-id="4e88b-243">tooavoid probléma hello valamelyikét teheti:</span><span class="sxs-lookup"><span data-stu-id="4e88b-243">tooavoid this problem you can do one of hello following:</span></span>

- <span data-ttu-id="4e88b-244">Adja hozzá a hello tanúsítvány tooyour megbízható tanúsítványok tárolójába.</span><span class="sxs-lookup"><span data-stu-id="4e88b-244">Add hello certificate tooyour trusted certificate store.</span></span> <span data-ttu-id="4e88b-245">Ez feloldja széle és az Internet Explorerben.</span><span class="sxs-lookup"><span data-stu-id="4e88b-245">This unblocks Edge and Internet Explorer.</span></span>
- <span data-ttu-id="4e88b-246">Használja a Chrome és toohello SCM hely először lépnek, hello nem megbízható tanúsítvány elfogadásához, és folytassa a toohello portálon.</span><span class="sxs-lookup"><span data-stu-id="4e88b-246">Use Chrome and go toohello SCM site first, accept hello untrusted certificate and then go toohello portal.</span></span>
- <span data-ttu-id="4e88b-247">A böngésző megbízhatósági lánc a kereskedelmi tanúsítványt használjon.</span><span class="sxs-lookup"><span data-stu-id="4e88b-247">Use a commercial certificate that is in your browser chain of trust.</span></span>  <span data-ttu-id="4e88b-248">Ez a lehetőség hello ajánlott.</span><span class="sxs-lookup"><span data-stu-id="4e88b-248">This is hello best option.</span></span>  

## <a name="dns-configuration"></a><span data-ttu-id="4e88b-249">DNS-konfiguráció</span><span class="sxs-lookup"><span data-stu-id="4e88b-249">DNS configuration</span></span> ##

<span data-ttu-id="4e88b-250">Egy külső VIP használatakor hello DNS Azure kezeli.</span><span class="sxs-lookup"><span data-stu-id="4e88b-250">When you use an External VIP, hello DNS is managed by Azure.</span></span> <span data-ttu-id="4e88b-251">Bármely alkalmazás a ASE létrehozott automatikusan hozzáadása tooAzure DNS, amely egy nyilvános DNS-ben.</span><span class="sxs-lookup"><span data-stu-id="4e88b-251">Any app created in your ASE is automatically added tooAzure DNS, which is a public DNS.</span></span> <span data-ttu-id="4e88b-252">A saját DNS-Példánynak környezetben kell kezelni.</span><span class="sxs-lookup"><span data-stu-id="4e88b-252">In an ILB ASE, you must manage your own DNS.</span></span> <span data-ttu-id="4e88b-253">Például egy adott tartomány _contoso.net_, a DNS-ben rögzíti DNS A pont tooyour ILB címet toocreate van szüksége:</span><span class="sxs-lookup"><span data-stu-id="4e88b-253">For a given domain such as _contoso.net_, you need toocreate DNS A records in your DNS that point tooyour ILB address for:</span></span>

- <span data-ttu-id="4e88b-254">*. contoso.net</span><span class="sxs-lookup"><span data-stu-id="4e88b-254">*.contoso.net</span></span>
- <span data-ttu-id="4e88b-255">*. scm.contoso.net</span><span class="sxs-lookup"><span data-stu-id="4e88b-255">*.scm.contoso.net</span></span>

<span data-ttu-id="4e88b-256">Ha a ILB ASE tartományt a ASE kívül több dolgot, szükség lehet a DNS toomanage /-alkalmazás-neve alapján.</span><span class="sxs-lookup"><span data-stu-id="4e88b-256">If your ILB ASE domain is used for multiple things outside this ASE, you might need toomanage DNS on a per-app-name basis.</span></span> <span data-ttu-id="4e88b-257">Ez a módszer van kihívást, mert kell tooadd minden új alkalmazás neve a DNS-be történő létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="4e88b-257">This method is challenging because you need tooadd each new app name into your DNS when you create it.</span></span> <span data-ttu-id="4e88b-258">Ezért azt javasoljuk, hogy dedikált tartományt használ.</span><span class="sxs-lookup"><span data-stu-id="4e88b-258">For this reason, we recommend that you use a dedicated domain.</span></span>

## <a name="publish-with-an-ilb-ase"></a><span data-ttu-id="4e88b-259">Egy ILB mértékéig közzététele</span><span class="sxs-lookup"><span data-stu-id="4e88b-259">Publish with an ILB ASE</span></span> ##

<span data-ttu-id="4e88b-260">Minden alkalmazást, amely jön létre nincsenek két végpontot.</span><span class="sxs-lookup"><span data-stu-id="4e88b-260">For every app that's created, there are two endpoints.</span></span> <span data-ttu-id="4e88b-261">-Példánynak környezetben van  *&lt;alkalmazás neve >.&lt; ILB ASE tartományi >* és  *&lt;alkalmazás neve > .scm.&lt; ILB ASE tartományi >*.</span><span class="sxs-lookup"><span data-stu-id="4e88b-261">In an ILB ASE, you have *&lt;app name>.&lt;ILB ASE Domain>* and *&lt;app name>.scm.&lt;ILB ASE Domain>*.</span></span> 

<span data-ttu-id="4e88b-262">hello SCM helynév viszi toohello Kudu konzol hello nevű **speciális portal**, melyhez hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="4e88b-262">hello SCM site name takes you toohello Kudu console, called hello **Advanced portal**, within hello Azure portal.</span></span> <span data-ttu-id="4e88b-263">hello Kudu konzol segítségével megtekintheti a környezeti változók, hello lemez vizsgálatát, használja a konzolon, és még sok más.</span><span class="sxs-lookup"><span data-stu-id="4e88b-263">hello Kudu console lets you view environment variables, explore hello disk, use a console, and much more.</span></span> <span data-ttu-id="4e88b-264">További információkért lásd: [Kudu konzol az Azure App Service-][Kudu].</span><span class="sxs-lookup"><span data-stu-id="4e88b-264">For more information, see [Kudu console for Azure App Service][Kudu].</span></span> 

<span data-ttu-id="4e88b-265">Hello több-bérlős App Service és a külső Service-környezetben van az egyszeri bejelentkezés hello Azure-portálon és hello Kudu konzol között.</span><span class="sxs-lookup"><span data-stu-id="4e88b-265">In hello multitenant App Service and in an External ASE, there's single sign-on between hello Azure portal and hello Kudu console.</span></span> <span data-ttu-id="4e88b-266">A hello ILB ASE azonban kell toouse a közzétételi hitelesítő adatok toosign hello Kudu konzolba.</span><span class="sxs-lookup"><span data-stu-id="4e88b-266">For hello ILB ASE, however, you need toouse your publishing credentials toosign into hello Kudu console.</span></span>

<span data-ttu-id="4e88b-267">Internet alapú CI rendszereknek, például a Githubon és a Visual Studio Team Services, egy ILB mértékéig nem működnek, mert hello közzétételi végpont nem érhető el az internet.</span><span class="sxs-lookup"><span data-stu-id="4e88b-267">Internet-based CI systems, such as GitHub and Visual Studio Team Services, don't work with an ILB ASE because hello publishing endpoint isn't internet accessible.</span></span> <span data-ttu-id="4e88b-268">Ehelyett egy CI rendszer, amely lekéréses modellt használ, például a Dropbox toouse van szüksége.</span><span class="sxs-lookup"><span data-stu-id="4e88b-268">Instead, you need toouse a CI system that uses a pull model, such as Dropbox.</span></span>

<span data-ttu-id="4e88b-269">hello közzétételi végpontok-Példánynak környezetben alkalmazások használata hello tartomány adott hello ILB ASE hozták létre.</span><span class="sxs-lookup"><span data-stu-id="4e88b-269">hello publishing endpoints for apps in an ILB ASE use hello domain that hello ILB ASE was created with.</span></span> <span data-ttu-id="4e88b-270">Ez a tartomány hello alkalmazás közzétételi profil és a hello alkalmazás portálpaneljéhez jelenik meg (**áttekintése** > **Essentials** és is **tulajdonságok**).</span><span class="sxs-lookup"><span data-stu-id="4e88b-270">This domain appears in hello app's publishing profile and in hello app's portal blade (**Overview** > **Essentials** and also **Properties**).</span></span> <span data-ttu-id="4e88b-271">Ha egy ILB ASE a hello altartomány *contoso.net* és az alkalmazás neve *mytest*, használjon *mytest.contoso.net* az FTP és *mytest.scm.contoso.net*  központi telepítésére.</span><span class="sxs-lookup"><span data-stu-id="4e88b-271">If you have an ILB ASE with hello subdomain *contoso.net* and an app named *mytest*, use *mytest.contoso.net* for FTP and *mytest.scm.contoso.net* for web deployment.</span></span>

## <a name="couple-an-ilb-ase-with-a-waf-device"></a><span data-ttu-id="4e88b-272">Egy ILB ASE gömbcsatlakozók WAF-eszközzel rendelkező</span><span class="sxs-lookup"><span data-stu-id="4e88b-272">Couple an ILB ASE with a WAF device</span></span> ##

<span data-ttu-id="4e88b-273">Az Azure App Service számos biztonsági intézkedéseket hello rendszer védelmét biztosítja.</span><span class="sxs-lookup"><span data-stu-id="4e88b-273">Azure App Service provides many security measures that protect hello system.</span></span> <span data-ttu-id="4e88b-274">Hogy egy alkalmazás lett megtámadott is segítenek toodetermine.</span><span class="sxs-lookup"><span data-stu-id="4e88b-274">They also help toodetermine whether an app was hacked.</span></span> <span data-ttu-id="4e88b-275">hello legjobb védelmet webalkalmazás toocouple egy üzemeltetési platformtól, például az Azure App Service-ben a webalkalmazási tűzfal (waf-ot).</span><span class="sxs-lookup"><span data-stu-id="4e88b-275">hello best protection for a web application is toocouple a hosting platform, such as Azure App Service, with a web application firewall (WAF).</span></span> <span data-ttu-id="4e88b-276">Mivel hello ILB ASE egy elszigetelt hálózati alkalmazás végponttal rendelkezik, célszerű a használatra.</span><span class="sxs-lookup"><span data-stu-id="4e88b-276">Because hello ILB ASE has a network-isolated application endpoint, it's appropriate for such a use.</span></span>

<span data-ttu-id="4e88b-277">További információk hogyan tooconfigure a ILB ASE WAF-eszközzel rendelkező: toolearn [webalkalmazási tűzfal konfigurálása az App Service-környezet][ASEWAF].</span><span class="sxs-lookup"><span data-stu-id="4e88b-277">toolearn more about how tooconfigure your ILB ASE with a WAF device, see [Configure a web application firewall with your App Service environment][ASEWAF].</span></span> <span data-ttu-id="4e88b-278">Ez a cikk bemutatja, hogyan toouse egy Barracuda virtuális készülék a mértékéig.</span><span class="sxs-lookup"><span data-stu-id="4e88b-278">This article shows how toouse a Barracuda virtual appliance with your ASE.</span></span> <span data-ttu-id="4e88b-279">Másik lehetőség is toouse Azure Application Gateway.</span><span class="sxs-lookup"><span data-stu-id="4e88b-279">Another option is toouse Azure Application Gateway.</span></span> <span data-ttu-id="4e88b-280">Alkalmazásátjáró hello OWASP core szabályok toosecure alkalmazások mögé azt használja.</span><span class="sxs-lookup"><span data-stu-id="4e88b-280">Application Gateway uses hello OWASP core rules toosecure any applications placed behind it.</span></span> <span data-ttu-id="4e88b-281">Alkalmazásátjáró kapcsolatos további információkért lásd: [bemutatása toohello Azure webalkalmazási tűzfal][AppGW].</span><span class="sxs-lookup"><span data-stu-id="4e88b-281">For more information about Application Gateway, see [Introduction toohello Azure web application firewall][AppGW].</span></span>

## <a name="get-started"></a><span data-ttu-id="4e88b-282">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="4e88b-282">Get started</span></span> ##

<span data-ttu-id="4e88b-283">Minden cikkeket, és hogyan-tooinstructions ASEs a érhetők el a [App Service-környezetek – fontos fájl][ASEReadme].</span><span class="sxs-lookup"><span data-stu-id="4e88b-283">All articles and how-tooinstructions for ASEs are available in the [README for App Service environments][ASEReadme].</span></span>

* <span data-ttu-id="4e88b-284">tooget használatába ASEs, lásd: [bemutatása tooApp Service-környezetek][Intro].</span><span class="sxs-lookup"><span data-stu-id="4e88b-284">tooget started with ASEs, see [Introduction tooApp Service environments][Intro].</span></span>
* <span data-ttu-id="4e88b-285">Hello Azure App Service platformmal kapcsolatos további információkért lásd: [Azure App Service](http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/).</span><span class="sxs-lookup"><span data-stu-id="4e88b-285">For more information about hello Azure App Service platform, see [Azure App Service](http://azure.microsoft.com/documentation/articles/app-service-value-prop-what-is/).</span></span>
 
<!--Image references-->
[1]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-network.png
[2]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-webapp.png
[3]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate.png
[4]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-certificate2.png
[5]: ./media/creating_and_using_an_internal_load_balancer_with_app_service_environment/createilbase-ipaddresses.png

<!--Links-->
[Intro]: ./intro.md
[MakeExternalASE]: ./create-external-ase.md
[MakeASEfromTemplate]: ./create-from-template.md
[MakeILBASE]: ./create-ilb-ase.md
[ASENetwork]: ./network-info.md
[ASEReadme]: ./readme.md
[UsingASE]: ./using-an-ase.md
[UDRs]: ../../virtual-network/virtual-networks-udr-overview.md
[NSGs]: ../../virtual-network/virtual-networks-nsg.md
[ConfigureASEv1]: ../../app-service-web/app-service-web-configure-an-app-service-environment.md
[ASEv1Intro]: ../../app-service-web/app-service-app-service-environment-intro.md
[webapps]: ../../app-service-web/app-service-web-overview.md
[mobileapps]: ../../app-service-mobile/app-service-mobile-value-prop.md
[APIapps]: ../../app-service-api/app-service-api-apps-why-best-platform.md
[Functions]: ../../azure-functions/index.yml
[Pricing]: http://azure.microsoft.com/pricing/details/app-service/
[ARMOverview]: ../../azure-resource-manager/resource-group-overview.md
[ConfigureSSL]: ../../app-service-web/web-sites-purchase-ssl-web-site.md
[Kudu]: http://azure.microsoft.com/resources/videos/super-secret-kudu-debug-console-for-azure-web-sites/
[AppDeploy]: ../../app-service-web/web-sites-deploy.md
[ASEWAF]: ../../app-service-web/app-service-app-service-environment-web-application-firewall.md
[AppGW]: ../../application-gateway/application-gateway-web-application-firewall-overview.md
