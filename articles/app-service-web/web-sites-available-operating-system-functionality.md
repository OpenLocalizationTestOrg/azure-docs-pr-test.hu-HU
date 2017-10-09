---
title: "Azure App Service aaaOperating rendszer funkciói"
description: "Hello OS funkció elérhető tooweb alkalmazások, a mobil-háttéralkalmazások és az Azure App Service API apps megismerése"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: 39d5514f-0139-453a-b52e-4a1c06d8d914
ms.service: app-service
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/01/2016
ms.author: cephalin
ms.openlocfilehash: 695188c48102b10ece326fba8d50a5f55b03b003
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="operating-system-functionality-on-azure-app-service"></a>Operációs rendszer működőképességét Azure App Service
Ez a cikk ismerteti a hello közös alapvető operációs rendszer funkció, amely elérhető tooall futó alkalmazások [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714). Ez a funkció tartalmazza a fájl, hálózati, és a beállításjegyzék elérése és diagnosztikai naplók és események. 

<a id="tiers"></a>

## <a name="app-service-plan-tiers"></a>Az alkalmazásszolgáltatási csomag rétegek
App Service alkalmazások vevői futtat egy több-bérlős üzemeltetési környezet. Hello a telepített alkalmazások **szabad** és **megosztott** rétegek munkavégző folyamatok futtatása a megosztott virtuális gépeken, miközben hello a telepített alkalmazások **szabványos** és  **Prémium szintű** rétegek futtassa a kifejezetten a hello alkalmazásokkal kapcsolatos egy dedikált virtuális gépek.

Mivel az App Service támogatja különböző rétegek közötti méretezési gördülékenyen, hello biztonsági konfiguráció kényszerített az App Service apps marad hello azonos. Ez biztosítja, hogy alkalmazásokat nem hirtelen eltérően viselkednek, nem várt módon sikertelenek lesznek, amikor az App Service-csomag egyrétegű tooanother vált.

<a id="developmentframeworks"></a>

## <a name="development-frameworks"></a>Fejlesztési keretrendszerek
App Service díjszabás rétegek vezérlő hello mennyisége számítási erőforrások (CPU, lemezterület, memória és a kimenő hálózati forgalom) elérhető tooapps. Hello mélysége keretrendszer funkció elérhető tooapps azonban azonos, függetlenül attól, hogy hello rétegek skálázás hello marad.

App Service számos fejlesztési keretrendszerek, beleértve az ASP.NET, a klasszikus ASP, node.js, PHP és Python -, amelyek mindegyikén kiterjesztéseket IIS belül. A sorrend toosimplify és biztonsági konfigurációs optimalizálására, App Service-alkalmazásokhoz általában akkor futnak a hello különböző fejlesztési keretrendszerek az alapértelmezett beállításokkal. Egyik módszer tooconfiguring alkalmazások lehetett volna, toocustomize hello API felület és minden egyes fejlesztési keretrendszer funkciót. App Service egy általánosabbá megközelítés helyette azáltal, hogy az operációs rendszer működőképességét, függetlenül az alkalmazás-fejlesztési keretrendszer közös alapterv vesz igénybe.

hello következő szakaszok összegzik hello általános típusú operációs rendszer funkció elérhető tooApp Service-alkalmazásokhoz.

<a id="FileAccess"></a>

## <a name="file-access"></a>Fájlhozzáférés
Különböző meghajtó belül App Service, beleértve a helyi és hálózati meghajtókat.

<a id="LocalDrives"></a>

### <a name="local-drives"></a>Helyi meghajtók
A mag az App Service hello Azure PaaS (platformok) infrastruktúrán futó szolgáltatásban. Hello helyi meghajtókat, amelyek következtében "hozzá van kapcsolva" tooa virtuális gép hello azonos meghajtó típusok elérhető tooany feldolgozói szerepkör futtatja az Azure-ban. Ez magában foglalja egy rendszermeghajtón (hello D:\ meghajtóra), egy alkalmazás kizárólag az App Service (és nem érhető el toocustomers) által használt Azure-csomag cspkg fájlokat tartalmazó meghajtót és a "user" meghajtó (hello C:\ meghajtóra), amelynek mérete változó hello méretétől függően a virtuális gép hello.

<a id="NetworkDrives"></a>

