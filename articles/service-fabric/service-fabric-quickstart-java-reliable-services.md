---
title: "Java-alkalmazás létrehozása az Azure Service Fabrickel | Microsoft Docs"
description: "Az Azure platformon futó Java-alkalmazás létrehozása a Service Fabric gyors üzembe helyezési mintájával."
services: service-fabric
documentationcenter: java
author: suhuruli
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: java
ms.topic: quickstart
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 10/23/2017
ms.author: suhuruli
ms.custom: mvc, devcenter
ms.openlocfilehash: aec4db684a9067e1dee424f2c0e05e3674f84d1a
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: HT
ms.contentlocale: hu-HU
ms.lasthandoff: 12/08/2017
---
# <a name="create-a-java-application"></a>Java-alkalmazás létrehozása
Az Azure Service Fabric egy elosztott rendszerplatform, amely mikroszolgáltatások és tárolók üzembe helyezésére és kezelésére szolgál. 

Ez a gyors útmutató azt ismerteti, hogy hogyan helyezheti üzembe első Java-alkalmazását a Service Fabricben az Eclipse IDE használatával, Linux fejlesztői gépen. Az útmutató elvégzése után rendelkezni fog egy Java webes kezelőfelületes szavazóalkalmazással, amely egy a fürtben található állapotalapú háttérszolgáltatásba menti a szavazati adatokat.

![Képernyőkép az alkalmazásról](./media/service-fabric-quickstart-java/votingapp.png)

Ezen rövid útmutató segítségével megtanulhatja a következőket:

> [!div class="checklist"]
> * Az Eclipse használata a Service Fabricben üzemeltetett Java-alkalmazásokhoz
> * Az alkalmazás központi telepítése a helyi fürtre 
> * Az alkalmazás központi telepítése egy fürtre az Azure-ban
> * Az alkalmazás horizontális felskálázása több csomópontra

