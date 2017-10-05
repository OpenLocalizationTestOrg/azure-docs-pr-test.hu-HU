---
title: "Azure hálózati figyelő NSG folyamata naplók nyílt forráskódú eszközökkel megjelenítése |} Microsoft Docs"
description: "Ezen a lapon NSG folyamata naplók megjelenítése nyílt forráskódú eszközök használatával ismerteti."
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
ms.openlocfilehash: 20f60ccd9108a7473705c2368f28d3152d0dd614
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="visualize-azure-network-watcher-nsg-flow-logs-using-open-source-tools"></a><span data-ttu-id="91240-103">Azure hálózati figyelő NSG folyamata naplók nyílt forráskódú eszközökkel megjelenítése</span><span class="sxs-lookup"><span data-stu-id="91240-103">Visualize Azure Network Watcher NSG flow logs using open source tools</span></span>

<span data-ttu-id="91240-104">Hálózati biztonsági csoport folyamata naplók információkkal használható IP-bemenő és kimenő forgalmat a hálózati biztonsági csoportok ismertetése.</span><span class="sxs-lookup"><span data-stu-id="91240-104">Network Security Group flow logs provide information that can be used understand ingress and egress IP traffic on Network Security Groups.</span></span> <span data-ttu-id="91240-105">A folyamat naplók kimenő és bejövő forgalom megjelenítése / szabály alapján, a hálózati adapter a folyamat vonatkozik, a folyamat (forrás vagy a cél IP, forrás vagy a cél Port Protocol), információ 5 rekordos és ha a forgalom lett engedélyez vagy tilt.</span><span class="sxs-lookup"><span data-stu-id="91240-105">These flow logs show outbound and inbound flows on a per rule basis, the NIC the flow applies to, 5 tuple information about the flow (Source/Destination IP, Source/Destination Port, Protocol), and if the traffic was allowed or denied.</span></span>

<span data-ttu-id="91240-106">A folyamat naplók manuális elemzése és a dcu nehéz lehet.</span><span class="sxs-lookup"><span data-stu-id="91240-106">These flow logs can be difficult to manually parse and gain insights from.</span></span> <span data-ttu-id="91240-107">Vannak azonban számos nyílt forráskódú eszköz, amelynek segítségével jelenítheti meg ezeket az adatokat.</span><span class="sxs-lookup"><span data-stu-id="91240-107">However, there are several open source tools that can help visualize this data.</span></span> <span data-ttu-id="91240-108">Ez a cikk ad meg, ezek a naplók a rugalmas készlet, amely lehetővé teszi a gyors index, és jelenítheti meg a folyamat használatával megjelenítheti az adott megoldás Kibana irányítópult bejelentkezik.</span><span class="sxs-lookup"><span data-stu-id="91240-108">This article will provide a solution to visualize these logs using the Elastic Stack, which will allow you to quickly index and visualize your flow logs on a Kibana dashboard.</span></span>

## <a name="scenario"></a><span data-ttu-id="91240-109">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="91240-109">Scenario</span></span>

<span data-ttu-id="91240-110">Ebben a cikkben egy megoldást, amely lehetővé teszi a hálózati biztonsági csoport folyamata naplók a rugalmas készlet használatával megjelenítheti üzembe helyezünk.</span><span class="sxs-lookup"><span data-stu-id="91240-110">In this article, we will set up a solution that will allow you to visualize Network Security Group flow logs using the Elastic Stack.</span></span>  <span data-ttu-id="91240-111">Egy Logstash bemeneti beépülő modul beszerzi a folyamat naplók közvetlenül a tárolási blob, a folyamat naplókat tartalmazó konfigurálva.</span><span class="sxs-lookup"><span data-stu-id="91240-111">A Logstash input plugin will obtain the flow logs directly from the storage blob configured for containing the flow logs.</span></span> <span data-ttu-id="91240-112">Ezt követően a rugalmas készlet használatával, a folyamat naplókat fog kell indexelt és Kibana irányítópulton jelenítheti meg az információkat létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="91240-112">Then, using the Elastic Stack, the flow logs will be indexed and used to create a Kibana dashboard to visualize the information.</span></span>

