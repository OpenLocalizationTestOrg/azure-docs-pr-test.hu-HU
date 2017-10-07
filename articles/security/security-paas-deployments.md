---
title: "aaaSecuring PaaS üzemelő példányok |} Microsoft Docs"
description: " Ismerje meg más felhőalapú szolgáltatási modellt és PaaS hello biztonsági előnyöket, és ismerje meg, biztonságossá tétele az Azure PaaS központi telepítésének ajánlott eljárásai. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: techlake
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: terrylan
ms.openlocfilehash: b94fe03b6f3a43d82e1e6e834f10a423e4c1db95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="securing-paas-deployments"></a>PaaS üzemelő példányok biztonságossá tétele

A cikkben információk segítségével:

- Hello előnyeinek hello felhőben üzemeltetéséhez ismertetése
- Értékelje ki a hello biztonsági előnyeit platform szolgáltatásként (PaaS) vagy más felhőalapú szolgáltatás modellek
- A biztonsági fókusz módosításához az identitás-központú szegélyhálózati hálózati-központú tooan biztonsági megközelítés
- Általános PaaS biztonság ajánlott eljárásait megvalósítása

## <a name="cloud-security-advantages"></a>Felhőalapú biztonsági előnyeit
Nincsenek biztonsági előnyeit toobeing hello felhő. A helyszíni környezetben, valószínűleg szervezeteknél unmet felelősségi és biztonsági, amely létrehoz egy környezetében, ahol a támadók képesek tooexploit biztonsági rétegeket, a korlátozott erőforrásokat a rendelkezésre álló tooinvest.

![Felhő éra biztonsági előnyeit][1]

A szervezetek esetén egy szolgáltató felhőalapú biztonsági képességei és felhő eszközintelligencia használatával történő képes tooimprove válaszidejét, valamint a fenyegetések észlelése.  Feladatkörök toohello felhőszolgáltatóként túl, a szervezetek és kapják meg további biztonsági érvényességének, amelyek lehetővé teszik, hogy tooreallocate biztonsági erőforrások költségvetési tooother üzleti prioritások.

## <a name="division-of-responsibility"></a>Felelőssége
Fontos toounderstand hello részlege felelőssége, és a Microsoft között. A helyszíni hello teljes verem saját, de toohello felhő mozgatásakor látnia néhány feladatkört átviteli tooMicrosoft. hello következő feladata mátrix hello verem hello területéhez egy felelőssége SaaS, a PaaS és IaaS-telepítés és a Microsoft felelős.

![Felelős zónák][2]

Összes felhő központi telepítési típusok akkor saját adatait és identitások. Ön hello biztonsági az adatok és az identitások, a helyszíni erőforrások védelmét, és szabályozhatja felhőalapú összetevőinek hello (ami függ a szolgáltatás típusa).

Felelőssége, hogy Ön hello típusú telepítés, függetlenül mindig megőrzi a következők:

- Adatok
- Végpontok
- Fiók
- Hozzáférés-kezelés

## <a name="security-advantages-of-a-paas-cloud-service-model"></a>A felhő modell egy PaaS biztonsági előnyeit
Hello használatával most azonos felelősségi mátrix, nézze meg az Azure PaaS központi telepítése a helyi és hello biztonsági előnyeit.

![A PaaS biztonsági előnyeit][3]

Kezdő pozíció: hello alsó hello vermet, hello fizikai infrastruktúra, a Microsoft csökkenti közös kockázatok és felelősségek meghatározása. Mivel hello Microsoft felhő folyamatosan figyelni a Microsoft által, szigorú tooattack is. Azt nem bírnak egy támadó toopursue hello Microsoft felhő célként. Kivéve hello támadás nagy mennyiségű pénzügyek szolgáltatásban és az erőforrások és a hello támadó van valószínűleg toomove tooanother célponton.  

Hello középső hello vermet nincs egy PaaS üzembe helyezése és a helyszíni közötti különbség. Hello alkalmazásréteg és hello fiók és a hozzáférés a felügyeleti réteg hogy hasonló kockázatokat. Hello további lépések szakaszt az ennek a cikknek, a Microsoft végigvezeti toobest eljárások elkerüléséhez, vagy a kockázatok minimalizálja a.

Hello felső hello verem, az adatok irányítási és a rights Managementet akkor igénybe egy kockázata, hogy a kulcskezelő enyhíthetők. (Ajánlott eljárások a kulcskezelési vonatkozik.) Kulcskezelés feladata egy további, amíg egy PaaS-telepítésben, hogy már nem rendelkezik toomanage, akkor is az eltolás mértékét megadó erőforrások tookey felügyeleti területek rendelkezik.

