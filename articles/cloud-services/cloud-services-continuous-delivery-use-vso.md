---
title: "a Visual Studio Team Services, Azure-ban aaaContinuous kézbesítési |} Microsoft Docs"
description: "Ismerje meg, hogyan tooconfigure a Visual Studio Team Services team projects tooautomatically és toohello webalkalmazás funkció az Azure App Service vagy a felhőbeli szolgáltatások üzembe."
services: cloud-services
documentationcenter: .net
author: mlearned
manager: douge
editor: 
ms.assetid: 797f67ad-e4d4-4063-ae91-41cbdf154191
ms.service: cloud-services
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 07/06/2016
ms.author: mlearned
ms.openlocfilehash: eae75729e1c1a55f9bc3375604a8192f329d0042
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="continuous-delivery-tooazure-using-visual-studio-team-services"></a>Folyamatos kézbesítési tooAzure Visual Studio Team Services használatával
A Visual Studio Team Services team projects tooautomatically build konfigurálása és tooAzure webes alkalmazásokat, vagy a felhőalapú szolgáltatások.  (Tooset akár folyamatos build és a rendszer szabályzatsablonokat információt egy *helyszíni* a Team Foundation Server, lásd: [Azure felhőszolgáltatások folyamatos kézbesítési](cloud-services-dotnet-continuous-delivery.md).)

