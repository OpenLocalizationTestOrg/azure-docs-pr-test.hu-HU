---
title: "aaaLog Analytics napló search REST API |} Microsoft Docs"
description: "Ez az útmutató egy alapszintű hello használatát ismertető oktatóanyag Naplóelemzési keresni REST API hello Operations Management Suite (OMS), és példák, amelyek bemutatják, hogyan biztosít toouse hello parancsok."
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: 
ms.assetid: b4e9ebe8-80f0-418e-a855-de7954668df7
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: bwren
ms.openlocfilehash: dafe5eeb8cc11a339f2cbf78cec657e344d87cac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="log-analytics-log-search-rest-api"></a>A Naplóelemzési jelentkezzen search REST API-n
Ez az útmutató egy alapszintű oktatóanyag, beleértve a példákat a hello napló Analytics Search REST API használatát. A Naplóelemzési hello Operations Management Suite (OMS) részét képezi.

> [!NOTE]
> Ha a munkaterületet frissített toohello [új Log Analytics lekérdezési nyelv](log-analytics-log-search-upgrade.md), majd folytatja kell toouse hello örökölt lekérdezési nyelv hello napló keresése API az ebben a cikkben leírtak szerint.  Egy olyan új API megjelenik frissített munkaterületek, valamint hello dokumentáció frissülni fog ekkor. 

> [!NOTE]
> Naplóelemzési korábban meghívták az Operational Insights, ezért az erőforrás-szolgáltatón hello használt hello név.
>
>

