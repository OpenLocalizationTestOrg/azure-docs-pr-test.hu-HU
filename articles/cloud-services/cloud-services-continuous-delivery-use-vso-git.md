---
title: "A Git és az Azure-ban a Visual Studio Team Services folyamatos kézbesítési |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja a Visual Studio Team Services csapatprojektek Git automatikusan létrehozásához, és a webes alkalmazás funkció az Azure App Service vagy a felhőbeli szolgáltatások telepítéséhez használandó."
services: cloud-services
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: 4b3297ef-0de6-4d5f-925c-fcdacc3085ac
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/06/2016
ms.author: mlearned
ms.openlocfilehash: f4f5f231536bc381d17898ff2c592be821168a65
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="continuous-delivery-to-azure-using-visual-studio-team-services-and-git"></a>Folyamatos készregyártás az Azure-ban a Visual Studio Team Services-zel és a Gittel
Visual Studio Team Services csapatprojektek segítségével egy Git-tárház gazdagépet a forráskódot, és automatikusan build és Azure web Apps alkalmazások központi telepítése vagy felhőalapú szolgáltatások, amikor egy véglegesítési leküldése a tárházban.

Visual Studio 2013 és az Azure SDK telepítve lesz szüksége. Ha még nem rendelkezik a Visual Studio 2013, töltse le kiválasztásával a **elkezdheti használni az ingyenes** hivatkozás [www.visualstudio.com](http://www.visualstudio.com). Az Azure SDK telepítése [Itt](http://go.microsoft.com/fwlink/?LinkId=239540).

> [!NOTE]
> Az oktatóanyag elvégzéséhez egy Visual Studio Team Services-fiók szükséges: is [ingyenes Visual Studio Team Services-fiók megnyitása](http://go.microsoft.com/fwlink/p/?LinkId=512979).
> 
> 

Egy felhőalapú szolgáltatás automatikusan építsenek, és telepítse az Azure, a Visual Studio Team Services telepítéséhez kövesse az alábbi lépéseket.

## <a name="1-create-a-git-repository"></a>1: a Git-tárház létrehozása
1. Ha még nem rendelkezik a Visual Studio Team Services-fiók, akkor kaphat egy [Itt](http://go.microsoft.com/fwlink/?LinkId=397665). A csapatprojekt létrehozásakor a forrásrendszerben vezérlő, válassza a Git. Kövesse az utasításokat a csapatprojekt Visual Studio csatlakozni.
2. A **Team Explorer**, válassza ki a **klónozza a tárházat** hivatkozásra.
   
    ![][3]
3. Adja meg a helyi másolat helyét, és válassza a **Klónozás** gombra.

## <a name="2-create-a-project-and-commit-it-to-the-repository"></a>2: projekt létrehozása és véglegesítheti a tárházban
1. A **Team Explorer**, a a **megoldások** területen válassza ki a **új** hozzon létre egy új projektet a helyi tárházban mutató hivatkozást.
   
    ![][4]
2. A bemutatóban szereplő lépéseket követve telepítheti a webes alkalmazás vagy a felhőszolgáltatás (Azure-alkalmazás). Hozzon létre egy új Azure Cloud Service-projektet, vagy egy új ASP.NET MVC projekt. Győződjön meg arról, hogy a projekt célozza a .NET-keretrendszer 4 vagy újabb. Egy felhőszolgáltatás-projekt létrehozásakor, adjon hozzá egy ASP.NET MVC webes és feldolgozói szerepkörök.
   Ha szeretne létrehozni egy webalkalmazást, válassza ki azt a **ASP.NET Web Application** a project, és válassza a **MVC**. Lásd: [egy ASP.NET-webalkalmazás létrehozása az Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md) további információt.
3. Nyissa meg a helyi menüben a megoldás, és válassza a **véglegesítése**.
   
    ![][7]
4. Ha az első alkalommal a Visual Studio Team Services már használta a Git, szüksége arra, hogy néhány adatra, hogy azonosítsa magát a Gitben. Az a **függőben lévő módosítások** területén **Team Explorer**, adja meg a felhasználónevet és e-mail címét. Írjon megjegyzést a véglegesítés, és válassza ki a **véglegesítési** gombra.
   
    ![][8]
5. Vegye figyelembe a beállításokat, vagy adott módosítások kizárja a verziókezelőbe mentésekor. Ha a szükséges módosításokat ki vannak zárva, válassza a **tartalmazza az összes**.
6. Most már véglegesített a módosításokat a tárházba helyi példánya. Ezután szinkronizálhatja a kiszolgáló a módosításokat kiválasztásával a **szinkronizálási** hivatkozásra.

## <a name="3-connect-the-project-to-azure"></a>3: a projekt csatlakozzon az Azure-bA
1. Most, hogy az egyes forráskód azt a Visual Studio Team Services Git-tárház, készen áll a git-tárház csatlakozzon az Azure-bA.  Az a [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), válassza ki a felhőalapú szolgáltatás, vagy a webes alkalmazás, vagy hozzon létre egy újat, válassza ki a + ikonra a lap alján maradt, és válassza **Felhőszolgáltatás** vagy **Web App** és majd **Gyorslétrehozás**.
   
    ![][9]
2. A felhőszolgáltatások, válassza ki a **Visual Studio Team Services való közzététel beállítása** hivatkozásra. A web Apps, válassza ki a **verziókövetésből üzembe helyezés beállítása** hivatkozásra.
   
    ![][10]
3. A varázslóban a szövegmezőben írja be a Visual Studio Team Services-fiók nevét, és válassza ki a **engedélyezik most** hivatkozásra. Jelentkezzen be a kérheti.
   
    ![][11]
4. Az a **kapcsolatkérelem** előugró párbeszédpanelen válasszon **elfogadás** a csapatprojekt konfigurálása a Visual Studio Team Services Azure engedélyezése.
   
    ![][12]
5. Után engedélyezési sikeres, megjelenik egy legördülő listát, amely tartalmazza a Visual Studio Team Services csapatprojektek.  Válassza ki az előző lépésben létrehozott csapatprojekt nevét, és a varázsló a pipa gombra.
   
    ![][13]
   
    A véglegesítési leküldése a tárház legközelebb a Visual Studio Team Services elkészíti és a projekt telepítése az Azure-bA.

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: egy újjáépítését indítja, és telepítse újra a projekthez
1. A Visual Studióban nyissa meg a fájlt, és módosítsa úgy. Módosítsa a fájl például `_Layout.cshtml` a nézetek alatt\\egy MVC webes szerepkör megosztott mappába.
   
    ![][17]
2. Szerkessze az előláb szövegét, a helyhez, és mentse a fájlt.
   
    ![][18]
3. A **Megoldáskezelőben**, nyissa meg a helyi menüben a megoldás csomópont, projektcsomópontra vagy megváltozott, és válassza a fájl **véglegesítése**.
4. Írja be a megjegyzést, és válassza a **véglegesítése**.
   
    ![][20]
5. Válassza ki a **szinkronizálási** hivatkozásra.
   
    ![][38]
6. Válassza ki a **leküldéses** a véglegesítési leküldése a Visual Studio Team Services-tárház mutató hivatkozást. (Is használhatja a **szinkronizálási** gombra kattintva a véglegesítések másolja a tárházba. A különbség az, hogy **szinkronizálási** is kéri le. a legutóbbi változtatásokat a tárházból.)
   
    ![][39]
7. Válassza ki a **otthoni** gombra kattintva visszatérhet a **Team Explorer** kezdőlapján.
   
    ![][21]
8. Válasszon **buildek** megtekintéséhez a buildek folyamatban van.
   
    ![][22]
   
    **Vonja össze az Explorer** jeleníti meg, hogy a bejelentkezés a build lett elindítva.
   
    ![][23]
9. A build előrehaladtával a részletes napló megtekintéséhez kattintson duplán a a build folyamatban van.
10. Bár a összeállítása folyamatban, tekintse meg a build-definíciót, amely jött létre, amikor a varázsló segítségével csatolja az Azure-bA.  Nyissa meg a helyi menüben a build definíciójához, és válassza a **Build definíciójának szerkesztése**.
    
    ![][25]
11. Az a **eseményindító** lapon látni fogja, hogy a build definition létrehozásához, minden egyes bejelentkezéskor értéke alapértelmezés szerint. (Egy felhőalapú szolgáltatás, a Visual Studio Team Services alapszik, és automatikusan telepíti a főágba az átmeneti környezet. Továbbra is fennáll az élő webhelyet szeretne telepíteni egy manuális lépés elvégzéséhez. A webalkalmazás nem található a környezet előkészítési telepíti a főágba közvetlenül az élő webhelyet.
    
    ![][26]
12. Az a **folyamat** lapon megtekintheti a telepítési környezet van beállítva a felhőalapú szolgáltatás, vagy a webes alkalmazás neve.
    
     ![][27]
13. Adja meg a tulajdonságok értékeit, ha azt szeretné, hogy a különböző értékeket, mint az alapértelmezett beállításokat. Az Azure közzétételi tulajdonságok szerepelnek a **telepítési** szakaszt, és előfordulhat, hogy is be kell MSBuild paraméterek. Például a felhő service-projektet, adja meg a "Felhő" eltérő szolgáltatáskonfiguráció állítsa az MSbuild paramétereit `/p:TargetProfile=[YourProfile]` ahol *[YourProfile]* hasonló nevű szolgáltatás konfigurációs fájlok ServiceConfiguration. *YourProfile*.cscfg.
    
     Az alábbi táblázat a rendelkezésre álló tulajdonságok a **telepítési** szakasz:
    
    | Tulajdonság | Alapértelmezett érték |
    | --- | --- |
    | Nem megbízható tanúsítványok engedélyezése |Ha értéke HAMIS, a legfelső szintű hitelesítésszolgáltatóval SSL-tanúsítványokat kell aláírni. |
    | Frissítés engedélyezése |Lehetővé teszi, hogy a központi telepítés helyett újat hoz létre egy meglévő üzemelő példány frissítése. Megőrzi az IP-címet. |
    | Ne törölje |Amennyiben az értéke igaz, ne írja felül a meglévő független telepítés (frissítés engedélyezett). |
    | A telepítési beállítások elérési útja |A fájl elérési útját a .pubxml webes alkalmazások esetén a tárház gyökérkönyvtárában viszonyítva. Cloud services csomag figyelmen kívül hagyja. |
    | SharePoint-környezet |Ugyanaz, mint a szolgáltatás nevét. |
    | Az Azure-telepítés környezet |A webes alkalmazás vagy a felhőbeli szolgáltatás neve. |
14. Időpontig a build sikeresen meg kell adni.
    
     ![][28]
15. Ha duplán kattint a build nevét, a Visual Studio megjeleníti a **Build összegzés**, többek között a vizsgálati eredmények tartozó egység teszt projektek.
    
     ![][29]
16. Az a [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), a tekintheti meg a társított központi telepítéshez a **központi telepítések** az átmeneti kiválasztásakor fülre.
    
     ![][30]
17. Tallózással keresse meg a webhely URL-címe. A webes alkalmazás imént válassza a **Tallózás** a portál gombjára. Egy felhőalapú szolgáltatás, válassza az URL-címet a **gyors áttekintése** szakasza a **irányítópult** oldal, amely az átmeneti környezet látható.
    
    A folyamatos integrációt szolgáltatásokhoz központi telepítések alapértelmezés szerint az átmeneti környezetben kerülnek közzétételre. Ez módosítható úgy, hogy a **másik felhőalapú szolgáltatási környezet** tulajdonságot **éles**. Ez a webhely URL-címe is helyezi a felhőalapú szolgáltatás irányítópult-oldalon.
    
    ![][31]
    
    Hogy láthatóvá váljon a futó helyet egy új böngészőlapon nyílik meg.
    
    ![][32]
18. Ha más módosítja a projekthez, több alkot, eseményindító, és akkor felhalmozódnak több központi telepítését. A legújabb egy aktív van megjelölve.
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: telepítse újra egy korábbi verzióját
Ez a lépés nem kötelező megadni. A klasszikus Azure portálon, válasszon egy korábbi üzemelő, és válassza a **újratelepíteni** való visszatekerés a webhely egy korábbi be. Vegye figyelembe, hogy ezzel a TFS-ben egy új példányt indít, és hozzon létre egy új központi telepítési előzményekben.

![][34]

## <a name="6-change-the-production-deployment"></a>6: módosítsa az éles környezet
Ha készen áll, előléptetheti kiválasztásával az éles környezetbe átmeneti környezet **felcserélése** a klasszikus Azure portálon. Az újonnan telepített átmeneti környezet éles van előléptetve, és az előző éles környezetben, ha van ilyen, lesz egy átmeneti környezet. Az aktív központi telepítéssel üzemi és átmeneti környezetek eltérőek lehetnek, de az üzembe helyezési előzményeket legutóbbi buildek környezet függetlenül ugyanaz.

![][35]

## <a name="7-deploy-from-a-working-branch"></a>7: egy munkaágat a telepítése.
Git használata esetén általában egy munkaágat a módosítások végrehajtása és a főágba integrálása, ha a fejlesztési elér egy állapotban. A fázisban a fejlesztési projekt érdemes létrehozásához és telepítéséhez a munkaágat az Azure-bA.

1. A **Team Explorer**, válassza ki a **Home** gombra, majd válassza ki a **ágak** gombra.
   
    ![][40]
2. Válassza ki a **új fiókirodai** hivatkozásra.
   
    ![][41]
3. Írja be a fiókiroda, például a "folyamatban"állapotra vált, a nevét, és válassza a **ág létrehozása**. Ezzel létrehoz egy új helyi ágat.
   
    ![][42]
4. A fiókirodai közzététele. Válassza ki a fiókiroda nevét a **közzé nem tett ágak**, és válassza a **közzététel**.
   
    ![][44]
5. Alapértelmezés szerint csak a főágba változtatások folyamatos build indítható el. Egy munkaágat folyamatos buildet beállításához válassza ki a **buildek** lap **Team Explorer**, válassza **Build definíciójának szerkesztése**.
6. Nyissa meg a **adatforrás-beállítások** fülre. A **folyamatos integrációt és build ágak figyelt**, válassza a **kattintson ide egy új sort ad hozzá a**.
   
    ![][47]
7. Adja meg a létrehozott, például a refs fájlrendszer/fejek/működő ágat.
   
    ![][48]
8. Módosítja a kódban, nyissa meg a helyi menüben a módosított fájlt, és válassza **véglegesítése**.
   
    ![][43]
9. Válassza ki a **szinkronizálatlan véglegesítések** hivatkozásra, és válassza ki a **szinkronizálási** gombra vagy a **leküldéses** másolja a módosításokat a munkaágat a Visual Studio Team Services példányát mutató hivatkozást.
   
   ![][45]
10. Keresse meg a **buildek** megtekintése és a munkaágat a build kiváltó csak kapott található.

## <a name="next-steps"></a>Következő lépések
A Git használata a Visual Studio Team Services további tippek további tudnivalókért lásd: [fejlesztése és megoszthatja a Visual Studio használatával Git kód](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs-2017) és a Git-tárház, amely nem Visual Studio Team Services közzétételére által kezelt információk Az Azure, lásd: [folyamatos üzembe helyezés az Azure App Service](../app-service-web/app-service-continuous-deployment.md). A Visual Studio Team Services további információkért lásd: [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateTeamProjectInGit.PNG
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png
[3]: ./media/cloud-services-continuous-delivery-use-vso-git/CloneThisRepository.PNG
[4]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateNewSolutionInClonedRepo.PNG
[7]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitMenuItem.PNG
[8]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[9]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateCloudService.PNG
[10]: ./media/cloud-services-continuous-delivery-use-vso-git/SetUpPublishingWithVSO.PNG
[11]: ./media/cloud-services-continuous-delivery-use-vso-git/AuthorizeConnection.PNG
[12]: ./media/cloud-services-continuous-delivery-use-vso-git/ConnectionRequest.PNG
[13]: ./media/cloud-services-continuous-delivery-use-vso-git/ChooseARepo3.PNG
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso-git/MakeACodeChange.PNG
[20]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[21]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerHome.png
[22]: ./media/cloud-services-continuous-delivery-use-vso-git/TeamExplorerBuilds.PNG
[23]: ./media/cloud-services-continuous-delivery-use-vso-git/BuildInQueue.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso-git/ProcessTab.PNG
[28]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso-git/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateANewAccount.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso-git/PushCurrentBranch.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso-git/BranchesInTeamExplorer.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso-git/NewBranch.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso-git/CreateBranch.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso-git/CommitAChange2.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso-git/PublishBranch.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso-git/SyncChanges2.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso-git/SourceSettingsPage.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso-git/IncludeWorkingBranch.PNG
