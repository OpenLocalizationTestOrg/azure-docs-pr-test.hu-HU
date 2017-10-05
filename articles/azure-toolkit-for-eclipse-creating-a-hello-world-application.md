---
title: "Az Azure-Hello World felhőalapú szolgáltatás létrehozása az eclipse-ben"
description: "Ismerje meg, hogyan hozhat létre egy egyszerű Hello World alkalmazást az Azure-eszközkészlet használata az eclipse-ben."
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
ms.openlocfilehash: 9b31f0faeb6ee7b5e7b8fe3a1f2827133d6188e6
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-hello-world-cloud-service-for-azure-in-eclipse"></a>Az Azure-Hello World felhőalapú szolgáltatás létrehozása az eclipse-ben
A következő lépések bemutatják a létrehozása és telepítése az Azure-bA alapszintű JSP-alkalmazás az Azure-eszközkészlet használata az eclipse-ben. A JSP példa az egyszerűség kedvéért azonban nagyon hasonló lépéseket az Azure-telepítés illeti lenne egy Java servlet megfelelő.

Az alkalmazás az alábbihoz hasonlóan fog kinézni:

![][ic600360]

## <a name="prerequisites"></a>Előfeltételek
* A Java fejlesztői készlet (JDK), v 1.7 vagy újabb.
* Java EE-fejlesztőknek IDE Holdas Indigo vagy újabb. Ez letölthető <http://www.eclipse.org/downloads/>.
* A Java-alapú webkiszolgáló vagy a kiszolgáló, például az Apache Tomcat, GlassFish, JBoss alkalmazáskiszolgáló, Jetty vagy IBM® WebSphere Liberty® alkalmazás kiszolgálómag terjesztési.
* Azure-előfizetéssel, amely szerezhető: a <http://azure.microsoft.com/pricing/purchase-options/>.
* Az Azure eszközkészlet az eclipse-ben. További információkért lásd: [az eclipse-ben az Azure eszközkészlet telepítésével][Installing the Azure Toolkit for Eclipse].

## <a name="to-create-a-hello-world-application"></a>Hello World alkalmazás létrehozása
Először először foglalkozunk létrehoz egy Java-projektet.

1. Indítsa el az Eclipse, és válassza a menü **fájl**, kattintson a **új**, és kattintson a **dinamikus webes projekt**. (Ha nem lát **dinamikus webes projekt** kattintás után az elérhető projektek tulajdonosaként **fájl**, **új**, majd tegye a következőket: kattintson **fájl**, kattintson a **új**, kattintson a **projekt …** , bontsa ki a **webes**, kattintson a **dinamikus webes projekt**, és kattintson a **következő**.)

1. Ebben az oktatóanyagban a nevet a projektnek **MyHelloWorld**. (Ellenőrizze a nevet használja, ez az oktatóanyag következő lépései a WAR-fájlt, hogy az elnevezésük MyHelloWorld várt). A képernyő jelenik meg a következőhöz hasonló:

   ![][ic589576]

1. Kattintson a **Befejezés** gombra.

1. A Eclipse Project Explorer nézet, bontsa ki a **MyHelloWorld**. Kattintson a jobb gombbal a **WebContent** (Webes tartalom), majd a **New** (Új) elemre, és végül a **JSP File** (JSP-fájl) elemre.

1. Az a **új JSP-fájl** párbeszédpanelen, a fájl neve **index.jsp**. A szülőmappa mint **MyHelloWorld/WebContent**, ahogy az a következőket:

   ![][ic659262]

1. Az a **JSP-sablon kiválasztása** párbeszédpanel, az oktatóanyag válasszon alkalmazásában **új JSP-fájl (html)** kattintson **Befejezés**.

1. Ha megnyílt az index.jsp fájl az eclipse-ben, vegye fel dinamikusan megjelenő szöveg **Hello World!** a már meglévő `<body>` elemhez. A frissített `<body>` tartalma megjelenjen-e az alábbiak szerint:
   ```
   <body>
   <b><% out.println("Hello World!"); %></b>
   </body>
   ```
1. Mentse az index.jsp.

