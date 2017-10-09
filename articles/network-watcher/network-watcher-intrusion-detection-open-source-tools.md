---
title: "hálózati behatolásérzékelési aaaPerform nyílt forráskódú eszközök és az Azure hálózati figyelőt |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan toouse Azure hálózati figyelőt és nyílt forráskódú eszközök tooperform hálózati behatolásérzékelési"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 0f043f08-19e1-4125-98b0-3e335ba69681
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: b5a909b827ab32ad6b2fd8e2911a944fd940249e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="perform-network-intrusion-detection-with-network-watcher-and-open-source-tools"></a><span data-ttu-id="de650-103">Hajtsa végre a behatolás-észlelés hálózati nyílt forráskódú eszközök és hálózati figyelőt</span><span class="sxs-lookup"><span data-stu-id="de650-103">Perform network intrusion detection with Network Watcher and open source tools</span></span>

<span data-ttu-id="de650-104">Csomag rögzíti a végrehajtási hálózati behatolás rendszerek (Azonosítók) és a hálózati biztonsági figyelése (NSM) végrehajtása nyilvános kulcsokra épülő.</span><span class="sxs-lookup"><span data-stu-id="de650-104">Packet captures are a key component for implementing network intrusion detection systems (IDS) and performing Network Security Monitoring (NSM).</span></span> <span data-ttu-id="de650-105">Számos nyílt forráskódú Azonosítók eszköz, amely feldolgozza a csomag rögzíti, és keresse meg az esetleges hálózati behatolások és a rosszindulatú tevékenységhez van.</span><span class="sxs-lookup"><span data-stu-id="de650-105">There are several open source IDS tools that process packet captures and look for signatures of possible network intrusions and malicious activity.</span></span> <span data-ttu-id="de650-106">Használja a hálózati figyelő által biztosított hello csomagok készítését, elemezheti a hálózati biztonsági rések vagy káros behatolások.</span><span class="sxs-lookup"><span data-stu-id="de650-106">Using hello packet captures provided by Network Watcher, you can analyze your network for any harmful intrusions or vulnerabilities.</span></span>

<span data-ttu-id="de650-107">Egy ilyen nyílt forráskódú eszköz Suricata, egy Azonosítók motor, amely szabálykészletek toomonitor hálózati forgalom használ, és elindítja a riasztásokat gyanús események előfordulásakor.</span><span class="sxs-lookup"><span data-stu-id="de650-107">One such open source tool is Suricata, an IDS engine that uses rulesets toomonitor network traffic and triggers alerts whenever suspicious events occur.</span></span> <span data-ttu-id="de650-108">Suricata kínál a többszálas motor, azaz a hálózati forgalom elemzése a nagyobb sebesség és a hatékonyság műveleteket hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="de650-108">Suricata offers a multi-threaded engine, meaning it can perform network traffic analysis with increased speed and efficiency.</span></span> <span data-ttu-id="de650-109">Suricata és platformképességei kapcsolatos további részletekért látogasson el a https://suricata-ids.org/ webhelyen.</span><span class="sxs-lookup"><span data-stu-id="de650-109">For more details about Suricata and its capabilities, visit their website at https://suricata-ids.org/.</span></span>

## <a name="scenario"></a><span data-ttu-id="de650-110">Forgatókönyv</span><span class="sxs-lookup"><span data-stu-id="de650-110">Scenario</span></span>

