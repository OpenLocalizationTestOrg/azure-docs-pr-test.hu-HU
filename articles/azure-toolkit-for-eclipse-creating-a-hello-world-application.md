---
title: "aaaCreate Hello World Felhőszolgáltatás az Azure-hoz az eclipse-ben"
description: "Ismerje meg, hogyan egy egyszerű Hello World használó toocreate hello Azure eszköztára eclipse-ben."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 7262e705-59d6-43ce-b888-29a21c8e0cb7
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: dfb81374aaf78e933c0bf83a1dbd98023801491a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-hello-world-cloud-service-for-azure-in-eclipse"></a>Az Azure-Hello World felhőalapú szolgáltatás létrehozása az eclipse-ben
hello következő lépések bemutatják, hogyan toocreate és központi telepítése egy alapszintű JSP alkalmazás tooAzure hello Azure eszközkészlet használata az eclipse-ben. A JSP példa az egyszerűség kedvéért azonban nagyon hasonló lépéseket az Azure-telepítés illeti lenne egy Java servlet megfelelő.

hello alkalmazás hasonló toohello következő fog kinézni:

![][ic600360]

## <a name="prerequisites"></a>Előfeltételek
* A Java fejlesztői készlet (JDK), v 1.7 vagy újabb.
* Java EE-fejlesztőknek IDE Holdas Indigo vagy újabb. Ez letölthető <http://www.eclipse.org/downloads/>.
* A Java-alapú webkiszolgáló vagy a kiszolgáló, például az Apache Tomcat, GlassFish, JBoss alkalmazáskiszolgáló, Jetty vagy IBM® WebSphere Liberty® alkalmazás kiszolgálómag terjesztési.
* Azure-előfizetéssel, amely szerezhető: a <http://azure.microsoft.com/pricing/purchase-options/>.
* hello Azure eszköztára eclipse-ben. További információkért lásd: [telepítése hello Azure eszköztára Eclipse][Installing hello Azure Toolkit for Eclipse].

## <a name="toocreate-a-hello-world-application"></a>toocreate egy Hello World alkalmazásról
Először először foglalkozunk létrehoz egy Java-projektet.

1. Indítsa el az eclipse-ben, és hello menüben kattintson **fájl**, kattintson a **új**, és kattintson a **dinamikus webes projekt**. (Ha nem lát **dinamikus webes projekt** kattintás után az elérhető projektek tulajdonosaként **fájl**, **új**, majd hello a következő: kattintson a **fájl**, kattintson a **új**, kattintson a **projekt...** , bontsa ki a **webes**, kattintson a **dinamikus webes projekt**, és kattintson a **következő**.)

1. Ez az oktatóanyag céljából, nevet hello projektnek **MyHelloWorld**. (Ellenőrizze a nevet használja, ez az oktatóanyag következő lépései a WAR-fájl MyHelloWorld nevű toobe várt). A képernyő jelenik meg hasonló toohello következő:

   ![][ic589576]

1. Kattintson a **Befejezés** gombra.

1. A Eclipse Project Explorer nézet, bontsa ki a **MyHelloWorld**. Kattintson a jobb gombbal a **WebContent** (Webes tartalom), majd a **New** (Új) elemre, és végül a **JSP File** (JSP-fájl) elemre.

1. A hello **új JSP-fájl** párbeszédpanelen nevű hello fájl **index.jsp**. Hello fölérendelt mappája, tartsa **MyHelloWorld/WebContent**hello következő szerint:

   ![][ic659262]

1. A hello **JSP-sablon kiválasztása** párbeszédpanel, az oktatóanyag válasszon alkalmazásában **új JSP-fájl (html)** kattintson **Befejezés**.

1. Hello index.jsp fájl megnyitásakor az eclipse-ben, adja hozzá szöveg toodynamically megjelenítési **Hello World!** hello meglévő belül `<body>` elemet. A frissített `<body>` tartalom hello következő jelennek meg:
   ```
   <body>
   <b><% out.println("Hello World!"); %></b>
   </body>
   ```
1. Mentse az index.jsp.

## <a name="toodeploy-your-application-tooazure-hello-quick-and-simple-way"></a>toodeploy az alkalmazás tooAzure, hello gyors és egyszerű módja
Amint egy Java webes alkalmazás készen áll a tootest rendelkezik, a következő helyi tootry azt ki közvetlenül a hello Azure cloud hello is használhatja.

