---
title: "Azure Data Factory entitások elnevezési szabályai |} Microsoft Docs"
description: "Adat-előállító entitások elnevezési szabályainak ismerteti."
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: bc5e801d-0b3b-48ec-9501-bb4146ea17f1
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: shlo
ms.openlocfilehash: d447bbceb4ab344e011311eaf143b20f0a0400d3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="azure-data-factory---naming-rules"></a>Az Azure Data Factory - elnevezési szabályok
A következő táblázat elnevezési szabályoknak az adat-előállító összetevők.

| Név | Név egyedisége | Érvényességi ellenőrzéseket |
|:--- |:--- |:--- |
| Data Factory |Egyedi Microsoft Azure között. Nevek nem különböztetik meg, ez azt jelenti, hogy `MyDF` és `mydf` adat-előállító hivatkozik. |<ul><li>Minden adat-előállító pontosan egy Azure-előfizetés van kötve.</li><li>Objektumnevek betűvel vagy számmal kell kezdődnie, és csak betűket, számokat és a kötőjel (-) karaktert tartalmazhat.</li><li>Minden kötőjel (-) karaktert legyen azonnal előtt, és betűvel vagy számmal követ. A tároló neve nem szerepelhetnek egymást követő kötőjeleket.</li><li>Neve 3 – 63 karakter hosszú lehet.</li></ul> |
| Szolgáltatások/táblák/folyamatok csatolt |Egyedi az adat-előállítóban. Nevek nem különböztetik meg. |<ul><li>A táblanév maximális karakterszámot: 260.</li><li>Objektumnevek betű, szám vagy aláhúzásjel (_) kell kezdődnie.</li><li>Következő karakterek nem engedélyezettek: ".", "+","?", "/", "<", ">","*", "%", "&", ":","\\"</li></ul> |
| Erőforráscsoport |Egyedi Microsoft Azure között. Nevek nem különböztetik meg. |<ul><li>Karakterek maximális száma: 1000.</li><li>Név tartalmazhat betűket, számjegyeket és a következő karaktereket: "-", "_",","és"."</li></ul> |