<span data-ttu-id="de650-111">Ez a cikk azt ismerteti, hogyan másolatot a környezet tooperform tooset behatolásérzékelési használatával hálózati figyelőt, Suricata, hálózati és a rugalmas készlet hello.</span><span class="sxs-lookup"><span data-stu-id="de650-111">This article explains how tooset up your environment tooperform network intrusion detection using Network Watcher, Suricata, and hello Elastic Stack.</span></span> <span data-ttu-id="de650-112">Hálózati figyelőt tartalmaz, akkor hello csomaggal használt tooperform hálózati behatolásérzékelési rögzíti.</span><span class="sxs-lookup"><span data-stu-id="de650-112">Network Watcher provides you with hello packet captures used tooperform network intrusion detection.</span></span> <span data-ttu-id="de650-113">Suricata folyamatok hello csomagok rögzíti, és a csomagot, amely megfelel az adott szabálykészletben fenyegetések alapján eseményindító riasztások.</span><span class="sxs-lookup"><span data-stu-id="de650-113">Suricata processes hello packet captures and trigger alerts based on packets that match its given ruleset of threats.</span></span> <span data-ttu-id="de650-114">Ezek a riasztások a naplófájl a helyi számítógépen tárolja.</span><span class="sxs-lookup"><span data-stu-id="de650-114">These alerts are stored in a log file on your local machine.</span></span> <span data-ttu-id="de650-115">Hello rugalmas készlet használatával, Suricata által létrehozott hello naplók indexelhetők és toocreate Kibana irányítópulton, így a hello naplók vizuális ábrázolását, és egy azt jelenti, hogy tooquickly nyereség insights toopotential hálózati biztonsági rések használt.</span><span class="sxs-lookup"><span data-stu-id="de650-115">Using hello Elastic Stack, hello logs generated by Suricata can be indexed and used toocreate a Kibana dashboard, providing you with a visual representation of hello logs and a means tooquickly gain insights toopotential network vulnerabilities.</span></span>  

![egyszerű webes alkalmazás forgatókönyv][1]

<span data-ttu-id="de650-117">Mindkét nyílt forráskódú eszközök beállítható egy Azure virtuális gépen, így tooperform az elemzés a saját Azure hálózati környezetben.</span><span class="sxs-lookup"><span data-stu-id="de650-117">Both open source tools can be set up on an Azure VM, allowing you tooperform this analysis within your own Azure network environment.</span></span>

## <a name="steps"></a><span data-ttu-id="de650-118">Lépések</span><span class="sxs-lookup"><span data-stu-id="de650-118">Steps</span></span>

### <a name="install-suricata"></a><span data-ttu-id="de650-119">Suricata telepítése</span><span class="sxs-lookup"><span data-stu-id="de650-119">Install Suricata</span></span>

<span data-ttu-id="de650-120">Minden egyéb módszer telepítés esetén keresse fel a http://suricata.readthedocs.io/en/latest/install.html</span><span class="sxs-lookup"><span data-stu-id="de650-120">For all other methods of installation, visit http://suricata.readthedocs.io/en/latest/install.html</span></span>

1. <span data-ttu-id="de650-121">Futtassa a következő parancsok hello hello parancssori terminált, a virtuális Gépet:</span><span class="sxs-lookup"><span data-stu-id="de650-121">In hello command-line terminal of your VM run hello following commands:</span></span>

    ```
    sudo add-apt-repository ppa:oisf/suricata-stable
    sudo apt-get update
    sudo sudo apt-get install suricata
    ```

1. <span data-ttu-id="de650-122">tooverify a telepítési hello paranccsal `suricata -h` toosee hello teljes parancsok listáját.</span><span class="sxs-lookup"><span data-stu-id="de650-122">tooverify your installation, run hello command `suricata -h` toosee hello full list of commands.</span></span>

### <a name="download-hello-emerging-threats-ruleset"></a><span data-ttu-id="de650-123">Hello felmerülő veszélyek szabálykészletben letöltése</span><span class="sxs-lookup"><span data-stu-id="de650-123">Download hello Emerging Threats ruleset</span></span>

<span data-ttu-id="de650-124">Ezen a ponton nem találtunk Suricata toorun szabályok.</span><span class="sxs-lookup"><span data-stu-id="de650-124">At this stage, we do not have any rules for Suricata toorun.</span></span> <span data-ttu-id="de650-125">A saját szabályokat hozhat létre, ha az egyes veszélyforrások tooyour hálózati toodetect szeretné, vagy is fejlesztett használata szabálykészletek szolgáltatók, köztük a felmerülő veszélyek vagy Snort VRT szabályok számos.</span><span class="sxs-lookup"><span data-stu-id="de650-125">You can create your own rules if there are specific threats tooyour network you would like toodetect, or you can also use developed rule sets from a number of providers, such as Emerging Threats, or VRT rules from Snort.</span></span> <span data-ttu-id="de650-126">Hello ingyenesen elérhető felmerülő veszélyek szabálykészletben itt használjuk:</span><span class="sxs-lookup"><span data-stu-id="de650-126">We use hello freely accessible Emerging Threats ruleset here:</span></span>

