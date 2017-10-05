---
title: "Azure Tárolószolgáltatás útmutató - fürt központi telepítése |} Microsoft Docs"
description: "Azure Tárolószolgáltatás útmutató - fürt központi telepítése"
services: container-service
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Docker, tárolók, mikroszolgáltatások, Kubernetes, DC/OS, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: azurecli
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/21/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 472697c1f0c18859087d7b448e1786d85c27aca0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="deploy-a-kubernetes-cluster-in-azure-container-service"></a>Az Azure Tárolószolgáltatásban Kubernetes fürt központi telepítése

Kubernetes elosztott platformot kínál a tárolóalapú alkalmazások. Azure Tárolószolgáltatás készen áll a Kubernetes éles fürt üzembe esetén egyszerűen és gyorsan. Ez az oktatóanyag, 7, 3. rész egy Azure-tároló szolgáltatás Kubernetes fürtre van telepítve. Befejeződött a lépések az alábbiak:

> [!div class="checklist"]
> * Egy Kubernetes ACS-fürt telepítése
> * A Kubernetes CLI (kubectl) telepítése
> * Kubectl konfigurálása

A következő útmutatókból az Azure szavazattal alkalmazás központi telepítése a fürt, méretezhető, frissítése, és az Operations Management Suite a Kubernetes fürt figyelésére van beállítva.

## <a name="before-you-begin"></a>Előkészületek

Az előző oktatóanyagokat a tároló-lemezkép létrejött, de feltöltött egy Azure-tároló beállításjegyzék-példányon. Ha nem volna ezeket a lépéseket, és szeretné követéséhez, vissza [oktatóanyag 1 – létrehozás tároló képek](./container-service-tutorial-kubernetes-prepare-app.md).

## <a name="create-kubernetes-cluster"></a>Kubernetes-fürt létrehozása

Az a [az oktatóanyag előző](./container-service-tutorial-kubernetes-prepare-acr.md), nevű erőforráscsoport *myResourceGroup* lett létrehozva. Ha nem tette, ez az erőforráscsoport most hozzon létre.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

Hozzon létre egy Kubernetes-fürtöt az Azure Container Service-ben az [az acs create](/cli/azure/acs#create) paranccsal. 

A következő példa egy *myK8sCluster* nevű fürtöt hoz létre egy Linux-főcsomóponttal és három Linux-ügyfélcsomóponttal.

```azurecli-interactive 
az acs create --orchestrator-type=kubernetes --resource-group myResourceGroup --name=myK8SCluster --generate-ssh-keys 
```

Pár perc múlva a parancs végrehajtását, és értéket ad vissza json formátumú az ACS telepítési információkat.

## <a name="install-the-kubectl-cli"></a>A kubectl parancssori felület telepítése

Az Kubernetes fürthöz csatlakoztatja az ügyfélszámítógépről, használja a [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), a Kubernetes parancssori ügyfél. 

Az Azure CloudShell használata esetén a `kubectl` már telepítve van. Ha helyileg telepíteni szeretne, használja a [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) parancsot.

Ha a Linux vagy macOS fut, szükség lehet a sudo futtatásához. A Windows győződjön meg arról, a rendszerhéj futtatása rendszergazdaként.

```azurecli-interactive 
az acs kubernetes install-cli 
```

A Windows, az alapértelmezett telepítési van *c:\program files (x86)\kubectl.exe*. Szükség lehet a fájl hozzáadása a Windows útvonalhoz. 

## <a name="connect-with-kubectl"></a>Kapcsolódás a kubectl parancssori ügyfélhez

Az [az acs kubernetes get-credentials](/cli/azure/acs/kubernetes#get-credentials) parancs futtatásával konfigurálja a `kubectl` ügyfelet úgy, hogy a saját Kubernetes-fürthöz kapcsolódjon.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8SCluster
```

Ellenőrizze a kapcsolatot a fürthöz, futtassa a [kubectl beolvasása csomópontok](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) parancsot.

```azurecli-interactive
kubectl get nodes
```

Kimenet:

```bash
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.6.2
k8s-agent-98dc3136-1    Ready                      5m        v1.6.2
k8s-agent-98dc3136-2    Ready                      5m        v1.6.2
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.6.2
```

Oktatóanyag befejezése után következő, hogy az ACS Kubernetes fürt készen áll a munkaterhelések. A következő útmutatókból egy több tároló alkalmazás üzemel a fürthöz, horizontálisan, frissítése, és figyeli.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban az Azure-tároló szolgáltatás Kubernetes fürt tették elérhetővé telepítésre. A következő lépéseket hajtotta végre:

> [!div class="checklist"]
> * Egy Kubernetes ACS-fürt telepítése
> * A Kubernetes CLI (kubectl) telepítése
> * Konfigurált kubectl

Továbblépés a következő oktatóanyag fürtben futó alkalmazás megismeréséhez.

> [!div class="nextstepaction"]
> [Kubernetes az alkalmazás központi telepítése](./container-service-tutorial-kubernetes-deploy-application.md)
