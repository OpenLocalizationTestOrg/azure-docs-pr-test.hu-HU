---
title: "aaaCreate az első szereplő-alapú Azure mikroszolgáltatási Java nyelven |} Microsoft Docs"
description: "Ez az oktatóanyag végigvezeti hello létrehozása, a Hibakeresés és a Service Fabric Reliable Actors használatával egyszerű szereplő-alapú szolgáltatás telepítése."
services: service-fabric
documentationcenter: .net
author: vturecek
manager: timlt
editor: 
ms.assetid: d31dc8ab-9760-4619-a641-facb8324c759
ms.service: service-fabric
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 01/04/2017
ms.author: vturecek
ms.openlocfilehash: 24718a8d7034360c53597f139169580f1a6ce732
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-reliable-actors"></a>Bevezetés a Reliable Actors
> [!div class="op_single_selector"]
> * [C# Windowson](service-fabric-reliable-actors-get-started.md)
> * [Java Linuxon](service-fabric-reliable-actors-get-started-java.md)
> 
> 

Ez a cikk ismerteti az Azure Service Fabric Reliable Actors hello alapjait, és végigvezeti a létrehozott és telepített egy egyszerű megbízható szereplő alkalmazást Java nyelven.

## <a name="installation-and-setup"></a>Telepítés és beállítás
Megkezdése előtt győződjön meg arról, hogy a Service Fabric környezet hello állítsa be a számítógépre.
Ha tooset van szüksége, akkor nyissa meg túl[Mac bevezetés](service-fabric-get-started-mac.md) vagy [Linux első lépések](service-fabric-get-started-linux.md).

## <a name="basic-concepts"></a>Alapfogalmak
lépések a Reliable Actors, akkor csak tooget toounderstand néhány főbb fogalmait és kifejezéseit van szükség:

* **Aktor szolgáltatás**. Reliable Actors Reliable Services hello Service Fabric infrastruktúra telepített vannak csomagolva. Szereplő példányok a megnevezett példánya. a rendszer aktiválja.
* **Aktor regisztrációs**. Mint a Reliable Services egy megbízható szereplő szolgáltatásnak kell toobe hello Service Fabric-futtatókörnyezet regisztrálva. Emellett hello szereplő típus kell toobe hello szereplő futásidejű regisztrálva.
* **Aktor felület**. hello szereplő objektumfelület használt toodefine egy szereplő egy szigorú típusmegadású nyilvános felületén. Hello szereplő felület hello megbízható szereplő modell terminológia, határozza meg a hello szereplő hello üzenetek típusát megértéséhez, és feldolgozni. más szereplője hello szereplő felületet használja, és ügyfélalkalmazások túl "" (aszinkron küldés) üzenetek toohello szereplő. Reliable Actors több adapter is létrehozható.
* **ActorProxy osztály**. hello ActorProxy osztály ügyfél alkalmazások tooinvoke által használt módszerek hello szereplő felületen keresztül elérhetővé tett hello. hello ActorProxy osztály két fontos funkciók biztosítja:
  
  * Nevek feloldása: képes toolocate hello szereplő hello fürt (hello csomópont keresése hello fürt, ahol tárolása).
  * Kezelési hiba: metódus meghívásához újra és újra oldja fel a hello szereplő helyet után, például hello szereplő toobe igénylő hiba áthelyezését tooanother hello fürt csomópontja.

a következő szabályok, amelyek a projektjükhöz tooactor felületek hello érdemes megemlíteni a következők:

* Aktor a felület metódusai nem túlterhelt.
* Aktor felület metódusok nem lehetnek kimenő, ref vagy választható paraméterek:.
* Általános illesztőfelületeinek nem támogatottak.

## <a name="create-an-actor-service"></a>Aktor szolgáltatás létrehozása
Először hozzon létre egy új Service Fabric-alkalmazás. Service Fabric SDK Linux hello tartalmaz egy Yeoman generátor tooprovide hello állványok a Service Fabric-alkalmazás, egy állapot nélküli szolgáltatással. Indítsa el hello futtatásával Yeoman következő parancsot:

```bash
$ yo azuresfjava
```

Kövesse az utasításokat toocreate hello egy **megbízható szereplő szolgáltatás**. Ebben az oktatóanyagban name hello alkalmazás "HelloWorldActorApplication" és a hello szereplő "HelloWorldActor." a következő állványok hello jön létre:

```bash
HelloWorldActorApplication/
├── build.gradle
├── HelloWorldActor
│   ├── build.gradle
│   ├── settings.gradle
│   └── src
│       └── reliableactor
│           ├── HelloWorldActorHost.java
│           └── HelloWorldActorImpl.java
├── HelloWorldActorApplication
│   ├── ApplicationManifest.xml
│   └── HelloWorldActorPkg
│       ├── Code
│       │   ├── entryPoint.sh
│       │   └── _readme.txt
│       ├── Config
│       │   ├── _readme.txt
│       │   └── Settings.xml
│       ├── Data
│       │   └── _readme.txt
│       └── ServiceManifest.xml
├── HelloWorldActorInterface
│   ├── build.gradle
│   └── src
│       └── reliableactor
│           └── HelloWorldActor.java
├── HelloWorldActorTestClient
│   ├── build.gradle
│   ├── settings.gradle
│   ├── src
│   │   └── reliableactor
│   │       └── test
│   │           └── HelloWorldActorTestClient.java
│   └── testclient.sh
├── install.sh
├── settings.gradle
└── uninstall.sh
```

## <a name="reliable-actors-basic-building-blocks"></a>Megbízható szereplője alapvető építőelemeket, amelyekből
a korábban ismertetett hello alapfogalmait jelenti azt, hogy hello megbízható szereplő szolgáltatás alapszintű építőelemeit.

### <a name="actor-interface"></a>Aktor felület
Ez a hello szereplő hello felület definícióját tartalmazza. Ez az interfész hello szereplő végrehajtása és a hívó hello szereplő, ezért általában teszi azt, amely helyen hello szereplő megvalósítása a külön, és több más megoszthatók logika toodefine hello ügyfelek által közösen használt hello szereplő szerződés meghatározása szolgáltatás vagy az ügyfélalkalmazások számára.

`HelloWorldActorInterface/src/reliableactor/HelloWorldActor.java`:

```java
public interface HelloWorldActor extends Actor {
    @Readonly   
    CompletableFuture<Integer> getCountAsync();

    CompletableFuture<?> setCountAsync(int count);
}
```

### <a name="actor-service"></a>Aktor szolgáltatás
Az aktor végrehajtása és szereplő regisztrációs kódot tartalmaz. hello szereplő osztály megvalósítja hello szereplő felületet. Ez azért, ahol a szereplő legalkalmasabb időpontot.

`HelloWorldActor/src/reliableactor/HelloWorldActorImpl`:

```java
@ActorServiceAttribute(name = "HelloWorldActor.HelloWorldActorService")
@StatePersistenceAttribute(statePersistence = StatePersistence.Persisted)
public class HelloWorldActorImpl extends ReliableActor implements HelloWorldActor {
    Logger logger = Logger.getLogger(this.getClass().getName());

    protected CompletableFuture<?> onActivateAsync() {
        logger.log(Level.INFO, "onActivateAsync");

        return this.stateManager().tryAddStateAsync("count", 0);
    }

    @Override
    public CompletableFuture<Integer> getCountAsync() {
        logger.log(Level.INFO, "Getting current count value");
        return this.stateManager().getStateAsync("count");
    }

    @Override
    public CompletableFuture<?> setCountAsync(int count) {
        logger.log(Level.INFO, "Setting current count value {0}", count);
        return this.stateManager().addOrUpdateStateAsync("count", count, (key, value) -> count > value ? count : value);
    }
}
```

### <a name="actor-registration"></a>Aktor regisztrációs
hello szereplő szolgáltatás regisztrálni kell a Service Fabric-futtatókörnyezet hello szolgáltatás típussal. A sorrendjének hello szereplő szolgáltatás toorun a szereplő példányok, az aktor típusát is regisztrálni kell a hello szereplő szolgáltatás. Hello `ActorRuntime` regisztrációs módszer a szereplőket végrehajtja ezt a műveletet.

`HelloWorldActor/src/reliableactor/HelloWorldActorHost`:

```java
public class HelloWorldActorHost {

    public static void main(String[] args) throws Exception {

        try {
            ActorRuntime.registerActorAsync(HelloWorldActorImpl.class, (context, actorType) -> new ActorServiceImpl(context, actorType, ()-> new HelloWorldActorImpl()), Duration.ofSeconds(10));

            Thread.sleep(Long.MAX_VALUE);

        } catch (Exception e) {
            e.printStackTrace();
            throw e;
        }
    }
}
```

### <a name="test-client"></a>Ügyfél tesztelése
Ez az egyszerű vizsgálati ügyfélalkalmazás külön is futtathatja a Service Fabric-alkalmazás tootest hello az aktor szolgáltatás. Például ha hello ActorProxy használt tooactivate kell és szereplő példányok kommunikálni. Az nincs telepítve, a szolgáltatással.

### <a name="hello-application"></a>hello alkalmazás
Végezetül hello alkalmazáscsomagok hello szereplő szolgáltatás, és előfordulhat, hogy hozzáadja a központi telepítés későbbi hello egyéb szolgáltatásokat. Hello tartalmaz *ApplicationManifest.xml* és a hello szereplő service-csomag helye tulajdonosai.

## <a name="run-hello-application"></a>Hello alkalmazás futtatása

hello Yeoman állványok magában foglalja a gradle-lel parancsfájl toobuild hello alkalmazás és a bash parancsfájlok toodeploy és az alkalmazás eltávolítása. toodeploy hello alkalmazás, a gradle-lel első build hello alkalmazás:

```bash
$ gradle
```

Ez a művelet létrehoz egy Service Fabric-alkalmazáscsomagot, amely Service Fabric parancssori eszközök segítségével telepíthető.

### <a name="deploy-service-fabric-cli"></a>A Service Fabric parancssori felület telepítése

hello install.sh parancsfájl hello szükséges Service Fabric CLI (sfctl) parancsok toodeploy hello alkalmazáscsomag tartalmazza.
Hello install.sh parancsfájl toodeploy hello alkalmazás futtatásához.

```bash
$ ./install.sh
```

## <a name="next-steps"></a>Következő lépések

* [A Service Fabric parancssori felület használatának első lépései](service-fabric-cli.md)