1. Kattintson a Eclipse Project Explorer **MyHelloWorld**.

2. Hello Eclipse eszköztárában kattintson hello **közzététel** legördülő gomb, és kattintson a **közzététele, Azure Cloud Service**

   ![][publishDropdownButton]

3. Ha közzéteszi a az alkalmazás tooAzure hello az első alkalommal, és nem hozott létre egy Azure-telepítés-projektjét, amely előtt az alkalmazást, az Azure-telepítés projektben, automatikusan létrejön. Meg kell jelennie a hello kérdezzen rá, amelyet követően is felsorolja a hello JDK csomagon és alkalmazáson kiszolgáló, amely automatikusan telepített toorun az alkalmazást.

   ![][ic789598]
   
   Ez a helyi megközelítés lehetővé teszi, hogy egy egyszerűen és gyorsan tootest az alkalmazás az Azure-ban tooconfigure egy adott kiszolgáló vagy a JDK hello alapértelmezett értéke eltérő. Ha elégedett hello alapértelmezett beállításokat, kattintson **OK** az alábbi lépésekkel hello toocontinue.
   Azonban ha azt szeretné, hogy toochange hello JDK vagy az alkalmazás alkalmazás-kiszolgáló toouse, ehhez később hello Azure telepítési projekt, automatikusan létrehozott szerkesztésével, vagy kattintson **Mégse** most és olvasása Hello **tudnivalók az Azure-telepítési projektek szakasz** oktatóanyag.

4. A hello **tooAzure közzététele** párbeszédpanel:

   1. Ha nincsenek előfizetések tooselect hello **előfizetés** listában, de kövesse ezeket a lépéseket tooimport az előfizetési adatai:
      1. Kattintson a **közzététele beállításfájl importálás**.
      2. A hello **importálási előfizetési adatok** párbeszédpanel, kattintson a **KÖZZÉTÉTELI-beállítások fájl letöltése**. Ha még nem jelentkezik be az Azure-fiókjába, a kért toolog fogja. Akkor kérni fogja az Azure toosave közzététele beállításfájl. Mentse a helyi számítógép tooyour.
      3. Még tart a hello **importálási előfizetési adatok** párbeszédpanelen hello kattintson **Tallózás** gombra, jelölje be hello közzététele beállításfájl helyileg hello előző lépésben mentett, és kattintson a  **Nyissa meg**. A képernyő alábbihoz hasonló toohello következő:![][ic644267]
      4. Kattintson az **OK** gombra.
   2. A **előfizetés**, válassza ki a kívánt az üzembe helyezéshez hello előfizetés.
   3. A **tárfiók**, válassza ki, hogy Ön toouse, vagy kattintson a hello tárfiókot **új** toocreate egy új tárfiókot.
   4. A **szolgáltatásnév**, válassza ki a hello felhőalapú szolgáltatás, hogy Ön toouse, vagy kattintson a **új** toocreate új felhőalapú szolgáltatás.
   5. A **cél operációs rendszer**, jelölje be hello hello operációs rendszer verziója, amelyet az toouse az üzembe helyezéshez.
   6. A **célkörnyezet**, válasszon ki a jelen oktatóanyag **átmeneti**. (Ha már készen áll a toodeploy tooyour munkakörnyezeti helyet, akkor módosítani fogjuk ezt túl**éles**.)
   7. Választható lehetőség: Gondoskodjon arról, hogy **felülírja a korábbi központi telepítés** be van jelölve, ha azt szeretné, hogy az új központi telepítési tooautomatically felülírása hello a korábbi központi telepítés. Ha engedélyezi ezt a beállítást, toohello közzétételekor fog nem élmény "409 ütközés" problémák ugyanazon a helyen.
      Vegye figyelembe, hogy hello **tooAzure közzététele** párbeszédpanel tartalmaz egy szakaszt **távelérési**. Alapértelmezés szerint nincs engedélyezve a távoli hozzáférés, és azt nem engedélyezi azt ebben a példában. Távelérés tooenable, írja be a felhasználói nevet és jelszót toouse bejelentkezéskor távolról. További információ a távelérés: [távelérés engedélyezése az Azure-telepítésekre az eclipse-ben][Enabling Remote Access for Azure Deployments in Eclipse].
      A **tooAzure közzététele** párbeszédpanel jelenik meg hasonló toohello következő:![][ic719488]

