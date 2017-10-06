---
title: "aaaAdd az egyéni tartomány neve az Active Directory tooAzure |} Microsoft Docs"
description: "Hogyan tooadd a vállalati tartománynevek tooAzure Active Directory, és hogyan tooverify hello tartomány nevét."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d97e57c6-578a-4929-8fb8-42e858a711c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.openlocfilehash: 88d5f443cd10b098a9a9ffb3137f5e1ca33b6aad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-a-custom-domain-name-tooazure-active-directory"></a>Egy egyéni tartomány nevét az Active Directory tooAzure hozzáadása
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-domains-add-azure-portal.md)
> * [klasszikus Azure portál](active-directory-add-domain.md)
> 

Az Azure Active Directoryval (Azure AD), a vállalati tartomány neve tooAzure AD is hozzáadhat. Lehetséges, hogy a tartomány nevét, hogy Ön szervezete által használt toodo üzleti és a felhasználók, akik jelentkezzen be vállalati tartománynév használatával. Hello tartomány neve tooAzure AD hozzáadni lehetővé teszi a tooassign felhasználónevek hello könyvtárban, amelyek a megszokott tooyour felhasználók, például a "alice@contoso.com." hello folyamat felettébb egyszerű:

1. Hello egyéni tartomány nevét tooyour könyvtár hozzáadása
2. Hello regisztrációs hello tartománynévhez tartozó DNS-bejegyzés hozzáadása
3. Ellenőrizze a hello egyéni tartomány nevét az Azure ad-ben

## <a name="how-do-i-add-a-domain-name"></a>Hogyan hozzá egy tartománynevet?
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **Azure Active Directory** hello szövegmezőbe, és válassza ki **Enter**.
   
   ![Nyitó felhasználók kezelése](./media/active-directory-domains-add-azure-portal/user-management.png)
3. A hello ***könyvtárnév*** panelen válassza **tartománynevek**.
4. A hello  ***könyvtárnév* -tartománynevek** panelen, jelölje be hello **Hozzáadás** parancsot.
   
   ![Hello hozzáadása paranccsal](./media/active-directory-domains-add-azure-portal/add-command.png)
