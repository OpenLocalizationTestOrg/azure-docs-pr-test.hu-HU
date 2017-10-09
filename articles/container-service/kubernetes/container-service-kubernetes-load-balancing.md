---
title: "aaaLoad Azure Kubernetes a tárolók elosztása |} Microsoft Docs"
description: "Csatlakozás kívülről, és az Azure Tárolószolgáltatásban Kubernetes fürtben több tároló terheléselosztása."
services: container-service
documentationcenter: 
author: dlepow
manager: timlt
editor: 
tags: acs, azure-container-service
keywords: "Tárolók, Micro-szolgáltatások, Kubernetes, Azure"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/17/2017
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: 8073c8d3a015a53a532c326749571cb2582e1bac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="load-balance-containers-in-a-kubernetes-cluster-in-azure-container-service"></a><span data-ttu-id="6dce7-104">Load balance tárolók Kubernetes fürtben az Azure Tárolószolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="6dce7-104">Load balance containers in a Kubernetes cluster in Azure Container Service</span></span> 
<span data-ttu-id="6dce7-105">Ez a cikk az Azure Tárolószolgáltatásban Kubernetes fürtben terheléselosztási be.</span><span class="sxs-lookup"><span data-stu-id="6dce7-105">This article introduces load balancing in a Kubernetes cluster in Azure Container Service.</span></span> <span data-ttu-id="6dce7-106">Terheléselosztás hello szolgáltatást biztosít egy kívülről elérhető IP-címet, majd továbbítja a hello három munkaállomás-csoporttal futó ügynök virtuális gépek közötti hálózati forgalom.</span><span class="sxs-lookup"><span data-stu-id="6dce7-106">Load balancing provides an externally accessible IP address for hello service and distributes network traffic among hello pods running in agent VMs.</span></span>

<span data-ttu-id="6dce7-107">Beállíthat egy Kubernetes szolgáltatás toouse [Azure Load Balancer](../../load-balancer/load-balancer-overview.md) toomanage külső (TCP) hálózati forgalom.</span><span class="sxs-lookup"><span data-stu-id="6dce7-107">You can set up a Kubernetes service toouse [Azure Load Balancer](../../load-balancer/load-balancer-overview.md) toomanage external network (TCP) traffic.</span></span> <span data-ttu-id="6dce7-108">A további konfigurációs terhelés, és a HTTP vagy HTTPS-forgalom vagy speciális forgatókönyvekhez útválasztási is előfordulhatnak.</span><span class="sxs-lookup"><span data-stu-id="6dce7-108">With additional configuration, load balancing and routing of HTTP or HTTPS traffic or more advanced scenarios are possible.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6dce7-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6dce7-109">Prerequisites</span></span>
* <span data-ttu-id="6dce7-110">[Kubernetes fürt központi telepítése](container-service-kubernetes-walkthrough.md) az Azure Tárolószolgáltatásban</span><span class="sxs-lookup"><span data-stu-id="6dce7-110">[Deploy a Kubernetes cluster](container-service-kubernetes-walkthrough.md) in Azure Container Service</span></span>
* <span data-ttu-id="6dce7-111">[Csatlakozás az ügyfél](../container-service-connect.md) tooyour fürt</span><span class="sxs-lookup"><span data-stu-id="6dce7-111">[Connect your client](../container-service-connect.md) tooyour cluster</span></span>

## <a name="azure-load-balancer"></a><span data-ttu-id="6dce7-112">Az Azure terheléselosztó</span><span class="sxs-lookup"><span data-stu-id="6dce7-112">Azure load balancer</span></span>

<span data-ttu-id="6dce7-113">Alapértelmezés szerint az Azure Tárolószolgáltatásban telepített Kubernetes fürt tartozik egy internetre irányuló Azure terheléselosztó hello ügynök virtuális gépeket.</span><span class="sxs-lookup"><span data-stu-id="6dce7-113">By default, a Kubernetes cluster deployed in Azure Container Service includes an Internet-facing Azure load balancer for hello agent VMs.</span></span> <span data-ttu-id="6dce7-114">(Külön terheléselosztó erőforrást hello fő virtuális gépek van konfigurálva.) Az Azure terheléselosztó a réteg 4 terheléselosztó.</span><span class="sxs-lookup"><span data-stu-id="6dce7-114">(A separate load balancer resource is configured for hello master VMs.) Azure load balancer is a Layer 4 load balancer.</span></span> <span data-ttu-id="6dce7-115">Jelenleg hello terheléselosztó csak támogatja a TCP-forgalom a Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="6dce7-115">Currently, hello load balancer only supports TCP traffic in Kubernetes.</span></span>

