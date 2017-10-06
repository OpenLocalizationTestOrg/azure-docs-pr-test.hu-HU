---
title: "aaaDashboard, a figyelő, a méretezés, a konfigurálás és a hibrid kapcsolatok a BizTalk szolgáltatások |} Microsoft Docs"
description: "További tudnivalók hello vezérlők, a BizTalk szolgáltatások hello klasszikus portál lapokon teljesítmény figyeléséhez: irányítópult, a figyelő, a méretezés, a konfigurálás és a hibrid kapcsolatok. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 7a1815db-0de2-4274-8be0-198c1b077324
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: c9fafdad20489571ee3849bbacd2c2b10933154f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="review-hello-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a><span data-ttu-id="7e908-104">Tekintse át a hello irányítópult, a figyelő, a méretezés, a konfigurálás és a hibrid kapcsolat lapokon</span><span class="sxs-lookup"><span data-stu-id="7e908-104">Review hello Dashboard, Monitor, Scale, Configure, and Hybrid Connection tabs</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="7e908-105">Miután a BizTalk szolgáltatás létrehozása és az alkalmazás központi telepítése, néhány hello BizTalk szolgáltatás beállításainak módosítása, és hello alkalmazás teljesítményének figyelése.</span><span class="sxs-lookup"><span data-stu-id="7e908-105">After you create your BizTalk Service and deploy your application, you can change some of hello BizTalk Service settings and monitor hello application performance.</span></span> 

<span data-ttu-id="7e908-106">A klasszikus Azure portálon hello megnyitásakor automatikusan elhelyezve hello **minden elem** külön-külön tooview a BizTalk szolgáltatás, válassza ki a BizTalk szolgáltatás hello **minden elem** lapon vagy select hello **BIZTALK szolgáltatások** ; lapra, és válassza ki a BizTalk szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="7e908-106">When you open hello Azure classic portal, you are automatically placed at hello **ALL ITEMS** tab. tooview your BizTalk Service, select your BizTalk Service in hello **ALL ITEMS** tab or select hello **BIZTALK SERVICES** tab; and then select your BizTalk Service name.</span></span>

<span data-ttu-id="7e908-107">Ez egy új ablakban nyílik meg a következő lapokon hello.</span><span class="sxs-lookup"><span data-stu-id="7e908-107">This opens a new window with hello following tabs.</span></span> <span data-ttu-id="7e908-108">Ez a témakör ismerteti az ezeken a lapokon.</span><span class="sxs-lookup"><span data-stu-id="7e908-108">This topic describes these tabs.</span></span>

## <a name="quickstart-quickstartquickstart"></a><span data-ttu-id="7e908-109">Gyors üzembe helyezés)</span><span class="sxs-lookup"><span data-stu-id="7e908-109">Quickstart (</span></span>![Első lépések][Quickstart]<span data-ttu-id="7e908-111">)</span><span class="sxs-lookup"><span data-stu-id="7e908-111">)</span></span>
<span data-ttu-id="7e908-112">Hello BizTalk szolgáltatások Edition, attól függően, hogy az összes felsorolt beállítások nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="7e908-112">Depending on hello BizTalk Services Edition, all options listed may not be available.</span></span> 

<table border="1">
    <tr>
        <td><span data-ttu-id="7e908-113"><strong>Hello eszközök beszerzése</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-113"><strong>Get hello tools</strong></span></span></td>
        <td><span data-ttu-id="7e908-114">Töltse le a hello BizTalk szolgáltatások SDK tooinstall hello Visual Studio projektsablonjai a helyi fejlesztési számítógépen.</span><span class="sxs-lookup"><span data-stu-id="7e908-114">Download hello BizTalk Services SDK tooinstall hello Visual Studio project templates on your on-premises development computer.</span></span> <span data-ttu-id="7e908-115">Ezek a sablonok létrehozása hello <strong>BizTalk szolgáltatások</strong> (híd) és hello <strong>BizTalk szolgáltatás-összetevők</strong> (átalakítási) Visual Studio-projektek, amelyek a telepített tooyour BizTalk szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7e908-115">These templates create hello <strong>BizTalk Services</strong> (bridge) and hello <strong>BizTalk Service Artifacts</strong> (Transform) Visual Studio projects that are deployed tooyour BizTalk Service.</span></span>
        <br/><br/><span data-ttu-id="7e908-116">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Hogyan tudom használata hello Azure BizTalk szolgáltatások SDK </a> és <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">telepítése hello Azure BizTalk szolgáltatások SDK</a> listák hello lépéseket tooget elindult.</span><span class="sxs-lookup"><span data-stu-id="7e908-116">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335"> How do I Start Using hello Azure BizTalk Services SDK </a> and <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">Installing hello Azure BizTalk Services SDK</a> lists hello steps tooget started.</span></span>
        </td>
    </tr>
    <tr>
        <td><span data-ttu-id="7e908-117"><strong>Partner megállapodások létrehozása</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-117"><strong>Create partner agreements</strong></span></span></td>
        <td><span data-ttu-id="7e908-118">Megnyílik a hello Azure BizTalk szolgáltatások portálja adja hozzá a partnerek, és ahol a X12, AS2, hozzon létre Azure-platformon futó és EDIFACT EDI-szerződések.</span><span class="sxs-lookup"><span data-stu-id="7e908-118">Opens hello Azure BizTalk Services Portal hosted on Azure where you add partners and create X12, AS2, and EDIFACT EDI agreements.</span></span>
        <br/><br/><span data-ttu-id="7e908-119">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">BizTalk szolgáltatások Portal üzenetküldéshez EDI összetevők konfigurálása</a> listák hello lépéseket tooget elindult.</span><span class="sxs-lookup"><span data-stu-id="7e908-119">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configuring Components for EDI Messaging on BizTalk Services Portal</a> lists hello steps tooget started.</span></span>
        </td>
    </tr>

<tr>
        <td><span data-ttu-id="7e908-120"><strong>További információ a BizTalk szolgáltatások</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-120"><strong>Learn more about BizTalk Services</strong></span></span></td>
        <td><span data-ttu-id="7e908-121">Nyissa meg toohello <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">center tanulási</a> toolearn Azure BizTalk szolgáltatásokkal kapcsolatos további.</span><span class="sxs-lookup"><span data-stu-id="7e908-121">Go toohello <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">learning center</a> toolearn more about Azure BizTalk Services.</span></span></td>
</tr>
</table>


<span data-ttu-id="7e908-122">A tálcán hello hello lap alján, a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="7e908-122">In hello task bar at hello bottom, you can:</span></span>

<table border="1">

