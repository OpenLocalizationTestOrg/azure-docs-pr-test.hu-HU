---
title: "Egy alapszintű Azure-webalkalmazás létrehozása az IntelliJ |} Microsoft Docs"
description: "Az oktatóanyag bemutatja, hogyan használható az IntelliJ Azure eszköztára Hello World webalkalmazás létrehozása az Azure."
services: app-service\web
documentationcenter: java
author: selvasingh
manager: erikre
editor: 
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 20b2c3d86f5bde9302647f345aa99b030d78613a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-basic-azure-web-app-in-intellij"></a>Egy alapszintű Azure-webalkalmazás létrehozása az intellij-t
Ez az oktatóanyag bemutatja, hogyan hozhat létre és telepíthet egy alapszintű Hello World alkalmazásról az Azure-ba, a webes alkalmazás segítségével a [IntelliJ Azure eszköztára]. Egy alapszintű JSP példa az egyszerűség, de hasonló lépésekkel akkor lehet megfelelő, egy Java servlet, az Azure-telepítés illetően.

Ez az oktatóanyag befejezése után, az alkalmazás majd meg az alábbi ábrához hasonlóan egy webböngészőben megtekintheti:

![A minta-weblap][01]

## <a name="prerequisites"></a>Előfeltételek
* A Java fejlesztői készlet (JDK), v 1,8 vagy újabb.
* Az IntelliJ IDEA Ultimate Edition. Ez letölthető <https://www.jetbrains.com/idea/download/index.html>.
* A Java-alapú webkiszolgáló vagy a kiszolgáló, terjesztési például [Apache Tomcat] vagy [Jetty].
* Azure-előfizetéssel, amely szerezhető: a <https://azure.microsoft.com/free/> vagy <http://azure.microsoft.com/pricing/purchase-options/>.
* A [IntelliJ Azure eszköztára]. Az Azure-eszközkészlet telepítésével kapcsolatos információkért lásd: [az intellij-t az Azure eszközkészlet telepítésével].

## <a name="to-create-a-hello-world-application"></a>Hello World alkalmazás létrehozása
Először először foglalkozunk létrehoz egy Java-projektet.

1. Indítsa el az intellij-t, és kattintson a **fájl** menü, majd kattintson a **új**, és kattintson a **projekt**.
   
    ![Fájl új projekt][02]
2. Jelölje ki az új projekt párbeszédpanel **Java**, majd **webalkalmazás**, és kattintson a **új** hozzáadása a projekthez SDK.
   
    ![Új projekt párbeszédpanel][03a]
   
3. A Kezdőkönyvtár kiválasztása a JDK párbeszédpanel megnyitásához, válassza ki a mappát, ahol a JDK telepítve van, és kattintson **OK**. Kattintson a **következő** folytatja az új projekt párbeszédpanel.
   
    ![Adja meg a JDK kezdőkönyvtára][03b]
4. Ebben az oktatóanyagban a nevet a projektnek **Java-webalkalmazás-alkalmazás-a-Azure**, és kattintson a **Befejezés**.
   
    ![Új projekt párbeszédpanel][04]
5. Az IntelliJ a Project Explorer nézet, bontsa ki a **Java-webalkalmazás-alkalmazás-a-Azure**, majd bontsa ki a **webes**, majd kattintson duplán **index.jsp**.
   
    ![Nyissa meg az Index lap][05c]
6. Az index.jsp fájl megnyitásakor az IntelliJ beépülő modul dinamikusan megjelenő szöveg **Hello World!** a már meglévő `<body>` elemhez. A frissített `<body>` tartalma az alábbihoz kell hasonlítania:
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
7. Mentse az index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Egy Azure Web App tárolóhoz az alkalmazás központi telepítése
Számos módon Azure Java-webalkalmazás-telepíthet. Ez az oktatóanyag leírja a legegyszerűbb egyikét: az alkalmazás telepítve lesz az Azure Web App tároló - semmilyen különleges projekttípus sem további eszközök van szükség. A JDK és a webes tároló szoftver biztosítja az Ön Azure-ban, nincs szükség töltse fel a saját; szüksége a Java-webalkalmazás. Ennek eredményeképpen a közzétételt az alkalmazás érvénybe másodperc, nem perc.

Az alkalmazás közzététele előtt először a modul beállítások konfigurálásához. Ehhez a következőket kell tennie:

1. Az intellij-t a Project Explorer, kattintson a jobb gombbal a **Java-webalkalmazás-alkalmazás-a-Azure** projekt. Amikor a helyi menü megjelenik, kattintson a **beállítások modul megnyitása**.

    ![Nyissa meg a modul beállításai][05a]
