---
title: "Alapszintű Azure web app használatával aaaCreate Holdas |} Microsoft Docs"
description: "Az oktatóanyag bemutatja, hogyan toouse hello Azure eszközkészlet Eclipse toocreate egy Hello World az Azure-webalkalmazást."
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
ms.openlocfilehash: b2f42e0e7a5b98760ec02fab2fc38f9f07b1156b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-basic-azure-web-app-using-eclipse"></a>Alapszintű Azure-webalkalmazás létrehozása használata az eclipse-ben
Ez az oktatóanyag bemutatja, hogyan toocreate és központi telepítése egy alapszintű Hello World alkalmazás tooAzure webalkalmazásként hello segítségével [Eclipse Azure eszköztára]. Egy alapszintű JSP példa az egyszerűség, de hasonló lépésekkel akkor lehet megfelelő, egy Java servlet, az Azure-telepítés illetően.

Ez az oktatóanyag befejezése után, az alkalmazás a következő ábrán egy webböngészőben megtekintésekor hasonló toohello fog kinézni:

![Előzetes verziójú Hello World alkalmazás][01]

## <a name="prerequisites"></a>Előfeltételek
* A Java fejlesztői készlet (JDK), v 1,8 vagy újabb.
* Java EE-fejlesztőknek, Luna IDE Holdas vagy újabb. Ez letölthető <http://www.eclipse.org/downloads/>.
* A Java-alapú webkiszolgáló vagy a kiszolgáló, terjesztési például [Apache Tomcat] vagy [Jetty].
* Azure-előfizetéssel, amely szerezhető: a <https://azure.microsoft.com/free/> vagy <http://azure.microsoft.com/pricing/purchase-options/>.
* Hello [Eclipse Azure eszköztára]. Hello Azure eszközkészlet telepítésével kapcsolatos információkért lásd: [telepítése hello Azure eszköztára Eclipse].

## <a name="toocreate-a-hello-world-application"></a>toocreate egy Hello World alkalmazásról
Először először foglalkozunk létrehoz egy Java-projektet.

1. Indítsa el az eclipse-ben, és hello menüben kattintson **fájl**, kattintson a **új**, és kattintson a **dinamikus webes projekt**. (Ha nem lát **dinamikus webes projekt** kattintás után az elérhető projektek tulajdonosaként **fájl** és **új**, majd hello a következő: kattintson a **fájl**, kattintson a **új**, kattintson a **projekt...** , bontsa ki a **webes**, kattintson a **dinamikus webes projekt**, és kattintson a **következő**.)
2. Ez az oktatóanyag céljából, nevet hello projektnek **MyWebApp**. A képernyő jelenik meg hasonló toohello következő:
   
    ![Új dinamikus webes projekt létrehozása][02]
3. Kattintson a **Befejezés** gombra.
4. A Eclipse Project Explorer nézet, bontsa ki a **MyWebApp**. Kattintson a jobb gombbal a **WebContent** (Webes tartalom), majd a **New** (Új) elemre, és végül a **JSP File** (JSP-fájl) elemre.
5. A hello **új JSP-fájl** párbeszédpanelen nevű hello fájl **index.jsp**, hello fölérendelt mappája, tartsa **MyWebApp/WebContent**, és kattintson a **tovább**.
6. A hello **JSP-sablon kiválasztása** párbeszédpanel, az oktatóanyag válasszon alkalmazásában **új JSP-fájl (html)**, és kattintson a **Befejezés**.
7. Megnyitásakor az index.jsp fájl az eclipse-ben, adja hozzá a szöveg toodynamically megjelenítési **Hello World!** hello meglévő belül `<body>` elemet. A frissített `<body>` tartalom a következő példa hello kell hasonlítania:
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
8. Mentse az index.jsp.

## <a name="toodeploy-your-application-tooan-azure-web-app-container"></a>toodeploy az alkalmazás tooan Azure Web App tároló
Számos módon egy Java webes alkalmazás tooAzure-telepíthet. Ez az oktatóanyag leírja az egyik legegyszerűbb hello: az alkalmazás lesz telepített tooan Azure Web App tároló - semmilyen különleges projekttípus sem további eszközök van szükség. hello JDK és hello webes tároló szoftver biztosítja az Ön Azure-ra, így nincs szükség tooupload a saját; szüksége a Java-webalkalmazás. Ennek eredményeképpen hello közzétételi alkalmazás vesz igénybe másodperc, nem perc.

