---
title: "aaaIdentity szinkronizálási és duplikált attribútum rugalmassági |} Microsoft Docs"
description: "Új viselkedését hogyan toohandle objektumokat az egyszerű Felhasználónevet vagy ProxyAddress ütközések használata az Azure AD Connect címtár-szinkronizálás során."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 537a92b7-7a84-4c89-88b0-9bce0eacd931
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: markvi
ms.openlocfilehash: e27dcbf9d71f83fa9566cae2fd99350297d1cd9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="identity-synchronization-and-duplicate-attribute-resiliency"></a>Identitásszinkronizálás és ismétlődő attribútumok rugalmassága
Ismétlődő attribútum rugalmasság az Azure Active Directoryban, amely kiküszöböli a okozta súrlódás szolgáltatása **UserPrincipalName** és **ProxyAddress** ütközik a Microsoft egyik futtatásakor szinkronizálási eszközöket.

Ez a két attribútum általában szükség toobe egyedi összes **felhasználói**, **csoport**, vagy **forduljon** objektumok egy adott Azure Active Directory-bérlő.

> [!NOTE]
> Csak a felhasználók rendelkezhetnek UPN-EK.
> 
> 

hello új viselkedését, hogy ez a funkció lehetővé teszi, hogy megtalálható-e hello szinkronizálási folyamat hello felhő része, ezért a független és bármely Microsoft szinkronizálási termékhez, beleértve az Azure AD Connect, a DirSync és a MIM + összekötő vonatkozó ügyfél. Ez a dokumentum toorepresent ezeket a termékeket valamelyikét használt hello általános kifejezés "sync ügyfél".

## <a name="current-behavior"></a>Jelenlegi viselkedése
Ha egy kísérlet tooprovision egy egyszerű felhasználónév vagy ProxyAddress érték, amely megsérti az egyediségre vonatkozó új objektumot, Azure Active Directory blokkolja az objektum a létrehozás alatt áll. Hasonlóképpen, ha nem egyedi UPN vagy ProxyAddress frissül egy objektum, hello frissítése sikertelen. Kísérlet történt vagy frissítés kiosztása hello hello Szinkronizáló ügyfél minden exportálási ciklus során a rendszer ismét megkísérli, és toofail továbbra is fennáll, addig, amíg hello ütközés fel lett oldva. Hiba történt a jelentés e-mailt minden kísérlet után történik, és hello Szinkronizáló ügyfél által naplózott hiba.

## <a name="behavior-with-duplicate-attribute-resiliency"></a>Ismétlődő attribútum rugalmasságú viselkedése
Teljesen helyett hiányában tooprovision, vagy frissíteni egy objektumot a duplikált attribútummal rendelkező Azure Active Directory "karanténba" hello ismétlődő attribútuma, amely sértené hello egyediségi megszorítást. Ha ez az attribútum szükséges történő üzembe helyezéséhez, UserPrincipalName, például a hello szolgáltatás hozzárendel egy helyőrző értéket. hello formátuma a következő ideiglenes értékek  
"***<OriginalPrefix>+ < 4DigitNumber > @<InitialTenantDomain>. onmicrosoft.com***".  
Hello attribútum nincs szükség, ha például egy **ProxyAddress**, Azure Active Directory egyszerűen karanténba hello ütközés attribútum, és a hello objektum-létrehozás vagy frissítés, majd.

Karanténba helyezés hello attribútum, akkor hello ütközés kapcsolatos információkat küldött ugyanazon hiba történt a jelentés e-mailek használt hello hello régi működésében. Azonban ezeket az adatokat csak egyszer jelenik meg hello esetleges hibajelentésben való megjelenítéshez, ha hello karantén történik, nem továbbra is toobe naplózza a jövőben e-maileket. Is mivel ehhez az objektumhoz hello exportálás sikeresen befejeződött, hello sync-ügyfél nem naplózza a hibát, és nem nem az újrapróbálkozási hello létrehozása / frissítése után a következő szinkronizálási ciklusok művelet.

