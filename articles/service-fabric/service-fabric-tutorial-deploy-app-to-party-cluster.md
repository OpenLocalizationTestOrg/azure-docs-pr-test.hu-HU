---
title: "az Azure Service Fabric-alkalmazás tooa fél fürt aaaDeploy |} Microsoft Docs"
description: "Megtudhatja, hogyan toodeploy egy alkalmazás tooa fél fürt."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: mikhegn
ms.openlocfilehash: db16b6418fa2533ed915c8b6e612b7a8e7311bed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-an-application-tooa-party-cluster-in-azure"></a>Egy alkalmazás tooa az Azure-ban fél fürt telepítése
Ez az oktatóanyag két egy sor és bemutatja, hogyan toodeploy egy Azure Service Fabric-alkalmazás tooa fél fürt az Azure-ban.

A második rész hello útmutató-sorozat, megismerheti, hogyan:
> [!div class="checklist"]
> * Fürt üzembe helyezése alkalmazás tooa távoli Visual Studio használatával
> * Az alkalmazás eltávolítása egy fürtről Service Fabric Explorerrel

Az oktatóanyag adatsorozat elsajátíthatja, hogyan:
> [!div class="checklist"]
> * [A .NET Service Fabric-alkalmazás létrehozása](service-fabric-tutorial-create-dotnet-app.md)
> * Hello alkalmazás tooa távoli fürt központi telepítése
> * [Konfigurálja a CI/CD Visual Studio Team Services használatával](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)