<span data-ttu-id="de650-127">Töltse le a hello szabálykészletben, és másolja őket abba hello könyvtár:</span><span class="sxs-lookup"><span data-stu-id="de650-127">Download hello rule set and copy them into hello directory:</span></span>

```
wget http://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz
tar zxf emerging.rules.tar.gz
sudo cp -r rules /etc/suricata/
```

### <a name="process-packet-captures-with-suricata"></a><span data-ttu-id="de650-128">Folyamat csomagrögzítéseket Suricata</span><span class="sxs-lookup"><span data-stu-id="de650-128">Process packet captures with Suricata</span></span>

<span data-ttu-id="de650-129">tooprocess csomag rögzíti Suricata, futtassa a következő parancs hello használata:</span><span class="sxs-lookup"><span data-stu-id="de650-129">tooprocess packet captures using Suricata, run hello following command:</span></span>

```
sudo suricata -c /etc/suricata/suricata.yaml -r <location_of_pcapfile>
```
<span data-ttu-id="de650-130">hello fast.log fájl toocheck hello létrejövő riasztások, olvassa el:</span><span class="sxs-lookup"><span data-stu-id="de650-130">toocheck hello resulting alerts, read hello fast.log file:</span></span>
```
tail -f /var/log/suricata/fast.log
```

### <a name="set-up-hello-elastic-stack"></a><span data-ttu-id="de650-131">A rugalmas készlet hello beállítása</span><span class="sxs-lookup"><span data-stu-id="de650-131">Set up hello Elastic Stack</span></span>

<span data-ttu-id="de650-132">Hello naplók Suricata előállító mi történik a hálózaton lévő értékes információkat tartalmaznak, amíg az ezekben a naplófájlokban hello legegyszerűbb tooread nem, és ismertetése.</span><span class="sxs-lookup"><span data-stu-id="de650-132">While hello logs that Suricata produces contain valuable information about what’s happening on our network, these log files aren’t hello easiest tooread and understand.</span></span> <span data-ttu-id="de650-133">A rugalmas készlet hello Suricata összekötésével azt is mi kiválaszthatjuk toosearch Kibana irányítópult létrehozása, diagramot, elemzése és insights származik a naplók.</span><span class="sxs-lookup"><span data-stu-id="de650-133">By connecting Suricata with hello Elastic Stack, we can create a Kibana dashboard what allows us toosearch, graph, analyze, and derive insights from our logs.</span></span>

#### <a name="install-elasticsearch"></a><span data-ttu-id="de650-134">Elasticsearch telepítése</span><span class="sxs-lookup"><span data-stu-id="de650-134">Install Elasticsearch</span></span>

