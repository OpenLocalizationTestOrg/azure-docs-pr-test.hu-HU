---
title: "egy megbízható Azure Service Fabric-szolgáltatás, a C# aaaCreate"
description: "Azure Service Fabric-alapú Reliable Services-alkalmazás létrehozása, üzembe helyezése és hibakeresése a Visual Studio használatával."
services: service-fabric
documentationcenter: .net
author: rwike77
manager: timlt
editor: vturecek
ms.assetid: c3655b7b-de78-4eac-99eb-012f8e042109
ms.service: service-fabric
ms.devlang: dotNet
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 06/28/2017
ms.author: ryanwi
ms.openlocfilehash: 740c866da6e639219b529fe92ed63cbeaa702a35
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-c-service-fabric-stateful-reliable-services-application"></a>Az első Service Fabric Stateful Reliable Services-alkalmazás létrehozása C#-környezettel

Megtudhatja, hogyan toodeploy az első Service Fabric-alkalmazás a .NET-hez a Windows néhány perc múlva. Ha elkészült, rendelkezni fog egy Reliable Services-alkalmazással futó helyi fürttel.

## <a name="prerequisites"></a>Előfeltételek

Mielőtt elkezdené, győződjön meg arról, hogy [beállította a fejlesztőkörnyezetet](service-fabric-get-started.md). Ez magában foglalja a Service Fabric SDK hello és a Visual Studio 2017 vagy 2015 telepítése.

## <a name="create-hello-application"></a>Hello alkalmazás létrehozása

Indítsa el a Visual Studiót **rendszergazdaként**.

Projekt létrehozása `CTRL`+`SHIFT`+`N` használatával

A hello **új projekt** párbeszédpanelen válasszon **felhő > Service Fabric-alkalmazás**.

Hello alkalmazás neve **MyApplication** nyomja le az ENTER **OK**.

   
![A Visual Studio Új projekt párbeszédpanelje][1]

Service Fabric-alkalmazás bármilyen típusú hello tovább párbeszédpanelről hozhat létre. Ebben a rövid útmutatóban válassza az **Állapotalapú szolgáltatás** lehetőséget.

Hello szolgáltatás **MyStatefulService** nyomja le az ENTER **OK**.

![A Visual Studio Új szolgáltatás párbeszédpanelje][2]


A Visual Studio létrehozza hello projektet és hello állapotalapú szolgáltatási projektet, és megjeleníti őket a Megoldáskezelőben.

![A Megoldáskezelő folytatja az alkalmazás létrehozását állapotalapú szolgáltatással][3]

hello projektet (**MyApplication**) nem tartalmaz közvetlenül semmilyen kódot. Helyette számos szolgáltatási projektre hivatkozik. Ezenfelül három egyéb típusú tartalmat is tartalmaz:

* **Profilok közzététele**  
Profilok üzembe helyezéséhez toodifferent környezetekben.

* **Szkriptek**  
Az alkalmazás üzembe helyezéséhez/frissítéséhez szükséges PowerShell-szkript.

* **Alkalmazásdefiníció**  
Hello ApplicationManifest.xml fájlt tartalmaz *ApplicationPackageRoot* amely ismerteti, hogy az alkalmazás az összeállításban. Kapcsolódó alkalmazásparaméter-fájlokat a rendszer *ApplicationParameters*, amely lehet használt toospecify környezet-specifikus paramétereket. Visual Studio választja ki, egy alkalmazás paraméterfájl hello megadott tartozó közzétételi profil központi telepítési tooa adott környezet során.
    
Hello hello szolgáltatási projekt tartalmának áttekintéséhez lásd: [Bevezetés a Reliable Services használatába](service-fabric-reliable-services-quick-start.md).

## <a name="deploy-and-debug-hello-application"></a>Üzembe helyezése és hibakeresése hello alkalmazás

Most, hogy már van egy alkalmazása, futtassa.

A Visual Studio, nyomja le a `F5` toodeploy hello alkalmazást a hibakereséshez.

>[!NOTE]
>hello első alkalommal futtatja, és hello-alkalmazás központi telepítése helyileg, a Visual Studio hibakeresési egy helyi fürtöt hoz létre. Ez eltarthat egy ideig. hello fürt létrehozási állapota hello Visual Studio kimeneti ablakában jelenik meg.

Amikor készen áll a fürt hello, hello helyi fürt rendszer manager alkalmazást hello SDK részét képező értesítést kap.
   
![A helyifürt-rendszertálca értesítése][4]

Egyszer hello alkalmazás elindul, a Visual Studio automatikusan megjeleníti hello **diagnosztikai eseménynapló**, ahol megtekintheti a nyomkövetési kimeneti a szolgáltatásokból.
   
![Diagnosztikai eseménynapló][5]

hello használtuk állapotalapú Szolgáltatássablon egyszerűen jeleníti meg a számláló értéke növekvő a hello `RunAsync` metódusában **MyStatefulService.cs**.

