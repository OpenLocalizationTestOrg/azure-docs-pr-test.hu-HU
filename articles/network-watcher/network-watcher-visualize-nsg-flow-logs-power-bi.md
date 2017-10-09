---
title: "a Power BI naplózza aaaVisualizing Azure hálózati biztonsági csoport folyamata |} Microsoft Docs"
description: "Ez a lap ismerteti, hogyan naplózza az toovisualize NSG folyamata a Power BI."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 1e4f95fa-f5f0-4e03-bc25-008fbfc4934c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: d9adcf256df8fed68c39be1a026ca64cc6b5c6d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="visualizing-network-security-group-flow-logs-with-power-bi"></a><span data-ttu-id="34367-103">Visualizing hálózati biztonsági csoport folyamata naplók és a Power bi-ban</span><span class="sxs-lookup"><span data-stu-id="34367-103">Visualizing Network Security Group flow logs with Power BI</span></span>

<span data-ttu-id="34367-104">Hálózati biztonsági csoport folyamata naplók tooview információkat bemenő és kimenő IP-forgalom hálózati biztonsági csoportok lehetővé teszik.</span><span class="sxs-lookup"><span data-stu-id="34367-104">Network Security Group flow logs allow you tooview information about ingress and egress IP traffic on Network Security Groups.</span></span> <span data-ttu-id="34367-105">A folyamat naplók megjelenítése kimenő és bejövő adatfolyamok / szabály alapján, hello NIC hello folyamata vonatkozik, hello folyamata (forrás vagy a cél IP, forrás vagy a cél Port Protocol), és ha hello forgalom lett engedélyezett vagy megtagadott 5 rekordos információt.</span><span class="sxs-lookup"><span data-stu-id="34367-105">These flow logs show outbound and inbound flows on a per rule basis, hello NIC hello flow applies to, 5-tuple information about hello flow (Source/Destination IP, Source/Destination Port, Protocol), and if hello traffic was allowed or denied.</span></span>

<span data-ttu-id="34367-106">Ez lehet a naplózási adatok hello naplófájlok manuálisan keresve folyamata bonyolult toogain betekintést.</span><span class="sxs-lookup"><span data-stu-id="34367-106">It can be difficult toogain insights into flow logging data by manually searching hello log files.</span></span> <span data-ttu-id="34367-107">Ebben a cikkben nyújtunk a megoldás toovisualize a legutóbbi folyamata naplózza, és a hálózati forgalom megismerése.</span><span class="sxs-lookup"><span data-stu-id="34367-107">In this article, we provide a solution toovisualize your most recent flow logs and learn about traffic on your network.</span></span>

## <a name="scenario"></a><span data-ttu-id="34367-108">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="34367-108">Scenario</span></span>

<span data-ttu-id="34367-109">A forgatókönyv a következő hello azt a Power BI asztali toohello tárfiók jelenleg konfigurált hello fogadó, az NSG Flow naplózási adatok csatlakozzon.</span><span class="sxs-lookup"><span data-stu-id="34367-109">In hello following scenario, we connect Power BI desktop toohello storage account we have configured as hello sink for our NSG Flow Logging data.</span></span> <span data-ttu-id="34367-110">Jelenleg a tárfiók tooour kapcsolódás után a Power BI letölti és elemez hello naplók tooprovide hello forgalmat hálózati biztonsági csoportok által naplózott vizuális ábrázolását.</span><span class="sxs-lookup"><span data-stu-id="34367-110">After we connect tooour storage account, Power BI downloads and parses hello logs tooprovide a visual representation of hello traffic that is logged by Network Security groups.</span></span>

<span data-ttu-id="34367-111">Ellenőrizheti a hello sablonban megadott hello látványelemek használatával:</span><span class="sxs-lookup"><span data-stu-id="34367-111">Using hello visuals supplied in hello template you can examine:</span></span>

