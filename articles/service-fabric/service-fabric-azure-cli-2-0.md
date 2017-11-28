---
title: "Azure Service Fabric és az Azure CLI 2.0-s használatába aaaGet"
description: "Ismerje meg, hogyan toouse hello Azure Service Fabric-parancsok modul Azure parancssori felületen, 2.0-s verziójában. Megtudhatja, hogyan tooconnect tooa fürt, és hogyan toomanage alkalmazások."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 06/21/2017
ms.author: edwardsa
ms.openlocfilehash: ddbd0ef503dd3fff61494cc2cfa7c9a2e8d0a9a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-and-azure-cli-20"></a><span data-ttu-id="91add-104">Az Azure Service Fabric és az Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="91add-104">Azure Service Fabric and Azure CLI 2.0</span></span>

<span data-ttu-id="91add-105">hello Azure parancssori eszköz (Azure CLI) 2.0-s verziójának kezelheti az Azure Service Fabric-fürtök parancsok toohelp tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="91add-105">hello Azure command-line tool (Azure CLI) version 2.0 includes commands toohelp you manage Azure Service Fabric clusters.</span></span> <span data-ttu-id="91add-106">Ismerje meg, hogyan tooget lépések az Azure parancssori felület és a Service Fabric.</span><span class="sxs-lookup"><span data-stu-id="91add-106">Learn how tooget started with Azure CLI and Service Fabric.</span></span>

## <a name="install-azure-cli-20"></a><span data-ttu-id="91add-107">Az Azure CLI 2.0 telepítése</span><span class="sxs-lookup"><span data-stu-id="91add-107">Install Azure CLI 2.0</span></span>

