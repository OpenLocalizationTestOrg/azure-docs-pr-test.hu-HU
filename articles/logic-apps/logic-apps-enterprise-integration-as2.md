---
title: "aaaAS2 üzenetek B2B vállalati integrációs - Azure Logic Apps |} Microsoft Docs"
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
ms.openlocfilehash: 23f9d74dd0ad807e9cdaedb320d60496cfef9bc3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="exchange-as2-messages-for-enterprise-integration-with-logic-apps"></a>A logic apps vállalati integrációs Exchange AS2-üzenetek

Az Azure Logic Apps továbbíthatja az AS2-üzenetek, mielőtt AS2-egyezmény létrehozása, és megállapodás tárolása integrációs fiókját. Az alábbiakban hello szükséges lépéseket az AS2 toocreate szerződést.

## <a name="before-you-start"></a>Előkészületek

Itt van szüksége hello elemeket:

* Egy [integrációs fiók](../logic-apps/logic-apps-enterprise-integration-accounts.md) , amely már definiált és az Azure-előfizetéshez társított
* Legalább két [partnerek](logic-apps-enterprise-integration-partners.md) , amely már definiált integrációs fiókjában és hello AS2 minősítő csoportban konfigurált **üzleti identitások**

> [!NOTE]
> Amikor egy szerződést hoz létre, hello megállapodás fájl tartalmának hello hello szerződés típusának meg kell egyeznie.    

Miután [integrációs-fiók létrehozása](../logic-apps/logic-apps-enterprise-integration-accounts.md) és [adja hozzá a partnerek](logic-apps-enterprise-integration-partners.md), AS2 szerződés ezeket a lépéseket követve hozhat létre.

## <a name="create-an-as2-agreement"></a>AS2-egyezmény létrehozása

