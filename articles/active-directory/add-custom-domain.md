---
title: "egy egyéni tartomány tooAzure AD aaaAdd |} Microsoft Docs"
description: "Azt ismerteti, hogyan tooadd egyéni tartományt az Azure Active Directoryban."
services: active-directory
author: jeffgilb
manager: femila
ms.assetid: 0a90c3c5-4e0e-43bd-a606-6ee00f163038
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jeffgilb
ms.reviewer: jsnow
ms.custom: it-pro
ms.openlocfilehash: 878cecad364ec47f1c6755d742aaccbce627dc5e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quickstart-add-a-custom-domain-name-tooazure-active-directory"></a>Gyors üzembe helyezés: Egy egyéni tartomány nevét az Active Directory tooAzure hozzáadása

Minden Azure AD-címtárral rendelkezik egy kezdeti tartománynevet hello formájában *tartománynév*. onmicrosoft.com. hello kezdeti tartomány neve nem módosítható vagy törölhető, de a vállalati tartomány neve tooAzure AD is hozzáadhat. Például a szervezet más tartomány használt nevek toodo üzleti és a felhasználók, akik jelentkezzen be vállalati tartománynév valószínűleg rendelkezik. Az egyéni tartomány nevét tooAzure AD hozzáadni lehetővé teszi a tooassign felhasználónevek hello könyvtárban, amelyek a megszokott tooyour felhasználók, például a "alice@contoso.com." Ahelyett, hogy "alice @*<domain name>*. onmicrosoft.com". hello folyamat felettébb egyszerű:

1. Hello egyéni tartomány nevét tooyour könyvtár hozzáadása
2. Hello regisztrációs hello tartománynévhez tartozó DNS-bejegyzés hozzáadása
3. Ellenőrizze a hello egyéni tartomány nevét az Azure ad-ben

## <a name="add-your-custom-domain"></a>Az egyéni tartomány hozzáadása
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely hello címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **Azure Active Directory** hello szövegmezőbe, és válassza ki **Enter**.
   
   ![Nyitó felhasználók kezelése](./media/active-directory-domains-add-azure-portal/user-management.png)
3. A hello ***könyvtárnév*** panelen válassza **tartománynevek**.
4. A hello  ***könyvtárnév* -tartománynevek** panelen, jelölje be hello **Hozzáadás** parancsot.
   
   ![Hello hozzáadása paranccsal](./media/active-directory-domains-add-azure-portal/add-command.png)
5. A hello **tartománynév** panelen írja be az egyéni tartomány nevét hello hello mezőbe, például "contoso.com", és válassza ki **tartomány hozzáadása**. Lehet, hogy tooinclude hello .com, .net vagy egyéb legfelső szintű kiterjesztés.
6. A hello ***tartománynév*** panel (az egyéni tartománynév hello cím), első hello DNS bejegyzés információk toouse tooverify, hogy a szervezete rendelkezik hello egyéni tartománynevet.
   
   ![DNS-bejegyzés adatainak lekérése](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

> [!TIP]
> Ha a helyszíni Windows Server AD és az Azure AD toofederate tervezi, akkor meg kell tooselect hello **I terv tooconfigure ebben a tartományban az egyszeri bejelentkezés a helyi Active Directory** hello Azure AD Connect eszköz jelölőnégyzet toosynchronize a címtárakat. Tooregister is szükség van a helyszíni címtárral a hello összevonását választva azonos tartománynév hello **Azure AD tartományi** hello varázsló lépést. Láthatja, hogy milyen hello varázsló tűnik, hogy lépése [ezeket az utasításokat a](./connect/active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Ha még nem rendelkezik hello Azure AD Connect eszközt, akkor [töltse le innen](http://go.microsoft.com/fwlink/?LinkId=615771).

Most, hogy a felvett hello tartománynév, az Azure AD ellenőriznie kell, hogy a szervezete rendelkezik hello tartomány nevét. Mielőtt az Azure AD végre tudná hajtani ezt az ellenőrzést, fel kell vennie egy DNS-bejegyzés hello hello tartománynév DNS zónafájljában. Ez a feladat hello tartománynév-regisztrációs hello webhelyen történik.

## <a name="add-hello-dns-entry-at-hello-domain-name-registrar-for-hello-domain"></a>Hello regisztrációs hello tartomány hello DNS-bejegyzés hozzáadása
hello az egyéni tartomány neve és az Azure AD tovább lépés toouse tooupdate hello DNS-zónafájlját hello tartományban. Az Azure AD majd ellenőrizheti, hogy a szervezete rendelkezik hello egyéni tartománynevet.

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

> [!TIP]
> Too900 egyéni tartomány neveket adhat hozzá, de csak egy lehet [állítja be elsődleges tartománynév hello az Azure AD-címtár](active-directory-domains-manage-azure-portal.md#set-the-primary-domain-name-for-your-azure-ad-directory) használatos alapértelmezett új fiókokat hozhat létre.

Most felhőalapú felhasználói fiókokat hozhat létre, vagy a frissítés korábban szinkronizált a helyi felhasználói fiók adatait, az egyéni tartománynév használatával. Módosíthatja is korábban szinkronizált felhasználói fióknak tartományi utótag adatokat a [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) vagy hello [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="troubleshooting"></a>Hibaelhárítás
Ha egy egyéni tartománynevet nem tudja ellenőrizni, próbálja meg következő hibaelhárítási lépéseket hello:

1. **Várjon egy órát**. DNS-rekordokat kell toopropagate, ahhoz az Azure AD tartományi hello ellenőrizheti. Ez egy vagy több órát is igénybe vehet.
2. **Győződjön meg arról, hello DNS-rekord lett megadva, és a helyes**. Ennek végrehajtásáról hello tartomány hello regisztrációs hello webhelyen. Az Azure AD hello tartománynév nem ellenőrizhető, ha hello DNS-bejegyzés nem DNS-zónafájlját hello szerepelnek, vagy ha még nincs terméknévnek pontosan egyeznie kell hello DNS-bejegyzést, hogy az Azure AD meg a megadott. Ha nem rendelkezik hozzáférési hello tartomány: hello regisztrációs tooupdate DNS-rekordjait, hello DNS-bejegyzés megosztása hello személyt vagy csapatot a szervezetnél, aki hozzáfér-e, és kérje meg őket tooadd hello DNS-bejegyzés.
3. **Hello tartománynév törlése egy másik címtárból az Azure AD**. A tartománynév csak egyetlen címtárban ellenőrizhető. Ha a tartománynév ellenőrzése egy másik címtárban már megtörtént, onnan előbb törölni kell, mielőtt ellenőrizni lehetne az új címtárban. tartománynevekkel törléséről toolearn olvasási [egyéni tartománynevek kezelése](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>További egyéni tartománynevek hozzáadása
Ha a szervezet használja egynél több egyéni tartománynevet, például "contoso.com" és "contosobank.com" is hozzáadhat too900 be több hello cikkben leírt lépéseket az egyes többszöri használatával.

### <a name="learn-more"></a>Részletek
[Az Azure ad-ben egyéni tartománynevek elméleti áttekintése](active-directory-add-domain-concepts.md)

[Egyéni tartománynevek kezelése](active-directory-domains-manage-azure-portal.md)


## <a name="next-steps"></a>Következő lépések
A gyors üzembe helyezés, hogy megtanulta, hogyan tooadd egy egyéni tartomány tooAzure AD. 

Hivatkozás tooadd új egyéni tartományt az Azure ad-ben a következő hello Azure-portálon a hello is használhatja.

> [!div class="nextstepaction"]
> [Egyéni tartomány hozzáadása](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/QuickStart) 