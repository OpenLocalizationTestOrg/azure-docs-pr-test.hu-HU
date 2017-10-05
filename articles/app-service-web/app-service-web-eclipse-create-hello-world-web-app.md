---
title: "Alapszintű Azure-webalkalmazás létrehozása Eclipse használatával |} Microsoft Docs"
description: "Az oktatóanyag bemutatja, hogyan használható az Azure-eszközkészlet az eclipse-ben Hello World webalkalmazás létrehozása az Azure."
services: app-service\web
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 20d41e88-9eab-462e-8ee3-89da71e7a33f
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm;asirveda
ms.openlocfilehash: 683d6828546995a4dc60d8cac0669f356501c723
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-basic-azure-web-app-using-eclipse"></a>Alapszintű Azure-webalkalmazás létrehozása használata az eclipse-ben
Ez az oktatóanyag bemutatja, hogyan hozhat létre és telepíthet egy alapszintű Hello World alkalmazásról az Azure-ba, a webes alkalmazás segítségével a [Eclipse-hez készült Azure-eszközkészlet]. Egy alapszintű JSP példa az egyszerűség, de hasonló lépésekkel akkor lehet megfelelő, egy Java servlet, az Azure-telepítés illetően.

Ez az oktatóanyag befejezése után, az alkalmazás majd meg az alábbi ábrához hasonlóan egy webböngészőben megtekintheti:

![Előzetes verziójú Hello World alkalmazás][01]

## <a name="prerequisites"></a>Előfeltételek
* A Java fejlesztői készlet (JDK), v 1,8 vagy újabb.
* Java EE-fejlesztőknek, Luna IDE Holdas vagy újabb. Ez letölthető <http://www.eclipse.org/downloads/>.
* A Java-alapú webkiszolgáló vagy a kiszolgáló, terjesztési például [Apache Tomcat] vagy [Jetty].
* Azure-előfizetéssel, amely szerezhető: a <https://azure.microsoft.com/free/> vagy <http://azure.microsoft.com/pricing/purchase-options/>.
* Az [Eclipse-hez készült Azure-eszközkészlet]. Az Azure-eszközkészlet telepítésével kapcsolatos információkért lásd: [az eclipse-ben az Azure eszközkészlet telepítésével].

## <a name="to-create-a-hello-world-application"></a>Hello World alkalmazás létrehozása
Először először foglalkozunk létrehoz egy Java-projektet.

1. Indítsa el az Eclipse, és válassza a menü **fájl**, kattintson a **új**, és kattintson a **dinamikus webes projekt**. (Ha nem lát **dinamikus webes projekt** kattintás után az elérhető projektek tulajdonosaként **fájl** és **új**, majd tegye a következőket: kattintson a **fájl**, kattintson a **új**, kattintson a **projekt...** , bontsa ki a **webes**, kattintson a **dinamikus webes projekt**, és kattintson a **következő**.)
2. Ebben az oktatóanyagban a nevet a projektnek **MyWebApp**. A képernyő jelenik meg a következőhöz hasonló:
   
    ![Új dinamikus webes projekt létrehozása][02]
3. Kattintson a **Befejezés** gombra.
4. A Eclipse Project Explorer nézet, bontsa ki a **MyWebApp**. Kattintson a jobb gombbal a **WebContent** (Webes tartalom), majd a **New** (Új) elemre, és végül a **JSP File** (JSP-fájl) elemre.
5. A a **új JSP-fájl** párbeszédablakban nevezze el a fájl **index.jsp**, mint a szülőmappa **MyWebApp/WebContent**, és kattintson a **következő**.
6. Az a **JSP-sablon kiválasztása** párbeszédpanel, az oktatóanyag válasszon alkalmazásában **új JSP-fájl (html)**, és kattintson a **Befejezés**.
7. Megnyitásakor az index.jsp fájl az eclipse-ben, adja hozzá a dinamikusan megjelenő szöveg **Hello World!** a már meglévő `<body>` elemhez. A frissített `<body>` tartalma az alábbihoz kell hasonlítania:
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
8. Mentse az index.jsp.

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a>Egy Azure Web App tárolóhoz az alkalmazás központi telepítése
Számos módon Azure Java-webalkalmazás-telepíthet. Ez az oktatóanyag leírja a legegyszerűbb egyikét: az alkalmazás telepítve lesz az Azure Web App tároló - semmilyen különleges projekttípus sem további eszközök van szükség. A JDK és a webes tároló szoftver biztosítja az Ön Azure-ban, nincs szükség töltse fel a saját; szüksége a Java-webalkalmazás. Ennek eredményeképpen a közzétételt az alkalmazás érvénybe másodperc, nem perc.