Ez a jelenség új attribútum lett toosupport toohello felhasználó, csoport és az elérhetőségi objektumosztályok hozzáadva:  
**DirSyncProvisioningErrors**

Ez a használt toostore hello ütköző attribútumokat, amelyek sértené hello egyediségi megszorítást kell adni őket általában egy többértékű attribútumok. Időzítő háttérfeladat engedélyezve van az Azure Active Directoryban, amely minden órában toolook ismétlődő attribútum ütközések feloldása, és automatikusan eltávolítja a szóban forgó hello attribútumok karanténba zárt futtatja.

### <a name="enabling-duplicate-attribute-resiliency"></a>Ismétlődő attribútum rugalmassági engedélyezése
Ismétlődő attribútum rugalmassági lesz hello alapértelmezett viselkedést összes Azure Active Directory-bérlők között. Alapértelmezés szerint egyetlen bérlő számára, amely először 2016 augusztusától 22. vagy újabb hello szinkronizálás engedélyezve lesz a. Bérlői, szinkronizálási előzetes toothis dátum engedélyezve lesz hello szolgáltatás kötegekben engedélyezve van. 2016 szeptemberétől bevezetés megkezdődik, és e-mailben értesítést küld tooeach bérlői technikai értesítés forduljon hello adott dátumot, amikor hello szolgáltatás engedélyezve lesz.

> [!NOTE]
> Amennyiben az ismétlődő attribútum rugalmassági be van kapcsolva nem lehet letiltani.

Ha hello szolgáltatás engedélyezve van a bérlő toocheck, ehhez hello hello Azure Active Directory PowerShell-modul legújabb verziójának letöltése és futtatása:

`Get-MsolDirSyncFeatures -Feature DuplicateUPNResiliency`

`Get-MsolDirSyncFeatures -Feature DuplicateProxyAddressResiliency`

> [!NOTE]
> Már nem használhatja a Set-MsolDirSyncFeature parancsmag tooproactively engedélyezése hello ismétlődő attribútum rugalmassági szolgáltatást, mielőtt van kapcsolva a bérlő. toobe képes tootest hello szolgáltatást, szüksége lesz egy új Azure Active Directory-bérlő toocreate.

## <a name="identifying-objects-with-dirsyncprovisioningerrors"></a>A DirSyncProvisioningErrors objektumok azonosítása
Kétféleképpen jelenleg ezek a hibák miatt tooduplicate tulajdonság ütközések, az Azure Active Directory PowerShell és az Office 365 felügyeleti portál hello tooidentify objektumokra. Nincsenek tervek tooextend tooadditional portal alapú jövőbeli hello jelentéskészítés érdekében.

### <a name="azure-active-directory-powershell"></a>Azure Active Directory PowerShell
Ez a témakör a PowerShell-parancsmagok hello hello következő értéke igaz:

* A következő parancsmagok hello mindegyike kis-és nagybetűket.
* Hello **– ErrorCategory PropertyConflict** mindig szerepelnie kell függvénykötésnek. Jelenleg nincs más típusú **ErrorCategory**, de ez a jövőbeni hello kiterjeszthető.

Először futtassa a kezdéshez **Connect-MsolService** és a hitelesítő adatok megadása egy Bérlői rendszergazda.

Majd használja a következő parancsmagok és operátorok tooview hibák különböző módon hello:

