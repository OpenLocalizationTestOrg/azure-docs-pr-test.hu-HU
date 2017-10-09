---
title: "Az Azure AD Connect: Ha már rendelkezik az Azure AD |} Microsoft Docs"
description: "Ez a témakör ismerteti, hogyan toouse kapcsolódnak, ha van meglévő Azure AD-bérlő."
services: active-directory
documentationcenter: 
author: andkjell
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/13/2017
ms.author: billmath
ms.openlocfilehash: f7b166e9e85c1f03330e8e0074d4afa4036738c2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-when-you-have-an-existent-tenant"></a>Az Azure AD Connect: Ha rendelkezik egy létező bérlő
Hello témaköröket hogyan toouse az Azure AD Connect feltételezi, hogy a legtöbb kezdődhet egy új Azure AD-bérlő és, hogy nincsenek-e egy felhasználó sem, vagy nincs más objektumokat. Azonban ha az Azure AD-bérlő indította megadva, akkor a felhasználók és más objektumok, és most szeretné, hogy toouse csatlakozás, majd ebben a témakörben, hogy a felhasználó.

## <a name="hello-basics"></a>hello alapjai
Az Azure AD-objektum vagy hello felhő (az Azure AD) vagy a helyszíni értékűre. Egy egyetlen objektumhoz nem kezelheti az egyes attribútumait a helyszíni és néhány egyéb attribútumai Azure AD-ben. Minden objektum rendelkezik egy jelzőt, amely megadja, ahol felügyelt hello objektum.

Kezelheti egy helyi felhasználók és más hello felhőben. Egy általános forgatókönyv ehhez a konfigurációhoz fiókkezelési dolgozó munkatársak és az értékesítési munkavállalók rendelkező szervezeteknél. hello fiókkezelési munkavállalók rendelkezik egy helyszíni AD-fiókot, de az értékesítési munkavállalók hello nem, az Azure AD-fiókkal rendelkeznek. Egyes felhasználók a helyszíni és néhány Azure AD-ben ugyanúgy felügyelheti.

Ha toomanage felhasználók használatba az Azure ad-ben, amelyek szerepelnek a helyszíni AD és később szeretné toouse Connect, akkor néhány további szempontok tooconsider szüksége van.