1. A Project Explorer Eclipse meg, kattintson a jobb gombbal **MyWebApp**.
2. Válassza ki a helyi menü **Azure**, majd kattintson a **Publish Azure Web Apps...**
   
    ![Az Azure webes alkalmazás közzététele][03]
   
    Azt is megteheti, amíg a webes projektet a Project Explorer van jelölve, kattintson a **közzététel** legördülő gomb az eszköztáron és a select **Publish Azure Web Apps,** innen:
   
    ![Az Azure webes alkalmazás közzététele][14]
3. Ha már nem regisztrált az Azure az eclipse-ben, a rendszer kéri, jelentkezzen be az Azure-fiókjába való:
   
    ![Az Azure bejelentkezési párbeszédpanel][04]
   
    Ha több Azure-fiókra, néhány, a program a bejelentkezési folyamat során a előfordulhat, hogy akkor is, ha úgy tűnik, mintha azonos egynél többször látható. Ha ez történik, folytassa a bejelentkezési utasításokat.
4. Miután sikeresen bejelentkezett az Azure-fiókjával, a a **előfizetések kezelése oldalt** párbeszédpanelen jelenik meg a hitelesítő adatok társított előfizetések listáját. Ha több előfizetéssel a listában, és ezek csak egy adott részének dolgozni szeretne, akkor előfordulhat, hogy nem kötelező, törölje a jelet meg a használni kívánt. Miután kiválasztotta az előfizetések, kattintson a **Bezárás**.
   
    ![Kezelése előfizetések párbeszédpanel][05]
5. Ha a **központi telepítése az Azure Web App tárolóhoz** párbeszédpanel jelenik meg, jeleníti meg, hogy a korábban létrehozott webalkalmazás tárolókkal; Ha nem hozott létre tárolót, a lista üres lesz.
   
    ![Azure Web App tároló párbeszédpanel telepítése][06]
6. Ha nem hozott létre egy Azure webes alkalmazás tároló előtt, vagy ha azt szeretné, az alkalmazás egy új tároló közzétételére, kövesse az alábbi lépéseket. Egyéb esetben válassza ki a meglévő Web App tároló, és ugorjon a 7 alatt.
   
   1. Kattintson a **új...**
      
       ![Azure Web App tároló párbeszédpanel telepítése][15]
   2. A **új webes alkalmazás tároló** párbeszédpanel fog megjelenni:
      
       ![Új webes alkalmazás tároló párbeszédpanel][07a]
   3. Adjon meg egy **DNS-címke** a webes alkalmazás tároló; ez alkotó a levél DNS-címke a gazdagépre URL-címének a webalkalmazás az Azure-ban. (Vegye figyelembe, hogy a név kell elérhetőnek lennie Azure Web Apps követelmények elnevezési felelnek meg.)
   4. Az a **webes tároló** legördülő menüben válassza ki a megfelelő szoftver az alkalmazáshoz.
      
       Jelenleg Tomcat 8 Tomcat 7 vagy Jetty 9 közül választhat. A kiválasztott szoftverfrissítés legutóbbi terjesztési Azure biztosítja, és akkor fog futni, a legutóbbi terjesztésipont JDK 8 Oracle által létrehozott és az Azure által biztosított.
   5. Az a **előfizetés** legördülő menüben válassza ki az ehhez a központi telepítéshez használni kívánt előfizetést.
   6. Az a **erőforráscsoport** legördülő menüben válasszon, amelyhez a webalkalmazás hozzárendelni kívánt erőforráscsoportot. (Az azure erőforráscsoportok lehetővé teszik kapcsolódó erőforrások egy csoportba, így például, hogy törölheti együtt.)
      
       Válasszon ki egy meglévő erőforráscsoportot (ha vannak ilyenek) és a skip. lépés: az alábbi g, vagy használja az alábbi lépéseket egy új erőforráscsoport létrehozásához a következő:
      
      * Kattintson a **új...**
      * A **új erőforráscsoport** párbeszédpanel fog megjelenni:
        
          ![Új erőforráscsoport párbeszédpanel][08]
      * Az a a **neve** szövegmező, adja meg az új erőforráscsoport nevét.
      * Az a a **régió** legördülő menüben válassza a megfelelő Azure adatközpont-erőforrásnak a helyét.
      * Választható lehetőség: Alapértelmezés szerint Java 8 legutóbbi terjesztési telepítve lesz az Azure automatikusan a webes alkalmazás tároló, a JVM-et. Azonban is megadhat egy másik verzió és elosztását illetően a JVM-et, ha a webalkalmazás írja elő. A webalkalmazás a JDK megadásához kattintson a **JDK** lapot, és jelölje be az alábbi lehetőségek közül:
        
        * **Az alapértelmezett Azure Web Apps szolgáltatás által kínált JDK telepítése**: Ez a beállítás telepíti a Java 8 legutóbbi eloszlásáról.
        * **A 3. fél elérhető Azure JDK telepítése**: Ez a beállítás lehetővé teszi a Microsoft Azure által biztosított JDKs listájából válasszon ki.
        * **A letöltési helyről saját JDK telepítése**: Ezzel a beállítással megadhatja a saját JDK terjesztési, amely kell csomagolni csomagot .zip fájlként, és feltölti a nyilvánosan elérhető letöltési hely vagy egy Azure storage-fiókot, amely rendelkezik a hozzáférés.
          
          ![Új webes alkalmazás tároló párbeszédpanel][07b]
   7. Kattintson az **OK** gombra.
   8. A **App Service-csomag** legördülő menü mutatja az erőforráscsoport kiválasztott társított app service-csomagokról. (App Service-csomagokról a webalkalmazás, az árképzési szint és a számítási példányméretének például a hely információkat adják meg. Egy egyetlen App Service-csomag használható több Web Apps, ezért azt egy adott webalkalmazás telepítésből külön megmarad.)
      
       Válasszon ki egy meglévő App Service-csomag (ha vannak ilyenek) és skip h az alábbi lépések, vagy használja az alábbi lépéseket egy új App Service-csomag létrehozása a következő:
      
      * Kattintson a **új...**
      * A **új App Service-csomag** párbeszédpanel fog megjelenni:
        
          ![Új App Service-csomag párbeszédpanel][09]
      * Az a a **neve** szövegmező, adjon meg egy nevet az új App Service-csomag.
      * Az a a **hely** legördülő menüben válassza a megfelelő Azure adatközpont helye a terv.
      * Az a a **Tarifacsomagot** legördülő menüben válassza ki a megfelelő árképzési séma. Tesztelési célokra választhat **szabad**.
      * Az a a **Példányméretének** legördülő menüben válassza a terv méretezés a megfelelő példányt. Tesztelési célokra választhat **kis**.
   9. A fenti lépés befejeződött, a webes alkalmazás új tároló párbeszédpanelen a következő ábra kell hasonlítania:
      
       ![Új webes alkalmazás tároló párbeszédpanel][10]
   10. Kattintson a **OK** az új webalkalmazáshoz tároló létrehozásának befejezéséhez.
       
        Várjon néhány másodpercet, frissíteni kell a webalkalmazás-tárolók listáját, és az újonnan létrehozott webes alkalmazás tároló most már ki a listában.
