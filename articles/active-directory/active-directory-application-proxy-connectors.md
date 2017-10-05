---
title: "Klasszikus portál Azure AD alkalmazás Proxy összekötők |} Microsoft Docs"
description: "Bemutatja, hogyan adhat az Azure AD alkalmazásproxy összekötők csoportok létrehozásához és kezeléséhez."
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
ms.openlocfilehash: fc65c4053c45d9c16c62ee0fe65924133a4bb94a
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups"></a><span data-ttu-id="43121-103">Külön hálózatok és helyek összekötő csoportokat használnak az alkalmazások közzététele</span><span class="sxs-lookup"><span data-stu-id="43121-103">Publish applications on separate networks and locations using connector groups</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="43121-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="43121-104">Azure portal</span></span>](active-directory-application-proxy-connectors-azure-portal.md)
> * [<span data-ttu-id="43121-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="43121-105">Azure classic portal</span></span>](active-directory-application-proxy-connectors.md)
>
>

<span data-ttu-id="43121-106">Összekötő csoportok hasznosak több, különböző esetekre, beleértve:</span><span class="sxs-lookup"><span data-stu-id="43121-106">Connector groups are useful for various scenarios, including:</span></span>

* <span data-ttu-id="43121-107">A több összekapcsolt adatközpontokkal helyek.</span><span class="sxs-lookup"><span data-stu-id="43121-107">Sites with multiple interconnected datacenters.</span></span> <span data-ttu-id="43121-108">Ebben az esetben meg szeretné tartani az adatközponton belül mértékű forgalom lehető mert kereszt-datacenter hivatkozások drága, és a lassú.</span><span class="sxs-lookup"><span data-stu-id="43121-108">In this case, you want to keep as much traffic within the datacenter as possible because cross-datacenter links are expensive and slow.</span></span> <span data-ttu-id="43121-109">Csak az adatközponton belül a alkalmazásokhoz kiszolgálására mindkét adatközpont összekötők telepítése.</span><span class="sxs-lookup"><span data-stu-id="43121-109">You can deploy connectors in each datacenter to serve only the applications that reside within the datacenter.</span></span> <span data-ttu-id="43121-110">Ezt a módszert minimálisra csökkenti a kereszt-datacenter hivatkozásokat, és teljes mértékben transzparens élményt nyújt a felhasználók számára.</span><span class="sxs-lookup"><span data-stu-id="43121-110">This approach minimizes cross-datacenter links and provides an entirely transparent experience to your users.</span></span>
* <span data-ttu-id="43121-111">Elszigetelt hálózatokban, amelyek nem a fő vállalati hálózat részei a telepített alkalmazások kezelése.</span><span class="sxs-lookup"><span data-stu-id="43121-111">Managing applications installed on isolated networks that are not part of the main corporate network.</span></span> <span data-ttu-id="43121-112">Összekötő csoportok segítségével dedikált összekötők telepítése is különítheti el a hálózati alkalmazások elkülönített hálózatokon.</span><span class="sxs-lookup"><span data-stu-id="43121-112">You can use connector groups to install dedicated connectors on isolated networks to also isolate applications to the network.</span></span>
* <span data-ttu-id="43121-113">Összekötő csoportok felhőalapú férhetnek hozzá az infrastruktúra-szolgáltatási a telepített alkalmazások, adja meg egy közös szolgáltatás a hozzáférés az alkalmazásokhoz.</span><span class="sxs-lookup"><span data-stu-id="43121-113">For applications installed on IaaS for cloud access, connector groups provide a common service to secure the access to all the apps.</span></span> <span data-ttu-id="43121-114">Összekötő csoportok nem további függőség létrehozása a vállalati hálózaton, vagy a felhasználói élményt darabolható.</span><span class="sxs-lookup"><span data-stu-id="43121-114">Connector groups don't create additional dependency on your corporate network, or fragment the app experience.</span></span> <span data-ttu-id="43121-115">Összekötők minden felhőbeli adatközpontot is telepíthető, és csak ezen a hálózaton lévő alkalmazások kiszolgálására.</span><span class="sxs-lookup"><span data-stu-id="43121-115">Connectors can be installed on every cloud datacenter and serve only applications that reside in this network.</span></span> <span data-ttu-id="43121-116">Magas rendelkezésre állás biztosítása érdekében több összekötő is telepítheti.</span><span class="sxs-lookup"><span data-stu-id="43121-116">You can install several connectors to achieve high availability.</span></span>
* <span data-ttu-id="43121-117">Többerdős környezetben, amelyben adott összekötők erdőnként telepítése és beállítása az adott alkalmazások kiszolgálására támogatása.</span><span class="sxs-lookup"><span data-stu-id="43121-117">Support for multi-forest environments in which specific connectors can be deployed per forest and set to serve specific applications.</span></span>
* <span data-ttu-id="43121-118">Összekötő csoportok használhatók vész-helyreállítási helyeken vagy a feladatátvételi észlelni, vagy biztonsági másolat az elsődleges hely.</span><span class="sxs-lookup"><span data-stu-id="43121-118">Connector groups can be used in Disaster Recovery sites to either detect failover or as backup for the main site.</span></span>
* <span data-ttu-id="43121-119">Összekötő csoportok több vállalatot kiszolgálására egyetlen bérlőtől is használható.</span><span class="sxs-lookup"><span data-stu-id="43121-119">Connector groups can also be used to serve multiple companies from a single tenant.</span></span>

## <a name="prerequisite-create-your-connectors"></a><span data-ttu-id="43121-120">Előfeltétel: Az összekötő létrehozása</span><span class="sxs-lookup"><span data-stu-id="43121-120">Prerequisite: Create your connectors</span></span>
<span data-ttu-id="43121-121">Az összekötők csoportosításához [több összekötők telepítése](active-directory-application-proxy-enable.md), majd a nevet, és csoportosításához.</span><span class="sxs-lookup"><span data-stu-id="43121-121">To group your connectors, [install multiple connectors](active-directory-application-proxy-enable.md), then name and group them.</span></span> <span data-ttu-id="43121-122">Végül kell rendelnie őket adott alkalmazásokra.</span><span class="sxs-lookup"><span data-stu-id="43121-122">Finally you have to assign them to specific apps.</span></span>

## <a name="step-1-create-connector-groups"></a><span data-ttu-id="43121-123">1. lépés: Az összekötő csoportok létrehozása</span><span class="sxs-lookup"><span data-stu-id="43121-123">Step 1: Create connector groups</span></span>
<span data-ttu-id="43121-124">Összekötő csoportot hozhat létre.</span><span class="sxs-lookup"><span data-stu-id="43121-124">You can create as many connector groups as you want.</span></span> <span data-ttu-id="43121-125">Összekötő-csoport létrehozása a klasszikus Azure portálon hajtható végre.</span><span class="sxs-lookup"><span data-stu-id="43121-125">Connector group creation is accomplished in the Azure classic portal.</span></span>

1. <span data-ttu-id="43121-126">Válassza ki azt a címtárat, és kattintson a **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="43121-126">Select your directory and click **Configure**.</span></span>  
    <span data-ttu-id="43121-127">![Alkalmazásproxy, konfigurálja a képernyőfelvétel - kattintson összekötő csoportok kezelése](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)</span><span class="sxs-lookup"><span data-stu-id="43121-127">![Application proxy, configure screenshot - click manage connector groups](./media/active-directory-application-proxy-connectors/app_proxy_connectors_creategroup.png)</span></span>
2. <span data-ttu-id="43121-128">Az alkalmazásproxy, kattintson **összekötő csoportok kezelése** , és hozzon létre egy összekötő csoport azzal, hogy a csoport nevét.</span><span class="sxs-lookup"><span data-stu-id="43121-128">Under Application Proxy, click **Manage Connector Groups** and create a connector group by giving the group a name.</span></span>  
    <span data-ttu-id="43121-129">![Application proxy connector csoportok képernyőfelvétel - új csoport neve](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)</span><span class="sxs-lookup"><span data-stu-id="43121-129">![Application proxy connector groups screenshot - name new group](./media/active-directory-application-proxy-connectors/app_proxy_connectors_namegroup.png)</span></span>

## <a name="step-2-assign-connectors-to-your-groups"></a><span data-ttu-id="43121-130">2. lépés: A csoportok összekötők hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="43121-130">Step 2: Assign connectors to your groups</span></span>
<span data-ttu-id="43121-131">Az összekötő csoportok jönnek létre, ha az összekötők áthelyezése a megfelelő csoporthoz.</span><span class="sxs-lookup"><span data-stu-id="43121-131">Once the connector groups are created, move the connectors to the appropriate group.</span></span>

1. <span data-ttu-id="43121-132">A **alkalmazásproxy**, kattintson a **összekötők kezelése**.</span><span class="sxs-lookup"><span data-stu-id="43121-132">Under **Application Proxy**, click **Manage Connectors**.</span></span>
2. <span data-ttu-id="43121-133">A **csoport**, válassza ki a csoportot, minden-összekötőhöz.</span><span class="sxs-lookup"><span data-stu-id="43121-133">Under **Group**, select the group you want for each connector.</span></span> <span data-ttu-id="43121-134">Az összekötők válik az új csoport aktív legfeljebb 10 percig is eltarthat.</span><span class="sxs-lookup"><span data-stu-id="43121-134">It might take the connectors up to 10 minutes to become active in the new group.</span></span>  
    <span data-ttu-id="43121-135">![Application proxy összekötők képernyőfelvétel - legördülő menüből válassza csoport](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)</span><span class="sxs-lookup"><span data-stu-id="43121-135">![Application proxy connectors screenshot - select group from dropdown menu](./media/active-directory-application-proxy-connectors/app_proxy_connectors_connectorlist.png)</span></span>

## <a name="step-3-assign-applications-to-your-connector-groups"></a><span data-ttu-id="43121-136">3. lépés: Az összekötő-csoportokat alkalmazások hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="43121-136">Step 3: Assign applications to your connector groups</span></span>
<span data-ttu-id="43121-137">Az utolsó lépést is, hogy az összekötő csoporthoz, azt látja, hogy minden alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="43121-137">The last step is to set each application to the connector group that serves it.</span></span>

1. <span data-ttu-id="43121-138">A klasszikus Azure portálon, a könyvtárban, válassza ki az alkalmazást, rendelje hozzá a csoporthoz, és kattintson a kívánt **konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="43121-138">In the Azure classic portal, in your directory, select the Application you want to assign to the group and click **Configure**.</span></span>
2. <span data-ttu-id="43121-139">A **összekötő csoport**, válassza ki a használni kívánt alkalmazást a csoportot.</span><span class="sxs-lookup"><span data-stu-id="43121-139">Under **Connector group**, select the group you want the application to use.</span></span> <span data-ttu-id="43121-140">Ez a változás azonnal lesz alkalmazva.</span><span class="sxs-lookup"><span data-stu-id="43121-140">This change is immediately applied.</span></span>  
    <span data-ttu-id="43121-141">![Application proxy connector csoport képernyőfelvétel - legördülő menüből válassza csoport](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)</span><span class="sxs-lookup"><span data-stu-id="43121-141">![Application proxy connector group screenshot - select group from dropdown menu](./media/active-directory-application-proxy-connectors/app_proxy_connectors_newgroup.png)</span></span>

## <a name="see-also"></a><span data-ttu-id="43121-142">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="43121-142">See also</span></span>
* [<span data-ttu-id="43121-143">Alkalmazásproxy engedélyezése</span><span class="sxs-lookup"><span data-stu-id="43121-143">Enable Application Proxy</span></span>](active-directory-application-proxy-enable.md)
* [<span data-ttu-id="43121-144">Egyszeri bejelentkezés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="43121-144">Enable single-sign on</span></span>](active-directory-application-proxy-sso-using-kcd.md)
* [<span data-ttu-id="43121-145">Feltételes hozzáférés engedélyezése</span><span class="sxs-lookup"><span data-stu-id="43121-145">Enable conditional access</span></span>](active-directory-application-proxy-conditional-access.md)
* [<span data-ttu-id="43121-146">Az alkalmazásproxy problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="43121-146">Troubleshoot issues you're having with Application Proxy</span></span>](active-directory-application-proxy-troubleshoot.md)

<span data-ttu-id="43121-147">A legújabb híreket és frissítéseket itt találja: [Alkalmazásproxy blog](http://blogs.technet.com/b/applicationproxyblog/).</span><span class="sxs-lookup"><span data-stu-id="43121-147">For the latest news and updates, check out the [Application Proxy blog](http://blogs.technet.com/b/applicationproxyblog/)</span></span>
