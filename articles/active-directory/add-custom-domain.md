---
title: "Az Azure AD hozzá egyéni tartományt |} Microsoft Docs"
description: "Ismerteti, hogyan adhatnak hozzá egyéni tartományt az Azure Active Directoryban."
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
ms.openlocfilehash: 4848130601ffa18ed1565e79cb0f0db3274e950f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="quickstart-add-a-custom-domain-name-to-azure-active-directory"></a>Gyors üzembe helyezés: Egyéni tartománynév hozzáadása az Azure Active Directoryhoz

Minden Azure AD-címtárral rendelkezik egy kezdeti tartománynevet formájában *tartománynév*. onmicrosoft.com. A kezdeti tartománynév nem módosítható vagy törölhető, de az Azure AD is adhat hozzá a vállalata tartománynevét. A szervezet például valószínűleg más üzleti és a felhasználók, akik jelentkezzen be vállalati tartománynév használt tartománynevek rendelkezik. Egyéni tartománynevek hozzáadása az Azure AD lehetővé teszi a felhasználóneveket rendeljen a könyvtárat, amelyek a felhasználók számára, mint "alice@contoso.com." Ahelyett, hogy "alice @*<domain name>*. onmicrosoft.com". A folyamat egyszerűen végrehajtható:

1. Az egyéni tartománynév hozzáadása a címtárhoz
2. Vegye fel a DNS-bejegyzést a tartománynévhez a tartománynév-regisztrálónál
3. Az egyéni tartománynév ellenőrzése az Azure AD-ben

## <a name="add-your-custom-domain"></a>Az egyéni tartomány hozzáadása
1. Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **Azure Active Directory** a szövegmezőbe, majd válassza ki azt a **Enter**.
   
   ![Nyitó felhasználók kezelése](./media/active-directory-domains-add-azure-portal/user-management.png)
3. Az a ***könyvtárnév*** panelen válassza **tartománynevek**.
4. Az a  ***könyvtárnév* -tartománynevek** panelen válassza a **Hozzáadás** parancsot.
   
   ![Az Add parancs kiválasztása](./media/active-directory-domains-add-azure-portal/add-command.png)