<span data-ttu-id="6dce7-116">Egy Kubernetes szolgáltatás létrehozásakor hello Azure load balancer tooallow toohello szolgáltatás automatikusan konfigurálhatja.</span><span class="sxs-lookup"><span data-stu-id="6dce7-116">When creating a Kubernetes service, you can automatically configure hello Azure load balancer tooallow access toohello service.</span></span> <span data-ttu-id="6dce7-117">tooconfigure hello terheléselosztó hello szolgáltatás beállítása `type` túl`LoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="6dce7-117">tooconfigure hello load balancer, set hello service `type` too`LoadBalancer`.</span></span> <span data-ttu-id="6dce7-118">hello terheléselosztót hoz létre egy szabály toomap egy nyilvános IP-cím és port bejövő szolgáltatás forgalom toohello privát IP-címek száma és hello három munkaállomás-csoporttal portszámait ügynök virtuális gépek (és fordítva a forgalom válasz).</span><span class="sxs-lookup"><span data-stu-id="6dce7-118">hello load balancer creates a rule toomap a public IP address and port number of incoming service traffic toohello private IP addresses and port numbers of hello pods in agent VMs (and vice versa for response traffic).</span></span> 

 <span data-ttu-id="6dce7-119">Az alábbiakban látható, hogyan tooset hello Kubernetes szolgáltatás két példa `type` túl`LoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="6dce7-119">Following are two examples showing how tooset hello Kubernetes service `type` too`LoadBalancer`.</span></span> <span data-ttu-id="6dce7-120">(Után közben hello példák törölni hello központi telepítések, ha már nem szükséges.)</span><span class="sxs-lookup"><span data-stu-id="6dce7-120">(After trying hello examples, delete hello deployments if you no longer need them.)</span></span>

### <a name="example-use-hello-kubectl-expose-command"></a><span data-ttu-id="6dce7-121">Példa: Használata hello `kubectl expose` parancs</span><span class="sxs-lookup"><span data-stu-id="6dce7-121">Example: Use hello `kubectl expose` command</span></span> 
<span data-ttu-id="6dce7-122">Hello [Kubernetes forgatókönyv](container-service-kubernetes-walkthrough.md) magában foglalja a példa bemutatja, hogyan tooexpose rendelkező hello szolgáltatást `kubectl expose` parancs és annak `--type=LoadBalancer` jelzőt.</span><span class="sxs-lookup"><span data-stu-id="6dce7-122">hello [Kubernetes walkthrough](container-service-kubernetes-walkthrough.md) includes an example of how tooexpose a service with hello `kubectl expose` command and its `--type=LoadBalancer` flag.</span></span> <span data-ttu-id="6dce7-123">Az alábbiakban hello lépéseket:</span><span class="sxs-lookup"><span data-stu-id="6dce7-123">Here are hello steps :</span></span>

1. <span data-ttu-id="6dce7-124">Indítsa el az új tároló központi telepítést.</span><span class="sxs-lookup"><span data-stu-id="6dce7-124">Start a new container deployment.</span></span> <span data-ttu-id="6dce7-125">Például a parancs elindul egy új központi telepítési hívása a következő hello `mynginx`.</span><span class="sxs-lookup"><span data-stu-id="6dce7-125">For example, hello following command starts a new deployment called `mynginx`.</span></span> <span data-ttu-id="6dce7-126">hello központi telepítés három tároló hello Nginx web Server hello Docker-lemezképen alapuló áll.</span><span class="sxs-lookup"><span data-stu-id="6dce7-126">hello deployment consists of three containers based on hello Docker image for hello Nginx web server.</span></span>

    ```console
    kubectl run mynginx --replicas=3 --image nginx
    ```
2. <span data-ttu-id="6dce7-127">Győződjön meg arról, hogy fut-e a hello tárolók.</span><span class="sxs-lookup"><span data-stu-id="6dce7-127">Verify that hello containers are running.</span></span> <span data-ttu-id="6dce7-128">Például, ha hello tárolók kérdezze le `kubectl get pods`, akkor tekintse meg a kimeneti hasonló toohello következőt:</span><span class="sxs-lookup"><span data-stu-id="6dce7-128">For example, if you query for hello containers with `kubectl get pods`, you see output similar toohello following:</span></span>

    ![Nginx tároló beolvasása](./media/container-service-kubernetes-load-balancing/nginx-get-pods.png)

