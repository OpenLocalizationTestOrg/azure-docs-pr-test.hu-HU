---
title: "a felhő a helyszíni identitás-infrastruktúra hello aaaMonitor."
description: "Ez a hello Azure AD Connect Health lap, amely leírja, mi az, és ezért használhatja azt."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 82798ea6-5cd3-4f30-93ae-d56536f8d8e3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 84d0b00ec800ba98094343731aa4e7317dfb0c61
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-your-on-premises-identity-infrastructure-and-synchronization-services-in-hello-cloud"></a><span data-ttu-id="b9493-103">A helyszíni identitás infrastruktúra és a szinkronizálási szolgáltatások hello felhő figyelése</span><span class="sxs-lookup"><span data-stu-id="b9493-103">Monitor your on-premises identity infrastructure and synchronization services in hello cloud</span></span>
<span data-ttu-id="b9493-104">Az Azure Active Directory (Azure AD) Connect Health segít a figyelheti, és betekintést nyerhet a helyszíni identitás-infrastruktúra és hello szinkronizálási szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="b9493-104">Azure Active Directory (Azure AD) Connect Health helps you monitor and gain insights into your on-premises identity infrastructure and hello synchronization services.</span></span> <span data-ttu-id="b9493-105">Lehetővé teszi a megbízható kapcsolatot tooOffice 365 toomaintain és a Microsoft Online Services figyelési lehetőségek körét biztosítva a fontos identitás-összetevők, például az Active Directory összevonási szolgáltatások (AD FS) kiszolgálók, az Azure AD Connect-kiszolgálók (más néven a szinkronizálási motor), mint az Active Directory-tartományvezérlők stb. Lesz hello kulcs adatpontok kapcsolatos összetevők könnyen elérhető, így kaphat a használati és egyéb fontos insights toomake küldjenek döntéseket.</span><span class="sxs-lookup"><span data-stu-id="b9493-105">It enables you toomaintain a reliable connection tooOffice 365 and Microsoft Online Services by providing monitoring capabilities for your key identity components such as Active Directory Federation Services (AD FS) servers, Azure AD Connect servers (also known as Sync Engine), Active Directory domain controllers, etc. It also makes hello key data points about these components easily accessible so that you can get usage and other important insights toomake informed decisions.</span></span>

