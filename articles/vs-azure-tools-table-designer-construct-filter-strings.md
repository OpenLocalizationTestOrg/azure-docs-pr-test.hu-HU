---
title: "aaaConstructing szűrőkarakterláncokban hello tábla Designer |} Microsoft Docs"
description: "Hello tábla Designer szűrőkarakterláncokban létrehozása"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: a1a10ea1-687a-4ee1-a952-6b24c2fe1a22
ms.service: storage
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/18/2016
ms.author: kraigb
ms.openlocfilehash: 48b38d27b97936064daa875e41881d51546bc11f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="constructing-filter-strings-for-hello-table-designer"></a>A tábla Designer hello hozhat létre, Szűrőkarakterláncokban
## <a name="overview"></a>Áttekintés
egy Azure-tábla megjelenő adatok toofilter hello Visual Studio **tábla Designer**, egy szűrési karakterláncot létrehozni, és írja be a hello szűrő mezőbe. hello szűrő karakterlánc-formátum: határozzák meg a WCF Data Services hello és hasonló tooa SQL WHERE záradék, de toohello Table szolgáltatás HTTP-kérelem keresztül zajlik. Hello **tábla Designer** leírók hello meg a kívánt tulajdonság értéke Igen toofilter megfelelő kódolás, hello tulajdonságnév, összehasonlító operátor feltétel érték csak akkor kell meg és szükség esetén hello lévő logikai operátor szűrése a mező. Nem kell tooinclude hello $filter lekérdezési lehetőség, mintha volt hozhat létre egy URL-cím tooquery hello tábla keresztül hello [Storage szolgáltatások REST API-referencia](http://go.microsoft.com/fwlink/p/?LinkId=400447).

hello a WCF Data Services hello alapuló [protokollhoz adatok](http://go.microsoft.com/fwlink/p/?LinkId=214805) (OData). Talál részletes információt hello szűrő rendszer lekérdezési lehetőség (**$filter**), lásd: hello [URI egyezmények OData specifikáció](http://go.microsoft.com/fwlink/p/?LinkId=214806).

## <a name="comparison-operators"></a>Összehasonlító operátorok
hello következő logikai operátorok támogatja az összes tulajdonság esetében:

| Logikai operátor | Leírás | Példa szűrési karakterláncot |
| --- | --- | --- |
| EQ |Egyenlő |Város eq "Redmond" |
| gt |Nagyobb mint |Árlista gt 20 |
| GE |Nagyobb vagy egyenlő túl|Árlista ge 10 |
| lt |Kisebb mint |Árlista lt 20 |
| le |Kisebb vagy egyenlő |Árlista le 100 |
| Ne |Nem egyenlő |Város ne "London" |
| és |És |Árlista le 200 és ár gt 3.5 |
| vagy |Vagy |Árlista le 3.5-ös vagy ár gt 200 |
| nem |nem |nem isAvailable |

Egy szűrési karakterláncot térített hello következő szabályok fontosak:

* Hello logikai operátorok toocompare tulajdonság tooa értéket használja. Ne feledje, hogy nem lehetséges toocompare tooa dinamikus tulajdonságérték; hello kifejezés oldalán állandónak kell lennie.
* Minden hello szűrési karakterláncot részei kis-és nagybetűket.
* hello hello állandó értéknek kell lennie ahhoz, hogy hello szűrő tooreturn érvényes eredmények hello tulajdonság írja ugyanazokat az adatokat. További információ a támogatott tulajdonságtípus: [ismertetése hello tábla szolgáltatás adatmodell](http://go.microsoft.com/fwlink/p/?LinkId=400448).

## <a name="filtering-on-string-properties"></a>Szűrés karakterlánc tulajdonságai
Karakterlánc-tulajdonságainál szűrésekor idézőjelek hello szövegkonstans egyetlen.

Példa szűrők követően hello hello **PartitionKey** és **RowKey** ; további nem kulcs tulajdonságai Tulajdonságok sikerült is hozzá kell adni toohello szűrési karakterláncot:

    PartitionKey eq 'Partition1' and RowKey eq '00001'

Tegye minden szűrőkifejezés kerek zárójeleket tartalmazhatnak, de nincs rá szükség:

    (PartitionKey eq 'Partition1') and (RowKey eq '00001')

Vegye figyelembe, hogy hello Table szolgáltatás nem támogatja a helyettesítő karakteres lekérdezések, és nem támogatják a tábla Designer hello vagy. Azonban előtag megfelelő hello kívánt előtag alapján összehasonlító operátorok használatával végezheti el. hello alábbi példa entitásokat ad vissza az "A" hello betűvel kezdődő vezetéknév tulajdonság:

    LastName ge 'A' and LastName lt 'B'

## <a name="filtering-on-numeric-properties"></a>A numerikus tulajdonságok szűrése
az egész szám vagy egy lebegőpontos szám toofilter adható meg hello idézőjelek nélkül.

Ez a példa egy kora tulajdonsággal, amelynek értéke nagyobb, mint 30 összes entitásokat ad vissza:

    Age gt 30

Ez a példa egy AmountDue tulajdonsággal, amelynek értéke összes entitásokat ad vissza kisebb vagy egyenlő too100.25:

    AmountDue le 100.25

## <a name="filtering-on-boolean-properties"></a>A logikai tulajdonságok szűrése
toofilter egy logikai értéket adja meg **igaz** vagy **hamis** idézőjelek nélkül.

hello alábbi példa entitásokat ad vissza az összes ahol hello IsActive tulajdonság túl beállítása**igaz**:

    IsActive eq true

A szűrőkifejezés hello logikai operátor anélkül is írhat. A következő példa hello, hello Table szolgáltatás is visszaadható entitásokhoz IsActive esetén **igaz**:

    IsActive

minden entitás, ahol IsActive értéke "false", használhatja hello nem tooreturn operátor:

    not IsActive

## <a name="filtering-on-datetime-properties"></a>Dátum és idő tulajdonságai szűrése
egy DateTime értéket toofilter adja meg a hello **datetime** kulcsszót, hello dátum/idő állandó szimpla idézőjelek között szerepel. hello dátum/idő állandó kombinált UTC formátumban kell megadni, a [formázás DateTime tulajdonságértékek](http://go.microsoft.com/fwlink/p/?LinkId=400449).

a következő példa hello entitásokat ad vissza hello CustomerSince tulajdonság esetén egyenlő tooJuly 10, 2008:

    CustomerSince eq datetime'2008-07-10T00:00:00Z'
