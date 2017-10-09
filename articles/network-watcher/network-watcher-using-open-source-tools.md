---
title: "aaaVisualize hálózati forgalmának mintáit nyílt forráskódú eszközök és az Azure hálózati figyelőt |} Microsoft Docs"
description: "Ez a lap ismerteti, hogyan toouse hálózati figyelőt csomag rögzítése a Capanalysis toovisualize forgalmi minták tooand a virtuális gépek."
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
ms.openlocfilehash: fca9a226729162cd90d412c7b699ac54d2257a0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-network-traffic-patterns-tooand-from-your-vms-using-open-source-tools"></a><span data-ttu-id="c6195-103">Hálózati forgalmi minták tooand a virtuális gépek nyílt forráskódú eszközökkel megjelenítése</span><span class="sxs-lookup"><span data-stu-id="c6195-103">Visualize network traffic patterns tooand from your VMs using open source tools</span></span>

<span data-ttu-id="c6195-104">Csomag rögzíti, amelyek lehetővé teszik a tooperform hálózati vizsgálatokhoz és a mély Csomagvizsgálat hálózati-adatokat tartalmaznak.</span><span class="sxs-lookup"><span data-stu-id="c6195-104">Packet captures contain network data that allow you tooperform network forensics and deep packet inspection.</span></span> <span data-ttu-id="c6195-105">Nincsenek sok megnyílik a hálózattal kapcsolatos tooanalyze csomag rögzítésekre toogain insights használható forrás-eszközöket.</span><span class="sxs-lookup"><span data-stu-id="c6195-105">There are many opens source tools you can use tooanalyze packet captures toogain insights about your network.</span></span> <span data-ttu-id="c6195-106">Egy ilyen eszköz CapAnalysis, egy nyílt forráskódú csomag rögzítési képi megjelenítés eszköz.</span><span class="sxs-lookup"><span data-stu-id="c6195-106">One such tool is CapAnalysis, an open source packet capture visualization tool.</span></span> <span data-ttu-id="c6195-107">Csomagadatok rögzítési megjelenítése módja a értékes tooquickly célosztályából insights megoldások és rendellenességek észlelését, a hálózaton belül.</span><span class="sxs-lookup"><span data-stu-id="c6195-107">Visualizing packet capture data is a valuable way tooquickly derive insights on patterns and anomalies within your network.</span></span> <span data-ttu-id="c6195-108">Képi megjelenítések is biztosít egy ilyen insights megosztási egyszerűen felhasználhatóvá módon.</span><span class="sxs-lookup"><span data-stu-id="c6195-108">Visualizations also provide a means of sharing such insights in an easily consumable manner.</span></span>

<span data-ttu-id="c6195-109">Azure hálózati figyelőt biztosítja, hogy rögzíti az értékes adatok tooperform csomag lehetővé teszi toocapture hello a hálózaton.</span><span class="sxs-lookup"><span data-stu-id="c6195-109">Azure’s Network Watcher provides you hello ability toocapture this valuable data by allowing you tooperform packet captures on your network.</span></span> <span data-ttu-id="c6195-110">Ez a cikk azt ismertetik a hogyan toovisualize és nyereség információkat kaphat a csomag rögzíti a CapAnalysis használata a hálózati figyelőt.</span><span class="sxs-lookup"><span data-stu-id="c6195-110">In this article, we provide a walkthrough of how toovisualize and gain insights from packet captures using CapAnalysis with Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="c6195-111">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="c6195-111">Scenario</span></span>

<span data-ttu-id="c6195-112">Egy egyszerű webalkalmazást telepítve van a virtuális gép az Azure kívánt toouse nyílt forráskódú eszközök toovisualize a hálózati forgalom tooquickly azonosítása folyamata mintákat és minden lehetséges rendellenességek észlelését.</span><span class="sxs-lookup"><span data-stu-id="c6195-112">You have a simple web application deployed on a VM in Azure want toouse open source tools toovisualize its network traffic tooquickly identify flow patterns and any possible anomalies.</span></span> <span data-ttu-id="c6195-113">A hálózati figyelőt szerezzen be hálózati környezete csomag rögzítőeszközt, és közvetlenül a tárfiók tárolja.</span><span class="sxs-lookup"><span data-stu-id="c6195-113">With Network Watcher, you can obtain a packet capture of your network environment and directly store it on your storage account.</span></span> <span data-ttu-id="c6195-114">CapAnalysis majd hello csomagrögzítéssel közvetlenül hello tárolási blobból betöltési, és megjelenítheti annak tartalmát.</span><span class="sxs-lookup"><span data-stu-id="c6195-114">CapAnalysis can then ingest hello packet capture directly from hello storage blob and visualize its contents.</span></span>

