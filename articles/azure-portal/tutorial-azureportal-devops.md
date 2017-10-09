---
title: "Oktatóanyag: DevOps a hello Azure portálon |} Microsoft Docs"
description: "Ismerje meg a hello Azure portál különböző DevOps munkafolyamatok hello."
services: azure-portal
documentationcenter: 
author: mlearned
manager: douge
editor: mlearned
ms.assetid: 4f1c5bc1-c732-4d35-b5df-0fd68e547d38
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2016
ms.author: mlearned
ms.openlocfilehash: 4c32dbbd4e4b1c3809ef4b01e1496e350183ebde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="tutorial-devops-with-hello-azure-portal"></a>Oktatóanyag: DevOps a hello Azure portál
hello Azure platformon rugalmas DevOps munkafolyamatok megtelt. Ebben az oktatóanyagban elsajátíthatja, hogyan tooleverage hello képességeit hello Azure Portal toodevelop, teszteléséhez, hibaelhárítása, figyelése, és kezelheti a futó alkalmazások. Ez az oktatóanyag következő hello koncentrál:

1. Webalkalmazás létrehozása és a folyamatos üzembe helyezés engedélyezése
2. Alkalmazás fejlesztése és tesztelése
3. Alkalmazás figyelése és hibaelhárítása
4. Általános alkalmazásfelügyeleti feladatok

