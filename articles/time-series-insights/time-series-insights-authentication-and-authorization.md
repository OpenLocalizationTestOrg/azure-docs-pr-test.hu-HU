---
title: "aaaConfigure hitelesítési és engedélyezési egy egyéni alkalmazás-t meghívó hello Azure idő adatsorozat Hirdetéselemző API-t |} Microsoft Docs"
description: "Ez az oktatóanyag azt ismerteti, hogyan tooconfigure hitelesítési és engedélyezési egy egyéni alkalmazás-t meghívó hello Azure idő adatsorozat Hirdetéselemző API-t"
keywords: 
services: time-series-insights
documentationcenter: 
author: dmdenmsft
manager: almineev
editor: cgronlun
ms.assetid: 
ms.service: time-series-insights
ms.devlang: na
ms.topic: how-to-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 05/24/2017
ms.author: dmden
ms.openlocfilehash: 5043468bfc2af3c0d27e8602508d92ba2848409e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="authentication-and-authorization-for-azure-time-series-insights-api"></a>Hitelesítési és engedélyezési Azure idő adatsorozat Insights API-hoz.

Ez a cikk azt ismerteti, hogyan tooconfigure egy egyéni alkalmazást, amely behívja hello Azure idő adatsorozat Hirdetéselemző API-t.

## <a name="service-principal"></a>Egyszerű szolgáltatásnév

Ez a szakasz ismerteti, hogyan tooconfigure egy alkalmazás tooaccess hello hello alkalmazás nevében idő adatsorozat Hirdetéselemző API-t. hello alkalmazás majd kérdezhet le adatokat, vagy hello idő adatsorozat Insights környezetben alkalmazás hitelesítő adatait, és nem a hello felhasználói hitelesítő adatokat a referenciaadatok közzététele.

Ha egy alkalmazás, amelyet a tooaccess idő adatsorozat Insights, állítson be egy Azure Active Directory-alkalmazás, és rendelje hozzá a hello adat-hozzáférési házirendjeit hello idő adatsorozat Insights környezetben. Ez a módszer előnyösebb toorunning hello alkalmazás a saját credentials azért, mert:

* Engedélyeket rendelhet toohello identitását, amelyek eltérnek a saját engedélyeit. Ezek az engedélyek általában korlátozott tooexactly milyen hello alkalmazásnak kell toodo. Engedélyezheti például hello app tooonly egy adott idő adatsorozat Insights környezetben adatokat olvasni.
* Ha módosítja az Ön feladatkörei nincs toochange hello alkalmazás hitelesítő adatait.
* Amikor egy felügyelet nélküli parancsfájllal futtatja használhatja a tanúsítványt, vagy az alkalmazás fő tooautomate hitelesítéshez.

Ez a cikk bemutatja, hogyan azokat lépésről-lépésre tooperform hello Azure-portálon. A single-bérlői alkalmazások hello alkalmazás esetén csak egy szervezet tervezett toorun összpontosít. Single-bérlő alkalmazásokat a szervezet futó üzleti alkalmazásokhoz általában használ.

hello telepítő folyamat három magas szintű lépésekből áll:

1. Alkalmazás létrehozása az Azure Active Directoryban.
2. Az alkalmazás tooaccess hello idő adatsorozat Insights környezet engedélyezéséhez.
3. Hello Alkalmazásazonosítót és a kulcs tooacquire token toohello `"https://api.timeseries.azure.com/"` célközönségtől vagy erőforrástól. hello token majd lehet használt toocall hello idő adatsorozat Hirdetéselemző API-t.

Az alábbiakban a részletes lépéseket hello:

1. Hello Azure-portálon, válassza ki **Azure Active Directory** > **App regisztrációk** > **új alkalmazás regisztrációja**.

   ![Új alkalmazás regisztrálása az Azure Active Directoryban](media/authentication-and-authorization/active-directory-new-application-registration.png)  

2. Hello alkalmazás biztosítják a nevét, jelölje be hello típus toobe **webalkalmazás / API**, válassza ki a bármilyen érvényes URI-JÁNAK **bejelentkezési URL-cím**, és kattintson **létrehozása**.

   ![Hello alkalmazás létrehozása az Azure Active Directoryban](media/authentication-and-authorization/active-directory-create-web-api-application.png)

