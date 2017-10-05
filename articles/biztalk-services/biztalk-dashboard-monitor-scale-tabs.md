---
title: "Figyelő, a méretezés, az irányítópult konfigurálásához és a BizTalk szolgáltatások hibrid kapcsolatok |} Microsoft Docs"
description: "További tudnivalók a vezérlők, a klasszikus portál lapokon BizTalk szolgáltatások teljesítményének figyelése: irányítópult, a figyelő, a méretezés, a konfigurálás és a hibrid kapcsolatok. MABS, WABS"
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
ms.openlocfilehash: 4ec88d9a681a5692b31f7e3990d1c153296b18ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="review-the-dashboard-monitor-scale-configure-and-hybrid-connection-tabs"></a><span data-ttu-id="b8717-104">Tekintse át az Irányítópult, Figyelő, Skála, Konfigurálás és Hibridkapcsolat lapokat</span><span class="sxs-lookup"><span data-stu-id="b8717-104">Review the Dashboard, Monitor, Scale, Configure, and Hybrid Connection tabs</span></span>

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

<span data-ttu-id="b8717-105">Miután a BizTalk szolgáltatás létrehozása és az alkalmazás központi telepítése, néhány a BizTalk szolgáltatás beállításainak módosítása, és az alkalmazás teljesítményének figyelése.</span><span class="sxs-lookup"><span data-stu-id="b8717-105">After you create your BizTalk Service and deploy your application, you can change some of the BizTalk Service settings and monitor the application performance.</span></span> 

<span data-ttu-id="b8717-106">Amikor megnyitja a klasszikus Azure portálra, akkor automatikusan elhelyezve a **minden elem** fülre.</span><span class="sxs-lookup"><span data-stu-id="b8717-106">When you open the Azure classic portal, you are automatically placed at the **ALL ITEMS** tab.</span></span> <span data-ttu-id="b8717-107">A BizTalk szolgáltatás megtekintéséhez jelölje ki a BizTalk szolgáltatás a **minden elem** lapon, vagy válassza ki a **BIZTALK szolgáltatások** ; lapra, és válassza ki a BizTalk szolgáltatás neve.</span><span class="sxs-lookup"><span data-stu-id="b8717-107">To view your BizTalk Service, select your BizTalk Service in the **ALL ITEMS** tab or select the **BIZTALK SERVICES** tab; and then select your BizTalk Service name.</span></span>

<span data-ttu-id="b8717-108">Az alábbi fülekkel ekkor megnyílik egy új ablakban.</span><span class="sxs-lookup"><span data-stu-id="b8717-108">This opens a new window with the following tabs.</span></span> <span data-ttu-id="b8717-109">Ez a témakör ismerteti az ezeken a lapokon.</span><span class="sxs-lookup"><span data-stu-id="b8717-109">This topic describes these tabs.</span></span>

## <a name="quickstart-quickstartquickstart"></a><span data-ttu-id="b8717-110">Gyors üzembe helyezés)</span><span class="sxs-lookup"><span data-stu-id="b8717-110">Quickstart (</span></span>![Első lépések][Quickstart]<span data-ttu-id="b8717-112">)</span><span class="sxs-lookup"><span data-stu-id="b8717-112">)</span></span>
<span data-ttu-id="b8717-113">BizTalk szolgáltatások kiadásoknál felsorolt összes beállítások nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="b8717-113">Depending on the BizTalk Services Edition, all options listed may not be available.</span></span> 

<table border="1">
    <tr>
        <td><span data-ttu-id="b8717-114"><strong>Eszközök</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-114"><strong>Get the tools</strong></span></span></td>
        <td><span data-ttu-id="b8717-115">Töltse le a BizTalk szolgáltatások SDK telepítése a Visual Studio projektsablonjai a helyi fejlesztési számítógépen.</span><span class="sxs-lookup"><span data-stu-id="b8717-115">Download the BizTalk Services SDK to install the Visual Studio project templates on your on-premises development computer.</span></span> <span data-ttu-id="b8717-116">Ezek a sablonok létrehozása a <strong>BizTalk szolgáltatások</strong> (híd) és a <strong>BizTalk szolgáltatás-összetevők</strong> (átalakítási) Visual Studio-projektek a BizTalk szolgáltatás telepített.</span><span class="sxs-lookup"><span data-stu-id="b8717-116">These templates create the <strong>BizTalk Services</strong> (bridge) and the <strong>BizTalk Service Artifacts</strong> (Transform) Visual Studio projects that are deployed to your BizTalk Service.</span></span>
        <br/><br/><span data-ttu-id="b8717-117">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335">Hogyan kezdhetem meg az Azure BizTalk Services SDK használatával </a> és <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">telepítése az Azure BizTalk szolgáltatások SDK</a> első lépések lépéseit sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="b8717-117">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302335"> How do I Start Using the Azure BizTalk Services SDK </a> and <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=241589">Installing the Azure BizTalk Services SDK</a> lists the steps to get started.</span></span>
        </td>
    </tr>
    <tr>
        <td><span data-ttu-id="b8717-118"><strong>Partner megállapodások létrehozása</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-118"><strong>Create partner agreements</strong></span></span></td>
        <td><span data-ttu-id="b8717-119">Megnyitja a Azure BizTalk Services portálra, ahol adja hozzá a partnerek, és hozzon létre X12, AS2, Azure-platformon futó és EDIFACT EDI-szerződések.</span><span class="sxs-lookup"><span data-stu-id="b8717-119">Opens the Azure BizTalk Services Portal hosted on Azure where you add partners and create X12, AS2, and EDIFACT EDI agreements.</span></span>
        <br/><br/><span data-ttu-id="b8717-120">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">BizTalk szolgáltatások Portal üzenetküldéshez EDI összetevők konfigurálása</a> első lépések lépéseit sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="b8717-120">
        <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configuring Components for EDI Messaging on BizTalk Services Portal</a> lists the steps to get started.</span></span>
        </td>
    </tr>

<tr>
        <td><span data-ttu-id="b8717-121"><strong>További információ a BizTalk szolgáltatások</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-121"><strong>Learn more about BizTalk Services</strong></span></span></td>
        <td><span data-ttu-id="b8717-122">Lépjen a <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">center tanulási</a> Azure BizTalk szolgáltatások tájékozódhat.</span><span class="sxs-lookup"><span data-stu-id="b8717-122">Go to the <a HREF="http://azure.microsoft.com/documentation/services/biztalk-services/">learning center</a> to learn more about Azure BizTalk Services.</span></span></td>
</tr>
</table>


<span data-ttu-id="b8717-123">A tálcán a lap alján, a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="b8717-123">In the task bar at the bottom, you can:</span></span>

<table border="1">

