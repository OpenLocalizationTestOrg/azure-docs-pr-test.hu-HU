---
title: "aaaMonitor és kezelése az Ambari REST API - Azure HDInsight Hadoop |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse Ambari toomonitor és az Azure HDInsight Hadoop-fürtök kezelése. Ebből a dokumentumból megtudhatja, hogyan toouse hello Ambari REST API-t tartalmazza a HDInsight-fürtök."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 2400530f-92b3-47b7-aa48-875f028765ff
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/07/2017
ms.author: larryfr
ms.openlocfilehash: 1866a77c8e402231bccbcfba7174253aca41339b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-hdinsight-clusters-by-using-hello-ambari-rest-api"></a>HDInsight-fürtök kezelése az Ambari REST API hello segítségével

[!INCLUDE [ambari-selector](../../includes/hdinsight-ambari-selector.md)]

Ismerje meg, hogyan toouse Ambari REST API toomanage hello és az Azure HDInsight Hadoop-fürtök figyelése.

Apache Ambari egyszerűbbé teszi a hello kezelése és figyelése a Hadoop fürtök egy egyszerű toouse webes felhasználói felület és a REST API-k megadásával. A HDInsight-fürtök hello Linux operációs rendszert használó Ambari szerepel. Ambari toomonitor hello fürtöt használ, és a konfigurációs módosításokat.

## <a id="whatis"></a>Mi az az Ambari