## <a name="sync-with-existing-users-in-azure-ad"></a>A meglévő felhasználók szinkronizálni az Azure ad-ben
Az Azure AD Connectet telepíti, és szinkronizálásának elindítása, hello Azure AD sync szolgáltatás (az Azure AD) ne ellenőrzése minden új objektumot, majd próbálja toofind egy meglévő objektum toomatch. Ez a folyamat használt három attribútum: **userPrincipalName**, **proxyAddresses**, és **sourceAnchor**/**immutableID**. Egyeztetés az **userPrincipalName** és **proxyAddresses** is ismert, egy **egyeznek helyreállítható**. Egyeztetés az **sourceAnchor** nevezik **rögzített egyezés**. A hello **proxyAddresses** attribútum csak hello érték a **SMTP:**, amely hello elsődleges e-mail címét, hello értékeléséhez használt.

hello egyezés csak értékeli ki a Connect érkező új objektumokat. Ha módosítja a meglévő objektum, akkor van megfelelő ezek az attribútumok, majd hibaüzenet jelenik meg helyette.

Ha az Azure AD talál, ahol hello attribútum értékei hello objektum hamarosan objektumhoz ugyanaz Connect, és hogy az már telepítve az Azure ad-ben, majd hello az Azure AD-objektum veszi át az irányítást Connect. hello korábban felhőben felügyelt objektum meg van jelölve, a helyszínen felügyelt. Összes attribútumot az Azure AD a érték szerepel a helyszíni AD hello helyszíni értékű felülíródnak. hello kivétel akkor, ha az attribútumhoz egy **NULL** helyszíni érték. Ebben az esetben hello értéke az Azure AD marad, de továbbra is csak módosíthatja azt a helyszíni toosomething más.

> [!WARNING]
> Óta az összes attribútum az Azure AD toobe hello helyszíni érték felül fog, győződjön meg arról, hogy a helyes adatok a helyszínen. Például ha csak rendelkezik felügyelt e-mail címet az Office 365-ben, és nem tartja azt frissítése az helyszíni Active Directory tartományi Szolgáltatásokban, majd az Azure AD/Office 365 rendszerben az AD DS-ben nem található értékeket elvesznek.

> [!IMPORTANT]
> Ha jelszó-szinkronizálás, amelyet a gyorsbeállítások mindig használ, akkor hello jelszót az Azure AD felülírja a helyszíni hello jelszót AD. Ha a felhasználók használt toomanage különböző jelszót, majd tooinform kell őket, amelyet használnia kell hello helyszíni jelszó Connect telepítését.

a tervezés hello előző szakaszban és figyelmeztető kell tekinteni. Ha sok módosításokat végzett az Azure ad-ben nem kerülnek a helyszíni Active Directory tartományi Szolgáltatásokban, tooplan az Active Directory tartományi Szolgáltatásokban a hello toopopulate frissítésének értékek az objektumok az Azure AD Connect szinkronizálása előtt kell használnia.

Ha megfelel a soft-egyezés objektumokat, majd hello **sourceAnchor** kerül toohello objektum az Azure ad-ben, rögzített egyezés később is használható.

### <a name="hard-match-vs-soft-match"></a>Merevlemez-match vs Soft-egyezés
Új telepítés Connect nincs gyakorlati különbség a soft - és merevlemez-egyezés között. hello különbség van a vész-helyreállítási helyzet. Ha a kiszolgáló és az Azure AD Connect elvesztette, telepítse újra egy új példányt adatvesztés nélkül. A sourceAnchor objektum tooConnect zajlik a kezdeti telepítés során. hello egyezés majd kiértékelése hello ügyfél (az Azure AD Connect), amely sokkal gyorsabb, mint ezzel hello azonos Azure AD-ben. Csatlakozás és az Azure AD rögzített egyezés értékeli. Az Azure AD csak kiértékeli a letölthető egyezés.

### <a name="other-objects-than-users"></a>Más objektumok, mint a felhasználók
Általában felhasználóknál userPrincipalName és a proxyAddresses, hogy könnyen hello egyezés. De más objektumok, például a biztonsági csoportok, nem kell azokat. Ebben az esetben csak párosítható a hello sourceAnchor használatával rögzített egyezés. hello sourceAnchor mindig Base64 konvertálni hello **objectGUID** a helyszínen, így hello érték frissítenie kell az Azure AD, ha két objektumok toomatch van szükség. hello sourceAnchor/immutableID csak akkor lehet frissíteni, a PowerShell segítségével, és nem hello portálon keresztül.

## <a name="create-a-new-on-premises-active-directory-from-data-in-azure-ad"></a>Hozzon létre egy új helyszíni Active Directory adatokból az Azure ad-ben
Egyes ügyfelek egy kizárólag felhőalapú megoldást az Azure ad-val kezdődő, és nem rendelkeznek helyszíni AD. Később azok tooconsume a helyszíni erőforrások és toobuild egy helyszíni AD az Azure AD-adatok alapján. Az Azure AD Connect nem tud segítséget ehhez a forgatókönyvhöz. Nem hoz létre helyszíni felhasználók, és nem rendelkezik semmilyen képességét tooset hello jelszó helyszíni toohello ugyanaz, mint az Azure AD.

Ha hello csak ok miért tooadd tervezi a helyszíni AD toosupport LOB (az üzletági alkalmazások), akkor talán érdemes toouse [az Azure AD tartományi szolgáltatások](../../active-directory-domain-services/index.md) helyette.

## <a name="next-steps"></a>Következő lépések
További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md).