5. A hello **tartománynév** panelen írja be az egyéni tartomány nevét hello hello mezőbe, például "contoso.com", és válassza ki **tartomány hozzáadása**. Lehet, hogy tooinclude hello .com, .net vagy egyéb legfelső szintű kiterjesztés.
6. A hello ***tartománynév*** panel (Ez azt jelenti, hogy hello panel, amelyen megnyitása, amelyen az új tartomány nevét a hello cím), első hello DNS-bejegyzés adatait, hogy az Azure AD, hogy a szervezete rendelkezik-e az egyéni tartománynév hello tooverify fogja használni.
   
   ![DNS-bejegyzés adatainak lekérése](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

Most, hogy a felvett hello tartománynév, az Azure AD ellenőriznie kell, hogy a szervezete rendelkezik hello tartomány nevét. Mielőtt az Azure AD végre tudná hajtani ezt az ellenőrzést, fel kell vennie egy DNS-bejegyzés hello hello tartománynév DNS zónafájljában. Ez a feladat hello tartománynév-regisztrációs hello webhelyen történik.

## <a name="add-hello-dns-entry-at-hello-domain-name-registrar-for-hello-domain"></a>Hello regisztrációs hello tartomány hello DNS-bejegyzés hozzáadása
hello az egyéni tartomány neve és az Azure AD tovább lépés toouse tooupdate hello DNS-zónafájlját hello tartományban. Ez lehetővé teszi, hogy a szervezete rendelkezik-e az egyéni tartománynév hello Azure AD tooverify.

1. Jelentkezzen be toohello regisztrációs hello tartomány. Ha még nem rendelkezik hozzáféréssel tooupdate hello DNS-bejegyzés, kérje meg a hello személy vagy a hozzáférés toocomplete 2. lépés rendelkező csoportját, és kész tudja toolet.
2. Frissítés hello DNS-zónafájlját hello tartomány hello DNS-bejegyzés hozzáadásával tooyou az Azure AD által biztosított. A DNS-bejegyzés lehetővé teszi, hogy az Azure AD tooverify a hello tartomány tulajdonjogát. hello DNS-bejegyzés semmilyen értelemben nem módosítja például a levélkezelési útválasztást vagy a webes üzemeltetést sem.

Súgó a hozzáadása hello DNS-bejegyzés használatával, olvassa el a [utasításokat a DNS-bejegyzés hozzáadásához népszerű DNS-regisztráló szervezetek:](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-hello-domain-name-with-azure-ad"></a>Ellenőrizze az Azure ad-val hello tartománynevet
Hello DNS-bejegyzés hozzáadását követően készen áll a tooverify hello tartomány nevét az Azure ad-val áll.

A tartománynév csak azt követően hello DNS-rekord propagálása ellenőrizhetők. Ez a propagálás általában csak másodperceket vesz igénybe, de néha egy vagy több óráig is eltarthat. Ha ellenőrzési első alkalommal nem működik a hello, próbálkozzon újra később.

1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **Tallózás**, a felhasználó felügyeleti hello mezőben adja meg, és válassza **Enter**.
   
   ![Nyitó felhasználók kezelése](./media/active-directory-domains-add-azure-portal/user-management.png)
3. A hello **felhasználókezelés - tartománynevek** panelen, a select hello nem ellenőrzött tartomány nevét, amelyet az tooverify.
4. A hello ***tartománynév*** (Ez azt jelenti, hogy hello panelen megjelenő, amely rendelkezik az új tartomány nevét hello címben) panelen válassza ki **ellenőrizze** toocomplete hello ellenőrzése.

Mostantól [hozzá tud rendelni egyéni tartománynevet tartalmazó felhasználóneveket](active-directory-users-create-azure-portal.md).

## <a name="troubleshooting"></a>Hibaelhárítás
Ha egy egyéni tartománynevet nem tudja ellenőrizni, próbálja hello következő. Először foglalkozunk hello közös és a munkahelyi legalább közös toohello le.

1. **Várjon egy órát**. DNS-rekordokat kell toopropagate, ahhoz az Azure AD tartományi hello ellenőrizheti. Ez egy vagy több órát is igénybe vehet.
2. **Győződjön meg arról, hello DNS-rekord lett megadva, és a helyes**. Ennek végrehajtásáról hello tartomány hello regisztrációs hello webhelyen. Az Azure AD hello tartománynév nem ellenőrizhető, ha hello DNS-bejegyzés nem DNS-zónafájlját hello szerepelnek, vagy ha még nincs terméknévnek pontosan egyeznie kell hello DNS-bejegyzést, hogy az Azure AD meg a megadott. Ha nem rendelkezik hozzáférési hello tartomány: hello regisztrációs tooupdate DNS-rekordjait, hello DNS-bejegyzés megosztása hello személyt vagy csapatot a szervezetnél, aki hozzáfér-e, és kérje meg őket tooadd hello DNS-bejegyzés.
3. **Hello tartománynév törlése egy másik címtárból az Azure AD**. A tartománynév csak egyetlen címtárban ellenőrizhető. Ha a tartománynév ellenőrzése egy másik címtárban már megtörtént, onnan előbb törölni kell, mielőtt ellenőrizni lehetne az új címtárban. tartománynevekkel törléséről toolearn olvasási [egyéni tartománynevek kezelése](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>További egyéni tartománynevek hozzáadása
Ha a szervezet használja több egyéni tartománynevek, például "contoso.com" és "contosobank.com", később is hozzáadhatja tooa legfeljebb 900 tartománynév fel. Ugyanaz a cikk tooadd minden tartományneve szükséges lépések hello használata.

## <a name="next-steps"></a>Következő lépések
[Egyéni tartománynevek kezelése](active-directory-domains-manage-azure-portal.md)