## <a name="to-deploy-your-application-to-azure-the-quick-and-simple-way"></a>Az Azure-ba, az alkalmazás telepítéséhez a gyors és egyszerű módja
Amint kötötte Java-webalkalmazás rendelkezik, a következő helyi használatával próbálja ki közvetlenül az Azure felhőben.

1. Kattintson a Eclipse Project Explorer **MyHelloWorld**.

2. Az Eclipse eszköztáron kattintson a **közzététel** legördülő gomb, és kattintson a **közzététele, Azure Cloud Service**

   ![][publishDropdownButton]

3. Ha az alkalmazás az Azure-bA az első alkalommal való közzétételekor, és nem hozott létre egy Azure-telepítés-projektjét, amely előtt az alkalmazást, az Azure-telepítés projektben, automatikusan létrejön. Meg kell jelennie a következő kérését, amelyet a JDK-csomagot is tartalmazza, és a kiszolgáló, amely automatikusan telepíti az alkalmazás futtatásához.

   ![][ic789598]
   
   A helyi megközelítés lehetővé teszi egy gyorsan és egyszerűen elvégezhető az alkalmazás teszteléséhez az Azure-ban anélkül, hogy egy adott kiszolgáló vagy a JDK, amely eltér az alapértelmezett értékeket konfigurálhatja. Ha elégedett az alapértelmezett beállításokat, kattintson **OK** folytatása előtt az alábbi lépéseket.
   Azonban ha meg szeretné változtatni a JDK vagy az alkalmazáshoz használandó alkalmazás-kiszolgáló teheti, hogy később az automatikusan létrehozott, az Azure-telepítés projekt szerkesztésével, vagy kattintson **Mégse** most és olvasási a **tudnivalók az Azure-telepítési projektek szakasz** oktatóanyag.

4. Az a **Azure közzététel** párbeszédpanel:

   1. Ha kijelöléséhez egyetlen előfizetés sincs a **előfizetés** listában, de az előfizetési adatok importálása a következő lépésekkel:
      1. Kattintson a **közzététele beállításfájl importálás**.
      2. Az a **importálási előfizetési adatok** párbeszédpanel, kattintson a **KÖZZÉTÉTELI-beállítások fájl letöltése**. Ha még nem jelentkezik be az Azure-fiókjába, fogja kérni fogja a bejelentkezéshez. Majd kéri az Azure mentési beállítások fájl közzététele. Mentse a helyi számítógépen.
      3. Még mindig a **importálási előfizetési adatok** párbeszédpanel, kattintson a **Tallózás** gombra, válassza ki a közzétételi beállítások fájlja helyileg az előző lépésben mentett, majd **nyissa meg**. A képernyőn a következőhöz hasonlóan kell kinéznie:![][ic644267]
      4. Kattintson az **OK** gombra.
   2. A **előfizetés**, válassza ki az előfizetést, amelyet az használja az üzembe helyezéshez.
   3. A **tárfiók**, válassza ki a tárfiókot használja, vagy kattintson a kívánt **új** egy új tárfiók létrehozásához.
   4. A **szolgáltatásnév**, válassza ki a felhőalapú szolgáltatást használja, vagy kattintson a kívánt **új** új felhőalapú szolgáltatás létrehozása.
   5. A **cél operációs rendszer**, válassza ki a telepítéshez használni kívánt operációs rendszer verziója.
   6. A **célkörnyezet**, válasszon ki a jelen oktatóanyag **átmeneti**. (Ha készen áll a munkakörnyezeti helyet szeretne telepíteni, akkor módosítani fogjuk ezt **éles**.)
   7. Választható lehetőség: Gondoskodjon arról, hogy **felülírja a korábbi központi telepítés** be van jelölve, ha azt szeretné, a korábbi központi telepítés automatikusan felülírja az új központi telepítést. Ha engedélyezi ezt a beállítást, tartalma nem élmény "409 ütközés" problémák ugyanazon a helyen történő közzététel esetén.
      Vegye figyelembe, hogy a **Azure közzététel** párbeszédpanel tartalmaz egy szakaszt **távelérési**. Alapértelmezés szerint nincs engedélyezve a távoli hozzáférés, és azt nem engedélyezi azt ebben a példában. Ahhoz, hogy a távoli elérés, akkor meg egy felhasználónevet és jelszót használja, ha távoli bejelentkezés. További információ a távelérés: [távelérés engedélyezése az Azure-telepítésekre az eclipse-ben][Enabling Remote Access for Azure Deployments in Eclipse].
      A **Azure közzététel** párbeszédpanel jelenik meg a következőhöz hasonló:![][ic719488]

