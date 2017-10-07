---
title: "egy alapszintű Azure-webalkalmazásban az IntelliJ aaaCreate |} Microsoft Docs"
description: "Az oktatóanyag bemutatja, hogyan toouse hello Azure eszközkészlet IntelliJ toocreate egy Hello World az Azure-webalkalmazást."
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
ms.openlocfilehash: 4667497213cac3ddf754d164e614c809f338cce5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-basic-azure-web-app-in-intellij"></a>Egy alapszintű Azure-webalkalmazás létrehozása az intellij-t
Ez az oktatóanyag bemutatja, hogyan toocreate és központi telepítése egy alapszintű Hello World alkalmazás tooAzure webalkalmazásként hello segítségével [IntelliJ Azure eszköztára]. Egy alapszintű JSP példa az egyszerűség, de hasonló lépésekkel akkor lehet megfelelő, egy Java servlet, az Azure-telepítés illetően.

Ez az oktatóanyag befejezése után, az alkalmazás a következő ábrán egy webböngészőben megtekintésekor hasonló toohello fog kinézni:

![A minta-weblap][01]

## <a name="prerequisites"></a>Előfeltételek
* A Java fejlesztői készlet (JDK), v 1,8 vagy újabb.
* Az IntelliJ IDEA Ultimate Edition. Ez letölthető <https://www.jetbrains.com/idea/download/index.html>.
* A Java-alapú webkiszolgáló vagy a kiszolgáló, terjesztési például [Apache Tomcat] vagy [Jetty].
* Azure-előfizetéssel, amely szerezhető: a <https://azure.microsoft.com/free/> vagy <http://azure.microsoft.com/pricing/purchase-options/>.
* Hello [IntelliJ Azure eszköztára]. Hello Azure eszközkészlet telepítésével kapcsolatos információkért lásd: [telepítése hello Azure eszköztára IntelliJ].

## <a name="toocreate-a-hello-world-application"></a>toocreate egy Hello World alkalmazásról
Először először foglalkozunk létrehoz egy Java-projektet.

1. Indítsa el az intellij-t, majd kattintson a hello **fájl** menü, majd kattintson a **új**, és kattintson a **projekt**.
   
    ![Fájl új projekt][02]
2. Hello új projekt párbeszédpanel, válassza ki a **Java**, majd **webalkalmazás**, és kattintson a **új** tooadd egy projekt SDK.
   
    ![Új projekt párbeszédpanel][03a]
   
3. A Kezdőkönyvtár kiválasztása hello JDK párbeszédpanel megnyitásához, válassza ki hello a JDK telepítési mappáját, és kattintson **OK**. Kattintson a **tovább** a hello új projekt párbeszédpanel bezárásához toocontinue.
   
    ![Adja meg a JDK kezdőkönyvtára][03b]
4. Ez az oktatóanyag céljából, nevet hello projektnek **Java-webalkalmazás-alkalmazás-a-Azure**, és kattintson a **Befejezés**.
   
    ![Új projekt párbeszédpanel][04]
5. Az IntelliJ a Project Explorer nézet, bontsa ki a **Java-webalkalmazás-alkalmazás-a-Azure**, majd bontsa ki a **webes**, majd kattintson duplán **index.jsp**.
   
    ![Nyissa meg az Index lap][05c]
6. Az index.jsp fájl megnyitásakor az IntelliJ beépülő modul toodynamically megjelenített szöveg **Hello World!** hello meglévő belül `<body>` elemet. A frissített `<body>` tartalom a következő példa hello kell hasonlítania:
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
7. Mentse az index.jsp.

## <a name="toodeploy-your-application-tooan-azure-web-app-container"></a>toodeploy az alkalmazás tooan Azure Web App tároló
Számos módon egy Java webes alkalmazás tooAzure-telepíthet. Ez az oktatóanyag leírja az egyik legegyszerűbb hello: az alkalmazás lesz telepített tooan Azure Web App tároló - semmilyen különleges projekttípus sem további eszközök van szükség. hello JDK és hello webes tároló szoftver biztosítja az Ön Azure-ra, így nincs szükség tooupload a saját; szüksége a Java-webalkalmazás. Ennek eredményeképpen hello közzétételi alkalmazás vesz igénybe másodperc, nem perc.

Az alkalmazás közzététele előtt először tooconfigure a modul beállításait. toodo Igen, használja a lépéseket követve hello:

1. Az intellij-t a Project Explorer, kattintson a jobb gombbal hello **Java-webalkalmazás-alkalmazás-a-Azure** projekt. Amikor hello helyi menü megjelenik, kattintson a **beállítások modul megnyitása**.

    ![Nyissa meg a modul beállításai][05a]