5. Kattintson a **közzététel** toopublish toohello átmeneti környezet.

   Amikor a kért tooperform teljes build, kattintson **Igen**. Ez az első buildekhez hello több percig is eltarthat.
   Egy **Azure tevékenységnapló** az Eclipse többlapos Nézetek szakaszban megjeleníti.
   ![][ic719489]Akkor is ezt a naplót használja, valamint hello **konzol** megtekintéséhez toosee hello a központi telepítés végrehajtási állapotát. A másik lehetőség az toohello toolog [Azure felügyeleti portálon][Azure Management Portal], és hello **Felhőszolgáltatások** toomonitor hello állapot szakasz.

6. Ha a központi telepítés telepítése sikeres volt, hello **Azure tevékenységnapló** állapota megjelenik **közzétett**. Kattintson a **közzétett**, ahogy az alábbi hello lemezképet, és a böngészőben nyílik meg a központi telepítési példánya.

   ![][ic719490]

Mivel ez egy átmeneti környezet központi telepítési tooa, hello DNS-neve lesz hello űrlap http://&lt;*guid*&gt;. cloudapp.net és hello URL-cím hello DNS-nevet és egy utótagból, az alkalmazás fogja tartalmazni. Például http://447564652c20426f6220526f636b7321.cloudapp.net/MyHelloWorld. (hello **MyHelloWorld** részére a kis-és nagybetűket.) Azt is láthatja, hello DNS nevét, ha a hello Azure Platform Management Portal (belül hello kezelési portál hello Felhőszolgáltatások része) hello központi telepítés nevére kattint.

Bár ez az útmutató egy átmeneti környezet központi telepítési toohello az volt, a következő egy központi telepítési tooproduction hello ugyanazokat a lépéseket, kivéve belül hello **tooAzure közzététele** párbeszédablakban válassza **éles** Ahelyett, hogy **átmeneti** a hello **célkörnyezet**. A központi telepítés tooproduction eredményezi egy URL-címet a választott helyett egy GUID Azonosítót az átmeneti használt hello DNS-neve alapján.

> [!WARNING]
> Ezen a ponton telepítette az Azure-alkalmazásokban toohello felhő. Azonban a továbblépés előtt rájön, hogy egy telepített alkalmazást, akkor is, ha nem fut, továbbra is tooaccrue számlázható idő az előfizetéséhez. Ezért fontos rendkívül nem kívánt központi telepítések törlése az Azure-előfizetéshez.
> 
> 

## <a name="about-azure-deployment-projects"></a>Az Azure-telepítés projektek
A rendezés toodeploy egy vagy több Java-alkalmazások tooAzure, az Azure-telepítési projekt van szükség. Lejátszás hello "csomagja", az alkalmazások kell csomagolni rendelés toobe közzé az Azure-on történő toobe hello szerepe.

Mellett az alkalmazások hello tudnivalókat az Azure-telepítés projektben vonatkozó információkat is tartalmaz egyéb legfontosabb összetevők a központi telepítés, a legfontosabb: hello application server tároló toorun lévő, és a Java runtime toorun hello azt a. Azure Java futtatókörnyezeteket és választhat Java-alkalmazáskiszolgálók nagy mennyiségű támogatja.

Itt hello példában jelentősen egyszerűsített oktatási célokra, bár az Azure-telepítés projektben is tartalmazhat más fontos konfigurációs adatait, amely lehetővé teszi toocreate szinte önkényesen összetett, méretezhető, magas rendelkezésre állású, többrétegű felhőszolgáltatások az alkalmazásokkal. Engedélyezheti a **munkamenet affinitás ("állandóságát munkamenet")**, **gyors gyorsítótárazás**, **SSL-feladatkiszervezést**, **tűzfalportot/útválasztási**, **távelérési**, és számos egyéb hatékony képességet.

Ha ez az oktatóanyag korábbi szakaszában hello befejeződött ("toodeploy az alkalmazás tooAzure, hello gyors és egyszerű módot"), ekkor egy új Azure-telepítés projektre a Project Explorer hozta létre automatikusan, és nevű hello " **MyHelloWorld_onAzure**".

Sikerült is használatba vette az oktatóanyag először hoz létre egy üres az Azure-telepítés projekt saját magának, és majd adja hozzá a alkalmazás(ok) tooit. Már egy folyamatot, de felkínálva hello kezdeti konfigurációs hello elejétől teljesebb körű vezérlése.

