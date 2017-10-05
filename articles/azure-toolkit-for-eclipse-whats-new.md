---
title: "What's New in Eclipse Azure eszköztára"
description: "További tudnivalók a legújabb funkciókat az Eclipse Azure eszköztára."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 16b066ea-aae7-4c30-9a12-fa0c3711b93e
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm;asirveda;martinsawicki
ms.openlocfilehash: 71c4f2d2298ea54f18e9ed64b246966b470a4ff4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="whats-new-in-the-azure-toolkit-for-eclipse"></a>What's New in Eclipse Azure eszköztára
## <a name="azure-toolkit-for-eclipse-releases"></a>Eclipse kiadásokban Azure eszköztára
Ez a cikk ismerteti a különböző kiadások és az Azure-eszközkészlet eclipse-ben a legújabb frissítéseket.

> [!NOTE]
> Az IntelliJ IDE egy Azure eszközkészletét is van. További információkért lásd: [IntelliJ Azure eszköztára].
> 
> 

### <a name="april-14-2017"></a>2017. április 14.
Az Azure eszközkészlet az Eclipse - 2017. április kiadásban a következő fejlesztéseket tartalmazza:

* **Továbbfejlesztett Azure bejelentkezési élmény**: az eclipse-ben az Azure-eszközkészlet mostantól támogatja az Azure-fiókjába való bejelentkezés két módszerrel: *interaktív* és *automatikus*. További információkért lásd: [Azure bejelentkezési az utasításokat az Eclipse Azure eszköztára].
* **Közzététel a Docker-tárolók**: most közzéteheti a webalkalmazások Azure eszközkészlet használata az Eclipse Docker tárolóként. További információkért lásd: [webalkalmazás közzététele az Azure-eszközkészlet használata az Eclipse Docker tárolóként].
* **Tárolási fiókkezelés**: az Azure-eszközkészlet az eclipse-ben mostantól támogatja a storage-fiókok kezelése az Azure Explorer nézetből. További információkért lásd: [kezelése Storage-fiókok az Azure-kezelővel használata az Eclipse].
* **Virtuálisgép-kezelő**: az Azure-eszközkészlet az eclipse-ben mostantól támogatja a virtuális gépek kezelése az Azure Explorer nézetből. További információkért lásd: [kezelése használó virtuális gépek az Azure-kezelővel az Eclipse].
* **Távoli hibaelhárítási támogatás eltávolítása**. Java-webalkalmazások Azure App Service távoli hibakeresés el lett távolítva az eclipse-ben; Azure eszköztára Ez volt néhány problémát, amely az ügyfelek volt tapasztalják az eszközkészlet használata esetén feloldásához szükséges.

### <a name="august-26-2016"></a>2016 augusztusától 26
Az Eclipse - kiadás 2016 augusztusától Azure eszköztára a következő fejlesztéseket tartalmazza:

* **Egyéni JDK Terjesztéseket**. Az Azure-eszközkészlet az eclipse-ben mostantól támogatja a megadása, és az Azure-webalkalmazás tárolóhoz egy tetszőleges JDK-verzió telepítése:
  * Az Azure által biztosított JDKs, mellett is választhat Zulu OpenJDK verziók által elérhetővé tett Azure Azul rendszerek széles kijelölés.
  * A saját JDK terjesztési is megadható, ha a feltöltés csomagot .zip fájlként a tárfiókhoz.
* **Az Azure terület Intézőbeli nézetében fejlesztései**:
  * Új Azure Resource Manager-modell használatával a virtuális gép felügyelet támogatása: listában, létrehozhat, és törölje a resource manager-alapú virtuális gépek anélkül, hogy az IDE.
  * A Tárfiók a blob felügyelet kiegészíti a meglévő funkciók "klasszikus" storage-fiókok Azure Resource Manager használatával támogatása.
* **Az SQL Server 6.0 Microsoft JDBC-illesztőt**. A frissítés a Microsoft SQL Server (v6.0), amely magában foglalja a legfrissebb JDBC-illesztőt most része, amely egyszerűen hozzáadhatja a Java-projektekhez tárként, ezáltal a mag cseréje a régebbi verziót.

### <a name="june-29-2016"></a>2016. június 29.
Az Azure eszközkészlet az Eclipse - 2016. június kiadásban a következő fejlesztéseket tartalmazza:

* **Java 8 követelmény**. Az Eclipse Azure eszköztára megköveteli Java 8, bár ez a követelmény csak az eszközkészlet van – az alkalmazások továbbra is használhatja a Java Azure által támogatott összes verzióját.
* **A legújabb Java JDKs támogatása**. A Java JDKs a legfrissebb verziója most Eclipse Azure eszköztára által támogatott.
* **Az Azure SDK v2.9.1 támogatása**. Az Azure SDK legújabb verziójára már az Eclipse Azure eszköztára minimális előzetesen szükséges.
* **Minták integrált**. Az Eclipse Azure eszköztára most funkciókat több minta alkalmazás segítségével a fejlesztők a kezdéshez.
* **A HDInsight eszközzel integrációs**. Azure HDInsight eszközök most vannak becsomagolva Azure eszközkészlete az eclipse-ben. További információkért lásd: [Eclipse HDInsight-eszközei beépülő].
* **Java-webalkalmazások távoli hibakeresés**. Az Azure-eszközkészlet az eclipse-ben mostantól támogatja a Java-webalkalmazások Azure App Service távoli hibakeresés.
* **Az Eclipse Luna kiadás támogatása.** Az új minimális szükséges Eclipse IDE verziója Luna.

### <a name="april-12-2016"></a>2016. április 12.
Az Azure eszközkészlet az Eclipse - 2016. április kiadásban a következő fejlesztéseket tartalmazza:

* **Az Azure SDK v2.9.0 támogatása**. Az Azure SDK legújabb verziójára már az Eclipse Azure eszköztára minimális előzetesen szükséges.
* **Vegyes használhatóság, válaszkészsége és teljesítménnyel kapcsolatos fejlesztések az Azure Web Apps támogatás kapcsolódó**. Hogyan kommunikál a az eszközkészlet Azure eredménye a teljesítményoptimalizálás számos olyan gyorsabban felhasználói felületén.
* **Az Eclipse belül az Azure-ban meglévő webes alkalmazás tároló törlésére**. Az Azure-eszközkészlet az eclipse-ben mostantól lehetővé teszi egy meglévő Azure webes tároló törlése anélkül, hogy eclipse-ben.

### <a name="march-7-2016"></a>2016. március 7.
Az Azure eszközkészlet az Eclipse - 2016. március kiadásban a következő fejlesztéseket tartalmazza:

* **Egyszerűsített Java-alkalmazások gyors üzembe helyezés támogatása**. Eclipse Azure eszköztára mostantól támogatja a gyors telepítését egyszerűsített Java-alkalmazások az Azure Web App tárolók, a, a Java-alkalmazások telepítését most helyett perc másodpercet vesz igénybe.
* **Az Azure-kezelővel nézet használata a webalkalmazás-felügyelet támogatása**. Az Azure terület Intézőbeli nézetében az eszközkészlet a listázása, és leállításáért Azure Web Apps támogatja.
* **Frissített Zulu OpenJDK, Tomcat vagy Jetty terjesztéseket**. Az Eclipse Azure eszköztára támogatást nyújt a frissített verzióit Tomcat vagy Jetty, Zulu OpenJDK Java-telepítések az Azure felhőszolgáltatások.

