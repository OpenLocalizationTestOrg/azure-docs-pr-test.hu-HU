---
title: "aaaTesting Azure virtuális hálózat átbocsátóképességét |} Microsoft Docs"
description: "Ismerje meg, hogyan tootest Azure virtuális gép hálózati átviteli sebesség."
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
ms.openlocfilehash: 2da85c27bc8d16a443b215891f4cd0460f41926f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="bandwidththroughput-testing-ntttcp"></a><span data-ttu-id="1f4c9-103">Sávszélesség/átvitel (NTTTCP) tesztelése</span><span class="sxs-lookup"><span data-stu-id="1f4c9-103">Bandwidth/Throughput testing (NTTTCP)</span></span>

<span data-ttu-id="1f4c9-104">Hálózat teljesítménye szempontjából tesztelése az Azure-ban, esetén ajánlott toouse olyan eszköz, amely hello hálózati teszteléséhez célozza, és minimálisra csökkenti a teljesítményt befolyásoló egyéb erőforrások hello használata.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-104">When testing network throughput performance in Azure, it's best toouse a tool that targets hello network for testing and minimizes hello use of other resources that could impact performance.</span></span> <span data-ttu-id="1f4c9-105">NTTTCP ajánlott.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-105">NTTTCP is recommended.</span></span>

<span data-ttu-id="1f4c9-106">Másolja a hello eszköz tootwo hello Azure virtuális gépeinek azonos méretűnek.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-106">Copy hello tool tootwo Azure VMs of hello same size.</span></span> <span data-ttu-id="1f4c9-107">Egy virtuális gép úgy működik, mint a KÜLDŐ és fogadó, hello.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-107">One VM functions as SENDER and hello other as RECEIVER.</span></span>

#### <a name="deploying-vms-for-testing"></a><span data-ttu-id="1f4c9-108">Virtuális gépek telepítése teszteléshez</span><span class="sxs-lookup"><span data-stu-id="1f4c9-108">Deploying VMs for testing</span></span>
<span data-ttu-id="1f4c9-109">Ebben a tesztben hello célokra két virtuális gépek kell hello vagy hello ugyanazt a felhőalapú szolgáltatás, vagy hello az azonos rendelkezésre állási csoportban, hogy azt használja a belső IP-címek és hello Terheléselosztók kizárása hello teszt.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-109">For hello purposes of this test, hello two VMs should be in either hello same Cloud Service or hello same Availability Set so that we can use their internal IPs and exclude hello Load Balancers from hello test.</span></span> <span data-ttu-id="1f4c9-110">Hello VIP a lehetséges tootest, de az ilyen vizsgálat a dokumentum hello hatókör kívül esik.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-110">It is possible tootest with hello VIP but this kind of testing is outside hello scope of this document.</span></span>
 
<span data-ttu-id="1f4c9-111">Jegyezze fel a címzett hello IP-címe.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-111">Make a note of hello RECEIVER's IP address.</span></span> <span data-ttu-id="1f4c9-112">Most hívható meg, hogy IP "a.b.c.r"</span><span class="sxs-lookup"><span data-stu-id="1f4c9-112">Let's call that IP "a.b.c.r"</span></span>

<span data-ttu-id="1f4c9-113">Jegyezze fel a hello magok száma a virtuális gép hello.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-113">Make a note of hello number of cores on hello VM.</span></span> <span data-ttu-id="1f4c9-114">Most hívása a "\#num\_magok"</span><span class="sxs-lookup"><span data-stu-id="1f4c9-114">Let's call this "\#num\_cores"</span></span>
 
<span data-ttu-id="1f4c9-115">Hello NTTTCP teszteléséhez 300 másodperc (azaz 5 perc) virtuális gép hello küldő és fogadó virtuális gép fut.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-115">Run hello NTTTCP test for 300 seconds (or 5 minutes) on hello sender VM and receiver VM.</span></span>

<span data-ttu-id="1f4c9-116">Tipp: Beállításakor a hello ellenőrzéséhez először, próbálja meg egy rövidebb tesztelési időszak tooget visszajelzés hamarabb.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-116">Tip: When setting up this test for hello first time, you might try a shorter test period tooget feedback sooner.</span></span> <span data-ttu-id="1f4c9-117">Miután hello eszköz a várt módon működik, kiterjesztése hello tesztelési időszak too300 másodperc hello legpontosabb eredmények.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-117">Once hello tool is working as expected, extend hello test period too300 seconds for hello most accurate results.</span></span>

> [!NOTE]
> <span data-ttu-id="1f4c9-118">hello küldő **és** adjon meg fogadó **hello azonos** időtartam paraméter tesztelése (-t).</span><span class="sxs-lookup"><span data-stu-id="1f4c9-118">hello sender **and** receiver must specify **hello same** test duration parameter (-t).</span></span>

