---
title: "a webes alkalmazás a sablon - Azure Cosmos DB aaaDeploy |} Microsoft Docs"
description: "Ismerje meg, hogyan toodeploy Azure Cosmos DB fiókkal, az Azure App Service Web Apps és a minta-webalkalmazás Azure Resource Manager-sablonnal."
services: cosmos-db, app-service\web
author: mimig1
manager: jhubbard
editor: monicar
documentationcenter: 
ms.assetid: 087d8786-1155-42c7-924b-0eaba5a8b3e0
ms.service: cosmos-db
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2016
ms.author: mimig
ms.openlocfilehash: b2bdde9279aad570606d7bf06dfc710f564b4d0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-azure-cosmos-db-and-azure-app-service-web-apps-using-an-azure-resource-manager-template"></a>Azure Cosmos adatbázis és az Azure App Service Web Apps Azure Resource Manager-sablonnal telepítése
Az oktatóanyag bemutatja, hogyan toouse az Azure Resource Manager sablon toodeploy, és integrálhatja a [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/), egy [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) web app és egy minta-webalkalmazáshoz.

A Azure Resource Manager-sablonok, könnyen automatizálhatja az Azure-erőforrások a hello telepítését és konfigurálását.  Ez az oktatóanyag bemutatja, hogyan toodeploy egy webalkalmazást, és automatikusan konfigurálja az Azure Cosmos DB fiók kapcsolódási adatait.

Ez az oktatóanyag befejezése után fog tudni tooanswer hello a következő kérdéseket:  

* Hogyan használja az Azure Resource Manager sablon toodeploy és egy Azure Cosmos DB-fiókot és egy webalkalmazást az Azure App Service integrálható?
* Hogyan használja az Azure Resource Manager sablon toodeploy és integrálja az Azure Cosmos DB fiókkal, egy webalkalmazást az App Service Web Apps és a Webdeploy-alkalmazás?

<a id="Prerequisites"></a>

## <a name="prerequisites"></a>Előfeltételek
> [!TIP]
> Ez az oktatóanyag nem feltételezi azt tapasztalattal az Azure Resource Manager-sablonok vagy JSON-NÁ, amíg kell kívánja toomodify hello hivatkozott sablonok vagy a telepítési lehetőségeket, majd az egyes ezek a területek Tudásbázis lesz szükség.
> 
> 

Mielőtt hozzálátna hello utasításokat ebben az oktatóanyagban, ellenőrizze, hogy hello következő:

