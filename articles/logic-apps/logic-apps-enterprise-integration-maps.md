---
title: "az XSLT-leképezések - Azure Logic Apps XML aaaTransform |} Microsoft Docs"
description: "XSLT-leképezések tootransform XML-adatok az Azure Logic Apps és hello vállalati integrációs csomag hozzáadása"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 90f5cfc4-46b2-4ef7-8ac4-486bb0e3f289
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.openlocfilehash: 720e6f988b8542136dfcc402c3c463fcfb2f23cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-maps-for-xml-data-transform"></a>Az XML-adatok átalakító maps hozzáadása

Vállalati integrációs használ a maps tootransform XML-adatok között. A térkép az XML-dokumentum, egy dokumentumot, amely egy másik formátumra alakítja át kell hello adatok definiáló. 

## <a name="why-use-maps"></a>A maps miért érdemes használni?

Tegyük fel, hogy rendszeresen kapott B2B rendeléseket és a számlák hello YYYMMDD formátumot használja a dátumok ügyfélnek. Azonban a szervezet tárolási hello MMDDYYY formátumú dátum. Használhatja a térkép túl*átalakítási* hello YYYMMDD dátumformátum hello MMDDYYY előtt hello sorrend, illetve a Számla részletek tárolni a felhasználói tevékenység adatbázisban történő.

## <a name="how-do-i-create-a-map"></a>Hogyan térkép létrehozásához?

BizTalk integrációs projekteket hozhat létre hello [vállalati integrációs csomag](logic-apps-enterprise-integration-overview.md "további információ a hello vállalati integrációs csomag") a Visual Studio 2015. Ezután létrehozhat olyan integrációs térképe fájlt, amely lehetővé teszi az elemek között két XML-sémafájlok vizuálisan hozzárendelését. Ez a projekt létrehozása után fog az XSLT-dokumentum.

## <a name="how-do-i-add-a-map"></a>Hogyan vegyen fel egy társítást?

1. Hello Azure-portálon, válassza ki **Tallózás**.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Hello szűrő keresési mezőbe, írja be a **integrációs**, majd jelölje be **integrációs fiókok** hello eredmények listából.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Válassza ki a hello integrációs fiókra, amelyhez tooadd hello leképezés.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Jelölje be hello **Maps** csempére.

    ![](./media/logic-apps-enterprise-integration-maps/map-1.png)

5. Miután hello Maps panel nyílik meg, válassza ki azt **Hozzáadás**.

    ![](./media/logic-apps-enterprise-integration-maps/map-2.png)  

6. Adjon meg egy **neve** a térképen. tooupload hello térkép fájlt, válassza ki a hello ikonja hello hello jobb oldalán **térkép** szövegmezőben. Hello feltöltési folyamat befejezése után válassza ki a **OK**.

    ![](./media/logic-apps-enterprise-integration-maps/map-3.png)

7. Miután az Azure ad hello térkép tooyour integrációs fiók, bemutatja, hogy a leképezés vagy nem lett-e adva képernyőn üzenetet kap. Miután ezt az üzenetet kap, válassza ki azt a hello **Maps** csempe szeretné megjeleníteni az hello újonnan hozzáadott leképezés.

    ![](./media/logic-apps-enterprise-integration-maps/map-4.png)

## <a name="how-do-i-edit-a-map"></a>Hogyan szerkeszthetők a térkép?

Új leképezési fájl hello módosítások kell feltöltenie. Először letöltheti hello térkép szerkesztésre.

tooupload új leképezés, amely kiváltja a hello meglévő térkép, kövesse az alábbi lépéseket.

1. Válassza ki a hello **Maps** csempére.

2. Után hello Maps panel nyílik meg, válassza ki a megjeleníteni kívánt tooedit hello leképezés.

3. A hello **Maps** paneljén válassza **frissítés**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-1.png)

4. A hello fájlválasztó, válassza ki a hello nyilvántartását fájlját, hogy szeretné, hogy tooupload, majd válassza ki **nyitott**.

    ![](./media/logic-apps-enterprise-integration-maps/edit-2.png)

## <a name="how-toodelete-a-map"></a>Hogyan toodelete térképre?

1. Válassza ki a hello **Maps** csempére.

2. Után hello Maps panel nyílik meg, válassza ki a kívánt toodelete hello leképezés.

3. Válasszon **törlése**.

    ![](./media/logic-apps-enterprise-integration-maps/delete.png)

4. Győződjön meg arról, hogy szeretné-e toodelete hello leképezés.

    ![](./media/logic-apps-enterprise-integration-maps/delete-confirmation-1.png)

## <a name="next-steps"></a>Következő lépések
* [További tudnivalók a vállalati integrációs csomag hello](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag")  
* [További információ a megállapodások](../logic-apps/logic-apps-enterprise-integration-agreements.md "vállalati integrációs megállapodások ismertetése")  
* [További tudnivalók átalakítások](logic-apps-enterprise-integration-transform.md "vállalati integrációs átalakítások ismertetése")  

