---
title: "Webes felhasználói felület Azure Kubernetes fürt kezelése |} Microsoft Docs"
description: "Az Azure Tárolószolgáltatásban a Kubernetes webes felhasználói felület használatával"
services: container-service
documentationcenter: 
author: bburns
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
ms.date: 02/21/2017
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: e31f90d61fc61f17582372fe9f491a1e21f628b0
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="using-the-kubernetes-web-ui-with-azure-container-service"></a><span data-ttu-id="ede39-103">Azure Tárolószolgáltatás a Kubernetes webes felhasználói felület használatával</span><span class="sxs-lookup"><span data-stu-id="ede39-103">Using the Kubernetes web UI with Azure Container Service</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ede39-104">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ede39-104">Prerequisites</span></span>
<span data-ttu-id="ede39-105">Ez az útmutató feltételezi, hogy rendelkezik [a Kubernetes Azure Tárolószolgáltatási fürt létrehozott](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="ede39-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>


<span data-ttu-id="ede39-106">Azt is feltételezi, hogy rendelkezik-e az Azure CLI 2.0 és `kubectl` eszközök vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="ede39-106">It also assumes that you have the Azure CLI 2.0 and `kubectl` tools installed.</span></span>

<span data-ttu-id="ede39-107">Ha tesztelheti a `az` eszköz futtatásával telepítve:</span><span class="sxs-lookup"><span data-stu-id="ede39-107">You can test if you have the `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="ede39-108">Ha nem rendelkezik a `az` eszköz telepítve, az e-mail utasításokat is [Itt](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="ede39-108">If you don't have the `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>

<span data-ttu-id="ede39-109">Ha tesztelheti a `kubectl` eszköz futtatásával telepítve:</span><span class="sxs-lookup"><span data-stu-id="ede39-109">You can test if you have the `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="ede39-110">Ha nem rendelkezik `kubectl` telepítve, futtatható:</span><span class="sxs-lookup"><span data-stu-id="ede39-110">If you don't have `kubectl` installed, you can run:</span></span>

```console
$ az acs kubernetes install-cli
```

## <a name="overview"></a><span data-ttu-id="ede39-111">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="ede39-111">Overview</span></span>

### <a name="connect-to-the-web-ui"></a><span data-ttu-id="ede39-112">A webes felhasználói felület csatlakozik</span><span class="sxs-lookup"><span data-stu-id="ede39-112">Connect to the web UI</span></span>
<span data-ttu-id="ede39-113">A Kubernetes webes felhasználói felület futtatásával indíthatja el:</span><span class="sxs-lookup"><span data-stu-id="ede39-113">You can launch the Kubernetes web UI by running:</span></span>

```console
$ az acs kubernetes browse -g [Resource Group] -n [Container service instance name]
```

<span data-ttu-id="ede39-114">Ez kell nyisson meg egy webböngészőt, kérdezze meg a helyi számítógép csatlakozik a Kubernetes webes felhasználói felület biztonságos proxy konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="ede39-114">This should open a web browser configured to talk to a secure proxy connecting your local machine to the Kubernetes web UI.</span></span>

### <a name="create-and-expose-a-service"></a><span data-ttu-id="ede39-115">Hozzon létre, és tegye elérhetővé a szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="ede39-115">Create and expose a service</span></span>
1. <span data-ttu-id="ede39-116">A Kubernetes webes felhasználói felületen kattintson **létrehozása** gombra a jobb felső ablakban.</span><span class="sxs-lookup"><span data-stu-id="ede39-116">In the Kubernetes web UI, click **Create** button in the upper right window.</span></span>

    ![Kubernetes felhasználói Felületüket létrehozni.](./media/container-service-kubernetes-ui/create.png)

    <span data-ttu-id="ede39-118">Egy adott elindíthatja az alkalmazás létrehozása párbeszédpanel megnyílik.</span><span class="sxs-lookup"><span data-stu-id="ede39-118">A dialog box opens where you can start creating your application.</span></span>

2. <span data-ttu-id="ede39-119">A nevet `hello-nginx`.</span><span class="sxs-lookup"><span data-stu-id="ede39-119">Give it the name `hello-nginx`.</span></span> <span data-ttu-id="ede39-120">Használja a [ `nginx` tárolót a Docker](https://hub.docker.com/_/nginx/) és három replikák webszolgáltatás telepítése.</span><span class="sxs-lookup"><span data-stu-id="ede39-120">Use the [`nginx` container from Docker](https://hub.docker.com/_/nginx/) and deploy three replicas of this web service.</span></span>

    ![Kubernetes Pod létrehozása párbeszédpanel](./media/container-service-kubernetes-ui/nginx.png)

3. <span data-ttu-id="ede39-122">A **szolgáltatás**, jelölje be **külső** , és írja be a 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="ede39-122">Under **Service**, select **External** and enter port 80.</span></span>

    <span data-ttu-id="ede39-123">Ez a beállítás kiegyenlíti forgalom a három replikához.</span><span class="sxs-lookup"><span data-stu-id="ede39-123">This setting load-balances traffic to the three replicas.</span></span>

    ![Kubernetes Service létrehozás párbeszédpanel](./media/container-service-kubernetes-ui/service.png)

4. <span data-ttu-id="ede39-125">Kattintson a **telepítés** ezek a tárolók és szolgáltatások telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ede39-125">Click **Deploy** to deploy these containers and services.</span></span>

    ![Kubernetes központi telepítése](./media/container-service-kubernetes-ui/deploy.png)

### <a name="view-your-containers"></a><span data-ttu-id="ede39-127">A tárolók megtekintése</span><span class="sxs-lookup"><span data-stu-id="ede39-127">View your containers</span></span>
<span data-ttu-id="ede39-128">Miután rákattintott **telepítés**, a felhasználói felületen jeleníti meg, a szolgáltatás módon telepíti:</span><span class="sxs-lookup"><span data-stu-id="ede39-128">After you click **Deploy**, the UI shows a view of your service as it deploys:</span></span>

![Kubernetes állapota](./media/container-service-kubernetes-ui/status.png)

<span data-ttu-id="ede39-130">Megtekintheti a kör Kubernetes objektumok állapotának a felhasználói felület, a bal oldalon **három munkaállomás-csoporttal**.</span><span class="sxs-lookup"><span data-stu-id="ede39-130">You can see the status of each Kubernetes object in the circle on the left-hand side of the UI, under **Pods**.</span></span> <span data-ttu-id="ede39-131">Ha egy részben teljes kör, az objektum még telepítés.</span><span class="sxs-lookup"><span data-stu-id="ede39-131">If it is a partially full circle, then the object is still deploying.</span></span> <span data-ttu-id="ede39-132">Az objektum teljes mértékben telepítve, egy zöld pipa jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="ede39-132">When an object is fully deployed, it displays a green check mark:</span></span>

![Telepített Kubernetes](./media/container-service-kubernetes-ui/deployed.png)

<span data-ttu-id="ede39-134">Ha minden fut, kattintson a három munkaállomás-csoporttal a futó webes szolgáltatás kapcsolatos részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="ede39-134">Once everything is running, click one of your pods to see details about the running web service.</span></span>

![Kubernetes három munkaállomás-csoporttal](./media/container-service-kubernetes-ui/pods.png)

<span data-ttu-id="ede39-136">Az a **három munkaállomás-csoporttal** nézet, a tárolók fogyasztanak, valamint a CPU és memória-erőforrások összes blobhoz által használt információ látható:</span><span class="sxs-lookup"><span data-stu-id="ede39-136">In the **Pods** view, you can see information about the containers in the pod as well as the CPU and memory resources used by those containers:</span></span>

![Kubernetes erőforrások](./media/container-service-kubernetes-ui/resources.png)

<span data-ttu-id="ede39-138">Ha nem látja az erőforrásokat, esetleg Várjon néhány percet a figyelési adatok továbbítására.</span><span class="sxs-lookup"><span data-stu-id="ede39-138">If you don't see the resources, you may need to wait a few minutes for the monitoring data to propagate.</span></span>

<span data-ttu-id="ede39-139">A tároló a naplók megtekintéséhez kattintson **naplók megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="ede39-139">To see the logs for your container, click **View logs**.</span></span>

![Kubernetes naplók](./media/container-service-kubernetes-ui/logs.png)

### <a name="viewing-your-service"></a><span data-ttu-id="ede39-141">A szolgáltatás megtekintése</span><span class="sxs-lookup"><span data-stu-id="ede39-141">Viewing your service</span></span>
<span data-ttu-id="ede39-142">A Kubernetes felhasználói felület mellett futó a tárolók, külső hozott létre `Service` amely látja el egy terhelés-kiegyenlítő forgalom kerüljön a tárolók a fürtön.</span><span class="sxs-lookup"><span data-stu-id="ede39-142">In addition to running your containers, the Kubernetes UI has created an external `Service` which provisions a load balancer to bring traffic to the containers in your cluster.</span></span>

<span data-ttu-id="ede39-143">A bal oldali navigációs ablaktáblán kattintson **szolgáltatások** megtekintéséhez az összes szolgáltatást (kell csak egyet).</span><span class="sxs-lookup"><span data-stu-id="ede39-143">In the left navigation pane, click **Services** to view all services (there should be only one).</span></span>

![Kubernetes szolgáltatások](./media/container-service-kubernetes-ui/service-deployed.png)

<span data-ttu-id="ede39-145">A fenti nézetben megjelenik a szolgáltatás felosztott külső végpont (IP-cím).</span><span class="sxs-lookup"><span data-stu-id="ede39-145">In that view, you should see an external endpoint (IP address) that has been allocated to your service.</span></span>
<span data-ttu-id="ede39-146">Ha az IP-címet gombra kattint, megtekintheti a terheléselosztó mögött futó Nginx tároló.</span><span class="sxs-lookup"><span data-stu-id="ede39-146">If you click that IP address, you should see your Nginx container running behind the load balancer.</span></span>

![nginx megtekintése](./media/container-service-kubernetes-ui/nginx-page.png)

### <a name="resizing-your-service"></a><span data-ttu-id="ede39-148">A szolgáltatás átméretezése</span><span class="sxs-lookup"><span data-stu-id="ede39-148">Resizing your service</span></span>
<span data-ttu-id="ede39-149">Is megtekintheti az objektumok a felhasználói felület, szerkesztheti, és frissítse az Kubernetes API objektumokat.</span><span class="sxs-lookup"><span data-stu-id="ede39-149">In addition to viewing your objects in the UI, you can edit and update the Kubernetes API objects.</span></span>

<span data-ttu-id="ede39-150">Első lépésként kattintson **központi telepítések** a bal oldali navigációs panelen, hogy a szolgáltatás központi telepítését.</span><span class="sxs-lookup"><span data-stu-id="ede39-150">First, click **Deployments** in the left navigation pane to see the deployment for your service.</span></span>

<span data-ttu-id="ede39-151">Miután a fenti nézetben, kattintson a replikakészlethez a, majd **szerkesztése** a felső navigációs sávján látható:</span><span class="sxs-lookup"><span data-stu-id="ede39-151">Once you are in that view, click on the replica set, and then click **Edit** in the upper navigation bar:</span></span>

![Kubernetes szerkesztése](./media/container-service-kubernetes-ui/edit.png)

<span data-ttu-id="ede39-153">Szerkessze a `spec.replicas` mezőre, amely `2`, és kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="ede39-153">Edit the `spec.replicas` field to be `2`, and click **Update**.</span></span>

<span data-ttu-id="ede39-154">Ennek hatására két egy a három munkaállomás-csoporttal eldobása replikák száma.</span><span class="sxs-lookup"><span data-stu-id="ede39-154">This causes the number of replicas to drop to two by deleting one of your pods.</span></span>

 

