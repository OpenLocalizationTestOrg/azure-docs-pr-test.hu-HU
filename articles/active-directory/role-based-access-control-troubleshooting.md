---
title: Azure RBAC aaaTroubleshoot |} Microsoft Docs
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
ms.openlocfilehash: 15feced32d8459d90c4c246d335932f90e1fc91f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="role-based-access-control-troubleshooting"></a><span data-ttu-id="7eb63-103">Szerepköralapú hozzáférés-vezérlés hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="7eb63-103">Role-Based Access Control troubleshooting</span></span>

<span data-ttu-id="7eb63-104">Ez a dokumentum a cikk kapcsolatos kérdésekre ad közös hello speciális hozzáférési jogok, amelyeket a szerepkörök, megállapításához, hogy milyen tooexpect használatával hello hello Azure-portálon a szerepkörök és hozzáférési problémák megoldása is.</span><span class="sxs-lookup"><span data-stu-id="7eb63-104">This document article answers common questions about hello specific access rights that are granted with roles, so that you know what tooexpect when using hello roles in hello Azure portal and can troubleshoot access problems.</span></span> <span data-ttu-id="7eb63-105">Ezek a szerepkörök az összes erőforrástípus terjed ki:</span><span class="sxs-lookup"><span data-stu-id="7eb63-105">These three roles cover all resource types:</span></span>

* <span data-ttu-id="7eb63-106">Tulajdonos</span><span class="sxs-lookup"><span data-stu-id="7eb63-106">Owner</span></span>  
* <span data-ttu-id="7eb63-107">Közreműködő</span><span class="sxs-lookup"><span data-stu-id="7eb63-107">Contributor</span></span>  
* <span data-ttu-id="7eb63-108">Olvasó</span><span class="sxs-lookup"><span data-stu-id="7eb63-108">Reader</span></span>  

<span data-ttu-id="7eb63-109">Tulajdonos és közreműködő szerepkörrel rendelkező személyek mindkét rendelkezik teljes körű hozzáférési toohello felügyeleti tapasztalhat, de egy közreműködői nem adhat hozzáférést tooother felhasználókat vagy csoportokat.</span><span class="sxs-lookup"><span data-stu-id="7eb63-109">Owners and contributors both have full access toohello management experience, but a contributor can’t give access tooother users or groups.</span></span> <span data-ttu-id="7eb63-110">Részek lesznek még ennél is érdekesebb megoldást hello olvasó szerepkört, az, hogy az adott azt fogja szánjon némi időt.</span><span class="sxs-lookup"><span data-stu-id="7eb63-110">Things get a little more interesting with hello reader role, so that’s where we'll spend some time.</span></span> <span data-ttu-id="7eb63-111">Lásd: hello [szerepköralapú hozzáférés-vezérlés a get-started cikk](role-based-access-control-configure.md) miként férhetnek hozzá az toogrant leírását.</span><span class="sxs-lookup"><span data-stu-id="7eb63-111">See hello [Role-Based Access Control get-started article](role-based-access-control-configure.md) for details on how toogrant access.</span></span>

## <a name="app-service-workloads"></a><span data-ttu-id="7eb63-112">App service munkaterhelések</span><span class="sxs-lookup"><span data-stu-id="7eb63-112">App service workloads</span></span>
### <a name="write-access-capabilities"></a><span data-ttu-id="7eb63-113">Írási képességek</span><span class="sxs-lookup"><span data-stu-id="7eb63-113">Write access capabilities</span></span>
<span data-ttu-id="7eb63-114">Ha a felhasználó csak olvasási hozzáféréssel tooa egyetlen webalkalmazás biztosít, néhány funkció le vannak tiltva, hogy nem várt.</span><span class="sxs-lookup"><span data-stu-id="7eb63-114">If you grant a user read-only access tooa single web app, some features are disabled that you might not expect.</span></span> <span data-ttu-id="7eb63-115">a következő felügyeleti képességeket hello szükséges **írási** férnek hozzá a tooa web app (tulajdonos vagy közreműködő), és nem érhetők el a olyan írásvédett forgatókönyv.</span><span class="sxs-lookup"><span data-stu-id="7eb63-115">hello following management capabilities require **write** access tooa web app (either Contributor or Owner), and aren’t available in any read-only scenario.</span></span>

