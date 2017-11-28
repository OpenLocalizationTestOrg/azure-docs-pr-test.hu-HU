---
title: "aaaApplication vagy felhasználóspecifikus Marathon-szolgáltatás |} Microsoft Docs"
description: "Alkalmazás- vagy felhasználóspecifikus Marathon-szolgáltatás létrehozása"
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Tárolók, Marathon, mikroszolgáltatások, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/12/2016
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 1e6f69ed64e113a3a059788a71ddb57b6d3ad8da
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-or-user-specific-marathon-service"></a><span data-ttu-id="5ed6f-104">Alkalmazás- vagy felhasználóspecifikus Marathon-szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5ed6f-104">Create an application or user-specific Marathon service</span></span>
<span data-ttu-id="5ed6f-105">Az Azure tárolószolgáltatás előkonfigurált Apache Mesos és Marathon rendszerű főkiszolgálókat biztosít.</span><span class="sxs-lookup"><span data-stu-id="5ed6f-105">Azure Container Service provides a set of master servers on which we preconfigure Apache Mesos and Marathon.</span></span> <span data-ttu-id="5ed6f-106">Ezek lehetnek használt tooorchestrate hello fürt, de az alkalmazások nem toouse hello főkiszolgálók ajánlott erre a célra.</span><span class="sxs-lookup"><span data-stu-id="5ed6f-106">These can be used tooorchestrate your applications on hello cluster, but it's best not toouse hello master servers for this purpose.</span></span> <span data-ttu-id="5ed6f-107">Például Marathon konfigurációjának hello tökéletesítse maguk főkiszolgálók hello való bejelentkezés és igényel módosítása – Ez egyedi főkiszolgálók, kissé eltérő hello standard és a szükséges toobe tartozó és a felügyelt bátorítja egymástól függetlenül.</span><span class="sxs-lookup"><span data-stu-id="5ed6f-107">For example, tweaking hello configuration of Marathon requires logging into hello master servers themselves and making changes--this encourages unique master servers that are a little different from hello standard and need toobe cared for and managed independently.</span></span> <span data-ttu-id="5ed6f-108">Emellett egy csapat által igényelt hello konfigurációs nem feltétlenül hello optimális konfigurációját egy másik.</span><span class="sxs-lookup"><span data-stu-id="5ed6f-108">Additionally, hello configuration required by one team might not be hello optimal configuration for another team.</span></span>

<span data-ttu-id="5ed6f-109">Ez a cikk azt ismertetjük, hogyan tooadd egy alkalmazás- vagy felhasználóspecifikus Marathon-szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="5ed6f-109">In this article, we'll explain how tooadd an application or user-specific Marathon service.</span></span>

<span data-ttu-id="5ed6f-110">Ez a szolgáltatás tooa egyetlen felhasználóhoz vagy csoporthoz fog tartozni, mert azok szabad tooconfigure bármely olyan módon, hogy kívánják azt.</span><span class="sxs-lookup"><span data-stu-id="5ed6f-110">Because this service will belong tooa single user or team, they are free tooconfigure it in any way that they desire.</span></span> <span data-ttu-id="5ed6f-111">Azure Tárolószolgáltatás is, biztosítja, hogy a hello szolgáltatás toorun folytatódik.</span><span class="sxs-lookup"><span data-stu-id="5ed6f-111">Also, Azure Container Service will ensure that hello service continues toorun.</span></span> <span data-ttu-id="5ed6f-112">Hello szolgáltatás sikertelen lesz, ha az Azure Tárolószolgáltatás újraindítja azt.</span><span class="sxs-lookup"><span data-stu-id="5ed6f-112">If hello service fails, Azure Container Service will restart it for you.</span></span> <span data-ttu-id="5ed6f-113">Hello legtöbbször ennek meg nem is lehet észrevenni állásidő.</span><span class="sxs-lookup"><span data-stu-id="5ed6f-113">Most of hello time you won't even notice it had downtime.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5ed6f-114">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="5ed6f-114">Prerequisites</span></span>
<span data-ttu-id="5ed6f-115">[Azure Tárolószolgáltatás-példányt telepítése](container-service-deployment.md) az orchestrator írja be a DC/OS és [győződjön meg arról, hogy az ügyfelek kapcsolódhatnak-e tooyour fürt](../container-service-connect.md).</span><span class="sxs-lookup"><span data-stu-id="5ed6f-115">[Deploy an instance of Azure Container Service](container-service-deployment.md) with orchestrator type DC/OS and  [ensure that your client can connect tooyour cluster](../container-service-connect.md).</span></span> <span data-ttu-id="5ed6f-116">Emellett hello lépéseket követve.</span><span class="sxs-lookup"><span data-stu-id="5ed6f-116">Also, do hello following steps.</span></span>

