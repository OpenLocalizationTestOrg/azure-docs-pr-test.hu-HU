---
title: "Tesztelés Azure virtuális hálózat átbocsátóképességét |} Microsoft Docs"
description: "Megtudhatja, hogyan tesztelheti az Azure virtuális gép hálózati teljesítményt."
services: virtual-network
documentationcenter: na
author: steveesp
manager: Gerald DeGrace
editor: 
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/21/2017
ms.author: steveesp
ms.openlocfilehash: ccebc722386a19014674d7a59757a3685bd50793
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="bandwidththroughput-testing-ntttcp"></a><span data-ttu-id="a07e7-103">Sávszélesség/átvitel (NTTTCP) tesztelése</span><span class="sxs-lookup"><span data-stu-id="a07e7-103">Bandwidth/Throughput testing (NTTTCP)</span></span>

<span data-ttu-id="a07e7-104">Hálózat teljesítménye szempontjából tesztelése az Azure-ban, célszerű olyan eszköz, amely a tesztelési hálózati célozza, és minimálisra csökkenti a teljesítményt befolyásoló egyéb erőforrások használatára.</span><span class="sxs-lookup"><span data-stu-id="a07e7-104">When testing network throughput performance in Azure, it's best to use a tool that targets the network for testing and minimizes the use of other resources that could impact performance.</span></span> <span data-ttu-id="a07e7-105">NTTTCP ajánlott.</span><span class="sxs-lookup"><span data-stu-id="a07e7-105">NTTTCP is recommended.</span></span>

<span data-ttu-id="a07e7-106">Másolja az eszköz két Azure virtuális gépek azonos méretűnek.</span><span class="sxs-lookup"><span data-stu-id="a07e7-106">Copy the tool to two Azure VMs of the same size.</span></span> <span data-ttu-id="a07e7-107">Egy virtuális gép úgy működik, mint a KÜLDŐ és a többi fogadó.</span><span class="sxs-lookup"><span data-stu-id="a07e7-107">One VM functions as SENDER and the other as RECEIVER.</span></span>

#### <a name="deploying-vms-for-testing"></a><span data-ttu-id="a07e7-108">Virtuális gépek telepítése teszteléshez</span><span class="sxs-lookup"><span data-stu-id="a07e7-108">Deploying VMs for testing</span></span>
<span data-ttu-id="a07e7-109">Ez a vizsgálat céljából a két virtuális gépek ugyanazt a felhőalapú szolgáltatást vagy a rendelkezésre állási csoportban kell lennie, hogy azt a belső IP-címet használja, és zárja ki azokat a terheléselosztókat a vizsgálat a.</span><span class="sxs-lookup"><span data-stu-id="a07e7-109">For the purposes of this test, the two VMs should be in either the same Cloud Service or the same Availability Set so that we can use their internal IPs and exclude the Load Balancers from the test.</span></span> <span data-ttu-id="a07e7-110">Lehetséges tesztelése a virtuális IP-címre, de az ilyen tesztelés van ez a dokumentum nem terjed.</span><span class="sxs-lookup"><span data-stu-id="a07e7-110">It is possible to test with the VIP but this kind of testing is outside the scope of this document.</span></span>
 
<span data-ttu-id="a07e7-111">Jegyezze fel a címzett IP-címe.</span><span class="sxs-lookup"><span data-stu-id="a07e7-111">Make a note of the RECEIVER's IP address.</span></span> <span data-ttu-id="a07e7-112">Most hívható meg, hogy IP "a.b.c.r"</span><span class="sxs-lookup"><span data-stu-id="a07e7-112">Let's call that IP "a.b.c.r"</span></span>

<span data-ttu-id="a07e7-113">Jegyezze fel a magok száma a virtuális Gépen.</span><span class="sxs-lookup"><span data-stu-id="a07e7-113">Make a note of the number of cores on the VM.</span></span> <span data-ttu-id="a07e7-114">Most hívása a "\#num\_magok"</span><span class="sxs-lookup"><span data-stu-id="a07e7-114">Let's call this "\#num\_cores"</span></span>
 
