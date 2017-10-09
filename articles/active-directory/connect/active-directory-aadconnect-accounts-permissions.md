---
title: "Az Azure AD Connect: Fiókok és engedélyek |} Microsoft Docs"
description: "Ez a témakör ismerteti a használt, és létre hello fiókok és a szükséges engedélyekkel."
services: active-directory
documentationcenter: 
author: billmath
manager: femila
editor: 
ms.reviewer: cychua
ms.assetid: b93e595b-354a-479d-85ec-a95553dd9cc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/27/2017
ms.author: billmath
ms.openlocfilehash: 70a7013e0353d74714ec8a3ff54b3e811789a0b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-ad-connect-accounts-and-permissions"></a>Az Azure AD Connect: Fiókok és engedélyek
hello Azure AD Connect telepítővarázsló biztosít két különböző elérési utak:

* A Gyorsbeállítások használatához hello varázsló több jogosultságra van szüksége.  Ez az, hogy a konfiguráció könnyen, beállítása nélkül toocreate felhasználók vagy -engedélyek konfigurálása.
* Az egyéni beállítások hello varázsló biztosít további lehetőségek és beállítások. Vannak azonban olyan helyzetek, amelyben tooensure engedélye hello megfelelő saját magának kell.

## <a name="related-documentation"></a>Kapcsolódó dokumentáció
Ha a nem olvasta hello dokumentáció [a helyszíni identitások integrálása az Azure Active Directoryval](../active-directory-aadconnect.md), hello alábbi táblázatban hivatkozások toorelated témaköröket.

