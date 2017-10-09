---
title: "egy Azure Kubernetes aaaMonitor fürt CoScale |} Microsoft Docs"
description: "Az Azure Tárolószolgáltatásban CoScale használatával Kubernetes fürt figyelése"
services: container-service
documentationcenter: 
author: fryckbos
manager: 
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/22/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: f835e82d2be3afe1d85070bd0bf69649cc6dd2ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-kubernetes-cluster-with-coscale"></a>Egy Azure-tároló szolgáltatás Kubernetes fürt CoScale figyelése

Ez a cikk azt mutatja be toodeploy hello [CoScale](https://www.coscale.com/) az Azure Tárolószolgáltatásban fürt minden csomópontján és a tárolók a Kubernetes ügynök toomonitor. Ez a konfiguráció CoScale rendelkező fiók szükséges. 


## <a name="about-coscale"></a>CoScale kapcsolatos 

CoScale egy felügyeleti platform, metrikákkal és eseményekkel gyűjt az összes tároló több vezénylési platformokon. CoScale kínál a teljes verem Kubernetes környezetek figyelése. Képi megjelenítések és az elemzések biztosít az összes rétegének hello verem: hello az operációs rendszer, Kubernetes, Docker és a tárolók belül futó alkalmazások. CoScale kínál számos beépített figyelési irányítópultok, és beépített anomáliadetektálási észlelési tooallow operátorok rendelkezik, és a fejlesztők toofind infrastruktúra és az alkalmazás problémák gyors.

![Felhasználói felület coScale](./media/container-service-kubernetes-coscale/coscale.png)

Amint ez a cikk, ügynökök telepíthető egy Kubernetes fürt toorun CoScale, mint a Szolgáltatottszoftver-megoldás. Ha azt szeretné, tookeep az adatok helyszíni, CoScale is rendelkezésre áll a helyszíni telepítéshez.


## <a name="prerequisites"></a>Előfeltételek

Először túl[CoScale-fiók létrehozása](https://www.coscale.com/free-trial).

Ez az útmutató feltételezi, hogy rendelkezik [a Kubernetes Azure Tárolószolgáltatási fürt létrehozott](container-service-kubernetes-walkthrough.md).

Azt is feltételezi, hogy rendelkezik-e hello `az` Azure CLI és `kubectl` eszközök vannak telepítve.

Ha hello tesztelheti `az` eszköz futtatásával telepítve:

```azurecli
az --version
```

Ha még nem rendelkezik hello `az` eszköz telepítve, az e-mail utasításokat is [Itt](/cli/azure/install-azure-cli).

Ha hello tesztelheti `kubectl` eszköz futtatásával telepítve:

```bash
kubectl version
```

Ha nem rendelkezik `kubectl` telepítve, futtatható:

```azurecli
az acs kubernetes install-cli
```

## <a name="installing-hello-coscale-agent-with-a-daemonset"></a>Egy DaemonSet hello CoScale ügynök telepítése
[DaemonSets](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) rendszer által használt Kubernetes toorun a tároló egyetlen példány hello fürt minden állomáson.
Fontosságúak tökéletes megoldás például hello CoScale ügynök figyelőügynökök futtatásához.

Miután tooCoScale a bejelentkezést, lépjen a toohello [lap](https://app.coscale.com/) tooinstall CoScale ügynököt a fürt egy DaemonSet használatával. hello CoScale felhasználói felület interaktív konfigurációs lépéseket toocreate biztosít, az ügynök és a figyelő a teljes Kubernetes fürt indításának.

![CoScale ügynök konfigurálása](./media/container-service-kubernetes-coscale/installation.png)

toostart hello ügynök hello fürtön, futtassa a megadott hello parancsot:

![Indítsa el a hello CoScale ügynök](./media/container-service-kubernetes-coscale/agent_script.png)

Ennyi az egész! Ha hello ügynökök megfelelően működik, akkor jelenik meg: hello adatai néhány perc múlva. A Microsoft hello [lap](https://app.coscale.com/) toosee összegzését a fürt további konfigurációs lépések, és ellenőrizze, például hello irányítópultok **Kubernetes fürt áttekintése**.

![Kubernetes fürtök – áttekintés](./media/container-service-kubernetes-coscale/dashboard_clusteroverview.png)

hello CoScale ügynök automatikusan új gépek hello fürt központi telepítése. hello ügynök frissítések automatikus verziójának kiadásakor.


## <a name="next-steps"></a>Következő lépések

Lásd: hello [dokumentáció CoScale](http://docs.coscale.com/) és [blog](https://www.coscale.com/blog) figyelési megoldásoknak CoScale további információt. 

