---
title: "Azure AD tartományi szolgáltatások: Hasonlítsa össze az Azure AD tartományi szolgáltatások tooDIY tartományvezérlők |} Microsoft Docs"
description: "Azure Active Directory tartományi szolgáltatások tooDIY tartományvezérlők összehasonlítása"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 165249d5-e0e7-4ed1-aa26-91a05a87bdc9
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/07/2017
ms.author: maheshu
ms.openlocfilehash: 5e384f6a676e76e4f22ff62d4aeb578bc8481ef7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodecide-if-azure-ad-domain-services-is-right-for-your-use-case"></a>Hogyan toodecide Ha Azure AD tartományi szolgáltatások nem megfelelő a a használati eset
Az Azure AD tartományi szolgáltatások telepítése anélkül, hogy az Azure-identitás-infrastruktúra karbantartása tooworry Azure infrastruktúra-szolgáltatásokat, a munkaterhelés. A felügyelt szolgáltatás eltér a szokásos Windows Server Active Directory központi telepítéséhez, telepítheti és felügyelheti a saját. hello szolgáltatás egyszerű toodeploy és automatizált állapotfigyelés és javítási. A Microsoft folyamatosan fejlődik hello szolgáltatás tooadd támogatása a gyakori telepítési forgatókönyvekben.

toodecide e toouse Azure AD tartományi szolgáltatások a következő anyagok hello javasoljuk:

