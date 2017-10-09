---
title: "Azure DC/OS-fürtről a aaaLoad egyenleg tárolók |} Microsoft Docs"
description: "Egy Azure tároló szolgáltatás DC/OS-fürtben több tároló terheléselosztása."
services: container-service
documentationcenter: 
author: rgardler
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Tárolók, mikroszolgáltatások, DC/OS, Azure"
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/02/2017
ms.author: rogardle
ms.custom: mvc
ms.openlocfilehash: 2249cb06880cdb7e9a3aa94c0750c6a27316d349
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-containers-in-an-azure-container-service-dcos-cluster"></a>Betöltési egyenleg tárolók egy Azure tároló szolgáltatás DC/OS-fürtben
Ez a cikk azt megismerkedhet toocreate belső terheléselosztót a DC/OS módjára vonatkozik. az Azure Tárolószolgáltatás Marathon-LB használatával. Ez a konfiguráció lehetővé teszi, hogy Ön tooscale az alkalmazások vízszintesen. Azt is lehetővé teszi hello nyilvános és titkos ügynök fürtök tootake előnyeit úgy, hogy a terheléselosztó hello nyilvános és titkos fürtön hello alkalmazás tárolók skálázása. Ebben az oktatóanyagban az alábbiakat végezte el:

> [!div class="checklist"]
> * A Marathon terheléselosztó konfigurálása
> * Hello terheléselosztót használó alkalmazások központi telepítése
> * Konfigurálja és Azure terheléselosztó

Az ACS a DC/OS fürtben toocomplete hello lépések ebben az oktatóanyagban van szüksége. Ha szükséges, [a parancsfájl minta](./../kubernetes/scripts/container-service-cli-deploy-dcos.md) hozhat létre egyet.

Ez az oktatóanyag szükséges hello Azure CLI 2.0.4 verzió vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooupgrade van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="load-balancing-overview"></a>Terheléselosztás – áttekintés

Az alábbi két terheléselosztási réteg Azure tároló szolgáltatás DC/OS-fürtben lévő: 

**Az Azure Load Balancer** biztosít a nyilvános belépési pontok (hello azokat, a végfelhasználók számára a hozzáférést). Az Azure LB automatikusan Azure tárolószolgáltatáson, és, alapértelmezés szerint beállított tooexpose port a 80-as, a 443-as és a 8080-as.

**hello Marathon terheléselosztó (marathon-lb)** útvonalak bejövő kérelmek toocontainer példányai, amelyeket a szolgáltatás ilyen kérelmeket. A webszolgáltatás nyújtó hello tárolók skálázásához, hello marathon-lb dinamikusan alkalmazkodik. Ez a terheléselosztó nem szerepel a Tárolószolgáltatás alapértelmezés szerint, de egyszerű tooinstall.

## <a name="configure-marathon-load-balancer"></a>A Marathon terheléselosztó konfigurálása

A Marathon terheléselosztó dinamikusan újrakonfigurálása telepítése után hello tárolókat alapján. Is egy tároló vagy ügynök - rugalmas toohello elvesztését, ha ez történik, Apache Mesos újraindul hello tárolót máshol, és a marathon-lb alkalmazkodik.

Futtassa a parancsot tooinstall hello marathon terheléselosztó hello nyilvános ügynök fürtön a következő hello.

```azurecli-interactive
dcos package install marathon-lb
```

## <a name="deploy-load-balanced-application"></a>Elosztott terhelésű alkalmazás központi telepítése

Most, hogy hello marathon-lb csomag, telepíthetünk egy alkalmazás-tárolót, hogy jelenítsük tooload egyensúlyára. 

Szereznie hello nyilvánosan elérhetővé tett hello ügynökök teljes Tartományneve.

```azurecli-interactive
az acs list --resource-group myResourceGroup --query "[0].agentPoolProfiles[0].fqdn" --output tsv
```

Következő lépésként hozzon létre egy fájlt *hello-web.json* és hello példányának a következő tartalmát. Hello `HAPROXY_0_VHOST` címke kell toobe hello hello DC/OS-ügynökök teljes Tartományneve frissül. 

```json
{
  "id": "web",
  "container": {
    "type": "DOCKER",
    "docker": {
      "image": "yeasy/simple-web",
      "network": "BRIDGE",
      "portMappings": [
        { "hostPort": 0, "containerPort": 80, "servicePort": 10000 }
      ],
      "forcePullImage":true
    }
  },
  "instances": 3,
  "cpus": 0.1,
  "mem": 65,
  "healthChecks": [{
      "protocol": "HTTP",
      "path": "/",
      "portIndex": 0,
      "timeoutSeconds": 10,
      "gracePeriodSeconds": 10,
      "intervalSeconds": 2,
      "maxConsecutiveFailures": 10
  }],
  "labels":{
    "HAPROXY_GROUP":"external",
    "HAPROXY_0_VHOST":"YOUR FQDN",
    "HAPROXY_0_MODE":"http"
  }
}
```

Hello DC/OS parancssori felület toorun hello alkalmazás használja. Alapértelmezés szerint a Marathon hello hello alkalmazástelepítések toohello titkos fürt központilag telepíti. Ez azt jelenti, hogy a fenti telepítési hello csak keresztül érhető a terheléselosztó, amely általában a szükséges hello működés.

```azurecli-interactive
dcos marathon app add hello-web.json
```

Miután hello alkalmazás telepítve van, keresse meg a toohello hello ügynök fürt tooview elosztott terhelésű alkalmazások teljes Tartományneve.

![Elosztott terhelésű alkalmazások képe](./media/container-service-load-balancing/lb-app.png)

## <a name="configure-azure-load-balancer"></a>Az Azure terheléselosztó konfigurálása

Alapértelmezés szerint az Azure Load Balancer a 80-as, 8080-as és 443-as portokat teszi elérhetővé. Használata esetén egy három portok (mint mi a fenti példa hello), majd semmilyen toodo van szüksége. Meg kell tudni toohit az ügynök betölti a terheléselosztó FQDN, és minden alkalommal, amikor frissíti, lesz találati egyet a három webkiszolgálók ciklikus multiplexelés. 

Ha egy másik portot használ, szükség van tooadd egy ciklikus multiplexelési szabályt és egy mintavételt hello terheléselosztón hello használt portot. Ehhez a hello [Azure CLI](../../azure-resource-manager/xplat-cli-azure-resource-manager.md), hello parancsokkal `azure network lb rule create` és `azure network lb probe create`.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóprogramban megismerte megtudni a terheléselosztásról az ACS hello Marathon és az Azure load beleértve terheléselosztók hello a következő műveleteket:

> [!div class="checklist"]
> * A Marathon terheléselosztó konfigurálása
> * Hello terheléselosztót használó alkalmazások központi telepítése
> * Konfigurálja és Azure terheléselosztó

Előzetes toohello oktatóanyag következő toolearn az Azure storage együttes használata a DC/OS az Azure-ban.

> [!div class="nextstepaction"]
> [A DC/OS-fürt csatlakoztatási Azure fájlmegosztás](container-service-dcos-fileshare.md)