3. <span data-ttu-id="6dce7-130">tooconfigure hello terhelés terheléselosztó tooaccept külső forgalom toohello központi telepítését, futtassa `kubectl expose` rendelkező `--type=LoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="6dce7-130">tooconfigure hello load balancer tooaccept external traffic toohello deployment, run `kubectl expose` with `--type=LoadBalancer`.</span></span> <span data-ttu-id="6dce7-131">hello következő parancsot tesz elérhetővé hello Nginx-kiszolgáló 80-as porton:</span><span class="sxs-lookup"><span data-stu-id="6dce7-131">hello following command exposes hello Nginx server on port 80:</span></span>

    ```console
    kubectl expose deployments mynginx --port=80 --type=LoadBalancer
    ```

4. <span data-ttu-id="6dce7-132">Típus `kubectl get svc` hello szolgáltatásainak hello fürt toosee hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="6dce7-132">Type `kubectl get svc` toosee hello state of hello services in hello cluster.</span></span> <span data-ttu-id="6dce7-133">Amíg hello terheléselosztó hello szabály konfigurálja, hello `EXTERNAL-IP` a hello szolgáltatás jelenik meg `<pending>`.</span><span class="sxs-lookup"><span data-stu-id="6dce7-133">While hello load balancer configures hello rule, hello `EXTERNAL-IP` of hello service appears as `<pending>`.</span></span> <span data-ttu-id="6dce7-134">Néhány perc elteltével hello külső IP-cím van konfigurálva:</span><span class="sxs-lookup"><span data-stu-id="6dce7-134">After a few minutes, hello external IP address is configured:</span></span> 

    ![Az Azure terheléselosztó konfigurálása](./media/container-service-kubernetes-load-balancing/nginx-external-ip.png)

5. <span data-ttu-id="6dce7-136">Győződjön meg arról, hogy van-e hozzáférési hello szolgáltatás hello külső IP-címen.</span><span class="sxs-lookup"><span data-stu-id="6dce7-136">Verify that you can access hello service at hello external IP address.</span></span> <span data-ttu-id="6dce7-137">Például nyisson meg egy webes böngésző toohello IP-cím látható.</span><span class="sxs-lookup"><span data-stu-id="6dce7-137">For example, open a web browser toohello IP address shown.</span></span> <span data-ttu-id="6dce7-138">hello böngésző hello Nginx szolgáltatást futtató kiszolgáló valamelyik hello tárolók jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="6dce7-138">hello browser shows hello Nginx web server running in one of hello containers.</span></span> <span data-ttu-id="6dce7-139">Vagy futtatási hello `curl` vagy `wget` parancsot.</span><span class="sxs-lookup"><span data-stu-id="6dce7-139">Or, run hello `curl` or `wget` command.</span></span> <span data-ttu-id="6dce7-140">Példa:</span><span class="sxs-lookup"><span data-stu-id="6dce7-140">For example:</span></span>

    ```
    curl 13.82.93.130
    ```

    <span data-ttu-id="6dce7-141">A következőhöz hasonló kimenetnek kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="6dce7-141">You should see output similar to:</span></span>

    ![A curl hozzáférés Nginx](./media/container-service-kubernetes-load-balancing/curl-output.png)

6. <span data-ttu-id="6dce7-143">hello Azure terheléselosztó, nyissa meg toohello toosee hello konfigurációja [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6dce7-143">toosee hello configuration of hello Azure load balancer, go toohello [Azure portal](https://portal.azure.com).</span></span>

7. <span data-ttu-id="6dce7-144">Tallózással keresse meg a tárolószolgáltatási fürt hello erőforráscsoportot, és válassza ki a terheléselosztó hello hello ügynök virtuális gépek.</span><span class="sxs-lookup"><span data-stu-id="6dce7-144">Browse for hello resource group for your container service cluster, and select hello load balancer for hello agent VMs.</span></span> <span data-ttu-id="6dce7-145">A név ugyanaz, mint a hello tárolószolgáltatás kell hello.</span><span class="sxs-lookup"><span data-stu-id="6dce7-145">Its name should be hello same as hello container service.</span></span> <span data-ttu-id="6dce7-146">(Válassza ki a terheléselosztó hello hello fő csomópontok, hello, amelynek a neve tartalmaz egy nem **master-lb**.)</span><span class="sxs-lookup"><span data-stu-id="6dce7-146">(Don't choose hello load balancer for hello master nodes, hello one whose name includes **master-lb**.)</span></span> 

    ![Terheléselosztó erőforráscsoportban található](./media/container-service-kubernetes-load-balancing/container-resource-group-portal.png)

8. <span data-ttu-id="6dce7-148">toosee hello részleteit hello terheléselosztó-konfiguráció, kattintson a **terheléselosztási szabályok** és hello szabály konfigurált hello neve.</span><span class="sxs-lookup"><span data-stu-id="6dce7-148">toosee hello details of hello load balancer configuration, click **Load balancing rules** and hello name of hello rule that was configured.</span></span>

    ![Terheléselosztói szabály](./media/container-service-kubernetes-load-balancing/load-balancing-rules.png) 

### <a name="example-specify-type-loadbalancer-in-hello-service-configuration-file"></a><span data-ttu-id="6dce7-150">Például: Adja meg, `type: LoadBalancer` hello szolgáltatás konfigurációs fájljában</span><span class="sxs-lookup"><span data-stu-id="6dce7-150">Example: Specify `type: LoadBalancer` in hello service configuration file</span></span>

<span data-ttu-id="6dce7-151">Ha JSON vagy YAM Kubernetes tároló alkalmazást telepít [szolgáltatás konfigurációs fájlja](https://kubernetes.io/docs/user-guide/services/operations/#service-configuration-file), adjon meg egy külső terheléselosztó a következő sor toohello szolgáltatás meghatározása hello hozzáadásával:</span><span class="sxs-lookup"><span data-stu-id="6dce7-151">If you deploy a Kubernetes container app from a YAML or JSON [service configuration file](https://kubernetes.io/docs/user-guide/services/operations/#service-configuration-file), specify an external load balancer by adding hello following line toohello service specification:</span></span>

```YAML
 "type": "LoadBalancer"