## <a name="prerequisites"></a>Előfeltételek
Ez az oktatóanyag elkezdéséhez:
- Ha nem rendelkezik Azure-előfizetéssel, hozzon létre egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- [Telepítse a Visual Studio 2017](https://www.visualstudio.com/) és hello telepítése **Azure fejlesztési** és **ASP.NET és a webes fejlesztési** munkaterhelések.
- [Hello Service Fabric SDK telepítése](service-fabric-get-started.md)

## <a name="download-hello-voting-sample-application"></a>Hello Voting mintaalkalmazás letöltése
Ha Ön nem build hello Voting mintaalkalmazást [rész az oktatóanyag adatsorozat](service-fabric-tutorial-create-dotnet-app.md), tölthető le. A parancs-ablakban futtassa a következő parancs tooclone hello sample app tárház tooyour helyi számítógép hello.

```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="set-up-a-party-cluster"></a>Egy entitás fürt beállítása
Entitás fürtök ingyenes, a korlátozott idejű Service Fabric-fürtök Azure-platformon futó, és indítsa el hello Service Fabric team ahol bárki alkalmazások központi telepítése és hello platform megismerése. Az ingyenes!

tooget hozzáférés tooa fél fürt Tallózás toothis webhely: http://aka.ms/tryservicefabric és követi hello utasításokat tooget hozzáférés tooa fürt. A Facebook-on vagy a Githubon fiók tooget hozzáférés tooa fél fürt van szüksége.

> [!NOTE]
> Entitás fürtök nem biztonságosak, ezért az alkalmazások és azok helyezett adatok látható tooothers lehet. Nem telepítése semmit nem szeretné, hogy mások toosee. A használati feltételek minden hello részletekért meg arról, hogy tooread lehetnek.

## <a name="configure-hello-listening-port"></a>Hello figyelőportja konfigurálása
Hello VotingWeb előtér-szolgáltatás létrehozásakor, a Visual Studio véletlenszerűen választ hello szolgáltatás toolisten port a.  hello VotingWeb szolgáltatás úgy működik, mint az alkalmazás előtér-hello és elfogadja a külső forgalom, így érdemes kötése adott szolgáltatás tooa rögzített és jól ismeri port. A Solution Explorerben nyissa meg a *VotingWeb/PackageRoot/ServiceManifest.xml*.  Hello található **végpont** hello erőforrás **erőforrások** szakaszt, és módosítsa a hello **Port** érték too80.

```xml
<Resources>
    <Endpoints>
      <!-- This endpoint is used by hello communication listener tooobtain hello port on which too
           listen. Please note that if your service is partitioned, this port is shared with 
           replicas of different partitions that are placed in your code. -->
      <Endpoint Protocol="http" Name="ServiceEndpoint" Type="Input" Port="80" />
    </Endpoints>
  </Resources>
```

Hello alkalmazás URL-Címének tulajdonság értéke hello Voting projektben frissíteni, egy webböngésző toohello megfelelő portot nyit meg hibakeresése "F5" használatával.  A Megoldáskezelőben, válassza ki a hello **Voting** projektet és a frissítés hello **alkalmazás URL-Címének** tulajdonság.

![Alkalmazás URL-címe](./media/service-fabric-tutorial-deploy-app-to-party-cluster/application-url.png)

## <a name="deploy-hello-app-toohello-azure"></a>Hello app toohello Azure telepítéséhez
Most, hogy hello alkalmazás készen áll, ezután telepítheti azt közvetlenül a Visual Studio fél fürt toohello.

1. Kattintson a jobb gombbal **Voting** a hello a Megoldáskezelőben, és válassza a **közzététel**.

    ![Publish (Közzététel) párbeszédpanel](./media/service-fabric-tutorial-deploy-app-to-party-cluster/publish-app.png)

2. A csatlakozási végpont hello típusú hello fél fürt a hello **csatlakozási végpont** mezőben, majd kattintson a **közzététel**.

    Miután hello közzététele van kész, meg kell tudni toosend egy kérelem toohello alkalmazást böngésző használatával.

3. Nyissa meg azt az előnyben részesített böngésző és írja be a hello fürt címe (csatlakozási végpont hello hello port adatok, például win1kw5649s.westus.cloudapp.azure.com nélkül).

    Meg kell jelennie hello azonos vezethet, ahogy azt hello alkalmazás a helyi futtatás során.

    ![A fürt API-válaszból](./media/service-fabric-tutorial-deploy-app-to-party-cluster/response-from-cluster.png)

## <a name="remove-hello-application-from-a-cluster-using-service-fabric-explorer"></a>Hello alkalmazás eltávolítása egy fürtről Service Fabric Explorerrel
Service Fabric Explorer egy grafikus felhasználói felület tooexplore, és a Service Fabric-fürt alkalmazásokat kezeléséhez.

hello alkalmazás tooremove hello fél fürt:

1. Keresse meg a Service Fabric Explorer hello fél fürt regisztrációs oldalon által biztosított hello hivatkozással toohello. Például http://win1kw5649s.westus.cloudapp.azure.com:19080/Explorer/index.html.

2. A Service Fabric Explorerben nyissa meg a toohello **fabric://Voting** hello treeview hello bal oldali csomópontjában.

3. Hello kattintson **művelet** gombra a jobb oldali hello **Essentials** ablaktáblán, és válassza a **alkalmazás törlése**. Erősítse meg törlése hello alkalmazáspéldányt, amely eltávolítja a hello fürtben futó alkalmazás hello példányát.

![A Service Fabric Explorerben alkalmazás törlése](./media/service-fabric-tutorial-deploy-app-to-party-cluster/delete-application.png)

## <a name="remove-hello-application-type-from-a-cluster-using-service-fabric-explorer"></a>Hello alkalmazástípus eltávolítása egy fürtről Service Fabric Explorerrel
Alkalmazástípusok a Service Fabric-fürt, amely lehetővé teszi, hogy Ön toohave több példányát és hello alkalmazás hello fürtön belül futó alkalmazások üzemelnek. Miután eltávolította az alkalmazás példányát futtató hello, azt is eltávolíthatja hello típusa, toocomplete hello tisztítás hello központi telepítés.

A Service Fabric hello alkalmazásmodell kapcsolatos további információkért lásd: [egy alkalmazás a Service Fabric modell](service-fabric-application-model.md).

1. Keresse meg a toohello **VotingType** hello treeview csomópontja.

2. Hello kattintson **művelet** gombra a jobb oldali hello **Essentials** ablaktáblán, és válassza a **Unprovision típus**. Erősítse meg leépítése hello alkalmazás típusa.

![A Service Fabric Explorerben alkalmazástípus leépíteni a következőt:](./media/service-fabric-tutorial-deploy-app-to-party-cluster/unprovision-type.png)

Ennyi lenne hello oktatóanyag.

## <a name="next-steps"></a>Következő lépések
Ez az oktatóanyag bemutatta, hogyan végezheti el az alábbi műveleteket:

> [!div class="checklist"]
> * Fürt üzembe helyezése alkalmazás tooa távoli Visual Studio használatával
> * Az alkalmazás eltávolítása egy fürtről Service Fabric Explorerrel

Előzetes toohello következő oktatóanyaga:
> [!div class="nextstepaction"]
> [Beállíthat folyamatos integrációt, a Visual Studio Team Services használatával](service-fabric-tutorial-deploy-app-with-cicd-vsts.md)