2. Amikor megjelenik a projekt struktúra párbeszédpanel:

   a. Kattintson a **összetevők** listájában **Projektbeállítások**.
   b. Az összetevő neve, a módosítása a **neve** jelölőnégyzetet, hogy nem tartalmaz szóközt vagy speciális karaktereket; Ez azért szükséges, mivel a név lesz a az egységes erőforrás-azonosító (URI).
   c. Módosítsa a **típus** való **webes alkalmazás: archív**.
   d. Kattintson a **OK** a projekt struktúra párbeszédpanel bezárásához.

    ![Nyissa meg a modul beállításai][05b]

A modul beállítások konfigurálásakor az alábbi lépéseket követve teheti közzé az alkalmazás az Azure-bA:

1. Az intellij-t a Project Explorer, kattintson a jobb gombbal a **Java-webalkalmazás-alkalmazás-a-Azure** projekt. Ha a helyi menü megjelenik, válassza ki a **Azure**, és kattintson a **Azure Web Apps közzététel...**
   
    ![Azure közzététel helyi menü][06]
2. Ha már nem regisztrált az Azure az intellij-t, kérni fogja az Azure-fiókjával bejelentkezni. (Még több Azure-fiókra, egyes az utasításokat a bejelentkezési folyamat során előfordulhat, hogy megjelenik-e egynél többször, akkor is, ha úgy tűnik, mintha azonos. Ha ez történik, továbbra is kövesse az utasításokat a bejelentkezéskor.)
   
    ![Az Azure bejelentkezési párbeszédpanel][07]
3. Miután sikeresen bejelentkezett az Azure-fiókjával, a a **előfizetések kezelése oldalt** párbeszédpanelen jelenik meg a hitelesítő adatok társított előfizetések listáját. (Ha nem szerepel a listában több előfizetéssel, és csak egy adott részével azokat használni kívánt, előfordulhat, hogy opcionálisan, törölje a jelet az előfizetések nem kívánja használni.) Miután kiválasztotta az előfizetések, kattintson a **Bezárás**.
   
    ![Előfizetések kezelése][08]
4. Ha a **központi telepítése az Azure Web App tárolóhoz** párbeszédpanel jelenik meg, jeleníti meg, hogy a korábban létrehozott webalkalmazás tárolókkal; Ha nem hozott létre tárolót, a lista üres lesz.
   
    ![Alkalmazás tárolók][09]
