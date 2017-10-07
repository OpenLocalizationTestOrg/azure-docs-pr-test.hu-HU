---
title: "aaaCreate az első megbízható Azure mikroszolgáltatási Java nyelven |} Microsoft Docs"
description: "Bevezetés toocreating egy Microsoft Azure Service Fabric-alkalmazás az állapotmentes és állapotalapú szolgáltatással."
services: service-fabric
documentationcenter: java
author: vturecek
manager: timlt
editor: 
ms.assetid: 7831886f-7ec4-4aef-95c5-b2469a5b7b5d
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: vturecek
ms.openlocfilehash: 577d96591797bbfe6be5c1094426b5f1435cca0f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-reliable-services"></a>Ismerkedés a Reliable Services használatával
> [!div class="op_single_selector"]
> * [C# Windowson](service-fabric-reliable-services-quick-start.md)
> * [Java Linuxon](service-fabric-reliable-services-quick-start-java.md)
>
>

Ez a cikk ismerteti az Azure Service Fabric Reliable Services hello alapjait, és végigvezeti a létrehozott és telepített egy egyszerű Java nyelven írt megbízható szolgáltatásalkalmazás. A Microsoft Virtual Academy videó azt is bemutatja, hogyan toocreate állapotmentes működnie:<center><a target="_blank" href="https://mva.microsoft.com/en-US/training-courses/building-microservices-applications-on-azure-service-fabric-16747?l=DOX8K86yC_206218965">  
<img src="./media/service-fabric-reliable-services-quick-start-java/ReliableServicesJavaVid.png" WIDTH="360" HEIGHT="244">  
</a></center>

## <a name="installation-and-setup"></a>Telepítés és beállítás
Megkezdése előtt győződjön meg arról, hogy a Service Fabric környezet hello állítsa be a számítógépre.
Ha tooset van szüksége, akkor nyissa meg túl[Mac bevezetés](service-fabric-get-started-mac.md) vagy [Linux első lépések](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Alapfogalmak
csak akkor Reliable Services használatába tooget toounderstand néhány főbb fogalmait és kifejezéseit van szükség:

* **Szolgáltatás típusa**: Ez a szolgáltatás megvalósítása. Hello osztályt írt induló `StatelessService` és bármely más kód vagy függőségek használt, valamint nevét és verziószámát.
* **Szolgáltatáspéldány nevű**: toorun a szolgáltatás létrehozása a szolgáltatás típusának nevesített példányai sokkal például létrehozhat egy osztálytípus objektum példánya. Szolgáltatáspéldány valóban objektum példányának a szolgáltatásosztály írást.
* **Szolgáltatásgazda**: hello szolgáltatáspéldány létrehozása egy gazdagépen belül kell toorun nevű. hello szolgáltatás állomás csak az egyik folyamat, ahol a szolgáltatás példányának futtatható.
* **Eszközregisztrációs szolgáltatás**: regisztráció egyesíti mindent. hello szolgáltatástípus regisztrálva kell lennie a Service Fabric hello futásidejű szolgáltatás a gazdagép tooallow Service Fabric toocreate példányai azt toorun.  

## <a name="create-a-stateless-service"></a>Állapot nélküli szolgáltatás létrehozása
Először hozzon létre egy Service Fabric-alkalmazás. Service Fabric SDK Linux hello tartalmaz egy Yeoman generátor tooprovide hello állványok a Service Fabric-alkalmazás, egy állapot nélküli szolgáltatással. Indítsa el hello futtatásával Yeoman következő parancsot:

```bash
$ yo azuresfjava
```

Kövesse az utasításokat toocreate hello egy **megbízható állapotmentes szolgáltatások**. Ebben az oktatóanyagban neve hello alkalmazás "HelloWorldApplication" és a hello szolgáltatás "HelloWorld". hello eredmény tartalmazza hello könyvtárak `HelloWorldApplication` és `HelloWorld`.

```bash
HelloWorldApplication/
├── build.gradle
├── HelloWorld
│   ├── build.gradle
│   └── src
│       └── statelessservice
│           ├── HelloWorldServiceHost.java
│           └── HelloWorldService.java
├── HelloWorldApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   └── _readme.txt
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="implement-hello-service"></a>Hello szolgáltatás megvalósítása
Nyissa meg **HelloWorldApplication/HelloWorld/src/statelessservice/HelloWorldService.java**. Ez az osztály hello szolgáltatás típusa határozza meg, és semmilyen kódot futtathat. hello szolgáltatás API két belépési pontok biztosít a kódot:

* Egy nyílt belépési pont metódus hívása `runAsync()`, ahol el lehet kezdeni a munkaterheléseket, ideértve a hosszan futó számítási feladatok végrehajtása.

```java
@Override
protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {
    ...
}
```

* Egy kommunikációs belépési pontot, amelyen csatlakoztathatja a kommunikációs verem választott. Ez azért, ahol elindíthatja kéréseket fogad a felhasználók és más szolgáltatások védelmét.

```java
@Override
protected List<ServiceInstanceListener> createServiceInstanceListeners() {
    ...
}
```

Az oktatóanyag azt összpontosítani hello `runAsync()` belépési pont metódusa. Ez azért, ahol azonnal elindíthatja a kódja.

### <a name="runasync"></a>RunAsync
hello platform meghívja ezt a módszert, a szolgáltatás egy példánya esetén elhelyezett és készen állnak a tooexecute. Az állapotmentes szolgáltatások, amely egyszerűen jelenti, hogy hello service-példány már meg van nyitva. A megszakítási jogkivonat van megadva toocoordinate, amikor a szolgáltatáspéldány toobe zárva. A Service Fabric a megnyitása/bezárása ciklus egy szolgáltatáspéldány fordulhatnak elő sokszor élettartama hello hello szolgáltatás egészére. Ez akkor fordulhat elő, beleértve a különböző okokból:

* hello rendszer áthelyezi a szolgáltatáspéldány terheléselosztás erőforrás.
* Hibák merülnek fel, a kódban.
* hello alkalmazás- vagy frissítése.
* hello mögöttes hardver során kimaradás lép fel.

A vezénylési kezeli a Service Fabric tookeep a szolgáltatás magas rendelkezésre állású és megfelelően kiegyensúlyozott.

`runAsync()`meg nem blokkolják szinkron módon történik. RunAsync megvalósítását egy CompletableFuture tooallow hello futásidejű toocontinue kell visszaadnia. Ha a terhelést tooimplement kell végezhető el egy hosszú ideig futó feladat hello CompletableFuture.

#### <a name="cancellation"></a>Megszakítása
A számítási feladatok megszakítását egy együttműködési elérhető cancellation jogkivonat megadott hello által összehangolva. hello rendszer vár a feladat tooend (sikeres befejezése, megszakítása vagy hiba) előtt átvitel során. Fontos toohonor hello cancellation token, Befejezés bármely működik, és zárja be `runAsync()` mihamarabbi Ha hello rendszer törlését kéri. hello következő példa bemutatja, hogyan toohandle egy megszakítási esemény:

```java
    @Override
    protected CompletableFuture<?> runAsync(CancellationToken cancellationToken) {

        // TODO: Replace hello following sample code with your own logic
        // or remove this runAsync override if it's not needed in your service.

        CompletableFuture.runAsync(() -> {
          long iterations = 0;
          while(true)
          {
            cancellationToken.throwIfCancellationRequested();
            logger.log(Level.INFO, "Working-{0}", ++iterations);

            try
            {
              Thread.sleep(1000);
            }
            catch (IOException ex) {}
          }
        });
    }
```

### <a name="service-registration"></a>Eszközregisztrációs szolgáltatás
Szolgáltatástípusok hello Service Fabric-futtatókörnyezet regisztrálva kell lennie. hello szolgáltatás típus van definiálva hello `ServiceManifest.xml` és a szolgáltatás osztály, amely megvalósítja az `StatelessService`. Szolgáltatás regisztrációs hello folyamat fő belépési pont történik. Ebben a példában a fő belépési pont folyamat hello `HelloWorldServiceHost.java`:

```java
public static void main(String[] args) throws Exception {
    try {
        ServiceRuntime.registerStatelessServiceAsync("HelloWorldType", (context) -> new HelloWorldService(), Duration.ofSeconds(10));
        logger.log(Level.INFO, "Registered stateless service type HelloWorldType.");
        Thread.sleep(Long.MAX_VALUE);
    }
    catch (Exception ex) {
        logger.log(Level.SEVERE, "Exception in registration:", ex);
        throw ex;
    }
}
```

## <a name="run-hello-application"></a>Hello alkalmazás futtatása

hello Yeoman állványok magában foglalja a gradle-lel parancsfájl toobuild hello alkalmazás és a bash parancsfájlok toodeploy és az alkalmazás eltávolítása. toorun hello alkalmazás, a gradle-lel első build hello alkalmazás:

```bash
$ gradle
```

Ezzel létrehozza a Service Fabric alkalmazáscsomagok, amelyek a Service Fabric parancssori felület használatával is telepíthető.

### <a name="deploy-with-service-fabric-cli"></a>A Service Fabric parancssori felület telepítése

hello install.sh parancsfájl hello szükséges Service Fabric CLI parancsok toodeploy hello alkalmazáscsomag tartalmazza. Futtassa a install.sh parancsfájl toodeploy hello alkalmazást.

```bash
$ ./install.sh
```

## <a name="next-steps"></a>Következő lépések

* [A Service Fabric parancssori felület használatának első lépései](service-fabric-cli.md)