<tr>
<td><span data-ttu-id="b8717-124"><strong>Kezelése</strong> az alkalmazások központi telepítésének</span><span class="sxs-lookup"><span data-stu-id="b8717-124"><strong>Manage</strong> your application deployment</span></span></td>
<td><span data-ttu-id="b8717-125">Megnyitja a Azure BizTalk Services portálra.</span><span class="sxs-lookup"><span data-stu-id="b8717-125">Opens the Azure BizTalk Services portal.</span></span> <span data-ttu-id="b8717-126">A BizTalk szolgáltatások portál nem EDI-konfiguráció hozzáadása a partnerek és létrehozása X12, AS2, beleértve a Megjelenés és EDIFACT-egyezmények.</span><span class="sxs-lookup"><span data-stu-id="b8717-126">The BizTalk Services Portal is the entrance to EDI configuration, including adding partners and creating X12, AS2, and EDIFACT agreements.</span></span>
<br/><br/>
<span data-ttu-id="b8717-127">Ez az azonos <strong>egyezmények létrehozása</strong> a a <strong>gyors üzembe helyezés</strong> lapon.</span><span class="sxs-lookup"><span data-stu-id="b8717-127">This is the same as <strong>Create partner agreements</strong> on the <strong>Quick Start</strong> tab.</span></span>
<br/><br/><span data-ttu-id="b8717-128">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">BizTalk szolgáltatások Portal üzenetküldéshez EDI összetevők konfigurálása</a> nyújt részletesebb információt a BizTalk Services portálra.</span><span class="sxs-lookup"><span data-stu-id="b8717-128">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configuring Components for EDI Messaging on BizTalk Services Portal</a> provides more information on the BizTalk Services Portal.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="b8717-129"><strong>Kapcsolat információi</strong> , a hozzáférés-vezérlési Namespace</span><span class="sxs-lookup"><span data-stu-id="b8717-129"><strong>Connection Information</strong> of the Access Control Namespace</span></span></td>
<td><span data-ttu-id="b8717-130">Ha bejelöli a kapcsolati adatokat, majd a hozzáférés-vezérlési Namespace, az alapértelmezett kibocsátó és a kulcs alapértelmezett jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="b8717-130">When you select Connection Information, then the Access Control Namespace, Default Issuer, and Default Key are displayed.</span></span> <span data-ttu-id="b8717-131">Ezek az értékek másolhatja.</span><span class="sxs-lookup"><span data-stu-id="b8717-131">You can copy these values.</span></span>
<br/><br/>
<span data-ttu-id="b8717-132">A hozzáférés-vezérlő portál is megnyithatja.</span><span class="sxs-lookup"><span data-stu-id="b8717-132">You can also open the Access Control Portal.</span></span> <span data-ttu-id="b8717-133"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Hozzon létre egy hozzáférés-vezérlés Namespace</a> a hozzáférés-vezérlő Portal nyújt részletesebb információt.</span><span class="sxs-lookup"><span data-stu-id="b8717-133"><a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Create an Access control Namespace</a> provides more information on the Access Control Portal.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="b8717-134"><strong>Kulcsok szinkronizálása</strong> tárfiókban</span><span class="sxs-lookup"><span data-stu-id="b8717-134"><strong>Sync Keys</strong> in the Storage Account</span></span></td>
<td><span data-ttu-id="b8717-135">Storage-fiók létrehozásakor automatikusan létrejön egy elsődleges kulcs és egy másodlagos kulcs.</span><span class="sxs-lookup"><span data-stu-id="b8717-135">When you create a Storage account, a Primary Key and Secondary Key are automatically created.</span></span> <span data-ttu-id="b8717-136">A titkosítási kulcsokat a Tárfiók való hozzáférés szabályozása.</span><span class="sxs-lookup"><span data-stu-id="b8717-136">These encryption Keys control access to your Storage Account.</span></span> <span data-ttu-id="b8717-137">A BizTalk szolgáltatás automatikusan az elsődleges kulcsot használja.</span><span class="sxs-lookup"><span data-stu-id="b8717-137">Your BizTalk Service automatically uses the Primary Key.</span></span> <span data-ttu-id="b8717-138"><strong>Kulcsok szinkronizálása</strong> engedélyezése a felhasználóknak a BizTalk szolgáltatás megszakítása nélkül az elsődleges és a másodlagos kulcs közötti váltáshoz.</span><span class="sxs-lookup"><span data-stu-id="b8717-138"><strong>Sync Keys</strong> enable users to switch between the Primary Key and the Secondary Key without disrupting the BizTalk Service.</span></span>
<br/><br/>
<span data-ttu-id="b8717-139">Például azt szeretné a BizTalk szolgáltatás egy új elsődleges kulcsot a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="b8717-139">For example, you want the BizTalk Service to use a new Primary Key for the Storage Account.</span></span> <span data-ttu-id="b8717-140">Ehhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="b8717-140">To do this:</span></span>
<br/><br/>
<ol>
<li><span data-ttu-id="b8717-141">Válassza ki a BizTalk szolgáltatás, majd <strong>szinkronizálási kulcsok</strong>.</span><span class="sxs-lookup"><span data-stu-id="b8717-141">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="b8717-142">Válassza ki a másodlagos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="b8717-142">Select the Secondary Key.</span></span> <span data-ttu-id="b8717-143">Ha így tesz, a BizTalk szolgáltatás elindul, a másodlagos kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="b8717-143">When you do this, the BizTalk Service starts using the Secondary Key.</span></span></li>
<li><span data-ttu-id="b8717-144">A klasszikus Azure portálon válassza ki a tárfiók, és az elsődleges kulcs újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="b8717-144">In the Azure classic portal, select your Storage account and Regenerate the Primary Key.</span></span> <span data-ttu-id="b8717-145">Ne feledje, hogy a BizTalk szolgáltatás használ a másodlagos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="b8717-145">Remember, your BizTalk Service is using the Secondary Key.</span></span></li>
<li><span data-ttu-id="b8717-146">Válassza ki a BizTalk szolgáltatás, majd <strong>szinkronizálási kulcsok</strong>.</span><span class="sxs-lookup"><span data-stu-id="b8717-146">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="b8717-147">Jelölje ki, az elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="b8717-147">Now, select the Primary Key.</span></span> <span data-ttu-id="b8717-148">Az új elsődleges kulcs, akkor a rendszer újra létrehozza azt.</span><span class="sxs-lookup"><span data-stu-id="b8717-148">This is the new Primary Key you regenerated.</span></span></li>
<li><span data-ttu-id="b8717-149">A klasszikus Azure portálon válassza ki a tárfiók, és a másodlagos kulcs újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="b8717-149">In the Azure classic portal, select your Storage account and Regenerate the Secondary Key.</span></span></li>
</ol>
<br/>
<span data-ttu-id="b8717-150">A folyamat elnevezése "helyettesítő kulcsok".</span><span class="sxs-lookup"><span data-stu-id="b8717-150">This process is called "rollover keys".</span></span> <span data-ttu-id="b8717-151">A célja engedélyezése a felhasználóknak a BizTalk szolgáltatás megszakítása nélkül az elsődleges és a másodlagos kulcs közötti váltáshoz.</span><span class="sxs-lookup"><span data-stu-id="b8717-151">The purpose is to enable users to switch between the Primary Key and the Secondary Key without disrupting the BizTalk Service.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="b8717-152"><strong>Törlés</strong> az alkalmazás</span><span class="sxs-lookup"><span data-stu-id="b8717-152"><strong>Delete</strong> your application</span></span></td>
<td><span data-ttu-id="b8717-153">A BizTalk szolgáltatás törléséhez és telepített összes elemek eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="b8717-153">When you select Delete, your BizTalk Service and all items deployed to it are removed.</span></span></td>
</tr>
</table>


## <a name="dashboard"></a><span data-ttu-id="b8717-154">Irányítópult</span><span class="sxs-lookup"><span data-stu-id="b8717-154">Dashboard</span></span>
<span data-ttu-id="b8717-155">BizTalk szolgáltatások kiadásoknál felsorolt összes beállítások nem érhetők el.</span><span class="sxs-lookup"><span data-stu-id="b8717-155">Depending on the BizTalk Services Edition, all options listed may not be available.</span></span> 

<span data-ttu-id="b8717-156">Akkor válassza ki a BizTalk szolgáltatás nevét, az irányítópult fület jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b8717-156">When you select your BizTalk Service name, the Dashboard tab is displayed.</span></span> <span data-ttu-id="b8717-157">Az irányítópult a következőket teheti:</span><span class="sxs-lookup"><span data-stu-id="b8717-157">In Dashboard, you can:</span></span>

