---
title: "A sablon - Azure Cosmos DB webalkalmazás üzembe helyezése |} Microsoft Docs"
description: "Egy Azure Cosmos DB fiókot, Azure App Service Web Apps és egy minta-webalkalmazáshoz, Azure Resource Manager-sablonnal telepítésének megismerése."
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
ms.custom: mvc
ms.openlocfilehash: 7ceb4bf97c29a18d6879af55615eea46037c51ce
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/05/2017
---
# <a name="deploy-azure-cosmos-db-and-azure-app-service-web-apps-using-an-azure-resource-manager-template"></a>Azure Cosmos adatbázis és az Azure App Service Web Apps Azure Resource Manager-sablonnal telepítése
Az oktatóanyag bemutatja, hogyan Azure Resource Manager-sablonok segítségével telepítheti, és integrálhatja a [Microsoft Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/), egy [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) web app és egy minta-webalkalmazáshoz.

A Azure Resource Manager-sablonok, könnyen automatizálhatja a telepítése és konfigurálása az Azure-erőforrások.  Ez az oktatóanyag bemutatja, hogyan telepítheti egy webalkalmazás, és automatikusan konfigurálja az Azure Cosmos DB fiók kapcsolat adatait.

Ez az oktatóanyag befejezése után lesz a következő kérdések megválaszolásához:  

* Hogyan használhatók az Azure Resource Manager-sablon üzembe helyezését, és egy Azure Cosmos DB-fiókot és egy webalkalmazást az Azure App Service integrálható?
* Hogyan használhatók az Azure Resource Manager-sablon üzembe helyezését és integrálja az Azure Cosmos DB fiókkal, egy webalkalmazást az App Service Web Apps és a Webdeploy-alkalmazás?

<a id="Prerequisites"></a>

## <a name="prerequisites"></a>Előfeltételek
> [!TIP]
> Ez az oktatóanyag nem feltételezi azt tapasztalattal az Azure Resource Manager-sablonok vagy JSON-NÁ, amíg kell módosítani kívánt a hivatkozott sablonokat és a telepítési lehetőségeket, majd ismerete, ezek a területek lesz szükség.
> 
> 

Ez az oktatóanyag utasításainak követése, előtt ellenőrizze, hogy a következő:

