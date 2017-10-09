---
title: "a Git és az Azure-ban a Visual Studio Team Services aaaContinuous kézbesítési |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure a Visual Studio Team Services team projects toouse Git tooautomatically és toohello webalkalmazás funkció az Azure App Service vagy a felhőbeli szolgáltatások üzembe."
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
ms.openlocfilehash: 936c42194f45be55597a77f9a3a6deb4480ed94b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-delivery-tooazure-using-visual-studio-team-services-and-git"></a>Folyamatos kézbesítési tooAzure Visual Studio Team Services és a Git használatával
Használja a Visual Studio Team Services team projects toohost Git-tárház a forráskódot, és automatikusan build és tooAzure webes alkalmazások telepítését vagy felhőalapú szolgáltatások, amikor egy véglegesítési toohello tárház leküldéses.

Visual Studio 2013 és Azure SDK telepítése hello lesz szüksége. Ha még nem rendelkezik a Visual Studio 2013, töltse le hello kiválasztásával **elkezdheti használni az ingyenes** hivatkozás [www.visualstudio.com](http://www.visualstudio.com). Telepítse az Azure SDK hello [Itt](http://go.microsoft.com/fwlink/?LinkId=239540).

> [!NOTE]
> Ez az oktatóanyag kell egy Visual Studio Team Services-fiók toocomplete: is [ingyenes Visual Studio Team Services-fiók megnyitása](http://go.microsoft.com/fwlink/p/?LinkId=512979).
> 
> 

egy felhőalapú szolgáltatás tooautomatically mentése tooset build és tooAzure telepíteni a Visual Studio Team Services, kövesse az alábbi lépéseket.

## <a name="1-create-a-git-repository"></a>1: a Git-tárház létrehozása
1. Ha még nem rendelkezik a Visual Studio Team Services-fiók, akkor kaphat egy [Itt](http://go.microsoft.com/fwlink/?LinkId=397665). A csapatprojekt létrehozásakor a forrásrendszerben vezérlő, válassza a Git. Hajtsa végre a hello utasításokat tooconnect Visual Studio tooyour csapatprojektben.
2. A **Team Explorer**, válassza ki a hello **klónozza a tárházat** hivatkozásra.
   
    ![][3]
3. Adja meg a helyi példány hello hello helyét, és válassza a hello **Klónozás** gombra.

## <a name="2-create-a-project-and-commit-it-toohello-repository"></a>2: projekt létrehozása és véglegesítheti toohello tárház
1. A **Team Explorer**, a hello **megoldások** területen válassza ki a hello **új** toocreate egy új projektet hello helyi tárházban található hivatkozásra.
   
    ![][4]
2. Webes alkalmazást telepít, vagy a bemutatóban szereplő lépések egy felhőalapú szolgáltatás (Azure-alkalmazás) a következő hello által. Hozzon létre egy új Azure Cloud Service-projektet, vagy egy új ASP.NET MVC projekt. Győződjön meg arról, hogy hello projekt célok hello .NET-keretrendszer 4 vagy újabb. Egy felhőszolgáltatás-projekt létrehozásakor, adjon hozzá egy ASP.NET MVC webes és feldolgozói szerepkörök.
   Ha azt szeretné, hogy a webes alkalmazás toocreate, válassza ki azt a hello **ASP.NET Web Application** a project, és válassza a **MVC**. Lásd: [egy ASP.NET-webalkalmazás létrehozása az Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md) további információt.
3. Nyissa meg a helyi menü hello hello megoldás, és válassza a **véglegesítése**.
   
    ![][7]
4. Ha hello első alkalommal a Visual Studio Team Services már használta a Git, szüksége lesz tooprovide néhány információt tooidentify saját kezűleg a Gitben. A hello **függőben lévő módosítások** területén **Team Explorer**, adja meg a felhasználónevet és e-mail címét. Írjon megjegyzést hello véglegesítési, és válassza a hello **véglegesítési** gombra.
   
    ![][8]
5. Megjegyzés: hello beállítások tooinclude, vagy adott módosítások kizárása a verziókezelőbe mentésekor. Ha hello változik meg akarja ki vannak zárva, válassza a **tartalmazza az összes**.
6. Megismerte a most már véglegesített hello hello tárház helyi példányának módosításait. Ezután szinkronizálhatja a módosításokat, hello kiszolgálóval hello kiválasztásával **szinkronizálási** hivatkozásra.

## <a name="3-connect-hello-project-tooazure"></a>3: hello projekt tooAzure csatlakozás
1. Most, hogy az egyes forráskód azt a Visual Studio Team Services Git-tárház, tooconnect készen áll a git-tárház tooAzure.  A hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), válassza ki a felhőalapú szolgáltatás, vagy a webes alkalmazás, vagy hozzon létre egy újat hello + balra hello le, majd válassza a ikonra kiválasztásával **Felhőszolgáltatás** vagy **webalkalmazás**, majd **Gyorslétrehozás**.
   
    ![][9]
2. A felhőszolgáltatások, válassza ki a hello **Visual Studio Team Services való közzététel beállítása** hivatkozásra. A web Apps, válassza ki a hello **verziókövetésből üzembe helyezés beállítása** hivatkozásra.
   
    ![][10]
3. Hello varázslóban, írja be a Visual Studio Team Services-fiók nevét hello hello szövegmezőben, és válassza a hello **engedélyezik most** hivatkozásra. Előfordulhat, hogy a toosign kéri.
   
    ![][11]
4. A hello **kapcsolatkérelem** előugró párbeszédpanelen válasszon **elfogadás** tooauthorize a csapat projektre a Visual Studio Team Services, Azure tooconfigure.
   
    ![][12]
5. Után engedélyezési sikeres, megjelenik egy legördülő listát, amely tartalmazza a Visual Studio Team Services csapatprojektek.  Válassza ki a csapatprojekt hello előző lépésekben létrehozott hello nevét, és válassza a hello varázsló pipa gombra.
   
    ![][13]
   
    hello legközelebb leküldéses egy véglegesítési tooyour tárházat, a Visual Studio Team Services elkészíti és központi telepítése a projekt tooAzure.

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: egy újjáépítését indítja, és telepítse újra a projekthez
1. A Visual Studióban nyissa meg a fájlt, és módosítsa úgy. Például a hello fájl módosítása `_Layout.cshtml` hello nézetek alapján\\egy MVC webes szerepkör megosztott mappába.
   
    ![][17]
2. Hello előláb szövegét hello hely szerkesztése, és mentse hello fájlt.
   
    ![][18]
3. A **Megoldáskezelőben**, hello helyi menü megnyitásához, az hello megoldás csomópont, projektcsomópontra vagy hello fájl módosult, és válassza a **véglegesítése**.
4. Írja be a megjegyzést, és válassza a **véglegesítése**.
   
    ![][20]
5. Válassza ki a hello **szinkronizálási** hivatkozásra.
   
    ![][38]
6. Válassza ki a hello **leküldéses** toopush hivatkozásra a Visual Studio Team Services véglegesítési toohello tárházba. (Használhatja hello **szinkronizálási** toocopy a véglegesítések toohello tárház gombra. hello különbség az, hogy **szinkronizálási** is ponttá hello legutóbbi módosítások adattárból hello.)
   
    ![][39]
7. Válassza ki a hello **otthoni** gomb tooreturn toohello **Team Explorer** kezdőlapján.
   
    ![][21]
8. Válasszon **buildek** tooview hello buildek folyamatban van.
   
    ![][22]
   
    **Vonja össze az Explorer** jeleníti meg, hogy a bejelentkezés a build lett elindítva.
   
    ![][23]
9. tooview részletes napló szerint hello létrehozása történik, kattintson duplán a hello összeállítása folyamatban hello nevére.
10. Amíg hello build folyamatban, tekintse meg hello build-definíciót, amely során használt hello varázsló toolink tooAzure jött létre.  Hello build definíciójának hello helyi menü megnyitásához, és válassza a **Build definíciójának szerkesztése**.
    
    ![][25]
11. A hello **eseményindító** lapon látni fogja, hogy hello build definíciója van megadva toobuild a minden bejelentkezés alapértelmezés szerint. (Egy felhőalapú szolgáltatás, a Visual Studio Team Services alapszik, és automatikusan átmeneti környezet hello főághoz toohello telepíti. Továbbra is meg kell toodo egy manuális lépés toodeploy toohello élő webhelyet. A webalkalmazás nem található a környezet előkészítési telepíti hello főághoz közvetlen toohello élő hely.
    
    ![][26]
12. A hello **folyamat** lapon látható hello környezet van beállítva a felhőalapú szolgáltatás, vagy a webes alkalmazás toohello nevét.
    
     ![][27]
13. Hello tulajdonságok értékeket megadni, ha azt szeretné, hogy a különböző hello alapértelmezett értékekkel. hello Azure közzétételi tulajdonságainak vannak hello **telepítési** szakaszt, és előfordulhat, hogy is kell tooset MSBuild paraméterek. Például egy felhőszolgáltatás-projekt toospecify eltérő "Felhő" szolgáltatáskonfiguráció található hello MSbuild paraméterek beállítása túl`/p:TargetProfile=[YourProfile]` ahol *[YourProfile]* hasonló nevű szolgáltatás konfigurációs fájlok ServiceConfiguration. *YourProfile*.cscfg.
    
     hello alábbi táblázat tartalmazza hello rendelkezésre álló tulajdonságok hello **telepítési** szakasz:
    
    | Tulajdonság | Alapértelmezett érték |
    | --- | --- |
    | Nem megbízható tanúsítványok engedélyezése |Ha értéke HAMIS, a legfelső szintű hitelesítésszolgáltatóval SSL-tanúsítványokat kell aláírni. |
    | Frissítés engedélyezése |Lehetővé teszi, hogy hello telepítési tooupdate létre egy új, hanem egy meglévő telepítését. Megőrzi a hello IP-címet. |
    | Ne törölje |Amennyiben az értéke igaz, ne írja felül a meglévő független telepítés (frissítés engedélyezett). |
    | Elérési út tooDeployment beállítások |hello elérési tooyour .pubxml fájl egy webalkalmazás, hello tárház relatív toohello gyökérmappájában. Cloud services csomag figyelmen kívül hagyja. |
    | SharePoint-környezet |hello hello szolgáltatás neve azonos. |
    | Az Azure-telepítés környezet |hello webszolgáltatás alkalmazás vagy felhő neve. |
14. Időpontig a build sikeresen meg kell adni.
    
     ![][28]
15. Ha duplán kattint a hello build nevét, a Visual Studio megjeleníti egy **Build összegzés**, többek között a vizsgálati eredmények tartozó egység teszt projektek.
    
     ![][29]
16. A hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), hello tartozó központi telepítést megtekintheti a hello **központi telepítések** átmeneti környezet hello kiválasztásakor fülre.
    
     ![][30]
17. Keresse meg a tooyour webhelyének URL-címe. A webes alkalmazás imént válassza hello **Tallózás** hello portál gombjára. Egy felhőalapú szolgáltatás, válassza ki az hello URL-címet a hello **gyors áttekintése** hello szakasza **irányítópult** oldal, amely hello átmeneti környezet látható.
    
    A folyamatos integrációt szolgáltatásokhoz központi telepítések közzétett toohello átmeneti környezet alapértelmezés szerint. Módosíthatja a hello beállítása **másik felhőalapú szolgáltatási környezet** tulajdonság túl**éles**. Itt található, ahol hello webhely URL-címe megtalálható-e hello felhőalapú szolgáltatás irányítópult-oldalon.
    
    ![][31]
    
    Egy új böngészőlapon nyílik tooreveal futó webhelyét.
    
    ![][32]
18. Ha egyéb módosítások tooyour projekt, több alkot, eseményindító, és Ön felhalmozódnak több központi telepítését. hello legújabb egy aktív jelölésű.
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: telepítse újra egy korábbi verzióját
Ez a lépés nem kötelező megadni. A klasszikus Azure portálon hello, egy korábbi üzemelő és kiválasztható **újratelepíteni** toorewind a hely tooan korábban be. Vegye figyelembe, hogy ezzel a TFS-ben egy új példányt indít, és hozzon létre egy új központi telepítési előzményekben.

![][34]

## <a name="6-change-hello-production-deployment"></a>6: hello éles környezet módosítása
Ha készen áll, előléptetheti kiválasztásával hello átmeneti toohello üzemi környezet **felcserélése** a hello a klasszikus Azure portálon. újonnan telepített hello átmeneti környezet előléptetett tooProduction, és hello előző éles környezetben, ha van ilyen, lesz egy átmeneti környezet. hello aktív központi telepítéssel hello üzemi és átmeneti környezetek eltérőek lehetnek, de hello üzembe helyezési előzményeket legutóbbi buildek van hello azonos környezetben függetlenül.

![][35]

## <a name="7-deploy-from-a-working-branch"></a>7: egy munkaágat a telepítése.
Git használata esetén, általában egy munkaágat módosítsa, majd hello főághoz integrálása, amikor a fejlesztési eléri a állapotban. Fázisban hello fejlesztési projekt fogja toobuild szeretné és hello működő fiókirodai tooAzure telepítése.

1. A **Team Explorer**, válassza ki a hello **Home** gombra, majd kattintson a hello **ágak** gombra.
   
    ![][40]
2. Válassza ki a hello **új fiókirodai** hivatkozásra.
   
    ![][41]
3. Hello ág, például a "folyamatban"állapotra vált, hello nevét adja meg, és válassza a **ág létrehozása**. Ezzel létrehoz egy új helyi ágat.
   
    ![][42]
4. Hello fiókirodai közzététele. Válasszon hello fiókirodai nevet a **közzé nem tett ágak**, és válassza a **közzététel**.
   
    ![][44]
5. Alapértelmezés szerint csak akkor változik meg toohello főághoz eseményindító folyamatos build. fel egy munkaágat folyamatos buildet tooset válasszon hello **buildek** lap **Team Explorer**, és válassza **Build definíciójának szerkesztése**.
6. Nyissa meg hello **adatforrás-beállítások** fülre. A **folyamatos integrációt és build ágak figyelt**, válassza a **kattintson ide egy új sor tooadd**.
   
    ![][47]
7. Adja meg a létrehozott, például a refs fájlrendszer/fejek/működő hello ág.
   
    ![][48]
8. Olyan módosítást hello kódban, nyissa meg hello helyi menüje hello módosított fájlt, és válassza a **véglegesítése**.
   
    ![][43]
9. Válassza ki a hello **szinkronizálatlan véglegesítések** hivatkozásra, és válassza ki a hello **szinkronizálási** gombra való kattintást vagy hello **leküldéses** hivatkozás toocopy hello módosítja a Visual Studio hello munkaágat toohello példányát Team Services.
   
   ![][45]
10. Keresse meg a toohello **buildek** megtekintése és hello munkaágat hello build kiváltó csak kapott található.

## <a name="next-steps"></a>Következő lépések
toolearn további tippek a Visual Studio Team Services, a Git használatával, lásd: [fejlesztése és megoszthatja a Visual Studio használatával Git kód](https://www.visualstudio.com/en-us/docs/git/share-your-code-in-git-vs-2017) és a Git-tárház, amely nem Visual Studio Team Services toopublish által kezelt információk tooAzure, lásd: [folyamatos üzembe helyezés tooAzure App Service](../app-service-web/app-service-continuous-deployment.md). A Visual Studio Team Services további információkért lásd: [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

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