<tr>
<td><span data-ttu-id="7e908-123"><strong>Kezelése</strong> az alkalmazások központi telepítésének</span><span class="sxs-lookup"><span data-stu-id="7e908-123"><strong>Manage</strong> your application deployment</span></span></td>
<td><span data-ttu-id="7e908-124">Megnyílik a hello Azure BizTalk Services portálra.</span><span class="sxs-lookup"><span data-stu-id="7e908-124">Opens hello Azure BizTalk Services portal.</span></span> <span data-ttu-id="7e908-125">BizTalk szolgáltatások portálja hello hello be tooEDI konfiguráció – beleértve a partnerek hozzáadása és X12, AS2, létrehozása és EDIFACT-egyezmények.</span><span class="sxs-lookup"><span data-stu-id="7e908-125">hello BizTalk Services Portal is hello entrance tooEDI configuration, including adding partners and creating X12, AS2, and EDIFACT agreements.</span></span>
<br/><br/>
<span data-ttu-id="7e908-126">Ez az azonos hello <strong>egyezmények létrehozása</strong> a hello <strong>gyors üzembe helyezés</strong> lapon.</span><span class="sxs-lookup"><span data-stu-id="7e908-126">This is hello same as <strong>Create partner agreements</strong> on hello <strong>Quick Start</strong> tab.</span></span>
<br/><br/><span data-ttu-id="7e908-127">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">BizTalk szolgáltatások Portal üzenetküldéshez EDI összetevők konfigurálása</a> hello BizTalk szolgáltatások portálja nyújt részletesebb információt.</span><span class="sxs-lookup"><span data-stu-id="7e908-127">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configuring Components for EDI Messaging on BizTalk Services Portal</a> provides more information on hello BizTalk Services Portal.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="7e908-128"><strong>Kapcsolat információi</strong> a hozzáférés-vezérlési Namespace hello</span><span class="sxs-lookup"><span data-stu-id="7e908-128"><strong>Connection Information</strong> of hello Access Control Namespace</span></span></td>
<td><span data-ttu-id="7e908-129">Kapcsolati adatokat, majd hello Access Control Namespace, alapértelmezett kibocsátó, és alapértelmezett kulcs jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="7e908-129">When you select Connection Information, then hello Access Control Namespace, Default Issuer, and Default Key are displayed.</span></span> <span data-ttu-id="7e908-130">Ezek az értékek másolhatja.</span><span class="sxs-lookup"><span data-stu-id="7e908-130">You can copy these values.</span></span>
<br/><br/>
<span data-ttu-id="7e908-131">Nyissa meg a hozzáférés-vezérlési Portal hello is.</span><span class="sxs-lookup"><span data-stu-id="7e908-131">You can also open hello Access Control Portal.</span></span> <span data-ttu-id="7e908-132"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Hozzon létre egy hozzáférés-vezérlés Namespace</a> hello hozzáférés-vezérlési Portal nyújt részletesebb információt.</span><span class="sxs-lookup"><span data-stu-id="7e908-132"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Create an Access control Namespace</a> provides more information on hello Access Control Portal.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="7e908-133"><strong>Kulcsok szinkronizálása</strong> a hello Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="7e908-133"><strong>Sync Keys</strong> in hello Storage Account</span></span></td>
<td><span data-ttu-id="7e908-134">Storage-fiók létrehozásakor automatikusan létrejön egy elsődleges kulcs és egy másodlagos kulcs.</span><span class="sxs-lookup"><span data-stu-id="7e908-134">When you create a Storage account, a Primary Key and Secondary Key are automatically created.</span></span> <span data-ttu-id="7e908-135">A titkosítási kulcsokat a Tárfiók hozzáférési tooyour szabályozza.</span><span class="sxs-lookup"><span data-stu-id="7e908-135">These encryption Keys control access tooyour Storage Account.</span></span> <span data-ttu-id="7e908-136">A BizTalk szolgáltatás automatikusan használ hello elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="7e908-136">Your BizTalk Service automatically uses hello Primary Key.</span></span> <span data-ttu-id="7e908-137"><strong>Kulcsok szinkronizálása</strong> engedélyezése a felhasználók tooswitch hello elsődleges kulcs és a másodlagos kulcs hello közötti hello BizTalk szolgáltatás megszakítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="7e908-137"><strong>Sync Keys</strong> enable users tooswitch between hello Primary Key and hello Secondary Key without disrupting hello BizTalk Service.</span></span>
<br/><br/>
<span data-ttu-id="7e908-138">Például azt szeretné, hello BizTalk szolgáltatás toouse hello Tárfiók új elsődleges kulcsot.</span><span class="sxs-lookup"><span data-stu-id="7e908-138">For example, you want hello BizTalk Service toouse a new Primary Key for hello Storage Account.</span></span> <span data-ttu-id="7e908-139">toodo ezt:</span><span class="sxs-lookup"><span data-stu-id="7e908-139">toodo this:</span></span>
<br/><br/>
<ol>
<li><span data-ttu-id="7e908-140">Válassza ki a BizTalk szolgáltatás, majd <strong>szinkronizálási kulcsok</strong>.</span><span class="sxs-lookup"><span data-stu-id="7e908-140">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="7e908-141">Válassza ki a hello másodlagos kulcsát.</span><span class="sxs-lookup"><span data-stu-id="7e908-141">Select hello Secondary Key.</span></span> <span data-ttu-id="7e908-142">Ekkor a hello BizTalk szolgáltatás elindul, hello másodlagos kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="7e908-142">When you do this, hello BizTalk Service starts using hello Secondary Key.</span></span></li>
<li><span data-ttu-id="7e908-143">A klasszikus Azure portálon hello válassza ki a tárfiók, és hello elsődleges kulcs újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="7e908-143">In hello Azure classic portal, select your Storage account and Regenerate hello Primary Key.</span></span> <span data-ttu-id="7e908-144">Ne feledje, hogy a BizTalk szolgáltatás hello másodlagos kulcsot használja.</span><span class="sxs-lookup"><span data-stu-id="7e908-144">Remember, your BizTalk Service is using hello Secondary Key.</span></span></li>
<li><span data-ttu-id="7e908-145">Válassza ki a BizTalk szolgáltatás, majd <strong>szinkronizálási kulcsok</strong>.</span><span class="sxs-lookup"><span data-stu-id="7e908-145">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="7e908-146">Jelölje ki, hello elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="7e908-146">Now, select hello Primary Key.</span></span> <span data-ttu-id="7e908-147">Ez az hello új elsődleges kulcs, akkor a rendszer újra létrehozza.</span><span class="sxs-lookup"><span data-stu-id="7e908-147">This is hello new Primary Key you regenerated.</span></span></li>
<li><span data-ttu-id="7e908-148">A klasszikus Azure portálon hello válassza ki a tárfiók, és hello másodlagos kulcs újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="7e908-148">In hello Azure classic portal, select your Storage account and Regenerate hello Secondary Key.</span></span></li>
</ol>
<br/>
<span data-ttu-id="7e908-149">A folyamat elnevezése "helyettesítő kulcsok".</span><span class="sxs-lookup"><span data-stu-id="7e908-149">This process is called "rollover keys".</span></span> <span data-ttu-id="7e908-150">hello célja tooenable felhasználók tooswitch hello elsődleges kulcs és a másodlagos kulcs hello közötti hello BizTalk szolgáltatás megszakítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="7e908-150">hello purpose is tooenable users tooswitch between hello Primary Key and hello Secondary Key without disrupting hello BizTalk Service.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="7e908-151"><strong>Törlés</strong> az alkalmazás</span><span class="sxs-lookup"><span data-stu-id="7e908-151"><strong>Delete</strong> your application</span></span></td>
<td><span data-ttu-id="7e908-152">A BizTalk szolgáltatás törléséhez és a rendszer eltávolítja az összes telepített elemek tooit.</span><span class="sxs-lookup"><span data-stu-id="7e908-152">When you select Delete, your BizTalk Service and all items deployed tooit are removed.</span></span></td>
</tr>
</table>


## <a name="dashboard"></a><span data-ttu-id="7e908-153">Irányítópult</span><span class="sxs-lookup"><span data-stu-id="7e908-153">Dashboard</span></span>
<span data-ttu-id="7e908-154">Hello BizTalk szolgáltatások Edition, attól függően, hogy az összes felsorolt beállítások nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="7e908-154">Depending on hello BizTalk Services Edition, all options listed may not be available.</span></span> 

<span data-ttu-id="7e908-155">A BizTalk szolgáltatás neve kiválasztásakor hello irányítópult lap akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="7e908-155">When you select your BizTalk Service name, hello Dashboard tab is displayed.</span></span> <span data-ttu-id="7e908-156">Az irányítópult a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="7e908-156">In Dashboard, you can:</span></span>