## <a name="creating-a-web-app-and-enabling-continuous-deployment"></a>Webalkalmazás létrehozása és a folyamatos üzembe helyezés engedélyezése
A webalkalmazás létrehozása [Azure App Service](https://azure.microsoft.com/services/app-service/), amely ebben az oktatóanyagban hello többi fogja használni. Először engedélyeznie kell a folyamatos üzembe helyezést a forráskód tárházából a futó Azure-környezetbe.

1. Jelentkezzen be a hello Azure portál
2. Válasszon **alkalmazásszolgáltatások** &gt; **Hozzáadás ikon** , és adjon meg egy nevet, és válassza ki az előfizetés, hozzon létre egy új erőforrás csoport tooserve hello tárolóként hello szolgáltatás.
   
   Erőforráscsoportok lehetővé teszik toomanage különféle aspektusaihoz rendelkezésre álló hello megoldás például a számlázást, a központi telepítések és keresztül egyetlen csoportként összes figyelési [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).
   
   ![image1][image1]
3. Pár pillanat múlva létrejön az alkalmazásszolgáltatás. Igénybe vehet néhány percet tooexplore hello hello szolgáltatás különböző menüpontok hello portálon.
   
   ![image2][image2]    
4. Kattintson a hello URL-CÍMÉT. Figyelje meg a rendelkezésre álló lehetőségek a eszközök és a tárolóhelyekkel hello különböző. Hello nyelv és keretrendszer az Ön által választott, beleértve a .NET, Java és Ruby is használható.
   
   ![image3][image3]    
5. hello Azure-portál lehetővé teszi a folyamatos üzembe helyezés az egyszerű folyamat, amely csak néhány egyszerű lépést foglal magában. Hello Azure-portálon, a hello ikon az imént létrehozott hello app service beállítások választhat.
   
   ![image4][image4]
   
   Hello panelen jobb hello megnyíló görgessen toohello szakasz közzététele.
   
   ![image5][image5]
6. Ezután konfigurálja az egyes beállítások tooenable folyamatos üzembe helyezés hello alkalmazás. Kattintson a Központi telepítés forrása elemre, és kattintson a Forrás kiválasztása elemre. Figyelje meg hello számos tárház adatforrások beállításait.
   
   ![image6][image6]
7. Ebben a példában válassza a Githubot. Opcionálisan válassza ki az Ön által választott hello tárház és hello engedélyezési hitelesítő adatok beállítása.
   
   ![image7][image7]
8. Engedélyezési tooyour tárház, után dönthet a projekt és a fiókiroda toodeploy kívánja. Alább több fiktív mintapéldát láthat.
   
   ![image8][image8]
9. A projekt és az ág kiválasztása után kattintson az OK gombra. A központi telepítés toosee értesítések kell kezdődnie.
   
   ![image9][image9]
10. Lépjen vissza tooGitHub toosee hello webhook Azure toointegrate hello forrás vezérlő tárház hoztak létre. hello Azure portál lehetővé teszi az integrációt a Githubon csak néhány egyszerű lépésben.
    
    ![image10][image10]
11. toodemonstrate folyamatos üzembe helyezés, gyorsan hozzá néhány tartalom toohello tárházba. Egy egyszerű példa adjon hozzá egy minta szöveges fájl tooa GitHub-tárház. Szabad toouse .NET, Ruby, Python vagy egyéb típusú alkalmazás az App Service áll. Szabad tooadd egy szövegfájlt érzi, hogy az ASP.NET MVC, Java vagy Ruby alkalmazás toohello tárház az Ön által választott.
    
    ![image11][image11]
12. Után tooyour tárház módosítások véglegesítése, megjelenik egy új központi telepítési hello portál értesítési területen lévő kezdeményezni. Ha gyorsan jelennek meg módosítások tooyour tárház véglegesítése után, kattintson a szinkronizálás.
    
    ![image12][image12]
13. Ezen a ponton Ha próbálja és hello app Service hello lap betöltése, a 403-as hiba jelenhet. Ebben a példában ez azért, mert nincs alapértelmezett dokumentum beállítása például egy fájl például index.HTML vagy default.html hello lap. Gyorsan megoldhatja ezt a tooling eszköz az Azure portál hello hello.  A hello Azure portálon válassza a beállítások &gt; alkalmazás beállításait.
    
     ![image13][image13]
14. Megjelenik egy panel az alkalmazásbeállításokkal. Hello lap "SamplePage.html" hello nevét adja meg, és kattintson a Mentés gombra. Igénybe vehet néhány percet tooexplore hello egyéb beállításokat.
    
    ![image14][image14]
15. Opcionálisan frissítse a böngésző URL-cím tooensure várt hello módosításokat láthatják. Ebben az esetben van néhány egyszerű szöveg most feltöltése hello lap. Minden további módosítása toohello tárház egy új automatikus központi telepítési eredményezne.
    
    ![image15][image15]
    
    Folyamatos üzembe helyezés az Azure portál hello engedélyezése egy egyszerű élményt. Összetettebb kiadás folyamatok létrehozhatja és sok más módszerek használata meglévő verziókezelő és folyamatos integrációt rendszerek toodeploy tooAzure, például az automatikus build és kiadás felügyeleti rendszerekkel is.

## <a name="develop-and-test-an-app"></a>Alkalmazás fejlesztése és tesztelése
A következő néhány módosítást toohello kód alap, és gyorsan telepítheti ezeket a módosításokat. Rendszer is telepítenie néhány teljesítménytesztelési hello webalkalmazás.

1. Az Azure portál hello alkalmazásszolgáltatások hello navigációs ablakból válassza, és keresse meg az App Service.
   
   ![image16][image16]
2. Kattintson az Eszközök elemre.
   
   ![image17][image17]
3. Figyelje meg a fejlesztés alatt eszközök kategória hello. Nincsenek számos hasznos eszközök itt, amelyek lehetővé teszik a számunkra alkalmazásokkal toowork hello Azure Portal maradjanak. Kattintson a Konzol elemre.
   
   ![image18][image18]
4. Hello konzolablakban élő parancsok adhat ki az alkalmazásra vonatkozóan. Típus hello dir parancs, és nyomja le a következő adatokat adja meg. Megjegyzendő, hogy az emelt szintű jogosultságokat igénylő parancsok nem működnek.
   
   ![image19][image19]
5. Helyezze vissza toohello Develop kategória, és válassza ki a Visual Studio online-hoz. Megjegyzés: A Visual Studio Online új neve Visual Studio Team Services.
   
   ![image20][image20]
6. Váltás a hello böngészőn belüli szerkesztési funkciót alkalmazásához.
   
   ![image21][image21]
7. Egy webes bővítmény települ az alkalmazáshoz. Bővítmények gyorsan és könnyen egészíthessék funkció tooapps az Azure-ban. Figyelje meg, néhány hello más érhető el az alábbi képernyőképen hello bővítménytípusoknak.
   
   ![image22][image22]
8. Miután telepíti a Visual Studio Online-bővítmény hello, Ugrás gombra.
   
   ![image23][image23]
9. Egy böngészőben lapon válaszoknál láthatja a fejlesztési IDE közvetlenül hello böngészőben megnyílik. Értesítés hello az alábbi szolgáltatás a Chrome-ban.
   
   ![image24][image24]
10. Hajtsa végre több tevékenységeket, például a Szerkesztés fájlok, fájlok és mappák hozzáadása, és a tartalom letöltése a hello élő webhelyet. Készítsen egy gyors Szerkesztés toohello SamplePage.html fájlt.
    
    ![image25][image25]
11. Néhány perc múlva hello módosítások automatikusan megtörténik. Lépjen vissza toohello lapon, ha hello módosítások tekintheti meg. Ne feledje, hogy az ehhez hasonló élő szerkesztések minden valószínűség szerint nem alkalmazhatók az éles környezetben. Azonban hello eszközök gyors módosításokat végezhet, nagyon könnyen toomake a fejlesztési és tesztelési környezetben.
    
    ![image26][image26]
    
    ![image27][image27]
12. Helyezze vissza toohello Eszközök panelen, majd a hello Develop kategória, kattintson a vizsgálat.
    
    ![image28][image28]
13. Tooset team services-fiók szükséges. További információt itt talál: [Team Services-fiók létrehozása](https://www.visualstudio.com/docs/setup-admin/team-services/sign-up-for-visual-studio-team-services).
14. Kattintson az új toocreate teljesítményének teszteléséhez.
    
    ![image29][image29]
    
    Konfigurálása hello különböző értékeket, majd kattintson a hello aljához hello párbeszéd tooinitiate teljesítményének teszteléséhez futtassa tesztelése.
    
    ![image30][image30]
    
    ![image31][image31]
15. Hello teszt futásának indításakor, miután hello állapot figyelheti meg.
    
    ![image32][image32]
    
    Hello teszt befejezése után kattintson a hello eredmény jeleníti meg a további részleteket.
    
    ![image33][image33]
16. Ebben a példában létrehozott egy kis teszt futtatásakor, így korlátozott az tooanalyze, de akkor is tekintse meg a különböző metrikák, valamint futtassa újra a teszt ebben a nézetben. hello Azure Portal létrehozása, végrehajtása és elemzésével webes teljesítménytesztek az egyszerű folyamat teszi. az alábbi képernyőképek hello hello teljesítményadatait jeleníti meg.
    
    ![image34][image34]
    
    ![image35][image35]
    
    ![image36][image36]

## <a name="monitoring-and-troubleshooting-an-app"></a>Alkalmazás figyelése és hibaelhárítása
Az Azure számos funkciót kínál a futó alkalmazások figyeléséhez és hibaelhárításához.

1. Válassza ki a webalkalmazás Azure Portal hello eszközök.
   
   ![image37][image37]
2. A hello kapcsolatos problémák elhárítása kategória futó alkalmazáshoz eszközök tootroubleshoot lehetséges problémák használatára vonatkozó különböző döntések hello figyelje. Lehetőség van például az élő HTTP-forgalom figyelésére, az önjavítás engedélyezésére, a naplók megtekintésére stb.
   
   ![image38][image38]
3. Válassza a Webhelymetrikák tooquickly get egy nézet egy HTTP-kódját.
   
   ![image39][image39]
4. Válassza a Diagnosztika lehetőséget szolgáltatásként. Válassza ki az alkalmazás típusát, majd kattintson a Futtatás gombra.
   
   ![image40][image40]
   
   Elkezdődik az adatgyűjtés.
   
   ![image41][image41]
5. Úgy is dönthet, hello megfelelő napló toodiagnose lehetséges problémák kezeléséhez. Meg kell tooenable naplózási toosee hello rendelkezésre álló adatok például a HTTP-naplók beállításai közül.
   
   ![image42][image42]
   
   Hello memóriakép fájl letöltése és elemezheti a debugdiag lehetőséget kattintva elemző jelentés toohelp található lehetséges problémák kezeléséhez.
   
   ![image43][image43]
6. tooview több adatot tooenable további naplózás van szüksége. Az hello Azure portál keresse meg a toohello webalkalmazást, és válassza a beállítások.
   
   ![image44][image44]
7. Görgessen lefelé toohello szolgáltatások kategóriát, és válassza a diagnosztikai naplókat.
   
      ![image45][image45]
8. Figyelje meg, hello különféle naplózási beállításai. Kapcsolja be a webkiszolgálók naplózását, és kattintson a Mentés gombra.
   
   ![image46][image46]
9. Helyezze vissza toohello eszközök terület hello alkalmazáshoz, és válassza ki a diagnosztika szolgáltatásként, és kattintson a Futtatás toorerun hello adatgyűjtés.
   
   ![image47][image47]
10. Hello HTTP-naplózás beállítás engedélyezése esetén a HTTP-naplók gyűjtött adatok ekkor megjelenik.
    
    ![image48][image48]
11. Hello HTML-fájl napló kattintva további vizsgálatok gazdag böngészőalapú jelentést készít.
    
    ![image49][image49]
12. Visszalépés toohello eszközök szakasz hello Azure Portal hello alkalmazás. Görgessen toohello eszközök szakaszban, és válassza ki a Process Explorer.
    
    ![image50][image50]
13. A Process Explorer kiválasztásával megtekintheti a futó folyamatok adatait. Figyelje meg, az alábbi folyamatok elemezze, és még kill minden hello Azure-portál a folyamatokat.
    
    ![image51][image51]
    
    ![image52][image52]
14. Visszalépés toohello beállítások panel hello bal oldalon. Kattintson az Új támogatási kérelem elemre.
    
    ![image53][image53]
15. Hello panelen a jobb oldali hello töltse ki a hello hibákkal kapcsolatos információkat, adja meg a kapcsolattartási adatokat, és még a diagnosztikai adatok feltöltése. hello Azure Portal lehetővé teszi, hogy a Microsoft támogatási szolgálatához zökkenőmentes élményt használata.
    
    ![image54][image54]
    
    ![image55][image55]
    
    hello Azure Portal biztosítanak a hatékony és ismerős lép toohelp figyelő tooling eszköz segítségével, és hibaelhárítása a futó alkalmazások. Biztosan is képes tootake művelet gyorsan folyamatok újrahasznosítása, engedélyezése és letiltása különböző adatok gyűjtemények és a Microsoft professzionális támogatás még integrálása feladatok végrehajtásával.

## <a name="general-application-management"></a>Általános alkalmazásfelügyelet
Alkalmazások kezelése, ha gyakran kell tooperform tevékenységek, például a biztonsági mentési stratégia, végrehajtási és identitás-szolgáltatóktól kezelése és a szerepköralapú hozzáférés-vezérlés konfigurálása széles választékában. A hello más DevOps lép, mivel hello Azure platformon integrálódik ezeket a feladatokat közvetlenül hello portálon.

1. amelyek megőrzésével tooensure hello webalkalmazás tooconfigure biztonsági mentések szüksége biztonságos adatvesztés ellen. Keresse meg a webalkalmazás toohello beállítások területen.
   
   ![image56][image56]
2. Hello panelen a jobb oldali hello görgetve toohello szolgáltatásai kategóriában.
   
    ![image57][image57]
3. Válassza ki a biztonsági mentések; a jobb oldali hello megnyílik egy panel.
   
   ![image58][image58]
4. Kattintson a Konfigurálás gombra, válasszon tárfiókot hello panelen a jobb oldali hello.
   
   ![image59][image59]
5. Most hozzon létre, és válassza ki a tárolási tároló toohold a biztonsági másolatok. Kattintson a létrehozás hello hello panel alsó részén. Válassza ki a hello tároló.
   
   ![image60][image60]
6. Miután kiválasztotta a hello tároló, az adatbázisok a ütemezéseket, valamint a telepítés biztonsági mentések állíthatja be. Ebben az esetben kattintson a mentés ikon hello.
   
    ![image61][image61]
7. A mentés után görgessen hello bal oldali hátsó toohello panel a biztonsági mentésekhez. Kattintson a biztonsági mentés most tooback hello alkalmazás.
   
    ![image62][image62]
8. Rövidesen múlva megjelenik a létrehozott biztonsági másolat. Értesítés hello visszaállítás most az alábbi képernyőfelvételen hello lehetőséget.
   
    ![image63][image63]
9. Kattintson a visszaállítás most, és vizsgálja meg jobb hello hello beállítások toohello paneljét. Lehetősége van egy megfelelő biztonsági mentés és egyszerű visszaállítás tooan korábbi állapot szükség szerint. hello Azure-portálon hozzájárult, könnyen engedélyezheti egy egyszerű vész-helyreállítási stratégiát hello alkalmazást.
   
    ![image64][image64]
10. Visszalépés toohello beállítások panel hello bal oldali és a szolgáltatások, és válassza ki a hitelesítési/engedélyezési.
    
     ![image65][image65]
11. Hello panelen a jobb oldali hello válassza ki az App Service hitelesítés. Figyelje meg hello a számos népszerű szolgáltatók konfigurálható.
    
     ![image66][image66]
12. Válassza ki azt az Ön által választott hello szolgáltatót, és figyelje meg hello hatókör hello beállításait. Adjon meg egy alkalmazás Azonosítóját és egy alkalmazás titkos kulcs, és engedélyezheti a Facebook-hitelesítési hello alkalmazás. hello Azure portál lehetővé teszi a hitelesítést az alkalmazások azonnal használható megoldás.
    
     ![image67][image67]
13. Helyezze vissza toohello beállítások panelen, és válassza ki a felhasználók hello erőforrás kezelése kategóriához tartozó.
    
     ![image68][image68]
14. Vizsgálja meg a megfelelő hello hello paneljét hello különböző beállítások a szerepkörök és a felhasználók hozzáadásához. hello Azure portál segítségével szabályozhatja könnyen RBAC (szerepköralapú hozzáférés-vezérlés) hello alkalmazáshoz.
    
     ![image69][image69]

## <a name="summary"></a>Összefoglalás
Ez az oktatóanyag bemutatott egyes hello power hello Azure platformon, gyorsan engedélyezése az webalkalmazáshoz a folyamatos üzembe helyezés, hajt végre a különböző fejlesztési és tesztelési tevékenységeket, figyelés és hibaelhárítás élő app, végül és kezelhet, és kulcs például a vész-helyreállítási, identitáskezelési és szerepköralapú hozzáférés-vezérlés stratégiák. hello Azure platform lehetővé teszi, hogy a DevOps munkafolyamatok integrált megoldást, és dolgozhat hatékonyan által az aktuális hello feladathoz tartozó környezet marad.

## <a name="next-steps"></a>Következő lépések
* Az Azure Resource Manager fontos DevOps engedélyezését hello Azure platformon.  több látogasson el az toolearn [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).
* Azure App Service telepítési olvashat toolearn [az alkalmazás tooAzure App Service telepítése](../app-service-web/web-sites-deploy.md)

[image1]: ./media/tutorial-azureportal-devops/image1.png
[image2]: ./media/tutorial-azureportal-devops/image2.png
[image3]: ./media/tutorial-azureportal-devops/image3.png
[image4]: ./media/tutorial-azureportal-devops/image4.png
[image5]: ./media/tutorial-azureportal-devops/image5.png
[image6]: ./media/tutorial-azureportal-devops/image6.png
[image7]: ./media/tutorial-azureportal-devops/image7.png
[image8]: ./media/tutorial-azureportal-devops/image8.png
[image9]: ./media/tutorial-azureportal-devops/image9.png
[image10]: ./media/tutorial-azureportal-devops/image10.png
[image11]: ./media/tutorial-azureportal-devops/image11.png
[image12]: ./media/tutorial-azureportal-devops/image12.png
[image13]: ./media/tutorial-azureportal-devops/image13.png
[image14]: ./media/tutorial-azureportal-devops/image14.png
[image15]: ./media/tutorial-azureportal-devops/image15.png
[image16]: ./media/tutorial-azureportal-devops/image16.png
[image17]: ./media/tutorial-azureportal-devops/image17.png
[image18]: ./media/tutorial-azureportal-devops/image18.png
[image19]: ./media/tutorial-azureportal-devops/image19.png
[image20]: ./media/tutorial-azureportal-devops/image20.png
[image21]: ./media/tutorial-azureportal-devops/image21.png
[image22]: ./media/tutorial-azureportal-devops/image22.png
[image23]: ./media/tutorial-azureportal-devops/image23.png
[image24]: ./media/tutorial-azureportal-devops/image24.png
[image25]: ./media/tutorial-azureportal-devops/image25.png
[image26]: ./media/tutorial-azureportal-devops/image26.png
[image27]: ./media/tutorial-azureportal-devops/image27.png
[image28]: ./media/tutorial-azureportal-devops/image28.png
[image29]: ./media/tutorial-azureportal-devops/image29.png
[image30]: ./media/tutorial-azureportal-devops/image30.png
[image31]: ./media/tutorial-azureportal-devops/image31.png
[image32]: ./media/tutorial-azureportal-devops/image32.png
[image33]: ./media/tutorial-azureportal-devops/image33.png
[image34]: ./media/tutorial-azureportal-devops/image34.png
[image35]: ./media/tutorial-azureportal-devops/image35.png
[image36]: ./media/tutorial-azureportal-devops/image36.png
[image37]: ./media/tutorial-azureportal-devops/image37.png
[image38]: ./media/tutorial-azureportal-devops/image38.png
[image39]: ./media/tutorial-azureportal-devops/image39.png
[image40]: ./media/tutorial-azureportal-devops/image40.png
[image41]: ./media/tutorial-azureportal-devops/image41.png
[image42]: ./media/tutorial-azureportal-devops/image42.png
[image43]: ./media/tutorial-azureportal-devops/image43.png
[image44]: ./media/tutorial-azureportal-devops/image44.png
[image45]: ./media/tutorial-azureportal-devops/image45.png
[image46]: ./media/tutorial-azureportal-devops/image46.png
[image47]: ./media/tutorial-azureportal-devops/image47.png
[image48]: ./media/tutorial-azureportal-devops/image48.png
[image49]: ./media/tutorial-azureportal-devops/image49.png
[image50]: ./media/tutorial-azureportal-devops/image50.png
[image51]: ./media/tutorial-azureportal-devops/image51.png
[image52]: ./media/tutorial-azureportal-devops/image52.png
[image53]: ./media/tutorial-azureportal-devops/image53.png
[image54]: ./media/tutorial-azureportal-devops/image54.png
[image55]: ./media/tutorial-azureportal-devops/image55.png
[image56]: ./media/tutorial-azureportal-devops/image56.png
[image57]: ./media/tutorial-azureportal-devops/image57.png
[image58]: ./media/tutorial-azureportal-devops/image58.png
[image59]: ./media/tutorial-azureportal-devops/image59.png
[image60]: ./media/tutorial-azureportal-devops/image60.png
[image61]: ./media/tutorial-azureportal-devops/image61.png
[image62]: ./media/tutorial-azureportal-devops/image62.png
[image63]: ./media/tutorial-azureportal-devops/image63.png
[image64]: ./media/tutorial-azureportal-devops/image64.png
[image65]: ./media/tutorial-azureportal-devops/image65.png
[image66]: ./media/tutorial-azureportal-devops/image66.png
[image67]: ./media/tutorial-azureportal-devops/image67.png
[image68]: ./media/tutorial-azureportal-devops/image68.png
[image69]: ./media/tutorial-azureportal-devops/image69.png
