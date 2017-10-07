---
title: "aaaPricing & számlázási - Azure Logic Apps |} Microsoft Docs"
description: "Ismerje meg, az Azure Logic Apps árak és számlázás működése."
author: kevinlam1
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: f8f528f5-51c5-4006-b571-54ef74532f32
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: LADocs; klam
ms.openlocfilehash: fa10cbbf7657afba7fadb7c76817b7a5e4af7b42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="logic-apps-pricing-model"></a>Logic Apps díjszabási modell
Az Azure Logic Apps tooscale lehetővé teszi, majd hajtsa végre az integráció munkafolyamatok hello felhőben.  Az alábbiakban a számlázási és a Logic Apps díjszabások hello részletei.
## <a name="consumption-pricing"></a>Felhasználás díjszabása
Az újonnan létrehozott Logic Apps a fogyasztás csomag használata. Hello Logic Apps fogyasztás árképzési modellt csak kell fizetnie a valóban használt funkciókért.  A Logic Apps nem szabályozott fogyasztás terv használatakor.
A logic app-példány futását végrehajtott összes műveletek mérése.
### <a name="what-are-action-executions"></a>Mik azok a műveleti végrehajtások?
A logic app-definíciót minden lépése egy művelet, beleértve eseményindítók, ellenőrzési folyamata lépéseket mint állapotától függően a hatókört, minden egyes hurkok hurkok, amíg meghívja tooconnectors és hívások toonative műveletek.
Eseményindítók olyan speciális művelet, a rendszer tervezett tooinstantiate logikai alkalmazás új példánya, egy adott esemény bekövetkezésekor.  Nincsenek eseményindítók, amely befolyásolhatja, hogyan hello logikai alkalmazás forgalmi díjas számos különböző beállításokat.
* **Lekérdezési eseményindító** – ehhez az eseményindítóhoz folyamatosan lekérdezi a végpont, amíg nem kap egy üzenetet, amely eleget tesz a hello feltételek megadása a logikai alkalmazás példányának létrehozásakor.  a Logic App Designer hello hello eseményindító hello lekérdezési időközt konfigurálható.  Egyes lekérdezési kérelmek akkor is, ha nem hoz létre egy logikai alkalmazás egy példánya számít egy művelet végrehajtását.
* **Webhook eseményindító** – ehhez az eseményindítóhoz vár egy ügyfél toosend azt egy adott végpont a kérelmet.  Minden kérelmet küldött toohello webhook végpont számít egy művelet végrehajtását. hello kérelem és a HTTP Webhook eseményindító hello is mindkét webhook eseményindítók.
* **Ismétlődés eseményindító** – ehhez az eseményindítóhoz létrehoz egy új hello logikai alkalmazás hello ismétlési időköz hello eseményindító konfigurált alapján.  Például egy ismétlődési eseményindító lehet konfigurált toorun három naponta vagy akár percenként.

Eseményindító végrehajtások hello Logic Apps erőforráspanelen hello eseményindító előzmények része látható.

Minden műveletek végrehajtódtak, egy művelet végrehajtását, hogy sikeres vagy sikertelen volt-e azok mérése.  A feltétel nem teljesül tooa miatt kihagyta vagy műveletekről, amelyek nem hajtható végre, mert még a befejeződése előtt megszakadt hello logikai alkalmazás nem számítanak művelet végrehajtások.

Hurkok belül végrehajtható műveletek / hello hurok iterációs bájtjai számítanak.  Például az egyetlen művelettel egy for-each ciklus 10 elemek listája iteráció számítanak hello száma (10) hello lista elemeinek hello hurok (1) a műveletek hello számát szorozva plusz egy hello hurok hello kezdeményezése a , amely, ebben a példában lenne (10 * 1) + 1 = 11 művelet végrehajtások.
Letiltott Logic Apps nem lehet létrehozni új példányok, és ezért le van tiltva, miközben nem számolnak.  Kell szem előtt tartva, hogy a logikai alkalmazás letiltását követően is eltarthat egy kis időt hello példányok tooquiesce előtt teljesen le van tiltva.
### <a name="integration-account-usage"></a>Integráció fiók használata
Használati fogyasztás alapján szereplő van egy [integrációs fiók](logic-apps-enterprise-integration-create-integration-account.md) feltárása, fejlesztési és tesztelési célra, hogy lehetővé teszi a toouse hello [B2B/EDI](logic-apps-enterprise-integration-b2b.md) és [XML-feldolgozás](logic-apps-enterprise-integration-xml.md)funkciók a Logic Apps minden további költség nélkül. Képes toocreate legfeljebb egy fiók régiónként és too10 szerződések és 25 maps tárolja. Sémákat, a tanúsítványok és a partnerek is nem határoz meg, és feltöltheti a lehető legtöbb, szükség szerint.

Ezenkívül toohello felvétel integrációs fiókok felhasználás, is létrehozhat szabványos integrációs fiókok ezek a korlátozások nélkül és a szabványos Logic Apps SLA-t. További információkért lásd: [Azure-beli árakról](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="app-service-plans"></a>App Service-csomagok
A Logic apps korábban létrehozott egy App Service-csomag hivatkozó toobehave, mielőtt folytatódik. Attól függően, hogy a kiválasztott hello terv szabályozott után hello előírt napi végrehajtások túllépése, de hello művelet végrehajtási mérő vannak számlázva.
Nagyvállalati ügyfelek, amelyeknek az előfizetését, amelyek nem rendelkeznek explicit módon társított logikai alkalmazás hello toobe, az App Service-csomag része hello mennyiségek juttatás beolvasása.  Például ha egy Standard App Service-csomag EA előfizetése és a logikai alkalmazás hello ugyanahhoz az előfizetéshez, majd meg nem szó, a 10 000 művelet végrehajtások napi (lásd a következő táblázat). 

App Service-csomagok és a napi megengedett művelet végrehajtások:
|  | Ingyenes/megosztott/egyszerű | Standard | Prémium |
| --- | --- | --- | --- |
| A művelet végrehajtások száma naponta |200 |10,000 |50,000 |
### <a name="convert-from-app-service-plan-pricing-tooconsumption"></a>App Service-csomag tooConsumption árképzési konvertálása
toochange, amely rendelkezik az App Service-csomag társított logikai alkalmazás tooa fogyasztás modell eltávolítása hello hivatkozás toohello App Service-csomag a hello Logic App-definíciót.  Ez a módosítás nem végezhető hívás tooa PowerShell-parancsmagot:`Set-AzureRmLogicApp -ResourceGroupName ‘rgname’ -Name ‘wfname’ –UseConsumptionModel -Force`
## <a name="pricing"></a>Díjszabás
Díjszabása, lásd: [Logic Apps árképzési](https://azure.microsoft.com/pricing/details/logic-apps).

## <a name="next-steps"></a>Következő lépések
* [A Logic Apps áttekintése][whatis]
* [Az első logikai alkalmazás létrehozása][create]

[pricing]: https://azure.microsoft.com/pricing/details/logic-apps/
[whatis]: logic-apps-what-are-logic-apps.md
[create]: logic-apps-create-a-logic-app.md