[!INCLUDE [install hello DC/OS CLI](../../../includes/container-service-install-dcos-cli-include.md)]

## <a name="create-an-application-or-user-specific-marathon-service"></a><span data-ttu-id="5ed6f-117">Alkalmazás- vagy felhasználóspecifikus Marathon-szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="5ed6f-117">Create an application or user-specific Marathon service</span></span>
<span data-ttu-id="5ed6f-118">Először hozzon létre egy JSON-konfigurációs fájlt, amely meghatározza, hogy szeretné-e toocreate hello alkalmazásszolgáltatás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="5ed6f-118">Begin by creating a JSON configuration file that defines hello name of hello application service that you want toocreate.</span></span> <span data-ttu-id="5ed6f-119">Jelen példában használjuk `marathon-alice` hello keretrendszer neveként.</span><span class="sxs-lookup"><span data-stu-id="5ed6f-119">Here we use `marathon-alice` as hello framework name.</span></span> <span data-ttu-id="5ed6f-120">Hello fájl mentése hasonlót `marathon-alice.json`:</span><span class="sxs-lookup"><span data-stu-id="5ed6f-120">Save hello file as something like `marathon-alice.json`:</span></span>

```json
{"marathon": {"framework-name": "marathon-alice" }}
```

<span data-ttu-id="5ed6f-121">A következő használata hello DC/OS parancssori felület tooinstall hello Marathon-példányt a hello beállítások a konfigurációs fájlban:</span><span class="sxs-lookup"><span data-stu-id="5ed6f-121">Next, use hello DC/OS CLI tooinstall hello Marathon instance with hello options that are set in your configuration file:</span></span>

```bash
dcos package install --options=marathon-alice.json marathon
```

<span data-ttu-id="5ed6f-122">Meg kell jelennie a `marathon-alice` szolgáltatás fut a DC/OS felhasználói felületének hello szolgáltatások lapján található.</span><span class="sxs-lookup"><span data-stu-id="5ed6f-122">You should now see your `marathon-alice` service running in hello Services tab of your DC/OS UI.</span></span> <span data-ttu-id="5ed6f-123">hello felhasználói felület lesz `http://<hostname>/service/marathon-alice/` Ha azt szeretné, hogy tooaccess közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="5ed6f-123">hello UI will be `http://<hostname>/service/marathon-alice/` if you want tooaccess it directly.</span></span>

## <a name="set-hello-dcos-cli-tooaccess-hello-service"></a><span data-ttu-id="5ed6f-124">Adja meg a hello DC/OS parancssori felület tooaccess hello szolgáltatást</span><span class="sxs-lookup"><span data-stu-id="5ed6f-124">Set hello DC/OS CLI tooaccess hello service</span></span>
<span data-ttu-id="5ed6f-125">Konfigurálhatja a DC/OS parancssori felület tooaccess ezen új szolgáltatás által hello beállítása `marathon.url` tulajdonság toopoint toohello `marathon-alice` példány az alábbiak szerint:</span><span class="sxs-lookup"><span data-stu-id="5ed6f-125">You can optionally configure your DC/OS CLI tooaccess this new service by setting hello `marathon.url` property toopoint toohello `marathon-alice` instance as follows:</span></span>

```bash
dcos config set marathon.url http://<hostname>/service/marathon-alice/
```

<span data-ttu-id="5ed6f-126">Ellenőrizheti a, amely hello működik a parancssori felület Marathon melyik példányával `dcos config show` parancsot.</span><span class="sxs-lookup"><span data-stu-id="5ed6f-126">You can verify which instance of Marathon that your CLI is working against with hello `dcos config show` command.</span></span> <span data-ttu-id="5ed6f-127">Visszaállíthatja toousing a fő Marathon-szolgáltatás hello paranccsal `dcos config unset marathon.url`.</span><span class="sxs-lookup"><span data-stu-id="5ed6f-127">You can revert toousing your master Marathon service with hello command `dcos config unset marathon.url`.</span></span>