1.  Jelentkezzen be toohello [Azure-portálon](http://portal.azure.com "Azure-portálon").  

2.  Hello bal oldali menüben válassza ki a **további szolgáltatások**. Hello keresési mezőbe, írja be a **integrációs** szűrőként. Hello eredmények listában válassza ki a **integrációs fiókok**.

    > [!TIP]
    > Ha nem lát **további szolgáltatások**, lehetséges, hogy tooexpand hello menü először. Hello összecsukott hello menü felső részén válassza **megjelenítése menü**.

    ![További szolgáltatásokat, szűrőt a "integrációt", "Integrációs fiókok" kiválasztása](./media/logic-apps-enterprise-integration-agreements/overview-1.png)

3. A hello **integrációs fiókok** panelt megnyitó, jelölje be hello integrációs fiókra, amelyhez toocreate hello szerződést.
Ha nem lát minden integrációs fiókok [hozzon létre egyet első](../logic-apps/logic-apps-enterprise-integration-accounts.md "integrációs fiókokkal kapcsolatos összes").  

    ![Ha toocreate hello megállapodás hello integrációs fiók kiválasztása](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Válassza ki a hello **megállapodások** csempére. Ha egy megállapodások csempe nem rendelkezik, először adjon hozzá hello csempe.

    ![Válassza a "Szerződés" csempe](./media/logic-apps-enterprise-integration-agreements/agreement-1.png)

5. Hello megállapodások paneljén megjelenő válassza **Hozzáadás**.

    ![Válassza a "Hozzáadás"](./media/logic-apps-enterprise-integration-agreements/agreement-2.png)

6. A **Hozzáadás**, adjon meg egy **neve** a szerződés. A **szerződés típusának**, jelölje be **AS2**. Jelölje be hello **fogadó Partner**, **gazdagép identitását**, **Vendég Partner**, és **Vendég identitás** a szerződés.

    ![Adja meg a szerződés részletei](./media/logic-apps-enterprise-integration-agreements/agreement-3.png)  

    | Tulajdonság | Leírás |
    | --- | --- |
    | Név |Hello szerződés neve |
    | A szerződés típusa | AS2 kell lennie. |
    | Fogadó Partner |Egy szerződést kell a gazdagép és a Vendég partner. hello fogadó partner által konfigurált házirendsablonok hello megállapodás hello szervezet jelöli. |
    | Gazdagép identitását |Hello fogadó partner azonosítója |
    | Vendég Partner |Egy szerződést kell a gazdagép és a Vendég partner. hello Vendég partner hello szervezet, amely hello állomás partnerrel üzleti jelöli. |
    | Vendég identitás |Hello Vendég partner azonosítója |
    | Beállítások |Ezeket a tulajdonságokat a szerződés által fogadott üzenetek küldési tooall alkalmazni. |
    | Beállítások küldése |Ezek a Tulajdonságok tooall üzeneteket a szerződés vonatkozik. |

## <a name="configure-how-your-agreement-handles-received-messages"></a>Hogyan a szerződés leírók fogadott üzenetek konfigurálása

Most, hogy hello megállapodás tulajdonságok beállítása, beállíthatja a módját a jelen szerződés azonosítja, és kezeli a bejövő üzenetek és a szerződés partnerétől kapott.

1.  A **Hozzáadás**, jelölje be **fogadási beállítások**.
Állítsa be ezeket a tulajdonságokat a szerződés hello partnerrel, amely az üzenetek Önnel alapján. Tulajdonságleírások lásd: hello tábla ebben a szakaszban.

    ![Konfigurálja a "Kapják meg a beállításokat"](./media/logic-apps-enterprise-integration-agreements/agreement-4.png)

2. Szükség esetén felülírják a bejövő üzenetek hello tulajdonságok kiválasztásával **bírálja felül az üzenet tulajdonságai**.

3. toorequire minden bejövő üzenetek toobe aláírt, válassza a **üzenetet alá kell írni**. A hello **tanúsítvány** listára, válassza ki egy meglévő [Vendég partner nyilvános tanúsítvány](../logic-apps/logic-apps-enterprise-integration-certificates.md) köszönőüzenetei hello aláírása ellenőrzéséhez. Vagy hozzon létre hello tanúsítványt, ha még nem rendelkezik ilyennel.

4.  toorequire minden bejövő üzenetek toobe titkosított, válasszon **üzenet titkosítani**. A hello **tanúsítvány** listára, válassza ki egy meglévő [állomás partner személyes tanúsítvány](../logic-apps/logic-apps-enterprise-integration-certificates.md) tartalomkulcsot a bejövő üzenetek. Vagy hozzon létre hello tanúsítványt, ha még nem rendelkezik ilyennel.

5. toorequire üzenetek toobe tömörített, válassza ki **üzenet kell tömörített**.

6. szinkron törlése értesítő üzenetet (MDN) fogadott üzenetek toosend kiválasztása **küldése MDN**.

7. toosend MDNs aláírt fogadott üzenetek, jelölje be a **küldési aláírt MDN**.

8. toosend érkező üzenetek aszinkron MDNs kiválasztása **aszinkron MDN küldése**.

9. Miután elkészült, győződjön meg arról, hogy toosave a beállítások kiválasztásával **OK**.

Mostantól a szerződés készen toohandle bejövő üzenetek tooyour tartó kiválasztott beállításokat.

| Tulajdonság | Leírás |
| --- | --- |
| Bírálja felül az üzenet tulajdonságai |Azt jelzi, hogy a Beérkezett üzenetek tulajdonságok bírálható felül. |
| Az üzenetet alá kell írni |Digitális aláírással üzenetek toobe igényel. Hello Vendég partner nyilvános tanúsítvány konfigurálása aláírás-ellenőrzés.  |
| Az üzenetnek titkosítottnak kell lennie |Titkosított üzenetek toobe igényel. A nem titkosított üzenetek utasítja el. Hello állomás partner személyes tanúsítvány konfigurálása köszönőüzenetei visszafejtése.  |
| Az üzenetnek tömörítettnek kell lennie |Tömörített üzenetek toobe igényel. Nem-tömörített üzenetek utasítja el. |
| MDN szöveg |hello alapértelmezett üzenet törlése (MDN) értesítési toobe toohello üzenet küldőjének küldött. |
| MDN küldése |Küldött MDNs toobe igényel. |
| Aláírt MDN küldése |Aláírt MDNs toobe igényel. |
| MIC algoritmus |Válassza ki a hello algoritmus toouse üzenetek aláírására. |
| Aszinkron MDN küldése | Aszinkron módon küldött üzenetek toobe igényel. |
| URL-CÍME | Adja meg, ahol toosend hello MDNs hello URL-CÍMÉT. |

## <a name="configure-how-your-agreement-sends-messages"></a>Hogyan a szerződés küld üzeneteket konfigurálása

Hogyan a jelen szerződés azonosítja, és kezeli a kimenő üzenetek küldését a jelen szerződés tooyour partnerekkel is konfigurálhatja.

1.  A **Hozzáadás**, jelölje be **küldési beállítások**.
Állítsa be ezeket a tulajdonságokat a szerződés hello partnerrel, amely az üzenetek Önnel alapján. Tulajdonságleírások lásd: hello tábla ebben a szakaszban.

    ![Hello "Küldése beállítások" tulajdonságainak beállítása](./media/logic-apps-enterprise-integration-agreements/agreement-51.png)

2. toosend aláírt üzenetek tooyour partner, jelölje be **üzenet aláírása engedélyezése**. Az aláíráshoz köszönőüzenetei hello a **MIC algoritmus** listában, jelölje be hello *állomás partner személyes tanúsítvány MIC algoritmus*. A hello **tanúsítvány** listára, válassza ki egy meglévő [állomás partner személyes tanúsítvány](../logic-apps/logic-apps-enterprise-integration-certificates.md).

3. toosend titkosított üzenetek toohello partner, jelölje be **üzenettitkosítás engedélyezése**. Titkosítási köszönőüzenetei, a hello **titkosítási algoritmus** listában, jelölje be hello *Vendég partner nyilvános tanúsítvány algoritmus*.
A hello **tanúsítvány** listára, válassza ki egy meglévő [Vendég partner nyilvános tanúsítvány](../logic-apps/logic-apps-enterprise-integration-certificates.md).

4. toocompress üdvözlőüzenetére, jelölje be **üzenet tömörítésének engedélyezéséhez**.

5. Egysoros, válassza ki azokat a toounfold hello HTTP content-type fejléc **Unfold HTTP-fejlécek**.

6. tooreceive az elküldött üzenetek, jelölje be a hello szinkron MDNs **kérelem MDN**.

7. tooreceive MDNs aláírt hello elküldött üzenetek, jelölje be a **kérelem aláírt MDN**.

8. tooreceive az elküldött üzenetek, jelölje be a hello aszinkron MDNs **aszinkron MDN kérelem**. Ha ezt a beállítást, ha toosend hello MDNs meg hello URL-CÍMÉT.

9. Válassza ki a toorequire nem hitelesítettek fogadását, **engedélyezése NRR**.  

10. toospecify algoritmus formátum toouse a hello MIC vagy a bejelentkezés hello kimenő fejléceket AS2 üdvözlőüzenetére vagy MDN, válassza ki a **SHA2 algoritmus formátum**.  

11. Miután elkészült, győződjön meg arról, hogy toosave a beállítások kiválasztásával **OK**.

A szerződés most már készen áll a kimenő üzenetek tooyour kiválasztott beállítások tartó toohandle áll.

| Tulajdonság | Leírás |
| --- | --- |
| Üzenet aláírása engedélyezése |Minden aláírt hello megállapodás toobe a küldött üzenetek igényel. |
| MIC algoritmus |hello algoritmus toouse üzenetek aláírására. Konfigurálja a hello állomás partner személyes tanúsítvány MIC algoritmus köszönőüzenetei aláírásra. |
| Tanúsítvány |Válassza ki a hello tanúsítvány toouse üzenetek aláírására. Konfigurálja az hello állomás partner személyes tanúsítvány köszönőüzenetei aláírásra. |
| Üzenettitkosítás engedélyezése |A jelen szerződés küldött összes üzenet-titkosítást igényel. Hello Vendég partner nyilvános tanúsítvány titkosításához használt algoritmus köszönőüzenetei konfigurálja. |
| Titkosítási algoritmus |hello titkosítási algoritmus toouse a üzenet titkosításához. Hello Vendég partner nyilvános titkosítási tanúsítvány köszönőüzenetei konfigurálja. |
| Tanúsítvány |tanúsítvány toouse tooencrypt köszönőüzenetei. Konfigurálja az hello Vendég partner személyes tanúsítvány hello üzenetek titkosítására. |
| Üzenet tömörítésének engedélyezése |A jelen szerződés küldött összes üzenet tömörítési igényel. |
| HTTP-fejlécek unfold |Hello HTTP content-type fejléc alakzatot egy sorba helyezi. |
| Kérelem MDN |A jelen szerződés küldött összes üzenet egy MDN igényel. |
| Aláírt MDN kérése |Toothis megállapodás toobe küldött összes MDNs igényel. |
| Aszinkron MDN kérése |Aszinkron MDNs küldött toobe toothis szerződés szükséges. |
| URL-CÍME |Adja meg, ahol toosend hello MDNs hello URL-CÍMÉT. |
| NRR engedélyezése |Szükséges fogadását (NRR), egy kommunikációs attribútumot, amelyet bizonyítékait hello adatok nem megtagadás érkezett, címzett. |
| 2 algoritmust formátumban |Válassza ki a algoritmus formátum toouse hello MIC vagy kimenő fejléceket AS2 üdvözlőüzenetére vagy MDN hello bejelentkezés |

## <a name="find-your-created-agreement"></a>A létrehozott megállapodás keresése

1.  Hello a szerződés tulajdonságok beállítása után **Hozzáadás** paneljén válassza **OK** toofinish létrehozása a szerződés és a visszatérési tooyour integrációs fiók panelen.

    Az újonnan hozzáadott foglalt most megjelenik a **megállapodások** listája.

2.  Az integrációs fiókok áttekintése is megtekintheti a szerződéseket. Az integráció-fiók panelen válassza **áttekintése**, majd jelölje be hello **megállapodások** csempére. 

    ![Válassza ki a "Szerződés" csempe tooview minden szerződés](./media/logic-apps-enterprise-integration-agreements/agreement-6.png)

## <a name="view-hello-swagger"></a>Nézet hello swagger
Lásd: hello [részletek swagger](/connectors/as2/). 

## <a name="next-steps"></a>Következő lépések
* [További tudnivalók a vállalati integrációs csomag hello](logic-apps-enterprise-integration-overview.md "további információ a vállalati integrációs csomag")  