<span data-ttu-id="b9493-106">hello információt az hello [Azure AD Connect Health portálon](https://aka.ms/aadconnecthealth).</span><span class="sxs-lookup"><span data-stu-id="b9493-106">hello information is presented in hello [Azure AD Connect Health portal](https://aka.ms/aadconnecthealth).</span></span> <span data-ttu-id="b9493-107">Hello Azure AD Connect Health portálon megtekintheti a riasztásokat, teljesítményfigyelési, használatelemzési információkat és más információkat.</span><span class="sxs-lookup"><span data-stu-id="b9493-107">In hello Azure AD Connect Health portal, you can view alerts, performance monitoring, usage analytics, and other information.</span></span> <span data-ttu-id="b9493-108">Az Azure AD Connect Health lehetővé teszi, hogy a hello egyetlen helyre gyűjti a legfontosabb identitás-összetevők, egy helyen.</span><span class="sxs-lookup"><span data-stu-id="b9493-108">Azure AD Connect Health enables hello single lens of health for your key identity components in one place.</span></span>

![Mi az az Azure AD Connect Health?](./media/active-directory-aadconnect-health/aadconnecthealth2.png)

<span data-ttu-id="b9493-110">Az Azure AD Connect Health szolgáltatásai hello növelése érdekében hello portálon keresztül hello fókuszban identitás egyazon irányítópultra biztosít.</span><span class="sxs-lookup"><span data-stu-id="b9493-110">As hello features in Azure AD Connect Health increase, hello portal provides a single dashboard through hello lens of identity.</span></span> <span data-ttu-id="b9493-111">Ön munkavégzésre egy még inkább robusztus, kifogástalan állapotú és integrált környezetet a a felhasználók tooincrease azok képességét tooget.</span><span class="sxs-lookup"><span data-stu-id="b9493-111">You get an even more robust, healthy, and integrated environment for your users tooincrease their ability tooget things done.</span></span>

## <a name="why-use-azure-ad-connect-health"></a><span data-ttu-id="b9493-112">Miért érdemes az Azure AD Connect Health eszközt használni?</span><span class="sxs-lookup"><span data-stu-id="b9493-112">Why use Azure AD Connect Health?</span></span>
<span data-ttu-id="b9493-113">Az Azure AD integrálása a helyszíni címtárak, amikor a felhasználók is hatékonyabban dolgozhasson, mert nincs közös identitás tooaccess mind a felhőalapú és helyszíni erőforrások.</span><span class="sxs-lookup"><span data-stu-id="b9493-113">When you integrate your on-premises directories with Azure AD, your users are more productive because there's a common identity tooaccess both cloud and on-premises resources.</span></span> <span data-ttu-id="b9493-114">Ez az integráció azonban hello kihívást állapotának biztosítása, hogy ebben a környezetben, hogy a felhasználók bármely eszközről megbízható hozzáférése a helyszíni és az olyan hello felhőalapú erőforrások hoz létre.</span><span class="sxs-lookup"><span data-stu-id="b9493-114">However, this integration creates hello challenge of ensuring that this environment is healthy so that users can reliably access resources both on premises and in hello cloud from any device.</span></span> <span data-ttu-id="b9493-115">Az Azure AD Connect Health segít a figyelheti, és betekintést nyerhet a helyszíni identitás-infrastruktúra, amely használt tooaccess Office 365 vagy más Azure AD-alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="b9493-115">Azure AD Connect Health helps you monitor and gain insights into your on-premises identity infrastructure that is used tooaccess Office 365 or other Azure AD applications.</span></span> <span data-ttu-id="b9493-116">Használata éppolyan egyszerű, mintha egy-egy ügynököt telepítene a helyszíni identitás-kiszolgálókra.</span><span class="sxs-lookup"><span data-stu-id="b9493-116">It is as simple as installing an agent on each of your on-premises identity servers.</span></span>

## <a name="azure-ad-connect-health-for-ad-fsactive-directory-aadconnect-health-adfsmd"></a>[<span data-ttu-id="b9493-117">Azure AD Connect Health for AD FS</span><span class="sxs-lookup"><span data-stu-id="b9493-117">Azure AD Connect Health for AD FS</span></span>](active-directory-aadconnect-health-adfs.md)
<span data-ttu-id="b9493-118">Az AD FS-hez készült Azure AD Connect Health a Windows Server 2008 R2, a Windows Server 2012 és a Windows Server 2012 R2 rendszeren támogatja az AD FS 2.0-s verzióját.</span><span class="sxs-lookup"><span data-stu-id="b9493-118">Azure AD Connect Health for AD FS supports AD FS 2.0 on Windows Server 2008 R2, Windows Server 2012, and Windows Server 2012 R2.</span></span> <span data-ttu-id="b9493-119">Figyelési hello AD FS proxy is támogatja, vagy, hogy hitelesítésre webalkalmazás-proxy kiszolgálók támogatása extranetes elérését.</span><span class="sxs-lookup"><span data-stu-id="b9493-119">It also supports monitoring hello AD FS proxy or web application proxy servers that provide authentication support for extranet access.</span></span> <span data-ttu-id="b9493-120">A könnyű és alacsony költségű hello Health-ügynök a telepítést az Azure AD Connect Health AD FS legfontosabb képességei a következő hello biztosítja:</span><span class="sxs-lookup"><span data-stu-id="b9493-120">With an easy and low-cost installation of hello Health Agent, Azure AD Connect Health for AD FS provides hello following set of key capabilities:</span></span>

* <span data-ttu-id="b9493-121">Figyelés a riasztások tooknow, ha az AD FS és az AD FS-proxykiszolgálók nem megfelelő</span><span class="sxs-lookup"><span data-stu-id="b9493-121">Monitoring with alerts tooknow when AD FS and AD FS proxy servers are not healthy</span></span>
* <span data-ttu-id="b9493-122">kritikus riasztásokhoz kapcsolódó e-mail értesítések;</span><span class="sxs-lookup"><span data-stu-id="b9493-122">Email notifications for critical alerts</span></span>
* <span data-ttu-id="b9493-123">Teljesítményadat-tendenciák, amelyek hasznosak az AD FS kapacitástervezéséhez;</span><span class="sxs-lookup"><span data-stu-id="b9493-123">Trends in performance data, which are useful for capacity planning of AD FS</span></span>
* <span data-ttu-id="b9493-124">Használatelemzés az AD FS bejelentkezések a pivots (alkalmazások, felhasználók, hálózati hely stb.), amelyek hasznos toounderstand, hogy az AD FS módjai</span><span class="sxs-lookup"><span data-stu-id="b9493-124">Usage analytics for AD FS sign-ins with pivots (apps, users, network location etc.), which are useful toounderstand how AD FS is getting utilized</span></span>
* <span data-ttu-id="b9493-125">AD FS-jelentések, például a rossz felhasználónév/jelszó párossal legtöbbet próbálkozó 50 felhasználó, az utolsó IP-címükkel együtt</span><span class="sxs-lookup"><span data-stu-id="b9493-125">Reports for AD FS such as top 50 users who have bad username/password attempts and their last IP address</span></span>

<span data-ttu-id="b9493-126">hello következő videó áttekintést nyújt az Azure AD Connect Health AD FS.</span><span class="sxs-lookup"><span data-stu-id="b9493-126">hello following video provides an overview of Azure AD Connect Health for AD FS.</span></span>

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health--Monitor-you-identity-bridge/player]
>
>