[Apache Ambari](http://ambari.apache.org) webes felhasználói felület használt tooprovision, kezeléséhez, és figyelheti a Hadoop-fürtök biztosít. A fejlesztők integrálható a ezeket a képességeket a alkalmazások hello segítségével [Ambari REST API-k](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

Alapértelmezés szerint a Linux-alapú HDInsight-fürtök Ambari valósul meg.

## <a name="how-toouse-hello-ambari-rest-api"></a>Hogyan toouse hello Ambari REST API

> [!IMPORTANT]
> hello útmutatást és példákat a jelen dokumentum igényelnek a Linux operációs rendszert használó HDInsight-fürtöt. További információkért lásd: [első lépései a hdinsight eszközzel](hdinsight-hadoop-linux-tutorial-get-started.md).

Ebben a dokumentumban hello példák hello Bourne rendszerhéj (bash) és a PowerShell-okat. példák alapján történő teszteléskor az GNU hello bash bash 4.3.11, de más Unix ismertetése együtt kell működnie. hello PowerShell-példák PowerShell 5.0 teszteltük, de a PowerShell 3.0-s vagy újabb verzióját kell működnie.

Ha használja a hello __Bourne rendszerhéj__ (Bash), rendelkeznie kell hello következőkkel:

* [cURL](http://curl.haxx.se/): cURL egy segédprogram, amely a REST API-khoz használt toowork hello parancssorból lehet. Az ebben a dokumentumban az Ambari REST API hello használt toocommunicate is.

E Bash vagy a PowerShell használatával, rendelkeznie kell [jq](https://stedolan.github.io/jq/) telepítve. Jq olyan eszköz, amellyel a JSON-dokumentumok használata. Szerepel, **összes** hello Bash példák és **egy** hello PowerShell-példák.

### <a name="base-uri-for-ambari-rest-api"></a>Alap URI-JÁNAK Ambari Rest API

hello hello Ambari REST API-t a HDInsight az alap URI-azonosító https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME, ahol **CLUSTERNAME** hello a fürt neve van.

> [!IMPORTANT]
> Amíg a fürtnév hello hello a teljesen minősített tartománynév (FQDN) része hello URI (CLUSTERNAME.azurehdinsight.net) azonban nem, más előfordulás hello URI-és nagybetűk. Például, ha a fürt neve `MyCluster`, az alábbiakban hello érvényes URI-azonosítók:
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/MyCluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/MyCluster`
> 
> hello következő URI-azonosítók hibát vissza, mert hello hello neve második előfordulása nem hello. Javítsa ki a eset.
> 
> `https://mycluster.azurehdinsight.net/api/v1/clusters/mycluster`
>
> `https://MyCluster.azurehdinsight.net/api/v1/clusters/mycluster`

### <a name="authentication"></a>Authentication

A HDInsight tooAmbari csatlakozás igényel a HTTPS PROTOKOLLT. Használjon hello rendszergazdai fiók neve (hello alapértelmezett érték **admin**) és a fürt létrehozása során megadott jelszót.

## <a name="examples-authentication-and-parsing-json"></a>Példák: Hitelesítés és elemzése JSON

hello a következő példák azt mutatják be, hogyan toomake hello egy GET kérelmet kiinduló Ambari REST API:

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
```

> [!IMPORTANT]
> hello Bash példák ebben a dokumentumban hajtsa végre a következő feltételek hello:
>
> * hello bejelentkezési név hello fürt az alapértelmezett érték hello `admin`.
> * `$PASSWORD`a HDInsight bejelentkezési parancs hello hello jelszót tartalmaz. Használja ezt az értéket is megadhat `PASSWORD='mypassword'`.
> * `$CLUSTERNAME`hello fürt hello nevét tartalmazza. Használja ezt az értéket is megadhat`set CLUSTERNAME='clustername'`

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$resp.Content
```

> [!IMPORTANT]
> hello PowerShell-példák ebben a dokumentumban hajtsa végre a következő feltételek hello:
>
> * `$creds`a hitelesítő objektum, amely tartalmazza a hello rendszergazda felhasználónevet és jelszót hello fürthöz van. Használja ezt az értéket is megadhat `$creds = Get-Credential -UserName "admin" -Message "Enter hello HDInsight login"` és hello hitelesítő adatokat biztosít.
> * `$clusterName`egy olyan karakterlánc, amely hello fürt hello nevét tartalmazza. Használja ezt az értéket is megadhat `$clusterName="clustername"`.

Mindkét példák adja vissza egy JSON-dokumentum információkat a következő példa hasonló toohello karakterlánccal kezdődik:

```json
{
"href" : "http://10.0.0.10:8080/api/v1/clusters/CLUSTERNAME",
"Clusters" : {
    "cluster_id" : 2,
    "cluster_name" : "CLUSTERNAME",
    "health_report" : {
    "Host/stale_config" : 0,
    "Host/maintenance_state" : 0,
    "Host/host_state/HEALTHY" : 7,
    "Host/host_state/UNHEALTHY" : 0,
    "Host/host_state/HEARTBEAT_LOST" : 0,
    "Host/host_state/INIT" : 0,
    "Host/host_status/HEALTHY" : 7,
    "Host/host_status/UNHEALTHY" : 0,
    "Host/host_status/UNKNOWN" : 0,
    "Host/host_status/ALERT" : 0
    ...
```

### <a name="parsing-json-data"></a>JSON-adatok elemzése

hello alábbi példában `jq` tooparse hello válasz JSON-dokumentum, és csak hello megjelenítése `health_report` hello eredmények adatait.

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME" \
| jq '.Clusters.health_report'
```

A PowerShell 3.0-s és újabb rendszer biztosít hello `ConvertFrom-Json` parancsmag, amely olyan objektumot, amely a powershellből könnyebb toowork hello JSON-dokumentum alakítja. hello alábbi példában `ConvertFrom-Json` toodisplay csak hello `health_report` hello eredmények adatait.

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.Clusters.health_report
```

> [!NOTE]
> Ez a dokumentum használatát a legtöbb példa során `ConvertFrom-Json` toodisplay elemek hello-dokumentumból, hello [frissítés Ambari konfigurációs](#example-update-ambari-configuration) példa jq használ. Jq használatban van ebben a példában tooconstruct hello JSON-válasz dokumentum egy új sablont.

Hello REST API-t a teljes referenciáért lásd: [Ambari API-referencia V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

## <a name="example-get-hello-fqdn-of-cluster-nodes"></a>Példa: Hello fürtcsomópont FQDN beolvasása

A HDInsight használata, ha esetleg tooknow hello teljesen minősített tartományneve (FQDN) egy fürt csomópontja. Hello FQDN a hello segítségével a következő példák hello hello fürt különböző csomópontja egyszerűen beolvashatók:

* **Minden csomópont**

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" \
    | jq '.items[].Hosts.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.Hosts.host_name
    ```

* **HEAD csomópontok**

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/HDFS/components/NAMENODE" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/NAMENODE" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

* **Munkavégző csomópontokhoz**

    ```bash
    curl -u admin:PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/HDFS/components/DATANODE" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/HDFS/components/DATANODE" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

* **Zookeeper csomópontok**

    ```bash
    curl -u admin:PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" \
    | jq '.host_components[].HostRoles.host_name'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/ZOOKEEPER/components/ZOOKEEPER_SERVER" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.host_components.HostRoles.host_name
    ```

## <a name="example-get-hello-internal-ip-address-of-cluster-nodes"></a>Példa: Hello belső IP-cím fürtcsomópontok beolvasása

> [!IMPORTANT]
> a szakaszban szereplő példák hello által visszaadott hello IP-címek vannak, nem közvetlenül elérhető over internet hello. Csak azok hello Azure virtuális hálózat, amely tartalmazza a HDInsight-fürt hello belülről érhetők el.
>
> További információk a HDInsight és virtuális hálózatok: [kiterjesztése HDInsight képességek egyéni Azure virtuális hálózat használatával](hdinsight-extend-hadoop-virtual-network.md).

toofind hello IP-cím, ismernie kell a hello belső teljesen minősített tartományneve (FQDN) hello fürtcsomópontok. Miután hello teljes Tartománynevét, majd kaphat a hello állomás hello IP-címét. hello alábbi példák először Ambari lekérdezése a hello hello minden gazdagép-csomópontok teljes Tartománynevét, majd hello IP-cím az egyes állomások Ambari lekérdezése.

```bash
for HOSTNAME in $(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts" | jq -r '.items[].Hosts.host_name')
do
    IP=$(curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/hosts/$HOSTNAME" | jq -r '.Hosts.ip')
  echo "$HOSTNAME <--> $IP"
done
```

```powershell
$uri = "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/hosts"
$resp = Invoke-WebRequest -Uri $uri -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
foreach($item in $respObj.items) {
    $hostName = [string]$item.Hosts.host_name
    $hostInfoResp = Invoke-WebRequest -Uri "$uri/$hostName" `
        -Credential $creds
    $hostInfoObj = ConvertFrom-Json $hostInfoResp 
    $hostIp = $hostInfoObj.Hosts.ip
    "$hostName <--> $hostIp"
}
```

## <a name="example-get-hello-default-storage"></a>Példa: Hello alapértelmezett tárolási beolvasása

HDInsight-fürtök létrehozásakor kell használnia az Azure Storage-fiók vagy a Data Lake Store hello alapértelmezett tárolóként hello fürthöz. Használhatja Ambari tooretrieve ezt az információt hello fürt létrehozása után. Ha például azt szeretné, hogy toohello adattároló tooread/írás HDInsight kívül.

hello alábbi példák lekérése hello alapértelmezett tárolási konfigurációt hello fürt:

```bash
curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
| jq '.items[].configurations[].properties["fs.defaultFS"] | select(. != null)'
```

```powershell
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties.'fs.defaultFS'
```

> [!IMPORTANT]
> Ezek a példák vissza hello első alkalmazott konfiguráció toohello kiszolgáló (`service_config_version=1`) amely tartalmazza ezt az információt. Egy érték, amely a fürt létrehozása után módosítva lett letöltésekor, toolist hello konfigurációs verziók kell, és hello legújabb beolvasása.

hello visszatérési érték a következő példák hello hasonló tooone:

* `wasb://CONTAINER@ACCOUNTNAME.blob.core.windows.net`-Ez azt jelzi, hogy hello fürt által használt Azure Storage-fiók alapértelmezett tárolási. Hello `ACCOUNTNAME` értéke hello tárfiók hello neve. Hello `CONTAINER` részét hello blob tároló hello tárfiókban hello neve. hello tároló hello HDFS-kompatibilis tároló hello fürt hello gyökérkönyvtárában.

* `adl://home`-Ez azt jelzi, hogy hello fürt által használt egy Azure Data Lake Store alapértelmezett tárolási.

    toofind hello Data Lake Store-fiók neve, a következő példák hello használata:

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.hostname"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.hostname'
    ```

    hello visszatérési értéke hasonló túl`ACCOUNTNAME.azuredatalakestore.net`, ahol `ACCOUNTNAME` hello hello Data Lake Store-fiók neve.

    Data Lake Store hello tárolási hello fürthöz, a következő példák használata hello tartalmazó belüli toofind hello könyvtár:

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" \
    | jq '.items[].configurations[].properties["dfs.adls.home.mountpoint"] | select(. != null)'
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.items.configurations.properties.'dfs.adls.home.mountpoint'
    ```

    hello visszatérési értéke hasonló túl`/clusters/CLUSTERNAME/`. Ez az érték út belül hello Data Lake Store-fiók. Ez az elérési út hello HDFS kompatibilis fájlrendszer hello fürt hello gyökérmappájában. 

> [!NOTE]
> Hello `Get-AzureRmHDInsightCluster` parancsmag által biztosított [Azure PowerShell](/powershell/azure/overview) is vissza hello tárolással kapcsolatos hello fürthöz.


## <a name="example-get-configuration"></a>Példa: Get-konfiguráció

1. A fürt számára rendelkezésre álló hello konfigurációk beolvasása.

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    $respObj = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    $respObj.Content
    ```

    Ebben a példában egy JSON-dokumentum hello aktuális konfigurációs tartalmazó adja vissza (hello által azonosított *címke* érték) hello összetevők hello fürtön telepítve. hello következő példa egy fájlból egy Spark-fürt típusa által visszaadott hello adatokat.
   
   ```json
   "spark-metrics-properties" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   },
   "spark-thrift-fairscheduler" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   },
   "spark-thrift-sparkconf" : {
       "tag" : "INITIAL",
       "user" : "admin",
       "version" : 1
   }
   ```

2. Hello összetevő, amely érdekli hello konfigurációjának beolvasása. Hello a következő példában cserélje le `INITIAL` hello előző kérelem által visszaadott hello címke értékkel.

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=core-site&tag=INITIAL"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=core-site&tag=INITIAL" `
        -Credential $creds
    $resp.Content
    ```

    Ez a példa adja vissza egy JSON-dokumentum hello hello vonatkozó aktuális konfigurációját tartalmazó `core-site` összetevő.

## <a name="example-update-configuration"></a>Példa: Konfigurációjának frissítése

1. Töltse le a hello jelenlegi konfigurációt, amely Ambari hello "kívánt konfiguráció" tárolja:

    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME?fields=Clusters/desired_configs"
    ```

    ```powershell
    Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName`?fields=Clusters/desired_configs" `
        -Credential $creds
    ```

    Ebben a példában egy JSON-dokumentum hello aktuális konfigurációs tartalmazó adja vissza (hello által azonosított *címke* érték) hello összetevők hello fürtön telepítve. hello következő példa egy fájlból egy Spark-fürt típusa által visszaadott hello adatokat.
   
    ```json
    "spark-metrics-properties" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    },
    "spark-thrift-fairscheduler" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    },
    "spark-thrift-sparkconf" : {
        "tag" : "INITIAL",
        "user" : "admin",
        "version" : 1
    }
    ```
   
    Ebből a listából kell hello összetevő toocopy hello neve (például **spark\_thrift\_sparkconf** és hello **címke** érték.

2. Lekéri a hello összetevő és a címke hello konfigurálása hello a következő parancsok használatával:
   
    ```bash
    curl -u admin:$PASSWORD -sS -G "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/configurations?type=spark-thrift-sparkconf&tag=INITIAL" \
    | jq --arg newtag $(echo version$(date +%s%N)) '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    ```powershell
    $epoch = Get-Date -Year 1970 -Month 1 -Day 1 -Hour 0 -Minute 0 -Second 0
    $now = Get-Date
    $unixTimeStamp = [math]::truncate($now.ToUniversalTime().Subtract($epoch).TotalMilliSeconds)
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations?type=spark-thrift-sparkconf&tag=INITIAL" `
        -Credential $creds
    $resp.Content | jq --arg newtag "version$unixTimeStamp" '.items[] | del(.href, .version, .Config) | .tag |= $newtag | {"Clusters": {"desired_config": .}}' > newconfig.json
    ```

    > [!NOTE]
    > Cserélje le **spark-thrift-sparkconf** és **kezdeti** hello összetevő és tooretrieve hello konfigurációja kívánt címkét.
   
    Jq használt tooturn hello származó adatok HDInsight új konfigurációs sablonba. Pontosabban ezek a példák hajtsa végre a következő műveletek hello:
   
    * Létrehoz egy egyedi értéket tartalmazó hello karakterlánc "version" és a hello dátum, ami a `newtag`.

    * Az új szükségeskonfiguráció hello legfelső szintű dokumentum létrehozása.

    * Lekérdezi hello hello tartalmát `.items[]` tömb, és hozzáadja azt a hello **desired_config** elemet.

    * Törlések hello `href`, `version`, és `Config` elemek, ezeket az elemeket, nem szükséges toosubmit egy új konfigurációt.

    * Hozzáad egy `tag` elem értéke az `version#################`. hello numerikus részét alapul hello aktuális dátumot. Minden egyes-konfiguráció egyedi kódot kell rendelkeznie.
     
    Végezetül hello mentett adatok toohello `newconfig.json` dokumentum. hello dokumentumstruktúrával megjelenjen-e a következő példa hasonló toohello:
     
     ```json
    {
        "Clusters": {
            "desired_config": {
            "tag": "version1459260185774265400",
            "type": "spark-thrift-sparkconf",
            "properties": {
                ....
            },
            "properties_attributes": {
                ....
            }
        }
    }
    ```