5. Az a **tartománynév** panelen írja be a mezőbe, például "contoso.com", az egyéni tartomány nevét, és válassza ki **tartomány hozzáadása**. A névben mindenképpen szerepeljen a .com, a .net vagy egyéb legfelső szintű kiterjesztés.
6. Az a ***tartománynév*** panel (az egyéni tartománynév a címben), a DNS-bejegyzés adatait, hogy a szervezete rendelkezik-e az egyéni tartománynév ellenőrzésére használt beolvasása.
   
   ![DNS-bejegyzés adatainak lekérése](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

> [!TIP]
> Ha tervezi összevonni a helyszíni Windows Server AD az Azure ad-vel, akkor ki kell választania a **I tervezi, hogy ebben a tartományban az egyszeri bejelentkezés konfigurálása a helyi Active Directoryval** jelölőnégyzetet az Azure AD Connect eszköz futtatásakor a címtárak szinkronizálása. Ugyanazon a néven a helyszíni címtárral a összevonását választva regisztrálnia kell a **Azure AD tartományi** lépés a varázslóban. Megtekintheti, hogyan néz ki ez a lépés a varázslóban [ezekben az útmutatásokban](./connect/active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Ha még nem rendelkezik Azure AD Connect eszközzel, [innen letöltheti](http://go.microsoft.com/fwlink/?LinkId=615771).

A tartománynév hozzáadását követően az Azure AD-nek ellenőriznie kell, hogy az adott tartománynév valóban a szervezete tulajdonában van-e. Mielőtt az Azure AD végre tudná hajtani ezt az ellenőrzést, DNS-bejegyzést kell hozzáadnia a tartománynév DNS zónafájljában. Ezt a feladatot a tartománynév-regisztrációs webhelyén kell végrehajtani.

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Vegye fel a DNS-bejegyzést a tartomány tartománynév-regisztrálójánál.
A következő lépés az egyéni tartománynév Azure AD-vel történő használatához a tartomány DNS-zónafájljának frissítése. Az Azure AD majd ellenőrizheti, hogy a szervezete rendelkezik-e az egyéni tartománynév.

1. Jelentkezzen be a tartomány tartománynév-regisztrálójába. Ha nem rendelkezik hozzáféréssel a DNS-bejegyzés frissítéséhez, kérjen meg egy olyan személyt vagy csapatot, amely rendelkezik a 2. lépés végrehajtásához szükséges hozzáféréssel, és értesíti, amikor a frissítés befejeződött.
2. Frissítse a tartomány DNS-zónafájlját az Azure AD által rendelkezésre bocsátott DNS-bejegyzés hozzáadásával. Ez a DNS-bejegyzés lehetővé teszi az Azure AD számára az adott tartomány tulajdonosának ellenőrzését. A DNS-bejegyzés semmilyen értelemben nem módosítja a rendszer viselkedését, például a levélkezelési útválasztást vagy a webes üzemeltetést sem.

DNS-bejegyzések hozzáadásával kapcsolatos segítségért olvassa el az [Útmutató DNS-bejegyzések hozzáadásához népszerű DNS-regisztrálókon](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/) című részt.

## <a name="verify-the-domain-name-with-azure-ad"></a>A tartománynév ellenőrzése az Azure AD-vel
A DNS-bejegyzés hozzáadását követően készen áll a tartománynév Azure AD-vel történő ellenőrzésére.

A tartománynév csak azt követően a DNS-rekordok propagálása ellenőrizhetők. Ez a propagálás általában csak másodperceket vesz igénybe, de néha egy vagy több óráig is eltarthat. Ha az ellenőrzés nem sikerül első alkalommal, próbálkozzon újra később.

1. Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.
2. Válassza ki **Tallózás**, a mezőben adja meg a felhasználói kezelését, és válassza **Enter**.
   
   ![Nyitó felhasználók kezelése](./media/active-directory-domains-add-azure-portal/user-management.png)
3. Az a **felhasználókezelés - tartománynevek** panelen válassza ki az ellenőrizni kívánt nem ellenőrzött tartomány nevét.
4. A a ***tartománynév*** panel (Ez azt jelenti, hogy a panel, amelyen megnyitása, amelyen az új tartomány nevét a cím), válasszon **ellenőrizze** az ellenőrzés végrehajtásához.

> [!TIP]
> Legfeljebb 900 egyéni tartományneveket adhat hozzá, de csak egy lehet [állítja be az elsődleges tartomány nevét az Azure AD-címtár](active-directory-domains-manage-azure-portal.md#set-the-primary-domain-name-for-your-azure-ad-directory) használatos alapértelmezett új fiókokat hozhat létre.

Most felhőalapú felhasználói fiókokat hozhat létre, vagy a frissítés korábban szinkronizált a helyi felhasználói fiók adatait, az egyéni tartománynév használatával. Módosíthatja is korábban szinkronizált felhasználói fióknak tartományi utótag adatokat a [Microsoft PowerShell](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains) vagy a [Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations).

## <a name="troubleshooting"></a>Hibaelhárítás
Ha egy egyéni tartománynevet nem tudja ellenőrizni, próbálja meg a következő hibaelhárítási lépéseket:

1. **Várjon egy órát**. A DNS-rekordokat propagálni kell, mielőtt az Azure AD ellenőrizni tudja a tartományt. Ez egy vagy több órát is igénybe vehet.
2. **Győződjön meg róla, hogy a DNS-rekord helyesen lett megadva**. Hajtsa végre ezt a lépést a tartomány tartománynév-regisztrációs webhelyén. Az Azure AD nem tudja ellenőrizni a tartománynevet, ha a DNS-bejegyzés nincs a DNS-zónafájlban, vagy ha az nem pontosan egyezik azzal a DNS-bejegyzéssel, amelyet az Azure AD adott meg. Ha nincs hozzáférése a tartomány DNS-rekordjainak frissítéséhez a tartománynév-regisztrálónál, ossza meg a DNS-bejegyzést egy ilyen engedélyekkel rendelkező személlyel vagy csapattal, majd kérje a DNS-bejegyzés hozzáadását.
3. **Tartománynév törlése egy Azure AD-ben található másik címtárból**. A tartománynév csak egyetlen címtárban ellenőrizhető. Ha a tartománynév ellenőrzése egy másik címtárban már megtörtént, onnan előbb törölni kell, mielőtt ellenőrizni lehetne az új címtárban. Információ a tartománynevek törléséről: [Egyéni tartománynevek kezelése](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>További egyéni tartománynevek hozzáadása
Ha a szervezet használja egynél több egyéni tartománynevet, például "contoso.com" és "contosobank.com", a cikkben leírt lépéseket az egyes megismétlésével további legfeljebb 900 is hozzáadhat.

### <a name="learn-more"></a>Részletek
[Az Azure ad-ben egyéni tartománynevek elméleti áttekintése](active-directory-add-domain-concepts.md)

[Egyéni tartománynevek kezelése](active-directory-domains-manage-azure-portal.md)


## <a name="next-steps"></a>Következő lépések
A gyors üzembe helyezés az egyéni tartománynév felvétele az Azure AD hogy megismerte. 

A következő hivatkozás segítségével új egyéni tartomány hozzáadása az Azure ad-ben az Azure portálról.

> [!div class="nextstepaction"]
> [Egyéni tartomány hozzáadása](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/QuickStart) 