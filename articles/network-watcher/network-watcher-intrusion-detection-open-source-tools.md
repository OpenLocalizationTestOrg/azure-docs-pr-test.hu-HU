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
# <a name="perform-network-intrusion-detection-with-network-watcher-and-open-source-tools"></a>Hajtsa végre a behatolás-észlelés hálózati nyílt forráskódú eszközök és hálózati figyelőt

Csomag rögzíti a végrehajtási hálózati behatolás rendszerek (Azonosítók) és a hálózati biztonsági figyelése (NSM) végrehajtása nyilvános kulcsokra épülő. Számos nyílt forráskódú Azonosítók eszköz, amely feldolgozza a csomag rögzíti, és keresse meg az esetleges hálózati behatolások és a rosszindulatú tevékenységhez van. Használja a hálózati figyelő által biztosított hello csomagok készítését, elemezheti a hálózati biztonsági rések vagy káros behatolások.

Egy ilyen nyílt forráskódú eszköz Suricata, egy Azonosítók motor, amely szabálykészletek toomonitor hálózati forgalom használ, és elindítja a riasztásokat gyanús események előfordulásakor. Suricata kínál a többszálas motor, azaz a hálózati forgalom elemzése a nagyobb sebesség és a hatékonyság műveleteket hajthat végre. Suricata és platformképességei kapcsolatos további részletekért látogasson el a https://suricata-ids.org/ webhelyen.

## <a name="scenario"></a>Forgatókönyv

Ez a cikk azt ismerteti, hogyan másolatot a környezet tooperform tooset behatolásérzékelési használatával hálózati figyelőt, Suricata, hálózati és a rugalmas készlet hello. Hálózati figyelőt tartalmaz, akkor hello csomaggal használt tooperform hálózati behatolásérzékelési rögzíti. Suricata folyamatok hello csomagok rögzíti, és a csomagot, amely megfelel az adott szabálykészletben fenyegetések alapján eseményindító riasztások. Ezek a riasztások a naplófájl a helyi számítógépen tárolja. Hello rugalmas készlet használatával, Suricata által létrehozott hello naplók indexelhetők és toocreate Kibana irányítópulton, így a hello naplók vizuális ábrázolását, és egy azt jelenti, hogy tooquickly nyereség insights toopotential hálózati biztonsági rések használt.  

![egyszerű webes alkalmazás forgatókönyv][1]

Mindkét nyílt forráskódú eszközök beállítható egy Azure virtuális gépen, így tooperform az elemzés a saját Azure hálózati környezetben.

## <a name="steps"></a>Lépések

### <a name="install-suricata"></a>Suricata telepítése

Minden egyéb módszer telepítés esetén keresse fel a http://suricata.readthedocs.io/en/latest/install.html

1. Futtassa a következő parancsok hello hello parancssori terminált, a virtuális Gépet:

    ```
    sudo add-apt-repository ppa:oisf/suricata-stable
    sudo apt-get update
    sudo sudo apt-get install suricata
    ```

1. tooverify a telepítési hello paranccsal `suricata -h` toosee hello teljes parancsok listáját.

### <a name="download-hello-emerging-threats-ruleset"></a>Hello felmerülő veszélyek szabálykészletben letöltése

Ezen a ponton nem találtunk Suricata toorun szabályok. A saját szabályokat hozhat létre, ha az egyes veszélyforrások tooyour hálózati toodetect szeretné, vagy is fejlesztett használata szabálykészletek szolgáltatók, köztük a felmerülő veszélyek vagy Snort VRT szabályok számos. Hello ingyenesen elérhető felmerülő veszélyek szabálykészletben itt használjuk:

Töltse le a hello szabálykészletben, és másolja őket abba hello könyvtár:

```
wget http://rules.emergingthreats.net/open/suricata/emerging.rules.tar.gz
tar zxf emerging.rules.tar.gz
sudo cp -r rules /etc/suricata/
```

### <a name="process-packet-captures-with-suricata"></a>Folyamat csomagrögzítéseket Suricata

