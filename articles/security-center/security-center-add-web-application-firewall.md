---
title: "Webalkalmazási tűzfal hozzáadása az Azure Security Centerben |} Microsoft Docs"
description: "Ez a dokumentum azt ismerteti, hogyan valósítja meg az Azure Security Center javaslatait ** hozzáadása a webes alkalmazás tűzfal ** és ** véglegesítése alkalmazás védelmi **."
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 8f56139a-4466-48ac-90fb-86d002cf8242
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/09/2017
ms.author: terrylan
ms.openlocfilehash: d04a07237029953d8a9b20704d85e852ce45d867
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="add-a-web-application-firewall-in-azure-security-center"></a><span data-ttu-id="e49f7-103">Webalkalmazási tűzfal hozzáadása az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="e49f7-103">Add a web application firewall in Azure Security Center</span></span>
<span data-ttu-id="e49f7-104">Az Azure Security Center javasolhatja, hogy a webes alkalmazás tűzfalat (WAF) hozzáadása egy Microsoft-partner biztonságossá tételéhez a webes alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="e49f7-104">Azure Security Center may recommend that you add a web application firewall (WAF) from a Microsoft partner to secure your web applications.</span></span> <span data-ttu-id="e49f7-105">Ez a dokumentum útmutatást nyújt a példa bemutatja, hogyan Ez a javaslat alkalmazása.</span><span class="sxs-lookup"><span data-stu-id="e49f7-105">This document walks you through an example of how to apply this recommendation.</span></span>

<span data-ttu-id="e49f7-106">WAF ajánlás bármely nyilvános felé néző IP-cím (példány szint IP vagy terhelés eloszlik IP), amely rendelkezik egy társított hálózati biztonsági csoportot a nyissa meg a bejövő webes portokkal (80,443) jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="e49f7-106">A WAF recommendation is shown for any public facing IP (either Instance Level IP or Load Balanced IP) that has an associated network security group with open inbound web ports (80,443).</span></span>

