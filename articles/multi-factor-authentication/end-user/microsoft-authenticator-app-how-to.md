---
title: "a mobiltelefonok aaaMicrosoft hitelesítőalkalmazása |} Microsoft Docs"
description: "Megtudhatja, hogyan tooupgrade toohello legújabb verziójára Azure Authenticator."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 3065a1ee-f253-41f0-a68d-2bd84af5ffba
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: kgremban
ms.reviewer: librown
ms.custom: H1Hack27Feb2017, end-user
ms.openlocfilehash: d895d92d89613cbafd9fc09d4ff4810cbf25652e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-hello-microsoft-authenticator-app"></a>Ismerkedés a hello Microsoft Authenticator alkalmazásról
hello Microsoft Authenticator alkalmazást egy további szintű biztonságot nyújt a munkahelyi vagy iskolai fiókját (például bsimon@contoso.com) vagy a Microsoft-fiókját (például bsimon@outlook.com).

hello az alkalmazás akkor működik az alábbi két módszer egyikével:

* **Értesítési**. hello alkalmazás megakadályozza a jogosulatlan hozzáférés tooaccounts, és állítsa le a csalárd tranzakciókat egy értesítési tooyour okostelefonján vagy táblagépén küldésével segítségével. Egyszerűen hello értesítést, és ha az megbízható, választhatja ki **ellenőrizze**. Ellenkező esetben kiválaszthatja **Megtagadás**. 
* **Az ellenőrző kódot**. hello alkalmazás használhat egy szoftverfrissítési token toogenerate az OAuth-ellenőrző kódot. Miután megadta a felhasználónevét és jelszavát, meg kell adnia a hello bejelentkezési képernyő hello alkalmazás által biztosított hello kódot. hello ellenőrzőkódot hitelesítés második formáját nyújtja.

hello Microsoft Authenticator alkalmazást a felváltja hello Azure Authenticator alkalmazást. hello Azure Authenticator alkalmazás továbbra is működik, de ha úgy dönt, hogy toomove toohello új Microsoft Authenticator alkalmazást, ez a cikk segítséget nyújthatnak.  

## <a name="opt-in-for-two-step-verification"></a>A kétlépéses ellenőrzéshez részt

hello Microsoft Authenticator alkalmazás önmagában nem működik. Konfigurálható, a fiókok tooprompt egy második hitelesítési módszert, miután a jelentkezik be a felhasználónevét és jelszavát. 

A munkahelyi vagy iskolai fiókkal nem általában kap toochoose Ez a funkció a szolgáltatást. Ehelyett a biztonsági rendszergazda jóváhagyja a nevünkben, és majd értesíti, tooregister ellenőrzési módszereket a fiókjához. Ebben a forgatókönyvben tooyou vonatkozik, ha további információk: [mit Azure multi-factor Authentication jelent a számomra](multi-factor-authentication-end-user.md).