1. A Project Explorer Eclipse meg, kattintson a jobb gombbal **MyWebApp**.
2. Hello helyi menüben válasszon ki **Azure**, majd kattintson a **Azure Web Apps közzététel...**
   
    ![Az Azure webes alkalmazás közzététele][03]
   
    Azt is megteheti, amíg a webes projektet a Project Explorer hello van jelölve, kattintson hello **közzététel** a hello eszköztár, és válassza a legördülő gomb **Azure Web Apps közzététel** innen:
   
    ![Az Azure webes alkalmazás közzététele][14]
3. Ha már nem regisztrált az Azure az eclipse-ben, a Azure fiókjába fogja felszólító toosign:
   
    ![Az Azure bejelentkezési párbeszédpanel][04]
   
    Több Azure fiók, van néhány hello kér során hello jelentkezzen be a folyamat előfordulhat, hogy megjelenik-e egynél többször, még akkor is, ha úgy tűnik, mintha toobe hello azonos. Ha ez történik, továbbra is a következő utasításokat hello bejelentkezés.
4. Miután sikeresen bejelentkezett az Azure-fiókjával a, hello **előfizetések kezelése oldalt** párbeszédpanelen jelenik meg a hitelesítő adatok társított előfizetések listáját. Ha több felsorolt előfizetések tartoznak, és azt szeretné, hogy az egyik csak egy adott részének toowork, a hello meg a kívánt toouse szükség lehet, hogy jelet. Miután kiválasztotta az előfizetések, kattintson a **Bezárás**.
   
    ![Kezelése előfizetések párbeszédpanel][05]
5. Ha hello **központi telepítése a webes alkalmazás tároló tooAzure** párbeszédpanel jelenik meg, jeleníti meg, hogy a korábban létrehozott webalkalmazás tárolókkal; Ha nem hozott létre tárolót, hello listája üres lesz.
   
    ![Webes alkalmazás tároló párbeszédpanel tooAzure telepítése][06]