##### <a name="usage-overview-shows-the-number-of-used-hybrid-connections"></a><span data-ttu-id="b8717-158">Használati áttekintése: Használt hibrid kapcsolatok számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b8717-158">Usage Overview: Shows the number of used Hybrid Connections</span></span>
<span data-ttu-id="b8717-159">Megjeleníti a használati adatok is GB-ban.</span><span class="sxs-lookup"><span data-stu-id="b8717-159">Also displays the data usage in GB.</span></span> 

##### <a name="metric-graph-shows-a-fixed-list-of-performance-metrics"></a><span data-ttu-id="b8717-160">Metrika Graph: Metrikák rögzített listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b8717-160">Metric Graph: Shows a fixed list of performance metrics</span></span>
<span data-ttu-id="b8717-161">Ezeket a mérési adja meg a BizTalk szolgáltatás valós idejű értékeit.</span><span class="sxs-lookup"><span data-stu-id="b8717-161">These metrics provide real-time values regarding the health of the BizTalk Service.</span></span> <span data-ttu-id="b8717-162">Is beállíthatja a **relatív** vagy **abszolút** értékek és időtartomány **időköz** számos a metrika a grafikonon megjelenített.</span><span class="sxs-lookup"><span data-stu-id="b8717-162">You can also choose the **Relative** or **Absolute** values and the time range **Interval** of the metrics that are displayed in the graph.</span></span> 

