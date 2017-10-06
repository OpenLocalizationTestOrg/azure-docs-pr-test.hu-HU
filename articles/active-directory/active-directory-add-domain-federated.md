---
title: "aaaAdd az egyéni tartománynevet és az összevont bejelentkezési tooAzure Active Directory beállítása |} Microsoft Docs"
description: "Hogyan tooadd a vállalati tartománynevek tooAzure Active Directory tooset összevont bejelentkezés Azure Active Directory és a helyszíni-összevonási megoldás közötti mentése"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 27126c7e-e6d6-4ef3-a4fb-f5f0706e749d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.openlocfilehash: 77f8cc87c27871ac96581360762aaa8d24c0eb8d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="add-your-custom-domain-name-tooazure-active-directory"></a>Adja hozzá az egyéni tartomány nevét tooAzure Active Directory
Konfigurálhat egyéni tartománynevet (például: „contoso.com”), így a contoso.com felhasználóinak összevont egyszeri bejelentkezési élményben lehet részük a vállalati hálózatról. Ha már rendelkezik az Active Directory összevonási szolgáltatások (AD FS) vagy egy másik összevonási kiszolgáló, a vállalati hálózaton futó, konfigurálhatja az egyéni tartománynevet hello Azure AD Connect eszköz használatával az Azure AD toouse. Az Azure AD Connect toodeploy egy új Active Directory összevonási szolgáltatások környezetet használni, és konfigurálja az összevont egyszeri bejelentkezés tooAzure AD is.

Ha nem rendelkezik, és nem tervezi toodeploy AD FS vagy egy másik összevonási kiszolgáló, kövesse az alábbi utasításokat: [hozzáadása egy egyéni tartomány nevét az Active Directory tooAzure](active-directory-add-domain.md).

## <a name="add-a-custom-domain-name-tooyour-directory"></a>Adja hozzá ezeket az egyéni tartomány nevét tooyour
1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com/) , amely az Azure AD-címtár globális rendszergazdájának felhasználói fiókkal.
2. A **Active Directory**, nyissa meg a címtárat, és válassza ki a hello **tartományok** fülre.
3. Hello parancssávon válassza **Hozzáadás**, majd adja meg az egyéni tartomány, például "contoso.com" hello nevét. Lehet, hogy tooinclude hello .com, .net vagy egyéb legfelső szintű kiterjesztés.
4. Jelölje be hello **I terv tooconfigure ebben a tartományban az egyszeri bejelentkezés a helyi Active Directory** jelölőnégyzetet.
5. Válassza a **Hozzáadás** lehetőséget.