tooprocess csomag rögzíti Suricata, futtassa a következő parancs hello használata:

```
sudo suricata -c /etc/suricata/suricata.yaml -r <location_of_pcapfile>
```
hello fast.log fájl toocheck hello létrejövő riasztások, olvassa el:
```
tail -f /var/log/suricata/fast.log
```

### <a name="set-up-hello-elastic-stack"></a>A rugalmas készlet hello beállítása

Hello naplók Suricata előállító mi történik a hálózaton lévő értékes információkat tartalmaznak, amíg az ezekben a naplófájlokban hello legegyszerűbb tooread nem, és ismertetése. A rugalmas készlet hello Suricata összekötésével azt is mi kiválaszthatjuk toosearch Kibana irányítópult létrehozása, diagramot, elemzése és insights származik a naplók.

#### <a name="install-elasticsearch"></a>Elasticsearch telepítése

1. hello rugalmas verem 5.0-s verziójáról és a fent Java 8 igényel. Hello paranccsal `java -version` toocheck adott verziójában. Ha nem kell telepíteni, tekintse meg a toodocumentation a java [Oracle-webhely](http://docs.oracle.com/javase/8/docs/technotes/guides/install/install_overview.html)
1. Töltse le a hello helyes bináris csomagot a rendszer:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
    sudo dpkg -i elasticsearch-5.2.0.deb
    sudo /etc/init.d/elasticsearch start
    ```

    Egyéb módszerek található [Elasticsearch telepítése](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html)

1. Győződjön meg arról, hogy fut-e Elasticsearch hello paranccsal:

    ```
    curl http://127.0.0.1:9200
    ```

    A válasz hasonló toothis kell megjelennie:

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

Rugalmas keresési telepítése további útmutatásra van szüksége, tekintse meg a toohello lap [telepítése](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)

### <a name="install-logstash"></a>Logstash telepítése

1. tooinstall Logstash hello a következő parancsok futtatásával:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
1. Ezután azt kell tooconfigure Logstash tooread eve.json fájl hello kimenetből. Hozzon létre egy logstash.conf fájl használatával:

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. Adja hozzá a következő tartalom toohello fájl hello (Győződjön meg róla, hogy hello elérési toohello eve.json fájl helyes):

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

1. Győződjön meg arról, hogy toogive hello megfelelő engedélyek toohello eve.json fájl, hogy Logstash fogadására képes hello fájlt.
    
    ```
    sudo chmod 775 /var/log/suricata/eve.json
    ```

1. toostart Logstash hello paranccsal:

    ```
    sudo /etc/init.d/logstash start
    ```

További telepítésével kapcsolatos utasításokat Logstash, tekintse meg a toohello [dokumentációs](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)

### <a name="install-kibana"></a>Kibana telepítése

1. Futtassa a következő parancsok tooinstall Kibana hello:

    ```
    curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
    tar xzvf kibana-5.2.0-linux-x86_64.tar.gz

    ```
1. toorun Kibana hello parancsokat használja:

    ```
    cd kibana-5.2.0-linux-x86_64/
    ./bin/kibana
    ```

1. tooview a Kibana webes felület, keresse meg a túl`http://localhost:5601`
1. Ebben a forgatókönyvben hello index használt hello Suricata naplózza a mintája "logstash-*"

1. Ha távolról tooview hello Kibana irányítópult, hozzon létre NSG bejövő szabály hozzáférést túl**port 5601**.

### <a name="create-a-kibana-dashboard"></a>Kibana irányítópult létrehozása

Ez a cikk adtunk meg tooview trendek egy minta-irányítópult és a riasztások részletes adatait.

1. Hello irányítópult-fájl letöltésére [Itt](https://aka.ms/networkwatchersuricatadashboard), hello képi megjelenítés fájl [Itt](https://aka.ms/networkwatchersuricatavisualization), és hello mentett keresési fájl [Itt](https://aka.ms/networkwatchersuricatasavedsearch).

1. A hello **felügyeleti** lapon a Kibana, lépjen túl**mentett objektumok** mindhárom fájlt importálja. Ezután a hello **irányítópult** megnyithatja lapon és a minta-irányítópult hello.

A saját képi megjelenítések és saját egyik fontos metrikák felé szabott irányítópultok is létrehozhat. További információk Kibana képi megjelenítés létrehozása Kibana tartozó [dokumentációs](https://www.elastic.co/guide/en/kibana/current/visualize.html).

![kibana irányítópult][2]

### <a name="visualize-ids-alert-logs"></a>Riasztási naplók Azonosítók megjelenítése

hello minta-irányítópult biztosít több képi megjelenítések hello Suricata értesítési naplói:

1. A riasztások GeoIP – hello terjesztése riasztások származási (IP által meghatározott) földrajzi helye alapján szerint megjelenítő térkép szerint

    ![földrajzi ip][3]

1. A felső 10 riasztások – hello leggyakoribb 10 kiváltott riasztások összefoglaló adatait és leírását. Egyéni riasztást kattintva szűrők hello irányítópult toohello információk toothat adott riasztásra vonatkozó le.

    ![Kép: 4][4]

1. Riasztások – száma hello hello szabálykészletben által kiváltott figyelmeztetések teljes száma

    ![Kép: 5][5]

1. Első 20 forrás vagy a cél IP-címek és portok - megjelenítő tortadiagramok hello felső 20 IP-címek és portok, amely riasztást küld indított volt. Szűrést végezhet le a megadott IP-címek és portok toosee, hogy hány és milyen típusú riasztások aktiválódnak alatt.

    ![a kép 6][6]

1. Riasztási összegzése – egy tábla minden egyes riasztás részletes összegzése. Testre szabhatja a tábla tooshow más paramétereket az egyes riasztások iránt.

    ![kép 7][7]

Egyéni képi megjelenítések és irányítópultokat hozhat létre további dokumentációiért lásd: [Kibana tartozó dokumentációs](https://www.elastic.co/guide/en/kibana/current/introduction.html).

## <a name="conclusion"></a>Összegzés

Csomag kombinálásával rögzíti a megadott hálózati figyelőt, és nyílt forráskódú Azonosítók eszközöket, például a Suricata, esetében fenyegetések számos hálózati behatolásérzékelési hajthat végre. Ezek az irányítópultok engedélyezése meg tooquickly direkt trendek és a hálózaton belüli rendellenességek észlelését, valamint elmerülne a rendszer toodiscover okait adatok például a rosszindulatú felhasználók ügynökök vagy sebezhető portok riasztások hello be. A kibontott adatok hogyan tooreact tooand megvédje a hálózatot káros behatolás tett kísérletek tájékozott döntést hozni, és hozzon létre szabályokat tooprevent jövőbeli behatolások tooyour hálózatot.

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan tootrigger csomag rögzíti látogasson el a riasztások [csomag rögzítési toodo proaktív hálózati figyelést az Azure Functions](network-watcher-alert-triggered-packet-capture.md)

Ismerje meg, hogyan toovisualize az NSG-folyamat naplózza a Power BI ellátogatva [megjelenítése NSG forgalomáramlás naplók és a Power bi-ban](network-watcher-visualize-nsg-flow-logs-power-bi.md)



<!-- images -->
[1]: ./media/network-watcher-intrusion-detection-open-source-tools/figure1.png
[2]: ./media/network-watcher-intrusion-detection-open-source-tools/figure2.png
[3]: ./media/network-watcher-intrusion-detection-open-source-tools/figure3.png
[4]: ./media/network-watcher-intrusion-detection-open-source-tools/figure4.png
[5]: ./media/network-watcher-intrusion-detection-open-source-tools/figure5.png
[6]: ./media/network-watcher-intrusion-detection-open-source-tools/figure6.png
[7]: ./media/network-watcher-intrusion-detection-open-source-tools/figure7.png