## <a name="prerequisites"></a>Előfeltételek
A gyorsútmutató elvégzéséhez:
1. [A Service Fabric SDK és a Service Fabric parancssori felület (CLI) telepítése](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#installation-methods)
2. [A Git telepítése](https://git-scm.com/)
3. [Az Eclipse telepítése](https://www.eclipse.org/downloads/)
4. [A Java környezet beállítása](https://docs.microsoft.com/azure/service-fabric/service-fabric-get-started-linux#set-up-java-development) az Eclipse beépülő modul telepítéséhez szükséges választható lépések elvégzésével 

## <a name="download-the-sample"></a>A minta letöltése
Egy parancssori ablakban futtassa a következő parancsot a mintaalkalmazás-adattár helyi számítógépre történő klónozásához.
```
git clone https://github.com/Azure-Samples/service-fabric-java-quickstart.git
```

## <a name="run-the-application-locally"></a>Az alkalmazás helyi futtatása
1. A helyi fürt elindításához futtassa a következő parancsot:

    ```bash
    sudo /opt/microsoft/sdk/servicefabric/common/clustersetup/devclustersetup.sh
    ```
    A helyi fürt elindítása eltarthat egy ideig. A fürt üzembe állásának ellenőrzéséhez nyissa meg a Service Fabric Explorert a **http://localhost:19080** címen. Ha öt kifogástalan csomópontot lát, a fürt megfelelően üzemel. 
    
    ![A helyi fürt kifogástalan](./media/service-fabric-quickstart-java/localclusterup.png)

2. Nyissa meg az Eclipse környezetet.
3. Kattintson a File -> Open Projects from File System... (Fájl -> Projekt megnyitása fájlrendszerből) lehetőségre. 
4. Kattintson a Directory (Címtár) elemre, és válassza ki a `Voting` könyvtárat abból a `service-fabric-java-quickstart` mappából, amelyet a GitHubról klónozott. Kattintson a Befejezés gombra. 

    ![Az Eclipse importálási párbeszédablaka](./media/service-fabric-quickstart-java/eclipseimport.png)
    
5. Ezzel létrehozta a `Voting` projektet az Eclipse-hez készült Package Explorerben. 
6. Kattintson a jobb gombbal a projektre, és a válassza a **Publish Application...** (Alkalmazás közzététele) lehetőséget a **Service Fabric** legördülő menüből. Célprofilnak (Target Profile) válassza a **PublishProfiles/Local.json** lehetőséget, majd kattintson a Publish (Közzététel) gombra. 

    ![Publish (Közzététel) helyi párbeszédpanel](./media/service-fabric-quickstart-java/localjson.png)
    
7. Nyissa meg a kedvenc webböngészőjét, és nyissa meg az alkalmazást a **http://localhost:8080** címen. 

    ![Helyi alkalmazás kezelőfelülete](./media/service-fabric-quickstart-java/runninglocally.png)
    
Most hozzáadhat szavazási lehetőségeket az alkalmazáshoz, és megkezdheti a szavazatok gyűjtését. Az alkalmazás különálló adatbázis nélkül futtatja és tárolja az összes adatot a Service Fabric-fürtben.

## <a name="deploy-the-application-to-azure"></a>Az alkalmazás központi telepítése az Azure-ban

### <a name="set-up-your-azure-service-fabric-cluster"></a>Azure Service Fabric-fürt beállítása
Az alkalmazás Azure-fürtön történő üzembe helyezéséhez hozzon létre egy saját fürtöt, vagy használjon nyilvános fürtöt.

A nyilvános fürtök ingyenes, korlátozott időtartamú Azure Service Fabric-fürtök. Ezeket a Service Fabric csapata üzemelteti, és bárki üzembe helyezhet rajtuk alkalmazásokat, illetve megismerkedhet a platform használatával. A nyilvános fürt eléréséhez [kövesse az alábbi utasításokat](http://aka.ms/tryservicefabric). 

További információk saját fürtök létrehozásáról: [Service Fabric-fürt létrehozása az Azure-on](service-fabric-tutorial-create-vnet-and-linux-cluster.md).

> [!Note]
> A webes kezelőfelület a konfigurációja szerint a 8080-as porton figyeli a bejövő forgalmat. Győződjön meg róla, hogy a port nyitva van a fürtön. Ha a nyilvános fürtöt használja, ez a port nyitva van.
>

### <a name="deploy-the-application-using-eclipse"></a>Az alkalmazás üzembe helyezése az Eclipse használatával
Az alkalmazást és a fürtjét a létrehozása után közvetlenül az Eclipse-ből telepítheti egy fürtre.

1. Nyissa meg a **PublishProfiles** könyvtárban található **Cloud.json** fájlt, és töltse ki megfelelően a `ConnectionIPOrURL` és a `ConnectionPort` mezőket. Például: 

    ```bash
    {
         "ClusterConnectionParameters": 
         {
            "ConnectionIPOrURL": "lnxxug0tlqm5.westus.cloudapp.azure.com",
            "ConnectionPort": "19080",
            "ClientKey": "",
            "ClientCert": ""
         }
    }
    ```

2. Kattintson a jobb gombbal a projektre, és a válassza a **Publish Application...** (Alkalmazás közzététele) lehetőséget a **Service Fabric** legördülő menüből. Célprofilnak (Target Profile) válassza a **PublishProfiles/Cloud.json** lehetőséget, majd kattintson a Publish (Közzététel) gombra. 

    ![A felhő közzétételi párbeszédablaka](./media/service-fabric-quickstart-java/cloudjson.png)

3. Nyissa meg a kedvenc webböngészőjét, és nyissa meg az alkalmazást a **http://\<ConnectionIPOrURL>:8080** címen. 

    ![Felhőalkalmazás kezelőfelülete](./media/service-fabric-quickstart-java/runningcloud.png)
    
## <a name="scale-applications-and-services-in-a-cluster"></a>Alkalmazások és szolgáltatások méretezése a fürtökben
A szolgáltatások skálázhatók egy adott fürtben, hogy kövessék a szolgáltatások terhelésének változásait. A szolgáltatások méretezése a fürtben futó példányok számának módosításával történik. A szolgáltatásokat többféleképpen is méretezheti – használhat szkripteket, vagy a Service Fabric parancssori felület (sfctl) parancsait. Ebben a példában a Service Fabric Explorert használjuk.

A Service Fabric Explorer az összes Service Fabric-fürtben fut. Eléréséhez egy böngészőben navigáljon az adott fürt HTTP-kezelési portjára (19080), például: `http://lnxxug0tlqm5.westus.cloudapp.azure.com:19080`.

A webes előtér-szolgáltatás méretezéséhez hajtsa végre a következő lépéseket:

1. Nyissa meg a Service Fabric Explorert a fürtben – például: `http://lnxxug0tlqm5.westus.cloudapp.azure.com:19080`.
2. Kattintson a három pontra a fanézetben a **fabric:/Voting/VotingWeb** csomópont mellett, és válassza a **Scale Service** (Szolgáltatás méretezése) lehetőséget.

    ![A Service Fabric Explorer méretezési szolgáltatása](./media/service-fabric-quickstart-java/scaleservicejavaquickstart.png)

    Most már méretezheti a webes előtér-szolgáltatás példányainak számát.

3. Módosítsa a számot **2**-re, és kattintson a **Szolgáltatás méretezése** gombra.
4. Kattintson a **fabric:/Voting/VotingWeb** csomópontra a fanézetben, és bontsa ki a partíciós csomópontot (egy GUID jelöli).

    ![A Service Fabric Explorer skálázási szolgáltatása kész](./media/service-fabric-quickstart-java/servicescaled.png)

    Ekkor láthatja, hogy a szolgáltatás két példányban jelenik meg, a fanézetben pedig megtekintheti, hogy a példányok melyik csomópontokon futnak.

Ezzel az egyszerű felügyeleti eljárással megdupláztuk az előtér-szolgáltatás számára a felhasználói terhelések feldolgozásához rendelkezésre álló erőforrások mennyiségét. Fontos megérteni, hogy nincs szükség több példányra ahhoz, hogy a szolgáltatás megbízhatóan fusson. Ha egy szolgáltatás meghibásodik, a Service Fabric gondoskodik róla, hogy egy új szolgáltatáspéldány kezdjen futni a fürtben.

## <a name="next-steps"></a>Következő lépések
Ennek a rövid útmutatónak a segítségével megtanulta a következőket:

> [!div class="checklist"]
> * Az Eclipse használata a Service Fabricben üzemeltetett Java-alkalmazásokhoz
> * Java-alkalmazások központi telepítése a helyi fürtre 
> * Java-alkalmazások központi telepítése egy fürtre az Azure-ban
> * Az alkalmazás horizontális felskálázása több csomópontra

* További információk a [Java-szolgáltatások hibáinak kereséséről az Eclipse használatával](service-fabric-debugging-your-application-java.md)
* További információk a [folyamatos integráció és üzembe helyezés beállításáról a Jenkins használatával](service-fabric-cicd-your-linux-applications-with-jenkins.md)
* Egyéb [Java-minták](https://github.com/Azure-Samples/service-fabric-java-getting-started) megtekintése.