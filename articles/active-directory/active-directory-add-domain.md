---
title: "aaaAdd az egyéni tartomány neve az Active Directory tooAzure |} Microsoft Docs"
description: "Hogyan tooadd a vállalati tartománynevek tooAzure Active Directory, és hogyan tooverify hello tartomány nevét."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 35a6e20a-9907-432b-9d36-16b916a5c249
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: eb208138f2633aaecc54f68dc947caf80d856d23
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
> 

Van egy vagy több tartománynevet, hogy a szervezet üzleti toodo használ, és a felhasználói bejelentkezés a vállalati hálózat tooyour vállalati tartománynév használatával. Most, hogy az Azure Active Directory (Azure AD) használ, a vállalati tartomány neve tooAzure AD is hozzáadhat. Ez lehetővé teszi tooassign felhasználónevek hello könyvtárban, amelyek a megszokott tooyour felhasználók, például a "alice@contoso.com." hello folyamat felettébb egyszerű:

1. Hello egyéni tartomány nevét tooyour könyvtár hozzáadása
2. Hello regisztrációs hello tartománynévhez tartozó DNS-bejegyzés hozzáadása
3. Ellenőrizze a hello egyéni tartomány nevét az Azure ad-ben

> [!IMPORTANT]
> A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon. Hogyan tooadd a vállalata tartománynevét hello Azure AD felügyeleti központban, tekintse meg a [rendszergazdai szerepkörök hozzárendelése az Azure Active Directory](active-directory-domains-add-azure-portal.md).

Ha azt tervezi, tooconfigure az Active Directory összevonási szolgáltatások (AD FS) vagy egy másik biztonsági jogkivonat-szolgáltatás (STS) a vállalati hálózaton használt egyéni tartomány nevét toobe, kövesse a hello utasításait [hozzáadása és egy tartományt a konfigurálása az Azure Active Directory összevonási](active-directory-add-domain-federated.md). Ez akkor hasznos, ha azt tervezi, toosynchronize felhasználók a vállalati címtárban tooAzure AD, a és [Jelszókivonat-szinkronizálás](active-directory-aadconnectsync-implement-password-synchronization.md) nem felel meg a követelményeknek.

## <a name="add-a-custom-domain-name-tooyour-directory"></a>Adja hozzá ezeket az egyéni tartomány nevét tooyour
1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/) , amely az Azure AD-címtár globális rendszergazdájának felhasználói fiókkal.
2. A **Active Directory**, nyissa meg a címtárat, és válassza ki a hello **tartományok** fülre.
3. Hello parancssávon válassza **Hozzáadás**. Adja meg az egyéni tartomány, például "contoso.com" hello nevét. Ellenőrizze, hogy tooinclude hello .com, .net vagy egyéb legfelső szintű kiterjesztés, és az "egyszeri bejelentkezéshez" hello jelölőnégyzetet hagyja bejelölve (összevonási).
4. Válassza a **Hozzáadás** lehetőséget.
5. Hello hello tartomány hozzáadása varázsló második lapján DNS-bejegyzést, hogy az Azure AD fogja használni, hogy a szervezete rendelkezik-e az egyéni tartománynév hello tooverify hello beolvasása.

Most, hogy a felvett hello tartománynév, az Azure AD ellenőriznie kell, hogy a szervezete rendelkezik hello tartomány nevét. Mielőtt az Azure AD végre tudná hajtani ezt az ellenőrzést, fel kell vennie egy DNS-bejegyzés hello hello tartománynév DNS zónafájljában. Ez a feladat hello tartománynév-regisztrációs hello webhelyen történik.

## <a name="add-hello-dns-entry-at-hello-domain-name-registrar-for-hello-domain"></a>Hello regisztrációs hello tartomány hello DNS-bejegyzés hozzáadása
hello az egyéni tartomány neve és az Azure AD tovább lépés toouse tooupdate hello DNS-zónafájlját hello tartományban. Ez lehetővé teszi, hogy a szervezete rendelkezik-e az egyéni tartománynév hello Azure AD tooverify.

1. Jelentkezzen be toohello regisztrációs hello tartomány. Ha még nem rendelkezik hozzáféréssel tooupdate hello DNS-bejegyzés, kérje meg a hello személy vagy a hozzáférés toocomplete 2. lépés rendelkező csoportját, és kész tudja toolet.
2. Frissítés hello DNS-zónafájlját hello tartomány hello DNS-bejegyzés hozzáadásával tooyou az Azure AD által biztosított. A DNS-bejegyzés lehetővé teszi, hogy az Azure AD tooverify a hello tartomány tulajdonjogát. hello DNS-bejegyzés semmilyen értelemben nem módosítja például a levélkezelési útválasztást vagy a webes üzemeltetést sem.

