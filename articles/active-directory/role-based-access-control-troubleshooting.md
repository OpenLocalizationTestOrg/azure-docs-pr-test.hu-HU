---
title: "Az Azure RBAC hibaelhárítása |} Microsoft Docs"
description: "Segítség problémák vagy a szerepköralapú hozzáférés-vezérlés erőforrások kapcsolatos kérdésekre."
services: azure-portal
documentationcenter: na
author: andredm7
manager: femila
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: andredm
ms.reviewer: rqureshi
ms.openlocfilehash: 407c030ea159915d4d7ac21760a3d17ec2204372
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="role-based-access-control-troubleshooting"></a><span data-ttu-id="5f4b4-103">Szerepköralapú hozzáférés-vezérlés hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="5f4b4-103">Role-Based Access Control troubleshooting</span></span>

<span data-ttu-id="5f4b4-104">A dokumentum cikk a szerepköröket, meghatározott hozzáférési jogosultságai kapcsolatos gyakori kérdésekre ad választ, megállapításához, hogy mi történik, ha használja a szerepkörök az Azure portál és a részleg-hozzáférési problémák megoldása.</span><span class="sxs-lookup"><span data-stu-id="5f4b4-104">This document article answers common questions about the specific access rights that are granted with roles, so that you know what to expect when using the roles in the Azure portal and can troubleshoot access problems.</span></span> <span data-ttu-id="5f4b4-105">Ezek a szerepkörök az összes erőforrástípus terjed ki:</span><span class="sxs-lookup"><span data-stu-id="5f4b4-105">These three roles cover all resource types:</span></span>

* <span data-ttu-id="5f4b4-106">Tulajdonos</span><span class="sxs-lookup"><span data-stu-id="5f4b4-106">Owner</span></span>  
* <span data-ttu-id="5f4b4-107">Közreműködő</span><span class="sxs-lookup"><span data-stu-id="5f4b4-107">Contributor</span></span>  
* <span data-ttu-id="5f4b4-108">Olvasó</span><span class="sxs-lookup"><span data-stu-id="5f4b4-108">Reader</span></span>  

<span data-ttu-id="5f4b4-109">Tulajdonos és közreműködő szerepkörrel rendelkező személyek mindkét megoldást vezet a teljes hozzáféréssel rendelkeznek, de a közreműködői nem hozzáférést más felhasználóknak vagy csoportoknak.</span><span class="sxs-lookup"><span data-stu-id="5f4b4-109">Owners and contributors both have full access to the management experience, but a contributor can’t give access to other users or groups.</span></span> <span data-ttu-id="5f4b4-110">Részek lesznek még ennél is érdekesebb megoldást az olvasó szerepkört, hogy az adott azt fogja szánjon némi időt.</span><span class="sxs-lookup"><span data-stu-id="5f4b4-110">Things get a little more interesting with the reader role, so that’s where we'll spend some time.</span></span> <span data-ttu-id="5f4b4-111">Tekintse meg a [szerepköralapú hozzáférés-vezérlés a get-started cikk](role-based-access-control-configure.md) talál részletes hozzáférést.</span><span class="sxs-lookup"><span data-stu-id="5f4b4-111">See the [Role-Based Access Control get-started article](role-based-access-control-configure.md) for details on how to grant access.</span></span>

## <a name="app-service-workloads"></a><span data-ttu-id="5f4b4-112">App service munkaterhelések</span><span class="sxs-lookup"><span data-stu-id="5f4b4-112">App service workloads</span></span>
### <a name="write-access-capabilities"></a><span data-ttu-id="5f4b4-113">Írási képességek</span><span class="sxs-lookup"><span data-stu-id="5f4b4-113">Write access capabilities</span></span>
<span data-ttu-id="5f4b4-114">Ha megadta a felhasználói csak olvasható hozzáférést egyetlen webalkalmazáshoz, néhány funkció le vannak tiltva, hogy nem várt.</span><span class="sxs-lookup"><span data-stu-id="5f4b4-114">If you grant a user read-only access to a single web app, some features are disabled that you might not expect.</span></span> <span data-ttu-id="5f4b4-115">A következő felügyeleti képességeket szükséges **írási** egy webalkalmazást (tulajdonos vagy közreműködő) által elérhető, és nem érhetők el a olyan írásvédett forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="5f4b4-115">The following management capabilities require **write** access to a web app (either Contributor or Owner), and aren’t available in any read-only scenario.</span></span>