<span data-ttu-id="a07e7-115">A NTTTCP teszt futtatása 300 másodperc (azaz 5 perc) a virtuális gép küldő és fogadó virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="a07e7-115">Run the NTTTCP test for 300 seconds (or 5 minutes) on the sender VM and receiver VM.</span></span>

<span data-ttu-id="a07e7-116">Tipp: Ez a teszt az első alkalommal történő beállításakor próbálja meg egy rövidebb tesztelési időszakra visszajelzés hamarabb eléréséhez.</span><span class="sxs-lookup"><span data-stu-id="a07e7-116">Tip: When setting up this test for the first time, you might try a shorter test period to get feedback sooner.</span></span> <span data-ttu-id="a07e7-117">Miután az eszköz a várt módon működik, terjessze ki a tesztelési időszakra 300 másodperc, a legpontosabb eredmény elérése érdekében.</span><span class="sxs-lookup"><span data-stu-id="a07e7-117">Once the tool is working as expected, extend the test period to 300 seconds for the most accurate results.</span></span>

> [!NOTE]
> <span data-ttu-id="a07e7-118">A küldő **és** adjon meg fogadó **azonos** időtartam paraméter tesztelése (-t).</span><span class="sxs-lookup"><span data-stu-id="a07e7-118">The sender **and** receiver must specify **the same** test duration parameter (-t).</span></span>

<span data-ttu-id="a07e7-119">Ennek teszteléséhez egyetlen TCP-folyam 10 másodperc:</span><span class="sxs-lookup"><span data-stu-id="a07e7-119">To test a single TCP stream for 10 seconds:</span></span>

<span data-ttu-id="a07e7-120">Fogadó paraméterek: ntttcp - r -t 10 - P 1</span><span class="sxs-lookup"><span data-stu-id="a07e7-120">Receiver parameters: ntttcp -r -t 10 -P 1</span></span>

<span data-ttu-id="a07e7-121">Küldő paraméterek: ntttcp-s10.27.33.7 -t 10 - n 1 -P 1</span><span class="sxs-lookup"><span data-stu-id="a07e7-121">Sender parameters: ntttcp -s10.27.33.7 -t 10 -n 1 -P 1</span></span>

> [!NOTE]
> <span data-ttu-id="a07e7-122">Az előző példa csak használatával ellenőrizze a konfigurációját.</span><span class="sxs-lookup"><span data-stu-id="a07e7-122">The preceding sample should only be used to confirm your configuration.</span></span> <span data-ttu-id="a07e7-123">Érvényes példák a vizsgálat a dokumentum későbbi szakaszában tartoznak.</span><span class="sxs-lookup"><span data-stu-id="a07e7-123">Valid examples of testing are covered later in this document.</span></span>

## <a name="testing-vms-running-windows"></a><span data-ttu-id="a07e7-124">WINDOWS rendszerű virtuális gépek ellenőrzési:</span><span class="sxs-lookup"><span data-stu-id="a07e7-124">Testing VMs running WINDOWS:</span></span>

#### <a name="get-ntttcp-onto-the-vms"></a><span data-ttu-id="a07e7-125">A virtuális gépek alakzatot NTTTCP beolvasása.</span><span class="sxs-lookup"><span data-stu-id="a07e7-125">Get NTTTCP onto the VMs.</span></span>

<span data-ttu-id="a07e7-126">A legújabb verzió letöltéséhez: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769></span><span class="sxs-lookup"><span data-stu-id="a07e7-126">Download the latest version: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769></span></span>

<span data-ttu-id="a07e7-127">Vagy keresse meg, ha át: <https://www.bing.com/search?q=ntttcp+download> \< --először találati kell lennie</span><span class="sxs-lookup"><span data-stu-id="a07e7-127">Or search for it if moved: <https://www.bing.com/search?q=ntttcp+download>\< -- should be first hit</span></span>