* <span data-ttu-id="34367-112">Felső Talkers</span><span class="sxs-lookup"><span data-stu-id="34367-112">Top Talkers</span></span>
* <span data-ttu-id="34367-113">Adatsorozat Flow időadatok irányt vagy szabály döntése alapján</span><span class="sxs-lookup"><span data-stu-id="34367-113">Time Series Flow Data by direction and rule decision</span></span>
* <span data-ttu-id="34367-114">Hálózati illesztő MAC-cím alapján adatfolyamok</span><span class="sxs-lookup"><span data-stu-id="34367-114">Flows by Network Interface MAC address</span></span>
* <span data-ttu-id="34367-115">NSG-t és a szabály által adatfolyamok</span><span class="sxs-lookup"><span data-stu-id="34367-115">Flows by NSG and Rule</span></span>
* <span data-ttu-id="34367-116">Folyamatok által célport</span><span class="sxs-lookup"><span data-stu-id="34367-116">Flows by Destination Port</span></span>

<span data-ttu-id="34367-117">hello sablon nem szerkeszthető, ezért a tooadd új adatokat, a látványelemek, módosítsa vagy a lekérdezések toosuit szerkesztése az igényeinek.</span><span class="sxs-lookup"><span data-stu-id="34367-117">hello template provided is editable so you can modify it tooadd new data, visuals, or edit queries toosuit your needs.</span></span>

## <a name="setup"></a><span data-ttu-id="34367-118">Beállítás</span><span class="sxs-lookup"><span data-stu-id="34367-118">Setup</span></span>

