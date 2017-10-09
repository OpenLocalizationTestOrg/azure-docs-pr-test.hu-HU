---
title: "hibaelhárítás a Azure hálózati figyelőt aaaIntroduction tooresource |} Microsoft Docs"
description: "Ezen a lapon hello hálózati figyelőt erőforrás hibaelhárítási képességei áttekintése"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: c1145cd6-d1cf-4770-b1cc-eaf0464cc315
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 06/19/2017
ms.author: gwallace
ms.openlocfilehash: ccbe4c1c2364473aba06e709460d67c773cf25ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-tooresource-troubleshooting-in-azure-network-watcher"></a>Hibaelhárítás a Azure hálózati figyelőt bemutatása tooresource

Virtuális hálózati átjárók biztosít a helyi erőforrások és más Azure-ban virtuális hálózatok közötti kapcsolatot. Ezek átjárók és a kapcsolatok figyelés létfontosságú tooensuring kommunikáció nem megszakad. Hálózati figyelőt hello funkció tootroubleshoot biztosít a virtuális hálózati átjárók és kapcsolatok. Ez hello portál, a PowerShell, a CLI vagy a REST API-n keresztül kell meghívni. Meghívásakor, a hálózati figyelőt diagnosztizálja a virtuális hálózati átjáró hello vagy a kapcsolat és a visszatérési hello megfelelő eredmények hello állapotának. A kérelem egy hosszú ideig futó tranzakció, hello eredményeinek hello diagnosztikai végrehajtása után.

![portal][2]

## <a name="results"></a>Results (Eredmények)

hello előzetes eredményének hello rendszerállapot hello erőforrás átfogó képet ad. Mélyebb információk biztosítható, hogy az erőforrások hello a következő szakaszban ismertetett módon:

hello alábbi lista a következő hello értékeket ad vissza, amelyben hello API hibáinak elhárítása:

* **startTime** -ezt az értéket az hello idő hello hibaelhárítása API-hívások indítása.
* **endTime** -ezt az értéket az hello idő, amikor hello hibaelhárítás befejeződött.
* **kód** -ezt az értéket nem kifogástalan állapotú, akkor, ha sikertelen egy egyetlen elemzés céljából.
* **eredmények** -eredmények hello kapcsolat vagy hello virtuális hálózati átjáró visszaadott eredmények gyűjteménye.
    * **azonosító** -hello hibatípus értéke.
    * **összegző** -e értéke hello tartalék összegzését.
    * **részletes** -ezt az értéket hello hiba részletes leírását tartalmazza.
    * **recommendedActions** – Ez a tulajdonság műveleteket tootake gyűjteménye.
      * **actionText** -Ez az érték milyen művelet tootake leíró hello szöveget tartalmaz.
      * **actionUri** -Ez az érték szolgál az hello URI toodocumentation tooact.
      * **actionUriText** -e értéke hello művelet szöveg rövid leírása.

hello a következő táblák megjelenítése hello különböző tartalék típusok (azonosító: hello listát megelőző eredményei alapján), és ha hello hibát hoz létre a naplókat.

### <a name="gateway"></a>Átjáró

| Hibatípus | Ok | Napló|
|---|---|---|
| NoFault | Ha nincs hiba észlelhető. |Igen|
| GatewayNotFound | Nem található átjáró vagy az átjáró nem lett beállítva. |Nem|
| PlannedMaintenance |  Átjárópéldány karbantartás alatt áll.  |Nem|
| UserDrivenUpdate | Amikor a felhasználó frissítése folyamatban van. Ennek oka lehet egy átméretezés. | Nem |
| VipUnResponsive | Hello elsődleges hello átjáró példánya nem érhető el. Ez akkor fordul elő, amikor hello állapotmintáihoz sikertelen. | Nem |
| PlatformInActive | A hello platformmal probléma van. | Nem|
| ServiceNotRunning | hello alapul szolgáló szolgáltatás nem fut. | Nem|
| NoConnectionsFoundForGateway | Hello átjáró kapcsolat nem létezik. Ez a figyelmeztetés csak.| Nem|
| ConnectionsNotConnected | Kapcsolatok nem kapcsolódnak. Ez a figyelmeztetés csak.| Igen|
| GatewayCPUUsageExceeded | hello aktuális átjáró CPU-használat > 95 %. | Igen |

