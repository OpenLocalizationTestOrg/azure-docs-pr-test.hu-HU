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
# <a name="azure-service-fabric-and-azure-cli-20"></a>Az Azure Service Fabric és az Azure CLI 2.0

hello Azure parancssori eszköz (Azure CLI) 2.0-s verziójának kezelheti az Azure Service Fabric-fürtök parancsok toohelp tartalmazza. Ismerje meg, hogyan tooget lépések az Azure parancssori felület és a Service Fabric.

## <a name="install-azure-cli-20"></a>Az Azure CLI 2.0 telepítése

Az Azure CLI 2.0 parancsok toointeract használja, és a Service Fabric-fürtök kezelése. tooget hello legújabb verziójára Azure CLI használata esetén kövesse hello [Azure CLI 2.0 szabványos telepítési folyamat](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli).

További információkért lásd: hello [Azure CLI 2.0 áttekintése](https://docs.microsoft.com/en-us/cli/azure/overview).

## <a name="azure-cli-syntax"></a>Azure CLI-szintaxis

Az Azure CLI-ben minden Service Fabric-parancs `az sf` előtaggal van ellátva. Hello parancsokkal kapcsolatos általános információkért használja `az sf -h`. Ha egyetlen paranccsal kapcsolatban van szüksége segítségre, használja az `az sf <command> -h` parancsot.

Az Azure CLI-ben található Service Fabric-parancsok az alábbi elnevezési mintázatot követik:

```azurecli
az sf <object> <action>
```

`<object>`a hello cél `<action>`.

## <a name="select-a-cluster"></a>Fürt kiválasztása

Mielőtt elvégezné a műveleteket, ki kell választania egy fürt tooconnect számára. Egy vonatkozó példáért lásd: a következő kód hello. hello kód tooan nem biztonságos fürthöz csatlakozik.

> [!WARNING]
> Éles környezetben ne használjon nem védett Service Fabric-fürtöket.

```azurecli
az sf cluster select --endpoint http://testcluster.com:19080
```

a fürt végpontja hello kell szerepelnie, hogy `http` vagy `https`. Hello port hello HTTP átjáró tartalmaznia kell. hello portok és hello azonos a Service Fabric Explorer URL-cím hello történik.

A tanúsítvánnyal védett fürtök esetében nem titkosított .pem, vagy .crt és .key fájlokat használhat. Példa:

```azurecli
az sf cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

További információkért lásd: [Connect tooa biztonságos Azure Service Fabric-fürt](service-fabric-connect-to-secure-cluster.md).

> [!NOTE]
> Hello `select` előtt adja vissza, a parancs nem minden kérésnél működésre. hogy a megadott fürt megfelelően tooverify paranccsal például `az sf cluster health`. Győződjön meg arról, hogy hello parancs nem ad visszatérési ki a hibákat.

## <a name="basic-operations"></a>Alapszintű műveletek

A fürt kapcsolatadatai több Azure CLI-munkamenetben is megmaradnak. Miután kiválasztotta a Service Fabric-fürt, a Service Fabric-parancs hello fürtön is futtathatja.

Tooget hello Service Fabric-fürt állapotát, például a következő parancs hello használata:

```azurecli
az sf cluster health
```

Hello parancsot (feltéve, hogy JSON-kimenetét hello Azure CLI konfiguráció van megadva) kimeneti hello következő eredményezi:

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

## <a name="tips-and-troubleshooting"></a>Tippek és hibaelhárítás

Bizonyára hasznosnak találja a következő információ hasznos, ha közben az Azure parancssori felület a Service Fabric-parancsokkal problémákat tapasztal hello.

### <a name="convert-a-certificate-from-pfx-toopem-format"></a>Alakítsa át a tanúsítvány PFX tooPEM formátumból

Az Azure CLI PEM- (.pem kiterjesztésű) fájlok formájában támogatja az ügyféloldali tanúsítványokat. A Windows a PFX-fájlokkal, ha ezen tanúsítványok tooPEM formátumban kell konvertálnia. tooconvert a PFX fájl tooa PEM-fájl, a következő parancs hello használata:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

További információkért lásd: hello [OpenSSL-dokumentáció](https://www.openssl.org/docs/).

### <a name="connection-issues"></a>Kapcsolódási problémák

Egyes műveletek hozhat létre a következő üzenet hello:

`Failed tooestablish a new connection: [Errno 8] nodename nor servname provided, or not known`

Győződjön meg arról, hogy hello megadva, a fürt végpontja rendelkezésre áll és figyelő. Azt is ellenőrizze, hogy hello Service Fabric Explorer felhasználói felület érhető el, amely állomás és port. tooupdate hello végpont használatát `az sf cluster select`.

### <a name="detailed-logs"></a>Részletes naplók

A részletes naplók gyakran hasznosak a hibák javításához vagy jelentéséhez. Az Azure CLI biztosít a globális `--debug` jelzőt, amely növeli a naplófájlok hello részletességi.

### <a name="command-help-and-syntax"></a>Parancsok súgója és szintaxisa

A Service Fabric parancsok hajtsa végre, az Azure parancssori felület azonos egyezmények hello. Egy adott parancs vagy parancsok egész csoportját, használatáról a hello `-h` jelző:

```azurecli
az sf application -h
```

Egy további példa:

```azurecli
az sf application create -h
```
