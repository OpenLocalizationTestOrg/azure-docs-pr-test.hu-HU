---
title: "az Azure Security Center átjáró integrációja aaaApplication |} Microsoft Docs"
description: "Ezen a lapon bemutatja, hogyan Alkalmazásátjáró integrálva van-e az Azure Security Center."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: 
ms.assetid: e5ea5cf9-3b41-4b85-a12c-e758bff7f3ec
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.custom: 
ms.workload: infrastructure-services
ms.date: 06/07/2017
ms.author: gwallace
ms.openlocfilehash: 6f6ace105e84c01f525ab02938e81ce040c5c9d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-integration-between-application-gateway-and-azure-security-center"></a><span data-ttu-id="22f61-103">Alkalmazásátjáró és az Azure Security Center közötti integráció áttekintése</span><span class="sxs-lookup"><span data-stu-id="22f61-103">Overview of integration between Application Gateway and Azure Security Center</span></span>

<span data-ttu-id="22f61-104">Ismerje meg, hogyan Application Gateway és a Security Center védelmét a webes alkalmazás erőforrásaihoz.</span><span class="sxs-lookup"><span data-stu-id="22f61-104">Learn how Application Gateway and Security Center help protect your web application resources.</span></span> <span data-ttu-id="22f61-105">Alkalmazás átjáró webalkalmazási tűzfal (WAF) integrálható [Security Center](../security-center/security-center-intro.md) tooprovide egy zökkenőmentes nézet tooprevent észlelése és válasz toothreats toounprotected webalkalmazások a környezetben.</span><span class="sxs-lookup"><span data-stu-id="22f61-105">Application gateway web application firewall (WAF) integrates with [Security Center](../security-center/security-center-intro.md) tooprovide a seamless view tooprevent, detect and respond toothreats toounprotected web applications in your environment.</span></span>

## <a name="overview"></a><span data-ttu-id="22f61-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="22f61-106">Overview</span></span>

<span data-ttu-id="22f61-107">Alkalmazás átjáró WAF várja, hogy egy érték, a Security Center webalkalmazások biztonsági réseket és biztonsági rések elleni védelme.</span><span class="sxs-lookup"><span data-stu-id="22f61-107">Application Gateway WAF is a recommendation in Security Center for protecting web applications from exploits and vulnerabilities.</span></span> <span data-ttu-id="22f61-108">Magas súlyosságú javaslatok, hello a security center engedélyezve van a webes erőforrásokat WAF által nem védett megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="22f61-108">Web enabled resources that are not protected by WAF show in hello security center as high severity recommendations.</span></span> <span data-ttu-id="22f61-109">Webalkalmazási tűzfalak javaslatok látható a hello **áttekintése** lap **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="22f61-109">Recommendations for web application firewalls are shown on hello **Overview** page, under **Applications**.</span></span>

![a security Center Integration][1]

<span data-ttu-id="22f61-111">Webalkalmazási tűzfal vonatkozó javaslatokkal kattintva megnyílik egy új hello hello ajánlás részleteit megjelenítő panelen.</span><span class="sxs-lookup"><span data-stu-id="22f61-111">Clicking any recommendations regarding web application firewall opens a new blade showing hello details of hello recommendation.</span></span>

## <a name="add-a-web-application-firewall-tooan-existing-resource"></a><span data-ttu-id="22f61-112">A webes alkalmazás tűzfal tooan meglévő erőforrás hozzáadása</span><span class="sxs-lookup"><span data-stu-id="22f61-112">Add a web application firewall tooan existing resource</span></span>

<span data-ttu-id="22f61-113">Keresse meg a túl**több szolgáltatások** > **biztonság + identitás szakaszában** > **Security Center** a hello **Security Center – áttekintés**  panelen kattintson a **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="22f61-113">Navigate too**More Services** > **Security + Identity** > **Security Center** and on hello **Security Center - Overview** blade, click **Applications**.</span></span> <span data-ttu-id="22f61-114">A hello **Security Center - alkalmazások** panelen hello tábla Security Center az előfizetésében észlelt alkalmazások listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="22f61-114">On hello **Security Center - Applications** blade, hello table contains a list of applications that Security Center detected in your subscription.</span></span>

![webalkalmazások][3]

<span data-ttu-id="22f61-116">Egy kritikus problémát webalkalmazás parancsával hello beolvasása **biztonsági állapotának** panelen.</span><span class="sxs-lookup"><span data-stu-id="22f61-116">By clicking on a web application with a critical issue, you get hello **Application security health** blade.</span></span> <span data-ttu-id="22f61-117">Az alábbi hello kép webalkalmazás, amelyet nem véd a webalkalmazási tűzfal hello.</span><span class="sxs-lookup"><span data-stu-id="22f61-117">In hello image below, hello web application that is not protected by a web application firewall.</span></span> 

![nem védett webes erőforrások][2]