1. [Összes](#see-all)
2. [Tulajdonság típus szerint](#by-property-type)
3. [Ütköző értékkel](#by-conflicting-value)
4. [Egy karakterlánc-keresés](#using-a-string-search)
5. [Rendezve](#sorted)
6. [Az összes vagy egy korlátozott mennyiség](#in-a-limited-quantity-or-all)

#### <a name="see-all"></a>Az összes megtekintése
A csatlakozás után toosee kiépítési hibák hello bérlői attribútum általános listájának futtassa:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict`

Ennek eredménye egy hasonló hello:  
 ![Get-MsolDirSyncProvisioningError](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1.png "Get-MsolDirSyncProvisioningError")  

#### <a name="by-property-type"></a>Tulajdonság típus szerint
tulajdonság típus szerinti toosee hibák hozzáadása hello **- PropertyName** jelzőt mellékel hello **UserPrincipalName** vagy **ProxyAddresses** argumentum:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName UserPrincipalName`

Vagy

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyName ProxyAddresses`

#### <a name="by-conflicting-value"></a>Ütköző értékkel
tooa adott tulajdonságra vonatkozó toosee hibák hozzáadása hello **- PropertyValue** jelző (**- PropertyName** kell használni, valamint a jelző hozzáadásakor):

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -PropertyValue User@domain.com -PropertyName UserPrincipalName`

#### <a name="using-a-string-search"></a>Egy karakterlánc-keresés
a széles körű karakterlánc keresési toodo hello használata **- Keresési_karakterlánc** jelzőt. Az összes fent jelzők hello kivételével hello egymástól függetlenül használható **- ErrorCategory PropertyConflict**, amely mindig szükség:

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -SearchString User`

#### <a name="in-a-limited-quantity-or-all"></a>Az összes vagy egy korlátozott mennyiség
1. **MaxResults <Int>**  lehet toolimit hello lekérdezés tooa adott számú értéket használja.
2. **Minden** lehet használt tooensure, hogy a rendszer minden eredmény hello esetben hibák nagy számú létező kérdezi le.

`Get-MsolDirSyncProvisioningError -ErrorCategory PropertyConflict -MaxResults 5`

## <a name="office-365-admin-portal"></a>Office 365 felügyeleti portál
Címtár-szinkronizálási hibák az hello Office 365 felügyeleti központjában tekintheti meg. hello Office 365 portál csak jeleníti meg a jelentés hello **felhasználói** objektumok, amelyek ezeket a hibákat. Az információ közötti ütközések nem megjelenítése **csoportok** és **névjegyek**.

![Aktív felhasználók](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/1234.png "aktív felhasználók")

Hogyan tooview directory objektumszinkronizálási hibák hello Office 365 felügyeleti központ, lásd: [az Office 365-ben a címtár szinkronizálási hibák](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067).

### <a name="identity-synchronization-error-report"></a>Identitás szinkronizálási hibajelentés
Ismétlődő attribútum ütközés objektum értesítést tartalmazza az új viselkedéssel kezelésekor identitás szinkronizálási hiba jelentés e-mailek, hello standard küldött toohello technikai értesítés forduljon hello bérlő számára. Van azonban fontos változás az ezt a viselkedést. Az elmúlt hello ismétlődő attribútum ütközés információ szerepel minden későbbi esetleges hibajelentésben való megjelenítéshez amíg hello ütközés feloldásának. Az új viselkedéssel hello hiba értesítést az adott ütközés csak jelennek meg többször - hello ütköző attribútum karanténba hello időpontban.

Íme egy példa egy ProxyAddress ütközés néz milyen hello e-mailben értesítést:  
    ![Aktív felhasználók](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "aktív felhasználók")  

## <a name="resolving-conflicts"></a>Az ütközések feloldása
Hibaelhárítási stratégia és megoldás szerint taktikai a hibák kell nem ismétlődő attribútum-hibákat a korábbi hello kezelt hello módon különböznek. hello egyetlen különbség, hogy hello időzítő feladat el a hello Szolgáltatásoldali tooautomatically hello bérlői keresztül hello attribútum hozzáadása kérdés toohello megfelelő objektumban után hello ütközés meg van oldva.

hello következő cikke ismerteti különböző hibakeresési és feloldási stratégiák: [duplikált vagy érvénytelen attribútumok megelőzése az Office 365-ben a címtár-szinkronizálás](https://support.microsoft.com/kb/2647098).

## <a name="known-issues"></a>Ismert problémák
Ezek a problémák egyike hatására az adatok elvesztése vagy szolgáltatás teljesítménycsökkenést. Ezek esztétikai, a többi standard "*előtti rugalmassági*" ismétlődő attribútum hibák toobe hello ütközés attribútum, és egy másik karanténba helyezés helyett fel okoz bizonyos hibák toorequire extra manuális javítás felfelé.

**Core viselkedést:**

1. A specifikus attribútummal konfigurációival objektumok tooreceive exportálási hibák folytatható, mert a karanténba helyezett megakadályozását toohello duplikált attribútum.  
   Példa:
   
    a. Új felhasználó jön létre egy egyszerű az Active Directory  **Joe@contoso.com**  és ProxyAddress**smtp:Joe@contoso.com**
   
    b. hello Ez az objektum tulajdonságainak ütközik egy meglévő csoportot, ahol ProxyAddress az  **SMTP:Joe@contoso.com** .
   
    c. Exportáláskor egy **ProxyAddress ütközés** hibát vált ki, ahelyett, hogy hello ütközés attribútumok karanténba helyezve. hello művelet megismétlése minden ezt követő szinkronizálási ciklust követően, mielőtt hello rugalmassági szolgáltatás engedélyezve lett volna.
2. Ha a két csoport jön létre a helyszíni az hello azonos SMTP-cím, egy sikertelen tooprovision hello ismétlődő szabványos az első kísérlet alkalmával **ProxyAddress** hiba. Azonban hello ismétlődő értéket megfelelően karanténba hello után következő szinkronizálási ciklusban.

**Office-portál jelentés**:

1. hello részletes hibaüzenet a következő két objektumok egy egyszerű felhasználónév ütközés készlet: hello azonos. Ez azt jelzi, hogy azok mind volt az egyszerű Felhasználónevük megváltozott / karanténba helyezve, amikor tulajdonképpen csak egy ezek közül kellett módosult adatokat.
2. részletes hibaüzenetet hello UPN ütközés hello helytelen displayName egy felhasználóhoz, aki volt-e az egyszerű Felhasználónevük megváltozott vagy karanténba helyezve jeleníti meg. Példa:
   
    a. **A felhasználó** fel az első szinkronizálásának **UPN = User@contoso.com** .
   
    b. **"B" felhasználó** megkísérelt toobe be a következő szinkronizált **UPN = User@contoso.com** .
   
    c. **Felhasználói B** UPN túl megváltozott **User1234@contoso.onmicrosoft.com**  és  **User@contoso.com**  túl szerepel-e**DirSyncProvisioningErrors**.
   
    d. hello hibaüzenetet **felhasználói B** kell jeleznie, hogy **a felhasználó** már  **User@contoso.com**  egy egyszerű, de látható **felhasználói B** saját displayName.

**Identitás szinkronizálási esetleges hibajelentésben való megjelenítéshez**:

hello hivatkozását *lépéseit tooresolve probléma* helytelen:  
    ![Aktív felhasználók](./media/active-directory-aadconnectsyncservice-duplicate-attribute-resiliency/6.png "aktív felhasználók")  

Túl kell mutatnia[https://aka.ms/duplicateattributeresiliency](https://aka.ms/duplicateattributeresiliency).

## <a name="see-also"></a>Lásd még:
* [Az Azure AD Connect szinkronizálása](active-directory-aadconnectsync-whatis.md)
* [Helyszíni identitások integrálása az Azure Active Directoryval](active-directory-aadconnect.md)
* [Az Office 365-ben a címtár szinkronizálási hibák](https://support.office.com/en-us/article/Identify-directory-synchronization-errors-in-Office-365-b4fc07a5-97ea-4ca6-9692-108acab74067)

