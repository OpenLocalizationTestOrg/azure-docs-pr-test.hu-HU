---
title: "A Power BI visualizing Azure hálózati biztonsági csoport folyamata naplók |} Microsoft Docs"
description: "Ezen a lapon jelenítheti meg NSG folyamata naplók és a Power BI ismerteti."
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
ms.openlocfilehash: d8c61ca2a3bd5affe02e8f9500655db6699a245f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="visualizing-network-security-group-flow-logs-with-power-bi"></a><span data-ttu-id="9ab16-103">Visualizing hálózati biztonsági csoport folyamata naplók és a Power bi-ban</span><span class="sxs-lookup"><span data-stu-id="9ab16-103">Visualizing Network Security Group flow logs with Power BI</span></span>

<span data-ttu-id="9ab16-104">Hálózati biztonsági csoport folyamata naplók lehetővé teszik a bemenő és kimenő IP-forgalom vonatkozó információk megtekintése a hálózati biztonsági csoportok.</span><span class="sxs-lookup"><span data-stu-id="9ab16-104">Network Security Group flow logs allow you to view information about ingress and egress IP traffic on Network Security Groups.</span></span> <span data-ttu-id="9ab16-105">Ezek egymást követő naplók megjelenítése vonatkozik a kimenő és bejövő forgalom alapon egy szabályt, a hálózati adapter a folyamatot, 5 rekordos információ a folyamattal (forrás vagy a cél IP, forrás vagy a cél Port Protocol), és ha a forgalom lett engedélyez vagy tilt.</span><span class="sxs-lookup"><span data-stu-id="9ab16-105">These flow logs show outbound and inbound flows on a per rule basis, the NIC the flow applies to, 5-tuple information about the flow (Source/Destination IP, Source/Destination Port, Protocol), and if the traffic was allowed or denied.</span></span>

<span data-ttu-id="9ab16-106">A naplófájlok manuálisan keresve dcu adatbázisába folyamata naplózási adatok nehézkes lehet.</span><span class="sxs-lookup"><span data-stu-id="9ab16-106">It can be difficult to gain insights into flow logging data by manually searching the log files.</span></span> <span data-ttu-id="9ab16-107">Ez a cikk azt jelenítheti meg az utolsó folyamat-naplók és a hálózati forgalom megismerése megoldást nyújt.</span><span class="sxs-lookup"><span data-stu-id="9ab16-107">In this article, we provide a solution to visualize your most recent flow logs and learn about traffic on your network.</span></span>

## <a name="scenario"></a><span data-ttu-id="9ab16-108">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="9ab16-108">Scenario</span></span>

<span data-ttu-id="9ab16-109">Az alábbi példa nem csatlakozni a Power BI desktop a tárfiók jelenleg konfigurált, a gyűjtő az NSG Flow naplózási adatok.</span><span class="sxs-lookup"><span data-stu-id="9ab16-109">In the following scenario, we connect Power BI desktop to the storage account we have configured as the sink for our NSG Flow Logging data.</span></span> <span data-ttu-id="9ab16-110">Azt a kapcsolódás után a tárfiókhoz a Power BI tölti le, és a elemzi a forgalmat hálózati biztonsági csoportok által naplózott vizuális ábrázolását adja meg a naplókat.</span><span class="sxs-lookup"><span data-stu-id="9ab16-110">After we connect to our storage account, Power BI downloads and parses the logs to provide a visual representation of the traffic that is logged by Network Security groups.</span></span>

<span data-ttu-id="9ab16-111">A látványelemek, tekintse meg a sablonban megadott használatával:</span><span class="sxs-lookup"><span data-stu-id="9ab16-111">Using the visuals supplied in the template you can examine:</span></span>