<span data-ttu-id="1f4c9-119">10 másodperc az egyetlen TCP adatfolyam tootest:</span><span class="sxs-lookup"><span data-stu-id="1f4c9-119">tootest a single TCP stream for 10 seconds:</span></span>

<span data-ttu-id="1f4c9-120">Fogadó paraméterek: ntttcp - r -t 10 - P 1</span><span class="sxs-lookup"><span data-stu-id="1f4c9-120">Receiver parameters: ntttcp -r -t 10 -P 1</span></span>

<span data-ttu-id="1f4c9-121">Küldő paraméterek: ntttcp-s10.27.33.7 -t 10 - n 1 -P 1</span><span class="sxs-lookup"><span data-stu-id="1f4c9-121">Sender parameters: ntttcp -s10.27.33.7 -t 10 -n 1 -P 1</span></span>

> [!NOTE]
> <span data-ttu-id="1f4c9-122">a minta megelőző hello csak akkor használt tooconfirm a konfigurációt.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-122">hello preceding sample should only be used tooconfirm your configuration.</span></span> <span data-ttu-id="1f4c9-123">Érvényes példák a vizsgálat a dokumentum későbbi szakaszában tartoznak.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-123">Valid examples of testing are covered later in this document.</span></span>

## <a name="testing-vms-running-windows"></a><span data-ttu-id="1f4c9-124">WINDOWS rendszerű virtuális gépek ellenőrzési:</span><span class="sxs-lookup"><span data-stu-id="1f4c9-124">Testing VMs running WINDOWS:</span></span>

#### <a name="get-ntttcp-onto-hello-vms"></a><span data-ttu-id="1f4c9-125">NTTTCP jussanak el a virtuális gépek hello.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-125">Get NTTTCP onto hello VMs.</span></span>

<span data-ttu-id="1f4c9-126">Hello legújabb verzió letöltéséhez: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769></span><span class="sxs-lookup"><span data-stu-id="1f4c9-126">Download hello latest version: <https://gallery.technet.microsoft.com/NTttcp-Version-528-Now-f8b12769></span></span>

<span data-ttu-id="1f4c9-127">Vagy keresse meg, ha át: <https://www.bing.com/search?q=ntttcp+download> \< --először találati kell lennie</span><span class="sxs-lookup"><span data-stu-id="1f4c9-127">Or search for it if moved: <https://www.bing.com/search?q=ntttcp+download>\< -- should be first hit</span></span>

<span data-ttu-id="1f4c9-128">Vegye figyelembe a külön mappába, például a c: NTTTCP üzembe\\eszközök</span><span class="sxs-lookup"><span data-stu-id="1f4c9-128">Consider putting NTTTCP in separate folder, like c:\\tools</span></span>

#### <a name="allow-ntttcp-through-hello-windows-firewall"></a><span data-ttu-id="1f4c9-129">Hello Windows tűzfalon keresztül NTTTCP engedélyezése</span><span class="sxs-lookup"><span data-stu-id="1f4c9-129">Allow NTTTCP through hello Windows firewall</span></span>
<span data-ttu-id="1f4c9-130">Hello fogadó hozzon létre egy olyan engedélyezési szabály a Windows tűzfal tooallow hello a NTTTCP forgalom tooarrive.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-130">On hello RECEIVER, create an Allow rule on hello Windows Firewall tooallow the NTTTCP traffic tooarrive.</span></span> <span data-ttu-id="1f4c9-131">Legegyszerűbb tooallow hello teljes NTTTCP program neve helyett tooallow adott TCP-portot a bejövő.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-131">It's easiest tooallow hello entire NTTTCP program by name rather than tooallow specific TCP ports inbound.</span></span>

<span data-ttu-id="1f4c9-132">A Windows tűzfal hello keresztül ntttcp engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="1f4c9-132">Allow ntttcp through hello Windows Firewall like this:</span></span>

<span data-ttu-id="1f4c9-133">netsh advfirewall tűzfal program szabály hozzáadása =\<ELÉRÉSI\>\\ntttcp.exe neve = "ntttcp" protokoll bármely dir = művelet = = enable engedélyezése = yes profil = ANY</span><span class="sxs-lookup"><span data-stu-id="1f4c9-133">netsh advfirewall firewall add rule program=\<PATH\>\\ntttcp.exe name="ntttcp" protocol=any dir=in action=allow enable=yes profile=ANY</span></span>

<span data-ttu-id="1f4c9-134">Például, ha a másolt ntttcp.exe toohello "c:\\eszközök" mappa, hello parancs lenne:</span><span class="sxs-lookup"><span data-stu-id="1f4c9-134">For example, if you copied ntttcp.exe toohello "c:\\tools" folder, this would be hello command:</span></span> 