``` 



<span data-ttu-id="6dce7-152">hello következő lépések használják hello Kubernetes [vendégkönyv példa](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook).</span><span class="sxs-lookup"><span data-stu-id="6dce7-152">hello following steps use hello Kubernetes [Guestbook example](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook).</span></span> <span data-ttu-id="6dce7-153">Ez a példa egy többrétegű webalkalmazás Redis és a PHP Docker képek alapján.</span><span class="sxs-lookup"><span data-stu-id="6dce7-153">This example is a multi-tier web app based on  Redis and PHP Docker images.</span></span> <span data-ttu-id="6dce7-154">Hello szolgáltatás konfigurációs fájljában megadhatja, hello előtér PHP kiszolgálón hello Azure terheléselosztó használja.</span><span class="sxs-lookup"><span data-stu-id="6dce7-154">You can specify in hello service configuration file that hello frontend PHP server uses hello Azure load balancer.</span></span>

1. <span data-ttu-id="6dce7-155">Töltse le a hello fájl `guestbook-all-in-one.yaml` a [GitHub](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook/all-in-one).</span><span class="sxs-lookup"><span data-stu-id="6dce7-155">Download hello file `guestbook-all-in-one.yaml` from [GitHub](https://github.com/kubernetes/kubernetes/tree/master/examples/guestbook/all-in-one).</span></span> 
2. <span data-ttu-id="6dce7-156">Tallózással keresse meg a hello `spec` a hello `frontend` szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="6dce7-156">Browse for hello `spec` for hello `frontend` service.</span></span>
3. <span data-ttu-id="6dce7-157">Állítsa vissza a hello sor `type: LoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="6dce7-157">Uncomment hello line `type: LoadBalancer`.</span></span>

    ![Terheléselosztó szolgáltatás konfigurációja](./media/container-service-kubernetes-load-balancing/guestbook-frontend-loadbalance.png)

4. <span data-ttu-id="6dce7-159">Hello fájl mentéséhez és hello alkalmazás telepítése hello a következő parancs futtatásával:</span><span class="sxs-lookup"><span data-stu-id="6dce7-159">Save hello file, and deploy hello app by running hello following command:</span></span>

    ```
    kubectl create -f guestbook-all-in-one.yaml
    ```