5. Kattintson a **közzététel** átmeneti környezet közzétételére.

   Teljes build végrehajtásához felkéréskor kattintson **Igen**. Ez az első buildjéhez több percig is eltarthat.
   Egy **Azure tevékenységnapló** az Eclipse többlapos Nézetek szakaszban megjeleníti.
   ![][ic719489]Használhatja ezt a naplót, valamint a **konzol** nézet, hogy a telepítés előrehaladását. Helyett jelentkezzen be a [Azure felügyeleti portálon][Azure Management Portal], és használja a **Felhőszolgáltatások** szakasz állapotának figyelésére.

6. Ha a központi telepítés telepítése sikeres volt, a **Azure tevékenységnapló** állapota megjelenik **közzétett**. Kattintson a **közzétett**, a következő ábrán látható módon, és a böngésző csak akkor nyílik meg a központi telepítési példánya.

   ![][ic719490]

Az átmeneti környezetben való központi telepítés volt, mert a DNS-neve lesz a képernyő http://&lt;*guid*&gt;. cloudapp.net, és a DNS-nevét és az alkalmazás egy utótagot tartalmaz URL-címet fog. Például http://447564652c20426f6220526f636b7321.cloudapp.net/MyHelloWorld. (A **MyHelloWorld** részére a kis-és nagybetűket.) A DNS-nevét is megtekinthető, ha a telepítés neve az Azure Platform felügyeleti portálon (belül a kezelési portálon, a Cloud Services része) gombra kattint.

Bár ez az útmutató az átmeneti üzembe helyezése, egy éles környezetbe való telepítés a következő ugyanazokat a lépéseket, kivéve belül a **Azure közzététel** párbeszédablakban válassza **éles** helyett **átmeneti** a a **célkörnyezet**. Egy éles környezetbe való telepítés eredményezi egy URL-címet a választott helyett egy GUID Azonosítót az átmeneti használt DNS-neve alapján.

> [!WARNING]
> Ezen a ponton a Azure alkalmazást a felhőbe telepített. Azonban a továbblépés előtt rájön, hogy egy telepített alkalmazást, akkor is, ha nem fut, továbbra is keletkeznek az előfizetéshez tartozó számlázható idő. Ezért fontos rendkívül nem kívánt központi telepítések törlése az Azure-előfizetéshez.
> 
> 

## <a name="about-azure-deployment-projects"></a>Az Azure-telepítés projektek
Egy vagy több Azure Java-alkalmazások központi telepítéséhez, egy Azure-telepítési projekt van szükség. Azt a szerepet "csomag", az alkalmazások kell ahhoz, hogy közzé kell tenni az Azure-on be kell csomagolni.

Mellett az alkalmazások kapcsolatos információkért az Azure-telepítés projektben vonatkozó információkat is tartalmaz egyéb legfontosabb összetevők a központi telepítés, a legfontosabb: az alkalmazás server tároló a webalkalmazás futtatásához, és a Java-futtatókörnyezet futtatásához. Azure Java futtatókörnyezeteket és választhat Java-alkalmazáskiszolgálók nagy mennyiségű támogatja.

Bár az itt bemutatott példában jelentősen egyszerűsített oktatási célokra, az Azure-telepítés projektben is tartalmazhatnak más fontos konfigurációs adatait, amely lehetővé teszi az alkalmazásokkal szinte önkényesen összetett, méretezhető, magas rendelkezésre állású, többrétegű felhőszolgáltatások létrehozásához. Engedélyezheti a **munkamenet affinitás ("állandóságát munkamenet")**, **gyors gyorsítótárazás**, **SSL-feladatkiszervezést**, **tűzfalportot/útválasztási**, **távelérési**, és számos egyéb hatékony képességet.

Ha befejezte az előző szakaszban a jelen oktatóanyag ("az alkalmazás központi telepítése az Azure-ba, a gyors és egyszerű módot"), ekkor a Project Explorer automatikusan jönnek létre, és nevű új Azure-telepítés projekt "**MyHelloWorld_onAzure**".

