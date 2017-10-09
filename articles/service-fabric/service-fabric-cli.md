---
title: "aaaGet lépések az Azure Service Fabric CLI (sfctl)"
description: "Ismerje meg, hogyan toouse hello Azure Service Fabric parancssori felület. Megtudhatja, hogyan tooconnect tooa fürt, és hogyan toomanage alkalmazások."
services: service-fabric
author: samedder
manager: timlt
ms.service: service-fabric
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: edwardsa
ms.openlocfilehash: f76e8ff65bb38dfb63791da0a23e19b93b337f6b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-service-fabric-command-line"></a>Azure Service Fabric-parancssor

hello Azure Service Fabric CLI (sfctl) parancssori segédprogram, amely használják, és kezelése az Azure Service Fabric entitások. Az Sfctl Windows- vagy Linux-fürtökön is használható. Az Sfctl bármilyen platformon fut, amely támogatja a Pythont.

## <a name="prerequisites"></a>Előfeltételek

Előzetes tooinstallation, ellenőrizze, hogy a környezet python és a pip telepítve rendelkezik. További információkért tekintse meg hello [pip gyors üzembe helyezési dokumentációja](https://pip.pypa.io/en/latest/quickstart/), és hivatalos [python telepítése dokumentáció](https://wiki.python.org/moin/BeginnersGuide/Download).

Mindkét python 2.7-es és 3.6 támogatottak, ajánlott toouse python 3.6.

## <a name="install"></a>Telepítés

hello Azure Service Fabric CLI (sfctl) python-csomagként van csomagolva. tooinstall hello legfrissebb verzióját futtassa:

```bash
pip install sfctl
```

A telepítés után futtassa `sfctl -h` elérhető parancsok tooget információt.

## <a name="cli-syntax"></a>A parancssori felület szintaxisa

A parancsok előtt mindig a következő előtag áll: `sfctl`. Az összes használható paranccsal kapcsolatos általános információkért használja az `sfctl -h` parancsot. Ha egyetlen paranccsal kapcsolatban van szüksége segítségre, használja az `sfctl <command> -h` parancsot.

Parancsok hajtsa végre egy ismételhető struktúra, hello hello célja az előző hello műveletet vagy művelet parancsot:

```azurecli
sfctl <object> <action>
```

Ebben a példában `<object>` hello célja a `<action>`.

## <a name="select-a-cluster"></a>Fürt kiválasztása

Mielőtt elvégezné a műveleteket, ki kell választania egy fürt tooconnect számára. Például futtassa a következő tooselect hello és csatlakoztassa a toohello fürtöt hello nevű `testcluster.com`.

> [!WARNING]
> Éles környezetben ne használjon nem védett Service Fabric-fürtöket.

```azurecli
sfctl cluster select --endpoint http://testcluster.com:19080
```

a fürt végpontja hello kell szerepelnie, hogy `http` vagy `https`. Hello port hello HTTP átjáró tartalmaznia kell. hello portok és hello azonos a Service Fabric Explorer URL-cím hello történik.

A tanúsítvánnyal védett fürtök esetében megadhat egy PEM-kódolású tanúsítványt. egyetlen fájl vagy tanúsítvány-és kulcspárt hello tanúsítvány adható meg.

```azurecli
sfctl cluster select --endpoint https://testsecurecluster.com:19080 --pem ./client.pem
```

További információkért lásd: [Connect tooa biztonságos Azure Service Fabric-fürt](service-fabric-connect-to-secure-cluster.md).

## <a name="basic-operations"></a>Alapszintű műveletek

A fürt kapcsolatadatai több sfctl-munkamenetben is megmaradnak. Miután kiválasztotta a Service Fabric-fürt, a Service Fabric-parancs hello fürtön is futtathatja.

Tooget hello Service Fabric-fürt állapotát, például a következő parancs hello használata:

```azurecli
sfctl cluster health
```

hello parancs hello a következő kimenetet eredményezi:

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

Néhány javaslat és tipp a gyakori problémák megoldásához.

### <a name="convert-a-certificate-from-pfx-toopem-format"></a>Alakítsa át a tanúsítvány PFX tooPEM formátumból

Service Fabric CLI hello ügyféloldali tanúsítványokkal PEM (PEM kiterjesztésű) fájlokat támogatja. A Windows a PFX-fájlokkal, ha ezen tanúsítványok tooPEM formátumban kell konvertálnia. tooconvert a PFX fájl tooa PEM-fájl, használja a következő parancsot:

```bash
openssl pkcs12 -in certificate.pfx -out mycert.pem -nodes
```

További információkért lásd: hello [OpenSSL-dokumentáció](https://www.openssl.org/docs/).

### <a name="connection-issues"></a>Kapcsolódási problémák

Egyes műveletek hozhat létre a következő üzenet hello:

`Failed tooestablish a new connection: [Errno 8] nodename nor servname provided, or not known`

Győződjön meg arról, hogy hello megadva, a fürt végpontja rendelkezésre áll és figyelő. Azt is ellenőrizze, hogy hello Service Fabric Explorer felhasználói felület érhető el, amely állomás és port. tooupdate hello végpont használatát `sfctl cluster select`.

### <a name="detailed-logs"></a>Részletes naplók

A részletes naplók gyakran hasznosak a hibák javításához vagy jelentéséhez. Nincs a globális `--debug` jelzőt, amely növeli a naplófájlok hello részletességi.

### <a name="command-help-and-syntax"></a>Parancsok súgója és szintaxisa

Egy adott parancs vagy parancsok egész csoportját, használatáról a hello `-h` jelző:

```azurecli
sfctl application -h
```

Egy másik példa:

```azurecli
sfctl application create -h
```

## <a name="next-steps"></a>Következő lépések

* [Hello Azure Service Fabric CLI az alkalmazás központi telepítését](service-fabric-application-lifecycle-sfctl.md)
* [A Service Fabric használatának első lépései Linuxon](service-fabric-get-started-linux.md)
