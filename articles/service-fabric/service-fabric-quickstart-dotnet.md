---
title: "a .NET Service Fabric-alkalmazás az Azure-ban aaaCreate |} Microsoft Docs"
description: ".NET-alkalmazás létrehozása az Azure-ban hello Service Fabric gyors üzembe helyezési minta."
services: service-fabric
documentationcenter: .net
author: mikkelhegn
manager: msfussell
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: get-started
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 08/09/2017
ms.author: mikhegn
ms.openlocfilehash: 20ef88dbf9fb0def31234ef05679a19009b9b529
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-net-service-fabric-application-in-azure"></a>.NET Service Fabric-alkalmazás létrehozása az Azure-ban
Az Azure Service Fabric egy elosztott rendszerplatform, amely skálázható és megbízható mikroszolgáltatások és tárolók üzembe helyezésére és kezelésére szolgál. 

A gyors üzembe helyezés bemutatja, hogyan toodeploy az első .NET alkalmazás tooService háló. Amikor végzett, hogy a szavazóalkalmazást az ASP.NET Core webes előtér-egy állapotalapú háttérszolgáltatásnak hello fürt szavazó eredmények takarít meg, amely a.

![Alkalmazás képernyőképe](./media/service-fabric-quickstart-dotnet/application-screenshot.png)

Ezzel az alkalmazással megismerheti, hogyan:
> [!div class="checklist"]
> * Hozzon létre egy alkalmazást a .NET- és a Service Fabric
> * Az ASP.NET core használják egy előtér-webkiszolgáló
> * Az állapotalapú szolgáltatás alkalmazásadatainak tárolására
> * Az alkalmazás helyi hibakeresése
> * Hello alkalmazás tooa fürt az Azure-ban központi telepítése
> * Kibővített hello alkalmazás több csomópont között
> * Alkalmazás a frissítéshez szükséges