## <a name="overview-of-hello-log-search-rest-api"></a>Hello napló Search REST API – áttekintés
hello napló Analytics Search REST API RESTful és hello Azure Resource Manager API keresztül érhető el. Ez a cikk példákat hello API használatával történő eléréséhez [ARMClient](https://github.com/projectkudu/ARMClient), egy nyílt forráskódú parancssori eszköz, amely egyszerűbbé teszi a meghívása hello Azure Resource Manager API-val. hello ARMClient használata sok beállítások tooaccess hello napló Analytics Search API közül. Másik lehetőség is toouse hello Azure PowerShell-modul OperationalInsights, amely keresési eléréséhez parancsmagokat tartalmaz. Ezekkel az eszközökkel hello Azure Resource Manager API toomake hívások tooOMS munkaterületek használják, és hajtsa végre a keresési parancsok rajtuk. hello API kiírja a keresési eredmények JSON formátumban, így toouse hello keresési eredmények között számos különböző módon programozott módon.

hello Azure Resource Manager használható keresztül egy [.NET-keretrendszerhez készült](https://msdn.microsoft.com/library/azure/dn910477.aspx) és hello [REST API](https://msdn.microsoft.com/library/azure/mt163658.aspx). toolearn több, tekintse át a kapcsolódó hello weblapok.

> [!NOTE]
> Ha egy összesítési paranccsal például `|measure count()` vagy `distinct`, minden hívás toosearch visszatérhessen legfeljebb 500 000 rekordot. Keresések, amelyek nem tartalmaznak egy összesítési parancs legfeljebb 5 000 rekordot ad vissza.
>
>

## <a name="basic-log-analytics-search-rest-api-tutorial"></a>Egyszerű napló Analytics Search REST API-oktatóanyag
### <a name="toouse-armclient"></a>toouse ARMClient
1. Telepítés [Chocolatey](https://chocolatey.org/), amelyen egy megnyitott forrás Package Manager Windows. Nyisson meg egy parancssort rendszergazdaként, és futtassa a parancsot a következő hello:

    ```
    @powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%ALLUSERSPROFILE%\chocolatey\bin
    ```
2. Telepítse a ARMClient hello a következő parancs futtatásával:

    ```
    choco install armclient
    ```

### <a name="tooperform-a-search-using-armclient"></a>a kívánt ARMClient használatával tooperform
1. Jelentkezzen be Microsoft-fiókjával vagy a munkahelyi vagy iskolai fiók használatával:

    ```
    armclient login
    ```

    Sikeres bejelentkezés megadott fiók összes kötött előfizetések toohello sorolja fel:

    ```
    PS C:\Users\SampleUserName> armclient login
    Welcome YourEmail@ORG.com (Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz)
    User: YourEmail@ORG.com, Tenant: zzzzzzzz-zzzz-zzzz-zzzz-zzzzzzzzzzzz (Example org)
    There are 3 subscriptions
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 1)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 2)
    Subscription xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx (Example Name 3)
    ```
2. Hello Operations Management Suite-munkaterület érhető el:

    ```
    armclient get /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces?api-version=2015-03-20
    ```

    Sikeres Get hívást volna kimeneti összes munkaterületek toohello előfizetés társítva:

    ```
    {
    "value": [
    {
      "properties": {
    "source": "External",
    "customerId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "portalUrl": "https://eus.login.mms.microsoft.com/SignIn.aspx?returnUrl=https%3a%2f%2feus.mms.microsoft.com%2fMain.aspx%3fcid%xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx"
      },
      "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourcegroups/oi-default-east-us/providers/microsoft.operationalinsights/workspaces/{WORKSPACE NAME}",
      "name": "{WORKSPACE NAME}",
      "type": "Microsoft.OperationalInsights/workspaces",
      "location": "East US"
       ]
    }
    ```
3. A keresési változó létrehozása:

    ```
    $mySearch = "{ 'top':150, 'query':'Error'}";
    ```
4. A Keresés az új keresés változó használatával:

    ```
    armclient post /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{WORKSPACE NAME}/search?api-version=2015-03-20 $mySearch
    ```

## <a name="log-analytics-search-rest-api-reference-examples"></a>Naplófájl Analytics Search REST API referencia példák
hello a következő példák bemutatják, hogyan használhatja a keresés API hello.

### <a name="search---actionread"></a>Keresési - művelet olvasása
**A minta URL-címe:**

```
    /subscriptions/{SubscriptionId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search?api-version=2015-03-20
```

**A kérelem:**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```
a következő táblázat hello hello tulajdonságait ismerteti.

| **Tulajdonság** | **Leírás** |
| --- | --- |
| Felső |az eredmények tooreturn hello maximális számát. |
| Jelöljön ki |Megfelelő mezőkben konzolban használt általában előtti és utáni paramétereket tartalmaz |
| előre |Előtagok hello megadott karakterlánc megfelel tooyour mezőket. |
| Post |Hozzáfűzi a megadott karakterlánc megfelel tooyour mezők hello. |
| lekérdezés |hello keresési lekérdezés toocollect használja, és adja vissza az eredményeket. |
| start |azt szeretné, hogy a található eredmények toobe hello időkerete hello elejére. |
| Vége |azt szeretné, hogy a található eredmények toobe hello időkerete hello végét. |

**Válasz:**

```
    {
      "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
      "__metadata" : {
        "resultType" : "raw",
        "total" : 1455,
        "top" : 150,
        "StartTime" : "2015-02-11T21:09:07.0345815Z",
        "Status" : "Successful",
        "LastUpdated" : "2015-02-11T21:09:07.331463Z",
        "CoreResponses" : [],
        "sort" : [{
          "name" : "TimeGenerated",
          "order" : "desc"
        }],
        "requestTime" : 450
      },
      "value": [{
        "SourceSystem" : "OpsManager",
        "TimeGenerated" : "2015-02-07T14:07:33Z",
        "Source" : "SideBySide",
        "EventLog" : "Application",
        "Computer" : "BAMBAM",
        "EventCategory" : 0,
        "EventLevel" : 1,
        "EventLevelName" : "Error",
        "UserName" : "N/A",
        "EventID" : 78,
        "MG" : "00000000-0000-0000-0000-000000000001",
        "TimeCollected" : "2015-02-07T14:10:04.69Z",
        "ManagementGroupName" : "AOI-5bf9a37f-e841-462b-80d2-1d19cd97dc40",
        "id" : "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
        "Type" : "Event",
        "__metadata" : {
          "Type" : "Event",
          "TimeGenerated" : "2015-02-07T14:07:33Z",
          "highlighting" : {
          "EventLevelName" : ["{[hl]}Error{[/hl]}"]
        }
      }]
    ],
            "start" : "2015-02-04T21:03:29.231Z",
            "end" : "2015-02-11T21:03:29.231Z"
          }
        }
      }]
    }
```

### <a name="searchid---actionread"></a>Keresés / {azonosító} - művelet olvasása
**A kérelem egy mentett keresés hello tartalma:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/search/{SearchId}?api-version=2015-03-20
```

> [!NOTE]
> Ha hello keresési "Folyamatban" állapotú ad vissza, majd lekérdezési frissítése hello eredmények végezhető el ez az API. 6 perc után hello hello keresés eredménye így a rendszer eldobja hello gyorsítótárból, és HTTP állapotba adja vissza. Hello kezdeti keresési kérelem azonnal visszaadja a "Sikeres" állapotba, ha hello eredmények nem lettek hozzáadva a HTTP már meg API tooreturn okoz, ha a lekérdezés toohello gyorsítótár. hello egy 200-as HTTP-eredmény tartalma a hello hello kezdeti keresési kérelem csak a frissített értékekkel formátuma azonos.
>
>

### <a name="saved-searches"></a>Mentett keresések
**A kérelem mentett keresések listája:**

```
    armclient post /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/savedSearches?api-version=2015-03-20
```

Támogatott módszerek: GET PUT törlése

Gyűjtemény módszereket támogatja: beolvasása

