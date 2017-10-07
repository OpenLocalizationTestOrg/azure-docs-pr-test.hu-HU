---
title: "aaaConvert XML-adatok átalakító - Azure Logic Apps |} Microsoft Docs"
description: "Átalakítások vagy mapps tooconvert XML-adatok között szereplő hello vállalati integrációs SDK használatával a logic apps-formátumok létrehozása"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: add01429-21bc-4bab-8b23-bc76ba7d0bde
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: b56ec1072c5058d3aefc7f88ac9b2748ebe56456
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enterprise-integration-with-xml-transforms"></a>XML-átalakítók vállalati integrációja
## <a name="overview"></a>Áttekintés
hello vállalati integrációs átalakító összekötő formátum tooanother formátumba konvertálja az adatokat. Például előfordulhat, hogy egy bejövő üzenet, amely tartalmazza a hello aktuális hello YearMonthDay formátumú dátumot. Használhat egy átalakítási tooreformat hello dátum toobe hello MonthDayYear formátumban.

## <a name="what-does-a-transform-do"></a>Mire átalakító?
Átalakító, amely más néven a térkép, a forrás XML-séma (hello bemeneti) és a célként megadott XML-séma (hello kimeneti) áll. Különféle beépített funkciók toohelp módosítására, vagy szabályozó hello, használhatja a karakterlánc-feldolgozás, feltételes hozzárendelés, aritmetikai kifejezésekben, dátum idő is beleértve, és még ismétlési szerkezeteket.

## <a name="how-toocreate-a-transform"></a>Hogyan toocreate átalakító?
Átalakítás/térképre hello Visual Studio használatával hozhat létre [vállalati integrációs SDK](https://aka.ms/vsmapsandschemas). Amikor végzett létrehozása és tesztelése hello átalakító, az integráció figyelembe feltöltött hello átalakító. 

## <a name="how-toouse-a-transform"></a>Hogyan toouse átalakítás
Az integráció figyelembe hello átalakító/térkép feltöltésekor használata toocreate logikai alkalmazás. hello logikai alkalmazás a átalakítások fut, amikor hello logikai alkalmazás elindul (vagy át legyenek-e toobe igénylő beviteli tartalom).

**Az alábbiakban hello lépéseket toouse átalakító**:

### <a name="prerequisites"></a>Előfeltételek

* Integráció-fiók létrehozása és egy térképen tooit hozzáadása  

Most, hogy a hello Előfeltételek korábban hozott ítélt, a rendszer idő toocreate a Logic Apps alkalmazást:  

1. Logikai alkalmazás létrehozása és [tooyour integrációs fiók hivatkozás](../logic-apps/logic-apps-enterprise-integration-accounts.md "ismerje meg, az integráció fiók tooa logikai alkalmazás toolink") hello térkép tartalmazza.
2. Adja hozzá a **kérelem** eseményindító tooyour logikai alkalmazás  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-1.png)    
3. Hozzáadás hello **átalakítása XML** első kiválasztásával művelet **művelet hozzáadása**   
   ![](./media/logic-apps-enterprise-integration-transforms/transform-2.png)   
4. Adja meg a hello word *átalakítási* hello Keresés mezőbe toofilter összes műveletek toohello egy megjeleníteni kívánt toouse hello  
   ![](./media/logic-apps-enterprise-integration-transforms/transform-3.png)  
5. Jelölje be hello **átalakítása XML** művelet   
6. Adja hozzá az hello XML **tartalom** , amely alakít át. Is használhatja, mint hello hello HTTP-kérelem érkezik XML-adatokat **tartalom**. Ebben a példában válassza ki a hello hello logikai alkalmazás kiváltó hello HTTP-kérelem törzsét.
7. Jelölje be hello neve hello **térkép** , amelyet az toouse tooperform hello átalakítása. hello térkép szerepelniük kell a integrációs fiókját. A korábbi lépésben a Logic app hozzáférési tooyour integrációs fiók, amely tartalmazza a térképre már megadott.      
   ![](./media/logic-apps-enterprise-integration-transforms/transform-4.png) 
8. Mentse a munkáját  
    ![](./media/logic-apps-enterprise-integration-transforms/transform-5.png) 

Ezen a ponton befejezte a térképre beállítása. Egy valós alkalmazás érdemes lehet egy LOB-alkalmazás, például SalesForce toostore hello át legyenek-e adatokat. Hello művelet toosend hello kimeneteként könnyen tooSalesforce alakíthatja át. 

Az átalakítási azáltal, hogy egy kérelem toohello HTTP-végpont most tesztelheti.  

## <a name="features-and-use-cases"></a>Szolgáltatások és a használati esetek
* lehet, hogy a térképen létrehozott hello átalakítása egyszerű, például egy dokumentum tooanother másolása nevét és címét. Vagy összetettebb átalakítások hello out-of-az-box térkép műveletekkel is létrehozhat.  
* Több leképezés műveletek vagy funkciók azonnal elérhetők, beleértve a karakterláncok, dátum időpont függvényei, és így tovább.  
* Hello sémák közötti közvetlen adatmásolás teheti meg. Hello leképező hello SDK szerepel ez az más dolga, mint egy vonal hello elemek hello forrás sémában kapcsoló, mint a hello cél sémában.  
* Olyan térképet létrehozásakor megtekintheti hello leképezés, megjelenítheti az összes hello kapcsolat és hivatkozásokat hoz létre grafikus ábrázolása.
* Hello teszt leképezés funkció tooadd XML mintaüzenet használja. Egy egyszerű kattintással hello térkép hozott létre, és tekintse meg a létrehozott hello kimeneti tesztelheti.  
* Töltse fel a meglévő leképezéseket  
* Hello XML-formátuma támogatását is magában foglalja.

## <a name="learn-more"></a>Részletek
* [További tudnivalók a vállalati integrációs csomag hello](../logic-apps/logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag")  
* [További információ a maps](../logic-apps/logic-apps-enterprise-integration-maps.md "vállalati integrációs maps ismertetése")  