|Témakör |Hivatkozás|  
| --- | --- |
|Az Azure AD Connect letöltése | [Az Azure AD Connect letöltése](http://go.microsoft.com/fwlink/?LinkId=615771)|
|Telepítés gyorsbeállítások használatával | [Az Azure AD Connect gyorstelepítése](./active-directory-aadconnect-get-started-express.md)|
|Telepítés testreszabott beállítások használatával | [Az Azure AD Connect testreszabott telepítése](./active-directory-aadconnect-get-started-custom.md)|
|Frissítés a DirSync szolgáltatásról | [Frissítés az Azure AD szinkronizáló eszközéről (DirSync)](./active-directory-aadconnect-dirsync-upgrade-get-started.md)|
|A telepítést követően | [Hello telepítésének ellenőrzése és licencek hozzárendelése](active-directory-aadconnect-whats-next.md)|

## <a name="express-settings-installation"></a>Expressz telepítés beállításai
Az expressz beállításokat hello telepítővarázsló Active Directory tartományi szolgáltatások vállalati rendszergazdai hitelesítő adatokat kér.  Ez a helyzet a helyszíni Active Directory konfigurálható az Azure AD Connect szükséges engedélyekkel. Ha a Dirsync szolgáltatásról frissít, hello AD DS vállalati rendszergazdák hitelesítő adatok a DirSync által használt hello fiók használt tooreset hello jelszavát. Szükség az Azure AD globális rendszergazda hitelesítő adatait.

| Varázslólap | Hitelesítő adatok gyűjtése | Szükséges jogosultságok | A használt |
| --- | --- | --- | --- |
| N/A |Felhasználó futó hello telepítési varázsló |Hello helyi kiszolgáló rendszergazdája |<li>Hello használt hello helyi fiókot hoz létre [szinkronizálás a motor szolgáltatásfiók](#azure-ad-connect-sync-service-account). |
| TooAzure AD Connect |Az Azure Active directory hitelesítő adatok |Az Azure AD globális rendszergazdai szerepkörrel |<li>A hello Azure AD-címtár-szinkronizálás engedélyezése.</li>  <li>Hello létrehozása [Azure AD-fiókot](#azure-ad-service-account) lévő szinkronizálási műveletek használt Azure AD-ben.</li> |
| Csatlakozás tooAD DS |A helyszíni Active Directory hitelesítő adatok |Az Active Directory hello vállalati rendszergazdák (EA) csoport tagjai |<li>Létrehoz egy [fiók](#active-directory-account) az Active Directory és az engedélyek tooit biztosít. Ezt a fiókot hozta létre az használt tooread és írási directory információt szinkronizálás során.</li> |

### <a name="enterprise-admin-credentials"></a>Vállalati rendszergazdai hitelesítő adatokat
Ezek a hitelesítő adatok csak hello telepítés során használt, és ne lehessen felhasználni hello telepítés befejezése után. hello vállalati rendszergazdai, nem tartományi rendszergazda hello győződjön meg róla, az Active Directory hello engedélyeit a tartományokban található állítható be.

### <a name="global-admin-credentials"></a>Globális rendszergazda hitelesítő adataival
Ezek a hitelesítő adatok csak hello telepítés során használt, és ne lehessen felhasználni hello telepítés befejezése után. Használt toocreate hello [Azure AD-fiókot](#azure-ad-service-account) módosítások tooAzure AD szinkronizálásához használt. hello fiók is lehetővé teszi, hogy szinkronizálás szolgáltatásként az Azure ad-ben.

### <a name="permissions-for-hello-created-ad-ds-account-for-express-settings"></a>Hello engedélyeinek létrehozott gyorsbeállítások AD DS-fiókjához
Hello [fiók](#active-directory-account) olvasására vagy írására tooAD DS alábbi gyorsbeállítások létrehozásakor engedélyek hello kell létrehozni:

| Engedély | A használt |
| --- | --- |
| <li>Változások replikálása</li><li>Replikálása Directory összes módosítása |A jelszó-szinkronizálás |
| Olvasási/írási összes tulajdonság felhasználó |Importálás és az Exchange hibrid |
| Olvasási/írási összes tulajdonságok iNetOrgPerson |Importálás és az Exchange hibrid |
| Az összes tulajdonság csoport olvasási/írási |Importálás és az Exchange hibrid |
| Az összes tulajdonság forduljon olvasási/írási |Importálás és az Exchange hibrid |
| Új jelszó létrehozása |Felkészülés a jelszóvisszaírás engedélyezése |

## <a name="custom-settings-installation"></a>Egyéni beállítások telepítése
Az Azure AD Connect 1.1.524.0 verziót, és később hello beállítás toolet hello Azure AD Connect varázsló hello használt fiók tooconnect tooActive könyvtár létrehozása.  A korábbi szükséges, hogy létrejött-e hello fiók hello telepítés előtt. hello engedélyeket kell adnia ezt a fiókot található [hello AD DS-fiók létrehozása](#create-the-ad-ds-account). 

| Varázslólap | Hitelesítő adatok gyűjtése | Szükséges jogosultságok | A használt |
| --- | --- | --- | --- |
| N/A |Felhasználó futó hello telepítési varázsló |<li>Hello helyi kiszolgáló rendszergazdája</li><li>Ha teljes SQL Server rendszert hello felhasználónak kell-e rendszergazdai (SA) SQL-ben</li> |Alapértelmezés szerint létrehoz hello helyi fiók, amely hello [szinkronizálás a motor szolgáltatásfiók](#azure-ad-connect-sync-service-account). hello fiók csak akkor jönnek létre, amikor Üdvözöljük a rendszergazdákat nem ad meg egy különös figyelmet. |
| Szinkronizálási szolgáltatások, a szolgáltatás fiók lehetőséget telepítése |AD vagy a helyi felhasználói fiók hitelesítő adatait |Felhasználó, engedélyekkel hello telepítő varázsló |Üdvözöljük a rendszergazdákat egy fiókot adja meg, ha ez a fiók használatos hello szolgáltatásfiókként hello szinkronizálási szolgáltatás. |
| TooAzure AD Connect |Az Azure Active directory hitelesítő adatok |Az Azure AD globális rendszergazdai szerepkörrel |<li>A hello Azure AD-címtár-szinkronizálás engedélyezése.</li>  <li>Hello létrehozása [Azure AD-fiókot](#azure-ad-service-account) lévő szinkronizálási műveletek használt Azure AD-ben.</li> |
| Csatlakoztassa a címtárakat |Az egyes erdőkhöz, amely csatlakoztatott tooAzure AD a helyi Active Directorybeli hitelesítő adatokat |hello engedélyek függ, mely a szolgáltatások engedélyezése, és itt található: [hello AD DS-fiók létrehozása](#create-the-ad-ds-account) |Ez a fiók akkor használt tooread és írási címtáradatok szinkronizálás során. |
| AD FS-kiszolgálók |Az egyes kiszolgálók hello listában a hello varázsló hitelesítő adatokat gyűjti, hello bejelentkezési hitelesítő adatok hello felhasználó hello varázsló futtatása esetén elegendő tooconnect |Tartományi rendszergazda |Telepítés és konfigurálás hello AD FS kiszolgálói szerepkör. |
| Webalkalmazás-proxy kiszolgálók |Az egyes kiszolgálók hello listában a hello varázsló hitelesítő adatokat gyűjti, hello bejelentkezési hitelesítő adatok hello felhasználó hello varázsló futtatása esetén elegendő tooconnect |Helyi rendszergazda hello célszámítógépen |Telepítés és konfigurálás a WAP-kiszolgálói szerepkör. |
| Proxy megbízhatósági hitelesítő adatai |Összevonási szolgáltatás bizalmi kapcsolat hitelesítő adatait (hello hitelesítő adatok hello proxy használ a tooenroll hello FS megbízható tanúsítványt |Tartományi fiókot, amely hello AD FS-kiszolgáló helyi rendszergazdája |A regisztráció FS-WAP megbízható tanúsítvány. |
| AD FS szolgáltatásfiókjának, "Tartományi felhasználói fiók lehetőség használata" |AD-felhasználói fiókjának hitelesítő adatait |Tartományi felhasználó |hello AD felhasználói fiók, amelynek hitelesítő adatok megadása az AD FS hello szolgáltatás hello bejelentkezési fiókként szolgál. |

### <a name="create-hello-ad-ds-account"></a>Active Directory tartományi szolgáltatások hello-fiók létrehozása
hello a megadott fióknak hello **csatlakoztassa a címtárakat** lap Active Directory korábbi tooinstallation jelen kell lennie.  Megadott hello szükséges engedélyeket is kell rendelkeznie. hello telepítővarázsló nem ellenőrzi a hello engedélyek és probléma merül fel csak találhatók meg a szinkronizálás során.

Engedélyezi szükséges jogosultságokat hello választható szolgáltatások függ. Ha több tartományban vannak, hello kell adhatók engedélyek hello erdőben lévő összes tartományban. Ha nem engedélyezi a szolgáltatások, hello alapértelmezett **tartományi felhasználó** elegendő.

| Szolgáltatás | Engedélyek |
| --- | --- |
| Az msDS-ConsistencyGuid szolgáltatás |Írási engedélyek toohello msDS-ConsistencyGuid attribútum részletes ismertetését lásd: [tervezési alapelvek - msDS-ConsistencyGuid használata sourceAnchor](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor). | 
| A jelszó-szinkronizálás |<li>Változások replikálása</li>  <li>Replikálása Directory összes módosítása |
| Hibrid Exchange-telepítés |Írás a dokumentált engedélyeket toohello attribútumok [Exchange hibrid visszaírási](active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) felhasználók, csoportok és ügyfelek. |
| Exchange E-mail nyilvános mappa |Olvasási engedélyek toohello attribútumok dokumentált [Exchange E-mail nyilvános mappa](active-directory-aadconnectsync-attributes-synchronized.md#exchange-mail-public-folder) nyilvános mappák. | 
| Jelszóvisszaíró |Írás a dokumentált engedélyeket toohello attribútumok [Ismerkedés a jelszókezeléssel](../active-directory-passwords-writeback.md) a felhasználók számára. |
| Eszközvisszaíró |A PowerShell-parancsfájllal megadott engedélyeket [eszközvisszaíró](active-directory-aadconnect-feature-device-writeback.md). |
| Group writeback (Csoportvisszaíró) |Olvassa el, létrehozási, frissítési és törlési objektumokat csoport a szinkronizált **Office 365-csoportok**.  További információ: [Csoportvisszaírásról](active-directory-aadconnect-feature-preview.md#group-writeback).|

## <a name="upgrade"></a>Frissítés
Az Azure AD Connect tooa új kiadási verziójához frissítésekor kell hello alábbi engedélyek használata:

| Egyszerű | Szükséges jogosultságok | A használt |
| --- | --- | --- |
| Felhasználó futó hello telepítési varázsló |Hello helyi kiszolgáló rendszergazdája |A frissítés bináris fájljait. |
| Felhasználó futó hello telepítési varázsló |ADSyncAdmins tagja |TooSync szabályok és egyéb konfigurációs módosítása. |
| Felhasználó futó hello telepítési varázsló |A teljes SQL server használata esetén: DBO (vagy hasonlót) hello szinkronizálási motor adatbázis |Adatbázis szintű módosításokat végezni, például táblák frissítése új oszlopokkal. |

## <a name="more-about-hello-created-accounts"></a>További információ az hello fiókok létrehozása
### <a name="active-directory-account"></a>Active Directory-fiókkal
Ha az expressz beállításokat használja, majd egy fiókot az Active Directoryban, amely a szinkronizáláshoz használt jön létre. hello létrehozott fiók hello erdő szintű gyökértartomány hello felhasználók tárolóban található, és rendelkezik a név előtagként **MSOL_**. hello fiók jön létre egy hosszú összetett jelszót, amely nem jár le. Ha a jelszóházirend abban a tartományban, győződjön meg arról, hogy hosszú, és bonyolultabb jelszavakat szeretné engedélyezni az ehhez a fiókhoz.

![AD-fiókot](./media/active-directory-aadconnect-accounts-permissions/adsyncserviceaccount.png)

Ha egyéni beállításokat használja, majd való telepítésért felelős hello fiók létrehozása hello telepítésének megkezdése előtt.

### <a name="azure-ad-connect-sync-service-account"></a>Az Azure AD Connect szinkronizáláshoz használt szolgáltatásfióknak
hello szinkronizálási szolgáltatás eltérő fiókkal is futhat. Csak akkor futtathatók a **virtuális szolgáltatásfiók** (Szállítóspecifikus), egy **csoportosan felügyelt szolgáltatásfiók** (csoportosan felügyelt szolgáltatásfiókot vagy önállóan felügyelt szolgáltatásfiókot), vagy egy felhasználói fiókot. hello támogatott beállítások módosított rendelkező hello Connect verzióját 2017. április Ha így tesz, friss telepítését. Ha az Azure AD Connect egy korábbi kiadásáról frissít, a további beállítások nem érhetők el.

| Fiók típusa | Telepítési lehetőség | Leírás |
| --- | --- | --- |
| [Virtuális fiók](#virtual-service-account) | Az expressz és egyéni, 2017. április és újabb verziók | Ez a lehetőség hello használt összes Expressz telepítés, kivéve a tartományvezérlővé való telepítéséhez. Egyéni célszerű hello alapértelmezett beállítást, hacsak egy másik kapcsolót. |
| [Csoportosan felügyelt szolgáltatásfiók](#group-managed-service-account) | Egyéni, 2017. április és újabb verziók | Ha távoli SQL server, akkor azt javasoljuk toouse csoport felügyelt szolgáltatásfiók. |
| [Felhasználói fiók](#user-account) | Az expressz és egyéni, 2017. április és újabb verziók | Egy felhasználói fiókot AAD_ előtagként csak telepített Windows Server 2008 és a tartományvezérlőre van telepítve, a telepítés során jön létre. |
| [Felhasználói fiók](#user-account) | Az expressz és egyéni, 2017. március és korábbi | AAD_ előtagként helyi fiók a telepítés során jön létre. Egyéni telepítés használata esetén egy másik fiókot is megadható. |

Ha a csatlakozás a 2017. március build használ, vagy korábbi, akkor nem kell alaphelyzetbe óta Windows hello szolgáltatás fiók jelszavának hello megsemmisít hello titkosítási kulcsok biztonsági okokból. Az Azure AD Connect újratelepítése nélkül hello fiók tooany más fiókja nem módosítható. Ha verzióról tooa build 2017. április vagy újabb, majd támogatott toochange hello hello szolgáltatási fiók jelszava de hello művelet végrehajtására használt fiók nem módosítható.

> [!Important]
> Hello szolgáltatás fiókja csak első telepítési adhatók meg. Nem támogatott toochange hello szolgáltatásfiók hello telepítés befejezése után.

Ez a táblázat hello alapértelmezés szerint ajánlott, a, és hello támogatott beállításai szinkronizálva szolgáltatásfiók.

Jelmagyarázat:

- **A félkövér betűvel írott** hello alapértelmezett beállítás azt jelzi, és a legtöbb esetben hello ajánlott beállítás.
- *Dőlt* jelzi hello javasolt beállítást, amikor nincs hello alapértelmezett beállítás.
- 2008 - alapértelmezett beállítás a Windows Server 2008 rendszeren
- A nem félkövér - támogatott beállítás
- Helyi fiók – helyi felhasználói fiók hello kiszolgálón
- Tartományi fiók - tartományi felhasználói fiók
- önállóan felügyelt szolgáltatásfiókot - [önálló felügyelt szolgáltatásfiók](https://technet.microsoft.com/library/dd548356.aspx)
- csoportosan felügyelt szolgáltatásfiók - [csoportos felügyelt szolgáltatásfiók](https://technet.microsoft.com/library/hh831782.aspx)

| | LocalDB</br>Express | LocalDB/LocalSQL</br>Egyéni | Távoli SQL</br>Egyéni |
| --- | --- | --- | --- |
| **önálló vagy munkacsoporthoz tartozó számítógépeken** | Nem támogatott | **SZÁLLÍTÓSPECIFIKUS**</br>Helyi fiók (2008)</br>Helyi fiók |  Nem támogatott |
| **a tartományhoz gép** | **SZÁLLÍTÓSPECIFIKUS**</br>Helyi fiók (2008) | **SZÁLLÍTÓSPECIFIKUS**</br>Helyi fiók (2008)</br>Helyi fiók</br>Tartományi fiók</br>önállóan felügyelt szolgáltatásfiókot, csoportosan felügyelt szolgáltatásfiók | **csoportosan felügyelt szolgáltatásfiók**</br>Tartományi fiók |
| **Tartományvezérlő** | **Tartományi fiók** | *csoportosan felügyelt szolgáltatásfiók*</br>**Tartományi fiók**</br>önállóan felügyelt szolgáltatásfiókot| *csoportosan felügyelt szolgáltatásfiók*</br>**Tartományi fiók**|

#### <a name="virtual-service-account"></a>Virtuális fiók
Virtuális szolgáltatásfiók egy olyan fiókkal, amely egy jelszó és a Windows által felügyelt különleges típusú.

![SZÁLLÍTÓSPECIFIKUS](./media/active-directory-aadconnect-accounts-permissions/aadsyncvsa.png)

Szállítóspecifikus hello a hello hello szinkronizálási motor és az SQL esetén a forgatókönyvekben használt tervezett toobe ugyanazon a kiszolgálón. Ha távoli SQL, akkor azt javasoljuk, hogy toouse egy [csoportosan felügyelt szolgáltatásfiók](#managed-service-account) helyette.

Ez a funkció használatához Windows Server 2008 R2 vagy újabb. Ha az Azure AD Connect telepítése Windows Server 2008, majd hello telepítési visszavált toousing egy [felhasználói fiók](#user-account) helyette.

#### <a name="group-managed-service-account"></a>Csoportosan felügyelt szolgáltatásfiók
Ha távoli SQL-kiszolgálón, akkor azt javasoljuk, hogy toousing egy **csoport felügyelt szolgáltatásfiók**. További információt a tooprepare az Active Directory csoport által felügyelt szolgáltatásfiók, lásd: [csoportosan felügyelt szolgáltatásfiókok áttekintése](https://technet.microsoft.com/library/hh831782.aspx).

Ezt a beállítást, a hello toouse [szükséges összetevők telepítése](active-directory-aadconnect-get-started-custom.md#install-required-components) lapon jelölje be **meglévő szolgáltatásfiók használata**, és válassza ki **felügyelt szolgáltatásfiók**.  
![SZÁLLÍTÓSPECIFIKUS](./media/active-directory-aadconnect-accounts-permissions/serviceaccount.png)  
Célszerű is támogatott toouse egy [önálló felügyelt szolgáltatásfiók](https://technet.microsoft.com/library/dd548356.aspx). Azonban ezek csak a helyi számítógépen hello használható, és nem juttatás toouse van hello alapértelmezett virtuális szolgáltatásfiók alatt őket.

A funkció használatához Windows Server 2012 vagy újabb. Ha egy régebbi operációs rendszer toouse kell, és használjon távoli SQL, akkor kell használnia egy [felhasználói fiók](#user-account).

#### <a name="user-account"></a>Felhasználói fiók
A helyi szolgáltatás fiók létrehozása hello telepítési varázslóval (kivéve, ha a fiók toouse hello meg a egyéni beállítások). hello fiók van hozzáfűzik **AAD_** és hello tényleges szinkronizálási szolgáltatás toorun, használatos. Ha az Azure AD Connect tartományvezérlőre telepíti, a hello fiók hello tartományban jön létre. Hello **AAD_** szolgáltatásfiók hello tartományban kell lennie, ha:
   - SQL server rendszert futtató távoli kiszolgáló használata
   - egy hitelesítést igénylő proxyt használ

![Szolgáltatásfiók](./media/active-directory-aadconnect-accounts-permissions/syncserviceaccount.png)

hello fiók jön létre egy hosszú összetett jelszót, amely nem jár le.

Ez a fiók biztonságos módon más fiókokat használt toostore hello jelszavait hello van. Ezek más fiókokat a jelszavak titkosított hello adatbázisban tárolja. hello titkos kulcsok hello titkosításikulcs hello titkosítási szolgáltatások titkos kulcsú titkosítás Windows Data Protection API (DPAPI) használatával védi.

Ha teljes SQL Server használatához hello szolgáltatásfiók hello hello szinkronizálási motor hello létrehozott adatbázis DBO. hello szolgáltatást nem fognak működni, mint bármely más engedélyekkel. Egy SQL-bejelentkezési is létrejön.

hello fiók engedélyek toofiles, beállításkulcsokat és más objektumok kapcsolódó toohello szinkronizálási motor is megkapja.

### <a name="azure-ad-service-account"></a>Azure AD-szolgáltatásfiók
Hello szinkronizálási szolgáltatás használata az Azure AD-fiók jön létre. Ez a fiók a megjelenített név alapján azonosítható.

![AD-fiókot](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccount.png)

hello hello szolgáltatásfiókjától hello nevét használja a második részében hello hello felhasználónév azonosítható. Hello kép hello kiszolgáló nevének megadása FABRIKAMCON. Ha még kiszolgáló átmeneti, minden kiszolgáló rendelkezik saját fiók.

hello szolgáltatásfiók létrejön egy hosszú összetett jelszót, amely nem jár le. Egy különös szerepet kap **szinkronizálási Címtárfiókjainak** , amelyhez csak a engedélyek tooperform directory szinkronizálási feladatok. Ez a különleges beépített szerepkör hello Azure AD Connect varázsló kívül nem adható meg. hello Azure-portálon jeleníti meg ezt a fiókot hello szerepkör **felhasználói**.

Nincs maximális hossza 20 szinkronizálási szolgáltatás fiókok Azure AD-ben. tooget hello meglévő az Azure AD szolgáltatás fiókok listáját az Azure AD-ben futtassa a következő Azure AD PowerShell-parancsmag hello:`Get-AzureADDirectoryRole | where {$_.DisplayName -eq "Directory Synchronization Accounts"} | Get-AzureADDirectoryRoleMember`

nem használt tooremove az Azure AD szolgáltatásfiókok, futtassa a következő Azure AD PowerShell-parancsmag hello:`Remove-AzureADUser -ObjectId <ObjectId-of-the-account-you-wish-to-remove>`

## <a name="next-steps"></a>Következő lépések
További információ: [Helyszíni identitások integrálása az Azure Active Directoryval](../active-directory-aadconnect.md).
