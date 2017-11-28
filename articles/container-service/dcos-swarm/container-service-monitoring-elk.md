---
title: "egy Azure DC/OS-fürtről - ELK verem aaaMonitor |} Microsoft Docs"
description: "A DC/OS fürtben Azure Tárolószolgáltatás-fürt ELK (Elasticsearch Logstash és Kibana) a figyelheti."
services: container-service
documentationcenter: 
author: sauryadas
manager: madhana
editor: 
tags: acs, azure-container-service
keywords: "Tárolók, a DC/OS, Azure, figyelés, elk"
ms.assetid: 
ms.service: container-service
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/27/2017
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 8d81c5342616ec14880d38803cdf95f5845a669b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-an-azure-container-service-cluster-with-elk"></a><span data-ttu-id="21681-104">A figyelő egy ELK az Azure Tárolószolgáltatás-fürt</span><span class="sxs-lookup"><span data-stu-id="21681-104">Monitor an Azure Container Service cluster with ELK</span></span>
<span data-ttu-id="21681-105">Ebben a cikkben bemutatjuk, hogyan toodeploy hello ELK (Elasticsearch, Logstash, Kibana) verem az Azure Tárolószolgáltatásban DC/OS-fürtön.</span><span class="sxs-lookup"><span data-stu-id="21681-105">In this article, we demonstrate how toodeploy hello ELK (Elasticsearch, Logstash, Kibana) stack on a DC/OS cluster in Azure Container Service.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="21681-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="21681-106">Prerequisites</span></span>
<span data-ttu-id="21681-107">[Telepítése](container-service-deployment.md) és [csatlakozás](../container-service-connect.md) egy Azure Tárolószolgáltatásban konfigurált DC/OS-fürtben.</span><span class="sxs-lookup"><span data-stu-id="21681-107">[Deploy](container-service-deployment.md) and [connect](../container-service-connect.md) a DC/OS cluster configured by Azure Container Service.</span></span> <span data-ttu-id="21681-108">Megismerkedhet a hello DC/OS-irányítópult és a Marathon szolgáltatások [Itt](container-service-mesos-marathon-ui.md).</span><span class="sxs-lookup"><span data-stu-id="21681-108">Explore hello DC/OS dashboard and Marathon services [here](container-service-mesos-marathon-ui.md).</span></span> <span data-ttu-id="21681-109">Is telepítheti a hello [Marathon terheléselosztó](container-service-load-balancing.md).</span><span class="sxs-lookup"><span data-stu-id="21681-109">Also install hello [Marathon Load Balancer](container-service-load-balancing.md).</span></span>


## <a name="elk-elasticsearch-logstash-kibana"></a><span data-ttu-id="21681-110">ELK (Elasticsearch, Logstash, Kibana)</span><span class="sxs-lookup"><span data-stu-id="21681-110">ELK (Elasticsearch, Logstash, Kibana)</span></span>
<span data-ttu-id="21681-111">ELK verem Elasticsearch, Logstash, kombinációja és, hogy egy záró tooend verem előírja, hogy Kibana is használt toomonitor és a fürt naplóinak elemzése.</span><span class="sxs-lookup"><span data-stu-id="21681-111">ELK stack is a combination of Elasticsearch, Logstash, and Kibana that provides an end tooend stack that can be used toomonitor and analyze logs in your cluster.</span></span>

