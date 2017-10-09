---
title: "Azure besorolásának aaaData |} Microsoft Docs"
description: "Ez a cikk egy bevezető toohello adatbesorolást alapjai biztosít, és kiemeli a értéke, kifejezetten a felhőalapú informatikával és a Microsoft Azure használatával hello környezetben"
services: security
documentationcenter: na
author: YuriDio
manager: swadhwa
editor: TomSh
ROBOTS: NOINDEX
redirect_url: https://aka.ms/data-classification-cloud
ms.assetid: 91d4afed-b80f-4a26-b526-b52b9c858cfb
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/18/2017
ms.author: yurid
ms.openlocfilehash: 726da2482beab3bf7b0ac33510f2b523d5074df8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="data-classification-for-azure"></a>Az Azure-adatok besorolása
Ez a cikk egy bevezető toohello adatbesorolást alapjai biztosít, és kiemeli a értéke, kifejezetten a felhőalapú informatikával és a Microsoft Azure használatával hello környezetben. 

## <a name="data-classification-fundamentals"></a>Adatok besorolása – alapok
A szervezet sikeres adatbesorolást széles körű tájékoztatást nyújthatnak a szervezete szükségleteinek és az adategységek tároló alapos ismerete szükséges.  

Adatok az alábbi három alapvető állapotban van: 

* Inaktív 
* Folyamatban 
* Az átvitel során 

Minden Háromállapotú az egyedi műszaki megoldás az adatok besorolása érdekében kell lennie, de hello adatbesorolást alkalmazott alapelvei kell kell hello ugyanaz az egyes. Bizalmas besorolt adatokat kell toostay bizalmas amikor aktívan, a folyamat, és az átvitel során. 

Strukturált vagy strukturálatlan is lehet az adatokat. Tipikus besorolási folyamatok hello strukturált adatok találhatók az adatbázisok és táblázatok kevésbé összetett és időigényes toomanage, mint a strukturálatlan adatok, például e-mail, dokumentumok vagy forráskód. 

> [!TIP]
> Azure-képességek és ajánlott eljárások az adatok titkosításához kapcsolatos további tudnivalókért olvassa el a [Azure Data Encryption gyakorlati tanácsok](azure-security-data-encryption-best-practices.md)
> 
> 

Általában a szervezetek lesz több mint strukturált adatok strukturálatlan adatok. Függetlenül attól, hogy adatokat strukturált vagy strukturálatlan fontos, hogy toomanage adatok érzékenysége a. A megfelelően megvalósított adatbesorolást biztosíthatja adategységeiről, amelyeket nyilvános vagy szabad toodistribute minősülnek-nál nagyobb felügyeletet a felügyelt eszközök bizalmas adatokat. 

### <a name="controlling-access-toodata"></a>Hozzáférés toodata szabályozása
Hitelesítési és engedélyezési rendszer gyakran összetéveszthető egymáshoz, és azok a szerepkörök böngésző. A valóságban ezek eltérnek a üzembe helyezését, ahogy az ábra a következő hello.  

![Adathozzáférés és -vezérlő](./media/azure-security-data-classification/azure-security-data-classification-fig1.png)

### <a name="authentication"></a>Authentication
Hitelesítési általában legalább két részből áll: egy felhasználónév vagy a felhasználói azonosító tooidentify egy felhasználó és egy jogkivonat, például a jelszót, tooconfirm, amely hello username hitelesítő adat érvénytelen. hello folyamat nem biztosít hello hitelesített felhasználónak hozzáférést tooany elemek vagy szolgáltatások; ellenőrzi, hogy hello a felhasználó, akinek azokat fel vannak.   

> [!TIP]
> [Az Azure Active Directory](../active-directory/active-directory-whatis.md) , amelyek lehetővé teszik tooauthenticate, és a felhasználóknak engedélyezik, felhőalapú identitás-szolgáltatásokat biztosít. 
> 
> 