* <span data-ttu-id="7eb63-116">Parancsok (például a start, stop, stb.)</span><span class="sxs-lookup"><span data-stu-id="7eb63-116">Commands (like start, stop, etc.)</span></span>
* <span data-ttu-id="7eb63-117">Általános konfiguráció, a skálázási beállításokat, a biztonsági mentés beállításait és a figyelési beállítások például beállítások módosítása</span><span class="sxs-lookup"><span data-stu-id="7eb63-117">Changing settings like general configuration, scale settings, backup settings, and monitoring settings</span></span>
* <span data-ttu-id="7eb63-118">Közzétételi hitelesítő adatokat és más titkos adatokat, például az alkalmazásbeállítások és kapcsolati karakterláncok használata</span><span class="sxs-lookup"><span data-stu-id="7eb63-118">Accessing publishing credentials and other secrets like app settings and connection strings</span></span>
* <span data-ttu-id="7eb63-119">A folyamatos átviteli naplók</span><span class="sxs-lookup"><span data-stu-id="7eb63-119">Streaming logs</span></span>
* <span data-ttu-id="7eb63-120">Diagnosztikai naplók konfiguráció</span><span class="sxs-lookup"><span data-stu-id="7eb63-120">Diagnostic logs configuration</span></span>
* <span data-ttu-id="7eb63-121">Konzol (parancssor)</span><span class="sxs-lookup"><span data-stu-id="7eb63-121">Console (command prompt)</span></span>
* <span data-ttu-id="7eb63-122">Aktív és a legutóbbi központi telepítéseket (helyi git folyamatos üzembe helyezés)</span><span class="sxs-lookup"><span data-stu-id="7eb63-122">Active and recent deployments (for local git continuous deployment)</span></span>
* <span data-ttu-id="7eb63-123">Becsült töltött</span><span class="sxs-lookup"><span data-stu-id="7eb63-123">Estimated spend</span></span>
* <span data-ttu-id="7eb63-124">Webteszt</span><span class="sxs-lookup"><span data-stu-id="7eb63-124">Web tests</span></span>
* <span data-ttu-id="7eb63-125">Virtuális hálózat (csak látható tooa olvasó Ha egy virtuális hálózat már be lett állítva egy írási hozzáféréssel rendelkező felhasználónak).</span><span class="sxs-lookup"><span data-stu-id="7eb63-125">Virtual network (only visible tooa reader if a virtual network has previously been configured by a user with write access).</span></span>

<span data-ttu-id="7eb63-126">Ha sem tudja már használni ezen csempék, akkor tooask a rendszergazda közreműködői hozzáférés toohello webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="7eb63-126">If you can't access any of these tiles, you need tooask your administrator for Contributor access toohello web app.</span></span>

### <a name="dealing-with-related-resources"></a><span data-ttu-id="7eb63-127">Kapcsolódó erőforrások kezelése</span><span class="sxs-lookup"><span data-stu-id="7eb63-127">Dealing with related resources</span></span>
<span data-ttu-id="7eb63-128">Webalkalmazások vannak bonyolítja néhány különböző erőforrások együttműködés hello jelenlétét.</span><span class="sxs-lookup"><span data-stu-id="7eb63-128">Web apps are complicated by hello presence of a few different resources that interplay.</span></span> <span data-ttu-id="7eb63-129">Íme néhány webhelyekkel tipikus erőforráscsoport:</span><span class="sxs-lookup"><span data-stu-id="7eb63-129">Here is a typical resource group with a couple websites:</span></span>