5. <span data-ttu-id="6dce7-160">Típus `kubectl get svc` hello szolgáltatásainak hello fürt toosee hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="6dce7-160">Type `kubectl get svc` toosee hello state of hello services in hello cluster.</span></span> <span data-ttu-id="6dce7-161">Amíg hello terheléselosztó hello szabály konfigurálja, hello `EXTERNAL-IP` a hello `frontend` szolgáltatás jelenik meg `<pending>`.</span><span class="sxs-lookup"><span data-stu-id="6dce7-161">While hello load balancer configures hello rule, hello `EXTERNAL-IP` of hello `frontend` service appears as `<pending>`.</span></span> <span data-ttu-id="6dce7-162">Néhány perc elteltével hello külső IP-cím van konfigurálva:</span><span class="sxs-lookup"><span data-stu-id="6dce7-162">After a few minutes, hello external IP address is configured:</span></span> 

    ![Az Azure terheléselosztó konfigurálása](./media/container-service-kubernetes-load-balancing/guestbook-external-ip.png)

6. <span data-ttu-id="6dce7-164">Győződjön meg arról, hogy van-e hozzáférési hello szolgáltatás hello külső IP-címen.</span><span class="sxs-lookup"><span data-stu-id="6dce7-164">Verify that you can access hello service at hello external IP address.</span></span> <span data-ttu-id="6dce7-165">Például egy webes böngésző toohello külső IP-cím hello szolgáltatást is megnyithatja.</span><span class="sxs-lookup"><span data-stu-id="6dce7-165">For example, you can open a web browser toohello external IP address of hello service.</span></span>

    ![Külső hozzáférés vendégkönyv](./media/container-service-kubernetes-load-balancing/guestbook-web.png)

    <span data-ttu-id="6dce7-167">Vendégkönyv bejegyzést adhat hozzá.</span><span class="sxs-lookup"><span data-stu-id="6dce7-167">You can add guestbook entries.</span></span>

