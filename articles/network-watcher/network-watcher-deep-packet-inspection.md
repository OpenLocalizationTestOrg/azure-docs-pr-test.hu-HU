---
title: "Azure hálózati figyelőt aaaPacket vizsgálatának |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toouse hálózati figyelőt tooperform mély Csomagvizsgálat begyűjti a virtuális gép"
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
ms.openlocfilehash: 4aeddcd482edc4df3d63e87b5c4b0788c540850b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="packet-inspection-with-azure-network-watcher"></a><span data-ttu-id="29b57-103">Az Azure hálózati figyelőt Csomagvizsgálat</span><span class="sxs-lookup"><span data-stu-id="29b57-103">Packet inspection with Azure Network Watcher</span></span>

<span data-ttu-id="29b57-104">A szolgáltatással hello csomagok rögzítése a hálózati figyelőt, kezdeményezzen, és rögzíti munkameneteket kezelhessen a portálról hello PowerShell parancssori felület, és szoftveres hello SDK keresztül Azure virtuális gépeken és a REST API-t.</span><span class="sxs-lookup"><span data-stu-id="29b57-104">Using hello packet capture feature of Network Watcher, you can initiate and manage captures sessions on your Azure VMs from hello portal, PowerShell, CLI, and programmatically through hello SDK and REST API.</span></span> <span data-ttu-id="29b57-105">Csomagrögzítéssel lehetővé teszi csomagadatok szintű hello információk könnyen használható formátumban igénylő tooaddress helyzeteket.</span><span class="sxs-lookup"><span data-stu-id="29b57-105">Packet capture allows you tooaddress scenarios that require packet level data by providing hello information in a readily usable format.</span></span> <span data-ttu-id="29b57-106">Ingyenesen elérhető eszközök tooinspect hello adatok kihasználva, vizsgálja meg a virtuális gépek tooand küldött kommunikációra, és betekintést nyerhet a hálózati forgalmat.</span><span class="sxs-lookup"><span data-stu-id="29b57-106">Leveraging freely available tools tooinspect hello data, you can examine communications sent tooand from your VMs and gain insights into your network traffic.</span></span> <span data-ttu-id="29b57-107">Néhány példa-használati csomag rögzítési adatok többek között: hálózati vagy alkalmazás problémákat vizsgálja, hálózati visszaélés és a behatolás kísérletek észlelése vagy előírásoknak való megfelelés karbantartása.</span><span class="sxs-lookup"><span data-stu-id="29b57-107">Some example uses of packet capture data include: investigating network or application issues, detecting network misuse and intrusion attempts, or maintaining regulatory compliance.</span></span> <span data-ttu-id="29b57-108">Ebben a cikkben megmutatjuk, hogyan tooopen csomag rögzítési fájlt által biztosított hálózati figyelőt népszerű nyílt forráskódú eszköz használatával.</span><span class="sxs-lookup"><span data-stu-id="29b57-108">In this article, we show how tooopen a packet capture file provided by Network Watcher using a popular open source tool.</span></span> <span data-ttu-id="29b57-109">Hogyan toocalculate egy kapcsolat késleltetése azonosítani a rendellenes forgalom, és vizsgálja meg a hálózati statisztika bemutató példák azt is nyújt.</span><span class="sxs-lookup"><span data-stu-id="29b57-109">We will also provide examples showing how toocalculate a connection latency, identify abnormal traffic, and examine networking statistics.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="29b57-110">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="29b57-110">Before you begin</span></span>