## <a name="azure-ad-connect-health-for-syncactive-directory-aadconnect-health-syncmd"></a>[<span data-ttu-id="b9493-127">Azure AD Connect Health szinkronizálási szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="b9493-127">Azure AD Connect Health for sync</span></span>](active-directory-aadconnect-health-sync.md)
<span data-ttu-id="b9493-128">Az Azure AD Connect Health szinkronizálási szolgáltatás figyeli, és hello szinkronizálások között a helyszíni Active Directory és az Azure AD tájékoztatást.</span><span class="sxs-lookup"><span data-stu-id="b9493-128">Azure AD Connect Health for sync monitors and provides information about hello syncs that occur between your on-premises Active Directory and Azure AD.</span></span> <span data-ttu-id="b9493-129">Az Azure AD Connect Health for sync legfontosabb képességei a következő hello biztosítja:</span><span class="sxs-lookup"><span data-stu-id="b9493-129">Azure AD Connect Health for sync provides hello following set of key capabilities:</span></span>

* <span data-ttu-id="b9493-130">Figyelés a riasztások tooknow, amikor az Azure AD Connect-kiszolgáló, más néven hello szinkronizálási motor állapota nem kifogástalan</span><span class="sxs-lookup"><span data-stu-id="b9493-130">Monitoring with alerts tooknow when an Azure AD Connect server, also known as hello Sync Engine, is not healthy</span></span>
* <span data-ttu-id="b9493-131">kritikus riasztásokhoz kapcsolódó e-mail értesítések;</span><span class="sxs-lookup"><span data-stu-id="b9493-131">Email notifications for critical alerts</span></span>
* <span data-ttu-id="b9493-132">A szinkronizálási műveletek elemzései, beleértve a rájuk vonatkozó késési diagramokat és olyan különböző műveletek tendenciáit, mint például a hozzáadások, a frissítések vagy a törlések;</span><span class="sxs-lookup"><span data-stu-id="b9493-132">Sync operational insights, which include latency charts for sync operations and trends in different operations such as adds, updates, deletes</span></span>
* <span data-ttu-id="b9493-133">Gyors áttekintő információkat a szinkronizálási tulajdonságok és az utolsó sikeres tooAzure AD exportálása</span><span class="sxs-lookup"><span data-stu-id="b9493-133">Quick glance information about sync properties and last successful export tooAzure AD</span></span>
* <span data-ttu-id="b9493-134">Az objektumszintű szinkronizációs hibákról való jelentésekhez \(nem szükséges Prémium szintű Microsoft Azure AD\)</span><span class="sxs-lookup"><span data-stu-id="b9493-134">Reports about object-level sync errors \(does not require Azure AD Premium\)</span></span>

