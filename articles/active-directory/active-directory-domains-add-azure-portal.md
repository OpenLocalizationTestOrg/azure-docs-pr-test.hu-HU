---
title: "Egyéni tartománynév hozzáadása az Azure Active Directoryhoz | Microsoft Docs"
description: "A vállalati tartománynevek hozzáadása az Azure Active Directoryhoz és a nevek ellenőrzése."
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
ms.openlocfilehash: ad72f768add7edc1d34a85c27dc2aa1b4e4b3a50
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="add-a-custom-domain-name-to-azure-active-directory"></a>Egyéni tartománynév hozzáadása az Azure Active Directoryhoz
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-domains-add-azure-portal.md)
> * [klasszikus Azure portál](active-directory-add-domain.md)
> 

Azure Active Directory (Azure AD) segítségével adhat hozzá a vállalata tartománynevét, valamint az Azure AD. Lehetséges, hogy a tartománynév, amely akkor használnak üzleti, és jelentkezzen be a vállalati tartománynév használatával a felhasználók. A tartománynév hozzáadása az Azure AD lehetővé teszi a felhasználóneveket rendeljen a könyvtárat, amelyek a felhasználók számára, mint "alice@contoso.com." A folyamat egyszerűen végrehajtható:

1. Az egyéni tartománynév hozzáadása a címtárhoz
2. Vegye fel a DNS-bejegyzést a tartománynévhez a tartománynév-regisztrálónál
3. Az egyéni tartománynév ellenőrzése az Azure AD-ben

## <a name="how-do-i-add-a-domain-name"></a>Hogyan hozzá egy tartománynevet?
1. Jelentkezzen be a [Azure-portálon](https://portal.azure.com) egy olyan fiókkal, amely a címtár globális rendszergazdája.
2. Válassza ki **további szolgáltatások**, adja meg **Azure Active Directory** a szövegmezőbe, majd válassza ki azt a **Enter**.
   
   ![Nyitó felhasználók kezelése](./media/active-directory-domains-add-azure-portal/user-management.png)
3. Az a ***könyvtárnév*** panelen válassza **tartománynevek**.
4. Az a  ***könyvtárnév* -tartománynevek** panelen válassza a **Hozzáadás** parancsot.
   
   ![Az Add parancs kiválasztása](./media/active-directory-domains-add-azure-portal/add-command.png)
5. Az a **tartománynév** panelen írja be a mezőbe, például "contoso.com", az egyéni tartomány nevét, és válassza ki **tartomány hozzáadása**. A névben mindenképpen szerepeljen a .com, a .net vagy egyéb legfelső szintű kiterjesztés.
6. Az a ***tartománynév*** panel (Ez azt jelenti, hogy a panel, amelyen megnyitása, amelyen az új tartomány nevét a cím), az Azure AD segítségével győződjön meg arról, hogy a szervezete rendelkezik-e az egyéni tartománynév DNS-bejegyzés információk beolvasása.
   
   ![DNS-bejegyzés adatainak lekérése](./media/active-directory-domains-add-azure-portal/get-dns-info.png)

A tartománynév hozzáadását követően az Azure AD-nek ellenőriznie kell, hogy az adott tartománynév valóban a szervezete tulajdonában van-e. Mielőtt az Azure AD végre tudná hajtani ezt az ellenőrzést, DNS-bejegyzést kell hozzáadnia a tartománynév DNS zónafájljában. Ezt a feladatot a tartománynév-regisztrációs webhelyén kell végrehajtani.

## <a name="add-the-dns-entry-at-the-domain-name-registrar-for-the-domain"></a>Vegye fel a DNS-bejegyzést a tartomány tartománynév-regisztrálójánál.
A következő lépés az egyéni tartománynév Azure AD-vel történő használatához a tartomány DNS-zónafájljának frissítése. Ez lehetővé teszi, hogy az Azure AD ellenőrizze, hogy az adott tartománynév valóban a szervezete tulajdonában van-e.

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

Mostantól [hozzá tud rendelni egyéni tartománynevet tartalmazó felhasználóneveket](active-directory-users-create-azure-portal.md).

## <a name="troubleshooting"></a>Hibaelhárítás
Ha nem képes egyéni tartománynév ellenőrzésére, próbálja meg a következőket. A leggyakoribb okok ismertetésével kezdjük, és a végén megemlítünk néhány ritkábban jelentkező okot is.

1. **Várjon egy órát**. A DNS-rekordokat propagálni kell, mielőtt az Azure AD ellenőrizni tudja a tartományt. Ez egy vagy több órát is igénybe vehet.
2. **Győződjön meg róla, hogy a DNS-rekord helyesen lett megadva**. Hajtsa végre ezt a lépést a tartomány tartománynév-regisztrációs webhelyén. Az Azure AD nem tudja ellenőrizni a tartománynevet, ha a DNS-bejegyzés nincs a DNS-zónafájlban, vagy ha az nem pontosan egyezik azzal a DNS-bejegyzéssel, amelyet az Azure AD adott meg. Ha nincs hozzáférése a tartomány DNS-rekordjainak frissítéséhez a tartománynév-regisztrálónál, ossza meg a DNS-bejegyzést egy ilyen engedélyekkel rendelkező személlyel vagy csapattal, majd kérje a DNS-bejegyzés hozzáadását.
3. **Tartománynév törlése egy Azure AD-ben található másik címtárból**. A tartománynév csak egyetlen címtárban ellenőrizhető. Ha a tartománynév ellenőrzése egy másik címtárban már megtörtént, onnan előbb törölni kell, mielőtt ellenőrizni lehetne az új címtárban. Információ a tartománynevek törléséről: [Egyéni tartománynevek kezelése](active-directory-domains-manage-azure-portal.md).    

## <a name="add-more-custom-domain-names"></a>További egyéni tartománynevek hozzáadása
Ha a szervezete több tartománynevet használ (például: „contoso.com” és „contosobank.com”), legfeljebb 900 tartománynév felvételére van lehetőség. Minden egyes tartománynév hozzáadásához hajtsa végre a jelen cikkben ismertetett lépéseket.

## <a name="next-steps"></a>Következő lépések
[Egyéni tartománynevek kezelése](active-directory-domains-manage-azure-portal.md)