<span data-ttu-id="e49f7-107">A Security Center javasolja, hogy egy WAF a webes alkalmazások, virtuális gépek és az App Service Environment-környezet célzó támadások elleni védelemre való ellátásához.</span><span class="sxs-lookup"><span data-stu-id="e49f7-107">Security Center recommends that you provision a WAF to help defend against attacks targeting your web applications on virtual machines and on App Service Environment.</span></span> <span data-ttu-id="e49f7-108">Az App Service környezetben (ASE) van egy [prémium](https://azure.microsoft.com/pricing/details/app-service/) terv lehetőséget az Azure App Service egy teljesen elkülönített és dedikált környezetben történő biztonságos futtatására az Azure App Service apps biztosító szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="e49f7-108">An App Service Environment (ASE) is a [Premium](https://azure.microsoft.com/pricing/details/app-service/) service plan option of Azure App Service that provides a fully isolated and dedicated environment for securely running Azure App Service apps.</span></span> <span data-ttu-id="e49f7-109">ASE kapcsolatos további tudnivalókért tekintse meg a [App Service környezetben dokumentáció](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="e49f7-109">To learn more about ASE, see the [App Service Environment Documentation](../app-service/app-service-app-service-environments-readme.md).</span></span>

> [!NOTE]
> <span data-ttu-id="e49f7-110">Ez a dokumentum egy üzembe helyezést szemléltető példa segítségével mutatja be a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="e49f7-110">This document introduces the service by using an example deployment.</span></span>  <span data-ttu-id="e49f7-111">Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="e49f7-111">This document is not a step-by-step guide.</span></span>
>
>

## <a name="implement-the-recommendation"></a><span data-ttu-id="e49f7-112">A javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="e49f7-112">Implement the recommendation</span></span>
1. <span data-ttu-id="e49f7-113">Az a **javaslatok** panelen válassza **biztonságossá webalkalmazását webalkalmazási tűzfal használatával**.</span><span class="sxs-lookup"><span data-stu-id="e49f7-113">In the **Recommendations** blade, select **Secure web application using web application firewall**.</span></span>
   <span data-ttu-id="e49f7-114">![Biztonságos webes alkalmazás][1]</span><span class="sxs-lookup"><span data-stu-id="e49f7-114">![Secure web Application][1]</span></span>
2. <span data-ttu-id="e49f7-115">Az a **a webes alkalmazások webalkalmazási tűzfal biztonságos** panelen válassza ki a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e49f7-115">In the **Secure your web applications using web application firewall** blade, select a web application.</span></span> <span data-ttu-id="e49f7-116">A **webalkalmazási tűzfal hozzáadása** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="e49f7-116">The **Add a Web Application Firewall** blade opens.</span></span>
   <span data-ttu-id="e49f7-117">![Webalkalmazási tűzfal hozzáadása][2]</span><span class="sxs-lookup"><span data-stu-id="e49f7-117">![Add a web application firewall][2]</span></span>
3. <span data-ttu-id="e49f7-118">Választhat egy létező webalkalmazási tűzfal használata, ha elérhető, vagy létrehozhat egy újat.</span><span class="sxs-lookup"><span data-stu-id="e49f7-118">You can choose to use an existing web application firewall if available or you can create a new one.</span></span> <span data-ttu-id="e49f7-119">Ebben a példában érhetők el nem létező WAFs, létrehozhatunk egy WAF.</span><span class="sxs-lookup"><span data-stu-id="e49f7-119">In this example, there are no existing WAFs available so we create a WAF.</span></span>
4. <span data-ttu-id="e49f7-120">Egy WAF létrehozásához válassza a megoldás az integrált partnereink listájából.</span><span class="sxs-lookup"><span data-stu-id="e49f7-120">To create a WAF, select a solution from the list of integrated partners.</span></span> <span data-ttu-id="e49f7-121">Ez a példa azt válassza **Barracuda webalkalmazási tűzfal**.</span><span class="sxs-lookup"><span data-stu-id="e49f7-121">In this example, we select **Barracuda Web Application Firewall**.</span></span>
5. <span data-ttu-id="e49f7-122">A **Barracuda webalkalmazási tűzfal** panel nyílik meg, biztosítva a partnermegoldáshoz kapcsolatos információkat.</span><span class="sxs-lookup"><span data-stu-id="e49f7-122">The **Barracuda Web Application Firewall** blade opens providing you information about the partner solution.</span></span> <span data-ttu-id="e49f7-123">Válassza ki **létrehozása** az adatokat a panelen.</span><span class="sxs-lookup"><span data-stu-id="e49f7-123">Select **Create** in the information blade.</span></span>

   ![Tűzfal információk panel][3]

6. <span data-ttu-id="e49f7-125">A **új webalkalmazási tűzfal** panel megnyitása, ahol elvégezhető **Virtuálisgép-konfiguráció** lépéseit, és adja meg **WAF információk**.</span><span class="sxs-lookup"><span data-stu-id="e49f7-125">The **New Web Application Firewall** blade opens, where you can perform **VM Configuration** steps and provide **WAF Information**.</span></span> <span data-ttu-id="e49f7-126">Válassza ki **Virtuálisgép-konfiguráció**.</span><span class="sxs-lookup"><span data-stu-id="e49f7-126">Select **VM Configuration**.</span></span>
7. <span data-ttu-id="e49f7-127">Az a **Virtuálisgép-konfiguráció** panelen meg lépett fel a virtuális gép, amelyen a WAF szükséges adatokat.</span><span class="sxs-lookup"><span data-stu-id="e49f7-127">In the **VM Configuration** blade, you enter information required to spin up the virtual machine that runs the WAF.</span></span>
   <span data-ttu-id="e49f7-128">![Virtuálisgép-konfiguráció][4]</span><span class="sxs-lookup"><span data-stu-id="e49f7-128">![VM configuration][4]</span></span>
8. <span data-ttu-id="e49f7-129">Lépjen vissza a **új webalkalmazási tűzfal** panelhez, és válassza **WAF információk**.</span><span class="sxs-lookup"><span data-stu-id="e49f7-129">Return to the **New Web Application Firewall** blade and select **WAF Information**.</span></span> <span data-ttu-id="e49f7-130">Az a **WAF információt** panelen konfigurálja a WAF magát.</span><span class="sxs-lookup"><span data-stu-id="e49f7-130">In the **WAF Information** blade, you configure the WAF itself.</span></span> <span data-ttu-id="e49f7-131">7. lépés lehetővé teszi, hogy konfigurálja a virtuális gép, amelyen a WAF fut, és 8. lépés lehetővé teszi, hogy hozzon létre a WAF magát.</span><span class="sxs-lookup"><span data-stu-id="e49f7-131">Step 7 allows you to configure the virtual machine on which the WAF runs and step 8 enables you to provision the WAF itself.</span></span>

## <a name="finalize-application-protection"></a><span data-ttu-id="e49f7-132">Alkalmazás védelem véglegesítése</span><span class="sxs-lookup"><span data-stu-id="e49f7-132">Finalize application protection</span></span>
1. <span data-ttu-id="e49f7-133">Lépjen vissza a **javaslatok** panelen.</span><span class="sxs-lookup"><span data-stu-id="e49f7-133">Return to the **Recommendations** blade.</span></span> <span data-ttu-id="e49f7-134">Egy új bejegyzést jött létre, miután létrehozta a WAF nevű **alkalmazás védelem véglegesítése**.</span><span class="sxs-lookup"><span data-stu-id="e49f7-134">A new entry was generated after you created the WAF, called **Finalize application protection**.</span></span> <span data-ttu-id="e49f7-135">Ez a bejegyzés azt jelzi, hogy végre kell hajtania a folyamat ténylegesen az Azure virtuális hálózaton belül WAF elvégeztük így megvédheti a az alkalmazás lehetővé teszi.</span><span class="sxs-lookup"><span data-stu-id="e49f7-135">This entry lets you know that you need to complete the process of actually wiring up the WAF within the Azure Virtual Network so that it can protect the application.</span></span>

   ![Alkalmazás védelem véglegesítése][5]

2. <span data-ttu-id="e49f7-137">Válassza ki **alkalmazás védelem véglegesítése**.</span><span class="sxs-lookup"><span data-stu-id="e49f7-137">Select **Finalize application protection**.</span></span> <span data-ttu-id="e49f7-138">Ekkor megnyílik egy új panel.</span><span class="sxs-lookup"><span data-stu-id="e49f7-138">A new blade opens.</span></span> <span data-ttu-id="e49f7-139">Láthatja, hogy van-e egy webes alkalmazás, amely a forgalom átirányítása megtörténik rendelkeznie kell.</span><span class="sxs-lookup"><span data-stu-id="e49f7-139">You can see that there is a web application that needs to have its traffic rerouted.</span></span>
3. <span data-ttu-id="e49f7-140">Válassza ki a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e49f7-140">Select the web application.</span></span> <span data-ttu-id="e49f7-141">Ekkor megnyílik egy panel, amely lehetővé teszi az lépéseket a véglegesítése a webalkalmazás tűzfal beállítása.</span><span class="sxs-lookup"><span data-stu-id="e49f7-141">A blade opens that gives you steps for finalizing the web application firewall setup.</span></span> <span data-ttu-id="e49f7-142">Hajtsa végre a lépéseket, majd válassza ki **korlátozzák a forgalmat**.</span><span class="sxs-lookup"><span data-stu-id="e49f7-142">Complete the steps, and then select **Restrict traffic**.</span></span> <span data-ttu-id="e49f7-143">A Security Center majd elvégzi a vezetékezést fel.</span><span class="sxs-lookup"><span data-stu-id="e49f7-143">Security Center then does the wiring-up for you.</span></span>

   ![Korlátozzák a forgalmat.][6]

> [!NOTE]
> <span data-ttu-id="e49f7-145">Adja hozzá ezeket az alkalmazásokat a meglévő WAF-központitelepítések védelmet biztosíthat a Security Center több webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="e49f7-145">You can protect multiple web applications in Security Center by adding these applications to your existing WAF deployments.</span></span>
>
>

<span data-ttu-id="e49f7-146">A naplók az adott WAF most már teljes mértékben.</span><span class="sxs-lookup"><span data-stu-id="e49f7-146">The logs from that WAF are now fully integrated.</span></span> <span data-ttu-id="e49f7-147">A Security Center automatikusan összegyűjtése és elemzése a naplókat, hogy az Ön számára fontos biztonsági riasztások azt is surface elindíthatja.</span><span class="sxs-lookup"><span data-stu-id="e49f7-147">Security Center can start automatically gathering and analyzing the logs so that it can surface important security alerts to you.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e49f7-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e49f7-148">Next steps</span></span>
<span data-ttu-id="e49f7-149">Ez a dokumentum bemutatta megvalósításához a Security Center javaslat "Add a web application."</span><span class="sxs-lookup"><span data-stu-id="e49f7-149">This document showed you how to implement the Security Center recommendation "Add a web application."</span></span> <span data-ttu-id="e49f7-150">Webalkalmazási tűzfal konfigurálásával kapcsolatos további tudnivalókért olvassa el a következőket:</span><span class="sxs-lookup"><span data-stu-id="e49f7-150">To learn more about configuring a web application firewall, see the following:</span></span>

* [<span data-ttu-id="e49f7-151">Webalkalmazási tűzfal (WAF) App Service Environment-környezet konfigurálása</span><span class="sxs-lookup"><span data-stu-id="e49f7-151">Configuring a Web Application Firewall (WAF) for App Service Environment</span></span>](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

<span data-ttu-id="e49f7-152">A Security Centerrel kapcsolatos további információkért olvassa el a következőket:</span><span class="sxs-lookup"><span data-stu-id="e49f7-152">To learn more about Security Center, see the following:</span></span>

* <span data-ttu-id="e49f7-153">[Biztonsági szabályzatok beállítása az Azure Security Centerben](security-center-policies.md) – Ez a cikk bemutatja, hogyan konfigurálhat biztonsági házirendeket Azure-előfizetései és -erőforráscsoportjai számára.</span><span class="sxs-lookup"><span data-stu-id="e49f7-153">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how to configure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="e49f7-154">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – útmutató az Azure-erőforrások állapotának figyelésére.</span><span class="sxs-lookup"><span data-stu-id="e49f7-154">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how to monitor the health of your Azure resources.</span></span>
* <span data-ttu-id="e49f7-155">[Biztonsági riasztások kezelése és válaszadás a riasztásokra az Azure Security Centerben](security-center-managing-and-responding-alerts.md) – A biztonsági riasztások kezelése és az azokra való reagálás.</span><span class="sxs-lookup"><span data-stu-id="e49f7-155">[Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how to manage and respond to security alerts.</span></span>
* <span data-ttu-id="e49f7-156">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="e49f7-156">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="e49f7-157">[Azure Security Center – gyakran ismételt kérdések](security-center-faq.md) – Gyakran ismételt kérdések a szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="e49f7-157">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using the service.</span></span>
* <span data-ttu-id="e49f7-158">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="e49f7-158">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
