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
# <a name="how-toovalidate-vpn-throughput-tooa-virtual-network"></a>Hogyan toovalidate VPN átviteli tooa virtuális hálózat

VPN gateway-kapcsolattal lehetővé teszi tooestablish biztonságos, létesítmények közötti közötti kapcsolatot a virtuális hálózat az Azure és a helyszíni informatikai infrastruktúrát.

Ez a cikk bemutatja, hogyan toovalidate hálózati teljesítményt hello a helyszíni erőforrások tooan Azure virtuális géphez. Azt is nyújt hibaelhárítási útmutatót.

>[!NOTE]
>Ez a cikk toohelp diagnosztizálhatja és gyakori problémák megoldásával kapcsolatban. Ha még nem toosolve hello probléma a következő információ hello segítségével [forduljon a támogatási szolgálathoz](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).
>
>

## <a name="overview"></a>Áttekintés

hello VPN gateway-kapcsolatot a következő összetevők hello foglal magában:

- A helyszíni VPN-eszköz (listájának megtekintéséhez [érvényesíteni a VPN-eszközök)](vpn-gateway-about-vpn-devices.md#devicetable).
- Nyilvános Internet
- Az Azure VPN gateway
- Azure-os virtuális gép

a következő diagram hello hello logikai kapcsolatát egy a helyszíni hálózati tooan Azure virtuális hálózat VPN-en keresztül jeleníti meg.

![Logikai ügyfél hálózati kapcsolat tooMSFT hálózati VPN-nel](./media/vpn-gateway-validate-throughput-to-vnet/VPNPerf.png)

## <a name="calculate-hello-maximum-expected-ingressegress"></a>Hello maximális várt be-és kilépési kiszámítása

1.  Az alkalmazás eredeti átviteli követelmények meghatározása.
2.  Az Azure VPN gateway átviteli sebességének korlátai határozza meg. Útmutatásért lásd: hello "összesített átviteli SKU- és VPN-típus szerint" szakasza [tervezése és kialakítása VPN-átjáró](vpn-gateway-plan-design.md).
3.  Határozza meg a hello [Azure virtuális gép átviteli útmutatást](../virtual-machines/virtual-machines-windows-sizes.md) a Virtuálisgép-mérethez.
4.  Határozza meg, hogy az internetszolgáltató (ISP) sávszélességet.
5.  A várt átviteli - (VM-átjárón internetszolgáltató) minimális sávszélesség kiszámításához * 0,8 értéket.

Ha a számított teljesítményt nem felel meg az alkalmazás eredeti átviteli követelményeinek, akkor tooincrease hello sávszélesség hello szűk azonosított hello erőforrás. Lásd az Azure VPN Gateway tooresize [módosítása egy átjáró-Termékváltozat](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku). a virtuális gép tooresize lásd: [méretezze át a virtuális gépek](../virtual-machines/virtual-machines-windows-resize-vm.md). Ha nem várt internetes sávszélességet, is érdemes lehet toocontact az Internetszolgáltató.

## <a name="validate-network-throughput-by-using-performance-tools"></a>Hálózati átviteli teljesítmény eszközökkel ellenőrzése

Az ellenőrzés során nem csúcsidőre kell hajtható végre, mert VPN alagút átviteli telítettségét tesztelés során nem ad pontos eredményeket.

a teszteléshez használjuk hello eszköz iPerf, amely Windows és Linux rendszeren működik és ügyfél- és módok. Az Windows alapú virtuális gépek korlátozott too3 GB/s.

Ez az eszköz nem végez semmilyen olvasási/írási műveletek toodisk. Kizárólag a saját TCP-forgalom a egy záró toohello más hoz létre. Az előállított statisztikát kísérletezhet, és mérheti hello sávszélesség álljon rendelkezésre, ügyfél és kiszolgáló-csomópont között. Ha két csomópont között, egy hello kiszolgálóval és-ügyfélként hello működik. Ha ez a vizsgálat befejeződött, azt javasoljuk, hogy megfordította hello szerepkörök tootest egyaránt le- és feltöltése mindkét csomópont átviteli sebességet.

### <a name="download-iperf"></a>Töltse le a iPerf
Töltse le [iPerf](https://iperf.fr/download/iperf_3.1/iperf-3.1.2-win64.zip). További információkért lásd: [iPerf dokumentáció](https://iperf.fr/iperf-doc.php).

 >[!NOTE]
 >hello külső cikkben említett termékeket független, a Microsoft által. A Microsoft nem vállal, vélelmezett vagy hello teljesítménye és megbízhatósága szempontjából ezeket a termékeket.
 >
 >

### <a name="run-iperf-iperf3exe"></a>Futtassa a iPerf (iperf3.exe)
1. Engedélyezze az NSG/ACL-szabályának, hogy hello forgalmat (a nyilvános IP-cím tesztelés Azure virtuális gépen).

2. Mindkét csomópontján olyan érvényes tűzfalkivétel port 5001 engedélyezése.

    **Windows:** Futtatás hello következő parancsot a rendszergazda:

    ```CMD
    netsh advfirewall firewall add rule name="Open Port 5001" dir=in action=allow protocol=TCP localport=5001
    ```

    tooremove hello szabály tesztelése során befejeződött, a parancs futtatásához:

    ```CMD
    netsh advfirewall firewall delete rule name="Open Port 5001" protocol=TCP localport=5001
    ```
    </br>
    **Az Azure Linux:** Azure Linux képek megengedő tűzfalak vannak. Ha egy alkalmazás egy portot figyel, hello adatforgalom keresztül engedélyezett. Egyéni képek, védett esetleg explicit módon megnyitott portok. Közös Linux operációsrendszer-réteg tűzfalak tartalmaznak `iptables`, `ufw`, vagy `firewalld`.

3. Hello csomóponton könyvtárváltás toohello ahol iperf3.exe ki kell olvasni. Majd iPerf kiszolgáló módban fut, és állítsa be toolisten 5001 porton hello a következő parancsokat:

     ```CMD
     cd c:\iperf-3.1.2-win65

     iperf3.exe -s -p 5001
     ```

4. Hello ügyfél csomóponton, ahol iperf eszköz kibontott, és futtassa a következő parancs hello toohello directory módosítása:

    ```CMD
    iperf3.exe -c <IP of hello iperf Server> -t 30 -p 5001 -P 32
    ```

    hello ügyfél forgalom port 5001 toohello kiszolgálón van hogy 30 másodpercig. hello jelzőt "-P", amely azt jelzi, hogy 32 egyidejű kapcsolatok toohello csomópont használjuk.

    hello alábbi képernyőn látható ebben a példában hello kimenete:

    ![Kimenet](./media/vpn-gateway-validate-throughput-to-vnet/06theoutput.png)

5. Vizsgálati eredmények, futtassa a parancsot (nem kötelező) toopreserve hello:

    ```CMD
    iperf3.exe -c IPofTheServerToReach -t 30 -p 5001 -P 32  >> output.txt
    ```

6. Hello előző lépések végrehajtását követően hajtható végre hello fordított irányú hello szerepkörök ugyanazokat a lépéseket, így hello kiszolgáló-csomópont lesz hello ügyfél, és fordítva.

## <a name="address-slow-file-copy-issues"></a>Lassú fájl másolása problémák megoldása
Problémákat tapasztalhat a másolás, ha a Windows Intézővel vagy áthúzza egy RDP-kapcsolaton keresztül lassú fájlt. Ez a probléma általában nem megfelelő tooone vagy mindkét hello a következő tényezőket:

- Fájl másolása alkalmazások, például a Windows Intézőt, és RDP, ne használja egyszerre használható szálak, amikor a fájlok másolása. A jobb teljesítmény érdekében használjon egy többszálas fájl másolása alkalmazás például [Richcopy](https://technet.microsoft.com/en-us/magazine/2009.04.utilityspotlight.aspx) toocopy fájlok 16 vagy 32 szálak használatával. toochange hello szál száma fájlmásolási Richcopy, kattintson a **művelet** > **beállításai** > **fájlmásolás**.<br><br>
![Lassú fájl másolása problémák](./media/vpn-gateway-validate-throughput-to-vnet/Richcopy.png)<br>
- Nincs elegendő méretű lemez olvasási/írási sebessége. További információkért lásd: [Azure Storage hibaelhárítása](../storage/common/storage-e2e-troubleshooting.md).

## <a name="on-premises-device-external-facing-interface"></a>A helyszíni eszközök külső irányuló felület
Ha hello a helyszíni VPN-eszköz Internet felé néző IP-cím szerepel hello [helyi hálózati](vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) definition az Azure-ban, nem tudja toobring hello VPN, szórványos bontja a kapcsolatot, vagy a teljesítményproblémák léphetnek.

## <a name="checking-latency"></a>Késés ellenőrzése
Tracert tootrace tooMicrosoft Azure peremhálózati eszköz toodetermine használja, ha bármely meghaladja a 100 ms közötti ugrások késések fordulnak elő.

A helyszíni hálózat hello, futtassa a *tracert* toohello VIP hello Azure átjáró vagy a virtuális gép. Miután látja csak * lett visszaadva, akkor tudja hello Azure peremhálózati elérte. Amikor megjelenik, amely tartalmazza az adott vissza "MSN" DNS-nevek, ismernie elérte hello Microsoft gerincét.<br><br>
![Késés ellenőrzése](./media/vpn-gateway-validate-throughput-to-vnet/08checkinglatency.png)

## <a name="next-steps"></a>Következő lépések
További információt vagy a help tekintse meg a következő hivatkozások hello:

- [Az Azure virtuális gépek hálózati teljesítmény optimalizálása](../virtual-network/virtual-network-optimize-network-bandwidth.md)
- [A Microsoft támogatás](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
