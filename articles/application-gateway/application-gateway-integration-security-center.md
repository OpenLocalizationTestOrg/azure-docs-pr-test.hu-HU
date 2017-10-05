---
title: "Alkalmazások átjáró integrálása az Azure Security Center |} Microsoft Docs"
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
ms.openlocfilehash: 737cdff3140be68cf9d6d396b470dd09c65c52f2
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="overview-of-integration-between-application-gateway-and-azure-security-center"></a><span data-ttu-id="86eff-103">Alkalmazásátjáró és az Azure Security Center közötti integráció áttekintése</span><span class="sxs-lookup"><span data-stu-id="86eff-103">Overview of integration between Application Gateway and Azure Security Center</span></span>

<span data-ttu-id="86eff-104">Ismerje meg, hogyan Application Gateway és a Security Center védelmét a webes alkalmazás erőforrásaihoz.</span><span class="sxs-lookup"><span data-stu-id="86eff-104">Learn how Application Gateway and Security Center help protect your web application resources.</span></span> <span data-ttu-id="86eff-105">Alkalmazás átjáró webalkalmazási tűzfal (WAF) integrálható [Security Center](../security-center/security-center-intro.md) megjelenítésére szolgáló zökkenőmentes megelőzése, észlelése és a környezetben nem védett webalkalmazások veszélyek válaszolni.</span><span class="sxs-lookup"><span data-stu-id="86eff-105">Application gateway web application firewall (WAF) integrates with [Security Center](../security-center/security-center-intro.md) to provide a seamless view to prevent, detect and respond to threats to unprotected web applications in your environment.</span></span>

## <a name="overview"></a><span data-ttu-id="86eff-106">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="86eff-106">Overview</span></span>

<span data-ttu-id="86eff-107">Alkalmazás átjáró WAF várja, hogy egy érték, a Security Center webalkalmazások biztonsági réseket és biztonsági rések elleni védelme.</span><span class="sxs-lookup"><span data-stu-id="86eff-107">Application Gateway WAF is a recommendation in Security Center for protecting web applications from exploits and vulnerabilities.</span></span> <span data-ttu-id="86eff-108">Magas súlyosságú javaslatok, a security center engedélyezve van a webes erőforrásokat WAF által nem védett megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="86eff-108">Web enabled resources that are not protected by WAF show in the security center as high severity recommendations.</span></span> <span data-ttu-id="86eff-109">Webalkalmazási tűzfalak javaslatok jelenjenek meg a **áttekintése** lap **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="86eff-109">Recommendations for web application firewalls are shown on the **Overview** page, under **Applications**.</span></span>

![a security Center Integration][1]

<span data-ttu-id="86eff-111">Kattintson a javaslatokkal kapcsolatos webalkalmazási tűzfal megnyílik egy új, az ajánlás részleteit megjelenítő panelen.</span><span class="sxs-lookup"><span data-stu-id="86eff-111">Clicking any recommendations regarding web application firewall opens a new blade showing the details of the recommendation.</span></span>

## <a name="add-a-web-application-firewall-to-an-existing-resource"></a><span data-ttu-id="86eff-112">Webalkalmazási tűzfal hozzáadása a meglévő erőforrás</span><span class="sxs-lookup"><span data-stu-id="86eff-112">Add a web application firewall to an existing resource</span></span>

<span data-ttu-id="86eff-113">Navigáljon a **több szolgáltatások** > **biztonság + identitás szakaszában** > **Security Center** és az a **Security Center – áttekintés** panelen kattintson a **alkalmazások**.</span><span class="sxs-lookup"><span data-stu-id="86eff-113">Navigate to **More Services** > **Security + Identity** > **Security Center** and on the **Security Center - Overview** blade, click **Applications**.</span></span> <span data-ttu-id="86eff-114">Az a **Security Center - alkalmazások** panelen, a tábla a Security Center az előfizetésében észlelt alkalmazások listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="86eff-114">On the **Security Center - Applications** blade, the table contains a list of applications that Security Center detected in your subscription.</span></span>

![webalkalmazások][3]

<span data-ttu-id="86eff-116">Kattintva kritikus problémát webalkalmazás, azzal a **biztonsági állapotának** panelen.</span><span class="sxs-lookup"><span data-stu-id="86eff-116">By clicking on a web application with a critical issue, you get the **Application security health** blade.</span></span> <span data-ttu-id="86eff-117">Az alábbi képen, a webes alkalmazás webalkalmazási tűzfal által nem védett.</span><span class="sxs-lookup"><span data-stu-id="86eff-117">In the image below, the web application that is not protected by a web application firewall.</span></span> 

![nem védett webes erőforrások][2]

<span data-ttu-id="86eff-119">Kattintson a **webalkalmazási tűzfal hozzáadása** alatt **javaslatok** megnyitásához a **webalkalmazási tűzfal hozzáadása** panelen.</span><span class="sxs-lookup"><span data-stu-id="86eff-119">Click **Add a web application firewall** under **Recommendations** to open the **Add a Web Application Firewall** blade.</span></span>