### <a name="january-4-2016"></a>2016. január 4.
Az Azure eszközkészlet az Eclipse - 2016. január kiadásban a következő fejlesztéseket tartalmazza:

* **A Zulu OpenJDK frissítésekhez**. További információkért lásd: a [Azul rendszerek weboldalra a Zulu OpenJDK].
* **Frissített Tomcat- és Jetty terjesztéseket**. A Jetty és Tomcat terjesztéseket, amelyek elérhetők a Microsoft Azure Azure eszközkészlete az Eclipse frissítve lett.
* **Szolgáltatásparitás az eclipse-ben és az Azure-eszközök IntelliJ gazdag közötti**. Az Azure-eszközkészlet az eclipse-ben és a [IntelliJ Azure eszköztára] most támogatja ugyanazokat a funkciókat.

### <a name="september-1-2015"></a>2015. szeptember 1.
Az Azure eszközkészlet az Eclipse - 2015. szeptember kiadásban a következő fejlesztéseket tartalmazza:

* **A Zulu OpenJDK frissítésekhez**. További információkért lásd: a [Azul rendszerek weboldalra a Zulu OpenJDK].
* **Frissített Tomcat- és Jetty terjesztéseket**. A Jetty és Tomcat terjesztéseket, amelyek elérhetők a Microsoft Azure Azure eszközkészlete az Eclipse frissítve lett. (A ezek terjesztéseket lehetővé teszi a fejlesztők gyors fejlesztési és-projektek az Azure-eszközkészlet az eclipse-ben.
* **Automatikusan frissített Tomcat- és Jetty-hivatkozások támogatását**. Mellett verzióról Tomcat- és Jetty is elérhetők az Azure-on, fejlesztők hivatkozhat egy terjesztési néven a "legújabb (automatikus frissítése)", amely automatikusan frissíti a legújabb terjesztési az egyes főverzióját Jetty vagy a szerepkörpéldányok újrahasznosítására legközelebb Tomcat. (Újrahasznosítás automatikusan, de a fejlesztők manuálisan is elindíthatja az Azure portálon keresztül újrahasznosítást.) Ez új funkció lehetővé teszi, fejlesztőknek nem kell újratelepíteni az alkalmazást, hogy tudja a frissített kiszolgálón szoftverei. (
* Ez a funkció jelenleg csak a fejlesztési és tesztelési célokra vagy nem létfontosságú alkalmazások számára készült, és nem javasolt éles üzemmódhoz.)
* **Azure terület Intézőbeli nézetében a blobok, várólisták és az Azure storage táblák**. Ez lehetővé teszi a fejlesztők számára a tárolási összetevők gyakori feladatokat hajtsák végre közvetlenül a az Eclipse IDE. Például: törlés, vagy feltöltése a BLOB.

### <a name="august-1-2015"></a>2015. augusztus 1.
Az Azure eszközkészlet az Eclipse - 2015. augusztus kiadásban a következő fejlesztéseket tartalmazza:

* **Application Insights Instrumentation kulcskezelés**. A frissítés lehetővé teszi, szerezzen be, létrehozása és az Application Insights instrumentation kulcsok kezelése az Eclipse IDE-ről.
* **Az SQL Server 4.1 Microsoft JDBC-illesztőt**. A frissítés tartalmazza a Microsoft SQL Server a legújabb JDBC-illesztőt támogatását.
* **Az Azure SDK 2.7-es verziójának**. A legutóbbi frissítés az Azure SDK az új előfeltételeként az eszközkészlet Windows rendszeren. (Megjegyzés: Ez nem szükséges a nem Windows operációs rendszereken.)
* **Frissítés a Zulu OpenJDK v7 támogatási**. További információkért lásd: a [Azul rendszerek weboldalra a Zulu OpenJDK].

### <a name="may-1-2015"></a>2015. május 1.
Az Azure eszközkészlet az Eclipse - 2015. május kiadásban a következő fejlesztéseket tartalmazza:

* **A kiválasztott felhasználói felület jobb**. Ebben a kiadásban egyszerűbbé teszi a nem Windows operációs rendszereken az eszközkészlet használata.
* **Maven-projektek támogatása**. Ez a kiadás támogatja a Maven-projektek, alkalmazások, amelyek az Azure-bA az eszközkészlet telepítheti és konfigurálhatja az Application Insights.
* **Az Azure SDK 2.6-os verziójának**. A legutóbbi frissítés az Azure SDK az új előfeltételeként az eszközkészlet Windows rendszeren. (Megjegyzés: Ez nem szükséges a nem Windows operációs rendszereken.)
* **Központi telepítés frissítés helyett ismételt**. Ha az előző verzió már élő ismét közzétett a telepítési projekteket, ha az eszközkészlet most funkcióit használja az Azure telepítési frissítési helyett a korábbi központi telepítés leáll, és teljesen új újbóli közzétételét, mint korábban. Ez lehetővé teszi a felhőalapú szolgáltatás, amikor csak lehetséges – útmutatás nyújtása a magas rendelkezésre állás elérése esetén egy adott frissítés megszakítás nélkül futtatásához, és felgyorsítja a újbóli közzétételt.
* **A legújabb Zulu OpenJDK v8 támogatása – 40 frissítése**. További információkért lásd: a [Azul rendszerek weboldalra a Zulu OpenJDK].

### <a name="march-9-2015"></a>2015. március 9.
Az Azure eszközkészlet az Eclipse - 2015. márciusi kiadásban a következő fejlesztéseket tartalmazza:

* **Támogatja a Mac, Ubuntu és további Linux támadáshoz**. Ebben a kiadásban az eclipse-ben az Azure eszközkészlet támogatást biztosít Mac OS és több Unix rendszerek, így a fejlesztők az eszközkészlet létrehozása, konfigurálása és közzététele a Java-projektek Azure felhőszolgáltatások (PaaS) telepítheti az operációs rendszeren futó Eclipse Windows.

> [!NOTE]
> Ez a funkció jelenleg előzetes verzióban érhető, és a termelési környezetben nem ajánlott. Nincs ügyfél támogatás szolgáltatási szint szerződés (SLA), de a visszajelzéseket értékeljük, és javasolt.
> 
> 

* **Új Application Insights beépülő modul**. A fejlesztők is használja az Application Insights szolgáltatást Azure automatikus server telemetriai konfigurálható.
* **Parancssori telepítsenek-alapú telepítési automation**. Ez a funkció lehetővé teszi a fejlesztők a telepítések telepítsenek kívül eclipse-ben újabb verzióiban a közzétételi automatizálásához. Egy előre generált parancsfájl automatikusan konfigurálva van a projekt után először telepítették az eclipse-ben, és további központi telepítéseket a parancsfájl használatával teljesen automatizálhatja a telepítéseket csak a parancssorból.
* **Tomcat- és Jetty rendelkezésre állásának Azure egyszerűbb és gyorsabb telepítési**. A fejlesztők különböző Tomcat- és Jetty verziókról Azure közvetlen helyette, hogy töltse fel a Java-kiszolgáló a fiókba történő (vagy az eszközkészlet keresztül), így nincs szükség a Java-kiszolgálót gyors, az első lépéseket forgatókönyvek feltöltéséhez hivatkozhat.
* **Java-webalkalmazások közzétételéhez Azure felhőszolgáltatások helyi metódus**. A tanulási görbére egyszerű fejlesztési és tesztelési forgatókönyvek csökkentése érdekében a fejlesztők mostantól több közvetlenül az Azure Java alkalmazások közzététele. Így ahelyett hogy történő létrehozásáról és konfigurálásáról az Azure-telepítés projektben teendője, alkalmazások telepítése a Tomcat v8 és Zulu JVM (OpenJDK) alapértelmezett példányát.

### <a name="january-30-2015"></a>2015. január 30.
Az Azure eszközkészlet az Eclipse - 2015. januári kiadásban a következő fejlesztéseket tartalmazza:

* **IBM® WebSphere® alkalmazás kiszolgálómag Liberty támogatása**. Ebben a kiadásban az IBM WebSphere Application Server Liberty Core ad hozzá a listához, a támogatott alkalmazáskiszolgálók, amelyből az eszközkészlet tudja telepíteni az Azure-bA. A legújabb hozzáadást a támogatott alkalmazáskiszolgálók aktuális listájának kibontása &quot;out-of-az-box&quot; által az eszközkészlet, amely már tartalmazza Tomcat, Jetty, JBoss és GlassFish különböző verzióiban.
* **Az Application Insights SDK felvétel**. Az újonnan kiadott API ügyfélkódtár (v0.9.0) mostantól a Javához készült Azure-tárak csomag részét képezi.
* **Az Azure-könyvtárakban Java frissítette**. A frissítés Azure szalagtárak Java v0.7.0 és tárolási API-ja v2.0.0, valamint az újonnan kiadott Application Insights SDK v0.9.0 tartalmazza.

### <a name="november-12-2014"></a>2014. november 12.
Az Azure-eszközkészlet az Eclipse - 2014. novemberi kiadásban a következő fejlesztéseket tartalmazza:

* **Az Azure SDK 2.5 támogatása**. A legutóbbi frissítés az Azure SDK-t az új előfeltételeként az eszközkészlet.
* **A Zulu OpenJDK v1.8, v1.7 és 1.6 csomagok frissített verzióit támogatása**. További információkért lásd: a [Azul rendszerek weboldalra a Zulu OpenJDK].
* **Az új szabványos D méretét a felhőszolgáltatások támogatása**, amely nagyobb teljesítményt és a további memória-erőforrásokat kínálnak. További információkért lásd: [virtuális gépek és Felhőszolgáltatások mérete az Azure-].

### <a name="october-17-2014"></a>2014. október 17.
Az Azure-eszközkészlet az Eclipse - 2014. októberi kiadása a következő fejlesztéseket tartalmazza:

* **A közzététel a felhős rendszerekben a teljesítménnyel kapcsolatos fejlesztések**. Előfizetési adatok betöltésének sokkal gyorsabb, ha a felhasználók több előfizetések és a storage-fiókok.
* **A Zulu OpenJDK v1.8 csomag frissített verziója támogatása**. További információkért lásd: a [Azul rendszerek weboldalra a Zulu OpenJDK].
* **3. fél JDKs régebbi verzióit elavulttá támogatása**. Elavult JDK csomagok már nem jelennek meg az új központi telepítési projektek legördülő menüből. Hivatkozási elavult JDK csomagok továbbra is nem működik, így az idő alatt, de ajánlott ilyen projektek használják a legújabb frissítése a már meglévőket.
* **Az Azure-tárakat csomag frissített verziója Java API ügyféloldali kódtára a**. További információkért lásd: a [Microsoft Azure API-ja].
* **Hibajavításokat tartalmaz.** Ez a kiadás számos különböző hibajavításokat tartalmaz, amelyek felhasználói jelentések és a teszteléshez alapuló tartalmaz.

### <a name="august-5-2014"></a>2014. augusztus 5.
Az Azure-eszközkészlet az Eclipse - augusztus 2014 kiadásban a következő fejlesztéseket tartalmazza.

* **Az Azure SDK 2.4 támogatása.** Az Eclipse eszközkészlet régebbi verziói nem működik az újonnan kiadott SDK-val.
* **Frissített verziói Zulu OpenJDK 1.6 1.7 és v1.8 csomagok.** További információkért lásd: a [Azul rendszerek weboldalra a Zulu OpenJDK].
* **Java API ügyfélkódtár a Azure-tárak csomag frissített verziója.** További információkért lásd: a [Microsoft Azure API-ja].
* **Támogatja a legújabb beállítások fájlformátum közzététele.** Támogatást kaptak a közzétételi beállítások fájlformátum 2.0-s verzióját.
* **A közzététel a felhőalapú szolgáltatás mögött architekturális módosításokat.** Az eszközkészlet használ az újonnan kiadott Microsoft Azure API-ja Java közzététele felhő támogatása miatt.
* **Hibajavításokat tartalmaz.** Ez a kiadás számos felhasználó által kért hibajavításokat tartalmaz.

### <a name="june-12-2014"></a>2014. június 12.
Az Eclipse - június 2014 kiadásban az Azure eszközkészlet kisebb karbantartási frissítéseket, amelyek a következő fejlesztéseket biztosítja:

* **A Zulu OpenJDK csomag v1.8 támogatása.** További információkért lásd: a [Azul rendszerek weboldalra a Zulu OpenJDK].
* **A Zulu OpenJDK 1.6 és 1.7 csomagok frissített verzióit.** További információkért lásd: a [Azul rendszerek weboldalra a Zulu OpenJDK].
* **Java API ügyfélkódtár a Azure-tárak csomag frissített verziója.** További információkért lásd: a [Microsoft Azure API-ja].
* **Hibajavításokat tartalmaz.** Ez a kiadás számos felhasználó által kért hibajavításokat tartalmaz.

### <a name="april-4-2014"></a>2014. április 4.
Az Azure beépülő modul az Eclipse - 2014. április kiadás kiadott. Ez a kísérő az Azure SDK 2.3, amely olyan előfeltételt, és letölti automatikusan a beépülő modul telepítésekor kiadása frissítés. A frissítés óta a 2014. február tekintse meg új funkciókat, hibajavításokat tartalmaz és néhány visszajelzés adatvezérelt használhatóság továbbfejlesztett tartalmazza:

* **Az Azure SDK 2.3 kiadás támogatása.** Az Azure beépülő modul az Eclipse - 2014. április kiadás Azure SDK 2.3 igényel. Az új beépülő modul használata, ha még nem rendelkezik Azure SDK 2.3, kérni fogja a telepítéséhez. Ne használja az Azure SDK 2.3 a beépülő modul korábbi verzióival.
* **Frissíti az alkalmazások teljes csomag telepítése nélkül.** A projekt részét képező Java-alkalmazások telepítésekor a beépülő modul most automatikusan feltölti a kiválasztott tárolási figyelembe, hogy a frissítést, és hasznosítsa újra anélkül, hogy a legfrissebb alkalmazás bits telepítendő szerepkör-példányok és Telepítse újra a teljes csomag.
* **Tomcat 8 most már egy felismert alkalmazáskiszolgáló.** Ha a számítógépen a Tomcat 8 telepítési címtár kiválasztása a **Server** lapján a **Azure telepítési projekt** párbeszédpanelen, a beépülő modul most automatikusan felismeri és tudja telepíteni a Tomcat 8 egy automatikus módon, a korábbi verzióihoz hasonlóan Tomcat már a listában.
* **Azul Zulu OpenJDK csomag frissítések: v1.7 frissítés 51 és 1.6 frissítés 47.** Hatékony ebben a kiadásban, Azul rendszer Zulu nyitott JDK v7 Csomagfrissítés 51 érhető el. Emellett Zulu nyitott JDK v6 tartalomverzióval fog rendelkezni érhetők el, 47 frissítéstől kezdődően. Ezek a frissítések mellett a korábban elérhető Zulu nyitott JDK v7 csomag frissítés 45, 40, és 25 frissítése.
* **A8 és a9-es Microsoft Azure virtuális gép mérete támogatása.** Egy felhőalapú szolgáltatás most már telepítheti a felső memóriaterület A8 és A9 virtuális gépek méretét. A Virtuálisgép-méretek kapcsolatos további információkért lásd: [virtuális gépek és Felhőszolgáltatások mérete az Azure-].
* **Automatikus átirányítása HTTP – HTTPS szerepkörök SSL-kompatibilis.** Ha a felhőszolgáltatás csak HTTPS szerepkör(ök) tartalmaz, ha a felhasználói kérelem HTTP határozza meg, akkor automatikusan átirányítja a HTTPS. Nincs szükség a HTTP-kérelmek kezeléséhez külön szerepkör létrehozása.
* **Helyi emuláció használt Emulator Express.** Az Azure Express Emulator most már használatban van az emulátor helyileg az alkalmazások hibakeresése során.
* **Azure rendelkezik lett rebranded, mint a Microsoft Azure.** Felhasználói felület képernyők most tükrözi, hogy Azure rebranded lett, és már nem nevű az Azure.

### <a name="february-6-2014"></a>2014. február 6.
Az Azure beépülő modul az eclipse-ben – 2014. február közzétette az előzetes verzió. A frissítés óta a 2013. október tekintse meg új funkciókat, hibajavításokat tartalmaz és néhány visszajelzés adatvezérelt használhatóság továbbfejlesztett tartalmazza:

* **Támogatja az SSL-feladatkiszervezést.** Secure Sockets Layer (SSL)-feladatkiszervezést szolgáltatásként, amellyel könnyen engedélyezheti a Hypertext Transfer Protocol Secure (HTTPS) támogatása az Azure, a Java környezetben anélkül, hogy az SSL konfigurálása a Java-alkalmazáskiszolgáló lett hozzáadva. Ez különösen fontos a munkamenet-affinitás és/vagy kommunikációs helyzetek hitelesíteni. Például, amikor az Access Control Service (ACS) szűrő használatával, amely már támogatja az eszközkészlet. További információkért lásd: [SSL-Feladatkiszervezést] és [hogyan használja az SSL-Feladatkiszervezést].
* **GlassFish 4 most már egy felismert alkalmazáskiszolgáló.** Ha egy GlassFish 4 telepítési könyvtárat a számítógépen a válassza ki a **Server** lapján a **Azure telepítési projekt** párbeszédpanelen, a beépülő modul most automatikusan felismeri és GlassFish telepíthet OSE 4 automatizált módon, hasonlóan a GlassFish OSE 3 verzió már szerepel a listában.
* **Azul Zulu OpenJDK Csomagfrissítés 45.** Hatékony ebben a kiadásban, Azul rendszer Zulu (nyitott JDK v7 csomag) frissítés 45 áll rendelkezésre; Ez az a korábban elérhetővé vált frissítés mellett, 40, és 25-ös frissítést.
* **"Automatikus" privát végpontportokat támogatja.** Beállíthatja a magánhálózati port bemeneti végpont és a belső végpontok ahhoz, hogy egy port automatikusan rendel az adott végpontra Azure automatikus. Korábban csak lehetett hozzárendelni a megadott port számát.
* **A tanúsítvány neve (CN) önaláírt tanúsítvány létrehozása felhasználói felület testreszabása támogatása.** Korábban a kódolt néven használt minden új tanúsítvány; Mostantól megadhatja a saját tanúsítvány nevet, amely több tanúsítványt többféle célra használt Azure-portálon megkülönböztetésére.
* **Az Azure eszköztár:** az Azure eszköztár rendelkezik egy frissített a következő módosításokat: 
  * ![][ic710876]Ez az ikon kaptak a **új Azure-telepítési projekt**.
  * ![][ic710877]Erre az ikonra az önaláírt tanúsítvány létrehozása párbeszédpanel gyors lett adva.
* **A5 Azure virtuálisgép-méret támogatása.** Most már a felső memóriaterület A5 virtuálisgép-méret telepítene egy felhőalapú szolgáltatás. A Virtuálisgép-méretet kapcsolatos további információkért lásd: [virtuális gépek és Felhőszolgáltatások mérete az Azure-].
* **Microsoft Windows Server 2012 R2 támogatása.** A felhő operációs rendszerként kiválaszthatják a Windows Server 2012 R2.

### <a name="october-22-2013"></a>2013. október 22.
Az Azure beépülő modul az Eclipse - 2013. október közzétette az előzetes verzió. A frissítés óta a 2013. szeptemberi tekintse meg új funkciókat, hibajavításokat tartalmaz és néhány visszajelzés adatvezérelt használhatóság továbbfejlesztett tartalmazza:

* **Az Azure SDK 2.2 kiadás támogatása.** Az Azure beépülő modul az Eclipse - 2013. október tekintse meg az Azure SDK 2.2 támogatja. A beépülő modul Azure SDK 2.1-es továbbra is működni fog, és automatikusan telepít Azure SDK 2.2, ha még nem rendelkezik legalább az Azure SDK 2.1-es verziójának telepítése.
* **Azul Zulu OpenJDK Csomagfrissítés 40.** A 2013. szeptemberi a jelent megtekintéséhez a beépülő modul mostantól lehetővé teszi, hogy egy harmadik fél által biztosított JDK használatával közvetlenül az Azure, anélkül, hogy a saját JDK feltöltését. 2013. októberi kiadása Azul rendszer Zulu (nyitott JDK v7 csomag) frissítés 40 áll rendelkezésre; Ez egy, az eredetileg közzétett frissítés 25 mellett.
* **A műveletnapló felhő telepítési hivatkozásra.** Az Azure tevékenységnapló, amikor a központi telepítés állapota belül **közzétett**, kattinthat **közzétett** azóta most már egy hivatkozást a központi telepítés; a központi telepítés majd nyitja meg a böngésző. (Állapotának **közzétett** volt korábban feliratú **futtató**.)
* **Cél operációs rendszer kijelölés rendelkezésre álló idő közzététele.** A **Azure közzététel** párbeszédpanel új mezőt tartalmaz **cél operációs rendszer**, meg kell adnia a cél operációs rendszer felfedezését megoldást kínál, amelyek.
* **A korábbi központi telepítés automatikusan felülírja.** A **Azure közzététel** párbeszédpanel tartalmaz egy új jelölőnégyzet **felülírja a korábbi központi telepítés**. Ha ez a beállítás be van jelölve, amikor az új központi telepítési közzétett automatikusan felülírja a korábbi központi telepítés; nem lenne tapasztal &quot;409 ütközés&quot; ad ki, ha közzététele ugyanarra a helyre közzétételének megszüntetése, a korábbi központi telepítés nélkül.
* **Jetty 9 most már egy felismert alkalmazáskiszolgáló.** Ha a számítógépen a Jetty 9 telepítési címtár kiválasztása a **Server** lapján a **Azure telepítési projekt** párbeszédpanelen, a beépülő modul mostantól automatikusan észlelését és a Jetty 9 telepíthet egy automatikus módon, hasonló Jetty régebbi verzióit már szerepel a listában.
* **Adja hozzá a szerepkört a projekt helyi menüjéből.** A **Azure** projekt helyi menü most már tartalmaz egy új menüpont **szerepkör hozzáadása**, amely biztosítja a gyorsabb és további felderíthető módja annak, hogy egy új szerepkör hozzáadása az Azure-projekt.
* **Az Azure-könyvtárakban Java kódtár csomagja frissítése.** Ez a 0.4.6 verzióján alapul a [Microsoft Azure API-ja].

### <a name="september-25-2013"></a>2013. szeptemberi 25
Az Azure beépülő modul az Eclipse - 2013. szeptemberi közzétette az előzetes verzió. A frissítés óta a augusztus 2013 előzetes új funkciókat, hibajavításokat tartalmaz és néhány visszajelzés adatvezérelt használhatóság továbbfejlesztett tartalmazza:

* **Lehetővé teszi a Azul Zulu OpenJDK elérhető Azure-csomag telepítését.** Egy új beállítás lett elérhető az Azure-telepítés használata JDK megadásakor. Ezzel a beállítással telepítheti a harmadik féltől származó JDK csomagot közvetlenül az Azure felhőben anélkül, hogy töltse fel a saját. Azul rendszerek szolgáltató az első ilyen csomag hívott Zulu, a OpenJDK, amelyet most telepítve Ez a beállítás alapján.
* **Az Azure-könyvtárakban Java kódtár csomagja frissítése.** Ez a 0.4.5 verzióján alapul a [Microsoft Azure API-ja].

### <a name="august-1-2013"></a>2013. augusztus 1.
Az Azure beépülő modul az Eclipse - augusztus 2013 Preview közzétette. Ez a kísérő az Azure SDK 2.1, amely olyan előfeltételt, és letölti automatikusan a beépülő modul telepítésekor a kiadás frissítés. A frissítés óta a július 2013 előzetes új funkciókat, hibajavításokat tartalmaz és néhány visszajelzés adatvezérelt használhatóság továbbfejlesztett tartalmazza:

* **A helyi JDK és a helyi kiszolgáló tartalmazza a központi telepítési csomag részeként beállítások eltávolítása.** A JDK letöltése és felhőalapú tárolás a telepítés során az alkalmazáskiszolgáló előnyösebb ezeket az összetevőket beágyazása a csomaghoz, mivel a elemek letöltése kisebb központi telepítési csomag mérete, a gyorsabb üzembe helyezési idők eredményez, és a könnyebb karbantartás. Ennek eredményeképpen a lehetőségeket a JDK és a központi telepítési csomagban alkalmazáskiszolgáló el lettek távolítva. Meglévő projekteket közé tartoznak a helyi JDK és a helyi kiszolgáló automatikusan a központi telepítési csomag része az automatikus – feltöltés a JDK konvertálható konfigurált és az alkalmazáskiszolgáló felhőbeli tárhelyére.
* **Az Azure SDK 2.1-es kiadás támogatása.** Az Azure beépülő modul az Eclipse - augusztus 2013 Preview Azure SDK 2.1-es verziója szükséges. Ne használja a augusztus 2013 preview korábbi verzióiban az Azure SDK-t, és ne használja Azure SDK 2.1 korábbi verzióiban a Azure beépülő modul az eclipse-ben.
* **Az Eclipse Kepler kiadás támogatása.** Az ezzel kapcsolatos, az új minimálisan szükséges Eclipse IDE verziószám Indigo. Az Azure beépülő modul az eclipse-ben már nem a hivatalosan a Helios lett tesztelve.

### <a name="july-3-2013"></a>2013. július 3.
Az Azure beépülő modul az Eclipse - július 2013 Preview közzétette. A frissítés óta az lehet, hogy 2013 előzetes új funkciókat, hibajavításokat tartalmaz és néhány visszajelzés adatvezérelt használhatóság továbbfejlesztett tartalmazza:

* **Hozzon létre egy új tárfiókot lehetősége.** A **új** gomb hozzá lett adva a **Tárfiók hozzáadása** párbeszédpanel. Ez lehetővé teszi, hogy anélkül, hogy jelentkezzen be az Azure felügyeleti portálon belül az Eclipse beépülő modul, storage-fiók létrehozása. (Már rendelkeznie kell a szolgáltatás használatához Azure-előfizetés.) Új tárfiók létrehozásával kapcsolatos további információkért lásd: [egy új tárfiók létrehozása].
* **Új &quot;(automatikus)&quot; JDK és a kiszolgáló automatikus központi telepítéshez, és a gyorsítótárazáshoz használt tárfiók lehetőséget.** Használatakor a **automatikus feltöltése** beállítás megadása a JDK és az alkalmazáskiszolgáló, megadhat **(automatikus)** feltöltésekor a rendszer a JDK használandó URL-cím és a tárolási fiók és a kiszolgáló, vagy ha Azure-gyorsítótárazás használata Ezt követően ezeket a szolgáltatásokat automatikusan használja ugyanazt a tárfiókot, akkor jelölje ki a **Azure közzététel** párbeszédpanel. A [egy Hello World alkalmazás létrehozása az Azure az eclipse-ben] oktatóanyag frissítve lett, így az új **(automatikus)** lehetőséget.
* **Állítsa be az Azure szolgáltatásvégpontokra lehetősége.** Adja meg a szolgáltatás végpontok, amelyek meghatározzák, hogy az alkalmazás központi telepítése és a globális Azure platform által kezelt, Azure által üzemeltetett Kína, vagy egy olyan magánhálózat 21Vianet Azure platformon. További információkért lásd: [Azure Szolgáltatásvégpontok].
* **Nagy telepítések adhatja meg a helyi tároló egyik erőforrásához.** Abban az esetben, ha a központi telepítés mérete túl nagy, az alapértelmezett approot mappában található, megadhat egy helyi tároló-erőforrás a JDK központi telepítés célját, és az alkalmazáskiszolgálóhoz. További információkért lásd: [nagy méretű telepítéséhez telepítése].
* **A6 méretű és A7 Azure virtuális gép mérete támogatása.** Egy felhőalapú szolgáltatás most már telepítheti a felső memóriaterület A6 és A7 virtuális gépek méretét. Ezek méretek kapcsolatos további információkért lásd: [virtuális gépek és Felhőszolgáltatások mérete az Azure-].
* **Az Azure-könyvtárakban Java kódtár csomagja frissítése.** Ez a 0.4.4 verzióján alapul a [Microsoft Azure API-ja].

### <a name="may-1-2013"></a>2013. május 1.
Az Eclipse - Azure beépülő modul lehetséges, hogy 2013 Preview kiadott. Ez az az Azure SDK 2.0 rendszerbe, amely olyan előfeltételt, és letölti automatikusan a beépülő modul telepítésekor kiadása kísérő fő frissítés. Mivel a február 2013 előzetes ebben a kiadásban új funkciókat, hibajavításokat tartalmaz és néhány visszajelzés adatvezérelt használhatóság továbbfejlesztett tartalmazza:

* **A JDK-alkalmazáskiszolgáló és telepítését, az Azure storage automatikus feltöltése.** Egy új beállítás, mely automatikusan feltölti a kijelölt JDK és az alkalmazáskiszolgálóhoz, ha szükséges, a megadott Azure storage-fiók, és ezeket az összetevőket ott helyett a központi telepítési csomag beágyazása-nak vagy a felhasználó feltöltés majd manuálisan telepíti. A gyakran lekérdezett szolgáltatás jelentősen növelheti a könnyű üzembe helyezése a JDK és a kiszolgáló összetevőket, különösen a kezdő felhasználókat. Ez az útmutató ezeket a beállításokat használja, lásd: [egy Hello World alkalmazás létrehozása az Azure az eclipse-ben].
* **A központi követési tárfiók és a hivatkozás storage-fiókok további könnyen (a legördülő lista vezérlő) keresztül képességét.** Ez vonatkozik a tárhelyen, például a JDK és a kiszolgáló-összetevő telepítési és a gyorsítótárazást használó több funkció. További információkért lásd: [Azure Storage-fiókok listáján].
* **Egyszerűbb a távelérés beállítása a közzététel a felhő varázsló.** Mindössze annyit kell tennie típus egy felhasználónevet és jelszót a távoli hozzáférés engedélyezéséhez, vagy hagyja üresen, ha meg szeretné tartani a távoli hozzáférés letiltva.
* **Az Azure-könyvtárakban Java kódtár csomagja frissítése.** Ez a 0.4.2 verzióján alapul a [Microsoft Azure API-ja].
* **A kapcsolódó munkamenetek a Windows Server 2012 támogatása.** Korábban a kapcsolódó munkamenetek dolgozott csak Windows Server 2008 R2, most is cloud Az operációs rendszer célok támogatási munkamenet kapcsolat.
* **Csomag feltöltési teljesítménynövekedést.** Akkor is, ha a JDK és alkalmazáskiszolgáló beágyazott a központi telepítési csomag, a központi telepítést, a feltöltési része lehet körülbelül kétszer lassabbak, mint az előző verziókat.

### <a name="february-8-2013"></a>2013. február 8.
Az Azure beépülő modul az Eclipse - 2013. február közzétette az előzetes verzió. Ez az a kisebb frissítéseket, beleértve a hibajavításokat tartalmaz, visszajelzés adatvezérelt használhatóság fejlesztése és egyes új funkcióit óta a 2012. November megtekintése:

* Támogatás az központi JDKs, alkalmazás-kiszolgálókat, és tetszőleges más összetevők, a saját vagy nyilvános Azure blob-tároló letöltések helyett, beleértve azokat a központi telepítési csomagban, a felhőben való üzembe helyezés esetén.
* Megváltoztathatja a amely összetevők felhasználói szerepkör feldolgozásának sorrendjét, hozzáadásával **fel** és **le** gombja a **összetevők** szakasz az a **Azure Role szerepkör tulajdonságai**.
* Egy frissítést adunk ki a **a Javához készült Azure-tárak csomag** könyvtár 0.4.0 verziója alapján a [Microsoft Azure API-ja].

### <a name="november-5-2012"></a>2012. november 5.
Az Azure beépülő modul az eclipse-ben – a 2012. November közzétette az előzetes verzió. Ez a, amely számos olyan új funkciókat, valamint további hibajavításokat és visszajelzés adatvezérelt használhatóság fejlesztések tekintse meg a szeptember 2012 óta fő frissítés:

* Microsoft Windows Server 2012 felhő operációs rendszer támogatása.
* Azure közös elhelyezésű gyorsítótárazási támogatási memcached-ügyfelek támogatása.
* Az Apache Qpid JMS ügyfél könyvtárakban kihasználja az Azure AMQP-alapú üzenetküldési belefoglalásához.
* Egy továbbfejlesztett **új projekt** varázsló befejezése után, amely segítségével gyorsan engedélyezheti a projekt több közös kulcs funkciókat biztosít a felhasználók számára egy új lapok: állandóságát munkamenetek, gyorsítótárazás és távoli hibakeresés.
* 1, ha az a compute emulator kiszolgálópéldányok közötti port kötés ütközések elkerülése érdekében a szerepkörpéldányok automatikus csökkentése.

### <a name="september-28-2012"></a>2012. szeptember 28.
Az Azure beépülő modul az eclipse-ben – a 2012. szeptember közzétette az előzetes verzió. Ez a szolgáltatásfrissítés számos olyan további hibajavításokat tartalmaz, mivel a augusztus 2012 megtekintése, valamint néhány visszajelzés adatvezérelt használhatóság javításai, a meglévő funkciók:

* Támogatja a Microsoft Windows 8 és a Microsoft Windows Server 2012 fejlesztési operációs rendszert, hogy a korábban említett operációs rendszereken nem működnek megfelelően miatt nem sikerült a beépülő modul problémák elhárításához.
* Továbbfejlesztett támogatása a végpont porttartományok megadása.
* A szóközöket tartalmazó elérési utat kapcsolatos hibajavítások.
* Szerepkör helyi menü elősegíteni gyorsabb hozzáférés a szerepkör-specifikus konfigurációs beállításokat.
* A finomításokra kisebb a **közzététele a felhőalapú** varázsló, és számos további hibajavításokat tartalmaz.

### <a name="august-28-2012"></a>2012. augusztus 28.
Az Azure beépülő modul az Eclipse - augusztus 2012 közzétette az előzetes verzió. A szolgáltatás frissítés tartalmazza a további hibajavításokat tartalmaz, mivel a 2012. júliusi megtekintése, valamint a meglévő szolgáltatások számos visszajelzés adatvezérelt használhatóság fejlesztései:

* Belül az Azure Access Control szolgáltatások szűrő párbeszédpanel:
  * **Az aláíró tanúsítvány beágyazására** a az alkalmazás WAR-fájlt, egyszerűbbé teheti a felhő üzembe helyezése.
  * **Létrehozhat egy önaláírt tanúsítványt** belül az ACS szűrheti a felhasználói felület. A Azure Access Control szolgáltatások szűrő kapcsolatos további információkért lásd: [webes felhasználók hitelesítéséhez az Azure Access Control Service használatával Eclipse hogyan].
* Az Azure-telepítési projekt varázslóban (a szerepkör kiszolgálókonfiguráció tulajdonságlapján is vonatkozik):
  * **A JDK-hely automatikus felderítés** a számítógépen (amely felülírhatja, ha szükséges).
  * **A kiszolgáló típusú automatikus észlelési** kiválasztásakor az alkalmazás kiszolgáló telepítési könyvtárában.

### <a name="july-15-2012"></a>2012. július 15.
Az Azure beépülő modul az eclipse-ben – a 2012. júliusi közzétette az előzetes verzió, amely a legmagasabb prioritású hibák található, és/vagy a 2012. júniusi megjelenése után a felhasználók által jelentett számos foglalkozik,. Ez a szolgáltatás frissítése csak, nincs tervben új szolgáltatások tartalmazza.

### <a name="june-7-2012"></a>2012. június 7.
Az eclipse-ben – a 2012. júniusi CTP Azure beépülő modul kiadott. Új funkciók a következők:

* **Új Azure-telepítési projekt varázsló:** lehetővé teszi a JDK Java-alkalmazáskiszolgáló, és a Java-alkalmazások közvetlenül a felhasználói felület jobb varázslóban. Szerepel a listán, a-kész kiszolgálóbeállítás választhat a következők Tomcat 6, Tomcat 7, GlassFish OSE 3, Jetty 7, Jetty 8, JBoss 6, és JBoss 7 (önálló). Emellett testre szabhatja a konfigurációk listáját. A felhasználói felület javítása az alternatív húzással és tömörített fájlok, és cserélje le a indítási parancsfájlok, amelyek korábban a fő módszer. Ez a módszer továbbra is jól működik, de valószínűleg csak speciális forgatókönyvek esetén használható.
* **Konfigurációs tulajdonság szerepkörlapjáról:** lehetővé teszi, hogy átváltson az a JDKs, Java-alkalmazáskiszolgálók és a projekt létrehozása után a központi telepítés társított alkalmazásokat. További információkért lásd: [kiszolgáló konfigurációs tulajdonságai].
* **&quot;Közzététele felhőben&quot; varázsló:** egyszerű megoldás a projekt telepítése az Azure-ba közvetlenül az eclipse-ben automatizálása a korábban manuális gyakori-megszüntetéséről beolvasásához használt hitelesítő adatok, az Azure felügyeleti portálra való bejelentkezéskor feltöltése egy csomag stb. Példa bemutatja, hogyan közvetlenül projekt telepítése az Azure-ba, lásd: [egy Hello World alkalmazás létrehozása az Azure az eclipse-ben].
* **Az Azure eszköztár:** Azure eszköztár már elérhető az eclipse-ben, amely a következő funkciók meghívása gombok tartalmazó:
  * ![][ic710879]**Azure emulátorban futtatása**: a projekthez az emulátorban futtatja.
  * ![][ic710880]**Visszaállítása Azure Emulátorban**: alaphelyzetbe állítja az emulátor.
  * ![][ic710881]**Felhő csomag létrehozása az Azure-**: lefordítja a központi telepítési csomag.
  * ![][ic710876]**Új Azure-telepítési projekt**: létrehoz egy új projektet az Azure-telepítés.
  * ![][ic710882]**Publish Azure felhőbe**: közzéteszi a projekthez az Azure-bA.
  * ![][ic710883]**Közzététel megszüntetése**: törli a központi telepítés.
  * Sok Azure eszköztár gombok használt [egy Hello World alkalmazás létrehozása az Azure az eclipse-ben].
* **Azure Java-könyvtárakban:** most már hozzáférhető a egyetlen csomag részeként a Azure-tárak Java szalagtár az eclipse-ben, a beépülő modul telepítése kísérő és az összes, valamint a szükséges függőségek. Csak egy hivatkozást a beépülő modul a Java-projektet, és nincs szükség semmilyen külön letöltést. További információkért lásd: [az eclipse-ben az Azure eszközkészlet telepítésével].
* **Microsoft JDBC 4.0-s illesztőprogram az SQL Server rendelkezésre álló beépülő modul telepítése során:** az új beépülő modul telepítésekor, telepítheti a legújabb verzióját a Microsoft SQL Server JDBC-illesztőt.
* **Az Azure hozzáférést vezérlő szolgáltatás szűrő beépülő modul telepítésekor érhető el:** ezt az új összetevőt, az eszközkészlet egy Eclipse szalagtár néven lehetővé teszi, hogy a Java-webalkalmazás zökkenőmentesen kihasználásához meg az Azure Access Control Service (ACS) hitelesítés különböző Identitásszolgáltatók, például a Google, Live.com és Yahoo!-s. Nem kell saját kezűleg hitelesítési logika írását, csak néhány beállításokat adhat meg, és lehetővé teszik, hogy a felhasználók jelentkezhetnek be az ACS használatával a gyakori emelő tegye a szűrő. Csak összpontosíthat programozás a felhasználók hozzáférést biztosító erőforrásokhoz az identitásukat, hogy az alkalmazást a Request objektumon belül szűrő által visszaadott formában. Az ACS-szűrő használatával oktatóanyag, lásd: [webes felhasználók hitelesítéséhez az Azure Access Control Service használatával Eclipse hogyan].
* **Az Azure SDK 1.7 előfeltétel automatikus észlelése:** egy új Azure-telepítési projekt létrehozásakor Azure SDK 1.7 fogja letölteni, ha még nincs telepítve.
* **Végpontok példány:** lehetővé teszi közvetlen port végpont hozzáférési kommunikáció a terhelés eloszlik a szerepkörpéldányok. Példány végpontok hozzáadása is lehetséges a felhasználói felületén keresztül elérhető végpontok keresztül a [végpontok tulajdonságok] lap. Ez segít a távoli hibakeresésének engedélyezése és JMX diagnosztika adott számítási a többszörös helyzetekben a felhőben futó példányok-példány központi telepítéseket. 
* **Felhasználói felületi összetevőket:** megkönnyíti a tapasztalt felhasználók beállítása az adott Azure szerepkörök a projekt és más külső erőforrások, például Java-alkalmazás projektek között projektfüggőségek; is megkönnyíti a központi telepítési programot ismertetik. További információkért lásd: [összetevők tulajdonságok].
* **A korábbi verzióktól projektek automatikus frissítését:** , amely rendelkezik a beépülő modul egy korábbi verziójával létrehozott Azure-projekt munkaterület megnyitásakor a régi projektek megjelennek az eclipse-ben zárva, mert projektek korábbi verziói nem kompatibilis az új verziót. Nyissa meg a régi projektek kísérli meg, ha egy frissítés varázsló elindul. Ha elfogadja a frissítés, egy új projektet **_Upgraded** hozzáfűzve, a rendszer létrehozott, majd automatikusan frissül az új kiadási együttműködni. Az új projekt át lehet nevezni igény szerint. A frissítés részeként az eredeti projekt nem módosulnak (és maradnak lezárt).

### <a name="december-10-2011"></a>2011. december 10.
Az Eclipse - December 2011 CTP Azure beépülő modul kiadott. Új funkciók a következők:

* **Munkamenet-affinitás (&quot;állandóságát munkamenetek&quot;) támogatja:** segíti az állapotalapú alkalmazások és szolgáltatások, engedélyezése fürtözött csak egyetlen jelölőnégyzetű Java-alkalmazások. További információkért lásd: [munkamenet affinitás].
* **Előre végrehajtott indítási parancsfájl minták:** legnépszerűbb Java kiszolgálók (Tomcat, Jetty, JBoss, GlassFish), akkor is csak másolja és illessze be a projekt minták könyvtárból a indítási parancsfájlba.
* **Valós idejű emulátor indítási kimeneti:** most figyelemmel követheti a lépések végrehajtása a dedikált konzolablakban, az indítási parancsfájl jeleníti meg a folyamatban lévő és sikertelen a parancsfájlban, az Azure-ban végrehajtása.
* **Automatikus, passzív java.exe figyelési:** szerepkör újrahasznosítást, amely arra kényszeríti, amikor java.exe leáll, automatikusan bekerül a központi telepítés könnyű, előre elkészített parancsfájl használatával.
* **Távoli Java-alkalmazás konfigurációs UI hibakeresés:** könnyen engedélyezheti az Eclipse meg távoli hibakereső a Java-alkalmazás fut, amelyen az emulátor vagy az Azure felhőalapú, ezért teljesítéséhez, és a valós idejű Java kód hibakereséséhez eléréséhez. További információkért lásd: [hibakeresés Azure-alkalmazások az eclipse-ben].
* **Helyi storage erőforrás-konfiguráció felhasználói felületi:** így már nem kell helyi erőforrások konfigurálása az XML-fájl közvetlenül kezelésével. Ez a funkció lehetővé teszi keresztül egy környezeti változó, melyeket referenciaként használhat, közvetlenül az indítási parancsfájl a telepítés után a hatékony fájl elérési útját a helyi erőforrás elérését. További információkért lásd: [helyi tároló tulajdonságainak].
* **Környezeti változó konfigurációs UI:** így már nem kell a konfigurációs XML-kód manuális szerkesztésnél keresztül környezeti változók értékét. További információkért lásd: [környezeti változók tulajdonságok].
* **Az SQL Azure JDBC-illesztőt:** tárként zavartalanul integrált eclipse-ben, az SQL Azure könnyebben programozása engedélyezése telepíti a beépülő modul segítségével. 
* **Gyors helyimenü-hozzáférés a szerepkör konfigurációs UI**: Ebben az esetben kattintson a jobb gombbal a szerepkör mappára, és kattintson a **tulajdonságok**.
* **Egyéni Azure projektet és a szerepkör mappa ikonok:** nagyobb Láthatóság és könnyebb Navigálás érdekében a munkaterület és a projekt belül.

## <a name="see-also"></a>Lásd még:
A Java IDE környezetekhez készült Azure-eszközkészlettel kapcsolatos további információkért lásd az alábbi hivatkozásokat:

* [Eclipse Azure eszköztára]
  * *What's New in Eclipse (Ez a cikk) Azure eszköztára*
  * [az eclipse-ben az Azure eszközkészlet telepítésével]
  * [Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]
  * [Bejelentkezési utasítások az Eclipse-hez készült Azure-eszközkészlethez]
* [IntelliJ Azure eszköztára]
  * [Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]
  * [Az IntelliJ-hez készült Azure-eszközkészlet telepítése]
  * [Bejelentkezési utasítások az IntelliJ-hez készült Azure-eszközkészlethez]
  * [Hello World webalkalmazás létrehozása az intellij-t az Azure]

Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ].

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[az eclipse-ben az Azure eszközkészlet telepítésével]: ./azure-toolkit-for-eclipse-installation.md
[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md
[Bejelentkezési utasítások az Eclipse-hez készült Azure-eszközkészlethez]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Bejelentkezési utasítások az IntelliJ-hez készült Azure-eszközkészlethez]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[What's New in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md
[Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]: ./azure-toolkit-for-intellij-whats-new.md

[Azure bejelentkezési az utasításokat az Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[webalkalmazás közzététele az Azure-eszközkészlet használata az Eclipse Docker tárolóként]: ./azure-toolkit-for-eclipse-publish-as-docker-container.md
[kezelése Storage-fiókok az Azure-kezelővel használata az Eclipse]: ./azure-toolkit-for-eclipse-managing-storage-accounts-using-azure-explorer.md
[kezelése használó virtuális gépek az Azure-kezelővel az Eclipse]: ./azure-toolkit-for-eclipse-managing-virtual-machines-using-azure-explorer.md

[Azure Java fejlesztői központ]: http://go.microsoft.com/fwlink/?LinkID=699547

[Azul rendszerek weboldalra a Zulu OpenJDK]: http://go.microsoft.com/fwlink/?LinkId=402457
[Azure Szolgáltatásvégpontok]: http://go.microsoft.com/fwlink/?LinkID=699526
[Azure Storage-fiókok listáján]: http://go.microsoft.com/fwlink/?LinkID=699528
[összetevők tulajdonságok]: http://go.microsoft.com/fwlink/?LinkID=699525#components_properties
[egy Hello World alkalmazás létrehozása az Azure az eclipse-ben]: http://go.microsoft.com/fwlink/?LinkID=699533
[hibakeresés Azure-alkalmazások az eclipse-ben]: http://go.microsoft.com/fwlink/?LinkID=699535
[nagy méretű telepítéséhez telepítése]: http://go.microsoft.com/fwlink/?LinkID=699536
[végpontok tulajdonságok]: http://go.microsoft.com/fwlink/?LinkID=699525#endpoints_properties
[környezeti változók tulajdonságok]: http://go.microsoft.com/fwlink/?LinkID=699525#environment_variables_properties
[Eclipse HDInsight-eszközei beépülő]: ./hdinsight/hdinsight-apache-spark-eclipse-tool-plugin.md
[webes felhasználók hitelesítéséhez az Azure Access Control Service használatával Eclipse hogyan]: http://go.microsoft.com/fwlink/?LinkID=264703
[hogyan használja az SSL-Feladatkiszervezést]: http://go.microsoft.com/fwlink/?LinkID=699545
[Az Eclipse-hez készült Azure-eszközkészlet telepítése]: http://go.microsoft.com/fwlink/?LinkId=699546
[helyi tároló tulajdonságainak]: http://go.microsoft.com/fwlink/?LinkID=699525#local_storage_properties
[Microsoft Azure API-ja]: http://go.microsoft.com/fwlink/?LinkId=280397
[kiszolgáló konfigurációs tulajdonságai]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[munkamenet affinitás]: http://go.microsoft.com/fwlink/?LinkID=699548
[SSL-Feladatkiszervezést]: http://go.microsoft.com/fwlink/?LinkID=699549
[egy új tárfiók létrehozása]: http://go.microsoft.com/fwlink/?LinkID=699528#create_new
[virtuális gépek és Felhőszolgáltatások mérete az Azure-]: http://go.microsoft.com/fwlink/?LinkId=466520

<!-- IMG List -->

[ic710876]: ./media/azure-toolkit-for-eclipse-whats-new/ic710876.png
[ic710877]: ./media/azure-toolkit-for-eclipse-whats-new/ic710877.png
[ic710879]: ./media/azure-toolkit-for-eclipse-whats-new/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-whats-new/ic710880.png
[ic710881]: ./media/azure-toolkit-for-eclipse-whats-new/ic710881.png
[ic710876]: ./media/azure-toolkit-for-eclipse-whats-new/ic710876.png
[ic710882]: ./media/azure-toolkit-for-eclipse-whats-new/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-whats-new/ic710883.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh694270.aspx -->
