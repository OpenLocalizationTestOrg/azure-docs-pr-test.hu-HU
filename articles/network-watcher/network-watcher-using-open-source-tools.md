---
title: "Hálózati forgalmának mintáit nyílt forráskódú eszközök és az Azure hálózati figyelőt megjelenítése |} Microsoft Docs"
description: "Ez a lap ismerteti Capanalysis megjelenítéséhez a virtuális gépek érkező vagy oda irányuló forgalmat hálózati figyelőt csomagrögzítéssel használata."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 936d881b-49f9-4798-8e45-d7185ec9fe89
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: e27bb694d0cbcf1ff7c9d8ca4682a79c8b5c5cb1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="visualize-network-traffic-patterns-to-and-from-your-vms-using-open-source-tools"></a><span data-ttu-id="89a82-103">Hálózati forgalmának mintáit felé és felől a virtuális gépek nyílt forráskódú eszközökkel megjelenítése</span><span class="sxs-lookup"><span data-stu-id="89a82-103">Visualize network traffic patterns to and from your VMs using open source tools</span></span>

<span data-ttu-id="89a82-104">Csomag rögzítésekre hajthat végre a hálózati vizsgálatokhoz és a mély Csomagvizsgálat hálózati adatokat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="89a82-104">Packet captures contain network data that allow you to perform network forensics and deep packet inspection.</span></span> <span data-ttu-id="89a82-105">Nincsenek sok megnyílik csomag rögzíti a hálózattal kapcsolatos dcu elemzéséhez használható forrás-eszközöket.</span><span class="sxs-lookup"><span data-stu-id="89a82-105">There are many opens source tools you can use to analyze packet captures to gain insights about your network.</span></span> <span data-ttu-id="89a82-106">Egy ilyen eszköz CapAnalysis, egy nyílt forráskódú csomag rögzítési képi megjelenítés eszköz.</span><span class="sxs-lookup"><span data-stu-id="89a82-106">One such tool is CapAnalysis, an open source packet capture visualization tool.</span></span> <span data-ttu-id="89a82-107">Gyorsan kapcsolattípusokból megoldások és a hálózaton belüli rendellenességeket elemzések hasznos eszközök csomag rögzítési adatok megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="89a82-107">Visualizing packet capture data is a valuable way to quickly derive insights on patterns and anomalies within your network.</span></span> <span data-ttu-id="89a82-108">Képi megjelenítések is biztosít egy ilyen insights megosztási egyszerűen felhasználhatóvá módon.</span><span class="sxs-lookup"><span data-stu-id="89a82-108">Visualizations also provide a means of sharing such insights in an easily consumable manner.</span></span>

<span data-ttu-id="89a82-109">Azure hálózati figyelőt lehetővé teszi az értékes adatok rögzítéséhez azzal, hogy lehetővé teszi a végrehajtását csomag rögzíti a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="89a82-109">Azure’s Network Watcher provides you the ability to capture this valuable data by allowing you to perform packet captures on your network.</span></span> <span data-ttu-id="89a82-110">Ebben a cikkben nyújtunk, amelyekkel megjelenítheti és dcu-csomagból bemutatóért rögzíti CapAnalysis használata a hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="89a82-110">In this article, we provide a walkthrough of how to visualize and gain insights from packet captures using CapAnalysis with Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="89a82-111">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="89a82-111">Scenario</span></span>

<span data-ttu-id="89a82-112">Rendelkezik egy egyszerű webalkalmazást telepített egy Azure-ban jelenítheti meg a hálózati forgalmat, és gyorsan azonosíthatja a folyamat mintákat és bármely esetleges rendellenességeket nyílt forráskódú eszközök segítségével.</span><span class="sxs-lookup"><span data-stu-id="89a82-112">You have a simple web application deployed on a VM in Azure want to use open source tools to visualize its network traffic to quickly identify flow patterns and any possible anomalies.</span></span> <span data-ttu-id="89a82-113">A hálózati figyelőt szerezzen be hálózati környezete csomag rögzítőeszközt, és közvetlenül a tárfiók tárolja.</span><span class="sxs-lookup"><span data-stu-id="89a82-113">With Network Watcher, you can obtain a packet capture of your network environment and directly store it on your storage account.</span></span> <span data-ttu-id="89a82-114">CapAnalysis ezután a csomagrögzítéssel az közvetlenül a tárolási blobból betöltési és jelenítheti meg annak tartalmát.</span><span class="sxs-lookup"><span data-stu-id="89a82-114">CapAnalysis can then ingest the packet capture directly from the storage blob and visualize its contents.</span></span>

![forgatókönyv][1]

