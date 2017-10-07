---
title: "aaaAzure Active Directory hibrid identitáskezelési elrendezésével kapcsolatos szempontok - data protection stratégia kidolgozása |} Microsoft Docs"
description: "Hibrid identitáskezelési megoldás toomeet hello üzleti igényeinek definiált hello data protection stratégia fogja definiálni."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: e76fd1f4-340a-492a-84d9-e05f3b7cc396
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 8fd7ab364a09de3b60293a4a1cbb6e0fa4a3295d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="define-data-protection-strategy-for-your-hybrid-identity-solution"></a>A hibrid identitáskezelési megoldás az adatok védelme stratégia meghatározása
Ebben a feladatban fogja definiálni, hello data protection stratégia hibrid identitáskezelési megoldás toomeet hello üzleti igényeinek, amelyet a megadott:

* [Adatvédelmi követelményeinek meghatározása](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md)
* [A Tartalomkezelés követelmények meghatározása](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md)
* [Hozzáférés-vezérlési követelményeinek meghatározása](active-directory-hybrid-identity-design-considerations-accesscontrol-requirements.md)
* [Incidensválasz-követelmények meghatározása](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md)

## <a name="define-data-protection-options"></a>Adatok védelmi beállítások meghatározása
Lett leírtak szerint [címtár-szinkronizálás követelményeinek meghatározása](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md), a Microsoft Azure AD képes szinkronizálni az Active Directory tartományi szolgáltatások (AD DS) a helyszínen található. Segítségével a szervezetek tooleverage az Azure AD tooverify felhasználó hitelesítő adatainak tooaccess vállalati erőforrások közben. Ezt megteheti a mindkét forgatókönyvet: rest a helyszíni és felhőben hello adatokat.  Az Azure AD hozzáférési toodata biztonságijogkivonat-szolgáltatás (STS) keresztül felhasználóhitelesítést igényel.

Amennyiben hitelesítése után hello egyszerű felhasználónév (UPN) olvasása az hello hitelesítésére szolgáló jogkivonat és hello replikált partíció, a tároló megfelelő toohello felhasználó tartományát határozza meg. Hello felhasználói fenntartása, az engedélyezett állapotát és a szerepkör hello engedélyezési rendszer toodetermine használja e hello szükséges hozzáférést toohello cél bérlő jogosult felhasználó ebben a munkamenetben. Bizonyos engedélyezett műveletek (pontosabban, hozzon létre felhasználói jelszó-visszaállítás) létrehozása a bérlő által használt elővizsgálati rendszergazda toomanage megfelelőségi erőfeszítéseket vagy vizsgálatokat.