### <a name="authorization"></a>Engedélyezés
Engedélyezési hello folyamat, amikor a hitelesített felhasználó hello képességét tooaccess, egy alkalmazás, adatkészlet, adatfájlt vagy más objektum. Hitelesített felhasználók hello jogok toouse hozzárendelése módosíthatja vagy eléréséhez elemek törlése toodata besorolás figyelmet igényel. 

A sikeres hitelesítést igényel, a mechanizmus toovalidate egyes felhasználók végrehajtásának kell tooaccess fájlokat és információkat szerepkör, a biztonsági házirend és a kockázati házirenddel kapcsolatos megfontolások alapján. Például bizonyos-üzletági (LOB) alkalmazások adatait nem minden esetben kell toobe érhető el, amelyet minden alkalmazott, és az alkalmazottak csak egy szűk részhalmaza valószínűleg kell hozzáférésének toohuman erőforrások (HR) fájlok. De a szervezetek toocontrol ki érje el az adatokat, valamint mikor és hogyan, hatékony rendszer azonosítja a felhasználókat kell teljesülniük. 

> [!TIP]
> a Microsoft Azure-ban győződjön meg arról, hogy tooleverage átruházásához hozzáférés-vezérlés (RBAC) toogrant csak hello mértékű hozzáférést, a felhasználóknak frissíteniük kell tooperform a munkájukat. Olvasási [szerepkör hozzárendelések toomanage hozzáférés tooyour Azure Active Directory-erőforrások használata](../active-directory/role-based-access-control-configure.md) további információt. 
> 
> 

### <a name="roles-and-responsibilities-in-cloud-computing"></a>Szerepkörök és felelősségek meghatározása, a felhőalapú informatika
Bár szolgáltatók segítségével a kockázatok kezelése, a felhasználóknak kell tooensure adott adatok besorolások kezelése és a kényszerítési szabályszerű tooprovide hello adatok szolgáltatások megfelelő szintje.  

Feladatkörök változnak az adatok besorolásával alapján mely felhőszolgáltatási modellnek van beállítva, a hello a következő ábrán látható módon. hello három elsődleges felhő szolgáltatási modellt infrastruktúra (IaaS) szolgáltatás, platformszolgáltatást (PaaS), és a szoftverfrissítési szolgáltatásként (SaaS). Adatok besorolása mechanizmusok megvalósítása hello igényel és az azzal kapcsolatos elvárások, hello felhőszolgáltatóként alapján is változhatnak. 

![Szerepkörök](./media/azure-security-data-classification/azure-security-data-classification-fig2.png)

Annak ellenére, hogy Ön felelős az adatok osztályozásához, szolgáltatók biztonságos és a felhőben tárolt ügyféladatok hello hello adatvédelmének lesz írva kötelezettségvállalások kell tennie.  

* **IaaS-szolgáltatók** követelmények korlátozódnak, amelyek a virtuális környezet hello tooensuring képes adatosztályozási képességeket, és az ügyfél megfelelőségi követelményeket. IaaS-szolgáltatók van az adatok besorolását kisebb szerepkör, mert csak van szükségük, hogy ügyféladatokat címek megfelelőségi követelmények tooensure. Azonban szolgáltatók továbbra is győződjön meg arról, hogy a virtuális környezetek veszi a hozzáadása toosecuring adatbesorolási követelményeinek az adatközpontokban.
* **A PaaS szolgáltatók** feladatkörök keverhetők, mert a besorolás eszköz hello platform felhasználhatók réteges megközelítésének tooprovide biztonsági. PaaS szolgáltatók lehet, hogy hitelesítést és esetleg néhány engedélyezési szabályok felelős, valamint biztonsági és az adatok besorolás képességek tootheir alkalmazásréteg kell adnia. Sokkal hasonló IaaS-szolgáltatók PaaS szolgáltatók a szükséges, amely megfelel a platform tooensure bármely megfelelő adatbesorolási követelményeinek.
* **SaaS-szolgáltatók** gyakran számítanak engedélyezési láncon keresztül, és fog kell, hogy a hello SaaS-alkalmazás adataihoz hello tooensure vezérelhető besorolás típus szerint. SaaS-alkalmazásokhoz használható a LOB-alkalmazások, és természetüknél fogva kell tooprovide azt jelenti, hogy tooauthenticate hello és engedélyezését, amely használja, és tárolja az adatokat. 