hello Azure platformon is nyújt erős DDoS-védelem különböző hálózati technológiák használatával. Azonban minden típusú DDoS védelmi módszert hálózatalapú korlátozottak csatlakozásonként és adatközpont alapon. toohelp hello nagy DDoS-támadások hatásainak elkerüléséhez veheti hasznát Azure core felhő funkció, amely lehetővé teszi, tooquickly, és automatikusan kibővítési toodefend DDoS-támadások ellen. Részletesen ismertetjük a hogyan ehhez az ajánlott eljárások cikkek hello.

## <a name="modernizing-hello-defenders-mindset"></a>Korszerűsítése hello defender alaposan
A PaaS központi telepítések a teljes megközelítés toosecurity eltolódása származnak. Ekkor vált át kellene toocontrol mindent saját kezűleg toosharing felelős a Microsofttal.

Egy másik a jelentős különbség PaaS és a hagyományos helyszínen telepítések esetén, mi hello elsődleges biztonsági szegélyhálózati határozza meg az új nézet. Hagyományosan hello elsődleges helyszíni biztonsági szegélyhálózati volt a hálózat és a helyszíni és az elsődleges biztonsági pivot biztonsági tervek használata hello hálózathoz. A PaaS üzemelő példányok akkor vannak a legjobban kiszolgálni identitás toobe hello elsődleges biztonsági szegélyhálózati figyelembe véve.

## <a name="identity-as-hello-primary-security-perimeter"></a>Azonosító adatai különböznek hello elsődleges biztonsági szegélyhálózat
Hello öt alapvető jellemzőt, a felhőalapú számítástechnika a széleskörű hálózati hozzáférés, így hálózati-központú a kevésbé fontos végezni. hello célja nagy részét a felhőalapú informatika egy tooallow felhasználók tooaccess erőforrások helyétől függetlenül. A legtöbb felhasználó esetén a hely van folyamatban toobe valahol hello Internet.

hello következő ábrán látható hogyan hello biztonsági szegélyhálózati az egy hálózati szegélyhálózati tooan identitás szegélyhálózati kialakulásának. Biztonsági válik, gondoskodnia kell a hálózati kapcsolatban, és további tájékozódhat az adatok védelme, valamint hello biztonsága érdekében az alkalmazások és felhasználók kezeléséhez. hello fő különbség, hogy szeretné-e toopush biztonsági szorosabb toowhat tartozó fontos tooyour vállalati.

![Azonosító adatai különböznek új biztonsági szegélyhálózat][4]

Kezdetben Azure PaaS-szolgáltatások (például a webes szerepkörök és az Azure SQL) megadott kevéssé vagy egyáltalán ne hagyományos hálózati szegélyhálózati védelmet. Az értelmezése hello elem célja volt kitéve toobe toohello Internet (webes szerepkör), és, hogy a hitelesítési hello új szegélyhálózat (például a BLOB vagy az Azure SQL) biztosít.

Modern biztonsági eljárások azt feltételezik, hogy adott hello ellenfél lett szegve hello hálózati konfigurációtól. Ezért modern védelmi eljárások tooidentity került át. A szervezetek létre kell hoznia egy biztonsági azonosító-alapú szegélyhálózati az erős hitelesítési és engedélyezési higiéniai (ajánlott eljárások).

## <a name="recommendations-for-managing-hello-identity-perimeter"></a>Javaslatok hello identitás szegélyhálózati kezelése

Alapelvek és minták a hello hálózati szegélyhálózati hozzáférhetők évtizedeken. Ezzel szemben hello iparági van használva identity hello elsődleges biztonsági szegélyhálózati viszonylag kevesebb szolgáltatást. Az említett, azt rendelkezik halmozott elegendő élmény tooprovide hello mezőben bizonyítottan néhány általános javaslatokat és alkalmazása tooalmost minden PaaS-szolgáltatás.

hello alábbi általános, ajánlott eljárások megközelítés toomanaging a identitás szegélyhálózati foglalja össze.

