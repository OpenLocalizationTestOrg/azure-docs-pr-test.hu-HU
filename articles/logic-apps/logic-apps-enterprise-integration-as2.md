---
title: "AS2-üzenetek B2B vállalati integrációs - Azure Logic Apps |} Microsoft Docs"
description: "Az Azure Logic Apps B2B vállalati integrációs Exchange AS2-üzenetek"
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: c9b7e1a9-4791-474c-855f-988bd7bf4b7f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/08/2017
ms.author: LADocs; mandia
ms.openlocfilehash: 91b2f16611b88aa4b9395ca301d88042065ad9dd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="exchange-as2-messages-for-enterprise-integration-with-logic-apps"></a>A logic apps vállalati integrációs Exchange AS2-üzenetek

Az Azure Logic Apps továbbíthatja az AS2-üzenetek, mielőtt AS2-egyezmény létrehozása, és megállapodás tárolása integrációs fiókját. Az alábbiakban a szükséges lépéseket az AS2-egyezmény létrehozása.

## <a name="before-you-start"></a>Előkészületek

A szükséges elemeket itt található:

* Egy [integrációs fiók](../logic-apps/logic-apps-enterprise-integration-accounts.md) , amely már definiált és az Azure-előfizetéshez társított
* Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) már definiált integrációs fiókját, és, amely alatt az AS2 minősítő konfigurálva **üzleti identitások**

> [!NOTE]
> Amikor egy szerződést hoz létre, a fájl tartalma a szerződés típusának meg kell egyeznie.    

Miután [integrációs-fiók létrehozása](../logic-apps/logic-apps-enterprise-integration-accounts.md) és [adja hozzá a partnerek](logic-apps-enterprise-integration-partners.md), AS2 szerződés ezeket a lépéseket követve hozhat létre.

## <a name="create-an-as2-agreement"></a>AS2-egyezmény létrehozása