<span data-ttu-id="86eff-120">Ha nem rendelkezik meglévő Alkalmazásátjáró, vagy hozzon létre egy újat szeretne, kattintson a **hozzon létre új** és az a **hozzon létre egy új webalkalmazási tűzfal** panelen, majd kattintson **Microsoft - Alkalmazásátjáró**.</span><span class="sxs-lookup"><span data-stu-id="86eff-120">If you do not have an existing Application Gateway, or want to create a new one, click **Create New** and on the **Create a new Web Application Firewall** blade, and click **Microsoft - Application Gateway**.</span></span> <span data-ttu-id="86eff-121">Ez végigvezeti Alkalmazásátjáró létrehozásához szükséges lépéseket.</span><span class="sxs-lookup"><span data-stu-id="86eff-121">This takes you through the steps to create an application gateway.</span></span> <span data-ttu-id="86eff-122">Ezen a ponton a webes alkalmazás meg van adva egy védett erőforrásokhoz, most már a Security Center követi nyomon, hogy ehhez az erőforráshoz webalkalmazási tűzfal védi.</span><span class="sxs-lookup"><span data-stu-id="86eff-122">At this point, your web application is added as a protected resource, Security Center now tracks that this resource is protected by a web application firewall.</span></span> <span data-ttu-id="86eff-123">Ez nem adja hozzá azt egy háttér címkészletet tag.</span><span class="sxs-lookup"><span data-stu-id="86eff-123">This does not add it as a backend pool member.</span></span>

<span data-ttu-id="86eff-124">Ha egy meglévő Alkalmazásátjáró, kiválaszthatja azt a **meglévő megoldással**</span><span class="sxs-lookup"><span data-stu-id="86eff-124">If you have an existing application gateway, you can choose it under **Use existing solution**</span></span>

![webalkalmazási tűzfal hozzáadása panel][4]

<span data-ttu-id="86eff-126">Ad hozzá, a webalkalmazások biztonsági központon keresztül Alkalmazásátjáró nem adja az erőforrást a háttér-készlet tagja, a kell végezni az alkalmazás erőforrás-átjáró közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="86eff-126">Adding a web application to an application gateway through Security Center does not add the resource as a backend pool member, this must be done on the application gateway resource directly.</span></span>

## <a name="add-a-resource-to-an-existing-web-application-firewall"></a><span data-ttu-id="86eff-127">Erőforrás hozzáadása egy meglévő webalkalmazási tűzfal</span><span class="sxs-lookup"><span data-stu-id="86eff-127">Add a resource to an existing web application firewall</span></span>

<span data-ttu-id="86eff-128">Navigáljon a **több szolgáltatások** > **biztonság + identitás szakaszában** > **Security Center** és az a **Security Center – áttekintés** panelen kattintson a **partneri megoldások**.</span><span class="sxs-lookup"><span data-stu-id="86eff-128">Navigate to **More Services** > **Security + Identity** > **Security Center** and on the **Security Center - Overview** blade, click **Partner solutions**.</span></span> <span data-ttu-id="86eff-129">Meglévő Security Center felismerésre képes alkalmazást átjárók megjelenítése a **partneri megoldások** panelen.</span><span class="sxs-lookup"><span data-stu-id="86eff-129">Existing Security Center aware application gateways show in the **Partner Solutions** blade.</span></span>

![Partneri megoldások][7]

<span data-ttu-id="86eff-131">Kattintson a **összekapcsolás** megnyitásához a **összekapcsolás** panelen Itt lehetősége van a beállítások, jelölje be a meglévő alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="86eff-131">Click **Link app** to open the **Link Applications** blade, here you are given the options to select existing applications.</span></span> <span data-ttu-id="86eff-132">Válassza ki azokat a védelmét, majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="86eff-132">Choose the applications to protect and click **OK**.</span></span> <span data-ttu-id="86eff-133">Ez nem bővíti ki a webalkalmazás a háttérkészletbe az Alkalmazásátjáró.</span><span class="sxs-lookup"><span data-stu-id="86eff-133">This does not add the web application to the backend pool of the application gateway.</span></span> <span data-ttu-id="86eff-134">Ez beállítja az erőforrások védett erőforrásként, a Security Center nyomon követheti.</span><span class="sxs-lookup"><span data-stu-id="86eff-134">This sets the resources as a protected resource so Security Center can track it.</span></span> <span data-ttu-id="86eff-135">Az erőforrás hozzáadása a háttér-készlet tagja, ez a kell elvégezni a jelenlegi paneljén kattintson az Alkalmazásátjáró **megoldáskonzol** kell fordítani az átjáró-erőforráshoz a webes alkalmazás háttérkészlet adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="86eff-135">To add the resource as a backend pool member, this must be done on the application gateway, from the current blade you can click **Solution console** to be taken to the application gateway resource where you can add the web application to the backend pool.</span></span>

