---
title: "aaaGet Azure Search Java használatába |} Microsoft Docs"
description: "A szolgáltatott felhők toobuild keresési alkalmazás Azure-ban a Java programozási nyelv."
services: search
documentationcenter: 
author: EvanBoyle
manager: pablocas
editor: v-lincan
ms.assetid: 8b4df3c9-3ae5-4e3a-b4bb-74b516a91c8e
ms.service: search
ms.devlang: na
ms.workload: search
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.date: 07/14/2016
ms.author: evboyle
ms.openlocfilehash: 5476a2103f3b60fe6ec78ff3d3fdba9fcff55c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-search-in-java"></a>Bevezetés az Azure Search használatába Java nyelven
> [!div class="op_single_selector"]
> * [Portál](search-get-started-portal.md)
> * [.NET](search-howto-dotnet-sdk.md)
> 
> 

Ismerje meg, olyan egyéni Java toobuild a keresési alkalmazás az Azure Search szolgáltatást a keresésekhez használ. Ez az oktatóanyag használja hello [Azure Search szolgáltatás REST API](https://msdn.microsoft.com/library/dn798935.aspx) tooconstruct hello objektumok és műveletek ebben a gyakorlatban.

toorun Ez a minta rendelkeznie kell egy Azure Search szolgáltatással, amely jelentkezhet be a hello [Azure Portal](https://portal.azure.com). Lásd: [Azure Search szolgáltatás létrehozása a portál hello](search-create-service-portal.md) részletes útmutatásait.

Azt a következő szoftver toobuild hello használt, és ez a minta teszteléséhez:

* [Eclipse IDE for Java EE Developers](https://eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/lunar). Lehet, hogy toodownload hello EE-verziót. Egy szolgáltatás, amely csak a jelen kiadásában megtalálható hello ellenőrzési lépések egyikét igényli.
* [JDK 8u40](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Apache Tomcat 8.0](http://tomcat.apache.org/download-80.cgi)

## <a name="about-hello-data"></a>Hello adatokról
A mintaalkalmazás hello adatait használja [az Amerikai Egyesült Államok geológiai szolgáltatások (USGS)](http://geonames.usgs.gov/domestic/download_data.htm), szűrt a hello Rhode Island államra tooreduce hello dataset mérete. Az adatok toobuild, amely jellegzetes épületeket, például kórházakat és iskolákat, valamint geológiai jellegzetességeket, például folyókat, tavakat és hegycsúcsokat ad vissza egy olyan keresőalkalmazás fogjuk használni.

Ebben az alkalmazásban hello **SearchServlet.java** program létrehozza és betölti az indexet hello egy [indexelő](https://msdn.microsoft.com/library/azure/dn798918.aspx) szerkezet hello szűrt USGS-adatkészletet a nyilvános Azure SQL-adatbázis. Előre meghatározott hitelesítő adatokat és kapcsolati adatokat toohello online adatforrás hello programkód találhatók. Az adatelérés szempontjából nincs szükség további konfigurációra.

> [!NOTE]
> Olyan szűrőt alkalmaztunk a dataset toostay hello 10 000 dokumentumos korlátja az ingyenes tarifacsomag hello alatt. Hello standard csomagot használja, ha nem vonatkozik ez a korlátozás, és módosíthatja a kód toouse nagyobb adatkészletet. Az egyes tarifacsomagok kapacitásával kapcsolatos részletes információkat lásd: [Korlátozások és megkötések](search-limits-quotas-capacity.md).
> 
> 

## <a name="about-hello-program-files"></a>Hello program fájlokkal kapcsolatban
hello alábbi lista részletesen ismerteti, amelyek megfelelő toothis minta hello fájlokat.

* Search.jsp: Hello felhasználói felületet biztosít.
* SearchServlet.java: Biztosít módszerek (hasonló tooa az MVC-vezérlőhöz)
* SearchServiceClient.java: a HTTP-kérelmeket kezeli
* SearchServiceHelper.java: egy statikus módszereket biztosító segítőosztály
* Document.Java: Hello adatok modellt biztosít.
* Config.Properties: Beállítja hello Search szolgáltatás URL-CÍMÉT és api-kulcs
* Pom.XML: Maven-függőség

<a id="sub-2"></a>

## <a name="find-hello-service-name-and-api-key-of-your-azure-search-service"></a>Hello szolgáltatás- és api-kulcsot az Azure Search szolgáltatás keresése
Azure Search szolgáltatásba történő minden REST API-hívások meg kell adnia hello szolgáltatás URL-CÍMÉT és api-kulcsát. 

1. Jelentkezzen be toohello [Azure Portal](https://portal.azure.com).
2. Hello Ugrás sávon kattintson **keresési szolgáltatás** toolist megjelenjen az előfizetéséhez kapcsolódó összes hello Azure Search szolgáltatást.
3. Válassza ki a kívánt toouse hello szolgáltatást.
4. A hello szolgáltatás irányítópultján megjelennek az alapvető információkat csempék, valamint hello adminisztrációs kulcsok eléréséhez szükséges kulcs ikon hello.
   
      ![][3]
5. Másolja a hello szolgáltatás URL-címet és egy adminisztrációs kulcsot. Később szüksége lesz rájuk, miután hozzáadta őket toohello **config.properties** fájlt.

## <a name="download-hello-sample-files"></a>Hello mintafájlok letöltése
1. Nyissa meg túl[AzureSearchJavaDemo](https://github.com/AzureSearch/AzureSearchJavaIndexerDemo) a Githubon.
2. Kattintson a **töltse le a ZIP-**, hello .zip fájl toodisk mentéséhez, és bontsa ki a benne található összes hello fájlt. Vegye figyelembe a kibontása hello fájlok azokat a Java-munkaterület toomake könnyebb toofind hello projekt később.
3. hello mintafájlt csak olvasható. Kattintson a jobb gombbal a mappa tulajdonságai és egyértelmű hello csak olvasható attribútumot.

Minden további fájlmódosítás és utasításfuttatás az ebben a mappában lévő fájlokra vonatkozóan fog történni.  

## <a name="import-project"></a>Projekt importálása
1. Az Eclipse-ben válassza ki a **File** (Fájl)  > **Import** (Importálás)  > **General** (Általános)  > **Existing Projects into Workspace** (Meglévő projekteket a munkaterületre) lehetőséget.
   
    ![][4]
2. A **Select root directory**, keresse meg a mintafájlokat tartalmazó toohello mappát. Válassza ki a hello .project mappát tartalmazó hello mappára. hello meg kell jelennie a projektben hello **projektek** listában kiválasztott elemként.
   
    ![][12]
3. Kattintson a **Befejezés** gombra.
4. Használjon **Project Explorer** tooview és Szerkesztés hello fájlokat. Ha még nincs nyitva, kattintson a **ablak** > **nézet megjelenítése** > **Project Explorer** vagy hello helyi tooopen használja azt.

## <a name="configure-hello-service-url-and-api-key"></a>Hello szolgáltatás URL-CÍMÉT és api-kulcsának konfigurálása
1. A **Project Explorer**, kattintson duplán a **config.properties** tooedit hello konfigurációs beállításokat tartalmazó hello kiszolgálónevet és api-kulcsot.
2. Tekintse meg a cikkben korábban, ha Ön megtalálható hello szolgáltatás URL-CÍMÉT és api-kulcs hello toohello lépéseket [Azure Portal](https://portal.azure.com), akkor írja be a tooget hello értékek **config.properties**.
3. A **config.properties**, "Api-kulcsot" cserélje hello api-kulcsot a szolgáltatás. A következő szolgáltatás neve (hello hello URL-cím http://servicename.search.windows.net első összetevője) behelyettesíti "a szolgáltatás neve" hello ugyanazt a fájlt.
   
    ![][5]

## <a name="configure-hello-project-build-and-runtime-environments"></a>Hello projekt, a build és a futtatókörnyezetek környezetek konfigurálása
1. Az Eclipse Project Explorer, kattintson a jobb gombbal hello project > **tulajdonságok** > **Project Facets**.
2. Válassza ki a **Dynamic Web Module** (Dinamikus webmodul), a **Java** és a **JavaScript** elemet.
   
    ![][6]
3. Kattintson az **Apply** (Alkalmaz) gombra.
4. Válassza ki a **Window** (Ablak)  > **Preferences** (Beállítások)  > **Server** (Kiszolgáló)  > **Runtime Environments** (Futtatókörnyezetek)  > **Add..** (Hozzáadás) lehetőséget.
5. Bontsa ki az Apache, és válassza ki a korábban telepített hello Apache Tomcat kiszolgálót hello verzióját. Mi a 8-as verziót telepítettük a rendszerünkre.
   
    ![][7]
6. Hello következő lapon adja meg a hello Tomcat telepítési könyvtárát. Windows rendszerű számítógépeken ez valószínűleg a következő lesz: C:\Program Files\Apache Software Foundation\Tomcat *verzió*.
7. Kattintson a **Finish** (Befejezés) gombra.
8. Válassza ki a **Window** (Ablak)  > **Preferences** (Beállítások)  > **Java** > **Installed JREs** (Telepített JRE-k)  > **Add** (Hozzáadás) lehetőséget.
9. Az **Add JRE** (JRE hozzáadása) panelen válassza ki a **Standard VM** elemet.
10. Kattintson a **Tovább** gombra.
11. A JRE Definition (JRE_definíció) ablakban, a JRE kezdőlapján kattintson a **Directory** (Könyvtár) elemre.
12. Keresse meg a túl**Program Files** > **Java** válassza ki a korábban telepített JDK hello. Fontos tooselect hello JDK hello JRE szerint is.
13. A telepített JREs, válassza a hello **JDK**. A beállítások a következő képernyőkép hasonló toohello kell kinéznie.
    
    ![][9]
14. Bejelölheti **ablak** > **webböngésző** > **Internet Explorer** tooopen hello alkalmazást egy külső böngészőablakban. Külső böngészővel fokozhatja a webalkalmazás használatának élményét.
    
    ![][8]

Hello konfigurációs feladatok most már befejeződött. A következő lesz hozza létre, és futtassa hello projektet.

## <a name="build-hello-project"></a>Hello projekt létrehozása
1. A Project Explorer, kattintson a jobb gombbal a projekt neve hello, és válassza a **futtató** > **Maven build...**  tooconfigure hello projekt.
   
    ![][10]
2. Az Edit Configuration (Konfiguráció szerkesztése) panelen a Goals (Célok) mezőbe írja be a „clean install” („tiszta telepítés”) kifejezést, majd kattintson a **Run** (Futtatás) gombra.

Állapotüzenetek kimeneti toohello konzolablakban. BUILD SIKERESSÉGÉT jelző hello projektet egy hiba nélkül kell megjelennie.

## <a name="run-hello-app"></a>Hello alkalmazás futtatása
Ezen utolsó lépésében hello alkalmazás a helyi kiszolgáló futtatókörnyezetében fog futni.

Ha az eclipse-ben még nem határozta meg egy kiszolgáló futtatókörnyezetét, toodo, először lesz szüksége.

1. A Project Explorer (Projektböngésző) nézetben bontsa ki a **WebContent** elemet.
2. Kattintson a jobb gombbal a **Search.jsp** fájlra, majd kattintson a  > **Run As** (Futtatás másként)  > **Run on Server** (Futtatás a kiszolgálón) elemre. Válassza ki a hello Apache Tomcat kiszolgálót, és kattintson a **futtatása**.

> [!TIP]
> Ha egy nem alapértelmezett munkaterületen toostore a projekthez, szüksége lesz a toomodify **futtatása konfigurációs** toopoint toohello projekt helye tooavoid Kiszolgálóindítási hiba. A Project Explorer (Projektböngésző) nézetben kattintson a jobb gombbal a **Search.jsp** fájlra, majd kattintson a  > **Run As** (Futtatás másként)  > **Run Configurations** (Konfigurációk futtatása) elemre. Válassza ki a hello Apache Tomcat kiszolgálót. Kattintson az **Arguments** (Argumentumok) elemre. Kattintson a **munkaterület** vagy **fájlrendszer** tooset hello csomagot hello projektet tartalmazó mappa.
> 
> 

Hello alkalmazás futtatásakor kell megjelennie a böngészőablakban feltételek megadása a keresőmező biztosítva.

Várjon körülbelül egy percet való kattintás előtt **keresési** toogive hello szolgáltatás idő és a toocreate hello indexet. HTTP 404 hibaüzenetet kap, ha csupán be kell toowait, majd próbálkozzon újra egy kicsit tovább.

## <a name="search-on-usgs-data"></a>USGS-adatok keresése
hello USGS-adatkészlet a Rhode Island államra vonatkozó toohello rekordokat tartalmaz. Ha **keresési** egy üres keresőmező kap hello 50 legfontosabb bejegyzés; hello alapértelmezett.

A keresett kifejezés beírása rendszerében hello keresőmotor valami toogo meg. Próbáljon meg a helyhez kötődő nevet beírni. "Roger Williams" volt Rhode Island első kormányzója hello. Számos parkot, épületet és iskolát neveztek el róla.

![][11]

Megpróbálhatja beírni az alábbi kifejezések bármelyikét is:

* Pawtucket
* Pembroke
* goose+cape

## <a name="next-steps"></a>Következő lépések
Ez az hello Azure Search első oktatóanyaga Java és USGS-adatkészletet hello alapján. Idővel majd tovább bővítjük ezen oktatóanyag toodemonstrate kiegészítő keresési funkciókat, amelyeket esetleg szívesen toouse egyéni megoldásaiban.

Ha már rendelkezik bizonyos tapasztalattal az Azure Search, használhatja ezt a mintát akár ugródeszkaként a további kísérletezéshez, például bővítheti a hello [keresőoldalt](search-pagination-page-layout.md), vagy a végrehajtási [jellemzőalapú navigációs](search-faceted-navigation.md). Akkor is tovább fejlesztheti hello keresési eredmények oldalát által számok és kötegelt dokumentumok, így a felhasználók lapozhassanak az eredmények hello hozzáadásával.

Új tooAzure keresési? Azt javasoljuk, próbáljon más oktatóanyagok toodevelop megismerhesse, mit hozhat létre. Látogasson el a [dokumentációs oldal](https://azure.microsoft.com/documentation/services/search/) toofind több erőforrást. Hello hivatkozások is megtekintheti a [videók és oktatóanyagok listáját](search-video-demo-tutorial-list.md) tooaccess további információt.

<!--Image references-->
[1]: ./media/search-get-started-java/create-search-portal-1.PNG
[2]: ./media/search-get-started-java/create-search-portal-21.PNG
[3]: ./media/search-get-started-java/create-search-portal-31.PNG
[4]: ./media/search-get-started-java/AzSearch-Java-Import1.PNG
[5]: ./media/search-get-started-java/AzSearch-Java-config1.PNG
[6]: ./media/search-get-started-java/AzSearch-Java-ProjectFacets1.PNG
[7]: ./media/search-get-started-java/AzSearch-Java-runtime1.PNG
[8]: ./media/search-get-started-java/AzSearch-Java-Browser1.PNG
[9]: ./media/search-get-started-java/AzSearch-Java-JREPref1.PNG
[10]: ./media/search-get-started-java/AzSearch-Java-BuildProject1.PNG
[11]: ./media/search-get-started-java/rogerwilliamsschool1.PNG
[12]: ./media/search-get-started-java/AzSearch-Java-SelectProject.png