* <span data-ttu-id="5f4b4-116">Parancsok (például a start, stop, stb.)</span><span class="sxs-lookup"><span data-stu-id="5f4b4-116">Commands (like start, stop, etc.)</span></span>
* <span data-ttu-id="5f4b4-117">Általános konfiguráció, a skálázási beállításokat, a biztonsági mentés beállításait és a figyelési beállítások például beállítások módosítása</span><span class="sxs-lookup"><span data-stu-id="5f4b4-117">Changing settings like general configuration, scale settings, backup settings, and monitoring settings</span></span>
* <span data-ttu-id="5f4b4-118">Közzétételi hitelesítő adatokat és más titkos adatokat, például az alkalmazásbeállítások és kapcsolati karakterláncok használata</span><span class="sxs-lookup"><span data-stu-id="5f4b4-118">Accessing publishing credentials and other secrets like app settings and connection strings</span></span>
* <span data-ttu-id="5f4b4-119">A folyamatos átviteli naplók</span><span class="sxs-lookup"><span data-stu-id="5f4b4-119">Streaming logs</span></span>
* <span data-ttu-id="5f4b4-120">Diagnosztikai naplók konfiguráció</span><span class="sxs-lookup"><span data-stu-id="5f4b4-120">Diagnostic logs configuration</span></span>
* <span data-ttu-id="5f4b4-121">Konzol (parancssor)</span><span class="sxs-lookup"><span data-stu-id="5f4b4-121">Console (command prompt)</span></span>
* <span data-ttu-id="5f4b4-122">Aktív és a legutóbbi központi telepítéseket (helyi git folyamatos üzembe helyezés)</span><span class="sxs-lookup"><span data-stu-id="5f4b4-122">Active and recent deployments (for local git continuous deployment)</span></span>
* <span data-ttu-id="5f4b4-123">Becsült töltött</span><span class="sxs-lookup"><span data-stu-id="5f4b4-123">Estimated spend</span></span>
* <span data-ttu-id="5f4b4-124">Webteszt</span><span class="sxs-lookup"><span data-stu-id="5f4b4-124">Web tests</span></span>
* <span data-ttu-id="5f4b4-125">Virtuális hálózat (csak egy olvasó, ha egy virtuális hálózat már be lett állítva egy írási hozzáféréssel rendelkező felhasználó számára látható).</span><span class="sxs-lookup"><span data-stu-id="5f4b4-125">Virtual network (only visible to a reader if a virtual network has previously been configured by a user with write access).</span></span>

<span data-ttu-id="5f4b4-126">Ha sem tudja már használni ezen csempék, kérje a rendszergazdától, közreműködő eléréséhez a webalkalmazás szeretné.</span><span class="sxs-lookup"><span data-stu-id="5f4b4-126">If you can't access any of these tiles, you need to ask your administrator for Contributor access to the web app.</span></span>

### <a name="dealing-with-related-resources"></a><span data-ttu-id="5f4b4-127">Kapcsolódó erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="5f4b4-127">Dealing with related resources</span></span>
<span data-ttu-id="5f4b4-128">Webalkalmazások vannak bonyolítja néhány különböző erőforrások együttműködés jelenlétét.</span><span class="sxs-lookup"><span data-stu-id="5f4b4-128">Web apps are complicated by the presence of a few different resources that interplay.</span></span> <span data-ttu-id="5f4b4-129">Íme néhány webhelyekkel tipikus erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="5f4b4-129">Here is a typical resource group with a couple websites:</span></span>