2. Hello szerkezetének párbeszédpanel megjelenésekor:

   a. Kattintson a **összetevők** hello listájában **Projektbeállítások**.
   b. Módosítás hello összetevő neve, a hello **neve** jelölőnégyzetet, hogy nem tartalmaz szóközt vagy speciális karaktereket; Ez azért szükséges, mivel az egységes erőforrás-azonosító (URI) hello hello neve lesz.
   c. Változás hello **típus** túl**webalkalmazás: archív**.
   d. Kattintson a **OK** tooclose hello szerkezetének párbeszédpanel megnyitásához.

    ![Nyissa meg a modul beállításai][05b]

Ha konfigurálta a modul beállításait, közzéteheti a alkalmazás tooAzure hello lépések segítségével:

1. Az intellij-t a Project Explorer, kattintson a jobb gombbal hello **Java-webalkalmazás-alkalmazás-a-Azure** projekt. Amikor hello helyi menü megjelenik, válassza ki a **Azure**, és kattintson a **Azure Web Apps közzététel...**
   
    ![Azure közzététel helyi menü][06]
2. Ha már nem regisztrált az Azure az intellij-t, rákérdezéses toosign fogja be Azure-fiókjába. (Még több Azure-fiókra, néhány hello kér hello bejelentkezési folyamat során előfordulhat, hogy megjelenik-e egynél többször, még akkor is, ha úgy tűnik, mintha toobe hello azonos. Ha ez történik, továbbra is toofollow hello bejelentkezési utasításokban.)
   
    ![Az Azure bejelentkezési párbeszédpanel][07]
3. Miután sikeresen bejelentkezett az Azure-fiókjával a, hello **előfizetések kezelése oldalt** párbeszédpanelen jelenik meg a hitelesítő adatok társított előfizetések listáját. (Ha szerepel a listában több előfizetése van, és azt szeretné, hogy csak egy adott részével őket toowork, akkor előfordulhat, hogy nem kötelező, törölje a jelet hello előfizetések nem szeretné, hogy toouse.) Miután kiválasztotta az előfizetések, kattintson a **Bezárás**.
   
    ![Előfizetések kezelése][08]
4. Ha hello **központi telepítése a webes alkalmazás tároló tooAzure** párbeszédpanel jelenik meg, jeleníti meg, hogy a korábban létrehozott webalkalmazás tárolókkal; Ha nem hozott létre tárolót, hello listája üres lesz.
   
    ![Alkalmazás tárolók][09]