1. <span data-ttu-id="de650-135">hello rugalmas verem 5.0-s verziójáról és a fent Java 8 igényel.</span><span class="sxs-lookup"><span data-stu-id="de650-135">hello Elastic Stack from version 5.0 and above requires Java 8.</span></span> <span data-ttu-id="de650-136">Hello paranccsal `java -version` toocheck adott verziójában.</span><span class="sxs-lookup"><span data-stu-id="de650-136">Run hello command `java -version` toocheck your version.</span></span> <span data-ttu-id="de650-137">Ha nem kell telepíteni, tekintse meg a toodocumentation a java [Oracle-webhely](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span><span class="sxs-lookup"><span data-stu-id="de650-137">If you do not have java install, refer toodocumentation on [Oracle's website](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)</span></span>
1. <span data-ttu-id="de650-138">Töltse le a hello helyes bináris csomagot a rendszer:</span><span class="sxs-lookup"><span data-stu-id="de650-138">Download hello correct binary package for your system:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    <span data-ttu-id="de650-139">Egyéb módszerek található [Elasticsearch telepítése](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span><span class="sxs-lookup"><span data-stu-id="de650-139">Other installation methods can be found at [Elasticsearch Installation](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)</span></span>

1. <span data-ttu-id="de650-140">Győződjön meg arról, hogy fut-e Elasticsearch hello paranccsal:</span><span class="sxs-lookup"><span data-stu-id="de650-140">Verify that Elasticsearch is running with hello command:</span></span>

    ```
    curl http://127.0.0.1:9200
    ```

    <span data-ttu-id="de650-141">A válasz hasonló toothis kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="de650-141">You should see a response similar toothis:</span></span>

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

<span data-ttu-id="de650-142">Rugalmas keresési telepítése további útmutatásra van szüksége, tekintse meg a toohello lap [telepítése](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span><span class="sxs-lookup"><span data-stu-id="de650-142">For further instructions on installing Elastic search, refer toohello page [Installation](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)</span></span>

### <a name="install-logstash"></a><span data-ttu-id="de650-143">Logstash telepítése</span><span class="sxs-lookup"><span data-stu-id="de650-143">Install Logstash</span></span>

1. <span data-ttu-id="de650-144">tooinstall Logstash hello a következő parancsok futtatásával:</span><span class="sxs-lookup"><span data-stu-id="de650-144">tooinstall Logstash run hello following commands:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. <span data-ttu-id="de650-145">Ezután azt kell tooconfigure Logstash tooread eve.json fájl hello kimenetből.</span><span class="sxs-lookup"><span data-stu-id="de650-145">Next we need tooconfigure Logstash tooread from hello output of eve.json file.</span></span> <span data-ttu-id="de650-146">Hozzon létre egy logstash.conf fájl használatával:</span><span class="sxs-lookup"><span data-stu-id="de650-146">Create a logstash.conf file using:</span></span>

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. <span data-ttu-id="de650-147">Adja hozzá a következő tartalom toohello fájl hello (Győződjön meg róla, hogy hello elérési toohello eve.json fájl helyes):</span><span class="sxs-lookup"><span data-stu-id="de650-147">Add hello following content toohello file (make sure that hello path toohello eve.json file is correct):</span></span>

    ```ruby
    input {
    file {
        path => ["/var/log/suricata/eve.json"]
        codec =>  "json"
        type => "SuricataIDPS"
    }

    }

    filter {
    if [type] == "SuricataIDPS" {
        date {
        match => [ "timestamp", "ISO8601" ]
        }
        ruby {
        code => "
            if event.get('[event_type]') == 'fileinfo'
            event.set('[fileinfo][type]', event.get('[fileinfo][magic]').to_s.split(',')[0])
            end
        "
        }

        ruby{
        code => "
            if event.get('[event_type]') == 'alert'
            sp = event.get('[alert][signature]').to_s.split(' group ')
            if (sp.length == 2) and /\A\d+\z/.match(sp[1])
                event.set('[alert][signature]', sp[0])
            end
            end
            "
        }
    }

    if [src_ip]  {
        geoip {
        source => "src_ip"
        target => "geoip"
        #database => "/opt/logstash/vendor/geoip/GeoLiteCity.dat"
        add_field => [ "[geoip][coordinates]", "%{[geoip][longitude]}" ]
        add_field => [ "[geoip][coordinates]", "%{[geoip][latitude]}"  ]
        }
        mutate {
        convert => [ "[geoip][coordinates]", "float" ]
        }
        if ![geoip.ip] {
        if [dest_ip]  {
            geoip {
            source => "dest_ip"
            target => "geoip"
            #database => "/opt/logstash/vendor/geoip/GeoLiteCity.dat"
            add_field => [ "[geoip][coordinates]", "%{[geoip][longitude]}" ]
            add_field => [ "[geoip][coordinates]", "%{[geoip][latitude]}"  ]
            }
            mutate {
            convert => [ "[geoip][coordinates]", "float" ]
            }
        }
        }
    }
    }

    output {
    elasticsearch {
        hosts => "localhost"
    }
    }
    ```

1. <span data-ttu-id="de650-148">Győződjön meg arról, hogy toogive hello megfelelő engedélyek toohello eve.json fájl, hogy Logstash fogadására képes hello fájlt.</span><span class="sxs-lookup"><span data-stu-id="de650-148">Make sure toogive hello correct permissions toohello eve.json file so that Logstash can ingest hello file.</span></span>
    
    ```
    sudo chmod 775 /var/log/suricata/eve.json
    ```

1. <span data-ttu-id="de650-149">toostart Logstash hello paranccsal:</span><span class="sxs-lookup"><span data-stu-id="de650-149">toostart Logstash run hello command:</span></span>

    ```
    sudo /etc/init.d/logstash start
    ```

<span data-ttu-id="de650-150">További telepítésével kapcsolatos utasításokat Logstash, tekintse meg a toohello [dokumentációs](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span><span class="sxs-lookup"><span data-stu-id="de650-150">For further instructions on installing Logstash, refer toohello [official documentation](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)</span></span>

### <a name="install-kibana"></a><span data-ttu-id="de650-151">Kibana telepítése</span><span class="sxs-lookup"><span data-stu-id="de650-151">Install Kibana</span></span>

1. <span data-ttu-id="de650-152">Futtassa a következő parancsok tooinstall Kibana hello:</span><span class="sxs-lookup"><span data-stu-id="de650-152">Run hello following commands tooinstall Kibana:</span></span>

    ```
    curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
    tar xzvf kibana-5.2.0-linux-x86_64.tar.gz

    ```
1. <span data-ttu-id="de650-153">toorun Kibana hello parancsokat használja:</span><span class="sxs-lookup"><span data-stu-id="de650-153">toorun Kibana use hello commands:</span></span>

    ```
    cd kibana-5.2.0-linux-x86_64/
    ./bin/kibana
    ```

1. <span data-ttu-id="de650-154">tooview a Kibana webes felület, keresse meg a túl`http://localhost:5601`</span><span class="sxs-lookup"><span data-stu-id="de650-154">tooview your Kibana web interface, navigate too`http://localhost:5601`</span></span>
1. <span data-ttu-id="de650-155">Ebben a forgatókönyvben hello index használt hello Suricata naplózza a mintája "logstash-*"</span><span class="sxs-lookup"><span data-stu-id="de650-155">For this scenario, hello index pattern used for hello Suricata logs is "logstash-*"</span></span>

1. <span data-ttu-id="de650-156">Ha távolról tooview hello Kibana irányítópult, hozzon létre NSG bejövő szabály hozzáférést túl**port 5601**.</span><span class="sxs-lookup"><span data-stu-id="de650-156">If you want tooview hello Kibana dashboard remotely, create an inbound NSG rule allowing access too**port 5601**.</span></span>

### <a name="create-a-kibana-dashboard"></a><span data-ttu-id="de650-157">Kibana irányítópult létrehozása</span><span class="sxs-lookup"><span data-stu-id="de650-157">Create a Kibana dashboard</span></span>

<span data-ttu-id="de650-158">Ez a cikk adtunk meg tooview trendek egy minta-irányítópult és a riasztások részletes adatait.</span><span class="sxs-lookup"><span data-stu-id="de650-158">For this article, we have provided a sample dashboard for you tooview trends and details in your alerts.</span></span>

1. <span data-ttu-id="de650-159">Hello irányítópult-fájl letöltésére [Itt](https://aka.ms/networkwatchersuricatadashboard), hello képi megjelenítés fájl [Itt](https://aka.ms/networkwatchersuricatavisualization), és hello mentett keresési fájl [Itt](https://aka.ms/networkwatchersuricatasavedsearch).</span><span class="sxs-lookup"><span data-stu-id="de650-159">Download hello dashboard file [here](https://aka.ms/networkwatchersuricatadashboard), hello visualization file [here](https://aka.ms/networkwatchersuricatavisualization), and hello saved search file [here](https://aka.ms/networkwatchersuricatasavedsearch).</span></span>

1. <span data-ttu-id="de650-160">A hello **felügyeleti** lapon a Kibana, lépjen túl**mentett objektumok** mindhárom fájlt importálja.</span><span class="sxs-lookup"><span data-stu-id="de650-160">Under hello **Management** tab of Kibana, navigate too**Saved Objects** and import all three files.</span></span> <span data-ttu-id="de650-161">Ezután a hello **irányítópult** megnyithatja lapon és a minta-irányítópult hello.</span><span class="sxs-lookup"><span data-stu-id="de650-161">Then from hello **Dashboard** tab you can open and load hello sample dashboard.</span></span>

<span data-ttu-id="de650-162">A saját képi megjelenítések és saját egyik fontos metrikák felé szabott irányítópultok is létrehozhat.</span><span class="sxs-lookup"><span data-stu-id="de650-162">You can also create your own visualizations and dashboards tailored towards metrics of your own interest.</span></span> <span data-ttu-id="de650-163">További információk Kibana képi megjelenítés létrehozása Kibana tartozó [dokumentációs](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span><span class="sxs-lookup"><span data-stu-id="de650-163">Read more about creating Kibana visualizations from Kibana's [official documentation](https://www.elastic.co/guide/en/kibana/current/visualize.html).</span></span>

![kibana irányítópult][2]

### <a name="visualize-ids-alert-logs"></a><span data-ttu-id="de650-165">Riasztási naplók Azonosítók megjelenítése</span><span class="sxs-lookup"><span data-stu-id="de650-165">Visualize IDS alert logs</span></span>

<span data-ttu-id="de650-166">hello minta-irányítópult biztosít több képi megjelenítések hello Suricata értesítési naplói:</span><span class="sxs-lookup"><span data-stu-id="de650-166">hello sample dashboard provides several visualizations of hello Suricata alert logs:</span></span>

1. <span data-ttu-id="de650-167">A riasztások GeoIP – hello terjesztése riasztások származási (IP által meghatározott) földrajzi helye alapján szerint megjelenítő térkép szerint</span><span class="sxs-lookup"><span data-stu-id="de650-167">Alerts by GeoIP – a map showing hello distribution of alerts by their country of origin based on geographic location (determined by IP)</span></span>

    ![földrajzi ip][3]

1. <span data-ttu-id="de650-169">A felső 10 riasztások – hello leggyakoribb 10 kiváltott riasztások összefoglaló adatait és leírását.</span><span class="sxs-lookup"><span data-stu-id="de650-169">Top 10 Alerts – a summary of hello 10 most frequent triggered alerts and their description.</span></span> <span data-ttu-id="de650-170">Egyéni riasztást kattintva szűrők hello irányítópult toohello információk toothat adott riasztásra vonatkozó le.</span><span class="sxs-lookup"><span data-stu-id="de650-170">Clicking an individual alert filters down hello dashboard toohello information pertaining toothat specific alert.</span></span>

    ![Kép: 4][4]

1. <span data-ttu-id="de650-172">Riasztások – száma hello hello szabálykészletben által kiváltott figyelmeztetések teljes száma</span><span class="sxs-lookup"><span data-stu-id="de650-172">Number of Alerts – hello total count of alerts triggered by hello ruleset</span></span>

    ![Kép: 5][5]

1. <span data-ttu-id="de650-174">Első 20 forrás vagy a cél IP-címek és portok - megjelenítő tortadiagramok hello felső 20 IP-címek és portok, amely riasztást küld indított volt.</span><span class="sxs-lookup"><span data-stu-id="de650-174">Top 20 Source/Destination IPs/Ports - pie charts showing hello top 20 IPs and ports that alerts were triggered on.</span></span> <span data-ttu-id="de650-175">Szűrést végezhet le a megadott IP-címek és portok toosee, hogy hány és milyen típusú riasztások aktiválódnak alatt.</span><span class="sxs-lookup"><span data-stu-id="de650-175">You can filter down on specific IPs/ports toosee how many and what kind of alerts are being triggered.</span></span>

    ![a kép 6][6]

1. <span data-ttu-id="de650-177">Riasztási összegzése – egy tábla minden egyes riasztás részletes összegzése.</span><span class="sxs-lookup"><span data-stu-id="de650-177">Alert Summary – a table summarizing specific details of each individual alert.</span></span> <span data-ttu-id="de650-178">Testre szabhatja a tábla tooshow más paramétereket az egyes riasztások iránt.</span><span class="sxs-lookup"><span data-stu-id="de650-178">You can customize this table tooshow other parameters of interest for each alert.</span></span>

    ![kép 7][7]

<span data-ttu-id="de650-180">Egyéni képi megjelenítések és irányítópultokat hozhat létre további dokumentációiért lásd: [Kibana tartozó dokumentációs](https://www.elastic.co/guide/en/kibana/current/introduction.html).</span><span class="sxs-lookup"><span data-stu-id="de650-180">For more documentation on creating custom visualizations and dashboards, see [Kibana’s official documentation](https://www.elastic.co/guide/en/kibana/current/introduction.html).</span></span>

## <a name="conclusion"></a><span data-ttu-id="de650-181">Összegzés</span><span class="sxs-lookup"><span data-stu-id="de650-181">Conclusion</span></span>

<span data-ttu-id="de650-182">Csomag kombinálásával rögzíti a megadott hálózati figyelőt, és nyílt forráskódú Azonosítók eszközöket, például a Suricata, esetében fenyegetések számos hálózati behatolásérzékelési hajthat végre.</span><span class="sxs-lookup"><span data-stu-id="de650-182">By combining packet captures provided by Network Watcher and open source IDS tools such as Suricata, you can perform network intrusion detection for a wide range of threats.</span></span> <span data-ttu-id="de650-183">Ezek az irányítópultok engedélyezése meg tooquickly direkt trendek és a hálózaton belüli rendellenességek észlelését, valamint elmerülne a rendszer toodiscover okait adatok például a rosszindulatú felhasználók ügynökök vagy sebezhető portok riasztások hello be.</span><span class="sxs-lookup"><span data-stu-id="de650-183">These dashboards allow you tooquickly spot trends and anomalies within your network, as well dig into hello data toodiscover root causes of alerts such as malicious user agents or vulnerable ports.</span></span> <span data-ttu-id="de650-184">A kibontott adatok hogyan tooreact tooand megvédje a hálózatot káros behatolás tett kísérletek tájékozott döntést hozni, és hozzon létre szabályokat tooprevent jövőbeli behatolások tooyour hálózatot.</span><span class="sxs-lookup"><span data-stu-id="de650-184">With this extracted data, you can make informed decisions on how tooreact tooand protect your network from any harmful intrusion attempts, and create rules tooprevent future intrusions tooyour network.</span></span>

## <a name="next-steps"></a><span data-ttu-id="de650-185">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="de650-185">Next steps</span></span>

<span data-ttu-id="de650-186">Ismerje meg, hogyan tootrigger csomag rögzíti látogasson el a riasztások [csomag rögzítési toodo proaktív hálózati figyelést az Azure Functions](network-watcher-alert-triggered-packet-capture.md)</span><span class="sxs-lookup"><span data-stu-id="de650-186">Learn how tootrigger packet captures based on alerts by visiting [Use packet capture toodo proactive network monitoring with Azure Functions](network-watcher-alert-triggered-packet-capture.md)</span></span>

<span data-ttu-id="de650-187">Ismerje meg, hogyan toovisualize az NSG-folyamat naplózza a Power BI ellátogatva [megjelenítése NSG forgalomáramlás naplók és a Power bi-ban](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="de650-187">Learn how toovisualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>



<!-- images -->
[1]: ./media/network-watcher-intrusion-detection-open-source-tools/figure1.png
[2]: ./media/network-watcher-intrusion-detection-open-source-tools/figure2.png
[3]: ./media/network-watcher-intrusion-detection-open-source-tools/figure3.png
[4]: ./media/network-watcher-intrusion-detection-open-source-tools/figure4.png
[5]: ./media/network-watcher-intrusion-detection-open-source-tools/figure5.png
[6]: ./media/network-watcher-intrusion-detection-open-source-tools/figure6.png
[7]: ./media/network-watcher-intrusion-detection-open-source-tools/figure7.png