![Webes alkalmazás erőforráscsoport](./media/role-based-access-control-troubleshooting/website-resource-model.png)

<span data-ttu-id="5f4b4-131">Ennek eredményeképpen ha biztosít valaki csak a web app, nagy részét a funkciói a webhely paneljén az Azure-portálon való hozzáférés le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="5f4b4-131">As a result, if you grant someone access to just the web app, much of the functionality on the website blade in the Azure portal is disabled.</span></span>

<span data-ttu-id="5f4b4-132">Ezek az elemek szükség **írási** a hozzáférést a **App Service-csomag** , amely megfelel a webhely:</span><span class="sxs-lookup"><span data-stu-id="5f4b4-132">These items require **write** access to the **App Service plan** that corresponds to your website:</span></span>  

* <span data-ttu-id="5f4b4-133">A webes alkalmazás megtekintése a tarifacsomag (ingyenes vagy normál)</span><span class="sxs-lookup"><span data-stu-id="5f4b4-133">Viewing the web app’s pricing tier (Free or Standard)</span></span>  
* <span data-ttu-id="5f4b4-134">Skála configuration (a példányok, a virtuális gép méretét, az automatikus skálázási beállítások száma)</span><span class="sxs-lookup"><span data-stu-id="5f4b4-134">Scale configuration (number of instances, virtual machine size, autoscale settings)</span></span>  
* <span data-ttu-id="5f4b4-135">Kvóták (tároló, a sávszélesség, a CPU)</span><span class="sxs-lookup"><span data-stu-id="5f4b4-135">Quotas (storage, bandwidth, CPU)</span></span>  

<span data-ttu-id="5f4b4-136">Ezek az elemek szükség **írási** a teljes hozzáférés **erőforráscsoport** , amely tartalmazza a webhely:</span><span class="sxs-lookup"><span data-stu-id="5f4b4-136">These items require **write** access to the whole **Resource group** that contains your website:</span></span>  

* <span data-ttu-id="5f4b4-137">SSL-tanúsítványokat és a kötések (SSL-tanúsítványok megoszthatók ugyanabban az erőforráscsoportban a helyek és a földrajzi helyhez között)</span><span class="sxs-lookup"><span data-stu-id="5f4b4-137">SSL Certificates and bindings (SSL certificates can be shared between sites in the same resource group and geo-location)</span></span>  
* <span data-ttu-id="5f4b4-138">A riasztási szabályok</span><span class="sxs-lookup"><span data-stu-id="5f4b4-138">Alert rules</span></span>  
* <span data-ttu-id="5f4b4-139">automatikus skálázási beállításokat</span><span class="sxs-lookup"><span data-stu-id="5f4b4-139">Autoscale settings</span></span>  
* <span data-ttu-id="5f4b4-140">Application insights összetevőinek</span><span class="sxs-lookup"><span data-stu-id="5f4b4-140">Application insights components</span></span>  
* <span data-ttu-id="5f4b4-141">Webteszt</span><span class="sxs-lookup"><span data-stu-id="5f4b4-141">Web tests</span></span>  

## <a name="virtual-machine-workloads"></a><span data-ttu-id="5f4b4-142">Virtuális gépek terheléséhez</span><span class="sxs-lookup"><span data-stu-id="5f4b4-142">Virtual machine workloads</span></span>
<span data-ttu-id="5f4b4-143">Sokkal hasonlóan a web apps, a virtuális gépek paneljét bizonyos funkcióinak írási hozzáférés szükséges a virtuális gépet, vagy további erőforrások az erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="5f4b4-143">Much like with web apps, some features on the virtual machine blade require write access to the virtual machine, or to other resources in the resource group.</span></span>

<span data-ttu-id="5f4b4-144">Virtuális gépek tartomány nevét, virtuális hálózatok, storage-fiókok és a riasztási szabályok kapcsolódnak.</span><span class="sxs-lookup"><span data-stu-id="5f4b4-144">Virtual machines are related to Domain names, virtual networks, storage accounts, and alert rules.</span></span>

