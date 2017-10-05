---
title: "A helyszíni fájlrendszer csatlakozni az Azure Logic Apps |} Microsoft Docs"
description: "A logic app munkafolyamat a helyszíni adatátjáró és fájlrendszer összekötő a helyszíni fájl rendszerekhez történő csatlakozáshoz"
keywords: "fájlrendszer"
services: logic-apps
author: derek1ee
manager: anneta
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/27/2017
ms.author: LADocs; deli
ms.openlocfilehash: f33e7c58103c57e17e4e273caba1ab9b83f0cd2b
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-on-premises-file-systems-from-logic-apps-with-the-file-system-connector"></a>Csatlakozás helyszíni fájlrendszer a logic Apps alkalmazásokból, a fájlrendszer-összekötőn keresztül

Hibrid felhő kapcsolat központi helyet foglalnak el a logic apps, így az adatok kezelése és biztonságos hozzáférés a helyszíni erőforrásokhoz, a logic apps segítségével az helyszíni átjáró. Ebben a cikkben megmutatjuk, hogyan csatlakozhat egy helyszíni fájlrendszer egy egyszerű forgatókönyv: másolja át a fájlt, amely fájlmegosztásba a dropbox alkalmazásba feltöltött, majd küldje el e-mailt.

## <a name="prerequisites"></a>Előfeltételek

- Telepítse és konfigurálja a legújabb [helyszíni adatátjáró](https://www.microsoft.com/download/details.aspx?id=53127).
- Telepítse a legújabb a helyszíni adatok gateway, 1.15.6150.1 verzió vagy újabb. [A helyszíni adatok átjárón](http://aka.ms/logicapps-gateway) lépéseit sorolja fel. A további lépéseket a folytatás előtt a helyi gépen telepítenie kell az átjárót.

## <a name="add-trigger-and-actions-for-connecting-to-your-file-system"></a>Adja hozzá eseményindító és műveleteket a fájlrendszer való kapcsolódáshoz

1. Logikai alkalmazás létrehozása, és vegye fel a Dropbox eseményindító: **fájl létrehozásakor** 
2. Válassza ki az eseményindító **tovább** > **művelet hozzáadása**. 
3. A keresési mezőbe, írja be a `file system` szeretné megjeleníteni az összes támogatott műveletek a fájlrendszer-összekötőhöz.

   ![Keresse meg a fájl összekötő](media/logic-apps-using-file-connector/search-file-connector.png)

2. Válassza ki a **létrehozás fájl** művelet, és a fájlrendszerhez kapcsolat létrehozásához.

   Ha nincs meglévő kapcsolat, a rendszer kéri a hozzon létre egyet.

   1. Válasszon **keresztül, a helyszíni adatátjáró**. További tulajdonságok jelennek meg.
   2. Jelölje ki a legfelső szintű mappát, a fájlrendszer.
      
       > [!NOTE]
       > A legfelső szintű mappa nem a fő szülőmappa, relatív útvonalakat minden fájl kapcsolatos műveletekhez használt. Megadhat egy helyi mappába a számítógépen, ahol telepítve van a helyszíni data gateway, vagy a mappa lehet a számítógép által elérhető hálózati megosztáson.

   3. Adja meg a felhasználónevet és jelszót az átjárót.
   4. Válassza ki a korábban telepített átjárót.

       ![Kapcsolat beállítása](media/logic-apps-using-file-connector/create-file.png)

3. Miután megadta a részleteit, válassza ki a **létrehozása**. 

   A Logic Apps konfigurálja, és teszteli a hálózati kapcsolatot, annak biztosítása, hogy a kapcsolat megfelelően működik-e. 
   Ha a kapcsolat megfelelően van beállítva, lásd: a korábban kiválasztott művelet beállításait. 
   A fájl system-összekötő mostantól készen áll a használatra.

4. Adja meg, hogy szeretné-e fájlok másolása a Dropbox a helyszíni fájlmegosztás gyökérmappájába.

   ![Fájl művelet létrehozása](media/logic-apps-using-file-connector/create-file-filled.png)

5. Miután a Logic Apps alkalmazást átmásolja a fájlt, egy e-mailt küld, így az új fájl megfelelő felhasználók funkcióját, az Outlook művelet hozzáadása. Adja meg a címzetteket, cím és az e-mail. 

   A dinamikus tartalom választó, az adatok kimenetek közül választhat a fájl összekötő, hozzáadhat további részleteket az e-mailt.

   ![Küldjön e-mailek művelet](media/logic-apps-using-file-connector/send-email.png)

6. Mentse a Logic Apps alkalmazást. Tesztelje az alkalmazás feltölteni a fájlt a dropbox alkalmazásba. A helyszíni fájlmegosztáshoz beolvasása átmásolhatja a fájlt, és a műveletekre vonatkozó e-mailt kell kapnia.

   > [!TIP] 
   > Megtudhatja, hogyan [logikai alkalmazások figyelése](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Gratulálunk, most már rendelkezik egy működő logikai alkalmazás, amely képes csatlakozni a helyszíni fájlrendszer. Próbálja meg az összekötő kínál, például más elavuló felfedezése:

- Fájl létrehozása
- Mappában lévő fájlok listázása
- Fájl hozzáfűzése
- Fájl törlése
- Fájl tartalmának lekérdezése
- Fájl tartalmának beolvasása elérési út segítségével
- Fájl metaadatot beszerezni
- Fájlmetaadatok beolvasása elérési út segítségével
- Gyökérmappában lévő fájlok listázása
- Fájl frissítése

## <a name="view-the-swagger"></a>A swagger megtekintése
Tekintse meg a [részletek swagger](/connectors/fileconnector/). 

## <a name="get-help"></a>Segítségkérés

Látogasson el az [Azure Logic Apps fórumára](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps), ahol kérdéseket tehet fel és válaszolhat meg, valamint megtudhatja, mivel foglalkoznak az Azure Logic Apps más felhasználói.

Ha szeretné segíteni az Azure Logic Apps és összekötők fejlesztését, szavazzon az ötletekre az [Azure Logic Apps felhasználói visszajelzések oldalán](http://aka.ms/logicapps-wish), vagy küldje be saját javaslatait.

## <a name="next-steps"></a>Következő lépések

- [A helyszíni adatokhoz](../logic-apps/logic-apps-gateway-connection.md) a logic Apps alkalmazásokból
- További tudnivalók [vállalati integrációs](../logic-apps/logic-apps-enterprise-integration-overview.md)