* <span data-ttu-id="9ab16-112">Felső Talkers</span><span class="sxs-lookup"><span data-stu-id="9ab16-112">Top Talkers</span></span>
* <span data-ttu-id="9ab16-113">Adatsorozat Flow időadatok irányt vagy szabály döntése alapján</span><span class="sxs-lookup"><span data-stu-id="9ab16-113">Time Series Flow Data by direction and rule decision</span></span>
* <span data-ttu-id="9ab16-114">Hálózati illesztő MAC-cím alapján adatfolyamok</span><span class="sxs-lookup"><span data-stu-id="9ab16-114">Flows by Network Interface MAC address</span></span>
* <span data-ttu-id="9ab16-115">NSG-t és a szabály által adatfolyamok</span><span class="sxs-lookup"><span data-stu-id="9ab16-115">Flows by NSG and Rule</span></span>
* <span data-ttu-id="9ab16-116">Folyamatok által célport</span><span class="sxs-lookup"><span data-stu-id="9ab16-116">Flows by Destination Port</span></span>

<span data-ttu-id="9ab16-117">A megadott sablon szerkeszthető, így módosíthatja úgy, hogy az új adatok, a látványelemek, vagy szerkessze a lekérdezéseket az igényeinek.</span><span class="sxs-lookup"><span data-stu-id="9ab16-117">The template provided is editable so you can modify it to add new data, visuals, or edit queries to suit your needs.</span></span>

## <a name="setup"></a><span data-ttu-id="9ab16-118">Beállítás</span><span class="sxs-lookup"><span data-stu-id="9ab16-118">Setup</span></span>