7. <span data-ttu-id="6dce7-168">hello Azure terheléselosztó, hello terheléselosztó erőforrást tallózzon a hello hello fürt toosee hello konfigurációja [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6dce7-168">toosee hello configuration of hello Azure load balancer, browse for hello load balancer resource for hello cluster in hello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="6dce7-169">Lásd: hello hello előző példában szereplő lépéseket.</span><span class="sxs-lookup"><span data-stu-id="6dce7-169">See hello steps in hello previous example.</span></span>

### <a name="considerations"></a><span data-ttu-id="6dce7-170">Megfontolandó szempontok</span><span class="sxs-lookup"><span data-stu-id="6dce7-170">Considerations</span></span>

* <span data-ttu-id="6dce7-171">Hello terheléselosztási szabály létrehozása aszinkron módon történik, és a kiépített hello terheléselosztó információt közzé van téve, hello szolgáltatásban `status.loadBalancer` mező.</span><span class="sxs-lookup"><span data-stu-id="6dce7-171">Creation of hello load balancer rule happens asynchronously, and information about hello provisioned balancer is published in hello service’s `status.loadBalancer` field.</span></span>
* <span data-ttu-id="6dce7-172">Minden szolgáltatás automatikusan saját hello terheléselosztó virtuális IP-címhez.</span><span class="sxs-lookup"><span data-stu-id="6dce7-172">Every service is automatically assigned its own virtual IP address in hello load balancer.</span></span>
* <span data-ttu-id="6dce7-173">Ha a DNS-név tooreach hello terheléselosztóhoz, dolgozni a tartományi szolgáltatás szolgáltató toocreate hello szabály IP-címet a DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="6dce7-173">If you want tooreach hello load balancer by a DNS name, work with your domain service provider toocreate a DNS name for hello rule's IP address.</span></span>

## <a name="http-or-https-traffic"></a><span data-ttu-id="6dce7-174">HTTP vagy HTTPS-forgalom</span><span class="sxs-lookup"><span data-stu-id="6dce7-174">HTTP or HTTPS traffic</span></span>

<span data-ttu-id="6dce7-175">tooload egyenleg HTTP vagy HTTPS-forgalom toocontainer webalkalmazások és kezelheti a transport layer security (TLS) tanúsítványait, használhatja a hello Kubernetes [érkező](https://kubernetes.io/docs/user-guide/ingress/) erőforrás.</span><span class="sxs-lookup"><span data-stu-id="6dce7-175">tooload balance HTTP or HTTPS traffic toocontainer web apps and manage certificates for transport layer security (TLS), you can use hello Kubernetes [Ingress](https://kubernetes.io/docs/user-guide/ingress/) resource.</span></span> <span data-ttu-id="6dce7-176">Egy érkező olyan szabályok, amelyek lehetővé teszik a bejövő kapcsolatok tooreach hello fürtszolgáltatások gyűjteménye.</span><span class="sxs-lookup"><span data-stu-id="6dce7-176">An Ingress is a collection of rules that allow inbound connections tooreach hello cluster services.</span></span> <span data-ttu-id="6dce7-177">Egy érkező erőforrás toowork hello Kubernetes fürt rendelkeznie kell egy [érkező vezérlő](https://kubernetes.io/docs/user-guide/ingress/#ingress-controllers) futtatása.</span><span class="sxs-lookup"><span data-stu-id="6dce7-177">For an Ingress resource toowork, hello Kubernetes cluster must have an [Ingress controller](https://kubernetes.io/docs/user-guide/ingress/#ingress-controllers) running.</span></span>

<span data-ttu-id="6dce7-178">Az Azure Tárolószolgáltatás nem valósítja meg a Kubernetes érkező vezérlő automatikusan.</span><span class="sxs-lookup"><span data-stu-id="6dce7-178">Azure Container Service does not implement a Kubernetes Ingress controller automatically.</span></span> <span data-ttu-id="6dce7-179">Több vezérlő megvalósítások érhetők el.</span><span class="sxs-lookup"><span data-stu-id="6dce7-179">Several controller implementations are available.</span></span> <span data-ttu-id="6dce7-180">Jelenleg hello [Nginx érkező vezérlő](https://github.com/kubernetes/ingress/tree/master/examples/deployment/nginx) javasolt tooconfigure érkező szabályok és a terheléselosztás HTTP és HTTPS-forgalmat.</span><span class="sxs-lookup"><span data-stu-id="6dce7-180">Currently, hello [Nginx Ingress controller](https://github.com/kubernetes/ingress/tree/master/examples/deployment/nginx) is recommended tooconfigure Ingress rules and load balance HTTP and HTTPS traffic.</span></span> 

<span data-ttu-id="6dce7-181">További információkért lásd: hello [Nginx érkező dokumentációjában](https://github.com/kubernetes/ingress/tree/master/controllers/nginx/README.md).</span><span class="sxs-lookup"><span data-stu-id="6dce7-181">For more information, see hello [Nginx Ingress controller documentation](https://github.com/kubernetes/ingress/tree/master/controllers/nginx/README.md).</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6dce7-182">Hello Nginx érkező vezérlő használata az Azure Tárolószolgáltatásban, fel kell fednie hello tartományvezérlő központi telepítése a szolgáltatásként `type: LoadBalancer`.</span><span class="sxs-lookup"><span data-stu-id="6dce7-182">When using hello Nginx Ingress controller in Azure Container Service, you must expose hello controller deployment as a service with `type: LoadBalancer`.</span></span> <span data-ttu-id="6dce7-183">Ez konfigurálja az hello Azure load balancer tooroute forgalom toohello vezérlő.</span><span class="sxs-lookup"><span data-stu-id="6dce7-183">This configures hello Azure load balancer tooroute traffic toohello controller.</span></span> <span data-ttu-id="6dce7-184">További információkért lásd: hello előző szakaszban.</span><span class="sxs-lookup"><span data-stu-id="6dce7-184">For more information, see hello previous section.</span></span>


## <a name="next-steps"></a><span data-ttu-id="6dce7-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6dce7-185">Next steps</span></span>

* <span data-ttu-id="6dce7-186">Lásd: hello [Kubernetes terheléselosztó dokumentáció](https://kubernetes.io/docs/user-guide/load-balancer/)</span><span class="sxs-lookup"><span data-stu-id="6dce7-186">See hello [Kubernetes LoadBalancer documentation](https://kubernetes.io/docs/user-guide/load-balancer/)</span></span>
* <span data-ttu-id="6dce7-187">További információ [Kubernetes érkező és a bejövő adatok tartományvezérlők](https://kubernetes.io/docs/user-guide/ingress/)</span><span class="sxs-lookup"><span data-stu-id="6dce7-187">Learn more about [Kubernetes Ingress and Ingress controllers](https://kubernetes.io/docs/user-guide/ingress/)</span></span>
* <span data-ttu-id="6dce7-188">Lásd: [Kubernetes példák](https://github.com/kubernetes/kubernetes/tree/master/examples)</span><span class="sxs-lookup"><span data-stu-id="6dce7-188">See [Kubernetes examples](https://github.com/kubernetes/kubernetes/tree/master/examples)</span></span>

