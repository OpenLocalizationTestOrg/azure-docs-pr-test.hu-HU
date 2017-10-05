---
title: "Az Azure hálózati figyelőt Csomagvizsgálat |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan mély Csomagvizsgálat begyűjti a virtuális gép hálózati figyelőt segítségével"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 7b907d00-9c35-40f5-a61e-beb7b782276f
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 91c47bb8922a9be21dff72e7cf64b29b14a36e9e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="packet-inspection-with-azure-network-watcher"></a><span data-ttu-id="98f4d-103">Az Azure hálózati figyelőt Csomagvizsgálat</span><span class="sxs-lookup"><span data-stu-id="98f4d-103">Packet inspection with Azure Network Watcher</span></span>

<span data-ttu-id="98f4d-104">Csomag rögzítése az szolgáltatásával hálózati figyelőt, kezdeményezzen, és az Azure virtuális gépeken a portálról, PowerShell parancssori felület, és az SDK-t és a REST API-n keresztül szoftveresen rögzítésekre munkameneteket kezelhessen.</span><span class="sxs-lookup"><span data-stu-id="98f4d-104">Using the packet capture feature of Network Watcher, you can initiate and manage captures sessions on your Azure VMs from the portal, PowerShell, CLI, and programmatically through the SDK and REST API.</span></span> <span data-ttu-id="98f4d-105">Csomagrögzítéssel lehetővé teszi az információk könnyen használható formátumban szintű csomagadatok igénylő forgatókönyvek kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="98f4d-105">Packet capture allows you to address scenarios that require packet level data by providing the information in a readily usable format.</span></span> <span data-ttu-id="98f4d-106">Kihasználva ingyenesen elérhető eszközök az adatok vizsgálata, és a virtuális gépek küldött kommunikációs vizsgálja meg, és betekintést nyerhet a hálózati forgalom.</span><span class="sxs-lookup"><span data-stu-id="98f4d-106">Leveraging freely available tools to inspect the data, you can examine communications sent to and from your VMs and gain insights into your network traffic.</span></span> <span data-ttu-id="98f4d-107">Néhány példa-használati csomag rögzítési adatok többek között: hálózati vagy alkalmazás problémákat vizsgálja, hálózati visszaélés és a behatolás kísérletek észlelése vagy előírásoknak való megfelelés karbantartása.</span><span class="sxs-lookup"><span data-stu-id="98f4d-107">Some example uses of packet capture data include: investigating network or application issues, detecting network misuse and intrusion attempts, or maintaining regulatory compliance.</span></span> <span data-ttu-id="98f4d-108">Ebben a cikkben megmutatjuk, hogyan kattintva megnyithatja a csomag rögzítési hálózati figyelő által biztosított népszerű nyílt forráskódú eszköz használatával.</span><span class="sxs-lookup"><span data-stu-id="98f4d-108">In this article, we show how to open a packet capture file provided by Network Watcher using a popular open source tool.</span></span> <span data-ttu-id="98f4d-109">Példa bemutatja, hogyan kapcsolat késleltetése kiszámításához, azonosítani a rendellenes forgalom, és vizsgálja meg a hálózati statisztikákat is lesz elérhető.</span><span class="sxs-lookup"><span data-stu-id="98f4d-109">We will also provide examples showing how to calculate a connection latency, identify abnormal traffic, and examine networking statistics.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="98f4d-110">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="98f4d-110">Before you begin</span></span>