![Partneri megoldások alkalmazások][6]

## <a name="finalize-configuration"></a><span data-ttu-id="86eff-137">Konfigurálás lezárásaképpen</span><span class="sxs-lookup"><span data-stu-id="86eff-137">Finalize configuration</span></span>

<span data-ttu-id="86eff-138">A Security Center nyomon követi az alkalmazások Alkalmazásátjáró védett erőforrásként hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="86eff-138">Security Center tracks applications added to an application gateway as a protected resource.</span></span>  <span data-ttu-id="86eff-139">Ez az erőforrás állapotát figyeli, és biztosítja, hogy egy alkalmazás-átjáró által védett.</span><span class="sxs-lookup"><span data-stu-id="86eff-139">It monitors the health of this resource and ensures that it is protected by an application gateway.</span></span> <span data-ttu-id="86eff-140">A következő lépés a magánhálózati IP-címe, nyilvános IP-cím vagy a virtuális gép hálózati Adapterének hozzáadása az Alkalmazásátjáró háttérkészlet.</span><span class="sxs-lookup"><span data-stu-id="86eff-140">The next step is to add the private IP, public IP, or NIC of your virtual machine to the backend pool of the application gateway.</span></span> <span data-ttu-id="86eff-141">Amíg ez történik, egy további ajánlása **alkalmazás védelem véglegesítése** látható, amíg az erőforrás nincs hozzáadva.</span><span class="sxs-lookup"><span data-stu-id="86eff-141">Until this is done an additional recommendation of **Finalize application protection** is shown until the resource is added.</span></span>

![webalkalmazási tűzfal hozzáadása panel][5]

## <a name="security-alerts"></a><span data-ttu-id="86eff-143">Biztonsági riasztások</span><span class="sxs-lookup"><span data-stu-id="86eff-143">Security Alerts</span></span>

<span data-ttu-id="86eff-144">A Security Center Ugrás **ÉSZLELÉSI** > **biztonsági riasztások**.</span><span class="sxs-lookup"><span data-stu-id="86eff-144">Within Security Center navigate to **DETECTION** > **Security Alerts**.</span></span>  <span data-ttu-id="86eff-145">Itt megtalálja az Ön WAF riasztások az alkalmazás átjárók.</span><span class="sxs-lookup"><span data-stu-id="86eff-145">Here you find WAF alerts for your application gateways.</span></span> <span data-ttu-id="86eff-146">Riasztások WAF szabály bontásban.</span><span class="sxs-lookup"><span data-stu-id="86eff-146">Alerts are broken down by WAF rule.</span></span>

![biztonsági riasztások][8]

<span data-ttu-id="86eff-148">Kattintson egy szabály nyújtják adott WAF szabályhoz tartozó riasztások listája.</span><span class="sxs-lookup"><span data-stu-id="86eff-148">Clicking an rule will provide a list of alerts for that specific WAF rule.</span></span> <span data-ttu-id="86eff-149">Minden riasztás további részleteket a megállapítás mutatja be.</span><span class="sxs-lookup"><span data-stu-id="86eff-149">Each alert shows additional details on the finding.</span></span> <span data-ttu-id="86eff-150">A részleteket az Alkalmazásátjáró mutató hivatkozás biztosítása.</span><span class="sxs-lookup"><span data-stu-id="86eff-150">The details provide a link to the application gateway.</span></span>
 
![riasztás részletei][9]

## <a name="next-steps"></a><span data-ttu-id="86eff-152">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="86eff-152">Next steps</span></span>

<span data-ttu-id="86eff-153">Ismerje meg, hogy a meglévő Alkalmazásátjáró webalkalmazási tűzfal engedélyezése, látogasson el a [létrehozás vagy frissítés webalkalmazási tűzfal az Azure-Alkalmazásátjáró](application-gateway-web-application-firewall-portal.md#add-web-application-firewall-to-an-existing-application-gateway)</span><span class="sxs-lookup"><span data-stu-id="86eff-153">To learn how to enable web application firewall on an existing application gateway, visit [Create or update an Azure Application Gateway with web application firewall](application-gateway-web-application-firewall-portal.md#add-web-application-firewall-to-an-existing-application-gateway)</span></span>

[1]: ./media/application-gateway-integration-security-center/figure1.png
[2]: ./media/application-gateway-integration-security-center/figure2.png
[3]: ./media/application-gateway-integration-security-center/figure3.png
[4]: ./media/application-gateway-integration-security-center/figure4.png
[5]: ./media/application-gateway-integration-security-center/figure5.png
[6]: ./media/application-gateway-integration-security-center/figure6.png
[7]: ./media/application-gateway-integration-security-center/figure7.png
[8]: ./media/application-gateway-integration-security-center/securitycenter.png
[9]: ./media/application-gateway-integration-security-center/figure9.png