### <a name="network-drives-aka-unc-shares"></a>Hálózati meghajtók (más néven UNC közös)
App Service-alkalmazás telepítési és karbantartási egyszerű teszi egyedi aspektusainak hello egyike, hogy minden felhasználó tartalom UNC-megosztásokhoz a megfelelő tárolja. Ez a modell maps nagyon szépen toohello megszokott mintát követi a tartalom tárolási helyszíni webszolgáltatási környezetekben, melyekben még több terhelésű kiszolgálókat használja. 

App Service-ben belül vannak száma minden adatközpont létrehozott UNC-megosztásokhoz. Hello felhasználói tartalom az ügyfelek minden egyes adatközpont százalékaként UNC-megosztásnevet tooeach van lefoglalva. Továbbá minden egyetlen ügyfelek előfizetésének hello fájl tartalma mindig helyezkedik el ugyanazon UNC-megosztásnevet hello. 

Vegye figyelembe, hogy toohow felhőszolgáltatások miatt működik, a hello meghatározott virtuálisgép üzemeltetési UNC-megosztásnevet felelős időbeli változik. A garantált, hogy különböző virtuális gépek által UNC-megosztásokhoz lesz csatlakoztatva, azok során hello felhőműveletek normális működés során felfelé és lefelé kerülnek. Emiatt alkalmazásokat kell soha ne, hogy egy UNC elérési útja hello gép adatainak marad stabil időbeli kódolt feltételezéseket. Helyette használják inkább a kényelmes hello *ál* abszolút elérési út **D:\home\site** , amely az App Service biztosít. Az ál abszolút elérési út lehetővé teszi a hordozható, alkalmazás-és-felhasználó-független tooone saját alkalmazás hivatkozik. A **D:\home\site**, egy át lehet vinni megosztott fájlok app tooapp anélkül, hogy tooconfigure új abszolút elérési út minden.

<a id="TypesOfFileAccess"></a>

### <a name="types-of-file-access-granted-tooan-app"></a>Típusú fájl hozzáférést kap tooan alkalmazás
Minden ügyfél-előfizetést egy fenntartott directory struktúrája adatközponton belül meghatározott UNC-megosztáson található. Ügyfél lehet egy adott adatközpont belül létrehozott több alkalmazás, így az összes tooa egyetlen ügyfél-előfizetést tartozó hello könyvtár jönnek létre az hello azonos UNC közös. hello megosztás könyvtárak, például a tartalmat, a hiba és a diagnosztikai naplók és a hello alkalmazás hozta létre a verziókövetési rendszerrel korábbi verzióinak is tartalmazhat. Elvárás ügyfél app könyvtárak érhetők el az olvasási és írási hozzáférés hello app alkalmazást kód futási időben.

Hello a helyi meghajtókra csatolt toohello rendszert futtató virtuális gépet egy alkalmazást, az App Service fenntartja a rendszer területet hello C:\ meghajtó alkalmazásspecifikus ideiglenes helyi tárolására. Habár egy alkalmazás tooits saját ideiglenes teljes olvasási/írási hozzáférést a helyi tároló, tárolási valóban nem tervezett toobe közvetlenül hello alkalmazáskód használja. Ehelyett hello célja tooprovide ideiglenes fájlok tárolására az IIS és a webes alkalmazás-keretrendszerek számára. App Service is ideiglenes helyi tárhely rendelkezésre tooeach app tooprevent egyes alkalmazások ne eméssze fel a helyi fájl tárolási túl nagy mennyiségű hello mennyisége korlátozza.

Hogyan használja az App Service a helyi ideiglenes tárolási két példa hello könyvtár az ideiglenes ASP.NET-fájlok és az IIS hello directory tömörített fájlok. ASP.NET-fordítási rendszer hello hello "Temporary ASP.NET Files" könyvtárban használja, mint egy ideiglenes fordítási gyorsítótár helyét. Az IIS hello "IIS ideiglenes tömörített fájlok" directory tömörített toostore válasz kimenetének használ. Újra mindkét említett típusú fájl használati (valamint más) társítva, az App Service tooper alkalmazáson belüli ideiglenes helyi tárterület. Az újbóli hozzárendelés biztosítja, hogy funkció továbbra is a várt módon.

