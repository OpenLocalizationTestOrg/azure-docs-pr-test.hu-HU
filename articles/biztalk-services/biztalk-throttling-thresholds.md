---
title: "BizTalk szolgáltatások a sávszélesség-szabályozási kapcsolatos aaaLearn |} Microsoft Docs"
description: "Ismerje meg a sávszélesség-szabályozás küszöbértékeit, és az amiatt végbemenő futtatási viselkedés BizTalk szolgáltatások. Sávszélesség-szabályozás a memóriahasználat és üzenetek száma alapul. MABS, WABS"
services: biztalk-services
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: f6663cf2-cda4-4bac-855e-27d2ad5c4fa4
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/07/2016
ms.author: mandia
ms.openlocfilehash: 46c8806c3a1f4eeb793f721f849771e0ec155197
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="biztalk-services-throttling"></a>BizTalk Services: Szabályozás

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

Az Azure BizTalk szolgáltatások megvalósítja szolgáltatás sávszélesség-szabályozás két feltételek alapján: memória kihasználtsága és hello egyidejű üzenetek száma feldolgozása. Ez a témakör küszöbértékek szabályozás hello és hello működését ismerteti, ha sávszélesség-szabályozási állapot akkor fordul elő.

## <a name="throttling-thresholds"></a>Sávszélesség-szabályozási küszöbértékek
a következő táblázat hello hello szabályozási forrás- és a küszöbértékek:

|  | Leírás | Alsó küszöbérték | Magas küszöbértéket |
| --- | --- | --- | --- |
| Memory (Memória) |%-a teljes rendszer memória rendelkezésre álló/PageFileBytes. <p><p>Teljes rendelkezésre álló PageFileBytes körülbelül 2 alkalommal hello RAM hello rendszer. |60% |70% |
| Üzenet feldolgozása |Feldolgozott üzenetek száma |40 * magok száma |100 * magok száma |

A magas küszöbérték elérésekor a Azure BizTalk szolgáltatások toothrottle elindul. Sávszélesség-szabályozási leállítja hello alsó küszöbérték elérésekor. A szolgáltatás használata esetén például 65 % rendszermemória. Ebben a helyzetben hello szolgáltatást nem szabályozás. A szolgáltatás elindul, a rendszer memória 70 %. Ebben a helyzetben hello szolgáltatást azelőtt gyorsítja fel, és toothrottle továbbra is fennáll, addig, amíg hello szolgáltatás 60 % (alsó küszöbérték) rendszer memóriát használ.

Azure BizTalk szolgáltatások hello állapot normál szabályozottan halmozott állapotát és szabályozás és a szabályozás időtartama hello követi nyomon.

## <a name="runtime-behavior"></a>Futtatási viselkedés
BizTalk szolgáltatások Azure szabályozási állapotba kerül, ha hello következő történik:

* Sávszélesség-szabályozás egy szerepkör-példány van. Példa:<br/>
  A szabályozás RoleInstanceA. RoleInstanceB nem a szabályozás. Ebben a helyzetben a RoleInstanceB üzenetek feldolgozása várt módon. RoleInstanceA üzeneteket a rendszer elveti és hello a következő hiba miatt sikertelen:<br/><br/>
  **Kiszolgáló túlterhelt. Próbálkozzon újra.**<br/><br/>
* Egyetlen lekéréses forrás ne kérdezze le, és töltse le az üzenetet. Példa:<br/>
  Egy folyamat üzenetek FTP külső forrásból kéri le. Ennek során hello lekéréses hello szerepkörpéldányt lekérdezi a sávszélesség-szabályozási állapot. Ebben a helyzetben hello folyamat leáll, letölti a további üzeneteket, amíg hello szerepkörpéldányt leállítja a sávszélesség-szabályozás.
* Választ küldött toohello ügyfél így hello ügyfél üdvözlőüzenetére is újraküldése.
* Meg kell várnia, amíg hello sávszélesség-szabályozás nem oldódik. Pontosabban meg kell várnia hello alsó küszöbérték elérésekor.

## <a name="important-notes"></a>Fontos megjegyzések
* Sávszélesség-szabályozás nem tiltható le.
* Sávszélesség-szabályozás küszöbértékek nem módosíthatók.
* Sávszélesség-szabályozás rendszerszintű valósul meg.
* hello Azure SQL adatbázis-kiszolgáló is rendelkezik beépített szabályozás.

## <a name="additional-azure-biztalk-services-topics"></a>További Azure BizTalk szolgáltatások kapcsolatos témakörök
* [Hello Azure BizTalk szolgáltatások SDK telepítése](http://go.microsoft.com/fwlink/p/?LinkID=241589)<br/>
* [Oktatóanyag: Azure BizTalk szolgáltatások](http://go.microsoft.com/fwlink/p/?LinkID=236944)<br/>
* [Hogyan tudom használata hello Azure BizTalk szolgáltatások SDK-t](http://go.microsoft.com/fwlink/p/?LinkID=302335)<br/>
* [Az Azure BizTalk szolgáltatások](http://go.microsoft.com/fwlink/p/?LinkID=303664)<br/>

## <a name="see-also"></a>Lásd még:
* [BizTalk szolgáltatások: Fejlesztői, Basic, Standard és prémium kiadás diagram](http://go.microsoft.com/fwlink/p/?LinkID=302279)<br/>
* [BizTalk szolgáltatások: Kiépítés használata Azure klasszikus portál](http://go.microsoft.com/fwlink/p/?LinkID=302280)<br/>
* [BizTalk Services: Kiépítési állapot diagramja](http://go.microsoft.com/fwlink/p/?LinkID=329870)<br/>
* [BizTalk Services: Irányítópult, Figyelés és Méret lapok](http://go.microsoft.com/fwlink/p/?LinkID=302281)<br/>
* [BizTalk Services: Biztonsági mentés és visszaállítás](http://go.microsoft.com/fwlink/p/?LinkID=329873)<br/>
* [BizTalk Services: Kiállító neve és kiállító kulcsa](http://go.microsoft.com/fwlink/p/?LinkID=303941)<br/>