<span data-ttu-id="a07e7-128">Vegye figyelembe a külön mappába, például a c: NTTTCP üzembe\\eszközök</span><span class="sxs-lookup"><span data-stu-id="a07e7-128">Consider putting NTTTCP in separate folder, like c:\\tools</span></span>

#### <a name="allow-ntttcp-through-the-windows-firewall"></a><span data-ttu-id="a07e7-129">A Windows tűzfalon keresztül NTTTCP engedélyezése</span><span class="sxs-lookup"><span data-stu-id="a07e7-129">Allow NTTTCP through the Windows firewall</span></span>
<span data-ttu-id="a07e7-130">A fogadó hozzon létre egy olyan engedélyezési szabály engedélyezi a érkezésére NTTTCP forgalmat a Windows tűzfalon.</span><span class="sxs-lookup"><span data-stu-id="a07e7-130">On the RECEIVER, create an Allow rule on the Windows Firewall to allow the NTTTCP traffic to arrive.</span></span> <span data-ttu-id="a07e7-131">A legegyszerűbb engedélyezése a teljes NTTTCP program neve, ahelyett, hogy az adott TCP-portok bejövő.</span><span class="sxs-lookup"><span data-stu-id="a07e7-131">It's easiest to allow the entire NTTTCP program by name rather than to allow specific TCP ports inbound.</span></span>

<span data-ttu-id="a07e7-132">Ehhez hasonló a Windows tűzfalon ntttcp engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="a07e7-132">Allow ntttcp through the Windows Firewall like this:</span></span>

<span data-ttu-id="a07e7-133">netsh advfirewall tűzfal program szabály hozzáadása =\<ELÉRÉSI\>\\ntttcp.exe neve = "ntttcp" protokoll bármely dir = művelet = = enable engedélyezése = yes profil = ANY</span><span class="sxs-lookup"><span data-stu-id="a07e7-133">netsh advfirewall firewall add rule program=\<PATH\>\\ntttcp.exe name="ntttcp" protocol=any dir=in action=allow enable=yes profile=ANY</span></span>

<span data-ttu-id="a07e7-134">Például, ha a ntttcp.exe másolta a "c:\\eszközök" mappa, a parancs lenne:</span><span class="sxs-lookup"><span data-stu-id="a07e7-134">For example, if you copied ntttcp.exe to the "c:\\tools" folder, this would be the command:</span></span> 

<span data-ttu-id="a07e7-135">netsh advfirewall tűzfal hozzáadása szabály program = c:\\eszközök\\ntttcp.exe neve = "ntttcp" protokoll bármely dir = művelet = = enable engedélyezése = yes profil = bármely</span><span class="sxs-lookup"><span data-stu-id="a07e7-135">netsh advfirewall firewall add rule program=c:\\tools\\ntttcp.exe name="ntttcp" protocol=any dir=in action=allow enable=yes profile=ANY</span></span>

#### <a name="running-ntttcp-tests"></a><span data-ttu-id="a07e7-136">Tesztek futtatása NTTTCP</span><span class="sxs-lookup"><span data-stu-id="a07e7-136">Running NTTTCP tests</span></span>

<span data-ttu-id="a07e7-137">Indítsa el a fogadó NTTTCP (**CMD-ről futtatva**, nem a PowerShell):</span><span class="sxs-lookup"><span data-stu-id="a07e7-137">Start NTTTCP on the RECEIVER (**run from CMD**, not from PowerShell):</span></span>