Minden alkalmazás az App Service futtatható, mint egy véletlenszerű egyedi alacsony jogosultsági szintű munkavégző folyamat identitását nevű hello "alkalmazáskészlet-identitása", itt további leírt: [http://www.iis.net/learn/manage/configuring-security/application-pool-identities ](http://www.iis.net/learn/manage/configuring-security/application-pool-identities). Alkalmazáskód ezzel az identitással alapvető csak olvasási hozzáféréssel toohello operációs rendszer meghajtójának (hello D:\ meghajtóra) használja. Ez azt jelenti, hogy alkalmazáskód közös könyvtárstruktúrák listában, és az operációs rendszer meghajtóján közös fájlok olvasását. Bár ez előfordulhat, hogy hozzáférési némileg széles körben toobe jelenik meg, hello ugyanazon könyvtárak és fájlok érhetők el egy Azure feldolgozói szerepkörök kiépítése amikor üzemeltetett szolgáltatás és hello meghajtó tartalmának olvasása. 

<a name="multipleinstances"></a>

### <a name="file-access-across-multiple-instances"></a>Fájlhozzáférés több példánya között
hello kezdőkönyvtár tartalma egy alkalmazást, és alkalmazáskód tooit írhat. Ha egy alkalmazás több példánya fut, hello kezdőkönyvtár között megosztott összes példányát, hogy minden példány hello ugyanabban a könyvtárban. Így például ha egy alkalmazás feltöltött fájlok toohello kezdőkönyvtár menti, ezeket a fájlokat olyan azonnal tooall példányai. 

<a id="NetworkAccess"></a>

## <a name="network-access"></a>Hálózati hozzáférés
Alkalmazáskód TCP/IP és a kimenő UDP-alapú protokollok toomake hálózati kapcsolatok tooInternet elérhető végpontok külső szolgáltatások visszaállítását. Alkalmazások használhatják ezeket azonos protokollok tooconnect tooservices Azure & #151 belül, például HTTPS-kapcsolatok tooSQL adatbázis létrehozásával.

Szerepel továbbá egy korlátozott képesség alkalmazások tooestablish egyik helyi visszacsatolási kapcsolat, és figyelni, hogy helyi visszacsatolási szoftvercsatornán egy alkalmazás. Ez a szolgáltatás létezik-e elsősorban tooenable-alkalmazásokat, amelyek a helyi visszacsatolási sockets működés részeként figyelik. Vegye figyelembe, hogy egyes alkalmazások látja-e a "privát" visszacsatolási kapcsolat; "A" App "B" alkalmazás által létrehozott tooa helyi visszacsatolási szoftvercsatorna nem lehet figyelni.

A nevesített csöveket is támogatottak, amelyek együttesen az IOS-alkalmazásokat futtatnak, különböző folyamatok közötti folyamatok közti kommunikációja (IPC) módszerként. Például hello IIS FastCGI-modul támaszkodik a nevesített csöveket toocoordinate hello egyes futtatott folyamatokat PHP lapok.

<a id="Code"></a>

## <a name="code-execution-processes-and-memory"></a>Programkód, folyamatok és memória
Ahogy azt korábban említettük, alkalmazások egy véletlenszerű alkalmazáskészlet-identitás használatával alacsony jogosultsági szintű munkavégző folyamatok belül fut. Alkalmazáskód hozzáférés toohello memóriaterületet társított hello munkavégző folyamat, valamint a gyermek folyamatokat is lehet indított CGI folyamatok és más alkalmazásokkal rendelkezik. Azonban egy alkalmazás nem fér hozzá a hello memória, vagy egy másik alkalmazás akkor is, ha az adatok hello ugyanahhoz a virtuális géphez.

Alkalmazások parancsfájlokat, illetve a támogatott webes fejlesztési keretrendszerek írt lapok futtathatók. App Service bármilyen webes keretrendszer beállítások korlátozott toomore módok nem konfigurálja. Például App Service futó ASP.NET-alkalmazások futtatása "teljes" megbízhatósági szinten megakadályozását tooa további korlátozott megbízhatóság módban. Webes keretrendszerek, beleértve a klasszikus ASP és az ASP.NET, a folyamaton belüli COM-komponensek (de nem kívüli folyamat COM-komponensek) például ADO (ActiveX Data Objects) alapértelmezés szerint hello Windows operációs rendszerben regisztrált elő tudják hívni.

Alkalmazások elindítanak és futtathatják. Akkor engedélyezett, az alkalmazás toodo dolgot hasonló elindítanak egy parancshéj vagy egy PowerShell-parancsprogrammal. Azonban annak ellenére, hogy tetszőleges kódot és a folyamatok tud indított az alkalmazásokból, a végrehajtható programokat és parancsfájlokat pedig továbbra is korlátozott toohello jogosultságokkal rendelkeznek toohello szülő alkalmazáskészletet. Például egy alkalmazás is elindítanak egy végrehajtható fájl, amely lehetővé teszi egy kimenő HTTP-hívás, azonban, hogy az azonos végrehajtható nem próbálja meg toounbind hello IP-címet a virtuális gépek létrehozását a hálózati adaptert. Egy kimenő hálózati hívás engedélyezett toolow szintű jogosultságokat igénylő kódokat, de a virtuális gép hálózati beállításai tooreconfigure kísérlet rendszergazdai jogosultságokra van szüksége.

<a id="Diagnostics"></a>

## <a name="diagnostics-logs-and-events"></a>Diagnosztikai naplók és események
Naplózási adatok egy másik, hogy egyes alkalmazások kísérlet tooaccess adatok. napló információ elérhető toocode az App Service-ben futó hello típusú diagnosztikai tartalmazza, és egy alkalmazás, amely egyúttal könnyen hozzáférhető toohello alkalmazás által generált adatok naplózására. 

Például a W3C HTTP egy aktív alkalmazás által generált feldolgozásra elérhető hello hálózat naplókönyvtár vagy megosztásának helyét létre hello alkalmazáshoz, vagy elérhető, a blob Storage tárolóban, ha az ügyfél által beállított W3C-naplózás toostorage. hello ez utóbbi lehetőség lehetővé teszi, hogy a nagy mennyiségű meghaladó hello fájl tárolási egy hálózati megosztásra társított hello kockázata nélkül gyűjtött naplók toobe.

Egy hasonló tekintettel a .NET-alkalmazások adatait is bejelentkezhet hello .NET nyomkövetés és diagnosztika infrastruktúra, a beállítások toowrite használatával valós idejű diagnosztika hello nyomkövetési adatokat tooeither hello alkalmazás hálózati megosztásra, vagy másik lehetőségként tooa blob-tároló hely.

Diagnosztikai naplózás és nyomkövetés, amely nem érhető el tooapps területek a következők: Windows ETW-események és a gyakori Windows-Eseménynapló (pl. rendszer, az alkalmazás és a biztonság eseménynaplók). Mivel az ETW-nyomkövetési adatokat is lehet megtekinthető gépre kiterjedő (hello jogosultsággal rendelkező ACL-ek), olvasási és írási hozzáférés tooETW események le vannak tiltva. A fejlesztők észrevette, hogy adott API-hívások tooread és írási ETW-esemény és közös Windows-Eseménynapló toowork jelenik meg, de ez mert App Service van "faking" hello hívások toosucceed megjelenítését. A valóságban hello alkalmazás kódja nem tartalmaz hozzáférés toothis esemény adatokat.

<a id="RegistryAccess"></a>

## <a name="registry-access"></a>Beállításjegyzék elérésének
Az alkalmazások rendelkezzenek olvasási hozzáférést toomuch (azonban nem mindegyik) hello beállításjegyzék hello virtuális gép a futnak. A gyakorlatban ez azt jelenti, amelyek lehetővé teszik az olvasási hozzáférést toohello helyi felhasználók csoport beállításkulcsok érhetők el alkalmazások. Hello beállításjegyzék olvasási vagy írási hozzáférése jelenleg nem támogatott egy részén hello HKEY\_aktuális\_felhasználói struktúra.

Írási hozzáféréssel toohello beállításjegyzék le van tiltva, beleértve a hozzáférési tooany felhasználói beállításkulcsok. Hello alkalmazás szempontjából, írási hozzáférést toohello beállításjegyzék kell soha nem lehet hivatkozni a hello Azure környezetben óta alkalmazások (illetve használhat) települnek át a különböző virtuális gépek között. hello csak állandó írható tároló, amely az alkalmazás által kell függött hello alkalmazáson belüli tartalom könyvtárstruktúrát közösen használja az App Service UNC hello tárolt. 

## <a name="more-information"></a>További információ

[Az Azure Web App védőfal](https://github.com/projectkudu/kudu/wiki/Azure-Web-App-sandbox) -hello hello végrehajtási környezet az App Service a legtöbb naprakész információkat. Ezen a lapon közvetlenül hello App Service fejlesztői csapat tartja fenn.

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 