3. Válassza ki az újonnan létrehozott alkalmazást, és másolja az alkalmazás azonosítója tooyour kedvenc szövegszerkesztőjével.

   ![Másolja a hello Alkalmazásazonosító](media/authentication-and-authorization/active-directory-copy-application-id.png)

4. Válassza ki **kulcsok**, hello kulcs nevét, jelölje be hello lejárati, adja meg, és kattintson a **mentése**.

   ![Kulcsok – alkalmazás választása](media/authentication-and-authorization/active-directory-application-keys.png)

   ![Adja meg a hello kulcs nevét és a lejárati, és kattintson a Mentés gombra](media/authentication-and-authorization/active-directory-application-keys-save.png)

5. Másolja a hello kulcs tooyour kedvenc szövegszerkesztőjével.

   ![Hello alkalmazás kulcs másolása](media/authentication-and-authorization/active-directory-copy-application-key.png)

6. Hello idő adatsorozat Insights környezet, válassza a **adat-hozzáférési házirendjeit** kattintson **Hozzáadás**.

   ![Adja hozzá az új adatok hozzáférési házirend toohello idő adatsorozat Insights környezet](media/authentication-and-authorization/time-series-insights-data-access-policies-add.png)

7. A hello **felhasználó kiválasztása** párbeszédpanelen Beillesztés hello alkalmazás neve (a 2. lépés) vagy alkalmazás-azonosító (a 3. lépés).

   ![Alkalmazások keresésére hello felhasználó kiválasztása párbeszédpanel](media/authentication-and-authorization/time-series-insights-data-access-policies-select-user.png)

8. Jelölje be hello szerepkör (**olvasó** a lekérdezésre adatok, **közreműködő** az adatok lekérdezése és referenciaadatok módosítása), és kattintson a **Ok**.

   ![Válassza ki az olvasó és közreműködő hello szerepkör kiválasztása párbeszédpanel](media/authentication-and-authorization/time-series-insights-data-access-policies-select-role.png)

9. Hello szabályzat mentése kattintva **Ok**.

10. Hello Alkalmazásazonosítót használja (a 3. lépés) és az alkalmazás (az 5. lépés) kulcs tooacquire hello token hello alkalmazás nevében. hello token majd adhatók át a hello `Authorization` Ha hello alkalmazás hívások hello idő adatsorozat Hirdetéselemző API-t.

    Használata C#, a következő kód tooacquire hello token hello alkalmazás nevében hello is használhatja. Egy teljes mintát, lásd: [adatait használó C#](time-series-insights-query-data-csharp.md).

    ```csharp
    var authenticationContext = new AuthenticationContext(
        "https://login.microsoftonline.com/common",
        TokenCache.DefaultShared);

    AuthenticationResult token = await authenticationContext.AcquireTokenAsync(
        // Set hello resource URI toohello Azure Time Series Insights API
        resource: "https://api.timeseries.azure.com/", 
        clientCredential: new ClientCredential(
            // Application ID of application registered in Azure Active Directory
            clientId: "1bc3af48-7e2f-4845-880a-c7649a6470b8", 
            // Application key of hello application that's registered in Azure Active Directory
            clientSecret: "aBcdEffs4XYxoAXzLB1n3R2meNCYdGpIGBc2YC5D6L2="));

    string accessToken = token.AccessToken;
    ```

## <a name="next-steps"></a>Következő lépések

Használja a hello alkalmazás Azonosítóját és kulcsát az alkalmazásban. Hello idő adatsorozat Hirdetéselemző API-t behívó kód a minta, lásd: [adatait használó C#](time-series-insights-query-data-csharp.md).

## <a name="see-also"></a>Lásd még:

* [Lekérdezési API](/rest/api/time-series-insights/time-series-insights-reference-queryapi) a hello teljes lekérdezés API-referencia
* [Hello Azure-portálon található egyszerű szolgáltatás létrehozása](../azure-resource-manager/resource-group-create-service-principal-portal.md)
