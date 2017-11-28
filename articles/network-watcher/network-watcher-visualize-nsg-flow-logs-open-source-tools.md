---
title: "Azure hálózati figyelő NSG folyamat aaaVisualize naplózza, nyílt forráskódú eszközökkel |} Microsoft Docs"
description: "Ez a lap ismerteti, hogyan toouse forrás eszközök toovisualize NSG folyamata naplófájlok megnyitása."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: e9b2dcad-4da4-4d6b-aee2-6d0afade0cb8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 47cb529d4a1e00e8c4c0fa6885cbf72aed3e74c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-azure-network-watcher-nsg-flow-logs-using-open-source-tools"></a><span data-ttu-id="020b2-103">Azure hálózati figyelő NSG folyamata naplók nyílt forráskódú eszközökkel megjelenítése</span><span class="sxs-lookup"><span data-stu-id="020b2-103">Visualize Azure Network Watcher NSG flow logs using open source tools</span></span>

<span data-ttu-id="020b2-104">Hálózati biztonsági csoport folyamata naplók információkkal használható IP-bemenő és kimenő forgalmat a hálózati biztonsági csoportok ismertetése.</span><span class="sxs-lookup"><span data-stu-id="020b2-104">Network Security Group flow logs provide information that can be used understand ingress and egress IP traffic on Network Security Groups.</span></span> <span data-ttu-id="020b2-105">A folyamat naplók megjelenítése kimenő és bejövő adatfolyamok / szabály alapján, hello NIC hello folyamata vonatkozik, hello folyamata (forrás vagy a cél IP, forrás vagy a cél Port Protocol), és ha hello forgalom lett engedélyezett vagy megtagadott 5 rekordos információt.</span><span class="sxs-lookup"><span data-stu-id="020b2-105">These flow logs show outbound and inbound flows on a per rule basis, hello NIC hello flow applies to, 5 tuple information about hello flow (Source/Destination IP, Source/Destination Port, Protocol), and if hello traffic was allowed or denied.</span></span>

<span data-ttu-id="020b2-106">A folyamat naplók nehéz toomanually elemzési kell, és a dcu.</span><span class="sxs-lookup"><span data-stu-id="020b2-106">These flow logs can be difficult toomanually parse and gain insights from.</span></span> <span data-ttu-id="020b2-107">Vannak azonban számos nyílt forráskódú eszköz, amelynek segítségével jelenítheti meg ezeket az adatokat.</span><span class="sxs-lookup"><span data-stu-id="020b2-107">However, there are several open source tools that can help visualize this data.</span></span> <span data-ttu-id="020b2-108">Ez a cikk nyújt a megoldás toovisualize a naplók hello rugalmas készlet használatával, amely lehetővé teszi tooquickly index és a folyamat a naplók Kibana irányítópult megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="020b2-108">This article will provide a solution toovisualize these logs using hello Elastic Stack, which will allow you tooquickly index and visualize your flow logs on a Kibana dashboard.</span></span>

## <a name="scenario"></a><span data-ttu-id="020b2-109">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="020b2-109">Scenario</span></span>

<span data-ttu-id="020b2-110">Ebben a cikkben egy megoldást, amely lehetővé teszi a toovisualize hálózati biztonsági csoport folyamat használatával a naplókat, a rugalmas készlet hello üzembe helyezünk.</span><span class="sxs-lookup"><span data-stu-id="020b2-110">In this article, we will set up a solution that will allow you toovisualize Network Security Group flow logs using hello Elastic Stack.</span></span>  <span data-ttu-id="020b2-111">Egy Logstash bemeneti beépülő modul beszerzi hello folyamata naplók közvetlenül hello tárolási blob hello folyamata naplókat tartalmazó konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="020b2-111">A Logstash input plugin will obtain hello flow logs directly from hello storage blob configured for containing hello flow logs.</span></span> <span data-ttu-id="020b2-112">Ezt követően hello rugalmas készlet használatával, hello folyamata naplók lesz indexelve és toocreate egy Kibana irányítópult toovisualize hello információkat használja.</span><span class="sxs-lookup"><span data-stu-id="020b2-112">Then, using hello Elastic Stack, hello flow logs will be indexed and used toocreate a Kibana dashboard toovisualize hello information.</span></span>

![forgatókönyv][scenario]

## <a name="steps"></a><span data-ttu-id="020b2-114">Lépések</span><span class="sxs-lookup"><span data-stu-id="020b2-114">Steps</span></span>