##### <a name="usage-overview-shows-hello-number-of-used-hybrid-connections"></a><span data-ttu-id="7e908-157">Használati áttekintés: A használt hibrid kapcsolatok hello számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="7e908-157">Usage Overview: Shows hello number of used Hybrid Connections</span></span>
<span data-ttu-id="7e908-158">Hello használati adatok is megjelennek GB.</span><span class="sxs-lookup"><span data-stu-id="7e908-158">Also displays hello data usage in GB.</span></span> 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a><span data-ttu-id="7e908-159">Metrika Graph: Metrikák rögzített listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="7e908-159">Metric Graph: Shows a fixed list of performance metrics</span></span>
<span data-ttu-id="7e908-160">Ezeket a mérési adja meg a valós idejű vonatkozó hello hello BizTalk szolgáltatás állapotát.</span><span class="sxs-lookup"><span data-stu-id="7e908-160">These metrics provide real-time values regarding hello health of hello BizTalk Service.</span></span> <span data-ttu-id="7e908-161">Meg lehet hello **relatív** vagy **abszolút** értékek és hello időtartománynak **időköz** hello grafikonon megjelenített hello mérőszámokat.</span><span class="sxs-lookup"><span data-stu-id="7e908-161">You can also choose hello **Relative** or **Absolute** values and hello time range **Interval** of hello metrics that are displayed in hello graph.</span></span> 