5. Ha nem hozott létre egy Azure webes alkalmazás tároló előtt, vagy ha azt szeretné, az alkalmazás egy új tároló közzétételére, kövesse az alábbi lépéseket. Ellenkező esetben válassza ki a meglévő Web App tároló, és folytassa a 6. lépés alatt.
   
   1. Kattintson a**+**
      
       ![Alkalmazás-tároló hozzáadása][10]
   2. A **új webes alkalmazás tároló** párbeszédpanel jelenik meg, amely az alábbi lépéseket követve fog történni.
      
       ![Új alkalmazás-tároló][11a]
   3. Adjon meg egy **DNS-címke** a webes alkalmazás tároló; ez alkotó a levél DNS-címke a gazdagépre URL-címének a webalkalmazás az Azure-ban. Vegye figyelembe, hogy a név kell elérhetőnek lennie Azure Web Apps követelmények elnevezési felelnek meg.
   4. Az a **webes tároló** legördülő menüben válassza ki a megfelelő szoftver az alkalmazáshoz.
      
       Jelenleg Tomcat 8 Tomcat 7 vagy Jetty 9 közül választhat. A kiválasztott szoftverfrissítés legutóbbi terjesztési Azure biztosítja, és akkor fog futni, a legutóbbi terjesztésipont JDK 8 Oracle által létrehozott és az Azure által biztosított.
   5. Az a **előfizetés** legördülő menüben válassza ki az ehhez a központi telepítéshez használni kívánt előfizetést.
   6. Az a **erőforráscsoport** legördülő menüben válasszon, amelyhez a webalkalmazás hozzárendelni kívánt erőforráscsoportot. (Az azure erőforráscsoportok lehetővé teszik kapcsolódó erőforrások egy csoportba, így például, hogy törölheti együtt.)
      
       Válasszon ki egy meglévő erőforráscsoportot (ha vannak ilyenek) és a skip. lépés: az alábbi g, vagy hozzon létre egy új erőforráscsoportot a következő lépések segítségével:
      
      * Válassza ki  **&lt; &lt; hozzon létre új erőforráscsoportot &gt; &gt;**  a a **erőforráscsoport** legördülő menü.
      * A **új erőforráscsoport** párbeszédpanel fog megjelenni:
        
          ![Új erőforráscsoport][12]
      * Az a a **neve** szövegmező, adja meg az új erőforráscsoport nevét.
      * Az a a **régió** legördülő menüben válassza a megfelelő Azure adatközpont-erőforrásnak a helyét.
      * Kattintson az **OK** gombra.
   7. A **App Service-csomag** legördülő menü mutatja az erőforráscsoport kiválasztott társított app service-csomagokról. (A következő információkat, például a Web App alkalmazásban az árképzési szint és a számítási példányméretének helyét egy App Service-csomag határozza meg. Egy egyetlen App Service-csomag használható több Web Apps, ezért azt egy adott webalkalmazás telepítésből külön megmarad.)
      
       Válasszon ki egy meglévő App Service-csomag (ha vannak ilyenek) és skip h az alábbi lépések, vagy hozzon létre egy új App Service-csomag a következő lépések segítségével:
      
      * Válassza ki  **&lt; &lt; új App Service-csomag létrehozása &gt; &gt;**  a a **App Service-csomag** legördülő menü.
      * A **új App Service-csomag** párbeszédpanel fog megjelenni:
        
          ![Új App Service-csomag][13]
      * Az a a **neve** szövegmező, adjon meg egy nevet az új App Service-csomag.
      * Az a a **hely** legördülő menüben válassza a megfelelő Azure adatközpont helye a terv.
      * Az a a **Tarifacsomagot** legördülő menüben válassza ki a megfelelő árképzési séma. Tesztelési célokra választhat **szabad**.
      * Az a a **Példányméretének** legördülő menüben válassza a terv méretezés a megfelelő példányt. Tesztelési célokra választhat **kis**.
      * Kattintson az **OK** gombra.
   8. (Választható) Alapértelmezés szerint Java 8 legutóbbi eloszlásáról automatikusan telepíti a JVM-et, az Azure-ban a webes alkalmazás tároló. Választhatja azonban egy másik verzió és elosztását illetően a JVM-et. Ehhez a következőket kell tennie:
      
      * Kattintson a **JDK** lapra a **új webes alkalmazás tároló** párbeszédpanel megnyitásához.
      * Az alábbi lehetőségek közül választhat:
        
        * Az alapértelmezett JDK, amely az Azure által kínált telepítése
        * 3. fél JDK Azure rendelkezésre álló további JDKs legördülő listájából telepítése
        * Központi telepítése egy egyéni JDK, és be kell csomagolni, és vagy egy ZIP-fájl nyilvánosan elérhető vagy az Azure storage-fiók
        
        ![Új alkalmazás tároló JDK lapon][11b]
   9. A fenti lépés befejeződött, a webes alkalmazás új tároló párbeszédpanelen a következő ábra kell hasonlítania:
      
       ![Új alkalmazás-tároló][14]
   10. Kattintson a **OK** az új webalkalmazáshoz tároló létrehozásának befejezéséhez.
       
        Várjon néhány másodpercet, frissíteni kell a webalkalmazás-tárolók listáját, és az újonnan létrehozott webes alkalmazás tároló most már ki a listában.
6. Most már készen áll a webalkalmazás Azure; a kezdeti telepítés befejezéséhez Kattintson a **OK** a kijelölt webalkalmazás tárolóhoz a Java-alkalmazás központi telepítése. Alapértelmezés szerint az alkalmazás telepítve, a kiszolgáló alkönyvtára lehet. Ha azt szeretné, hogy a legfelső szintű alkalmazás telepíthető központilag, ellenőrizze a **legfelső szintű telepítés** való kattintás előtt jelölőnégyzet **OK**.
   
    ![Telepítse az Azure][15]
7. Ezt követően kell megjelennie a **Azure tevékenységnapló** nézet, ami a webalkalmazás központi telepítési állapotát jelzi.
   
    ![Folyamatjelző][16]
   
    A webalkalmazás az Azure-bA üzembe helyezésének folyamata gyorsabban csak néhány másodpercig. Ha alkalmazás készen áll, megjelenik egy hivatkozás nevű **közzétett** a a **állapot** oszlop. A hivatkozásra kattintva, időt vesz igénybe, a telepített webalkalmazás kezdőlap, vagy tallózással keresse meg a webalkalmazás a következő szakaszban a lépéseket is használhat.

## <a name="browsing-to-your-web-app-on-azure"></a>A webalkalmazás Azure tallózással
Tallózással keresse meg a webalkalmazás az Azure-on, használhatja a **Azure Explorer** nézet.

Ha a **Azure Explorer** nézet még nincs megnyitva, majd kattintva megnyithatja azt **nézet** menü az intellij-t, majd kattintson a **eszköz Windows**, és kattintson a  **Szolgáltatás Explorer**. Ha korábban nem jelentkezett be, akkor arra fogja kérni teszi.

Ha a **Azure Explorer** nézet jelenik meg, használja a lépések végrehajtásával keresse meg a webalkalmazás: 

