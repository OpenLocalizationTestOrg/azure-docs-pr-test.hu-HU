---
title: "aaaHow toouse Alkalmazásjelszók az Azure MFA? | Microsoft Docs"
description: "Ezen a lapon segít megérteni az alkalmazásjelszók és a definíció a felhasználóknak az MFA legutóbb tooAzure használatos."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 345b757b-5a2b-48eb-953f-d363313be9e5
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/15/2017
ms.author: kgremban
ms.custom: end-user
ms.openlocfilehash: bf2c11edc0ca81f2950eff0f7a3a24c8a5b34623
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="what-are-app-passwords-in-azure-multi-factor-authentication"></a>Mik a Azure multi-factor Authentication Alkalmazásjelszókat?
Bizonyos böngészőn kívüli alkalmazások, többek között a hello Apple natív e-mail-ügyfélprogram, amely használja az Exchange Active Sync jelenleg nem támogatják a multi-factor authentication. Többtényezős hitelesítést felhasználónként kell engedélyezni. Ez azt jelenti, hogy ha egy felhasználó a többtényezős hitelesítés engedélyezve van, és megpróbálnak toouse böngészőn kívüli alkalmazásokat, azok lesznek nem toodo úgy. Az alkalmazásjelszó lehetővé teszi, hogy a toooccur.

Miután egy alkalmazásjelszót, akkor az eredeti jelszóval ezen böngészőn kívüli alkalmazások helyett ezzel. Ennek oka az, amikor regisztrál a kétlépéses ellenőrzést, akkor még szólítja fel nem toolet bárki aláírására Microsoft be a jelszót még nem végzett hello második ellenőrzési. hello Apple natív e-mail-ügyfélprogram telefonján nem tud bejelentkezni, ha Ön, mert nem a kétlépéses ellenőrzéshez kérje meg. hello ezen megoldás, toocreate egy biztonságosabb alkalmazásjelszót, amelyek nem használják az alkalmazások, amelyek nem támogatják a kétlépéses ellenőrzést napi, de csak. Hello alkalmazásjelszót használnak, így alkalmazások multi-factor authentication kihagyásához és toowork folytatni.

> [!NOTE]
> Office 2013-ügyfelek (például az Outlook) támogatja az új hitelesítési protokollok és a kétlépéses ellenőrzéshez használttal használható.  Ez azt jelenti, hogy engedélyezve van, az alkalmazásjelszók nem Office 2013-ügyfelekkel való használathoz.  További információkért lásd: [Office 2013 modern hitelesítés nyilvános előzetes bejelentette](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/).


## <a name="how-toouse-app-passwords"></a>Hogyan toouse alkalmazásjelszók
hello az alábbiakban néhány dolgot tooremember hogyan toouse alkalmazásjelszókat.

* Ne hozzon létre egy saját alkalmazásjelszókat. Ehelyett azok automatikusan jönnek létre. Mivel tooenter hello alkalmazásjelszót alkalmazásonként egyszer rendelkezik, célszerű biztonságosabb toouse egy összetettebb, automatikusan generált jelszót döntések helyett egy könnyen megjegyezhető.
* Jelenleg csak egy legfeljebb 40 jelszó felhasználónként. Ha hello korlát elérése után megpróbál egy toocreate, fogjuk felszólító toodelete egy, a meglévő alkalmazásjelszavak, előtt hozzon létre egy újat.
* Minden eszközhöz, ne alkalmazásonként egy alkalmazásjelszót kell használnia. Például hozzon létre egy alkalmazásjelszót a hordozható számítógép, és azt használja az összes, az alkalmazások adott hordozható számítógépen. Ezután hozzon létre egy második app jelszó toouse az alkalmazások az asztalon. 
* Lehetősége van egy alkalmazás jelszó hello első alkalommal regisztrál a kétlépéses ellenőrzéshez.  Ha további megfelelően van szüksége, létrehozhat őket.



## <a name="creating-and-deleting-app-passwords"></a>Létrehozása és törlése alkalmazásjelszók
Az első bejelentkezés során lehetősége van az alkalmazásjelszó használható.  Ezen kívül is hozzon létre és alkalmazásjelszók később törölni.  Ennek módja attól függ, hogy a multi-factor authentication használatát. A következő válaszfájl hello toomanage alkalmazásjelszók hová kell toodetermine kérdésekre: 

