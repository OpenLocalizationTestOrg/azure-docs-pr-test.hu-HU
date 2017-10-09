---
title: "aaaConnect tooan helyszíni SAP rendszer az Azure Logic Apps |} Microsoft Docs"
description: "A logic app munkafolyamat hello a helyszíni adatok átjárón keresztül kapcsolódó tooan helyszíni SAP rendszer"
services: logic-apps
author: padmavc
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/01/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: 594ec5fed337398bf931d396684630ee9f907d2f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooan-on-premises-sap-system-from-logic-apps-with-hello-sap-connector"></a>Tooan helyszíni SAP rendszer csatlakoztatja a logic apps hello SAP-összekötőn keresztül 

hello helyszíni adatátjáró lehetővé teszi toomanage adatokat, és biztonságosan férhetnek hozzá a helyszíni erőforrásokhoz. Ez a témakör bemutatja, hogyan csatlakoztathatja a logic apps tooan helyszíni SAP rendszert. Ebben a példában a Logic Apps alkalmazást az IDOC-kérelmek HTTP Protokollon keresztül, és vissza hello választ küld.    

> [!NOTE]
> Aktuális korlátozások vonatkoznak: 
> - A Logic Apps alkalmazást időtúllépés történik, ha hello válasz szükséges lépéseket nem fejeződik be hello [kérelem időkorlátja](./logic-apps-limits-and-config.md). Ebben a forgatókönyvben kérelmek előfordulhat, hogy blokkolnánk a hozzáférését. 
> - hello fájlválasztó nem jeleníti meg az összes hello elérhető mezők. Ebben a forgatókönyvben kézzel is hozzáadhatja a elérési utakat.

## <a name="prerequisites"></a>Előfeltételek

- Telepítse és konfigurálja a hello legújabb [helyszíni adatátjáró](https://www.microsoft.com/download/details.aspx?id=53127) 1.15.6150.1 verzió vagy újabb. [Hogyan tooconnect toohello helyszíni adatátjáró a logikai alkalmazás](http://aka.ms/logicapps-gateway) listák hello lépéseket. a folytatás előtt a helyi gépen telepítenie kell hello átjáró.

- A letöltés és telepítés hello legújabb SAP ügyféloldali kódtára a hello azonos számítógépre hello adatátjáró telepítési. Használja a következő SAP verziók hello valamelyikét: 
    - SAP-kiszolgálóhoz
        - Egyetlen SAP-kiszolgálóhoz, hogy támogatási hello (Ice) 3.0-s .NET-összekötő
 
    - SAP-ügyfél
        - SAP .NET Connector (Ice) 3.0

## <a name="add-triggers-and-actions-for-connecting-tooyour-sap-system"></a>Eseményindítók és műveletek tooyour SAP rendszer kapcsolódás hozzáadása

hello SAP összekötő műveleteket, de nem eseményindítók rendelkezik. Igen van toouse egy másik eseményindító hello munkafolyamat hello elején. 

1. Adja hozzá a hello kérelem/válasz eseményindító, majd válassza ki **új lépés**.

2. Válassza ki **művelet hozzáadása**, majd válassza ki a hello SAP összekötő beírásával `SAP` hello keresési mező:    

     ![SAP-alkalmazáskiszolgáló vagy SAP üzenet kiszolgáló kiválasztása](media/logic-apps-using-sap-connector/sap-action.png)

3. Válassza ki [ **SAP alkalmazáskiszolgáló** ](https://wiki.scn.sap.com/wiki/display/ABAP/ABAP+Application+Server) vagy [ **SAP üzenet Server**](http://help.sap.com/saphelp_nw70/helpdata/en/40/c235c15ab7468bb31599cc759179ef/frameset.htm)a SAP-beállításai alapján. Ha nincs meglévő kapcsolat, egy felszólító toocreate áll.

   1. Válassza ki **keresztül, a helyszíni adatátjáró**, és adja meg a SAP rendszer hello részleteit:   

       ![Adja hozzá a kapcsolati karakterlánc tooSAP](media/logic-apps-using-sap-connector/picture2.png)  

   2. A **átjáró**jelöljön ki egy meglévő átjárót, vagy tooinstall egy új átjárót, válassza ki **átjáró telepítése**.

        ![Telepítsen egy új átjárót](media/logic-apps-using-sap-connector/install-gateway.png)
  
   3. Minden hello részletek megadása után válassza ki a **létrehozása**. 
   A Logic Apps konfigurálja, és teszteli hello kapcsolat, annak biztosítása, hogy hello kapcsolat megfelelően működik-e.

4. Adjon meg egy nevet az SAP-kapcsolat.

5. hello különböző SAP beállítások is elérhető. toofind az IDOC kategória, hello listából válassza ki. Vagy manuálisan adja meg a hello elérési utat, és válassza hello HTTP-válasz a hello **törzs** mező:

     ![SAP művelet](media/logic-apps-using-sap-connector/picture3.png)

6. Hello művelet létrehozásához hozzáadása egy **HTTP-válasz**. az SAP-kimenetből hello hello válaszüzenetet kell lennie.

7. Mentse a Logic Apps alkalmazást. Tesztelje úgy, hogy az IDOC keresztül hello HTTP-eseményindító URL-cím küldésekor. Hello IDOC zajlik, után várjon, amíg hello logikai alkalmazás hello válaszát:   

     > [!TIP]
     > Ellenőrizze, hogyan túl[logikai alkalmazások figyelése](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Most, hogy hello SAP összekötő tooyour logikai alkalmazás ad hozzá, Fedezze fel egyéb funkciók:

- BAPI
- RFC

## <a name="get-help"></a>Segítségkérés

tooask kérdések kérdést, és ismerje meg, milyen egyéb Azure Logic Apps felhasználók végzi, látogasson el hello [Azure Logic Apps-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp Azure Logic Apps alkalmazások és összekötők javítása, szavazhatnak, vagy küldje el a következő hello ötleteket [Azure Logic Apps felhasználói visszajelzési webhelyet](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Következő lépések

- Megtudhatja, hogyan toovalidate, átalakítási és más hasonló BizTalk funkciók hello [vállalati integrációs csomag](../logic-apps/logic-apps-enterprise-integration-overview.md). 
- [Csatlakozás tooon helyszíni adatokhoz](../logic-apps/logic-apps-gateway-connection.md) a logic Apps alkalmazásokból