Súgó a hozzáadása hello DNS-bejegyzés használatával, olvassa el a [utasításokat a DNS-bejegyzés hozzáadásához népszerű DNS-regisztráló szervezetek:](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/)

## <a name="verify-hello-domain-name-with-azure-ad"></a>Ellenőrizze az Azure ad-val hello tartománynevet
Hello DNS-bejegyzés hozzáadását követően készen áll a tooverify hello tartomány nevét az Azure ad-val áll.

Ha továbbra is fennáll hello **tartomány hozzáadása** varázsló megnyitása, válasszon **ellenőrizze** hello hello varázsló harmadik oldalán. Ha bejelöli **ellenőrizze**, az Azure AD hello DNS-bejegyzést a DNS-zónafájlját hello hello tartományhoz fog keresni. Az Azure AD hello tartománynév ellenőrizheti, csak azt követően hello DNS-rekord propagálása. Ez a propagálás általában csak másodperceket vesz igénybe, de néha egy vagy több óráig is eltarthat. Ha ellenőrzési első alkalommal nem működik a hello, próbálkozzon újra később.

Ha hello **tartomány hozzáadása** varázsló nincs megnyitva, ellenőrizheti a hello hello tartomány [a klasszikus Azure portálon](https://manage.windowsazure.com/):

1. Jelentkezzen be az Azure AD-címtár globális rendszergazdájának felhasználói fiókjával.
2. Nyissa meg a könyvtárhoz, és válassza ki a hello **tartományok** fülre.
3. Jelölje be hello tartománynév tooverify szeretne, és válassza ki **ellenőrizze** hello parancssávon.
4. Válassza ki **ellenőrizze** hello párbeszédpanel bezárásához toocomplete hello ellenőrzése.

Mostantól [hozzá tud rendelni egyéni tartománynevet tartalmazó felhasználóneveket](active-directory-add-domain-add-users.md).

## <a name="troubleshooting"></a>Hibaelhárítás
Ha egy egyéni tartománynevet nem tudja ellenőrizni, próbálja hello következő. Először foglalkozunk hello közös és a munkahelyi legalább közös toohello le.

1. **Várjon egy órát**. DNS-rekordokat kell toopropagate, ahhoz az Azure AD tartományi hello ellenőrizheti. Ez egy vagy több órát is igénybe vehet.
2. **Győződjön meg arról, hello DNS-rekord lett megadva, és a helyes**. Ennek végrehajtásáról hello tartomány hello regisztrációs hello webhelyen. Az Azure AD hello tartománynév nem ellenőrizhető, ha hello DNS-bejegyzés nem DNS-zónafájlját hello szerepelnek, vagy ha még nincs terméknévnek pontosan egyeznie kell hello DNS-bejegyzést, hogy az Azure AD meg a megadott. Ha nem rendelkezik hozzáférési hello tartomány: hello regisztrációs tooupdate DNS-rekordjait, hello DNS-bejegyzés megosztása hello személyt vagy csapatot a szervezetnél, aki hozzáfér-e, és kérje meg őket tooadd hello DNS-bejegyzés.
3. **Hello tartománynév törlése egy másik címtárból az Azure AD**. A tartománynév csak egyetlen címtárban ellenőrizhető. Ha a tartománynév ellenőrzése egy másik címtárban már megtörtént, onnan előbb törölni kell, mielőtt ellenőrizni lehetne az új címtárban. tartománynevekkel törléséről toolearn olvasási [egyéni tartománynevek kezelése](active-directory-add-manage-domain-names.md).

## <a name="add-more-custom-domain-names"></a>További egyéni tartománynevek hozzáadása
Ha a szervezet használja több egyéni tartománynevek, például "contoso.com" és "contosobank.com", később is hozzáadhatja tooa legfeljebb 900 tartománynév fel. Ugyanaz a cikk tooadd minden tartományneve szükséges lépések hello használata.

## <a name="next-steps"></a>Következő lépések
* [Egyéni tartománynevet tartalmazó felhasználónevek hozzárendelése](active-directory-add-domain-add-users.md)
* [Egyéni tartománynevek kezelése](active-directory-add-manage-domain-names.md)
* [Ismerkedés az Azure AD tartománykezelési fogalmaival](active-directory-add-domain-concepts.md)
* [Vállalati arculat megjelenítése a felhasználói bejelentkezés során](active-directory-add-company-branding.md)
* [Az Azure AD PowerShell toomanage tartománynevek használata](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

