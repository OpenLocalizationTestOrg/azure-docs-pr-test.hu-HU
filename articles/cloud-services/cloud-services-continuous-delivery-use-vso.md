---
title: "A Visual Studio Team Services, Azure-ban folyamatos kézbesítési |} Microsoft Docs"
description: "Megtudhatja, hogyan konfigurálhatja a Visual Studio Team Services csapatprojektek automatikusan létrehozásához, és a webes alkalmazás funkció az Azure App Service vagy a felhőbeli szolgáltatások telepítéséhez."
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
ms.openlocfilehash: d80ce63eb7ddfd7c45726be887a772f9a7594b28
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="continuous-delivery-to-azure-using-visual-studio-team-services"></a>Folyamatos készregyártás az Azure-ban a Visual Studio Team Services-zel
A Visual Studio Team Services csapatprojektek automatikusan létrehozása és telepítése az Azure web apps, vagy a felhőalapú szolgáltatások konfigurálása  (Folyamatos build beállítása és telepítése a rendszer használatával kapcsolatos egy *helyszíni* a Team Foundation Server, lásd: [Azure felhőszolgáltatások folyamatos kézbesítési](cloud-services-dotnet-continuous-delivery.md).)

Ez az oktatóanyag feltételezi, hogy rendelkezik a Visual Studio 2013 és az Azure SDK telepítése. Ha még nem rendelkezik a Visual Studio 2013, töltse le kiválasztásával a **elkezdheti használni az ingyenes** hivatkozás [www.visualstudio.com](http://www.visualstudio.com). Az Azure SDK telepítése [Itt](http://go.microsoft.com/fwlink/?LinkId=239540).

> [!NOTE]
> Az oktatóanyag elvégzéséhez egy Visual Studio Team Services-fiók szükséges: is [ingyenes Visual Studio Team Services-fiók megnyitása](http://go.microsoft.com/fwlink/p/?LinkId=512979).
> 
> 

Egy felhőalapú szolgáltatás automatikusan építsenek, és telepítse az Azure, a Visual Studio Team Services telepítéséhez kövesse az alábbi lépéseket.

## <a name="1-create-a-team-project"></a>1: team-projekt létrehozása
Kövesse az utasításokat [Itt](http://go.microsoft.com/fwlink/?LinkId=512980) a csapatprojekt létrehozásához, és csatolja a Visual Studio. Ez a forgatókönyv azt feltételezi, hogy a forrás vezérlő megoldásként használja a Team Foundation verzió vezérlő (TFVC). Ha szeretné a Git verziókövető, lásd: [Ez a forgatókönyv Git verziójának](http://go.microsoft.com/fwlink/p/?LinkId=397358).

## <a name="2-check-in-a-project-to-source-control"></a>2: a verziókövetési projektet
1. A Visual Studióban nyissa meg a megoldást, amelyet központilag telepíteni szeretne, vagy hozzon létre egy újat.
   A bemutatóban szereplő lépéseket követve telepítheti a webes alkalmazás vagy a felhőszolgáltatás (Azure-alkalmazás).
   Ha szeretne létrehozni egy új megoldást, hozzon létre egy új Azure Cloud Service-projektet, vagy egy új ASP.NET MVC projekt. Győződjön meg arról, hogy a projekt irányul-e a .NET-keretrendszer 4 vagy 4.5 és a felhőszolgáltatás-projekt létrehozásakor, egy ASP.NET MVC webes és feldolgozói szerepkörök hozzáadása, és válassza a webes szerepkör az internetes alkalmazás. Amikor a rendszer kéri, válassza ki a **az internetes alkalmazás**.
   Ha szeretne létrehozni egy webalkalmazást, válassza ki az ASP.NET-webalkalmazás projekt sablont, és válassza a MVC. Lásd: [egy ASP.NET-webalkalmazás létrehozása az Azure App Service](../app-service-web/app-service-web-get-started-dotnet.md).
   
   > [!NOTE]
   > Visual Studio Team Services csak a jelenleg támogatott CI központi telepítések, a Visual Studio-webalkalmazások. Webhely-projektek kívül esnek a hatókörön.
   > 
   > 
2. Nyissa meg a helyi menüben a megoldás, és válassza a **megoldás hozzáadása a verziókövetési**.
   
    ![][5]
3. Fogadja el vagy módosítsa az alapértelmezett beállításokat, és válassza ki a **OK** gombra. Amennyiben az a folyamat befejeződik, forrás vezérlő ikonok jelennek meg **Megoldáskezelőben**.
   
    ![][6]
4. Nyissa meg a helyi menüben a megoldás, és válassza a **felvétel**.
   
    ![][7]
5. Az a **függőben lévő módosítások** területén **Team Explorer**, írja be a jelölőnégyzetet a Megjegyzés, és válassza ki a **felvétel** gombra.
   
    ![][8]
   
    Vegye figyelembe a beállításokat, vagy adott módosítások kizárja a verziókezelőbe mentésekor. Ha szükséges, nem tartoznak a módosítások, válassza ki azt a **tartalmazza az összes** hivatkozásra.
   
    ![][9]

## <a name="3-connect-the-project-to-azure"></a>3: a projekt csatlakozzon az Azure-bA
1. Most, hogy a Visual STUDIO Team Services csapatprojekt néhány adatforrás kóddal azt, készen áll a csapatprojekt csatlakozzon az Azure-bA.  A a [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), válassza ki a felhőalapú szolgáltatás, vagy a webes alkalmazás, vagy hozzon létre egy újat, válassza ki a  **+**  ikonra a lap alján maradt, és válassza **Felhőszolgáltatás** vagy **webalkalmazás** , majd **Gyorslétrehozás**. Válassza ki a **Visual Studio Team Services való közzététel beállítása** hivatkozásra.
   
    ![][10]
2. A varázslót, írja be a szövegmezőbe, majd kattintson a Visual Studio Team Services fiókja nevét a **engedélyezik most** hivatkozásra. Jelentkezzen be a kérheti.
   
    ![][11]
3. Az a **kapcsolatkérelem** előugró párbeszédpanelen válassza ki a **elfogadás** gombra a Visual STUDIO Team Services a csapatprojekt konfigurálása Azure engedélyezése.
   
    ![][12]
4. Ha engedélyezési sikeres, megjelenik egy legördülő lista a Visual Studio Team Services csapatprojektek listáját tartalmazó. Válassza ki az előző lépésben létrehozott csapatprojekt nevét, és kattintson a pipa gombra a varázsló.
   
    ![][13]
5. Után a projekt ki van kapcsolva, néhány utasítások a Visual Studio Team Services csapatprojekt megváltozik ellenőrzése a következő jelenik meg.  A következő bejelentkezéskor, a Visual Studio Team Services elkészíti és a projekt telepítése az Azure-bA.  Ez most ikonjára kattintva próbálkozzon a **ellenőrizze a Visual Studio** hivatkozásra, majd a **indítsa el a Visual Studio** hivatkozásra (vagy ezzel egyenértékű **Visual Studio** gomb alsó részén a Letöltés gombra).
   
    ![][14]

## <a name="4-trigger-a-rebuild-and-redeploy-your-project"></a>4: egy újjáépítését indítja, és telepítse újra a projekthez
1. A Visual Studio **Team Explorer**, válassza ki a **forrás vezérlő Explorer** hivatkozásra.
   
    ![][15]
2. Keresse meg a megoldásfájlt, majd nyissa meg.
   
    ![][16]
3. A **Megoldáskezelőben**, nyissa meg a fájlt, és módosítsa azt. Módosítsa a fájl például `_Layout.cshtml` a nézetek alatt\\egy MVC webes szerepkör megosztott mappába.
   
    ![][17]
4. Az embléma a hely szerkesztése, és válassza a **Ctrl + S** fájl mentéséhez.
   
    ![][18]
5. A **Team Explorer**, válassza ki a **függőben lévő módosítások** hivatkozásra.
   
    ![][19]
6. Írjon egy megjegyzést, és válassza ki a **felvétel** gombra.
   
    ![][20]
7. Válassza ki a **otthoni** gombra kattintva visszatérhet a **Team Explorer** kezdőlapján.
   
    ![][21]
8. Válassza ki a **buildek** hivatkozásra a buildek megtekintéséhez folyamatban van.
   
    ![][22]
   
    **Vonja össze az Explorer** jeleníti meg, hogy a bejelentkezés a build lett elindítva.
   
    ![][23]
9. Kattintson duplán a összeállítása folyamatban van a részletes napló megtekintéséhez a build előrehaladtával nevére.
   
    ![][24]
10. Bár a összeállítása folyamatban, tekintse meg a build-definíciót, amely jött létre, amikor a varázsló segítségével az Azure-bA a TFS kapcsolódó..  Nyissa meg a helyi menüben a build definíciójához, és válassza a **Build definíciójának szerkesztése**.
    
     ![][25]
    
     Az a **eseményindító** lapon látni fogja, hogy a build definition létrehozásához, minden egyes bejelentkezéskor értéke alapértelmezés szerint.
    
     ![][26]
    
     Az a **folyamat** lapon megtekintheti a telepítési környezet van beállítva a felhőalapú szolgáltatás, vagy a webes alkalmazás neve. Ha a webalkalmazásokkal dolgozik, látható tulajdonságainak eltérő itt látható lesz.
    
     ![][27]
11. Adja meg a tulajdonságok értékeit, ha azt szeretné, hogy a különböző értékeket, mint az alapértelmezett beállításokat. Az Azure közzétételi tulajdonságok szerepelnek a **telepítési** szakasz.
    
     Az alábbi táblázat a rendelkezésre álló tulajdonságok a **telepítési** szakasz:
    
    | Tulajdonság | Alapértelmezett érték |
    | --- | --- |
    | Nem megbízható tanúsítványok engedélyezése |Ha értéke HAMIS, a legfelső szintű hitelesítésszolgáltatóval SSL-tanúsítványokat kell aláírni. |
    | Frissítés engedélyezése |Lehetővé teszi, hogy a központi telepítés helyett újat hoz létre egy meglévő üzemelő példány frissítése. Megőrzi az IP-címet. |
    | Ne törölje |Amennyiben az értéke igaz, ne írja felül a meglévő független telepítés (frissítés engedélyezett). |
    | A telepítési beállítások elérési útja |A fájl elérési útját a .pubxml webes alkalmazások esetén a tárház gyökérkönyvtárában viszonyítva. Cloud services csomag figyelmen kívül hagyja. |
    | SharePoint-környezet |Ugyanaz, mint a szolgáltatás nevét. |
    | Az Azure-telepítés környezet |A webes alkalmazás vagy a felhőbeli szolgáltatás neve. |
12. Ha több szolgáltatáskonfiguráció (.cscfg-fájlok) használ, megadhatja a kívánt szolgáltatás konfigurációját az a **felépítéséhez, speciális, MSBuild-argumentumok** beállítást. Például ServiceConfiguration.Test.cscfg használatához állítsa MSBuild-argumentumok telepítés `/p:TargetProfile=Test`.
    
     ![][38]
    
     Időpontig a build sikeresen meg kell adni.
    
     ![][28]
13. Ha duplán kattint a build nevét, a Visual Studio megjeleníti a **Build összegzés**, többek között a vizsgálati eredmények tartozó egység teszt projektek.
    
     ![][29]
14. Az a [a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkID=213885), a tekintheti meg a társított központi telepítéshez a **központi telepítések** az átmeneti kiválasztásakor fülre.
    
     ![][30]
15. Tallózással keresse meg a webhely URL-címe. A webalkalmazás, kattintson a **Tallózás** gombra a parancssávon. Egy felhőalapú szolgáltatás, válassza az URL-címet a **gyors áttekintése** szakasza a **irányítópult** oldal, amely bemutatja az átmeneti környezetben egy felhőalapú szolgáltatás. A folyamatos integrációt szolgáltatásokhoz központi telepítések alapértelmezés szerint az átmeneti környezetben kerülnek közzétételre. Ez módosítható úgy, hogy a **másik felhőalapú szolgáltatási környezet** tulajdonságot **éles**. Ez képernyőkép jeleníti meg, ha a webhely URL-címe megtalálható-e a felhőalapú szolgáltatás irányítópult-oldalon.
    
    ![][31]
    
    Hogy láthatóvá váljon a futó helyet egy új böngészőlapon nyílik meg.
    
    ![][32]
    
    A felhőszolgáltatások, ha más módosításokat szeretne a projekt, akkor eseményindító több épít fel és meg felhalmozódnak több központi telepítését. A legújabb egy aktív jelölésű.
    
    ![][33]

## <a name="5-redeploy-an-earlier-build"></a>5: telepítse újra egy korábbi verzióját
Ez a lépés nem kötelező, és a felhőszolgáltatások vonatkozik. A klasszikus Azure portálon, válasszon egy korábbi üzemelő, és válassza a **újratelepíteni** a webhely egy korábbi be visszatekerés gombra.  Vegye figyelembe, hogy ezzel a TFS-ben egy új példányt indít, és hozzon létre egy új központi telepítési előzményekben.

![][34]

## <a name="6-change-the-production-deployment"></a>6: módosítsa az éles környezet
Ez a lépés csak a felhőszolgáltatások, nem a webalkalmazások vonatkozik. Ha készen áll, az éles környezetbe átmeneti környezet kiválasztásával előléptetheti az **felcserélése** gombra a klasszikus Azure portálon. Az újonnan telepített átmeneti környezet éles van előléptetve, és az előző éles környezetben, ha van ilyen, lesz egy átmeneti környezet. Az aktív központi telepítéssel üzemi és átmeneti környezetek eltérőek lehetnek, de az üzembe helyezési előzményeket legutóbbi buildek környezet függetlenül ugyanaz.

![][35]

## <a name="7-run-unit-tests"></a>7: egység tesztek futtatása
Ez a lépés csak a webes alkalmazásokat, nem a felhőalapú szolgáltatások vonatkozik. Minőségi kaput elhelyezése a központi telepítés, egység tesztek futtatása, és ha azt elmulasztják, leállíthatja a központi telepítést.

1. A Visual Studio egy egység teszt projekt hozzáadása.
   
   ![][39]
2. A vizsgálni kívánt projekt projekt hivatkozásokat adni.
   
   ![][40]
3. Adja hozzá az egyes egység tesztek. Először próbálja meg egy előzetes vizsgálat, amely mindig adjon át.
   
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
4. Szerkessze a build definíciót, válassza ki a **folyamat** lapot, és bontsa ki a **teszt** csomópont.
5. Állítsa be a **tesztje sikertelen sikertelen épülő** igaz értékre. Ez azt jelenti, hogy a telepítés nem fordulhat elő, ha a teszt sikeresen lezajlott.
   
   ![][41]
6. Várólista új buildverziót.
   
   ![][42]
   
   ![][43]
7. A build folyik, amíg a folyamat ellenőrzéséhez.
   
    ![][44]
   
    ![][45]
8. A build történik, ellenőrizze a teszteredmények.
   
    ![][46]
   
    ![][47]
9. Próbálja meg létrehozni egy tesztet, amely sikertelen lesz. Új teszt hozzáadása az első címtárra másolásával, nevezze át és arról, hogy NotImplementedException ugyanakkor nem várt kódsort megjegyzéssé.
   
       ```
       [TestMethod]
       //[ExpectedException(typeof(NotImplementedException))]
       public void TestMethod2()
       {
           throw new NotImplementedException();
       }
       ```
10. Ellenőrizze, hogy a módosítás várólistára új buildverziót.
    
     ![][48]
11. A hibával kapcsolatos részletek megtekintéséhez a teszteredmények megtekintéséhez.
    
     ![][49]
    
     ![][50]

## <a name="next-steps"></a>Következő lépések
A Visual Studio Team Services tesztelés egység kapcsolatban bővebben lásd: [egység tesztek futtatása a építés](http://go.microsoft.com/fwlink/p/?LinkId=510474). Ha a Git használata esetén lásd: [megosztani a Git kód](http://www.visualstudio.com/get-started/share-your-code-in-git-vs.aspx) és [folyamatos üzembe helyezés az Azure App Service](../app-service-web/app-service-continuous-deployment.md).  További információ a Visual Studio Team Services: [Visual Studio Team Services](http://go.microsoft.com/fwlink/?LinkId=253861).

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