<span data-ttu-id="9ab16-119">Mielőtt hozzákezd, rendelkeznie kell hálózati biztonsági csoport Flow naplózás engedélyezve van egy vagy több hálózati biztonsági csoportok a fiókjában.</span><span class="sxs-lookup"><span data-stu-id="9ab16-119">Before you begin, you must have Network Security Group Flow Logging enabled on one or many Network Security Groups in your account.</span></span> <span data-ttu-id="9ab16-120">Hálózati biztonsági engedélyezésével kapcsolatos flow a naplókat, a következő cikk tartalmazza: [folyamata naplózási a hálózati biztonsági csoportok bemutatása](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="9ab16-120">For instructions on enabling Network Security flow logs, refer to the following article: [Introduction to flow logging for Network Security Groups](network-watcher-nsg-flow-logging-overview.md).</span></span>

<span data-ttu-id="9ab16-121">A Power BI Desktop-ügyféllel a gépen, és elég szabad hely a letöltése és betölteni a napló adatait, hogy létezik-e a tárfiókban lévő számítógépre is rendelkeznie kell.</span><span class="sxs-lookup"><span data-stu-id="9ab16-121">You must also have the Power BI Desktop client installed on your machine, and enough free space on your machine to download and load the log data that exists in your storage account.</span></span>

![Visio diagram][1]

### <a name="steps"></a><span data-ttu-id="9ab16-123">Lépések</span><span class="sxs-lookup"><span data-stu-id="9ab16-123">Steps</span></span>

1. <span data-ttu-id="9ab16-124">Töltse le és nyissa meg a következő Power BI-sablont a Power BI Desktop alkalmazás [hálózati figyelő Power bi folyamata naplózza sablon](https://aka.ms/networkwatcherpowerbiflowlogstemplate)</span><span class="sxs-lookup"><span data-stu-id="9ab16-124">Download and open the following Power BI template in the Power BI Desktop Application [Network Watcher PowerBI flow logs template](https://aka.ms/networkwatcherpowerbiflowlogstemplate)</span></span>
1. <span data-ttu-id="9ab16-125">Adja meg a szükséges lekérdezés-paraméterek</span><span class="sxs-lookup"><span data-stu-id="9ab16-125">Enter the required Query parameters</span></span>
    1. <span data-ttu-id="9ab16-126">**StorageAccountName** – Megadja azt az NSG-t szeretné betölteni, és megjelenítheti folyamata naplók tartalmazó storage-fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="9ab16-126">**StorageAccountName** – Specifies to the name of the storage account containing the NSG flow logs that you would like to load and visualize.</span></span>
    1. <span data-ttu-id="9ab16-127">**NumberOfLogFiles** – Itt adhatja meg, amelyeket szeretne letölteni, és megjelenítheti a Power BI fájlok száma.</span><span class="sxs-lookup"><span data-stu-id="9ab16-127">**NumberOfLogFiles** – Specifies the number of log files that you would like to download and visualize in Power BI.</span></span> <span data-ttu-id="9ab16-128">Ha 50 van megadva, például az 50 legfrissebb naplófájlokat.</span><span class="sxs-lookup"><span data-stu-id="9ab16-128">For example, if 50 is specified, the 50 latest log files.</span></span> <span data-ttu-id="9ab16-129">FF tudunk 2 NSG-k engedélyezve és konfigurálva van az NSG folyamata naplókat küld ennek a fióknak, majd a naplók az elmúlt 25 óra lehet megtekinteni.</span><span class="sxs-lookup"><span data-stu-id="9ab16-129">Ff we have 2 NSGs enabled and configured to send NSG flow logs to this account, then the past 25 hours of logs can be viewed.</span></span>

    ![a Power BI fő][2]

1. <span data-ttu-id="9ab16-131">Adja meg a tárfiók elérési kulcsának.</span><span class="sxs-lookup"><span data-stu-id="9ab16-131">Enter the Access Key for your storage account.</span></span> <span data-ttu-id="9ab16-132">Érvényes elérési kulcsok nyissa meg a storage-fiókot az Azure portál és választ talál **hívóbetűk** a beállítások menüből.</span><span class="sxs-lookup"><span data-stu-id="9ab16-132">You can find valid access keys by navigating to your storage account in the Azure portal and selecting **Access Keys** from the Settings menu.</span></span> <span data-ttu-id="9ab16-133">Kattintson a **Connect** majd a módosítások alkalmazásához.</span><span class="sxs-lookup"><span data-stu-id="9ab16-133">Click **Connect** then apply changes.</span></span>

    ![Tárelérési kulcsok][3]

    ![hozzáférési kulcs 2][4]

4.  <span data-ttu-id="9ab16-136">A naplók letöltéséhez és értelmezni, és most már használhatja a korábban létrehozott látványelemek.</span><span class="sxs-lookup"><span data-stu-id="9ab16-136">Your logs are download and parsed and you can now utilize the pre-created visuals.</span></span>

## <a name="understanding-the-visuals"></a><span data-ttu-id="9ab16-137">A látványelemek ismertetése</span><span class="sxs-lookup"><span data-stu-id="9ab16-137">Understanding the visuals</span></span>

<span data-ttu-id="9ab16-138">A sablonban megadott látványelemek, amelyek segítenek az NSG Flow naplóadatokat célszerű olyan.</span><span class="sxs-lookup"><span data-stu-id="9ab16-138">Provided in the template are a set of visuals that help make sense of the NSG Flow Log data.</span></span> <span data-ttu-id="9ab16-139">A következő ábrákon az irányítópult néz amikor feltöltve adatokkal mintát láthatók.</span><span class="sxs-lookup"><span data-stu-id="9ab16-139">The following images show a sample of what the dashboard looks like when populated with data.</span></span> <span data-ttu-id="9ab16-140">Az alábbiakban azt vizsgálja meg az egyes visual nagyobb részletességgel</span><span class="sxs-lookup"><span data-stu-id="9ab16-140">Below we examine each visual in greater detail</span></span> 

![Power BI][5]
 
<span data-ttu-id="9ab16-142">A felső Talkers visual azt mutatja be az IP-címek, amelyek az adott időszakban a legtöbb kapcsolatok kezdeményezett megadott.</span><span class="sxs-lookup"><span data-stu-id="9ab16-142">The Top Talkers visual shows the IPs that have initiated the most connections over the period specified.</span></span> <span data-ttu-id="9ab16-143">A listák összhangban kapcsolatok relatív száma.</span><span class="sxs-lookup"><span data-stu-id="9ab16-143">The size of the boxes corresponds to the relative number of connections.</span></span> 

![toptalkers][6]

<span data-ttu-id="9ab16-145">A következő alkalommal adatsorozat diagramokon megjelenítése az adott időszakban adatfolyamok száma.</span><span class="sxs-lookup"><span data-stu-id="9ab16-145">The following time series graphs show the number of flows over the period.</span></span> <span data-ttu-id="9ab16-146">A felső grafikon által a folyamat iránya van szegmentált, és az alacsonyabb van szegmentált által végrehajtott (engedélyezése vagy megtagadása) alkalmazással.</span><span class="sxs-lookup"><span data-stu-id="9ab16-146">The upper graph is segmented by the flow direction, and the lower is segmented by the decision made (allow or deny).</span></span> <span data-ttu-id="9ab16-147">A Megjelenítés választhat vizsgálja meg a forgalom trendek az idő múlásával és bármely rendellenes igényeiben jelentkező direkt vagy elutasítja a forgalom vagy forgalom szegmentálását.</span><span class="sxs-lookup"><span data-stu-id="9ab16-147">With this visual, you can examine your traffic trends over time, and spot any abnormal spikes or decline in traffic or traffic segmentation.</span></span>

![flowsoverperiod][7]

<span data-ttu-id="9ab16-149">Hálózati adapterenként, a folyamat iránya és az alacsonyabb szegmentált felső folyamatok által végzett döntési szegmentált következő diagramjait megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="9ab16-149">The following graphs show the flows per Network interface, with the upper segmented by flow direction and the lower segmented by decision made.</span></span> <span data-ttu-id="9ab16-150">Az információ, amely a virtuális gépek ezekről a legtöbb más viszonyítva, és ha folyamatban van-e egy adott virtuális gép férhet engedélyez vagy tilt.</span><span class="sxs-lookup"><span data-stu-id="9ab16-150">With this information, you can gain insights into which of your VMs communicated the most relative to others, and if traffic to a specific VM is being allowed or denied.</span></span>

![flowspernic][8]

<span data-ttu-id="9ab16-152">A következő fánk kerék diagram által célport adatfolyamok részletes információkat jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="9ab16-152">The following donut wheel chart shows a breakdown of Flows by Destination Port.</span></span> <span data-ttu-id="9ab16-153">Az információ megtekintheti a leggyakrabban használt célport használja a megadott időn belül.</span><span class="sxs-lookup"><span data-stu-id="9ab16-153">With this information, you can view the most commonly used destination ports used within the specified period.</span></span>

![Fánk][9]

<span data-ttu-id="9ab16-155">A következő sávdiagram szemlélteti az NSG-t és a szabály által.</span><span class="sxs-lookup"><span data-stu-id="9ab16-155">The following bar chart shows the Flow by NSG and Rule.</span></span> <span data-ttu-id="9ab16-156">Az információ megtekintheti az NSG-ket a legtöbb forgalmat, és a forgalom felosztása felelős egy NSG-szabály által.</span><span class="sxs-lookup"><span data-stu-id="9ab16-156">With this information, you can see the NSGs responsible for the most traffic, and the breakdown of traffic on an NSG by rule.</span></span>

![barchart][10]
 
<span data-ttu-id="9ab16-158">A következő tájékoztató diagramokon szerepel a naplókat, az időszak, és a legkorábbi naplóban rögzített dátumának rögzített adatfolyamok száma NSG-ket információkat jelenítenek meg.</span><span class="sxs-lookup"><span data-stu-id="9ab16-158">The following informational charts display information about the NSGs present in the logs, the number of Flows captured over the period, and the date of the earliest log captured.</span></span> <span data-ttu-id="9ab16-159">Ezt az információt lehetővé teszi egy meghatározni, hogy milyen NSG-k vannak naplózott és a forgalom dátumtartományt.</span><span class="sxs-lookup"><span data-stu-id="9ab16-159">This information gives you an idea of what NSGs are being logged and the date range of flows.</span></span>

![infochart1][11]

![infochart2][12]

<span data-ttu-id="9ab16-162">Ez a sablon tartalmazza a következő szeletelők használatával csak a leginkább megváltozása adatok megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="9ab16-162">This template includes the following slicers to allow you to view only the data you are most interested in.</span></span> <span data-ttu-id="9ab16-163">Az erőforráscsoportok, NSG-ket és szabályok alapján szűrhet.</span><span class="sxs-lookup"><span data-stu-id="9ab16-163">You can filter on your resource groups, NSGs, and rules.</span></span> <span data-ttu-id="9ab16-164">5 rekordos információt, döntést és az idő, a napló írása történt is végezhet.</span><span class="sxs-lookup"><span data-stu-id="9ab16-164">You can also filter on 5-tuple information, decision, and the time the log was written.</span></span>

![a szeletelők][13]

## <a name="conclusion"></a><span data-ttu-id="9ab16-166">Összegzés</span><span class="sxs-lookup"><span data-stu-id="9ab16-166">Conclusion</span></span>

<span data-ttu-id="9ab16-167">Jelenleg mutatott ebben a forgatókönyvben, hogy a hálózati figyelőt, és a Power BI által biztosított hálózati biztonsági csoport Flow naplókat, azt el tudják jelenítheti meg, és megismerheti a forgalmat.</span><span class="sxs-lookup"><span data-stu-id="9ab16-167">We showed in this scenario that by using Network Security Group Flow logs provided by Network Watcher and Power BI, we are able to visualize and understand the traffic.</span></span> <span data-ttu-id="9ab16-168">A megadott sablont használ, a Power BI tölti le a naplók közvetlenül a tárolási és dolgozza fel a helyileg.</span><span class="sxs-lookup"><span data-stu-id="9ab16-168">Using the provided template, Power BI downloads the logs directly from storage and processes them locally.</span></span> <span data-ttu-id="9ab16-169">Sablon betöltéséhez szükséges idő függ a kért fájlok száma, és a fájlok összesített mérete tölti le.</span><span class="sxs-lookup"><span data-stu-id="9ab16-169">Time taken to load the template varies depending on the number of files requested and total size of files downloaded.</span></span>

<span data-ttu-id="9ab16-170">Nyugodtan szabja testre a sablon az igényeinek.</span><span class="sxs-lookup"><span data-stu-id="9ab16-170">Feel free to customize this template for your needs.</span></span> <span data-ttu-id="9ab16-171">Számos módon számos használható a Power BI és hálózati biztonsági csoport Flow naplófájlokat.</span><span class="sxs-lookup"><span data-stu-id="9ab16-171">There are many numerous ways that you can use Power BI with Network Security Group Flow Logs.</span></span> 

## <a name="notes"></a><span data-ttu-id="9ab16-172">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="9ab16-172">Notes</span></span>

* <span data-ttu-id="9ab16-173">Alapértelmezés szerint naplója`https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`</span><span class="sxs-lookup"><span data-stu-id="9ab16-173">Logs by default are stored in `https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`</span></span>

    * <span data-ttu-id="9ab16-174">Ha más adatok ezeket a lekérdezéseket a lehívásos és a folyamat az adatok lehet módosítani egy másik címtárban már létezik.</span><span class="sxs-lookup"><span data-stu-id="9ab16-174">If other data exists in another directory they the queries to pull and process the data must be modified.</span></span>

* <span data-ttu-id="9ab16-175">A megadott sablon használata nem ajánlott a naplók több mint 1 GB.</span><span class="sxs-lookup"><span data-stu-id="9ab16-175">The provided template is not recommended for use with more than 1 GB of logs.</span></span>

* <span data-ttu-id="9ab16-176">Ha nagy mennyiségű naplókat, azt javasoljuk, hogy a használatával, például a Data Lake vagy SQL server egy másik adattároló megoldás vizsgálata.</span><span class="sxs-lookup"><span data-stu-id="9ab16-176">If you have a large amount of logs, we recommend that you investigate a solution using another data store like Data Lake or SQL server.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9ab16-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9ab16-177">Next Steps</span></span>

<span data-ttu-id="9ab16-178">Útmutató: az NSG-folyamat naplók megjelenítése a Elastick veremmel rendelkező ellátogatva [hálózati forgalmának mintáit felé és felől a virtuális gépek nyílt forráskódú eszközökkel megjelenítése](network-watcher-using-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="9ab16-178">Learn how to visualize your NSG flow logs with the Elastick Stack by visiting [Visualize network traffic patterns to and from your VMs using open source tools](network-watcher-using-open-source-tools.md)</span></span>

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