![forgatókönyv][scenario]

## <a name="steps"></a><span data-ttu-id="91240-114">Lépések</span><span class="sxs-lookup"><span data-stu-id="91240-114">Steps</span></span>

### <a name="enable-network-security-group-flow-logging"></a><span data-ttu-id="91240-115">Engedélyezze a hálózati biztonsági csoport folyamata naplózás</span><span class="sxs-lookup"><span data-stu-id="91240-115">Enable Network Security Group flow logging</span></span>
<span data-ttu-id="91240-116">Ebben az esetben rendelkeznie kell hálózati biztonsági csoport Flow naplózás legalább egy hálózati biztonsági csoport a fiók engedélyezve.</span><span class="sxs-lookup"><span data-stu-id="91240-116">For this scenario, you must have Network Security Group Flow Logging enabled on at least one Network Security Group in your account.</span></span> <span data-ttu-id="91240-117">A hálózati biztonsági folyamata naplók engedélyezése útmutatásért tekintse meg a következő cikk [folyamata naplózási a hálózati biztonsági csoportok bemutatása](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="91240-117">For instructions on enabling Network Security Flow Logs, refer to the following article [Introduction to flow logging for Network Security Groups](network-watcher-nsg-flow-logging-overview.md).</span></span>


### <a name="set-up-the-elastic-stack"></a><span data-ttu-id="91240-118">A rugalmas készlet beállítása</span><span class="sxs-lookup"><span data-stu-id="91240-118">Set up the Elastic Stack</span></span>
<span data-ttu-id="91240-119">A rugalmas készlet NSG folyamata naplók összekötésével létrehozhatjuk Kibana irányítópult mi kiválaszthatjuk, hogy a keresés, diagramot, elemzése és elemzések származik a naplókat.</span><span class="sxs-lookup"><span data-stu-id="91240-119">By connecting NSG flow logs with the Elastic Stack, we can create a Kibana dashboard what allows us to search, graph, analyze, and derive insights from our logs.</span></span>

#### <a name="install-elasticsearch"></a><span data-ttu-id="91240-120">Elasticsearch telepítése</span><span class="sxs-lookup"><span data-stu-id="91240-120">Install Elasticsearch</span></span>

1. <span data-ttu-id="91240-121">A rugalmas készlet 5.0-s verziójáról és a fent Java 8 igényel.</span><span class="sxs-lookup"><span data-stu-id="91240-121">The Elastic Stack from version 5.0 and above requires Java 8.</span></span> <span data-ttu-id="91240-122">Futtassa a parancsot `java -version` a verziójának.</span><span class="sxs-lookup"><span data-stu-id="91240-122">Run the command `java -version` to check your version.</span></span> <span data-ttu-id="91240-123">Ha nem kell telepíteni, a részletek a dokumentációban találhatók java [Oracle-webhely](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span><span class="sxs-lookup"><span data-stu-id="91240-123">If you do not have java install, refer to documentation on [Oracle's website](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span></span>
1. <span data-ttu-id="91240-124">Töltse le a megfelelő bináris csomagot a rendszer:</span><span class="sxs-lookup"><span data-stu-id="91240-124">Download the correct binary package for your system:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    <span data-ttu-id="91240-125">Egyéb módszerek található [Elasticsearch telepítése](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span><span class="sxs-lookup"><span data-stu-id="91240-125">Other installation methods can be found at [Elasticsearch Installation](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span></span>

1. <span data-ttu-id="91240-126">Győződjön meg arról, hogy Elasticsearch fut-e a parancsot:</span><span class="sxs-lookup"><span data-stu-id="91240-126">Verify that Elasticsearch is running with the command:</span></span>

    ```
    curl http://127.0.0.1:9200
    ```

    <span data-ttu-id="91240-127">Ez hasonló választ kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="91240-127">You should see a response similar to this:</span></span>

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

<span data-ttu-id="91240-128">Rugalmas keresési telepítése további útmutatásra van szüksége, tekintse meg a lap [telepítése](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span><span class="sxs-lookup"><span data-stu-id="91240-128">For further instructions on installing Elastic search, refer to the page [Installation](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span></span>

### <a name="install-logstash"></a><span data-ttu-id="91240-129">Logstash telepítése</span><span class="sxs-lookup"><span data-stu-id="91240-129">Install Logstash</span></span>

1. <span data-ttu-id="91240-130">A következő parancsokat Logstash telepítése:</span><span class="sxs-lookup"><span data-stu-id="91240-130">To install Logstash run the following commands:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. <span data-ttu-id="91240-131">Ezután azt kell Logstash eléréséhez, és a folyamat naplók elemzése.</span><span class="sxs-lookup"><span data-stu-id="91240-131">Next we need to configure Logstash to access and parse the flow logs.</span></span> <span data-ttu-id="91240-132">Hozzon létre egy logstash.conf fájl használatával:</span><span class="sxs-lookup"><span data-stu-id="91240-132">Create a logstash.conf file using:</span></span>

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. <span data-ttu-id="91240-133">Vegye fel a következő tartalmat a fájlba:</span><span class="sxs-lookup"><span data-stu-id="91240-133">Add the following content to the file:</span></span>

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

<span data-ttu-id="91240-134">További telepítésével kapcsolatos utasításokat Logstash, tekintse meg a [dokumentációs](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span><span class="sxs-lookup"><span data-stu-id="91240-134">For further instructions on installing Logstash, refer to the [official documentation](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span></span>

### <a name="install-the-logstash-input-plugin-for-azure-blob-storage"></a><span data-ttu-id="91240-135">A Logstash bemeneti beépülő modul az Azure blob Storage telepítése</span><span class="sxs-lookup"><span data-stu-id="91240-135">Install the Logstash input plugin for Azure blob storage</span></span>

<span data-ttu-id="91240-136">A Logstash beépülő modul lehetővé teszi a kijelölt tárfiókkal közvetlenül elérje a folyamat naplók.</span><span class="sxs-lookup"><span data-stu-id="91240-136">This Logstash plugin will allow you to directly access the flow logs from their designated storage account.</span></span> <span data-ttu-id="91240-137">A beépülő modul telepítéséhez az alapértelmezett Logstash telepítési könyvtárában (Ez esetben /usr/share/logstash/bin) futtassa a parancsot:</span><span class="sxs-lookup"><span data-stu-id="91240-137">To install this plugin, from the default Logstash installation directory (in this case /usr/share/logstash/bin) run the command:</span></span>

```
logstash-plugin install logstash-input-azureblob
```

<span data-ttu-id="91240-138">Logstash elindításához futtassa a parancsot:</span><span class="sxs-lookup"><span data-stu-id="91240-138">To start Logstash run the command:</span></span>

```
sudo /etc/init.d/logstash start
```

<span data-ttu-id="91240-139">A beépülő modul kapcsolatos további információkért tekintse meg a dokumentációját [Itt](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)</span><span class="sxs-lookup"><span data-stu-id="91240-139">For more information about this plugin, refer to documentation [here](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)</span></span>

### <a name="install-kibana"></a><span data-ttu-id="91240-140">Kibana telepítése</span><span class="sxs-lookup"><span data-stu-id="91240-140">Install Kibana</span></span>

1. <span data-ttu-id="91240-141">A következő parancsokat Kibana telepítéséhez:</span><span class="sxs-lookup"><span data-stu-id="91240-141">Run the following commands to install Kibana:</span></span>

  ```
  curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
  tar xzvf kibana-5.2.0-linux-x86_64.tar.gz
  ```

1. <span data-ttu-id="91240-142">Kibana használja parancsok futtatásához:</span><span class="sxs-lookup"><span data-stu-id="91240-142">To run Kibana use the commands:</span></span>

  ```
  cd kibana-5.2.0-linux-x86_64/
  ./bin/kibana
  ```

1. <span data-ttu-id="91240-143">Lépjen a Kibana webes felület megtekintéséhez`http://localhost:5601`</span><span class="sxs-lookup"><span data-stu-id="91240-143">To view your Kibana web interface, navigate to `http://localhost:5601`</span></span>
1. <span data-ttu-id="91240-144">Ebben a forgatókönyvben az index a folyamat használható a mintája "nsg-adatfolyam-logs".</span><span class="sxs-lookup"><span data-stu-id="91240-144">For this scenario, the index pattern used for the flow logs is "nsg-flow-logs".</span></span> <span data-ttu-id="91240-145">Változtassa meg az index minta a logstash.conf fájl a "kimeneti" szakaszában.</span><span class="sxs-lookup"><span data-stu-id="91240-145">You may change the index pattern in the "output" section of your logstash.conf file.</span></span>

1. <span data-ttu-id="91240-146">Ha távolról Kibana irányítópultjának megjelenítése, hozzon létre egy bejövő NSG szabályt, amely engedélyezi webtartalmak elérését **port 5601**.</span><span class="sxs-lookup"><span data-stu-id="91240-146">If you want to view the Kibana dashboard remotely, create an inbound NSG rule allowing access to **port 5601**.</span></span>

### <a name="create-a-kibana-dashboard"></a><span data-ttu-id="91240-147">Kibana irányítópult létrehozása</span><span class="sxs-lookup"><span data-stu-id="91240-147">Create a Kibana dashboard</span></span>

<span data-ttu-id="91240-148">Ebben a cikkben adtunk egy minta-irányítópult ahhoz, hogy a riasztásokat a trendek és a részletek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="91240-148">For this article, we have provided a sample dashboard for you to view trends and details in your alerts.</span></span>

![1. ábra][1]

1. <span data-ttu-id="91240-150">Az irányítópult-fájl letöltésére [Itt](https://aka.ms/networkwatchernsgflowlogdashboard), a képi megjelenítés fájl [Itt](https://aka.ms/networkwatchernsgflowlogvisualizations), és a mentett keresés fájl [Itt](https://aka.ms/networkwatchernsgflowlogsearch).</span><span class="sxs-lookup"><span data-stu-id="91240-150">Download the dashboard file [here](https://aka.ms/networkwatchernsgflowlogdashboard), the visualization file [here](https://aka.ms/networkwatchernsgflowlogvisualizations), and the saved search file [here](https://aka.ms/networkwatchernsgflowlogsearch).</span></span>

1. <span data-ttu-id="91240-151">A a **felügyeleti** lapon lépjen a Kibana, **objektumok mentése** mindhárom fájlt importálja.</span><span class="sxs-lookup"><span data-stu-id="91240-151">Under the **Management** tab of Kibana, navigate to **Saved Objects** and import all three files.</span></span> <span data-ttu-id="91240-152">Ezután az a **irányítópult** lap megnyitásához, és a minta-irányítópult betöltése.</span><span class="sxs-lookup"><span data-stu-id="91240-152">Then from the **Dashboard** tab you can open and load the sample dashboard.</span></span>

<span data-ttu-id="91240-153">A saját képi megjelenítések és saját egyik fontos metrikák felé szabott irányítópultok is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="91240-153">You can also create your own visualizations and dashboards tailored towards metrics of your own interest.</span></span> <span data-ttu-id="91240-154">További információk Kibana képi megjelenítés létrehozása Kibana tartozó [dokumentációs](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span><span class="sxs-lookup"><span data-stu-id="91240-154">Read more about creating Kibana visualizations from Kibana's [official documentation](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span></span>

### <a name="visualize-nsg-flow-logs"></a><span data-ttu-id="91240-155">NSG folyamata naplók megjelenítése</span><span class="sxs-lookup"><span data-stu-id="91240-155">Visualize NSG flow logs</span></span>

<span data-ttu-id="91240-156">A minta-irányítópult a folyamat naplók több képi megjelenítések biztosítja:</span><span class="sxs-lookup"><span data-stu-id="91240-156">The sample dashboard provides several visualizations of the flow logs:</span></span>

1. <span data-ttu-id="91240-157">Döntési és irányok keresztül időpontjára - adatfolyamok a azokról az az idő alatt adatfolyamok idő adatsorozat diagramjait.</span><span class="sxs-lookup"><span data-stu-id="91240-157">Flows by Decision/Direction Over Time - time series graphs showing the number of flows over the time period.</span></span> <span data-ttu-id="91240-158">Időegység, és mindkét alábbi képi megjelenítést span szerkesztheti.</span><span class="sxs-lookup"><span data-stu-id="91240-158">You can edit the unit of time and span of both these visualizations.</span></span> <span data-ttu-id="91240-159">Döntési által adatfolyamok arányát jeleníti meg, engedélyezése vagy megtagadása döntések, amíg a forgalom iránya által a bejövő és kimenő forgalom arányát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="91240-159">Flows by Decision shows the proportion of allow or deny decisions made, while Flows by Direction shows the proportion of inbound and outbound traffic.</span></span> <span data-ttu-id="91240-160">Ezek a látványelemek vizsgálja meg a forgalmi adott idő alatt, és bármely igényeiben jelentkező vagy szokatlan mintákat keressen.</span><span class="sxs-lookup"><span data-stu-id="91240-160">With these visuals you can examine traffic trends over time and look for any spikes or unusual patterns.</span></span>

  ![2. ábra][2]

1. <span data-ttu-id="91240-162">Cél/forrásport – által adatfolyamok a tortadiagramok megjelenítő bontásban tartalmazza a megfelelő portokhoz forgalom.</span><span class="sxs-lookup"><span data-stu-id="91240-162">Flows by Destination/Source Port – pie charts showing the breakdown of flows to their respective ports.</span></span> <span data-ttu-id="91240-163">Az ebben a nézetben láthatja a leggyakrabban használt portok.</span><span class="sxs-lookup"><span data-stu-id="91240-163">With this view you can see your most commonly used ports.</span></span> <span data-ttu-id="91240-164">Ha egy adott portot a kördiagram belül kattint, a többi irányítópult le, hogy a port adatfolyamok szűrheti.</span><span class="sxs-lookup"><span data-stu-id="91240-164">If you click on a specific port within the pie chart, the rest of the dashboard will filter down to flows of that port.</span></span>

  ![figure3][3]

1. <span data-ttu-id="91240-166">Több adatfolyamok és legkorábbi napló időben – adatfolyamok rögzített száma és a legkorábbi naplóban rögzített dátumának megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="91240-166">Number of Flows and Earliest Log Time – metrics showing you the number of flows recorded and the date of the earliest log captured.</span></span>

  ![4. ábra][4]

1. <span data-ttu-id="91240-168">Adatfolyamok NSG-t és a szabály – egy oszlopdiagramot belül minden NSG forgalom eloszlását, valamint belüli egyes NSG-szabályok eloszlását jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="91240-168">Flows by NSG and Rule – a bar graph showing you the distribution of flows within each NSG, as well as the distribution of rules within each NSG.</span></span> <span data-ttu-id="91240-169">Itt láthatja, melyik NSG-t és a szabályok jönnek létre a legtöbb forgalmat.</span><span class="sxs-lookup"><span data-stu-id="91240-169">From here you can see which NSG and rules generated the most traffic.</span></span>

  ![figure5][5]

1. <span data-ttu-id="91240-171">Felső 10 forrás vagy a cél IP-címek – sávdiagramok első 10 forrás és cél IP-címek.</span><span class="sxs-lookup"><span data-stu-id="91240-171">Top 10 Source/Destination IPs – bar charts showing the top 10 source and destination IPs.</span></span> <span data-ttu-id="91240-172">Beállíthatja, hogy több vagy kevesebb felső IP-cím megjelenítése a diagramokat.</span><span class="sxs-lookup"><span data-stu-id="91240-172">You can adjust these charts to show more or less top IPs.</span></span> <span data-ttu-id="91240-173">Itt láthatja a leggyakrabban előforduló IP-címek, valamint a forgalom döntési (engedélyezése vagy megtagadása) végrehajtott minden IP felé.</span><span class="sxs-lookup"><span data-stu-id="91240-173">From here you can see the most commonly occurring IPs as well as the traffic decision (allow or deny) being made towards each IP.</span></span>

  ![figure6][6]

1. <span data-ttu-id="91240-175">Folyamat rekordokat – ezt a táblázatot jeleníti meg az adatokat belül minden folyamat rekordot, valamint a megfelelő NGS és szabály található.</span><span class="sxs-lookup"><span data-stu-id="91240-175">Flow Tuples – this table shows you the information contained within each flow tuple, as well as its corresponding NGS and rule.</span></span>

  ![figure7][7]

<span data-ttu-id="91240-177">A lekérdezés sáv segítségével az irányítópult tetején, szűrheti az adatfolyamok, például az előfizetés-azonosító, erőforráscsoport-sablonok, szabály vagy bármely más változó érdeklő bármely paramétere alapján irányítópult le.</span><span class="sxs-lookup"><span data-stu-id="91240-177">Using the query bar at the top of the dashboard, you can filter down the dashboard based on any parameter of the flows, such as subscription ID, resource groups, rule, or any other variable of interest.</span></span> <span data-ttu-id="91240-178">További információ a Kibana a lekérdezések és a szűrők, tekintse meg a [dokumentációs](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)</span><span class="sxs-lookup"><span data-stu-id="91240-178">For more about Kibana's queries and filters, refer to the [official documentation](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)</span></span>

## <a name="conclusion"></a><span data-ttu-id="91240-179">Összegzés</span><span class="sxs-lookup"><span data-stu-id="91240-179">Conclusion</span></span>

<span data-ttu-id="91240-180">A hálózati biztonsági csoport folyamata naplók és a rugalmas készlet együttes, azt kell elérni jelenítheti meg a hálózati forgalom hatékony és testre szabható módszert.</span><span class="sxs-lookup"><span data-stu-id="91240-180">By combining the Network Security Group flow logs with the Elastic Stack, we have come up with powerful and customizable way to visualize our network traffic.</span></span> <span data-ttu-id="91240-181">Ezek az irányítópultok engedélyezi, hogy gyorsan kapnak, és a hálózati forgalom, valamint a szűrő észrevételeket oszthatnak meg, és vizsgálja meg az összes esetleges rendellenességeket.</span><span class="sxs-lookup"><span data-stu-id="91240-181">These dashboards allow you to quickly gain and share insights about your network traffic, as well as filter down and investigate on any potential anomalies.</span></span> <span data-ttu-id="91240-182">Kibana használ, ezek az irányítópultok testre szabni, és adott képi megjelenítéseket készíthet, a biztonsági, naplózási és megfelelőségi igényeinek.</span><span class="sxs-lookup"><span data-stu-id="91240-182">Using Kibana, you can tailor these dashboards and create specific visualizations to meet any security, audit, and compliance needs.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91240-183">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="91240-183">Next steps</span></span>

<span data-ttu-id="91240-184">Megtudhatja, hogyan jelenítheti meg az NSG folyamata naplók a Power BI ellátogatva [megjelenítése NSG forgalomáramlás naplók és a Power bi-ban](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="91240-184">Learn how to visualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>


<!--Image references-->

[scenario]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/scenario.png
[1]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure7.png