Egy személyes fiók kell tooset fel saját maga a kétlépéses ellenőrzést. Ha a Microsoft-fiókkal rendelkezik, ezeket a lépéseket elérhetők-e a [tudnivalók a kétlépéses ellenőrzést](https://support.microsoft.com/help/12408/microsoft-account-about-two-step-verification). 

A Microsoft Authenticator hello nem Microsoft-fiókok is használható. Előfordulhat, hogy meghívják hello szolgáltatás nem a kétlépéses ellenőrzést, de meg kell tudni toofind azt a biztonsági vagy bejelentkezési beállítások részben. 

## <a name="install-hello-app"></a>Hello alkalmazás telepítéséhez
hello Microsoft Authenticator alkalmazás érhető el [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072), és [iOS](http://go.microsoft.com/fwlink/?Linkid=825073).

## <a name="add-accounts-toohello-app"></a>Fiókok toohello alkalmazás hozzáadása
Az egyes fiókokhoz, hogy szeretné-e tooadd toohello Microsoft Authenticator alkalmazást használja a következő eljárások hello egyikét:

### <a name="add-a-personal-microsoft-account-toohello-app"></a>Személyes Microsoft-fiók toohello alkalmazás hozzáadása

Személyes Microsoft-fiók (egy toosign használt tooOutlook.com, Xbox, Skype, a stb.), mindössze meg kell toodo tooyour fiók hello Microsoft Authenticator alkalmazást a bejelentkezéshez.

### <a name="add-a-work-or-school-account-toohello-app-using-hello-qr-code-scanner"></a>Adja hozzá a munkahelyi vagy iskolai fiók toohello alkalmazást hello QR-kód képolvasó segítségével
1. Nyissa meg toohello biztonsági ellenőrzési beállítások képernyő.  Információ tooget toothis képernyő, lásd: [a biztonsági beállítások módosítása](multi-factor-authentication-end-user-manage-settings.md#where-to-find-the-settings-page).
2. Jelölőnégyzetet hello mellett túl**hitelesítőalkalmazása** válassza **konfigurálása**.

    ![hello biztonsági ellenőrzési beállítások képernyőjén hello gomb](./media/authenticator-app-how-to/azureauthe.png)

    Ekkor megjelenik egy képernyőt rajta egy QR-kódot.

    ![Hello QR-kód biztosító képernyő](./media/authenticator-app-how-to/barcode2.png)
3. Nyissa meg hello Microsoft Authenticator alkalmazást. A hello **fiókok** képernyőn válassza ki  **+** , és adja meg, hogy szeretné-e tooadd munkahelyi vagy iskolai fiókkal.
4. Hello kamera tooscan hello QR-kódot, és válassza **végzett** tooclose hello QR-kód képernyő.

    Ha a kamera nem működik megfelelően, akkor [hello QR-kódot és URL-cím manuális megadása](#add-an-account-to-the-app-manually).

5. Hello alkalmazás egy hatjegyű kódot alatta a fiók nevét jeleníti meg, amikor elkészült. 

    ![Fiókok képernyő](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-toohello-app-manually"></a>Egy fiók toohello alkalmazás manuális hozzáadása
1. Nyissa meg toohello biztonsági ellenőrzési beállítások képernyő.  Információ tooget toothis képernyő, lásd: [a biztonsági beállítások módosítása](multi-factor-authentication-end-user-manage-settings.md).
2. Válassza ki **konfigurálása**.

    ![hello biztonsági ellenőrzési beállítások képernyőjén hello gomb](./media/authenticator-app-how-to/azureauthe.png)

    Ekkor megjelenik egy képernyőt rajta egy QR-kódot.  Megjegyzés: hello kódot és URL-címet.

    ![Hello QR-kódot és URL-címet biztosító képernyő](./media/authenticator-app-how-to/barcode2.png)
3. Nyissa meg hello Microsoft Authenticator alkalmazást. A hello **fiókok** képernyőn válassza ki  **+** , és adja meg, hogy szeretné-e tooadd munkahelyi vagy iskolai fiókkal.

4. Hello képolvasó, válassza ki **írja be a kódot kézzel**.

    ![A QR-kód vizsgálatának képernyő](./media/multi-factor-authentication-end-user-first-time/scan2.png)
5. Adja meg a hello kódot és hello URL hello megfelelő mezőkbe hello alkalmazásban, majd válasszon **Befejezés**.

    ![Képernyőn írja be a kódot és URL-címe](./media/authenticator-app-how-to/manual.png)

6. Hello alkalmazás egy hatjegyű kódot alatta a fiók nevét jeleníti meg, amikor elkészült.

    ![Fiókok képernyő](./media/authenticator-app-how-to/accounts.png)

### <a name="add-an-account-toohello-app-using-touch-id"></a>Touch ID használata fiók toohello alkalmazás hozzáadása
hello Microsoft Authenticator alkalmazás IOS rendszerű eszközökön támogatja-e Touch.  Az Azure multi-factor Authentication lehetővé teszi a szervezetek toorequire eszközök PIN-kódot. Touch ID, az iOS-felhasználói tooenter PIN-kód nem szükséges. Ehelyett a válassza ki azokat és az ujjlenyomat beolvasása **jóváhagyás**.

A Microsoft Authenticator Touch ID beállítása felettébb egyszerű. Egy normál ellenőrző kérdéssel és a PIN-kód végrehajtása. Ha az eszköz támogatja a Touch ID, a Microsoft Authenticator beállítja az automatikusan fiók.

![A Touch ID telepítés ellenőrzése](./media/authenticator-app-how-to/touchid1.png)

A terjesztésipont-előre, amikor Ön tooverify szükséges a bejelentkezés hello kapott leküldéses értesítést válasszon ki, és a PIN-kód megadása helyett az ujjlenyomat beolvasása.

![Leküldéses értesítések](./media/authenticator-app-how-to/touchid2.png)

## <a name="use-hello-app-when-you-sign-in"></a>Hello alkalmazás használata, amikor bejelentkezik

A fiók toohello alkalmazás hozzáadása után esetleg felszólító toodo egy teszt ellenőrzési toomake, hogy minden helyesen lett-e beállítva. Ezt követően elkészült! Nincs szükség a toodo bármi más amíg hello legközelebb bejelentkezik.

Ha úgy dönt, toouse ellenőrző kódok kezelésére hello alkalmazásban, megkezdése toosee hello kezdőlapján őket. Így ha szükség van egy mindig kell egy új kódot 30 másodpercenként változik. De nem kell toodo semmit, amíg bejelentkezhet, és felszólító tooenter ellenőrző kódot.  