5. Ha még nem hozott egy Azure webes alkalmazás tároló előtt, vagy ha azt szeretné, hogy toopublish az alkalmazás tooa új tároló, használja a lépéseket követve hello. Ellenkező esetben válassza ki a meglévő Web App tároló, és hagyja ki az alábbi 6 toostep.
   
   1. Kattintson a**+**
      
       ![Alkalmazás-tároló hozzáadása][10]
   2. Hello **új webes alkalmazás tároló** párbeszédpanel jelenik meg, amely hello használt mellett számos lépést.
      
       ![Új alkalmazás-tároló][11a]
   3. Adjon meg egy **DNS-címke** a webes alkalmazás tároló; ez alkotó hello levél DNS-címke hello gazdagépre URL-címet a webalkalmazás az Azure-ban. Megjegyzés: hello neve kell érhető el, és felel meg a tooAzure webalkalmazás-kiosztási követelményeinek.
   4. A hello **webes tároló** legördülő menüre, válassza hello megfelelő szoftvert az alkalmazáshoz.
      
       Jelenleg Tomcat 8 Tomcat 7 vagy Jetty 9 közül választhat. Kijelölt hello szoftver legújabb terjesztési Azure biztosítja, és akkor fog futni, a legutóbbi terjesztésipont JDK 8 Oracle által létrehozott és az Azure által biztosított.
   5. A hello **előfizetés** legördülő menüre, válassza hello előfizetés toouse ehhez a központi telepítéshez használni szeretne.
   6. A hello **erőforráscsoport** legördülő menüben válassza hello erőforráscsoportot, amelyhez tooassociate a webalkalmazás. (Az azure erőforráscsoport-sablonok engedélyezi, akkor toogroup kapcsolódó erőforrások együtt, így például, hogy törölheti együtt.)
      
       Egy meglévő erőforráscsoportot kiválaszthatja (ha vannak ilyenek), és a skip toostep g az alábbi, vagy használjon hello következő lépések toocreate új erőforráscsoport:
      
      * Válassza ki  **&lt; &lt; hozzon létre új erőforráscsoportot &gt; &gt;**  a hello **erőforráscsoport** legördülő menü.
      * Hello **új erőforráscsoport** párbeszédpanel fog megjelenni:
        
          ![Új erőforráscsoport][12]
      * A hello hello **neve** szövegmező, adja meg az új erőforráscsoport nevét.
      * A hello hello **régió** legördülő menüben válassza hello megfelelő Azure adatközpont-erőforrásnak a helyét.
      * Kattintson az **OK** gombra.
   7. Hello **App Service-csomag** legördülő menü hello app service-csomagokról hello erőforráscsoport kiválasztott társított sorolja fel. (A következő információkat, például a Web App alkalmazásban tarifacsomag és hello számítási példányméretének hello hello helyét egy App Service-csomag határozza meg. Egy egyetlen App Service-csomag használható több Web Apps, ezért azt egy adott webalkalmazás telepítésből külön megmarad.)
      
       Kiválaszthat egy meglévő App Service-csomag (ha vannak ilyenek), és hagyja ki az alábbi toostep h, vagy használja a következő lépéseket toocreate egy új App Service-csomag hello:
      
      * Válassza ki  **&lt; &lt; új App Service-csomag létrehozása &gt; &gt;**  a hello **App Service-csomag** legördülő menü.
      * Hello **új App Service-csomag** párbeszédpanel fog megjelenni:
        
          ![Új App Service-csomag][13]
      * A hello hello **neve** szövegmező, adjon meg egy nevet az új App Service-csomag.
      * A hello hello **hely** legördülő menüben válassza hello megfelelő Azure adatközpont hello terv helyét.
      * A hello hello **Tarifacsomagot** legördülő menüben válassza hello megfelelő hello terv árképzési. Tesztelési célokra választhat **szabad**.
      * A hello hello **Példányméretének** legördülő menüre, válassza hello megfelelő példányméretének hello terv. Tesztelési célokra választhat **kis**.
      * Kattintson az **OK** gombra.
   8. (Választható) Alapértelmezés szerint 8 Java legutóbbi terjesztési automatikusan telepíti a JVM-et, az Azure tooyour webes alkalmazás tároló. Választhatja azonban egy másik verzió és elosztását illetően hello JVM-et. toodo Igen, használja a lépéseket követve hello:
      
      * Kattintson a hello **JDK** hello lapján **új webes alkalmazás tároló** párbeszédpanel megnyitásához.
      * Hello alábbi beállítások közül választhat:
        
        * Hello alapértelmezett JDK, amely az Azure által kínált telepítése
        * 3. fél JDK Azure rendelkezésre álló további JDKs legördülő listájából telepítése
        * Központi telepítése egy egyéni JDK, és be kell csomagolni, és vagy egy ZIP-fájl nyilvánosan elérhető vagy az Azure storage-fiók
        
        ![Új alkalmazás tároló JDK lapon][11b]
   9. Az összes fenti lépéseket hello befejeződött, hello új webes alkalmazás tároló párbeszédpanelen a következő ábra hello kell hasonlítania:
      
       ![Új alkalmazás-tároló][14]
   10. Kattintson a **OK** toocomplete hello létrehozása az új webalkalmazáshoz tároló.
       
        Várjon néhány másodpercet hello webalkalmazás tárolók toobe hello listája frissítve, és az újonnan létrehozott webes alkalmazás tároló kijelöli hello listában.
6. Most már áll készen toocomplete hello kezdeti telepítése a webes alkalmazás tooAzure; Kattintson a **OK** toodeploy a Java-alkalmazás toohello kijelölt webalkalmazás tároló. Alapértelmezés szerint az alkalmazás telepítése, hello alkalmazáskiszolgáló alkönyvtára. Ha azt szeretné, központilag telepített hello gyökéralkalmazás toobe, ellenőrizze a hello **tooroot telepítése** való kattintás előtt jelölőnégyzet **OK**.
   
    ![TooAzure telepítése][15]
7. Ezt követően megtekintheti az hello **Azure tevékenységnapló** nézetet, amely a webalkalmazás hello központi telepítésének állapotát jelzi.
   
    ![Folyamatjelző][16]
   
    a webalkalmazás tooAzure telepítési folyamata hello gyorsabban csupán néhány másodpercet toocomplete. Ha alkalmazás készen áll, megjelenik egy hivatkozás nevű **közzétett** a hello **állapot** oszlop. Hello hivatkozásra telepített tooyour Web App kezdőlapján fog tartani, vagy a következő szakasz toobrowse tooyour webalkalmazás hello hello lépéseket használhatja.

## <a name="browsing-tooyour-web-app-on-azure"></a>Az Azure Web App tooyour tallózása
toobrowse tooyour Azure Web App, hello használható **Azure Explorer** nézet.

