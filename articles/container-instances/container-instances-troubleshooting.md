---
title: "Azure-tároló példányok aaaTroubleshooting"
description: "Ismerje meg, hogyan tootroubleshoot Azure tároló osztályt problémák"
services: container-instances
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/03/2017
ms.author: seanmck
ms.custom: mvc
ms.openlocfilehash: dfec636a0a174c74a6f2e9d9c4da6e871f8d2fda
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a>Azure-tároló példányaival telepítési problémák elhárításához

Ez a cikk bemutatja, hogyan tootroubleshoot ad ki, ha a tárolók tooAzure tároló példányok telepítése. Ismerteti néhány hello is futtathatja a gyakori problémákat.

## <a name="getting-diagnostic-events"></a>A diagnosztikai beolvasása

tooview napló, az alkalmazás kódjában olyan tárolóban, hello használható [az tároló naplók](/cli/azure/container#logs) parancsot. De ha a tároló telepítése nem sikerült, tooreview hello diagnosztikai adatokat hello Azure tároló példányok erőforrás-szolgáltató által biztosított. a tároló, futtassa a következő parancs hello tooview hello eseményeket:

```azurecli-interactive
az container show -n mycontainername -g myresourcegroup
```

hello kimeneti hello core tulajdonságainak a tárolót, és telepítési eseményeket tartalmaz:

```bash
{
  "containers": [
    {
      "command": null,
      "environmentVariables": [],
      "image": "microsoft/aci-helloworld",
      ...

      "events": [
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:52+00:00",
        "lastTimestamp": "2017-08-03T22:12:52+00:00",
        "message": "Pulling: pulling image \"microsoft/aci-helloworld\"",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Pulled: Successfully pulled image \"microsoft/aci-helloworld\"",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Created: Created container with id 61602059d6c31529c27609ef4ec0c858b0a96150177fa045cf944d7cf8fbab69",
        "type": "Normal"
      },
      {
        "count": 1,
        "firstTimestamp": "2017-08-03T22:12:55+00:00",
        "lastTimestamp": "2017-08-03T22:12:55+00:00",
        "message": "Started: Started container with id 61602059d6c31529c27609ef4ec0c858b0a96150177fa045cf944d7cf8fbab69",
        "type": "Normal"
      }
    ],
    "name": "helloworld",
      "ports": [
        {
          "port": 80
        }
      ],
    ...
  ]
}
```

## <a name="common-deployment-issues"></a>Gyakori telepítési problémákkal

Van néhány gyakori problémákat fiók a központi telepítésben lévő legtöbb hibákat.

### <a name="unable-toopull-image"></a>Nem lehet toopull kép

Ha Azure-tároló példányok nem toopull a lemezkép kezdetben újbóli valamennyi ideje előtt esetleg sikertelenek lesznek. Ha nem kell húzni hello lemezképet, hello hasonló események jelennek meg:

```bash
"events": [
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:31+00:00",
    "lastTimestamp": "2017-08-03T22:19:31+00:00",
    "message": "Pulling: pulling image \"microsoft/aci-hellowrld\"",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:32+00:00",
    "lastTimestamp": "2017-08-03T22:19:32+00:00",
    "message": "Failed: Failed toopull image \"microsoft/aci-hellowrld\": rpc error: code 2 desc Error: image microsoft/aci-hellowrld:latest not found",
    "type": "Warning"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:19:33+00:00",
    "lastTimestamp": "2017-08-03T22:19:33+00:00",
    "message": "BackOff: Back-off pulling image \"microsoft/aci-hellowrld\"",
    "type": "Normal"
  }
]
```

tooresolve, hello tároló törlése, és ismételje meg a központi telepítés kifizető nagy figyelmet, hogy helyesen adta-hello kép neve.

### <a name="container-continually-exits-and-restarts"></a>Tároló folyamatosan kilép, és újraindul

Jelenleg a Azure tároló példányok csak támogatja a hosszan futó szolgáltatásokat. Ha a tároló toocompletion és kilépés futtatja, automatikusan újraindul, és ismét elindul. Ha ez történik, például a következő események jelennek meg. Vegye figyelembe a hello tároló sikeresen elindul, és gyorsan újraindul. hello tároló példányok API tartalmaz egy `retryCount` tulajdonság, amely bemutatja, hogy hány alkalommal fordult elő az adott tároló újraindult.

```bash
"events": [
  {
    "count": 5,
    "firstTimestamp": "2017-08-03T22:21:55+00:00",
    "lastTimestamp": "2017-08-03T22:23:22+00:00",
    "message": "Pulling: pulling image \"alpine\"",
    "type": "Normal"
  },
  {
    "count": 5,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:23:23+00:00",
    "message": "Pulled: Successfully pulled image \"alpine\"",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:21:57+00:00",
    "message": "Created: Created container with id ad2bf9bc51761c5f935260b4bab53b164d52d9cbc045b16afcb26fb4d14d0a70",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:57+00:00",
    "lastTimestamp": "2017-08-03T22:21:57+00:00",
    "message": "Started: Started container with id ad2bf9bc51761c5f935260b4bab53b164d52d9cbc045b16afcb26fb4d14d0a70",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:58+00:00",
    "lastTimestamp": "2017-08-03T22:21:58+00:00",
    "message": "Created: Created container with id 7687b9bd15dc01731fa66fc45f6f0241495600602dd03841e559453245e7f70b",
    "type": "Normal"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:21:58+00:00",
    "lastTimestamp": "2017-08-03T22:21:58+00:00",
    "message": "Started: Started container with id 7687b9bd15dc01731fa66fc45f6f0241495600602dd03841e559453245e7f70b",
    "type": "Normal"
  },
  {
    "count": 13,
    "firstTimestamp": "2017-08-03T22:21:59+00:00",
    "lastTimestamp": "2017-08-03T22:24:36+00:00",
    "message": "BackOff: Back-off restarting failed container",
    "type": "Warning"
  },
  {
    "count": 1,
    "firstTimestamp": "2017-08-03T22:22:13+00:00",
    "lastTimestamp": "2017-08-03T22:22:13+00:00",
    "message": "Created: Created container with id 72e347e891290e238135e4a6b3078748ca25a1275dbbff30d8d214f026d89220",
    "type": "Normal"
  },
  ...
```

> [!NOTE]
> A legtöbb tároló képek Linux terjesztésekről, bash, például a rendszerhéj hello alapértelmezett parancs állítsa be. Mivel a rendszerhéj önmagában nem hosszan futó szolgáltatás, a ezekhez a tárolókhoz azonnal lépjen ki, és újraindítás hurok esnek.

### <a name="container-takes-a-long-time-toostart"></a>Tároló egy hosszú ideig toostart vesz igénybe.

Ha a tároló egy hosszú ideig toostart vesz igénybe, de végül sikerül, indítsa el a tároló lemezkép hello mérete alapján. Azure-tároló példányok az igény szerinti kéri le a tároló kép, mert hello indítási tapasztal ideje közvetlenül kapcsolódó tooits méretét.

A tároló lemezkép hello Docker parancssori felület használatával hello méretét tekintheti meg:

```bash
docker images
```

Kimenet:

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
microsoft/aci-helloworld               latest              7f78509b568e        13 days ago         68.1MB
```

hello kulcs tookeeping lemezkép mérete kisebb annak ellenőrzése, hogy a végső kép nem tartalmaz semmit, amelyre nincs szükség a futási időben. Ez egyirányú toodo [többlépcsős buildek](https://docs.docker.com/engine/userguide/eng-image/multistage-build/). Többlépcsős buildek révén könnyen tooensure hello végső kép tartalmazza a szükséges az alkalmazás csak hello összetevők, és nem bármelyike felesleges hello tartalomtár, amelyet volt szükség összeállítása során.

hello hello kép lekéréses a tároló indítási idő a más módon tooreduce hello hatása toohost hello tároló a képet a hello Azure tároló beállításjegyzék hello toouse Azure tároló példányok célhelyeként ugyanabban a régióban. Ez lerövidíti a tároló kép igények tootravel, jelentősen lerövidíteni hello letöltési idő hello hello hálózati elérési útját.