<span data-ttu-id="a07e7-138">ntttcp - r-m [2\*\#num\_magok],\*, a.b.c.r -t 300</span><span class="sxs-lookup"><span data-stu-id="a07e7-138">ntttcp -r –m [2\*\#num\_cores],\*,a.b.c.r -t 300</span></span>

<span data-ttu-id="a07e7-139">Ha a virtuális gép négy maggal és 10.0.0.4 IP-címét, azt néznek ki:</span><span class="sxs-lookup"><span data-stu-id="a07e7-139">If the VM has four cores and an IP address of 10.0.0.4, it would look like this:</span></span>

<span data-ttu-id="a07e7-140">ntttcp - r – m 8,\*, 10.0.0.4 -t 300</span><span class="sxs-lookup"><span data-stu-id="a07e7-140">ntttcp -r –m 8,\*,10.0.0.4 -t 300</span></span>


<span data-ttu-id="a07e7-141">Indítsa el a KÜLDŐ NTTTCP (**CMD-ről futtatva**, nem a PowerShell):</span><span class="sxs-lookup"><span data-stu-id="a07e7-141">Start NTTTCP on the SENDER (**run from CMD**, not from PowerShell):</span></span>

<span data-ttu-id="a07e7-142">ntttcp -s – m 8,\*, 10.0.0.4 -t 300</span><span class="sxs-lookup"><span data-stu-id="a07e7-142">ntttcp -s –m 8,\*,10.0.0.4 -t 300</span></span> 

<span data-ttu-id="a07e7-143">Várjon, amíg az eredményeket.</span><span class="sxs-lookup"><span data-stu-id="a07e7-143">Wait for the results.</span></span>


## <a name="testing-vms-running-linux"></a><span data-ttu-id="a07e7-144">Linuxos virtuális gépek ellenőrzési:</span><span class="sxs-lookup"><span data-stu-id="a07e7-144">Testing VMs running LINUX:</span></span>

<span data-ttu-id="a07e7-145">Nttcp a linux használja.</span><span class="sxs-lookup"><span data-stu-id="a07e7-145">Use nttcp-for-linux.</span></span> <span data-ttu-id="a07e7-146">Az elérhető <https://github.com/Microsoft/ntttcp-for-linux></span><span class="sxs-lookup"><span data-stu-id="a07e7-146">It is available from <https://github.com/Microsoft/ntttcp-for-linux></span></span>

<span data-ttu-id="a07e7-147">A Linux virtuális gépeken (KÜLDŐ és fogadó), futtassa az alábbi parancsokat ntttcp-az-linux a virtuális gépeken előkészítése:</span><span class="sxs-lookup"><span data-stu-id="a07e7-147">On the Linux VMs (both SENDER and RECEIVER), run these commands to prepare ntttcp-for-linux on your VMs:</span></span>

<span data-ttu-id="a07e7-148">CentOS - telepítés Git:</span><span class="sxs-lookup"><span data-stu-id="a07e7-148">CentOS - Install Git:</span></span>
``` bash
  yum install gcc -y  
  yum install git -y
```
<span data-ttu-id="a07e7-149">Ubuntu - telepítés Git:</span><span class="sxs-lookup"><span data-stu-id="a07e7-149">Ubuntu - Install Git:</span></span>
``` bash
 apt-get -y install build-essential  
 apt-get -y install git
```
<span data-ttu-id="a07e7-150">Ellenőrizze, és mindkét telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="a07e7-150">Make and Install on both:</span></span>
``` bash
 git clone <https://github.com/Microsoft/ntttcp-for-linux>
 cd ntttcp-for-linux/src
 make && make install
```

<span data-ttu-id="a07e7-151">Hasonlóan a Windows a példában feltételezzük, hogy a Linux fogadó IP-cím 10.0.0.4</span><span class="sxs-lookup"><span data-stu-id="a07e7-151">As in the Windows example, we assume the Linux RECEIVER's IP is 10.0.0.4</span></span>

<span data-ttu-id="a07e7-152">A fogadó NTTTCP a Linux Kezdés:</span><span class="sxs-lookup"><span data-stu-id="a07e7-152">Start NTTTCP-for-Linux on the RECEIVER:</span></span>

``` bash
ntttcp -r -t 300
```

<span data-ttu-id="a07e7-153">És a feladó futtassa:</span><span class="sxs-lookup"><span data-stu-id="a07e7-153">And on the SENDER, run:</span></span>

``` bash
ntttcp -s10.0.0.4 -t 300
```
 
<span data-ttu-id="a07e7-154">Teszt hossza az alapértelmezett érték 60 másodperc, ha a paraméter nem kap</span><span class="sxs-lookup"><span data-stu-id="a07e7-154">Test length defaults to 60 seconds if no time parameter is given</span></span>

## <a name="testing-between-vms-running-windows-and-linux"></a><span data-ttu-id="a07e7-155">Az ellenőrzési Windows és LINUX rendszerű virtuális gépek között:</span><span class="sxs-lookup"><span data-stu-id="a07e7-155">Testing between VMs running Windows and LINUX:</span></span>

<span data-ttu-id="a07e7-156">Ez a forgatókönyv nem kell engedélyezni a a nem szinkron módban, a vizsgálat futtatható számára.</span><span class="sxs-lookup"><span data-stu-id="a07e7-156">On this scenarios we should enable the no-sync mode so the test can run.</span></span> <span data-ttu-id="a07e7-157">Ehhez használja a **-N jelző** Linux, és **-ns jelző** Windows.</span><span class="sxs-lookup"><span data-stu-id="a07e7-157">This is done by using the **-N flag** for Linux, and **-ns flag** for Windows.</span></span>

#### <a name="from-linux-to-windows"></a><span data-ttu-id="a07e7-158">A Linux a Windowsba:</span><span class="sxs-lookup"><span data-stu-id="a07e7-158">From Linux to Windows:</span></span>

<span data-ttu-id="a07e7-159">Fogadó <Windows>:</span><span class="sxs-lookup"><span data-stu-id="a07e7-159">Receiver <Windows>:</span></span>

``` bash
ntttcp -r -m <2 x nr cores>,*,<Windows server IP>
```

<span data-ttu-id="a07e7-160">Küldő <Linux> :</span><span class="sxs-lookup"><span data-stu-id="a07e7-160">Sender <Linux> :</span></span>

``` bash
ntttcp -s -m <2 x nr cores>,*,<Windows server IP> -N -t 300
```

#### <a name="from-windows-to-linux"></a><span data-ttu-id="a07e7-161">A Windows, Linux –:</span><span class="sxs-lookup"><span data-stu-id="a07e7-161">From Windows to Linux:</span></span>

<span data-ttu-id="a07e7-162">Fogadó <Linux>:</span><span class="sxs-lookup"><span data-stu-id="a07e7-162">Receiver <Linux>:</span></span>

``` bash
ntttcp -r -m <2 x nr cores>,*,<Linux server IP>
```

<span data-ttu-id="a07e7-163">Küldő <Windows>:</span><span class="sxs-lookup"><span data-stu-id="a07e7-163">Sender <Windows>:</span></span>

``` bash
ntttcp -s -m <2 x nr cores>,*,<Linux  server IP> -ns -t 300
```

## <a name="next-steps"></a><span data-ttu-id="a07e7-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a07e7-164">Next steps</span></span>
* <span data-ttu-id="a07e7-165">Attól függően, hogy az eredmények lehet hely [optimalizálhatja a hálózati átviteli gépek](virtual-network-optimize-network-bandwidth.md) a forgatókönyvéhez.</span><span class="sxs-lookup"><span data-stu-id="a07e7-165">Depending on results, there may be room to [Optimize network throughput machines](virtual-network-optimize-network-bandwidth.md) for your scenario.</span></span>
* <span data-ttu-id="a07e7-166">Ismerje meg [Azure Virtual Network gyakori kérdések (GYIK)](virtual-networks-faq.md)</span><span class="sxs-lookup"><span data-stu-id="a07e7-166">Learn more wtih [Azure Virtual Network frequently asked questions (FAQ)](virtual-networks-faq.md)</span></span>