Ha hello **Azure Explorer** nézet még nincs megnyitva, majd kattintva megnyithatja azt **nézet** menü az intellij-t, majd kattintson a **eszköz Windows**, és kattintson a  **Szolgáltatás Explorer**. Ha korábban nem jelentkezett be, amely felszólítja toodo stb.

Ha hello **Azure Explorer** nézet jelenik meg, használja, hajtsa végre a lépéseket toobrowse tooyour Web App: 

1. Bontsa ki a hello **Azure** csomópont.
2. Bontsa ki a hello **webalkalmazások** csomópont. 
3. Kattintson a jobb gombbal hello kívánt webalkalmazáshoz.
4. Amikor hello helyi menü megjelenik, kattintson a **nyissa meg böngészőben**.
   
    ![Keresse meg a webes alkalmazás][17]

## <a name="updating-your-web-app"></a>Webalkalmazás frissítése
Frissítése egy meglévő Azure-webalkalmazás fut egy gyors és egyszerű eljárás, és frissítési lehetőségei vannak:

* Java-webalkalmazás telepítése hello frissítheti.
* Egy további Java-alkalmazás toohello közzététele Web App tárolóhoz.

Mindkét esetben hello folyamata azonos, és csupán néhány másodpercet vesz igénybe:

1. Hello IntelliJ project Explorer kattintson a jobb gombbal hello Java-alkalmazás hozzáadása a webes alkalmazás tároló meglévő tooan vagy tooupdate szeretné.
2. Amikor hello helyi menü megjelenik, válassza ki a **Azure** , majd **Azure Web Apps közzététel...**
3. Már már bejelentkezett korábban, mivel látni fogja a meglévő Web App tárolók listája. Válasszon egyet a Java-alkalmazás tooand kattintson újra közzé vagy toopublish kívánt hello **OK**.

Néhány másodperc múlva, hello **Azure tevékenységnapló** nézetben megjelenik, a frissített telepítési **közzétett** , és a frissített alkalmazást egy webböngészőben lesz képes tooverify.

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a>Indítása, leállítása vagy egy már meglévő webalkalmazás újraindítása
toostart vagy állítsa le a meglévő Azure Web Apps tároló, (beleértve az összes telepített hello Java-alkalmazások azt), használhatja a hello **Azure Explorer** nézet.

Ha hello **Azure Explorer** nézet még nincs megnyitva, majd kattintva megnyithatja azt **nézet** menü az intellij-t, majd kattintson a **eszköz Windows**, és kattintson a  **Szolgáltatás Explorer**. Ha korábban nem jelentkezett be, amely felszólítja toodo stb.

Ha hello **Azure Explorer** nézet jelenik meg, kövesse az alábbi lépéseket toostart használatát, vagy a webalkalmazás leállítása: 

1. Bontsa ki a hello **Azure** csomópont.
2. Bontsa ki a hello **webalkalmazások** csomópont. 
3. Kattintson a jobb gombbal hello kívánt webalkalmazáshoz.
4. Amikor hello helyi menü megjelenik, kattintson a **Start**, **leállítása**, vagy **indítsa újra a**. Ne feledje, hogy hello menü döntések környezet használatát, így csak a futó webalkalmazás leállítása vagy egy webalkalmazást, ami jelenleg nem fut. Indítsa el.
   
    ![A webalkalmazás leállítása][18]

## <a name="next-steps"></a>Következő lépések
A Java IDEs hello Azure eszközök gazdag kapcsolatos további információkért tekintse meg a következő hivatkozások hello:

* [Eclipse Azure eszköztára]
  * [Hello Azure eszköztára Eclipse telepítése]
  * [Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]
  * [Újdonságok az Eclipse Azure eszköztára hello]
* [IntelliJ Azure eszköztára]
  * [telepítése hello Azure eszköztára IntelliJ]
  * *Hello World webalkalmazás létrehozása az Azure-hoz az intellij-t (Ez a cikk)*
  * [Újdonságok az intellij-t Azure eszköztára hello]

<a name="see-also"></a>

## <a name="see-also"></a>Lásd még:
Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból].

Azure Web Apps létrehozásával kapcsolatos további információkért lásd: hello [webes alkalmazások – áttekintés].

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Eclipse Azure eszköztára]: ../azure-toolkit-for-eclipse.md
[IntelliJ Azure eszköztára]: ../azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Hello Azure eszköztára Eclipse telepítése]: ../azure-toolkit-for-eclipse-installation.md
[telepítése hello Azure eszköztára IntelliJ]: ../azure-toolkit-for-intellij-installation.md
[Újdonságok az Eclipse Azure eszköztára hello]: ../azure-toolkit-for-eclipse-whats-new.md
[Újdonságok az intellij-t Azure eszköztára hello]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
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
