---
title: "aaaQuickly meglévő app tooan Azure Service Fabric-fürt üzembe helyezése"
description: "Az Azure Service Fabric-fürt toohost egy meglévő Node.js alkalmazást használja a Visual Studio."
services: service-fabric
documentationcenter: nodejs
author: thraka
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/13/2017
ms.author: adegeo
ms.openlocfilehash: 20a3eb4a9206ba465acf96d0976ba241b07158bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="host-a-nodejs-application-on-azure-service-fabric"></a>Node.js-alkalmazás üzemeltetése az Azure Service Fabricban

A gyors üzembe helyezés a meglévő alkalmazás (Node.js ebben a példában) tooa Service Fabric-fürt üzembe helyezése az Azure-on futó nyújt segítséget.

## <a name="prerequisites"></a>Előfeltételek

Mielőtt elkezdené, győződjön meg arról, hogy [beállította a fejlesztőkörnyezetet](service-fabric-get-started.md). Amely magában foglalja a Service Fabric SDK hello és a Visual Studio 2017 vagy 2015 telepítése.

Szükség toohave egy meglévő Node.js-alkalmazás központi telepítéshez. Ez a rövid útmutató egy egyszerű Node.js-webhelyet használ, amely [innen][download-sample] tölthető le. Bontsa ki a fájl tooyour `<path-to-project>\ApplicationPackageRoot\<package-name>\Code\` mappa a következő lépésben hello hello projekt létrehozása után.

Ha nem rendelkezik Azure-előfizetéssel, hozzon létre egy [ingyenes fiókot][create-account].

## <a name="create-hello-service"></a>Hello szolgáltatás létrehozása

Indítsa el a Visual Studiót **rendszergazdaként**.

Projekt létrehozása `CTRL`+`SHIFT`+`N` használatával

A hello **új projekt** párbeszédpanelen válasszon **felhő > Service Fabric-alkalmazás**.

Hello alkalmazás neve **MyGuestApp** nyomja le az ENTER **OK**.

>[!IMPORTANT]
>NODE.js könnyen érvénytelenné elérési utak, amelyen a windows hello 260 karakteres korlátot. Használjon például rövid elérési utat maga hello projekt **c:\code\svc1**.
   
![A Visual Studio Új projekt párbeszédpanelje][new-project]

Service Fabric-szolgáltatás bármilyen típusú hello tovább párbeszédpanelről hozhat létre. Ebben a rövid útmutatóban válassza az **Vendég végrehajtható** lehetőséget.

Hello szolgáltatás **MyGuestService** és a jobb oldali toohello hello hello-beállítások megadása a következő értékeket:

| Beállítás                   | Érték |
| ------------------------- | ------ |
| Kódcsomag mappája       | _&lt;hello mappában, amelynek a Node.js-alkalmazás&gt;_ |
| Kódcsomag viselkedése     | Mappa tartalmának tooproject másolása |
| Program                   | node.exe |
| Argumentumok                 | server.js |
| Munkamappa            | CodePackage |

Kattintson az **OK** gombra.

![A Visual Studio Új szolgáltatás párbeszédpanelje][new-service]

A Visual Studio hello alkalmazási projektet és hello szereplő projektet hoz létre, és megjeleníti őket a Megoldáskezelőben.

hello projektet (**MyGuestApp**) nem tartalmaz közvetlenül semmilyen kódot. Helyette számos szolgáltatási projektre hivatkozik. Ezenfelül három egyéb típusú tartalmat is tartalmaz:

* **Profilok közzététele**  
Különböző környezetek eszközbeállításai.

* **Szkriptek**  
Az alkalmazás üzembe helyezéséhez/frissítéséhez szükséges PowerShell-szkript.

* **Alkalmazásdefiníció**  
Magában foglalja az alkalmazásjegyzék hello alatt *ApplicationPackageRoot*. Kapcsolódó alkalmazásparaméter-fájlokat a rendszer *ApplicationParameters*, amely hello alkalmazás meghatározása, és lehetővé teszik tooconfigure kifejezetten az adott környezetben.
    
Hello hello szolgáltatási projekt tartalmának áttekintéséhez lásd: [Bevezetés a Reliable Services használatába](service-fabric-reliable-services-quick-start.md).

## <a name="set-up-networking"></a>A hálózat beállítása

Node.js-alkalmazás telepít azt a portot használja hello példa **80** ezért ellenőriznünk kell, hogy elérhetővé tett port szükséges Service Fabric tootell.

Nyissa meg hello **ServiceManifest.xml** fájl hello projektben. Hello jegyzékfájl hello alján van egy `<Resources> \ <Endpoints>` olyan bejegyzés már meg van adva. Módosítsa az adott bejegyzés tooadd `Port`, `Protocol`, és `Type`. 

```xml
  <Resources>
    <Endpoints>
      <!-- This endpoint is used by hello communication listener tooobtain hello port on which too
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Name="MyGuestAppServiceTypeEndpoint" Port="80" Protocol="http" Type="Input" />
    </Endpoints>
  </Resources>