## <a name="steps"></a><span data-ttu-id="89a82-116">Lépések</span><span class="sxs-lookup"><span data-stu-id="89a82-116">Steps</span></span>

### <a name="install-capanalysis"></a><span data-ttu-id="89a82-117">CapAnalysis telepítése</span><span class="sxs-lookup"><span data-stu-id="89a82-117">Install CapAnalysis</span></span>

<span data-ttu-id="89a82-118">A virtuális gépen CapAnalysis telepítéséhez hivatkozhat a hivatalos utasításokat https://www.capanalysis.net/ca/how-to-install-capanalysis.</span><span class="sxs-lookup"><span data-stu-id="89a82-118">To install CapAnalysis on a virtual machine, you can refer to the official instructions here https://www.capanalysis.net/ca/how-to-install-capanalysis.</span></span>
<span data-ttu-id="89a82-119">Ahhoz, távolról fér hozzá CapAnalysis, igazolnia kell a nyissa meg a virtuális gép 9877 portjához új bejövő szabály hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="89a82-119">In order access CapAnalysis remotely, we need to open port 9877 on your VM by adding a new inbound security rule.</span></span> <span data-ttu-id="89a82-120">A hálózati biztonsági szabályok létrehozásával kapcsolatban bővebben lásd [egy meglévő NSG-szabályok létrehozására](../virtual-network/virtual-networks-create-nsg-arm-pportal.md#create-rules-in-an-existing-nsg).</span><span class="sxs-lookup"><span data-stu-id="89a82-120">For more about creating rules in Network Security Groups, refer to [Create rules in an existing NSG](../virtual-network/virtual-networks-create-nsg-arm-pportal.md#create-rules-in-an-existing-nsg).</span></span> <span data-ttu-id="89a82-121">A szabály sikeres hozzáadása után kell tudni elérni a CapAnalysis`http://<PublicIP>:9877`</span><span class="sxs-lookup"><span data-stu-id="89a82-121">Once the rule has been successfully added, you should be able to access CapAnalysis from `http://<PublicIP>:9877`</span></span>

### <a name="use-azure-network-watcher-to-start-a-packet-capture-session"></a><span data-ttu-id="89a82-122">Egy csomag rögzítési munkamenet elindításához használt Azure hálózati figyelőt</span><span class="sxs-lookup"><span data-stu-id="89a82-122">Use Azure Network Watcher to start a packet capture session</span></span>

<span data-ttu-id="89a82-123">Hálózati figyelő lehetővé teszi a csomagok és nyomon követheti a forgalom mindkét virtuális gép rögzítése.</span><span class="sxs-lookup"><span data-stu-id="89a82-123">Network Watcher allows you to capture packets to track traffic in and out of a virtual machine.</span></span> <span data-ttu-id="89a82-124">Olvassa el a következő utasításokat [kezelése csomagrögzítéseket hálózati figyelőt](network-watcher-packet-capture-manage-portal.md) egy csomag rögzítési munkamenet elindításához.</span><span class="sxs-lookup"><span data-stu-id="89a82-124">You can refer to the instructions at [Manage packet captures with Network Watcher](network-watcher-packet-capture-manage-portal.md) to start a packet capture session.</span></span> <span data-ttu-id="89a82-125">A csomagrögzítéssel CapAnalysis érhető el, a tárolási blob is tárolható.</span><span class="sxs-lookup"><span data-stu-id="89a82-125">This packet capture can be stored in a storage blob to be accessed by CapAnalysis.</span></span>

### <a name="upload-a-packet-capture-to-capanalysis"></a><span data-ttu-id="89a82-126">A csomagrögzítéssel CapAnalysis feltöltése</span><span class="sxs-lookup"><span data-stu-id="89a82-126">Upload a packet capture to CapAnalysis</span></span>
<span data-ttu-id="89a82-127">A csomagrögzítéssel meghoznia hálózati figyelőt "Importálása az URL" lapján, és a tárolási blob mutató hivatkozás biztosítása a csomagrögzítéssel tároló közvetlenül feltölthet.</span><span class="sxs-lookup"><span data-stu-id="89a82-127">You can directly upload a packet capture taken by network watcher using the “Import from URL” tab and providing a link to the storage blob where the packet capture is stored.</span></span>

<span data-ttu-id="89a82-128">Hivatkozás biztosítása CapAnalysis, ellenőrizze, SAS-jogkivonat hozzáfűzése a tárolási blob URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="89a82-128">When providing a link to CapAnalysis, make sure to append a SAS token to the storage blob URL.</span></span>  <span data-ttu-id="89a82-129">Ehhez a közös hozzáférésű jogosultságkód navigáljon a tárfiókból, jelölje ki az engedélyezett engedélyek és a SAS létre gombra kattintva hozzon létre egy tokent.</span><span class="sxs-lookup"><span data-stu-id="89a82-129">To do this, navigate to Shared access signature from the storage account, designate the allowed permissions, and press the Generate SAS button to create a token.</span></span> <span data-ttu-id="89a82-130">A SAS-jogkivonat majd fűzze hozzá a csomag rögzítési tárolási blob URL-cím.</span><span class="sxs-lookup"><span data-stu-id="89a82-130">You can then append this SAS token to the packet capture storage blob URL.</span></span>

<span data-ttu-id="89a82-131">Az eredményül kapott URL-cím fog megjelenni ehhez hasonló: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere</span><span class="sxs-lookup"><span data-stu-id="89a82-131">The resulting URL will look something like this: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere</span></span>


### <a name="analyzing-packet-captures"></a><span data-ttu-id="89a82-132">Rögzíti a elemzésekor csomag</span><span class="sxs-lookup"><span data-stu-id="89a82-132">Analyzing packet captures</span></span>

<span data-ttu-id="89a82-133">CapAnalysis jelenítheti meg a csomagrögzítéssel, minden olyan elemzés különböző szempontjából különböző lehetőséget kínál.</span><span class="sxs-lookup"><span data-stu-id="89a82-133">CapAnalysis offers various options to visualize your packet capture, each providing analysis from a different perspective.</span></span> <span data-ttu-id="89a82-134">A vizuális összesítések a hálózati forgalom trendjeinek megértését, és gyorsan direkt szokatlan tevékenység.</span><span class="sxs-lookup"><span data-stu-id="89a82-134">With these visual summaries, you can understand your network traffic trends and quickly spot any unusual activity.</span></span> <span data-ttu-id="89a82-135">Ezek a szolgáltatások néhány az alábbi listában láthatók:</span><span class="sxs-lookup"><span data-stu-id="89a82-135">A few of these features are shown in the following list:</span></span>

1. <span data-ttu-id="89a82-136">Attribútumfolyam táblák</span><span class="sxs-lookup"><span data-stu-id="89a82-136">Flow Tables</span></span>

    <span data-ttu-id="89a82-137">Ebben a táblázatban lehetővé teszi a csomagadatok, a tranzakciós társított időbélyegzőjét és a különböző protokollok, a folyamat, valamint a forrás és cél IP társított folyamatok listáját.</span><span class="sxs-lookup"><span data-stu-id="89a82-137">This table gives you the list of flows in the packet data, the time stamp associated with the flows and the various protocols associated with the flow, as well as source and destination IP.</span></span>

    ![capanalysis folyamat lap][5]

1. <span data-ttu-id="89a82-139">Protokoll – áttekintés</span><span class="sxs-lookup"><span data-stu-id="89a82-139">Protocol Overview</span></span>

    <span data-ttu-id="89a82-140">Ezen az ablaktáblán lehetővé teszi, hogy gyorsan megtekintheti a különböző protokollok és földrajzi keresztüli hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="89a82-140">This pane allows you to quickly see the distribution of network traffic over the various protocols and geographies.</span></span>

    ![capanalysis protokoll – áttekintés][6]

1. <span data-ttu-id="89a82-142">Statisztika</span><span class="sxs-lookup"><span data-stu-id="89a82-142">Statistics</span></span>

    <span data-ttu-id="89a82-143">Ezen az ablaktáblán lehetővé teszi a hálózati forgalom statisztikáit a következőképpen tekintheti – bájt küldött és protokollt használja a forrás és cél IP-címek, adatfolyamok mind a forrás és cél IP-címek, kapott különböző tranzakciók, és az adatfolyamok időtartama.</span><span class="sxs-lookup"><span data-stu-id="89a82-143">This pane allows you to view network traffic statistics – bytes sent and received from source and destination IPs, flows for each of the source and destination IPs, protocol used for various flows, and the duration of flows.</span></span>

    ![capanalysis statisztikák][7]

1. <span data-ttu-id="89a82-145">geomap</span><span class="sxs-lookup"><span data-stu-id="89a82-145">Geomap</span></span>

    <span data-ttu-id="89a82-146">Ezen az ablaktáblán biztosít a nézet a hálózati forgalom az egyes országok forgalom mennyiségét méretezhetők színeivel.</span><span class="sxs-lookup"><span data-stu-id="89a82-146">This pane provides you with a map view of your network traffic, with colors scaling to the volume of traffic from each country.</span></span> <span data-ttu-id="89a82-147">Kiválaszthatja a kijelölt országok további folyamata statisztikákról, például az adatok küldése és fogadása szükséges a IP-címeket az adott ország arányát megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="89a82-147">You can select highlighted countries to view additional flow statistics such as the proportion of data sent and received from IPs in that country.</span></span>

    ![geomap][8]

1. <span data-ttu-id="89a82-149">Szűrők</span><span class="sxs-lookup"><span data-stu-id="89a82-149">Filters</span></span>

    <span data-ttu-id="89a82-150">CapAnalysis biztosít az adott csomagok gyors elemzés szűrők.</span><span class="sxs-lookup"><span data-stu-id="89a82-150">CapAnalysis provides a set of filters for quick analysis of specific packets.</span></span> <span data-ttu-id="89a82-151">Például ha szeretné, a forgalom részhalmaz adott dcu protokoll által szűrje az adatokat.</span><span class="sxs-lookup"><span data-stu-id="89a82-151">For example, you can choose to filter the data by protocol to gain specific insights on that subset of traffic.</span></span>

    ![szűrők][11]

    <span data-ttu-id="89a82-153">Látogasson el [https://www.capanalysis.net/ca/#about](https://www.capanalysis.net/ca/#about) összes CapAnalysis képességeivel kapcsolatos további.</span><span class="sxs-lookup"><span data-stu-id="89a82-153">Visit [https://www.capanalysis.net/ca/#about](https://www.capanalysis.net/ca/#about) to learn more about all CapAnalysis' capabilities.</span></span>

## <a name="conclusion"></a><span data-ttu-id="89a82-154">Összegzés</span><span class="sxs-lookup"><span data-stu-id="89a82-154">Conclusion</span></span>

<span data-ttu-id="89a82-155">Hálózati figyelőt csomag rögzítése funkció lehetővé teszi a hálózati Törvényszéki ellátásához, és jobb megértése érdekében a hálózati forgalom szükséges adatok rögzítéséhez.</span><span class="sxs-lookup"><span data-stu-id="89a82-155">Network Watcher’s packet capture feature allows you to capture the data necessary to perform network forensics and better understand your network traffic.</span></span> <span data-ttu-id="89a82-156">Ebben a forgatókönyvben azt lehet bemutatta, hogyan hálózati figyelőt csomagok készítését könnyen integrálható a nyílt forráskódú képi megjelenítés eszközök.</span><span class="sxs-lookup"><span data-stu-id="89a82-156">In this scenario, we showed how packet captures from Network Watcher can easily be integrated with open source visualization tools.</span></span> <span data-ttu-id="89a82-157">Az adott nyílt forráskódú eszközökkel CapAnalysis például megjelenítheti a csomagok készítését, mély Csomagvizsgálat, és gyorsan azonosíthatja a trendeket belül a hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="89a82-157">By using open source tools such as CapAnalysis to visualize packets captures, you can perform deep packet inspection and quickly identify trends within your network traffic.</span></span>

## <a name="next-steps"></a><span data-ttu-id="89a82-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="89a82-158">Next steps</span></span>

<span data-ttu-id="89a82-159">NSG folyamata naplók kapcsolatos további információkért látogasson el a [naplózza az NSG-folyamat](network-watcher-nsg-flow-logging-overview.md)</span><span class="sxs-lookup"><span data-stu-id="89a82-159">To learn more about NSG flow logs, visit [NSG Flow logs](network-watcher-nsg-flow-logging-overview.md)</span></span>

<span data-ttu-id="89a82-160">Megtudhatja, hogyan jelenítheti meg az NSG folyamata naplók a Power BI ellátogatva [megjelenítése NSG forgalomáramlás naplók és a Power bi-ban](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="89a82-160">Learn how to visualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>
<!--Image references-->

[1]: ./media/network-watcher-using-open-source-tools/figure1.png
[2]: ./media/network-watcher-using-open-source-tools/figure2.png
[3]: ./media/network-watcher-using-open-source-tools/figure3.png
[4]: ./media/network-watcher-using-open-source-tools/figure4.png
[5]: ./media/network-watcher-using-open-source-tools/figure5.png
[6]: ./media/network-watcher-using-open-source-tools/figure6.png
[7]: ./media/network-watcher-using-open-source-tools/figure7.png
[8]: ./media/network-watcher-using-open-source-tools/figure8.png
[9]: ./media/network-watcher-using-open-source-tools/figure9.png
[10]: ./media/network-watcher-using-open-source-tools/figure10.png
[11]: ./media/network-watcher-using-open-source-tools/figure11.png
