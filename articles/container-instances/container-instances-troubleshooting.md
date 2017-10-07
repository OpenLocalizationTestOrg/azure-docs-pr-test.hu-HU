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
# <a name="troubleshoot-deployment-issues-with-azure-container-instances"></a><span data-ttu-id="cf440-103">Azure-tároló példányaival telepítési problémák elhárításához</span><span class="sxs-lookup"><span data-stu-id="cf440-103">Troubleshoot deployment issues with Azure Container Instances</span></span>

<span data-ttu-id="cf440-104">Ez a cikk bemutatja, hogyan tootroubleshoot ad ki, ha a tárolók tooAzure tároló példányok telepítése.</span><span class="sxs-lookup"><span data-stu-id="cf440-104">This article shows how tootroubleshoot issues when deploying containers tooAzure Container Instances.</span></span> <span data-ttu-id="cf440-105">Ismerteti néhány hello is futtathatja a gyakori problémákat.</span><span class="sxs-lookup"><span data-stu-id="cf440-105">It also describes some of hello common issues you may run into.</span></span>

## <a name="getting-diagnostic-events"></a><span data-ttu-id="cf440-106">A diagnosztikai beolvasása</span><span class="sxs-lookup"><span data-stu-id="cf440-106">Getting diagnostic events</span></span>

