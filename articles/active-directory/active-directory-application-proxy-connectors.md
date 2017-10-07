---
title: "aaaClassic portál Azure AD alkalmazás Proxy összekötők |} Microsoft Docs"
description: "Magában foglalja az hogyan toocreate és az Azure AD alkalmazásproxy összekötők csoportok kezelése."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: b283796a-9679-4c79-b703-802bb850f65d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro; oldportal
ms.openlocfilehash: 43559b0f4ffc3c7dbbf00901e89ac276d01796e2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a><span data-ttu-id="f0db6-103">Külön hálózatok és helyek összekötő csoportokat használnak az alkalmazások közzététele</span><span class="sxs-lookup"><span data-stu-id="f0db6-103">Publish applications on separate networks and locations using connector groups</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f0db6-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="f0db6-104">Azure portal</span></span>](active-directory-application-proxy-connectors-azure-portal.md)
> * [<span data-ttu-id="f0db6-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="f0db6-105">Azure classic portal</span></span>](active-directory-application-proxy-connectors.md)
>
>

<span data-ttu-id="f0db6-106">Összekötő csoportok hasznosak több, különböző esetekre, beleértve:</span><span class="sxs-lookup"><span data-stu-id="f0db6-106">Connector groups are useful for various scenarios, including:</span></span>

* <span data-ttu-id="f0db6-107">A több összekapcsolt adatközpontokkal helyek.</span><span class="sxs-lookup"><span data-stu-id="f0db6-107">Sites with multiple interconnected datacenters.</span></span> <span data-ttu-id="f0db6-108">Ebben az esetben érdemes tookeep mértékű forgalom hello adatközponton belül a lehető mert kereszt-datacenter hivatkozások drága, és a lassú.</span><span class="sxs-lookup"><span data-stu-id="f0db6-108">In this case, you want tookeep as much traffic within hello datacenter as possible because cross-datacenter links are expensive and slow.</span></span> <span data-ttu-id="f0db6-109">Minden egyes datacenter tooserve csak hello alkalmazásokhoz hello adatközponton belül összekötők telepítése.</span><span class="sxs-lookup"><span data-stu-id="f0db6-109">You can deploy connectors in each datacenter tooserve only hello applications that reside within hello datacenter.</span></span> <span data-ttu-id="f0db6-110">Ez a megközelítés minimálisra csökkenti a kereszt-datacenter hivatkozások, és teljesen átlátszó élményt nyújt akkor tooyour felhasználók.</span><span class="sxs-lookup"><span data-stu-id="f0db6-110">This approach minimizes cross-datacenter links and provides an entirely transparent experience tooyour users.</span></span>
* <span data-ttu-id="f0db6-111">Elszigetelt hálózatokban, amelyek nincsenek hello fő vállalati hálózat részei a telepített alkalmazások kezelése.</span><span class="sxs-lookup"><span data-stu-id="f0db6-111">Managing applications installed on isolated networks that are not part of hello main corporate network.</span></span> <span data-ttu-id="f0db6-112">Összekötő csoportok dedikált tooinstall összekötők elkülönített hálózatok tooalso elkülönítheti alkalmazások toohello hálózaton használható.</span><span class="sxs-lookup"><span data-stu-id="f0db6-112">You can use connector groups tooinstall dedicated connectors on isolated networks tooalso isolate applications toohello network.</span></span>
* <span data-ttu-id="f0db6-113">Összekötő csoportok felhőalapú férhetnek hozzá az infrastruktúra-szolgáltatási a telepített alkalmazások, adja meg a közös szolgáltatás toosecure hello hozzáférés tooall hello alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="f0db6-113">For applications installed on IaaS for cloud access, connector groups provide a common service toosecure hello access tooall hello apps.</span></span> <span data-ttu-id="f0db6-114">Összekötő csoportok nem további függőség létrehozása a vállalati hálózaton, vagy darabolható hello felhasználói élményt.</span><span class="sxs-lookup"><span data-stu-id="f0db6-114">Connector groups don't create additional dependency on your corporate network, or fragment hello app experience.</span></span> <span data-ttu-id="f0db6-115">Összekötők minden felhőbeli adatközpontot is telepíthető, és csak ezen a hálózaton lévő alkalmazások kiszolgálására.</span><span class="sxs-lookup"><span data-stu-id="f0db6-115">Connectors can be installed on every cloud datacenter and serve only applications that reside in this network.</span></span> <span data-ttu-id="f0db6-116">Több összekötők tooachieve magas rendelkezésre állású telepítése.</span><span class="sxs-lookup"><span data-stu-id="f0db6-116">You can install several connectors tooachieve high availability.</span></span>
* <span data-ttu-id="f0db6-117">Többerdős környezetben, amelyben adott összekötők erdőnként telepítése és beállítása tooserve bizonyos alkalmazások támogatása.</span><span class="sxs-lookup"><span data-stu-id="f0db6-117">Support for multi-forest environments in which specific connectors can be deployed per forest and set tooserve specific applications.</span></span>
* <span data-ttu-id="f0db6-118">Összekötő csoportok használhatók a vész-helyreállítási helyeken tooeither feladatátvételi észleli, és mivel a biztonsági mentés hello fő hely.</span><span class="sxs-lookup"><span data-stu-id="f0db6-118">Connector groups can be used in Disaster Recovery sites tooeither detect failover or as backup for hello main site.</span></span>
* <span data-ttu-id="f0db6-119">Összekötő csoportokat is használt tooserve több vállalatot egyetlen bérlőtől kell.</span><span class="sxs-lookup"><span data-stu-id="f0db6-119">Connector groups can also be used tooserve multiple companies from a single tenant.</span></span>

## <a name="prerequisite-create-your-connectors"></a><span data-ttu-id="f0db6-120">Előfeltétel: Az összekötő létrehozása</span><span class="sxs-lookup"><span data-stu-id="f0db6-120">Prerequisite: Create your connectors</span></span>
<span data-ttu-id="f0db6-121">toogroup az összekötők [több összekötők telepítése](active-directory-application-proxy-enable.md), majd a nevet, és csoportosításához.</span><span class="sxs-lookup"><span data-stu-id="f0db6-121">toogroup your connectors, [install multiple connectors](active-directory-application-proxy-enable.md), then name and group them.</span></span> <span data-ttu-id="f0db6-122">Végül rendelkezik tooassign toospecific alkalmazások őket.</span><span class="sxs-lookup"><span data-stu-id="f0db6-122">Finally you have tooassign them toospecific apps.</span></span>

## <a name="step-1-create-connector-groups"></a><span data-ttu-id="f0db6-123">1. lépés: Az összekötő csoportok létrehozása</span><span class="sxs-lookup"><span data-stu-id="f0db6-123">Step 1: Create connector groups</span></span>
<span data-ttu-id="f0db6-124">Összekötő csoportot hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="f0db6-124">You can create as many connector groups as you want.</span></span> <span data-ttu-id="f0db6-125">Összekötő-csoport létrehozása a klasszikus Azure portálon hello valósítható meg.</span><span class="sxs-lookup"><span data-stu-id="f0db6-125">Connector group creation is accomplished in hello Azure classic portal.</span></span>

1. <span data-ttu-id="f0db6-126">Válassza ki azt a címtárat, és kattintson a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="f0db6-126">Select your directory and click **Configure**.</span></span>  
    <span data-ttu-id="f0db6-127">![Alkalmazásproxy, konfigurálja a képernyőfelvétel - kattintson összekötő csoportok kezelése](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)</span><span class="sxs-lookup"><span data-stu-id="f0db6-127">![Application proxy, configure screenshot - click manage connector groups](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)</span></span>
2. <span data-ttu-id="f0db6-128">Az alkalmazásproxy, kattintson **összekötő csoportok kezelése** , és hozzon létre egy összekötő csoport hello csoport adjon egy nevet.</span><span class="sxs-lookup"><span data-stu-id="f0db6-128">Under Application Proxy, click **Manage Connector Groups** and create a connector group by giving hello group a name.</span></span>  
    <span data-ttu-id="f0db6-129">![Application proxy connector csoportok képernyőfelvétel - új csoport neve](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)</span><span class="sxs-lookup"><span data-stu-id="f0db6-129">![Application proxy connector groups screenshot - name new group](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)</span></span>

## <a name="step-2-assign-connectors-tooyour-groups"></a><span data-ttu-id="f0db6-130">2. lépés: Hozzárendelése összekötők tooyour csoportok</span><span class="sxs-lookup"><span data-stu-id="f0db6-130">Step 2: Assign connectors tooyour groups</span></span>
<span data-ttu-id="f0db6-131">Miután hello összekötő csoportokat hoz létre, helyezze át a hello összekötők toohello megfelelő csoportot.</span><span class="sxs-lookup"><span data-stu-id="f0db6-131">Once hello connector groups are created, move hello connectors toohello appropriate group.</span></span>

1. <span data-ttu-id="f0db6-132">A **alkalmazásproxy**, kattintson a **összekötők kezelése**.</span><span class="sxs-lookup"><span data-stu-id="f0db6-132">Under **Application Proxy**, click **Manage Connectors**.</span></span>
2. <span data-ttu-id="f0db6-133">A **csoport**, jelölje be minden összekötőhöz kívánt hello csoport.</span><span class="sxs-lookup"><span data-stu-id="f0db6-133">Under **Group**, select hello group you want for each connector.</span></span> <span data-ttu-id="f0db6-134">Is telhet, mire hello összekötők too10 perc toobecome be aktív hello új csoportba.</span><span class="sxs-lookup"><span data-stu-id="f0db6-134">It might take hello connectors up too10 minutes toobecome active in hello new group.</span></span>  
    <span data-ttu-id="f0db6-135">![Application proxy összekötők képernyőfelvétel - legördülő menüből válassza csoport](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)</span><span class="sxs-lookup"><span data-stu-id="f0db6-135">![Application proxy connectors screenshot - select group from dropdown menu](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)</span></span>

## <a name="step-3-assign-applications-tooyour-connector-groups"></a><span data-ttu-id="f0db6-136">3. lépés: Hozzárendelni tooyour összekötő csoportok</span><span class="sxs-lookup"><span data-stu-id="f0db6-136">Step 3: Assign applications tooyour connector groups</span></span>
<span data-ttu-id="f0db6-137">hello utolsó lépése tooset minden egyes toohello összekötő alkalmazáscsoport azt látja, hogy van.</span><span class="sxs-lookup"><span data-stu-id="f0db6-137">hello last step is tooset each application toohello connector group that serves it.</span></span>

1. <span data-ttu-id="f0db6-138">A klasszikus Azure portálon, a címtárban hello válassza ki, tooassign toohello csoportba, majd kattintson az alkalmazás hello **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="f0db6-138">In hello Azure classic portal, in your directory, select hello Application you want tooassign toohello group and click **Configure**.</span></span>
2. <span data-ttu-id="f0db6-139">A **összekötő csoport**, jelölje be kívánt alkalmazás toouse hello hello csoport.</span><span class="sxs-lookup"><span data-stu-id="f0db6-139">Under **Connector group**, select hello group you want hello application toouse.</span></span> <span data-ttu-id="f0db6-140">Ez a változás azonnal lesz alkalmazva.</span><span class="sxs-lookup"><span data-stu-id="f0db6-140">This change is immediately applied.</span></span>  
    <span data-ttu-id="f0db6-141">![Application proxy connector csoport képernyőfelvétel - legördülő menüből válassza csoport](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)</span><span class="sxs-lookup"><span data-stu-id="f0db6-141">![Application proxy connector group screenshot - select group from dropdown menu](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)</span></span>

## <a name="see-also"></a><span data-ttu-id="f0db6-142">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="f0db6-142">See also</span></span>
* [<span data-ttu-id="f0db6-143">Alkalmazásproxy engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f0db6-143">Enable Application Proxy</span></span>](active-directory-application-proxy-enable.md)
* [<span data-ttu-id="f0db6-144">Egyszeri bejelentkezés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f0db6-144">Enable single-sign on</span></span>](active-directory-application-proxy-sso-using-kcd.md)
* [<span data-ttu-id="f0db6-145">Feltételes hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="f0db6-145">Enable conditional access</span></span>](active-directory-application-proxy-conditional-access.md)
* [<span data-ttu-id="f0db6-146">Az alkalmazásproxy problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="f0db6-146">Troubleshoot issues you're having with Application Proxy</span></span>](active-directory-application-proxy-troubleshoot.md)

<span data-ttu-id="f0db6-147">Hello legfrissebb híreket és frissítéseket, tekintse meg a hello [alkalmazásproxy blog](http://blogs.technet.com/b/applicationproxyblog/)</span><span class="sxs-lookup"><span data-stu-id="f0db6-147">For hello latest news and updates, check out hello [Application Proxy blog](http://blogs.technet.com/b/applicationproxyblog/)</span></span>