a következő táblázat hello hello tulajdonságait ismerteti.

| Tulajdonság | Leírás |
| --- | --- |
| Azonosító |hello egyedi azonosítója. |
| ETag |**Szükséges javítás**. Minden egyes kiszolgáló frissítve. Érték egyenlő toohello aktuális tárolt értéknek kell lennie, vagy "*" tooupdate. 409 régi vagy érvénytelen értéket adott vissza. |
| Properties.Query |**Szükséges**. hello keresési lekérdezés. |
| properties.displayName |**Szükséges**. hello lekérdezés hello felhasználó által definiált megjelenítési neve. |
| Properties.Category |**Szükséges**. hello felhasználói kategória hello lekérdezés. |

> [!NOTE]
> hello Naplóelemzési keresési API-JÁNAK jelenleg ad vissza, felhasználó által létrehozott mentett keresések során kérdezi le azt a mentett kereséseket a munkaterületen. hello API nem ad vissza a mentett keresések megoldások által biztosított.
>
>

### <a name="create-saved-searches"></a>Mentett keresések létrehozása
**A kérelem:**

```
    $savedSearchParametersJson = "{'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisismyid?api-version=2015-03-20 $savedSearchParametersJson
```

> [!NOTE]
> minden mentett keresések, ütemezéseihez és hello napló Analytics API létre műveletek hello nevét kisbetűs kell lennie.

### <a name="delete-saved-searches"></a>Törölje a mentett kereséseket
**A kérelem:**

```
    armclient delete /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20
```

### <a name="update-saved-searches"></a>Mentett keresések frissítése
 **A kérelem:**

```
    $savedSearchParametersJson = "{'etag': 'W/`"datetime\'2015-04-16T23%3A35%3A35.3182423Z\'`"', 'properties': { 'Category': 'myCategory', 'DisplayName':'myDisplayName', 'Query':'* | measure Count() by Source', 'Version':'1'  }"
    armclient put /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/savedSearches/thisIsMyId?api-version=2015-03-20 $savedSearchParametersJson
```

### <a name="metadata---json-only"></a>Metaadat - JSON csak
Íme egy módon toosee hello mezők a munkaterület hello adatgyűjtés az összes napló típusához. Például ha szeretné tudni, hogy ha hello eseménytípus van-e a számítógép neve mező, akkor ez a lekérdezés értéke egyirányú toocheck.

**Kérelem mezők:**

```
    armclient get /subscriptions/{SubId}/resourceGroups/{ResourceGroupId}/providers/Microsoft.OperationalInsights/workspaces/{WorkspaceName}/schema?api-version=2015-03-20
```

**Válasz:**

```
    {
      "__metadata" : {
        "schema" : {
          "name" : "Example Name",
          "version" : 2
        },
        "resultType" : "schema",
        "requestTime" : 35
      },
      "value" : [{
          "name" : "MG",
          "displayName" : "MG",
          "type" : "Guid",
          "facetable" : true,
          "display" : false,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Capacity_SMBUtilizationByHost", "Capacity_ArrayUtilization", "Capacity_SMBShareUtilization", "SQLAssessmentRecommendation", "Event", "ConfigurationChange", "ConfigurationAlert", "ADAssessmentRecommendation", "ConfigurationObject", "ConfigurationObjectProperty"]
        }, {
          "name" : "ManagementGroupName",
          "displayName" : "ManagementGroupName",
          "type" : "String",
          "facetable" : true,
          "display" : true,
          "ownerType" : ["PerfHourly", "ProtectionStatus", "Event", "ConfigurationChange", "ConfigurationAlert", "W3CIISLog", "AlertHistory", "Recommendation", "Alert", "SecurityEvent", "UpdateAgent", "RequiredUpdate", "ConfigurationObject", "ConfigurationObjectProperty"]
        }
      ]
    }
```

a következő táblázat hello hello tulajdonságait ismerteti.

| **Tulajdonság** | **Leírás** |
| --- | --- |
| név |Mező neve. |
| displayName |hello hello mező nevét jeleníti meg. |
| type |hello hello mező értékének típusa. |
| kategorizálható |"Indexed", aktuális kombinációja "tárolt" és "dimenzió" tulajdonságait. |
| Megjelenítése |Aktuális "üzenet megjelenítése" tulajdonság. Értéke TRUE, ha mező a keresési látható. |
| ownerType |Csökkentett tooonly típusok tooonboarded IP-címekhez tartozó. |

## <a name="optional-parameters"></a>Nem kötelező paraméterek
a következő információk hello rendelkezésre álló választható paramétereket ismerteti.

### <a name="highlighting"></a>Kiemelve
hello "Highlight" paraméter egy nem kötelező paraméter, használhatja a toorequest hello keresési alrendszer tartalmaznak jelölők válaszában.

Ezek a jelölők hello start jelzik, és a záró kijelölt szöveg, amely megfelel a keresési lekérdezés megadott hello feltételeket.
Előfordulhat, hogy adja meg a hello kezdő és záró jelölők toowrap kiemelt hello keresőkifejezéssel által használt.

**Példa keresési lekérdezés**

```
    $savedSearchParametersJson =
    {
      "top":150,
      "highlight":{
        "pre":"{[hl]}",
        "post":"{[/hl]}"
      },
      "query":"*",
      "start":"2015-02-04T21:03:29.231Z",
      "end":"2015-02-11T21:03:29.231Z"
    }
    armclient post /subscriptions/{Subscription ID}/resourceGroups/OI-Default-East-US/providers/Microsoft.OperationalInsights/workspaces/{Workspace ID}/search?api-version=2015-03-20 $searchParametersJson