6. Ha még nem hozott egy Azure webes alkalmazás tároló előtt, vagy ha azt szeretné, hogy toopublish az alkalmazás tooa új tároló, használja a lépéseket követve hello. Ellenkező esetben válassza ki a meglévő Web App tároló, és hagyja ki az alábbi 7 toostep.
   
   1. Kattintson a **új...**
      
       ![Webes alkalmazás tároló párbeszédpanel tooAzure telepítése][15]
   2. Hello **új webes alkalmazás tároló** párbeszédpanel fog megjelenni:
      
       ![Új webes alkalmazás tároló párbeszédpanel][07a]
   3. Adjon meg egy **DNS-címke** a webes alkalmazás tároló; ez alkotó hello levél DNS-címke hello gazdagépre URL-címet a webalkalmazás az Azure-ban. (Vegye figyelembe hello névvel kell érhető el, és tooAzure webalkalmazás-kiosztási követelményeinek felel meg.)
   4. A hello **webes tároló** legördülő menüre, válassza hello megfelelő szoftvert az alkalmazáshoz.
      
       Jelenleg Tomcat 8 Tomcat 7 vagy Jetty 9 közül választhat. Kijelölt hello szoftver legújabb terjesztési Azure biztosítja, és akkor fog futni, a legutóbbi terjesztésipont JDK 8 Oracle által létrehozott és az Azure által biztosított.
   5. A hello **előfizetés** legördülő menüre, válassza hello előfizetés toouse ehhez a központi telepítéshez használni szeretne.
   6. A hello **erőforráscsoport** legördülő menüben válassza hello erőforráscsoportot, amelyhez tooassociate a webalkalmazás. (Az azure erőforráscsoport-sablonok engedélyezi, akkor toogroup kapcsolódó erőforrások együtt, így például, hogy törölheti együtt.)
      
       (Ha vannak ilyenek) egy meglévő erőforráscsoportot kiválaszthatja, és skip toostep g az alábbi, vagy használjon hello ezeket a következő lépéseket egy új erőforráscsoportot toocreate:
      
      * Kattintson a **új...**
      * Hello **új erőforráscsoport** párbeszédpanel fog megjelenni:
        
          ![Új erőforráscsoport párbeszédpanel][08]
      * A hello hello **neve** szövegmező, adja meg az új erőforráscsoport nevét.
      * A hello hello **régió** legördülő menüben válassza hello megfelelő Azure adatközpont-erőforrásnak a helyét.
      * Választható lehetőség: Alapértelmezés szerint Java 8 legutóbbi eloszlásáról telepíti az Azure automatikusan tooyour web app tárolóban a JVM-et. Azonban is megadhat egy másik verziót és a terjesztési hello JVM-et, ha a webalkalmazás számára szükséges. a webalkalmazás JDK toospecify hello kattintson hello **JDK** fülre, és válassza ki a hello alábbi beállítások egyikét:
        
        * **Hello alapértelmezett Azure Web Apps szolgáltatás által kínált JDK telepítése**: Ez a beállítás telepíti a Java 8 legutóbbi eloszlásáról.
        * **A 3. fél elérhető Azure JDK telepítése**: Ez a beállítás lehetővé teszi toochoose JDKs, amely a Microsoft Azure által biztosított hello listája.
        * **A letöltési helyről saját JDK telepítése**: Ez a beállítás lehetővé teszi toospecify saját JDK terjesztési, amely egy ZIP-fájlba kell csomagolni, és a nyilvánosan elérhető letöltési hely vagy egy Azure storage-fiók, amelynek tooeither feltöltött, rendelkezik hozzáféréssel.
          
          ![Új webes alkalmazás tároló párbeszédpanel][07b]
   7. Kattintson az **OK** gombra.
   8. Hello **App Service-csomag** legördülő menü hello app service-csomagokról hello erőforráscsoport kiválasztott társított sorolja fel. (App Service-csomagokról adja meg például a Web App alkalmazásban tarifacsomag és hello számítási példányméretének hello hello helyét. Egy egyetlen App Service-csomag használható több Web Apps, ezért azt egy adott webalkalmazás telepítésből külön megmarad.)
      
       Kiválaszthat egy meglévő App Service-csomag (ha vannak ilyenek), és hagyja ki az alábbi toostep h, vagy ezek lépések toocreate egy új App Service-csomag a következő hello használata:
      
      * Kattintson a **új...**
      * Hello **új App Service-csomag** párbeszédpanel fog megjelenni:
        
          ![Új App Service-csomag párbeszédpanel][09]
      * A hello hello **neve** szövegmező, adjon meg egy nevet az új App Service-csomag.
      * A hello hello **hely** legördülő menüben válassza hello megfelelő Azure adatközpont hello terv helyét.
      * A hello hello **Tarifacsomagot** legördülő menüben válassza hello megfelelő hello terv árképzési. Tesztelési célokra választhat **szabad**.
      * A hello hello **Példányméretének** legördülő menüre, válassza hello megfelelő példányméretének hello terv. Tesztelési célokra választhat **kis**.
   9. Az összes fenti lépéseket hello befejeződött, hello új webes alkalmazás tároló párbeszédpanelen a következő ábra hello kell hasonlítania:
      
       ![Új webes alkalmazás tároló párbeszédpanel][10]
   10. Kattintson a **OK** toocomplete hello létrehozása az új webalkalmazáshoz tároló.
       
        Várjon néhány másodpercet hello webalkalmazás tárolók toobe hello listája frissítve, és az újonnan létrehozott webes alkalmazás tároló kijelöli hello listában.
7. Most már készen áll a toocomplete hello kezdeti telepítése a webes alkalmazás tooAzure áll:
   
    ![Webes alkalmazás tároló párbeszédpanel tooAzure telepítése][11]
   
    Kattintson a **OK** toodeploy a Java-alkalmazás toohello kijelölt webalkalmazás tároló.
   
    Alapértelmezés szerint az alkalmazás telepítése, hello alkalmazáskiszolgáló alkönyvtára. Ha azt szeretné, központilag telepített hello gyökéralkalmazás toobe, ellenőrizze a hello **tooroot telepítése** való kattintás előtt jelölőnégyzet **OK**.