* Hello tartalmazó lista [Azure AD tartományi szolgáltatások által kínált szolgáltatások](active-directory-ds-features.md).
* Felülvizsgálati közös [az Azure AD tartományi szolgáltatásokhoz központi telepítési forgatókönyvek](active-directory-ds-scenarios.md).
* Végezetül [hasonlítsa össze az Azure AD tartományi szolgáltatások tooa saját munka AD lehetőséget](active-directory-ds-comparison.md#compare-azure-ad-domain-services-to-diy-ad-domain-in-azure).

## <a name="compare-azure-ad-domain-services-toodiy-ad-domain-in-azure"></a>Hasonlítsa össze az Azure AD tartományi szolgáltatások tooDIY AD tartományhoz az Azure-ban
hello következő táblázat segítségével eldöntheti, hogy Azure AD tartományi szolgáltatások és a saját Azure AD-infrastruktúra kezelése között.

| **Funkció** | **Azure AD tartományi szolgáltatások** | **"Saját munka" AD az Azure virtuális gépeken** |
| --- |:---:|:---:|
| [**Felügyelt szolgáltatás**](active-directory-ds-comparison.md#managed-service) |**&amp;#x2713;;** |**& #x 2715;** |
| [**Biztonságos telepítéshez**](active-directory-ds-comparison.md#secure-deployments) |**&amp;#x2713;;** |A rendszergazdának toosecure hello központi telepítés. |
| [**DNS-kiszolgáló**](active-directory-ds-comparison.md#dns-server) |**& #x 2713;**  (a felügyelt) |**&amp;#x2713;;** |
| [**Tartományi vagy vállalati rendszergazdai jogosultságokkal**](active-directory-ds-comparison.md#domain-or-enterprise-administrator-privileges) |**& #x 2715;** |**&amp;#x2713;;** |
| [**Csatlakozás tartományhoz**](active-directory-ds-comparison.md#domain-join) |**&amp;#x2713;;** |**&amp;#x2713;;** |
| [**Tartomány hitelesítése NTLM és Kerberos használatával**](active-directory-ds-comparison.md#domain-authentication-using-ntlm-and-kerberos) |**&amp;#x2713;;** |**&amp;#x2713;;** |
| [**Kerberos által korlátozott delegálás**](active-directory-ds-comparison.md#kerberos-constrained-delegation)|erőforrás-alapú|erőforrás-alapú & ügyfélalapú|
| [**Egyéni Szervezetiegység-struktúrája**](active-directory-ds-comparison.md#custom-ou-structure) |**&amp;#x2713;;** |**&amp;#x2713;;** |
| [**Sémakiterjesztések**](active-directory-ds-comparison.md#schema-extensions) |**& #x 2715;** |**&amp;#x2713;;** |
| [**AD-tartomány vagy erdő megbízhatónak tekinti**](active-directory-ds-comparison.md#ad-domain-or-forest-trusts) |**& #x 2715;** |**&amp;#x2713;;** |
| [**LDAP olvasása**](active-directory-ds-comparison.md#ldap-read) |**&amp;#x2713;;** |**&amp;#x2713;;** |
| [**Biztonságos LDAP (LDAPS)**](active-directory-ds-comparison.md#secure-ldap) |**&amp;#x2713;;** |**&amp;#x2713;;** |
| [**LDAP írási**](active-directory-ds-comparison.md#ldap-write) |**& #x 2715;** |**&amp;#x2713;;** |
| [**Csoportházirend**](active-directory-ds-comparison.md#group-policy) |**&amp;#x2713;;** |**&amp;#x2713;;** |
| [**Földrajzilag elosztott központi telepítések**](active-directory-ds-comparison.md#geo-dispersed-deployments) |**& #x 2715;** |**&amp;#x2713;;** |

#### <a name="managed-service"></a>Felügyelt szolgáltatás
A Microsoft által felügyelt Azure AD tartományi szolgáltatások-tartományt. Nincs tooworry javítását, frissítések, figyelés, a biztonsági mentések, és a tartomány rendelkezésre állását biztosítja. A felügyeleti feladatok kínálják szolgáltatásként a Microsoft Azure által a felügyelt tartományok.

#### <a name="secure-deployments"></a>Biztonságos telepítéshez
hello által kezelt tartomány biztonságosan zárolva van a Microsoft biztonsági javaslatok AD telepítések szerint. Ezek a javaslatok hello AD termékcsapatának évtizedekben mérnöki és az azt támogató AD központi telepítések felhasználói felület vezethető vissza. Saját munka telepítések esetén szükséges tootake adott központi telepítési lépéseket toolock le/secure a központi telepítés.

#### <a name="dns-server"></a>DNS-kiszolgáló
Az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz felügyelt DNS-szolgáltatásokat tartalmazza. Hello "AAD DC rendszergazdák" csoportba kezelheti DNS hello által felügyelt tartományon. A csoport tagjai teljes DNS-felügyeleti jogosultságokkal jellegűek hello által felügyelt tartományokhoz. DNS-kezelés használatával végezheti el hello "DNS adminisztrációs konzol" hello távoli kiszolgáló felügyeleti eszközei (RSAT) csomagban található.
[További információ](active-directory-ds-admin-guide-administer-dns.md)

#### <a name="domain-or-enterprise-administrator-privileges"></a>Tartományi vagy vállalati rendszergazdai jogosultságokkal
Ezek a emelt szintű jogosultságok nem érhető el a az AAD-Directory tartományi szolgáltatások által felügyelt tartományokhoz. Ezek a emelt szintű jogosultságok nem telepíthető az AAD-DS-en igénylő alkalmazások felügyelt tartományok. Rendszergazdai jogosultságokkal kisebb részhalmazát elérhető toomembers hello delegált felügyeleti csoport "AAD DC rendszergazdák" nevezik. Ezek a jogosultságok jogosultságokkal tooconfigure DNS tartalmazzák, csoportházirend konfigurálásához, kapnak rendszergazdai jogosultságokkal a tartományhoz csatlakozó gépek stb.

#### <a name="domain-join"></a>Csatlakozás tartományhoz
Csatlakozhat a virtuális gépek toohello felügyelt tartomány hasonló toohow számítógépek tooan AD-tartományhoz csatlakozik.

#### <a name="domain-authentication-using-ntlm-and-kerberos"></a>Tartomány hitelesítése NTLM és Kerberos használatával
Az Azure AD tartományi szolgáltatásokkal használhatja a vállalati hitelesítő adatok tooauthenticate hello felügyelt tartomány. Hitelesítő adatok szinkronban vannak az Azure AD-bérlő tartanak. Szinkronizált bérlők számára az Azure AD Connect biztosítja, hogy módosításokat végzett toocredentials helyszíni szinkronizált tooAzure AD. A saját munka Tartománykonfiguráció, szükség lehet a tooset be az AD-tartomány megbízhatósági kapcsolatban áll a helyszíni AD-hez a felhasználók tooauthenticate a vállalati hitelesítő adataikkal. Alternatív megoldásként szükség lehet, hogy a felhasználói jelszavakat szinkronizálja tooyour Azure tartományvezérlő virtuális gépeket AD replikációs tooensure mentése tooset.

#### <a name="kerberos-constrained-delegation"></a>Kerberos által korlátozott delegálás
Nincs "tartományi rendszergazda" jogosultsággal az Active Directory tartományi szolgáltatások által felügyelt tartományokhoz. Fiók-alapú (hagyományos) Kerberos által korlátozott delegálást, ezért nem konfigurálható. Azonban konfigurálhatja a biztonságosabb hello erőforrás-alapú korlátozott delegálás.
[További információ](active-directory-ds-enable-kcd.md)

#### <a name="custom-ou-structure"></a>Egyéni Szervezetiegység-struktúrája
Hello "AAD DC rendszergazdák" csoportba hozhat létre egyéni szervezeti egységek hello által felügyelt tartományon belül. Felhasználók, akik egyéni szervezeti egységek létrehozása teljes körű rendszergazdai jogosultságokat kapnak hello OU keresztül.
[További információ](active-directory-ds-admin-guide-create-ou.md)

#### <a name="schema-extensions"></a>Sémabővítmények
Az Azure AD tartományi szolgáltatások által felügyelt tartományokhoz hello alap sémája nem bővíthető. Ezért a bővítmények tooAD séma (például új attribútumot hello felhasználói objektum) használó alkalmazások nem vonható vissza, és tooAAD-DS tartományok vette.

#### <a name="ad-domain-or-forest-trusts"></a>AD-tartomány vagy erdőszintű megbízhatóság
Felügyelt tartományok konfigurált tooset más tartományokkal (bejövő vagy kimenő) megbízhatósági kapcsolatokat nem lehet. Ezért erőforrás erdő telepítési forgatókönyvek Azure AD tartományi szolgáltatások nem használhatók. Hasonlóképpen központi telepítések, ahol inkább nem toosynchronize jelszavak tooAzure AD Azure AD tartományi szolgáltatások nem használhatók.

#### <a name="ldap-read"></a>LDAP olvasása
hello felügyelt tartomány támogatja LDAP olvasási munkaterhelések. Ezért olvasási műveletek hello felügyelt tartomány alapján LDAP végző alkalmazásokat helyezhet üzembe.

#### <a name="secure-ldap"></a>Biztonságos LDAP
Konfigurálhatja az Azure AD tartományi szolgáltatások tooprovide biztonságos LDAP hozzáférés tooyour felügyelt tartomány, beleértve a keresztül hello internet.
[További információ](active-directory-ds-admin-guide-configure-secure-ldap.md)

#### <a name="ldap-write"></a>LDAP írási
hello kezelt tartományi felhasználói objektumok írásvédett. Hello felhasználói objektum attribútumokat LDAP írási műveleteket végző alkalmazások, ezért nem működnek a felügyelt tartományhoz. Emellett felhasználói jelszavakat nem lehet módosítani a hello felügyelt tartományon belül. Egy másik példa lehet módosítása csoporttagságok vagy csoport attribútumok hello felügyelt tartományon belül, amely nem engedélyezett. Azonban toouser attribútumot, vagy az Azure ad-ben (PowerShell vagy az Azure portálon) végzett jelszavak módosítása vagy a helyszíni AD szinkronizálja a rendszer az AAD-DS toohello felügyelt tartomány.

#### <a name="group-policy"></a>Csoportházirend
Nincs beépített csoportházirend-objektum minden egyes az hello "AADDC Computers" és "AADDC felhasználók" tárolókat. Testre szabhatja a beépített csoportházirend-objektumok tooconfigure csoportházirend. Hello "AAD DC" rendszergazdákra is egyéni csoportházirend-objektumok létrehozása, és csatolja őket tooexisting szervezeti egységek (beleértve az egyéni ou-k).
[További információ](active-directory-ds-admin-guide-administer-group-policy.md)

#### <a name="geo-dispersed-deployments"></a>Földrajzi megvédheti központi telepítések
Az Azure AD tartományi szolgáltatások felügyelt tartományok érhetők el az Azure-ban egy virtuális hálózaton. Tartományi vezérlők toobe érhető el a több Azure-régiók közötti hello world igénylő forgatókönyvek esetén az Azure IaaS virtuális gépeken tartományvezérlők beállításával lehet hello jobb megoldás.


## <a name="do-it-yourself-diy-ad-deployment-options"></a>"Saját munka" (DIY) AD üzembe helyezési lehetőségei
Előfordulhat, hogy telepítési használati esetek kell bizonyos hello képességeket kínál a Windows Server AD-telepítés. Ezekben az esetekben fontolja meg az alábbi saját munka (DIY) beállítások hello egyikét:

* **Önálló felhőalapú tartományt:** "felhőalapú tartományt" használatával az Azure virtuális gépek tartományvezérlőként konfigurált önálló állíthat be. Ez az infrastruktúra nem integrálható a helyszíni AD-környezetben. Ez a beállítás a "hitelesítő adatok felhőbeli" toologin/felügyelik hello felhőben eltérő szabályzatkészletet lenne.
* **Erőforrások erdő telepítése:** állíthat be egy tartományba hello erőforrás szolgának, használja az Azure virtuális gépek konfigurálásra tartományvezérlőként. Ezután konfigurálja a az AD megbízhatósági kapcsolat a helyszíni AD-környezetben. Tartományhoz való csatlakozást (Azure virtuális gépeken) számítógépek toothis Erőforráserdő hello felhőben is. Felhasználói hitelesítés vagy keresztül történik egy VPN vagy ExpressRoute kapcsolat tooyour helyszíni címtár.
* **Kiterjeszti a helyszíni tartományi tooAzure:** csatlakoztathatja egy Azure-beli virtuális hálózat tooyour a helyszíni hálózat VPN vagy ExpressRoute kapcsolattal. Ez a beállítás lehetővé teszi, hogy az Azure virtuális gépek toobe illesztett tooyour a helyszíni AD. Egy másik lehetőség toopromote replika tartományvezérlőket az Azure-ban a helyi tartomány, mint egy virtuális Gépet. Ezután beállíthat azt tooreplicate keresztül egy VPN vagy ExpressRoute kapcsolat tooyour helyszíni címtárba. Ebben a telepítési módban hatékonyan kiterjeszti a helyszíni tartományi tooAzure.

> [!NOTE]
> Előfordulhat, hogy határozhatja meg, hogy a központi telepítés használati esetek jobban megfelelő DIY lehetőséget. Fontolja meg [visszajelzés megosztása](active-directory-ds-contact-us.md) velünk megértéséhez szolgáltatások segítene toohelp Azure AD tartományi szolgáltatások későbbi hello lehetőséget választotta. Ezzel a visszajelzéssel segít nekünk hello szolgáltatás toobetter színből kell a központi telepítési és használati esetek fejleszteni.
>
>

A Microsoft közzétett [telepítése Windows Server Active Directory telepítése Azure virtuális gépekre vonatkozó irányelvek](https://msdn.microsoft.com/library/azure/jj156090.aspx) toohelp könnyebben DIY telepítések.

## <a name="related-content"></a>Kapcsolódó tartalom
* [Szolgáltatások – Azure AD tartományi szolgáltatások](active-directory-ds-features.md)
* [Központi telepítési forgatókönyvek - Azure AD tartományi szolgáltatások](active-directory-ds-scenarios.md)
* [Windows Server Active Directory az Azure virtuális gépeken telepítési útmutatója](https://msdn.microsoft.com/library/azure/jj156090.aspx)