```

**A minta eredmény:**

```
    {
        "TimeGenerated":
        "2015-05-18T23:55:59Z", "Source":
        "EventLog": "Application",
        "Computer": "smokedturkey.net",
        "EventCategory": 0,
        "EventLevel":1,
        "EventLevelName":
        "Error"
        "Manager ", "__metadata":
        {
            "Type": "Event",
            "TimeGenerated": "2015-05-18T23:55:59Z",
            "highlighting": {
                "EventLevelName":
                ["{[hl]}Error{[/hl]}"]
            }
        }
    }
```

Figyelje meg, hogy az előző eredmény hello olyan hibaüzenetet, amely a következő előtaggal, és hozzáfűzi tartalmazza.

## <a name="computer-groups"></a>Számítógépcsoportok
Számítógépcsoportok különleges mentett keresések, amely a számítógépek készlettel tért vissza.  Más lekérdezések toolimit hello eredmények toohello csoportban lévő számítógépek hello is használhatja a számítógép (csoport).  A számítógép (csoport), a mentett kereséseket értékkel rendelkező számítógép csoport címkével ellátott lett megvalósítva.

Az alábbiakban látható egy számítógépcsoport mintát választ.

```
    "etag": "W/\"datetime'2016-04-01T13%3A38%3A04.7763203Z'\"",
    "properties": {
        "Category": "My Computer Groups",
        "DisplayName": "My Computer Group",
        "Query": "srv* | Distinct Computer",
        "Tags": [{
            "Name": "Group",
            "Value": "Computer"
          }],
    "Version": 1
    }
```

### <a name="retrieving-computer-groups"></a>Számítógépcsoportok beolvasása
tooretrieve egy számítógépcsoport használata hello Get metódus hello csoporttal azonosítóját.

```
armclient get /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/{Group ID}`?api-version=2015-03-20
```

### <a name="creating-or-updating-a-computer-group"></a>Létrehozásakor vagy frissítésekor. a számítógép (csoport)
toocreate egy számítógépcsoport hello Put metódust használja a mentett keresés egyedi azonosítót. Használjon egy meglévő számítógép-csoport azonosítója, ha egy módosul. Ha hello Log Analytics-portálról egy számítógép (csoport) hoz létre, majd hello azonosító hello csoportból, neve jön létre.

hello lekérdezés hello csoport definíciójának megfelelően a hello csoport toofunction számítógépcsoportot kell visszaadnia.  Javasoljuk, hogy a lekérdezés leállítása `| Distinct Computer` tooensure hello megfelelő adat.

mentett keresés hello hello meghatározása tartalmaznia kell egy csoport nevű számítógép érték hello keresési toobe fontosként megjelölt számítógépcsoport címkét.

```
    $etag=Get-Date -Format yyyy-MM-ddThh:mm:ss.msZ
    $groupName="My Computer Group"
    $groupQuery = "Computer=srv* | Distinct Computer"
    $groupCategory="My Computer Groups"
    $groupID = "My Computer Groups | My Computer Group"

    $groupJson = "{'etag': 'W/`"datetime\'" + $etag + "\'`"', 'properties': { 'Category': '" + $groupCategory + "', 'DisplayName':'"  + $groupName + "', 'Query':'" + $groupQuery + "', 'Tags': [{'Name': 'Group', 'Value': 'Computer'}], 'Version':'1'  }"

    armclient put /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20 $groupJson
```

### <a name="deleting-computer-groups"></a>Számítógép-csoportok törlése
toodelete egy számítógépcsoport használata hello Delete metódust hello csoporttal azonosítóját.

```
armclient delete /subscriptions/{Subscription ID}/resourceGroups/{Resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Workspace Name}/savedSearches/$groupId`?api-version=2015-03-20
```


## <a name="next-steps"></a>Következő lépések
* További tudnivalók [keresések jelentkezzen](log-analytics-log-searches.md) toobuild lekérdezések egyéni mezők feltétel használatával.