![forgatókönyv][1]

## <a name="steps"></a><span data-ttu-id="c6195-116">Lépések</span><span class="sxs-lookup"><span data-stu-id="c6195-116">Steps</span></span>

### <a name="install-capanalysis"></a><span data-ttu-id="c6195-117">CapAnalysis telepítése</span><span class="sxs-lookup"><span data-stu-id="c6195-117">Install CapAnalysis</span></span>

<span data-ttu-id="c6195-118">tooinstall CapAnalysis virtuális gépen, olvassa el a toohello hivatalos utasításokat itt https://www.capanalysis.net/ca/how-to-install-capanalysis.</span><span class="sxs-lookup"><span data-stu-id="c6195-118">tooinstall CapAnalysis on a virtual machine, you can refer toohello official instructions here https://www.capanalysis.net/ca/how-to-install-capanalysis.</span></span>
<span data-ttu-id="c6195-119">Rendelés távolról fér hozzá CapAnalysis, igazolnia kell tooopen portjához 9877 a virtuális Gépet egy új bejövő szabály hozzáadásával.</span><span class="sxs-lookup"><span data-stu-id="c6195-119">In order access CapAnalysis remotely, we need tooopen port 9877 on your VM by adding a new inbound security rule.</span></span> <span data-ttu-id="c6195-120">A hálózati biztonsági szabályok létrehozásával kapcsolatban bővebben lásd túl[egy meglévő NSG-szabályok létrehozására](../virtual-network/virtual-networks-create-nsg-arm-pportal.md#create-rules-in-an-existing-nsg).</span><span class="sxs-lookup"><span data-stu-id="c6195-120">For more about creating rules in Network Security Groups, refer too[Create rules in an existing NSG](../virtual-network/virtual-networks-create-nsg-arm-pportal.md#create-rules-in-an-existing-nsg).</span></span> <span data-ttu-id="c6195-121">Miután hello szabály sikeresen hozzá lett adva, képes tooaccess CapAnalysis kell-e a`http://<PublicIP>:9877`</span><span class="sxs-lookup"><span data-stu-id="c6195-121">Once hello rule has been successfully added, you should be able tooaccess CapAnalysis from `http://<PublicIP>:9877`</span></span>

### <a name="use-azure-network-watcher-toostart-a-packet-capture-session"></a><span data-ttu-id="c6195-122">Használja az Azure hálózati figyelőt toostart csomagot munkamenet rögzítése</span><span class="sxs-lookup"><span data-stu-id="c6195-122">Use Azure Network Watcher toostart a packet capture session</span></span>

<span data-ttu-id="c6195-123">Hálózati figyelő lehetővé teszi toocapture csomagok tootrack forgalom mindkét virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="c6195-123">Network Watcher allows you toocapture packets tootrack traffic in and out of a virtual machine.</span></span> <span data-ttu-id="c6195-124">Olvassa el a toohello található utasítások segítségével: [kezelése csomagrögzítéseket hálózati figyelőt](network-watcher-packet-capture-manage-portal.md) toostart csomag rögzítési munkamenet.</span><span class="sxs-lookup"><span data-stu-id="c6195-124">You can refer toohello instructions at [Manage packet captures with Network Watcher](network-watcher-packet-capture-manage-portal.md) toostart a packet capture session.</span></span> <span data-ttu-id="c6195-125">A csomagrögzítéssel tárolhatja a tárolási blob toobe CapAnalysis érhetők el.</span><span class="sxs-lookup"><span data-stu-id="c6195-125">This packet capture can be stored in a storage blob toobe accessed by CapAnalysis.</span></span>

### <a name="upload-a-packet-capture-toocapanalysis"></a><span data-ttu-id="c6195-126">A csomag rögzítési tooCapAnalysis feltöltése</span><span class="sxs-lookup"><span data-stu-id="c6195-126">Upload a packet capture tooCapAnalysis</span></span>
<span data-ttu-id="c6195-127">A hálózati figyelőt hello "Importálása az URL" lapon és a tárolási blob hivatkozás toohello által végrehajtott hello csomagrögzítéssel tároló csomagrögzítéssel közvetlenül is feltölthet.</span><span class="sxs-lookup"><span data-stu-id="c6195-127">You can directly upload a packet capture taken by network watcher using hello “Import from URL” tab and providing a link toohello storage blob where hello packet capture is stored.</span></span>

<span data-ttu-id="c6195-128">Ha egy hivatkozás tooCapAnalysis biztosít, győződjön meg arról, hogy tooappend SAS-token toohello tárolási blob URL-címet.</span><span class="sxs-lookup"><span data-stu-id="c6195-128">When providing a link tooCapAnalysis, make sure tooappend a SAS token toohello storage blob URL.</span></span>  <span data-ttu-id="c6195-129">toodo, keresse meg a tooShared hozzáférésű jogosultságkódot hello tárfiókból, engedélyezett engedélyek hello kijelölni, majd nyomja meg hello SAS létrehozása gomb toocreate jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="c6195-129">toodo this, navigate tooShared access signature from hello storage account, designate hello allowed permissions, and press hello Generate SAS button toocreate a token.</span></span> <span data-ttu-id="c6195-130">Majd fűzze hozzá a SAS token toohello csomag rögzítési tárolási blob URL-CÍMÉT.</span><span class="sxs-lookup"><span data-stu-id="c6195-130">You can then append this SAS token toohello packet capture storage blob URL.</span></span>

<span data-ttu-id="c6195-131">hello eredményül kapott URL-cím fog megjelenni ehhez hasonló: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere</span><span class="sxs-lookup"><span data-stu-id="c6195-131">hello resulting URL will look something like this: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere</span></span>


### <a name="analyzing-packet-captures"></a><span data-ttu-id="c6195-132">Rögzíti a elemzésekor csomag</span><span class="sxs-lookup"><span data-stu-id="c6195-132">Analyzing packet captures</span></span>

<span data-ttu-id="c6195-133">CapAnalysis kínál különböző beállítások toovisualize a csomagrögzítéssel, minden olyan elemzés különböző szempontjából.</span><span class="sxs-lookup"><span data-stu-id="c6195-133">CapAnalysis offers various options toovisualize your packet capture, each providing analysis from a different perspective.</span></span> <span data-ttu-id="c6195-134">A vizuális összesítések a hálózati forgalom trendjeinek megértését, és gyorsan direkt szokatlan tevékenység.</span><span class="sxs-lookup"><span data-stu-id="c6195-134">With these visual summaries, you can understand your network traffic trends and quickly spot any unusual activity.</span></span> <span data-ttu-id="c6195-135">Ezek a szolgáltatások néhány hello a következő listában láthatók:</span><span class="sxs-lookup"><span data-stu-id="c6195-135">A few of these features are shown in hello following list:</span></span>

1. <span data-ttu-id="c6195-136">Attribútumfolyam táblák</span><span class="sxs-lookup"><span data-stu-id="c6195-136">Flow Tables</span></span>

    <span data-ttu-id="c6195-137">A táblázat által biztosított hello hello csomagadatok forgalmának listája, hello adatfolyamok társított időbélyeg hello és hello különböző protokollok társított hello folyamat, valamint a forrás és cél IP.</span><span class="sxs-lookup"><span data-stu-id="c6195-137">This table gives you hello list of flows in hello packet data, hello time stamp associated with hello flows and hello various protocols associated with hello flow, as well as source and destination IP.</span></span>

    ![capanalysis folyamat lap][5]

1. <span data-ttu-id="c6195-139">Protokoll – áttekintés</span><span class="sxs-lookup"><span data-stu-id="c6195-139">Protocol Overview</span></span>

    <span data-ttu-id="c6195-140">Ezen az ablaktáblán lehetővé teszi a tooquickly tekintse meg a hálózati forgalom hello terjesztési hello keresztül, különböző protokollok és földrajzi.</span><span class="sxs-lookup"><span data-stu-id="c6195-140">This pane allows you tooquickly see hello distribution of network traffic over hello various protocols and geographies.</span></span>

    ![capanalysis protokoll – áttekintés][6]

1. <span data-ttu-id="c6195-142">Statisztika</span><span class="sxs-lookup"><span data-stu-id="c6195-142">Statistics</span></span>

    <span data-ttu-id="c6195-143">Ezen az ablaktáblán lehetővé teszi, hogy tooview hálózati forgalom statisztika – bájt küldött és protokollt használja a forrás és cél IP-címek, a folyamatok egyes hello forrás és cél IP-címek, kapott különböző tranzakciók, és az adatfolyamok hello időtartama.</span><span class="sxs-lookup"><span data-stu-id="c6195-143">This pane allows you tooview network traffic statistics – bytes sent and received from source and destination IPs, flows for each of hello source and destination IPs, protocol used for various flows, and hello duration of flows.</span></span>

    ![capanalysis statisztikák][7]

1. <span data-ttu-id="c6195-145">geomap</span><span class="sxs-lookup"><span data-stu-id="c6195-145">Geomap</span></span>

    <span data-ttu-id="c6195-146">Ezen az ablaktáblán biztosít a nézet a hálózati forgalom az egyes országok forgalom mennyiségét toohello skálázás színeivel.</span><span class="sxs-lookup"><span data-stu-id="c6195-146">This pane provides you with a map view of your network traffic, with colors scaling toohello volume of traffic from each country.</span></span> <span data-ttu-id="c6195-147">Kiválaszthatja a kijelölt országok tooview további folyamata statisztikákról, például adatok küldése és fogadása szükséges a IP-címeket az adott ország hello részét.</span><span class="sxs-lookup"><span data-stu-id="c6195-147">You can select highlighted countries tooview additional flow statistics such as hello proportion of data sent and received from IPs in that country.</span></span>

    ![geomap][8]

1. <span data-ttu-id="c6195-149">Szűrők</span><span class="sxs-lookup"><span data-stu-id="c6195-149">Filters</span></span>

    <span data-ttu-id="c6195-150">CapAnalysis biztosít az adott csomagok gyors elemzés szűrők.</span><span class="sxs-lookup"><span data-stu-id="c6195-150">CapAnalysis provides a set of filters for quick analysis of specific packets.</span></span> <span data-ttu-id="c6195-151">Toofilter hello adatok például protokoll toogain adott áttekinthetik a forgalom részhalmaz szerint is választhat.</span><span class="sxs-lookup"><span data-stu-id="c6195-151">For example, you can choose toofilter hello data by protocol toogain specific insights on that subset of traffic.</span></span>

    ![szűrők][11]

    <span data-ttu-id="c6195-153">Látogasson el [https://www.capanalysis.net/ca/#about](https://www.capanalysis.net/ca/#about) összes CapAnalysis képességeivel kapcsolatos további toolearn.</span><span class="sxs-lookup"><span data-stu-id="c6195-153">Visit [https://www.capanalysis.net/ca/#about](https://www.capanalysis.net/ca/#about) toolearn more about all CapAnalysis' capabilities.</span></span>

## <a name="conclusion"></a><span data-ttu-id="c6195-154">Összegzés</span><span class="sxs-lookup"><span data-stu-id="c6195-154">Conclusion</span></span>

<span data-ttu-id="c6195-155">Hálózati figyelőt csomag rögzítése funkció lehetővé teszi a toocapture hello adatok szükséges tooperform hálózati Törvényszéki, és jobb megértése érdekében a hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="c6195-155">Network Watcher’s packet capture feature allows you toocapture hello data necessary tooperform network forensics and better understand your network traffic.</span></span> <span data-ttu-id="c6195-156">Ebben a forgatókönyvben azt lehet bemutatta, hogyan hálózati figyelőt csomagok készítését könnyen integrálható a nyílt forráskódú képi megjelenítés eszközök.</span><span class="sxs-lookup"><span data-stu-id="c6195-156">In this scenario, we showed how packet captures from Network Watcher can easily be integrated with open source visualization tools.</span></span> <span data-ttu-id="c6195-157">A nyílt forráskódú eszközök, például CapAnalysis toovisualize csomagok rögzíti, mély Csomagvizsgálat, és gyorsan azonosíthatja a trendeket belül a hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="c6195-157">By using open source tools such as CapAnalysis toovisualize packets captures, you can perform deep packet inspection and quickly identify trends within your network traffic.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c6195-158">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c6195-158">Next steps</span></span>

<span data-ttu-id="c6195-159">További információ az NSG-folyamat naplók toolearn látogasson el [naplózza az NSG-folyamat](network-watcher-nsg-flow-logging-overview.md)</span><span class="sxs-lookup"><span data-stu-id="c6195-159">toolearn more about NSG flow logs, visit [NSG Flow logs](network-watcher-nsg-flow-logging-overview.md)</span></span>

<span data-ttu-id="c6195-160">Ismerje meg, hogyan toovisualize az NSG-folyamat naplózza a Power BI ellátogatva [megjelenítése NSG forgalomáramlás naplók és a Power bi-ban](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="c6195-160">Learn how toovisualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>
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