1. Használhatók a kétlépéses ellenőrzést személyes Microsoft-fiókja? Ha igen, akkor olvassa el toohello [alkalmazásjelszók és a kétlépéses ellenőrzést](https://support.microsoft.com/help/12409/microsoft-account-app-passwords-two-step-verification) cikk segítségét. Ha nem, továbbra is két tooquestion.

2. OK, hogy a kétlépéses ellenőrzést használni a munkahelyi vagy iskolai fiókját. Használja az toosign tooOffice 365 alkalmazásokban? Ha igen, tekintse át túl[hozza létre az alkalmazásjelszót az Office 365](https://support.office.com/article/Create-an-app-password-for-Office-365-3e7c860f-bda4-4441-a618-b53953ee1183) segítségét. Ha nem, továbbra is három tooquestion. 

3. Használhatók a kétlépéses ellenőrzésről a Microsoft Azure-ban? Ha igen, akkor folytassa a toohello [hello Azure portálra az alkalmazás-jelszavak kezelése](#manage-app-passwords-in-the-Azure-portal) című szakaszát. Ha nem, továbbra is tooquestion négy.

4. Nem biztos benne, ahol használhatja a kétlépéses ellenőrzést? Továbbra is toohello [hello MyApps portal szolgáltatással használt alkalmazásjelszók kezelése](#manage-app-passwords-with-the-myapps-portal) című szakaszát. 


## <a name="manage-app-passwords-in-hello-azure-portal"></a>Hello Azure portálra az alkalmazás-jelszavak kezelése
Az Azure-ral használhatja a kétlépéses ellenőrzést, ha azt szeretné, toocreate alkalmazásjelszók hello Azure-portálon keresztül.

### <a name="toocreate-app-passwords-in-hello-azure-portal"></a>alkalmazásjelszók toocreate a hello Azure-portálon
1. Jelentkezzen be toohello a klasszikus Azure portálon.
2. Hello tetején kattintson a jobb gombbal a felhasználónevét, és válassza ki a további biztonsági ellenőrzés.
3. Hello proofup lapon hello bal felső válassza ki az alkalmazásjelszók
4. Kattintson a **Create** (Létrehozás) gombra.
5. Adjon meg egy nevet hello alkalmazásjelszót, majd kattintson **tovább**
6. Hello app jelszó toohello vágólapra másolja és illessze be az alkalmazást.
   
   ![Felhő](./media/multi-factor-authentication-end-user-app-passwords/app2.png)


### <a name="toodelete-app-passwords-in-hello-azure-portal"></a>alkalmazásjelszók toodelete a hello Azure-portálon
1. Jelentkezzen be toohello a klasszikus Azure portálon.
2. Hello tetején kattintson a jobb gombbal a felhasználónevét, és válassza ki a további biztonsági ellenőrzés.
3. Hello bal felső, tovább tooadditional biztonsági ellenőrzési, válasszon **alkalmazásjelszókat.**
4. Azt szeretné, hogy toodelete, jelölje be tovább toohello alkalmazásjelszót **törlése**.
5. Hello törlésének megerősítése kattintva **Igen**.
6. A törölt hello alkalmazásjelszót kattinthat **bezárása**.


## <a name="manage-app-passwords-with-hello-myapps-portal"></a>Hello MyApps portal szolgáltatással használt alkalmazásjelszók kezelése.
Ha nem biztos abban, hogy a multi-factor authentication használatát, majd bármikor létrehozása és törlése alkalmazásjelszók hello myapps portálon keresztül.

### <a name="toocreate-an-app-password-using-hello-myapps-portal"></a>egy alkalmazás jelszó használatával toocreate hello Myapps portál
1. Jelentkezzen be a túl[https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Kattintson a jobb oldali hello tetején nevére, és válassza a **profil**.
3. Válassza ki **további biztonsági ellenőrzés**.
   ![Válassza ki a további biztonsági ellenőrzés – képernyőkép](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Válassza ki **alkalmazásjelszók**.
   ![Válassza ki az alkalmazásjelszók – képernyőkép](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Kattintson a **Create** (Létrehozás) gombra.
6. Adjon meg egy nevet hello alkalmazásjelszót, majd kattintson **következő**.
7. Hello app jelszó toohello vágólapra másolja és illessze be az alkalmazást.
   ![Hozza létre az alkalmazásjelszót](./media/multi-factor-authentication-end-user-app-passwords/create2.png)

### <a name="toodelete-an-app-password-using-hello-myapps-portal"></a>egy alkalmazás jelszó használatával toodelete hello Myapps portál
1. Jelentkezzen be a túl[https://myapps.microsoft.com](https://myapps.microsoft.com)
2. Hello bal felső válassza ki a profilt.
3. Válassza ki **további biztonsági ellenőrzés**.

   ![Válassza ki a további biztonsági ellenőrzés – képernyőkép](./media/multi-factor-authentication-end-user-manage/myapps1.png)

4. Válassza ki **alkalmazásjelszók**.

   ![Válassza ki az alkalmazásjelszók – képernyőkép](./media/multi-factor-authentication-end-user-app-passwords/apppass2.png)

5. Kattintson a kívánt toodelete, tovább toohello alkalmazásjelszót **törlése**.

   ![Az alkalmazásjelszó törlése](./media/multi-factor-authentication-end-user-app-passwords/delete1.png)

6. Erősítse meg toodelete, hogy a jelszó kattintva **Igen**.
7. A törölt hello alkalmazásjelszót kattinthat **bezárása**.

## <a name="next-steps"></a>Következő lépések

- [A kétlépéses ellenőrzés beállításait kezelheti](multi-factor-authentication-end-user-manage-settings.md)

- Próbálja ki hello [Microsoft Authenticator alkalmazás](microsoft-authenticator-app-how-to.md) tooverify a bejelentkezéseket app értesítések szövegek vagy hívások fogadása helyett. 
