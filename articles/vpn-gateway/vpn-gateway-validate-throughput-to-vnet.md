---
title: "VPN átviteli tooa Microsoft Azure virtuális hálózat ellenőrzése |} Microsoft Docs"
description: "hello a jelen dokumentum célja toohelp a felhasználó érvényesítése hello hálózati teljesítményt az azok helyszíni erőforrások tooan Azure virtuális gép."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: jasmc
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/10/2017
ms.author: radwiv;chadmat;genli
ms.openlocfilehash: 9cf0b67145645a3c2a9556b0cc910066cc62a9ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toovalidate-vpn-throughput-tooa-virtual-network"></a><span data-ttu-id="c5830-103">Hogyan toovalidate VPN átviteli tooa virtuális hálózat</span><span class="sxs-lookup"><span data-stu-id="c5830-103">How toovalidate VPN throughput tooa virtual network</span></span>

<span data-ttu-id="c5830-104">VPN gateway-kapcsolattal lehetővé teszi tooestablish biztonságos, létesítmények közötti közötti kapcsolatot a virtuális hálózat az Azure és a helyszíni informatikai infrastruktúrát.</span><span class="sxs-lookup"><span data-stu-id="c5830-104">A VPN gateway connection enables you tooestablish secure, cross-premises connectivity between your Virtual Network within Azure and your on-premises IT infrastructure.</span></span>

<span data-ttu-id="c5830-105">Ez a cikk bemutatja, hogyan toovalidate hálózati teljesítményt hello a helyszíni erőforrások tooan Azure virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="c5830-105">This article shows how toovalidate network throughput from hello on-premises resources tooan Azure virtual machine.</span></span> <span data-ttu-id="c5830-106">Azt is nyújt hibaelhárítási útmutatót.</span><span class="sxs-lookup"><span data-stu-id="c5830-106">It also provides troubleshooting guidance.</span></span>