7. Most már készen áll az első Azure webalkalmazás üzembe befejezéséhez:
   
    ![Azure Web App tároló párbeszédpanel telepítése][11]
   
    Kattintson a **OK** a kijelölt webalkalmazás tárolóhoz a Java-alkalmazás központi telepítése.
   
    Alapértelmezés szerint az alkalmazás telepítve, a kiszolgáló alkönyvtára lehet. Ha azt szeretné, hogy a legfelső szintű alkalmazás telepíthető központilag, ellenőrizze a **legfelső szintű telepítés** való kattintás előtt jelölőnégyzet **OK**.
8. Ezt követően kell megjelennie a **Azure tevékenységnapló** nézet, ami a webalkalmazás központi telepítési állapotát jelzi.
   
    ![Azure tevékenységnapló][12]
   
    A webalkalmazás az Azure-bA üzembe helyezésének folyamata gyorsabban csak néhány másodpercig. Ha alkalmazás készen áll, megjelenik egy hivatkozás nevű **közzétett** a a **állapot** oszlop. Amikor a hivatkozásra kattint, a telepített webalkalmazás kezdőlapra eltarthat meg.

## <a name="updating-your-web-app"></a>Webalkalmazás frissítése
Frissítése egy meglévő Azure-webalkalmazás fut egy gyors és egyszerű eljárás, és frissítési lehetőségei vannak:

* Java-webalkalmazás telepítése frissítheti.
* Egy webes alkalmazás tárolóhoz további Java-alkalmazást is közzétehet.

Mindkét esetben a folyamat megegyezik, és csupán néhány másodpercet vesz igénybe:

1. Az Eclipse project explorer kattintson a jobb gombbal a Java-alkalmazást szeretne frissíteni, vagy meglévő Web App tároló hozzáadása.
2. Ha a helyi menü megjelenik, válassza ki a **Azure** , majd **Azure Web Apps közzététel...**
3. Már már bejelentkezett korábban, mivel látni fogja a meglévő Web App tárolók listája. Jelölje ki a közzétenni vagy újra közzé a Java-alkalmazást, és kattintson a kívánt **OK**.

