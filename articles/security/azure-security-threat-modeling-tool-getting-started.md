---
title: "-Microsoft fenyegetések modellezése eszköz - elindítva Azure aaaGetting |} Microsoft Docs"
description: "Ez a művelet a fenyegetések modellezése eszköz hello kiemelés egy mélyebb áttekintése."
services: security
documentationcenter: na
author: RodSan
manager: RodSan
editor: RodSan
ms.assetid: na
ms.service: security
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/17/2017
ms.author: rodsan
ms.openlocfilehash: 75ef139071e8abd0e743aa17b443a6e353f29372
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-hello-threat-modeling-tool"></a>Ismerkedés a fenyegetések modellezése eszköz hello

hello felhő- és eszközök a vállalati biztonsági csoport, amely hello fenyegetések modellezése eszköz Preview a korábban az év egy szabadként  **[kattintson-letöltési](https://aka.ms/tmtpreview)**. kézbesítési mechanizmusként hello változás lehetővé teszi legújabb-es számú toopush hello fejlesztései és hibajavítások rendszerhez készült toocustomers hello eszközt, hogy könnyebben toomaintain és használatát, így minden egyes megnyitásakor.
Ez a cikk végigvezeti a bevezetés hello Microsoft SDL veszéllyel megközelítés modellezési hello folyamat, és bemutatja, hogyan toouse hello eszköz toodevelop kiváló fenyegetés, a biztonsági folyamat egy gerincét modellek.

Ez a cikk épít hello SDL fenyegetések modellezése megközelítés meglévő ismerete. A gyors áttekintését lásd túl**[fenyegetések modellezése webalkalmazások](https://msdn.microsoft.com/library/ms978516.aspx)**  és archivált verziója  **[felderíthesse biztonsági hiányosságokat használatával hello STRIDE megközelítés](https://docs.google.com/viewer?a=v&pid=sites&srcid=ZGVmYXVsdGRvbWFpbnxzZWN1cmVwcm9ncmFtbWluZ3xneDo0MTY1MmM0ZDI0ZjQ4ZDMy)**  MSDN-cikk 2006 közzétéve.

tooquickly összefoglalója, hello megközelítés diagram létrehozása, fenyegetések azonosítására, őket kiküszöböléséhez és ellenőrzése minden megoldás. Ez a diagram, amely kiemeli ezt a folyamatot:

![SDL-folyamat](./media/azure-security-threat-modeling-tool/sdlapproach.png)

## <a name="starting-hello-threat-modeling-process"></a>Hello fenyegetések modellezése folyamat elindítása

Amikor hello fenyegetések modellezése eszköz elindításához néhány dolog, észrevehette, hello képen látható módon:

![Üres Start lap](./media/azure-security-threat-modeling-tool/tmtstart.png)

### <a name="threat-model-section"></a>Fenyegetés modell szakasz

| Összetevő                                   | Részletek                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |
| ------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Visszajelzés, a javaslatok és a problémák gomb** | Ön hello vesz  **[MSDN fórum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=sdlprocess)**  összes dolgot SDL. Biztosít egy lehetőség tooread keresztül más felhasználók tevékenységeit, és a lehetséges megoldások és javaslatokat. Ha még mindig nem talál meg, amit keres, e-mailben tmtextsupport@microsoft.com a támogatási csapat toohelp a,                                                                                                                            |
| **A modell létrehozása**                          | Megnyílik az Ön toodraw üres vászonból a diagram. Győződjön meg arról, hogy tooselect melyik sablont toouse szeretné a modell                                                                                                                                                                                                                                                                                                                                                                       |
| **Új modell sablon**                 | Mely sablon toouse a modell létrehozása előtt ki kell választania. A fő sablon hello Azure fenyegetés folyamatmodell-sablont, amely tartalmazza az Azure-rajzsablonok, fenyegetések és azok mérséklési. Általános esetében válassza ki az SDL TM Tudásbázis hello hello legördülő menüből. Szeretné toocreate saját sablont, vagy küldje el az összes felhasználó számára egy új? Tekintse meg a  **[sablon tárház](https://github.com/Microsoft/threat-modeling-templates)**  GitHub-oldalon toolearn további                              |
| **Nyissa meg a modell**                            | <p>Megnyílik a fenyegetés modellek korábban mentett. hello modellek nemrég megnyitott szolgáltatása nagyszerű, ha a legfrissebb fájlokat kell tooopen. Ha hello kijelölés mutat, találhat 2 módon tooopen modellek:</p><p><ul><li>Nyissa meg a számítógépről – klasszikus mód a helyi tárhelyet használ a fájlok megnyitása</li><li>Nyissa meg a OneDrive – csapatok használhatja a onedrive vállalati verzió toosave mappák és a fenyegetés modellek megosztani egy egyetlen helyen toohelp növekedése termelékenység és együttműködés</li></ul></p> |
| **Első lépések útmutató**                   | Megnyílik hello  **[Microsoft fenyegetések modellezése eszköz](./azure-security-threat-modeling-tool.md)**  főoldala                                                                                                                                                                                                                                                                                                                                                                                            |

### <a name="template-section"></a>Sablon szakasz

| Összetevő               | Részletek                                                                                                                                                          |
| ----------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Új sablon létrehozása** | Megnyílik egy üres sablont, toobuild az. Ha nincs teljesen új sablonok létrehozásakor a széles körű Tudásbázis, javasoljuk, hogy a már meglévőket toobuild |
| **Nyissa meg sablon**       | Megnyitja a sablonok meglévő meg toomake módosítások túl                                                                                                             |

hello fenyegetések modellezése eszköz team folyamatosan működik-e tooimprove eszköz funkciókat és a felhasználói élmény. Néhány másodlagos változtatások hello év folyamán hello előfordulhat, hogy kerül sor, de minden fontosabb változását igénylő újraírások hello útmutatóban. Tekintse meg a tooit gyakran tooensure hello legújabb közlemények kap.

## <a name="building-a-model"></a>A modell létrehozása

Ez a szakasz azt kövesse:

- Cristina (egy fejlesztő)
- Richárd (Programvezető) és
- Szerzője Ashish (tester)

Az első fenyegetések modellezése fejlődő hello folyamatot a rendszer hamarosan.

> Richárd: Nagy Cristina, I hello fenyegetés modell diagram dolgozott, és kívánta toomake meg arról, hogy a Microsoft azt hello részletek jobb. Is segítséget kérek, tekintse át?
> Cristina: elengedhetetlen. Ismerkedjen meg.
> Richárd hello eszköz megnyitása, és a képernyő Cristina megosztja.

![Alapszintű fenyegetések modellezése](./media/azure-security-threat-modeling-tool/basictmt.png)

> Cristina: Ok, egyszerű, keres, de az is, bízná me azt?
> Richárd: meg arról, hogy! Hello részletes információkat a következő:
> - Az emberi felhasználói megrajzolása egy külső egységként – négyzet
> - Ezek a parancsok tooour webkiszolgáló küldjük – hello kör
> - hello webkiszolgálón van tanácsadás egy adatbázist (két párhuzamos sorok)

Mi Richárd csak bemutatta Cristina DFD, rövid a  **[adatfolyam-Diagram](https://en.wikipedia.org/wiki/Data_flow_diagram)**. hello fenyegetések modellezése eszköz lehetővé teszi, hogy a felhasználók toospecify bizalmi kapcsolat határain, piros hello pontozott vonalak vezérlő belül hol áll különféle entitásokat tooshow jelölik. Például a rendszergazdák megkövetelése az Active Directory rendszer hitelesítési célokra, az Active Directory hello hozzáférésének szabályozására kívül esik.

> Cristina: Jobb toome keres. Mi a helyzet hello fenyegetések?
> Richárd: Én is láthat.

## <a name="analyzing-threats"></a>Fenyegetések elemzése

Amennyiben az elemre kattint a hello elemző nézetet hello ikon a menüpont kiválasztásának (fájl nagyítóüveg), tooa listája létrehozott fenyegetések hello található fenyegetések modellezése eszköz hello alapértelmezett sablon, alapján vették hello SDL megközelítés nevű használó  **[ STRIDE (IP-hamisítás, illetéktelen módosítás, információ felfedése, szolgáltatásmegtagadás és jogok kiterjesztése)](https://en.wikipedia.org/wiki/STRIDE_(security))**. hello lényege, hogy a szoftver a fenyegetéseket, amely 6 kategóriákkal is található egy előre jelezhető készletét származik.

Erre akkor van például biztonságossá tétele a ház minden ajtók és biztosításával rendelkezik zárolási mechanizmus az riasztó rendszerek hozzáadása vagy hello tolvaj után követésnek előtt.

![Alapszintű fenyegetések](./media/azure-security-threat-modeling-tool/basicthreats.png)

Richárd hello hello lista első elemét kiválasztásával kezdődik. Ez történik:

Első lépésként hello két rajzsablonok hello egymásra továbbfejlesztett

![Interakció](./media/azure-security-threat-modeling-tool/interaction.png)

Hello fenyegetés második, további információkat hello fenyegetés tulajdonságai ablakban jelenik meg

![Interakció adatai](./media/azure-security-threat-modeling-tool/interactioninfo.png)

generált hello fenyegetés segít neki a lehetséges tervezési hibái ismertetése. hello STRIDE kategorizálási ad neki egy a potenciális támadási, miközben hello további leírás amelyből megtudja, pontosan mit rendelkezik helytelen, és lehetséges módokon toomitigate azt. Ezután használhatja szerkeszthető mezők toowrite megjegyzések hello indoklás részleteit, vagy attól függően, hogy a szervezet hiba sáv minősítés módosítása.

Azure-sablonok vannak további részleteket toohelp felhasználói, nem csak rendszer nem megfelelő, de még hogyan toofix, adja hozzá a leírásokat, példák és hivatkozások tooAzure vonatkozó dokumentációt.

hello leírás miatt hello fontosnak tartja, olyan hitelesítési mechanizmus tooprevent felhasználók felvételéről hamisíthatók, amelyek hello első fenyegetés toobe dolgozott. Néhány percig, amíg be hello vitafórum a Cristina, azok megértettem hello fontosak a végrehajtási hozzáférés-vezérléshez és a szerepkörök. Richárd néhány gyors megjegyzések toomake meg arról, hogy ezek bevezetett kitöltve.

Richárd került, hello fenyegetések az információk felfedése, mivel azt is tudjuk hello hozzáférés-vezérlési tervek egyes írásvédett fiókok szükséges naplózási és jelentéskészítés céljából. Ezután már azon, hogy e egy új fenyegetés legyen, de azok mérséklési volt hello hello azonos, ő külön jelezve hello fenyegetést.
Ezután is-re vonatkozó információk felfedése további műveletek, és a is tudjuk, hogy hello mentést tartalmazó szalagok tooneed titkosítási, egy feladatot az hello műveleti csapata volt fog.

Nem alkalmazható toohello tervezési megfelelő tooexisting megoldást vagy biztonsági biztosítja, hogy túl módosítható fenyegetések "Nem alkalmazható" hello állapot legördülő a. Három lehetősége van: nem indult el – alapértelmezett kell vizsgálat – használta toofollow elemek és Mitigated – Miután teljesen előre.

## <a name="reports--sharing"></a>Jelentések és megosztása

Miután Richárd Cristina hello lista végighalad, és hozzáadja a fontos megjegyzések, azok mérséklési/indokok, a prioritás és az állapot változik, he választja ki a Jelentések -> teljes jelentés -> mentse a jelentést, mely egy töltött kinyomtatása jelentés őt toogo keresztül a létrehozása munkatársak tooensure hello megfelelő biztonsági munkahelyi valósul meg.

![Interakció adatai](./media/azure-security-threat-modeling-tool/report.png)

Ha Richárd tooshare hello fájl Ehelyett azt szeretné, hogy egyszerűen megteheti mentése a OneDrive-fiókja a szervezet által. Amennyiben az általa Igen, ezután hello dokumentum hivatkozás másolása, és ossza meg a munkatársaival. 

## <a name="threat-modeling-meetings"></a>Fenyegetés modellezési értekezletek

A fenyegetés modell toohis munkatársát, OneDrive, Ashish, hello tester használatával Richárd elküldésekor underwhelmed volt. Úgy tűnik, például Richárd és Cristina számos fontos esetekben, amelyek könnyen sérülhet kimaradt. A skepticism egy komplemens számnak toothreat modellek.

Ebben a forgatókönyvben követően Ashish átvette hello fenyegetések modellezése, azt hívja meg két fenyegetés modellezési értekezletek esetén: egy értekezlet toosynchronize hello folyamat és a lépésein végighaladva hello diagramok és egy második értekezlet fenyegetés áttekintésre és kijelentkezési.

Hello első értekezlet, Ashish töltött mindenki keresztül hello SDL fenyegetés modellezési folyamatban érdekében 10 perc. Ezután hívja elő a hello fenyegetés modell diagram és részletesen elmagyarázza az elindult. Öt percen belül egy fontos hiányzik egy összetevő azonosították.

Néhány perc múlva újabb Ashish és Richárd került egy bővített leírását a hogyan hello webkiszolgáló lett létrehozva. Nem volt hello ideális egy értekezlet tooproceed, de mindenki előfordulhat, hogy hello ellentmondás korai felderítéséhez folyamatban volt őket ideje a jövőben hello toosave egyeztetett.

A hello második értekezlet, telefonon keresztül hello fenyegetések hello team tárgyalt bizonyos módokon tooaddress őket, és aláírt a fenyegetések modellezése hello ki. Ezek hello dokumentum beadja a verziókövetési rendszerrel, és folytatni a fejlesztési.

## <a name="thinking-about-assets"></a>Eszközök számbavétele

Bizonyos fenyegetések modellezése rendelkező olvasók tapasztalhatja, hogy még nem megtartásról vonatkozó minden. Megismerte, azt, hogy sok szoftverfejlesztő ismerniük eszközök hello fogalmát, milyen eszközöket egy támadó el iránt érdeklődik jobb megértése-e a szoftvereket.

Ha toothreat minta egy házat, előfordulhat, hogy az első a családi, pótolhatatlan fényképeket vagy értékes alkotást számbavétele. Például előfordulhat, hogy megkezdéséhez ki lehet, hogy feltörhessék és hello aktuális biztonsági rendszer továbbléphetnek. Vagy előfordulhat, hogy az első annak eldöntéséhez, hogy a hello fizikai funkciók – például a hello készletet vagy hello első porch. Ezek azok az eszközök, a támadók vagy a szoftver tervezési hasonló toothinking. Ezek a módszerek bármelyikét működik.

Itt azt korábban bemutatott hello megközelítés toothreat modellezési jóval egyszerűbb, mint mi a Microsoft megtörtént az elmúlt hello. Észleltünk, hogy hello szoftver tervezett módszert alkalmaz sok csoportok esetén működik-e. Reméljük, amelyek tartalmazzák a saját.

## <a name="next-steps"></a>Következő lépések

Küldje el kérdéseit, javaslatait és aggályokat tootmtextsupport@microsoft.com. **[Töltse le](https://aka.ms/tmtpreview)**  hello fenyegetések modellezése eszköz tooget elindult.