<span data-ttu-id="98f4d-111">Ez a cikk a korábban futtatott egy csomagrögzítéssel végighalad az néhány előre konfigurált forgatókönyvet.</span><span class="sxs-lookup"><span data-stu-id="98f4d-111">This article goes through some pre-configured scenarios on a packet capture that was run previously.</span></span> <span data-ttu-id="98f4d-112">Ezek a forgatókönyvek a csomagrögzítéssel megtekintésével elérhető képességek mutatják be.</span><span class="sxs-lookup"><span data-stu-id="98f4d-112">These scenarios illustrate capabilities that can be accessed by reviewing a packet capture.</span></span> <span data-ttu-id="98f4d-113">Ez a forgatókönyv használ [WireShark](https://www.wireshark.org/) a csomagrögzítéssel vizsgálata.</span><span class="sxs-lookup"><span data-stu-id="98f4d-113">This scenario uses [WireShark](https://www.wireshark.org/) to inspect the packet capture.</span></span>

<span data-ttu-id="98f4d-114">Ez a forgatókönyv azt feltételezi, hogy a virtuális gépen már futott egy csomagrögzítéssel.</span><span class="sxs-lookup"><span data-stu-id="98f4d-114">This scenario assumes you already ran a packet capture on a virtual machine.</span></span> <span data-ttu-id="98f4d-115">Megtudhatja, hogyan hozzon létre egy csomagot rögzítési látogasson el a [kezelése csomagrögzítéseket a portál](network-watcher-packet-capture-manage-portal.md) vagy látogasson el a többi [kezelése Csomagrögzítéseket REST API](network-watcher-packet-capture-manage-rest.md).</span><span class="sxs-lookup"><span data-stu-id="98f4d-115">To learn how to create a packet capture visit [Manage packet captures with the portal](network-watcher-packet-capture-manage-portal.md) or with REST by visiting [Managing Packet Captures with REST API](network-watcher-packet-capture-manage-rest.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="98f4d-116">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="98f4d-116">Scenario</span></span>

<span data-ttu-id="98f4d-117">Ebben a forgatókönyvben azt:</span><span class="sxs-lookup"><span data-stu-id="98f4d-117">In this scenario, you:</span></span>

* <span data-ttu-id="98f4d-118">A csomagrögzítéssel áttekintése</span><span class="sxs-lookup"><span data-stu-id="98f4d-118">Review a packet capture</span></span>

## <a name="calculate-network-latency"></a><span data-ttu-id="98f4d-119">Kiszámítja a hálózati késés</span><span class="sxs-lookup"><span data-stu-id="98f4d-119">Calculate network latency</span></span>

<span data-ttu-id="98f4d-120">Ebben a forgatókönyvben megmutatjuk, hogyan kell megtekinteni a Transmission Control Protocol (TCP) témakör két végpontok közötti előforduló kezdeti oda-vissza időt (RTT).</span><span class="sxs-lookup"><span data-stu-id="98f4d-120">In this scenario, we show how to view the initial Round Trip Time (RTT) of a Transmission Control Protocol (TCP) conversation occurring between two endpoints.</span></span>

<span data-ttu-id="98f4d-121">Ha a TCP-kapcsolat létrejött, az első három az kapcsolatban küldött csomagok járjon el egy mintát, más néven a háromutas kézfogás.</span><span class="sxs-lookup"><span data-stu-id="98f4d-121">When a TCP connection is established, the first three packets sent in the connection follow a pattern commonly referred to as the three-way handshake.</span></span> <span data-ttu-id="98f4d-122">Az első két a kézfogás, az eredeti kérést az ügyfél és a kiszolgáló válaszára küldött csomagok megvizsgálásával azt is kiszámíthatja a késés, ha létrejött a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="98f4d-122">By examining the first two packets sent in this handshake, an initial request from the client and a response from the server, we can calculate the latency when this connection was established.</span></span> <span data-ttu-id="98f4d-123">Ez a késés hivatkozik, az oda-vissza időt (RTT).</span><span class="sxs-lookup"><span data-stu-id="98f4d-123">This latency is referred to as the Round Trip Time (RTT).</span></span> <span data-ttu-id="98f4d-124">A TCP protokoll és a három kézfogás további információt talál a következő erőforrás.</span><span class="sxs-lookup"><span data-stu-id="98f4d-124">For more information on the TCP protocol and the three-way handshake refer to the following resource.</span></span> <span data-ttu-id="98f4d-125">https://support.microsoft.com/en-us/help/172983/Explanation-of-the-three-Way-Handshake-via-TCP-IP</span><span class="sxs-lookup"><span data-stu-id="98f4d-125">https://support.microsoft.com/en-us/help/172983/explanation-of-the-three-way-handshake-via-tcp-ip</span></span>

### <a name="step-1"></a><span data-ttu-id="98f4d-126">1. lépés</span><span class="sxs-lookup"><span data-stu-id="98f4d-126">Step 1</span></span>

<span data-ttu-id="98f4d-127">Indítsa el a WireShark</span><span class="sxs-lookup"><span data-stu-id="98f4d-127">Launch WireShark</span></span>

### <a name="step-2"></a><span data-ttu-id="98f4d-128">2. lépés</span><span class="sxs-lookup"><span data-stu-id="98f4d-128">Step 2</span></span>

<span data-ttu-id="98f4d-129">Betöltési a **.cap** a csomagrögzítéssel a fájlt.</span><span class="sxs-lookup"><span data-stu-id="98f4d-129">Load the **.cap** file from your packet capture.</span></span> <span data-ttu-id="98f4d-130">Ez a fájl megtalálható a blob mentik el a helyileg a virtuális gépen, attól függően, hogyan konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="98f4d-130">This file can be found in the blob it was saved in our locally on the virtual machine, depending on how you configured it.</span></span>

### <a name="step-3"></a><span data-ttu-id="98f4d-131">3. lépés</span><span class="sxs-lookup"><span data-stu-id="98f4d-131">Step 3</span></span>

<span data-ttu-id="98f4d-132">A kezdeti oda-vissza időt (RTT) megtekintéséhez TCP témák, azt fogja csak megtekintik a TCP-kézfogás részt vevő első két csomagok.</span><span class="sxs-lookup"><span data-stu-id="98f4d-132">To view the initial Round Trip Time (RTT) in TCP conversations, we will only be looking at the first two packets involved in the TCP handshake.</span></span> <span data-ttu-id="98f4d-133">Azt fogja használni a három kézfogás, az első két csomagok amelyek [SZIN], [SZIN, Nyugtázási] csomagok.</span><span class="sxs-lookup"><span data-stu-id="98f4d-133">We will be using the first two packets in the three-way handshake, which are the [SYN], [SYN, ACK] packets.</span></span> <span data-ttu-id="98f4d-134">A TCP-fejlécében beállítású néven szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="98f4d-134">They are named for flags set in the TCP header.</span></span> <span data-ttu-id="98f4d-135">A kézfogás [ACK]-csomagját, az utolsó csomag nem használható ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="98f4d-135">The last packet in the handshake, the [ACK] packet, will not be used in this scenario.</span></span> <span data-ttu-id="98f4d-136">A [SZIN] csomagot az ügyfél által küldött.</span><span class="sxs-lookup"><span data-stu-id="98f4d-136">The [SYN] packet is sent by the client.</span></span> <span data-ttu-id="98f4d-137">Miután érkezik a kiszolgáló küldi [Nyugtázási] az ügyféltől a SZIN befogadására jóváhagyva.</span><span class="sxs-lookup"><span data-stu-id="98f4d-137">Once it is received the server sends the [ACK] packet as an acknowledgement of receiving the SYN from the client.</span></span> <span data-ttu-id="98f4d-138">Kihasználva a tényt, hogy a kiszolgáló válaszát igényel csekély terhelést, azt kiszámítása a RTT idő kivonja a [SZIN, Nyugtázási] csomag érkezett az ügyfél által a [SZIN] csomagot az ügyfél által küldött.</span><span class="sxs-lookup"><span data-stu-id="98f4d-138">Leveraging the fact that the server’s response requires very little overhead, we calculate the RTT by subtracting the time the [SYN, ACK] packet was received by the client by the time [SYN] packet was sent by the client.</span></span>

<span data-ttu-id="98f4d-139">WireShark használatával Ez az érték kiszámítása nekünk.</span><span class="sxs-lookup"><span data-stu-id="98f4d-139">Using WireShark this value is calculated for us.</span></span>

<span data-ttu-id="98f4d-140">Könnyebb megtekintéséhez az első két csomagokat a TCP háromutas kézfogás, WireShark által biztosított szűrési képesség felhasznál azt.</span><span class="sxs-lookup"><span data-stu-id="98f4d-140">To more easily view the first two packets in the TCP three-way handshake, we will utilize the filtering capability provided by WireShark.</span></span>

<span data-ttu-id="98f4d-141">A szűrni WireShark, bontsa ki a "Transmission Control Protocol" szegmens egy [SZIN] csomag, a rögzítés, és vizsgálja meg a beállítású, az TCP-fejlécben.</span><span class="sxs-lookup"><span data-stu-id="98f4d-141">To apply the filter in WireShark, expand the “Transmission Control Protocol” Segment of a [SYN] packet in your capture and examine the flags set in the TCP header.</span></span>

<span data-ttu-id="98f4d-142">Mivel azt szeretnék szűrhet minden [SZIN] és [SZIN, Nyugtázási] csomagok jelzők a cofirm, hogy a szinkronizálás a mi bit értéke 1, majd kattintson a jobb gombbal a szin bites, a szűrő -> kijelölt -> alkalmaz.</span><span class="sxs-lookup"><span data-stu-id="98f4d-142">Since we are looking to filter on all [SYN] and [SYN, ACK] packets, under flags cofirm that the Syn bit is set to 1, then right click on the Syn bit -> Apply as Filter -> Selected.</span></span>

![7. ábra][7]

### <a name="step-4"></a><span data-ttu-id="98f4d-144">4. lépés</span><span class="sxs-lookup"><span data-stu-id="98f4d-144">Step 4</span></span>

<span data-ttu-id="98f4d-145">Most, hogy a ablakot, csak a [SZIN] bittel csomagok szűrését, egyszerűen kiválaszthatja beszélgetések érdekli a kezdeti RTT megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="98f4d-145">Now that you have filtered the window to only see packets with the [SYN] bit set, you can easily select conversations you are interested in to view the initial RTT.</span></span> <span data-ttu-id="98f4d-146">Egyszerűen tekintse meg a RTT WireShark egyszerűen kattintson a legördülő listában megjelölt "SEQ/Nyugtázási" elemzés.</span><span class="sxs-lookup"><span data-stu-id="98f4d-146">A simple way to view the RTT in WireShark simply click the dropdown marked “SEQ/ACK” analysis.</span></span> <span data-ttu-id="98f4d-147">Ekkor megjelenik a RTT jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="98f4d-147">You will then see the RTT displayed.</span></span> <span data-ttu-id="98f4d-148">Ebben az esetben a RTT 0.0022114 másodperc, vagy 2.211 ms volt.</span><span class="sxs-lookup"><span data-stu-id="98f4d-148">In this case, the RTT was 0.0022114 seconds, or 2.211 ms.</span></span>

![8. ábra][8]

## <a name="unwanted-protocols"></a><span data-ttu-id="98f4d-150">Nem kívánt protokollok</span><span class="sxs-lookup"><span data-stu-id="98f4d-150">Unwanted protocols</span></span>

<span data-ttu-id="98f4d-151">Akkor is számos olyan alkalmazás, az Azure-ban telepített virtuálisgép-példányon futnak.</span><span class="sxs-lookup"><span data-stu-id="98f4d-151">You can have many applications running on a virtual machine instance you have deployed in Azure.</span></span> <span data-ttu-id="98f4d-152">Ezek az alkalmazások számos kommunikálni a hálózaton keresztül, akár Ön engedélye nélkül.</span><span class="sxs-lookup"><span data-stu-id="98f4d-152">Many of these applications communicate over the network, perhaps without your explicit permission.</span></span> <span data-ttu-id="98f4d-153">Csomagrögzítéssel tárolásához a hálózati kommunikációt használ, azt is vizsgálja ki hogyan alkalmazás vannak a hálózaton van szó, és keresse meg az esetleges problémákat.</span><span class="sxs-lookup"><span data-stu-id="98f4d-153">Using packet capture to store network communication, we can investigate how application are talking on the network and look for any issues.</span></span>

<span data-ttu-id="98f4d-154">Ebben a példában egy korábbi tanulmányozzák csomagrögzítéssel nemkívánatos protokollok, amelyek nem hitelesített kommunikáció a számítógépen futó valamelyik alkalmazás futott.</span><span class="sxs-lookup"><span data-stu-id="98f4d-154">In this example, we review a previous ran packet capture for unwanted protocols that may indicate unauthorized communication from an application running on your machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="98f4d-155">1. lépés</span><span class="sxs-lookup"><span data-stu-id="98f4d-155">Step 1</span></span>

<span data-ttu-id="98f4d-156">Kattintson az előző példában a azonos rögzítési használatával **statisztika** > **protokoll hierarchia**</span><span class="sxs-lookup"><span data-stu-id="98f4d-156">Using the same capture in the previous scenario click **Statistics** > **Protocol Hierarchy**</span></span>

![protokoll hierarchia menü][2]

<span data-ttu-id="98f4d-158">A protokoll hierarchia ablak.</span><span class="sxs-lookup"><span data-stu-id="98f4d-158">The protocol hierarchy window appears.</span></span> <span data-ttu-id="98f4d-159">Ez a nézet a rögzítési munkamenet és a protokollal fogadott és küldött csomagok száma során használt összes protokollok listáját jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="98f4d-159">This view provides a list of all the protocols that were in use during the capture session and the number of packets transmitted and received using the protocols.</span></span> <span data-ttu-id="98f4d-160">Ez a nézet a nem kívánt hálózati forgalmat a virtuális gépek vagy a hálózaton kereséshez hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="98f4d-160">This view can be useful for finding unwanted network traffic on your virtual machines or network.</span></span>

![protokoll hierarchia megnyitása][3]

<span data-ttu-id="98f4d-162">Mint az alábbi képernyőfelvételen látható, hiba történt a forgalmat a társ-társ fájlmegosztás használt BitTorrent protokoll segítségével.</span><span class="sxs-lookup"><span data-stu-id="98f4d-162">As you can see in the following screen capture, there was traffic using the BitTorrent protocol, which is used for peer to peer file sharing.</span></span> <span data-ttu-id="98f4d-163">Rendszergazdaként nem várható, hogy BitTorrent forgalom az adott virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="98f4d-163">As an administrator you do not expect to see BitTorrent traffic on this particular virtual machines.</span></span> <span data-ttu-id="98f4d-164">Most már tisztában legyen a forgalom, akkor eltávolíthatja a társ-társ szoftver, a virtuális gépen telepített, vagy az adatforgalmat, a hálózati biztonsági csoportok vagy egy tűzfal.</span><span class="sxs-lookup"><span data-stu-id="98f4d-164">Now you aware of this traffic, you can remove the peer to peer software that installed on this virtual machine, or block the traffic using Network Security Groups or a Firewall.</span></span> <span data-ttu-id="98f4d-165">Emellett előfordulhat, hogy en csomag rögzítésekre fussanak ütemezés szerint, ezért tekintse át a protokoll használatát a virtuális gépeken rendszeresen.</span><span class="sxs-lookup"><span data-stu-id="98f4d-165">Additionally, you may elect to run packet captures on a schedule, so you can review the protocol use on your virtual machines regularly.</span></span> <span data-ttu-id="98f4d-166">Az azure-ban a hálózati feladatok automatizálása a például a Microsoft [az azure Automation szolgáltatásban, a hálózati erőforrások figyelése](network-watcher-monitor-with-azure-automation.md)</span><span class="sxs-lookup"><span data-stu-id="98f4d-166">For an example on how to automate network tasks in azure visit [Monitor network resources with azure automation](network-watcher-monitor-with-azure-automation.md)</span></span>

## <a name="finding-top-destinations-and-ports"></a><span data-ttu-id="98f4d-167">Felső a célok és portok keresése</span><span class="sxs-lookup"><span data-stu-id="98f4d-167">Finding top destinations and ports</span></span>

<span data-ttu-id="98f4d-168">A típusú forgalom, a végpontok és a portok keresztül érkezett megértése, egy fontos figyelés vagy hibaelhárítási alkalmazások és a hálózati erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="98f4d-168">Understanding the types of traffic, the endpoints, and the ports communicated over is an important when monitoring or troubleshooting applications and resources on your network.</span></span> <span data-ttu-id="98f4d-169">A fenti csomag rögzítési fájlt használ, azt is gyorsan ismerje meg a virtuális gép kommunikál a felső célhelyekre és a portok kihasználtságának.</span><span class="sxs-lookup"><span data-stu-id="98f4d-169">Utilizing a packet capture file from above, we can quickly learn the top destinations our VM is communicating with and the ports being utilized.</span></span>

### <a name="step-1"></a><span data-ttu-id="98f4d-170">1. lépés</span><span class="sxs-lookup"><span data-stu-id="98f4d-170">Step 1</span></span>

<span data-ttu-id="98f4d-171">Kattintson az előző példában a azonos rögzítési használatával **statisztika** > **IPv4 statisztika** > **a célok és portok**</span><span class="sxs-lookup"><span data-stu-id="98f4d-171">Using the same capture in the previous scenario click **Statistics** > **IPv4 Statistics** > **Destinations and Ports**</span></span>

![csomag rögzítési ablak][4]

### <a name="step-2"></a><span data-ttu-id="98f4d-173">2. lépés</span><span class="sxs-lookup"><span data-stu-id="98f4d-173">Step 2</span></span>

<span data-ttu-id="98f4d-174">Reméljük, az eredményeket egy sor feladat keresztül, mivel nincsenek 111 porton több kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="98f4d-174">As we look through the results a line stands out, there were multiple connections on port 111.</span></span> <span data-ttu-id="98f4d-175">A leggyakrabban használt port lett 3389-es, vagyis a távoli asztal, és a fennmaradó RPC dinamikus portok.</span><span class="sxs-lookup"><span data-stu-id="98f4d-175">The most used port was 3389, which is remote desktop, and the remaining are RPC dynamic ports.</span></span>

<span data-ttu-id="98f4d-176">Ez az adatforgalom azt jelentheti, semmi sem, de napjainkban a portot, amelyet a sok kapcsolattal lett megadva, és a rendszergazda számára ismeretlen.</span><span class="sxs-lookup"><span data-stu-id="98f4d-176">While this traffic may mean nothing, it is a port that was used for many connections and is unknown to the administrator.</span></span>

![5. ábra][5]

### <a name="step-3"></a><span data-ttu-id="98f4d-178">3. lépés</span><span class="sxs-lookup"><span data-stu-id="98f4d-178">Step 3</span></span>

<span data-ttu-id="98f4d-179">Most, hogy azt szűrheti, hogy a port alapján a rögzítési hely port kívüli azt észlelte.</span><span class="sxs-lookup"><span data-stu-id="98f4d-179">Now that we have determined an out of place port we can filter our capture based on the port.</span></span>

<span data-ttu-id="98f4d-180">A szűrő ebben a forgatókönyvben a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="98f4d-180">The filter in this scenario would be:</span></span>

```
tcp.port == 111
```

<span data-ttu-id="98f4d-181">Azt adja meg a fenti szűrő szöveget, a szűrő szövegmezőben, majd nyomja le adjon meg.</span><span class="sxs-lookup"><span data-stu-id="98f4d-181">We enter the filter text from above in the filter textbox and hit enter.</span></span>

![6. ábra][6]

<span data-ttu-id="98f4d-183">A találatokban láthatja az összes forgalom érkezik a helyi virtuális gép ugyanazon az alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="98f4d-183">From the results, we can see all the traffic is coming from a local virtual machine on the same subnet.</span></span> <span data-ttu-id="98f4d-184">Ha még nem tudjuk miért jelentkezik a forgalmat, azt további vizsgálhatja meg a csomagok és meghatározni, miért azt, hogy így hívásokat a 111 porton.</span><span class="sxs-lookup"><span data-stu-id="98f4d-184">If we still don’t understand why this traffic is occurring, we can further inspect the packets to determine why it is making these calls on port 111.</span></span> <span data-ttu-id="98f4d-185">Az információ azt is elvégezheti a szükséges műveleteket.</span><span class="sxs-lookup"><span data-stu-id="98f4d-185">With this information we can take the appropriate action.</span></span>

## <a name="next-steps"></a><span data-ttu-id="98f4d-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="98f4d-186">Next steps</span></span>

<span data-ttu-id="98f4d-187">Látogasson el a hálózati figyelőt más diagnosztikai szolgáltatásainak bemutatása [Azure hálózatfigyelési áttekintése](network-watcher-monitoring-overview.md)</span><span class="sxs-lookup"><span data-stu-id="98f4d-187">Learn about the other diagnostic features of Network Watcher by visiting [Azure network monitoring overview](network-watcher-monitoring-overview.md)</span></span>

[1]: ./media/network-watcher-deep-packet-inspection/figure1.png
[2]: ./media/network-watcher-deep-packet-inspection/figure2.png
[3]: ./media/network-watcher-deep-packet-inspection/figure3.png
[4]: ./media/network-watcher-deep-packet-inspection/figure4.png
[5]: ./media/network-watcher-deep-packet-inspection/figure5.png
[6]: ./media/network-watcher-deep-packet-inspection/figure6.png
[7]: ./media/network-watcher-deep-packet-inspection/figure7.png
[8]: ./media/network-watcher-deep-packet-inspection/figure8.png













