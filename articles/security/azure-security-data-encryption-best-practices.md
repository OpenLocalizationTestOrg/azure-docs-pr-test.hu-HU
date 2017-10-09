---
title: "aaaData biztonsági és titkosítási gyakorlati tanácsok |} Microsoft Docs"
description: "Ez a cikk számos gyakorlati tanácsok az adatok biztonságát, és a beépített titkosítási használata Azure-képességek."
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ms.assetid: 17ba67ad-e5cd-4a8f-b435-5218df753ca4
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/09/2017
ms.author: yurid
ms.openlocfilehash: 5057c85ed3107921462a40045e716675ea41e4bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-data-security-and-encryption-best-practices"></a>Az Azure Data biztonsági és a titkosítás gyakorlati tanácsok
Egy hello kulcsok toodata védelmi hello felhőben van elszámolása hello lehetséges állapota, amelyben az adatok akkor fordulhat elő, és milyen vezérlők érhetők el az adott állapotban. Hello célja az adatok az Azure biztonsági és titkosítási gyakorlati tanácsokat hello javaslatok körül hello adatot meg a következő lesz:

* Nyugalmi: Ez magában foglalja a tárolási objektum, a tárolók és a fizikai adathordozó statikusan létező típusok kell azt mágneses vagy optikai lemez összes információt.
* Az átvitel közbeni: Amikor adatátvitel közötti összetevők, a helyek vagy a programok, például hello hálózaton keresztül egy service bus (a helyszíni toocloud, és fordítva, beleértve a hibrid kapcsolatok, például az ExpressRoute) keresztül vagy egy bemeneti/kimeneti során folyamat, hogy van-re, hogy a mozgásérzékelési.

Ez a cikk az Azure data biztonsági és a titkosítás az ajánlott eljárások gyűjteménye ismertetik. Az alábbi gyakorlati tanácsok az Azure data biztonsági származtatja tapasztalatunk, és a titkosítás és hello élményének ügyfelek, például a saját maga.

Az egyes ajánlott eljárás azt ismertetjük:

* Milyen hello ajánlott eljárás
* Miért érdemes tooenable, hogy a legjobb
* Mi lehet hello eredmény, ha Ön nem tooenable hello ajánlott
* Lehetséges alternatívák toohello ajánlott
* Hogyan szerezhet tooenable hello ajánlott

A Azure Adatbiztonság és -titkosítás gyakorlati tanácsok cikk az együttműködési véleményét, és az Azure platform olyan képességeit és a szolgáltatáskészletek, alapján ez a cikk írásának hello időpontban léteznek. Vélemények és technológiák változnak az idők, és ez a cikk lesznek frissítve egy rendszeresen tooreflect ezeket a módosításokat.

Az Azure data biztonsági és titkosítási gyakorlati tanácsokat cikkben említett a következők:

* Többtényezős hitelesítés kikényszerítéséhez
* Használjon szerepköralapú hozzáférés-vezérlést (RBAC)
* Az Azure virtuális gépek titkosítása
* Hardveres biztonsági modellek használata
* Biztonságos munkaállomások kezelése
* SQL-titkosítás engedélyezéséhez
* Adatok védelmére átvitel
* Fájl szintű adatok titkosításának kényszerítése

## <a name="enforce-multi-factor-authentication"></a>Többtényezős hitelesítés kikényszerítéséhez
hello első lépéseként adatelérési és a Microsoft Azure-vezérlés tooauthenticate hello felhasználó. [Az Azure multi-factor Authentication (MFA)](../multi-factor-authentication/multi-factor-authentication.md) csupán felhasználónévvel és jelszóval mint egy másik módszer használatával felhasználói identitás ellenőrzése módot. Ezt a hitelesítési módszert segít a biztonságos működés érdekében hozzáférés toodata és alkalmazások mellett egyszerű bejelentkezési folyamatot a felhasználó igény szerint.

A felhasználók számára az Azure MFA engedélyezésével ad hozzá egy második réteg biztonsági toouser bejelentkezéseket és tranzakciókat. Ebben az esetben egy tranzakció lehetséges, hogy használja a dokumentum egy fájlkiszolgálón, vagy a SharePoint Online-ban található. Az Azure MFA informatikai tooreduce hello valószínűsége, hogy a feltört hitelesítő adatot lesz-e a hozzáférés tooorganization adatok is segíti.