1. Bontsa ki a **Azure** csomópont.
2. Bontsa ki a **webalkalmazások** csomópont. 
3. Kattintson a jobb gombbal a kívánt webalkalmazáshoz.
4. Amikor a helyi menü megjelenik, kattintson a **nyissa meg böngészőben**.
   
    ![Keresse meg a webes alkalmazás][17]

## <a name="updating-your-web-app"></a>Webalkalmazás frissítése
Frissítése egy meglévő Azure-webalkalmazás fut egy gyors és egyszerű eljárás, és frissítési lehetőségei vannak:

* Java-webalkalmazás telepítése frissítheti.
* Egy webes alkalmazás tárolóhoz további Java-alkalmazást is közzétehet.

Mindkét esetben a folyamat megegyezik, és csupán néhány másodpercet vesz igénybe:

1. Az IntelliJ project explorer kattintson a jobb gombbal a Java-alkalmazást szeretne frissíteni, vagy meglévő Web App tároló hozzáadása.
2. Ha a helyi menü megjelenik, válassza ki a **Azure** , majd **Azure Web Apps közzététel...**
3. Már már bejelentkezett korábban, mivel látni fogja a meglévő Web App tárolók listája. Jelölje ki a közzétenni vagy újra közzé a Java-alkalmazást, és kattintson a kívánt **OK**.

Néhány másodperc múlva, a **Azure tevékenységnapló** nézetben megjelenik, a frissített telepítési **közzétett** és nem fogja tudni ellenőrizni a frissített alkalmazást egy webböngészőben.

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a>Indítása, leállítása vagy egy már meglévő webalkalmazás újraindítása
Indítsa el, vagy állítsa le a meglévő Azure Web Apps tároló, (beleértve az összes telepített Java-alkalmazások azt), használja a **Azure Explorer** nézet.

Ha a **Azure Explorer** nézet még nincs megnyitva, majd kattintva megnyithatja azt **nézet** menü az intellij-t, majd kattintson a **eszköz Windows**, és kattintson a  **Szolgáltatás Explorer**. Ha korábban nem jelentkezett be, akkor arra fogja kérni teszi.

Ha a **Azure Explorer** nézet jelenik meg, használjon kövesse az alábbi lépéseket indítása vagy leállítása a webalkalmazás: 

1. Bontsa ki a **Azure** csomópont.
2. Bontsa ki a **webalkalmazások** csomópont. 
3. Kattintson a jobb gombbal a kívánt webalkalmazáshoz.
4. Amikor a helyi menü megjelenik, kattintson a **Start**, **leállítása**, vagy **indítsa újra a**. Vegye figyelembe, hogy a menüjéből választások környezet használatát, így csak a futó webalkalmazás leállítása vagy egy webalkalmazást, ami jelenleg nem fut. Indítsa el.
   
    ![A webalkalmazás leállítása][18]

## <a name="next-steps"></a>Következő lépések
A Java IDE környezetekhez készült Azure-eszközkészlettel kapcsolatos további információkért lásd az alábbi hivatkozásokat:

* [Eclipse Azure eszköztára]
  * [Az Eclipse-hez készült Azure-eszközkészlet telepítése]
  * [Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]
  * [Az Eclipse-hez készült Azure-eszközkészlet újdonságai]
* [IntelliJ Azure eszköztára]
  * [az intellij-t az Azure eszközkészlet telepítésével]
  * *Hello World webalkalmazás létrehozása az Azure-hoz az intellij-t (Ez a cikk)*
  * [Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]

<a name="see-also"></a>

## <a name="see-also"></a>Lásd még:
Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ].

Azure Web Apps létrehozásával kapcsolatos további információkért lásd: a [webes alkalmazások – áttekintés].

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Eclipse Azure eszköztára]: ../azure-toolkit-for-eclipse.md
[IntelliJ Azure eszköztára]: ../azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Az Eclipse-hez készült Azure-eszközkészlet telepítése]: ../azure-toolkit-for-eclipse-installation.md
[az intellij-t az Azure eszközkészlet telepítésével]: ../azure-toolkit-for-intellij-installation.md
[Az Eclipse-hez készült Azure-eszközkészlet újdonságai]: ../azure-toolkit-for-eclipse-whats-new.md
[Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/
[webes alkalmazások – áttekintés]: ./app-service-web-overview.md
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[01]: ./media/app-service-web-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-SDK-Dialog.png
[04]: ./media/app-service-web-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05a]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Module-Settings.png
[05b]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Project-Structure-Dialog.png
[05c]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/app-service-web-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/app-service-web-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/app-service-web-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/app-service-web-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/app-service-web-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11a]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container.png
[11b]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container-JDK-Tab.png
[12]: ./media/app-service-web-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/app-service-web-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/app-service-web-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/app-service-web-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/app-service-web-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/app-service-web-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/app-service-web-intellij-create-hello-world-web-app/18-Stop-Web-App.png