## <a name="configure-hello-elk-stack-on-a-dcos-cluster"></a><span data-ttu-id="21681-112">A DC/OS-fürtről hello ELK verem konfigurálása</span><span class="sxs-lookup"><span data-stu-id="21681-112">Configure hello ELK stack on a DC/OS cluster</span></span>
<span data-ttu-id="21681-113">A DC/OS felhasználói felületén keresztül elérni [http://localhost:80 /](http://localhost:80/) hello DC/OS felhasználói felületének túl keresse meg az egyszer**Universe**.</span><span class="sxs-lookup"><span data-stu-id="21681-113">Access your DC/OS UI via [http://localhost:80/](http://localhost:80/) Once in hello DC/OS UI navigate too**Universe**.</span></span> <span data-ttu-id="21681-114">Keressen, és telepítse a Elasticsearch Logstash és Kibana, a DC/OS Universe hello és meghatározott sorrendben.</span><span class="sxs-lookup"><span data-stu-id="21681-114">Search and install Elasticsearch, Logstash, and Kibana from hello DC/OS Universe and in that specific order.</span></span> <span data-ttu-id="21681-115">További konfigurációjával kapcsolatos Ha toohello **speciális telepítési** hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="21681-115">You can learn more about configuration if you go toohello **Advanced Installation** link.</span></span>

![ELK1](./media/container-service-monitoring-elk/elk1.PNG) ![ELK2](./media/container-service-monitoring-elk/elk2.PNG) ![ELK3](./media/container-service-monitoring-elk/elk3.PNG) 

<span data-ttu-id="21681-119">Egyszer hello ELK tárolók és a használatra, tooenable Kibana toobe Marathon-LB keresztül érhető el.</span><span class="sxs-lookup"><span data-stu-id="21681-119">Once hello ELK containers and are up and running, you need tooenable Kibana toobe accessed through Marathon-LB.</span></span> <span data-ttu-id="21681-120">Keresse meg a túl **szolgáltatások** > **kibana**, és kattintson a **szerkesztése** alább látható módon.</span><span class="sxs-lookup"><span data-stu-id="21681-120">Navigate too **Services** > **kibana**, and click **Edit** as shown below.</span></span>

![ELK4](./media/container-service-monitoring-elk/elk4.PNG)


<span data-ttu-id="21681-122">Váltás a túl**JSON üzemmódot** és toohello címkék szakasz görgetve.</span><span class="sxs-lookup"><span data-stu-id="21681-122">Toggle too**JSON mode** and scroll down toohello labels section.</span></span>
<span data-ttu-id="21681-123">Tooadd van szüksége egy `"HAPROXY_GROUP": "external"` bejegyzés itt látható az alábbi.</span><span class="sxs-lookup"><span data-stu-id="21681-123">You need tooadd a `"HAPROXY_GROUP": "external"` entry here as shown below.</span></span>
<span data-ttu-id="21681-124">Miután rákattintott **módosítása**, a tároló újraindul.</span><span class="sxs-lookup"><span data-stu-id="21681-124">Once you click **Deploy changes**, your container restarts.</span></span>

![ELK5](./media/container-service-monitoring-elk/elk5.PNG)


<span data-ttu-id="21681-126">Ha azt szeretné, hogy tooverify adott Kibana regisztrálva van szolgáltatásként hello haproxy esetén irányítópulton, portra van szüksége tooopen 9090 hello ügynök fürtön, a port 9090 futó haproxy esetén.</span><span class="sxs-lookup"><span data-stu-id="21681-126">If you want tooverify that Kibana is registered as a service in hello HAPROXY dashboard, you need tooopen port 9090 on hello agent cluster as HAPROXY runs on port 9090.</span></span>
<span data-ttu-id="21681-127">Alapértelmezés szerint azt portok megnyitása 80, 8080-as, és a 443-as hello DC/OS-ügynök fürtben.</span><span class="sxs-lookup"><span data-stu-id="21681-127">By default, we open ports 80, 8080, and 443 in hello DC/OS agent cluster.</span></span>
<span data-ttu-id="21681-128">Útmutatás tooopen egy portot, és adja meg a nyilvános értékeléséhez biztosított [Itt](container-service-enable-public-access.md).</span><span class="sxs-lookup"><span data-stu-id="21681-128">Instructions tooopen a port and provide public assess are provided [here](container-service-enable-public-access.md).</span></span>

<span data-ttu-id="21681-129">tooaccess hello haproxy esetén az irányítópult nyitott hello Marathon-LB admin illesztő: `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats`.</span><span class="sxs-lookup"><span data-stu-id="21681-129">tooaccess hello HAPROXY dashboard, open hello Marathon-LB admin interface at: `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats`.</span></span>
<span data-ttu-id="21681-130">Miután toohello URL-cím keresse meg, megtekintheti az hello haproxy esetén irányítópult alább látható módon, és megjelenik egy tételt a Kibana.</span><span class="sxs-lookup"><span data-stu-id="21681-130">Once you navigate toohello URL, you should see hello HAPROXY dashboard as shown below and you should see a service entry for Kibana.</span></span>

![ELK6](./media/container-service-monitoring-elk/elk6.PNG)


<span data-ttu-id="21681-132">port 5601 van telepítve, tooaccess hello Kibana irányítópult portra van szüksége tooopen 5601.</span><span class="sxs-lookup"><span data-stu-id="21681-132">tooaccess hello Kibana dashboard, which is deployed on port 5601, you need tooopen port 5601.</span></span> <span data-ttu-id="21681-133">Kövesse az utasításokat [Itt](container-service-enable-public-access.md).</span><span class="sxs-lookup"><span data-stu-id="21681-133">Follow instructions [here](container-service-enable-public-access.md).</span></span> <span data-ttu-id="21681-134">Nyissa meg a hello Kibana irányítópult: `http://localhost:5601`.</span><span class="sxs-lookup"><span data-stu-id="21681-134">Then open hello Kibana dashboard at: `http://localhost:5601`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="21681-135">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="21681-135">Next steps</span></span>

* <span data-ttu-id="21681-136">Rendszer- és napló továbbítás és a telepítés [napló kezelése a DC/OS rendelkező ELK](https://docs.mesosphere.com/1.8/administration/logging/elk/).</span><span class="sxs-lookup"><span data-stu-id="21681-136">For system and application log forwarding and setup, see [Log Management in DC/OS with ELK](https://docs.mesosphere.com/1.8/administration/logging/elk/).</span></span>

* <span data-ttu-id="21681-137">toofilter naplókat, lásd: [naplók szűrése és ELK](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/).</span><span class="sxs-lookup"><span data-stu-id="21681-137">toofilter logs, see [Filtering Logs with ELK](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/).</span></span> 

 