- **Ne veszítse el a kulcsok vagy a hitelesítő adatok** kulcsok és hitelesítő adatok védelme az alapvető toosecure PaaS üzemelő példányok. Kulcsok és a hitelesítő adatok elvesztése gyakori probléma. Egy jó megoldás toouse a központi megoldás, ahol kulcsok és titkos tárolhatja hardveres biztonsági modulokkal (HSM). Azure biztosít az hello felhőben HSM [Azure Key Vault](../key-vault/key-vault-whatis.md).
- **Hitelesítő adatok és más titkos adatokat nem kerüljenek, a forráskód vagy GitHub** hello csak dolog ami még rosszabb, mint a kulcsok és a hitelesítő adatok elvesztése nehézségekkel hogy jogosulatlan személyek toothem eléréséhez. A támadók képes tootake előnyeit, botot technológiák toofind kulcsok és titkos adattárakban például GitHub tárolva. Nem be kulcs és a titkos kulcsok ezek nyilvános forráskódú adattárakban.
- **A virtuális gép felületek hibrid PaaS és IaaS-szolgáltatásoknak védelme** IaaS és PaaS szolgáltatások futó virtuális gépek (VM). Szolgáltatás hello típusától függően több felügyeleti felület érhetők el, amelyek lehetővé teszik tooremote közvetlenül a virtuális gépek kezeléséhez. Például a távoli felügyeleti protokollokat [Secure Shell-protokoll (SSH)](https://en.wikipedia.org/wiki/Secure_Shell), [Remote Desktop Protocol (RDP)](https://support.microsoft.com/kb/186607), és [távoli PowerShell](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.core/enable-psremoting) is használható. Általában javasoljuk, hogy nem engedélyezi a hello Internet közvetlen távelérési tooVMs. Ha elérhető, más megközelítést választani, például az Azure virtuális hálózat virtuális magánhálózat használatával kell használnia. Ha alternatív módszerek nem érhetők el, akkor győződjön meg arról, hogy használja-e összetett jelszavak, és ha elérhető, kéttényezős hitelesítést (például [Azure multi-factor Authentication](../multi-factor-authentication/multi-factor-authentication.md)).
- **Használjon erős hitelesítési és engedélyezési platformok**

  - Összevont identitások kialakítása használata helyett egyéni felhasználói tárolja az Azure AD-ben. Összevont identitások kialakítása használata esetén, a platform-alapú megközelítés előnyeit, és delegálja a hello felügyeleti engedéllyel rendelkező identitások tooyour partnerek. Egy összevont identitáskezelési megközelítés különösen fontos a forgatókönyvek esetén alkalmazottak leáll, és több identitáskezelési és engedélyezési tükrözi, hogy adatokat kell toobe rendszerek.
  - A megadott hitelesítési és engedélyezési mechanizmusok helyett egyéni kód használata platform. hello oka, hogy az egyéni hitelesítési kód fejlesztése lehet nagyon eséllyel fordulnak elő hiba. A fejlesztők a legtöbb nem biztonsági szakértők és valószínűleg nem kompatibilis hello subtleties és a hitelesítési és engedélyezési legújabb végbement hello toobe. Kereskedelmi kód (például a Microsoft): gyakran széles körben biztonsági vizsgálni.
  - Többtényezős hitelesítés használata. Többtényezős hitelesítés az aktuális hello a hitelesítéshez és engedélyezéshez szabványos, mivel ezzel elkerülheti hello biztonsági hiányosságok szerves része a felhasználónév és jelszó típusú hitelesítés. Hozzáférés tooboth hello Azure (portal/távoli PowerShell) felületek és irányuló szolgáltatások toocustomer úgy kell megtervezni, valamint toouse konfigurálta [Azure multi-factor Authentication (MFA)](../multi-factor-authentication/multi-factor-authentication.md).
  - Szabványon alapuló hitelesítési protokollok, például OAuth2 és a Kerberos használatára. Ezeket a protokollokat nagymértékben társ áttekintette és valószínűleg valósíthatók meg a platform-könyvtárakban hitelesítési és engedélyezési részeként.

## <a name="next-steps"></a>Következő lépések
Ez a cikk azt arra irányul, az Azure PaaS üzembe helyezésének biztonsági előnyeit. Ismerje meg a következő, a PaaS webes és mobil megoldások biztonságossá tételének ajánlott eljárásai. Először az Azure App Service, Azure SQL Database és Azure SQL Data Warehouse foglalkozunk. Amint elérhetővé válnak más Azure-szolgáltatásokhoz ajánlott gyakorlati tanácsok a cikkeket, hivatkozások lesznek közzétéve, a következő lista hello:

- [Azure App Service](security-paas-applications-using-app-services.md)
- [Az Azure SQL Database és az Azure SQL Data Warehouse](security-paas-applications-using-sql.md)
- Azure Storage
- Azure REDIS gyorsítótár
- Azure Service Bus
- Webalkalmazási tűzfalak

<!--Image references-->
[1]: ./media/security-paas-deployments/advantages-of-cloud.png
[2]: ./media/security-paas-deployments/responsibility-zones.png
[3]: ./media/security-paas-deployments/advantages-of-paas.png
[4]: ./media/security-paas-deployments/identity-perimeter.png
