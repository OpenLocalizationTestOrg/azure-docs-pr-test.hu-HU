---
title: "aaaManage Azure DC/OS fürtben a Marathon REST API-hoz |} Microsoft Docs"
description: "Tárolók tooan Azure tároló szolgáltatás DC/OS-fürt üzembe hello Marathon REST API használatával."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Mesos, Azure"
ms.assetid: c7175446-4507-4a33-a7a2-63583e5996e3
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/04/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: d926b9b90f5d4eda85a015d9ea0d96fea2c4b566
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="dcos-container-management-through-hello-marathon-rest-api"></a>A DC/OS hello Marathon REST API-tárolók kezelése
A DC/OS telepítését és skálázását lehetővé fürtözött munkaterhelések, absztrakt módon megjelenítve a mögöttes hardver hello közben környezetet biztosít. A DC/OS fölötti keretrendszer gondoskodik a számítási feladatok ütemezéséről és végrehajtásáról. Bár számos népszerű számítási elérhetők keretrendszerek, ez a dokumentum beolvasása létrehozásán és skálázásán üzemelő tárolópéldányokat a Marathon REST API hello lépésekhez. 

## <a name="prerequisites"></a>Előfeltételek

A példákban szereplő feladatok elvégzéséhez szüksége lesz egy az Azure tárolószolgáltatásban konfigurált DC/OS-fürtre, Meg kell toohave távoli kapcsolatot toothis fürtöt is. További információ ezekről az elemekről tekintse meg a következő cikkek hello:

* [Azure Container Service-fürt üzembe helyezése](container-service-deployment.md)
* [Csatlakozás Azure Tárolószolgáltatás-fürt tooan](../container-service-connect.md)

## <a name="access-hello-dcos-apis"></a>Hozzáférés hello DC/OS API-k
Után csatlakoztatott toohello Azure Tárolószolgáltatási fürthöz, a DC/OS hello és a kapcsolódó REST API-k elérheti http://localhost-porton keresztül. Ebben a dokumentumban a hello példák feltételezik, hogy, hogy az alagutat a 80-as porton. Például hello Marathon végpontok címen érhető el URI-azonosítók kezdve `http://localhost/marathon/v2/`. 

