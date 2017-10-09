---
title: "aaaDeploy az alkalmazás tooAzure App Service |} Microsoft Docs"
description: "Megtudhatja, hogyan toodeploy az alkalmazás tooAzure App Service."
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: 
ms.assetid: f1464f71-2624-400e-86a2-e687e385804f
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/05/2017
ms.author: cephalin
ms.openlocfilehash: 5c84e4ca502874209d750c94efeb86a59aa71a48
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-your-app-tooazure-app-service"></a>Az alkalmazás tooAzure App Service telepítése
Ez a cikk segít a hello ajánlott beállítás toodeploy hello fájlok a webalkalmazást, mobil-háttéralkalmazás vagy API-alkalmazás túl meghatározni[Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714), és majd végig is vezeti Önt utasításokat adott tooyour tooappropriate erőforrások előnyben részesített beállítás.

## <a name="overview"></a>Az Azure App Service telepítésének áttekintése
Az Azure App Service alkalmazás-keretrendszer hello az Ön (az ASP.NET, PHP, Node.js, stb.) kezeli. Néhány keretrendszerek például Java és Python, mások pedig alapértelmezés szerint engedélyezve vannak, előfordulhat, hogy egy egyszerű pipa konfigurációs tooenable kell azt. Emellett testre szabhatja az alkalmazás-keretrendszer, például hello PHP verzióját és a futásidejű hello bitszámának. További információkért lásd: [állítsa be alkalmazását az Azure App Service](web-sites-configure.md).

