---
title: "Gyakori kérdések (GYIK) a Service Bus aaaAzure |} Microsoft Docs"
description: "Azure Service Bus kapcsolatban gyakran feltett kérdésekre adott válaszok."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: cc75786d-3448-4f79-9fec-eef56c0027ba
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/07/2017
ms.author: sethm
ms.openlocfilehash: 71fe9eac46647a3e4026dbcaf2196984dd4b6a44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="service-bus-faq"></a>Service Bus – GYIK
Ebben a cikkben megválaszolunk néhány kapcsolatos gyakori kérdések a Microsoft Azure Service Bus. Meglátogathatja hello [Azure támogatja – gyakori kérdések](http://go.microsoft.com/fwlink/?LinkID=185083) általános Azure tarifa- és támogatási információkat.

## <a name="general-questions-about-azure-service-bus"></a>Azure Service Bus kapcsolatos általános kérdésekre
### <a name="what-is-azure-service-bus"></a>Mi az Azure Service Bus?
[Az Azure Service Bus](service-bus-messaging-overview.md) egy aszinkron üzenetkezelési felhő platform, amely lehetővé teszi, hogy Ön toosend leválasztott rendszerek között. A Microsoft a szolgáltatás kínál szolgáltatásként, ami azt jelenti, hogy nem kell toohost valamelyik rendelés toouse a hardver saját azt.

### <a name="what-is-a-service-bus-namespace"></a>Mi az a Service Bus-névtér?
A [névtér](service-bus-create-namespace-portal.md) egy hatókörkezelési tárolót biztosít az alkalmazáson belül a Service Bus erőforrásainak kezeléséhez. Hozzon létre egyet szükséges toouse Service Bus, azaz hello egyik első lépések első lépéseket.

### <a name="what-is-an-azure-service-bus-queue"></a>Mi az az Azure Service Bus-üzenetsorba?
A [Service Bus-üzenetsorba](service-bus-queues-topics-subscriptions.md) az üzeneteket tároló entitás. A várólisták különösen hasznosak, ha több alkalmazást, vagy egymással toocommunicate igénylő elosztott alkalmazás több részét. hello várólista hasonló tooa kulcskiosztó központ abban, hogy több termékeket (üzenet) kapott, és elküldi a helyről.

### <a name="what-are-azure-service-bus-topics-and-subscriptions"></a>Mik az Azure Service Bus-üzenettémák és előfizetések?
A témakör is ábrázolt, várólista és használ több előfizetéssel, lesz egy gazdagabb üzenetkezelési modellt; lényegében egy-a-többhöz kommunikációs eszközt. Ez a közzétételi/előfizetési modell (vagy *pub/sub*) lehetővé teszi, hogy egy alkalmazást, amely egy üzenetet tooa témakör több előfizetések toohave, hogy több alkalmazás által fogadott üzenet.

### <a name="what-is-a-partitioned-entity"></a>Mi az a particionált entitás?
Hagyományos üzenetsor vagy témakör egyetlen üzenet ügynök által kezelt és egy üzenetküldési tárolóban tárolja. A [particionált üzenetsor vagy témakör](service-bus-partitioning.md) több üzenetet brókerek kezeli és több üzenetküldési tároló tárolja. Ez azt jelenti, hogy a teljes átviteli sebességgel particionált üzenetsor vagy témakör hello hello teljesítményét egy egyetlen üzenet broker vagy az üzenetküldési tárolóban már nem korlátozzák. Ezenkívül átmenetileg nem működik az üzenetküldési tárolóban nem képezhető le egy particionált üzenetsor vagy témakör nem érhető el.

Vegye figyelembe, hogy rendezés nem biztosított particionálási entitások használata esetén. Hello esemény, hogy a partíció nem érhető el továbbra is küldhet és üzenetek fogadása hello más partíciókkal.

## <a name="best-practices"></a>Ajánlott eljárások
### <a name="what-are-some-azure-service-bus-best-practices"></a>Mik az Azure Service Bus tanácsokat?
* [Ajánlott eljárások a teljesítménnyel kapcsolatos fejlesztések a Service Bus használatával] [ Best practices for performance improvements using Service Bus] – Ez a cikk ismerteti, hogyan toooptimize teljesítmény adatcsere üzenetek.

### <a name="what-should-i-know-before-creating-entities"></a>Mit kell tudnia entitások létrehozása előtt?
hello következő várólista és a témakör tulajdonságai nem módosíthatók. Hajtsa végre ezt figyelembe amikor az entitások, ez nem lehet módosítani, csere új entitás létrehozása nélkül.

* Méret
* Particionálás
* Munkamenetek
* Kettős észlelés
* Entitás Express

## <a name="pricing"></a>Díjszabás
Ez a szakasz hello Service Bus árképzési struktúra kapcsolatban gyakran feltett kérdésekre adott válaszok.

Hello [Service Bus árak és számlázás](service-bus-pricing-billing.md) cikk azt ismerteti, számlázási mérőszámok hello a Service Bus és információ a Service Bus árképzési beállítások című [díjszabása Service Bus](https://azure.microsoft.com/pricing/details/service-bus/).

Meglátogathatja hello [Azure támogatás – gyakori kérdések](http://go.microsoft.com/fwlink/?LinkID=185083) általános Azure-beli árakról. 

### <a name="how-do-you-charge-for-service-bus"></a>Hogyan tegye díjat számítanak, Service Bus?
A Service Bus árazással kapcsolatos részletes információkért lásd: [díjszabása Service Bus][Pricing overview]. Ezenkívül toohello árak nincs jelezve, a kimenő forgalom kívül, amelyben az alkalmazás ki van építve hello adatközpont társított adatátviteli van szó.

### <a name="what-usage-of-service-bus-is-subject-toodata-transfer-what-is-not"></a>A Service Bus milyen használatát az tulajdonos toodata átviteli? Mi az a nem?
Bármely adatátvitel belül egy adott Azure-régió, díjmentesen, valamint a bejövő adatforgalom valósul meg. Adatátvitel terület kívül tulajdonos tooegress díjak található [Itt](https://azure.microsoft.com/pricing/details/bandwidth/).

### <a name="does-service-bus-charge-for-storage"></a>A Service Bus a tároló nem díjat?
Nem, a Service Bus nem terheli a tároláshoz. Van azonban egy kvótát korlátozó hello adatok maximális mérete / várólista/témakör fennállásának. Lásd: hello következő gyakran ismételt kérdések.

## <a name="quotas"></a>Kvóták

A Service Bus-korlátozások és a kvóták listájáért lásd: hello [Service Bus kvóták áttekintése][Quotas overview].

### <a name="does-service-bus-have-any-usage-quotas"></a>Rendelkezik a Service Bus bármely használati kvóták?
Alapértelmezés szerint minden felhő szolgáltatás Microsoft állítja be az összes olyan ügyfél-előfizetések számított egy összesített havi memóriahasználati kvóta. Mert tudjuk, hogy szükség lehet több, mint a működés felső korlátjának, lépjen kapcsolatba az ügyfélszolgálat bármikor, hogy azt igényeinek megismeréséhez és annak megfelelően módosítsa a működés felső korlátjának. A Service Bus hello összesített memóriahasználati kvóta havonta 5 milliárd üzenetek esetén.

Amíg azt lefoglalni hello jobb toodisable egy felhasználói fiókot, amely a használati kvóták túllépte az adott hónapban, rendszer adja meg az e-mail értesítési és bármely megtétele előtt az ügyfél több kísérletek toocontact ellenőrizze. Továbbra is meghaladja a ezek mely százalékértékénél kéri ügyfelek felelős, amelyek mérete meghaladja a hello kvóták díjakat.

Csakúgy, mint a más Azure-szolgáltatások, a Service Bus kényszeríti annak engedélyezése, hogy van-e az erőforrások igazságos használati adott kvóták tooensure készlete. Ezek a kvóták kapcsolatos további részletek találhatók hello [Service Bus kvóták áttekintése][Quotas overview].

## <a name="troubleshooting"></a>Hibaelhárítás
### <a name="what-are-some-of-hello-exceptions-generated-by-azure-service-bus-apis-and-their-suggested-actions"></a>Mik az egyes hello kivételek Azure Service Bus API-k és a javasolt lépések által létrehozott?
A lehetséges a Service Bus-kivételek listájáért lásd: [kivételek áttekintése][Exceptions overview].

### <a name="what-is-a-shared-access-signature-and-which-languages-support-generating-a-signature"></a>Mi az, hogy egy közös hozzáférésű Jogosultságkód és nyelveket támogatja az aláírás generálása?
Megosztott hozzáférési aláírásokkal olyan hitelesítési mechanizmus, SHA-256 biztonságos kivonatok vagy URI-k alapján. További információ toogenerate saját aláírások csomópont, PHP, Java és C\#, lásd: hello [megosztott hozzáférési aláírásokkal] [ Shared Access Signatures] cikk.

## <a name="subscription-and-namespace-management"></a>Előfizetés és a névtér-kezelés
### <a name="how-do-i-migrate-a-namespace-tooanother-azure-subscription"></a>Hogyan át egy névtér tooanother Azure-előfizetés?

Léphet a névtér egy Azure-előfizetés tooanother vagy hello segítségével [Azure-portálon](https://portal.azure.com) vagy PowerShell-parancsokat. Rendelés tooexecute hello műveletben hello névtér már aktívnak kell lennie. hello felhasználói hello parancsok végrehajtása kell mindkét hello forrás rendszergazdának lennie, és előfizetések célként.

#### <a name="portal"></a>Portál

az Azure portál toomigrate Service Bus névterek tooanother előfizetés toouse hello hello kövesse [Itt](../azure-resource-manager/resource-group-move-resources.md#use-portal). 

#### <a name="powershell"></a>PowerShell

hello PowerShell-parancsokat az alábbi sorrendben az áll át, a névtér egy Azure-előfizetés tooanother. tooexecute Ez a művelet hello névtér már aktív kell lennie, és hello PowerShell-parancsokat futtató hello felhasználónak rendszergazdai jogokkal kell rendelkeznie mindkét hello forrású és előfizetések.

```powershell
# Create a new resource group in target subscription
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move namespace from source subscription tootarget subscription
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="next-steps"></a>Következő lépések
toolearn Service Bus kapcsolatos további információkért tekintse meg a következő témakörök hello.

* [Introducing Azure Service Bus prémium (blogbejegyzés)](http://azure.microsoft.com/blog/introducing-azure-service-bus-premium-messaging/)
* [Introducing Azure Service Bus prémium (bemutatása) Channel9)](https://channel9.msdn.com/Blogs/Subscribe/Introducing-Azure-Service-Bus-Premium-Messaging)
* [Service Bus áttekintése](service-bus-messaging-overview.md)
* [Az Azure Service Bus architektúrájának áttekintése](service-bus-fundamentals-hybrid-solutions.md)
* [Bevezetés a Service Bus által kezelt üzenetsorok használatába](service-bus-dotnet-get-started-with-queues.md)

[Best practices for performance improvements using Service Bus]: service-bus-performance-improvements.md
[Best practices for insulating applications against Service Bus outages and disasters]: service-bus-outages-disasters.md
[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Quotas overview]: service-bus-quotas.md
[Exceptions overview]: service-bus-messaging-exceptions.md
[Shared Access Signatures]: service-bus-sas.md