8. Ezt követően megtekintheti az hello **Azure tevékenységnapló** nézetet, amely a webalkalmazás hello központi telepítésének állapotát jelzi.
   
    ![Azure tevékenységnapló][12]
   
    a webalkalmazás tooAzure telepítési folyamata hello gyorsabban csupán néhány másodpercet toocomplete. Ha alkalmazás készen áll, megjelenik egy hivatkozás nevű **közzétett** a hello **állapot** oszlop. Hello hivatkozásra tart, telepített tooyour webes alkalmazás kezdőlapja.

## <a name="updating-your-web-app"></a>Webalkalmazás frissítése
Frissítése egy meglévő Azure-webalkalmazás fut egy gyors és egyszerű eljárás, és frissítési lehetőségei vannak:

* Java-webalkalmazás telepítése hello frissítheti.
* Egy további Java-alkalmazás toohello közzététele Web App tárolóhoz.

Mindkét esetben hello folyamata azonos, és csupán néhány másodpercet vesz igénybe:

1. Az Eclipse project explorer hello kattintson a jobb gombbal hello Java-alkalmazás hozzáadása a webes alkalmazás tároló meglévő tooan vagy tooupdate szeretné.
2. Amikor hello helyi menü megjelenik, válassza ki a **Azure** , majd **Azure Web Apps közzététel...**
3. Már már bejelentkezett korábban, mivel látni fogja a meglévő Web App tárolók listája. Válasszon egyet a Java-alkalmazás tooand kattintson újra közzé vagy toopublish kívánt hello **OK**.

Néhány másodperc múlva, hello **Azure tevékenységnapló** nézetben megjelenik, a frissített telepítési **közzétett** , és a frissített alkalmazást egy webböngészőben lesz képes tooverify.

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a>Indítása, leállítása vagy egy már meglévő webalkalmazás újraindítása
toostart vagy állítsa le a meglévő Azure Web Apps tároló, (beleértve az összes telepített hello Java-alkalmazások azt), használhatja a hello **Azure Explorer** nézet.

Ha hello **Azure Explorer** nézet még nincs megnyitva, majd kattintva megnyithatja azt **ablak** menü az eclipse-ben, majd kattintson a **nézet megjelenítése**, majd **más...** , majd **Azure**, és kattintson a **Azure Explorer**. Ha korábban nem jelentkezett be, amely felszólítja toodo stb.

Ha hello **Azure Explorer** nézet jelenik meg, kövesse az alábbi lépéseket toostart használatát, vagy a webalkalmazás leállítása: 

1. Bontsa ki a hello **Azure** csomópont.
2. Bontsa ki a hello **webalkalmazások** csomópont. 
3. Kattintson a jobb gombbal hello kívánt webalkalmazáshoz.
4. Amikor hello helyi menü megjelenik, kattintson a **Start**, **leállítása**, vagy **indítsa újra a**. Ne feledje, hogy hello menü döntések környezet használatát, így csak a futó webalkalmazás leállítása vagy egy webalkalmazást, ami jelenleg nem fut. Indítsa el.
   
    ![Egy már meglévő webalkalmazás leállítása][13]

## <a name="next-steps"></a>Következő lépések
A Java IDEs hello Azure eszközök gazdag kapcsolatos további információkért tekintse meg a következő hivatkozások hello:

* [Eclipse Azure eszköztára]
  * [telepítése hello Azure eszköztára Eclipse]
  * *Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben (Ez a cikk)*
  * [Újdonságok az Eclipse Azure eszköztára hello]
* [Az IntelliJ-hez készült Azure-eszközkészlet]
  * [Az IntelliJ hello Azure eszközkészlet telepítése]
  * [Hello World webalkalmazás létrehozása az intellij-t az Azure]
  * [Újdonságok az intellij-t Azure eszköztára hello]

Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból].

Azure Web Apps létrehozásával kapcsolatos további információkért lásd: hello [webes alkalmazások – áttekintés].

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Eclipse Azure eszköztára]: ../azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ../azure-toolkit-for-intellij.md
[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web-intellij-create-hello-world-web-app.md
[telepítése hello Azure eszköztára Eclipse]: ../azure-toolkit-for-eclipse-installation.md
[Az IntelliJ hello Azure eszközkészlet telepítése]: ../azure-toolkit-for-intellij-installation.md
[Újdonságok az Eclipse Azure eszköztára hello]: ../azure-toolkit-for-eclipse-whats-new.md
[Újdonságok az intellij-t Azure eszköztára hello]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
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
