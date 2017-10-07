---
title: "MyDriving Azure IoT-példa: összeállítani, |} Microsoft Docs"
description: "Egy alkalmazás, amely bemutatja, hogyan átfogó bemutatója elkészítésére tooarchitect az IoT-rendszer Microsoft Azure Stream Analytics, a Machine Learning és az Event Hubs használatával."
services: 
documentationcenter: .net
suite: 
author: harikmenon
manager: douge
ms.assetid: c2fcd6ee-3bbe-43d1-a066-dce52cc3a53d
ms.service: multiple
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: dotnet
ms.topic: article
ms.date: 06/30/2017
ms.author: harikm
ms.openlocfilehash: e78571225697f745fe011c722e57c8600704c392
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="build-and-deploy-hello-mydriving-solution-tooyour-environment"></a>Hozza létre és hello MyDriving megoldás tooyour környezet telepítése
MyDriving egy az eszközök internetes hálózatát (IoT) megoldás, amely adatokat gyűjt a car, dolgozza fel a machine learning segítségével, és megadja azt a mobiltelefonjára. a Microsoft Azure által nyújtott szolgáltatások számos hello háttér áll. hello ügyfeleknek Android, iOS vagy Windows 10-es telefonok lehet.

Hello MyDriving megoldás toogive létrehozott meg létrehozni a saját IoT rendszer címeibe. A hello [GitHub tárházából MyDriving](https://github.com/Azure-Samples/MyDriving), kaphat Azure Resource Manager parancsfájlok toodeploy hello háttér-architektúra a saját Azure fiókjába. Ettől a ponttól konfigurálja újra a hello különböző szolgáltatások, a saját adatok hello lekérdezések toosuit módosítása, és így tovább. Ezek a parancsfájlok – hello mobilalkalmazás, hello Azure App Service API-projektet, és több – hello MyDriving tárházban kódjának együtt található.

Ha még nem próbálta még hello app, nézze meg hello [Get lépésekről szóló útmutatót](iot-solution-get-started.md).

Nincs a hello hello architektúra részletes fiók [MyDriving használati útmutató](http://aka.ms/mydrivingdocs). Az összegzés nincsenek több adatra, hogy beállítjuk toocreate hasonló projekt:

* A **ügyfélalkalmazás** Android, iOS és Windows 10-es telefonokon futtatja. Hello Xamarin platform tooshare használjuk százalékát hello kód, amely megtalálható a Githubon `src/MobileApp`. hello app valójában két különböző funkciókat hajtja végre:
  * Amikor az telemetriai hello (OBD) a helyi diagnosztikai eszköz, és a saját helyét szolgáltatás toohello rendszer felhő háttérből továbbítja.
  * A felhasználói felület, ahol felhasználók lekérdezhetik a rögzített közúti utazgatással kapcsolatos.
* A **felhőalapú szolgáltatás** ingests hello közúti-út adatok valós időben, és feldolgozza őket. hello főbb munkahelyi Ez a szolgáltatás létrehozása toochoose, parametrizálja, és hozzá kell fűznie mentése számos különböző Azure-szolgáltatáshoz. Néhány hello részek szükséges parancsfájlok toofilter és a folyamat hello bejövő adatok. Használjuk az Azure Resource Manager sablon tooconfigure minden hello részét.
* A **mobilszolgáltatás app** hello webszolgáltatás hello felhasználói felület részét hello eszközalkalmazás mögött. A fő folyamat tooquery hello tárolt, feldolgozott adatokat tároló adatbázis alapján. A kód megtalálható a Githubon `src/MobileAppService`.
* **Visual Studio és Xamarin** a fejlesztési környezet. Xamarin, mely már a Visual Studio összetevője mind egy önálló integrált fejlesztési környezeti (IDE), használható toobuild hello platformfüggetlen kód. toobuild hello iOS kódot, akkor a szükséges toohave az OS X gépen futó Xamarin példánya. Ha szükséges, az ügynök, a Visual Studio eszközből felügyelt futtatható.
* **Egységek tesztelése** hello eszköz alkalmazások Xamarin teszt felhőben történik.
* **GitHub** ahol tároljuk az összes hello kódot, parancsfájlok és sablonok hello tárház van.
* **A Visual Studio Team Services** egy felhőalapú szolgáltatás által használt toomanage hello folyamatos build és hello szolgáltatás és eszköz webalkalmazások vizsgálata.
* **Hockeyappra** hello kód használt toodistribute kiadásaiban van. Összeomlások és használati jelentések és a felhasználói visszajelzések is gyűjti.
* **A Visual Studio Application Insights** figyelők hello mobil webszolgáltatáshoz.

Igen nézzük meg, hogyan beállítjuk, amelyek mindegyikét. 

> [!NOTE] 
> A lépéseket követve hello számos nem kötelező.
>
>

## <a name="sign-up-for-accounts"></a>Feliratkozás a fiókok
* [A Visual Studio fejlesztői Essentials](https://www.visualstudio.com/products/visual-studio-dev-essentials-vs.aspx). A szabad program egyszerű a hozzáférés toomany fejlesztői eszközöket és a szolgáltatások, beleértve a Visual Studio, a Visual Studio Team Services és az Azure biztosít. Biztosít egy $25/ hónap jóváírás Azure 12 hónapig. Előfizetések tooPluralsight képzési és a Xamarin egyetemi is tartalmaz. Akkor is regisztrálhatnak külön szabad rétegből álló [Azure](https://azure.com) és [Visual Studio Team Services](https://www.visualstudio.com/products/visual-studio-team-services-vs.aspx), de ezek nem szabad megadni az Azure-krediteket.
* [Hockeyappra](https://rink.hockeyapp.net/) (nem kötelező), teszt terjesztési a mobilalkalmazások kezelésére és telemetriai adatokat gyűjt.
* [Xamarin](https://xamarin.com/) (kötelező), hello mobilalkalmazás és futó hibakeresési futtatása és a tesztek készítéséhez [Xamarin teszt felhő](https://xamarin.com/test-cloud).
* [GitHub](https://github.com/Azure-Samples/MyDriving/) (nem kötelező), toocreate szabad nyilvános tárolóhelyekkel Ön saját kódját (saját adattárak fizetős). Másik lehetőségként a Visual Studio Team Services saját adattárak alapszintű hello is használhatja.
* [A Power BI](https://powerbi.microsoft.com/) (nem kötelező), toocreate gazdag képi adatok hello teljes rendszer között.

> [!NOTE]
> Nincs szükség a GitHub-fiók tooaccess hello MyDriving kód [GitHub MyDriving-tárház hello](https://github.com/Azure-Samples/MyDriving).
> 
> 

## <a name="install-development-tools"></a>Fejlesztői eszközök telepítése
hello a következő várja hello teljes megoldás fejlesztése: az iOS, Android és Windows 10 Mobile, az Azure platformfüggetlen alkalmazás háttér.

Alternatív megoldásként használhatja is Xamarin Studio Mac vagy Windows toodevelop hello mobilalkalmazások Ha nem dolgozik hello Azure vissza végén.

Van egy [hosszabb leírást ehhez a telepítéshez](https://msdn.microsoft.com/library/mt613162.aspx).

### <a name="windows-development-machine"></a>A Windows fejlesztői számítógépén
a Windows hello központi eszközt Visual Studio végzett munka esetén hello MyDriving alkalmazást az Android és Windows hello App Service API-projektet és mikroszolgáltatási bővítmények.

Xamarin, Git, emulátorok és más hasznos összetevői összes integrálva vannak a Visual Studio.

Telepítés:

* [Visual Studio és Xamarin](https://www.visualstudio.com/products/visual-studio-community-vs) (minden kiadás--közösségi szabad).
* [Univerzális Windows Platform SQLite](https://visualstudiogallery.msdn.microsoft.com/4913e7d5-96c9-4dde-a1a1-69820d615936). Toobuild hello Windows 10 Mobile-kód szükséges.
* [Az Azure SDK for Visual Studio](https://www.visualstudio.com/vs/azure-tools/). Azure-beli alkalmazások, valamint Azure felügyeletére szolgáló parancssori eszközök SDK hello biztosít.
* [Az Azure Service Fabric SDK](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric). Szükséges toobuild hello [mikroszolgáltatási](../service-fabric/service-fabric-get-started.md) bővítmény.

Győződjön meg arról, hogy hello megfelelő Visual Studio bővítmény. Ellenőrizze, hogy a **eszközök**, látni **Android, iOS, Xamarin...** . Ha nem, nyissa meg a Visual Studio, keresse meg a Xamarin, és kövesse az utasításokat tooinstall hello azt. Ellenőrizze, hogy **Git for Windows** telepítve van. Ha nem, a Visual Studio, keresse meg azt, és kövesse az utasításokat tooinstall hello azt. 

### <a name="mac-development-machine"></a>Mac-fejlesztői gépére
hello Mac (Yosemite vagy újabb) szükség, ha az iOS toodevelop használni szeretne. Bár azt használja a Visual Studio with xamarin eszközt a Windows toodevelop, és az összes hello kód kezelése, Xamarin használja order toobuild a Mac számítógépen telepítve ügynök, és a bejelentkezési hello iOS kódot.

![Ki a Windows és Mac létrehozása](./media/iot-solution-build-system/image1.png)

(Másik megoldásként, használhat Xamarin Studio közvetlenül a hello Mac toodevelop platformfüggetlen alkalmazások.)

Ha nem szeretné, hogy a cél platformként tooinclude iOS, hello Mac nem szükséges.

Telepítés:

* [Az IOS rendszerhez készült Xamarin Studio](https://developer.xamarin.com/guides/ios/getting_started/installation/mac/). Is beállíthat a Visual Studio és Xamarin egy Windows rendszerű virtuális gép operációs rendszert futtató Mac számítógépen. Lásd: [beállítás, telepítés és ellenőrzés Mac](https://msdn.microsoft.com/library/mt488770.aspx) az MSDN Webhelyén.
* [Az Azure Fejlesztőeszközök](https://azure.microsoft.com/downloads/) (nem kötelező).

Hello Mac távoli bejelentkezés engedélyezése Nyissa meg **rendszerbeállítások** > **megosztás**, majd válassza ki **távoli bejelentkezési**.

Egy iOS-projekt megnyitásakor a Visual Studio, a Windows hello beépülő modul Xamarin kérni fogja az hello Mac hello azonosítója

## <a name="fetch-hello-github-repository"></a>Hello GitHub-tárházban beolvasása
Egy helyi példányát Fetch [GitHub MyDriving-tárház hello](https://github.com/Azure-Samples/MyDriving) hello segítségével **töltse le a ZIP-** gombra a Githubon, Visual Studio vagy egy másik Git-ügyfél.

Bontsa ki a hello tooa nevű mappába rövid elérési útját, például a C:\\kódot.

Másik lehetőségként Ha szeretné, tookeep toodate a mentést, vagy tooour kód hozzájárul, klónozza hello tárházat az alábbiak szerint:

**git-klón https://github.com/Azure-Samples/MyDriving.git**

## <a name="get-a-bing-maps-api-key"></a>A Bing térképek API-kulcs beszerzése
[Regisztrálja a Bing térképek API-kulcs](https://msdn.microsoft.com/library/ff428642.aspx).

Ez a sor a 22 tooreplace kell `src/MobileApps/MyDriving/MyDriving.Utils/Logger.cs`.

## <a name="build-hello-demo-app"></a>Hello bemutató alkalmazás létrehozása
A Visual Studióban nyissa meg ezek a megoldások:

* src\MobileApps\MyDriving.sln
* src\MobileAppService\MyDrivingService.sln
* src\Extensions\ServiceFabric\VINLookUpApplication\VINLookUpApplication.sln

Útmutatást kaphat:

* Néhány potenciálisan megbízhatatlan projektek megbízik. Válassza ki a tooopen őket, ha azt szeretné, hogy azokat, amelyek toogo.
* Állítsa be a fejlesztői mód, ha egy friss Windows 10-es gépen dolgozik.
* Adja meg a Xamarin hitelesítő adatokat.
* Csatlakozás toohello Xamarin Mac. Ha egy Mac nincs, kattintson a jobb gombbal hello iOS a Visual Studio projekt, és válassza **Unload projekt**.

Építse újra hello megoldás.

Ha hiba történt a épület, próbálja meg, amely azt talált hello megoldások tooquirks:

* *VINLookupApplication projekt nem tölthető be*: Ellenőrizze, hogy a hello telepítve [Azure SDK for Visual Studio](https://www.visualstudio.com/vs/azure-tools/).
* *A Service Fabric-projekt létrehozása nem*: hello felület projektek először felépítéséhez, és ellenőrizze, hogy a Service Fabric SDK hello telepítve.
* *Android-alkalmazás létrehozása nem*:
  
  * Nyissa meg **eszközök** > **Android** > **Android SDK Manager**, és győződjön meg arról, hogy Android 6 (API 23) / SDK Platform telepítve van.
  * Törölje a könyvtárat, majd építse újra:<br/>
    `%LocalAppData%\Xamarin\zips`

## <a name="get-tooknow-hello-code"></a>Tooknow hello kód beolvasása
A megoldás hello találhat:

* Azure-bővítményeket: a Service Fabric.
* Az Azure HDInsight: A parancsfájlok az Azure-ban út adatok feldolgozására.
* Mobilalkalmazások: hello eszközön futó alkalmazások.
* MobileAppsService/MyDrivingService: hello webes vissza végén.
* A Power BI: hello jelentésdefiníció.
* Parancsfájlok:
  
  * Erőforrás-kezelő: sablonok toobuild hello Azure-erőforrások.
  * PowerShell: Parancsfájlok toorun hello Resource Manager-sablonok.
  * Az Azure SQL Database: Hibakeresés adatbázisok.
* Az SQL Database: CreateTables: sémadefiníciók.
* Az Azure Stream Analytics: Lekérdezi, hogy átalakító hello bejövő adatfolyam.

## <a name="run-hello-apps-in-development-mode"></a>Hello alkalmazások fejlesztői módban fut
Hajtsa végre a művelet toorun hello alkalmazások használata hello eszköz alapján:

* Háttér: Set MyDrivingService hello kezdőprojektként, és nyomja le az F5 toorun hello webes szolgáltatás. Az hello API listaelem böngésző nézetét nyílik meg.
* A mobil ügyfelek: hello [mobilalkalmazások a Xamarin fejlesztett](https://developer.xamarin.com/guides/cross-platform/deployment,_testing,_and_metrics/debugging_with_xamarin/).
  
  * Android: További információkért lásd: [Android-hibakeresés a Xamarin](http://developer.xamarin.com/guides/android/deployment,_testing,_and_metrics/debugging_with_xamarin_android/).
  * iOS: további információkért lásd: [iOS-hibakeresést](http://developer.xamarin.com/guides/ios/deployment,_testing,_and_metrics/debugging_in_xamarin_ios/).
  * Windows Phone: További információkért lásd: [Xamarin + Windows Phone](https://developer.xamarin.com/guides/cross-platform/windows/phone/).

## <a name="upload-hello-mobile-app-toohockeyapp"></a>Hello mobilalkalmazás tooHockeyApp feltöltése
Hockeyappra kezeli a hello terjesztési az Android, iOS vagy Windows app tootest a felhasználók új kiadásainak felhasználók értesítésére. Hasznos összeomlási jelentések, felhasználói visszajelzés képernyőképeket és a szoftverhasználati mérési adatok is gyűjti.

[Első lépésként feltöltése](http://support.hockeyapp.net/kb/app-management-2/how-to-create-a-new-app) a build alkalmazást. Jelentkezzen be a túl[Hockeyappra](https://rink.hockeyapp.net) a fejlesztői gépen. Az hello fejlesztői irányítópultján kattintson **új alkalmazás**, és húzza a beépített hello fájlok hello ablak. (Később, automatizálhatja az összeállítási szolgáltatás toodo ez.)

Most még az alkalmazás irányítópulton.

![Hello alkalmazás irányítópult – Áttekintés lapra](./media/iot-solution-build-system/image2.png)

Ismételje meg minden egyes platform, amely az alkalmazás fut hello folyamat. Ezután elvégezhető hello következő:

* Használjon hello [Alkalmazásazonosító](http://support.hockeyapp.net/kb/app-management-2/how-to-find-the-app-id) hello irányítópult toosend összeomlási adatait és visszajelzés az alkalmazásból. A MyDriving frissítse a src/MobileApps/MyDriving/MyDriving.Utils/Logger.cs hello azonosítók.
* [Tesztfelhasználók meghívása](http://support.hockeyapp.net/kb/app-management-2/how-to-invite-beta-testers). Egy URL-cím toorecruit tesztelők webhelyről felhasználók beolvasása. Akkor lesz feliratkozott a csapat tudja toosign, hello alkalmazást tölthet le, és visszajelzés küldése.
* Ha több nyitott bétaverzió inkább, állítsa be a hello terjesztési toopublic. Kattintson a **alkalmazás kezelése** > **terjesztési** > **letöltése = nyilvános**. Most a visszajelzés küldése bárki és töltse le az alkalmazást, és azok megjelenik egy értesítés, ha új verzióra. Egyes összeomlási jelentések túl lehet lekérdezni a őket.
  
   ![Csoportok hello irányítópult](./media/iot-solution-build-system/image3.png)
* [Hivatkozás összeomlási jelentések tooVisual Studio Team Services](http://support.hockeyapp.net/kb/third-party-bug-trackers-services-and-webhooks/how-to-use-hockeyapp-with-visual-studio-team-services-vsts-or-team-foundation-server-tfs). Kattintson a **alkalmazás kezelése** > **Visual Studio Team Services**. Hockeyappra automatikusan hozhat létre munkaelemeket a Team Services Ha összeomlási jelentések vagy visszajelzés fogadásakor.

Több hello: olvasási [Hockeyappra hely](https://hockeyapp.net).

## <a name="test-hello-mobile-app-on-xamarin-test-cloud"></a>Xamarin teszt felhő hello mobilalkalmazás tesztelése
[Xamarin teszt felhő](https://developer.xamarin.com/guides/testcloud/introduction-to-test-cloud/) automatizálja a felhasználói felület tesztelése hello felhőben valós eszközökön. Hello NUnit keretrendszer használatával írt teszteket, az alkalmazás futtatása hello felhasználói felületen keresztül.

toouse Xamarin, használhatja az hello [Xamarin.UITests](https://developer.xamarin.com/guides/testcloud/uitest/intro-to-uitest/) SDK az alkalmazásba, amely a NuGet-csomag. Hello bemutató alkalmazás találhatja meg, és, ha hello Xamarin sablonok létrehozásához az új vizsgálat projektek tartalmazott.

![Ha toofind hello többplatformos SDK hello kapcsolaton](./media/iot-solution-build-system/image4.png)

Egy példaprojekt teszt hello tárházban hello alkalmazás része. A [MyDriving](https://github.com/Azure-Samples/MyDriving/tree/master/src/MobileAppService), keresse meg a [src](https://github.com/Azure-Samples/MyDriving/tree/master/src)/MobileApps/[MyDriving](https://github.com/Azure-Samples/MyDriving/tree/master/src/MobileApps/MyDriving)/MyDriving.UITests/.

Visual Studio Team Services build használatához esetén könnyen toowrite Xamarin felhasználói felület egység teszteli, és futtassa azokat a build részeként.

## <a name="deploy-azure-services"></a>Azure-szolgáltatások telepítése
egy automatikus központi telepítési Team Services build szolgáltatások, és az Azure-szolgáltatások tooperform toohello tekintse meg a részletes utasításokat a **scripts/README.md**.

A Microsoft Azure kínál számos különböző szolgáltatások használható toobuild felhőalapú alkalmazásokhoz. Bár számos külön-külön (például az App Service vagy webes alkalmazások) is használható, fontosságúak, a legjobb when összeköttetésben van az integrált rendszer tooform hasonlóan használjuk a MyDriving.

Lehetséges toocreate, és manuálisan kapcsolódnak össze az Azure-szolgáltatások, de sokkal hatékonyabb és sokkal megbízhatóbb toouse Azure Resource Manager-sablonok. [Erőforrás-kezelő](../azure-resource-manager/resource-group-overview.md) automatizálja a megoldáshoz tartozó erőforrások és a közöttük hello csatlakozás elvégzése hello telepítését.

Hello sablont találhat hello MyDriving rendszer hello GitHub-tárházban alatt [parancsfájlok/ARM](https://github.com/Azure-Samples/MyDriving/tree/master/scripts/ARM). Azt jeleníti meg az átfogó és pontos sikerkritériumokkal biztosíthatja hogyan hello különböző szolgáltatásokhoz a architektúrában vannak összekapcsolva. Azt ismertetik, hogy a fenti részletes hello [MyDriving használati útmutató](http://aka.ms/mydrivingdocs), de csak olvasásával maga hello sablon segítségével megismerheti a nagy.

> [!NOTE]
> Az Azure szolgáltatások társított költség szempontjából, attól függően, hogy az IP-címek hello rendelkeznek. Ha új tooAzure, akkor [ingyenesen kipróbálhatja](https://azure.microsoft.com/free/). Azonban ha nem tervezi toouse hello MyDriving rendszer az egyes összetevők, lehet, hogy tooremove tooavoid ezzel járó költségek őket. hello "Becsült működési költségek" szakaszban később ebben a cikkben a tipikus szolgáltatás költségek összegzését tartalmazza.
> 
> 

### <a name="edit-hello-template"></a>Hello sablon szerkesztése
toocustomize a telepítés esetleg tooremove felesleges összetevőket vagy tooadd mások, először másolatokat forgatókönyv\_complete.params.json és forgatókönyv\_complete.json mely toomake megváltozik.

Hello forgatókönyvet használhatja\_complete.params.json fájl toooverride különböző alapértelmezett értékei, hello szolgáltatás SKU vagy hello replikációs tárolótípus, például hello a következő táblázatban leírtak szerint. hello alapértelmezett értékek választhatók hello legalacsonyabb költséget.

| **A paraméter** | **Leírás** | **Alapértelmezett érték** |
| --- | --- | --- |
| Az IoT-központ Termékváltozat |Réteg Azure IoT-központ szolgáltatás |F1 |
| Tárfiók típusa |Tárolási replikáció típusa |Standard-LRS |
| SQL-szolgáltatási cél |Párhuzamossági tárolóhely-használat |DW100 |
| Üzemeltetési terv Termékváltozat |App Service szolgáltatás tervezése |F1 |

A forgatókönyvben\_complete.json:

* Keresse meg a "baseName", és módosítsa úgy az előnyben részesített tooa neve.
* Keresse meg a "Create". Ezek a szakaszok mindegyikének hoz létre egy erőforrást.
* SqlServerAdminLogin és sqlServerAdminPassword toosuitable értékeinek beállításához.
* Egy szakaszt, amely létrehoz egy erőforrás törlése előtt ellenőrizze, hogy azt van függő elemek a részeiben hello fájl nevét keresve. Vegye figyelembe, hogy minden, a szolgáltatás létrehozása rész tartalmaz egy *dependsOn* szakaszt, amely tartalmazza annak függőségeit.

Az alábbiakban milyen hello sablon konfigurálja. Részletek szerepelnek hello [használati útmutató](http://aka.ms/mydrivingdocs).

| **Szolgáltatás** | **Leírás és részletek** |
| --- | --- |
| Tárfiókok |hello sablon három fiókot hoz létre: |
| – SQL-adatbázis, amely összesített telemetriai adatokat fogad a Stream Analytics, és az Azure App Service-táblázatot, amely ezeket az adatokat API-végpontokon keresztül teszi közzé a hello biztonsági tárolóként szolgál. | |
| -Blob-tároló, amely egy másik Stream Analytics-feladat, a HDInsight által feldolgozott toobe az előzményadatok összesít. | |
| – SQL-adatbázis, amely megkapja a Power bi-ban való használathoz a HDInsight által feldolgozott eredmények. | |
| Azure IoT Hub |Megállapítja a kétirányú kapcsolat tooeach csatlakoztatott eszközön. Hello MyDriving megoldás, a hello mobilalkalmazás mező átjáró toosend adatok tooAzure IoT-központ funkcionál. Az Azure IoT Hub majd egy bemeneti tooStream Analytics funkcionál. |
| Azure Event Hubs |Egy, a várólisták hello Azure Service Fabric használatával létrehozott kimeneti tooextensions Stream Analytics-feladat kimeneti. |
| Azure SQL Data Warehouse | |
| Stream Analytics-feladatok |Csatlakozás egy lekérdezést, amely használt tooaggregate bemenetekhez és kimenetekhez hello App Service API-k, az Azure Machine Learning, a bővítmények és a Power bi-ban a valós idejű és a korábbi adatok. |
| Machine Learning-munkaterület |Kísérletek, az R-kód és a API szolgáltatást tartalmazza. |
| Azure Data Factory |Ütemezett Machine Learning átképezési. |
| A Service Fabric üzemeltetési terv |A bővítmények. |
| Az App Service ("mobilalkalmazás") |Gazdagépek hello Mobile Apps API-projektet, amely végpontok biztosít hello mobilalkalmazás. API-kódban hello szolgáltatás a Visual Studio eszközből üzembe helyezett tooApp kell lennie. |
| A riasztási szabályok |Elküldi az e-mailben elküldi hello app válaszok hibákat jelzik. |
| Application Insights |Az App Service API-k hello teljesítményfigyeléshez. A Visual Studio tooconfigure hello kapcsolattal rendelkezik. |
| Azure Key Vault |Hello web service fürt tanúsítvány mentéséhez. |

### <a name="run-hello-template"></a>Futtassa a hello sablon
A **scripts/README.md**, futó hello sablon részletes utasításokat is.

tooprovision szolgáltatásokról a saját Azure-fiók hello parancsfájl használatával teheti a következő hello:

* Használja a Powershellt:
  
  ```
  
  cd scripts/PowerShell;
  deploy.ps1 *location* *resourceGroupName*
  ```
  
  * *hely* hello van [Azure-beli hely](https://azure.microsoft.com/regions/), például a `North Europe` vagy `West US`. Használjon `Get-AzureLocation` toofind elérhető helyek listáját.
  * *resourceGroupName* toogive toohello csoportba tartozó összes hello erőforrás hello név. Ha befejezte, hello erőforrásokkal, törölhetők együtt ez a csoport törlésével.
* Futtassa a DeploymentScripts/Bash/deploy.sh Bash.
* Nyissa meg, és hello Visual Studio megoldás DeploymentScripts/VS/DeployARM.sln.

Vegye figyelembe, hogy minden alkalommal hello sablon futtatja, akkor hoz létre új erőforrások új névvel. toodelete hello erőforrások toohello portálon lépjen, és hello erőforráscsoportot.

Hello parancsfájl bármilyen okból nem sikerül, ha újra futtathatja.

hello parancsfájl által biztosított folyamatos integrációt beállíthatja a Visual Studio Team Services hello meg. Ha meg van egy Team Services projektet, konfigurálnia kell egy URL-cím: https://yourAccountName.visualstudio.com. Ha megkérdezi, adja meg hello teljes URL-címet. Ez egy új vagy meglévő nevet adhat Team Services projekt.

## <a name="set-up-build-and-test-definitions-in-visual-studio-team-services"></a>Build beállítása és tesztelése a definíciók a Visual Studio Team Services
Team Services felhasználása a projekten főleg a a build és tesztelje a szolgáltatásokat. De is biztosít kiváló együttműködés támogatása, például a tevékenység kezelésének Kanban modulok, kód tekintse át a feladatok és a verziókövetési integrált, és által kezdeményezett alkot. Jól integrálható a más eszközökkel, például a Githubon, Xamarin, Hockeyappra és természetesen Visual Studio. Hello webes felületen keresztül vagy a Visual Studio alkalmazással elérheti, amelyik kényelmesebb bármikor.

hello hello építés lépések és kiadás definíciókat használja a beépülő modul által biztosított hello Team Services szolgáltatások számos [piactér](https://marketplace.visualstudio.com/VSTS). Továbbá toobasic segédprogramok toorun parancssor vagy a fájlok másolása nincsenek szolgáltatások, amelyek aktiválják az Xamarin Android és más gyártók által buildek és tooHockeyApp csatlakozzon, amely.

![Build Team Services lehetőségeit](./media/iot-solution-build-system/image5.png)

### <a name="build-definitions"></a>Definíciók létrehozása
Az egyes hello fő cél a build definíciók van. A szolgáltatás és a regressziós teszteléshez változata is van. Biztosítanak számunkra:

* MyDriving.Services (hello mobilalkalmazás hello webes alkalmazás)
* MyDriving.Xamarin.Android
  
  * MyDriving.Xamarin.Android-funkció
  * MyDriving.Xamarin.Android-regressziós
* MyDriving.Xamarin.iOS
  
  * MyDriving.Xamarin.iOS-funkció
  * MyDriving.Xamarin.iOS-regressziós
* MyDriving.Xamarin.UWP
  
  * MyDriving.Xamarin.UWP-funkció
  * MyDriving.Xamarin.UWP-regressziós

Ha azt szeretné, hogy a konfiguráció toosee hello részletesen, című rész 4.7 hello [MyDriving használati útmutató](http://aka.ms/mydrivingdocs), "Build és kiadás konfigurációs." Hello követik ugyanazt. hello parancsfájlt:

1. Visszaállítja a hello NuGet-csomagot. Azt nem kell megőrizni lefordított kódot hello tárházban, így az első lépéseket hello minden build toorestore hello szükséges NuGet-csomagok.
2. Hello licenc aktiválja. hello build történik hello felhőben, így ha igazolnia kell a licenc – különösen a Xamarin build szolgáltatás – hello tudunk tooactivate gépen hello aktuális build a licenc. Ezt követően azt inaktiválásához ezt követően azonnal tooallow azt egy másik gépen használt toobe.
3. Buildek hello megfelelő szolgáltatással. Xamarin buildek hello mobilalkalmazásokhoz használjuk, és a Visual Studio létrehozza a hello webes szolgáltatás.
4. Tesztek alkot.
5. Tesztet futtatja. Futtatás hello mobilalkalmazás tesztek Xamarin teszt felhőben.
6. Közzéteszi a hello build eredmény toohello eldobott üzenetek helye.

hello eseményindítója a következőnek: hello fő buildek toocontinuous integrációs van beállítva. Ez azt jelenti, hogy hello build fut minden alkalommal, amikor a kód be van jelölve, a toohello főághoz.

![Csatoló set toocontinuous integrációs hello eseményindító esetén](./media/iot-solution-build-system/image6.png)

### <a name="release-definitions"></a>Kiadás definíciók
Kiadás definíciók beállítása sok hello azonos módon.

Hello webszolgáltatáshoz beállítjuk, egy Azure webalkalmazás telepítése:

![Felület érhető el az Azure-webalkalmazás, üzembe helyezés beállítása](./media/iot-solution-build-system/image7.png)

És hello kiadás eseményindító toocontinuous telepítési hivatott. Ez azt jelenti, hogy minden be követ egy frissítés toohello webes alkalmazás sikeres build eredményez.

![Csatoló hello kiadás eseményindító toocontinuous telepítési beállításához](./media/iot-solution-build-system/image8.png)

Mobileszköz-alkalmazások esetén azt telepíteni tooHockeyApp:

![A mobilalkalmazás tooHockeyApp telepítéséhez felület](./media/iot-solution-build-system/image9.png)

## <a name="explore-telemetry-by-using-application-insights"></a>Application Insights segítségével megismerkedhet a telemetriai adat
[Az Application Insights](../application-insights/app-insights-overview.md) hello teljesítmény és a webes szolgáltatások használatával kapcsolatos telemetriai adatokat gyűjt. Application Insights SDK hello telemetriai küldi hello szolgáltatás toohello Application Insights-erőforrást az Azure-ban.

Keresse meg az beállítása hello sablon toohello Application Insights-erőforrást. Hiba, ismerje meg a hello teljesítésének diagramok a [Mobile App Service-projekt](https://github.com/Azure-Samples/MyDriving/tree/master/src/MobileAppService). Írják le a kiszolgálói kérelem és a válaszidők, hibák, és a kivétel számát. Függőség válaszidejének – Ez azt jelenti, hogy a hívások toohello adatbázis és a például a Machine Learning API-k tooREST diagramok is vannak. Ha bármely teljesítményproblémákat, lesz képes toosee a rendszer melyik adat okozza-e őket.

![Példa teljesítménydiagramban](./media/iot-solution-build-system/image11.png)

Ha egy webszolgáltatás, amelyet kézzel állítsa be, az egyszerű tooget hello azonos diagramokat. Hello web service paneljén kattintson **eszközök** > **bővítmények** > **Hozzáadás**. Válassza ki **az Application Insights**.

![Az Application Insights tooget hello diagramok kiválasztásáról felület](./media/iot-solution-build-system/image12.png)

hello szolgáltatás által az alkalmazás az Application Insights SDK hello tagolása működik.

Is hozzáadhat egyéni telemetria (vagy egy alkalmazás kívül Azure valahol futtató eszköz) [Application Insights SDK hozzáadása hello](../application-insights/app-insights-asp-net.md) fejlesztése során. Ez akkor hasznos toolog metrikák hello alkalmazás, például a felhasználók átlagos út hossza vagy teljes távolság függnek. A Visual Studióban, kattintson a jobb gombbal a hello projektet, és válassza **Application Insights hozzáadása**.

![Az Application Insights hozzáadása tooadd egyéni telemetria kiválasztásáról felület](./media/iot-solution-build-system/image10.png)

Az Application Insights küld értesítő e-mailek, ha azt látja, hogy hiba válaszok szokatlan számát. Saját riasztások különböző mérőszámokat, például a válaszidők is állíthat be.

Csak toobe, hogy a webszolgáltatás mindig naprakészek és fut, akkor állíthatja [rendelkezésreállás figyelésére szolgáló tesztek](../application-insights/app-insights-monitor-web-app-availability.md). Ezek a tesztek 15 percenként Pingelje meg a webhely hello világ különböző helyekről. Ebben az esetben egy e-mailt fog kapni, ha úgy tűnik, probléma toobe.

## <a name="estimate-operational-costs"></a>A működési költségek becslése
Meglehetősen alacsony költségű toorun egy ehhez hasonló kis léptékű alkalmazást is. Hello szolgáltatások számos lennie szabad kezdő rétegek, fejlesztési és a művelet csak kevés számítógépet érintő költséget nagyon kevés. És természetesen a saját alkalmazásai toouse MyDriving bemutatott összes hello szolgáltatás nincs-e.

Ez egy durva becsült a költségek MyDriving hello fejlesztési konfigurációjának beállítása. Azt is vegye figyelembe azt elvégző néhány más *nem* használja. Ezek az információk hasznosak lehetnek a saját költségek becslései szerint.

Feltételezzük, hogy:

* Legfeljebb ötre csapata (és az érdekelt felek betartásával).
* A hónap kapcsolatos futtatja.
* 100 felhasználók naponta négy való adatváltások számát.

> [!NOTE]
> Új tooAzure, ha van egy [ingyenes fiókot](https://azure.microsoft.com/free/).
> 
> 

| **Szolgáltatás-összetevő** | **Megjegyzések** | **Költség/hónap** |
| --- | --- | --- |
| [A Visual Studio 2015 közösségi](https://www.visualstudio.com/products/visual-studio-community-vs) rendelkező [Xamarin](https://visualstudiogallery.msdn.microsoft.com/dcd5b7bd-48f0-4245-80b6-002d22ea6eee) <br/>Platformok közötti fejlesztői környezet |A Visual Studio Community. (Kell [Visual Studio Professional](https://www.visualstudio.com/vs-2015-product-editions) a [Xamarin.Forms](https://xamarin.com/forms), toodesign többplatformos egyetlen kódbázis.) |$0 |
| [Azure IoT Hub](https://azure.microsoft.com/pricing/details/iot-hub/) <br/>Kétirányú adatok kapcsolat toodevices |8000 üzenetek + 0,5 KB/üzenet szabad. |$0 |
| [Stream Analytics](https://azure.microsoft.com/pricing/details/stream-analytics/)  <br/>   Nagy mennyiségű adatfolyam adatok feldolgozása |A streaming unit, óránként engedélyezett állapota mellett / 0.031 $ kell fizetni. Úgy dönt, hogy a streamelési egységek kívánt; hello száma További tooscale fel. |$23 |
| [Machine Learning](https://azure.microsoft.com/documentation/services/machine-learning/)<br/> Adaptív válaszok |$10/ülések/hónap. <br/>                                                                                                                                                                                 + 3 órás kísérlet \* 1 USD / kísérletezhet óra. <br/>                                                                                                                                                           + 3.5 órás API CPU \* $2 / éles CPU óra. <br/>                                                                                                                                                          Bár ez több bemeneti adatokkal kellene növekedésnek API CPU-idő feltételezi, hogy átképezési, azaz 5 perc/nap.                   <br/>                                                                                                                                                                     + 2 perc/nap pontozási tooprocess 400 utazgatással/nap. |$20 |
| [APP SERVICE](https://azure.microsoft.com/pricing/details/app-service/)  <br/> Mobil háttér gazdagép |A K1 rétegbeli--éles webalkalmazások. |$56 |
| [A Visual Studio Team Services](https://azure.microsoft.com/pricing/details/visual-studio-team-services/)  <br/> Build, egységteszt, és a kiadáskezelés; tevékenység kezelése |Személyes ügynökök öt felhasználók. |$0 |
| [Application Insights](https://azure.microsoft.com/pricing/details/application-insights/) <br/>A teljesítmény és a web services és a helyek figyelése |Ingyenes szint. |$0 |
| [Hockeyappra](http://hockeyapp.net/pricing/) <br/> Terjesztési beta alkalmazások, valamint visszajelzést, használatát és összeomlási adatok gyűjteménye |Új felhasználók két ingyenes alkalmazásokat.<br/> $30/ hónap ezt követően. |$0 |
| [Xamarin](https://store.xamarin.com/)<br/> Egy egységes platform több eszközök kód |Ingyenes próbalehetőség. <br/>$25/ hónap ezt követően. |$0 |
| [SQL-adatbázis](https://azure.microsoft.com/pricing/details/sql-database/) Azure App Service |Az alapszintű csomag; önálló adatbázis modell. |$5 |
| [A Service Fabric](https://azure.microsoft.com/pricing/details/service-fabric/) (nem kötelező) |Futtassa a helyi fürthöz. |$0 |
| [Power BI](https://powerbi.microsoft.com/pricing/)<br/> Adatfolyamként továbbított és statikus adatok vizsgálata és sokoldalú jeleníti meg |Ingyenes szint: 1 GB-os, 10 000 sorok óránként, naponta frissítés. <br/> $10/felhasználó/hónap [magasabb korlátok](https://powerbi.microsoft.com/documentation/powerbi-power-bi-pro-content-what-is-it/), további kapcsolati beállítások, együttműködés. |$0 |
| [Tárolás](https://azure.microsoft.com/pricing/details/storage/) |L (helyileg redundáns) &lt; 100 G $0.024/GB. |$3 |
| [Data Factory](https://azure.microsoft.com/pricing/details/data-factory/) |$0,60 / tevékenység \* (8-5 FOC). |$2 |
| [HDInsight](https://azure.microsoft.com/pricing/details/hdinsight/) <br/>  Igény szerinti fürtöt a napi átképezési |Három A3 csomópontok $0.32 óránként, naponta 1 óráig * 31 nap. |$30 |
| [Event Hubs](https://azure.microsoft.com/pricing/details/event-hubs/) |Alapszintű $11/ hónap átviteli egység $0,028 érkező kiegészítve. |$11 |
| OBD hardverkulcs | |$12 |
| **Összesen** | |**$157** |

További információkért lásd:

* Az összefoglaló [Azure szolgáltatás kvótái és korlátai](../azure-subscription-service-limits.md#iot-hub-limits)
* Azure [árképzési Számológép](https://azure.microsoft.com/pricing/calculator/)

## <a name="send-us-your-feedback"></a>Visszajelzés küldése
A saját IoT rendszerek létrehozott MyDriving toohelp ismertető, mert biztosan szeretnénk toohear az Ön kapcsolatos mennyire működik. Értesítsen minket, ha:

* Nehézségek vagy kihívásokkal futtatja.
* Van így könnyebben megfelelőbbek tooyour forgatókönyv bővítmény pont.
* Egy sokkal hatékonyabb módja tooaccomplish található egyes igényeinek.
* Javaslatai bármely más MyDriving vagy ebben a dokumentációban javítására.

toogive visszajelzést, [a Githubon problémát] fájl, vagy hagyja meg az alábbi megjegyzést (en-us edition).

Számítunk toohearing az Ön!

## <a name="next-steps"></a>Következő lépések
Azt javasoljuk, hogy hello [MyDriving használati útmutató](http://aka.ms/mydrivingdocs), vagyis hello tervezési hello rendszer és az összetevőinek átfogó leírást.

