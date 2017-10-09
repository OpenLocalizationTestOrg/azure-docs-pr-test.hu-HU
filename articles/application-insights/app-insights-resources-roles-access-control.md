---
title: "Azure Application Insights aaaResources, szerepkörök és hozzáférés-vezérlés |} Microsoft Docs"
description: "Tulajdonosai, közreműködő szerepkörrel rendelkező személyek és a szervezet insights olvasói."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 49f736a5-67fe-4cc6-b1ef-51b993fb39bd
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 03/17/2017
ms.author: bwren
ms.openlocfilehash: a6f6ca0443b5f60239f094606e124f856967d8ca
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="resources-roles-and-access-control-in-application-insights"></a><span data-ttu-id="3fb50-103">Erőforrások, szerepkörök és hozzáférés-vezérlés az Application Insightsban</span><span class="sxs-lookup"><span data-stu-id="3fb50-103">Resources, roles, and access control in Application Insights</span></span>
<span data-ttu-id="3fb50-104">Szabályozhatja, ki rendelkezik-e olvasási és Azure access tooyour adatok frissítése [Application Insights][start], használatával [Microsoft Azure szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="3fb50-104">You can control who has read and update access tooyour data in Azure [Application Insights][start], by using [Role-based access control in Microsoft Azure](../active-directory/role-based-access-control-configure.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3fb50-105">Rendelje hozzá a hello hozzáférés toousers **erőforráscsoportba vagy előfizetésbe** toowhich az alkalmazás erőforrás tartozik - nem a hello erőforrás magát.</span><span class="sxs-lookup"><span data-stu-id="3fb50-105">Assign access toousers in hello **resource group or subscription** toowhich your application resource belongs - not in hello resource itself.</span></span> <span data-ttu-id="3fb50-106">Rendelje hozzá a hello **Application Insights-összetevővel kapcsolatos közreműködői** szerepkör.</span><span class="sxs-lookup"><span data-stu-id="3fb50-106">Assign hello **Application Insights component contributor** role.</span></span> <span data-ttu-id="3fb50-107">Ez biztosítja a hozzáférés tooweb tesztek és a riasztások mellett az alkalmazás erőforrás egységes irányítását.</span><span class="sxs-lookup"><span data-stu-id="3fb50-107">This ensures uniform control of access tooweb tests and alerts along with your application resource.</span></span> <span data-ttu-id="3fb50-108">[További információk](#access).</span><span class="sxs-lookup"><span data-stu-id="3fb50-108">[Learn more](#access).</span></span>
> 
> 

## <a name="resources-groups-and-subscriptions"></a><span data-ttu-id="3fb50-109">Erőforrások, csoportok és előfizetések</span><span class="sxs-lookup"><span data-stu-id="3fb50-109">Resources, groups and subscriptions</span></span>
<span data-ttu-id="3fb50-110">Első, néhány definíciók:</span><span class="sxs-lookup"><span data-stu-id="3fb50-110">First, some definitions:</span></span>

* <span data-ttu-id="3fb50-111">**Erőforrás** - egy Microsoft Azure service-példányhoz.</span><span class="sxs-lookup"><span data-stu-id="3fb50-111">**Resource** - An instance of a Microsoft Azure service.</span></span> <span data-ttu-id="3fb50-112">Az Application Insights-erőforrás gyűjti, elemzi és az alkalmazásból küldött hello telemetriai adatokat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="3fb50-112">Your Application Insights resource collects, analyzes and displays hello telemetry data sent from your application.</span></span>  <span data-ttu-id="3fb50-113">Más típusú Azure-erőforrások közé tartoznak a webalkalmazások, adatbázisok és virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="3fb50-113">Other types of Azure resources include web apps, databases, and VMs.</span></span>
  
    <span data-ttu-id="3fb50-114">toosee az erőforrásokat, nyissa meg a hello [Azure Portal][portal], jelentkezzen be, majd kattintson az összes erőforrást.</span><span class="sxs-lookup"><span data-stu-id="3fb50-114">toosee your resources, open hello [Azure Portal][portal], sign in, and click All Resources.</span></span> <span data-ttu-id="3fb50-115">toofind egy erőforrás típusa hello szűrőmező nevének egy részét.</span><span class="sxs-lookup"><span data-stu-id="3fb50-115">toofind a resource, type part of its name in hello filter field.</span></span>
  
    ![Azure-erőforrások listája](./media/app-insights-resources-roles-access-control/10-browse.png)

<a name="resource-group"></a>

* <span data-ttu-id="3fb50-117">[**Erőforráscsoport** ] [ group] -minden erőforrás tooone csoport tartozik.</span><span class="sxs-lookup"><span data-stu-id="3fb50-117">[**Resource group**][group] - Every resource belongs tooone group.</span></span> <span data-ttu-id="3fb50-118">Egy csoport egy kényelmes módot toomanage kapcsolódó erőforrások, különösen a hozzáférés-vezérléshez.</span><span class="sxs-lookup"><span data-stu-id="3fb50-118">A group is a convenient way toomanage related resources, particularly for access control.</span></span> <span data-ttu-id="3fb50-119">Például egy erőforrást a csoport egy webalkalmazást, az Application Insights-erőforrás toomonitor hello alkalmazások és a Storage erőforrás tookeep ütemezésbe helyezheti exportált adatok.</span><span class="sxs-lookup"><span data-stu-id="3fb50-119">For example, into one resource group you could put a Web App, an Application Insights resource toomonitor hello app, and a Storage resource tookeep exported data.</span></span>

    ![A Tallózás, erőforráscsoportok, majd válasszon ki egy csoportot](./media/app-insights-resources-roles-access-control/11-group.png)

* <span data-ttu-id="3fb50-121">[**Előfizetés** ](https://manage.windowsazure.com) -toouse Application Insights vagy más Azure-erőforrások, jelentkezzen be Azure-előfizetés tooan.</span><span class="sxs-lookup"><span data-stu-id="3fb50-121">[**Subscription**](https://manage.windowsazure.com) - toouse Application Insights or other Azure resources, you sign in tooan Azure subscription.</span></span> <span data-ttu-id="3fb50-122">Minden erőforráscsoport tooone Azure-előfizetéssel, ahol az ár csomag kiválasztása és, ha egy szervezet előfizetés válasszon hello tagok és a hozzáférési engedélyeik tartozik.</span><span class="sxs-lookup"><span data-stu-id="3fb50-122">Every resource group belongs tooone Azure subscription, where you choose your price package and, if it's an organization subscription, choose hello members and their access permissions.</span></span>
* <span data-ttu-id="3fb50-123">[**Microsoft-fiók** ] [ account] -felhasználónév és jelszó toosign használhatja a tooMicrosoft Azure hello előfizetések, XBox Live-, Outlook.com-os és más Microsoft-szolgáltatásokban.</span><span class="sxs-lookup"><span data-stu-id="3fb50-123">[**Microsoft account**][account] - hello username and password that you use toosign in tooMicrosoft Azure subscriptions, XBox Live, Outlook.com, and other Microsoft services.</span></span>

## <span data-ttu-id="3fb50-124"><a name="access"></a>Hello erőforráscsoportban elérés</span><span class="sxs-lookup"><span data-stu-id="3fb50-124"><a name="access"></a> Control access in hello resource group</span></span>
<span data-ttu-id="3fb50-125">Fontos, hogy hozzáadása toohello erőforrás létrehozta az alkalmazáshoz, nincsenek is külön riasztások és a webalkalmazás-tesztek rejtett erőforrások toounderstand.</span><span class="sxs-lookup"><span data-stu-id="3fb50-125">It's important toounderstand that in addition toohello resource you created for your application, there are also separate hidden resources for alerts and web tests.</span></span> <span data-ttu-id="3fb50-126">-E csatolt toohello azonos [erőforráscsoport](#resource-group) az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3fb50-126">They are attached toohello same [resource group](#resource-group) as your application.</span></span> <span data-ttu-id="3fb50-127">Előfordulhat, hogy is vezettek be más Azure-szolgáltatásokat az ott, például webhelyekhez vagy tároló.</span><span class="sxs-lookup"><span data-stu-id="3fb50-127">You might also have put other Azure services in there, such as websites or storage.</span></span>

![Az Application Insightsban erőforrások](./media/app-insights-resources-roles-access-control/00-resources.png)

<span data-ttu-id="3fb50-129">toocontrol toothese erőforrások eléréséhez ezért javasoljuk, hogy:</span><span class="sxs-lookup"><span data-stu-id="3fb50-129">toocontrol access toothese resources it's therefore recommended to:</span></span>

* <span data-ttu-id="3fb50-130">Hozzáférés: hello **erőforráscsoportba vagy előfizetésbe** szintjét.</span><span class="sxs-lookup"><span data-stu-id="3fb50-130">Control access at hello **resource group or subscription** level.</span></span>
* <span data-ttu-id="3fb50-131">Rendelje hozzá a hello **Application Insights-összetevővel kapcsolatos közreműködői** szerepkör toousers.</span><span class="sxs-lookup"><span data-stu-id="3fb50-131">Assign hello **Application Insights Component contributor** role toousers.</span></span> <span data-ttu-id="3fb50-132">Ez lehetővé teszi őket tooedit webes tesztjeinek használatát, a riasztások és az Application Insights-erőforrások hozzáférés tooany más szolgáltatások hello csoport megadása nélkül.</span><span class="sxs-lookup"><span data-stu-id="3fb50-132">This allows them tooedit web tests, alerts, and Application Insights resources, without providing access tooany other services in hello group.</span></span>

## <a name="tooprovide-access-tooanother-user"></a><span data-ttu-id="3fb50-133">tooprovide hozzáférés tooanother felhasználó</span><span class="sxs-lookup"><span data-stu-id="3fb50-133">tooprovide access tooanother user</span></span>
<span data-ttu-id="3fb50-134">Tulajdonosi jogok toohello előfizetés vagy hello erőforráscsoportban kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="3fb50-134">You must have Owner rights toohello subscription or hello resource group.</span></span>

<span data-ttu-id="3fb50-135">hello felhasználónak rendelkeznie kell egy [Microsoft Account][account], vagy nem érhető el tootheir [szervezeti Microsoft Account](../active-directory/sign-up-organization.md).</span><span class="sxs-lookup"><span data-stu-id="3fb50-135">hello user must have a [Microsoft Account][account], or access tootheir [organizational Microsoft Account](../active-directory/sign-up-organization.md).</span></span> <span data-ttu-id="3fb50-136">Megadhatja a hozzáférés tooindividuals, valamint az Azure Active Directory toouser csoportok.</span><span class="sxs-lookup"><span data-stu-id="3fb50-136">You can provide access tooindividuals, and also toouser groups defined in Azure Active Directory.</span></span>

#### <a name="navigate-toohello-resource-group"></a><span data-ttu-id="3fb50-137">Keresse meg a toohello erőforráscsoport</span><span class="sxs-lookup"><span data-stu-id="3fb50-137">Navigate toohello resource group</span></span>
<span data-ttu-id="3fb50-138">Hiba a hello felhasználó hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="3fb50-138">Add hello user there.</span></span>

![Az alkalmazás erőforrás panelén Essentials megnyitásához nyisson hello erőforráscsoportban, és nincs válassza ki a beállítások/felhasználókat.](./media/app-insights-resources-roles-access-control/01-add-user.png)

<span data-ttu-id="3fb50-141">Vagy lépjen be egy másik szolgáltatásiszint és hello felhasználói toohello előfizetés hozzáadása sikerült.</span><span class="sxs-lookup"><span data-stu-id="3fb50-141">Or you could go up another level and add hello user toohello Subscription.</span></span>

#### <a name="select-a-role"></a><span data-ttu-id="3fb50-142">Szerepkörválasztás</span><span class="sxs-lookup"><span data-stu-id="3fb50-142">Select a role</span></span>
![Válasszon egy hello új felhasználói szerepet](./media/app-insights-resources-roles-access-control/03-role.png)

| <span data-ttu-id="3fb50-144">Szerepkör</span><span class="sxs-lookup"><span data-stu-id="3fb50-144">Role</span></span> | <span data-ttu-id="3fb50-145">Hello erőforráscsoportban található</span><span class="sxs-lookup"><span data-stu-id="3fb50-145">In hello resource group</span></span> |
| --- | --- |
| <span data-ttu-id="3fb50-146">Tulajdonos</span><span class="sxs-lookup"><span data-stu-id="3fb50-146">Owner</span></span> |<span data-ttu-id="3fb50-147">Módosíthatja semmit, beleértve a felhasználói hozzáférés</span><span class="sxs-lookup"><span data-stu-id="3fb50-147">Can change anything, including user access</span></span> |
| <span data-ttu-id="3fb50-148">Közreműködő</span><span class="sxs-lookup"><span data-stu-id="3fb50-148">Contributor</span></span> |<span data-ttu-id="3fb50-149">Bármi, beleértve az összes erőforrás szerkesztése</span><span class="sxs-lookup"><span data-stu-id="3fb50-149">Can edit anything, including all resources</span></span> |
| <span data-ttu-id="3fb50-150">Application Insights-összetevővel kapcsolatos közreműködői</span><span class="sxs-lookup"><span data-stu-id="3fb50-150">Application Insights Component contributor</span></span> |<span data-ttu-id="3fb50-151">Az Application Insights erőforrásokat, webes tesztjeinek használatát, és a riasztások szerkesztése</span><span class="sxs-lookup"><span data-stu-id="3fb50-151">Can edit Application Insights resources, web tests and alerts</span></span> |
| <span data-ttu-id="3fb50-152">Olvasó</span><span class="sxs-lookup"><span data-stu-id="3fb50-152">Reader</span></span> |<span data-ttu-id="3fb50-153">Megtekintheti, de nem bármit módosíthat</span><span class="sxs-lookup"><span data-stu-id="3fb50-153">Can view but not change anything</span></span> |

<span data-ttu-id="3fb50-154">"Szerkesztés" magában foglalja a létrehozása, törlése és frissítése:</span><span class="sxs-lookup"><span data-stu-id="3fb50-154">'Editing' includes creating, deleting and updating:</span></span>

* <span data-ttu-id="3fb50-155">Erőforrások</span><span class="sxs-lookup"><span data-stu-id="3fb50-155">Resources</span></span>
* <span data-ttu-id="3fb50-156">Webteszt</span><span class="sxs-lookup"><span data-stu-id="3fb50-156">Web tests</span></span>
* <span data-ttu-id="3fb50-157">Riasztások</span><span class="sxs-lookup"><span data-stu-id="3fb50-157">Alerts</span></span>
* <span data-ttu-id="3fb50-158">Folyamatos exportálás</span><span class="sxs-lookup"><span data-stu-id="3fb50-158">Continuous export</span></span>

#### <a name="select-hello-user"></a><span data-ttu-id="3fb50-159">Hello felhasználó kiválasztása</span><span class="sxs-lookup"><span data-stu-id="3fb50-159">Select hello user</span></span>

<span data-ttu-id="3fb50-160">Ha azt szeretné, hello felhasználói nem hello könyvtárban, felajánlhatja bárki, aki a Microsoft-fiók.</span><span class="sxs-lookup"><span data-stu-id="3fb50-160">If hello user you want isn't in hello directory, you can invite anyone with a Microsoft account.</span></span>
<span data-ttu-id="3fb50-161">(Például Outlook.com, OneDrive, Windows Phone vagy XBox Live services használata, ha rendelkeznek a Microsoft-fiók.)</span><span class="sxs-lookup"><span data-stu-id="3fb50-161">(If they use services like Outlook.com, OneDrive, Windows Phone, or XBox Live, they have a Microsoft account.)</span></span>

## <a name="related-content"></a><span data-ttu-id="3fb50-162">Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="3fb50-162">Related content</span></span>

* [<span data-ttu-id="3fb50-163">Szerepköralapú hozzáférés-vezérlés az Azure-ban</span><span class="sxs-lookup"><span data-stu-id="3fb50-163">Role based access control in Azure</span></span>](../active-directory/role-based-access-control-configure.md)

<!--Link references-->

[account]: https://account.microsoft.com
[group]: ../azure-resource-manager/resource-group-overview.md
[portal]: https://portal.azure.com/
[start]: app-insights-overview.md
