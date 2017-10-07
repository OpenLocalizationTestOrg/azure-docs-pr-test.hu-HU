---
title: "központi telepítés tooAzure App Service aaaContinuous |} Microsoft Docs"
description: "Megtudhatja, hogyan tooenable folyamatos üzembe helyezés tooAzure App Service."
services: app-service
documentationcenter: 
author: dariagrigoriu
manager: erikre
editor: mollybos
ms.assetid: 6adb5c84-6cf3-424e-a336-c554f23b4000
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: dariagrigoriu
ms.openlocfilehash: 62a22cbda354fd5b0a1b9729c8c375408e75049f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-deployment-tooazure-app-service"></a>Folyamatos telepítés tooAzure App Service
Az oktatóanyag bemutatja, hogyan tooconfigure folyamatos üzembe helyezés munkafolyamat az a [Azure App Service] alkalmazást. App Service integrációja bitbucket szolgáltatásokhoz, a github webhelyen, és [Visual Studio Team Services (VSTS)](https://www.visualstudio.com/team-services/) lehetővé teszi, hogy egy folyamatos telepítési munkafolyamat, ahol a Azure kéri le a legújabb frissítéseket hello a projektből közzétett tooone ezen szolgáltatások. A folyamatos üzembe helyezés jó megoldás lehet olyan projektek esetén, amelyeknél többszöri és gyakori közreműködői változtatást kell integrálni.

toofind ki hogyan tooconfigure manuálisan egy felhőalapú-tárházat nem szerepel a folyamatos üzembe helyezés hello Azure Portal (például [GitLab](https://gitlab.com/)), lásd: [manuális lépéseket követve folyamatos üzembe helyezés beállítása](https://github.com/projectkudu/kudu/wiki/Continuous-deployment#setting-up-continuous-deployment-using-manual-steps).

## <a name="overview"></a>Folyamatos üzembe helyezés engedélyezése
tooenable folyamatos üzembe helyezés

1. Tegye közzé az alkalmazást tartalom toohello tárház, amely jelzi a folyamatos üzembe helyezés.  
    A közzétételi a projekt toothese szolgáltatások további információkért lásd: [hozzon létre egy tárház (GitHub)], [hozzon létre egy tárház (Bitbucketből)], és [Ismerkedés a VSTS].
2. Az alkalmazás menü panelen a hello [Azure-portálon], kattintson a **ALKALMAZÁSTELEPÍTÉS > telepítési lehetőségek**. Kattintson a **forrás választása**, majd jelölje be hello telepítési forrása.  
   
    ![](./media/app-service-continuous-deployment/cd_options.png)
   
   > [!NOTE]
   > a VSTS fiókot, az App Service üzembe helyezéshez tooconfigure lásd a [oktatóanyag](https://github.com/projectkudu/kudu/wiki/Setting-up-a-VSTS-account-so-it-can-deploy-to-a-Web-App).
   > 
   > 
3. Teljes hello engedélyezési munkafolyamat.
4. A hello **központi telepítés forrásának** panelen válassza ki a hello projekt és toodeploy a fiókirodai. Ha végzett, kattintson az **OK** gombra.
   
    ![](./media/app-service-continuous-deployment/github_option.png)
   
   > [!NOTE]
   > Ha engedélyezve van a folyamatos üzembe helyezés a GitHubbal vagy a BitBuckettel, mind a nyilvános, mind a magánjellegű projektek megjelennek.
   > 
   > 
   
    App Service hello kijelölt tárházzal való társítást hoz létre, lekéri a megadott fiókirodai hello hello fájlokat, és a tárház beállítása az App Service alkalmazáshoz készült másolat fenn. VSTS folyamatos üzembe helyezés az Azure-portálon hello konfigurálásakor hello integrációs használ hello App Service [Kudu telepítési motor](https://github.com/projectkudu/kudu/wiki), mely már automatizálja az buildelés és üzembe helyezés feladatok minden `git push`. Nem kell tooseparately VSTS a folyamatos üzembe helyezést beállítani. Ez a folyamat befejezése után hello **központi telepítési beállítások** panelen megjelenik egy aktív központi telepítéssel, amely jelzi a telepítés sikeresen befejeződött.
5. tooverify hello alkalmazás telepítése sikeres volt, kattintson a hello **URL-cím** hello app panel az Azure-portálon hello hello tetején.
6. amely a folyamatos üzembe helyezés folyamatban van az Ön által választott hello tárházból tooverify leküldéses módosítása toohello tára. Az alkalmazás hamarosan hello leküldéses toohello tárház befejeződése után frissítenie kell tooreflect hello módosításokat. Ellenőrizheti, hogy rendelkezik-e lekért hello frissítés a hello **központi telepítési beállítások** az App.

## <a name="VSsolution"></a>Egy Visual Studio-megoldás folyamatos üzembe helyezése
A Visual Studio megoldás tooAzure App Service kérdez le csak egyszerű módon kérdez le egy egyszerű index.html fájl. App Service-telepítési folyamat hello egyszerűsítheti az összes hello adatait, többek között a NuGet-függőségek visszaállítása és felépítése hello bináris alkalmazásfájlokat. Kövesse a hello forrás vezérlő bevált gyakorlatokat kód csak a Git-tárház fenntartásának, és lehetővé teszik az alkalmazás szolgáltatástelepítés hello rest gondoskodunk.

hello lépések a Visual Studio megoldás tooApp szolgáltatás tárházat hello ugyanaz, mint hello [előző szakaszban](#overview), feltéve, hogy konfigurálja a megoldás és a tárház az alábbiak szerint:

* Hello Visual Studio forrás vezérlő beállítás toogenerate használja egy `.gitignore` fájlt például az alábbi képen hello, vagy manuálisan adja hozzá a `.gitignore` fájlt az adattár gyökérkönyvtárában, a tartalom hasonló toothis [.gitignore minta](https://github.com/github/gitignore/blob/master/VisualStudio.gitignore).
  
  ![](./media/app-service-continuous-deployment/VS_source_control.png)
* Vegyen fel hello teljes megoldás directory fa tooyour tárházat, az adattár gyökérkönyvtárában hello hello .sln-fájlt.

Miután leírtak a tárház beállítása, és az alkalmazás konfigurálva az Azure-ban egy hello online Git adattárak folyamatos közzétételt, az ASP.NET-alkalmazás helyileg a Visual Studio fejlesztése, és folyamatosan egyszerűen, az a kód telepítésére a módosítások tooyour online Git-tárház kérdez le.

## <a name="disableCD"></a>Folyamatos üzembe helyezés letiltása
toodisable folyamatos üzembe helyezés

1. Az alkalmazás menü panelen a hello [Azure-portálon], kattintson a **ALKALMAZÁSTELEPÍTÉS > telepítési lehetőségek**. Kattintson a **Disconnect** a hello **központi telepítési beállítások** panelen.
   
    ![](./media/app-service-continuous-deployment/cd_disconnect.png)
2. Fogadás után **Igen** toohello megerősítő üzenetet, térjen vissza a tooyour alkalmazás paneljének szabadon kattintson **ALKALMAZÁSTELEPÍTÉS > telepítési lehetőségek** Ha azt szeretné, hogy fel más forrásból származó közzététel tooset.

## <a name="additional-resources"></a>További források
* [Hogyan tooinvestigate közös érintő problémákat folyamatos üzembe helyezés](https://github.com/projectkudu/kudu/wiki/Investigating-continuous-deployment)
* [Hogyan toouse az Azure PowerShell]
* [Hogyan toouse hello for Mac és Linux Azure parancssori eszközei]
* [A Git dokumentációja]
* [A Kudu projekt](https://github.com/projectkudu/kudu/wiki)
* [Használja az Azure tooautomatically egy CI/CD adatcsatorna toodeploy az ASP.NET 4 alkalmazás készítése](https://www.visualstudio.com/docs/build/get-started/aspnet-4-ci-cd-azure-automatic)

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

[Azure App Service]: https://azure.microsoft.com/en-us/documentation/articles/app-service-changes-existing-services/
[Azure-portálon]: https://portal.azure.com
[VSTS Portal]: https://www.visualstudio.com/en-us/products/visual-studio-team-services-vs.aspx
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[Hogyan toouse az Azure PowerShell]: /powershell/azureps-cmdlets-docs
[Hogyan toouse hello for Mac és Linux Azure parancssori eszközei]:../cli-install-nodejs.md
[A Git dokumentációja]: http://git-scm.com/documentation

[hozzon létre egy tárház (GitHub)]: https://help.github.com/articles/create-a-repo
[hozzon létre egy tárház (Bitbucketből)]: https://confluence.atlassian.com/display/BITBUCKET/Create+an+Account+and+a+Git+Repo
[Ismerkedés a VSTS]: https://www.visualstudio.com/docs/vsts-tfs-overview
