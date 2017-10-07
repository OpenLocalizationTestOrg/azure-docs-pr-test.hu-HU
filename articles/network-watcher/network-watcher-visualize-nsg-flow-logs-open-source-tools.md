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
# <a name="visualize-azure-network-watcher-nsg-flow-logs-using-open-source-tools"></a>Azure hálózati figyelő NSG folyamata naplók nyílt forráskódú eszközökkel megjelenítése

Hálózati biztonsági csoport folyamata naplók információkkal használható IP-bemenő és kimenő forgalmat a hálózati biztonsági csoportok ismertetése. A folyamat naplók megjelenítése kimenő és bejövő adatfolyamok / szabály alapján, hello NIC hello folyamata vonatkozik, hello folyamata (forrás vagy a cél IP, forrás vagy a cél Port Protocol), és ha hello forgalom lett engedélyezett vagy megtagadott 5 rekordos információt.

A folyamat naplók nehéz toomanually elemzési kell, és a dcu. Vannak azonban számos nyílt forráskódú eszköz, amelynek segítségével jelenítheti meg ezeket az adatokat. Ez a cikk nyújt a megoldás toovisualize a naplók hello rugalmas készlet használatával, amely lehetővé teszi tooquickly index és a folyamat a naplók Kibana irányítópult megjelenítése.

## <a name="scenario"></a>Forgatókönyv

Ebben a cikkben egy megoldást, amely lehetővé teszi a toovisualize hálózati biztonsági csoport folyamat használatával a naplókat, a rugalmas készlet hello üzembe helyezünk.  Egy Logstash bemeneti beépülő modul beszerzi hello folyamata naplók közvetlenül hello tárolási blob hello folyamata naplókat tartalmazó konfigurálva. Ezt követően hello rugalmas készlet használatával, hello folyamata naplók lesz indexelve és toocreate egy Kibana irányítópult toovisualize hello információkat használja.

![forgatókönyv][scenario]

## <a name="steps"></a>Lépések

### <a name="enable-network-security-group-flow-logging"></a>Engedélyezze a hálózati biztonsági csoport folyamata naplózás
Ebben az esetben rendelkeznie kell hálózati biztonsági csoport Flow naplózás legalább egy hálózati biztonsági csoport a fiók engedélyezve. A hálózati biztonsági Flow naplók engedélyezése útmutatásért tekintse meg a következő cikket toohello [bemutatása tooflow naplózási a hálózati biztonsági csoportok](network-watcher-nsg-flow-logging-overview.md).