További információ a hello különböző API-k: hello Mesosphere dokumentációjában hello [Marathon API](https://mesosphere.github.io/marathon/docs/rest-api.html) és a [Chronos API](https://mesos.github.io/chronos/docs/api.html), és az Apache dokumentációjában hello [Mesos Scheduler API-RÓL ](http://mesos.apache.org/documentation/latest/scheduler-http-api/).

## <a name="gather-information-from-dcos-and-marathon"></a>Információgyűjtés a DC/OS-ről és a Marathonról
Tárolók toohello DC/OS-fürt központi telepítése érdekében hello DC/OS fürtben, például hello neveket és hello DC/OS-ügynökök állapotának néhány információt gyűjteni. Igen, a lekérdezési hello toodo `master/slaves` hello DC/OS REST API végpontja. Ha minden megfelelően működik, az hello lekérdezés az egyes DC/OS-ügynökök és több tulajdonságok listáját adja vissza.

```bash
curl http://localhost/mesos/master/slaves
```

Most, használja a hello Marathon `/apps` végpont toocheck az aktuális alkalmazás központi telepítések toohello DC/OS-fürtről. Ha ez egy új fürt, akkor az alkalmazásoknál egy üres tömb jelenik meg.

```bash
curl localhost/marathon/v2/apps

{"apps":[]}
```

## <a name="deploy-a-docker-formatted-container"></a>Docker-formázású tároló üzembe helyezése
Docker-formátumú tárolók Marathon REST API hello telepítése hello szánt központi telepítését ismertető JSON-fájl használatával. hello következő minta Nginx tároló tooa titkos ügynököt telepít hello fürtben. 

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 32.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

egy Docker-formázású tároló toodeploy hello JSON fájlt tárolja elérhető helyen. Ezt követően toodeploy hello tároló, futtassa a következő parancs hello. Adja meg hello hello JSON-fájl nevét (`marathon.json` ebben a példában).

```bash
curl -X POST http://localhost/marathon/v2/apps -d @marathon.json -H "Content-type: application/json"
```

hello hasonló toohello következő kimenete:

```json
{"version":"2015-11-20T18:59:00.494Z","deploymentId":"b12f8a73-f56a-4eb1-9375-4ac026d6cdec"}
```

Ha az alkalmazásokat a marathonban, az új alkalmazás megjelenik hello kimeneti.

```bash
curl localhost/marathon/v2/apps
```

## <a name="reach-hello-container"></a>Hello tároló elérése

Ellenőrizheti, hogy Nginx fut a tárolóban lévő titkos ügynökök hello hello fürt egyik hello. toofind hello állomás és port hello tárolót futtató marathonban az éppen futó feladatok hello: 

```bash
curl localhost/marathon/v2/tasks
```

Keresse meg a hello értéket `host` hello kimenet (egy IP-hasonló túl`10.32.0.x`), és hello értékének `ports`.


Egy SSH terminál kapcsolat (ez nem egy bújtatott kapcsolat) toohello felügyeleti FQDN hello fürt ellenőrizze. A csatlakozás után ellenőrizze a következő kérelmet, és a megfelelő értékeket hello hello `host` és `ports`:

```bash
curl http://host:ports
```

hello Nginx server kimenet hasonló toohello következő:

![Nginx-tárolójából.](./media/container-service-mesos-marathon-rest/nginx.png)




## <a name="scale-your-containers"></a>A tárolók skálázása
Alkalmazások központi telepítésének hello Marathon API tooscale kimenő vagy skálája használhatja. Hello előző példában üzembe helyezett egy alkalmazáspéldányt az alkalmazások. Skálázzunk ez toothree alkalmazáspéldányra ki. toodo Igen, egy JSON-fájl létrehozása a következő JSON-szöveg hello használatával, és tárolja elérhető helyen.

```json
{ "instances": 3 }
```

A bújtatott kapcsolat létrehozásakor futtassa a következő parancs tooscale hello alkalmazás kimenő hello.

> [!NOTE]
> hello URI a http://localhost/marathon/v2/apps/ hello alkalmazás tooscale hello azonosítója követ. Ha itt használ hello Nginx mintát, hello URI http://localhost/marathon/v2/apps/nginx lesz.
> 
> 

```bash
curl http://localhost/marathon/v2/apps/nginx -H "Content-type: application/json" -X PUT -d @scale.json
```

Végezetül lekérdezési hello Marathon-végpont alkalmazások. Láthatja majd, hogy most már három Nginx-tároló létezik.

```bash
curl localhost/marathon/v2/apps
```

## <a name="equivalent-powershell-commands"></a>Egyenértékű PowerShell-parancsok
Ugyanezeket a műveleteket elvégezheti Windows rendszerben is a PowerShell-parancsok használatával.

toogather információ hello DC/OS fürtben, mint az ügynökök nevét és állapotát, futtassa a következő parancs hello:

```powershell
Invoke-WebRequest -Uri http://localhost/mesos/master/slaves
```

Docker-formátumú tárolók Marathon telepítése hello szánt központi telepítését ismertető JSON-fájl használatával. hello következő minta telepíti a kötelező hello DC/OS ügynök tooport 80 hello tároló 80-as portja hello Nginx-tároló.

```json
{
  "id": "nginx",
  "cpus": 0.1,
  "mem": 32.0,
  "instances": 1,
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "nginx",
      "network": "BRIDGE",
      "portMappings": [
        { "containerPort": 80, "servicePort": 9000, "protocol": "tcp" }
      ]
    }
  }
}
```

egy Docker-formázású tároló toodeploy hello JSON fájlt tárolja elérhető helyen. Ezt követően toodeploy hello tároló, futtassa a következő parancs hello. Adja meg a hello elérési toohello JSON-fájl (`marathon.json` ebben a példában).

```powershell
Invoke-WebRequest -Method Post -Uri http://localhost/marathon/v2/apps -ContentType application/json -InFile 'c:\marathon.json'
```

Az alkalmazások központi telepítéseit is hello Marathon API tooscale kimenő vagy skálája használható. Hello előző példában üzembe helyezett egy alkalmazáspéldányt az alkalmazások. Skálázzunk ez toothree alkalmazáspéldányra ki. toodo Igen, egy JSON-fájl létrehozása a következő JSON-szöveg hello használatával, és tárolja elérhető helyen.

```json
{ "instances": 3 }
```

Futtassa a következő parancs tooscale hello alkalmazás kimenő hello:

> [!NOTE]
> hello URI a http://localhost/marathon/v2/apps/ hello alkalmazás tooscale hello azonosítója követ. Ha itt használ hello Nginx-mintát, hello URI http://localhost/marathon/v2/apps/nginx lesz.
> 
> 

```powershell
Invoke-WebRequest -Method Put -Uri http://localhost/marathon/v2/apps/nginx -ContentType application/json -InFile 'c:\scale.json'
```

## <a name="next-steps"></a>Következő lépések
* [Tudjon meg többet a Mesos HTTP-végpontokról hello](http://mesos.apache.org/documentation/latest/endpoints/)
* [Tudjon meg többet a Marathon REST API hello](https://mesosphere.github.io/marathon/docs/rest-api.html)