Futtassa a hello Azure AD Connect eszköz tooget hello DNS-bejegyzést, hogy az Azure AD tooverify hello tartomány fogja használni. Látni fogja a DNS-bejegyzést hello hello **Azure AD tartományi** hello varázsló lépést. Láthatja, hogy milyen hello varázsló tűnik, hogy lépése [ezeket az utasításokat a](connect/active-directory-aadconnect-get-started-custom.md#verify-the-azure-ad-domain-selected-for-federation). Ha még nem rendelkezik hello Azure AD Connect eszközt, akkor [töltse le innen](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="add-hello-dns-entry-at-hello-domain-name-registrar-for-hello-domain"></a>Hello regisztrációs hello tartomány hello DNS-bejegyzés hozzáadása
hello az egyéni tartomány neve és az Azure AD tovább lépés toouse tooupdate hello DNS-zónafájlját hello tartományban. Ez lehetővé teszi, hogy a szervezete rendelkezik-e az egyéni tartománynév hello Azure AD tooverify.

1. Jelentkezzen be a tartománynév-regisztrációs toohello webhelyet. Ha nem rendelkezik hozzáféréssel toodo ez, kérje meg a hello személyt vagy csapatot a szervezet, aki rendelkezik a hozzáférési toocomplete 2. lépés és toolet tudja kész.
2. Frissítés hello DNS-zónafájlját hello tartomány hello DNS-bejegyzés hozzáadásával tooyou az Azure AD által biztosított. A DNS-bejegyzés lehetővé teszi, hogy az Azure AD tooverify a hello tartomány tulajdonjogát. hello DNS-bejegyzés semmilyen értelemben nem módosítja például a levélkezelési útválasztást vagy a webes üzemeltetést sem.

Az ezzel a lépéssel kapcsolatos segítségért olvassa el az [Útmutató DNS-bejegyzések hozzáadásához népszerű DNS-regisztrálókon](https://support.office.com/article/Create-DNS-records-for-Office-365-when-you-manage-your-DNS-records-b0f3fdca-8a80-4e8e-9ef3-61e8a2a9ab23/) című részt.

## <a name="verify-hello-domain-name-with-azure-ad"></a>Ellenőrizze az Azure ad-val hello tartománynevet
Hello DNS-bejegyzés hozzáadását követően készen áll a tooverify hello tartomány nevét az Azure ad-val áll.

tooverify hello tartomány, jelölje be **következő** a hello **Azure AD tartományi** hello Azure AD Connect varázsló. Az Azure AD hello DNS-bejegyzést a DNS-zónafájlját hello hello tartomány keresi. Az Azure AD csak ellenőrizze hello tartománynevet, miután hello DNS-rekord propagálása. A propagálás általában csak másodperceket vesz igénybe, de néha egy vagy több óráig is eltarthat. Ha ellenőrzési első alkalommal nem működik a hello, próbálkozzon újra később.

Ezután folytathatja a hello fennmaradó hello Azure AD Connect varázsló lépéseit. Ez szinkronizálja a Windows Server AD tooAzure AD a felhasználóknak. Összevonási beállított hello tartományban szinkronizált felhasználók fognak egy összevont egyszeri bejelentkezést a vállalati hálózat tooAzure AD tapasztalata képes tooget.

## <a name="troubleshooting"></a>Hibaelhárítás
Ha egy egyéni tartománynevet nem tudja ellenőrizni, próbálja hello következő. Először foglalkozunk hello közös és a munkahelyi legalább közös toohello le.

1. **Várjon egy órát**. DNS-rekordokat kell toopropagate, ahhoz az Azure AD tartományi hello ellenőrizheti. Ez egy vagy több órát is igénybe vehet.
2. **Győződjön meg arról, hello DNS-rekord lett megadva, és a helyes**. Ennek végrehajtásáról hello tartomány hello regisztrációs hello webhelyen. Az Azure AD hello tartománynév nem ellenőrizhető, ha hello DNS-bejegyzés nem DNS-zónafájlját hello szerepelnek, vagy ha még nincs terméknévnek pontosan egyeznie kell hello DNS-bejegyzést, hogy az Azure AD meg a megadott. Ha nem rendelkezik hozzáférési hello tartomány: hello regisztrációs tooupdate DNS-rekordjait, hello DNS-bejegyzés megosztása hello személyt vagy csapatot a szervezetnél, aki hozzáfér-e, és kérje meg őket tooadd hello DNS-bejegyzés.
3. **Hello tartománynév törlése egy másik címtárból az Azure AD**. A tartománynév csak egyetlen címtárban ellenőrizhető. Ha a tartománynév ellenőrzése egy másik címtárban már megtörtént, onnan előbb törölni kell, mielőtt ellenőrizni lehetne az új címtárban. tartománynevekkel törléséről toolearn olvasási [egyéni tartománynevek kezelése](active-directory-add-manage-domain-names.md).

## <a name="add-more-custom-domain-names"></a>További egyéni tartománynevek hozzáadása
Ha a szervezet használja több egyéni tartománynevek, például "contoso.com" és "contosobank.com", később is hozzáadhatja tooa legfeljebb 900 tartománynév fel. Ugyanaz a cikk tooadd minden tartományneve szükséges lépések hello használata.

## <a name="next-steps"></a>Következő lépések
* [Egyéni tartománynevek kezelése](active-directory-add-manage-domain-names.md)
* [Ismerkedés az Azure AD tartománykezelési fogalmaival](active-directory-add-domain-concepts.md)
* [Vállalati arculat megjelenítése a felhasználói bejelentkezés során](active-directory-add-company-branding.md)
* [Az Azure AD PowerShell toomanage tartománynevek használata](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)