<span data-ttu-id="22f61-119">Kattintson a **webalkalmazási tűzfal hozzáadása** alatt **javaslatok** tooopen hello **webalkalmazási tűzfal hozzáadása** panelen.</span><span class="sxs-lookup"><span data-stu-id="22f61-119">Click **Add a web application firewall** under **Recommendations** tooopen hello **Add a Web Application Firewall** blade.</span></span>

<span data-ttu-id="22f61-120">Ha nem meglévő Alkalmazásátjáró, esetleg toocreate egy új, kattintson a **hozzon létre új** a hello **hozzon létre egy új webalkalmazási tűzfal** panelen, majd kattintson **Microsoft - Alkalmazásátjáró**.</span><span class="sxs-lookup"><span data-stu-id="22f61-120">If you do not have an existing Application Gateway, or want toocreate a new one, click **Create New** and on hello **Create a new Web Application Firewall** blade, and click **Microsoft - Application Gateway**.</span></span> <span data-ttu-id="22f61-121">Ez végigvezeti hello lépéseket toocreate Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="22f61-121">This takes you through hello steps toocreate an application gateway.</span></span> <span data-ttu-id="22f61-122">Ezen a ponton a webes alkalmazás meg van adva egy védett erőforrásokhoz, most már a Security Center követi nyomon, hogy ehhez az erőforráshoz webalkalmazási tűzfal védi.</span><span class="sxs-lookup"><span data-stu-id="22f61-122">At this point, your web application is added as a protected resource, Security Center now tracks that this resource is protected by a web application firewall.</span></span> <span data-ttu-id="22f61-123">Ez nem adja hozzá azt egy háttér címkészletet tag.</span><span class="sxs-lookup"><span data-stu-id="22f61-123">This does not add it as a backend pool member.</span></span>

<span data-ttu-id="22f61-124">Ha egy meglévő Alkalmazásátjáró, kiválaszthatja azt a **meglévő megoldással**</span><span class="sxs-lookup"><span data-stu-id="22f61-124">If you have an existing application gateway, you can choose it under **Use existing solution**</span></span>

![webalkalmazási tűzfal hozzáadása panel][4]

<span data-ttu-id="22f61-126">Hozzáadásával egy webes alkalmazás tooan Alkalmazásátjáró Security Center keresztül nem hello erőforrás háttér-készlet tagja, ez a kell elvégezni hello alkalmazás átjáró erőforrás közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="22f61-126">Adding a web application tooan application gateway through Security Center does not add hello resource as a backend pool member, this must be done on hello application gateway resource directly.</span></span>

## <a name="add-a-resource-tooan-existing-web-application-firewall"></a><span data-ttu-id="22f61-127">Egy erőforrás tooan meglévő webalkalmazási tűzfal hozzáadása</span><span class="sxs-lookup"><span data-stu-id="22f61-127">Add a resource tooan existing web application firewall</span></span>

<span data-ttu-id="22f61-128">Keresse meg a túl**több szolgáltatások** > **biztonság + identitás szakaszában** > **Security Center** a hello **Security Center – áttekintés**  panelen kattintson a **partneri megoldások**.</span><span class="sxs-lookup"><span data-stu-id="22f61-128">Navigate too**More Services** > **Security + Identity** > **Security Center** and on hello **Security Center - Overview** blade, click **Partner solutions**.</span></span> <span data-ttu-id="22f61-129">Meglévő Security Center felismerésre képes alkalmazást átjárók megjelenítése a hello **partneri megoldások** panelen.</span><span class="sxs-lookup"><span data-stu-id="22f61-129">Existing Security Center aware application gateways show in hello **Partner Solutions** blade.</span></span>

![Partneri megoldások][7]

<span data-ttu-id="22f61-131">Kattintson a **összekapcsolás** tooopen hello **összekapcsolás** panelen Itt lehetősége van hello beállítások tooselect a meglévő alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="22f61-131">Click **Link app** tooopen hello **Link Applications** blade, here you are given hello options tooselect existing applications.</span></span> <span data-ttu-id="22f61-132">Hello alkalmazások tooprotect válasszon, majd kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="22f61-132">Choose hello applications tooprotect and click **OK**.</span></span> <span data-ttu-id="22f61-133">Ez nem bővíti ki hello webes alkalmazás toohello háttérkészlet hello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="22f61-133">This does not add hello web application toohello backend pool of hello application gateway.</span></span> <span data-ttu-id="22f61-134">Ez beállítja hello erőforrások védett erőforrásként, a Security Center nyomon követheti.</span><span class="sxs-lookup"><span data-stu-id="22f61-134">This sets hello resources as a protected resource so Security Center can track it.</span></span> <span data-ttu-id="22f61-135">tooadd hello erőforrás háttér-készlet tagja, ez a hello Alkalmazásátjáró kell elvégezni, hello aktuális paneljén kattintson **megoldáskonzol** toobe végrehajtott toohello alkalmazás átjáró erőforrás hello webes adhat hozzá alkalmazás toohello háttérkészlet.</span><span class="sxs-lookup"><span data-stu-id="22f61-135">tooadd hello resource as a backend pool member, this must be done on hello application gateway, from hello current blade you can click **Solution console** toobe taken toohello application gateway resource where you can add hello web application toohello backend pool.</span></span>

