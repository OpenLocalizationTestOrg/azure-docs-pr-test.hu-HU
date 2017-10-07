---
title: "az Azure Kubernetes Helm aaaDeploy tárolók |} Microsoft Docs"
description: "Az Azure Tárolószolgáltatásban Kubernetes fürtön hello Helm csomagolás eszköz toodeploy tárolók használata"
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/10/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: c7bd780afe00084ebe4e3a14873e1e340a29d144
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-helm-toodeploy-containers-on-a-kubernetes-cluster"></a>Kubernetes fürt Helm toodeploy tárolók használata 

[Helm](https://github.com/kubernetes/helm/) egy nyílt forráskódú csomagolás eszköz, amelynek segítségével telepítheti és kezelheti a Kubernetes alkalmazások hello életciklusát. Hasonló tooLinux csomag kezelők például Apt-get és Yum Helm használt toomanage Kubernetes diagramok, amelyek az előre konfigurált Kubernetes erőforrások csomagok. Ez a cikk bemutatja, hogyan Kubernetes fürtön a Helm toowork Azure Tárolószolgáltatás telepítve.

Helm két részből áll: 
* Hello **Helm CLI** helyileg vagy hello felhőben található a számítógépen futó ügyfél  

* **Kormányrúd** , amely hello Kubernetes fürtön fut, és a Kubernetes alkalmazások hello életciklusát felügyeli 
 
## <a name="prerequisites"></a>Előfeltételek

* [Hozzon létre egy Kubernetes fürtöt](container-service-kubernetes-walkthrough.md) az Azure Tárolószolgáltatásban

* [Telepítse és konfigurálja `kubectl` ](../container-service-connect.md) a helyi számítógépen

* [Telepítse a Helm](https://github.com/kubernetes/helm/blob/master/docs/install.md) a helyi számítógépen

## <a name="helm-basics"></a>Helm alapjai 

tooview információ hello Kubernetes fürt, hogy kormányrúd telepítése, és az alkalmazások telepítése, írja be a következő parancs hello:

```bash
kubectl cluster-info 
```
![kubectl fürt-adatai](./media/container-service-kubernetes-helm/clusterinfo.png)
 
Miután telepítette a Helm, kormányrúd a fürtön telepíteni a Kubernetes hello a következő parancs beírásával:

```bash
helm init --upgrade
```
Ha sikeres a végrehajtása, hasonló hello kimenet jelenik meg:

![Kormányrúd telepítése](./media/container-service-kubernetes-helm/tiller-install.png)
 
 
 
 
tooview hello Helm diagramjainak elérhető hello tárház, a következő típus hello parancsot:

```bash 
helm search 
```

Hello következő kimenet jelenik meg:

![Helm keresése](./media/container-service-kubernetes-helm/helm-search.png)
 
tooupdate hello diagramok tooget hello legújabb verziója, írja be:

```bash 
helm repo update 
```
## <a name="deploy-an-nginx-ingress-controller-chart"></a>Egy Nginx érkező vezérlő diagram telepítése 
 
egy Nginx érkező vezérlő diagram toodeploy egy parancs írja be:

```bash
helm install stable/nginx-ingress 
```
![Érkező tartományvezérlő központi telepítése](./media/container-service-kubernetes-helm/nginx-ingress.png)

Ha `kubectl get svc` tooview hello fürtön futó összes szolgáltatásokat, láthatja, hogy az IP-cím toohello érkező tartományvezérlő van hozzárendelve. (Hello hozzárendelés folyamatban van, amíg megjelenik `<pending>`. Igénybe vehet néhány percet toocomplete.) 

Után hello IP-cím van hozzárendelve, keresse meg a hello külső IP cím toosee hello Nginx háttér futtató toohello értékét. 
 
![Érkező IP-cím](./media/container-service-kubernetes-helm/ingress-ip-address.png)


toosee diagramok listája telepítve a fürtben, típus:

```bash
helm list 
```

Hello parancs túl rövidíthető`helm ls`.
 
 
 
 
## <a name="deploy-a-mariadb-chart-and-client"></a>A MariaDB diagram és az ügyfél telepítése

Mostantól telepítheti MariaDB diagram és MariaDB ügyfél tooconnect toohello adatbázis.

toodeploy hello MariaDB diagram, a következő parancs típusa hello:

```bash
helm install --name v1 stable/mariadb
```

Ha `--name` kiadásokban használt címke.

> [!TIP]
> Ha hello telepítése sikertelen, futtassa `helm repo update` , és próbálkozzon újra.
>
 
 
tooview a fürtben, típus regisztrálva diagramjainak hello:

```bash 
helm list
```
 
tooview írja be a fürtben futó összes központi telepítése:

```bash
kubectl get deployments 
``` 
 
 
Végezetül toorun pod tooaccess hello ügyfél, írja be:

```bash
kubectl run v1-mariadb-client --rm --tty -i --image bitnami/mariadb --command -- bash  
``` 
 
 
tooconnect toohello ügyfél, a következő parancsot, hogy a típus hello `v1-mariadb` hello nevet, a központi telepítés:

```bash
sudo mysql –h v1-mariadb
```
 
 
Most már használhatja a szokásos SQL-parancsok toocreate adatbázisok, táblák, stb. Például `Create DATABASE testdb1;` létrehoz egy üres adatbázist. 
 
 
 
## <a name="next-steps"></a>Következő lépések

* Kubernetes diagramok kezelésével kapcsolatos további információkért lásd: hello [Helm dokumentáció](https://github.com/kubernetes/helm/blob/master/docs/index.md). 