Sikerült is használatba vette az oktatóanyag először hoz létre egy üres az Azure-telepítés projekt saját magának, és majd adja hozzá a alkalmazás(ok) rá. Már egy folyamatot, de ad meg a kezdeti konfiguráció az elejétől.

A teljesen új Azure-telepítés projekt létrehozásához kattintson a **új Azure-telepítési projekt** gomb ![][ic710876].

Függetlenül attól, hogy Ön már meglévő Azure-telepítés projekttel működik, vagy hozzon létre egyet a teljesen, le is tudja módosítani a konfigurációs beállításokat és az összetevők, például a JDK vagy a kiszolgáló, egyaránt bármikor könnyen.

A JDK vagy a kiszolgáló vagy egy meglévő Azure-telepítés projektben Alkalmazáslista módosítása:

1. Bontsa ki a projekt csomópontot (pl. **MyHelloWorld_onAzure**) a Project Explorer

2. Kattintson a jobb gombbal **WorkerRole1**

3. Bontsa ki a **Azure** almenü a helyi menü

4. Kattintson a **kiszolgálókonfiguráció**

Függetlenül attól, hogy használatba kiszolgáló konfigurációs lépéseket a fentiek szerint a meglévő Azure-telepítés projekt szerkesztésével, vagy teljesen új újat hoz létre, azonos típusú lehet konfigurálni a JDK-, kiszolgáló és az alkalmazás-összetevők párbeszédpanelek jelenjen meg. Több e párbeszédpanelek, módosítsa a JDK, a kiszolgáló és vegye fel vagy távolítsa el az alkalmazások üzembe helyezése esetén például a beállítások módosításáról lásd: a [kiszolgáló konfigurációs tulajdonságai] [ Server configuration properties] cikk.

## <a name="windows-only-to-deploy-your-application-to-the-compute-emulator"></a>Csak Windows: a compute emulator az alkalmazás központi telepítése

> [!NOTE]
> Az Azure emulátorban Windows csak érhető el. Hagyja ki ebben a szakaszban, ha nem Windows operációs rendszert használ.
> 
> 

Ha létrehozott egy új Azure-telepítés projektet, a következő lépéseket korábbi, azaz implicit módon, Azure JDK és az alkalmazás az alkalmazás közzétételével kiszolgálók konfigurálva a felhő, de nem helyi emuláció. A projekt helyi emulátorban tesztelési előkészítéséhez kövesse az alábbi lépéseket:

1. Kattintson a Eclipse Project Explorer **MyHelloWorld_onAzure**.

2. Kattintson a jobb gombbal a **WorkerRole1**.

3. Bontsa ki a **Azure** almenü a helyi menü.

4. Kattintson a **kiszolgálókonfiguráció**.

5. Az a **JDK** fület, ellenőrizze, hogy ha az eszközkészlet előre konfigurált alapértelmezett helyi JDK meg. Ha nem, vagy ha meg szeretné változtatni az feltételezett alapértelmezett beállításokat, győződjön meg arról, hogy a **használja ezt az elérési utat a JDK helyi tesztelés céljából** jelölőnégyzet be van jelölve, és a JDK használni kívánt telepítési hely meg van adva. Ha szeretné módosítani, kattintson a **Tallózás** gombra, majd a Tallózás vezérlő meg és jelölje ki a használandó JDK könyvtár helyét.

6. Kattintson a **Server** fülre.

7. Az a **helyi kiszolgáló elérési útja** szövegmezőben a párbeszédpanel alján, adja meg a helyileg telepített kiszolgáló típusa és a párbeszédpanel tetején a kijelölt kiszolgáló fő verziószámának megfelelő elérési a **ilyen típusú kiszolgáló központi telepítése** jelölőnégyzetet. Ha az eltérő típusú vagy a kiszolgáló fő verziószáma, először módosítsa a kijelölés alapján tartozó jelölőnégyzet.

8. Kattintson az **OK** gombra.