<span data-ttu-id="34367-119">Mielőtt hozzákezd, rendelkeznie kell hálózati biztonsági csoport Flow naplózás engedélyezve van egy vagy több hálózati biztonsági csoportok a fiókjában.</span><span class="sxs-lookup"><span data-stu-id="34367-119">Before you begin, you must have Network Security Group Flow Logging enabled on one or many Network Security Groups in your account.</span></span> <span data-ttu-id="34367-120">Hálózati biztonsági engedélyezésével kapcsolatos flow a naplókat, tekintse meg a következő cikket toohello: [bemutatása tooflow naplózási a hálózati biztonsági csoportok](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="34367-120">For instructions on enabling Network Security flow logs, refer toohello following article: [Introduction tooflow logging for Network Security Groups](network-watcher-nsg-flow-logging-overview.md).</span></span>

<span data-ttu-id="34367-121">Hello Power BI Desktop ügyfél telepítve a számítógépre, és elég szabadítson fel lemezterületet a gép toodownload és a betöltés hello naplóadatokat, hogy létezik-e a tárfiókban lévő is rendelkeznie kell.</span><span class="sxs-lookup"><span data-stu-id="34367-121">You must also have hello Power BI Desktop client installed on your machine, and enough free space on your machine toodownload and load hello log data that exists in your storage account.</span></span>

![Visio diagram][1]

### <a name="steps"></a><span data-ttu-id="34367-123">Lépések</span><span class="sxs-lookup"><span data-stu-id="34367-123">Steps</span></span>

1. <span data-ttu-id="34367-124">A letöltés, és nyissa meg a Power BI Desktop alkalmazás hello Power BI-sablon a következő hello [hálózati figyelő Power bi folyamata naplózza sablon](https://aka.ms/networkwatcherpowerbiflowlogstemplate)</span><span class="sxs-lookup"><span data-stu-id="34367-124">Download and open hello following Power BI template in hello Power BI Desktop Application [Network Watcher PowerBI flow logs template](https://aka.ms/networkwatcherpowerbiflowlogstemplate)</span></span>
1. <span data-ttu-id="34367-125">Adja meg a szükséges hello lekérdezési paraméterekről</span><span class="sxs-lookup"><span data-stu-id="34367-125">Enter hello required Query parameters</span></span>
    1. <span data-ttu-id="34367-126">**StorageAccountName** – hello tárfiók tartalmazó hello NSG folyamata naplózza, hogy megadja annak a toohello nevére kívánja tooload, majd jelenítheti meg.</span><span class="sxs-lookup"><span data-stu-id="34367-126">**StorageAccountName** – Specifies toohello name of hello storage account containing hello NSG flow logs that you would like tooload and visualize.</span></span>
    1. <span data-ttu-id="34367-127">**NumberOfLogFiles** – naplófájlokat, hogy kívánja toodownload, majd a Power bi-ban megjelenítheti hello számát adja meg.</span><span class="sxs-lookup"><span data-stu-id="34367-127">**NumberOfLogFiles** – Specifies hello number of log files that you would like toodownload and visualize in Power BI.</span></span> <span data-ttu-id="34367-128">Például ha 50 meg van adva, hello 50 legfrissebb naplófájlokat.</span><span class="sxs-lookup"><span data-stu-id="34367-128">For example, if 50 is specified, hello 50 latest log files.</span></span> <span data-ttu-id="34367-129">FF 2 NSG-ket van engedélyezve, és toosend NSG folyamata naplók toothis fiók konfigurálva, majd hello naplók az elmúlt 25 óra lehet megtekinteni.</span><span class="sxs-lookup"><span data-stu-id="34367-129">Ff we have 2 NSGs enabled and configured toosend NSG flow logs toothis account, then hello past 25 hours of logs can be viewed.</span></span>

    ![a Power BI fő][2]

1. <span data-ttu-id="34367-131">Írja be a hozzáférési kulcsot a tárfiók hello.</span><span class="sxs-lookup"><span data-stu-id="34367-131">Enter hello Access Key for your storage account.</span></span> <span data-ttu-id="34367-132">Érvényes elérési kulcsok tooyour storage-fiókot az Azure portál és kiválasztásával hello útvonalon található **hívóbetűk** hello-beállítások menüjében.</span><span class="sxs-lookup"><span data-stu-id="34367-132">You can find valid access keys by navigating tooyour storage account in hello Azure portal and selecting **Access Keys** from hello Settings menu.</span></span> <span data-ttu-id="34367-133">Kattintson a **Connect** majd a módosítások alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="34367-133">Click **Connect** then apply changes.</span></span>

    ![Tárelérési kulcsok][3]

    ![hozzáférési kulcs 2][4]

4.  <span data-ttu-id="34367-136">A naplók letöltéséhez és értelmezni, és most már használhatja hello előre létrehozott látványelemek.</span><span class="sxs-lookup"><span data-stu-id="34367-136">Your logs are download and parsed and you can now utilize hello pre-created visuals.</span></span>

## <a name="understanding-hello-visuals"></a><span data-ttu-id="34367-137">Hello látványelemek ismertetése</span><span class="sxs-lookup"><span data-stu-id="34367-137">Understanding hello visuals</span></span>

<span data-ttu-id="34367-138">Feltéve, hogy a hello sablon olyan készlete, amelyek segítenek látványelemek ellenőrizze értelmében hello NSG Flow naplóadatokat.</span><span class="sxs-lookup"><span data-stu-id="34367-138">Provided in hello template are a set of visuals that help make sense of hello NSG Flow Log data.</span></span> <span data-ttu-id="34367-139">hello következő ábrákon láthatók milyen hello irányítópult néz amikor feltöltve adatokkal mintáját.</span><span class="sxs-lookup"><span data-stu-id="34367-139">hello following images show a sample of what hello dashboard looks like when populated with data.</span></span> <span data-ttu-id="34367-140">Az alábbiakban azt vizsgálja meg az egyes visual nagyobb részletességgel</span><span class="sxs-lookup"><span data-stu-id="34367-140">Below we examine each visual in greater detail</span></span> 

![Power BI][5]
 
<span data-ttu-id="34367-142">hello felső Talkers visual látható hello IP-címek, amelyek kezdeményezett többsége hello hello megadott időszak alatt.</span><span class="sxs-lookup"><span data-stu-id="34367-142">hello Top Talkers visual shows hello IPs that have initiated hello most connections over hello period specified.</span></span> <span data-ttu-id="34367-143">hello hello listamezők összhangban toohello relatív kapcsolatok száma.</span><span class="sxs-lookup"><span data-stu-id="34367-143">hello size of hello boxes corresponds toohello relative number of connections.</span></span> 

![toptalkers][6]

<span data-ttu-id="34367-145">hello következő idő adatsorozat diagramjait megjelenítése hello időszakban adatfolyamok hello száma.</span><span class="sxs-lookup"><span data-stu-id="34367-145">hello following time series graphs show hello number of flows over hello period.</span></span> <span data-ttu-id="34367-146">hello felső grafikon által hello folyamat iránya van szegmentált, valamint alacsonyabb hello van szegmentált hello döntési (engedélyezése vagy megtagadása) által.</span><span class="sxs-lookup"><span data-stu-id="34367-146">hello upper graph is segmented by hello flow direction, and hello lower is segmented by hello decision made (allow or deny).</span></span> <span data-ttu-id="34367-147">A Megjelenítés választhat vizsgálja meg a forgalom trendek az idő múlásával és bármely rendellenes igényeiben jelentkező direkt vagy elutasítja a forgalom vagy forgalom szegmentálását.</span><span class="sxs-lookup"><span data-stu-id="34367-147">With this visual, you can examine your traffic trends over time, and spot any abnormal spikes or decline in traffic or traffic segmentation.</span></span>

![flowsoverperiod][7]

<span data-ttu-id="34367-149">hello következő diagramjait megjelenítése hello adatfolyamok hálózati adapterenként, hello felső folyamat iránya által szegmentált és hello alacsonyabb által végrehajtott döntési szegmentált.</span><span class="sxs-lookup"><span data-stu-id="34367-149">hello following graphs show hello flows per Network interface, with hello upper segmented by flow direction and hello lower segmented by decision made.</span></span> <span data-ttu-id="34367-150">Az információ, amely a virtuális gépek közölt hello legtöbb relatív tooothers, és ha a forgalom tooa speciális virtuális Gépet folyamatban van férhet engedélyez vagy tilt.</span><span class="sxs-lookup"><span data-stu-id="34367-150">With this information, you can gain insights into which of your VMs communicated hello most relative tooothers, and if traffic tooa specific VM is being allowed or denied.</span></span>

![flowspernic][8]

<span data-ttu-id="34367-152">a következő fánk kerék diagram hello által célport adatfolyamok részletes információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="34367-152">hello following donut wheel chart shows a breakdown of Flows by Destination Port.</span></span> <span data-ttu-id="34367-153">Az információ, megtekintheti a leggyakrabban használt hello célport megadott hello belül használt időszak.</span><span class="sxs-lookup"><span data-stu-id="34367-153">With this information, you can view hello most commonly used destination ports used within hello specified period.</span></span>

![Fánk][9]

<span data-ttu-id="34367-155">hello következő sávdiagram látható hello folyamata NSG-t és a szabály által.</span><span class="sxs-lookup"><span data-stu-id="34367-155">hello following bar chart shows hello Flow by NSG and Rule.</span></span> <span data-ttu-id="34367-156">Az információ megtekintheti hello NSG-ket hello felelős legtöbb forgalom, valamint a forgalom felosztása hello egy NSG-szabály által.</span><span class="sxs-lookup"><span data-stu-id="34367-156">With this information, you can see hello NSGs responsible for hello most traffic, and hello breakdown of traffic on an NSG by rule.</span></span>

![barchart][10]
 
<span data-ttu-id="34367-158">hello következő tájékoztató diagramok megjelenített információkat hello NSG-ket hello naplók szerepelnek, hello adatfolyamok rögzített hello időszak, és hello dátum hello legkorábbi napló rögzített száma.</span><span class="sxs-lookup"><span data-stu-id="34367-158">hello following informational charts display information about hello NSGs present in hello logs, hello number of Flows captured over hello period, and hello date of hello earliest log captured.</span></span> <span data-ttu-id="34367-159">Ezt az információt biztosít egy meghatározni, hogy milyen NSG-ket naplózásának és hello flow dátumtartományt.</span><span class="sxs-lookup"><span data-stu-id="34367-159">This information gives you an idea of what NSGs are being logged and hello date range of flows.</span></span>

![infochart1][11]

![infochart2][12]

<span data-ttu-id="34367-162">Ez a sablon tartalmazza hello következő szeletelők tooallow meg tooview csak hello adatok tervezheti meg.</span><span class="sxs-lookup"><span data-stu-id="34367-162">This template includes hello following slicers tooallow you tooview only hello data you are most interested in.</span></span> <span data-ttu-id="34367-163">Az erőforráscsoportok, NSG-ket és szabályok alapján szűrhet.</span><span class="sxs-lookup"><span data-stu-id="34367-163">You can filter on your resource groups, NSGs, and rules.</span></span> <span data-ttu-id="34367-164">5 rekordos információt, döntési és hello napló írása történt hello idő is végezhet.</span><span class="sxs-lookup"><span data-stu-id="34367-164">You can also filter on 5-tuple information, decision, and hello time hello log was written.</span></span>

![a szeletelők][13]

## <a name="conclusion"></a><span data-ttu-id="34367-166">Összegzés</span><span class="sxs-lookup"><span data-stu-id="34367-166">Conclusion</span></span>

<span data-ttu-id="34367-167">Azt mutatott ebben a forgatókönyvben, hogy hálózati figyelőt, és a Power BI által biztosított hálózati biztonsági csoport Flow naplók segítségével azt tudja toovisualize és hello forgalom ismertetése.</span><span class="sxs-lookup"><span data-stu-id="34367-167">We showed in this scenario that by using Network Security Group Flow logs provided by Network Watcher and Power BI, we are able toovisualize and understand hello traffic.</span></span> <span data-ttu-id="34367-168">Hello megadott sablont használ, a Power BI hello naplók letölti közvetlenül a tárolási, és helyileg feldolgozza azokat.</span><span class="sxs-lookup"><span data-stu-id="34367-168">Using hello provided template, Power BI downloads hello logs directly from storage and processes them locally.</span></span> <span data-ttu-id="34367-169">Igénybe vett idő tooload hello sablon kért fájlok hello száma attól függ, és a fájlok összesített mérete le.</span><span class="sxs-lookup"><span data-stu-id="34367-169">Time taken tooload hello template varies depending on hello number of files requested and total size of files downloaded.</span></span>

<span data-ttu-id="34367-170">Érzi, hogy szabad toocustomize Ez a sablon az igényeinek.</span><span class="sxs-lookup"><span data-stu-id="34367-170">Feel free toocustomize this template for your needs.</span></span> <span data-ttu-id="34367-171">Számos módon számos használható a Power BI és hálózati biztonsági csoport Flow naplófájlokat.</span><span class="sxs-lookup"><span data-stu-id="34367-171">There are many numerous ways that you can use Power BI with Network Security Group Flow Logs.</span></span> 

## <a name="notes"></a><span data-ttu-id="34367-172">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="34367-172">Notes</span></span>

* <span data-ttu-id="34367-173">Alapértelmezés szerint naplója`https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`</span><span class="sxs-lookup"><span data-stu-id="34367-173">Logs by default are stored in `https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`</span></span>

    * <span data-ttu-id="34367-174">Ha más adatok már létezik egy másik címtárban azok a lekérdezések toopull hello és hello adatfeldolgozásra módosítani kell.</span><span class="sxs-lookup"><span data-stu-id="34367-174">If other data exists in another directory they hello queries toopull and process hello data must be modified.</span></span>

* <span data-ttu-id="34367-175">a megadott hello sablon nem ajánlott 1 GB-nál több naplók való használatra.</span><span class="sxs-lookup"><span data-stu-id="34367-175">hello provided template is not recommended for use with more than 1 GB of logs.</span></span>

* <span data-ttu-id="34367-176">Ha nagy mennyiségű naplókat, azt javasoljuk, hogy a használatával, például a Data Lake vagy SQL server egy másik adattároló megoldás vizsgálata.</span><span class="sxs-lookup"><span data-stu-id="34367-176">If you have a large amount of logs, we recommend that you investigate a solution using another data store like Data Lake or SQL server.</span></span>

## <a name="next-steps"></a><span data-ttu-id="34367-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="34367-177">Next Steps</span></span>

<span data-ttu-id="34367-178">Ismerje meg, hogyan toovisualize az NSG-folyamat naplózza a hello Elastick verem ellátogatva [hálózati forgalom minták tooand a virtuális gépek nyílt forráskódú eszközökkel megjelenítése](network-watcher-using-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="34367-178">Learn how toovisualize your NSG flow logs with hello Elastick Stack by visiting [Visualize network traffic patterns tooand from your VMs using open source tools](network-watcher-using-open-source-tools.md)</span></span>

[1]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure7.png
[8]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure8.png
[9]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure9.png
[10]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure10.png
[11]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure11.png
[12]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure12.png
[13]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure13.png