Például: Ha az Azure többtényezős hitelesítés kényszerítéséhez a felhasználók számára, és konfigurálni toouse egy telefonhívással vagy szöveges üzenettel ellenőrzést, ha hello felhasználó hitelesítő adatainak biztonsága sérül, hello támadó nem lehet a bármilyen olyan erőforrás képes tooaccess mivel ő nem kell hozzáférést toouser phone. A szervezeteknek, amelyek nem adja hozzá a további védelmi réteg biztosítása identitás amelyek jobban ki vannak téve a hitelesítő adatok jelszóellopásos támadáshoz, ami azt eredményezheti, toodata sérült biztonság esetén.

Olyan szervezeteknek, amelyek tookeep hello hitelesítési vezérlőt egy alternatív helyszíni toouse [Azure multi-factor Authentication kiszolgáló](../multi-factor-authentication/multi-factor-authentication-get-started-server.md), más néven az MFA a helyszínen. Ez a módszer használatával továbbra is meg fogja tudni tooenforce többtényezős hitelesítést, ugyanakkor változatlanul megőrizze a hello MFA kiszolgáló a helyi.

További információ az Azure MFA, olvassa el az hello cikk [Ismerkedés az Azure multi-factor Authentication hello felhőben](../multi-factor-authentication/multi-factor-authentication-get-started-cloud.md).