9. Az Eclipse eszköztáron kattintson a **Azure emulátorban futtatása** gomb, ![][ic710879]. Ha a **Azure emulátorban futtatása** gomb nincs engedélyezve, ellenőrizze, hogy **MyHelloWorld_onAzure** Project Explorer Eclipse meg van jelölve, és ellenőrizze, hogy rendelkezik-e a Eclipse Project Explorer fókusz, az aktuális ablak. A először indítsa el a projekt teljes build és a Java-webalkalmazás a compute emulator majd elindítani. (Vegye figyelembe, hogy attól függően, hogy a számítógép teljesítményt nyújt, az első build is igénybe vehet néhány percet néhány másodperc között, de további buildek kap gyorsabban.) Az első összeállítása lépés befejezése után, kérni fogja a Windows felhasználói fiókok felügyelete (UAC) engedélyezéséhez módosításokat végrehajtani ezt a parancsot. Kattintson a **Yes** (Igen) gombra.

> [!IMPORTANT]
> Ha nem látja a felhasználói fiókok felügyelete, ellenőrizze a felhasználói fiókok Felügyeletének ikon a Windows tálcán, és kattintson rá. Egyes esetekben a felhasználói fiókok felügyelete kérdés nem jelenik meg legfelső ablakban, de csak a tálca ikonként látható.
> 
> 

1. Vizsgálja meg a compute emulator UI vannak-e problémák merülnek fel a projektet a kimenetét. Attól függően, hogy a központi telepítés tartalmát az alkalmazás teljesen kell elindítani a compute emulator belül néhány percig is eltarthat.

2. A böngésző és az URL-címet használjon `http://localhost:8080/MyHelloWorld` címként (a `MyHelloWorld` az URL-cím része az kis-és nagybetűket). Megjelenik a MyHelloWorld alkalmazás (az index.jsp kimenete), az alábbi képen hasonlít:

   ![][ic589579]

Amikor készen áll az alkalmazás futtatását a compute emulator az Eclipse eszköztáron a leállításához kattintson a **visszaállítása Azure Emulátorban** gomb, ![][ic710880].

## <a name="to-delete-your-deployment"></a>A központi telepítés törlése
Ügyeljen arra, hogy törölni szeretné a központi telepítést Eclipse Azure eszköztára belül, **MyHelloWorld_onAzure** van tartozó Eclipse Project Explorer kijelölt, győződjön meg arról, az Eclipse Project Explorer van az aktuális ablak összpontosítson, és kattintson a **Közzététel megszüntetése** gomb, ![][ic710883], az Eclipse-eszköztáron. (Kattintson a jobb gombbal, végrehajtja a műveletet **MyHelloWorld_onAzure** tartozó Eclipse Project Explorer, kattintson a **Azure** , majd **Azure felhőből Undeploy**.) Ez megjeleníti a **közzétételének Azure-projekt** párbeszédpanel.

![][ic719491]

Válassza ki az előfizetés és a felhőalapú szolgáltatás, amely tartalmazza a központi telepítés, ki kell jelölni a telepítést, törlése, és kattintson a kívánt **Közzététel megszüntetése**.

(Törölni a példányt az eszközkészlet használata helyett, hogy használja a **Felhőszolgáltatások** az Azure felügyeleti portálon szakasza: keresse meg a központi telepítés, válassza ki azt, és kattintson a **törlése** gombra. Ezzel leállítja, és a központi telepítést, majd törölje. Ha csak szeretné, hogy állítsa le a telepítést, és nem törölhető, kattintson a **leállítása** gombra kattint, ahelyett, hogy a **törlése** gomb, de, mint a fenti nem törli a központi telepítést, ha számlázható díjakat továbbra is keletkeznek az üzembe helyezéshez, még akkor is, ha le van állítva).

## <a name="see-also"></a>Lásd még:
[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]

[Az eclipse-ben az Azure eszközkészlet telepítése][Installing the Azure Toolkit for Eclipse] 

[What's New in Eclipse Azure eszköztára][What's New in the Azure Toolkit for Eclipse]

Azure Java használatával kapcsolatos további információkért lásd: a [Azure Java fejlesztői központból][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Role Properties]: http://go.microsoft.com/fwlink/?LinkID=699525
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Enabling Remote Access for Azure Deployments in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699538
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546
[Server configuration properties]: http://go.microsoft.com/fwlink/?LinkID=699525#server_configuration_properties
[What's New in the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699552

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
