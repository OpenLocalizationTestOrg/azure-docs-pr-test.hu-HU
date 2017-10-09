---
title: "az Azure Security Centerben webalkalmazási tűzfal aaaAdd |} Microsoft Docs"
description: "Ez a dokumentum bemutatja, hogyan tooimplement hello Azure Security Center javaslatait ** hozzáadása a webes alkalmazás tűzfal ** és ** véglegesítése alkalmazás védelmi **."
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
ms.openlocfilehash: bff0aa2a5c6e0dde23396f93de52abe295053581
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-web-application-firewall-in-azure-security-center"></a><span data-ttu-id="a054e-103">Webalkalmazási tűzfal hozzáadása az Azure Security Centerben</span><span class="sxs-lookup"><span data-stu-id="a054e-103">Add a web application firewall in Azure Security Center</span></span>
<span data-ttu-id="a054e-104">Az Azure Security Center is javasoljuk, hogy ad hozzá egy webes alkalmazás tűzfalat (waf-ot) az egy Microsoft partnert toosecure a webes alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="a054e-104">Azure Security Center may recommend that you add a web application firewall (WAF) from a Microsoft partner toosecure your web applications.</span></span> <span data-ttu-id="a054e-105">Ez a dokumentum útmutatást nyújt a példa bemutatja, hogyan tooapply Ez a javaslat.</span><span class="sxs-lookup"><span data-stu-id="a054e-105">This document walks you through an example of how tooapply this recommendation.</span></span>

<span data-ttu-id="a054e-106">WAF ajánlás bármely nyilvános felé néző IP-cím (példány szint IP vagy terhelés eloszlik IP), amely rendelkezik egy társított hálózati biztonsági csoportot a nyissa meg a bejövő webes portokkal (80,443) jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a054e-106">A WAF recommendation is shown for any public facing IP (either Instance Level IP or Load Balanced IP) that has an associated network security group with open inbound web ports (80,443).</span></span>

