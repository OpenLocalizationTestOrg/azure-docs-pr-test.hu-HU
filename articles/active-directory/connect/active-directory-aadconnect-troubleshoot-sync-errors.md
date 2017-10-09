---
title: "Az Azure AD Connect: A szinkronizálás során hibák elhárítása |} Microsoft Docs"
description: "Ismerteti, hogyan tootroubleshoot hibákat észlelt az Azure AD Connect-szinkronizálás során."
services: active-directory
documentationcenter: 
author: karavar
manager: samueld
editor: curtand
ms.assetid: 2209d5ce-0a64-447b-be3a-6f06d47995f8
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: billmath
ms.openlocfilehash: af262dfe95d686e34697454c0dfe8b4a6d8693c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-errors-during-synchronization"></a>A szinkronizálás során hibák elhárítása
Hibák fordulhatnak elő, amikor a azonosító adatok szinkronizálása a Windows Server Active Directory (AD DS) tooAzure Active Directory (Azure AD). Ez a cikk áttekintést különböző típusú szinkronizálási hibák, néhány hello a következő eljárások, amelyek az érintett hibák, lehetséges módokon toofix hello hibái okozzák. Ez a cikk a következő hello közös hiba, és nem vonatkozhat hello lehetséges hibákat.

 Ez a cikk azt feltételezi, hogy hello olvasó ismeri az alapul szolgáló hello [tervezési alapelvek az Azure AD és az Azure AD Connect](active-directory-aadconnect-design-concepts.md).

