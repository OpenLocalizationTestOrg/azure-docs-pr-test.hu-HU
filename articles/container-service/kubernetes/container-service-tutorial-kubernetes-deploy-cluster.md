---
title: "aaaAzure Tárolószolgáltatás oktatóanyag - fürt központi telepítése |} Microsoft Docs"
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
ms.openlocfilehash: c4c8cc95c88d9c2077d0322f57e5d3159e2dd0ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-kubernetes-cluster-in-azure-container-service"></a>Az Azure Tárolószolgáltatásban Kubernetes fürt központi telepítése

Kubernetes elosztott platformot kínál a tárolóalapú alkalmazások. Azure Tárolószolgáltatás készen áll a Kubernetes éles fürt üzembe esetén egyszerűen és gyorsan. Ez az oktatóanyag, 7, 3. rész egy Azure-tároló szolgáltatás Kubernetes fürtre van telepítve. Befejeződött a lépések az alábbiak:

> [!div class="checklist"]
> * Egy Kubernetes ACS-fürt telepítése
> * Hello Kubernetes CLI (kubectl) telepítése
> * Kubectl konfigurálása

A következő útmutatókból hello Azure szavazattal alkalmazás telepített toohello fürt, méretezhető, frissítése és az Operations Management Suite konfigurált toomonitor hello Kubernetes fürt.

## <a name="before-you-begin"></a>Előkészületek

Az előző oktatóanyagok a tároló-lemezkép létrehozásának és fel kell tölteni tooan Azure tároló beállításjegyzék példány. Ha nem volna ezeket a lépéseket, és szeretné mentén toofollow, visszaadása túl[oktatóanyag 1 – létrehozás tároló képek](./container-service-tutorial-kubernetes-prepare-app.md).

## <a name="create-kubernetes-cluster"></a>Kubernetes-fürt létrehozása

A hello [az oktatóanyag előző](./container-service-tutorial-kubernetes-prepare-acr.md), nevű erőforráscsoport *myResourceGroup* lett létrehozva. Ha nem tette, ez az erőforráscsoport most hozzon létre.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```

Hozzon létre egy Kubernetes fürtöt az Azure Tárolószolgáltatásban hello [az acs létre](/cli/azure/acs#create) parancsot. 

hello alábbi példakód létrehozza a fürt nevű *myK8sCluster* egy Linux fő csomópont- és Linux-ügynök három csomópontot.

```azurecli-interactive 
az acs create --orchestrator-type=kubernetes --resource-group myResourceGroup --name=myK8SCluster --generate-ssh-keys 
```

Pár perc múlva hello parancs végrehajtását, és értéket ad vissza json formátumú hello ACS fejlesztésével kapcsolatos információk.

## <a name="install-hello-kubectl-cli"></a>Hello kubectl parancssori felület telepítése

az ügyfélszámítógépen, használja a fürt Kubernetes tooconnect toohello [kubectl](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes parancssori ügyfél. 

Az Azure CloudShell használata esetén a `kubectl` már telepítve van. Ha azt szeretné, hogy tooinstall helyileg, használja a hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) parancsot.

Ha a Linux vagy macOS fut, szükség lehet a sudo toorun. A Windows győződjön meg arról, a rendszerhéj futtatása rendszergazdaként.

```azurecli-interactive 
az acs kubernetes install-cli 
```

A Windows hello alapértelmezett telepítése van *c:\program files (x86)\kubectl.exe*. Szükség lehet tooadd a toohello Windows elérési útja. 

## <a name="connect-with-kubectl"></a>Kapcsolódás a kubectl parancssori ügyfélhez

tooconfigure `kubectl` tooconnect tooyour Kubernetes fürthöz, futtassa a hello [az acs kubernetes get-hitelesítő adatok](/cli/azure/acs/kubernetes#get-credentials) parancsot.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8SCluster
```

tooverify hello kapcsolat tooyour fürthöz, futtassa a hello [kubectl beolvasása csomópontok](https://kubernetes.io/docs/user-guide/kubectl/v1.6/#get) parancsot.

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

Oktatóanyag befejezése után következő, hogy az ACS Kubernetes fürt készen áll a munkaterhelések. A következő útmutatókból a tárolót több alkalmazás telepített toothis fürt, horizontálisan, frissítése és figyelemmel kísérni.

## <a name="next-steps"></a>Következő lépések

Ebben az oktatóanyagban az Azure-tároló szolgáltatás Kubernetes fürt tették elérhetővé telepítésre. befejeződtek a hello a következő lépéseket:

> [!div class="checklist"]
> * Egy Kubernetes ACS-fürt telepítése
> * Telepített hello Kubernetes CLI (kubectl)
> * Konfigurált kubectl

Előzetes toohello oktatóanyag következő toolearn kapcsolatos hello fürt alkalmazást futtat.

> [!div class="nextstepaction"]
> [Kubernetes az alkalmazás központi telepítése](./container-service-tutorial-kubernetes-deploy-application.md)