<span data-ttu-id="a054e-107">A Security Center javasolja, hogy egy WAF toohelp kiépítése a webalkalmazások virtuális gépek és az App Service Environment-környezet célzó támadások elleni védelemre-e.</span><span class="sxs-lookup"><span data-stu-id="a054e-107">Security Center recommends that you provision a WAF toohelp defend against attacks targeting your web applications on virtual machines and on App Service Environment.</span></span> <span data-ttu-id="a054e-108">Az App Service környezetben (ASE) van egy [prémium](https://azure.microsoft.com/pricing/details/app-service/) terv lehetőséget az Azure App Service egy teljesen elkülönített és dedikált környezetben történő biztonságos futtatására az Azure App Service apps biztosító szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="a054e-108">An App Service Environment (ASE) is a [Premium](https://azure.microsoft.com/pricing/details/app-service/) service plan option of Azure App Service that provides a fully isolated and dedicated environment for securely running Azure App Service apps.</span></span> <span data-ttu-id="a054e-109">toolearn ASE, kapcsolatos további információkért lásd: hello [App Service környezetben dokumentáció](../app-service/app-service-app-service-environments-readme.md).</span><span class="sxs-lookup"><span data-stu-id="a054e-109">toolearn more about ASE, see hello [App Service Environment Documentation](../app-service/app-service-app-service-environments-readme.md).</span></span>

> [!NOTE]
> <span data-ttu-id="a054e-110">Ez a dokumentum hello szolgáltatás telepítését bemutató példát használatával vezet be.</span><span class="sxs-lookup"><span data-stu-id="a054e-110">This document introduces hello service by using an example deployment.</span></span>  <span data-ttu-id="a054e-111">Ez a dokumentum nem tartalmaz lépésenkénti útmutatót.</span><span class="sxs-lookup"><span data-stu-id="a054e-111">This document is not a step-by-step guide.</span></span>
>
>

## <a name="implement-hello-recommendation"></a><span data-ttu-id="a054e-112">Hello javaslat megvalósítása</span><span class="sxs-lookup"><span data-stu-id="a054e-112">Implement hello recommendation</span></span>
1. <span data-ttu-id="a054e-113">A hello **javaslatok** panelen válassza **biztonságossá webalkalmazását webalkalmazási tűzfal használatával**.</span><span class="sxs-lookup"><span data-stu-id="a054e-113">In hello **Recommendations** blade, select **Secure web application using web application firewall**.</span></span>
   <span data-ttu-id="a054e-114">![Biztonságos webes alkalmazás][1]</span><span class="sxs-lookup"><span data-stu-id="a054e-114">![Secure web Application][1]</span></span>
2. <span data-ttu-id="a054e-115">A hello **a webes alkalmazások webalkalmazási tűzfal biztonságos** panelen válassza ki a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a054e-115">In hello **Secure your web applications using web application firewall** blade, select a web application.</span></span> <span data-ttu-id="a054e-116">Hello **webalkalmazási tűzfal hozzáadása** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="a054e-116">hello **Add a Web Application Firewall** blade opens.</span></span>
   <span data-ttu-id="a054e-117">![Webalkalmazási tűzfal hozzáadása][2]</span><span class="sxs-lookup"><span data-stu-id="a054e-117">![Add a web application firewall][2]</span></span>
3. <span data-ttu-id="a054e-118">Választhat toouse egy meglévő webalkalmazási tűzfal ha rendelkezésre áll, vagy létrehozhat egy újat.</span><span class="sxs-lookup"><span data-stu-id="a054e-118">You can choose toouse an existing web application firewall if available or you can create a new one.</span></span> <span data-ttu-id="a054e-119">Ebben a példában érhetők el nem létező WAFs, létrehozhatunk egy WAF.</span><span class="sxs-lookup"><span data-stu-id="a054e-119">In this example, there are no existing WAFs available so we create a WAF.</span></span>
4. <span data-ttu-id="a054e-120">egy WAF toocreate integrált partnereink hello listában jelöljön ki egy megoldást.</span><span class="sxs-lookup"><span data-stu-id="a054e-120">toocreate a WAF, select a solution from hello list of integrated partners.</span></span> <span data-ttu-id="a054e-121">Ez a példa azt válassza **Barracuda webalkalmazási tűzfal**.</span><span class="sxs-lookup"><span data-stu-id="a054e-121">In this example, we select **Barracuda Web Application Firewall**.</span></span>
5. <span data-ttu-id="a054e-122">Hello **Barracuda webalkalmazási tűzfal** panel nyílik meg, és, hogy hello partneri megoldás információt nyújt.</span><span class="sxs-lookup"><span data-stu-id="a054e-122">hello **Barracuda Web Application Firewall** blade opens providing you information about hello partner solution.</span></span> <span data-ttu-id="a054e-123">Válassza ki **létrehozása** hello információk panelen.</span><span class="sxs-lookup"><span data-stu-id="a054e-123">Select **Create** in hello information blade.</span></span>

   ![Tűzfal információk panel][3]

6. <span data-ttu-id="a054e-125">Hello **új webalkalmazási tűzfal** panel megnyitása, ahol elvégezhető **Virtuálisgép-konfiguráció** lépéseit, és adja meg **WAF információk**.</span><span class="sxs-lookup"><span data-stu-id="a054e-125">hello **New Web Application Firewall** blade opens, where you can perform **VM Configuration** steps and provide **WAF Information**.</span></span> <span data-ttu-id="a054e-126">Válassza ki **Virtuálisgép-konfiguráció**.</span><span class="sxs-lookup"><span data-stu-id="a054e-126">Select **VM Configuration**.</span></span>
7. <span data-ttu-id="a054e-127">A hello **Virtuálisgép-konfiguráció** panelen adhatja hello virtuális gép futó szükséges toospin hello WAF adatokat.</span><span class="sxs-lookup"><span data-stu-id="a054e-127">In hello **VM Configuration** blade, you enter information required toospin up hello virtual machine that runs hello WAF.</span></span>
   <span data-ttu-id="a054e-128">![Virtuálisgép-konfiguráció][4]</span><span class="sxs-lookup"><span data-stu-id="a054e-128">![VM configuration][4]</span></span>
8. <span data-ttu-id="a054e-129">Térjen vissza a toohello **új webalkalmazási tűzfal** panelhez, és válassza **WAF információk**.</span><span class="sxs-lookup"><span data-stu-id="a054e-129">Return toohello **New Web Application Firewall** blade and select **WAF Information**.</span></span> <span data-ttu-id="a054e-130">A hello **WAF információk** panelen konfigurálja magát hello WAF.</span><span class="sxs-lookup"><span data-stu-id="a054e-130">In hello **WAF Information** blade, you configure hello WAF itself.</span></span> <span data-ttu-id="a054e-131">7. lépés lehetővé teszi tooconfigure hello virtuális gép mely hello WAF a fut és 8. lépés lehetővé teszi a tooprovision hello WAF magát.</span><span class="sxs-lookup"><span data-stu-id="a054e-131">Step 7 allows you tooconfigure hello virtual machine on which hello WAF runs and step 8 enables you tooprovision hello WAF itself.</span></span>

## <a name="finalize-application-protection"></a><span data-ttu-id="a054e-132">Alkalmazás védelem véglegesítése</span><span class="sxs-lookup"><span data-stu-id="a054e-132">Finalize application protection</span></span>
1. <span data-ttu-id="a054e-133">Térjen vissza a toohello **javaslatok** panelen.</span><span class="sxs-lookup"><span data-stu-id="a054e-133">Return toohello **Recommendations** blade.</span></span> <span data-ttu-id="a054e-134">Egy új bejegyzést készítésének hello WAF nevű létrehozott **alkalmazás védelem véglegesítése**.</span><span class="sxs-lookup"><span data-stu-id="a054e-134">A new entry was generated after you created hello WAF, called **Finalize application protection**.</span></span> <span data-ttu-id="a054e-135">Ez a bejegyzés lehetővé teszi, hogy kell-e toocomplete hello folyamat ténylegesen ezzel elvégeztük hello WAF hello Azure virtuális hálózaton belül, hogy megvédheti a hello alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="a054e-135">This entry lets you know that you need toocomplete hello process of actually wiring up hello WAF within hello Azure Virtual Network so that it can protect hello application.</span></span>

   ![Alkalmazás védelem véglegesítése][5]

2. <span data-ttu-id="a054e-137">Válassza ki **alkalmazás védelem véglegesítése**.</span><span class="sxs-lookup"><span data-stu-id="a054e-137">Select **Finalize application protection**.</span></span> <span data-ttu-id="a054e-138">Ekkor megnyílik egy új panel.</span><span class="sxs-lookup"><span data-stu-id="a054e-138">A new blade opens.</span></span> <span data-ttu-id="a054e-139">Láthatja, hogy van-e egy webes alkalmazás, amelyet a toohave a forgalom átirányítása megtörténik.</span><span class="sxs-lookup"><span data-stu-id="a054e-139">You can see that there is a web application that needs toohave its traffic rerouted.</span></span>
3. <span data-ttu-id="a054e-140">Válassza ki a hello webes alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="a054e-140">Select hello web application.</span></span> <span data-ttu-id="a054e-141">Ekkor megnyílik egy panel, amely lehetővé teszi az lépéseket a véglegesítése hello webalkalmazás tűzfal beállítása.</span><span class="sxs-lookup"><span data-stu-id="a054e-141">A blade opens that gives you steps for finalizing hello web application firewall setup.</span></span> <span data-ttu-id="a054e-142">Hello lépéseket, majd válassza ki **korlátozzák a forgalmat**.</span><span class="sxs-lookup"><span data-stu-id="a054e-142">Complete hello steps, and then select **Restrict traffic**.</span></span> <span data-ttu-id="a054e-143">A Security Center majd hello vezetékezést-fel Önnek.</span><span class="sxs-lookup"><span data-stu-id="a054e-143">Security Center then does hello wiring-up for you.</span></span>

   ![Korlátozzák a forgalmat.][6]

> [!NOTE]
> <span data-ttu-id="a054e-145">A Security Center több webalkalmazás védelmet biztosíthat a következő alkalmazások tooyour meglévő WAF központi telepítések hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="a054e-145">You can protect multiple web applications in Security Center by adding these applications tooyour existing WAF deployments.</span></span>
>
>

<span data-ttu-id="a054e-146">az adott WAF hello naplók most már teljes mértékben.</span><span class="sxs-lookup"><span data-stu-id="a054e-146">hello logs from that WAF are now fully integrated.</span></span> <span data-ttu-id="a054e-147">A Security Center automatikusan összegyűjtése és elemzése hello naplókat, hogy azt is surface fontos biztonsági riasztások tooyou elindíthatja.</span><span class="sxs-lookup"><span data-stu-id="a054e-147">Security Center can start automatically gathering and analyzing hello logs so that it can surface important security alerts tooyou.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a054e-148">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a054e-148">Next steps</span></span>
<span data-ttu-id="a054e-149">Ez a dokumentum bemutatta, hogyan tooimplement hello Security Center ajánlás "Add a web application."</span><span class="sxs-lookup"><span data-stu-id="a054e-149">This document showed you how tooimplement hello Security Center recommendation "Add a web application."</span></span> <span data-ttu-id="a054e-150">bővebben a webes alkalmazás tűzfal konfigurálásáról toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="a054e-150">toolearn more about configuring a web application firewall, see hello following:</span></span>

* [<span data-ttu-id="a054e-151">Webalkalmazási tűzfal (WAF) App Service Environment-környezet konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a054e-151">Configuring a Web Application Firewall (WAF) for App Service Environment</span></span>](../app-service-web/app-service-app-service-environment-web-application-firewall.md)

<span data-ttu-id="a054e-152">További információ a Security Center toolearn hello következő lásd:</span><span class="sxs-lookup"><span data-stu-id="a054e-152">toolearn more about Security Center, see hello following:</span></span>

* <span data-ttu-id="a054e-153">[Biztonsági szabályzatok beállítása az Azure Security Center](security-center-policies.md) – megtudhatja, hogyan tooconfigure biztonsági házirendek az Azure-előfizetések és az erőforráscsoportokat.</span><span class="sxs-lookup"><span data-stu-id="a054e-153">[Setting security policies in Azure Security Center](security-center-policies.md) -- Learn how tooconfigure security policies for your Azure subscriptions and resource groups.</span></span>
* <span data-ttu-id="a054e-154">[Biztonsági állapotfigyelés az Azure Security Center](security-center-monitoring.md) – megtudhatja, hogyan toomonitor hello az Azure-erőforrások állapotát.</span><span class="sxs-lookup"><span data-stu-id="a054e-154">[Security health monitoring in Azure Security Center](security-center-monitoring.md) -- Learn how toomonitor hello health of your Azure resources.</span></span>
* <span data-ttu-id="a054e-155">[Az Azure Security Centerben riasztások kezelése és válaszol toosecurity](security-center-managing-and-responding-alerts.md) – megtudhatja, hogyan toomanage és válaszoljon toosecurity riasztásokat.</span><span class="sxs-lookup"><span data-stu-id="a054e-155">[Managing and responding toosecurity alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) -- Learn how toomanage and respond toosecurity alerts.</span></span>
* <span data-ttu-id="a054e-156">[Biztonsági javaslatok kezelése az Azure Security Center](security-center-recommendations.md) – megtudhatja, miként könnyítik meg a javaslatok az Azure-erőforrások védelme.</span><span class="sxs-lookup"><span data-stu-id="a054e-156">[Managing security recommendations in Azure Security Center](security-center-recommendations.md) -- Learn how recommendations help you protect your Azure resources.</span></span>
* <span data-ttu-id="a054e-157">[Azure Security Center: GYIK](security-center-faq.md) – gyakran ismételt kérdések hello szolgáltatás használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="a054e-157">[Azure Security Center FAQ](security-center-faq.md) -- Find frequently asked questions about using hello service.</span></span>
* <span data-ttu-id="a054e-158">[Az Azure biztonsági blog](http://blogs.msdn.com/b/azuresecurity/) – blogbejegyzések az Azure biztonsági és megfelelőségi funkcióiról.</span><span class="sxs-lookup"><span data-stu-id="a054e-158">[Azure Security blog](http://blogs.msdn.com/b/azuresecurity/) -- Find blog posts about Azure security and compliance.</span></span>

<!--Image references-->
[1]: ./media/security-center-add-web-application-firewall/secure-web-application.png
[2]:./media/security-center-add-web-application-firewall/add-a-waf.png
[3]: ./media/security-center-add-web-application-firewall/info-blade.png
[4]: ./media/security-center-add-web-application-firewall/select-vm-config.png
[5]: ./media/security-center-add-web-application-firewall/finalize-waf.png
[6]: ./media/security-center-add-web-application-firewall/restrict-traffic.png