* Azure-előfizetés. Azure előfizetés-alapú platform.  Előfizetés beszerzésével kapcsolatos további információkért lásd: [megvásárlási lehetőségeinek](https://azure.microsoft.com/pricing/purchase-options/), [ajánlatok](https://azure.microsoft.com/pricing/member-offers/), vagy [ingyenes](https://azure.microsoft.com/pricing/free-trial/).

## <a id="CreateDB"></a>1. lépés: Töltse le a sablonfájlokat importálni
Kezdjük úgy, hogy letölti a sablonfájlokat importálni, ebben az oktatóanyagban használjuk.

1. Töltse le a [hozzon létre egy Azure Cosmos DB fiókot, a Web Apps, és telepít egy bemutató alkalmazás](https://portalcontent.blob.core.windows.net/samples/DocDBWebsiteTodo.json) sablon egy helyi mappába (pl. C:\Azure Cosmos DBTemplates). Ez a sablon Azure Cosmos DB fiókkal, egy App Service web app és a webes alkalmazás telepíti.  Azt is automatikusan konfigurálja a webes alkalmazás kapcsolódni az Azure Cosmos DB fiókjához.
2. Töltse le a [hozzon létre egy Azure Cosmos DB fiókot és a Web Apps minta](https://portalcontent.blob.core.windows.net/samples/DocDBWebSite.json) sablon egy helyi mappába (pl. C:\Azure Cosmos DBTemplates). Ez a sablon Azure Cosmos DB adatait, az App Service web app alkalmazásban telepíti és módosítja a hely alkalmazás beállításait, hogy könnyen felület Azure Cosmos adatbázis-kapcsolati információkat, de nem tartalmaz egy webalkalmazást.  

<a id="Build"></a>

## <a name="step-2-deploy-the-azure-cosmos-db-account-app-service-web-app-and-demo-application-sample"></a>2. lépés: Az Azure Cosmos DB fiók, az App Service web app és a bemutató alkalmazás minta telepítése
Most tegyük a az első sablon üzembe helyezése.

> [!TIP]
> A sablon nem ellenőrzi, hogy a webes alkalmazás neve és az alább megadott Azure Cosmos DB-fiók neve egy) érvényes és b) érhető el.  Erősen ajánlott, hogy ellenőrizze a neveket azt tervezi, hogy az üzemelő példány elküldése előtt adja meg a rendelkezésre állását.
> 
> 

1. Jelentkezzen be a [Azure Portal](https://portal.azure.com), kattintson az új, és keresse meg a "Sablon-üzembehelyezés".
    ![A sablon-üzembehelyezés felhasználói felületének képernyőképe](./media/create-website/TemplateDeployment1.png)
2. Válassza ki a sablon telepítési elemet, és kattintson a **létrehozása** ![a sablon-üzembehelyezés felhasználói felületének képernyőképe](./media/create-website/TemplateDeployment2.png)
3. Kattintson a **Szerkesztés sablon**, illessze be a DocDBWebsiteTodo.json sablon fájl tartalmát, és kattintson a **mentése**.
   ![A sablon-üzembehelyezés felhasználói felületének képernyőképe](./media/create-website/TemplateDeployment3.png)
4. Kattintson a **paraméterek szerkesztése**, adjon meg értékeket a kötelező paraméterekhez, majd kattintson **OK**.  A paraméterek a következők:
   
   1. SITENAME: Az App Service web app nevét adja meg, és a webes alkalmazás eléréséhez használandó URL-cím használatával (pl. Ha "mydemodocdbwebapp" megad, akkor az URL-címet, amely szerint a web app érik lesz mydemodocdbwebapp.azurewebsites.net).
   2. HOSTINGPLANNAME: A neve az App Service üzemeltetési terv létrehozásához.
   3. HELYE: Adja meg az Azure-hely használandó létrehozni az Azure Cosmos DB és a webes alkalmazás-erőforrásokat.
   4. DATABASEACCOUNTNAME: A neve az Azure Cosmos DB-fiók létrehozásához.   
      
      ![A sablon-üzembehelyezés felhasználói felületének képernyőképe](./media/create-website/TemplateDeployment4.png)
5. Válasszon egy meglévő erőforráscsoportot, vagy adjon meg egy új erőforráscsoport létrehozásához, és az erőforráscsoport helyének kiválasztása.

    ![A sablon-üzembehelyezés felhasználói felületének képernyőképe](./media/create-website/TemplateDeployment5.png)
6. Kattintson a **tekintse át a jogi feltételeket**, **beszerzési**, és kattintson a **létrehozása** a telepítés megkezdéséhez.  Válassza ki **rögzítés az irányítópulton** , az eredményül kapott egyszerűen az Azure portál kezdőlapján látható.
   ![A sablon-üzembehelyezés felhasználói felületének képernyőképe](./media/create-website/TemplateDeployment6.png)
7. A telepítés befejezése után, az erőforráscsoport panel nyílik meg.
   ![Az erőforráscsoport panel képernyőképe](./media/create-website/TemplateDeployment7.png)  
8. Az alkalmazás használatához egyszerűen nyissa meg a webes alkalmazás URL-CÍMÉT (a fenti példában az URL-cím lenne http://mydemodocdbwebapp.azurewebsites.net).  A következő webalkalmazás jelenik meg:
   
   ![Teendők mintaalkalmazás](./media/create-website/image2.png)
9. Lépjen tovább és hozzon létre a feladatok néhány a web app alkalmazásban, és térjen vissza az erőforráscsoport panel az Azure portálon. Az Azure Cosmos DB fiók erőforrás az erőforrások listájában kattintson, majd **lekérdezéskezelő**.
    ![A kijelölt webalkalmazás Lencsekorrekció Képernyőkép az összefoglalás](./media/create-website/TemplateDeployment8.png)  
10. Alapértelmezett lekérdezés "VÁLASSZA * c", és vizsgálja meg az eredményeket.  Figyelje meg, hogy rendelkezik a lekérdezés olvassa be a fenti 7. lépésben létrehozott teendőlista elemeinek JSON-ábrázolását.  Nyugodtan kísérletezhet lekérdezések; például, próbálja meg futtatni a SELECT * c WHERE c.isComplete származó = igaz értékre, amely el van megjelölve todo elemeket.
    
    ![A Query Explorer és az eredmények paneleken, a lekérdezési eredmények ábrázoló képernyőfelvétel](./media/create-website/image5.png)
11. Nyugodtan megismerkedhet a portál Azure Cosmos DB élményt, vagy módosítsa a teendőlista mintaalkalmazást.  Ha elkészült, helyezzünk üzembe egy másik sablont.

<a id="Build"></a> 

## <a name="step-3-deploy-the-document-account-and-web-app-sample"></a>3. lépés: A dokumentum fiók és a webes alkalmazás mintaalkalmazás telepítését
Most tegyük a a második sablon üzembe helyezése.  Ez a sablon akkor hasznos, jeleníthető meg, hogyan akkor is behelyezése Azure Cosmos adatbázis-kapcsolati információkat, például a fiók végpont és a főkulcs egy webalkalmazást alkalmazás beállításait, vagy egyéni kapcsolati karakterláncot. Például lehet, hogy rendelkezik a saját webes alkalmazás, amelyet szeretne telepíteni egy olyan Azure Cosmos DB fiókkal, és rendelkezik a kapcsolati adatok központi telepítése során automatikusan feltöltődik értékkel.

> [!TIP]
> A sablon nem ellenőrzi, hogy a webes alkalmazás neve és az alább megadott Azure Cosmos DB-fiók neve egy) érvényes és b) érhető el.  Erősen ajánlott, hogy ellenőrizze a neveket azt tervezi, hogy az üzemelő példány elküldése előtt adja meg a rendelkezésre állását.
> 
> 

1. Az a [Azure Portal](https://portal.azure.com), kattintson az új, és keresse meg a "Sablon-üzembehelyezés".
    ![A sablon-üzembehelyezés felhasználói felületének képernyőképe](./media/create-website/TemplateDeployment1.png)
2. Válassza ki a sablon telepítési elemet, és kattintson a **létrehozása** ![a sablon-üzembehelyezés felhasználói felületének képernyőképe](./media/create-website/TemplateDeployment2.png)
3. Kattintson a **Szerkesztés sablon**, illessze be a DocDBWebSite.json sablon fájl tartalmát, és kattintson a **mentése**.
   ![A sablon-üzembehelyezés felhasználói felületének képernyőképe](./media/create-website/TemplateDeployment3.png)
4. Kattintson a **paraméterek szerkesztése**, adjon meg értékeket a kötelező paraméterekhez, majd kattintson **OK**.  A paraméterek a következők:
   
   1. SITENAME: Az App Service web app nevét adja meg, és a webes alkalmazás eléréséhez használandó URL-cím használatával (pl. Ha "mydemodocdbwebapp" megad, akkor az URL-címet, amely szerint a web app érik lesz mydemodocdbwebapp.azurewebsites.net).
   2. HOSTINGPLANNAME: A neve az App Service üzemeltetési terv létrehozásához.
   3. HELYE: Adja meg az Azure-hely használandó létrehozni az Azure Cosmos DB és a webes alkalmazás-erőforrásokat.
   4. DATABASEACCOUNTNAME: A neve az Azure Cosmos DB-fiók létrehozásához.   
      
      ![A sablon-üzembehelyezés felhasználói felületének képernyőképe](./media/create-website/TemplateDeployment4.png)
5. Válasszon egy meglévő erőforráscsoportot, vagy adjon meg egy új erőforráscsoport létrehozásához, és az erőforráscsoport helyének kiválasztása.

    ![A sablon-üzembehelyezés felhasználói felületének képernyőképe](./media/create-website/TemplateDeployment5.png)
6. Kattintson a **tekintse át a jogi feltételeket**, **beszerzési**, és kattintson a **létrehozása** a telepítés megkezdéséhez.  Válassza ki **rögzítés az irányítópulton** , az eredményül kapott egyszerűen az Azure portál kezdőlapján látható.
   ![A sablon-üzembehelyezés felhasználói felületének képernyőképe](./media/create-website/TemplateDeployment6.png)
7. A telepítés befejezése után, az erőforráscsoport panel nyílik meg.
   ![Az erőforráscsoport panel képernyőképe](./media/create-website/TemplateDeployment7.png)  
8. A webes alkalmazás-erőforrást az erőforrások listájában kattintson, majd **Alkalmazásbeállítások** ![képernyőfelvétel az erőforráscsoport](./media/create-website/TemplateDeployment9.png)  
9. Vegye figyelembe, hogyan vannak jelen a Azure Cosmos DB végpont és az Azure Cosmos DB főkulcsok Alkalmazásbeállítások.

    ![Alkalmazás-beállításaihoz](./media/create-website/TemplateDeployment10.png)  
10. Nyugodtan fel az Azure-portálon a folytatáshoz, vagy hajtsa végre az Azure Cosmos DB egyik [minták](http://go.microsoft.com/fwlink/?LinkID=402386) a saját Azure Cosmos DB alkalmazás létrehozásához.

<a name="NextSteps"></a>

## <a name="next-steps"></a>Következő lépések
Gratulálunk! Azure Cosmos-adatbázis, az App Service web app és az Azure Resource Manager-sablonok használatával egy minta-webalkalmazáshoz telepítése után.

* Azure Cosmos DB kapcsolatos további tudnivalókért kattintson a [Itt](http://azure.com/docdb).
* Azure App Service Web apps kapcsolatos további tudnivalókért kattintson a [Itt](http://go.microsoft.com/fwlink/?LinkId=325362).
* Azure Resource Manager-sablonokkal kapcsolatos további tudnivalókért kattintson [Itt](https://msdn.microsoft.com/library/azure/dn790549.aspx).

## <a name="whats-changed"></a>A változások
* Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)

> [!NOTE]
> Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](http://go.microsoft.com/fwlink/?LinkId=523751) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban. Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.
> 
> 