<span data-ttu-id="b9493-135">hello következő videó áttekintést nyújt az Azure AD Connect Health szinkronizálási szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b9493-135">hello following video provides an overview of Azure AD Connect Health for sync.</span></span>

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-Active-Directory-Connect-Health-Monitoring-the-sync-engine/player]
>
>

## <a name="azure-ad-connect-health-for-ad-dsactive-directory-aadconnect-health-addsmd"></a>[<span data-ttu-id="b9493-136">Azure AD Connect Health for AD DS</span><span class="sxs-lookup"><span data-stu-id="b9493-136">Azure AD Connect Health for AD DS</span></span>](active-directory-aadconnect-health-adds.md)
<span data-ttu-id="b9493-137">Az Active Directory Domain Serviceshez (AD DS) készült Azure AD Connect Health a Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2 vagy Windows Server 2016 rendszeren telepített tartományvezérlők figyelését teszi lehetővé.</span><span class="sxs-lookup"><span data-stu-id="b9493-137">Azure AD Connect Health for Active Directory Domain Services (AD DS) provides monitoring for domain controllers that are installed on Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, and Windows Server 2016.</span></span> <span data-ttu-id="b9493-138">hello Health-ügynök telepítése lehetővé teszi, hogy Ön toomonitor a helyszíni AD DS-környezet hello felhőből.</span><span class="sxs-lookup"><span data-stu-id="b9493-138">hello Health Agent installation enables you toomonitor your on-premises AD DS environment from hello cloud.</span></span> <span data-ttu-id="b9493-139">Az Active Directory tartományi szolgáltatások az Azure AD Connect Health legfontosabb képességei a következő hello biztosítja:</span><span class="sxs-lookup"><span data-stu-id="b9493-139">Azure AD Connect Health for AD DS provides hello following set of key capabilities:</span></span>

* <span data-ttu-id="b9493-140">Figyelési értesítések toodetect tartományvezérlők nem kifogástalan állapotú, és az e-mail értesítések a kritikus riasztások</span><span class="sxs-lookup"><span data-stu-id="b9493-140">Monitoring alerts toodetect when domain controllers are unhealthy and email notifications for critical alerts</span></span>
* <span data-ttu-id="b9493-141">hello tartományvezérlők irányítópult, ami hello állapotának gyors áttekintést és működési állapotát a tartományvezérlőket.</span><span class="sxs-lookup"><span data-stu-id="b9493-141">hello Domain Controllers dashboard, which provides a quick view of hello health and operational status of your domain controllers</span></span>
* <span data-ttu-id="b9493-142">hello replikációs állapota irányítópult hello replikációs szolgáltatáscsomagjaival és tootroubleshooting útmutatók hivatkozásait, amikor a rendszer hibát észlel</span><span class="sxs-lookup"><span data-stu-id="b9493-142">hello Replication Status dashboard that has hello latest replication information and links tootroubleshooting guides when errors are detected</span></span>
* <span data-ttu-id="b9493-143">Gyors helyfüggetlen hozzáférés tooperformance adatok diagramjait népszerű teljesítményszámlálók, hibaelhárítási és ellenőrzés céljából szükséges</span><span class="sxs-lookup"><span data-stu-id="b9493-143">Quick anywhere access tooperformance data graphs of popular performance counters, which are necessary for troubleshooting and monitoring purposes</span></span>

<span data-ttu-id="b9493-144">hello következő videó áttekintést nyújt az Azure AD Connect Health az Active Directory tartományi Szolgáltatásokban.</span><span class="sxs-lookup"><span data-stu-id="b9493-144">hello following video provides an overview of Azure AD Connect Health for AD DS.</span></span>