Nincs tooworry kapcsolatos hello web server vagy az alkalmazás-keretrendszer, mivel az alkalmazás tooApp szolgáltatás telepítése központi telepítése a kódot, bináris fájljai, fájlt és a megfelelő könyvtárstruktúrát, toohello [   **/Helykonfiguráció /WWWRoot** directory](https://github.com/projectkudu/kudu/wiki/File-structure-on-azure) az Azure-ban (vagy hello **/hely/wwwroot/App_Data/feladatok/** könyvtár a webjobs-feladatok). App Service három különböző központi telepítési folyamat támogatja. A cikkben szereplő összes hello központi telepítési módszerek hello a következő eljárások valamelyikével: 

* [FTP- vagy ftps-t](https://en.wikipedia.org/wiki/File_Transfer_Protocol): a kedvenc FTP vagy ftps-t használja-e jelölve eszköz toomove a fájlok tooAzure [FileZilla](https://filezilla-project.org) toofull körű IDEs, például [NetBeans](https://netbeans.org). Ez az szigorúan csak a fájl feltöltési folyamat. Nincsenek további szolgáltatások App Service által biztosított például verziókezelést, Fájlkezelés struktúra stb. 
* [A kudu (Git/Mercurial vagy a onedrive-on vagy Dropbox)](https://github.com/projectkudu/kudu/wiki/Deployment): Kudu hello [telepítési motor](https://github.com/projectkudu/kudu/wiki) az App Service-ben. A kód tooKudu leküldéses közvetlenül a tárházból. A kudu szolgáltatásokat is biztosít a hozzáadott kódot fejlesztőre tooit, például verziókezelést, csomag visszaállítási MSBuild, amikor és [hurkok webes](https://github.com/projectkudu/kudu/wiki/Web-hooks) folyamatos üzembe helyezés és egyéb automatizálási feladatok. hello Kudu telepítési motor a telepítési források 3 különböző típusú támogatja:   
  
  * A onedrive vállalati verzió és a Dropbox tartalom szinkronizálása   
  * Tárház-alapú folyamatos üzembe helyezés a Visual Studio Team Services, GitHub és Bitbucket automatikus szinkronizálása  
  * Tárház-alapú telepítés a helyi Git-manuális szinkronizálással  
* [Web Deploy](http://www.iis.net/learn/publish/using-web-deploy/introduction-to-web-deploy): telepítés kód tooApp közvetlenül a a kedvenc Microsoft eszközök, például a Visual Studio használatával, amely automatizálja a központi telepítés tooIIS kiszolgálók azonos tooling hello. Az eszköz támogatja a csak különbözeti telepítés, az adatbázis létrehozása, átalakítások kapcsolati karakterláncok stb. A Web Deploy a Kudu különbözik, hogy bináris fájlok még nem készült alkalmazás központi telepítése tooAzure. Hasonló tooFTP szolgáltatások nélkül App Service által biztosított.

Népszerű webes fejlesztési eszközök egy vagy több központi telepítési folyamatok támogatja. Mialatt hello választja eszközt meghatározza hello folyamatok kihasználhatja, hello tényleges DevOps a rendelkezésére függ hello kombinációja hello telepítési folyamat és a hello eszközöket. Például, ha hajt végre, a Web Deploy [Visual Studio Azure SDK-val](#vspros), annak ellenére, hogy a kudu automation nem kap, a Visual Studio csomag visszaállítása és MSBuild automation beolvasása. 

> [!NOTE]
> A központi telepítési folyamatok ténylegesen nem [rendelkezés hello Azure-erőforrások](../azure-resource-manager/resource-group-template-deploy-portal.md) , amely az alkalmazás esetleg. Azonban kapcsolódó hello hogyan-tooarticles a legtöbb megtudhatja, hogyan tooprovision hello alkalmazás és központi telepítése a kód tooit-végpontok. Azure-erőforrások hello kialakítási további beállítások is megkeresheti [parancssori eszközök segítségével automatizálni telepítését](#automate) szakasz.
> 
> 

## <a name="ftp"></a>FTP-vel fájlok feltöltésével manuális telepítése
Ha a webes tartalom tooa webkiszolgáló másolása használt toomanually, használhatja az [FTP](http://en.wikipedia.org/wiki/File_Transfer_Protocol) segédprogram toocopy fájlok, például a Windows Explorer vagy [FileZilla](https://filezilla-project.org/).

a fájlok manuális másolását hello szakemberek a következők:

* A tesztkörnyezet és az FTP-tooling minimális összetettségi. 
* Annak ismerete, pontosan ahol a fájlokat fogja.
* A biztonságosabb a ftps-t.

hello hátrányait fájlokat manuálisan másolja a következők:

* Ha tooknow hogyan toodeploy fájlok az App Service toohello megfelelő könyvtárak. 
* Nincs verziókezelést a visszaállításhoz, amikor hiba történik.
* Az a telepítési problémák elhárítása beépített üzembe helyezési előzményeket.
* Lehetséges hosszú telepítési időpontban, mert több FTP-eszközök nem csak különbözeti másolását és egyszerűen másolhatja az összes hello fájlt.  

### <a name="howtoftp"></a>Hogyan tooupload fájlok FTP-vel
Hello [Azure Portal](https://portal.azure.com) lehetővé teszi az FTP- vagy ftps-t használó tooconnect tooyour app könyvtárak szükséges összes hello információkat.

* [Az alkalmazás tooAzure App Service telepítése FTP használatával](app-service-deploy-ftp.md)

## <a name="dropbox"></a>Szinkronizálás a felhő mappa telepíteni
Egy megfelelő alternatívát használjanak túl[fájlok manuális másolását](#ftp) egy felhőalapú tárolási szolgáltatásba, például a onedrive vállalati verzió és a Dropbox a fájlok és mappák tooApp szolgáltatás szinkronizál. Szinkronizálás a felhő mappa hello Kudu telepítési folyamata használja (lásd: [folyamatok áttekintése](#overview)).

egy felhőalapú mappa feladatai hello szakemberek a következők:

* Egyszerű telepítés. Szolgáltatások, például a onedrive vállalati verzió és a Dropbox asztali szinkronizálási ügyfelek is biztosítsanak, így a helyi munkakönyvtár is a telepítési könyvtárban.
* Egy kattintással üzembe helyezése.
* Hello Kudu telepítési motor az összes funkció érhető el (például a csomag visszaállítási, az automation).

szinkronizálás a felhő mappa hello hátrányait a következők:

* Nincs verziókezelést a visszaállításhoz, amikor hiba történik.
* Nincs automatikus telepítési manuális szinkronizálás szükség.

### <a name="howtodropbox"></a>Hogyan toodeploy által felhő mappát szinkronizálása
A hello [Azure Portal](https://portal.azure.com), akkor meghatározhat egy mappát, a tartalom szinkronizálása a onedrive-on vagy a Dropbox felhőbeli tárolóhelyen, az alkalmazás és a mappában található tartalom, és szinkronizálási tooApp szolgáltatás a hello kattintson gomb.

* [Szinkronizálja a tartalmat egy felhőalapú mappa tooAzure App Service](app-service-deploy-content-sync.md). 

## <a name="continuousdeployment"></a>A felhő alapú forrás vezérlő szolgáltatásból folyamatosan központi telepítése
Ha a fejlesztési csapat egy felhőalapú forrás kódot (SCM) szolgáltatás például [Visual Studio Team Services](http://www.visualstudio.com/), [GitHub](https://www.github.com), vagy [BitBucket](https://bitbucket.org/), konfigurálhatja az alkalmazás A tárház a toointegrate szolgáltatást, és folyamatosan telepítése. 

a felhő alapú forrás vezérlő szolgáltatásból történő telepítésének hello szakemberek a következők:

* Verzió vezérlő tooenable visszaállítás.
* Képes tooconfigure folyamatos üzembe helyezés a Git (és adott esetben Mercurial) tárházak találhatók. 
* Fiókiroda-specifikus telepítési telepítheti ággal toodifferent [üzembe helyezési ponti](web-sites-staged-publishing.md).
* Hello Kudu telepítési motor az összes funkció érhető el (központi telepítési versioning, visszavonás, csomag visszaállítási, automation).

a felhő alapú forrás vezérlő szolgáltatásból történő telepítésének hello con van:

* Néhány hello szükséges megfelelő SCM szolgáltatás ismerete.

### <a name="vsts"></a>Hogyan folyamatosan forrásból felhőalapú toodeploy szabályozza-e a szolgáltatás
A hello [Azure Portal](https://portal.azure.com), konfigurálhatja a Visual Studio Team Services, GitHub és Bitbucket folyamatos üzembe helyezést.

* [Folyamatos telepítés tooAzure App Service](app-service-continuous-deployment.md). 

toofind ki hogyan tooconfigure manuálisan egy felhőalapú-tárházat nem szerepel a folyamatos üzembe helyezés hello Azure Portal (például [GitLab](https://gitlab.com/)), lásd: [manuális lépéseket követve folyamatos üzembe helyezés beállítása](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

## <a name="localgitdeployment"></a>A helyi Git telepítése
Ha a fejlesztési csapat egy helyszíni helyi forrás kódot (SCM) felügyeleti szolgáltatás Git alapján, ez beállíthat egy központi telepítési forrás tooApp szolgáltatás. 

Informatikai szakemberek a helyi Git üzembe a következők:

* Verzió vezérlő tooenable visszaállítás.
* Fiókiroda-specifikus telepítési telepítheti ággal toodifferent [üzembe helyezési ponti](web-sites-staged-publishing.md).
* Hello Kudu telepítési motor az összes funkció érhető el (központi telepítési versioning, visszavonás, csomag visszaállítási, automation).

A helyi Git telepítése hátrányait van:

* Néhány hello szükséges megfelelő SCM rendszer ismerete.
* A folyamatos üzembe helyezés kulcsrakész megoldás. 

### <a name="vsts"></a>Hogyan toodeploy a helyi Git
A hello [Azure Portal](https://portal.azure.com), konfigurálhatja a helyi Git-telepítés.

* [Helyi Git-telepítésének tooAzure App Service](app-service-deploy-local-git.md). 
* [A git/hg tárházból tooWeb alkalmazások közzététele](http://blog.davidebbo.com/2013/04/publishing-to-azure-web-sites-from-any.html).  

## <a name="deploy-using-an-ide"></a>Egy IDE szabályzatsablonokat
Ha már használja a [Visual Studio](https://www.visualstudio.com/products/visual-studio-community-vs.aspx) rendelkező egy [Azure SDK](https://azure.microsoft.com/downloads/), vagy más IDE csomagok, például [Xcode](https://developer.apple.com/xcode/), [Eclipse](https://www.eclipse.org), és [ IntelliJ IDEA](https://www.jetbrains.com/idea/), tooAzure közvetlenül a központilag telepíthető a IDE belül. Ez a beállítás akkor hasznos, ha az egyéni fejlesztői.

Visual Studio támogatja minden három központi telepítési folyamat (FTP, Git, és a Web Deploy), rendelkezésére, míg más IDEs telepítheti tooApp szolgáltatás, ha az FTP- vagy Git integráció rendelkeznek (lásd: [folyamatok áttekintése](#overview)).

a központi telepítés segítségével egy IDE hello szakemberek a következők:

* Potenciálisan minimalizálása érdekében a végpont alkalmazás-életciklus szükséges hello. Fejlesztése, hibakeresési, nyomon követheti és központi telepítése az alkalmazás tooAzure kívül a IDE áthelyezése nélkül. 

központi telepítés segítségével egy IDE hello hátrányait a következők:

* A hozzáadott összetettségi tooling.
* Továbbra is igényli egy csapatprojekt egy forrás rendszerhez.

<a name="vspros"></a>Központi telepítése a Visual Studio használatával Azure SDK-val további informatikai szakemberek a következők:

* Az Azure SDK első osztályú polgárok teszi Azure-erőforrások a Visual Studióban. Létrehozása, törlése, szerkesztése, indítsa el és alkalmazásokat, hello háttér SQL-adatbázis lekérdezése, élő hibakeresési hello Azure app és még sok más. 
* Élő szerkesztési kód fájlok az Azure-on.
* Az élő Azure hibakeresési alkalmazások.
* Integrált Azure explorer.
* Csak különbözeti telepítése. 

### <a name="vs"></a>Hogyan toodeploy a Visual Studio közvetlenül
* [Ismerkedés az Azure és az ASP.NET](app-service-web-get-started-dotnet.md). Hogyan toocreate és egy egyszerű ASP.NET MVC webes projekt telepítése a Visual Studio és a Web Deploy használatával.
* [Hogyan tooDeploy Visual Studio használatával Azure WebJobs](websites-dotnet-deploy-webjobs.md). Hogyan tooconfigure Konzolalkalmazás projektek, hogy azok telepítése webjobs-feladatok.  
* [Visual Studio használatával ASP.NET Web Deployment](http://www.asp.net/mvc/tutorials/deployment/visual-studio-web-deployment/introduction). 12-rész oktatóanyag sorozata, amely lefedi a feladatok végrehajtását, mint a listában szereplő hello mások teljesebb számos. Néhány az Azure-telepítés szolgáltatásának óta hello oktatóanyag lett írva, de később megjegyzéseket magyarázza el, mi hiányzik.
* [A Visual Studio 2012 a Git-tárház egy ASP.NET-webhely tooAzure közvetlenül telepítése](http://www.dotnetcurry.com/ShowArticle.aspx?ID=881). Ismerteti, hogyan toodeploy ASP.NET webes projekt a Visual Studio használatával hello Git beépülő modul toocommit hello kód tooGit és csatlakozás az Azure toohello Git-tárház. A Visual Studio 2013-től kezdődően Git-támogatást beépített, és nincs szükség a beépülő modul telepítését.

### <a name="aztk"></a>Hogyan toodeploy használatával hello Azure eszközök gazdag eclipse-ben és az IntelliJ IDEA
Microsoft lehetséges toodeploy webalkalmazások tooAzure közvetlenül az eclipse-ben és az IntelliJ hello keresztül teszi [Eclipse Azure eszköztára](../azure-toolkit-for-eclipse.md) és [IntelliJ Azure eszköztára](../azure-toolkit-for-intellij.md). hello alábbi oktatóanyagok bemutatják hello lépéseket, amelyek szerepet játszanak telepítése egyszerű a "Hello" globális webalkalmazás tooAzure használatával, vagy IDE:

* [Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben](app-service-web-eclipse-create-hello-world-web-app.md). Az oktatóanyag bemutatja, hogyan toouse hello Azure eszközkészlet Eclipse toocreate és a webalkalmazás üzembe helyezése Hello World az Azure-bA.
* [Hello World webalkalmazás létrehozása az intellij-t az Azure](app-service-web-intellij-create-hello-world-web-app.md). Az oktatóanyag bemutatja, hogyan toouse hello Azure eszközkészlet IntelliJ toocreate és a webalkalmazás üzembe helyezése Hello World az Azure-bA.

## <a name="automate"></a>Parancssori eszközök segítségével automatizálhatja a központi telepítés
Hello parancssori terminált hello fejlesztési környezet választott által előnyben részesített, ha az App Service alkalmazás parancssori eszközökkel lehet parancsprogramot futtatni a feladatok végrehajtását. 

Informatikai szakemberek a parancssori eszközök segítségével történő telepítéséhez a következők:

* Lehetővé teszi, hogy a központi telepítési forgatókönyvek parancsprogrammal létrehozva.
* Integrálja az Azure-erőforrások és a kód telepítési kiépítése.
* Az Azure-telepítés integrálása meglévő folyamatos integráció parancsfájlok.

Hátrányait parancssori eszközök segítségével történő telepítéséhez a következők:

* Nem a grafikus felhasználói Felülettel előnyben részesítve – fejlesztők számára.

### <a name="automatehow"></a>Hogyan tooautomate parancssori eszközök központi telepítése

Lásd: [automatizálhatja az Azure parancssori eszközök alkalmazás központi telepítési](app-service-deploy-command-line.md) parancssori eszközök és a hivatkozások tootutorials listáját. 

## <a name="nextsteps"></a>Következő lépések
Bizonyos esetekben érdemes lehet toobe képes tooeasily kapcsoló oda-vissza egy átmeneti és üzemi verzióval is rendelkezik az alkalmazás között. További információkért lásd: [előkészített telepítési webalkalmazásokban](web-sites-staged-publishing.md).

A biztonsági mentési és helyreállítási tervek helyen, akkor az összes telepítési munkafolyamat fontos része. Hello App Service biztonsági mentési és visszaállítási szolgáltatással kapcsolatos további információkért lásd: [Web Apps biztonsági mentések](web-sites-backup.md).  

Hogyan érik el toouse Azure szerepköralapú hozzáférés-vezérlés toomanage tooApp szolgáltatástelepítés kapcsolatos információkért lásd: [RBAC és a webes alkalmazás közzétételi](https://azure.microsoft.com/blog/2015/01/05/rbac-and-azure-websites-publishing/).

