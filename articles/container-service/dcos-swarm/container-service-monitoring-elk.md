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
# <a name="monitor-an-azure-container-service-cluster-with-elk"></a>A figyelő egy ELK az Azure Tárolószolgáltatás-fürt
Ebben a cikkben bemutatjuk, hogyan toodeploy hello ELK (Elasticsearch, Logstash, Kibana) verem az Azure Tárolószolgáltatásban DC/OS-fürtön. 

## <a name="prerequisites"></a>Előfeltételek
[Telepítése](container-service-deployment.md) és [csatlakozás](../container-service-connect.md) egy Azure Tárolószolgáltatásban konfigurált DC/OS-fürtben. Megismerkedhet a hello DC/OS-irányítópult és a Marathon szolgáltatások [Itt](container-service-mesos-marathon-ui.md). Is telepítheti a hello [Marathon terheléselosztó](container-service-load-balancing.md).


## <a name="elk-elasticsearch-logstash-kibana"></a>ELK (Elasticsearch, Logstash, Kibana)
ELK verem Elasticsearch, Logstash, kombinációja és, hogy egy záró tooend verem előírja, hogy Kibana is használt toomonitor és a fürt naplóinak elemzése.

## <a name="configure-hello-elk-stack-on-a-dcos-cluster"></a>A DC/OS-fürtről hello ELK verem konfigurálása
A DC/OS felhasználói felületén keresztül elérni [http://localhost:80 /](http://localhost:80/) hello DC/OS felhasználói felületének túl keresse meg az egyszer**Universe**. Keressen, és telepítse a Elasticsearch Logstash és Kibana, a DC/OS Universe hello és meghatározott sorrendben. További konfigurációjával kapcsolatos Ha toohello **speciális telepítési** hivatkozásra.

![ELK1](./media/container-service-monitoring-elk/elk1.PNG) ![ELK2](./media/container-service-monitoring-elk/elk2.PNG) ![ELK3](./media/container-service-monitoring-elk/elk3.PNG) 

Egyszer hello ELK tárolók és a használatra, tooenable Kibana toobe Marathon-LB keresztül érhető el. Keresse meg a túl **szolgáltatások** > **kibana**, és kattintson a **szerkesztése** alább látható módon.

![ELK4](./media/container-service-monitoring-elk/elk4.PNG)


Váltás a túl**JSON üzemmódot** és toohello címkék szakasz görgetve.
Tooadd van szüksége egy `"HAPROXY_GROUP": "external"` bejegyzés itt látható az alábbi.
Miután rákattintott **módosítása**, a tároló újraindul.

![ELK5](./media/container-service-monitoring-elk/elk5.PNG)


Ha azt szeretné, hogy tooverify adott Kibana regisztrálva van szolgáltatásként hello haproxy esetén irányítópulton, portra van szüksége tooopen 9090 hello ügynök fürtön, a port 9090 futó haproxy esetén.
Alapértelmezés szerint azt portok megnyitása 80, 8080-as, és a 443-as hello DC/OS-ügynök fürtben.
Útmutatás tooopen egy portot, és adja meg a nyilvános értékeléséhez biztosított [Itt](container-service-enable-public-access.md).

tooaccess hello haproxy esetén az irányítópult nyitott hello Marathon-LB admin illesztő: `http://$PUBLIC_NODE_IP_ADDRESS:9090/haproxy?stats`.
Miután toohello URL-cím keresse meg, megtekintheti az hello haproxy esetén irányítópult alább látható módon, és megjelenik egy tételt a Kibana.

![ELK6](./media/container-service-monitoring-elk/elk6.PNG)


port 5601 van telepítve, tooaccess hello Kibana irányítópult portra van szüksége tooopen 5601. Kövesse az utasításokat [Itt](container-service-enable-public-access.md). Nyissa meg a hello Kibana irányítópult: `http://localhost:5601`.

## <a name="next-steps"></a>Következő lépések

* Rendszer- és napló továbbítás és a telepítés [napló kezelése a DC/OS rendelkező ELK](https://docs.mesosphere.com/1.8/administration/logging/elk/).

* toofilter naplókat, lásd: [naplók szűrése és ELK](https://docs.mesosphere.com/1.8/administration/logging/filter-elk/). 

 