>[!NOTE]
><span data-ttu-id="c5830-107">Ez a cikk toohelp diagnosztizálhatja és gyakori problémák megoldásával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="c5830-107">This article is intended toohelp diagnose and fix common issues.</span></span> <span data-ttu-id="c5830-108">Ha még nem toosolve hello probléma a következő információ hello segítségével [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="c5830-108">If you're unable toosolve hello issue by using hello following information, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>
>
>

## <a name="overview"></a><span data-ttu-id="c5830-109">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="c5830-109">Overview</span></span>

<span data-ttu-id="c5830-110">hello VPN gateway-kapcsolatot a következő összetevők hello foglal magában:</span><span class="sxs-lookup"><span data-stu-id="c5830-110">hello VPN gateway connection involves hello following components:</span></span>

- <span data-ttu-id="c5830-111">A helyszíni VPN-eszköz (listájának megtekintéséhez [érvényesíteni a VPN-eszközök)](vpn-gateway-about-vpn-devices.md#devicetable).</span><span class="sxs-lookup"><span data-stu-id="c5830-111">On-premises VPN device (view a list of [validated VPN devices)](vpn-gateway-about-vpn-devices.md#devicetable).</span></span>
- <span data-ttu-id="c5830-112">Nyilvános Internet</span><span class="sxs-lookup"><span data-stu-id="c5830-112">Public Internet</span></span>
- <span data-ttu-id="c5830-113">Az Azure VPN gateway</span><span class="sxs-lookup"><span data-stu-id="c5830-113">Azure VPN gateway</span></span>
- <span data-ttu-id="c5830-114">Azure-os virtuális gép</span><span class="sxs-lookup"><span data-stu-id="c5830-114">Azure virtual machine</span></span>

<span data-ttu-id="c5830-115">a következő diagram hello hello logikai kapcsolatát egy a helyszíni hálózati tooan Azure virtuális hálózat VPN-en keresztül jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="c5830-115">hello following diagram shows hello logical connectivity of an on-premises network tooan Azure virtual network through VPN.</span></span>

![Logikai ügyfél hálózati kapcsolat tooMSFT hálózati VPN-nel](./media/vpn-gateway-validate-throughput-to-vnet/VPNPerf.png)

## <a name="calculate-hello-maximum-expected-ingressegress"></a><span data-ttu-id="c5830-117">Hello maximális várt be-és kilépési kiszámítása</span><span class="sxs-lookup"><span data-stu-id="c5830-117">Calculate hello maximum expected ingress/egress</span></span>

1.  <span data-ttu-id="c5830-118">Az alkalmazás eredeti átviteli követelmények meghatározása.</span><span class="sxs-lookup"><span data-stu-id="c5830-118">Determine your application's baseline throughput requirements.</span></span>
2.  <span data-ttu-id="c5830-119">Az Azure VPN gateway átviteli sebességének korlátai határozza meg.</span><span class="sxs-lookup"><span data-stu-id="c5830-119">Determine your Azure VPN gateway throughput limits.</span></span> <span data-ttu-id="c5830-120">Útmutatásért lásd: hello "összesített átviteli SKU- és VPN-típus szerint" szakasza [tervezése és kialakítása VPN-átjáró](vpn-gateway-plan-design.md).</span><span class="sxs-lookup"><span data-stu-id="c5830-120">For help, see hello "Aggregate throughput by SKU and VPN type" section of [Planning and design for VPN Gateway](vpn-gateway-plan-design.md).</span></span>
3.  <span data-ttu-id="c5830-121">Határozza meg a hello [Azure virtuális gép átviteli útmutatást](../virtual-machines/virtual-machines-windows-sizes.md) a Virtuálisgép-mérethez.</span><span class="sxs-lookup"><span data-stu-id="c5830-121">Determine hello [Azure VM throughput guidance](../virtual-machines/virtual-machines-windows-sizes.md) for your VM size.</span></span>
4.  <span data-ttu-id="c5830-122">Határozza meg, hogy az internetszolgáltató (ISP) sávszélességet.</span><span class="sxs-lookup"><span data-stu-id="c5830-122">Determine your Internet Service Provider (ISP) bandwidth.</span></span>
5.  <span data-ttu-id="c5830-123">A várt átviteli - (VM-átjárón internetszolgáltató) minimális sávszélesség kiszámításához * 0,8 értéket.</span><span class="sxs-lookup"><span data-stu-id="c5830-123">Calculate your expected throughput - Least bandwidth of (VM, Gateway, ISP) * 0.8.</span></span>

<span data-ttu-id="c5830-124">Ha a számított teljesítményt nem felel meg az alkalmazás eredeti átviteli követelményeinek, akkor tooincrease hello sávszélesség hello szűk azonosított hello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="c5830-124">If your calculated throughput does not meet your application's baseline throughput requirements, you need tooincrease hello bandwidth of hello resource that you identified as hello bottleneck.</span></span> <span data-ttu-id="c5830-125">Lásd az Azure VPN Gateway tooresize [módosítása egy átjáró-Termékváltozat](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="c5830-125">tooresize an Azure VPN Gateway, see [Changing a gateway SKU](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span></span> <span data-ttu-id="c5830-126">a virtuális gép tooresize lásd: [méretezze át a virtuális gépek](../virtual-machines/virtual-machines-windows-resize-vm.md).</span><span class="sxs-lookup"><span data-stu-id="c5830-126">tooresize a virtual machine, see [Resize a VM](../virtual-machines/virtual-machines-windows-resize-vm.md).</span></span> <span data-ttu-id="c5830-127">Ha nem várt internetes sávszélességet, is érdemes lehet toocontact az Internetszolgáltató.</span><span class="sxs-lookup"><span data-stu-id="c5830-127">If you are not experiencing expected Internet bandwidth, you may also want toocontact your ISP.</span></span>

## <a name="validate-network-throughput-by-using-performance-tools"></a><span data-ttu-id="c5830-128">Hálózati átviteli teljesítmény eszközökkel ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="c5830-128">Validate network throughput by using performance tools</span></span>

<span data-ttu-id="c5830-129">Az ellenőrzés során nem csúcsidőre kell hajtható végre, mert VPN alagút átviteli telítettségét tesztelés során nem ad pontos eredményeket.</span><span class="sxs-lookup"><span data-stu-id="c5830-129">This validation should be performed during non-peak hours, as VPN tunnel throughput saturation during testing does not give accurate results.</span></span>

<span data-ttu-id="c5830-130">a teszteléshez használjuk hello eszköz iPerf, amely Windows és Linux rendszeren működik és ügyfél- és módok.</span><span class="sxs-lookup"><span data-stu-id="c5830-130">hello tool we use for this test is iPerf, which works on both Windows and Linux and has both client and server modes.</span></span> <span data-ttu-id="c5830-131">Az Windows alapú virtuális gépek korlátozott too3 GB/s.</span><span class="sxs-lookup"><span data-stu-id="c5830-131">It is limited too3 Gbps for Windows VMs.</span></span>

<span data-ttu-id="c5830-132">Ez az eszköz nem végez semmilyen olvasási/írási műveletek toodisk.</span><span class="sxs-lookup"><span data-stu-id="c5830-132">This tool does not perform any read/write operations toodisk.</span></span> <span data-ttu-id="c5830-133">Kizárólag a saját TCP-forgalom a egy záró toohello más hoz létre.</span><span class="sxs-lookup"><span data-stu-id="c5830-133">It solely produces self-generated TCP traffic from one end toohello other.</span></span> <span data-ttu-id="c5830-134">Az előállított statisztikát kísérletezhet, és mérheti hello sávszélesség álljon rendelkezésre, ügyfél és kiszolgáló-csomópont között.</span><span class="sxs-lookup"><span data-stu-id="c5830-134">It generated statistics based on experimentation that measures hello bandwidth available between client and server nodes.</span></span> <span data-ttu-id="c5830-135">Ha két csomópont között, egy hello kiszolgálóval és-ügyfélként hello működik.</span><span class="sxs-lookup"><span data-stu-id="c5830-135">When testing between two nodes, one acts as hello server and hello other as a client.</span></span> <span data-ttu-id="c5830-136">Ha ez a vizsgálat befejeződött, azt javasoljuk, hogy megfordította hello szerepkörök tootest egyaránt le- és feltöltése mindkét csomópont átviteli sebességet.</span><span class="sxs-lookup"><span data-stu-id="c5830-136">Once this test is completed, we recommend that you reverse hello roles tootest both upload and download throughput on both nodes.</span></span>

### <a name="download-iperf"></a><span data-ttu-id="c5830-137">Töltse le a iPerf</span><span class="sxs-lookup"><span data-stu-id="c5830-137">Download iPerf</span></span>
<span data-ttu-id="c5830-138">Töltse le [iPerf](https://iperf.fr/download/iperf_3.1/iperf-3.1.2-win64.zip).</span><span class="sxs-lookup"><span data-stu-id="c5830-138">Download [iPerf](https://iperf.fr/download/iperf_3.1/iperf-3.1.2-win64.zip).</span></span> <span data-ttu-id="c5830-139">További információkért lásd: [iPerf dokumentáció](https://iperf.fr/iperf-doc.php).</span><span class="sxs-lookup"><span data-stu-id="c5830-139">For details, see [iPerf documentation](https://iperf.fr/iperf-doc.php).</span></span>

 >[!NOTE]
 ><span data-ttu-id="c5830-140">hello külső cikkben említett termékeket független, a Microsoft által.</span><span class="sxs-lookup"><span data-stu-id="c5830-140">hello third-party products that this article discusses are manufactured by companies that are independent of Microsoft.</span></span> <span data-ttu-id="c5830-141">A Microsoft nem vállal, vélelmezett vagy hello teljesítménye és megbízhatósága szempontjából ezeket a termékeket.</span><span class="sxs-lookup"><span data-stu-id="c5830-141">Microsoft makes no warranty, implied or otherwise, about hello performance or reliability of these products.</span></span>
 >
 >

### <a name="run-iperf-iperf3exe"></a><span data-ttu-id="c5830-142">Futtassa a iPerf (iperf3.exe)</span><span class="sxs-lookup"><span data-stu-id="c5830-142">Run iPerf (iperf3.exe)</span></span>
1. <span data-ttu-id="c5830-143">Engedélyezze az NSG/ACL-szabályának, hogy hello forgalmat (a nyilvános IP-cím tesztelés Azure virtuális gépen).</span><span class="sxs-lookup"><span data-stu-id="c5830-143">Enable an NSG/ACL rule allowing hello traffic (for public IP address testing on Azure VM).</span></span>

2. <span data-ttu-id="c5830-144">Mindkét csomópontján olyan érvényes tűzfalkivétel port 5001 engedélyezése.</span><span class="sxs-lookup"><span data-stu-id="c5830-144">On both nodes, enable a firewall exception for port 5001.</span></span>

    <span data-ttu-id="c5830-145">**Windows:** Futtatás hello következő parancsot a rendszergazda:</span><span class="sxs-lookup"><span data-stu-id="c5830-145">**Windows:** Run hello following command as an administrator:</span></span>

    ```CMD
    netsh advfirewall firewall add rule name="Open Port 5001" dir=in action=allow protocol=TCP localport=5001
    ```

    <span data-ttu-id="c5830-146">tooremove hello szabály tesztelése során befejeződött, a parancs futtatásához:</span><span class="sxs-lookup"><span data-stu-id="c5830-146">tooremove hello rule when testing is complete, run this command:</span></span>

    ```CMD
    netsh advfirewall firewall delete rule name="Open Port 5001" protocol=TCP localport=5001
    ```
    </br>
    <span data-ttu-id="c5830-147">**Az Azure Linux:** Azure Linux képek megengedő tűzfalak vannak.</span><span class="sxs-lookup"><span data-stu-id="c5830-147">**Azure Linux:**  Azure Linux images have permissive firewalls.</span></span> <span data-ttu-id="c5830-148">Ha egy alkalmazás egy portot figyel, hello adatforgalom keresztül engedélyezett.</span><span class="sxs-lookup"><span data-stu-id="c5830-148">If there is an application listening on a port, hello traffic is allowed through.</span></span> <span data-ttu-id="c5830-149">Egyéni képek, védett esetleg explicit módon megnyitott portok.</span><span class="sxs-lookup"><span data-stu-id="c5830-149">Custom images that are secured may need ports opened explicitly.</span></span> <span data-ttu-id="c5830-150">Közös Linux operációsrendszer-réteg tűzfalak tartalmaznak `iptables`, `ufw`, vagy `firewalld`.</span><span class="sxs-lookup"><span data-stu-id="c5830-150">Common Linux OS-layer firewalls include `iptables`, `ufw`, or `firewalld`.</span></span>

3. <span data-ttu-id="c5830-151">Hello csomóponton könyvtárváltás toohello ahol iperf3.exe ki kell olvasni.</span><span class="sxs-lookup"><span data-stu-id="c5830-151">On hello server node, change toohello directory where iperf3.exe is extracted.</span></span> <span data-ttu-id="c5830-152">Majd iPerf kiszolgáló módban fut, és állítsa be toolisten 5001 porton hello a következő parancsokat:</span><span class="sxs-lookup"><span data-stu-id="c5830-152">Then run iPerf in server mode and set it toolisten on port 5001 as hello following commands:</span></span>

     ```CMD
     cd c:\iperf-3.1.2-win65

     iperf3.exe -s -p 5001
     ```

4. <span data-ttu-id="c5830-153">Hello ügyfél csomóponton, ahol iperf eszköz kibontott, és futtassa a következő parancs hello toohello directory módosítása:</span><span class="sxs-lookup"><span data-stu-id="c5830-153">On hello client node, change toohello directory where iperf tool is extracted and then run hello following command:</span></span>

    ```CMD
    iperf3.exe -c <IP of hello iperf Server> -t 30 -p 5001 -P 32
    ```

    <span data-ttu-id="c5830-154">hello ügyfél forgalom port 5001 toohello kiszolgálón van hogy 30 másodpercig.</span><span class="sxs-lookup"><span data-stu-id="c5830-154">hello client is inducing traffic on port 5001 toohello server for 30 seconds.</span></span> <span data-ttu-id="c5830-155">hello jelzőt "-P", amely azt jelzi, hogy 32 egyidejű kapcsolatok toohello csomópont használjuk.</span><span class="sxs-lookup"><span data-stu-id="c5830-155">hello flag '-P ' that indicates we are using 32 simultaneous connections toohello server node.</span></span>

    <span data-ttu-id="c5830-156">hello alábbi képernyőn látható ebben a példában hello kimenete:</span><span class="sxs-lookup"><span data-stu-id="c5830-156">hello following screen shows hello output from this example:</span></span>

    ![Kimenet](./media/vpn-gateway-validate-throughput-to-vnet/06theoutput.png)

5. <span data-ttu-id="c5830-158">Vizsgálati eredmények, futtassa a parancsot (nem kötelező) toopreserve hello:</span><span class="sxs-lookup"><span data-stu-id="c5830-158">(OPTIONAL) toopreserve hello testing results, run this command:</span></span>

    ```CMD
    iperf3.exe -c IPofTheServerToReach -t 30 -p 5001 -P 32  >> output.txt
    ```

6. <span data-ttu-id="c5830-159">Hello előző lépések végrehajtását követően hajtható végre hello fordított irányú hello szerepkörök ugyanazokat a lépéseket, így hello kiszolgáló-csomópont lesz hello ügyfél, és fordítva.</span><span class="sxs-lookup"><span data-stu-id="c5830-159">After completing hello previous steps, execute hello same steps with hello roles reversed, so that hello server node will now be hello client and vice-versa.</span></span>

## <a name="address-slow-file-copy-issues"></a><span data-ttu-id="c5830-160">Lassú fájl másolása problémák megoldása</span><span class="sxs-lookup"><span data-stu-id="c5830-160">Address slow file copy issues</span></span>
<span data-ttu-id="c5830-161">Problémákat tapasztalhat a másolás, ha a Windows Intézővel vagy áthúzza egy RDP-kapcsolaton keresztül lassú fájlt.</span><span class="sxs-lookup"><span data-stu-id="c5830-161">You may experience slow file coping when using Windows Explorer or dragging and dropping through an RDP session.</span></span> <span data-ttu-id="c5830-162">Ez a probléma általában nem megfelelő tooone vagy mindkét hello a következő tényezőket:</span><span class="sxs-lookup"><span data-stu-id="c5830-162">This problem is normally due tooone or both of hello following factors:</span></span>

- <span data-ttu-id="c5830-163">Fájl másolása alkalmazások, például a Windows Intézőt, és RDP, ne használja egyszerre használható szálak, amikor a fájlok másolása.</span><span class="sxs-lookup"><span data-stu-id="c5830-163">File copy applications, such as Windows Explorer and RDP, do not use multiple threads when copying files.</span></span> <span data-ttu-id="c5830-164">A jobb teljesítmény érdekében használjon egy többszálas fájl másolása alkalmazás például [Richcopy](https://technet.microsoft.com/en-us/magazine/2009.04.utilityspotlight.aspx) toocopy fájlok 16 vagy 32 szálak használatával.</span><span class="sxs-lookup"><span data-stu-id="c5830-164">For better performance, use a multi-threaded file copy application such as [Richcopy](https://technet.microsoft.com/en-us/magazine/2009.04.utilityspotlight.aspx) toocopy files by using 16 or 32 threads.</span></span> <span data-ttu-id="c5830-165">toochange hello szál száma fájlmásolási Richcopy, kattintson a **művelet** > **beállításai** > **fájlmásolás**.</span><span class="sxs-lookup"><span data-stu-id="c5830-165">toochange hello thread number for file copy in Richcopy, click **Action** > **Copy options** > **File copy**.</span></span><br><br><span data-ttu-id="c5830-166">
![Lassú fájl másolása problémák](./media/vpn-gateway-validate-throughput-to-vnet/Richcopy.png)</span><span class="sxs-lookup"><span data-stu-id="c5830-166">
![Slow file copy issues](./media/vpn-gateway-validate-throughput-to-vnet/Richcopy.png)</span></span><br>
- <span data-ttu-id="c5830-167">Nincs elegendő méretű lemez olvasási/írási sebessége.</span><span class="sxs-lookup"><span data-stu-id="c5830-167">Insufficient VM disk read/write speed.</span></span> <span data-ttu-id="c5830-168">További információkért lásd: [Azure Storage hibaelhárítása](../storage/common/storage-e2e-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="c5830-168">For more information, see [Azure Storage Troubleshooting](../storage/common/storage-e2e-troubleshooting.md).</span></span>

## <a name="on-premises-device-external-facing-interface"></a><span data-ttu-id="c5830-169">A helyszíni eszközök külső irányuló felület</span><span class="sxs-lookup"><span data-stu-id="c5830-169">On-premises device external facing interface</span></span>
<span data-ttu-id="c5830-170">Ha hello a helyszíni VPN-eszköz Internet felé néző IP-cím szerepel hello [helyi hálózati](vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) definition az Azure-ban, nem tudja toobring hello VPN, szórványos bontja a kapcsolatot, vagy a teljesítményproblémák léphetnek.</span><span class="sxs-lookup"><span data-stu-id="c5830-170">If hello on-premises VPN device Internet-facing IP address is included in hello [local network](vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) definition in Azure, you may experience inability toobring up hello VPN, sporadic disconnects, or performance issues.</span></span>

## <a name="checking-latency"></a><span data-ttu-id="c5830-171">Késés ellenőrzése</span><span class="sxs-lookup"><span data-stu-id="c5830-171">Checking latency</span></span>
<span data-ttu-id="c5830-172">Tracert tootrace tooMicrosoft Azure peremhálózati eszköz toodetermine használja, ha bármely meghaladja a 100 ms közötti ugrások késések fordulnak elő.</span><span class="sxs-lookup"><span data-stu-id="c5830-172">Use tracert tootrace tooMicrosoft Azure Edge device toodetermine if there are any delays exceeding 100 ms between hops.</span></span>

<span data-ttu-id="c5830-173">A helyszíni hálózat hello, futtassa a *tracert* toohello VIP hello Azure átjáró vagy a virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="c5830-173">From hello on-premises network, run *tracert* toohello VIP of hello Azure Gateway or VM.</span></span> <span data-ttu-id="c5830-174">Miután látja csak * lett visszaadva, akkor tudja hello Azure peremhálózati elérte.</span><span class="sxs-lookup"><span data-stu-id="c5830-174">Once you see only * returned, you know you have reached hello Azure edge.</span></span> <span data-ttu-id="c5830-175">Amikor megjelenik, amely tartalmazza az adott vissza "MSN" DNS-nevek, ismernie elérte hello Microsoft gerincét.</span><span class="sxs-lookup"><span data-stu-id="c5830-175">When you see DNS names that include "MSN" returned, you know you have reached hello Microsoft backbone.</span></span><br><br><span data-ttu-id="c5830-176">
![Késés ellenőrzése](./media/vpn-gateway-validate-throughput-to-vnet/08checkinglatency.png)</span><span class="sxs-lookup"><span data-stu-id="c5830-176">
![Checking Latency](./media/vpn-gateway-validate-throughput-to-vnet/08checkinglatency.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="c5830-177">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c5830-177">Next steps</span></span>
<span data-ttu-id="c5830-178">További információt vagy a help tekintse meg a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="c5830-178">For more information or help, check out hello following links:</span></span>

- [<span data-ttu-id="c5830-179">Az Azure virtuális gépek hálózati teljesítmény optimalizálása</span><span class="sxs-lookup"><span data-stu-id="c5830-179">Optimize network throughput for Azure virtual machines</span></span>](../virtual-network/virtual-network-optimize-network-bandwidth.md)
- [<span data-ttu-id="c5830-180">A Microsoft támogatás</span><span class="sxs-lookup"><span data-stu-id="c5830-180">Microsoft Support</span></span>](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