<span data-ttu-id="b8717-163">A metrikák leírását itt [elérhető](#Metrics) ebben a témakörben.</span><span class="sxs-lookup"><span data-stu-id="b8717-163">For a description of these performance metrics, go to [Available Metrics](#Metrics) in this topic.</span></span>

##### <a name="quick-glance-lists-your-biztalk-service-properties"></a><span data-ttu-id="b8717-164">Gyors áttekintő: Felsorolja a BizTalk szolgáltatás tulajdonságai</span><span class="sxs-lookup"><span data-stu-id="b8717-164">Quick Glance: Lists your BizTalk Service properties</span></span>
<table border="1">

<tr>
<td><span data-ttu-id="b8717-165"><strong>Követés adatbázis-hitelesítő adatok frissítése</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-165"><strong>Update Tracking Database credentials</strong></span></span></td>
<td><span data-ttu-id="b8717-166">Módosítja a felhasználónevet és a nyomkövetési adatbázis szolgáltatásba való bejelentkezéshez használt jelszót.</span><span class="sxs-lookup"><span data-stu-id="b8717-166">Changes the user name and password used to log into the Tracking Database.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-167"><strong>SSL-tanúsítvány frissítése</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-167"><strong>Update SSL Certificate</strong></span></span></td>
<td><span data-ttu-id="b8717-168">Frissítheti a BizTalk szolgáltatás különböző SSL-tanúsítvány használatára.</span><span class="sxs-lookup"><span data-stu-id="b8717-168">Can update the BizTalk Service to use a different SSL certificate.</span></span> <span data-ttu-id="b8717-169">Egy önaláírt SSL-tanúsítvány mikor automatikusan létrejön, <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">a BizTalk szolgáltatás létrehozása</a>.</span><span class="sxs-lookup"><span data-stu-id="b8717-169">A self-signed SSL certificate is automatically created when you <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">create the BizTalk Service</a>.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-170"><strong>Tanúsítvány letöltése</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-170"><strong>Download Certificate</strong></span></span></td>
<td><span data-ttu-id="b8717-171">Letöltheti a BizTalk szolgáltatás a helyi számítógép által használt SSL-tanúsítványt.</span><span class="sxs-lookup"><span data-stu-id="b8717-171">You can download the SSL certificate used by your BizTalk Service to a local machine.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-172"><strong>Állapot</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-172"><strong>Status</strong></span></span></td>
<td><span data-ttu-id="b8717-173">A BizTalk szolgáltatás aktuális állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b8717-173">Displays the current status of your BizTalk Service.</span></span> <span data-ttu-id="b8717-174">Lásd: <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk szolgáltatások: szolgáltatás állapota diagram</a>.</span><span class="sxs-lookup"><span data-stu-id="b8717-174">See <a HREF="http://go.microsoft.com/fwlink/p/?LinkID=329870">BizTalk Services: Service state chart</a>.</span></span> </td>
</tr>
<tr>
<td><span data-ttu-id="b8717-175"><strong>URL-címe</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-175"><strong>Service URL</strong></span></span></td>
<td><span data-ttu-id="b8717-176">A BizTalk szolgáltatás URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="b8717-176">The URL for your BizTalk Service.</span></span> <span data-ttu-id="b8717-177">Ez megegyezik a <strong>tartomány URL-cím</strong> a BizTalk szolgáltatás létrehozásakor megadott.</span><span class="sxs-lookup"><span data-stu-id="b8717-177">This is the same as the <strong>Domain URL</strong> entered when your BizTalk Service is created.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-178"><strong>Nyilvános virtuális IP-cím (VIP) címe</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-178"><strong>Public Virtual IP (VIP) Address</strong></span></span></td>
<td><span data-ttu-id="b8717-179">A BizTalk szolgáltatás hozzárendelt IP-címet.</span><span class="sxs-lookup"><span data-stu-id="b8717-179">The IP address assigned to your BizTalk Service.</span></span> <span data-ttu-id="b8717-180">Az összes bemeneti végpont szolgál, és a kimenő adatforgalmat a forrás-címe.</span><span class="sxs-lookup"><span data-stu-id="b8717-180">It is used for all input endpoints and is the source address for outbound traffic.</span></span> <span data-ttu-id="b8717-181">Az IP-cím, amíg létrejön a BizTalk szolgáltatás tartozik.</span><span class="sxs-lookup"><span data-stu-id="b8717-181">This IP address belongs to your BizTalk Service as long as it is created.</span></span> <span data-ttu-id="b8717-182">Ha törli a BizTalk szolgáltatás, az IP-cím van hozzárendelve egy másik BizTalk szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="b8717-182">If you delete the BizTalk Service, the IP address is assigned to another BizTalk Service.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-183"><strong>Az ACS-Namespace</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-183"><strong>ACS Namespace</strong></span></span></td>
<td><span data-ttu-id="b8717-184">A BizTalk szolgáltatás végzi a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="b8717-184">Authenticates with the BizTalk Service.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-185"><strong>Kiadás</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-185"><strong>Edition</strong></span></span></td>
<td><span data-ttu-id="b8717-186">Listák kiadásának kitölti a a BizTalk szolgáltatás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="b8717-186">Lists the Edition entered when the BizTalk Service is created.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-187"><strong>Hely</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-187"><strong>Location</strong></span></span></td>
<td><span data-ttu-id="b8717-188">A földrajzi régiót, amelyen a BizTalk szolgáltatás jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b8717-188">Displays the geographic region that hosts your BizTalk Service.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-189"><strong>Létrehozva</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-189"><strong>Created</strong></span></span></td>
<td><span data-ttu-id="b8717-190">A dátum és a BizTalk szolgáltatás létrehozásakor jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b8717-190">Displays the date and time the BizTalk Service was created.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-191"><strong>Adatbázis nyomon követése</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-191"><strong>Tracking Database</strong></span></span></td>
<td><span data-ttu-id="b8717-192">Az Azure SQL-adatbázis neve, amely tárolja a nyomkövetési táblát, a BizTalk szolgáltatás által használt.</span><span class="sxs-lookup"><span data-stu-id="b8717-192">The Azure SQL Database name that stores the tracking tables used by your BizTalk Service.</span></span> 
<br/><br/><span data-ttu-id="b8717-193">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Követelmények Explained</a> a követési adatbázis részleteit.</span><span class="sxs-lookup"><span data-stu-id="b8717-193">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Requirements Explained</a>  provides details on the Tracking Database.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-194"><strong>Tárolási figyelés/archiválása</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-194"><strong>Monitoring/Archiving Storage</strong></span></span></td>
<td><span data-ttu-id="b8717-195">A figyelési kimenetet a BizTalk szolgáltatás Azure Storage fióknevet.</span><span class="sxs-lookup"><span data-stu-id="b8717-195">The Azure Storage account name that stores the monitoring output of your BizTalk Service.</span></span>
<br/><br/><span data-ttu-id="b8717-196">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Követelmények Explained</a> részleteit a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="b8717-196">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=302280">Requirements Explained</a>  provides details on the Storage account.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-197"><strong>Előfizetés neve</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-197"><strong>Subscription Name</strong></span></span></td>
<td><span data-ttu-id="b8717-198">Az előfizetés, amelyen a BizTalk szolgáltatás sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="b8717-198">Lists the subscription that hosts your BizTalk Service.</span></span> <span data-ttu-id="b8717-199">Az előfizetés szabályozza a klasszikus Azure portál eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="b8717-199">The subscription governs access to the Azure classic portal.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-200"><strong>Előfizetés-azonosító</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-200"><strong>Subscription ID</strong></span></span></td>
<td><span data-ttu-id="b8717-201">Előfizetés létrehozásakor a rendszer automatikusan előállítja az előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="b8717-201">When a subscription is created, a subscription ID is automatically generated.</span></span> <span data-ttu-id="b8717-202">REST API-k használatakor esetleg adja meg az előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="b8717-202">When using REST APIs, you may need to enter the Subscription ID.</span></span></td>
</tr>
</table>

<span data-ttu-id="b8717-203">[BizTalk szolgáltatások: Telepítési használata Azure klasszikus portálon](http://go.microsoft.com/fwlink/p/?LinkID=302280) BizTalk szolgáltatás létrehozása lépéseit sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="b8717-203">[BizTalk Services: Provisioning Using Azure classic portal](http://go.microsoft.com/fwlink/p/?LinkID=302280) lists the steps to create a BizTalk Service.</span></span>

##### <a name="manage-connection-information-sync-keys-and-delete-in-the-task-bar"></a><span data-ttu-id="b8717-204">Kezelése, a kapcsolatadatok, a szinkronizálási kulcsokat, és a tálcán törlése:</span><span class="sxs-lookup"><span data-stu-id="b8717-204">Manage, Connection Information, Sync Keys, and Delete in the task bar:</span></span>
<table border="1">

<tr>
<td><span data-ttu-id="b8717-205"><strong>Kezelése</strong> az alkalmazások központi telepítésének</span><span class="sxs-lookup"><span data-stu-id="b8717-205"><strong>Manage</strong> your application deployment</span></span></td>
<td><span data-ttu-id="b8717-206">Megnyitja a Azure BizTalk Services portálra.</span><span class="sxs-lookup"><span data-stu-id="b8717-206">Opens the Azure BizTalk Services Portal.</span></span> <span data-ttu-id="b8717-207">A BizTalk szolgáltatások portál nem EDI-konfiguráció hozzáadása a partnerek és létrehozása X12, AS2, beleértve a Megjelenés és EDIFACT-egyezmények.</span><span class="sxs-lookup"><span data-stu-id="b8717-207">The BizTalk Services Portal is the entrance to EDI configuration, including adding partners and creating X12, AS2, and EDIFACT agreements.</span></span>
<br/><br/>
<span data-ttu-id="b8717-208">Ez az azonos <strong>egyezmények létrehozása</strong> a a <strong>gyors üzembe helyezés</strong> lapon.</span><span class="sxs-lookup"><span data-stu-id="b8717-208">This is the same as <strong>Create partner agreements</strong> on the <strong>Quick Start</strong> tab.</span></span>
<br/><br/><span data-ttu-id="b8717-209">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">BizTalk szolgáltatások Portal üzenetküldéshez EDI összetevők konfigurálása</a> nyújt részletesebb információt a BizTalk Services portálra.</span><span class="sxs-lookup"><span data-stu-id="b8717-209">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=303653">Configuring Components for EDI Messaging on BizTalk Services Portal</a> provides more information on the BizTalk Services Portal.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-210"><strong>Kapcsolat információi</strong> , a hozzáférés-vezérlési Namespace</span><span class="sxs-lookup"><span data-stu-id="b8717-210"><strong>Connection Information</strong> of the Access Control Namespace</span></span></td>
<td><span data-ttu-id="b8717-211">Megjeleníti a hozzáférés-vezérlési Namespace, alapértelmezett kibocsátó és a kulcs alapértelmezett értékek; amely másolhatók.</span><span class="sxs-lookup"><span data-stu-id="b8717-211">Displays the Access Control Namespace, Default Issuer, and Default Key values; which can be copied.</span></span>
<br/><br/>
<span data-ttu-id="b8717-212">A hozzáférés-vezérlő portál is megnyithatja.</span><span class="sxs-lookup"><span data-stu-id="b8717-212">You can also open the Access Control Portal.</span></span> <span data-ttu-id="b8717-213">A hozzáférés-vezérlő portál megegyezik az Active Directory beállításával a bal oldali navigációs ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="b8717-213">This Access Control Portal is the same as using the Active Directory option in the left navigation pane.</span></span>
<br/><br/><span data-ttu-id="b8717-214">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Az ACS-Namespace kezelése</a> a hozzáférés-vezérlő Portal nyújt részletesebb információt.</span><span class="sxs-lookup"><span data-stu-id="b8717-214">
<a HREF="http://go.microsoft.com/fwlink/p/?LinkID=285670">Managing Your ACS Namespace</a> provides more information on the Access Control Portal.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-215"><strong>Kulcsok szinkronizálása</strong> tárfiókban</span><span class="sxs-lookup"><span data-stu-id="b8717-215"><strong>Sync Keys</strong> in the Storage Account</span></span></td>
<td><span data-ttu-id="b8717-216">Storage-fiók létrehozásakor automatikusan létrejön egy elsődleges kulcs és egy másodlagos kulcs.</span><span class="sxs-lookup"><span data-stu-id="b8717-216">When you create a Storage account, a Primary Key and Secondary Key are automatically created.</span></span> <span data-ttu-id="b8717-217">A titkosítási kulcsokat a Tárfiók való hozzáférés szabályozása.</span><span class="sxs-lookup"><span data-stu-id="b8717-217">These encryption Keys control access to your Storage Account.</span></span> <span data-ttu-id="b8717-218">A BizTalk szolgáltatás automatikusan az elsődleges kulcsot használja.</span><span class="sxs-lookup"><span data-stu-id="b8717-218">Your BizTalk Service automatically uses the Primary Key.</span></span> <span data-ttu-id="b8717-219"><strong>Kulcsok szinkronizálása</strong> engedélyezése a felhasználóknak a BizTalk szolgáltatás megszakítása nélkül az elsődleges és a másodlagos kulcs közötti váltáshoz.</span><span class="sxs-lookup"><span data-stu-id="b8717-219"><strong>Sync Keys</strong> enable users to switch between the Primary Key and the Secondary Key without disrupting the BizTalk Service.</span></span>
<br/><br/>
<span data-ttu-id="b8717-220">Például azt szeretné a BizTalk szolgáltatás egy új elsődleges kulcsot a tárfiók.</span><span class="sxs-lookup"><span data-stu-id="b8717-220">For example, you want the BizTalk Service to use a new Primary Key for the Storage Account.</span></span> <span data-ttu-id="b8717-221">Ehhez tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="b8717-221">To do this:</span></span>
<br/><br/>
<ol>
<li><span data-ttu-id="b8717-222">Válassza ki a BizTalk szolgáltatás, majd <strong>szinkronizálási kulcsok</strong>.</span><span class="sxs-lookup"><span data-stu-id="b8717-222">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="b8717-223">Válassza ki a másodlagos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="b8717-223">Select the Secondary Key.</span></span> <span data-ttu-id="b8717-224">Ha így tesz, a BizTalk szolgáltatás elindul, a másodlagos kulcs használatával.</span><span class="sxs-lookup"><span data-stu-id="b8717-224">When you do this, the BizTalk Service starts using the Secondary Key.</span></span></li>
<li><span data-ttu-id="b8717-225">A klasszikus Azure portálon válassza ki a tárfiók, és az elsődleges kulcs újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="b8717-225">In the Azure classic portal, select your Storage account and Regenerate the Primary Key.</span></span> <span data-ttu-id="b8717-226">Ne feledje, hogy a BizTalk szolgáltatás használ a másodlagos kulcsot.</span><span class="sxs-lookup"><span data-stu-id="b8717-226">Remember, your BizTalk Service is using the Secondary Key.</span></span></li>
<li><span data-ttu-id="b8717-227">Válassza ki a BizTalk szolgáltatás, majd <strong>szinkronizálási kulcsok</strong>.</span><span class="sxs-lookup"><span data-stu-id="b8717-227">Select your BizTalk Service and select <strong>Sync Keys</strong>.</span></span> <span data-ttu-id="b8717-228">Jelölje ki, az elsődleges kulcs.</span><span class="sxs-lookup"><span data-stu-id="b8717-228">Now, select the Primary Key.</span></span> <span data-ttu-id="b8717-229">Az új elsődleges kulcs, akkor a rendszer újra létrehozza azt.</span><span class="sxs-lookup"><span data-stu-id="b8717-229">This is the new Primary Key you regenerated.</span></span></li>
<li><span data-ttu-id="b8717-230">A klasszikus Azure portálon válassza ki a tárfiók, és a másodlagos kulcs újragenerálása.</span><span class="sxs-lookup"><span data-stu-id="b8717-230">In the Azure classic portal, select your Storage account and Regenerate the Secondary Key.</span></span></li>
</ol>
<br/>
<span data-ttu-id="b8717-231">A folyamat elnevezése "helyettesítő kulcsok".</span><span class="sxs-lookup"><span data-stu-id="b8717-231">This process is called "rollover keys".</span></span> <span data-ttu-id="b8717-232">A célja engedélyezése a felhasználóknak a BizTalk szolgáltatás megszakítása nélkül az elsődleges és a másodlagos kulcs közötti váltáshoz.</span><span class="sxs-lookup"><span data-stu-id="b8717-232">The purpose is to enable users to switch between the Primary Key and the Secondary Key without disrupting the BizTalk Service.</span></span></td>
</tr>

<tr>
<td><span data-ttu-id="b8717-233"><strong>Törlés</strong> az alkalmazás</span><span class="sxs-lookup"><span data-stu-id="b8717-233"><strong>Delete</strong> your application</span></span></td>
<td><span data-ttu-id="b8717-234">A BizTalk szolgáltatás és a telepített összes elemet a rendszer eltávolítja.</span><span class="sxs-lookup"><span data-stu-id="b8717-234">Your BizTalk Service and all items deployed to it are removed.</span></span></td>
</tr>
</table>


## <a name="monitor"></a><span data-ttu-id="b8717-235">Figyelés</span><span class="sxs-lookup"><span data-stu-id="b8717-235">Monitor</span></span>
<span data-ttu-id="b8717-236">Nem vonatkozik az ingyenes kiadásra.</span><span class="sxs-lookup"><span data-stu-id="b8717-236">Does not apply to the Free Edition.</span></span>

<span data-ttu-id="b8717-237">A BizTalk szolgáltatás neve kiválasztásakor a figyelés lapján érhető el, és megjeleníti a következő:</span><span class="sxs-lookup"><span data-stu-id="b8717-237">When you select your BizTalk Service name, the Monitor tab is available and displays the following:</span></span>

##### <a name="metric-graph-displays-the-selected-performance-metrics"></a><span data-ttu-id="b8717-238">Metrika Graph: Megjeleníti a kijelölt metrikák</span><span class="sxs-lookup"><span data-stu-id="b8717-238">Metric Graph: Displays the selected performance metrics</span></span>
<span data-ttu-id="b8717-239">Ezeket a mérési adja meg a BizTalk szolgáltatás valós idejű értékeit.</span><span class="sxs-lookup"><span data-stu-id="b8717-239">These metrics provide real-time values regarding the health of the BizTalk Service.</span></span> <span data-ttu-id="b8717-240">Úgy dönt, hogy mely metrikák jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="b8717-240">You choose which performance metrics are displayed.</span></span> <span data-ttu-id="b8717-241">Egyszerre legfeljebb hat teljesítménymutatók megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="b8717-241">A maximum of six performance metrics can be displayed simultaneously.</span></span> 

<span data-ttu-id="b8717-242">Is beállíthatja a **relatív** vagy **abszolút** értékek és időtartomány **időköz** a megjelenített mérőszámokat.</span><span class="sxs-lookup"><span data-stu-id="b8717-242">You can also choose the **Relative** or **Absolute** values and the time range **Interval** of the metrics that are displayed.</span></span> 

##### <a name="to-remove-or-display-metrics-in-the-graph"></a><span data-ttu-id="b8717-243">Távolítsa el, vagy a grafikonon metrikák megjelenítése:</span><span class="sxs-lookup"><span data-stu-id="b8717-243">To remove or display metrics in the graph:</span></span>
1. <span data-ttu-id="b8717-244">Válassza ki a **figyelő** fülre.</span><span class="sxs-lookup"><span data-stu-id="b8717-244">Select the **Monitor** tab.</span></span>
2. <span data-ttu-id="b8717-245">Válassza ki **metrikák hozzáadása** a tálcán:</span><span class="sxs-lookup"><span data-stu-id="b8717-245">Select **Add Metrics** in the task bar:</span></span>  
   <span data-ttu-id="b8717-246">![Válassza ki a metrikák hozzáadása][AddMetrics]</span><span class="sxs-lookup"><span data-stu-id="b8717-246">![Select Add Metrics][AddMetrics]</span></span>
3. <span data-ttu-id="b8717-247">Ellenőrizze a teljesítményi értékeket, akkor jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b8717-247">Check the performance metrics you want to display.</span></span>
4. <span data-ttu-id="b8717-248">Válassza ki a pipára térjen vissza a **figyelő** fülre.</span><span class="sxs-lookup"><span data-stu-id="b8717-248">Select the checkmark to return to the **Monitor** tab.</span></span>
5. <span data-ttu-id="b8717-249">Válassza ki a kör mellett a metrika az adott metrika értéket a grafikonon jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b8717-249">Select the circle next to the metric to display that metric's value in the graph.</span></span>  
   
    <span data-ttu-id="b8717-250">Például a **CPU-használat** metrika szürkén jelenik meg; kimenetét a diagramhoz nem jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="b8717-250">For example, the **CPU Usage** metric is grayed out; its output is not displayed in the graph:</span></span>  
   <span data-ttu-id="b8717-251">![CPU-használat metrika szürkén jelenik meg][GrayedMetric]</span><span class="sxs-lookup"><span data-stu-id="b8717-251">![CPU Usage metric is grayed out][GrayedMetric]</span></span>  
   
    <span data-ttu-id="b8717-252">Kimenő kör engedélyezéséhez válassza a szürke a **CPU-használat** metrika kimenetét a diagramon megjelenítendő:</span><span class="sxs-lookup"><span data-stu-id="b8717-252">Select the grayed out circle to enable the **CPU Usage** metric to display its output in the graph:</span></span>  
   <span data-ttu-id="b8717-253">![CPU-használat metrika engedélyezve van][EnabledMetric]</span><span class="sxs-lookup"><span data-stu-id="b8717-253">![CPU Usage metric is enabled][EnabledMetric]</span></span>
6. <span data-ttu-id="b8717-254">A megjelenített diagram és a lista a metrika eltávolításához jelölje ki **törölje metrikát** a tálcán.</span><span class="sxs-lookup"><span data-stu-id="b8717-254">To remove a metric from the display graph and the list, select **Delete Metric** in the task bar.</span></span> <span data-ttu-id="b8717-255">A metrika vissza felvenni a listára, válassza ki **metrikák hozzáadása** a tálcán, ellenőrizze a mérték, és válassza ki a pipa való visszatéréshez a **figyelő** fülre.</span><span class="sxs-lookup"><span data-stu-id="b8717-255">To add the metric back to the list, select **Add Metrics** in the task bar, check the metric, and select the checkmark to return to the **Monitor** tab.</span></span> <span data-ttu-id="b8717-256">Válassza ki a a szürke ahhoz, hogy a mérték kör ki.</span><span class="sxs-lookup"><span data-stu-id="b8717-256">Select the grayed out circle to enable the metric.</span></span>

## <span data-ttu-id="b8717-257"><a name="Metrics"></a>Elérhető</span><span class="sxs-lookup"><span data-stu-id="b8717-257"><a name="Metrics"></a>Available Metrics</span></span>
<span data-ttu-id="b8717-258">A következő számlálók/metrikák érhetők el:</span><span class="sxs-lookup"><span data-stu-id="b8717-258">The following performance counters/metrics are available:</span></span>

<table border="1">

<tr>
<td><span data-ttu-id="b8717-259"><strong>RountdTrip késés</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-259"><strong>RountdTrip Latency</strong></span></span></td>
<td><span data-ttu-id="b8717-260">Az átlagos ideje ezredmásodpercben (ms) feldolgozni egy üzenetet abból az időből, amikor az üzenetet kapja, amíg a teljes dolgozza fel a BizTalk szolgáltatás összes hidak jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b8717-260">Displays the average time taken in milliseconds (ms) to process a message from the time the message is received until the message is fully processed by the BizTalk Service across all bridges.</span></span> <span data-ttu-id="b8717-261">Csak a sikeresen feldolgozott üzeneteinek bájtjai számítanak.</span><span class="sxs-lookup"><span data-stu-id="b8717-261">Only messages successfully processed are counted.</span></span><br/><br/>
<span data-ttu-id="b8717-262">A következő események következnek be, amikor egy Timestamp típusú jön létre:</span><span class="sxs-lookup"><span data-stu-id="b8717-262">When the following events occur, a timestamp is created:</span></span>
<ul>
<li><span data-ttu-id="b8717-263">Üzenet kerül, az átjáró</span><span class="sxs-lookup"><span data-stu-id="b8717-263">Message enters the gateway</span></span></li>
<li><span data-ttu-id="b8717-264">Üzenet annak biztosítására, hogy a cél</span><span class="sxs-lookup"><span data-stu-id="b8717-264">Message is routed to the destination</span></span></li>
<li><span data-ttu-id="b8717-265">Cél választ</span><span class="sxs-lookup"><span data-stu-id="b8717-265">Destination response is received</span></span></li>
<li><span data-ttu-id="b8717-266">Cél nyugtázási választ küldött az átjáró</span><span class="sxs-lookup"><span data-stu-id="b8717-266">Destination acknowledgement response sent to the gateway</span></span></li>
</ul>
<br/>
<span data-ttu-id="b8717-267">Ez a mérőszám a következő számítás eredményét mutatja:</span><span class="sxs-lookup"><span data-stu-id="b8717-267">This metric shows the result of the following calculation:</span></span>
<br/><br/>
<span data-ttu-id="b8717-268">[Cél nyugtázási küldött választ az átjáró] - [üzenet kerül, az átjáró]</span><span class="sxs-lookup"><span data-stu-id="b8717-268">[Destination acknowledgement response sent to the gateway] - [Message enters the gateway]</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-269"><strong>Hibákat forrásnál</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-269"><strong>Failures At Source</strong></span></span></td>
<td><span data-ttu-id="b8717-270">Nem sikerült üzenetek teljes számát jeleníti meg a BizTalk szolgáltatás, ha a forrás-végpontok üzeneteit húzza.</span><span class="sxs-lookup"><span data-stu-id="b8717-270">Displays the total number of messages that failed by the BizTalk Service when pulling messages from the source endpoints.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-271"><strong>CPU-használat</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-271"><strong>CPU Usage</strong></span></span></td>
<td><span data-ttu-id="b8717-272">Az átlagos processzoridő % található összes szerepkörpéldány sorolja fel.</span><span class="sxs-lookup"><span data-stu-id="b8717-272">Lists the average %Processor Time of all role instances.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-273"><strong>Feldolgozási késleltetés</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-273"><strong>Processing Latency</strong></span></span></td>
<td><span data-ttu-id="b8717-274">Az átlagos ideje ezredmásodpercben (ms) feldolgozni egy üzenetet a BizTalk szolgáltatás összes hidak, kivéve az idő a célok jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b8717-274">Displays the average time taken In milliseconds (ms) to process a message by the BizTalk Service across all bridges, excluding the time spent in destinations.</span></span> <span data-ttu-id="b8717-275">Csak a sikeresen feldolgozott üzeneteinek bájtjai számítanak.</span><span class="sxs-lookup"><span data-stu-id="b8717-275">Only messages successfully processed are counted.</span></span><br/><br/>
<span data-ttu-id="b8717-276">A következő események bekövetkezésekor időbélyeg jön létre:</span><span class="sxs-lookup"><span data-stu-id="b8717-276">When each of the following events occur, a timestamp is created:</span></span>

<ul>
<li><span data-ttu-id="b8717-277">Üzenet kerül, az átjáró</span><span class="sxs-lookup"><span data-stu-id="b8717-277">Message enters the gateway</span></span></li>
<li><span data-ttu-id="b8717-278">Üzenet annak biztosítására, hogy a cél</span><span class="sxs-lookup"><span data-stu-id="b8717-278">Message is routed to the destination</span></span></li>
<li><span data-ttu-id="b8717-279">Cél választ</span><span class="sxs-lookup"><span data-stu-id="b8717-279">Destination response is received</span></span></li>
<li><span data-ttu-id="b8717-280">Cél nyugtázási választ küldött az átjáró</span><span class="sxs-lookup"><span data-stu-id="b8717-280">Destination acknowledgement response sent to the gateway</span></span></li>
</ul>
<br/><span data-ttu-id="b8717-281">Ez a mérőszám a következő számítás eredményét mutatja:</span><span class="sxs-lookup"><span data-stu-id="b8717-281">This metric shows the result of the following calculation:</span></span><br/><br/>
<span data-ttu-id="b8717-282">[Cél nyugtázási küldött választ az átjáró] - [üzenet kerül, az átjáró] - [cél választ] + [üzenet annak biztosítására, hogy a cél]</span><span class="sxs-lookup"><span data-stu-id="b8717-282">[Destination acknowledgement response sent to the gateway] - [Message enters the gateway] - [Destination response is received] + [Message is routed to the destination]</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-283"><strong>A folyamat sikertelen</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-283"><strong>Failures In Process</strong></span></span></td>
<td><span data-ttu-id="b8717-284">Nem sikerült feldolgozni a BizTalk szolgáltatás között a hidak időintervallumon belül-kezelési üzenetek teljes számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b8717-284">Displays the total number of messages that failed during processing by the BizTalk Service across all the bridges within a time interval.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-285"><strong>Küldött üzenetek</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-285"><strong>Messages Sent</strong></span></span></td>
<td><span data-ttu-id="b8717-286">Adott időintervallumon belül minden hidak között a BizTalk szolgáltatás által küldött üzenetek teljes számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b8717-286">Displays the total number of messages sent by the BizTalk Service across all bridges within a time interval.</span></span> <span data-ttu-id="b8717-287">Ez a metrika értéke akkor növekszik, ha a folyamat által küldött üzenet eléri az útvonal cél.</span><span class="sxs-lookup"><span data-stu-id="b8717-287">This metric is incremented when a message sent from a pipeline reaches the route destination.</span></span> <span data-ttu-id="b8717-288">Ez a metrika nem jelzi, hogy egy üzenet feldolgozása sikeresen.</span><span class="sxs-lookup"><span data-stu-id="b8717-288">This metric does not indicate that a message is successfully processed.</span></span><br/><br/>
<span data-ttu-id="b8717-289">A kérelem-válasz-forgatókönyvek a metrika értéke növekszik, ha az útvonal cél egy fogadását nyugtázási küld vissza a feldolgozási sor.</span><span class="sxs-lookup"><span data-stu-id="b8717-289">In a Request-Reply scenario, the metric is incremented when the route destination sends a receipt acknowledgement back to the pipeline.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-290"><strong>A Beérkezett üzenetek</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-290"><strong>Messages Received</strong></span></span></td>
<td><span data-ttu-id="b8717-291">Időintervallumon belül minden hidak között a BizTalk szolgáltatás által fogadott üzenetek teljes számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b8717-291">Displays the total number of messages received by the BizTalk Service across all bridges within a time interval.</span></span> <span data-ttu-id="b8717-292">Ez a metrika értéke akkor nő, amikor egy új üzenet jelenik meg a folyamat.</span><span class="sxs-lookup"><span data-stu-id="b8717-292">This metric is incremented when a new message is received by the pipeline.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-293"><strong>A folyamat üzenetek</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-293"><strong>Messages In Process</strong></span></span></td>
<td><span data-ttu-id="b8717-294">Megjeleníti az adott időintervallumon belül a BizTalk szolgáltatás által jelenleg feldolgozott üzenetek teljes száma.</span><span class="sxs-lookup"><span data-stu-id="b8717-294">Displays the total number of messages currently being processed by the BizTalk Service within a time interval.</span></span></td>
</tr>
<tr>
<td><span data-ttu-id="b8717-295"><strong>Az üzenetek feldolgozásának</strong></span><span class="sxs-lookup"><span data-stu-id="b8717-295"><strong>Messages Processed</strong></span></span></td>
<td><span data-ttu-id="b8717-296">Sikeresen feldolgozott üzenetek a BizTalk szolgáltatás közötti összes hidak időintervallumon belül teljes számát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="b8717-296">Displays the total number of messages successfully processed by the BizTalk Service across all bridges within a time interval.</span></span> <span data-ttu-id="b8717-297">Ez a metrika értéke akkor nő, amikor egy üzenetet a folyamat sikeresen fogad és sikeresen irányítja át a cél.</span><span class="sxs-lookup"><span data-stu-id="b8717-297">This metric is incremented when a message is successfully received by the pipeline and successfully routed to the destination.</span></span></td>
</tr>
</table>


## <a name="scale"></a><span data-ttu-id="b8717-298">Méretezés</span><span class="sxs-lookup"><span data-stu-id="b8717-298">Scale</span></span>
<span data-ttu-id="b8717-299">A skála lapon adhat hozzá vagy kivonása a BizTalk szolgáltatás által használt egységek száma.</span><span class="sxs-lookup"><span data-stu-id="b8717-299">In the Scale tab, you can add or subtract the number of units used by your BizTalk Service.</span></span> <span data-ttu-id="b8717-300">Alapértelmezés szerint van egy egység konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="b8717-300">By default, there is one Unit configured.</span></span> <span data-ttu-id="b8717-301">További egységek méretezése a BizTalk szolgáltatás lehet hozzáadni.</span><span class="sxs-lookup"><span data-stu-id="b8717-301">Additional Units can be added to scale your BizTalk Service.</span></span> <span data-ttu-id="b8717-302">Ha növeli a skála, amelyek növelve az adatátviteli sebességet.</span><span class="sxs-lookup"><span data-stu-id="b8717-302">When you increase the scale, you are increasing throughput.</span></span> <span data-ttu-id="b8717-303">Erőforrások mennyisége is növeli, beleértve a telepített hidak, a megállapodások, a LOB-kapcsolatok, és feldolgozási kapacitása révén.</span><span class="sxs-lookup"><span data-stu-id="b8717-303">The amount of resources also increases, including deployed bridges, agreements, LOB connections, and processing power.</span></span> <span data-ttu-id="b8717-304">Például növelheti a méret 1 egységből 2 egység.</span><span class="sxs-lookup"><span data-stu-id="b8717-304">For example, you increase the scale from 1 Unit to 2 Units.</span></span> <span data-ttu-id="b8717-305">Ebben a helyzetben kétszer annyi hidak telepíteni, a megállapodások duplán, duplán a LOB-kapcsolatok és a feldolgozási teljesítmény duplán.</span><span class="sxs-lookup"><span data-stu-id="b8717-305">In this situation, you can deploy double the number of bridges, double the agreements, double the LOB connections, and double the processing power.</span></span>

<span data-ttu-id="b8717-306">BizTalk kiadásaiban nem képes a skálázási beállítást.</span><span class="sxs-lookup"><span data-stu-id="b8717-306">Some BizTalk editions do not offer a scale option.</span></span> <span data-ttu-id="b8717-307">Ebben a helyzetben egy egység engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="b8717-307">In this situation, one Unit is permitted.</span></span> <span data-ttu-id="b8717-308">Annak meghatározásához, hány egység a edition méretezhetők, tekintse meg [BizTalk szolgáltatások: kiadások diagram](biztalk-editions-feature-chart.md).</span><span class="sxs-lookup"><span data-stu-id="b8717-308">To determine how many units your edition can be scaled, refer to [BizTalk Services: Editions Chart](biztalk-editions-feature-chart.md).</span></span>

<span data-ttu-id="b8717-309">Egységek számának növelésével hatással lehet a díjszabás.</span><span class="sxs-lookup"><span data-stu-id="b8717-309">Increasing the number of units may impact pricing.</span></span> <span data-ttu-id="b8717-310">Ha növeli az egységeket, kiválasztásával **mentése** egy üzenetben arra kéri, ha a számlázási kihatással van.</span><span class="sxs-lookup"><span data-stu-id="b8717-310">If you increase the Units, selecting **Save** displays a message that tells you if billing is impacted.</span></span> <span data-ttu-id="b8717-311">Ezután válasszon ki, a folytatáshoz.</span><span class="sxs-lookup"><span data-stu-id="b8717-311">You then choose to continue.</span></span> <span data-ttu-id="b8717-312">Ha növeli a BizTalk szolgáltatás állapota módosítások aktív Updating egységek száma.</span><span class="sxs-lookup"><span data-stu-id="b8717-312">When you increase the number of Units, the BizTalk Service status changes from Active to Updating.</span></span> <span data-ttu-id="b8717-313">A frissítés állapota a BizTalk szolgáltatás fut tovább.</span><span class="sxs-lookup"><span data-stu-id="b8717-313">In the Updating state, your BizTalk Service continues to run.</span></span>

<span data-ttu-id="b8717-314">[BizTalk szolgáltatások: Kiadások diagram](biztalk-editions-feature-chart.md) "egység".</span><span class="sxs-lookup"><span data-stu-id="b8717-314">[BizTalk Services: Editions Chart](biztalk-editions-feature-chart.md) defines a "Unit".</span></span>

## <a name="configure"></a><span data-ttu-id="b8717-315">Konfigurálás</span><span class="sxs-lookup"><span data-stu-id="b8717-315">Configure</span></span>
<span data-ttu-id="b8717-316">A hibrid kapcsolatok nem vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="b8717-316">Does not apply to Hybrid Connections.</span></span>

<span data-ttu-id="b8717-317">A biztonsági mentés állapotának beállítása None vagy automatikus.</span><span class="sxs-lookup"><span data-stu-id="b8717-317">Sets the Backup Status to None or Automatic.</span></span> <span data-ttu-id="b8717-318">Ha nincs beállítva, nem készült biztonsági másolat automatikusan létrejönnek.</span><span class="sxs-lookup"><span data-stu-id="b8717-318">When set to None, no backups are automatically created.</span></span> <span data-ttu-id="b8717-319">Ha a beállítás automatikus, konfigurálja a biztonsági mentési helyre, a biztonsági mentéshez, és mennyi ideig megőrzi a biztonsági mentési fájlokat gyakoriságát.</span><span class="sxs-lookup"><span data-stu-id="b8717-319">When set to Automatic, you configure the backup location, the frequency of the backup, and how long to keep the backup files.</span></span> 

<span data-ttu-id="b8717-320">[BizTalk szolgáltatások: Biztonsági mentése és visszaállítása](biztalk-backup-restore.md) részleteit.</span><span class="sxs-lookup"><span data-stu-id="b8717-320">[BizTalk Services: Backup and Restore](biztalk-backup-restore.md) provides the details.</span></span> 

## <span data-ttu-id="b8717-321"><a name="HybridConnections"></a>Hibrid kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="b8717-321"><a name="HybridConnections"></a>Hybrid Connections</span></span>
<span data-ttu-id="b8717-322">Hibrid kapcsolatok csatlakozzon az Azure-alkalmazások, például a Web Apps vagy az Azure App Service Mobile Apps egy helyszíni erőforrást, amely a statikus TCP-port, például az SQL Server, a MySQL, a HTTP webes API-k és a legtöbb egyéni webszolgáltatások használja.</span><span class="sxs-lookup"><span data-stu-id="b8717-322">Hybrid Connections connect an Azure application, like Web Apps or Mobile Apps in Azure App Service, to an on-premises resource that uses a static TCP port, such as SQL Server, MySQL, HTTP Web APIs, and most custom Web Services.</span></span> <span data-ttu-id="b8717-323">Hibrid kapcsolatok BizTalk szolgáltatások felügyelete a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="b8717-323">Hybrid Connections are managed in  BizTalk Services in the Azure classic portal.</span></span>

<span data-ttu-id="b8717-324">Hibrid kapcsolatok létrehozása az Azure App Service-ben: [hozzáférés a helyszíni erőforrások hibrid kapcsolatok használata az Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="b8717-324">To create Hybrid Connections in Azure App Service, see [Access on-premises resources using hybrid connections in Azure App Service](../app-service-web/web-sites-hybrid-connection-get-started.md).</span></span>

<span data-ttu-id="b8717-325">Hozzon létre, vagy hibrid kapcsolatok a Azure BizTalk szolgáltatások kezeléséhez, tekintse meg a [hibrid kapcsolatok](integration-hybrid-connection-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b8717-325">To create or manage Hybrid Connections in Azure BizTalk Services, see [Hybrid Connections](integration-hybrid-connection-overview.md).</span></span>

## <a name="next"></a><span data-ttu-id="b8717-326">Következő lépés</span><span class="sxs-lookup"><span data-stu-id="b8717-326">Next</span></span>
<span data-ttu-id="b8717-327">Most, hogy ismeri a különböző lappal, további Azure BizTalk szolgáltatások szolgáltatásait:</span><span class="sxs-lookup"><span data-stu-id="b8717-327">Now that you're familiar with the different tabs, you can learn more about the Azure BizTalk Services features:</span></span>

* [<span data-ttu-id="b8717-328">BizTalk Services: Szabályozás</span><span class="sxs-lookup"><span data-stu-id="b8717-328">BizTalk Services: Throttling</span></span>](biztalk-throttling-thresholds.md)  
* [<span data-ttu-id="b8717-329">BizTalk Services: Kiállító neve és kiállító kulcsa</span><span class="sxs-lookup"><span data-stu-id="b8717-329">BizTalk Services: Issuer Name and Issuer Key</span></span>](biztalk-issuer-name-issuer-key.md)  
* [<span data-ttu-id="b8717-330">BizTalk Services: Biztonsági mentés és visszaállítás</span><span class="sxs-lookup"><span data-stu-id="b8717-330">BizTalk Services: Backup and Restore</span></span>](biztalk-backup-restore.md)

## <a name="see-also"></a><span data-ttu-id="b8717-331">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="b8717-331">See Also</span></span>
* [<span data-ttu-id="b8717-332">Hibrid kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="b8717-332">Hybrid Connections</span></span>](integration-hybrid-connection-overview.md)  
* [<span data-ttu-id="b8717-333">BizTalk szolgáltatások: Fejlesztői, Basic, Standard és prémium kiadás diagram</span><span class="sxs-lookup"><span data-stu-id="b8717-333">BizTalk Services: Developer, Basic, Standard and Premium Editions Chart</span></span>](biztalk-editions-feature-chart.md)  
* [<span data-ttu-id="b8717-334">BizTalk szolgáltatások: Kiépítés használata Azure klasszikus portál</span><span class="sxs-lookup"><span data-stu-id="b8717-334">BizTalk Services: Provisioning Using Azure classic portal</span></span>](biztalk-provision-services.md)  
* [<span data-ttu-id="b8717-335">BizTalk szolgáltatások: BizTalk szolgáltatás állapotának diagram</span><span class="sxs-lookup"><span data-stu-id="b8717-335">BizTalk Services: BizTalk Service State Chart</span></span>](biztalk-service-state-chart.md)  
* [<span data-ttu-id="b8717-336">Hogyan kezdhetem el az Azure BizTalk Services SDK használatát</span><span class="sxs-lookup"><span data-stu-id="b8717-336">How do I Start Using the Azure BizTalk Services SDK</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=302335)

[Quickstart]: ./media/biztalk-dashboard-monitor-scale-tabs/QuickStartIcon.png
[AddMetrics]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_AddMetrics.png
[GrayedMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_GrayedMetric.png
[EnabledMetric]: ./media/biztalk-dashboard-monitor-scale-tabs/WABS_EnabledMetric.png