Bontsa ki az egyik hello események toosee további adatait, többek között a hello kódot futtató hello csomópont. Ebben az esetben ez a \_Node\_2, de az Ön számítógépén ez eltérő lehet.
   
![A diagnosztikai eseménynapló részletei][6]

hello helyi fürt egyetlen számítógépen lévő öt csomópontot tartalmaz. Éles környezetben minden egyes csomópont más fizikai vagy virtuális gépen üzemel. ugyanaz a hello hibakereső toosimulate hello egy gép elvesztését, miközben gyakorló hello Visual Studio idő, vegyük le hello helyi fürtön lévő hello csomópontok egyikét.

A hello **Megoldáskezelőben** ablakban megnyitott **MyStatefulService.cs**. 

Hello található `RunAsync` metódus, és állítsa be töréspont hello hello metódus első sorában.

![Töréspont az állapotalapú szolgáltatás RunAsync metódusában][7]

Indítsa el a hello **Service Fabric Explorer** kattintson a jobb gombbal a hello eszköz **Local Cluster Manager** rendszer alkalmazást, és válassza a **helyi fürt kezelése**.

![Service Fabric Explorer elindításához a Local Cluster Manager hello][systray-launch-sfx]

A [**Service Fabric Explorer**](service-fabric-visualizing-your-cluster.md) vizuálisan is megjeleníti a fürtöket, Ez magában foglalja a központilag telepített alkalmazások tooit hello készlete és az azt alkotó fizikai csomópontok készletét hello.

Hello bal oldali ablaktáblán bontsa ki a **fürt > csomópontok** és a kódot futtató keresés hello csomópont.

Kattintson a **műveletek > inaktiválás (újraindítás)** toosimulate a számítógép-újraindítás.

![Csomópont leállítása a Service Fabric Explorerben][sfx-stop-node]

Pillanatnyilag a töréspont megjelenését megjelennie elérte a Visual Studio, a hello korábban végzett számítása az egyik csomópont zökkenőmentesen átadja a feladatokat tooanother.


Ezután térjen vissza a diagnosztikai eseménynaplóhoz toohello, és tekintse meg az üdvözlő üzenetek. hello számláló értéke továbbra is növekedett, annak ellenére, hogy hello események valójában egy másik csomópont származik.

![A diagnosztikai eseménynapló a feladatátvétel után][diagnostic-events-viewer-detail-post-failover]

## <a name="cleaning-up-hello-local-cluster-optional"></a>Törölje a helyi fürt hello (nem kötelező)

Ne feledje, hogy ez a helyi fürt valós. Hello hibakereső leállítása eltávolítja az alkalmazáspéldány és hello alkalmazástípus regisztrációjának törlése. Azonban hello fürt toorun hello háttérben folytatódik. Amikor készen áll a toostop hello helyi fürthöz, van néhány lehetőség áll.

### <a name="keep-application-and-trace-data"></a>Alkalmazás- és nyomkövetési adatok megtartása

Kattintson a jobb gombbal a hello hello fürt leállítása **Local Cluster Manager** rendszer alkalmazást majd **helyi fürt leállítása**.

### <a name="delete-hello-cluster-and-all-data"></a>Hello fürt és az összes adat törlése

Kattintson a jobb gombbal a hello hello fürt eltávolítása **Local Cluster Manager** rendszer alkalmazást majd **helyi fürt eltávolítása**. 

Ha ezt a lehetőséget választja, a Visual Studio fog telepíteni hello fürt hello a Futtatás hello alkalmazás következő indításakor. Válassza ezt a beállítást, ha egy kis ideig nem szándékozik a toouse hello helyi fürtöt, vagy ha tooreclaim erőforrásokat kell.

## <a name="next-steps"></a>Következő lépések
További információk a [Reliable Services](service-fabric-reliable-services-introduction.md)-szolgáltatásokról.
<!-- Image References -->

[1]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog.png
[2]: ./media/service-fabric-create-your-first-application-in-visual-studio/new-project-dialog-2.png
[3]: ./media/service-fabric-create-your-first-application-in-visual-studio/solution-explorer-stateful-service-template.png
[4]: ./media/service-fabric-create-your-first-application-in-visual-studio/local-cluster-manager-notification.png
[5]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer.png
[6]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail.png
[7]: ./media/service-fabric-create-your-first-application-in-visual-studio/runasync-breakpoint.png
[sfx-stop-node]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-deactivate-node.png
[systray-launch-sfx]: ./media/service-fabric-create-your-first-application-in-visual-studio/launch-sfx.png
[diagnostic-events-viewer-detail-post-failover]: ./media/service-fabric-create-your-first-application-in-visual-studio/diagnostic-events-viewer-detail-post-failover.png
[sfe-delete-application]: ./media/service-fabric-create-your-first-application-in-visual-studio/sfe-delete-application.png
[switch-cluster-mode]: ./media/service-fabric-create-your-first-application-in-visual-studio/switch-cluster-mode.png
[cluster-setup-success-1-node]: ./media/service-fabric-get-started-with-a-local-cluster/cluster-setup-success-1-node.png