<span data-ttu-id="29b57-111">Ez a cikk a korábban futtatott egy csomagrögzítéssel végighalad az néhány előre konfigurált forgatókönyvet.</span><span class="sxs-lookup"><span data-stu-id="29b57-111">This article goes through some pre-configured scenarios on a packet capture that was run previously.</span></span> <span data-ttu-id="29b57-112">Ezek a forgatókönyvek a csomagrögzítéssel megtekintésével elérhető képességek mutatják be.</span><span class="sxs-lookup"><span data-stu-id="29b57-112">These scenarios illustrate capabilities that can be accessed by reviewing a packet capture.</span></span> <span data-ttu-id="29b57-113">Ez a forgatókönyv használ [WireShark](https://www.wireshark.org/) tooinspect hello csomagrögzítéssel.</span><span class="sxs-lookup"><span data-stu-id="29b57-113">This scenario uses [WireShark](https://www.wireshark.org/) tooinspect hello packet capture.</span></span>

<span data-ttu-id="29b57-114">Ez a forgatókönyv azt feltételezi, hogy a virtuális gépen már futott egy csomagrögzítéssel.</span><span class="sxs-lookup"><span data-stu-id="29b57-114">This scenario assumes you already ran a packet capture on a virtual machine.</span></span> <span data-ttu-id="29b57-115">hogyan keresse fel a csomagrögzítéssel toocreate toolearn [kezelése csomagrögzítéseket hello portal](network-watcher-packet-capture-manage-portal.md) vagy látogasson el a többi [kezelése Csomagrögzítéseket REST API](network-watcher-packet-capture-manage-rest.md).</span><span class="sxs-lookup"><span data-stu-id="29b57-115">toolearn how toocreate a packet capture visit [Manage packet captures with hello portal](network-watcher-packet-capture-manage-portal.md) or with REST by visiting [Managing Packet Captures with REST API](network-watcher-packet-capture-manage-rest.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="29b57-116">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="29b57-116">Scenario</span></span>

<span data-ttu-id="29b57-117">Ebben a forgatókönyvben azt:</span><span class="sxs-lookup"><span data-stu-id="29b57-117">In this scenario, you:</span></span>

* <span data-ttu-id="29b57-118">A csomagrögzítéssel áttekintése</span><span class="sxs-lookup"><span data-stu-id="29b57-118">Review a packet capture</span></span>

## <a name="calculate-network-latency"></a><span data-ttu-id="29b57-119">Kiszámítja a hálózati késés</span><span class="sxs-lookup"><span data-stu-id="29b57-119">Calculate network latency</span></span>

<span data-ttu-id="29b57-120">Ebben a forgatókönyvben megmutatjuk, hogyan tooview hello kezdeti oda-vissza időt (RTT) a Transmission Control Protocol (TCP) témakör két végpontok közötti lépett fel.</span><span class="sxs-lookup"><span data-stu-id="29b57-120">In this scenario, we show how tooview hello initial Round Trip Time (RTT) of a Transmission Control Protocol (TCP) conversation occurring between two endpoints.</span></span>

<span data-ttu-id="29b57-121">Ha a TCP-kapcsolat létrejött, hello hello kapcsolatban küldött első három csomagok járjon el a mintát gyakran említett tooas hello háromutas kézfogás.</span><span class="sxs-lookup"><span data-stu-id="29b57-121">When a TCP connection is established, hello first three packets sent in hello connection follow a pattern commonly referred tooas hello three-way handshake.</span></span> <span data-ttu-id="29b57-122">A kézfogás, az eredeti kérést hello ügyfélről és hello kiszolgáló válaszára küldi hello első két csomagok megvizsgálásával azt is kiszámíthatja hello késleltetés, ha létrejött a kapcsolat.</span><span class="sxs-lookup"><span data-stu-id="29b57-122">By examining hello first two packets sent in this handshake, an initial request from hello client and a response from hello server, we can calculate hello latency when this connection was established.</span></span> <span data-ttu-id="29b57-123">Ez a késés hivatkozott tooas hello oda-vissza időt (RTT).</span><span class="sxs-lookup"><span data-stu-id="29b57-123">This latency is referred tooas hello Round Trip Time (RTT).</span></span> <span data-ttu-id="29b57-124">Tekintse meg a következő erőforrás toohello hello TCP protokollt és hello háromutas kézfogás további információt.</span><span class="sxs-lookup"><span data-stu-id="29b57-124">For more information on hello TCP protocol and hello three-way handshake refer toohello following resource.</span></span> <span data-ttu-id="29b57-125">https://support.microsoft.com/en-us/help/172983/Explanation-of-the-three-Way-Handshake-via-TCP-IP</span><span class="sxs-lookup"><span data-stu-id="29b57-125">https://support.microsoft.com/en-us/help/172983/explanation-of-the-three-way-handshake-via-tcp-ip</span></span>

### <a name="step-1"></a><span data-ttu-id="29b57-126">1. lépés</span><span class="sxs-lookup"><span data-stu-id="29b57-126">Step 1</span></span>

<span data-ttu-id="29b57-127">Indítsa el a WireShark</span><span class="sxs-lookup"><span data-stu-id="29b57-127">Launch WireShark</span></span>

### <a name="step-2"></a><span data-ttu-id="29b57-128">2. lépés</span><span class="sxs-lookup"><span data-stu-id="29b57-128">Step 2</span></span>

<span data-ttu-id="29b57-129">Betöltési hello **.cap** a csomagrögzítéssel a fájlt.</span><span class="sxs-lookup"><span data-stu-id="29b57-129">Load hello **.cap** file from your packet capture.</span></span> <span data-ttu-id="29b57-130">Ez a fájl megtalálható hello blob mentik el a helyileg a hello virtuális gép, attól függően, hogyan konfigurálta.</span><span class="sxs-lookup"><span data-stu-id="29b57-130">This file can be found in hello blob it was saved in our locally on hello virtual machine, depending on how you configured it.</span></span>

### <a name="step-3"></a><span data-ttu-id="29b57-131">3. lépés</span><span class="sxs-lookup"><span data-stu-id="29b57-131">Step 3</span></span>

<span data-ttu-id="29b57-132">tooview hello kezdeti oda-vissza időt (RTT) TCP témák, azt fogja csak megtekintik hello első két csomagok hello TCP kézfogás részt.</span><span class="sxs-lookup"><span data-stu-id="29b57-132">tooview hello initial Round Trip Time (RTT) in TCP conversations, we will only be looking at hello first two packets involved in hello TCP handshake.</span></span> <span data-ttu-id="29b57-133">Azt fogja használni hello első két csomagok hello háromutas kézfogás, amelyek hello [SZIN], [SZIN, Nyugtázási] csomagok.</span><span class="sxs-lookup"><span data-stu-id="29b57-133">We will be using hello first two packets in hello three-way handshake, which are hello [SYN], [SYN, ACK] packets.</span></span> <span data-ttu-id="29b57-134">A TCP-fejlécben hello beállítású néven szerepelnek.</span><span class="sxs-lookup"><span data-stu-id="29b57-134">They are named for flags set in hello TCP header.</span></span> <span data-ttu-id="29b57-135">utolsó csomag hello hello kézfogás, hello [Nyugtázási] csomagot nem használható ebben a forgatókönyvben.</span><span class="sxs-lookup"><span data-stu-id="29b57-135">hello last packet in hello handshake, hello [ACK] packet, will not be used in this scenario.</span></span> <span data-ttu-id="29b57-136">[SZIN] hello csomagok hello ügyfél által küldött.</span><span class="sxs-lookup"><span data-stu-id="29b57-136">hello [SYN] packet is sent by hello client.</span></span> <span data-ttu-id="29b57-137">Miután érkezik a hello a kiszolgáló elküldi a hello [Nyugtázási] csomag hello SZIN fogadása hello ügyfél jóváhagyva.</span><span class="sxs-lookup"><span data-stu-id="29b57-137">Once it is received hello server sends hello [ACK] packet as an acknowledgement of receiving hello SYN from hello client.</span></span> <span data-ttu-id="29b57-138">Hello tényt, hogy a hello kiszolgáló válaszára csekély terhelés igényel, ami, azt kiszámításához hello [SZIN, Nyugtázási] csomag hello idő [SZIN] csomag fogadta hello ügyfél hello idő kivonásával RTT hello ügyfél által küldött hello.</span><span class="sxs-lookup"><span data-stu-id="29b57-138">Leveraging hello fact that hello server’s response requires very little overhead, we calculate hello RTT by subtracting hello time hello [SYN, ACK] packet was received by hello client by hello time [SYN] packet was sent by hello client.</span></span>

<span data-ttu-id="29b57-139">WireShark használatával Ez az érték kiszámítása nekünk.</span><span class="sxs-lookup"><span data-stu-id="29b57-139">Using WireShark this value is calculated for us.</span></span>

<span data-ttu-id="29b57-140">toomore könnyen hozzáférhető a hello első két csomagok hello TCP háromutas kézfogás, azt felhasznál hello WireShark által biztosított képesség szűrést.</span><span class="sxs-lookup"><span data-stu-id="29b57-140">toomore easily view hello first two packets in hello TCP three-way handshake, we will utilize hello filtering capability provided by WireShark.</span></span>

<span data-ttu-id="29b57-141">tooapply hello szűrő WireShark, bontsa ki a hello "Transmission Control Protocol" szegmens egy [SZIN] csomag, a rögzítés, és vizsgálja meg a TCP-fejlécben hello hello beállítású.</span><span class="sxs-lookup"><span data-stu-id="29b57-141">tooapply hello filter in WireShark, expand hello “Transmission Control Protocol” Segment of a [SYN] packet in your capture and examine hello flags set in hello TCP header.</span></span>

<span data-ttu-id="29b57-142">Mivel azt keresett toofilter összes [SZIN] és [SZIN, Nyugtázási] csomagok jelzők a cofirm hello szin bitet too1, majd kattintson a jobb gombbal a hello szin bit, a szűrő -> kijelölt -> alkalmaz.</span><span class="sxs-lookup"><span data-stu-id="29b57-142">Since we are looking toofilter on all [SYN] and [SYN, ACK] packets, under flags cofirm that hello Syn bit is set too1, then right click on hello Syn bit -> Apply as Filter -> Selected.</span></span>

![7. ábra][7]

### <a name="step-4"></a><span data-ttu-id="29b57-144">4. lépés</span><span class="sxs-lookup"><span data-stu-id="29b57-144">Step 4</span></span>

<span data-ttu-id="29b57-145">Most, hogy hello ablak tooonly lásd a csomagok szűrését hello [SZIN] bitje, egyszerűen kiválaszthatja tooview érdekli beszélgetések hello kezdeti RTT.</span><span class="sxs-lookup"><span data-stu-id="29b57-145">Now that you have filtered hello window tooonly see packets with hello [SYN] bit set, you can easily select conversations you are interested in tooview hello initial RTT.</span></span> <span data-ttu-id="29b57-146">Egy egyszerű módon tooview hello RTT a WireShark egyszerűen kattintson a "SEQ/Nyugtázási" elemzés megjelölve hello legördülő.</span><span class="sxs-lookup"><span data-stu-id="29b57-146">A simple way tooview hello RTT in WireShark simply click hello dropdown marked “SEQ/ACK” analysis.</span></span> <span data-ttu-id="29b57-147">Ekkor megjelenik a hello RTT jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="29b57-147">You will then see hello RTT displayed.</span></span> <span data-ttu-id="29b57-148">Ebben az esetben a hello RTT 0.0022114 másodperc, vagy 2.211 ms volt.</span><span class="sxs-lookup"><span data-stu-id="29b57-148">In this case, hello RTT was 0.0022114 seconds, or 2.211 ms.</span></span>

![8. ábra][8]

## <a name="unwanted-protocols"></a><span data-ttu-id="29b57-150">Nem kívánt protokollok</span><span class="sxs-lookup"><span data-stu-id="29b57-150">Unwanted protocols</span></span>

<span data-ttu-id="29b57-151">Akkor is számos olyan alkalmazás, az Azure-ban telepített virtuálisgép-példányon futnak.</span><span class="sxs-lookup"><span data-stu-id="29b57-151">You can have many applications running on a virtual machine instance you have deployed in Azure.</span></span> <span data-ttu-id="29b57-152">Ezek az alkalmazások számos kommunikációhoz hello hálózaton, akár az Ön engedélye nélkül.</span><span class="sxs-lookup"><span data-stu-id="29b57-152">Many of these applications communicate over hello network, perhaps without your explicit permission.</span></span> <span data-ttu-id="29b57-153">Csomag rögzítési toostore hálózati kommunikációra használja, azt is vizsgálja ki hogyan alkalmazás hello hálózaton beszélünk, és keresse meg az esetleges problémákat.</span><span class="sxs-lookup"><span data-stu-id="29b57-153">Using packet capture toostore network communication, we can investigate how application are talking on hello network and look for any issues.</span></span>

<span data-ttu-id="29b57-154">Ebben a példában egy korábbi tanulmányozzák csomagrögzítéssel nemkívánatos protokollok, amelyek nem hitelesített kommunikáció a számítógépen futó valamelyik alkalmazás futott.</span><span class="sxs-lookup"><span data-stu-id="29b57-154">In this example, we review a previous ran packet capture for unwanted protocols that may indicate unauthorized communication from an application running on your machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="29b57-155">1. lépés</span><span class="sxs-lookup"><span data-stu-id="29b57-155">Step 1</span></span>

<span data-ttu-id="29b57-156">Azonos rögzítése hello előző esetben kattintson a használatával hello **statisztika** > **protokoll hierarchia**</span><span class="sxs-lookup"><span data-stu-id="29b57-156">Using hello same capture in hello previous scenario click **Statistics** > **Protocol Hierarchy**</span></span>

![protokoll hierarchia menü][2]

<span data-ttu-id="29b57-158">hello protokoll hierarchia ablak.</span><span class="sxs-lookup"><span data-stu-id="29b57-158">hello protocol hierarchy window appears.</span></span> <span data-ttu-id="29b57-159">Ez a nézet az összes hello protokoll hello rögzítési munkamenet és hello csomagok számának, valamint küldött és fogadott hello protokollok használata során használt listáját tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="29b57-159">This view provides a list of all hello protocols that were in use during hello capture session and hello number of packets transmitted and received using hello protocols.</span></span> <span data-ttu-id="29b57-160">Ez a nézet a nem kívánt hálózati forgalmat a virtuális gépek vagy a hálózaton kereséshez hasznos lehet.</span><span class="sxs-lookup"><span data-stu-id="29b57-160">This view can be useful for finding unwanted network traffic on your virtual machines or network.</span></span>

![protokoll hierarchia megnyitása][3]

<span data-ttu-id="29b57-162">Ahogy az alábbi képernyőfelvétel-készítés hello látja, történt hello BitTorrent protokoll, amely társ toopeer fájlmegosztás használó forgalom.</span><span class="sxs-lookup"><span data-stu-id="29b57-162">As you can see in hello following screen capture, there was traffic using hello BitTorrent protocol, which is used for peer toopeer file sharing.</span></span> <span data-ttu-id="29b57-163">Rendszergazdaként nem várható toosee BitTorrent forgalom az adott virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="29b57-163">As an administrator you do not expect toosee BitTorrent traffic on this particular virtual machines.</span></span> <span data-ttu-id="29b57-164">Most már tisztában legyen a forgalmat, akkor eltávolíthatja hello társ toopeer szoftver van telepítve a virtuális gép vagy blokk hello forgalom hálózati biztonsági csoportok vagy egy tűzfal kezelését.</span><span class="sxs-lookup"><span data-stu-id="29b57-164">Now you aware of this traffic, you can remove hello peer toopeer software that installed on this virtual machine, or block hello traffic using Network Security Groups or a Firewall.</span></span> <span data-ttu-id="29b57-165">Emellett előfordulhat, hogy választja toorun csomag rögzítésekre ütemezés szerint, áttekintheti hello protokoll használatát rendszeresen a virtuális gépeken.</span><span class="sxs-lookup"><span data-stu-id="29b57-165">Additionally, you may elect toorun packet captures on a schedule, so you can review hello protocol use on your virtual machines regularly.</span></span> <span data-ttu-id="29b57-166">Hogyan tooautomate hálózati feladatainak azure látogasson el a példa [az azure Automation szolgáltatásban, a hálózati erőforrások figyelése](network-watcher-monitor-with-azure-automation.md)</span><span class="sxs-lookup"><span data-stu-id="29b57-166">For an example on how tooautomate network tasks in azure visit [Monitor network resources with azure automation](network-watcher-monitor-with-azure-automation.md)</span></span>

## <a name="finding-top-destinations-and-ports"></a><span data-ttu-id="29b57-167">Felső a célok és portok keresése</span><span class="sxs-lookup"><span data-stu-id="29b57-167">Finding top destinations and ports</span></span>

<span data-ttu-id="29b57-168">Hello típusú forgalom ismertetése, hello végpontok, és hello portokon keresztül érkezett egy fontos figyelés vagy hibaelhárítási alkalmazások és a hálózati erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="29b57-168">Understanding hello types of traffic, hello endpoints, and hello ports communicated over is an important when monitoring or troubleshooting applications and resources on your network.</span></span> <span data-ttu-id="29b57-169">A fenti csomag rögzítési fájlt használ, azt gyorsan tanul hello legfontosabb célok a virtuális gép kommunikál, és hello kihasználtságának portok.</span><span class="sxs-lookup"><span data-stu-id="29b57-169">Utilizing a packet capture file from above, we can quickly learn hello top destinations our VM is communicating with and hello ports being utilized.</span></span>

### <a name="step-1"></a><span data-ttu-id="29b57-170">1. lépés</span><span class="sxs-lookup"><span data-stu-id="29b57-170">Step 1</span></span>

<span data-ttu-id="29b57-171">Használatával hello azonos rögzítése hello előző esetben kattintson a **statisztika** > **IPv4 statisztika** > **a célok és portok**</span><span class="sxs-lookup"><span data-stu-id="29b57-171">Using hello same capture in hello previous scenario click **Statistics** > **IPv4 Statistics** > **Destinations and Ports**</span></span>

![csomag rögzítési ablak][4]

### <a name="step-2"></a><span data-ttu-id="29b57-173">2. lépés</span><span class="sxs-lookup"><span data-stu-id="29b57-173">Step 2</span></span>

<span data-ttu-id="29b57-174">Reméljük, hello eredmények keresztül egy vonal jelöli, történt több kapcsolatot 111 porton.</span><span class="sxs-lookup"><span data-stu-id="29b57-174">As we look through hello results a line stands out, there were multiple connections on port 111.</span></span> <span data-ttu-id="29b57-175">hello leggyakrabban használt port lett 3389-es, vagyis a távoli asztal, és hello fennmaradó RPC dinamikus portok.</span><span class="sxs-lookup"><span data-stu-id="29b57-175">hello most used port was 3389, which is remote desktop, and hello remaining are RPC dynamic ports.</span></span>

<span data-ttu-id="29b57-176">Ez az adatforgalom azt jelentheti, semmi sem, de napjainkban ismeretlen toohello rendszergazda, és sok kapcsolatokhoz használt portot.</span><span class="sxs-lookup"><span data-stu-id="29b57-176">While this traffic may mean nothing, it is a port that was used for many connections and is unknown toohello administrator.</span></span>

![5. ábra][5]

### <a name="step-3"></a><span data-ttu-id="29b57-178">3. lépés</span><span class="sxs-lookup"><span data-stu-id="29b57-178">Step 3</span></span>

<span data-ttu-id="29b57-179">Most, hogy a hely port kívüli azt észlelte azt a rögzítési hello port alapján szűrhetők.</span><span class="sxs-lookup"><span data-stu-id="29b57-179">Now that we have determined an out of place port we can filter our capture based on hello port.</span></span>

<span data-ttu-id="29b57-180">hello szűrő ebben a forgatókönyvben a következő lesz:</span><span class="sxs-lookup"><span data-stu-id="29b57-180">hello filter in this scenario would be:</span></span>

```
tcp.port == 111
```

<span data-ttu-id="29b57-181">Azt adja meg a fenti hello szűrő szöveget, hello szűrő szövegmezőben, majd nyomja le adjon meg.</span><span class="sxs-lookup"><span data-stu-id="29b57-181">We enter hello filter text from above in hello filter textbox and hit enter.</span></span>

![6. ábra][6]

<span data-ttu-id="29b57-183">Hello találatokban láthatja az összes hello forgalom helyi virtuális gépről érkező hello ugyanazon az alhálózaton.</span><span class="sxs-lookup"><span data-stu-id="29b57-183">From hello results, we can see all hello traffic is coming from a local virtual machine on hello same subnet.</span></span> <span data-ttu-id="29b57-184">Ha még nem tudjuk miért jelentkezik a forgalmat, azt további vizsgálhatja hello csomagok toodetermine miért azt, hogy így hívásokat a 111 porton.</span><span class="sxs-lookup"><span data-stu-id="29b57-184">If we still don’t understand why this traffic is occurring, we can further inspect hello packets toodetermine why it is making these calls on port 111.</span></span> <span data-ttu-id="29b57-185">Ezt az információt a Microsoft hello megfelelő lépéseket is tarthat.</span><span class="sxs-lookup"><span data-stu-id="29b57-185">With this information we can take hello appropriate action.</span></span>

## <a name="next-steps"></a><span data-ttu-id="29b57-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="29b57-186">Next steps</span></span>

<span data-ttu-id="29b57-187">Ismerje meg, körülbelül hello látogasson el a hálózati figyelőt más diagnosztikai funkcióit [Azure hálózatfigyelési áttekintése](network-watcher-monitoring-overview.md)</span><span class="sxs-lookup"><span data-stu-id="29b57-187">Learn about hello other diagnostic features of Network Watcher by visiting [Azure network monitoring overview](network-watcher-monitoring-overview.md)</span></span>

[1]: ./media/network-watcher-deep-packet-inspection/figure1.png
[2]: ./media/network-watcher-deep-packet-inspection/figure2.png
[3]: ./media/network-watcher-deep-packet-inspection/figure3.png
[4]: ./media/network-watcher-deep-packet-inspection/figure4.png
[5]: ./media/network-watcher-deep-packet-inspection/figure5.png
[6]: ./media/network-watcher-deep-packet-inspection/figure6.png
[7]: ./media/network-watcher-deep-packet-inspection/figure7.png
[8]: ./media/network-watcher-deep-packet-inspection/figure8.png