<span data-ttu-id="91add-108">Az Azure CLI 2.0 parancsok toointeract használja, és a Service Fabric-fürtök kezelése.</span><span class="sxs-lookup"><span data-stu-id="91add-108">You can use Azure CLI 2.0 commands toointeract with and manage Service Fabric clusters.</span></span> <span data-ttu-id="91add-109">tooget hello legújabb verziójára Azure CLI használata esetén kövesse hello [Azure CLI 2.0 szabványos telepítési folyamat](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="91add-109">tooget hello latest version of Azure CLI, follow hello [Azure CLI 2.0 standard installation process](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).</span></span>

<span data-ttu-id="91add-110">További információkért lásd: hello [Azure CLI 2.0 áttekintése](https://docs.microsoft.com/en-us/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="91add-110">For more information, see hello [Azure CLI 2.0 overview](https://docs.microsoft.com/en-us/cli/azure/overview).</span></span>

## <a name="azure-cli-syntax"></a><span data-ttu-id="91add-111">Azure CLI-szintaxis</span><span class="sxs-lookup"><span data-stu-id="91add-111">Azure CLI syntax</span></span>

<span data-ttu-id="91add-112">Az Azure CLI-ben minden Service Fabric-parancs `az sf` előtaggal van ellátva.</span><span class="sxs-lookup"><span data-stu-id="91add-112">In Azure CLI, all Service Fabric commands are prefixed with `az sf`.</span></span> <span data-ttu-id="91add-113">Hello parancsokkal kapcsolatos általános információkért használja `az sf -h`.</span><span class="sxs-lookup"><span data-stu-id="91add-113">For general information about hello commands you can use, use `az sf -h`.</span></span> <span data-ttu-id="91add-114">Ha egyetlen paranccsal kapcsolatban van szüksége segítségre, használja az `az sf <command> -h` parancsot.</span><span class="sxs-lookup"><span data-stu-id="91add-114">For help with a single command, use `az sf <command> -h`.</span></span>

<span data-ttu-id="91add-115">Az Azure CLI-ben található Service Fabric-parancsok az alábbi elnevezési mintázatot követik:</span><span class="sxs-lookup"><span data-stu-id="91add-115">Service Fabric commands in Azure CLI follow this naming pattern:</span></span>

```azurecli
az sf <object> <action>
```

<span data-ttu-id="91add-116">`<object>`a hello cél `<action>`.</span><span class="sxs-lookup"><span data-stu-id="91add-116">`<object>` is hello target for `<action>`.</span></span>

## <a name="select-a-cluster"></a><span data-ttu-id="91add-117">Fürt kiválasztása</span><span class="sxs-lookup"><span data-stu-id="91add-117">Select a cluster</span></span>

<span data-ttu-id="91add-118">Mielőtt elvégezné a műveleteket, ki kell választania egy fürt tooconnect számára.</span><span class="sxs-lookup"><span data-stu-id="91add-118">Before you perform any operations, you must select a cluster tooconnect to.</span></span> <span data-ttu-id="91add-119">Egy vonatkozó példáért lásd: a következő kód hello.</span><span class="sxs-lookup"><span data-stu-id="91add-119">For an example, see hello following code.</span></span> <span data-ttu-id="91add-120">hello kód tooan nem biztonságos fürthöz csatlakozik.</span><span class="sxs-lookup"><span data-stu-id="91add-120">hello code connects tooan unsecured cluster.</span></span>

> [!WARNING]
> <span data-ttu-id="91add-121">Éles környezetben ne használjon nem védett Service Fabric-fürtöket.</span><span class="sxs-lookup"><span data-stu-id="91add-121">Do not use unsecured Service Fabric clusters in a production environment.</span></span>

```azurecli
az sf cluster select --endpoint http://testcluster.com:19080
```

<span data-ttu-id="91add-122">a fürt végpontja hello kell szerepelnie, hogy `http` vagy `https`.</span><span class="sxs-lookup"><span data-stu-id="91add-122">hello cluster endpoint must be prefixed by `http` or `https`.</span></span> <span data-ttu-id="91add-123">Hello port hello HTTP átjáró tartalmaznia kell.</span><span class="sxs-lookup"><span data-stu-id="91add-123">It must include hello port for hello HTTP gateway.</span></span> <span data-ttu-id="91add-124">hello portok és hello azonos a Service Fabric Explorer URL-cím hello történik.</span><span class="sxs-lookup"><span data-stu-id="91add-124">hello port and address are hello same as hello Service Fabric Explorer URL.</span></span>

<span data-ttu-id="91add-125">A tanúsítvánnyal védett fürtök esetében nem titkosított .pem, vagy .crt és .key fájlokat használhat.</span><span class="sxs-lookup"><span data-stu-id="91add-125">For clusters that are secured with a certificate, you can use either unencrypted .pem files, or .crt and .key files.</span></span> <span data-ttu-id="91add-126">Példa:</span><span class="sxs-lookup"><span data-stu-id="91add-126">For example:</span></span>

```azurecli
az sf cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

<span data-ttu-id="91add-127">További információkért lásd: [Connect tooa biztonságos Azure Service Fabric-fürt](service-fabric-connect-to-secure-cluster.md).</span><span class="sxs-lookup"><span data-stu-id="91add-127">For more information, see [Connect tooa secure Azure Service Fabric cluster](service-fabric-connect-to-secure-cluster.md).</span></span>

> [!NOTE]
> <span data-ttu-id="91add-128">Hello `select` előtt adja vissza, a parancs nem minden kérésnél működésre.</span><span class="sxs-lookup"><span data-stu-id="91add-128">hello `select` command doesn't act on any requests before it returns.</span></span> <span data-ttu-id="91add-129">hogy a megadott fürt megfelelően tooverify paranccsal például `az sf cluster health`.</span><span class="sxs-lookup"><span data-stu-id="91add-129">tooverify that you've specified a cluster correctly, use a command like `az sf cluster health`.</span></span> <span data-ttu-id="91add-130">Győződjön meg arról, hogy hello parancs nem ad visszatérési ki a hibákat.</span><span class="sxs-lookup"><span data-stu-id="91add-130">Verify that hello command doesn't return any errors.</span></span>

## <a name="basic-operations"></a><span data-ttu-id="91add-131">Alapszintű műveletek</span><span class="sxs-lookup"><span data-stu-id="91add-131">Basic operations</span></span>

<span data-ttu-id="91add-132">A fürt kapcsolatadatai több Azure CLI-munkamenetben is megmaradnak.</span><span class="sxs-lookup"><span data-stu-id="91add-132">Cluster connection information persists across multiple Azure CLI sessions.</span></span> <span data-ttu-id="91add-133">Miután kiválasztotta a Service Fabric-fürt, a Service Fabric-parancs hello fürtön is futtathatja.</span><span class="sxs-lookup"><span data-stu-id="91add-133">After you select a Service Fabric cluster, you can run any Service Fabric command on hello cluster.</span></span>

<span data-ttu-id="91add-134">Tooget hello Service Fabric-fürt állapotát, például a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="91add-134">For example, tooget hello Service Fabric cluster health state, use hello following command:</span></span>

```azurecli
az sf cluster health
```

<span data-ttu-id="91add-135">Hello parancsot (feltéve, hogy JSON-kimenetét hello Azure CLI konfiguráció van megadva) kimeneti hello következő eredményezi:</span><span class="sxs-lookup"><span data-stu-id="91add-135">hello command results in hello following output (assuming that JSON output is specified in hello Azure CLI configuration):</span></span>

```json
{
  "aggregatedHealthState": "Ok",
  "applicationHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "name": "fabric:/System"
    }
  ],
  "healthEvents": [],
  "nodeHealthStates": [
    {
      "aggregatedHealthState": "Ok",
      "id": {
        "id": "66aa824a642124089ee474b398d06a57"
      },
      "name": "_Test_0"
    }
  ],
  "unhealthyEvaluations": []
}
```

## <a name="tips-and-troubleshooting"></a><span data-ttu-id="91add-136">Tippek és hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="91add-136">Tips and troubleshooting</span></span>

<span data-ttu-id="91add-137">Bizonyára hasznosnak találja a következő információ hasznos, ha közben az Azure parancssori felület a Service Fabric-parancsokkal problémákat tapasztal hello.</span><span class="sxs-lookup"><span data-stu-id="91add-137">You might find hello following information helpful if you run into issues while using Service Fabric commands in Azure CLI.</span></span>

### <a name="convert-a-certificate-from-pfx-toopem-format"></a><span data-ttu-id="91add-138">Alakítsa át a tanúsítvány PFX tooPEM formátumból</span><span class="sxs-lookup"><span data-stu-id="91add-138">Convert a certificate from PFX tooPEM format</span></span>

<span data-ttu-id="91add-139">Az Azure CLI PEM- (.pem kiterjesztésű) fájlok formájában támogatja az ügyféloldali tanúsítványokat.</span><span class="sxs-lookup"><span data-stu-id="91add-139">Azure CLI supports client-side certificates as PEM (.pem extension) files.</span></span> <span data-ttu-id="91add-140">A Windows a PFX-fájlokkal, ha ezen tanúsítványok tooPEM formátumban kell konvertálnia.</span><span class="sxs-lookup"><span data-stu-id="91add-140">If you use PFX files from Windows, you must convert those certificates tooPEM format.</span></span> <span data-ttu-id="91add-141">tooconvert a PFX fájl tooa PEM-fájl, a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="91add-141">tooconvert a PFX file tooa PEM file, use hello following command:</span></span>

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

<span data-ttu-id="91add-142">További információkért lásd: hello [OpenSSL-dokumentáció](https://www.openssl.org/docs/).</span><span class="sxs-lookup"><span data-stu-id="91add-142">For more information, see hello [OpenSSL documentation](https://www.openssl.org/docs/).</span></span>

### <a name="connection-issues"></a><span data-ttu-id="91add-143">Kapcsolódási problémák</span><span class="sxs-lookup"><span data-stu-id="91add-143">Connection issues</span></span>

<span data-ttu-id="91add-144">Egyes műveletek hozhat létre a következő üzenet hello:</span><span class="sxs-lookup"><span data-stu-id="91add-144">Some operations might generate hello following message:</span></span>

`Failed tooestablish a new connection: [Errno 8] nodename nor servname provided, or not known`

<span data-ttu-id="91add-145">Győződjön meg arról, hogy hello megadva, a fürt végpontja rendelkezésre áll és figyelő.</span><span class="sxs-lookup"><span data-stu-id="91add-145">Verify that hello specified cluster endpoint is available and listening.</span></span> <span data-ttu-id="91add-146">Azt is ellenőrizze, hogy hello Service Fabric Explorer felhasználói felület érhető el, amely állomás és port.</span><span class="sxs-lookup"><span data-stu-id="91add-146">Also, verify that hello Service Fabric Explorer UI is available at that host and port.</span></span> <span data-ttu-id="91add-147">tooupdate hello végpont használatát `az sf cluster select`.</span><span class="sxs-lookup"><span data-stu-id="91add-147">tooupdate hello endpoint, use `az sf cluster select`.</span></span>

### <a name="detailed-logs"></a><span data-ttu-id="91add-148">Részletes naplók</span><span class="sxs-lookup"><span data-stu-id="91add-148">Detailed logs</span></span>

<span data-ttu-id="91add-149">A részletes naplók gyakran hasznosak a hibák javításához vagy jelentéséhez.</span><span class="sxs-lookup"><span data-stu-id="91add-149">Detailed logs often are helpful when you debug or report an issue.</span></span> <span data-ttu-id="91add-150">Az Azure CLI biztosít a globális `--debug` jelzőt, amely növeli a naplófájlok hello részletességi.</span><span class="sxs-lookup"><span data-stu-id="91add-150">Azure CLI offers a global `--debug` flag that increases hello verbosity of log files.</span></span>

### <a name="command-help-and-syntax"></a><span data-ttu-id="91add-151">Parancsok súgója és szintaxisa</span><span class="sxs-lookup"><span data-stu-id="91add-151">Command help and syntax</span></span>

<span data-ttu-id="91add-152">A Service Fabric parancsok hajtsa végre, az Azure parancssori felület azonos egyezmények hello.</span><span class="sxs-lookup"><span data-stu-id="91add-152">Service Fabric commands follow hello same conventions as Azure CLI.</span></span> <span data-ttu-id="91add-153">Egy adott parancs vagy parancsok egész csoportját, használatáról a hello `-h` jelző:</span><span class="sxs-lookup"><span data-stu-id="91add-153">For help with a specific command or a group of commands, use hello `-h` flag:</span></span>

```azurecli
az sf application -h
```

<span data-ttu-id="91add-154">Egy további példa:</span><span class="sxs-lookup"><span data-stu-id="91add-154">Here's another example:</span></span>

```azurecli
az sf application create -h
```