```

## <a name="deploy-tooazure"></a>TooAzure telepítése

Ha lenyomja az **F5** , és futtassa a hello projektet, telepített toohello helyi fürthöz. Azonban helyezzünk üzembe tooAzure helyette.

Kattintson a jobb gombbal a hello projektet, és válassza a **közzététel...**  amely megnyílik egy párbeszédpanel toopublish tooAzure.

![A service fabric-szolgáltatás tooazure párbeszédpanel közzététele][publish]

Jelölje be hello **PublishProfiles\Cloud.xml** céloz profil.

Ha korábban még nem válasszon egy Azure-fiók toodeploy számára. Ha még nem rendelkezik ilyen fiókkal, [regisztráljon egyet][create-account].

A **csatlakozási végpont**, válassza ki a Service Fabric-fürt toodeploy hello. Ha még nem rendelkezik ilyennel, válassza ki a  **&lt;új fürt létrehozása... &gt;**  amely megnyílik webes böngésző ablak toohello Azure-portálon. További információkért lásd: [hello portálon hozzon létre egy fürtöt](service-fabric-cluster-creation-via-portal.md#create-cluster-in-the-azure-portal). 

Amikor hello Service Fabric-fürtöt hoz létre, győződjön meg arról, hogy tooset hello **egyéni végpontok** beállítás túl**80**.

![Service Fabric-csomóponttípus konfigurálása egyéni végponttal][custom-endpoint]

Bizonyos idő toocomplete egy új Service Fabric-fürt létrehozása vesz igénybe. Azt követően létrehozott, lépjen vissza toohello közzétételére párbeszédpanelt, és válassza ki  **&lt;frissítése&gt;**. hello új fürt szerepel az hello legördülő; Válassza ki azt.

Nyomja le az **közzététel** és hello telepítési toofinish várja.

Ez eltarthat néhány percig. Miután befejeződött, hello alkalmazás toobe teljes rendelkezésre álló néhány percet is eltarthat.

## <a name="test-hello-website"></a>Teszt hello webhely

A szolgáltatás közzététel után tesztelje a szolgáltatást egy webböngészőben. 

Először nyissa meg a hello Azure-portálon, és a Service Fabric-szolgáltatás található.

Hello áttekintése panel hello szolgáltatás címének ellenőrzése. Hello használata hello tartománynévnek _ügyfél-csatlakozási végpont_ tulajdonság. Például: `http://mysvcfab1.westus2.cloudapp.azure.com`.

![Service fabric áttekintése paneljét hello Azure-portálon][overview]

Keresse meg a toothis cím megjelenik hello `HELLO WORLD` választ.

## <a name="delete-hello-cluster"></a>Hello fürt törlése

Ne feledje toodelete minden Ön által létrehozott a gyors üzembe helyezés, ha Ön hello erőforrás van szó, az adott erőforrás.

## <a name="next-steps"></a>Következő lépések
További információk a [futtatható vendégalkalmazásokról](service-fabric-deploy-existing-app.md).

<!-- Image References -->

[new-project]: ./media/quickstart-guest-app/new-project.png
[new-service]: ./media/quickstart-guest-app/template.png
[solution-exp]: ./media/quickstart-guest-app/solution-explorer.png
[publish]: ./media/quickstart-guest-app/publish.png
[overview]: ./media/quickstart-guest-app/overview.png
[custom-endpoint]: ./media/quickstart-guest-app/custom-endpoint.png

[download-sample]: https://github.com/MicrosoftDocs/azure-cloud-services-files/raw/temp/service-fabric-node-website.zip
[create-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F