1.  Jelentkezzen be az [Azure Portalra](http://portal.azure.com "Azure Portal")  

2.  A bal oldali menüben válassza ki a **további szolgáltatások**. A keresési mezőbe, írja be a **integrációs** szűrőként. Az eredmények listájában válassza **integrációs fiókok**.

    > [!TIP]
    > Ha nem lát **további szolgáltatások**, lehetséges, hogy először bontsa ki a menüben. Jelölje be a becsukott menü felső részén **megjelenítése menü**.

    ![További szolgáltatásokat, szűrőt a "integrációt", "Integrációs fiókok" kiválasztása](./media/logic-apps-enterprise-integration-agreements/overview-1.png)

3. Az a **integrációs fiókok** panelt megnyitó, válassza ki az integráció fiókra, amelyhez a szerződést létrehozásához.
Ha nem lát minden integrációs fiókok [hozzon létre egyet első](../logic-apps/logic-apps-enterprise-integration-accounts.md "integrációs fiókokkal kapcsolatos összes").  

    ![Válassza ki a integrációs fiókot a szerződés létrehozásának a helyét](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Válassza ki a **megállapodások** csempére. Ha egy megállapodások csempe nem rendelkezik, először vegye fel a csempe.

    ![Válassza a "Szerződés" csempe](./media/logic-apps-enterprise-integration-agreements/agreement-1.png)

5. Szerződések paneljén válassza **Hozzáadás**.

    ![Válassza a "Hozzáadás"](./media/logic-apps-enterprise-integration-agreements/agreement-2.png)

6. A **Hozzáadás**, adjon meg egy **neve** a szerződés. A **szerződés típusának**, jelölje be **AS2**. Válassza ki a **fogadó Partner**, **gazdagép identitását**, **Vendég Partner**, és **Vendég identitás** a szerződés.

    ![Adja meg a szerződés részletei](./media/logic-apps-enterprise-integration-agreements/agreement-3.png)  

    | Tulajdonság | Leírás |
    | --- | --- |
    | Név |A szerződés nevét |
    | A szerződés típusa | AS2 kell lennie. |
    | Fogadó Partner |Egy szerződést kell a gazdagép és a Vendég partner. A fogadó partner szervezet, amely beállítja a szerződés jelöli. |
    | Gazdagép identitását |A fogadó partner azonosítója |
    | Vendég Partner |Egy szerződést kell a gazdagép és a Vendég partner. A Vendég partnert a szervezet, amely a gazdagép partnerrel üzleti jelöli. |
    | Vendég identitás |A Vendég partner azonosítója |
    | Beállítások |Minden szerződés által fogadott üzenetek alkalmazni ezeket a tulajdonságokat. |
    | Beállítások küldése |Ezeket a tulajdonságokat a szerződés által küldött összes üzenet vonatkozik. |

## <a name="configure-how-your-agreement-handles-received-messages"></a>Hogyan a szerződés leírók fogadott üzenetek konfigurálása

Most, hogy a szerződés tulajdonságok beállítása, beállíthatja a módját a jelen szerződés azonosítja, és kezeli a bejövő üzenetek és a szerződés partnerétől kapott.

1.  A **Hozzáadás**, jelölje be **fogadási beállítások**.
Állítsa be ezeket a tulajdonságokat a partnerrel, amely az üzenetek Önnel a szerződés alapján. Tulajdonságleírások lásd: a tábla ebben a szakaszban.

    ![Konfigurálja a "Kapják meg a beállításokat"](./media/logic-apps-enterprise-integration-agreements/agreement-4.png)

2. Szükség esetén felülírják a bejövő üzenetek tulajdonságainak kiválasztásával **bírálja felül az üzenet tulajdonságai**.

3. Az összes bejövő üzenetet alá kell írni, jelölje be a jelölőnégyzetet **üzenetet alá kell írni**. Az a **tanúsítvány** listára, válassza ki egy meglévő [Vendég partner nyilvános tanúsítvány](../logic-apps/logic-apps-enterprise-integration-certificates.md) az üzenetek az aláírás ellenőrzéséhez. Vagy a tanúsítvány létrehozása, ha még nem rendelkezik ilyennel.

4.  Az összes bejövő üzenet titkosításához szükséges, jelölje be **üzenet titkosítani**. Az a **tanúsítvány** listára, válassza ki egy meglévő [állomás partner személyes tanúsítvány](../logic-apps/logic-apps-enterprise-integration-certificates.md) tartalomkulcsot a bejövő üzenetek. Vagy a tanúsítvány létrehozása, ha még nem rendelkezik ilyennel.

5. Üzenetek tömöríthetők szükséges, jelölje be **üzenet kell tömörített**.

6. Szinkron üzenet törlése értesítés (MDN) fogadott üzenetek küldése, válassza ki a **küldése MDN**.

7. Aláírt MDNs érkező üzenetek küldéséhez válasszon **küldési aláírt MDN**.

8. Aszinkron MDNs érkező üzenetek küldéséhez válasszon **aszinkron MDN küldése**.

9. Miután elkészült, ügyeljen arra, hogy a beállítások mentéséhez válasszon **OK**.

Most már a szerződés készen áll a bejövő üzenetek, amelyek megfelelnek a kiválasztott beállítások kezeléséhez.

| Tulajdonság | Leírás |
| --- | --- |
| Bírálja felül az üzenet tulajdonságai |Azt jelzi, hogy a Beérkezett üzenetek tulajdonságok bírálható felül. |
| Az üzenetet alá kell írni |Szükséges az üzenetek digitális aláírását. Állítsa be a Vendég partner nyilvános tanúsítványt aláírás-ellenőrzésre.  |
| Az üzenetnek titkosítottnak kell lennie |Az üzenetek titkosításához szükséges. A nem titkosított üzenetek utasítja el. A gazdagép partner személyes tanúsítvány konfigurálása az üzenetek visszafejtésére.  |
| Az üzenetnek tömörítettnek kell lennie |Üzenetek tömöríthetők igényel. Nem-tömörített üzenetek utasítja el. |
| MDN szöveg |Az alapértelmezett üzenet törlése értesítés (MDN) az üzenet küldője küldendő. |
| MDN küldése |Küldendő MDNs igényel. |
| Aláírt MDN küldése |Szükséges MDNs kell aláírni. |
| MIC algoritmus |Válassza ki az üzenetek aláírására használt algoritmust. |
| Aszinkron MDN küldése | Üzenetek küldését aszinkron módon van szükség. |
| URL-CÍME | Adja meg, hova küldje a MDNs URL-CÍMÉT. |

## <a name="configure-how-your-agreement-sends-messages"></a>Hogyan a szerződés küld üzeneteket konfigurálása

Beállíthatja, hogyan a jelen szerződés azonosítja, és a partnerek számára, és ez a szerződés elküldött kimenő üzenetek kezeli.

1.  A **Hozzáadás**, jelölje be **küldési beállítások**.
Állítsa be ezeket a tulajdonságokat a partnerrel, amely az üzenetek Önnel a szerződés alapján. Tulajdonságleírások lásd: a tábla ebben a szakaszban.

    ![A "Küldési beállítások" tulajdonságainak beállítása](./media/logic-apps-enterprise-integration-agreements/agreement-51.png)

2. Aláírt üzeneteket küldhet partnere, válassza ki a **üzenet aláírása engedélyezése**. Az üzenetek aláírására a a **MIC algoritmus** listáról válassza ki a *állomás partner személyes tanúsítvány MIC algoritmus*. A a **tanúsítvány** listára, válassza ki egy meglévő [állomás partner személyes tanúsítvány](../logic-apps/logic-apps-enterprise-integration-certificates.md).

3. Válassza ki a titkosított üzeneteket küldhet a partner, **üzenettitkosítás engedélyezése**. Az üzenetek titkosításához a a **titkosítási algoritmus** listáról válassza ki a *Vendég partner nyilvános tanúsítvány algoritmus*.
A a **tanúsítvány** listára, válassza ki egy meglévő [Vendég partner nyilvános tanúsítvány](../logic-apps/logic-apps-enterprise-integration-certificates.md).

4. Az üzenet tömörítéséhez válassza **üzenet tömörítésének engedélyezéséhez**.

5. A HTTP content-type fejléc unfold be egy sorba, jelölje be **Unfold HTTP-fejlécek**.

6. Szinkron MDNs a küldött üzenetek fogadására, válassza ki a **kérelem MDN**.

7. Aláírt MDNs a küldött üzenetek fogadására, válassza ki a **kérelem aláírt MDN**.

8. A küldött üzenetek aszinkron MDNs fogadásához válassza **aszinkron MDN kérelem**. Ha ezt a beállítást, adja meg a hova küldje a MDNs URL-CÍMÉT.

9. Nem megtagadás fogadását az szükséges, jelölje be **engedélyezése NRR**.  

10. Adja meg a MIC vagy az AS2 üzenetet vagy MDN kimenő fejlécébe aláírási algoritmus formátumát, **SHA2 algoritmus formátum**.  

11. Miután elkészült, ügyeljen arra, hogy a beállítások mentéséhez válasszon **OK**.

Most már a szerződés készen áll a kimenő üzenetek, amelyek megfelelnek a kiválasztott beállítások kezeléséhez.

| Tulajdonság | Leírás |
| --- | --- |
| Üzenet aláírása engedélyezése |A szerződés aláírt érkező összes üzenet igényel. |
| MIC algoritmus |Az üzenetek aláírására használt algoritmust. A gazdagép partner személyes tanúsítvány MIC algoritmus konfigurálja az üzenetek aláírására. |
| Tanúsítvány |Válassza ki az üzenetek aláíráshoz használandó tanúsítványt. Konfigurálja a gazdagép partner titkos tanúsítványának az üzenetek aláírására. |
| Üzenettitkosítás engedélyezése |A jelen szerződés küldött összes üzenet-titkosítást igényel. Konfigurálja a Vendég partner nyilvános tanúsítvány algoritmus az üzenetek titkosítására. |
| Titkosítási algoritmus |A titkosítási algoritmust a üzenet titkosításához. Konfigurálja a Vendég partner nyilvános tanúsítvány az üzenetek titkosítására. |
| Tanúsítvány |Az üzenetek titkosításához használni kívánt tanúsítványt. Konfigurálja a Vendég partner személyes tanúsítvány az üzenetek titkosítására. |
| Üzenet tömörítésének engedélyezése |A jelen szerződés küldött összes üzenet tömörítési igényel. |
| HTTP-fejlécek unfold |A HTTP content-type fejléc alakzatot egy sorba helyezi. |
| Kérelem MDN |A jelen szerződés küldött összes üzenet egy MDN igényel. |
| Aláírt MDN kérése |Ez a szerződés aláírt küldött összes MDNs igényel. |
| Aszinkron MDN kérése |Ez a szerződés küldendő aszinkron MDNs igényel. |
| URL-CÍME |Adja meg, hova küldje a MDNs URL-CÍMÉT. |
| NRR engedélyezése |Nem megtagadás átvételének (NRR), egy kommunikációs attribútumot, amelyet bizonyítékait is igényel, amely a adatokat fogadta a program, mert a címzett. |
| 2 algoritmust formátumban |Válassza ki a MIC vagy az AS2 üzenetet vagy MDN kimenő fejlécébe aláírási algoritmus formátumát |

## <a name="find-your-created-agreement"></a>A létrehozott megállapodás keresése

1.  A szerződés tulajdonságainak a beállítása után a **Hozzáadás** paneljén válassza **OK** hozta létre a szerződést, és az integráció fiók panelen való visszatéréshez.

    Az újonnan hozzáadott foglalt most megjelenik a **megállapodások** listája.

2.  Az integrációs fiókok áttekintése is megtekintheti a szerződéseket. Az integráció-fiók panelen válassza **áttekintése**, majd jelölje be a **megállapodások** csempére. 

    ![Válassza a minden szerződés megtekintéséhez egymás "Szerződés"](./media/logic-apps-enterprise-integration-agreements/agreement-6.png)

## <a name="view-the-swagger"></a>A swagger megtekintése
Tekintse meg a [részletek swagger](/connectors/as2/). 

## <a name="next-steps"></a>Következő lépések
* [További tudnivalók a vállalati integrációs csomag](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag")  