Ez az oktatóanyag feltételezi, hogy rendelkezik a Visual Studio 2013 és Azure SDK telepítése hello. Ha még nem rendelkezik a Visual Studio 2013, töltse le hello kiválasztásával **elkezdheti használni az ingyenes** hivatkozás [www.visualstudio.com](http://www.visualstudio.com). Telepítse az Azure SDK hello [Itt](http://go.microsoft.com/fwlink/?LinkId=239540).

> [!NOTE]
> Ez az oktatóanyag kell egy Visual Studio Team Services-fiók toocomplete: is [ingyenes Visual Studio Team Services-fiók megnyitása](http://go.microsoft.com/fwlink/p/?LinkId=512979).
> 
> 

egy felhőalapú szolgáltatás tooautomatically mentése tooset build és tooAzure telepíteni a Visual Studio Team Services, kövesse az alábbi lépéseket.

## <a name="1-create-a-team-project"></a>1: team-projekt létrehozása
Útmutatás alapján hello [Itt](http://go.microsoft.com/fwlink/?LinkId=512980) toocreate a csapat projektre, és hivatkozás tooVisual Studio. Ez a forgatókönyv azt feltételezi, hogy a forrás vezérlő megoldásként használja a Team Foundation verzió vezérlő (TFVC). Ha szeretne toouse Git verziókezelést, lásd: [hello Git verziója, ez a forgatókönyv](http://go.microsoft.com/fwlink/p/?LinkId=397358).

## <a name="2-check-in-a-project-toosource-control"></a>2: Ellenőrizze, hogy a projekt toosource vezérlő szerepel
1. A Visual Studióban nyissa meg a hello megoldást szeretné, hogy toodeploy, vagy hozzon létre egy újat.
   Webes alkalmazást telepít, vagy a bemutatóban szereplő lépések egy felhőalapú szolgáltatás (Azure-alkalmazás) a következő hello által.
   Ha azt szeretné, hogy egy új megoldás toocreate, hozzon létre egy új Azure Cloud Service-projektet, vagy egy új ASP.NET MVC projekt. Győződjön meg arról, hogy hello projekt célok .NET-keretrendszer 4 vagy 4.5, és egy felhőszolgáltatás-projekt létrehozásakor egy ASP.NET MVC webes és feldolgozói szerepkörök hozzáadása, és válassza ki az internetes alkalmazás hello webes szerepkör. Amikor a rendszer kéri, válassza ki a **az internetes alkalmazás**.
   Ha azt szeretné, hogy a webes alkalmazás toocreate, válassza ki a hello ASP.NET Web Application project sablont, és válassza a MVC. Lásd: [egy ASP.NET-webalkalmazás létrehozása az Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md).
   
   > [!NOTE]
   > Visual Studio Team Services csak a jelenleg támogatott CI központi telepítések, a Visual Studio-webalkalmazások. Webhely-projektek kívül esnek a hatókörön.
   > 
   > 
2. Nyissa meg a helyi menü hello hello megoldás, és válassza a **megoldás hozzáadása tooSource vezérlő**.
   
    ![][5]
3. Fogadja el, vagy módosítsa a hello alapértelmezett beállításokat, és válassza ki a hello **OK** gombra. Miután hello folyamat befejeződött, forrás vezérlő ikonok jelennek meg **Megoldáskezelőben**.
   
    ![][6]
4. Nyissa meg a helyi menü hello hello megoldás, és válassza a **felvétel**.
   
    ![][7]
5. A hello **függőben lévő módosítások** területén **Team Explorer**, írja be a hello be megjegyzéseit, és válassza ki a hello **felvétel** gombra.
   
    ![][8]
   
    Megjegyzés: hello beállítások tooinclude, vagy adott módosítások kizárása a verziókezelőbe mentésekor. Ha szükséges, nem tartoznak a módosítások, válassza ki azt a hello **tartalmazza az összes** hivatkozásra.
   
    ![][9]

## <a name="3-connect-hello-project-tooazure"></a>3: hello projekt tooAzure csatlakozás
1. Most, hogy a Visual STUDIO Team Services csapatprojekt néhány adatforrás kóddal azt, tooconnect készen áll a csapat tooAzure projektre.  A hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), válassza ki a felhőalapú szolgáltatás, vagy a webes alkalmazás, vagy hozzon létre egy újat hello kiválasztásával  **+**  hello bal alsó és választja ikonra **Felhőszolgáltatás** vagy **webalkalmazás** , majd **Gyorslétrehozás**. Válassza ki a hello **Visual Studio Team Services való közzététel beállítása** hivatkozásra.
   
    ![][10]
2. Hello varázslóban, írja be a Visual Studio Team Services-fiók nevét hello hello szövegmezőben, majd kattintson a hello **engedélyezik most** hivatkozásra. Előfordulhat, hogy a toosign kéri.
   
    ![][11]
3. A hello **kapcsolatkérelem** előugró párbeszédpanelen válassza ki a hello **elfogadás** gomb tooauthorize a csapat projektre a Visual STUDIO Team Services, Azure tooconfigure.
   
    ![][12]
4. Ha engedélyezési sikeres, megjelenik egy legördülő lista a Visual Studio Team Services csapatprojektek listáját tartalmazó. Válassza ki a csapatprojekt hello előző lépésekben létrehozott hello nevét, és kattintson a pipa gombra hello varázsló.
   
    ![][13]
5. Után a projekt ki van kapcsolva, látni fogja, hogy egyes módosítások tooyour Visual Studio Team Services csapatprojektben ellenőrzése a következő útmutatást ki.  A következő bejelentkezéskor, a Visual Studio Team Services elkészíti és központi telepítése a projekt tooAzure.  Ez most hello kattintva próbálkozzon **ellenőrizze a Visual Studio** hivatkozásra, és ezután hello **indítsa el a Visual Studio** hivatkozásra (vagy ezzel egyenértékű hello **Visual Studio** hello aljához gomb hello Letöltés gombra).
   
    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: egy újjáépítését indítja, és telepítse újra a projekthez
1. A Visual Studio **Team Explorer**, válassza ki a hello **forrás vezérlő Explorer** hivatkozásra.
   
    ![][15]
2. Keresse meg a megoldásfájlt tooyour, majd nyissa meg.
   
    ![][16]
3. A **Megoldáskezelőben**, nyissa meg a fájlt, és módosítsa azt. Például a hello fájl módosítása `_Layout.cshtml` hello nézetek alapján\\egy MVC webes szerepkör megosztott mappába.
   
    ![][17]
4. Hello embléma hello hely szerkesztése, és válassza a **Ctrl + S** toosave hello fájlt.
   
    ![][18]
5. A **Team Explorer**, válassza ki a hello **függőben lévő módosítások** hivatkozásra.
   
    ![][19]
6. Írjon egy megjegyzést, és válassza a hello **felvétel** gombra.
   
    ![][20]
7. Válassza ki a hello **otthoni** gomb tooreturn toohello **Team Explorer** kezdőlapján.
   
    ![][21]
8. Válassza ki a hello **buildek** hivatkozás tooview hello buildek folyamatban van.
   
    ![][22]
   
    **Vonja össze az Explorer** jeleníti meg, hogy a bejelentkezés a build lett elindítva.
   
    ![][23]
9. Kattintson duplán a folyamatban lévő tooview részletes napló hello build hello nevére hello build előrehaladtával.
   
    ![][24]
10. Amíg hello build folyamatban, tekintse meg hello build-definíciót, amely jött létre, amikor a TFS tooAzure csatolta hello varázsló segítségével.  Hello build definíciójának hello helyi menü megnyitásához, és válassza a **Build definíciójának szerkesztése**.
    
     ![][25]
    
     A hello **eseményindító** lapon látni fogja, hogy hello build definíciója van megadva toobuild a minden bejelentkezés alapértelmezés szerint.
    
     ![][26]
    
     A hello **folyamat** lapon látható hello környezet van beállítva a felhőalapú szolgáltatás, vagy a webes alkalmazás toohello nevét. Ha a webalkalmazásokkal dolgozik, hello tulajdonságok látja eltérő itt látható lesz.
    
     ![][27]
11. Hello tulajdonságok értékeket megadni, ha azt szeretné, hogy a különböző hello alapértelmezett értékekkel. hello Azure közzétételi tulajdonságainak vannak hello **telepítési** szakasz.
    
     hello alábbi táblázat tartalmazza hello rendelkezésre álló tulajdonságok hello **telepítési** szakasz:
    
    | Tulajdonság | Alapértelmezett érték |
    | --- | --- |
    | Nem megbízható tanúsítványok engedélyezése |Ha értéke HAMIS, a legfelső szintű hitelesítésszolgáltatóval SSL-tanúsítványokat kell aláírni. |
    | Frissítés engedélyezése |Lehetővé teszi, hogy hello telepítési tooupdate létre egy új, hanem egy meglévő telepítését. Megőrzi a hello IP-címet. |
    | Ne törölje |Amennyiben az értéke igaz, ne írja felül a meglévő független telepítés (frissítés engedélyezett). |
    | Elérési út tooDeployment beállítások |hello elérési tooyour .pubxml fájl egy webalkalmazás, hello tárház relatív toohello gyökérmappájában. Cloud services csomag figyelmen kívül hagyja. |
    | SharePoint-környezet |hello hello szolgáltatás neve azonos. |
    | Az Azure-telepítés környezet |hello webszolgáltatás alkalmazás vagy felhő neve. |
12. Ha több szolgáltatáskonfiguráció (.cscfg-fájlok) használ, megadhat hello kívánt szolgáltatáskonfiguráció hello **felépítéséhez, speciális, MSBuild-argumentumok** beállítást. Például toouse ServiceConfiguration.Test.cscfg, beállításhoz MSBuild-argumentumok sor `/p:TargetProfile=Test`.
    
     ![][38]
    
     Időpontig a build sikeresen meg kell adni.
    
     ![][28]
13. Ha duplán kattint a hello build nevét, a Visual Studio megjeleníti egy **Build összegzés**, többek között a vizsgálati eredmények tartozó egység teszt projektek.
    
     ![][29]
14. A hello [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), hello tartozó központi telepítést megtekintheti a hello **központi telepítések** átmeneti környezet hello kiválasztásakor fülre.
    
     ![][30]
15. Keresse meg a tooyour webhelyének URL-címe. A webalkalmazás kattintson hello **Tallózás** hello parancssáv gombjára. Egy felhőalapú szolgáltatás, válassza ki az hello URL-címet a hello **gyors áttekintése** hello szakasza **irányítópult** oldal, amely hello átmeneti környezet egy felhőalapú szolgáltatás számára látható. A folyamatos integrációt szolgáltatásokhoz központi telepítések közzétett toohello átmeneti környezet alapértelmezés szerint. Módosíthatja a hello beállítása **másik felhőalapú szolgáltatási környezet** tulajdonság túl**éles**. Ezen a képernyőfelvételen látható látható, ahol hello webhely URL-címe hello felhőalapú szolgáltatás irányítópult-oldalon.
    
    ![][31]
    
    Egy új böngészőlapon nyílik tooreveal futó webhelyét.
    
    ![][32]
    
    A felhőszolgáltatások Ha egyéb módosítások tooyour projekt, több alkot, eseményindító, és Ön felhalmozódnak több központi telepítését. hello legújabb egy aktív jelölésű.
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: telepítse újra egy korábbi verzióját
Ez a lépés toocloud szolgáltatások vonatkozik, és nem kötelező megadni. A klasszikus Azure portálon hello, válasszon egy korábbi üzemelő, és válassza a hello **újratelepíteni** toorewind gombra a hely tooan korábban be.  Vegye figyelembe, hogy ezzel a TFS-ben egy új példányt indít, és hozzon létre egy új központi telepítési előzményekben.

![][34]

## <a name="6-change-hello-production-deployment"></a>6: hello éles környezet módosítása
Ez a lépés csak toocloud szolgáltatásokat, nem a webalkalmazások vonatkozik. Ha készen áll, előléptetheti hello kiválasztásával hello átmeneti toohello üzemi környezet **felcserélése** hello a klasszikus Azure portálon gombjára. újonnan telepített hello átmeneti környezet előléptetett tooProduction, és hello előző éles környezetben, ha van ilyen, lesz egy átmeneti környezet. hello aktív központi telepítéssel hello üzemi és átmeneti környezetek eltérőek lehetnek, de hello üzembe helyezési előzményeket legutóbbi buildek van hello azonos környezetben függetlenül.

![][35]

## <a name="7-run-unit-tests"></a>7: egység tesztek futtatása
Ez a lépés csak tooweb alkalmazásokat, nem a felhőszolgáltatások vonatkozik. Ha azt elmulasztják, leállíthatja, hello telepítési és tooput a telepítés minőségi kaput, is futtathatók egység tesztek.

1. A Visual Studio egy egység teszt projekt hozzáadása.
   
   ![][39]
2. Adja hozzá a projekt hivatkozásainak toohello projekt tootest szeretné.
   
   ![][40]
3. Adja hozzá az egyes egység tesztek. tooget elindult, próbálkozzon egy előzetes vizsgálat mindig adja meg, hogy.
   
       ```
       using System;
       using Microsoft.VisualStudio.TestTools.UnitTesting;
   
       namespace UnitTestProject1
       {
           [TestClass]
           public class UnitTest1
           {
               [TestMethod]
               [ExpectedException(typeof(NotImplementedException))]
               public void TestMethod1()
               {
                   throw new NotImplementedException();
               }
           }
       }
       ```
4. Hello build definíciójának szerkesztése, válassza a hello **folyamat** lapot, és bontsa ki a hello **teszt** csomópont.
5. Set hello **tesztje sikertelen sikertelen épülő** tooTrue. Ez azt jelenti, hogy hello telepítési nem fordulhat elő, ha a vizsgálatok sikeresek hello.
   
   ![][41]
6. Várólista új buildverziót.
   
   ![][42]
   
   ![][43]
7. Hello build folyik, amíg a folyamat ellenőrzéséhez.
   
    ![][44]
   
    ![][45]
8. Hello build történik, hogy hello teszteredmények.
   
    ![][46]
   
    ![][47]
9. Próbálja meg létrehozni egy tesztet, amely sikertelen lesz. Első hozzáadása egy új teszt hello másolásával, nevezze át és megjegyzéssé hello kódsort arról, hogy NotImplementedException várt kivétel.
   
       ```
       [TestMethod]
       //[ExpectedException(typeof(NotImplementedException))]
       public void TestMethod2()
       {
           throw new NotImplementedException();
       }
       ```
10. Az ellenőrzéshez hello tooqueue új buildverziót módosítása.
    
     ![][48]
11. Hello vizsgálati eredmények toosee részleteinek megtekintése hello hiba.
    
     ![][49]
    
     ![][50]

## <a name="next-steps"></a>Következő lépések
A Visual Studio Team Services tesztelés egység kapcsolatban bővebben lásd: [egység tesztek futtatása a építés](http://go.microsoft.com/fwlink/p/?LinkId=510474). Ha a Git használata esetén lásd: [megosztani a Git kód](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) és [folyamatos üzembe helyezés tooAzure App Service](../app-service-web/app-service-continuous-deployment.md).  További információ a Visual Studio Team Services: [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

[0]: ./media/cloud-services-continuous-delivery-use-vso/tfs0.PNG
[1]: ./media/cloud-services-continuous-delivery-use-vso/tfs1.png
[2]: ./media/cloud-services-continuous-delivery-use-vso/tfs2.png

[5]: ./media/cloud-services-continuous-delivery-use-vso/tfs5.png
[6]: ./media/cloud-services-continuous-delivery-use-vso/tfs6.png
[7]: ./media/cloud-services-continuous-delivery-use-vso/tfs7.png
[8]: ./media/cloud-services-continuous-delivery-use-vso/tfs8.png
[9]: ./media/cloud-services-continuous-delivery-use-vso/tfs9.png
[10]: ./media/cloud-services-continuous-delivery-use-vso/tfs10.png
[11]: ./media/cloud-services-continuous-delivery-use-vso/tfs11.png
[12]: ./media/cloud-services-continuous-delivery-use-vso/tfs12.png
[13]: ./media/cloud-services-continuous-delivery-use-vso/tfs13.png
[14]: ./media/cloud-services-continuous-delivery-use-vso/tfs14.png
[15]: ./media/cloud-services-continuous-delivery-use-vso/tfs15.png
[16]: ./media/cloud-services-continuous-delivery-use-vso/tfs16.png
[17]: ./media/cloud-services-continuous-delivery-use-vso/tfs17.png
[18]: ./media/cloud-services-continuous-delivery-use-vso/tfs18.png
[19]: ./media/cloud-services-continuous-delivery-use-vso/tfs19.png
[20]: ./media/cloud-services-continuous-delivery-use-vso/tfs20.png
[21]: ./media/cloud-services-continuous-delivery-use-vso/tfs21.png
[22]: ./media/cloud-services-continuous-delivery-use-vso/tfs22.png
[23]: ./media/cloud-services-continuous-delivery-use-vso/tfs23.png
[24]: ./media/cloud-services-continuous-delivery-use-vso/tfs24.png
[25]: ./media/cloud-services-continuous-delivery-use-vso/tfs25.png
[26]: ./media/cloud-services-continuous-delivery-use-vso/tfs26.png
[27]: ./media/cloud-services-continuous-delivery-use-vso/tfs27.png
[28]: ./media/cloud-services-continuous-delivery-use-vso/tfs28.png
[29]: ./media/cloud-services-continuous-delivery-use-vso/tfs29.png
[30]: ./media/cloud-services-continuous-delivery-use-vso/tfs30.png
[31]: ./media/cloud-services-continuous-delivery-use-vso/tfs31.png
[32]: ./media/cloud-services-continuous-delivery-use-vso/tfs32.png
[33]: ./media/cloud-services-continuous-delivery-use-vso/tfs33.png
[34]: ./media/cloud-services-continuous-delivery-use-vso/tfs34.png
[35]: ./media/cloud-services-continuous-delivery-use-vso/tfs35.png
[36]: ./media/cloud-services-continuous-delivery-use-vso/tfs36.PNG
[37]: ./media/cloud-services-continuous-delivery-use-vso/tfs37.PNG
[38]: ./media/cloud-services-continuous-delivery-use-vso/AdvancedMSBuildSettings.PNG
[39]: ./media/cloud-services-continuous-delivery-use-vso/AddUnitTestProject.PNG
[40]: ./media/cloud-services-continuous-delivery-use-vso/AddProjectReferences.PNG
[41]: ./media/cloud-services-continuous-delivery-use-vso/EditBuildDefinitionForTest.PNG
[42]: ./media/cloud-services-continuous-delivery-use-vso/QueueNewBuild.PNG
[43]: ./media/cloud-services-continuous-delivery-use-vso/QueueBuildDialog.PNG
[44]: ./media/cloud-services-continuous-delivery-use-vso/BuildInTeamExplorer.PNG
[45]: ./media/cloud-services-continuous-delivery-use-vso/BuildRequestInfo.PNG
[46]: ./media/cloud-services-continuous-delivery-use-vso/BuildSucceeded.PNG
[47]: ./media/cloud-services-continuous-delivery-use-vso/TestResultsPassed.PNG
[48]: ./media/cloud-services-continuous-delivery-use-vso/CheckInChangeToMakeTestsFail.PNG
[49]: ./media/cloud-services-continuous-delivery-use-vso/TestsFailed.PNG
[50]: ./media/cloud-services-continuous-delivery-use-vso/TestsResultsFailed.PNG