![Webes alkalmazás erőforráscsoport](./media/role-based-access-control-troubleshooting/website-resource-model.png)

<span data-ttu-id="7eb63-131">Ennek eredményeképpen ha valaki biztosít hozzáférést toojust hello webalkalmazás, nagy részét hello webhely panel az Azure-portálon hello hello funkciói le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="7eb63-131">As a result, if you grant someone access toojust hello web app, much of hello functionality on hello website blade in hello Azure portal is disabled.</span></span>

<span data-ttu-id="7eb63-132">Ezek az elemek szükség **írási** toohello hozzáférési **App Service-csomag** , amely megfelel a tooyour webhely:</span><span class="sxs-lookup"><span data-stu-id="7eb63-132">These items require **write** access toohello **App Service plan** that corresponds tooyour website:</span></span>  

* <span data-ttu-id="7eb63-133">Megtekintés hello webalkalmazás tartozó IP-címek (ingyenes vagy normál)</span><span class="sxs-lookup"><span data-stu-id="7eb63-133">Viewing hello web app’s pricing tier (Free or Standard)</span></span>  
* <span data-ttu-id="7eb63-134">Skála configuration (a példányok, a virtuális gép méretét, az automatikus skálázási beállítások száma)</span><span class="sxs-lookup"><span data-stu-id="7eb63-134">Scale configuration (number of instances, virtual machine size, autoscale settings)</span></span>  
* <span data-ttu-id="7eb63-135">Kvóták (tároló, a sávszélesség, a CPU)</span><span class="sxs-lookup"><span data-stu-id="7eb63-135">Quotas (storage, bandwidth, CPU)</span></span>  

<span data-ttu-id="7eb63-136">Ezek az elemek szükség **írási** teljes hozzáférési toohello **erőforráscsoport** , amely tartalmazza a webhely:</span><span class="sxs-lookup"><span data-stu-id="7eb63-136">These items require **write** access toohello whole **Resource group** that contains your website:</span></span>  

* <span data-ttu-id="7eb63-137">SSL-tanúsítványok és kötések (hello a helyek közötti SSL-tanúsítványok megoszthatók ugyanazt az erőforráscsoportot és földrajzi helyhez)</span><span class="sxs-lookup"><span data-stu-id="7eb63-137">SSL Certificates and bindings (SSL certificates can be shared between sites in hello same resource group and geo-location)</span></span>  
* <span data-ttu-id="7eb63-138">A riasztási szabályok</span><span class="sxs-lookup"><span data-stu-id="7eb63-138">Alert rules</span></span>  
* <span data-ttu-id="7eb63-139">automatikus skálázási beállításokat</span><span class="sxs-lookup"><span data-stu-id="7eb63-139">Autoscale settings</span></span>  
* <span data-ttu-id="7eb63-140">Application insights összetevőinek</span><span class="sxs-lookup"><span data-stu-id="7eb63-140">Application insights components</span></span>  
* <span data-ttu-id="7eb63-141">Webteszt</span><span class="sxs-lookup"><span data-stu-id="7eb63-141">Web tests</span></span>  

## <a name="virtual-machine-workloads"></a><span data-ttu-id="7eb63-142">Virtuális gépek terheléséhez</span><span class="sxs-lookup"><span data-stu-id="7eb63-142">Virtual machine workloads</span></span>
<span data-ttu-id="7eb63-143">Sokkal például a web apps, bizonyos funkcióinak hello virtuális gépek paneljét szükségük van írási toohello virtuális gép, illetve tooother erőforrások hello erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="7eb63-143">Much like with web apps, some features on hello virtual machine blade require write access toohello virtual machine, or tooother resources in hello resource group.</span></span>

<span data-ttu-id="7eb63-144">Virtuális gépek a kapcsolódó tooDomain nevek, virtuális hálózatok, storage-fiókok és a riasztási szabályok.</span><span class="sxs-lookup"><span data-stu-id="7eb63-144">Virtual machines are related tooDomain names, virtual networks, storage accounts, and alert rules.</span></span>