A helyszíni adatközpontját a Azure-tárolóba internetkapcsolaton keresztül áthelyezése adatait előfordulhat, hogy nem mindig valósítható toodata kötetet, sávszélesség vagy egyéb szempontok miatt. Hello [Azure Storage Import/Export szolgáltatás](../storage/common/storage-import-export-service.md) egy hardveralapú lehetőséget biztosít számára adatokat a blob Storage tárolóban nagy mennyiségű elhelyezéséhez/beolvasása. Ez lehetővé teszi toosend [BitLocker-titkosítású](https://technet.microsoft.com/library/dn306081#BKMK_BL2012R2) merevlemez-meghajtók közvetlen tooan Azure datacenter, ahol a felhő üzemeltetői fel kell töltenie hello tartalma tooyour tárfiók vagy letölthetik a az Azure data tooyour meghajtók tooreturn tooyou. Ez a folyamat (kulccsal a BitLocker által hello szolgáltatást hello feladat beállítása során létrehozott) csak a titkosított lemezek elfogadás. hello BitLocker kulcsra azért van tooAzure külön-külön, így biztosítva a kulcs megosztása sávon kívül.

Mivel adatokat átvitel közben különböző helyzetekben végrehajthatók, olyan is megfelelő tooknow Microsoft Azure használó [virtuális hálózat](https://azure.microsoft.com/documentation/services/virtual-network/) tooisolate bérlők forgalom egymástól, például a gazdagép és vendég szintű mértékek alkalmazó a tűzfalakra, IP-csomagok szűrését, blokkolja a port és HTTPS-végpontnak. Azonban a legtöbb Azure belső kommunikációs, beleértve az infrastruktúra-infrastruktúra és az infrastruktúra-ügyfelek (helyszíni) is titkosítást kapnak. Egy másik fontossággal bír az Azure adatközpontjaiban; hello kommunikációja A Microsoft kezeli, amely nem a virtuális gép megszemélyesíteni vagy kihallgassa hello IP-cím egy másik a hálózatok tooassure. A TLS/SSL Azure Storage vagy SQL-adatbázis elérésekor, vagy tooCloud szolgáltatások kapcsolódáskor használatos. Ebben az esetben hello felhasználói rendszergazda feladata egy TLS/SSL-tanúsítvány beszerzése és tootheir bérlői infrastruktúra telepítését. Helyezze át a virtuális gépek közötti adatforgalom az azonos környezetben hello, vagy egy központi telepítésnél, Microsoft Azure virtuális hálózaton keresztül a bérlők közötti védelme például HTTPS, az SSL/TLS vagy mások titkosított kommunikáció protokollokon keresztül.

Attól függően, hogy hogyan válaszolt kérdéseit hello [adatvédelmi követelményeinek meghatározása](active-directory-hybrid-identity-design-considerations-dataprotection-requirements.md), meg kell tudni toodetermine, hogyan szeretné tooprotect az adatok, és hogyan hello hibrid identitáskezelési megoldás segítséget nyújt a. a táblázat hello hello támogatott az Azure-ban rendelkezésre álló lehetőségek az egyes adatok védelmi forgatókönyvek esetén.

| Adatok védelmi beállítások | Hello felhő aktívan | A többi helyszíni | Az átvitel során |
| --- | --- | --- | --- |
| A BitLocker titkosítást |X |X | |
| SQL Server-adatbázisok tooencrypt |X |X | |
| VM-VM-titkosítás | | |X |
| SSL/TLS | | |X |
| VPN | | |X |

> [!NOTE]
> Olvasási [szolgáltatás megfelelőségi](https://azure.microsoft.com/support/trust-center/services/) : [Microsoft Azure Trust Center](https://azure.microsoft.com/support/trust-center/) tooknow, amely minden Azure szolgáltatás kompatibilis hello minősítései közül többet.
> A data protection hello-beállítások egy többrétegű módszert használja, mivel ezek a lehetőségek összehasonlítása nem vonatkoznak a feladathoz. Győződjön meg arról, hogy minden egyes állapot esetében hello adatokat kell a rendelkezésre álló lehetőségek hasznosítja ki.
>
>

## <a name="define-content-management-options"></a>A Tartalomkezelés beállítások megadása
Egy Azure AD toomanage egy hibrid identitás-infrastruktúra használatával előnye, hogy hello folyamat teljesen átlátszó hello végfelhasználó szempontjából. hello felhasználó megpróbál tooaccess egy megosztott erőforráson, hello erőforrás-hitelesítés szükséges, hello felhasználónak kell toosend hitelesítési kérelem tooAzure AD rendelés tooobtain hello token és hello erőforrás elérésére. A teljes folyamat a háttérben, felhasználói beavatkozás nélkül történik. Az is lehetséges toogrant engedély tooa [csoport](active-directory-manage-groups.md#getting-started-with-access-management) tooallow ahhoz a felhasználói őket tooperform bizonyos általános műveletek.

Kapcsolatos adatvédelmi probléma, amely általában azok a szervezetek, a megoldás az adatok besorolásával szükséges. A jelenlegi helyszíni infrastruktúrával már használja az adatok besorolásával, esetén lehetséges tooleverage az Azure AD hello fő tárház a felhasználó identitása szerint. Egy közös eszközt, hogy a rendszer használt a helyszíni az adatok besorolását nevezik [adatbesorolási eszközkészlet](https://msdn.microsoft.com/library/Hh204743.aspx) a Windows Server 2012 R2 rendszerben. Ez az eszköz tooidentify súgó, besorolásában és a fájlkiszolgálók a magánfelhőben található adatainak védelméhez. Az is lehetséges tooleverage hello [automatikus Fájlbesorolás](https://technet.microsoft.com/library/hh831672.aspx) a Windows Server 2012 tooaccomplish ez.

Ha a szervezet nem rendelkezik adatbesorolási, de tooprotect bizalmas fájlokat kell új kiszolgálók helyszíni hozzáadása nélkül, akkor a Microsoft [Azure Rights Management szolgáltatás](https://technet.microsoft.com/library/JJ585026.aspx).  Az Azure RMS titkosítási, identitáskezelési és engedélyezési házirendek toohelp biztonságos használ, a fájlok és e-mailt, és több eszközön is használható – telefonokon, táblagépeken és számítógépeken. Mivel az Azure RMS egy felhőszolgáltatás, nincs szükség tooexplicitly bizalmi kapcsolatok más szervezetek konfigurálása előtt védett tartalmakat megoszthatja velük. Ha már rendelkeznek Office 365- vagy Azure AD-címtárhoz, a szervezetek közötti együttműködés automatikusan támogatott. Most, hogy Azure RMS toosupport közös identitás számára szükséges a helyszíni Active Directory-fiókok Azure Active Directory szinkronizálási szolgáltatások (AAD Sync) vagy az Azure AD Connect használatával hello könyvtárattribútumokat is szinkronizálhatja.

Fontos része a Tartalomkezelés ki fér hozzá erőforrás-toounderstand, ezért a részletes naplózási képesség fontos hello identitáskezelési megoldás. Az Azure AD biztosít napló több mint 30 napja, beleértve:

* Szerepkör tagság változása (pl.: felhasználói tooGlobal rendszergazdai szerepkörhöz hozzá)
* Hitelesítőadat-frissítések (pl.: jelszó-változtatásának)
* Tartományok (pl.: egy egyéni tartományt, a tartomány eltávolításakor ellenőrzése)
* Hozzáadásával vagy eltávolításával alkalmazások
* Felhasználók kezelése (pl.: hozzáadását, eltávolítását, a felhasználó frissítése)
* Hozzáadásával vagy eltávolításával licencek

> [!NOTE]
> Olvasási [Microsoft Azure biztonsági és naplózási napló felügyeleti](http://download.microsoft.com/download/B/6/C/B6C0A98B-D34A-417C-826E-3EA28CDFC9DD/AzureSecurityandAuditLogManagement_11132014.pdf) tooknow további információk az Azure-ban naplózási képességeket.
> Attól függően, hogy hogyan válaszolt kérdéseit hello [tartalomkezelési követelmények meghatározása](active-directory-hybrid-identity-design-considerations-contentmgt-requirements.md), hogyan szeretné kezelni a hibrid identitáskezelési megoldás tartalom toobe hello képes toodetermine kell lennie. A 6 elérhetővé tett összes beállítás képesek az Azure AD integrálása, de napjainkban fontos toodefine, amely az üzleti igényeinek jobban megfelelő.
>
>

| Tartalomkezelési lehetőségeket | Előnyei | Hátrányok |
| --- | --- | --- |
| Helyszínen központosított (Active Directory tartalomvédelmi szolgáltatások) |Hello kiszolgálói infrastruktúra hello adatok zárolásának felelős teljes hozzáféréssel <br> A Windows Server, nincs szükség további licencek vagy előfizetések beépített kapacitásprofilokban <br> Integrálható az Azure AD-t egy hibrid forgatókönyvek <br> Információk rights management (IRM) képességeket támogatja a Microsoft Online szolgáltatásaiban, például az Exchange Online és SharePoint online-hoz, valamint Office 365 <br> Támogatja a helyszíni Microsoft kiszolgálói termékeket, például Exchange Server, SharePoint Server és Windows Server és a fájlbesorolási infrastruktúra (FCI) futtató fájlkiszolgálókat. |Magasabb, karbantartási (tartsa be a frissítéseket, a konfiguráció és a potenciális frissítéseinek), mert informatikai hello kiszolgáló tulajdonosa <br> Helyszíni kiszolgálói infrastruktúrát igényel<br> Doesn'tleverage Azure-képességek natív módon |
| (Azure RMS) hello felhőben központosított |Egyszerűbb képest toomanage toohello helyszíni megoldás <br> Integrálható az Active Directory tartományi szolgáltatások a hibrid forgatókönyvek <br>  Teljesen integrálva van az Azure AD <br> A kiszolgáló nem igényel helyi rendelés toodeploy hello szolgáltatásban <br> Támogatja a helyszíni Microsoft kiszolgálói termékeket, például Exchange Server, a SharePoint, a kiszolgáló és a Windows Server és a Fájlbesorolás, infrastruktúra (FCI) futtató fájlkiszolgálókat <br> Informatikai, teljes felügyeletet gyakorolhat a bérlői kulcs BYOK alkalmas lehet. |A szervezetnek rendelkeznie kell egy, az RMS-t támogató felhőalapú előfizetéssel <br> A szervezetnek rendelkeznie kell egy Azure Active directory toosupport felhasználói hitelesítés az RMS-hez |
| Hibrid (Azure RMS integrálva van, a helyszíni Active Directory tartalomvédelmi szolgáltatások) |Ebben a forgatókönyvben hello előnyeit, központosított a helyszíni és felhőben hello összegzi. |A szervezetnek rendelkeznie kell egy, az RMS-t támogató felhőalapú előfizetéssel <br> A szervezetnek rendelkeznie kell egy Azure Active directory toosupport felhasználói hitelesítés RMS-hez készült <br> Azure-felhőszolgáltatás közötti kapcsolatot igényel, és a helyszíni infrastruktúra |

## <a name="define-access-control-options"></a>Adja meg a hozzáférés-vezérlési lehetőségek
Ami hello hitelesítési, engedélyezési és hozzáférés szabályozása képes tooenable fogja az Azure AD-ben elérhető lehetőségek a vállalat toouse egy központi identitás-tárházat miközben lehetővé teszi a felhasználók és a partnerek toouse egyszeri bejelentkezés (SSO) látható módon hello az alábbi ábra:

![](./media/hybrid-id-design-considerations/centralized-management.png)

Központosított kezelését, és teljesen más címtárak integrációját

Az Azure Active Directory biztosít az egyszeri bejelentkezés toothousands Szolgáltatottszoftver-alkalmazáshoz, és a helyszíni webalkalmazások. Olvassa el a hello [Azure Active Directory összevonási kompatibilitási lista: harmadik fél Identitásszolgáltatók, amelyek lehetnek használt tooimplement egyszeri bejelentkezés](https://msdn.microsoft.com/library/azure/jj679342.aspx) cikk hello SSO külső által ellenőrzött kapcsolatos további részletekért Microsoft. Ez a funkció lehetővé teszi a szervezet tooimplement különféle forgatókönyvekhez, amik B2B hello identitások és hozzáférések felügyeletéhez irányítását megtartásával. Azonban hello B2B folyamat tervezése során a fontos toounderstand hello hitelesítési módszer hello partner által használható, és ellenőrizze, hogy ez a metódus Azure által támogatott. Jelenleg az Azure AD által támogatott módszerek a következők:

* Security Assertion Markup Language (SAML)
* OAuth
* Kerberos
* Tokenek
* Tanúsítványok

> [!NOTE]
> olvassa el [Azure Active Directory hitelesítési protokolljai](https://msdn.microsoft.com/library/azure/dn151124.aspx) tooknow további részleteket a minden egyes protokoll és annak képességei az Azure-ban.
>
>

Az Azure AD hello támogatásával, mobil üzleti alkalmazások hello azonos könnyen Mobile Services hitelesítési élmény tooallow alkalmazottak toosign azokat az alkalmazásokat a vállalati Active Directory hitelesítő adataikkal. Ez a szolgáltatás az Azure AD identitás-szolgáltatóként a Mobile Servicesben mellett a hello támogatott más identitásszolgáltatók már támogatja, (amelyek között a Microsoft Accounts, a Facebook-azonosító, a Google-azonosító és a Twitter azonosítója). Ha hello a helyszíni alkalmazások által használt hello hello vállalati Active Directory tartományi szolgáltatások helyen felhasználó hitelesítő adatait, hello a hozzáférést a partnerek és hello felhőből érkező felhasználók átlátszó kell lennie. Felhasználó feltételes hozzáférést vezérlő too(cloud-based) webes alkalmazásokhoz, webes API, a Microsoft cloud services csomag, 3. fél SaaS-alkalmazásokhoz és natív (mobil) ügyfél-alkalmazások kezelése, és rendelkezik a hello előnyei biztonsági, naplózási, mind az egyik reporting Helyezze el. Azonban ajánlott toovalidate ez nem éles környezetben, vagy a felhasználók csak korlátozott mennyiségű.

> [!TIP]
> Fontos, hogy az Azure AD nincs csoportházirend, ha az AD DS toomention. A sorrend tooenforce az eszközökre vonatkozó szabályzatot kell a mobileszköz-kezelési megoldás például [a Microsoft Intune](https://technet.microsoft.com/library/jj676587.aspx).
>
>

Hello felhasználó hitelesítése az Azure AD, ha fontos tooevaluate hello hozzáférési szintet, amelyet felhasználói hello kell azt. hello hozzáférési szintet, amelyet a felhasználó hello lesz erőforrás eltérőek lehetnek, az Azure AD egy további biztonsági réteget adhat ellenőrző toosome erőforrások eléréséhez, amíg kell még figyelembe venni, hogy hello erőforrás maga is beállíthatja, hogy a saját hozzáférés-vezérlési lista külön-külön, például egy fájlkiszolgálón lévő fájlok hello hozzáférés-vezérlést. az alábbi ábra hello hello szintű hozzáférés-vezérlés hibrid forgatókönyvek lehet foglalja össze:

![](./media/hybrid-id-design-considerations/accesscontrol.png)

Minden interakció. ábra x. bemutatta hello ábrán egy esetén elegendő az Azure AD hozzáférési vezérlési forgatókönyv jelöli. Az alábbiakban egy leírást az egyes forgatókönyvek közül választhat:

1. Feltételes hozzáférés tooapplications, amelyek helyben tárolt: használhatja a regisztrált eszközök hozzáférési házirendekkel kapcsolatos alkalmazások, amelyek toouse AD FS a Windows Server 2012 R2. A helyszíni feltételes hozzáférés beállításáról további információért lásd: [Helyszíni feltételes hozzáférés beállítása az Azure Active Directory eszközregisztrációjával](active-directory-conditional-access.md).

2. Hozzáférés-vezérlés toohello Azure-portálon: Azure emellett lehetővé teszi a vezérlő hozzáférés toohello portál szerepköralapú hozzáférés-vezérlést (RBAC) használatával). Ez a módszer lehetővé teszi, hogy a hello vállalati toorestrict hello mennyisége műveleteket, amelyek egy adott a hello Azure portálon teheti meg. Az RBAC toocontrol hozzáférés toohello portálon keresztül, IT-rendszergazdák a következő hozzáférés-felügyeleti módszerek hello segítségével is adhat hozzáférést:

* Csoportalapú szerepkör-hozzárendelést: hozzáférés tooAzure AD csoportokat rendelhet, hogy tud-e szinkronizálva a helyi Active Directoryból. Ez lehetővé teszi tooleverage hello használó megoldásokkal, amelyek a szervezet által végrehajtott tooling és-folyamatok csoportok kezelése. Használhatja az Azure AD Premium hello delegált csoport felügyeleti szolgáltatása.
* Használja ki a beépített szerepkörök az Azure-ban: használhatja három szerepkörök – tulajdonos, közreműködő, és ahhoz való olvasóra, tooensure, felhasználók és csoportok rendelkezik-e engedéllyel toodo csak hello feladatok toodo a feladatokat a van szükségük.
* A részletes hozzáférés tooresources: toousers szerepkörök és csoportok adott előfizetés, erőforráscsoport vagy egy egyéni Azure-erőforrás például egy webhely vagy az adatbázis rendelhet. Így biztosítható, hogy a felhasználók rendelkeznek tooall hello erőforrások eléréséhez szükséges és nem hozzáférés tooresources, hogy nincs szükségük toomanage.

> [!NOTE]
> Ön által létrehozott alkalmazások és toocustomize hello hozzáférés-vezérlés a számukra, esetén is lehetséges toouse az Azure AD alkalmazás-szerepkörök a hitelesítéshez. Tekintse át a [WebApp-RoleClaims-DotNet példa](https://github.com/AzureADSamples/WebApp-RoleClaims-DotNet) hogyan toobuild az alkalmazás toouse ezt a képességet.
>
>

3. Feltételes hozzáférés az Office 365-alkalmazások Microsoft Intune-nal: IT-rendszergazdák feltételes hozzáférési házirendek toosecure vállalati eszközerőforrások, építhető ki közben: hello ugyanaz az előírásoknak megfelelő eszközök tooaccess hello engedélyezése az információkkal dolgozó szakemberek idő szolgáltatások. További információ: [Feltételes hozzáférés eszközházirendjei Office 365-szolgáltatásokhoz](active-directory-conditional-access-device-policies.md).

4. Feltételes hozzáférés a Szolgáltatottszoftver-alkalmazásoknál: [Ez a szolgáltatás](http://blogs.technet.com/b/ad/archive/2015/06/25/azure-ad-conditional-access-preview-update-more-apps-and-blocking-access-for-users-not-at-work.aspx) lehetővé teszi az tooconfigure a multi-factor authentication alkalmazás hozzáférési szabályok és hello képességét tooblock hozzáférést a felhasználók számára nem megbízható hálózaton. Hello a multi-factor authentication szabályok tooall felhasználók vannak hozzárendelve toohello alkalmazás, vagy csak a felhasználók belül meghatározott biztonsági csoportok is alkalmazhatja. Felhasználók hello multi-factor authentication követelményeinek kizárhatók, ha érik el hello alkalmazás egy IP-címről, hogy a belső vállalati hálózaton hello.

Hozzáférés-vezérlési hello-beállítások egy többrétegű módszert használja, mivel ezek a lehetőségek összehasonlítása nem vonatkoznak a feladathoz. Győződjön meg arról, hogy minden beállítás érhető el az egyes forgatókönyvek esetében toocontrol tooyour-erőforrások eléréséhez szükséges hasznosítja ki.

## <a name="define-incident-response-options"></a>Incidensválasz-beállítások megadása
Az Azure AD segít a potenciális biztonsági kockázatok hello környezetben felhasználói tevékenységet, IT-csoport figyelésével kihasználhatja az Azure AD hozzáférési és használati jelentések funkció toogain láthatósága hello adatintegritási és biztonsági szervezete címtárát az informatikai tooidentity. Ezt az információt egy informatikai rendszergazda is jobban meghatározhatja, ahol lehetséges biztonsági kockázatokat, hogy azok megfelelően megtervezheti toomitigate kockázatok vizsgálandó.  [Az Azure AD Premium előfizetéssel](active-directory-get-started-premium.md) a biztonsági jelentések, engedélyezhet informatikai tooobtain ezt az információt tartalmaz. [Az Azure AD-jelentések](active-directory-view-access-usage-reports.md) szerint vannak kategóriába alább látható módon:

* **Az anomáliadetektálási jelentések**: Jelentkezzen be, hogy észleltünk toobe rendellenes eseményeket tartalmazza. Célunk toomake tud-e a tevékenység, és lehetővé teszik a toobe képes toomake arról, hogy az esemény gyanúsnak meghatározása.
* **Integrált alkalmazás jelentés**: hogyan használja a szervezet a felhőalapú alkalmazások betekintést nyújt. Az Azure Active Directory integrálható a felhőalapú alkalmazások ezer.
* **Hibajelentések**: fiókok tooexternal alkalmazások kiépítése során esetlegesen előforduló hibákat.
* **Felhasználó-specifikus jelentések**: a tevékenységek adatai egy adott felhasználó eszköz/sign megjelenítéséhez.
* **Tevékenységi naplóit**: feljegyzés az elmúlt 24 óra, 7 napja hello belül minden naplózott eseményeket tartalmaz, vagy az utolsó 30 nap, valamint a csoport tevékenység módosítások és a jelszó alaphelyzetbe állítása és nyilvántartási tevékenység.

> [!TIP]
> Egy másik olyan jelentést, amely segít is az esetek Incidensmegoldási team hello hello [kiszivárgott hitelesítő adatokkal rendelkező felhasználó](http://blogs.technet.com/b/ad/archive/2015/06/15/azure-active-directory-premium-reporting-now-detects-leaked-credentials.aspx) jelentés.  Ez a jelentés egyezéseket felfedi a kiszivárgott hitelesítő adatok lista és a bérlői között.
>
>

Más fontos beépített jelentések az Azure AD-az incidensekre adott reakciók vizsgálat során használható, és a következő:

* **Jelszó-visszaállítási tevékenység**: biztosítson Üdvözöljük a rendszergazdákat milyen aktívan jelszó-visszaállításhoz használatos hello szervezet betekintést.
* **Jelszó-visszaállítási regisztrációs tevékenység**: biztosít, amelybe felhasználók regisztrált a jelszó alaphelyzetbe állítása, módszerei insights és módszerek van kiválasztva.
* **Tevékenység csoport**: változások toohello csoport előzményeit biztosítja (pl.: hozzáadott vagy eltávolított felhasználók), amely a hozzáférési Panel hello kezdeményezték.

Ezenkívül toohello fő jelentéskészítési szolgáltatásával Azure AD Premium, amely is javítható az Incidensmegoldási vizsgálat során, az informatikai részleg során is kihasználhatja naplózási jelentés tooobtain információkat, mint:

* Szerepkör tagság változása (pl.: felhasználói tooGlobal rendszergazdai szerepkörhöz hozzá)
* Hitelesítőadat-frissítések (pl.: jelszó-változtatásának)
* Tartományok (pl.: egy egyéni tartományt, a tartomány eltávolításakor ellenőrzése)
* Hozzáadásával vagy eltávolításával alkalmazások
* Felhasználók kezelése (pl.: hozzáadását, eltávolítását, a felhasználó frissítése)
* Hozzáadásával vagy eltávolításával licencek

Hello lehetőségeinek incidensválaszra egy többrétegű módszert használja, mivel ezek a lehetőségek összehasonlítása nem vonatkoznak a feladathoz. Győződjön meg arról, hogy minden beállítás érhető el az egyes forgatókönyvek esetében szükséges toouse az Azure AD jelentéskészítési képességet a vállalat incidensválasz folyamat részeként hasznosítja ki.

## <a name="next-steps"></a>Következő lépések
[Hibrid identitás felügyeleti feladatok meghatározása](active-directory-hybrid-identity-design-considerations-hybrid-id-management-tasks.md)

## <a name="see-also"></a>Lásd még:
[Kialakítási szempontok áttekintése](active-directory-hybrid-identity-design-considerations-overview.md)