<span data-ttu-id="1f4c9-135">netsh advfirewall tűzfal hozzáadása szabály program = c:\\eszközök\\ntttcp.exe neve = "ntttcp" protokoll bármely dir = művelet = = enable engedélyezése = yes profil = bármely</span><span class="sxs-lookup"><span data-stu-id="1f4c9-135">netsh advfirewall firewall add rule program=c:\\tools\\ntttcp.exe name="ntttcp" protocol=any dir=in action=allow enable=yes profile=ANY</span></span>

#### <a name="running-ntttcp-tests"></a><span data-ttu-id="1f4c9-136">Tesztek futtatása NTTTCP</span><span class="sxs-lookup"><span data-stu-id="1f4c9-136">Running NTTTCP tests</span></span>

<span data-ttu-id="1f4c9-137">Indítsa el a NTTTCP a fogadó hello (**CMD-ről futtatva**, nem a PowerShell):</span><span class="sxs-lookup"><span data-stu-id="1f4c9-137">Start NTTTCP on hello RECEIVER (**run from CMD**, not from PowerShell):</span></span>

<span data-ttu-id="1f4c9-138">ntttcp - r-m [2\*\#num\_magok],\*, a.b.c.r -t 300</span><span class="sxs-lookup"><span data-stu-id="1f4c9-138">ntttcp -r –m [2\*\#num\_cores],\*,a.b.c.r -t 300</span></span>

<span data-ttu-id="1f4c9-139">Ha virtuális gép hello négy maggal és 10.0.0.4 IP-címét, azt néznek ki:</span><span class="sxs-lookup"><span data-stu-id="1f4c9-139">If hello VM has four cores and an IP address of 10.0.0.4, it would look like this:</span></span>

<span data-ttu-id="1f4c9-140">ntttcp - r – m 8,\*, 10.0.0.4 -t 300</span><span class="sxs-lookup"><span data-stu-id="1f4c9-140">ntttcp -r –m 8,\*,10.0.0.4 -t 300</span></span>


<span data-ttu-id="1f4c9-141">Indítsa el a NTTTCP a KÜLDŐ hello (**CMD-ről futtatva**, nem a PowerShell):</span><span class="sxs-lookup"><span data-stu-id="1f4c9-141">Start NTTTCP on hello SENDER (**run from CMD**, not from PowerShell):</span></span>

<span data-ttu-id="1f4c9-142">ntttcp -s – m 8,\*, 10.0.0.4 -t 300</span><span class="sxs-lookup"><span data-stu-id="1f4c9-142">ntttcp -s –m 8,\*,10.0.0.4 -t 300</span></span> 

<span data-ttu-id="1f4c9-143">Várjon, amíg a hello eredmények elérése érdekében.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-143">Wait for hello results.</span></span>


## <a name="testing-vms-running-linux"></a><span data-ttu-id="1f4c9-144">Linuxos virtuális gépek ellenőrzési:</span><span class="sxs-lookup"><span data-stu-id="1f4c9-144">Testing VMs running LINUX:</span></span>

<span data-ttu-id="1f4c9-145">Nttcp a linux használja.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-145">Use nttcp-for-linux.</span></span> <span data-ttu-id="1f4c9-146">Az elérhető <https://github.com/Microsoft/ntttcp-for-linux></span><span class="sxs-lookup"><span data-stu-id="1f4c9-146">It is available from <https://github.com/Microsoft/ntttcp-for-linux></span></span>

<span data-ttu-id="1f4c9-147">A Linux virtuális gépek (KÜLDŐ és fogadó) hello futtassa az alábbi parancsokat ntttcp-az-linux a virtuális gépeken előkészítése:</span><span class="sxs-lookup"><span data-stu-id="1f4c9-147">On hello Linux VMs (both SENDER and RECEIVER), run these commands to prepare ntttcp-for-linux on your VMs:</span></span>

<span data-ttu-id="1f4c9-148">CentOS - telepítés Git:</span><span class="sxs-lookup"><span data-stu-id="1f4c9-148">CentOS - Install Git:</span></span>
``` bash
  yum install gcc -y  
  yum install git -y
```
<span data-ttu-id="1f4c9-149">Ubuntu - telepítés Git:</span><span class="sxs-lookup"><span data-stu-id="1f4c9-149">Ubuntu - Install Git:</span></span>
``` bash
 apt-get -y install build-essential  
 apt-get -y install git
```
<span data-ttu-id="1f4c9-150">Ellenőrizze, és mindkét telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="1f4c9-150">Make and Install on both:</span></span>
``` bash
 git clone <https://github.com/Microsoft/ntttcp-for-linux>
 cd ntttcp-for-linux/src
 make && make install
```

<span data-ttu-id="1f4c9-151">Ahogy hello Windows példában feltételezzük, hogy hello Linux fogadó IP-cím 10.0.0.4</span><span class="sxs-lookup"><span data-stu-id="1f4c9-151">As in hello Windows example, we assume hello Linux RECEIVER's IP is 10.0.0.4</span></span>

<span data-ttu-id="1f4c9-152">Indítsa el a NTTTCP a Linux a fogadó hello:</span><span class="sxs-lookup"><span data-stu-id="1f4c9-152">Start NTTTCP-for-Linux on hello RECEIVER:</span></span>

``` bash
ntttcp -r -t 300
```

<span data-ttu-id="1f4c9-153">És hello KÜLDŐ, futtassa:</span><span class="sxs-lookup"><span data-stu-id="1f4c9-153">And on hello SENDER, run:</span></span>

``` bash
ntttcp -s10.0.0.4 -t 300
```
 
<span data-ttu-id="1f4c9-154">Ha nincs paraméter teszt hossza alapértelmezett too60 másodperc kap</span><span class="sxs-lookup"><span data-stu-id="1f4c9-154">Test length defaults too60 seconds if no time parameter is given</span></span>

## <a name="testing-between-vms-running-windows-and-linux"></a><span data-ttu-id="1f4c9-155">Az ellenőrzési Windows és LINUX rendszerű virtuális gépek között:</span><span class="sxs-lookup"><span data-stu-id="1f4c9-155">Testing between VMs running Windows and LINUX:</span></span>

<span data-ttu-id="1f4c9-156">Ez a forgatókönyv nem kell engedélyezni a hello nem szinkron módban hello vizsgálat futtatható, számára.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-156">On this scenarios we should enable hello no-sync mode so hello test can run.</span></span> <span data-ttu-id="1f4c9-157">Hello használata ehhez **-N jelző** Linux, és **-ns jelző** Windows.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-157">This is done by using hello **-N flag** for Linux, and **-ns flag** for Windows.</span></span>

#### <a name="from-linux-toowindows"></a><span data-ttu-id="1f4c9-158">A Linux tooWindows:</span><span class="sxs-lookup"><span data-stu-id="1f4c9-158">From Linux tooWindows:</span></span>

<span data-ttu-id="1f4c9-159">Fogadó <Windows>:</span><span class="sxs-lookup"><span data-stu-id="1f4c9-159">Receiver <Windows>:</span></span>

``` bash
ntttcp -r -m <2 x nr cores>,*,<Windows server IP>
```

<span data-ttu-id="1f4c9-160">Küldő <Linux> :</span><span class="sxs-lookup"><span data-stu-id="1f4c9-160">Sender <Linux> :</span></span>

``` bash
ntttcp -s -m <2 x nr cores>,*,<Windows server IP> -N -t 300
```

#### <a name="from-windows-toolinux"></a><span data-ttu-id="1f4c9-161">A Windows tooLinux:</span><span class="sxs-lookup"><span data-stu-id="1f4c9-161">From Windows tooLinux:</span></span>

<span data-ttu-id="1f4c9-162">Fogadó <Linux>:</span><span class="sxs-lookup"><span data-stu-id="1f4c9-162">Receiver <Linux>:</span></span>

``` bash
ntttcp -r -m <2 x nr cores>,*,<Linux server IP>
```

<span data-ttu-id="1f4c9-163">Küldő <Windows>:</span><span class="sxs-lookup"><span data-stu-id="1f4c9-163">Sender <Windows>:</span></span>

``` bash
ntttcp -s -m <2 x nr cores>,*,<Linux  server IP> -ns -t 300
```

## <a name="next-steps"></a><span data-ttu-id="1f4c9-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1f4c9-164">Next steps</span></span>
* <span data-ttu-id="1f4c9-165">Attól függően, hogy az eredmények lehet hely túl[optimalizálhatja a hálózati átviteli gépek](virtual-network-optimize-network-bandwidth.md) a forgatókönyvéhez.</span><span class="sxs-lookup"><span data-stu-id="1f4c9-165">Depending on results, there may be room too[Optimize network throughput machines](virtual-network-optimize-network-bandwidth.md) for your scenario.</span></span>
* <span data-ttu-id="1f4c9-166">Ismerje meg [Azure Virtual Network gyakori kérdések (GYIK)](virtual-networks-faq.md)</span><span class="sxs-lookup"><span data-stu-id="1f4c9-166">Learn more wtih [Azure Virtual Network frequently asked questions (FAQ)](virtual-networks-faq.md)</span></span>