<span data-ttu-id="7eb63-145">Ezek az elemek szükség **írási** toohello hozzáférési **virtuális gép**:</span><span class="sxs-lookup"><span data-stu-id="7eb63-145">These items require **write** access toohello **Virtual machine**:</span></span>

* <span data-ttu-id="7eb63-146">Végpontok</span><span class="sxs-lookup"><span data-stu-id="7eb63-146">Endpoints</span></span>  
* <span data-ttu-id="7eb63-147">IP-címek</span><span class="sxs-lookup"><span data-stu-id="7eb63-147">IP addresses</span></span>  
* <span data-ttu-id="7eb63-148">Lemezek</span><span class="sxs-lookup"><span data-stu-id="7eb63-148">Disks</span></span>  
* <span data-ttu-id="7eb63-149">Bővítmények</span><span class="sxs-lookup"><span data-stu-id="7eb63-149">Extensions</span></span>  

<span data-ttu-id="7eb63-150">Szükséges **írási** hozzáférés tooboth hello **virtuális gép**, és hello **erőforráscsoport** (együtt hello nevét), hogy az informatikai van:</span><span class="sxs-lookup"><span data-stu-id="7eb63-150">These require **write** access tooboth hello **Virtual machine**, and hello **Resource group** (along with hello Domain name) that it is in:</span></span>  

* <span data-ttu-id="7eb63-151">Rendelkezésre állási csoport</span><span class="sxs-lookup"><span data-stu-id="7eb63-151">Availability set</span></span>  
* <span data-ttu-id="7eb63-152">Elosztott terhelésű készlethez</span><span class="sxs-lookup"><span data-stu-id="7eb63-152">Load balanced set</span></span>  
* <span data-ttu-id="7eb63-153">A riasztási szabályok</span><span class="sxs-lookup"><span data-stu-id="7eb63-153">Alert rules</span></span>  

<span data-ttu-id="7eb63-154">Ha sem tudja már használni ezen csempék, kérje meg a rendszergazdát, közreműködő hozzáférés toohello erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="7eb63-154">If you can't access any of these tiles, ask your administrator for Contributor access toohello Resource group.</span></span>

## <a name="see-more"></a><span data-ttu-id="7eb63-155">Részletek</span><span class="sxs-lookup"><span data-stu-id="7eb63-155">See more</span></span>
* <span data-ttu-id="7eb63-156">[Szerepköralapú hozzáférés-vezérlés](role-based-access-control-configure.md): első lépések az RBAC a hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="7eb63-156">[Role Based Access Control](role-based-access-control-configure.md): Get started with RBAC in hello Azure portal.</span></span>
* <span data-ttu-id="7eb63-157">[Beépített szerepkörök](role-based-access-built-in-roles.md): részletes információkat szolgáltatva hello szerepköröket, az RBAC szabványos tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7eb63-157">[Built-in roles](role-based-access-built-in-roles.md): Get details about hello roles that come standard in RBAC.</span></span>
* <span data-ttu-id="7eb63-158">[Egyéni szerepkörök az Azure RBAC](role-based-access-control-custom-roles.md): megtudhatja, hogyan toocreate egyéni szerepkörök toofit hozzáférését kell.</span><span class="sxs-lookup"><span data-stu-id="7eb63-158">[Custom roles in Azure RBAC](role-based-access-control-custom-roles.md): Learn how toocreate custom roles toofit your access needs.</span></span>
* <span data-ttu-id="7eb63-159">[Access módosítási előzményeit jelentés létrehozása](role-based-access-control-access-change-history-report.md): nyomon követjük, hogy az RBAC más szerepkörök hozzárendeléséről.</span><span class="sxs-lookup"><span data-stu-id="7eb63-159">[Create an access change history report](role-based-access-control-access-change-history-report.md): Keep track of changing role assignments in RBAC.</span></span>

