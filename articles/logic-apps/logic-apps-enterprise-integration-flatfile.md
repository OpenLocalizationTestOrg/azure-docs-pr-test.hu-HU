---
title: "aaaEncode vagy egybesimított fájlok az Azure logic apps |} Microsoft Docs"
description: "Hogyan toouse hello fájl fájl kódoló és dekóder a logic Apps alkalmazásokat a vállalati integrációs csomag hello"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: 82152dab-c7ad-43df-b721-596559703be8
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; mandia
ms.openlocfilehash: 2c295586625fd84366ec7cbafdcebf0489ba234d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-enterprise-integration-with-flat-files"></a>Vállalati integrációs egybesimított fájlok áttekintése

Érdemes lehet tooencode XML tartalom elküldés előtt tooa üzleti partner üzleti vállalatközi (B2B) esetén. A logikai alkalmazás hello strukturálatlan fájlkódolás összekötő toodo ez is használhatja. hello az Ön által létrehozott logikai alkalmazás lekérheti az XML tartalom móddal, többek között egy HTTP-kérelem eseményindítóval, egy másik alkalmazás, vagy akár egy hello számos különböző [összekötők](../connectors/apis-list.md). A logic apps kapcsolatos további információkért lásd: hello [logic apps dokumentációs](logic-apps-what-are-logic-apps.md "további információk a Logic apps").  

## <a name="create-hello-flat-file-encoding-connector"></a>Hello egybesimított fájl kódolási összekötő létrehozása
Kövesse ezeket a lépéseket tooadd egy egyszerű fájlkódolás összekötő tooyour logikai alkalmazást.

1. Logikai alkalmazás létrehozása és [tooyour integrációs fiók hivatkozás](logic-apps-enterprise-integration-accounts.md "ismerje meg, az integráció fiók tooa logikai alkalmazás toolink"). Ezt a fiókot fogja használni tooencode hello XML-adatok hello sémát tartalmaz.  
2. Adja hozzá a **kérelem - amikor egy HTTP-kérelem érkezik** eseményindító tooyour logikai alkalmazást.  
   ![Képernyőkép a eseményindító tooselect](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)    
3. Adja hozzá a hello strukturálatlan fájlkódolás művelet, az alábbiak szerint:
   
    a. Jelölje be hello **plus** bejelentkezési.
   
    b. Jelölje be hello **művelet hozzáadása** (jelenik meg, miután kiválasztotta a hello plusz jel) hivatkozásra.
   
    c. Hello keresési mezőbe, írja be a *egyszerű* toofilter összes hello műveletek toohello egy megjeleníteni kívánt toouse.
   
    d. Jelölje be hello **strukturálatlan fájlkódolás** elemet hello listában.   
   ![Képernyőfelvétel a strukturálatlan fájlkódolás beállítás](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   
4. A hello **strukturálatlan fájlkódolás** párbeszédpanel megnyitásához, jelölje be hello **tartalom** szövegmezőben.  
   ![Képernyőfelvétel a tartalom szövegmező](media/logic-apps-enterprise-integration-flatfile/flatfile-3.png)  
5. Jelöljön ki hello törzs címke, a tartalom, amelyet az tooencode hello. hello törzs címke feltölti hello tartalom mezőben.     
   ![Képernyőkép a szervezet címke](media/logic-apps-enterprise-integration-flatfile/flatfile-4.png)  
6. Jelölje be hello **sémanév** listán, és válassza ki a kívánt toouse tooencode hello hello séma bemeneti tartalom.    
   ![Képernyőfelvétel a séma neve listamező](media/logic-apps-enterprise-integration-flatfile/flatfile-5.png)  
7. Mentse a munkáját.   
   ![Képernyőfelvétel a mentés ikon](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)  

Ezen a ponton befejezte a fájlszintű kódolási connector beállítása. Egy valós alkalmazás érdemes lehet egy sor üzleti alkalmazás, például a Salesforce toostore hello kódolású adatokat. Vagy a kódolt adatok tooa kereskedelmi partner küldhet. Egy művelet toosend hello kimeneti hello kódolási művelet tooSalesforce vagy tooyour kereskedelmi partner, a hello segítségével a többi összekötőt, a megadott egyszerű hozzáadása.

Az összekötő egy kérelem toohello HTTP-végpont, és a hello kérelem törzse hello is beleértve hello XML-tartalom most tesztelheti.  

## <a name="create-hello-flat-file-decoding-connector"></a>Hello egybesimított fájl dekódolás összekötő létrehozása

> [!NOTE]
> toocomplete ezeket a lépéseket, a következő sémafájl már feltöltött az integráció fiókjába toohave kell.

1. Adja hozzá a **kérelem - amikor egy HTTP-kérelem érkezik** eseményindító tooyour logikai alkalmazást.  
   ![Képernyőkép a eseményindító tooselect](./media/logic-apps-enterprise-integration-b2b/flatfile-1.png)    
2. Hello művelet, az alábbiak szerint dekódolás egybesimított fájl hozzáadása:
   
    a. Jelölje be hello **plus** bejelentkezési.
   
    b. Jelölje be hello **művelet hozzáadása** (jelenik meg, miután kiválasztotta a hello plusz jel) hivatkozásra.
   
    c. Hello keresési mezőbe, írja be a *egyszerű* toofilter összes hello műveletek toohello egy megjeleníteni kívánt toouse.
   
    d. Jelölje be hello **Egybesimított fájl dekódolás** elemet hello listában.   
   ![Képernyőfelvétel a Egybesimított fájl dekódolás beállítás](media/logic-apps-enterprise-integration-flatfile/flatfile-2.png)   
3. Jelölje be hello **tartalom** vezérlő. Ezzel létrehozza a korábbi lépések, amely akkor használható hello tartalom toodecode hello tartalmat listáját. Figyelje meg, hogy hello *törzs* hello bejövő HTTP-kérelem nem érhető el tartalom toodecode hello használt toobe. Közvetlenül a hello is megadhat hello tartalom toodecode **tartalom** vezérlő.     
4. Jelölje be hello *törzs* címke. Értesítés hello törzs címke már hello **tartalom** vezérlő.
5. Válassza ki a megjeleníteni kívánt toouse toodecode hello tartalom hello séma hello nevét. hello alábbi képernyőfelvételen látható, amely *OrderFile* hello kijelölt séma neve. A séma neve kellett feltöltve hello integrációs figyelembe korábban.
   
   ![Képernyőfelvétel a Egybesimított fájl dekódolás párbeszédpanel](media/logic-apps-enterprise-integration-flatfile/flatfile-decode-1.png)    
6. Mentse a munkáját.  
   ![Képernyőfelvétel a mentés ikon](media/logic-apps-enterprise-integration-flatfile/flatfile-6.png)    

Ezen a ponton befejezte a egybesimított fájl dekódolás összekötő beállítása. Egy valós alkalmazás érdemes lehet toostore hello dekódolni, adatok, például a Salesforce-üzletági alkalmazásokban. Egyszerűen hozzáadhatja művelet tooSalesforce dekódolás hello művelet toosend hello kimenete.

Az összekötő azáltal, hogy egy kérelem toohello HTTP-végpont most tesztelheti, és hello XML-tartalom toodecode hello kérelem törzse hello a kívánt.  

## <a name="next-steps"></a>Következő lépések
* [További tudnivalók a vállalati integrációs csomag hello](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag").  