### <a name="set-up-hello-elastic-stack"></a>A rugalmas készlet hello beállítása
NSG csatlakozzon a naplók és a rugalmas készlet hello flow, jelenleg milyen kiválaszthatjuk toosearch Kibana irányítópult létrehozása, diagramot, elemzése és elemzések származik a naplókat.

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
1. Ezután azt kell tooconfigure Logstash tooaccess, és hello folyamata naplók elemzése. Hozzon létre egy logstash.conf fájl használatával:

    ```
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

1. Adja hozzá a következő tartalom toohello fájl hello:

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

További telepítésével kapcsolatos utasításokat Logstash, tekintse meg a toohello [dokumentációs](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)

### <a name="install-hello-logstash-input-plugin-for-azure-blob-storage"></a>Hello Logstash bemeneti beépülő modul az Azure blob Storage telepítése

A Logstash beépülő modul lehetővé teszi a toodirectly belépési hello folyamata naplók a kijelölt tárfiókból. tooinstall a beépülő modul hello alapértelmezett Logstash telepítési könyvtárában (az az eset /usr/share/logstash/bin) parancsot hello:

```
logstash-plugin install logstash-input-azureblob
```

toostart Logstash hello paranccsal:

```
sudo /etc/init.d/logstash start
```

A beépülő modul kapcsolatos további információkért tekintse meg a toodocumentation [Itt](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)

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
1. Ebben a forgatókönyvben hello index hello folyamat használható a mintája "nsg-adatfolyam-logs". Módosíthatja a hello index mintát a logstash.conf fájl hello "kimeneti" szakaszában.

1. Ha távolról tooview hello Kibana irányítópult, hozzon létre NSG bejövő szabály hozzáférést túl**port 5601**.

### <a name="create-a-kibana-dashboard"></a>Kibana irányítópult létrehozása

Ez a cikk adtunk meg tooview trendek egy minta-irányítópult és a riasztások részletes adatait.

![1. ábra][1]

1. Hello irányítópult-fájl letöltésére [Itt](https://aka.ms/networkwatchernsgflowlogdashboard), hello képi megjelenítés fájl [Itt](https://aka.ms/networkwatchernsgflowlogvisualizations), és hello mentett keresési fájl [Itt](https://aka.ms/networkwatchernsgflowlogsearch).

1. A hello **felügyeleti** lapon a Kibana, lépjen túl**mentett objektumok** mindhárom fájlt importálja. Ezután a hello **irányítópult** megnyithatja lapon és a minta-irányítópult hello.

A saját képi megjelenítések és saját egyik fontos metrikák felé szabott irányítópultok is létrehozhat. További információk Kibana képi megjelenítés létrehozása Kibana tartozó [dokumentációs](https://www.elastic.co/guide/en/kibana/current/visualize.html).

### <a name="visualize-nsg-flow-logs"></a>NSG folyamata naplók megjelenítése

hello minta-irányítópult biztosít több képi megjelenítések hello folyamata naplói:

1. Döntési és irányok keresztül időpontjára - adatfolyamok a idő adatsorozat diagramjait adatfolyamok száma hello trendjét ábrázoló hello időszakra vonatkozóan. Hello egysége, és mindkét alábbi képi megjelenítést span szerkesztheti. Adatfolyamok döntési látható hello aránya engedélyezése vagy megtagadása döntések során forgalom irányát mutatja hello aránya bejövő és kimenő forgalom. Ezek a látványelemek vizsgálja meg a forgalmi adott idő alatt, és bármely igényeiben jelentkező vagy szokatlan mintákat keressen.

  ![2. ábra][2]

1. Cél/forrásport – által adatfolyamok a tortadiagramok megjelenítő hello lebontása flow tootheir megfelelő portok. Az ebben a nézetben láthatja a leggyakrabban használt portok. Ha egy adott portot hello kördiagram belül kattint, hello irányítópult hello részeinek szűrheti, hogy a port tooflows le.

  ![figure3][3]

1. Több adatfolyamok és legkorábbi napló időben – hello száma adatfolyamok rögzített, és hello dátum hello legkorábbi napló rögzített jeleníti meg.

  ![4. ábra][4]

1. Adatfolyamok NSG-t és a szabály – egy oszlopdiagramot hello terjesztési belül minden NSG forgalom, valamint hello terjesztése belüli egyes NSG-szabályok láthatók. Itt láthatja, melyik NSG-t, és a létrehozott szabályok hello legtöbb forgalmat.

  ![figure5][5]

1. Első 10 forrás vagy a cél IP-címek – hello első 10 forrás és cél IP-címek megjelenítő sávdiagramok. Módosíthatja a diagramok tooshow több vagy kevesebb felső IP-címek. Itt meg is tekintse meg a leggyakrabban előforduló az IP-címek hello, valamint a forgalom döntési hello (engedélyezése vagy megtagadása) végrehajtott minden IP felé.

  ![figure6][6]

1. Attribútumfolyam rekordokat – Ez a táblázat bemutatja, hello belül minden folyamat rekordot, valamint a megfelelő NGS és szabály található információt.

  ![figure7][7]

Hello lekérdezés sáv segítségével hello irányítópult hello tetején, végezhet a hello adatfolyamok, például az előfizetés-azonosító, erőforráscsoport-sablonok, szabály vagy bármely más változó érdeklő bármely paraméter alapján hello irányítópult le. További információ a Kibana a lekérdezések és a szűrők, tekintse meg a toohello [dokumentációs](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html)

## <a name="conclusion"></a>Összegzés

A rugalmas készlet hello hello hálózati biztonsági csoport folyamata naplók kombinálásával azt rendelkezik kapja meg hatékony és testre szabható módon toovisualize a hálózati forgalom. Ezek az irányítópultok tooquickly nyereség érdekében lehetővé teszi a hálózati forgalom, valamint a szűrő észrevételeket oszthatnak meg, és vizsgálja meg az összes esetleges rendellenességeket. Kibana használ, ezek az irányítópultok testre szabni, és hozzon létre adott képi megjelenítések toomeet bármilyen biztonsági, naplózási és megfelelőségi megfelelően.

## <a name="next-steps"></a>Következő lépések

Ismerje meg, hogyan toovisualize az NSG-folyamat naplózza a Power BI ellátogatva [megjelenítése NSG forgalomáramlás naplók és a Power bi-ban](network-watcher-visualize-nsg-flow-logs-power-bi.md)


<!--Image references-->

[scenario]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/scenario.png
[1]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure7.png