3. Nyissa meg hello `newconfig.json` hello dokumentum és a módosítása vagy hozzáadása értékek `properties` objektum. hello változásai példa hello értékének `"spark.yarn.am.memory"` a `"1g"` túl`"3g"`. Bővíti ezenkívül `"spark.kryoserializer.buffer.max"` értékkel rendelkező `"256m"`.
   
        "spark.yarn.am.memory": "3g",
        "spark.kyroserializer.buffer.max": "256m",
   
    Ha elkészült, hogy a módosításokat, mentse a hello fájlt.

4. A következő parancsok toosubmit hello frissített konfigurációs tooAmbari hello használata.
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" -X PUT -d @newconfig.json "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME"
    ```

    ```powershell
    $newConfig = Get-Content .\newconfig.json
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body $newConfig
    $resp.Content
    ```
   
    Ezek a parancsok nyújt hello hello tartalmát **newconfig.json** toohello fürt fájlt, hello új szükségeskonfiguráció-konfiguráció. hello kérelem JSON-dokumentum adja vissza. Hello **versionTag** elem ebben a dokumentumban meg kell felelnie a hello verzió elküldése megtörtént, és hello **configs** objektum tartalmazza a kért hello konfigurációs módosításokat.

### <a name="example-restart-a-service-component"></a>Példa: Indítsa újra a szolgáltatás valamelyik összetevője

Ezen a ponton hello Ambari webes felhasználói felület tekinti meg, ha hello Spark szolgáltatást azt jelzi, hogy kell-e toobe hello új konfiguráció életbe léptetéséhez újra. A következő lépéseket toorestart hello szolgáltatás hello használata.

1. A következő tooenable karbantartási mód hello Spark szolgáltatást hello használata:

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning on maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"ON"}}}'
    $resp.Content
    ```
   
    Ezek a parancsok küldése egy JSON dokumentum toohello kiszolgáló, amely bekapcsolja a karbantartási módból. Ellenőrizheti, hogy hello szolgáltatás most már karbantartási módban van hello kérelem a következő használatával:
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK" \
    | jq .ServiceInfo.maintenance_state
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK2" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.ServiceInfo.maintenance_state
    ```
   
    hello visszatérési értéke `ON`.

2. Következő lépésként az hello tooturn ki hello szolgáltatást a következő:

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"INSTALLED"}}}'
    $resp.Content
    ```
    
    a rendszer a következő példa hasonló toohello hello választ:
   
    ```json
    {
        "href" : "http://10.0.0.18:8080/api/v1/clusters/CLUSTERNAME/requests/29",
        "Requests" : {
            "id" : 29,
            "status" : "Accepted"
        }
    }
    ```
    
    > [!IMPORTANT]
    > Hello `href` ezt az URI által visszaadott érték hello fürtcsomópont hello belső IP-címét használja. toouse hello hello-fürt külső hello fürtből, akkor hello "10.0.0.18:8080" részét cserélje le. 
    
    a következő parancsok hello hello kérelem hello állapotának beolvasása:

    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/requests/29" \
    | jq .Requests.request_status
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/requests/29" `
        -Credential $creds
    $respObj = ConvertFrom-Json $resp.Content
    $respObj.Requests.request_status
    ```

    A válasz `COMPLETED` azt jelzi, hogy adott hello kérelem befejeződött.

3. Hello előző kérelem után használja a következő toostart hello szolgáltatás hello.
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```
   
    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo":{"context":"_PARSE_.STOP.SPARK","operation_level":{"level":"SERVICE","cluster_name":"CLUSTERNAME","service_name":"SPARK"}},"Body":{"ServiceInfo":{"state":"STARTED"}}}'
    ```
    hello szolgáltatás hello új konfigurációt használ.

4. Végezetül használja a következő tooturn ki a karbantartási módot hello.
   
    ```bash
    curl -u admin:$PASSWORD -sS -H "X-Requested-By: ambari" \
    -X PUT -d '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}' \
    "https://$CLUSTERNAME.azurehdinsight.net/api/v1/clusters/$CLUSTERNAME/services/SPARK"
    ```

    ```powershell
    $resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/services/SPARK" `
        -Credential $creds `
        -Method PUT `
        -Headers @{"X-Requested-By" = "ambari"} `
        -Body '{"RequestInfo": {"context": "turning off maintenance mode for SPARK"},"Body": {"ServiceInfo": {"maintenance_state":"OFF"}}}'
    ```

## <a name="next-steps"></a>Következő lépések

Hello REST API-t a teljes referenciáért lásd: [Ambari API-referencia V1](https://github.com/apache/ambari/blob/trunk/ambari-server/docs/api/v1/index.md).