<span data-ttu-id="5f4b4-145">Ezek az elemek szükség **írási** a hozzáférést a **virtuális gép**:</span><span class="sxs-lookup"><span data-stu-id="5f4b4-145">These items require **write** access to the **Virtual machine**:</span></span>

* <span data-ttu-id="5f4b4-146">Végpontok</span><span class="sxs-lookup"><span data-stu-id="5f4b4-146">Endpoints</span></span>  
* <span data-ttu-id="5f4b4-147">IP-címek</span><span class="sxs-lookup"><span data-stu-id="5f4b4-147">IP addresses</span></span>  
* <span data-ttu-id="5f4b4-148">Lemezek</span><span class="sxs-lookup"><span data-stu-id="5f4b4-148">Disks</span></span>  
* <span data-ttu-id="5f4b4-149">Bővítmények</span><span class="sxs-lookup"><span data-stu-id="5f4b4-149">Extensions</span></span>  

<span data-ttu-id="5f4b4-150">Szükséges **írási** is a hozzáférést a **virtuális gép**, és a **erőforráscsoport** (valamint a tartomány nevét), hogy az informatikai van:</span><span class="sxs-lookup"><span data-stu-id="5f4b4-150">These require **write** access to both the **Virtual machine**, and the **Resource group** (along with the Domain name) that it is in:</span></span>  

* <span data-ttu-id="5f4b4-151">Rendelkezésre állási csoport</span><span class="sxs-lookup"><span data-stu-id="5f4b4-151">Availability set</span></span>  
* <span data-ttu-id="5f4b4-152">Elosztott terhelésű készlethez</span><span class="sxs-lookup"><span data-stu-id="5f4b4-152">Load balanced set</span></span>  
* <span data-ttu-id="5f4b4-153">A riasztási szabályok</span><span class="sxs-lookup"><span data-stu-id="5f4b4-153">Alert rules</span></span>  

<span data-ttu-id="5f4b4-154">Ha sem tudja már használni ezen csempék, kérje meg a rendszergazdát az erőforráscsoport közreműködői elérésére.</span><span class="sxs-lookup"><span data-stu-id="5f4b4-154">If you can't access any of these tiles, ask your administrator for Contributor access to the Resource group.</span></span>

## <a name="see-more"></a><span data-ttu-id="5f4b4-155">Részletek</span><span class="sxs-lookup"><span data-stu-id="5f4b4-155">See more</span></span>
* <span data-ttu-id="5f4b4-156">[Szerepköralapú hozzáférés-vezérlés](role-based-access-control-configure.md): az RBAC első lépései az Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="5f4b4-156">[Role Based Access Control](role-based-access-control-configure.md): Get started with RBAC in the Azure portal.</span></span>
* <span data-ttu-id="5f4b4-157">[Beépített szerepkörök](role-based-access-built-in-roles.md): részletes információkat szolgáltatva a szerepköröket, az RBAC szabványos tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="5f4b4-157">[Built-in roles](role-based-access-built-in-roles.md): Get details about the roles that come standard in RBAC.</span></span>
* <span data-ttu-id="5f4b4-158">[Egyéni szerepkörök az Azure RBAC](role-based-access-control-custom-roles.md): megtudhatja, hogyan hozzon létre egyéni szerepkörök az access igényeihez.</span><span class="sxs-lookup"><span data-stu-id="5f4b4-158">[Custom roles in Azure RBAC](role-based-access-control-custom-roles.md): Learn how to create custom roles to fit your access needs.</span></span>
* <span data-ttu-id="5f4b4-159">[Access módosítási előzményeit jelentés létrehozása](role-based-access-control-access-change-history-report.md): nyomon követjük, hogy az RBAC más szerepkörök hozzárendeléséről.</span><span class="sxs-lookup"><span data-stu-id="5f4b4-159">[Create an access change history report](role-based-access-control-access-change-history-report.md): Keep track of changing role assignments in RBAC.</span></span>

