---
title: "aaaQuickstart - a Windows Azure Kubernetes fürt |} Microsoft Docs"
description: "Ismerje meg gyorsan, toocreate a Kubernetes fürtökben az Azure Tárolószolgáltatásban tárolók Windows hello Azure parancssori felület."
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service, kubernetes
keywords: 
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/18/2017
ms.author: danlep
ms.custom: H1Hack27Feb2017, mvc
ms.openlocfilehash: 85fe65a46ae8c78797e8a8a097c2a37f06329335
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-kubernetes-cluster-for-windows-containers"></a>Kubernetes-fürt üzembe helyezése Windows-tárolókhoz

hello Azure CLI használt toocreate és hello parancssorból vagy parancsfájlokban Azure-erőforrások kezeléséhez. Ez az útmutató részletek hello Azure CLI toodeploy használatával egy [Kubernetes](https://kubernetes.io/docs/home/) fürthöz [Azure Tárolószolgáltatás](../container-service-intro.md). Miután hello fürt központi telepítése, hello Kubernetes tooit csatlakozott `kubectl` parancssori eszköz, és az első Windows tároló üzembe.

Ha nem rendelkezik Azure-előfizetéssel, mindössze néhány perc alatt létrehozhat egy [ingyenes fiókot](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) a virtuális gép létrehozásának megkezdése előtt.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Ha Ön tooinstall kiválasztása és hello CLI helyileg, a gyors üzembe helyezés van szükség, hogy verzióját hello Azure CLI 2.0.4 vagy újabb. Futtatás `az --version` toofind hello verziója. Ha tooinstall vagy frissítés van szüksége, tekintse meg [Azure CLI 2.0 telepítése]( /cli/azure/install-azure-cli). 

> [!NOTE]
> Az Azure Container Service szolgáltatásban a Kubernetesen működő Windows-tárolók támogatása előzetes verzióban érhető el. 
>

## <a name="create-a-resource-group"></a>Hozzon létre egy erőforráscsoportot

Hozzon létre egy erőforráscsoportot hello [az csoport létrehozása](/cli/azure/group#create) parancsot. Az Azure-erőforráscsoport olyan logikai csoport, amelyben az Azure-erőforrások üzembe helyezése és kezelése zajlik. 

hello alábbi példa létrehoz egy erőforráscsoportot *myResourceGroup* a hello *eastus* helyét.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-kubernetes-cluster"></a>Kubernetes-fürt létrehozása
Hozzon létre egy Kubernetes fürtöt az Azure Tárolószolgáltatásban hello [az acs létre](/cli/azure/acs#create) parancsot. 

hello alábbi példakód létrehozza a fürt nevű *myK8sCluster* egy Linux fő csomópont és a Windows-ügynök két csomópont. Ez a példa SSH kulcsok szükséges tooconnect toohello Linuxos fő hoz létre. Ez a példa *azureuser* egy rendszergazda felhasználó neve és *myPassword12* hello jelszóként hello Windows csomópontján. Ezen értékek toosomething megfelelő tooyour környezet frissítése. 



```azurecli-interactive 
az acs create --orchestrator-type=kubernetes \
    --resource-group myResourceGroup \
    --name=myK8sCluster \
    --agent-count=2 \
    --generate-ssh-keys \
    --windows --admin-username azureuser \
    --admin-password myPassword12
```

Pár perc múlva hello parancs befejeződik, és a telepítéssel kapcsolatos információk láthatók.

## <a name="install-kubectl"></a>A kubectl telepítése

az ügyfélszámítógépen, használja a fürt Kubernetes tooconnect toohello [ `kubectl` ](https://kubernetes.io/docs/user-guide/kubectl/), hello Kubernetes parancssori ügyfél. 

Az Azure CloudShell használata esetén a `kubectl` már telepítve van. Ha azt szeretné, hogy tooinstall helyben, hogy használni tudja hello [az acs kubernetes install-cli](/cli/azure/acs/kubernetes#install-cli) parancsot.

a következő példa telepíti az Azure parancssori felület hello `kubectl` tooyour rendszer. Windows rendszerben rendszergazdaként futtassa ezt a parancsot.

```azurecli-interactive 
az acs kubernetes install-cli
```


## <a name="connect-with-kubectl"></a>Kapcsolódás a kubectl parancssori ügyfélhez

tooconfigure `kubectl` tooconnect tooyour Kubernetes fürthöz, futtassa a hello [az acs kubernetes get-hitelesítő adatok](/cli/azure/acs/kubernetes#get-credentials) parancsot. hello alábbi példa letölti a Kubernetes fürt hello fürtkonfiguráció.

```azurecli-interactive 
az acs kubernetes get-credentials --resource-group=myResourceGroup --name=myK8sCluster
```

tooverify hello kapcsolat tooyour fürt a számítógépről, futtassa:

```azurecli-interactive
kubectl get nodes
```

`kubectl`hello fő és ügynök csomópontok listája.

```azurecli-interactive
NAME                    STATUS                     AGE       VERSION
k8s-agent-98dc3136-0    Ready                      5m        v1.5.3
k8s-agent-98dc3136-1    Ready                      5m        v1.5.3
k8s-master-98dc3136-0   Ready,SchedulingDisabled   5m        v1.5.3

```

## <a name="deploy-a-windows-iis-container"></a>Windows IIS-tároló üzembe helyezése

Docker-tárolót futtathat egy olyan Kubernetes-*podon* belül, amely egy vagy több tárolót tartalmaz. 

Alapvető példában egy JSON-fájl toospecify egy Microsoft Internet Information Server (IIS) tárolót használ, és létrehozza a hello pod hello használata `kubctl apply` parancsot. 

Hozzon létre egy helyi fájlt `iis.json` és a következő szöveg másolása hello. Ez a fájl közli Kubernetes toorun IIS Windows Server 2016 Nano Server, a nyilvános tárolókban lemezképpel [Docker Hub](https://hub.docker.com/r/nanoserver/iis/). hello tároló 80-as portot, de kezdetben csak hello fürt hálózaton belülről érhetők el.

 ```JSON
 {
  "apiVersion": "v1",
  "kind": "Pod",
  "metadata": {
    "name": "iis",
    "labels": {
      "name": "iis"
    }
  },
  "spec": {
    "containers": [
      {
        "name": "iis",
        "image": "nanoserver/iis",
        "ports": [
          {
          "containerPort": 80
          }
        ]
      }
    ],
    "nodeSelector": {
     "beta.kubernetes.io/os": "windows"
     }
   }
 }
 ```

toostart hello pod, típus:
  
```azurecli-interactive
kubectl apply -f iis.json
```  

tootrack hello telepítés, típus:
  
```azurecli-interactive
kubectl get pods
```

Hello pod telepít, amíg hello állapota `ContainerCreating`. Hello tároló tooenter hello néhány percet is igénybe vehet `Running` állapotát.

```azurecli-interactive
NAME     READY        STATUS        RESTARTS    AGE
iis      1/1          Running       0           32s
```

## <a name="view-hello-iis-welcome-page"></a>Nézet hello IIS-kezdőlap

tooexpose hello pod toohello globális nyilvános IP-címmel, a következő parancs típusa hello:

```azurecli-interactive
kubectl expose pods iis --port=80 --type=LoadBalancer
```

Ezzel a paranccsal Kubernetes létrehoz egy szolgáltatást, és egy [Azure terheléselosztó szabályhoz](container-service-kubernetes-load-balancing.md) hello szolgáltatás nyilvános IP-címmel. 

Futtassa a következő parancs toosee hello hello szolgáltatás állapotának hello.

```azurecli-interactive
kubectl get svc
```

Kezdetben hello IP-cím jelenik meg `pending`. Néhány perc elteltével hello hello külső IP-címe `iis` pod van beállítva:
  
```azurecli-interactive
NAME         CLUSTER-IP     EXTERNAL-IP     PORT(S)        AGE       
kubernetes   10.0.0.1       <none>          443/TCP        21h       
iis          10.0.111.25    13.64.158.233   80/TCP         22m
```

A choice toosee hello alapértelmezett IIS üdvözlőlap webböngészővel hello külső IP-címen használhatja:

![Böngészés tooIIS képe](./media/container-service-kubernetes-windows-walkthrough/kubernetes-iis.png)  


## <a name="delete-cluster"></a>Fürt törlése
Amikor hello fürt már nem szükséges, használhatja a hello [az csoport törlése](/cli/azure/group#delete) tooremove hello erőforráscsoport, a tárolószolgáltatás és a minden kapcsolódó erőforrások parancsot.

```azurecli-interactive 
az group delete --name myResourceGroup
```


## <a name="next-steps"></a>Következő lépések

A rövid útmutató segítségével üzembe helyezett egy Kubernetes-fürtöt, kapcsolatot hozott létre a `kubectl` parancssori ügyféllel, és üzembe helyezett egy IIS-tárolóval rendelkező podot. További információ az Azure Tárolószolgáltatás toolearn toohello Kubernetes oktatóanyag továbbra is.

> [!div class="nextstepaction"]
> [ACS Kubernetes-fürtök kezelése](container-service-tutorial-kubernetes-prepare-app.md)