## <a name="prerequisites"></a>Előfeltételek
toocomplete a gyors üzembe helyezés:
1. [Telepítse a Visual Studio 2017](https://www.visualstudio.com/) a hello **Azure fejlesztési** és **ASP.NET és a webes fejlesztési** munkaterhelések.
2. [A Git telepítése](https://git-scm.com/)
3. [Telepítse a Microsoft Azure Service Fabric SDK hello](http://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=MicrosoftAzure-ServiceFabric-CoreSDK)
4. Futtassa a következő parancs tooenable Visual Studio toodeploy toohello helyi Service Fabric-fürt hello:
    ```powershell
    Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Force -Scope CurrentUser
    ```

## <a name="download-hello-sample"></a>Hello minta letöltése
A parancs-ablakban futtassa a következő parancs tooclone hello sample app tárház tooyour helyi számítógép hello.
```
git clone https://github.com/Azure-Samples/service-fabric-dotnet-quickstart
```

## <a name="run-hello-application-locally"></a>Hello alkalmazás helyileg történő futtatása
Kattintson a jobb gombbal a Start menü hello hello Visual Studio ikonjára, és válassza a **Futtatás rendszergazdaként**. A sorrend tooattach hello hibakereső tooyour szolgáltatások toorun Visual Studio rendszergazdaként kell.

Nyissa meg hello **Voting.sln** hello klónozott tárház a Visual Studio-megoldáshoz.

toodeploy hello alkalmazás, nyomja meg az **F5**.

> [!NOTE]
> hello első alkalommal futtatja, és telepítse központilag hello alkalmazást, a Visual Studio hibakeresési egy helyi fürtöt hoz létre. Ez a művelet eltarthat egy ideig. hello fürt létrehozási állapota hello Visual Studio kimeneti ablakában jelenik meg.

Hello telepítés befejeződése után elindít egy böngészőt, és nyissa meg az ezen a lapon: `http://localhost:8080` -hello webes előtér-hello alkalmazás.

![Előtér-alkalmazás](./media/service-fabric-quickstart-dotnet/application-screenshot-new.png)

Ezután hozzáadhat egy szavazás beállításainak, és indítsa el a szavazatok véve. hello alkalmazás fut, és az összes adatot tárol a Service Fabric-fürt egy másik adatbázist hello nélkül.

## <a name="walk-through-hello-voting-sample-application"></a>Szavazás mintaalkalmazás hello bízná
hello szavazás alkalmazás két szolgáltatásból áll:
- Webes előtér-szolgáltatás (VotingWeb) – az ASP.NET Core webes előtér-szolgáltatás, hello weblap szolgál, és tesz elérhetővé webes API-k toocommunicate a hello háttérszolgáltatásban.
- Háttér-szolgáltatás (VotingData)-az ASP.NET Core a webszolgáltatást tesz elérhetővé, az API toostore hello szavazattal megbízható dictionary eredményezi a lemezen maradnak.

![Alkalmazásdiagram](./media/service-fabric-quickstart-dotnet/application-diagram.png)

Amikor szavaz a hello alkalmazás hello a következő események következnek be:
1. JavaScript hello webes előtér-szolgáltatás hello szavazattal kérelem toohello webes API-t, egy HTTP PUT-kérelmet küld.

2. hello webes előtér-szolgáltatás egy proxy toolocate használ, és továbbítja a HTTP PUT kérés toohello háttér-szolgáltatásnak.

3. hello háttérszolgáltatásnak hello bejövő kérelem vesz igénybe, és a tárolók hello frissítve megbízható szótár, amely hello fürt csomópontja replikált toomultiple kap, és a lemezen tárolt eredményt. Minden hello alkalmazásadatok hello fürt tárolja, így nincs adatbázis szükséges.

## <a name="debug-in-visual-studio"></a>A Visual Studio hibakeresési
A Visual Studio alkalmazás nyomkövetésére használ egy helyi Service Fabric-fejlesztési fürtöt. A hibakeresési élmény tooyour környezettel rendelkezik hello beállítás tooadjust. Ebben az alkalmazásban tároljuk adatok a háttér-szolgáltatásban egy megbízható szótár használatával. A Visual Studio hello alkalmazás alapértelmezés szerint eltávolítja a hello hibakereső leállítása. Hello adatok hello alkalmazás eltávolítása hatására a hello háttér-szolgáltatás tooalso el kell távolítani. toopersist hello adatok hibakeresés a munkamenetek között, módosíthatja hello **alkalmazás hibakeresési módban** hello meg tulajdonságként **Voting** projektre a Visual Studióban.

toolook, mi történik, a lépéseket követve teljes hello hello kódban:
1. Nyissa meg hello **VotesController.cs** fájlt, és állítson be egy töréspontot hello webes API **Put** metódus (sor: 47) – hello fájlra a Visual Studio Solution Explorer hello kereshet.

2. Nyissa meg hello **VoteDataController.cs** fájlt, és állítson be egy töréspontot ezen webes API **Put** metódus (sor 50).

3. Lépjen vissza toohello böngésző és szavazó lehetőségre, vagy adja hozzá egy új szavazó beállítás. Kattintson az első töréspont hello hello webes első-end api-vezérlőben.
    - Ez azért, ahol hello böngészőjében JavaScript hello küldi hello előtér-szolgáltatás egy kérelem toohello webes API-vezérlőben.
    
    ![Szavazás előtér-szolgáltatás hozzáadása](./media/service-fabric-quickstart-dotnet/addvote-frontend.png)

    - Először a háttér-szolgáltatás hello URL-cím toohello ReverseProxy azt összeállításához **(1)**.
    - Ezt követően kapni hello HTTP PUT kérés toohello ReverseProxy **(2)**.
    - Végül hello a rendszer visszaadja a hello válasz hello háttérszolgáltatásnak toohello ügyfélről **(3)**.

4. Nyomja le az **F5** toocontinue
    - Ekkor a számítógép hello töréspontot hello háttér-szolgáltatás.
    
    ![Szavazás háttér-szolgáltatás hozzáadása](./media/service-fabric-quickstart-dotnet/addvote-backend.png)

    - Hello metódus első sorában hello **(1)** hello használjuk `StateManager` tooget nevű megbízható szótár felvétele vagy `counts`.
    - A megbízható szótárban értékek minden interakció tranzakció, a használatával szükséges utasítás **(2)** hoz létre, hogy a tranzakció.
    - Hello tranzakcióban, majd frissítjük hello megfelelő kulcs a szavazás beállítás hello hello értékét, és véglegesíti hello művelet **(3)**. Hello véglegesítése után metódus ad vissza, hello adatok hello szótárban frissül, és a replikált tooother hello fürt csomópontja. hello adatok immár biztonságosan tárolja hello fürtben, és hello háttérszolgáltatásnak átveheti tooother csomópontok, adatok hello továbbra is fennáll.
5. Nyomja le az **F5** toocontinue

hibakeresési munkamenetben, nyomja meg az toostop hello **Shift + F5**.

## <a name="deploy-hello-application-tooazure"></a>Hello alkalmazás tooAzure telepítése
toodeploy hello alkalmazás tooa fürt az Azure-ban, vagy dönthet úgy toocreate saját fürt, vagy fél fürt használatát.

Entitás fürtök ingyenes, a korlátozott idejű Service Fabric-fürtök Azure-platformon futó, és indítsa el hello Service Fabric team ahol bárki alkalmazások központi telepítése és hello platform megismerése. tooget hozzáférés tooa fél fürt [hello utasítások](http://aka.ms/tryservicefabric). 

További információk saját fürtök létrehozásáról: [Az első saját Service Fabric-fürt létrehozása az Azure-on](service-fabric-get-started-azure-cluster.md).

> [!Note]
> hello webes előtér-szolgáltatás a 8080-as portot a bejövő forgalmat konfigurált toolisten. Győződjön meg róla, hogy a port nyitva van a fürtön. Ha hello fél fürtöt használ, ez a port meg nyitva.
>

### <a name="deploy-hello-application-using-visual-studio"></a>Visual Studio használatával hello alkalmazás központi telepítése
Most, hogy hello alkalmazás készen áll, ezután telepítheti azt közvetlenül a Visual Studio tooa fürt.

1. Kattintson a jobb gombbal **Voting** a hello a Megoldáskezelőben, és válassza a **közzététel**. hello közzététel párbeszédpanel jelenik meg.

    ![Publish (Közzététel) párbeszédpanel](./media/service-fabric-quickstart-dotnet/publish-app.png)

2. A csatlakozási végpont hello típusú hello hello fürt **csatlakozási végpont** mezőben, majd kattintson a **közzététel**. Amikor regisztrál a hello fél fürt, hello csatlakozási végpont hello böngészőben valósul meg. – például `winh1x87d1d.westus.cloudapp.azure.com:19000`.

3. Nyisson meg egy böngészőt, és írja be a fürt címe hello - például `http://winh1x87d1d.westus.cloudapp.azure.com`. Meg kell jelennie az Azure-ban hello fürt hello alkalmazást.

![Előtér-alkalmazás](./media/service-fabric-quickstart-dotnet/application-screenshot-new-azure.png)

## <a name="scale-applications-and-services-in-a-cluster"></a>Alkalmazások és szolgáltatások méretezése a fürtökben
A Service Fabric-szolgáltatások könnyen hello szolgáltatások hello terhelése változás esetén a fürt tooaccommodate között lehet méretezni. A szolgáltatások méretezéséhez a hello fürtben futó példányok hello száma módosításával. A szolgáltatások skálázás több módja van, használhatja a parancsfájlokat vagy parancsokat a PowerShell vagy a Service Fabric CLI (sfctl). A jelen példában használjuk Service Fabric Explorerben talál.

Service Fabric Explorer összes Service Fabric-fürt fut, és keresse meg azt toohello fürtök HTTP felügyeleti portja (19080), például egy böngészőből elérhető `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.

tooscale hello webes előtér-szolgáltatás, a következő lépéseket hello:

1. Nyissa meg a Service Fabric Explorert a fürtben – például: `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.
2. Kattintson a három pont (három ponttal jelölt) következő toohello hello **fabric: / Voting/VotingWeb** csomópont a TreeView vezérlő hello, és válassza a **méretezési szolgáltatás**.

    ![Service Fabric Explorer](./media/service-fabric-quickstart-dotnet/service-fabric-explorer-scale.png)

    Ezután eldöntheti, tooscale hello több példányban hello webes előtér-szolgáltatás.

3. Hello száma túl módosítása**2** kattintson **méretezési szolgáltatás**.
4. Kattintson a hello **fabric: / Voting/VotingWeb** csomópontja fanézetben hello és hello partíció csomópontot (GUID képviseli).

    ![Service Fabric Explorer méretezési szolgáltatás](./media/service-fabric-quickstart-dotnet/service-fabric-explorer-scaled-service.png)

    Láthatja, hogy hello szolgáltatás két példánya van, és hello fanézetben hello példányok futtassa mely csomópontok látható.

Ez egyszerű fájlkezelési feladat által az előtér-szolgáltatás tooprocess felhasználói terhelést azt kettőzni hello forrásanyag is elérhető. Fontos, hogy nem kell több példánya a service toohave megbízhatóan futtatni toounderstand. Ha a szolgáltatás nem sikerül, a Service Fabric biztosítja, hogy hello fürtben fut egy új szolgáltatáspéldány.

## <a name="perform-a-rolling-application-upgrade"></a>Alkalmazás a frissítéshez szükséges
Új frissítések tooyour alkalmazás telepítésekor a Service Fabric bevezeti a hello frissítés biztonságos módon. Működés közbeni frissítés lehetővé teszi az állásidő nélkül hibákról kell és automatikus visszaállítási frissítése közben.

tooupgrade hello alkalmazás, a következő hello:

1. Nyissa meg hello **Index.cshtml** fájlt a Visual Studio - hello fájlra a Visual Studio Solution Explorer hello is kereshet.
2. Néhány szöveget – például hello címsor hello oldalon módosítása
    ```html
        <div class="col-xs-8 col-xs-offset-2 text-center">
            <h2>Service Fabric Voting Sample v2</h2>
        </div>
    ```
3. Hello fájl mentéséhez.
4. Kattintson a jobb gombbal **Voting** a hello a Megoldáskezelőben, és válassza a **közzététel**. hello közzététel párbeszédpanel jelenik meg.
5. Kattintson a hello **Manifest verzió** gomb toochange hello verziója hello szolgáltatást és alkalmazást.
6. Hello verziójának módosítása hello **kód** elem alatt **VotingWebPkg** túl "2.0.0", és kattintson **mentése**.

    ![Módosítási verzió párbeszédpanel](./media/service-fabric-quickstart-dotnet/change-version.png)
7. A hello **Fabric-alkalmazás közzététele** párbeszédpanelt, ellenőrzés hello frissítési hello alkalmazás jelölőnégyzetet, majd **közzététel**.

    ![Közzététele párbeszédpanelen beállítás frissítése](./media/service-fabric-quickstart-dotnet/upgrade-app.png)
8. Nyissa meg a böngészőt, és keresse meg például toohello fürt címe 19080 - porton `http://winh1x87d1d.westus.cloudapp.azure.com:19080`.
9. Kattintson a hello **alkalmazások** hello fanézetben, csomópontot, majd **folyamatban lévő frissítések** hello jobb oldali ablaktáblában. Látni hogyan hello frissítés áthalad hello frissítési tartományt a fürt meggyőződött arról, hogy minden egyes tartományhoz mellett állapota kifogástalan toohello a folytatás előtt.
    ![A Service Fabric Explorerben nézet frissítése](./media/service-fabric-quickstart-dotnet/upgrading.png)

    A Service Fabric teszi frissítések biztonságos hello fürt mindegyik csomópontján hello szolgáltatás a frissítés után két percet várakozik. Hello teljes frissítés tootake körülbelül nyolc percet vár.

10. Hello frissítés közben hello alkalmazás továbbra is használhatja. Hello szolgáltatást futtató hello fürt két példánya van, mert a kérelmek egy részénél kaphat hello alkalmazás, egy frissített verziója, míg mások továbbra is kaphat hello régi verzióját.

## <a name="next-steps"></a>Következő lépések
Ennek a rövid útmutatónak a segítségével megtanulta a következőket:

> [!div class="checklist"]
> * Hozzon létre egy alkalmazást a .NET- és a Service Fabric
> * Az ASP.NET core használják egy előtér-webkiszolgáló
> * Az állapotalapú szolgáltatás alkalmazásadatainak tárolására
> * Az alkalmazás helyi hibakeresése
> * Hello alkalmazás tooa fürt az Azure-ban központi telepítése
> * Kibővített hello alkalmazás több csomópont között
> * Alkalmazás a frissítéshez szükséges

További információ a Service Fabric és a .NET, toolearn vessen egy pillantást az oktatóanyag:
> [!div class="nextstepaction"]
> [A Service Fabric .NET alkalmazás](service-fabric-tutorial-create-dotnet-app.md)