### <a name="connection"></a>Kapcsolat

| Hibatípus | Ok | Napló|
|---|---|---|
| NoFault | Ha nincs hiba észlelhető. |Igen|
| GatewayNotFound | Nem található átjáró vagy az átjáró nem lett beállítva. |Nem|
| PlannedMaintenance | Átjárópéldány karbantartás alatt áll.  |Nem|
| UserDrivenUpdate | Amikor a felhasználó frissítése folyamatban van. Ennek oka lehet egy átméretezés.  | Nem |
| VipUnResponsive | Hello elsődleges hello átjáró példánya nem érhető el. Akkor történik, ha hello állapotmintáihoz sikertelen lesz. | Nem |
| ConnectionEntityNotFound | Kapcsolat konfigurációja hiányzik. | Nem |
| ConnectionIsMarkedDisconnected | hello kapcsolat "leválasztott" van megjelölve. |Nem|
| ConnectionNotConfiguredOnGateway | hello mögöttes szolgáltatás nincs kapcsolat konfigurálva hello. | Igen |
| ConnectionMarkedStandy | az alapul szolgáló szolgáltatás hello készenléti jelölésű.| Igen|
| Authentication | Előmegosztott kulcs nem megfelelő. | Igen|
| PeerReachability | hello társ átjáró nem érhető el. | Igen|
| IkePolicyMismatch | hello társ átjáró rendelkezik IKE szabályzatok Azure által nem támogatott. | Igen|
| WfpParse hiba | Hiba történt az elemzési hello WFP napló. |Igen|

## <a name="supported-gateway-types"></a>Támogatott átjáró típusok

hello alábbi lista mutatja azokat hello támogatási jeleníti meg, melyik átjárót és a kapcsolatok támogatottak a hibaelhárításban hálózati figyelőt.
|  |  |
|---------|---------|
|**Átjáró típusa**   |         |
|VPN      | Támogatott        |
|ExpressRoute | Nem támogatott |
|Hypernet | Nem támogatott|
|**VPN-típusai** | |
|Útvonal alapján | Támogatott|
|Csoportházirend-alapú | Nem támogatott|
|**Kapcsolat típusai**||
|IPSec| Támogatott|
|VNet2Vnet| Támogatott|
|ExpressRoute| Nem támogatott|
|Hypernet| Nem támogatott|
|VPNClient| Nem támogatott|

## <a name="log-files"></a>Naplófájlok

hello erőforrás hibaelhárítás naplófájljai vannak tárolva egy tárfiókot, erőforrás hibakeresési befejeződése után. hello következő kép bemutatja, hibát eredményezett hívás hello példa tartalmát.

![zip-fájl][1]

> [!NOTE]
> Bizonyos esetekben csak egy részhalmazát hello naplófájlokat toostorage írása.