## <a name="classification-process"></a>Minősítési folyamat
Számos szervezet, hello ismernie kell az adatok besorolása érdekében, és szeretné, hogy azt egy alapszintű kihívást tooimplement: Ha toobegin?

Egy egyszerű és hatékony módon tooimplement adatbesorolást toouse hello terv, hajtsa végre, ellenőrzés, művelet modell a [MOF](https://technet.microsoft.com/solutionaccelerators/dd320379.aspx). hello következő diagramok hello feladatokhoz szükséges toosuccessfully alkalmazzon adatbesorolást a modellben. ábra.  

1. **TERVEZZE MEG**. Adategységek, egy data konfigurációelem gondnoka képes legyen toodeploy hello besorolás programot, határozza meg és védelmi profilok fejlesztéséhez. 
2. **TEGYE**. Adatok besorolási házirendek megegyezésre, miután hello program központi telepítése, és kényszerítési technológiák megvalósításához a bizalmas adatok igény szerint.  
3. **ELLENŐRIZZE**. Ellenőrizze, és a jelentések, amelyek hello eszközök és a használt módszerek hatékonyan címzési tooensure hello besorolási házirendek ellenőrzése. 
4. **ACT**. Adatelérési hello állapotának ellenőrzését, és tekintse át a fájlokat és adatokat, átsorolását és változat módszertana tooadopt módosításokat és tooaddress új kockázatok verzió szükséges.  

![Tervezze meg, ellenőrizze, működésre](./media/azure-security-data-classification/azure-security-data-classification-fig3.png)

### <a name="select-a-terminology-model-that-addresses-your-needs"></a>Válasszon ki egy terminológia modellt, amely az igényeket elégíti ki
Számos különböző típusú folyamatok található adatokat, beleértve a manuális folyamatokat, helyalapú folyamatok, amelyek egy felhasználó vagy rendszer helye, alkalmazás-alapú folyamat, például adatbázis-specifikus besorolás alapján, és automatizálható, az adatok osztályozására osztályozásához a folyamatok különböző technológiákkal, hello "Bizalmas adatok védelmének" című szakaszban található ez a cikk ismerteti, amelyek használják.  

Ez a cikk két általánosított terminológia modellek jól használható és iparági tiszteletben modellek alapuló be. Mindkettő besorolás érzékenységi három szintje, terminológia modellek hello a következő táblázatban láthatók.  

> [!NOTE]
> Ha egy fájlt vagy erőforrást, hogy adatokat különböző szinteken hello legmagasabb szintű besorolást jelen általában szeretné besorolni, létre kell hoznia kombinálja hello általános besorolás besorolása. Például egy korlátozott és bizalmas adatokat tartalmazó fájlt szeretné besorolni, korlátozott.  
> 
> 

| **Érzékenység** | **1 modell terminológia** | **2 modell terminológia** |
| --- | --- | --- |
| Magas |Bizalmas |Korlátozott |
| Közepes |Csak belső használatra |Bizalmas |
| Alacsony |Nyilvános |Nem korlátozott |

#### <a name="confidential-restricted"></a>Bizalmas (korlátozott)
Bizalmas vagy korlátozott lesz minősítve információkat lehet katasztrofális tooone vagy további egyéni felhasználók számára és/vagy a szervezetek, ha sérült, vagy megszakadt adatokat tartalmazza. Ezeket az információkat gyakran rendelkezésre egy "szükséges tooknow", és előfordulhat, hogy tartalmazzák: 

* Személyes adatok, ideértve a személyes azonosításra alkalmas adatokat például társadalombiztosítási vagy nemzeti azonosító számok, passport számok, hitelkártyaszámok, vezető licenc számok, orvosi rögzíti és egészségbiztosítási házirend azonosítószámát.  
* Pénzügyi rögzíti, beleértve a pénzügyi fiók számokat, például ellenőrzése vagy befektetési fiók számokat. 
* Üzleti anyagokat, például a dokumentumok vagy egyedi vagy adott szellemi tulajdonra vonatkozó adatokat.  
* Jogi adatok, beleértve a potenciális ügyvéddel jogosultságú anyagot. 
* Hitelesítési adatok, beleértve a személyes titkosítási kulcsokat, felhasználónév-jelszó párok vagy egyéb azonosítási sorozatok például személyes biometrikus-fájlokat. 

Adatok besorolt bizalmas gyakran rendelkezik szabályozási és megfelelőségi követelményei az adatkezelési. 

#### <a name="for-internal-use-only-sensitive"></a>A csak belső használatra (bizalmas)
Van besorolva közepes érzékenységi információkat tartalmaz a fájlokat és adatokat, nem lenne egy személy és/vagy a szervezet súlyos következményekkel, ha elvész vagy megsemmisül. Ezeket az információkat a következők lehetnek: 

* E-mailt, amely a legtöbb törölhető vagy elosztott anélkül, hogy ez egy válság (kivéve a postaládák és e-mailek használhatják, akik hello bizalmas besorolást azonosítja a).  
* Dokumentumok és fájlok, amelyek nem tartalmaznak bizalmas adatokat.

Általában ez a besorolás tartalmaz mindent, ami nem bizalmas. Ehhez a besoroláshoz tartalmazhatnak legtöbb üzleti adatok, mert a legtöbb olyan fájlok, vagy használja a napi besorolhatók bizalmasként. Az adatok közzététele, vagy bizalmas hello kivétellel egy üzleti szervezeten belüli összes adat meghatározhassa bizalmas alapértelmezés szerint. 

#### <a name="public-unrestricted"></a>Nyilvános (korlátozás)
Információ a nyilvános besorolt adatokat és fájlokat, amelyek nem kritikus toobusiness igényeinek és műveletek tartalmaz. Ez a besorolás, amelyet a használatára, például a marketing anyagot vagy nyomja le az közlemények nyilvános kiadott toohello szándékosan adatokat is tartalmazhat. Ez a besorolás továbbá adatok, például egy e-mailek szolgáltatás által tárolt e-mailek levélszemetet is tartalmazza. 

### <a name="define-data-ownership"></a>Adja meg az adatok tulajdonjoga
Az összes adategységeket tulajdonjogának egyértelműen szabadságvesztés láncolata fontos tooestablish. hello következő táblázatból azonosíthatja különböző adatok tulajdonjoga szerepköröket az adatok besorolás erőfeszítéseket és megfelelő jogosultságai alapján.  

| **Szerepkör** | **Létrehozás** | **Módosítása vagy törlése** | **Delegált** | **Olvasás** | **Archiválás és visszaállítás** |
| --- | --- | --- | --- | --- | --- |
| Tulajdonos |X |X |X |X |X |
| Konfigurációelem gondnoka képes legyen | | |X | | |
| Rendszergazda | | | | |X |
| Felhasználó\* | |X | |X | |

**Felhasználók jogosultak a további jogokat, mint szerkesztése, és törölje a gondviselője* 

> [!NOTE]
> Ez a táblázat nem biztosít a szerepkörök és a jogok, de a csupán egy reprezentatív minta teljesnek. 
> 
> 

Hello **adatok az eszköz tulajdonosa** hello eredeti létrehozó hello adatok, akik tulajdonjogának delegálását, és rendelje hozzá a gondviselője van. Ha egy fájl jön létre, hello tulajdonos képes tooassign a besorolást, ami azt jelenti, hogy van egy felelősségi toounderstand fontosként megjelölt bizalmas toobe minek a szervezeti házirend alapján kell lennie. Egy eszköz tulajdonosa adatok lehet automatikus minősített esetében csak belső használatra (bizalmas) kivéve, ha azok bizalmas (korlátozott) adattípusok létrehozása vagy a tulajdonos felelős. Hello tulajdonosi szerepkör változik gyakran, miután hello adatok besorolásának. Például hello tulajdonos előfordulhat, hogy a minősített információk-adatbázis létrehozása és fejezheti be a jogosultságok toohello adatok konfigurációelem gondnoka képes legyen.  

> [!NOTE]
> adatok eszköz tulajdonosai szolgáltatások, eszközök és az adathordozó, amelyek személyes és toohello szervezet tartoznak, amelyeket gyakran használnak. Törölje a szervezeti házirend segítségével, gondoskodjon arról, hogy a hordozható eszközök és az intelligens eszközök használati adatok besorolási irányelvek megfelelően.  
> 
> 

Hello **adatok eszköz gondnoki** vagy által hozzárendelt hello az eszköz tulajdonosa (a delegált) toomanage hello eszköz függően tooagreements hello az eszköz tulajdonosa vagy vonatkozó házirend követelményeinek megfelelően. Ideális esetben az automatikus rendszer hello konfigurációelem gondnoka képes legyen szerepkör megvalósítható. Egy eszköz gondnoki biztosíthatja a szükséges hozzáférés-vezérlést kapnak, és felügyeletéért felelős és eszközök védelme meghatalmazott tootheir eljárni. hello eszköz gondnoki hello feladatai lehetnek:  

* Hello eszköz hello az eszköz tulajdonosa irányt vagy egyetért hello az eszköz tulajdonosa védelme 
* Győződjön meg arról, hogy teljesüljenek besorolási házirendek 
* Bármely eszköz tulajdonosait tájékoztatása tooagreed megváltozik-vezérlők és/vagy védelmi eljárások előzetes toothose változások lépnek érvénybe, 
* Módosítások tooor eltávolítással hello eszköz gondnoki feladatkörök jelentéskészítési toohello az eszköz tulajdonosa 
* Egy **rendszergazda** jelöli, hogy integritása, de nem egy adatok az eszköz tulajdonosa, a konfigurációelem gondnoka képes legyen, vagy a felhasználó biztosításáért felelős felhasználó. Tulajdonképpen számos rendszergazdai szerepkörök anélkül, hogy hozzáférési toohello adatok adatokat tároló szolgáltatásokat adja meg. hello rendszergazdai szerepkör tartalmazza azokat a biztonsági mentését és visszaállítását hello adatok hello eszközök, nyilvántartást és kiválasztása, beszerzése és üzemeltetési hello eszközök és a tárolási, hogy a ház hello eszközök. 
* hello eszköz felhasználói bárki, aki kap hozzáférést toodata vagy a fájl tartalmazza. Hozzáférés hozzárendelése gyakran meghatalmazott által hello tulajdonos toohello eszköz konfigurációelem gondnoka képes legyen.  

### <a name="implementation"></a>Megvalósítás
Eszközkezeléssel kapcsolatos szempontok tooall besorolás módszereit, amelyek érvényesek. Ezeket a szempontokat kell tooinclude adatait, aki mi, ahol, időpontját és okát egy adategységet akkor használja, elérhető, módosítható, vagy törölhető. Minden szoftverállomány-kezelés megértéséhez hogyan szervezet megtekinti a kockázatok kell végezni, de egyszerű módszer alkalmazható a hello adatok besorolási folyamat. Az adatok besorolása érdekében további szempontok közé tartoznak új alkalmazások és eszközök, a változások kezelése, a besorolási módszert bevezetése után hello bevezetése.  

### <a name="reclassification"></a>Átsorolási
Sorol át, vagy egy adategységet hello besorolás állapotának módosítása kell toobe végre, ha egy felhasználó vagy rendszer meghatározza, hogy hello adategységet fontosság vagy kockázatú profil megváltozott. Ebből a törekvésből pedig a fontos annak biztosítására, hogy a hello besorolás állapot toobe aktuális továbbra is érvényes. A legtöbb tartalmat, amely nem manuálisan besorolása automatikusan, vagy adatok tulajdonosa vagy adatok konfigurációelem gondnoka képes legyen a felhasználás alapján. 

### <a name="manual-data-reclassification"></a>Kézi átsorolási
Ideális esetben ebből a törekvésből volna győződjön meg arról, hogy olyan módosítás hello részleteit rögzített és naplózva. hello manuális átsorolási hiba legvalószínűbb oka az érzékenységi miatt, vagy papíralapú, vagy egy követelmény tooreview adatokat, amelyek eredetileg történt misclassified tartani rekordok lenne. A dokumentum úgy ítéli meg, az adatok besorolásával és a mozgási adatok toohello felhő, mert manuális átsorolási erőfeszítéseket-eseti alapon figyelmet igényel és kockázati management áttekintése ideális tooaddress tárhelybesorolási követelménynek. Általában ilyen annak érdekében volna kapcsolatos minősített toobe minek hello szervezeti házirend, hello alapértelmezett besorolás állapota (minden adat és fájl-és nagybetűket, de nem bizalmas alatt) és vegye igénybe vehet a magas kockázatú adatok kivételek. 

### <a name="automatic-data-reclassification"></a>Automatikus átsorolási
Automatikus átsorolási azonos általános szabály hello manuális osztályozás használja. hello kivétel ez alól, hogy automatikus megoldások bizonyosodjon meg arról, hogy szabályokat követni, és szükség szerint alkalmazza. Adatok besorolása egy adatok besorolási kényszerítési házirendet, amely tárolja, használja, és az átvitel során engedélyezési technológiával kényszeríthető részeként végezhető.

* Alkalmazás-alapú. Egyes alkalmazások használata alapértelmezés szerint állítja be a besorolási szint. Például az Ügyfélkapcsolat-kezelő (CRM) szoftver, a HR és az állapot rekord felügyeleti eszközök adata bizalmas alapértelmezés szerint. 
* Helyalapú. Adatok helye segítségével azonosítható adatok érzékenysége. HR vagy pénzügyi részleg által tárolt adatok például a nagy valószínűséggel toobe bizalmas jellegű.  

### <a name="data-retention-recovery-and-disposal"></a>Az adatmegőrzés, helyreállítási és értékesítés
Adat-helyreállítás és értékesítési adatokat átsorolási, például egy lényeges szempontja, hogy adategységeket kezelése. hello alapelvek adat-helyreállítás és eltávolítására volna határozza meg az adatmegőrzési házirend és hello érvényesül azonos módon adatok átsorolási; ilyen terjesztettünk hello konfigurációelem gondnoka képes legyen, és rendszergazdai szerepkörök az együttműködési feladatok végbemegy.  

Az adatmegőrzési házirend hiba toohave azt is jelentheti adatok elvesztése vagy sikertelen toocomply szabályozási és jogi felderítési követelmények. A legtöbb szervezet egyértelműen meghatározott adatmegőrzési házirend nem rendelkező általában toouse "mindent megtartása" alapértelmezett megőrzési házirend. Adatmegőrzési szabály azonban további kockázatok rendelkezik felhőalapú szolgáltatások forgatókönyvekben. 

Például az adatmegőrzési házirend felhőszolgáltatóknál tekinthető mint a "hello időtartam hello előfizetés", (feltéve, hello szolgáltatás fizetnek az hello adatok őrződnek meg). Ilyen egy fizetési a megőrzési megállapodás előfordulhat, hogy a vállalati vagy jogszabályi adatmegőrzési nem ad meg. A bizalmas adatok házirend meghatározása bizonyosodjon meg arról, hogy adatokat tárolja, és eltávolítja az ajánlott eljárások alapján. Emellett az archiválási házirend hozhatók létre tooformalize kapcsolatos adatok megértéséhez érdemes távolítható el, és mikor. 

Az adatmegőrzési házirend eleget kell tennie hello szükséges szabályozási és megfelelőségi követelményeket, valamint vállalati jogi adatmegőrzési követelmények. Bizalmas adatok kiváltó előfordulhat, hogy a megőrzési időtartam és a tárolt adatok szolgáltatóhoz; kapcsolatos kérdések ilyen kérdések valószínűbb, az adatok nem sorolt megfelelően. 

> [!TIP]
> Azure Data adatmegőrzési és több hello beolvasásával kapcsolatos további [Microsoft Online előfizetői szerződés](https://azure.microsoft.com/support/legal/subscription-agreement/)
> 
> 

## <a name="protecting-confidential-data"></a>Bizalmas adatok védelmének
Után az adatok besorolásának, keresése és végrehajtási módon tooprotect bizalmas adatok válik az adatok védelme telepítési stratégia szerves részét. Bizalmas adatok védelmének kell további figyelmet toohow adatok tárolásának és továbbított adatok köre hello felhő mint hagyományos architektúrákban is. 

Ez a témakör alapvető adatait, amely a automatizálhat végrehajtó egyes technológiák toohelp sorolt bizalmas adatok védelmét. 

Hello a következő ábra azt mutatja be, mert ezek a technológiák telepíthető központilag a helyszíni vagy felhőalapú megoldásokhoz – vagy hibrid módon, az egyes őket központilag telepített a helyszíni és néhány hello felhőben. (Egyes technológiák, például a titkosítás és tartalomvédelem, is kiterjeszthető toouser eszközöket.)  

![Technológiák](./media/azure-security-data-classification/azure-security-data-classification-fig4.png)

### <a name="rights-management-software"></a>Rights management szoftver
Megakadályozza az adatvesztést egy megoldást egy rights management szoftver. Ellentétben, amelyek toointerrupt hello információáramlás kilépési időpontokban, a szervezeten belüli megoldások rights management szoftver az adatok tárolási technológiákat belül mély szintű működik. Dokumentumok titkosított, és szabályozhatják, akik visszafejthetik őket használ a hitelesítési vezérlő megoldás például egy olyan meghatározott hozzáférés-vezérlést.  

> [!TIP]
> használhatja az Azure Rights Management (Azure RMS) adatként hello információk védelme megoldás tooprotect különböző helyzetekben. Olvasási [Mi az az Azure Rights Management?](https://docs.microsoft.com/rights-management/understand-explore/what-is-azure-rms) további információt a Azure megoldás.
> 
> 

Rights management szoftver hello előnyei a következők: 

* Biztonságos bizalmas adatokat. Felhasználók védheti az adatokat közvetlenül a rights management-kompatibilis alkalmazások használatával. További lépésekre nincs szükség – szerzői dokumentumok, e-mailek küldéséhez, és adatokat közzé kínálnak a azonos adatokat védelme érdekében. 
* Védelmi halad hello adatokkal. A vásárlók feletti hello felhő, meglévő informatikai infrastruktúrát, a vagy hello felhasználói asztali access tootheir adatokat, aki rendelkezik. A szervezetek választhat tooencrypt adataikat, és korlátozza a hozzáférést tootheir üzleti követelményeinek megfelelően. 
* Alapértelmezett adatvédelmi házirend. A rendszergazdák és felhasználók használhatják szabványos házirendeket számos elterjedt üzleti forgatókönyvek, például a "Vállalati bizalmas – csak Olvasás", és a "Ne továbbítsa." Használati jogok széles skáláját támogatottak, például az olvasási, másolási, nyomtatási, mentési, szerkesztése és határidős tooallow rugalmasságot biztosít az egyéni használati jogosultságok meghatározása. 

> [!TIP]
> az Azure Storage adatainak védelemmel való használatával [Azure Storage szolgáltatás titkosítási](../storage/storage-service-encryption.md) inaktív adatokat. Is [Azure Disk Encryption](azure-security-disk-encryption.md) toohelp használt Azure virtuális gépek virtuális lemezeken található adatok védelme.
> 
> 

### <a name="encryption-gateways"></a>Tartalomtitkosító átjárók
Tartalomtitkosító átjárók saját rétegek tooprovide titkosítási szolgáltatások összes access toocloud-alapú adatok átirányításához fog működni. Ez a megközelítés a kell nem keverendő össze a virtuális magánhálózatok (VPN), amely a. Titkosítási átjárók jellemzően egy átlátszó a tervezett tooprovide réteg toocloud-megoldásokat.   

Tartalomtitkosító átjárók egy azt jelenti, hogy toomanage és hello adatok az átvitel során, továbbá az inaktív adatok titkosítása a bizalmasnak minősített biztonságos adatokat biztosít.  

Tartalomtitkosító átjárók bekerülnek hello adatfolyam a felhasználói eszközök között, és az alkalmazás az adatközpontok tooprovide titkosítási/visszafejtési szolgáltatások. Ezek a megoldások, például VPN, amelyek túlnyomórészt a helyszíni megoldásokkal. Azok a tervezett tooprovide ellenőrzése alatt tartja a titkosítási kulcsokat, amely segít mind hello és egy szolgáltatóhoz kulcskezelés várakoztatásának hello kockázatok csökkentése a harmadik fél. Az ilyen megoldások készültek, hasonlóan a titkosítás, toowork zökkenőmentesen és transzparens módon felhasználók és hello szolgáltatás között. 

> [!TIP]
> használható Azure ExpressRoute tooextend a helyszíni hálózatokhoz hello Microsoft felhőbe egy dedikált titkos kapcsolaton keresztül. Olvasási [ExpressRoute műszaki áttekintés](../expressroute/expressroute-introduction.md) ezzel a funkcióval kapcsolatos további információk. Egy másik beállítások közötti kapcsolatot nyújthassanak a helyszíni hálózat között, és [Azure a telephelyek közötti VPN a](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md).
> 
> 

### <a name="data-loss-prevention"></a>Adatveszteség-megelőzési
Adatvesztés (néha hivatkozott tooas adatszivárgás) a következő fontos szempont, és véletlen és rosszindulatú belső támadásoktól keresztül külső adatvesztés megelőzése hello kiemelkedő a legtöbb szervezet számára.  

Adatok adatvesztés-megelőzési (DLP) technológiák segítségével, győződjön meg arról, hogy megoldások, például az e-mailek szolgáltatásokban nincs Küldés sorolt bizalmas adatokat. A szervezetek kihasználhatják a meglévő DLP szolgáltatásairól toohelp adatvesztés elkerülése érdekében. Ezek a funkciók házirendekkel egyszerűen létrehozhatja nulláról vagy hello szoftver szolgáltató által adott sablon használatával.  

DLP technológiák hajthat végre kulcsszó-megfeleltetéssel, a szótárbejegyzések megfeleltetésével, a reguláris kifejezések kiértékelésével és az egyéb tartalomvizsgálati eljárásokkal toodetect tartalmat, amely megsérti a szervezeti DLP-házirendek részletes tartalomelemzést. Például DLP segítségével a következő típusú adatok hello hello az adatvesztés elkerülése érdekében: 

* Társadalombiztosítási és nemzeti azonosító szám 
* Banki adatok 
* Hitelkártyaszámok  
* IP-címek 

Egyes DLP technológiák hello képességét toooverride hello DLP konfigurációja is megadható, (Ha például egy szervezetnek támogatnia kell a tootransmit társadalombiztosítási szám információk tooa Bérlista processzor). Ezenkívül akkor lehetséges tooconfigure DLP, hogy értesítse a felhasználókat ahhoz, azok még akkor is megkísérlik toosend bizalmas adatokat, amelyeket nem kell továbbítani. 

> [!TIP]
> Office 365 DLP képességek tooprotect használhatja a dokumentumokat. Olvasási [Office 365 megfelelőségi ellenőrzések: adatveszteség-megelőzési](https://blogs.office.com/2013/10/28/office-365-compliance-controls-data-loss-prevention/) további információt.
> 
> 

## <a name="see-also"></a>Lásd még:
* [Az Azure Data Encryption gyakorlati tanácsok](azure-security-data-encryption-best-practices.md)
* [Az Azure Identitáskezelés és hozzáférés szabályozása ajánlott biztonsági eljárások](azure-security-identity-management-best-practices.md)
* [Az Azure Security csapat blogja](http://blogs.msdn.com/b/azuresecurity/)
* [Microsoft Security Response Center](https://technet.microsoft.com/library/dn440717.aspx)