teljesen új, Azure-telepítés új projekt toocreate kattintson hello **új Azure-telepítési projekt** gomb ![][ic710876].

Függetlenül attól, hogy Ön már meglévő Azure-telepítés projekttel működik, vagy hozzon létre egyet a teljesen, képes toochange, a konfigurációs beállításokat és az összetevők, például hello JDK, vagy egyszerűen egyaránt hello alkalmazáskiszolgáló, tetszőleges időpontban.

toochange hello JDK, vagy hello alkalmazáskiszolgáló, vagy egy meglévő Azure-telepítés projektben hello alkalmazásainak listáját:

1. Bontsa ki a hello projektcsomópontra (pl. **MyHelloWorld_onAzure**) a Project Explorer hello

2. Kattintson a jobb gombbal **WorkerRole1**

3. Bontsa ki a hello **Azure** almenü hello helyi menü

4. Kattintson a **kiszolgálókonfiguráció**

Függetlenül attól, hogy a kiszolgáló konfigurációs lépések indításakor meglévő Azure-telepítés projekt szerkesztésével, ahogy fent látható, vagy teljesen új újat hoz létre, látni fogja hello ugyanilyen típusú párbeszédpanelek tooconfigure lehetővé teszi a JDK kiszolgáló és az alkalmazás összetevők. toolearn több hogyan toochange hello beállításai azokat párbeszédpanelek, például toochange hello JDK, a kiszolgáló hello és hozzáadása vagy eltávolítása alkalmazások üzembe helyezése esetén lásd: hello [kiszolgáló konfigurációs tulajdonságai] [ Server configuration properties] cikk.

## <a name="windows-only-toodeploy-your-application-toohello-compute-emulator"></a>Csak Windows: toodeploy az alkalmazás toohello compute emulator

> [!NOTE]
> hello Azure emulátorban Windows csak érhető el. Hagyja ki ebben a szakaszban, ha nem Windows operációs rendszert használ.
> 
> 

Ha létrehozott egy új Azure-telepítés projektet hello a korábbi, azaz implicit módon, az alkalmazás tooAzure közzétételével ismertetett lépéseket követve hello JDK és alkalmazáskiszolgálók hello felhő, de nem helyi emuláció lettek konfigurálva. tooprepare a projekt tesztelési hello helyi emulátorban, kövesse az alábbi lépéseket:

1. Kattintson a Eclipse Project Explorer **MyHelloWorld_onAzure**.

2. Kattintson a jobb gombbal a **WorkerRole1**.

3. Bontsa ki a hello **Azure** almenü hello helyi menü.

4. Kattintson a **kiszolgálókonfiguráció**.

5. A hello **JDK** fület, ellenőrizze, hogy ha hello eszközkészlet előre konfigurált alapértelmezett helyi JDK meg. Ha nem, vagy ha azt szeretné, hogy toochange hello feltételezett alapértelmezett beállításokat, győződjön meg arról, hogy hello **használata hello JDK-útvonalról a helyi tesztelés céljából** jelölőnégyzet be van jelölve, és hello JDK telepítési helyet, amelyet az toouse van megadva. Ha azt szeretné, hogy toochange, hello kattintson **Tallózás** gombra, majd hello Tallózás vezérlővel hello JDK toouse hello könyvtár helyét.

6. Kattintson a hello **Server** fülre.

7. A hello **helyi kiszolgáló elérési útja** hello párbeszédpanelen hello alján szövegmezőben adja meg a helyileg telepített kiszolgáló hello típusa és a kiválasztott hello párbeszédpanelen hello tetején hello kiszolgáló fő verziószámának megfelelő hello elérési Hello **ilyen típusú kiszolgáló központi telepítése** jelölőnégyzetet. Ha toouse egy eltérő típusú vagy hello alkalmazáskiszolgáló főverzióját, először módosítsa hello kiválasztás tartozó jelölőnégyzet alapján.

8. Kattintson az **OK** gombra.

9. Hello Eclipse eszköztárában kattintson hello **Azure emulátorban futtatása** gomb, ![][ic710879]. Ha hello **Azure emulátorban futtatása** gomb nincs engedélyezve, ellenőrizze, hogy **MyHelloWorld_onAzure** a Project Explorer Eclipse meg van jelölve, és ügyeljen arra, hogy a Eclipse Project Explorer fókusz hello, aktuális ablak. A először indítsa el a projekt teljes build és a Java-webalkalmazás hello compute emulator majd elindítani. (Vegye figyelembe, hogy attól függően, hogy a számítógép teljesítményt nyújt, hello első build eltarthat néhány másodpercig tooa néhány perc között, de további buildek kap gyorsabban.) Hello első összeállítása lépés befejezése után fog kérni fogja a Windows felhasználói fiókok felügyelete (UAC) tooallow által a parancs toomake tooyour számítógép változik. Kattintson a **Yes** (Igen) gombra.