Az Azure AD Connect legújabb verziójának hello \(augusztus 2016 vagy újabb\), szinkronizálási hibák jelentés érhető el hello [Azure Portal](https://aka.ms/aadconnecthealth) az Azure AD Connect Health szinkronizálási szolgáltatás részeként.

2016 szeptemberétől 1 indítása [Azure Active Directory ismétlődő attribútum rugalmassági](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) szolgáltatás engedélyezve lesz alapértelmezés szerint minden hello *új* Azure Active Directory-bérlő. Ez a funkció automatikusan engedélyezve lesz a meglévő bérlők számára hello jövőbeli hónapon belül.

Az Azure AD Connect szinkronban tartja hello címtárakban 3 típusú műveleteket hajtja végre: Import, a szinkronizálás és az exportálás. Hibák akkor kerül sor az összes hello műveletekben. Ez a cikk főként összpontosít hibákat exportálás tooAzure AD során.

## <a name="errors-during-export-tooazure-ad"></a>Exportálás tooAzure AD során hibák
A következő szakasz ismerteti a különböző szinkronizálási hello exportálási művelet tooAzure AD használata során előforduló hibák hello Azure AD-összekötő. Ez az összekötő segítségével azonosítható hello nevének formátuma alatt álló "contoso. *onmicrosoft.com*".
Exportálás tooAzure AD során utalnak, hogy hello művelet \(hozzáadása, frissítése és törlése stb.\) történt kísérlet az Azure AD Connect \(szinkronizálási motor\) az Azure Active Directoryban nem sikerült.

![Exportálási hiba – áttekintés](./media/active-directory-aadconnect-troubleshoot-sync-errors/Export_Errors_Overview_01.png)

## <a name="data-mismatch-errors"></a>Adatok eltérés hibák
### <a name="invalidsoftmatch"></a>InvalidSoftMatch
#### <a name="description"></a>Leírás
* Ha az Azure AD Connect \(szinkronizálási motor\) arra utasítja az Azure Active Directory tooadd vagy frissítés objektumok, az Azure AD megegyezik a bejövő hello segítségével objektum hello **sourceAnchor** toohello attribútum  **immutableId** az Azure Active Directory-objektumok attribútum. A megfelelés nevezik egy **rögzített megfelelő**.
* Ha az Azure AD **nem talál** bármely objektum, amely megfelel a hello **immutableId** hello attribútummal **sourceAnchor** hello bejövő objektum üzembe helyezése előtt attribútum egy Új objektum visszavált toouse hello ProxyAddresses, és a UserPrincipalName attribútum toofind egyezés. A megfelelés nevezik egy **egyeznek helyreállítható**. hello enyhe egyezés hello új objektumok szinkronizálás során hozzáadott vagy frissített, amelyek megfelelnek a hello már jelen van, az Azure ad-ben (azaz az Azure AD-forrása) kialakított toomatch objektumok ugyanaz az entitás (felhasználók, csoportok) a helyszínen.
* **InvalidSoftMatch** hiba akkor fordul elő, amikor hello rögzített egyezés minden egyező objektum nem található **és** enyhe egyezést talál egy hozzá tartozó objektum, de az objektumokat, amelyek különböző értékre van *immutableId* mint bejövő objektum hello *SourceAnchor*, amelyek arra utalnak, hogy a megfelelő objektum hello a helyszíni Active Directory egy másik objektum lett szinkronizálva.

Más szóval ahhoz, hogy hello egyeznek helyreállítható toowork, hello objektum toobe soft-megkeres nem kell hello bármely érték *immutableId*. Ha bármelyik rendelkező objektum *immutableId* érték beállítása sikertelen hello rögzített egyezés, de amelyek hello soft-feltételek, hello művelet InvalidSoftMatch szinkronizálási hiba eredményezne.

Az Azure Active Directory-séma nem engedélyezi két vagy több objektumot toohave hello ugyanazt az értéket a következő hello attribútumok. \(Ez nem egy tárgyal minden releváns kérdést.\)

* ProxyAddresses
* UserPrincipalName
* onPremisesSecurityIdentifier
* Objektumazonosító

> [!NOTE]
> [Az Azure AD attribútum ismétlődő attribútum rugalmassági](active-directory-aadconnectsyncservice-duplicate-attribute-resiliency.md) szolgáltatás akkor is megkezdődött az Azure Active Directory hello alapértelmezett viselkedésként.  Ez csökkenti az Azure AD Connect (valamint más szinkronizálási ügyfelek) által látott szinkronizálási hibák száma hello rugalmasabb, hogy az Azure AD duplikált ProxyAddresses és UserPrincipalName attribútum szerepel a helyszíni AD kezelési hello módja környezetekben. Ez a funkció nem oldja meg a hello duplikálva lettek-e hibák. Ezért hello adatok továbbra is kell rögzített toobe. De az új objektumokat, amelyek egyébként blokkolva vannak az Azure AD tooduplicated értékek miatt létre kiépítése lehetővé teszi. Ez hello toohello szinkronizálási ügyfél visszaadott szinkronizálási hibák számát is csökkenti.
> Ha ezt a szolgáltatást a bérlője, hello InvalidSoftMatch szinkronizálási hibák látható az új objektumok kiépítése során nem jelenik meg.
>
>

#### <a name="example-scenarios-for-invalidsoftmatch"></a>Példák InvalidSoftMatch
1. Két vagy több, ugyanazt az értéket a ProxyAddresses attribútum szerepel a helyszíni Active Directory hello objektumot. Csak az egyik van első üzembe helyezve az Azure ad-ben.
2. Két vagy több, ugyanazt az értéket a userPrincipalName szerepel a helyszíni Active Directory hello objektumot. Csak az egyik van első üzembe helyezve az Azure ad-ben.
3. Az objektum hozzá lett adva a hello a helyszíni Active Directory hello az azonos legyen, mint az Azure Active Directory objektum ProxyAddresses attribútum értéke. a helyszíni hozzáadott hello objektum nincs kiépítve első az Azure Active Directoryban.
4. Az objektum hozzá lett adva a a helyszíni Active Directory hello az azonos legyen, mint egy fiókot az Azure Active Directoryban a userPrincipalName attribútum értéke. hello objektum nincs kiépítve beolvasása az Azure Active Directoryban.
5. Egy szinkronizált fiókot az erdő A tooForest helyezték át B. az Azure AD Connect (szinkronizálási motor) használt ObjectGUID attribútum toocompute hello SourceAnchor. Hello erdő áthelyezés, után hello hello SourceAnchor értéke eltérő. Új objektum hello (a B erdő) hello meglévő objektummal toosync sikertelen Azure AD-ben.
6. Egy szinkronizált objektum véletlenül kapott a helyszíni Active Directory törlődnek, az új objektum lett létrehozva az Active Directory hello azonos entitás (például felhasználói) hello fiók az Azure Active Directoryban törlése nélkül. hello új fiók nem tud toosync hello meglévő Azure AD-objektum.
7. Az Azure AD Connect lett eltávolítása és újratelepítése. Hello újbóli telepítése során egy másik attribútum választása szerint hello SourceAnchor. Összes hello objektum, amely korábban volt-e szinkronizálva a szinkronizálás InvalidSoftMatch hiba a leállt.

#### <a name="example-case"></a>Példa eset:
1. **Bob Smith** , a szinkronizált felhasználót az Azure Active Directoryban a helyszíni Active Directory *contoso.com*
2. Bob Smith **UserPrincipalName** be van állítva az  **bobs@contoso.com** .
3. **"abcdefghijklmnopqrstuv =="** hello van **SourceAnchor** Bob Smith használata az Azure AD Connect számított **objectGUID** a helyszíni Active Directoryban, amely nem hello **immutableId** Bob Smith az Azure Active Directoryban.
4. Bob is rendelkezik a következő értékek hello **proxyAddresses** attribútum:
   * smtp:bobs@contoso.com
   * smtp:bob.smith@contoso.com
   * **smtp:bob@contoso.com**
5. Egy új felhasználóhoz **Bob Taylor**, a helyszíni Active Directory toohello kerül.
6. Bob Taylor **UserPrincipalName** be van állítva az  **bobt@contoso.com** .
7. **"abcdefghijkl0123456789 ==" "** hello van **sourceAnchor** Bob Taylor használata az Azure AD Connect számított **objectGUID** a a helyszíni Active Directoryban. Még Bob Taylor objektum nem rendelkezik szinkronizálva az Active Directory tooAzure.
8. Bob Taylor rendelkezik a következő értékek hello proxyAddresses attribútum hello
   * smtp:bobt@contoso.com
   * smtp:bob.taylor@contoso.com
   * **smtp:bob@contoso.com**
9. Szinkronizálás során az Azure AD Connect ismeri fel a helyszíni Active Directory a Bob Taylor hello hozzáadása lesz, és kérje meg az Azure AD toomake hello azonos módosítása.
10. Az Azure AD először hajt végre rögzített egyezés. Ez azt jelenti, ha bármely objektum hello immutableId egyenlő túl megkeresi "abcdefghijkl0123456789 ==". Rögzített egyeztetés sikertelen lesz, mivel nincs más objektumot az Azure ad-ben, hogy immutableId fog rendelkezni.
11. Az Azure AD majd megpróbál toosoft-match Bob Taylor. Ez azt jelenti, hogy ha értékek, beleértve a proxyAddresses egyenlő toohello bármely objektum nincs megkeresismtp:bob@contoso.com
12. Az Azure AD megkeresi Bob Smith objektum toomatch hello soft-feltételek. De ez az objektum immutableId hello értékének = "abcdefghijklmnopqrstuv ==". ami azt jelenti, hogy ez az objektum a helyszíni Active Directory a másik objektum lett szinkronizálva. Így az Azure AD nem soft-match ezek az objektumok és az eredményeket egy **InvalidSoftMatch** szinkronizálási hiba.

#### <a name="how-toofix-invalidsoftmatch-error"></a>Hogyan toofix InvalidSoftMatch hiba
hello leggyakoribb hello InvalidSoftMatch hiba oka, hogy két különböző SourceAnchor objektumok \(immutableId\) rendelkezik hello azonos hello ProxyAddresses és/vagy a UserPrincipalName attribútumok, amelyek hello során használt érték Azure ad-val folyamat Soft-egyeztetés. A sorrend toofix hello érvénytelen enyhe egyezés

1. Azonosítsa a duplikált hello proxyAddresses, userPrincipalName vagy más hello hibát okozó attribútum értéke. Is azonosítani tudja az két \(vagy több\) objektumok hello ütközés részt. a jelentés által generált hello [az Azure AD Connect Health szinkronizálási szolgáltatás](https://aka.ms/aadchsyncerrors) segítenek azonosítani azokat a két hello objektumok.
2. Határozza meg, melyik objektum toohave duplikált hello érték továbbra is, és melyik objektum nem kell.
3. Távolítsa el a duplikált hello érték hello objektumból, amely ezt az értéket nem lehet. Vegye figyelembe, hogy meg kell győződnie hello módosítása hello könyvtárban, ahol hello objektum származik. Egyes esetekben szükség lehet egy hello objektumot az ütköző toodelete.
4. A helyszíni AD hello változása hello végzett lehetővé teszik a az Azure AD Connect szinkronizálási hello módosítása.

Vegye figyelembe, hogy az Azure AD Connect Health szinkronizálási szolgáltatás belül szinkronizálási jelentést 30 percenként frissül, és a hello legutóbbi szinkronizálási kísérlet hello hibákat tartalmaz.

> [!NOTE]
> ImmutableId, definíció, nem szükséges módosítani a hello hello objektum élettartama. Ha az Azure AD Connect nincs konfigurálva néhány hello forgatókönyv szem előtt a lista fölött hello, sikerült befejezi a egy olyan esetben, ha az Azure AD Connect számítja ki, hogy jelöli hello ugyanaz az entitás hello AD-objektum SourceAnchor hello eltérő érték (ugyanahhoz a felhasználóhoz / csoport vagy partner stb.), amely rendelkezik egy meglévő Azure AD-objektum toocontinue használatával kívánja.
>
>

#### <a name="related-articles"></a>Kapcsolódó cikkek
* [Érvénytelen vagy ismétlődő attribútumok megelőzése az Office 365-ben a címtár-szinkronizálás](https://support.microsoft.com/en-us/kb/2647098)

### <a name="objecttypemismatch"></a>ObjectTypeMismatch
#### <a name="description"></a>Leírás
Az Azure AD toosoft egyezés két objektum próbál, esetén lehetséges, hogy a különböző két objektum "típusú objektum" (például felhasználó, csoport, az ügyfél stb) azonos hello attribútumok tooperform hello enyhe egyezés használt értékek hello rendelkezik. Ezek az attribútumok párhuzamos nem engedélyezett az Azure ad-ben, hello művelet "ObjectTypeMismatch" szinkronizálási hiba eredményezhet.

#### <a name="example-scenarios-for-objecttypemismatch-error"></a>Példák ObjectTypeMismatch hiba
* Engedélyezése levelezési biztonsági csoport jön létre az Office 365-ben. Rendszergazda hozzáad egy új felhasználót, vagy forduljon a helyszíni AD (amelyek AD tooAzure még nem szinkronizálta) értékeként azonos értéket hello ProxyAddresses attribútumra, mint az Office 365 hello hello a csoportban.

#### <a name="example-case"></a>Példa eset
1. Felügyelet engedélyezése levelezési új biztonsági csoportot hoz létre az Office 365-ben hello adó részleg számára, és itt egy e-mail címet, tax@contoso.com. Ez hozzárendeli hello ProxyAddresses attribútum ehhez a csoporthoz hello értékű**smtp:tax@contoso.com**
2. Új felhasználó csatlakozik a Contoso.com és a fiók jön létre a hello proxyAddress, mint a helyszíni hello felhasználó**smtp:tax@contoso.com**
3. Ha az Azure AD Connect szinkronizálja hello új felhasználói fiók, akkor hello "ObjectTypeMismatch" hiba jelenik meg.

#### <a name="how-toofix-objecttypemismatch-error"></a>Hogyan toofix ObjectTypeMismatch hiba
hello leggyakoribb hello ObjectTypeMismatch hiba oka, hogy két különböző típusú (felhasználó, csoport, az ügyfél stb) objektumoknak hello azonos hello ProxyAddresses attribútum értékét. A sorrend toofix hello ObjectTypeMismatch:

1. Duplikált hello azonosítása proxyAddresses (vagy más attribútum) érték meg, ezzel hello hiba. Is azonosítani tudja az két \(vagy több\) objektumok hello ütközés részt. a jelentés által generált hello [az Azure AD Connect Health szinkronizálási szolgáltatás](https://aka.ms/aadchsyncerrors) segítenek azonosítani azokat a két hello objektumok.
2. Határozza meg, melyik objektum toohave duplikált hello érték továbbra is, és melyik objektum nem kell.
3. Távolítsa el a duplikált hello érték hello objektumból, amely ezt az értéket nem lehet. Vegye figyelembe, hogy meg kell győződnie hello módosítása hello könyvtárban, ahol hello objektum származik. Egyes esetekben szükség lehet egy hello objektumot az ütköző toodelete.
4. A helyszíni AD hello változása hello végzett lehetővé teszik a az Azure AD Connect szinkronizálási hello módosítása. Az Azure AD Connect Health szinkronizálási szolgáltatás belül szinkronizálási jelentést 30 percenként frissül, és a hello legutóbbi szinkronizálási kísérlet hello hibákat tartalmaz.

## <a name="duplicate-attributes"></a>Ismétlődő attribútumok
### <a name="attributevaluemustbeunique"></a>AttributeValueMustBeUnique
#### <a name="description"></a>Leírás
Az Azure Active Directory-séma nem engedélyezi két vagy több objektumot toohave hello ugyanazt az értéket a következő hello attribútumok. Ez az Azure AD-ben az egyes objektumok kényszerített toohave ezeket a tulajdonságokat csak egy adott példányt egyedi érték.

* ProxyAddresses
* UserPrincipalName

Az Azure AD Connect megkísérli tooadd új objektumot, vagy objektum frissítésekor hello fent attribútumok, amelyek már hozzá van rendelve az Azure Active Directoryban tooanother objektum értékkel, hello művelet hello "AttributeValueMustBeUnique" hibát eredményez.

#### <a name="possible-scenarios"></a>A következő eljárások:
1. Ismétlődő értéke hozzárendelt tooan már szinkronizált objektum, amely ütközik egy másik szinkronizált objektum.

#### <a name="example-case"></a>Példa eset:
1. **Bob Smith** , a szinkronizált felhasználót az Azure Active Directoryban a contoso.com Active Directory helyszíni
2. Bob Smith **UserPrincipalName** a helyszíni be van állítva az  **bobs@contoso.com** .
3. Bob is rendelkezik a következő értékek hello **proxyAddresses** attribútum:
   * smtp:bobs@contoso.com
   * smtp:bob.smith@contoso.com
   * **smtp:bob@contoso.com**
4. Egy új felhasználóhoz **Bob Taylor**, a helyszíni Active Directory toohello kerül.
5. Bob Taylor **UserPrincipalName** be van állítva az  **bobt@contoso.com** .
6. **Bob Taylor** rendelkezik a következő értékek hello hello **ProxyAddresses** i attribútum. smtp:bobt@contoso.comII. smtp:bob.taylor@contoso.com
7. Bob Taylor objektum sikeresen szinkronizált Azure AD-val.
8. Rendszergazda úgy döntött, tooupdate Bob Taylor **ProxyAddresses** hello érték a következő attribútumot: i. **smtp:bob@contoso.com**
9. Az Azure AD megpróbál tooupdate Bob Taylor objektum hello érték felett az Azure AD-ben azonban, hogy sikertelen lesz hogy ProxyAddresses érték már hozzá van rendelve tooBob Smith, "AttributeValueMustBeUnique" hibát eredményezte.

#### <a name="how-toofix-attributevaluemustbeunique-error"></a>Hogyan toofix AttributeValueMustBeUnique hiba
hello leggyakoribb hello AttributeValueMustBeUnique hiba oka, hogy két különböző SourceAnchor objektumok \(immutableId\) azonos hello ProxyAddresses és/vagy a UserPrincipalName attribútum értékének hello rendelkezik. A sorrend toofix AttributeValueMustBeUnique hiba

1. Azonosítsa a duplikált hello proxyAddresses, userPrincipalName vagy más hello hibát okozó attribútum értéke. Is azonosítani tudja az két \(vagy több\) objektumok hello ütközés részt. a jelentés által generált hello [az Azure AD Connect Health szinkronizálási szolgáltatás](https://aka.ms/aadchsyncerrors) segítenek azonosítani azokat a két hello objektumok.
2. Határozza meg, melyik objektum toohave duplikált hello érték továbbra is, és melyik objektum nem kell.
3. Távolítsa el a duplikált hello érték hello objektumból, amely ezt az értéket nem lehet. Vegye figyelembe, hogy meg kell győződnie hello módosítása hello könyvtárban, ahol hello objektum származik. Egyes esetekben szükség lehet egy hello objektumot az ütköző toodelete.
4. Ha a helyszíni AD hello változása hello, lehetővé teszik az Azure AD Connect szinkronizálási hello hello hiba tooget rögzített megváltoztatása.

#### <a name="related-articles"></a>Kapcsolódó cikkek
-[Érvénytelen vagy ismétlődő attribútumok megelőzése az Office 365-ben a címtár-szinkronizálás](https://support.microsoft.com/en-us/kb/2647098)

## <a name="data-validation-failures"></a>Ellenőrzési hibával
### <a name="identitydatavalidationfailed"></a>IdentityDataValidationFailed
#### <a name="description"></a>Leírás
Az Azure Active Directory érvénybe lépteti a különböző korlátozásokat hello adatokat mozgatná adott hello könyvtárba írt adatok toobe engedélyezése előtt. Ez a tooensure, amely a végfelhasználók le hello lehetséges legjobb élmény ezek az adatok függő hello alkalmazások használata közben.

#### <a name="scenarios"></a>Forgatókönyvek
a. hello UserPrincipalName attribútum értéke érvénytelen vagy nem támogatott karaktert tartalmaz.
b. hello UserPrincipalName attribútum hello szükséges formátumot követi.

#### <a name="how-toofix-identitydatavalidationfailed-error"></a>Hogyan toofix IdentityDataValidationFailed hiba
a. Győződjön meg arról, hogy hello userPrincipalName attribútum támogatott tartalmaz karakterek és a megkívánt formátumban.

#### <a name="related-articles"></a>Kapcsolódó cikkek
* [Címtár-szinkronizálás tooOffice 365 tooprovision felhasználókon előkészítése](https://support.office.com/en-us/article/Prepare-to-provision-users-through-directory-synchronization-to-Office-365-01920974-9e6f-4331-a370-13aea4e82b3e)

### <a name="federateddomainchangeerror"></a>FederatedDomainChangeError
#### <a name="description"></a>Leírás
Ez az egy adott esetben az eredmények a **"FederatedDomainChangeError"** szinkronizálási hiba, a felhasználó UserPrincipalName hello utótagja tartományból egy összevont tartományt tooanother összevont megváltozásakor.

#### <a name="scenarios"></a>Forgatókönyvek
Egy szinkronizált felhasználó hello UserPrincipalName utótag egy összevont tartományt tooanother összevont tartományt a helyszíni változott. Például *UserPrincipalName = bob@contoso.com*  túl megváltozott*UserPrincipalName = bob@fabrikam.com* .

#### <a name="example"></a>Példa
1. Bob Smith, egy fiókot a Contoso.com, egy új felhasználót az Active Directory hello UserPrincipalName lekérdezi fel van vévebob@contoso.com
2. Bob áthelyezi tooa különböző osztás a Contoso.com, Fabrikam.com és a UserPrincipalName neve megváltozotttoobob@fabrikam.com
3. A contoso.com és fabrikam.com tartományai az Azure Active Directoryval összevont tartományt.
4. Bob userPrincipalName nem frissül, és a "FederatedDomainChangeError" hibát eredményez.

#### <a name="how-toofix"></a>Hogyan toofix
Ha egy felhasználó UserPrincipalName utótagot a bob @ frissült**contoso.com** toobob @**fabrikam.com**, ahol mindkettő **contoso.com** és **fabrikam.com** vannak **tartományok összevont**, és kövesse az alábbi lépéseket toofix hello szinkronizálási hiba

1. Az Azure AD-ben hello felhasználói UserPrincipalName frissítése bob@contoso.com toobob@contoso.onmicrosoft.com. A következő PowerShell-parancsot hello Azure AD PowerShell modult hello használhatja:`Set-MsolUserPrincipalName -UserPrincipalName bob@contoso.com -NewUserPrincipalName bob@contoso.onmicrosoft.com`
2. Hello következő szinkronizálási ciklusban tooattempt szinkronizálásának engedélyezése. Az időszinkronizálást sikeres lesz, és frissíti a UserPrincipalName a Bob hello toobob@fabrikam.com várt módon.

#### <a name="related-articles"></a>Kapcsolódó cikkek
* [A felhasználói fiók toouse másik összevont tartományt egyszerű hello módosítása után a módosítások nem hello Azure Active Directory szinkronizáló eszköz által szinkronizálása](https://support.microsoft.com/en-us/help/2669550/changes-aren-t-synced-by-the-azure-active-directory-sync-tool-after-you-change-the-upn-of-a-user-account-to-use-a-different-federated-domain)

## <a name="largeobject"></a>LargeObject
### <a name="description"></a>Leírás
Ha attribútum meghaladja a megengedett maximális mérete, a korlát vagy az Azure Active Directory-séma által beállított maximális száma hello, hello szinkronizálási művelet eredménye hello **LargeObject** vagy **ExceededAllowedLength** szinkronizálási hiba. Általában ez a hiba akkor fordul elő, a következő attribútumok hello

* userCertificate
* userSMIMECertificate
* thumbnailPhoto
* proxyAddresses

### <a name="possible-scenarios"></a>A következő eljárások
1. Bob userCertificate attribútum túl sok tanúsítványok hozzárendelése tooBob tárolja. Ezek közé tartozhatnak a régebbi, a lejárt tanúsítványok. hello rögzített korlátját 15 tanúsítványok. Hogyan toohandle LargeObject hibák userCertificate attribútumot, adjon további tájékoztatást tekintse meg a tooarticle [userCertificate attribútum által okozott kezelése LargeObject hibákat](active-directory-aadconnectsync-largeobjecterror-usercertificate.md).
2. Bob userSMIMECertificate attribútum túl sok tanúsítványok hozzárendelése tooBob tárolja. Ezek közé tartozhatnak a régebbi, a lejárt tanúsítványok. hello rögzített korlátját 15 tanúsítványok.
3. Bob thumbnailPhoto az Active Directoryban beállított értéke túl nagy toobe szinkronizált az Azure AD-ben.
4. Hello ProxyAddresses attribútum az Active Directory automatikus feltöltése során az objektumnak túl sok ProxyAddresses hozzárendelve.

### <a name="how-toofix"></a>Hogyan toofix
1. Győződjön meg arról, hogy hello attribútum hello hiba okozza korlátozás engedélyezett hello belül.

## <a name="related-links"></a>Kapcsolódó hivatkozások
* [Keresse meg az Active Directory-objektumok az Active Directory felügyeleti központ](https://technet.microsoft.com/library/dd560661.aspx)
* [Hogyan tooquery Azure Active Directoryban egy Azure Active Directory PowerShell-lel objektum számára](https://msdn.microsoft.com/library/azure/jj151815.aspx)