## <a name="use-role-based-access-control-rbac"></a>Használjon szerepköralapú hozzáférés-vezérlést (RBAC)
Hello alapuló hozzáférés korlátozása [tooknow kell](https://en.wikipedia.org/wiki/Need_to_know) és [legalacsonyabb jogosultsági szint](https://en.wikipedia.org/wiki/Principle_of_least_privilege) biztonsági alapelveket. Ez elengedhetetlen a szervezeteknek, amelyek szeretné, hogy az adatelérési tooenforce biztonsági házirendeket. Azure szerepköralapú hozzáférés-vezérlés (RBAC) lehet használt tooassign engedélyek toousers, csoportok és alkalmazások egy adott hatókörben. szerepkör-hozzárendelés hello hatóköre lehet előfizetés, egy erőforráscsoport vagy egy erőforrást.

Kihasználhatja [beépített RBAC-szerepkörök](../active-directory/role-based-access-built-in-roles.md) az Azure-ban tooassign toousers jogosultsággal. Érdemes lehet *tárolási fiók közreműködői* a felhő üzemeltetői toomanage tárfiókok igénylő és *klasszikus tárolási fiók közreműködői* szerepkör toomanage klasszikus tárfiókokat. A felhő üzemeltetői, amelyet a toomanage virtuális gépek és a tárfiókot, fontolja meg, azokat túl*virtuális gép közreműködő* szerepkör.

A szervezeteknek, amelyek kényszeríti ki a hozzáférés-vezérlés képességeinek például RBAC által előfordulhat, hogy kell jogosultságot ad mint azok a felhasználók számára szükséges további engedélyekkel. Azzal, hogy egyes felhasználók hozzáférési toodata, amelyek nem rendelkeznek hello első helyen kellene toodata biztonsági sérülés vezet.

További tudnivalók az Azure RBAC hello cikk olvasásával [átruházásához hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).

## <a name="encrypt-azure-virtual-machines"></a>Az Azure virtuális gépek titkosítása
A legtöbb szervezet számára [adatok titkosítását](https://blogs.microsoft.com/cybertrust/2015/09/10/cloud-security-controls-series-encrypting-data-at-rest/) kötelező lépés adatvédelmi, a megfelelőség és az adatok közös joghatóság alá felé. Az Azure Disk Encryption lehetővé teszi az informatikai rendszergazdák tooencrypt Windows és Linux rendszerű infrastruktúra-szolgáltatási virtuális gép (VM) lemezek. Az Azure Disk Encryption hello iparági szabványos BitLocker-szolgáltatás a Windows és Linux operációs rendszer hello tooprovide kötettitkosítást hello DM-Crypt funkciója és hello adatlemezek kihasználja.

Kihasználhatja az Azure Disk Encryption toohelp és megvédeni az adatok toomeet a szervezeti biztonsági és megfelelőségi követelményeknek. A szervezetek is figyelembe kell venni titkosítással toohelp kapcsolódó toounauthorized adatelérési kockázatok csökkentése. Ajánlott továbbá meghajtók előzetes toowriting bizalmas adatok toothem a titkosítást.

Győződjön meg arról, hogy tooencrypt a virtuális gép az adatkötetek és a rendszerindító kötet rendelés tooprotect adatok Azure-tárfiókot lévő, aktívan. Hello titkosítási kulcsok és titkos kulcsok védelme, ami [Azure Key Vault](../key-vault/key-vault-whatis.md).

A helyi Windows-kiszolgálók esetén fontolja meg a következő gyakorlati tanácsok titkosítási hello:

* Használjon [BitLocker](https://technet.microsoft.com/library/dn306081.aspx) az adatok titkosításához
* Az AD DS-ben helyreállítási információk tárolására.
* Ha a probléma, hogy a BitLocker-kulcsok biztonsága sérült, javasoljuk, vagy formázza hello meghajtó tooremove hello BitLocker metaadatok hello vagy, hogy az összes példányát fejti vissza és hello teljes meghajtót titkosítsa újra.

A szervezeteknek, amelyek nem a következő adatokat a titkosítás kényszerítéséhez nagy valószínűséggel kitett toobe toodata épségével kapcsolatos problémák, például a rosszindulatú vagy rosszindulatú felhasználók ellophassák az adatokat, és fiókokhoz való illetéktelen hozzáférés toodata egyértelmű formátumban sérült. Mellett a kockázatok vállalatok számára, hogy az iparági szabályozások toocomply igazolnia kell, hogy azok szokott vagy hello megfelelő biztonsági vezérlők tooenhance adatbiztonság használ.

További tudnivalók Azure Disk Encryption hello cikk olvasásával [lemez titkosítás a Windows Azure és a Linux IaaS virtuális gépeket](azure-security-disk-encryption.md).

## <a name="use-hardware-security-modules"></a>Hardveres biztonsági modulok használata
Iparági titkosítási megoldások titkos kulcsok tooencrypt adatok felhasználásával. Ezért fontos, hogy ezeket a kulcsokat biztonságosan kell tárolni. Kulcskezelés adatok védelmével, szerves része lesz, óta, akkor kihasználhatók toostore titkos kulcsokat is használt tooencrypt adatok.

Használja az Azure lemeztitkosítás [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) toohelp, szabályozása és lemez titkosítási kulcsok és titkos kódokat a kulcstartót az előfizetést, biztosítva, hogy a hello virtuális gépek lemezeit az összes adat titkosítva legyenek az Azure lévő, aktívan tárolás. Az Azure Key Vault tooaudit kulcsok és a házirend-használati kell használni.

Nincsenek sok rejlő kockázatokat kapcsolódó toonot megfelelő biztonsági vezérlőket rendelkező hely tooprotect hello titkos kulcsait, melyeket használt tooencrypt adatait. A támadók hozzáférhetnek toohello titkos kulcsait, hogy képes toodecrypt hello adatokat, és potenciálisan rendelkezik tooconfidential adatok eléréséhez.

További kapcsolatos általános javaslatok az Azure tanúsítványkezelés hello cikk olvasásával [Azure tanúsítványkezelés: javaslatok és dolog](https://blogs.msdn.microsoft.com/azuresecurity/2015/07/13/certificate-management-in-azure-dos-and-donts/).

Az Azure Key Vault kapcsolatos további információkért olvassa el a [Ismerkedés az Azure Key Vault](../key-vault/key-vault-get-started.md).

## <a name="manage-with-secure-workstations"></a>Biztonságos munkaállomások kezelése
Hello támadások cél hello végfelhasználói többsége hello, mert hello végpont lesz egyik hello elsődleges támadások. Ha egy támadó gyengíti hello végpont, ő kihasználhatják a hello felhasználói hitelesítő adatok toogain hozzáférés tooorganization tartozó adatokat. A legtöbb végpont támadások hello tényt, hogy a végfelhasználók a saját helyi munkaállomások rendszergazdák képesek tootake előnye.

A biztonságos felügyeleti munkaállomás használatával a kockázatok csökkentése érdekében. Azt javasoljuk, hogy használja a [Privileged Access munkaállomások (PAW)](https://technet.microsoft.com/library/mt634654.aspx) tooreduce hello támadási felületet a munkaállomások. A biztonságos felügyeleti munkaállomások segítségével ezek közül néhány támadások biztosítsa az adatok biztonságosabb. Győződjön meg arról, hogy toouse PAW tooharden és a zárolási le a munkaállomáson. Ez az egy fontos lépés tooprovide magas biztonsági garanciák kényes fiókok, feladatok és az adatok védelmére.

Az endpoint protection hiánya lehet, hogy az adatok veszélynek, győződjön meg arról, hogy tooenforce biztonsági házirendek, amelyek használt tooconsume adatok függetlenül hello adatok helye (felhőalapú vagy helyszíni) minden eszközön.

További információ a privilegizált hello cikk elolvasása eléréséhez munkaállomás tudhat [emelt szintű hozzáférés biztonságossá tétele](https://technet.microsoft.com/library/mt631194.aspx).

## <a name="enable-sql-data-encryption"></a>SQL-titkosítás engedélyezéséhez
[Az Azure SQL Database átlátható adattitkosítás](https://msdn.microsoft.com/library/dn948096.aspx) (TDE) segítségével valós idejű titkosítási és visszafejtési hello adatbázis, a társított biztonsági másolatok, a kártékony tevékenység hello fenyegetés elleni és a tranzakciós naplófájlok aktívan nélkül igénylő módosításokat toohello alkalmazás.  TDE hello tárolása teljes adatbázis szimmetrikus kulcs hívott hello adatbázis-titkosítási kulcs használatával titkosítja.

Még ha hello teljes tárolási titkosítva van, akkor nagyon fontos tooalso maga az adatbázis titkosítása. Ez az üdvözlő védelmi mélysége megközelítéssel a data protection megvalósítását. Ha használ [Azure SQL Database](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx) , és szeretnének tooprotect bizalmas adatok hitelkártyával vagy társadalombiztosítási számot, például titkosíthatja adatbázisok a FIPS 140-2 hitelesített 256 bites AES titkosítást és meg kell felelnie a hello követelményeinek számos az ipari szabványok (például a HIPAA, a PCI).

Fontos toounderstand fájlok kapcsolódó túl[kiterjesztés puffer](https://msdn.microsoft.com/library/dn133176.aspx) (BPE) Ha egy adatbázis titkosítása a TDE nincs titkosítva. A fájlszintű titkosítás Rendszereszközök fájlt például a BitLocker vagy hello kell használnia [titkosított fájlrendszer](https://technet.microsoft.com/library/cc700811.aspx) (EFS) BPE a kapcsolódó fájlokat.

Hitelesített felhasználó óta, mint például a biztonsági rendszergazda vagy egy adatbázis-rendszergazda hozzáférhessen hello adatokat, akkor is, ha a TDE, hello adatbázis van titkosítva is kövesse az alábbi hello ajánlásokat:

* SQL-hitelesítés hello adatbázis szintjén
* Az Azure AD-alapú hitelesítés használata az RBAC-szerepkörök
* Felhasználók és az alkalmazások tooauthenticate külön fiókot kell használnia. Ezzel a módszerrel toousers és az alkalmazások adott hello engedélyek korlátozása, és a kártékony tevékenység hello kockázatok csökkentése
* Rögzített adatbázis-szerepkörök (például a db_datareader vagy db_datawriter), vagy alkalmazzon adatbázis szintű biztonsági szerepköröket hozhatnak létre egyéni az alkalmazás toogrant a kifejezett engedélyek tooselected adatbázis-objektumok

Lehet, hogy adatbázis a blokkszintű titkosítás nem használó szervezetek jobban ki vannak téve a támadásokkal szemben, amelyek esetlegesen veszélyeztető SQL-adatbázisban található adatokhoz.

További Erőforráscsoportoknál titkosításával kapcsolatos hello cikk olvasásával [átlátható adattitkosítást az Azure SQL Database](https://msdn.microsoft.com/library/0bf7e8ff-1416-4923-9c4c-49341e208c62.aspx).

## <a name="protect-data-in-transit"></a>Adatok védelmére átvitel
Az átvitel során az adatok védelme a data protection stratégia nagyon fontos részét kell lennie. Oda-vissza adatokat fog áthelyezése több helyről, mivel a hello általános javaslat, hogy mindig használjon SSL/TLS protokollok tooexchange adatok különböző helyek között. Bizonyos esetekben érdemes lehet tooisolate hello teljes kommunikációs csatornát a helyszíni és a felhő közötti virtuális magánhálózati (VPN) segítségével infrastruktúra.

Az adatok áthelyezése a helyszíni infrastruktúra és az Azure között megfelelő védelmi funkciók, például a HTTPS- vagy VPN-érdemes lehet.

Használja a szervezet számára több munkaállomások helyszíni tooAzure toosecure hozzáférést igénylő [Azure telephelyek közötti VPN](../vpn-gateway/vpn-gateway-site-to-site-create.md).

Olyan szervezeteknek, amelyek egy munkaállomásról toosecure hozzáférésre van szükségük a helyszíni tooAzure, használjon található [pont-pont VPN](../vpn-gateway/vpn-gateway-point-to-site-create.md).

Nagyobb, mint egy dedikált nagy sebességű WAN-kapcsolaton keresztül anélkül áthelyezhetők [ExpressRoute](https://azure.microsoft.com/services/expressroute/). Ha toouse ExpressRoute, hello adatok hello alkalmazásszintű használatával is titkosíthatók [SSL/TLS](https://support.microsoft.com/kb/257591) vagy egyéb protokollok hozzáadott védelemre.

Interakció Azure Storage hello Azure portálon keresztül, ha minden tranzakciója történik, HTTPS használatával. [Storage REST API felülete](https://msdn.microsoft.com/library/azure/dd179355.aspx) HTTPS-KAPCSOLATON keresztül is lehet a használt toointeract [Azure Storage](https://azure.microsoft.com/services/storage/) és [Azure SQL Database](https://azure.microsoft.com/services/sql-database/).

A szervezeteknek, amelyek sikertelen tooprotect adatokat átvitel közben is jobban ki vannak téve a [-átjárójának](https://technet.microsoft.com/library/gg195821.aspx), [lehallgatás](https://technet.microsoft.com/library/gg195641.aspx) és munkamenet-eltérítés. Ilyen támadások hello első lépéseként való hozzáférés tooconfidential adatok is lehetnek.

További Azure VPN lehetőségekről hello cikk olvasásával [tervezése és kialakítása VPN-átjáró](../vpn-gateway/vpn-gateway-plan-design.md).

## <a name="enforce-file-level-data-encryption"></a>Fájl szintű adatok titkosításának kényszerítése
Hello fájl, függetlenül attól, hello fájl helye, amely növelheti az adatok biztonsági szintjét hello védelmi réteget titkosítja.

[Az Azure RMS](https://technet.microsoft.com/library/jj585026.aspx) által használt titkosítási, identitáskezelési és engedélyezési házirendek toohelp a fájlok és e-mailek biztonságos. Több eszközön működik az Azure RMS-telefonokon, táblagépeken és számítógépeken megvédi a szervezeten belül, mind a szervezeten kívülről. Ez a lehetőség azért lehetséges, mert az Azure RMS, ad hozzá egy védelmi szintet – hello adatok maradnak, még akkor is, ha elhagyják a szervezet területét.

Azure RMS tooprotect használatakor a fájlok szabványos kriptográfia használ a teljes körű támogatása [FIPS 140-2](http://csrc.nist.gov/groups/STM/cmvp/standards.html). A data protection is használja az Azure RMS, ha akkor is, ha másolt toostorage, amely nincs hello vezérlése alatt van, amely hello védelme mindig fennmarad hello fájl hello megbízhatósági informatikai, például egy felhőalapú tárolási szolgáltatásba. hello azonos fordul elő mailben megosztott fájlok, hello fájl védett mellékletet tooan e-mail üzenetben, utasításokkal hogyan tooopen hello melléklet védett.

Ha Azure RMS bevezetésének megtervezése hello következőket javasoljuk:

* Telepítse a hello [RMS-megosztó alkalmazás](https://technet.microsoft.com/library/dn339006.aspx). Ez az alkalmazás integrálja az Office-alkalmazások telepítésekor egy Office-bővítmény, hogy a felhasználók egyszerűen védhetik a fájlok közvetlenül.
* Alkalmazások és szolgáltatások toosupport Azure RMS konfigurálása
* Hozzon létre [egyéni sablonok](https://technet.microsoft.com/library/dn642472.aspx) , amely az üzleti igényeknek megfelelően. Például: felső titkos adatok, az összes felső titkos alkalmazni kívánt sablont kapcsolódó e-maileket.

A szervezetek, amelyek a gyenge [adatbesorolást](http://download.microsoft.com/download/0/A/3/0A3BE969-85C5-4DD2-83B6-366AA71D1FE3/Data-Classification-for-Cloud-Readiness.pdf) és lehet, hogy a fájlvédelem jobban ki vannak téve toodata adatszivárgást. Megfelelő fájl védelem nélkül szervezetek nem fog tudni tooobtain üzleti elemzéseket, visszaélés-figyelő és meg kell akadályozni rosszindulatú hozzáférésektől toofiles.

További tudnivalók az Azure RMS által hello cikk elolvasása [Ismerkedés az Azure Rights Management](https://technet.microsoft.com/library/jj585016.aspx).