A fájlok letöltését az azure storage-fiókok útmutatásért tekintse meg túl[az Azure Blob storage .NET használatának első lépései](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Egy másik eszköz, amely használható a Tártallózó. Tártallózó további információt itt található: a következő hivatkozás hello: [Tártallózó](http://storageexplorer.com/)

### <a name="connectionstatstxt"></a>ConnectionStats.txt

Hello **ConnectionStats.txt** fájl hello kapcsolat, beleértve a bemenő és kimenő bájtokat, a kapcsolat állapota és a hello idő hello kapcsolat jött létre az összesített statisztikák tartalmaz.

> [!NOTE]
> Hello hívás toohello API hibaelhárítási kifogástalan adja vissza, ha van-e a visszaadott hello zip-fájlban szereplő hello egyedül a **ConnectionStats.txt** fájlt.

hello a fájl tartalma a következő példa hasonló toohello:

```
Connectivity State : Connected
Remote Tunnel Endpoint :
Ingress Bytes (since last connected) : 288 B
Egress Bytes (Since last connected) : 288 B
Connected Since : 2/1/2017 8:22:06 PM
```

### <a name="cpustatstxt"></a>CPUStats.txt

Hello **CPUStats.txt** CPU-használat és a teszteléshez hello időpontjában elérhető memória-fájl tartalmazza.  a fájl hello tartalmát a következő példa hasonló toohello:

```
Current CPU Usage : 0 % Current Memory Available : 641 MBs
```

### <a name="ikeerrorstxt"></a>IKEErrors.txt

Hello **IKEErrors.txt** fájl bármely ellenőrzése során talált IKE hibákat tartalmazza.

hello következő példa bemutatja egy IKEErrors.txt fájl hello tartalmát. A hibák hello probléma függően eltérő lehet.

```
Error: Authentication failed. Check shared key. Check crypto. Check lifetimes. 
     based on log : Peer failed with Windows error 13801(ERROR_IPSEC_IKE_AUTH_FAIL)
Error: On-prem device sent invalid payload. 
     based on log : IkeFindPayloadInPacket failed with Windows error 13843(ERROR_IPSEC_IKE_INVALID_PAYLOAD)
```

### <a name="scrubbed-wfpdiagtxt"></a>Törlődik wfpdiag.txt

Hello **Scrubbed-wfpdiag.txt** naplófájl hello wfp naplófájl tartalmazza. Ez a napló tartalmaz csomagjai és IKE/AuthIP hibák naplózása.

hello következő példa bemutatja hello hello Scrubbed-wfpdiag.txt fájl tartalmát. Ebben a példában az hello megosztott kulcs kapcsolat nem volt pontosak, mivel a 3. sorából hello hello aljáról látható. a következő példa hello csak hello teljes napló, a részlet esetén, mert a hello napló megnőhet, attól függően, hogy hello probléma.

```
...
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Deleted ICookie from hello high priority thread pool list
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|IKE diagnostic event:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Event Header:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Timestamp: 1601-01-01T00:00:00.000Z
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Flags: 0x00000106
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Local address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    Remote address field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IP version field set
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP version: IPv4
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  IP protocol: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local address: 13.78.238.92
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote address: 52.161.24.36
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Local Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Remote Port: 0
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Application ID:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  User SID: <invalid>
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Failure type: IKE/Authip Main Mode Failure
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|Type specific info:
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure error code:0x000035e9
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|    IKE authentication credentials are unacceptable
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|
[0]0368.03A4::02/02/2017-17:36:01.496 [ikeext] 3038|52.161.24.36|  Failure point: Remote
...
```

### <a name="wfpdiagtxtsum"></a>wfpdiag.txt.Sum

Hello **wfpdiag.txt.sum** fájl a napló hello buffers és a feldolgozott események megjelenítése.

hello következő példa egy hello wfpdiag.txt.sum fájl tartalmát.
```
Files Processed:
    C:\Resources\directory\924336c47dd045d5a246c349b8ae57f2.GatewayTenantWorker.DiagnosticsStorage\2017-02-02T17-34-23\wfpdiag.etl
Total Buffers Processed 8
Total Events  Processed 2169
Total Events  Lost      0
Total Format  Errors    0
Total Formats Unknown   486
Elapsed Time            330 sec
+-----------------------------------------------------------------------------------+
|EventCount    EventName            EventType   TMF                                 |
+-----------------------------------------------------------------------------------+
|        36    ikeext               ike_addr_utils_c844  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        12    ikeext               ike_addr_utils_c857  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|        96    ikeext               ike_addr_utils_c832  a0c064ca-d954-350a-8b2f-1a7464eef8b6|
|         6    ikeext               ike_bfe_callbacks_c133  1dc2d67f-8381-6303-e314-6c1452eeb529|
|         6    ikeext               ike_bfe_callbacks_c61  1dc2d67f-8381-6303-e314-6c1452eeb529|
|        12    ikeext               ike_sa_management_c5698  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c8447  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c494  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c642  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|         6    ikeext               ike_sa_management_c3162  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
|        12    ikeext               ike_sa_management_c3307  7857a320-42ee-6e90-d5d9-3f414e3ea2d3|
```

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan toodiagnose VPN-átjárók és kapcsolatok keresztül hello portal ellátogatva [átjáró hibaelhárítás – Azure-portálon](network-watcher-troubleshoot-manage-portal.md).
<!--Image references-->

[1]: ./media/network-watcher-troubleshoot-overview/GatewayTenantWorkerLogs.png
[2]: ./media/network-watcher-troubleshoot-overview/portal.png
