---
title: "Azure Kubernetes-fürtöt aaaManage webes felhasználói felületen |} Microsoft Docs"
description: "Hello segítségével Kubernetes webes felhasználói felülete, az Azure Tárolószolgáltatásban"
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
ms.openlocfilehash: e24ea0b82c94d2fd4610e4442699ef756590e6bc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-hello-kubernetes-web-ui-with-azure-container-service"></a><span data-ttu-id="7d7c3-103">Hello segítségével Kubernetes webes felhasználói felülete, az Azure Tárolószolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="7d7c3-103">Using hello Kubernetes web UI with Azure Container Service</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7d7c3-104">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="7d7c3-104">Prerequisites</span></span>
<span data-ttu-id="7d7c3-105">Ez az útmutató feltételezi, hogy rendelkezik [a Kubernetes Azure Tárolószolgáltatási fürt létrehozott](container-service-kubernetes-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="7d7c3-105">This walkthrough assumes that you have [created a Kubernetes cluster using Azure Container Service](container-service-kubernetes-walkthrough.md).</span></span>


<span data-ttu-id="7d7c3-106">Azt is feltételezi, hogy rendelkezik-e hello Azure CLI 2.0 és `kubectl` eszközök vannak telepítve.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-106">It also assumes that you have hello Azure CLI 2.0 and `kubectl` tools installed.</span></span>

<span data-ttu-id="7d7c3-107">Ha hello tesztelheti `az` eszköz futtatásával telepítve:</span><span class="sxs-lookup"><span data-stu-id="7d7c3-107">You can test if you have hello `az` tool installed by running:</span></span>

```console
$ az --version
```

<span data-ttu-id="7d7c3-108">Ha még nem rendelkezik hello `az` eszköz telepítve, az e-mail utasításokat is [Itt](https://github.com/azure/azure-cli#installation).</span><span class="sxs-lookup"><span data-stu-id="7d7c3-108">If you don't have hello `az` tool installed, there are instructions [here](https://github.com/azure/azure-cli#installation).</span></span>

<span data-ttu-id="7d7c3-109">Ha hello tesztelheti `kubectl` eszköz futtatásával telepítve:</span><span class="sxs-lookup"><span data-stu-id="7d7c3-109">You can test if you have hello `kubectl` tool installed by running:</span></span>

```console
$ kubectl version
```

<span data-ttu-id="7d7c3-110">Ha nem rendelkezik `kubectl` telepítve, futtatható:</span><span class="sxs-lookup"><span data-stu-id="7d7c3-110">If you don't have `kubectl` installed, you can run:</span></span>

```console
$ az acs kubernetes install-cli
```

## <a name="overview"></a><span data-ttu-id="7d7c3-111">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="7d7c3-111">Overview</span></span>

### <a name="connect-toohello-web-ui"></a><span data-ttu-id="7d7c3-112">Csatlakozás toohello webes felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="7d7c3-112">Connect toohello web UI</span></span>
<span data-ttu-id="7d7c3-113">Hello Kubernetes webes felhasználói felület futtatásával indíthatja el:</span><span class="sxs-lookup"><span data-stu-id="7d7c3-113">You can launch hello Kubernetes web UI by running:</span></span>

```console
$ az acs kubernetes browse -g [Resource Group] -n [Container service instance name]
```

<span data-ttu-id="7d7c3-114">Ez meg kell nyitnia a böngésző konfigurált tootalk tooa biztonságos webproxy csatlakozás a helyi számítógép toohello Kubernetes webes felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-114">This should open a web browser configured tootalk tooa secure proxy connecting your local machine toohello Kubernetes web UI.</span></span>

### <a name="create-and-expose-a-service"></a><span data-ttu-id="7d7c3-115">Hozzon létre, és tegye elérhetővé a szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="7d7c3-115">Create and expose a service</span></span>
1. <span data-ttu-id="7d7c3-116">Hello Kubernetes webes felhasználói felület, kattintson **létrehozása** a hello felső jobb oldali gomb.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-116">In hello Kubernetes web UI, click **Create** button in hello upper right window.</span></span>

    ![Kubernetes felhasználói Felületüket létrehozni.](./media/container-service-kubernetes-ui/create.png)

    <span data-ttu-id="7d7c3-118">Egy adott elindíthatja az alkalmazás létrehozása párbeszédpanel megnyílik.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-118">A dialog box opens where you can start creating your application.</span></span>

2. <span data-ttu-id="7d7c3-119">Adjon neki nevet hello `hello-nginx`.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-119">Give it hello name `hello-nginx`.</span></span> <span data-ttu-id="7d7c3-120">Használjon hello [ `nginx` tárolót a Docker](https://hub.docker.com/_/nginx/) és három replikák webszolgáltatás telepítése.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-120">Use hello [`nginx` container from Docker](https://hub.docker.com/_/nginx/) and deploy three replicas of this web service.</span></span>

    ![Kubernetes Pod létrehozása párbeszédpanel](./media/container-service-kubernetes-ui/nginx.png)

3. <span data-ttu-id="7d7c3-122">A **szolgáltatás**, jelölje be **külső** , és írja be a 80-as porton.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-122">Under **Service**, select **External** and enter port 80.</span></span>

    <span data-ttu-id="7d7c3-123">Ez a beállítás kiegyenlíti forgalom toohello három replikákat.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-123">This setting load-balances traffic toohello three replicas.</span></span>

    ![Kubernetes Service létrehozás párbeszédpanel](./media/container-service-kubernetes-ui/service.png)

4. <span data-ttu-id="7d7c3-125">Kattintson a **telepítés** toodeploy ezeket tárolók és a szolgáltatásokat.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-125">Click **Deploy** toodeploy these containers and services.</span></span>

    ![Kubernetes központi telepítése](./media/container-service-kubernetes-ui/deploy.png)

### <a name="view-your-containers"></a><span data-ttu-id="7d7c3-127">A tárolók megtekintése</span><span class="sxs-lookup"><span data-stu-id="7d7c3-127">View your containers</span></span>
<span data-ttu-id="7d7c3-128">Miután rákattintott **telepítés**, felhasználói felület hello jeleníti meg, a szolgáltatás módon telepíti:</span><span class="sxs-lookup"><span data-stu-id="7d7c3-128">After you click **Deploy**, hello UI shows a view of your service as it deploys:</span></span>

![Kubernetes állapota](./media/container-service-kubernetes-ui/status.png)

<span data-ttu-id="7d7c3-130">Látható hello állapota hello kör Kubernetes objektumok hello bal oldalán lévő felhasználói felület, a **három munkaállomás-csoporttal**.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-130">You can see hello status of each Kubernetes object in hello circle on hello left-hand side of the UI, under **Pods**.</span></span> <span data-ttu-id="7d7c3-131">Ha egy részben teljes kör, hello objektum még telepítés.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-131">If it is a partially full circle, then hello object is still deploying.</span></span> <span data-ttu-id="7d7c3-132">Az objektum teljes mértékben telepítve, egy zöld pipa jeleníti meg:</span><span class="sxs-lookup"><span data-stu-id="7d7c3-132">When an object is fully deployed, it displays a green check mark:</span></span>

![Telepített Kubernetes](./media/container-service-kubernetes-ui/deployed.png)

<span data-ttu-id="7d7c3-134">Ha minden fut, kattintson a webszolgáltatást futtató hello három munkaállomás-csoporttal toosee részleteit.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-134">Once everything is running, click one of your pods toosee details about hello running web service.</span></span>

![Kubernetes három munkaállomás-csoporttal](./media/container-service-kubernetes-ui/pods.png)

<span data-ttu-id="7d7c3-136">A hello **három munkaállomás-csoporttal** nézetben hello tárolók hello pod, valamint a hello CPU és memória-erőforrások összes blobhoz által használt információ látható:</span><span class="sxs-lookup"><span data-stu-id="7d7c3-136">In hello **Pods** view, you can see information about hello containers in hello pod as well as hello CPU and memory resources used by those containers:</span></span>

![Kubernetes erőforrások](./media/container-service-kubernetes-ui/resources.png)

<span data-ttu-id="7d7c3-138">Ha hello erőforrások nem látható, szükség lehet toowait néhány percig, amíg a figyelési adatok toopropagate hello.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-138">If you don't see hello resources, you may need toowait a few minutes for hello monitoring data toopropagate.</span></span>

<span data-ttu-id="7d7c3-139">Kattintson a tároló toosee hello naplók **naplók megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-139">toosee hello logs for your container, click **View logs**.</span></span>

![Kubernetes naplók](./media/container-service-kubernetes-ui/logs.png)

### <a name="viewing-your-service"></a><span data-ttu-id="7d7c3-141">A szolgáltatás megtekintése</span><span class="sxs-lookup"><span data-stu-id="7d7c3-141">Viewing your service</span></span>
<span data-ttu-id="7d7c3-142">Továbbá toorunning a tárolók hello Kubernetes felhasználói felület létrehozott külső `Service` amely látja el a terhelés terheléselosztó toobring forgalom toohello tárolók a fürtön.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-142">In addition toorunning your containers, hello Kubernetes UI has created an external `Service` which provisions a load balancer toobring traffic toohello containers in your cluster.</span></span>

<span data-ttu-id="7d7c3-143">Hello bal oldali navigációs ablaktábláján kattintson **szolgáltatások** tooview összes szolgáltatást (kell csak egyet).</span><span class="sxs-lookup"><span data-stu-id="7d7c3-143">In hello left navigation pane, click **Services** tooview all services (there should be only one).</span></span>

![Kubernetes szolgáltatások](./media/container-service-kubernetes-ui/service-deployed.png)

<span data-ttu-id="7d7c3-145">A fenti nézetben megtekintheti az tooyour szolgáltatás felosztott külső végpont (IP-cím).</span><span class="sxs-lookup"><span data-stu-id="7d7c3-145">In that view, you should see an external endpoint (IP address) that has been allocated tooyour service.</span></span>
<span data-ttu-id="7d7c3-146">Ha az IP-címet gombra kattint, megtekintheti a terheléselosztó mögött futó Nginx tároló.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-146">If you click that IP address, you should see your Nginx container running behind the load balancer.</span></span>

![nginx megtekintése](./media/container-service-kubernetes-ui/nginx-page.png)

### <a name="resizing-your-service"></a><span data-ttu-id="7d7c3-148">A szolgáltatás átméretezése</span><span class="sxs-lookup"><span data-stu-id="7d7c3-148">Resizing your service</span></span>
<span data-ttu-id="7d7c3-149">A hozzáadása tooviewing az objektumok hello felhasználói felület, szerkesztheti és hello Kubernetes API objektumokat frissíteni.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-149">In addition tooviewing your objects in hello UI, you can edit and update hello Kubernetes API objects.</span></span>

<span data-ttu-id="7d7c3-150">Első lépésként kattintson **központi telepítések** a bal oldali navigációs ablaktábla toosee hello szolgáltatás központi telepítését a hello.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-150">First, click **Deployments** in hello left navigation pane toosee hello deployment for your service.</span></span>

<span data-ttu-id="7d7c3-151">Miután a fenti nézetben, kattintson a hello replikakészlethez, majd **szerkesztése** hello felső navigációs sáv:</span><span class="sxs-lookup"><span data-stu-id="7d7c3-151">Once you are in that view, click on hello replica set, and then click **Edit** in hello upper navigation bar:</span></span>

![Kubernetes szerkesztése](./media/container-service-kubernetes-ui/edit.png)

<span data-ttu-id="7d7c3-153">Hello szerkesztése `spec.replicas` mező toobe `2`, és kattintson a **frissítés**.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-153">Edit hello `spec.replicas` field toobe `2`, and click **Update**.</span></span>

<span data-ttu-id="7d7c3-154">Ennek hatására a replikák toodrop tootwo hello száma egy a három munkaállomás-csoporttal.</span><span class="sxs-lookup"><span data-stu-id="7d7c3-154">This causes hello number of replicas toodrop tootwo by deleting one of your pods.</span></span>

 