> [!IMPORTANT]
> Ha nem látja hello UAC rákérdezés, ellenőrizze a hello hello UAC ikonra a tálcán, és kattintson rá. Néha hello irányuló UAC-kérés nem jelenik meg legfelső ablakban, de csak a tálca ikonként látható.
> 
> 

1. Vizsgálja meg hello compute emulator UI toodetermine hello kimenete, ha merül fel a projektet. Attól függően, hogy a központi telepítés hello tartalmát az alkalmazás toobe hello compute emulator belül fut. néhány percig is eltarthat.

2. Indítsa el a böngészőt, és használják hello `http://localhost:8080/MyHelloWorld` hello címként (hello `MyHelloWorld` hello URL-cím része az kis-és nagybetűket). A MyHelloWorld alkalmazás (index.jsp hello kimenete), a kép a következő hasonló toohello kell megjelennie:

   ![][ic589579]

Készen áll a toostop az alkalmazás futtatását hello compute emulator, hello Eclipse eszköztárán kattintson hello **visszaállítása Azure Emulátorban** gomb, ![][ic710880].

## <a name="toodelete-your-deployment"></a>toodelete a központi telepítés
toodelete belül hello Azure eszköztára eclipse-ben a központi telepítés győződjön meg arról, hogy **MyHelloWorld_onAzure** van tartozó Eclipse Project Explorer kijelölt, győződjön meg arról, hello Eclipse Project Explorer rendelkezik hello aktuális ablak összpontosítson, és kattintson a Hello **Közzététel megszüntetése** gomb, ![][ic710883], hello Eclipse eszköztáron. (Azt megteheti ugyanazon művelet hello kattintson a jobb gombbal **MyHelloWorld_onAzure** tartozó Eclipse Project Explorer, kattintson a **Azure** , majd kattintson **UndeployAzure-felhőből**.) Megjelenik az hello **közzétételének Azure-projekt** párbeszédpanel.

![][ic719491]

Válassza ki a hello előfizetés és a felhőalapú szolgáltatás, amely tartalmazza a központi telepítés, a select hello telepítését szeretné, hogy toodelete, és kattintson **Közzététel megszüntetése**.

(Egy alternatív toousing hello eszközkészlet toodelete hello értékek egyikét toouse hello **Felhőszolgáltatások** hello Azure felügyeleti portálon szakasza: keresse meg a tooyour telepítési, válassza ki azt, és kattintson a hello **törlése** gombra. Ezzel leállítja, és majd törli, hello. Ha csak szeretné, hogy toostop hello központi telepítés, és nem törölhető, kattintson a hello **leállítása** gomb helyett hello **törlése** gombra kattint, a fentieknek, de ha nem törli a hello központi telepítését, számlázható díjak lesz továbbra is az üzembe helyezéshez tooaccrue akkor is, ha le van állítva).

## <a name="see-also"></a>Lásd még:
[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]

[Hello Azure eszköztára Eclipse telepítése][Installing hello Azure Toolkit for Eclipse] 

[Újdonságok az Eclipse Azure eszköztára hello][What's New in hello Azure Toolkit for Eclipse]

Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Role Properties]: http://go.microsoft.com/fwlink/?LinkID=699525
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Enabling Remote Access for Azure Deployments in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699538
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Server configuration properties]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[What's New in hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

<!-- IMG List -->

[ic589576]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589576.png
[ic589579]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic589579.png
[ic600360]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic600360.png
[ic644267]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic644267.png
[ic659262]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic659262.png
[ic710876]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710876.png
[ic710879]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710879.png
[ic710880]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710880.png
[ic710882]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710882.png
[ic710883]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic710883.png
[ic719488]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719488.png
[ic719489]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719489.png
[ic719490]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719490.png
[ic719491]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic719491.png
[ic789598]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/ic789598.png
[publishDropdownButton]: ./media/azure-toolkit-for-eclipse-creating-a-hello-world-application/publishDropdownButton.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690944.aspx -->
