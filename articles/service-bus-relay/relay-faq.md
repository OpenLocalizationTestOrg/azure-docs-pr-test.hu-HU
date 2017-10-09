---
title: "aaaAzure továbbító – gyakori kérdések |} Microsoft Docs"
description: "Azure-továbbítási kapcsolatos gyakori kérdések válaszok toosome beolvasása."
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 886d2c7f-838f-4938-bd23-466662fb1c8e
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: ab14431e27df43287940e7d12ea37e4093638d21
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-relay-faqs"></a>Az Azure továbbító – gyakori kérdések

Ebben a cikkben megválaszolunk néhány gyakran ismételt kérdések (GYIK) kapcsolatos [Azure továbbítási](https://azure.microsoft.com/services/service-bus/). Általános Azure tarifa- és támogatási információk: [Azure támogatja – gyakori kérdések](http://go.microsoft.com/fwlink/?LinkID=185083).

## <a name="general-questions"></a>Általános kérdések
### <a name="what-is-azure-relay"></a>Mi az az Azure Relay?
Hello [Azure továbbítási szolgáltatás](relay-what-is-it.md) segíti a hibrid alkalmazásait segítenek biztonságosabban teszi közzé a szolgáltatásokhoz, amelyek a vállalati hálózati toohello nyilvános felhők belül találhatók. Hello szolgáltatások is elérhetővé teheti, egy tűzfalkapcsolatot megnyitása nélkül, és anélkül, hogy zavaró módosításokat tooa vállalati hálózati infrastruktúrában.

### <a name="what-is-a-relay-namespace"></a>Mi az a továbbítási névtér?
A [névtér](relay-create-namespace-portal.md) egy hatókörkezelési tárolót használható tooaddress továbbítási források az alkalmazáson belül van. Létre kell hoznia egy névtér toouse továbbító. Ez az első lépések hello első lépések egyikét.

### <a name="what-happened-tooservice-bus-relay-service"></a>Milyen happened tooService service Bus Relay?
korábban a Service Bus Relay szolgáltatás nevű hello WCF továbbító neve. Folytatás toouse ezt a szolgáltatást a szokásos módon. hello hibrid kapcsolatok szolgáltatása egy olyan szolgáltatás, amely az Azure BizTalk szolgáltatások van lett visszaültetett frissített verzióját. WCF továbbító és a hibrid kapcsolatok mindkét továbbra is támogatott toobe.

## <a name="pricing"></a>Díjszabás
Ez a szakasz néhány gyakori kérdések választ ad arra hello struktúra árképzési továbbító. Is látható [Azure támogatás – gyakori kérdések](http://go.microsoft.com/fwlink/?LinkID=185083) általános Azure-beli árakról. A továbbító árazással kapcsolatos részletes információkért lásd: [díjszabása Service Bus][Pricing overview].

### <a name="how-do-you-charge-for-hybrid-connections-and-wcf-relay"></a>Hogyan tegye meg a díjat hibrid kapcsolatok és WCF továbbító?
A továbbító árazással kapcsolatos részletes információkért lásd: hello [hibrid kapcsolatok és WCF-továbbítók] [ Pricing overview] hello Service Bus árképzési részleteit megjelenítő oldalon a táblában. Ezenkívül toohello árak megjegyezni, hogy az oldalon van szó, a kimenő forgalom kívül hello datacenter, amelyben az alkalmazás ki van építve a kapcsolódó adatátvitel.

### <a name="how-am-i-billed-for-hybrid-connections"></a>Hogyan vagyok számlázása a hibrid kapcsolatok?
Az alábbiakban a három számlázási példaforgatókönyvek hibrid kapcsolatok:

*   1. forgatókönyv:
    *   Lehetősége van egy figyelő, például a hibrid kapcsolatok Manager telepítve, és folyamatosan futó hello teljes hónap hello példánya.
    *   Hello hónapban hello kapcsolaton keresztül küldött adatok 3 GB. 
    *   A teljes költség: $5.
*   2. forgatókönyv:
    *   Lehetősége van egy figyelő, például a hibrid kapcsolatok Manager telepítve, és folyamatosan futó hello teljes hónap hello példánya.
    *   10 GB adatot küld hello hónapban hello kapcsolaton keresztül.
    *   A teljes költség $7.50. $5 hello kapcsolat, amelyek első 5 GB + $2.50 a hello további 5 GB adatot.
*   3. forgatókönyv:
    *   Két olyan példányt, A és B, a hibrid kapcsolatok Manager telepítve, és folyamatosan futó hello teljes hónap hello rendelkezik.
    *   Hello hónapban A kapcsolaton keresztül küldött adatok 3 GB.
    *   6 GB adatot küld hello hónapban B kapcsolaton keresztül.
    *   A teljes költség $10,50. Ez a kapcsolat A $5 + $5 kapcsolat B + 0,50 $ (a B kapcsolat hatodik gigabájt hello).

Vegye figyelembe, hogy hello hello a példákban szereplő árak a megfelelő csak hello hibrid kapcsolatok próbaidőszak alatt. Árak a tulajdonos toochange általánosan rendelkezésre álló hibrid kapcsolatok esetén.

### <a name="how-are-hours-calculated-for-relay"></a>Hogyan kiszámítása a továbbítási óra?

WCF továbbító csak a Standard csomag névterek érhető el. Tarifacsomag és [kapcsolat kvóták](../service-bus-messaging/service-bus-quotas.md) a továbbítók más módon nem változtak. Ez azt jelenti, hogy továbbítók toobe felszámított üzenetek (nem műveletek) hello száma alapján folytatni óra továbbítják. További információkért lásd: hello ["Hibrid kapcsolatok és WCF továbbítók"](https://azure.microsoft.com/pricing/details/service-bus/) hello díjszabás részleteit megjelenítő oldalon a táblában.

### <a name="what-if-i-have-more-than-one-listener-connected-tooa-specific-relay"></a>Mi történik, ha egynél több figyelő csatlakoztatott tooa megtalálhatják van?
Bizonyos esetekben egyetlen továbbítóként rendelkezhet több kapcsolódó figyelők. A továbbító nyissa meg tekintendő, ha legalább egy továbbító figyelő csatlakoztatott tooit. Figyelők tooan nyitott továbbító eredmények hozzáadása a további továbbítási óra. hello számú továbbító feladók (invoke-ügyfelek vagy küldési üzenetek toorelays), amelyek csatlakoztatott tooa továbbító nem érinti a továbbítási óra hello kiszámítása.

### <a name="how-is-hello-messages-meter-calculated-for-wcf-relays"></a>Hogyan kiszámítása a WCF-továbbítók hello üzenetek mérő?
(**Ez csak tooWCF továbbítók vonatkozik. Üzenetek csak egy hibrid kapcsolatok költséget.** )

Általában számlázható üzenetek a továbbítók segítségével számított hello azonos metódus használt közvetítőalapú entitásokat (üzenetsorok, témakörök és előfizetések), a fentiekben ismertetett. Van azonban néhány fontos különbség.

Service Bus relay üzenet tooa küld a rendszer kezeli a "teljes keresztül" toohello továbbítási figyelő, amely megkapja a hello üzenet küldése. A küldési művelet toohello Service Bus-továbbító, egy kézbesítési toohello továbbítási figyelő követi nem minősül. Egy kérelem-válasz stílusú service meghíváshoz (a akár too64 KB) két számlázható üzenetek a továbbítási figyelő eredmény elleni: hello kérés-és egy számlázható hello válasz egy számlázható üzenet (feltéve hello válasz is 64 KB-os vagy kisebb). Ez eltér attól az esettől egy várólista toomediate egy ügyfél és a szolgáltatás között. Ha egy ügyfél és a szolgáltatás közötti várólista toomediate használja, hello azonos kérelem-válasz a kialakítás megköveteli egy küldési toohello kérésvárólistára, hello várólista toohello szolgáltatásból egy dequeue/kézbesítési követ. Ez követi egy küldési tooanother válaszvárólistáját, és egy dequeue/kézbesítési adott várólista toohello ügyfélről. Hello azonos méretezés feltételezéseket egész (felfelé too64 KB) használ, a hello várólista mintát eredményez, 4 számlázható üzenet által közvetített. Lesz számlázva, üzenetek tooimplement hello azonos mintát megvalósításához relay használatával kétszer hello száma. Természetesen vannak előnyei toousing várólisták tooachieve ebben a mintában, például a tartósság és a betöltés simítás. Ilyen előnyt előfordulhat, hogy indokolják hello további költség.

Hello segítségével megnyitott továbbítók **netTCPRelay** WCF kötés üzenetek kezelni, nem egyedi üzenetekként, de hello rendszeren keresztül áramló adatok adatfolyamként. A kötés használata esetén csak hello küldő és a figyelő belelátnak hello keretezési hello egyes üzenetek küldése és fogadása. A továbbítók hello használó **netTCPRelay** kötés, minden adatot a rendszer egy adatfolyam számlázható üzenetek kiszámításához. Ebben az esetben a Service Bus küldött, vagy minden egyes továbbítási 5 perces időközönként keresztül érkezett adatok teljes mennyiségének hello számítja ki. Ezt követően azt elosztja a teljes adatmennyiség 64 KB-os toodetermine hello az, hogy a továbbító számlázható üzenetek száma adott időszakra vonatkozóan.

## <a name="quotas"></a>Kvóták
| Kvóta neve | Hatókör | Típus | Viselkedés túllépésekor | Érték |
| --- | --- | --- | --- | --- |
| A továbbító egyidejű figyelőinek |Entitás |Statikus |További kapcsolatokhoz későbbi kérelmek azért lettek elutasítva, és kivételt érkezik hello kód hívása. |25 |
| Egyidejű továbbítóügynök-figyelői |Telepítenek |Statikus |További kapcsolatokhoz későbbi kérelmek azért lettek elutasítva, és kivételt érkezik hello kód hívása. |2,000 |
| A névtér összes továbbítási végpontok egyidejű továbbító-kapcsolatok |Telepítenek |Statikus |- |5,000 |
| Továbbítási végpontok száma a szolgáltatás névtere |Telepítenek |Statikus |- |10,000 |
| Üzenet mérete [NetOnewayRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.netonewayrelaybinding.aspx) és [NetEventRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.neteventrelaybinding.aspx) továbbítja |Telepítenek |Statikus |Meghaladnia ezek mely százalékértékénél kéri, hogy a bejövő üzenetek elutasították, és kivételt érkezik hello kód hívása. |64 KB |
| Üzenet mérete [HttpRelayTransportBindingElement](https://msdn.microsoft.com/library/microsoft.servicebus.httprelaytransportbindingelement.aspx) és [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) továbbítja |Telepítenek |Statikus |- |Korlátlan |

### <a name="does-relay-have-any-usage-quotas"></a>Rendelkezik a továbbítási bármely használati kvóták?
Alapértelmezés szerint az összes felhőalapú szolgáltatás a Microsoft összes egy ügyfél-előfizetések számított egy összesített havi memóriahasználati kvóta állítja be. Tudjuk, hogy időnként igényeinek előfordulhat, hogy meghaladja ezt a korlátot. Felveheti a kapcsolatot az ügyfélszolgálat bármikor, így azt igényeinek megismeréséhez és annak megfelelően módosítsa a működés felső korlátjának. Service Bus hello használati kvóták a következők:

* 5 milliárd üzenetek
* 2 millió továbbítási óra

Bár azt lefoglalni hello jobb toodisable, amelyek túllépik a havi használati kvóták fiók, nyújtunk e-mail értesítéseket, és több kísérletek toocontact hello ügyfél biztosítjuk bármely megtétele előtt. Ügyfelek, amelyek mérete meghaladja a ezek mely százalékértékénél kéri továbbra is felelőssége felesleges költségek.

### <a name="naming-restrictions"></a>Vonatkozó elnevezési korlátozás
A továbbító névtér nevét 6 és 50 karakter hosszúságúnak kell lennie.

## <a name="subscription-and-namespace-management"></a>Előfizetés és a névtér-kezelés
### <a name="how-do-i-migrate-a-namespace-tooanother-azure-subscription"></a>Hogyan át egy névtér tooanother Azure-előfizetés?

toomove a névtér egy Azure-előfizetés tooanother előfizetésből, vagy használjon hello is [Azure-portálon](https://portal.azure.com) vagy PowerShell-parancsokkal. egy névtér tooanother előfizetés toomove, hello névtér már aktívnak kell lennie. hello felhasználói hello parancsok futtatásával egy rendszergazda felhasználó mindkét hello forrása és célja előfizetések kell lennie.

#### <a name="azure-portal"></a>Azure Portal

Lásd az Azure portál toomigrate Azure továbbítási névterek egy előfizetés tooanother előfizetésből toouse hello [erőforrások tooa új erőforráscsoportba vagy előfizetésbe áthelyezése](../azure-resource-manager/resource-group-move-resources.md#use-portal). 

#### <a name="powershell"></a>PowerShell

toouse PowerShell toomove egy Azure-előfizetéssel tooanother előfizetés, a következő parancssorozat használata hello névtér. tooexecute Ez a művelet hello névtér már aktív kell lennie, és hello felhasználói hello PowerShell-parancsok futtatása egy rendszergazda felhasználó mindkét hello forrása és célja előfizetések kell lennie.

```powershell
# Create a new resource group in hello target subscription.
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move hello namespace from hello source subscription toohello target subscription.
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="troubleshooting"></a>Hibaelhárítás
### <a name="what-are-some-of-hello-exceptions-generated-by-azure-relay-apis-and-suggested-actions-you-can-take"></a>Melyek azok hello kivételek Azure továbbítási API-k által létrehozott, és a javasolt elvégezhető műveletekhez nyújtanak?
A közös kivételeket és a javasolt műveletek is igénybe vehet, olvassa el [kivételek továbbítása][Relay exceptions].

### <a name="what-is-a-shared-access-signature-and-which-languages-can-i-use-toogenerate-a-signature"></a>Mi az a közös hozzáférésű jogosultságkód, és milyen nyelveket használhatok toogenerate aláírás?
Megosztott hozzáférési aláírásokkal (SAS) olyan hitelesítési mechanizmus biztonságos SHA-256 kivonatokkal vagy URI-k alapján. Hogyan toogenerate csomópont, PHP, Java, C és C#, a saját aláírások: információ [Service Bus hitelesítési megosztott hozzáférési aláírásokkal][Shared Access Signatures].

### <a name="is-it-possible-toowhitelist-relay-endpoints"></a>Ennyi az egész lehetséges toowhitelist továbbítási végpontok?
Igen. hello továbbítási ügyfél toohello Azure továbbítási szolgáltatás teszi kapcsolatok teljes tartománynevek használatával. Az ügyfelek is hozzáadhat egy bejegyzést a `*.servicebus.windows.net` a tűzfalak, amely támogatja a DNS engedélyezett.

## <a name="next-steps"></a>Következő lépések
* [Névtér létrehozása](relay-create-namespace-portal.md)
* [Ismerkedés a .NET-tel](relay-hybrid-connections-dotnet-get-started.md)
* [Bevezetés a Node használatába](relay-hybrid-connections-node-get-started.md)

[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Relay exceptions]: relay-exceptions.md
[Shared access signatures]: ../service-bus-messaging/service-bus-sas.md