![Partneri megoldások alkalmazások][6]

## <a name="finalize-configuration"></a><span data-ttu-id="22f61-137">Konfigurálás lezárásaképpen</span><span class="sxs-lookup"><span data-stu-id="22f61-137">Finalize configuration</span></span>

<span data-ttu-id="22f61-138">A Security Center számok alkalmazások tooan Alkalmazásátjáró fel, mert egy védett erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="22f61-138">Security Center tracks applications added tooan application gateway as a protected resource.</span></span>  <span data-ttu-id="22f61-139">Ehhez az erőforráshoz hello állapotát figyeli, és biztosítja, hogy egy alkalmazás-átjáró által védett.</span><span class="sxs-lookup"><span data-stu-id="22f61-139">It monitors hello health of this resource and ensures that it is protected by an application gateway.</span></span> <span data-ttu-id="22f61-140">következő lépés hello tooadd hello magánhálózati IP-címe, nyilvános IP-cím vagy a virtuális gép toohello háttérkészlet hello Alkalmazásátjáró hálózati Adapterének.</span><span class="sxs-lookup"><span data-stu-id="22f61-140">hello next step is tooadd hello private IP, public IP, or NIC of your virtual machine toohello backend pool of hello application gateway.</span></span> <span data-ttu-id="22f61-141">Amíg ez történik, egy további ajánlása **alkalmazás védelem véglegesítése** látható, amíg nincs hozzáadva a hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="22f61-141">Until this is done an additional recommendation of **Finalize application protection** is shown until hello resource is added.</span></span>

![webalkalmazási tűzfal hozzáadása panel][5]

## <a name="security-alerts"></a><span data-ttu-id="22f61-143">Biztonsági riasztások</span><span class="sxs-lookup"><span data-stu-id="22f61-143">Security Alerts</span></span>

<span data-ttu-id="22f61-144">A Security Center Ugrás túl**ÉSZLELÉSI** > **biztonsági riasztások**.</span><span class="sxs-lookup"><span data-stu-id="22f61-144">Within Security Center navigate too**DETECTION** > **Security Alerts**.</span></span>  <span data-ttu-id="22f61-145">Itt megtalálja az Ön WAF riasztások az alkalmazás átjárók.</span><span class="sxs-lookup"><span data-stu-id="22f61-145">Here you find WAF alerts for your application gateways.</span></span> <span data-ttu-id="22f61-146">Riasztások WAF szabály bontásban.</span><span class="sxs-lookup"><span data-stu-id="22f61-146">Alerts are broken down by WAF rule.</span></span>

![biztonsági riasztások][8]

<span data-ttu-id="22f61-148">Kattintson egy szabály nyújtják adott WAF szabályhoz tartozó riasztások listája.</span><span class="sxs-lookup"><span data-stu-id="22f61-148">Clicking an rule will provide a list of alerts for that specific WAF rule.</span></span> <span data-ttu-id="22f61-149">Minden riasztás további részleteket hello megállapítás mutatja be.</span><span class="sxs-lookup"><span data-stu-id="22f61-149">Each alert shows additional details on hello finding.</span></span> <span data-ttu-id="22f61-150">hello részletek adjon meg egy hivatkozást toohello Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="22f61-150">hello details provide a link toohello application gateway.</span></span>
 
![riasztás részletei][9]

## <a name="next-steps"></a><span data-ttu-id="22f61-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="22f61-152">Next steps</span></span>

<span data-ttu-id="22f61-153">Hogyan tooenable webalkalmazási tűzfal egy meglévő alkalmazás átjárón, látogasson el toolearn [létrehozás vagy frissítés webalkalmazási tűzfal az Azure-Alkalmazásátjáró](application-gateway-web-application-firewall-portal.md#add-web-application-firewall-to-an-existing-application-gateway)</span><span class="sxs-lookup"><span data-stu-id="22f61-153">toolearn how tooenable web application firewall on an existing application gateway, visit [Create or update an Azure Application Gateway with web application firewall](application-gateway-web-application-firewall-portal.md#add-web-application-firewall-to-an-existing-application-gateway)</span></span>

[1]: ./media/application-gateway-integration-security-center/figure1.png
[2]: ./media/application-gateway-integration-security-center/figure2.png
[3]: ./media/application-gateway-integration-security-center/figure3.png
[4]: ./media/application-gateway-integration-security-center/figure4.png
[5]: ./media/application-gateway-integration-security-center/figure5.png
[6]: ./media/application-gateway-integration-security-center/figure6.png
[7]: ./media/application-gateway-integration-security-center/figure7.png
[8]: ./media/application-gateway-integration-security-center/securitycenter.png
[9]: ./media/application-gateway-integration-security-center/figure9.png