### <a name="enable-network-security-group-flow-logging"></a><span data-ttu-id="020b2-115">Engedélyezze a hálózati biztonsági csoport folyamata naplózás</span><span class="sxs-lookup"><span data-stu-id="020b2-115">Enable Network Security Group flow logging</span></span>
<span data-ttu-id="020b2-116">Ebben az esetben rendelkeznie kell hálózati biztonsági csoport Flow naplózás legalább egy hálózati biztonsági csoport a fiók engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="020b2-116">For this scenario, you must have Network Security Group Flow Logging enabled on at least one Network Security Group in your account.</span></span> <span data-ttu-id="020b2-117">A hálózati biztonsági Flow naplók engedélyezése útmutatásért tekintse meg a következő cikket toohello [bemutatása tooflow naplózási a hálózati biztonsági csoportok](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="020b2-117">For instructions on enabling Network Security Flow Logs, refer toohello following article [Introduction tooflow logging for Network Security Groups](network-watcher-nsg-flow-logging-overview.md).</span></span>


### <a name="set-up-hello-elastic-stack"></a><span data-ttu-id="020b2-118">A rugalmas készlet hello beállítása</span><span class="sxs-lookup"><span data-stu-id="020b2-118">Set up hello Elastic Stack</span></span>
<span data-ttu-id="020b2-119">NSG csatlakozzon a naplók és a rugalmas készlet hello flow, jelenleg milyen kiválaszthatjuk toosearch Kibana irányítópult létrehozása, diagramot, elemzése és elemzések származik a naplókat.</span><span class="sxs-lookup"><span data-stu-id="020b2-119">By connecting NSG flow logs with hello Elastic Stack, we can create a Kibana dashboard what allows us toosearch, graph, analyze, and derive insights from our logs.</span></span>

#### <a name="install-elasticsearch"></a><span data-ttu-id="020b2-120">Elasticsearch telepítése</span><span class="sxs-lookup"><span data-stu-id="020b2-120">Install Elasticsearch</span></span>

1. <span data-ttu-id="020b2-121">hello rugalmas verem 5.0-s verziójáról és a fent Java 8 igényel.</span><span class="sxs-lookup"><span data-stu-id="020b2-121">hello Elastic Stack from version 5.0 and above requires Java 8.</span></span> <span data-ttu-id="020b2-122">Hello paranccsal `java -version` toocheck adott verziójában.</span><span class="sxs-lookup"><span data-stu-id="020b2-122">Run hello command `java -version` toocheck your version.</span></span> <span data-ttu-id="020b2-123">Ha nem kell telepíteni, tekintse meg a toodocumentation a java [Oracle-webhely](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span><span class="sxs-lookup"><span data-stu-id="020b2-123">If you do not have java install, refer toodocumentation on [Oracle's website](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span></span>
1. <span data-ttu-id="020b2-124">Töltse le a hello helyes bináris csomagot a rendszer:</span><span class="sxs-lookup"><span data-stu-id="020b2-124">Download hello correct binary package for your system:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    <span data-ttu-id="020b2-125">Egyéb módszerek található [Elasticsearch telepítése](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span><span class="sxs-lookup"><span data-stu-id="020b2-125">Other installation methods can be found at [Elasticsearch Installation](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span></span>

1. <span data-ttu-id="020b2-126">Győződjön meg arról, hogy fut-e Elasticsearch hello paranccsal:</span><span class="sxs-lookup"><span data-stu-id="020b2-126">Verify that Elasticsearch is running with hello command:</span></span>

    ```
    curl http://127.0.0.1:9200
    ```

    <span data-ttu-id="020b2-127">A válasz hasonló toothis kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="020b2-127">You should see a response similar toothis:</span></span>

    ```
    {
    "name" : "Angela Del Toro",
    "cluster_name" : "elasticsearch",
    "version" : {
        "number" : "5.2.0",
        "build_hash" : "8ff36d139e16f8720f2947ef62c8167a888992fe",
        "build_timestamp" : "2016-01-27T13:32:39Z",
        "build_snapshot" : false,
        "lucene_version" : "6.1.0"
    },
    "tagline" : "You Know, for Search"
    }
    ```

<span data-ttu-id="020b2-128">Rugalmas keresési telepítése további útmutatásra van szüksége, tekintse meg a toohello lap [telepítése](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span><span class="sxs-lookup"><span data-stu-id="020b2-128">For further instructions on installing Elastic search, refer toohello page [Installation](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span></span>

### <a name="install-logstash"></a><span data-ttu-id="020b2-129">Logstash telepítése</span><span class="sxs-lookup"><span data-stu-id="020b2-129">Install Logstash</span></span>

1. <span data-ttu-id="020b2-130">tooinstall Logstash hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="020b2-130">tooinstall Logstash run hello following commands:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. <span data-ttu-id="020b2-131">Ezután azt kell tooconfigure Logstash tooaccess, és hello folyamata naplók elemzése.</span><span class="sxs-lookup"><span data-stu-id="020b2-131">Next we need tooconfigure Logstash tooaccess and parse hello flow logs.</span></span> <span data-ttu-id="020b2-132">Hozzon létre egy logstash.conf fájl használatával:</span><span class="sxs-lookup"><span data-stu-id="020b2-132">Create a logstash.conf file using:</span></span>

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. <span data-ttu-id="020b2-133">Adja hozzá a következő tartalom toohello fájl hello:</span><span class="sxs-lookup"><span data-stu-id="020b2-133">Add hello following content toohello file:</span></span>

  ```
    input {
      azureblob
        {
            storage_account_name => "mystorageaccount"
            storage_access_key => "storageaccesskey"
            container => "nsgflowlogContainerName"
            codec => "json"
        }
      }

      filter {
        split { field => "[records]" }
        split { field => "[records][properties][flows]"}
        split { field => "[records][properties][flows][flows]"}
        split { field => "[records][properties][flows][flows][flowTuples]"}

     mutate{
      split => { "[records][resourceId]" => "/"}
      add_field => {"Subscription" => "%{[records][resourceId][2]}"
                    "ResourceGroup" => "%{[records][resourceId][4]}"
                    "NetworkSecurityGroup" => "%{[records][resourceId][8]}"}
      convert => {"Subscription" => "string"}
      convert => {"ResourceGroup" => "string"}
      convert => {"NetworkSecurityGroup" => "string"}
      split => { "[records][properties][flows][flows][flowTuples]" => ","}
      add_field => {
                  "unixtimestamp" => "%{[records][properties][flows][flows][flowTuples][0]}"
                  "srcIp" => "%{[records][properties][flows][flows][flowTuples][1]}"
                  "destIp" => "%{[records][properties][flows][flows][flowTuples][2]}"
                  "srcPort" => "%{[records][properties][flows][flows][flowTuples][3]}"
                  "destPort" => "%{[records][properties][flows][flows][flowTuples][4]}"
                  "protocol" => "%{[records][properties][flows][flows][flowTuples][5]}"
                  "trafficflow" => "%{[records][properties][flows][flows][flowTuples][6]}"
                  "traffic" => "%{[records][properties][flows][flows][flowTuples][7]}"
                   }
      convert => {"unixtimestamp" => "integer"}
      convert => {"srcPort" => "integer"}
      convert => {"destPort" => "integer"}        
     }

     date{
       match => ["unixtimestamp" , "UNIX"]
     }
    }

    output {
      stdout { codec => rubydebug }
      elasticsearch {
        hosts => "localhost"
        index => "nsg-flow-logs"
      }
    }  

  ```

<span data-ttu-id="020b2-134">További telepítésével kapcsolatos utasításokat Logstash, tekintse meg a toohello [dokumentációs](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span><span class="sxs-lookup"><span data-stu-id="020b2-134">For further instructions on installing Logstash, refer toohello [official documentation](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span></span>

### <a name="install-hello-logstash-input-plugin-for-azure-blob-storage"></a><span data-ttu-id="020b2-135">Hello Logstash bemeneti beépülő modul az Azure blob Storage telepítése</span><span class="sxs-lookup"><span data-stu-id="020b2-135">Install hello Logstash input plugin for Azure blob storage</span></span>

<span data-ttu-id="020b2-136">A Logstash beépülő modul lehetővé teszi a toodirectly belépési hello folyamata naplók a kijelölt tárfiókból.</span><span class="sxs-lookup"><span data-stu-id="020b2-136">This Logstash plugin will allow you toodirectly access hello flow logs from their designated storage account.</span></span> <span data-ttu-id="020b2-137">tooinstall a beépülő modul hello alapértelmezett Logstash telepítési könyvtárában (az az eset /usr/share/logstash/bin) parancsot hello:</span><span class="sxs-lookup"><span data-stu-id="020b2-137">tooinstall this plugin, from hello default Logstash installation directory (in this case /usr/share/logstash/bin) run hello command:</span></span>

```
logstash-plugin install logstash-input-azureblob
```

<span data-ttu-id="020b2-138">toostart Logstash hello paranccsal:</span><span class="sxs-lookup"><span data-stu-id="020b2-138">toostart Logstash run hello command:</span></span>

```
sudo /etc/init.d/logstash start
```

<span data-ttu-id="020b2-139">A beépülő modul kapcsolatos további információkért tekintse meg a toodocumentation [Itt](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)</span><span class="sxs-lookup"><span data-stu-id="020b2-139">For more information about this plugin, refer toodocumentation [here](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)</span></span>

### <a name="install-kibana"></a><span data-ttu-id="020b2-140">Kibana telepítése</span><span class="sxs-lookup"><span data-stu-id="020b2-140">Install Kibana</span></span>

1. <span data-ttu-id="020b2-141">Futtassa a következő parancsok tooinstall Kibana hello:</span><span class="sxs-lookup"><span data-stu-id="020b2-141">Run hello following commands tooinstall Kibana:</span></span>

  ```
  curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
  tar xzvf kibana-5.2.0-linux-x86_64.tar.gz
  ```

1. <span data-ttu-id="020b2-142">toorun Kibana hello parancsokat használja:</span><span class="sxs-lookup"><span data-stu-id="020b2-142">toorun Kibana use hello commands:</span></span>

  ```
  cd kibana-5.2.0-linux-x86_64/
  ./bin/kibana
  ```

1. <span data-ttu-id="020b2-143">tooview a Kibana webes felület, keresse meg a túl`http://localhost:5601`</span><span class="sxs-lookup"><span data-stu-id="020b2-143">tooview your Kibana web interface, navigate too`http://localhost:5601`</span></span>
1. <span data-ttu-id="020b2-144">Ebben a forgatókönyvben hello index hello folyamat használható a mintája "nsg-adatfolyam-logs".</span><span class="sxs-lookup"><span data-stu-id="020b2-144">For this scenario, hello index pattern used for hello flow logs is "nsg-flow-logs".</span></span> <span data-ttu-id="020b2-145">Módosíthatja a hello index mintát a logstash.conf fájl hello "kimeneti" szakaszában.</span><span class="sxs-lookup"><span data-stu-id="020b2-145">You may change hello index pattern in hello "output" section of your logstash.conf file.</span></span>

1. <span data-ttu-id="020b2-146">Ha távolról tooview hello Kibana irányítópult, hozzon létre NSG bejövő szabály hozzáférést túl**port 5601**.</span><span class="sxs-lookup"><span data-stu-id="020b2-146">If you want tooview hello Kibana dashboard remotely, create an inbound NSG rule allowing access too**port 5601**.</span></span>

### <a name="create-a-kibana-dashboard"></a><span data-ttu-id="020b2-147">Kibana irányítópult létrehozása</span><span class="sxs-lookup"><span data-stu-id="020b2-147">Create a Kibana dashboard</span></span>

<span data-ttu-id="020b2-148">Ez a cikk adtunk meg tooview trendek egy minta-irányítópult és a riasztások részletes adatait.</span><span class="sxs-lookup"><span data-stu-id="020b2-148">For this article, we have provided a sample dashboard for you tooview trends and details in your alerts.</span></span>

![1. ábra][1]

1. <span data-ttu-id="020b2-150">Hello irányítópult-fájl letöltésére [Itt](https://aka.ms/networkwatchernsgflowlogdashboard), hello képi megjelenítés fájl [Itt](https://aka.ms/networkwatchernsgflowlogvisualizations), és hello mentett keresési fájl [Itt](https://aka.ms/networkwatchernsgflowlogsearch).</span><span class="sxs-lookup"><span data-stu-id="020b2-150">Download hello dashboard file [here](https://aka.ms/networkwatchernsgflowlogdashboard), hello visualization file [here](https://aka.ms/networkwatchernsgflowlogvisualizations), and hello saved search file [here](https://aka.ms/networkwatchernsgflowlogsearch).</span></span>

1. <span data-ttu-id="020b2-151">A hello **felügyeleti** lapon a Kibana, lépjen túl**mentett objektumok** mindhárom fájlt importálja.</span><span class="sxs-lookup"><span data-stu-id="020b2-151">Under hello **Management** tab of Kibana, navigate too**Saved Objects** and import all three files.</span></span> <span data-ttu-id="020b2-152">Ezután a hello **irányítópult** megnyithatja lapon és a minta-irányítópult hello.</span><span class="sxs-lookup"><span data-stu-id="020b2-152">Then from hello **Dashboard** tab you can open and load hello sample dashboard.</span></span>

<span data-ttu-id="020b2-153">A saját képi megjelenítések és saját egyik fontos metrikák felé szabott irányítópultok is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="020b2-153">You can also create your own visualizations and dashboards tailored towards metrics of your own interest.</span></span> <span data-ttu-id="020b2-154">További információk Kibana képi megjelenítés létrehozása Kibana tartozó [dokumentációs](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span><span class="sxs-lookup"><span data-stu-id="020b2-154">Read more about creating Kibana visualizations from Kibana's [official documentation](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span></span>

### <a name="visualize-nsg-flow-logs"></a><span data-ttu-id="020b2-155">NSG folyamata naplók megjelenítése</span><span class="sxs-lookup"><span data-stu-id="020b2-155">Visualize NSG flow logs</span></span>

<span data-ttu-id="020b2-156">hello minta-irányítópult biztosít több képi megjelenítések hello folyamata naplói:</span><span class="sxs-lookup"><span data-stu-id="020b2-156">hello sample dashboard provides several visualizations of hello flow logs:</span></span>

1. <span data-ttu-id="020b2-157">Döntési és irányok keresztül időpontjára - adatfolyamok a idő adatsorozat diagramjait adatfolyamok száma hello trendjét ábrázoló hello időszakra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="020b2-157">Flows by Decision/Direction Over Time - time series graphs showing hello number of flows over hello time period.</span></span> <span data-ttu-id="020b2-158">Hello egysége, és mindkét alábbi képi megjelenítést span szerkesztheti.</span><span class="sxs-lookup"><span data-stu-id="020b2-158">You can edit hello unit of time and span of both these visualizations.</span></span> <span data-ttu-id="020b2-159">Adatfolyamok döntési látható hello aránya engedélyezése vagy megtagadása döntések során forgalom irányát mutatja hello aránya bejövő és kimenő forgalom.</span><span class="sxs-lookup"><span data-stu-id="020b2-159">Flows by Decision shows hello proportion of allow or deny decisions made, while Flows by Direction shows hello proportion of inbound and outbound traffic.</span></span> <span data-ttu-id="020b2-160">Ezek a látványelemek vizsgálja meg a forgalmi adott idő alatt, és bármely igényeiben jelentkező vagy szokatlan mintákat keressen.</span><span class="sxs-lookup"><span data-stu-id="020b2-160">With these visuals you can examine traffic trends over time and look for any spikes or unusual patterns.</span></span>

  ![2. ábra][2]

1. <span data-ttu-id="020b2-162">Cél/forrásport – által adatfolyamok a tortadiagramok megjelenítő hello lebontása flow tootheir megfelelő portok.</span><span class="sxs-lookup"><span data-stu-id="020b2-162">Flows by Destination/Source Port – pie charts showing hello breakdown of flows tootheir respective ports.</span></span> <span data-ttu-id="020b2-163">Az ebben a nézetben láthatja a leggyakrabban használt portok.</span><span class="sxs-lookup"><span data-stu-id="020b2-163">With this view you can see your most commonly used ports.</span></span> <span data-ttu-id="020b2-164">Ha egy adott portot hello kördiagram belül kattint, hello irányítópult hello részeinek szűrheti, hogy a port tooflows le.</span><span class="sxs-lookup"><span data-stu-id="020b2-164">If you click on a specific port within hello pie chart, hello rest of hello dashboard will filter down tooflows of that port.</span></span>

  ![figure3][3]

1. <span data-ttu-id="020b2-166">Több adatfolyamok és legkorábbi napló időben – hello száma adatfolyamok rögzített, és hello dátum hello legkorábbi napló rögzített jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="020b2-166">Number of Flows and Earliest Log Time – metrics showing you hello number of flows recorded and hello date of hello earliest log captured.</span></span>

  ![4. ábra][4]

1. <span data-ttu-id="020b2-168">Adatfolyamok NSG-t és a szabály – egy oszlopdiagramot hello terjesztési belül minden NSG forgalom, valamint hello terjesztése belüli egyes NSG-szabályok láthatók.</span><span class="sxs-lookup"><span data-stu-id="020b2-168">Flows by NSG and Rule – a bar graph showing you hello distribution of flows within each NSG, as well as hello distribution of rules within each NSG.</span></span> <span data-ttu-id="020b2-169">Itt láthatja, melyik NSG-t, és a létrehozott szabályok hello legtöbb forgalmat.</span><span class="sxs-lookup"><span data-stu-id="020b2-169">From here you can see which NSG and rules generated hello most traffic.</span></span>

  ![figure5][5]

1. <span data-ttu-id="020b2-171">Első 10 forrás vagy a cél IP-címek – hello első 10 forrás és cél IP-címek megjelenítő sávdiagramok.</span><span class="sxs-lookup"><span data-stu-id="020b2-171">Top 10 Source/Destination IPs – bar charts showing hello top 10 source and destination IPs.</span></span> <span data-ttu-id="020b2-172">Módosíthatja a diagramok tooshow több vagy kevesebb felső IP-címek.</span><span class="sxs-lookup"><span data-stu-id="020b2-172">You can adjust these charts tooshow more or less top IPs.</span></span> <span data-ttu-id="020b2-173">Itt meg is tekintse meg a leggyakrabban előforduló az IP-címek hello, valamint a forgalom döntési hello (engedélyezése vagy megtagadása) végrehajtott minden IP felé.</span><span class="sxs-lookup"><span data-stu-id="020b2-173">From here you can see hello most commonly occurring IPs as well as hello traffic decision (allow or deny) being made towards each IP.</span></span>

  ![figure6][6]

1. <span data-ttu-id="020b2-175">Attribútumfolyam rekordokat – Ez a táblázat bemutatja, hello belül minden folyamat rekordot, valamint a megfelelő NGS és szabály található információt.</span><span class="sxs-lookup"><span data-stu-id="020b2-175">Flow Tuples – this table shows you hello information contained within each flow tuple, as well as its corresponding NGS and rule.</span></span>

  ![figure7][7]

<span data-ttu-id="020b2-177">Hello lekérdezés sáv segítségével hello irányítópult hello tetején, végezhet a hello adatfolyamok, például az előfizetés-azonosító, erőforráscsoport-sablonok, szabály vagy bármely más változó érdeklő bármely paraméter alapján hello irányítópult le.</span><span class="sxs-lookup"><span data-stu-id="020b2-177">Using hello query bar at hello top of hello dashboard, you can filter down hello dashboard based on any parameter of hello flows, such as subscription ID, resource groups, rule, or any other variable of interest.</span></span> <span data-ttu-id="020b2-178">További információ a Kibana a lekérdezések és a szűrők, tekintse meg a toohello [dokumentációs](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)</span><span class="sxs-lookup"><span data-stu-id="020b2-178">For more about Kibana's queries and filters, refer toohello [official documentation](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)</span></span>

## <a name="conclusion"></a><span data-ttu-id="020b2-179">Összegzés</span><span class="sxs-lookup"><span data-stu-id="020b2-179">Conclusion</span></span>

<span data-ttu-id="020b2-180">A rugalmas készlet hello hello hálózati biztonsági csoport folyamata naplók kombinálásával azt rendelkezik kapja meg hatékony és testre szabható módon toovisualize a hálózati forgalom.</span><span class="sxs-lookup"><span data-stu-id="020b2-180">By combining hello Network Security Group flow logs with hello Elastic Stack, we have come up with powerful and customizable way toovisualize our network traffic.</span></span> <span data-ttu-id="020b2-181">Ezek az irányítópultok tooquickly nyereség érdekében lehetővé teszi a hálózati forgalom, valamint a szűrő észrevételeket oszthatnak meg, és vizsgálja meg az összes esetleges rendellenességeket.</span><span class="sxs-lookup"><span data-stu-id="020b2-181">These dashboards allow you tooquickly gain and share insights about your network traffic, as well as filter down and investigate on any potential anomalies.</span></span> <span data-ttu-id="020b2-182">Kibana használ, ezek az irányítópultok testre szabni, és hozzon létre adott képi megjelenítések toomeet bármilyen biztonsági, naplózási és megfelelőségi megfelelően.</span><span class="sxs-lookup"><span data-stu-id="020b2-182">Using Kibana, you can tailor these dashboards and create specific visualizations toomeet any security, audit, and compliance needs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="020b2-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="020b2-183">Next steps</span></span>

<span data-ttu-id="020b2-184">Ismerje meg, hogyan toovisualize az NSG-folyamat naplózza a Power BI ellátogatva [megjelenítése NSG forgalomáramlás naplók és a Power bi-ban](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="020b2-184">Learn how toovisualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>


<!--Image references-->

[scenario]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/scenario.png
[1]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure7.png