> [!VIDEO https://channel9.msdn.com/Series/Azure-Active-Directory-Videos-Demos/Azure-AD-Connect-Health-monitors-on-premises-AD-Domain-Services/player]
>
>

## <a name="get-started-with-azure-ad-connect-health"></a><span data-ttu-id="b9493-145">Az Azure AD Connect Health használatának első lépései</span><span class="sxs-lookup"><span data-stu-id="b9493-145">Get started with Azure AD Connect Health</span></span>
<span data-ttu-id="b9493-146">tooget lépések az Azure AD Connect Health, használja hello a következő lépéseket:</span><span class="sxs-lookup"><span data-stu-id="b9493-146">tooget started with Azure AD Connect Health, use hello following steps:</span></span>

1. <span data-ttu-id="b9493-147">[Szerezze be a Prémium szintű Azure AD-t](../active-directory-get-started-premium.md) vagy [indítson egy próbaverziót](https://azure.microsoft.com/trial/get-started-active-directory/).</span><span class="sxs-lookup"><span data-stu-id="b9493-147">[Get Azure AD Premium](../active-directory-get-started-premium.md) or [start a trial](https://azure.microsoft.com/trial/get-started-active-directory/).</span></span>
2. <span data-ttu-id="b9493-148">[Töltse le és telepítse az Azure AD Connect Health-ügynököket](#download-and-install-azure-ad-connect-health-agent) az identitás-kiszolgálókra.</span><span class="sxs-lookup"><span data-stu-id="b9493-148">[Download and install Azure AD Connect Health Agents](#download-and-install-azure-ad-connect-health-agent) on your identity servers.</span></span>
3. <span data-ttu-id="b9493-149">Nézet hello Azure AD Connect Health irányítópultját a [https://aka.ms/aadconnecthealth](https://aka.ms/aadconnecthealth).</span><span class="sxs-lookup"><span data-stu-id="b9493-149">View hello Azure AD Connect Health dashboard at [https://aka.ms/aadconnecthealth](https://aka.ms/aadconnecthealth).</span></span>

> [!NOTE]
> <span data-ttu-id="b9493-150">Ne feledje, hogy adatokat az Azure AD Connect Health irányítópultján látja, jóvá kell tooinstall hello Azure AD Connect Health-ügynököket a célkiszolgálókon.</span><span class="sxs-lookup"><span data-stu-id="b9493-150">Remember that before you see data in your Azure AD Connect Health dashboard, you need tooinstall hello Azure AD Connect Health Agents on your targeted servers.</span></span>
>
>

## <a name="download-and-install-azure-ad-connect-health-agent"></a><span data-ttu-id="b9493-151">Az Azure AD Connect Health-ügynök letöltése és telepítése</span><span class="sxs-lookup"><span data-stu-id="b9493-151">Download and install Azure AD Connect Health Agent</span></span>
* <span data-ttu-id="b9493-152">Győződjön meg arról, hogy Ön [hello megfelelnek](active-directory-aadconnect-health-agent-install.md#requirements) az Azure AD Connect Health.</span><span class="sxs-lookup"><span data-stu-id="b9493-152">Make sure that you [satisfy hello requirements](active-directory-aadconnect-health-agent-install.md#requirements) for Azure AD Connect Health.</span></span>
* <span data-ttu-id="b9493-153">Ismerkedés az Azure AD Connect Health for AD FS használatával</span><span class="sxs-lookup"><span data-stu-id="b9493-153">Get started using Azure AD Connect Health for AD FS</span></span>
    * [<span data-ttu-id="b9493-154">Töltse le az Azure AD Connect Health-ügynököt az AD FS szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="b9493-154">Download Azure AD Connect Health Agent for AD FS.</span></span>](http://go.microsoft.com/fwlink/?LinkID=518973)
    * <span data-ttu-id="b9493-155">[Hello telepítési utasításokat lásd:](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs).</span><span class="sxs-lookup"><span data-stu-id="b9493-155">[See hello installation instructions](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-fs).</span></span>
* <span data-ttu-id="b9493-156">Ismerkedés az Azure AD Connect Health szinkronizálási szolgáltatás használatával</span><span class="sxs-lookup"><span data-stu-id="b9493-156">Get started using Azure AD Connect Health for sync</span></span>
    * <span data-ttu-id="b9493-157">[Töltse le és telepítse az Azure AD Connect legújabb verziójának hello](http://go.microsoft.com/fwlink/?linkid=615771).</span><span class="sxs-lookup"><span data-stu-id="b9493-157">[Download and install hello latest version of Azure AD Connect](http://go.microsoft.com/fwlink/?linkid=615771).</span></span> <span data-ttu-id="b9493-158">hello rendszerállapot-ügynöke való szinkronizálás hello Azure AD Connect telepítésének részeként telepíti (1.0.9125.0-s vagy újabb).</span><span class="sxs-lookup"><span data-stu-id="b9493-158">hello Health Agent for sync will be installed as part of hello Azure AD Connect installation (version 1.0.9125.0 or higher).</span></span>
* <span data-ttu-id="b9493-159">Ismerkedés az Azure AD Connect Health for AD DS használatával</span><span class="sxs-lookup"><span data-stu-id="b9493-159">Get started using Azure AD Connect Health for AD DS</span></span>
    * <span data-ttu-id="b9493-160">[Töltse le az Azure AD Connect Health-ügynököt az AD DS szolgáltatáshoz](http://go.microsoft.com/fwlink/?LinkID=820540).</span><span class="sxs-lookup"><span data-stu-id="b9493-160">[Download Azure AD Connect Health Agent for AD DS](http://go.microsoft.com/fwlink/?LinkID=820540).</span></span>
    * <span data-ttu-id="b9493-161">[Hello telepítési utasításokat lásd:](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-ds).</span><span class="sxs-lookup"><span data-stu-id="b9493-161">[See hello installation instructions](active-directory-aadconnect-health-agent-install.md#installing-the-azure-ad-connect-health-agent-for-ad-ds).</span></span>

## <a name="azure-ad-connect-health-portal"></a><span data-ttu-id="b9493-162">Az Azure AD Connect Health portál</span><span class="sxs-lookup"><span data-stu-id="b9493-162">Azure AD Connect Health portal</span></span>
<span data-ttu-id="b9493-163">hello Azure AD Connect Health portálon riasztásokat, teljesítményfigyelési feladatokat és használatelemzési információkat tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b9493-163">hello Azure AD Connect Health portal shows views of alerts, performance monitoring, and usage analytics.</span></span> <span data-ttu-id="b9493-164">URL-cím hello https://aka.ms/aadconnecthealth tart az Azure AD Connect Health fő panelje toohello.</span><span class="sxs-lookup"><span data-stu-id="b9493-164">hello  https://aka.ms/aadconnecthealth URL takes you toohello main blade of Azure AD Connect Health.</span></span> <span data-ttu-id="b9493-165">A panelek az ablakoknak megfelelő funkciót töltenek be.</span><span class="sxs-lookup"><span data-stu-id="b9493-165">You can think of a blade as a window.</span></span> <span data-ttu-id="b9493-166">Hello fő paneljén láthatja **gyors üzembe helyezés**, az Azure AD Connect Health, és további konfigurációs lehetőségek belül.</span><span class="sxs-lookup"><span data-stu-id="b9493-166">On hello main blade, you see **Quick Start**, services within Azure AD Connect Health, and additional configuration options.</span></span> <span data-ttu-id="b9493-167">Hello lásd a következő képernyőfelvétel és az alábbi képernyőfelvétel a hello rövid magyarázatot.</span><span class="sxs-lookup"><span data-stu-id="b9493-167">See hello following screenshot and brief explanations that follow hello screenshot.</span></span> <span data-ttu-id="b9493-168">Hello ügynökök telepítése után a hello Állapotfigyelő szolgáltatás automatikusan azonosítja az Azure AD Connect Health által figyelt hello szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="b9493-168">After you deploy hello agents, hello health service automatically identifies hello services that Azure AD Connect Health is monitoring.</span></span>

> [!NOTE]
> <span data-ttu-id="b9493-169">Licencelési információ: hello [az Azure AD Connect – gyakori kérdések](active-directory-aadconnect-health-faq.md) vagy hello [az Azure AD árazás lap](https://aka.ms/aadpricing).</span><span class="sxs-lookup"><span data-stu-id="b9493-169">For licensing information, see hello [Azure AD Connect FAQ](active-directory-aadconnect-health-faq.md) or hello [Azure AD Pricing page](https://aka.ms/aadpricing).</span></span>
    
![Az Azure AD Connect Health portál](./media/active-directory-aadconnect-health/portal4.png)

* <span data-ttu-id="b9493-171">**Gyors üzembe helyezési**: ezt a beállítást, ha hello **gyors üzembe helyezés** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="b9493-171">**Quick Start**: When you select this option, hello **Quick Start** blade opens.</span></span> <span data-ttu-id="b9493-172">Hello Azure AD Connect Health-ügynök kiválasztásával letöltheti **beolvasása eszközök**.</span><span class="sxs-lookup"><span data-stu-id="b9493-172">You can download hello Azure AD Connect Health Agent by selecting **Get Tools**.</span></span> <span data-ttu-id="b9493-173">Emellett hozzáférhet a dokumentációkhoz, és visszajelzést is küldhet.</span><span class="sxs-lookup"><span data-stu-id="b9493-173">You can also access documentation and provide feedback.</span></span>
* <span data-ttu-id="b9493-174">**Active Directory összevonási szolgáltatások**: ezt a lehetőséget az AD FS hello szolgáltatások láthatók, hogy az Azure AD Connect Health által aktuálisan figyelt.</span><span class="sxs-lookup"><span data-stu-id="b9493-174">**Active Directory Federation Services**: This option shows all hello AD FS services that Azure AD Connect Health is currently monitoring.</span></span> <span data-ttu-id="b9493-175">Amikor kiválaszt egy példányát, a hello panelt megnyitó adott szolgáltatáspéldány információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b9493-175">When you select an instance, hello blade that opens shows information about that service instance.</span></span> <span data-ttu-id="b9493-176">Az információk között tulajdonságokat, riasztásokat, megfigyelési adatokat, használatelemzést és egy áttekintést talál.</span><span class="sxs-lookup"><span data-stu-id="b9493-176">This information includes an overview, properties, alerts, monitoring, and usage analytics.</span></span> <span data-ttu-id="b9493-177">További információk a hello képességekről az [használata az Azure AD Connect Health AD FS-sel](active-directory-aadconnect-health-adfs.md).</span><span class="sxs-lookup"><span data-stu-id="b9493-177">Read more about hello capabilities at [Using Azure AD Connect Health with AD FS](active-directory-aadconnect-health-adfs.md).</span></span>
* <span data-ttu-id="b9493-178">**Azure Active Directory Connect (Sync)** ((Szinkronizálási) Azure Active Directory Connect): Ez a lehetőség az Azure AD Connect Health által aktuálisan monitorozott Azure AD Connect-kiszolgálókat mutatja.</span><span class="sxs-lookup"><span data-stu-id="b9493-178">**Azure Active Directory Connect (sync)**: This option shows your Azure AD Connect servers that Azure AD Connect Health is currently monitoring.</span></span> <span data-ttu-id="b9493-179">Hello bejegyzés kiválasztásakor a hello panel, amely megnyitja az Azure AD Connect-kiszolgálók információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b9493-179">When you select hello entry, hello blade that opens shows information about your Azure AD Connect servers.</span></span> <span data-ttu-id="b9493-180">További információk a hello képességekről az [használata Azure AD Connect Health szinkronizálási szolgáltatás](active-directory-aadconnect-health-sync.md).</span><span class="sxs-lookup"><span data-stu-id="b9493-180">Read more about hello capabilities at [Using Azure AD Connect Health for sync](active-directory-aadconnect-health-sync.md).</span></span>
* <span data-ttu-id="b9493-181">**Active Directory tartományi szolgáltatások**: ezt a beállítást jeleníti meg az összes Active Directory tartományi szolgáltatások hello erdők, hogy az Azure AD Connect Health által aktuálisan figyelt.</span><span class="sxs-lookup"><span data-stu-id="b9493-181">**Active Directory Domain Services**: This option shows all hello AD DS forests that Azure AD Connect Health is currently monitoring.</span></span> <span data-ttu-id="b9493-182">Amikor kiválaszt egy erdőben, a hello panelt megnyitó adott erdőben információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b9493-182">When you select a forest, hello blade that opens shows information about that forest.</span></span> <span data-ttu-id="b9493-183">Ezen információk közé tartozik az alapvető információkat, a hello tartományvezérlők irányítópult, a hello replikációs állapota irányítópult, a riasztások és a Figyelés áttekintése.</span><span class="sxs-lookup"><span data-stu-id="b9493-183">This information includes an overview of essential information, hello Domain Controllers dashboard, hello Replication Status dashboard, alerts, and monitoring.</span></span> <span data-ttu-id="b9493-184">További információk a hello képességekről az [használata Azure AD Connect Health használata az Active Directory tartományi szolgáltatások](active-directory-aadconnect-health-adds.md).</span><span class="sxs-lookup"><span data-stu-id="b9493-184">Read more about hello capabilities at [Using Azure AD Connect Health with AD DS](active-directory-aadconnect-health-adds.md).</span></span>
* <span data-ttu-id="b9493-185">**Konfigurálása**: Ebben a részben ismertetett beállítások tooturn hello következő be- és kikapcsolása:</span><span class="sxs-lookup"><span data-stu-id="b9493-185">**Configure**: This section includes options tooturn hello following on or off:</span></span>

  - <span data-ttu-id="b9493-186">Az automatikus frissítés tooautomatically update hello Azure AD Connect Health agent toohello legújabb verziója: hello Azure AD Connect Health-ügynök legújabb verziója automatikusan frissített toohello lesz, amikor azok elérhetővé válnak.</span><span class="sxs-lookup"><span data-stu-id="b9493-186">Auto-update tooautomatically update hello Azure AD Connect Health agent toohello latest version: You will be automatically updated toohello latest versions of hello Azure AD Connect Health Agent when they become available.</span></span> <span data-ttu-id="b9493-187">Ez e beállítás alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="b9493-187">This is enabled by default.</span></span>
  - <span data-ttu-id="b9493-188">Microsoft access tooyour az Azure AD címtár állapotadataihoz engedélyezése hibaelhárítási céllal: Ha ez engedélyezve van, a Microsoft látható hello ugyanazokat az adatokat, amelyek akkor jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="b9493-188">Allow Microsoft access tooyour Azure AD directory’s health data for troubleshooting purposes only: If this is enabled, Microsoft can see hello same data that you see.</span></span> <span data-ttu-id="b9493-189">Ez az információ a hibaelhárítás és a problémakezelés során jelenthet segítséget.</span><span class="sxs-lookup"><span data-stu-id="b9493-189">This information can help with troubleshooting and assistance with issues.</span></span> <span data-ttu-id="b9493-190">A beállítás alapértelmezés szerint le van tiltva.</span><span class="sxs-lookup"><span data-stu-id="b9493-190">This is disabled by default.</span></span>

## <a name="related-links"></a><span data-ttu-id="b9493-191">Kapcsolódó hivatkozások</span><span class="sxs-lookup"><span data-stu-id="b9493-191">Related links</span></span>
* [<span data-ttu-id="b9493-192">Az Azure AD Connect Health-ügynök telepítése</span><span class="sxs-lookup"><span data-stu-id="b9493-192">Azure AD Connect Health Agent installation</span></span>](active-directory-aadconnect-health-agent-install.md)
* [<span data-ttu-id="b9493-193">Az Azure AD Connect Health műveletei</span><span class="sxs-lookup"><span data-stu-id="b9493-193">Azure AD Connect Health operations</span></span>](active-directory-aadconnect-health-operations.md)
* [<span data-ttu-id="b9493-194">Az Azure AD Connect Health használata az AD FS szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="b9493-194">Using Azure AD Connect Health with AD FS</span></span>](active-directory-aadconnect-health-adfs.md)
* [<span data-ttu-id="b9493-195">Az Azure AD Connect Health szinkronizálási szolgáltatás használata</span><span class="sxs-lookup"><span data-stu-id="b9493-195">Using Azure AD Connect Health for sync</span></span>](active-directory-aadconnect-health-sync.md)
* [<span data-ttu-id="b9493-196">Az Azure AD Connect Health használata az AD DS szolgáltatással</span><span class="sxs-lookup"><span data-stu-id="b9493-196">Using Azure AD Connect Health with AD DS</span></span>](active-directory-aadconnect-health-adds.md)
* [<span data-ttu-id="b9493-197">Azure AD Connect Health – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="b9493-197">Azure AD Connect Health FAQ</span></span>](active-directory-aadconnect-health-faq.md)
* [<span data-ttu-id="b9493-198">Az Azure AD Connect Health verzióelőzményei</span><span class="sxs-lookup"><span data-stu-id="b9493-198">Azure AD Connect Health version history</span></span>](active-directory-aadconnect-health-version-history.md)