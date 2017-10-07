---
title: "az Azure CDN aaaReal idejű statisztikák |} Microsoft Docs"
description: "Valós idejű statisztikák Azure CDN hello teljesítményének valós idejű adatokat biztosít, amikor a tartalom tooyour ügyfelek kézbesítéséhez."
services: cdn
documentationcenter: 
author: zhangmanling
manager: erikre
editor: 
ms.assetid: c7989340-1172-4315-acbb-186ba34dd52a
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: mazha
ms.openlocfilehash: 68900a5092b767e45c1fdf9cef2cd03f55f38a6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="real-time-stats-in-microsoft-azure-cdn"></a>A Microsoft Azure CDN valós idejű statisztikák
[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Áttekintés
Ez a dokumentum ismerteti a Microsoft Azure CDN valós idejű statisztikák.  Ezt a funkciót biztosít a valós idejű adatok, például a sávszélesség, a gyorsítótár állapotok és az egyidejű kapcsolatok tooyour CDN profil meghatározott tartalom tooyour ügyfelek. Ez lehetővé teszi, hogy folyamatosan figyelje a hello állapotát a szolgáltatás bármikor, beleértve az éles események.

a következő diagramjait hello érhetők el:

* [Sávszélesség](#bandwidth)
* [Állapotkódok](#status-codes)
* [Gyorsítótár-állapotok](#cache-statuses)
* [Kapcsolatok](#connections)

## <a name="accessing-real-time-stats"></a>Valós idejű statisztikák elérése
1. A hello [Azure Portal](https://portal.azure.com), keresse meg a tooyour CDN-profilt.
   
    ![CDN-profil panelje](./media/cdn-real-time-stats/cdn-profile-blade.png)
2. A CDN-profil panelje hello, kattintson a hello **kezelése** gombra.
   
    ![CDN-profil panelje kezelése gomb](./media/cdn-real-time-stats/cdn-manage-btn.png)
   
    Megnyílik a hello CDN felügyeleti portálon.
3. Hello az egérmutatót **Analytics** fülre, majd az egérmutatót hello **valós idejű statisztikák** menü.  Kattintson a **HTTP nagy objektum**.
   
    ![CDN-felügyeleti portálon](./media/cdn-real-time-stats/cdn-premium-portal.png)
   
    hello valós idejű statisztikák diagramjait jelennek meg.

Egyes hello diagramjait jeleníti meg valós idejű statisztikák a kiválasztott hello időtartam kezdési hello oldal betöltésekor.  hello diagramjait automatikus frissítés minden néhány másodpercben.  Hello **frissítése Graph** gombra, ha van ilyen, törölni fogja a hello diagramot, amely után csak jelenik kijelölt hello adatokat.

## <a name="bandwidth"></a>Sávszélesség
![Sávszélesség-grafikon](./media/cdn-real-time-stats/cdn-bandwidth.png)

Hello **sávszélesség** graph hello kijelölt hello időtartamának hello aktuális platform használt sávszélesség mennyiségét jeleníti meg. hello hello graph árnyékolt része azt jelzi, hogy a sávszélesség-használat. hello pontos jelenleg használt sávszélesség mennyiségét közvetlenül hello vonaldiagram alább látható.

## <a name="status-codes"></a>Állapotkódok
![Kód állapotdiagram](./media/cdn-real-time-stats/cdn-status-codes.png)

Hello **állapotkódok** grafikon azt jelzi, hogy milyen gyakran bizonyos HTTP válaszkódot kijelölt hello időtartama alatt is megjelenhetnek.

> [!TIP]
> Az egyes HTTP-állapot kód lehetőségek ismertetését lásd: [Azure CDN HTTP-állapotkódok](https://msdn.microsoft.com/library/mt759238.aspx).
> 
> 

A HTTP-állapotkódok listáját közvetlenül hello graph felett jelenik meg. A lista azt jelzi, hogy minden állapotkód tartalmazhat hello vonaldiagram és hello aktuális előfordulások adott állapotkód másodpercenkénti száma. Alapértelmezés szerint egy sor minden ezek állapotkódok hello Graph jelenik meg. Azonban lehetősége van tooonly figyelő hello állapotkódok, amelyek különleges jelentőséggel a CDN-konfigurációhoz. toodo, ellenőrizze a szükségeskonfiguráció-hello állapotkódok és törölje minden egyéb beállításokat, majd kattintson **frissítése Graph**. 

Egy adott állapotkód: az a naplózott adatok ideiglenesen elrejthetők.  A hello jelmagyarázat közvetlenül hello graph alatt kattintson az toohide kívánt hello állapotkódot. hello állapotkód azonnal rejtett hello grafikonból. Kattintson ismét az adott állapotkód: eredményezi, hogy a beállítás toobe újra megjelenik.

## <a name="cache-statuses"></a>Gyorsítótár-állapotok
![Gyorsítótár-állapotok diagramhoz](./media/cdn-real-time-stats/cdn-cache-status.png)

Hello **gyorsítótár állapotok** grafikon azt jelzi, hogy milyen gyakran bizonyos típusú gyorsítótár állapotok kijelölt hello időtartama alatt is megjelenhetnek. 

> [!TIP]
> Az egyes gyorsítótár állapot kód lehetőségek ismertetését lásd: [Azure CDN gyorsítótár állapotkódok](https://msdn.microsoft.com/library/mt759237.aspx).
> 
> 

Gyorsítótár állapotkódok listája közvetlenül hello graph felett jelenik meg. A lista azt jelzi, hogy minden állapotkód tartalmazhat hello vonaldiagram és hello aktuális előfordulások adott állapotkód másodpercenkénti száma. Alapértelmezés szerint egy sor minden ezek állapotkódok hello Graph jelenik meg. Azonban lehetősége van tooonly figyelő hello állapotkódok, amelyek különleges jelentőséggel a CDN-konfigurációhoz. toodo, ellenőrizze a szükségeskonfiguráció-hello állapotkódok és törölje minden egyéb beállításokat, majd kattintson **frissítése Graph**. 

Egy adott állapotkód: az a naplózott adatok ideiglenesen elrejthetők.  A hello jelmagyarázat közvetlenül hello graph alatt kattintson az toohide kívánt hello állapotkódot. hello állapotkód azonnal rejtett hello grafikonból. Kattintson ismét az adott állapotkód: eredményezi, hogy a beállítás toobe újra megjelenik.

## <a name="connections"></a>Kapcsolatok
![Kapcsolatok diagramhoz](./media/cdn-real-time-stats/cdn-connections.png)

Ez a diagram azt jelzi, hogy hány kapcsolatok lett volna a meghatározott tooyour peremhálózati kiszolgálóinak. Minden egyes kérelem egy eszköz, amely áthalad a CDN-eredmények kapcsolaton keresztül.

## <a name="next-steps"></a>Következő lépések
* Az értesítés [valós idejű riasztások az Azure CDN szolgáltatás használata](cdn-real-time-alerts.md)
* Mélyebb a Dig [speciális HTTP-jelentések](cdn-advanced-http-reports.md)
* Elemezze [használati minták](cdn-analyze-usage-patterns.md)