<span data-ttu-id="cf440-107">tooview napló, az alkalmazás kódjában olyan tárolóban, hello használható [az tároló naplók](/cli/azure/container#logs) parancsot.</span><span class="sxs-lookup"><span data-stu-id="cf440-107">tooview logs from your application code within a container, you can use hello [az container logs](/cli/azure/container#logs) command.</span></span> <span data-ttu-id="cf440-108">De ha a tároló telepítése nem sikerült, tooreview hello diagnosztikai adatokat hello Azure tároló példányok erőforrás-szolgáltató által biztosított.</span><span class="sxs-lookup"><span data-stu-id="cf440-108">But if your container does not deploy successfully, you need tooreview hello diagnostic information provided by hello Azure Container Instances resource provider.</span></span> <span data-ttu-id="cf440-109">a tároló, futtassa a következő parancs hello tooview hello eseményeket:</span><span class="sxs-lookup"><span data-stu-id="cf440-109">tooview hello events for your container, run hello following command:</span></span>

```azurecli-interactive
az container show -n mycontainername -g myresourcegroup
```

<span data-ttu-id="cf440-110">hello kimeneti hello core tulajdonságainak a tárolót, és telepítési eseményeket tartalmaz:</span><span class="sxs-lookup"><span data-stu-id="cf440-110">hello output includes hello core properties of your container, along with deployment events:</span></span>

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

## <a name="common-deployment-issues"></a><span data-ttu-id="cf440-111">Gyakori telepítési problémákkal</span><span class="sxs-lookup"><span data-stu-id="cf440-111">Common deployment issues</span></span>

<span data-ttu-id="cf440-112">Van néhány gyakori problémákat fiók a központi telepítésben lévő legtöbb hibákat.</span><span class="sxs-lookup"><span data-stu-id="cf440-112">There are a few common issues that account for most errors in deployment.</span></span>

### <a name="unable-toopull-image"></a><span data-ttu-id="cf440-113">Nem lehet toopull kép</span><span class="sxs-lookup"><span data-stu-id="cf440-113">Unable toopull image</span></span>

<span data-ttu-id="cf440-114">Ha Azure-tároló példányok nem toopull a lemezkép kezdetben újbóli valamennyi ideje előtt esetleg sikertelenek lesznek.</span><span class="sxs-lookup"><span data-stu-id="cf440-114">If Azure Container Instances is unable toopull your image initially, it retries for some period before eventually failing.</span></span> <span data-ttu-id="cf440-115">Ha nem kell húzni hello lemezképet, hello hasonló események jelennek meg:</span><span class="sxs-lookup"><span data-stu-id="cf440-115">If hello image cannot be pulled, events like hello following are shown:</span></span>

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

<span data-ttu-id="cf440-116">tooresolve, hello tároló törlése, és ismételje meg a központi telepítés kifizető nagy figyelmet, hogy helyesen adta-hello kép neve.</span><span class="sxs-lookup"><span data-stu-id="cf440-116">tooresolve, delete hello container and retry your deployment, paying close attention that you have typed hello image name correctly.</span></span>

### <a name="container-continually-exits-and-restarts"></a><span data-ttu-id="cf440-117">Tároló folyamatosan kilép, és újraindul</span><span class="sxs-lookup"><span data-stu-id="cf440-117">Container continually exits and restarts</span></span>

<span data-ttu-id="cf440-118">Jelenleg a Azure tároló példányok csak támogatja a hosszan futó szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="cf440-118">Currently, Azure Container Instances only supports long-running services.</span></span> <span data-ttu-id="cf440-119">Ha a tároló toocompletion és kilépés futtatja, automatikusan újraindul, és ismét elindul.</span><span class="sxs-lookup"><span data-stu-id="cf440-119">If your container runs toocompletion and exits, it automatically restarts and runs again.</span></span> <span data-ttu-id="cf440-120">Ha ez történik, például a következő események jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="cf440-120">If this happens, events like those following are shown.</span></span> <span data-ttu-id="cf440-121">Vegye figyelembe a hello tároló sikeresen elindul, és gyorsan újraindul.</span><span class="sxs-lookup"><span data-stu-id="cf440-121">Note that hello container successfully starts, then quickly restarts.</span></span> <span data-ttu-id="cf440-122">hello tároló példányok API tartalmaz egy `retryCount` tulajdonság, amely bemutatja, hogy hány alkalommal fordult elő az adott tároló újraindult.</span><span class="sxs-lookup"><span data-stu-id="cf440-122">hello Container Instances API includes a `retryCount` property that shows how many times a particular container has restarted.</span></span>

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
> <span data-ttu-id="cf440-123">A legtöbb tároló képek Linux terjesztésekről, bash, például a rendszerhéj hello alapértelmezett parancs állítsa be.</span><span class="sxs-lookup"><span data-stu-id="cf440-123">Most container images for Linux distributions set a shell, such as bash, as hello default command.</span></span> <span data-ttu-id="cf440-124">Mivel a rendszerhéj önmagában nem hosszan futó szolgáltatás, a ezekhez a tárolókhoz azonnal lépjen ki, és újraindítás hurok esnek.</span><span class="sxs-lookup"><span data-stu-id="cf440-124">Since a shell on its own is not a long-running service, these containers immediately exit and fall into a restart loop.</span></span>

### <a name="container-takes-a-long-time-toostart"></a><span data-ttu-id="cf440-125">Tároló egy hosszú ideig toostart vesz igénybe.</span><span class="sxs-lookup"><span data-stu-id="cf440-125">Container takes a long time toostart</span></span>

<span data-ttu-id="cf440-126">Ha a tároló egy hosszú ideig toostart vesz igénybe, de végül sikerül, indítsa el a tároló lemezkép hello mérete alapján.</span><span class="sxs-lookup"><span data-stu-id="cf440-126">If your container takes a long time toostart, but eventually succeeds, start by looking at hello size of your container image.</span></span> <span data-ttu-id="cf440-127">Azure-tároló példányok az igény szerinti kéri le a tároló kép, mert hello indítási tapasztal ideje közvetlenül kapcsolódó tooits méretét.</span><span class="sxs-lookup"><span data-stu-id="cf440-127">Because Azure Container Instances pulls your container image on demand, hello startup time you experience is directly related tooits size.</span></span>

<span data-ttu-id="cf440-128">A tároló lemezkép hello Docker parancssori felület használatával hello méretét tekintheti meg:</span><span class="sxs-lookup"><span data-stu-id="cf440-128">You can view hello size of your container image using hello Docker CLI:</span></span>

```bash
docker images
```

<span data-ttu-id="cf440-129">Kimenet:</span><span class="sxs-lookup"><span data-stu-id="cf440-129">Output:</span></span>

```bash
REPOSITORY                             TAG                 IMAGE ID            CREATED             SIZE
microsoft/aci-helloworld               latest              7f78509b568e        13 days ago         68.1MB
```

<span data-ttu-id="cf440-130">hello kulcs tookeeping lemezkép mérete kisebb annak ellenőrzése, hogy a végső kép nem tartalmaz semmit, amelyre nincs szükség a futási időben.</span><span class="sxs-lookup"><span data-stu-id="cf440-130">hello key tookeeping image sizes small is ensuring that your final image does not contain anything that is not required at runtime.</span></span> <span data-ttu-id="cf440-131">Ez egyirányú toodo [többlépcsős buildek](https://docs.docker.com/engine/userguide/eng-image/multistage-build/).</span><span class="sxs-lookup"><span data-stu-id="cf440-131">One way toodo this is with [multi-stage builds](https://docs.docker.com/engine/userguide/eng-image/multistage-build/).</span></span> <span data-ttu-id="cf440-132">Többlépcsős buildek révén könnyen tooensure hello végső kép tartalmazza a szükséges az alkalmazás csak hello összetevők, és nem bármelyike felesleges hello tartalomtár, amelyet volt szükség összeállítása során.</span><span class="sxs-lookup"><span data-stu-id="cf440-132">Multi-stage builds make it easy tooensure that hello final image contains only hello artifacts you need for your application, and not any of hello extra content that was required at build time.</span></span>

<span data-ttu-id="cf440-133">hello hello kép lekéréses a tároló indítási idő a más módon tooreduce hello hatása toohost hello tároló a képet a hello Azure tároló beállításjegyzék hello toouse Azure tároló példányok célhelyeként ugyanabban a régióban.</span><span class="sxs-lookup"><span data-stu-id="cf440-133">hello other way tooreduce hello impact of hello image pull on your container's startup time is toohost hello container image using hello Azure Container Registry in hello same region where you intend toouse Azure Container Instances.</span></span> <span data-ttu-id="cf440-134">Ez lerövidíti a tároló kép igények tootravel, jelentősen lerövidíteni hello letöltési idő hello hello hálózati elérési útját.</span><span class="sxs-lookup"><span data-stu-id="cf440-134">This shortens hello network path that hello container image needs tootravel, significantly shortening hello download time.</span></span>
