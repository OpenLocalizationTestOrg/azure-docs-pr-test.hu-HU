---
title: "az Azure Logic Apps fájlrendszerek aaaConnect tooon helyszíni |} Microsoft Docs"
description: "A logic app munkafolyamat hello helyszíni az átjáró és a fájlrendszer összekötő kapcsolódó tooon helyszíni fájlrendszer"
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
ms.openlocfilehash: beb5565293def4aba81f63f19e77d7498aac38c5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooon-premises-file-systems-from-logic-apps-with-hello-file-system-connector"></a>Csatlakozás tooon helyszíni fájlrendszer a logic Apps alkalmazásokból hello fájlrendszer összekötőn keresztül

Hibrid felhő kapcsolat központi toologic alkalmazásokat, így toomanage adatok és a biztonságos hozzáférés a helyszíni erőforrásokkal, a logic apps hello helyszíni data gateway használható. Ebben a cikkben megmutatjuk, hogyan tooconnect tooan helyszíni fájlrendszer egy egyszerű forgatókönyv: másolja át a fájlt meg feltöltött tooDropbox tooa fájlmegosztásba, majd küldje el e-mailt.

## <a name="prerequisites"></a>Előfeltételek

- Telepítse és konfigurálja a hello legújabb [helyszíni adatátjáró](https://www.microsoft.com/download/details.aspx?id=53127).
- Telepíteni a hello legújabb helyszíni data gateway 1.15.6150.1 verzió vagy újabb. [Csatlakozás helyszíni adatátjáró toohello](http://aka.ms/logicapps-gateway) listák hello lépéseket. hello átjáró rendszerre telepíthető egy helyszíni gépen hello további hello lépéseit a folytatás előtt.

## <a name="add-trigger-and-actions-for-connecting-tooyour-file-system"></a>Eseményindító és műveletek tooyour fájlrendszer kapcsolódás hozzáadása

1. Logikai alkalmazás létrehozása, és vegye fel a Dropbox eseményindító: **fájl létrehozásakor** 
2. Hello eseményindító, válassza ki **tovább** > **művelet hozzáadása**. 
3. Hello keresési mezőbe, írja be a `file system` szeretné megjeleníteni az összes támogatott műveletek hello fájlrendszer összekötő.

   ![Keresse meg a fájl összekötő](media/logic-apps-using-file-connector/search-file-connector.png)

2. Válassza ki a hello **létrehozás fájl** művelet, és hozzon létre egy kapcsolat tooyour fájlrendszer.

   Ha nincs meglévő kapcsolat, egy felszólító toocreate áll.

   1. Válasszon **keresztül, a helyszíni adatátjáró**. További tulajdonságok jelennek meg.
   2. Jelölje ki a legfelső szintű mappát, a fájlrendszer.
      
       > [!NOTE]
       > hello gyökérmappája hello fő fölérendelt mappája, relatív útvonalakat minden fájl kapcsolatos műveletekhez használt. Megadhat egy helyi mappába hello gépen, ahol hello helyszíni az átjáró telepítve van, vagy hello mappa lehet egy hálózati megosztást, amelyet a gép hello férhetnek hozzá.

   3. Hello felhasználónév és jelszó megadása hello átjáró.
   4. Válassza ki a korábban telepített hello átjáró.

       ![Kapcsolat beállítása](media/logic-apps-using-file-connector/create-file.png)

3. Miután megadta az összes hello részletes, válassza ki a **létrehozása**. 

   A Logic Apps konfigurálja, és teszteli a hálózati kapcsolatot, annak biztosítása, hogy hello kapcsolat megfelelően működik-e. 
   Ha hello kapcsolat megfelelően van beállítva, beállítások, amelyeket korábban hello a művelethez látható. 
   hello fájl system-összekötő mostantól készen áll a használatra.

4. Adja meg, hogy a Dropbox toohello gyökérmappa toocopy fájlok a helyszíni fájlmegosztás.

   ![Fájl művelet létrehozása](media/logic-apps-using-file-connector/create-file-filled.png)

5. A logic app másolatok hello fájl után egy e-mailt küld a megfelelő felhasználóhoz hello új fájl funkcióját, az Outlook művelet hozzáadása. Adja meg a hello címzetteknek, a jogcímre és az üdvözlő e-mail törzsét. 

   Hello dinamikus tartalom választó, az adatok kimenetek közül választhat hello fájl összekötő, hozzáadhat további részleteket toohello e-mailt.

   ![Küldjön e-mailek művelet](media/logic-apps-using-file-connector/send-email.png)

6. Mentse a Logic Apps alkalmazást. Az alkalmazás tesztelése a fájl tooDropbox feltöltésével. hello fájl kapja meg a másolt toohello helyszíni fájlmegosztást, és hello műveletekre vonatkozó e-mailt kell kapnia.

   > [!TIP] 
   > Ismerje meg, hogyan túl[logikai alkalmazások figyelése](../logic-apps/logic-apps-monitor-your-logic-apps.md).

Gratulálunk, most már rendelkezik, amely képes kapcsolódni a helyi fájlrendszer tooyour működő logikai alkalmazás. Próbálja más összekötő ajánlatokat, például hello elavuló felfedezése:

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

## <a name="view-hello-swagger"></a>Nézet hello swagger
Lásd: hello [részletek swagger](/connectors/fileconnector/). 

## <a name="get-help"></a>Segítségkérés

tooask kérdések kérdést, és ismerje meg, milyen egyéb Azure Logic Apps felhasználók végzi, látogasson el hello [Azure Logic Apps-fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp Azure Logic Apps alkalmazások és összekötők javítása, szavazhatnak, vagy küldje el a következő hello ötleteket [Azure Logic Apps felhasználói visszajelzési webhelyet](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Következő lépések

- [Csatlakozás tooon helyszíni adatokhoz](../logic-apps/logic-apps-gateway-connection.md) a logic Apps alkalmazásokból
- További tudnivalók [vállalati integrációs](../logic-apps/logic-apps-enterprise-integration-overview.md)