<span data-ttu-id="7e908-162">A metrikák leírását, nyissa meg túl[elérhető](#Metrics) ebben a témakörben.</span><span class="sxs-lookup"><span data-stu-id="7e908-162">For a description of these performance metrics, go too[Available Metrics](#Metrics) in this topic.</span></span>

##### <a name="quick-glance-lists-your-biztalk-service-properties"></a><span data-ttu-id="7e908-163">Gyors áttekintő: Felsorolja a BizTalk szolgáltatás tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="7e908-163">Quick Glance: Lists your BizTalk Service properties</span></span>
<table border="1">

<tr>
<td><span data-ttu-id="7e908-164"><strong>Követés adatbázis-hitelesítő adatok frissítése</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-164"><strong>Update Tracking Database credentials</strong></span></span></td>
<td><span data-ttu-id="7e908-165">Módosítások hello felhasználónév és jelszó toolog hello követési adatbázis azokat.</span><span class="sxs-lookup"><span data-stu-id="7e908-165">Changes hello user name and password used toolog into hello Tracking Database.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-166"><strong>SSL-tanúsítvány frissítése</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-166"><strong>Update SSL Certificate</strong></span></span></td>
<td><span data-ttu-id="7e908-167">Frissítheti hello BizTalk szolgáltatás toouse egy másik SSL-tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="7e908-167">Can update hello BizTalk Service toouse a different SSL certificate.</span></span> <span data-ttu-id="7e908-168">Egy önaláírt SSL-tanúsítvány mikor automatikusan létrejön, <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">hello BizTalk szolgáltatás létrehozása</a>.</span><span class="sxs-lookup"><span data-stu-id="7e908-168">A self-signed SSL certificate is automatically created when you <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">create hello BizTalk Service</a>.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-169"><strong>Tanúsítvány letöltése</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-169"><strong>Download Certificate</strong></span></span></td>
<td><span data-ttu-id="7e908-170">A BizTalk szolgáltatás tooa helyi számítógép által használt SSL-tanúsítvány hello töltheti le.</span><span class="sxs-lookup"><span data-stu-id="7e908-170">You can download hello SSL certificate used by your BizTalk Service tooa local machine.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-171"><strong>Állapot</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-171"><strong>Status</strong></span></span></td>
<td><span data-ttu-id="7e908-172">A BizTalk szolgáltatás hello aktuális állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="7e908-172">Displays hello current status of your BizTalk Service.</span></span> <span data-ttu-id="7e908-173">Lásd: <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk szolgáltatások: szolgáltatás állapota diagram</a>.</span><span class="sxs-lookup"><span data-stu-id="7e908-173">See <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk Services: Service state chart</a>.</span></span> </td>
</tr>
<tr>
<td><span data-ttu-id="7e908-174"><strong>URL-címe</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-174"><strong>Service URL</strong></span></span></td>
<td><span data-ttu-id="7e908-175">a BizTalk szolgáltatás hello URL-címe</span><span class="sxs-lookup"><span data-stu-id="7e908-175">hello URL for your BizTalk Service.</span></span> <span data-ttu-id="7e908-176">Ez az hello ugyanaz, mint hello <strong>tartomány URL-cím</strong> a BizTalk szolgáltatás létrehozásakor megadott.</span><span class="sxs-lookup"><span data-stu-id="7e908-176">This is hello same as hello <strong>Domain URL</strong> entered when your BizTalk Service is created.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-177"><strong>Nyilvános virtuális IP-cím (VIP) címe</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-177"><strong>Public Virtual IP (VIP) Address</strong></span></span></td>
<td><span data-ttu-id="7e908-178">hello IP-cím hozzárendelése tooyour BizTalk szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7e908-178">hello IP address assigned tooyour BizTalk Service.</span></span> <span data-ttu-id="7e908-179">Bemeneti végpontjai szolgál, és a kimenő forgalom hello forráscím.</span><span class="sxs-lookup"><span data-stu-id="7e908-179">It is used for all input endpoints and is hello source address for outbound traffic.</span></span> <span data-ttu-id="7e908-180">Az IP-cím tartozik tooyour BizTalk szolgáltatás, mindaddig, amíg létrejön.</span><span class="sxs-lookup"><span data-stu-id="7e908-180">This IP address belongs tooyour BizTalk Service as long as it is created.</span></span> <span data-ttu-id="7e908-181">Ha törli a hello BizTalk szolgáltatás, hello IP-cím hozzá van rendelve tooanother BizTalk szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7e908-181">If you delete hello BizTalk Service, hello IP address is assigned tooanother BizTalk Service.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-182"><strong>Az ACS-Namespace</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-182"><strong>ACS Namespace</strong></span></span></td>
<td><span data-ttu-id="7e908-183">Hello BizTalk szolgáltatás végzi a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="7e908-183">Authenticates with hello BizTalk Service.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-184"><strong>Kiadás</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-184"><strong>Edition</strong></span></span></td>
<td><span data-ttu-id="7e908-185">Edition hello BizTalk szolgáltatás létrehozásakor megadott hello sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="7e908-185">Lists hello Edition entered when hello BizTalk Service is created.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-186"><strong>Hely</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-186"><strong>Location</strong></span></span></td>
<td><span data-ttu-id="7e908-187">Megjeleníti a földrajzi régióban, amelyen a BizTalk szolgáltatás hello.</span><span class="sxs-lookup"><span data-stu-id="7e908-187">Displays hello geographic region that hosts your BizTalk Service.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-188"><strong>Létrehozva</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-188"><strong>Created</strong></span></span></td>
<td><span data-ttu-id="7e908-189">Megjeleníti hello dátum és idő hello BizTalk szolgáltatás létrejött.</span><span class="sxs-lookup"><span data-stu-id="7e908-189">Displays hello date and time hello BizTalk Service was created.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-190"><strong>Adatbázis nyomon követése</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-190"><strong>Tracking Database</strong></span></span></td>
<td><span data-ttu-id="7e908-191">hello Azure SQL-adatbázis neve, amely nyomon követése a BizTalk szolgáltatás által használt táblák hello tárolja.</span><span class="sxs-lookup"><span data-stu-id="7e908-191">hello Azure SQL Database name that stores hello tracking tables used by your BizTalk Service.</span></span> 
<br/><br/><span data-ttu-id="7e908-192">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Követelmények Explained</a> hello követési adatbázis részleteit.</span><span class="sxs-lookup"><span data-stu-id="7e908-192">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Requirements Explained</a>  provides details on hello Tracking Database.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-193"><strong>Tárolási figyelés/archiválása</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-193"><strong>Monitoring/Archiving Storage</strong></span></span></td>
<td><span data-ttu-id="7e908-194">hello Azure Tárfiók neve, amely a BizTalk szolgáltatás kimeneti figyelési hello tárolja.</span><span class="sxs-lookup"><span data-stu-id="7e908-194">hello Azure Storage account name that stores hello monitoring output of your BizTalk Service.</span></span>
<br/><br/><span data-ttu-id="7e908-195">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Követelmények Explained</a> hello tárfiók részleteit.</span><span class="sxs-lookup"><span data-stu-id="7e908-195">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Requirements Explained</a>  provides details on hello Storage account.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-196"><strong>Előfizetés neve</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-196"><strong>Subscription Name</strong></span></span></td>
<td><span data-ttu-id="7e908-197">A BizTalk szolgáltatás üzemeltető hello előfizetés sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="7e908-197">Lists hello subscription that hosts your BizTalk Service.</span></span> <span data-ttu-id="7e908-198">hello előfizetés irányító hozzáférés toohello a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="7e908-198">hello subscription governs access toohello Azure classic portal.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-199"><strong>Előfizetés-azonosító</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-199"><strong>Subscription ID</strong></span></span></td>
<td><span data-ttu-id="7e908-200">Előfizetés létrehozásakor a rendszer automatikusan előállítja az előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="7e908-200">When a subscription is created, a subscription ID is automatically generated.</span></span> <span data-ttu-id="7e908-201">REST API-k használata esetén szükség lehet a tooenter hello előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="7e908-201">When using REST APIs, you may need tooenter hello Subscription ID.</span></span></td>
</tr>
</table>

<span data-ttu-id="7e908-202">[BizTalk szolgáltatások: Telepítési használata Azure klasszikus portálon](http://go.microsoft.com/fwlink/p/?LinkID=302280) listák hello lépéseket toocreate BizTalk szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7e908-202">[BizTalk Services: Provisioning Using Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280) lists hello steps toocreate a BizTalk Service.</span></span>

##### <a name="manage-connection-information-sync-keys-and-delete-in-hello-task-bar"></a><span data-ttu-id="7e908-203">Kezelése, a kapcsolatadatok, a szinkronizálási kulcsokat, és hello tálcán törlése:</span><span class="sxs-lookup"><span data-stu-id="7e908-203">Manage, Connection Information, Sync Keys, and Delete in hello task bar:</span></span>
<table border="1">

<tr>
<td><span data-ttu-id="7e908-204"><strong>Kezelése</strong> az alkalmazások központi telepítésének</span><span class="sxs-lookup"><span data-stu-id="7e908-204"><strong>Manage</strong> your application deployment</span></span></td>
<td><span data-ttu-id="7e908-205">Megnyílik a hello Azure BizTalk Services portálra.</span><span class="sxs-lookup"><span data-stu-id="7e908-205">Opens hello Azure BizTalk Services Portal.</span></span> <span data-ttu-id="7e908-206">BizTalk szolgáltatások portálja hello hello be tooEDI konfiguráció – beleértve a partnerek hozzáadása és X12, AS2, létrehozása és EDIFACT-egyezmények.</span><span class="sxs-lookup"><span data-stu-id="7e908-206">hello BizTalk Services Portal is hello entrance tooEDI configuration, including adding partners and creating X12, AS2, and EDIFACT agreements.</span></span>
<br/><br/>
<span data-ttu-id="7e908-207">Ez az azonos hello <strong>egyezmények létrehozása</strong> a hello <strong>gyors üzembe helyezés</strong> lapon.</span><span class="sxs-lookup"><span data-stu-id="7e908-207">This is hello same as <strong>Create partner agreements</strong> on hello <strong>Quick Start</strong> tab.</span></span>
<br/><br/><span data-ttu-id="7e908-208">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">BizTalk szolgáltatások Portal üzenetküldéshez EDI összetevők konfigurálása</a> hello BizTalk szolgáltatások portálja nyújt részletesebb információt.</span><span class="sxs-lookup"><span data-stu-id="7e908-208">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configuring Components for EDI Messaging on BizTalk Services Portal</a> provides more information on hello BizTalk Services Portal.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-209"><strong>Kapcsolat információi</strong> a hozzáférés-vezérlési Namespace hello</span><span class="sxs-lookup"><span data-stu-id="7e908-209"><strong>Connection Information</strong> of hello Access Control Namespace</span></span></td>
<td><span data-ttu-id="7e908-210">Megjeleníti a hello hozzáférés-vezérlési Namespace, alapértelmezett kibocsátó és a kulcs alapértelmezett értékek; amely másolhatók.</span><span class="sxs-lookup"><span data-stu-id="7e908-210">Displays hello Access Control Namespace, Default Issuer, and Default Key values; which can be copied.</span></span>
<br/><br/>
<span data-ttu-id="7e908-211">Nyissa meg a hozzáférés-vezérlési Portal hello is.</span><span class="sxs-lookup"><span data-stu-id="7e908-211">You can also open hello Access Control Portal.</span></span> <span data-ttu-id="7e908-212">A hozzáférés-vezérlő portál van hello ugyanaz, mint a hello bal oldali navigációs panelen hello Active Directory beállítás használatával.</span><span class="sxs-lookup"><span data-stu-id="7e908-212">This Access Control Portal is hello same as using hello Active Directory option in hello left navigation pane.</span></span>
<br/><br/><span data-ttu-id="7e908-213">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Az ACS-Namespace kezelése</a> hello hozzáférés-vezérlési Portal nyújt részletesebb információt.</span><span class="sxs-lookup"><span data-stu-id="7e908-213">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Managing Your ACS Namespace</a> provides more information on hello Access Control Portal.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-214"><strong>Kulcsok szinkronizálása</strong> a hello Storage-fiók</span><span class="sxs-lookup"><span data-stu-id="7e908-214"><strong>Sync Keys</strong> in hello Storage Account</span></span></td>
<td><span data-ttu-id="7e908-215">Storage-fiók létrehozásakor automatikusan létrejön egy elsődleges kulcs és egy másodlagos kulcs.</span><span class="sxs-lookup"><span data-stu-id="7e908-215">When you create a Storage account, a Primary Key and Secondary Key are automatically created.</span></span> <span data-ttu-id="7e908-216">A titkosítási kulcsokat a Tárfiók hozzáférési tooyour szabályozza.</span><span class="sxs-lookup"><span data-stu-id="7e908-216">These encryption Keys control access tooyour Storage Account.</span></span> <span data-ttu-id="7e908-217">A BizTalk szolgáltatás automatikusan használ hello elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="7e908-217">Your BizTalk Service automatically uses hello Primary Key.</span></span> <span data-ttu-id="7e908-218"><strong>Kulcsok szinkronizálása</strong> engedélyezése a felhasználók tooswitch hello elsődleges kulcs és a másodlagos kulcs hello közötti hello BizTalk szolgáltatás megszakítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="7e908-218"><strong>Sync Keys</strong> enable users tooswitch between hello Primary Key and hello Secondary Key without disrupting hello BizTalk Service.</span></span>
<br/><br/>
<span data-ttu-id="7e908-219">Például azt szeretné, hello BizTalk szolgáltatás toouse hello Tárfiók új elsődleges kulcsot.</span><span class="sxs-lookup"><span data-stu-id="7e908-219">For example, you want hello BizTalk Service toouse a new Primary Key for hello Storage Account.</span></span> <span data-ttu-id="7e908-220">toodo ezt:</span><span class="sxs-lookup"><span data-stu-id="7e908-220">toodo this:</span></span>
<br/><br/>
<ol>
<li><span data-ttu-id="7e908-221">Válassza ki a BizTalk szolgáltatás, majd <strong>szinkronizálási kulcsok</strong>.</span><span class="sxs-lookup"><span data-stu-id="7e908-221">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="7e908-222">Válassza ki a hello másodlagos kulcsát.</span><span class="sxs-lookup"><span data-stu-id="7e908-222">Select hello Secondary Key.</span></span> <span data-ttu-id="7e908-223">Ekkor a hello BizTalk szolgáltatás elindul, hello másodlagos kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="7e908-223">When you do this, hello BizTalk Service starts using hello Secondary Key.</span></span></li>
<li><span data-ttu-id="7e908-224">A klasszikus Azure portálon hello válassza ki a tárfiók, és hello elsődleges kulcs újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="7e908-224">In hello Azure classic portal, select your Storage account and Regenerate hello Primary Key.</span></span> <span data-ttu-id="7e908-225">Ne feledje, hogy a BizTalk szolgáltatás hello másodlagos kulcsot használja.</span><span class="sxs-lookup"><span data-stu-id="7e908-225">Remember, your BizTalk Service is using hello Secondary Key.</span></span></li>
<li><span data-ttu-id="7e908-226">Válassza ki a BizTalk szolgáltatás, majd <strong>szinkronizálási kulcsok</strong>.</span><span class="sxs-lookup"><span data-stu-id="7e908-226">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="7e908-227">Jelölje ki, hello elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="7e908-227">Now, select hello Primary Key.</span></span> <span data-ttu-id="7e908-228">Ez az hello új elsődleges kulcs, akkor a rendszer újra létrehozza.</span><span class="sxs-lookup"><span data-stu-id="7e908-228">This is hello new Primary Key you regenerated.</span></span></li>
<li><span data-ttu-id="7e908-229">A klasszikus Azure portálon hello válassza ki a tárfiók, és hello másodlagos kulcs újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="7e908-229">In hello Azure classic portal, select your Storage account and Regenerate hello Secondary Key.</span></span></li>
</ol>
<br/>
<span data-ttu-id="7e908-230">A folyamat elnevezése "helyettesítő kulcsok".</span><span class="sxs-lookup"><span data-stu-id="7e908-230">This process is called "rollover keys".</span></span> <span data-ttu-id="7e908-231">hello célja tooenable felhasználók tooswitch hello elsődleges kulcs és a másodlagos kulcs hello közötti hello BizTalk szolgáltatás megszakítása nélkül.</span><span class="sxs-lookup"><span data-stu-id="7e908-231">hello purpose is tooenable users tooswitch between hello Primary Key and hello Secondary Key without disrupting hello BizTalk Service.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="7e908-232"><strong>Törlés</strong> az alkalmazás</span><span class="sxs-lookup"><span data-stu-id="7e908-232"><strong>Delete</strong> your application</span></span></td>
<td><span data-ttu-id="7e908-233">A BizTalk szolgáltatás és az összes telepített elemek tooit törlődnek.</span><span class="sxs-lookup"><span data-stu-id="7e908-233">Your BizTalk Service and all items deployed tooit are removed.</span></span></td>
</tr>
</table>


## <a name="monitor"></a><span data-ttu-id="7e908-234">Figyelés</span><span class="sxs-lookup"><span data-stu-id="7e908-234">Monitor</span></span>
<span data-ttu-id="7e908-235">Nem érvényes toohello ingyenes kiadásban.</span><span class="sxs-lookup"><span data-stu-id="7e908-235">Does not apply toohello Free Edition.</span></span>

<span data-ttu-id="7e908-236">A BizTalk szolgáltatás neve kiválasztásakor hello figyelő lapon érhető el, és hello következő jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="7e908-236">When you select your BizTalk Service name, hello Monitor tab is available and displays hello following:</span></span>

##### <a name="metric-graph-displays-hello-selected-performance-metrics"></a><span data-ttu-id="7e908-237">Metrika Graph: Megjeleníti hello kijelölt metrikák</span><span class="sxs-lookup"><span data-stu-id="7e908-237">Metric Graph: Displays hello selected performance metrics</span></span>
<span data-ttu-id="7e908-238">Ezeket a mérési adja meg a valós idejű vonatkozó hello hello BizTalk szolgáltatás állapotát.</span><span class="sxs-lookup"><span data-stu-id="7e908-238">These metrics provide real-time values regarding hello health of hello BizTalk Service.</span></span> <span data-ttu-id="7e908-239">Úgy dönt, hogy mely metrikák jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="7e908-239">You choose which performance metrics are displayed.</span></span> <span data-ttu-id="7e908-240">Egyszerre legfeljebb hat teljesítménymutatók megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="7e908-240">A maximum of six performance metrics can be displayed simultaneously.</span></span> 

<span data-ttu-id="7e908-241">Másik lehetőségként hello **relatív** vagy **abszolút** értékek és hello időtartománynak **időköz** hello metrikák jelennek meg a.</span><span class="sxs-lookup"><span data-stu-id="7e908-241">You can also choose hello **Relative** or **Absolute** values and hello time range **Interval** of hello metrics that are displayed.</span></span> 

##### <a name="tooremove-or-display-metrics-in-hello-graph"></a><span data-ttu-id="7e908-242">a hello graph tooremove vagy a megjelenítési metrikák:</span><span class="sxs-lookup"><span data-stu-id="7e908-242">tooremove or display metrics in hello graph:</span></span>
1. <span data-ttu-id="7e908-243">Jelölje be hello **figyelő** fülre.</span><span class="sxs-lookup"><span data-stu-id="7e908-243">Select hello **Monitor** tab.</span></span>
2. <span data-ttu-id="7e908-244">Válassza ki **metrikák hozzáadása** hello tálcán:</span><span class="sxs-lookup"><span data-stu-id="7e908-244">Select **Add Metrics** in hello task bar:</span></span>  
   <span data-ttu-id="7e908-245">![Válassza ki a metrikák hozzáadása][AddMetrics]</span><span class="sxs-lookup"><span data-stu-id="7e908-245">![Select Add Metrics][AddMetrics]</span></span>
3. <span data-ttu-id="7e908-246">Ellenőrizze azt szeretné, hogy toodisplay hello teljesítménymutatók.</span><span class="sxs-lookup"><span data-stu-id="7e908-246">Check hello performance metrics you want toodisplay.</span></span>
4. <span data-ttu-id="7e908-247">Válassza ki a hello pipa tooreturn toohello **figyelő** fülre.</span><span class="sxs-lookup"><span data-stu-id="7e908-247">Select hello checkmark tooreturn toohello **Monitor** tab.</span></span>
5. <span data-ttu-id="7e908-248">Válasszon hello kör következő toohello metrika toodisplay adott metrika érték hello gráfban.</span><span class="sxs-lookup"><span data-stu-id="7e908-248">Select hello circle next toohello metric toodisplay that metric's value in hello graph.</span></span>  
   
    <span data-ttu-id="7e908-249">Például hello **CPU-használat** metrika szürkén jelenik meg; kimenetét hello Graph nem jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="7e908-249">For example, hello **CPU Usage** metric is grayed out; its output is not displayed in hello graph:</span></span>  
   <span data-ttu-id="7e908-250">![CPU-használat metrika szürkén jelenik meg][GrayedMetric]</span><span class="sxs-lookup"><span data-stu-id="7e908-250">![CPU Usage metric is grayed out][GrayedMetric]</span></span>  
   
    <span data-ttu-id="7e908-251">Jelölje be hello szürkén jelenik meg kör tooenable hello **CPU-használat** metrika toodisplay hello Graph kimenetét:</span><span class="sxs-lookup"><span data-stu-id="7e908-251">Select hello grayed out circle tooenable hello **CPU Usage** metric toodisplay its output in hello graph:</span></span>  
   <span data-ttu-id="7e908-252">![CPU-használat metrika engedélyezve van][EnabledMetric]</span><span class="sxs-lookup"><span data-stu-id="7e908-252">![CPU Usage metric is enabled][EnabledMetric]</span></span>
6. <span data-ttu-id="7e908-253">tooremove metrika hello megjelenítési graph és hello listájában válassza ki **törölje metrikát** hello tálcán.</span><span class="sxs-lookup"><span data-stu-id="7e908-253">tooremove a metric from hello display graph and hello list, select **Delete Metric** in hello task bar.</span></span> <span data-ttu-id="7e908-254">tooadd hello metrika hátsó toohello listáról válassza ki **metrikák hozzáadása** hello tálcán, ellenőrizze a hello metrika, és jelölje be hello pipa tooreturn toohello **figyelő** fülre. Jelölje be hello kör tooenable hello metrika használhatók.</span><span class="sxs-lookup"><span data-stu-id="7e908-254">tooadd hello metric back toohello list, select **Add Metrics** in hello task bar, check hello metric, and select hello checkmark tooreturn toohello **Monitor** tab. Select hello grayed out circle tooenable hello metric.</span></span>

## <span data-ttu-id="7e908-255"><a name="Metrics"></a>Elérhető</span><span class="sxs-lookup"><span data-stu-id="7e908-255"><a name="Metrics"></a>Available Metrics</span></span>
<span data-ttu-id="7e908-256">a következő számlálók/teljesítménymutatók hello érhetők el:</span><span class="sxs-lookup"><span data-stu-id="7e908-256">hello following performance counters/metrics are available:</span></span>

<table border="1">

<tr>
<td><span data-ttu-id="7e908-257"><strong>RountdTrip késés</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-257"><strong>RountdTrip Latency</strong></span></span></td>
<td><span data-ttu-id="7e908-258">Megjeleníti a hello átlagos idő ezredmásodpercben (ms) tooprocess hello idő üdvözlőüzenetére üzenetét érkezik, amíg üdvözlőüzenetére teljesen hello BizTalk szolgáltatás közötti összes hidak dolgoz fel.</span><span class="sxs-lookup"><span data-stu-id="7e908-258">Displays hello average time taken in milliseconds (ms) tooprocess a message from hello time hello message is received until hello message is fully processed by hello BizTalk Service across all bridges.</span></span> <span data-ttu-id="7e908-259">Csak a sikeresen feldolgozott üzeneteinek bájtjai számítanak.</span><span class="sxs-lookup"><span data-stu-id="7e908-259">Only messages successfully processed are counted.</span></span><br/><br/>
<span data-ttu-id="7e908-260">A következő események hello fordulhat elő, amikor egy Timestamp típusú jön létre:</span><span class="sxs-lookup"><span data-stu-id="7e908-260">When hello following events occur, a timestamp is created:</span></span>
<ul>
<li><span data-ttu-id="7e908-261">Üzenet kerül hello átjáró</span><span class="sxs-lookup"><span data-stu-id="7e908-261">Message enters hello gateway</span></span></li>
<li><span data-ttu-id="7e908-262">A következő irányított toohello cél:</span><span class="sxs-lookup"><span data-stu-id="7e908-262">Message is routed toohello destination</span></span></li>
<li><span data-ttu-id="7e908-263">Cél választ</span><span class="sxs-lookup"><span data-stu-id="7e908-263">Destination response is received</span></span></li>
<li><span data-ttu-id="7e908-264">Cél nyugtázási küldött válaszok toohello átjáró</span><span class="sxs-lookup"><span data-stu-id="7e908-264">Destination acknowledgement response sent toohello gateway</span></span></li>
</ul>
<br/>
<span data-ttu-id="7e908-265">Ez a mérőszám a következő számítási hello hello eredményét mutatja:</span><span class="sxs-lookup"><span data-stu-id="7e908-265">This metric shows hello result of hello following calculation:</span></span>
<br/><br/>
<span data-ttu-id="7e908-266">[Cél nyugtázási küldött válaszok toohello átjáró] - [üzenet kerül hello átjáró]</span><span class="sxs-lookup"><span data-stu-id="7e908-266">[Destination acknowledgement response sent toohello gateway] - [Message enters hello gateway]</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-267"><strong>Hibákat forrásnál</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-267"><strong>Failures At Source</strong></span></span></td>
<td><span data-ttu-id="7e908-268">Megjeleníti a sikertelen üzenetek teljes száma hello hello BizTalk szolgáltatás húzza hello forrás végpontok érkező üzenetek által.</span><span class="sxs-lookup"><span data-stu-id="7e908-268">Displays hello total number of messages that failed by hello BizTalk Service when pulling messages from hello source endpoints.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-269"><strong>CPU-használat</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-269"><strong>CPU Usage</strong></span></span></td>
<td><span data-ttu-id="7e908-270">Hello átlagos processzoridő % található összes szerepkörpéldány sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="7e908-270">Lists hello average %Processor Time of all role instances.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-271"><strong>Feldolgozási késleltetés</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-271"><strong>Processing Latency</strong></span></span></td>
<td><span data-ttu-id="7e908-272">Megjeleníti a hello átlagos idő ezredmásodpercben (ms) tooprocess hello BizTalk szolgáltatás közötti összes hidak, kivéve a hello idő szerint üzenet töltött célok szükséges.</span><span class="sxs-lookup"><span data-stu-id="7e908-272">Displays hello average time taken In milliseconds (ms) tooprocess a message by hello BizTalk Service across all bridges, excluding hello time spent in destinations.</span></span> <span data-ttu-id="7e908-273">Csak a sikeresen feldolgozott üzeneteinek bájtjai számítanak.</span><span class="sxs-lookup"><span data-stu-id="7e908-273">Only messages successfully processed are counted.</span></span><br/><br/>
<span data-ttu-id="7e908-274">Egyes események hello fordulhat elő, amikor egy Timestamp típusú jön létre:</span><span class="sxs-lookup"><span data-stu-id="7e908-274">When each of hello following events occur, a timestamp is created:</span></span>

<ul>
<li><span data-ttu-id="7e908-275">Üzenet kerül hello átjáró</span><span class="sxs-lookup"><span data-stu-id="7e908-275">Message enters hello gateway</span></span></li>
<li><span data-ttu-id="7e908-276">A következő irányított toohello cél:</span><span class="sxs-lookup"><span data-stu-id="7e908-276">Message is routed toohello destination</span></span></li>
<li><span data-ttu-id="7e908-277">Cél választ</span><span class="sxs-lookup"><span data-stu-id="7e908-277">Destination response is received</span></span></li>
<li><span data-ttu-id="7e908-278">Cél nyugtázási küldött válaszok toohello átjáró</span><span class="sxs-lookup"><span data-stu-id="7e908-278">Destination acknowledgement response sent toohello gateway</span></span></li>
</ul>
<br/><span data-ttu-id="7e908-279">Ez a mérőszám a következő számítási hello hello eredményét mutatja:</span><span class="sxs-lookup"><span data-stu-id="7e908-279">This metric shows hello result of hello following calculation:</span></span><br/><br/>
<span data-ttu-id="7e908-280">[Cél nyugtázási küldött válaszok toohello átjáró] - [üzenet kerül hello átjáró] - [cél választ] + [üzenet irányított toohello cél]</span><span class="sxs-lookup"><span data-stu-id="7e908-280">[Destination acknowledgement response sent toohello gateway] - [Message enters hello gateway] - [Destination response is received] + [Message is routed toohello destination]</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-281"><strong>A folyamat sikertelen</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-281"><strong>Failures In Process</strong></span></span></td>
<td><span data-ttu-id="7e908-282">Hello sikertelen feldolgozása során hello BizTalk szolgáltatás közötti összes hello hidak időintervallumon belül-kezelési üzenetek teljes számát mutatja.</span><span class="sxs-lookup"><span data-stu-id="7e908-282">Displays hello total number of messages that failed during processing by hello BizTalk Service across all hello bridges within a time interval.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-283"><strong>Küldött üzenetek</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-283"><strong>Messages Sent</strong></span></span></td>
<td><span data-ttu-id="7e908-284">Megjeleníti egy adott időintervallumban belül hello BizTalk szolgáltatás közötti összes hidak által küldött üzenetek teljes száma hello.</span><span class="sxs-lookup"><span data-stu-id="7e908-284">Displays hello total number of messages sent by hello BizTalk Service across all bridges within a time interval.</span></span> <span data-ttu-id="7e908-285">Ez a metrika értéke akkor növekszik, ha a folyamat által küldött üzenet eléri hello útvonal cél.</span><span class="sxs-lookup"><span data-stu-id="7e908-285">This metric is incremented when a message sent from a pipeline reaches hello route destination.</span></span> <span data-ttu-id="7e908-286">Ez a metrika nem jelzi, hogy egy üzenet feldolgozása sikeresen.</span><span class="sxs-lookup"><span data-stu-id="7e908-286">This metric does not indicate that a message is successfully processed.</span></span><br/><br/>
<span data-ttu-id="7e908-287">A kérelem-válasz forgatókönyvek a hello metrika értéke eggyel növekszik, hello útvonal cél üzenetet küld egy fogadását nyugtázási hátsó toohello folyamatot.</span><span class="sxs-lookup"><span data-stu-id="7e908-287">In a Request-Reply scenario, hello metric is incremented when hello route destination sends a receipt acknowledgement back toohello pipeline.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-288"><strong>A Beérkezett üzenetek</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-288"><strong>Messages Received</strong></span></span></td>
<td><span data-ttu-id="7e908-289">Megjeleníti egy adott időintervallumban belül hello BizTalk szolgáltatás közötti összes hidak által fogadott üzenetek száma hello.</span><span class="sxs-lookup"><span data-stu-id="7e908-289">Displays hello total number of messages received by hello BizTalk Service across all bridges within a time interval.</span></span> <span data-ttu-id="7e908-290">Ez a metrika értéke akkor nő, amikor egy új üzenet jelenik meg az adatcsatorna hello.</span><span class="sxs-lookup"><span data-stu-id="7e908-290">This metric is incremented when a new message is received by hello pipeline.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-291"><strong>A folyamat üzenetek</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-291"><strong>Messages In Process</strong></span></span></td>
<td><span data-ttu-id="7e908-292">Megjeleníti a hello időintervallumon belül hello BizTalk szolgáltatás által jelenleg feldolgozott üzenetek teljes száma.</span><span class="sxs-lookup"><span data-stu-id="7e908-292">Displays hello total number of messages currently being processed by hello BizTalk Service within a time interval.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="7e908-293"><strong>Az üzenetek feldolgozásának</strong></span><span class="sxs-lookup"><span data-stu-id="7e908-293"><strong>Messages Processed</strong></span></span></td>
<td><span data-ttu-id="7e908-294">Megjeleníti a hello időintervallumon belül hello BizTalk szolgáltatás közötti összes hidak sikeresen feldolgozott üzenetek teljes száma.</span><span class="sxs-lookup"><span data-stu-id="7e908-294">Displays hello total number of messages successfully processed by hello BizTalk Service across all bridges within a time interval.</span></span> <span data-ttu-id="7e908-295">Ez a metrika értéke akkor nő, amikor egy üzenet sikeresen megkapta hello feldolgozási sorban lévő és sikeresen irányított toohello cél.</span><span class="sxs-lookup"><span data-stu-id="7e908-295">This metric is incremented when a message is successfully received by hello pipeline and successfully routed toohello destination.</span></span></td>
</tr>
</table>


## <a name="scale"></a><span data-ttu-id="7e908-296">Méretezés</span><span class="sxs-lookup"><span data-stu-id="7e908-296">Scale</span></span>
<span data-ttu-id="7e908-297">Hello méretezési lapon adhat hozzá vagy a BizTalk szolgáltatás által használt egységek száma hello kivonandó időnél.</span><span class="sxs-lookup"><span data-stu-id="7e908-297">In hello Scale tab, you can add or subtract hello number of units used by your BizTalk Service.</span></span> <span data-ttu-id="7e908-298">Alapértelmezés szerint van egy egység konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="7e908-298">By default, there is one Unit configured.</span></span> <span data-ttu-id="7e908-299">További egységek tooscale lehet hozzáadni a BizTalk szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="7e908-299">Additional Units can be added tooscale your BizTalk Service.</span></span> <span data-ttu-id="7e908-300">Hello méretezési növelésével, amelyek növelve az adatátviteli sebességet.</span><span class="sxs-lookup"><span data-stu-id="7e908-300">When you increase hello scale, you are increasing throughput.</span></span> <span data-ttu-id="7e908-301">hello mennyiségű erőforrást is növeli, beleértve a telepített hidak, a megállapodások, a LOB-kapcsolatok, és feldolgozási kapacitása révén.</span><span class="sxs-lookup"><span data-stu-id="7e908-301">hello amount of resources also increases, including deployed bridges, agreements, LOB connections, and processing power.</span></span> <span data-ttu-id="7e908-302">Például növelheti hello méretezési 1 egység too2 egységektől.</span><span class="sxs-lookup"><span data-stu-id="7e908-302">For example, you increase hello scale from 1 Unit too2 Units.</span></span> <span data-ttu-id="7e908-303">Ebben a helyzetben telepítése hidak, kettős hello megállapodások, kettős hello LOB kapcsolatok és dupla hello feldolgozási teljesítmény dupla hello száma.</span><span class="sxs-lookup"><span data-stu-id="7e908-303">In this situation, you can deploy double hello number of bridges, double hello agreements, double hello LOB connections, and double hello processing power.</span></span>

<span data-ttu-id="7e908-304">BizTalk kiadásaiban nem képes a skálázási beállítást.</span><span class="sxs-lookup"><span data-stu-id="7e908-304">Some BizTalk editions do not offer a scale option.</span></span> <span data-ttu-id="7e908-305">Ebben a helyzetben egy egység engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="7e908-305">In this situation, one Unit is permitted.</span></span> <span data-ttu-id="7e908-306">toodetermine darabszám a edition méretezhetők, tekintse meg a túl[BizTalk szolgáltatások: kiadások diagram](biztalk-editions-feature-chart.md).</span><span class="sxs-lookup"><span data-stu-id="7e908-306">toodetermine how many units your edition can be scaled, refer too[BizTalk Services: Editions Chart](biztalk-editions-feature-chart.md).</span></span>

<span data-ttu-id="7e908-307">Hello egységek számának növelése hatással lehet a díjszabás.</span><span class="sxs-lookup"><span data-stu-id="7e908-307">Increasing hello number of units may impact pricing.</span></span> <span data-ttu-id="7e908-308">Ha hello egységek növeléséhez kiválasztása **mentése** egy üzenetben arra kéri, ha a számlázási kihatással van.</span><span class="sxs-lookup"><span data-stu-id="7e908-308">If you increase hello Units, selecting **Save** displays a message that tells you if billing is impacted.</span></span> <span data-ttu-id="7e908-309">Ezután válasszon ki toocontinue.</span><span class="sxs-lookup"><span data-stu-id="7e908-309">You then choose toocontinue.</span></span> <span data-ttu-id="7e908-310">Hello egységek számának növelésével hello BizTalk szolgáltatás állapota aktív tooUpdating módosítja.</span><span class="sxs-lookup"><span data-stu-id="7e908-310">When you increase hello number of Units, hello BizTalk Service status changes from Active tooUpdating.</span></span> <span data-ttu-id="7e908-311">A hello frissítési állapot a BizTalk szolgáltatás toorun továbbra is.</span><span class="sxs-lookup"><span data-stu-id="7e908-311">In hello Updating state, your BizTalk Service continues toorun.</span></span>

<span data-ttu-id="7e908-312">[BizTalk szolgáltatások: Kiadások diagram](biztalk-editions-feature-chart.md) "egység".</span><span class="sxs-lookup"><span data-stu-id="7e908-312">[BizTalk Services: Editions Chart](biztalk-editions-feature-chart.md) defines a "Unit".</span></span>

## <a name="configure"></a><span data-ttu-id="7e908-313">Konfigurálás</span><span class="sxs-lookup"><span data-stu-id="7e908-313">Configure</span></span>
<span data-ttu-id="7e908-314">Nem érvényes tooHybrid kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="7e908-314">Does not apply tooHybrid Connections.</span></span>

<span data-ttu-id="7e908-315">Beállítja a biztonsági mentés állapotának tooNone hello vagy automatikus.</span><span class="sxs-lookup"><span data-stu-id="7e908-315">Sets hello Backup Status tooNone or Automatic.</span></span> <span data-ttu-id="7e908-316">Ha beállítása tooNone, nem készült biztonsági másolat automatikusan jönnek létre.</span><span class="sxs-lookup"><span data-stu-id="7e908-316">When set tooNone, no backups are automatically created.</span></span> <span data-ttu-id="7e908-317">Ha beállítása tooAutomatic, hello biztonsági mentési helyre, hello gyakorisága hello biztonsági másolat, és mennyi ideig tookeep hello biztonságimásolat-fájlok konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="7e908-317">When set tooAutomatic, you configure hello backup location, hello frequency of hello backup, and how long tookeep hello backup files.</span></span> 

<span data-ttu-id="7e908-318">[BizTalk szolgáltatások: Biztonsági mentése és visszaállítása](biztalk-backup-restore.md) hello részletes adatokat biztosít.</span><span class="sxs-lookup"><span data-stu-id="7e908-318">[BizTalk Services: Backup and Restore](biztalk-backup-restore.md) provides hello details.</span></span> 

## <span data-ttu-id="7e908-319"><a name="HybridConnections"></a>Hibrid kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="7e908-319"><a name="HybridConnections"></a>Hybrid Connections</span></span>
<span data-ttu-id="7e908-320">Hibrid kapcsolatok csatlakoztatása az Azure-alkalmazások, például webalkalmazások vagy a Mobile Apps az Azure App Service szolgáltatásban a statikus TCP-port, például az SQL Server, a MySQL, a HTTP webes API-k és a legtöbb egyéni webszolgáltatásokat használó tooan helyszíni erőforráshoz.</span><span class="sxs-lookup"><span data-stu-id="7e908-320">Hybrid Connections connect an Azure application, like Web Apps or Mobile Apps in Azure App Service, tooan on-premises resource that uses a static TCP port, such as SQL Server, MySQL, HTTP Web APIs, and most custom Web Services.</span></span> <span data-ttu-id="7e908-321">Hibrid kapcsolatok a BizTalk szolgáltatások felügyelete az hello a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="7e908-321">Hybrid Connections are managed in  BizTalk Services in hello Azure classic portal.</span></span>

<span data-ttu-id="7e908-322">toocreate hibrid kapcsolatok az Azure App Service szolgáltatásban, lásd: [hozzáférés a helyszíni erőforrások hibrid kapcsolatok használata az Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="7e908-322">toocreate Hybrid Connections in Azure App Service, see [Access on-premises resources using hybrid connections in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md).</span></span>

<span data-ttu-id="7e908-323">toocreate vagy az Azure BizTalk szolgáltatások hibrid kapcsolatok kezelése című [hibrid kapcsolatok](integration-hybrid-connection-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7e908-323">toocreate or manage Hybrid Connections in Azure BizTalk Services, see [Hybrid Connections](integration-hybrid-connection-overview.md).</span></span>

## <a name="next"></a><span data-ttu-id="7e908-324">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="7e908-324">Next</span></span>
<span data-ttu-id="7e908-325">Most, hogy ismeri hello különböző lappal, részletesebben hello Azure BizTalk szolgáltatások szolgáltatásairól:</span><span class="sxs-lookup"><span data-stu-id="7e908-325">Now that you're familiar with hello different tabs, you can learn more about hello Azure BizTalk Services features:</span></span>

* [<span data-ttu-id="7e908-326">BizTalk Services: Szabályozás</span><span class="sxs-lookup"><span data-stu-id="7e908-326">BizTalk Services: Throttling</span></span>](biztalk-throttling-thresholds.md)  
* [<span data-ttu-id="7e908-327">BizTalk Services: Kiállító neve és kiállító kulcsa</span><span class="sxs-lookup"><span data-stu-id="7e908-327">BizTalk Services: Issuer Name and Issuer Key</span></span>](biztalk-issuer-name-issuer-key.md)  
* [<span data-ttu-id="7e908-328">BizTalk Services: Biztonsági mentés és visszaállítás</span><span class="sxs-lookup"><span data-stu-id="7e908-328">BizTalk Services: Backup and Restore</span></span>](biztalk-backup-restore.md)

## <a name="see-also"></a><span data-ttu-id="7e908-329">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="7e908-329">See Also</span></span>
* [<span data-ttu-id="7e908-330">Hibrid kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="7e908-330">Hybrid Connections</span></span>](integration-hybrid-connection-overview.md)  
* [<span data-ttu-id="7e908-331">BizTalk szolgáltatások: Fejlesztői, Basic, Standard és prémium kiadás diagram</span><span class="sxs-lookup"><span data-stu-id="7e908-331">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](biztalk-editions-feature-chart.md)  
* [<span data-ttu-id="7e908-332">BizTalk szolgáltatások: Kiépítés használata Azure klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="7e908-332">BizTalk Services: Provisioning Using Azure classic portal</span></span>](biztalk-provision-services.md)  
* [<span data-ttu-id="7e908-333">BizTalk szolgáltatások: BizTalk szolgáltatás állapotának diagram</span><span class="sxs-lookup"><span data-stu-id="7e908-333">BizTalk Services: BizTalk Service State Chart</span></span>](biztalk-service-state-chart.md)  
* [<span data-ttu-id="7e908-334">Hogyan tudom használata hello Azure BizTalk szolgáltatások SDK-t</span><span class="sxs-lookup"><span data-stu-id="7e908-334">How do I Start Using hello Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[Quickstart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png