* Azure-előfizetés. Azure előfizetés-alapú platform.  Előfizetés beszerzésével kapcsolatos további információkért lásd: [megvásárlási lehetőségeinek](https://azure.microsoft.com/pricing/purchase-options/), [ajánlatok](https://azure.microsoft.com/pricing/member-offers/), vagy [ingyenes](https://azure.microsoft.com/pricing/free-trial/).

## <a id="CreateDB"></a>1. lépés: Hello sablon fájlok letöltése
Kezdjük használjuk, ebben az oktatóanyagban hello sablon fájlok letöltésével.

1. Töltse le a hello [hozzon létre egy Azure Cosmos DB fiókot, a Web Apps, és telepít egy bemutató alkalmazás](https://portalcontent.blob.core.windows.net/samples/DocDBWebsiteTodo.json) sablon tooa helyi mappát (pl. C:\Azure Cosmos DBTemplates). Ez a sablon Azure Cosmos DB fiókkal, egy App Service web app és a webes alkalmazás telepíti.  Hello webes alkalmazás tooconnect toohello Azure Cosmos DB fiók is automatikusan be fogja beállítani.
2. Töltse le a hello [hozzon létre egy Azure Cosmos DB fiókot és a Web Apps minta](https://portalcontent.blob.core.windows.net/samples/DocDBWebSite.json) sablon tooa helyi mappát (pl. C:\Azure Cosmos DBTemplates). Ez a sablon Azure Cosmos DB adatait, az App Service web app alkalmazásban telepíti és módosítani fogja a hello hely alkalmazás beállítások tooeasily felület Azure Cosmos adatbázis-kapcsolati információkat, de nem tartalmazza a webalkalmazás.  

<a id="Build"></a>

## <a name="step-2-deploy-hello-azure-cosmos-db-account-app-service-web-app-and-demo-application-sample"></a>2. lépés: Központi telepítése hello Azure Cosmos DB fiók, az App Service web app és a bemutató alkalmazás minta
Most tegyük a az első sablon üzembe helyezése.

> [!TIP]
> hello sablon nem ellenőrzi, hogy hello web app és alatt megadott Azure Cosmos DB fióknév egy) érvényes és b) érhető el.  Javasoljuk, hogy ellenőrizze hello hello rendelkezésre állását neveknek toosupply előzetes toosubmitting hello üzembe helyezésének a tervezéséhez.
> 
> 

1. Bejelentkezési toohello [Azure Portal](https://portal.azure.com), kattintson az új, és keresse meg a "Sablon-üzembehelyezés".
    ![Képernyőfelvétel a hello sablon-üzembehelyezés felhasználói felület](./media/create-website/TemplateDeployment1.png)
2. Hello sablon telepítési elemet, és kattintson a **létrehozása** ![hello sablon-üzembehelyezés felhasználói felületének képernyőképe](./media/create-website/TemplateDeployment2.png)
3. Kattintson a **Szerkesztés sablon**, illessze be a hello DocDBWebsiteTodo.json sablonfájl hello tartalmát, és kattintson **mentése**.
   ![Képernyőfelvétel a hello sablon-üzembehelyezés felhasználói felület](./media/create-website/TemplateDeployment3.png)
4. Kattintson a **paraméterek szerkesztése**, adjon meg értékeket a kötelező paramétereknek hello, és kattintson a **OK**.  hello paraméterek a következők:
   
   1. SITENAME: Adja meg az App Service web app name hello és használt tooconstruct hello URL-cím tooaccess hello webalkalmazás használt (pl. Ha "mydemodocdbwebapp" megad, akkor hello URL-t, amely érik hello webalkalmazás mydemodocdbwebapp.azurewebsites.NET).
   2. HOSTINGPLANNAME: Az App Service üzemeltetési terv toocreate hello nevét adja meg.
   3. HELY: Határozza meg, mely toocreate hello Azure Cosmos adatbázis és a webes alkalmazás erőforrásokat az Azure-beli hely hello.
   4. DATABASEACCOUNTNAME: Hello Azure Cosmos DB fiók toocreate hello nevét adja meg.   
      
      ![Képernyőfelvétel a hello sablon-üzembehelyezés felhasználói felület](./media/create-website/TemplateDeployment4.png)
5. Válasszon egy meglévő erőforráscsoportot, vagy adjon meg egy nevet toomake egy új erőforráscsoportot, és hello erőforráscsoport helyét.

    ![Képernyőfelvétel a hello sablon-üzembehelyezés felhasználói felület](./media/create-website/TemplateDeployment5.png)
6. Kattintson a **tekintse át a jogi feltételeket**, **beszerzési**, és kattintson a **létrehozása** toobegin hello központi telepítés.  Válassza ki **PIN-kód toodashboard** így hello eredményül kapott telepítési könnyen látható, az Azure portál kezdőlapján.
   ![Képernyőfelvétel a hello sablon-üzembehelyezés felhasználói felület](./media/create-website/TemplateDeployment6.png)
7. Hello központi telepítés befejezése után hello erőforráscsoport panel nyílik meg.
   ![Képernyőfelvétel a hello erőforráscsoport panel](./media/create-website/TemplateDeployment7.png)  
8. toouse hello alkalmazás, egyszerűen nyissa meg a toohello webes alkalmazás URL-CÍMÉT (hello a fenti példában hello URL-cím lenne http://mydemodocdbwebapp.azurewebsites.net).  A következő webalkalmazás hello jelenik meg:
   
   ![Teendők mintaalkalmazás](./media/create-website/image2.png)
9. Lépjen tovább, és hozzon létre feladatok néhány hello web app alkalmazásban, és térjen vissza az Azure-portálon hello toohello erőforráscsoport panel. Kattintson a hello Azure Cosmos DB fiók erőforrás hello erőforrások listában, majd **lekérdezéskezelő**.
    ![Képernyőfelvétel a hello összegzés Lencsekorrekció hello webes alkalmazáshoz kiemelve](./media/create-website/TemplateDeployment8.png)  
10. Hello alapértelmezett lekérdezés, "Válassza ki * c", és vizsgálja meg a hello eredmények.  Figyelje meg, hogy hello lekérdezés fogadta hello todo elemeket a fenti 7. lépésben létrehozott hello JSON-ábrázolását.  Érzi, hogy a szabad tooexperiment lekérdezések; például, próbálja meg futtatni a SELECT * c WHERE c.isComplete származó = true tooreturn összes todo elemeket, amelyek el lettek megjelenítve.
    
    ![Képernyőfelvétel a hello lekérdezési eredményeket megjelenítő hello Query Explorer és az eredmények panelen](./media/create-website/image5.png)
11. Szabad tooexplore hello Azure Cosmos DB portál élmény látja, vagy módosítsa a hello Todo mintaalkalmazást.  Ha elkészült, helyezzünk üzembe egy másik sablont.

<a id="Build"></a> 

## <a name="step-3-deploy-hello-document-account-and-web-app-sample"></a>3. lépés: Dokumentum fiók hello és a webes alkalmazás minta központi telepítése
Most tegyük a a második sablon üzembe helyezése.  Ez a sablon hasznos tooshow hogyan akkor is behelyezése Azure Cosmos adatbázis-kapcsolati információkat, például a fiók végpont és a főkulcs egy webalkalmazást alkalmazás beállításait, vagy egyéni kapcsolati karakterláncot. Például lehet, hogy rendelkezik a saját webes alkalmazás, hogy kívánja toodeploy Azure Cosmos DB fiókkal, majd hello kapcsolatadatok központi telepítése során automatikusan feltöltődik értékkel rendelkezik.

> [!TIP]
> hello sablon nem ellenőrzi, hogy hello web app és alatt megadott Azure Cosmos DB fióknév egy) érvényes és b) érhető el.  Javasoljuk, hogy ellenőrizze hello hello rendelkezésre állását neveknek toosupply előzetes toosubmitting hello üzembe helyezésének a tervezéséhez.
> 
> 

1. A hello [Azure Portal](https://portal.azure.com), kattintson az új, és keresse meg a "Sablon-üzembehelyezés".
    ![Képernyőfelvétel a hello sablon-üzembehelyezés felhasználói felület](./media/create-website/TemplateDeployment1.png)
2. Hello sablon telepítési elemet, és kattintson a **létrehozása** ![hello sablon-üzembehelyezés felhasználói felületének képernyőképe](./media/create-website/TemplateDeployment2.png)
3. Kattintson a **Szerkesztés sablon**, illessze be a hello DocDBWebSite.json sablonfájl hello tartalmát, és kattintson **mentése**.
   ![Képernyőfelvétel a hello sablon-üzembehelyezés felhasználói felület](./media/create-website/TemplateDeployment3.png)
4. Kattintson a **paraméterek szerkesztése**, adjon meg értékeket a kötelező paramétereknek hello, és kattintson a **OK**.  hello paraméterek a következők:
   
   1. SITENAME: Adja meg az App Service web app name hello és használt tooconstruct hello URL-cím tooaccess hello webalkalmazás használt (pl. Ha "mydemodocdbwebapp" megad, akkor hello URL-t, amely érik hello webalkalmazás mydemodocdbwebapp.azurewebsites.NET).
   2. HOSTINGPLANNAME: Az App Service üzemeltetési terv toocreate hello nevét adja meg.
   3. HELY: Határozza meg, mely toocreate hello Azure Cosmos adatbázis és a webes alkalmazás erőforrásokat az Azure-beli hely hello.
   4. DATABASEACCOUNTNAME: Hello Azure Cosmos DB fiók toocreate hello nevét adja meg.   
      
      ![Képernyőfelvétel a hello sablon-üzembehelyezés felhasználói felület](./media/create-website/TemplateDeployment4.png)
5. Válasszon egy meglévő erőforráscsoportot, vagy adjon meg egy nevet toomake egy új erőforráscsoportot, és hello erőforráscsoport helyét.

    ![Képernyőfelvétel a hello sablon-üzembehelyezés felhasználói felület](./media/create-website/TemplateDeployment5.png)
6. Kattintson a **tekintse át a jogi feltételeket**, **beszerzési**, és kattintson a **létrehozása** toobegin hello központi telepítés.  Válassza ki **PIN-kód toodashboard** így hello eredményül kapott telepítési könnyen látható, az Azure portál kezdőlapján.
   ![Képernyőfelvétel a hello sablon-üzembehelyezés felhasználói felület](./media/create-website/TemplateDeployment6.png)
7. Hello központi telepítés befejezése után hello erőforráscsoport panel nyílik meg.
   ![Képernyőfelvétel a hello erőforráscsoport panel](./media/create-website/TemplateDeployment7.png)  
8. Kattintson a hello webalkalmazás erőforrás hello erőforrások listában, majd **Alkalmazásbeállítások** ![képernyőfelvétel a hello erőforráscsoport](./media/create-website/TemplateDeployment9.png)  
9. Vegye figyelembe, hogyan vannak jelen a hello Azure Cosmos DB végpont és hello Azure Cosmos DB főkulcsok Alkalmazásbeállítások.

    ![Alkalmazás-beállításaihoz](./media/create-website/TemplateDeployment10.png)  
10. Hello Azure Portal felfedezése szabad toocontinue látja, vagy hajtsa végre az Azure Cosmos DB egyik [minták](http://go.microsoft.com/fwlink/?LinkID=402386) toocreate a saját Azure Cosmos DB alkalmazás.

<a name="NextSteps"></a>

## <a name="next-steps"></a>Következő lépések
Gratulálunk! Azure Cosmos-adatbázis, az App Service web app és az Azure Resource Manager-sablonok használatával egy minta-webalkalmazáshoz telepítése után.

* További információ az Azure Cosmos DB, toolearn kattintson [Itt](http://azure.com/docdb).
* További információ az Azure App Service Web apps toolearn kattintson [Itt](http://go.microsoft.com/fwlink/?LinkId=325362).
* További információ az Azure Resource Manager-sablonok, toolearn kattintson [Itt](https://msdn.microsoft.com/library/azure/dn790549.aspx).

## <a name="whats-changed"></a>A változások
* Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)
* Egy útmutató toohello hello régi portál toohello új portál módosítását lásd: [navigáláshoz hivatkozás hello a klasszikus Azure portálon](http://go.microsoft.com/fwlink/?LinkId=529715)

> [!NOTE]
> Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](http://go.microsoft.com/fwlink/?LinkId=523751), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