Néhány másodperc múlva, a **Azure tevékenységnapló** nézetben megjelenik, a frissített telepítési **közzétett** és nem fogja tudni ellenőrizni a frissített alkalmazást egy webböngészőben.

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a>Indítása, leállítása vagy egy már meglévő webalkalmazás újraindítása
Indítsa el, vagy állítsa le a meglévő Azure Web Apps tároló, (beleértve az összes telepített Java-alkalmazások azt), használja a **Azure Explorer** nézet.

Ha a **Azure Explorer** nézet még nincs megnyitva, majd kattintva megnyithatja azt **ablak** menü az eclipse-ben, majd kattintson a **nézet megjelenítése**, majd **más...** , majd **Azure**, és kattintson a **Azure Explorer**. Ha korábban nem jelentkezett be, akkor arra fogja kérni teszi.

Ha a **Azure Explorer** nézet jelenik meg, használjon kövesse az alábbi lépéseket indítása vagy leállítása a webalkalmazás: 

1. Bontsa ki a **Azure** csomópont.
2. Bontsa ki a **webalkalmazások** csomópont. 
3. Kattintson a jobb gombbal a kívánt webalkalmazáshoz.
4. Amikor a helyi menü megjelenik, kattintson a **Start**, **leállítása**, vagy **indítsa újra a**. Vegye figyelembe, hogy a menüjéből választások környezet használatát, így csak a futó webalkalmazás leállítása vagy egy webalkalmazást, ami jelenleg nem fut. Indítsa el.
   
    ![Egy már meglévő webalkalmazás leállítása][13]

## <a name="next-steps"></a>Következő lépések
A Java IDE környezetekhez készült Azure-eszközkészlettel kapcsolatos további információkért lásd az alábbi hivatkozásokat:

* [Eclipse-hez készült Azure-eszközkészlet]
  * [az eclipse-ben az Azure eszközkészlet telepítésével]
  * *Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben (Ez a cikk)*
  * [Az Eclipse-hez készült Azure-eszközkészlet újdonságai]
* [Az IntelliJ-hez készült Azure-eszközkészlet]
  * [Az IntelliJ-hez készült Azure-eszközkészlet telepítése]
  * [Hello World webalkalmazás létrehozása az intellij-t az Azure]
  * [Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]

Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ].

Azure Web Apps létrehozásával kapcsolatos további információkért lásd: a [webes alkalmazások – áttekintés].

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Eclipse-hez készült Azure-eszközkészlet]: ../azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ../azure-toolkit-for-intellij.md
[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web-intellij-create-hello-world-web-app.md
[az eclipse-ben az Azure eszközkészlet telepítésével]: ../azure-toolkit-for-eclipse-installation.md
[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]: ../azure-toolkit-for-intellij-installation.md
[Az Eclipse-hez készült Azure-eszközkészlet újdonságai]: ../azure-toolkit-for-eclipse-whats-new.md
[Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/
[webes alkalmazások – áttekintés]: ./app-service-web-overview.md
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[01]: ./media/app-service-web-eclipse-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-eclipse-create-hello-world-web-app/02-Dynamic-Web-Project.png
[03]: ./media/app-service-web-eclipse-create-hello-world-web-app/03-Context-Menu.png
[04]: ./media/app-service-web-eclipse-create-hello-world-web-app/04-Log-In-Dialog.png
[05]: ./media/app-service-web-eclipse-create-hello-world-web-app/05-Manage-Subscriptions-Dialog.png
[06]: ./media/app-service-web-eclipse-create-hello-world-web-app/06-Deploy-To-Azure-Web-Container.png
[07a]: ./media/app-service-web-eclipse-create-hello-world-web-app/07a-New-Web-App-Container-Dialog.png
[07b]: ./media/app-service-web-eclipse-create-hello-world-web-app/07b-New-Web-App-Container-Dialog.png
[08]: ./media/app-service-web-eclipse-create-hello-world-web-app/08-New-Resource-Group-Dialog.png
[09]: ./media/app-service-web-eclipse-create-hello-world-web-app/09-New-Service-Plan-Dialog.png
[10]: ./media/app-service-web-eclipse-create-hello-world-web-app/10-Completed-Web-App-Container-Dialog.png
[11]: ./media/app-service-web-eclipse-create-hello-world-web-app/11-Completed-Deploy-Dialog.png
[12]: ./media/app-service-web-eclipse-create-hello-world-web-app/12-Activity-Log-View.png
[13]: ./media/app-service-web-eclipse-create-hello-world-web-app/13-Azure-Explorer-Web-App.png
[14]: ./media/app-service-web-eclipse-create-hello-world-web-app/14-publishDropdownButton.png
[15]: ./media/app-service-web-eclipse-create-hello-world-web-app/15-New-Azure-Web-Container.png
