---
title: "Gyors üzembe helyezési: az Azure Machine Learning javaslatok-API (ver. 1.) |} Microsoft Docs"
description: "Az Azure gépi tanulási ajánlásokkal – gyors üzembe helyezési útmutató"
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 5bce1a4a-1ad6-473f-812b-84f800fdc09a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: luisca
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: True
ms.openlocfilehash: d8f98e85f723a1104cb169b26d797934140979f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="quick-start-guide-for-hello-machine-learning-recommendations-api-version-1"></a>Rövid útmutató az hello Machine Learning javaslatok API (verzió: 1)

> [!NOTE]
> Hello webalkalmazásokba [javaslatok API kognitív szolgáltatás](https://www.microsoft.com/cognitive-services/recommendations-api) helyett jelen verziójában. hello javaslatok kognitív szolgáltatás adatokéval ezt a szolgáltatást, és minden hello új szolgáltatások nincs fejlesztik ki. Rendelkezik új szolgáltatásokat, például a kötegelés támogatása, a megfelelőbb API Explorer, a tisztító API felület, egységesebb előfizetési/számlázási élményt, stb.
>
> További információ [áttelepítése toohello új kognitív szolgáltatás](http://aka.ms/recomigrate).
> 
> 

Ez a dokumentum ismerteti, hogyan tooonboard a szolgáltatás vagy alkalmazás toouse Microsoft Azure Machine Learning javaslatokat. További részletekért található hello javaslatok API a hello [Cortana Intelligence Gallery](https://gallery.cortanaintelligence.com/MachineLearningAPIs/Recommendations-2).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="general-overview"></a>Általános – áttekintés
toouse Azure Machine Learning ajánlásokat, kell tootake hello a következő lépéseket:

* Hozzon létre egy modell - modell egy olyan tároló, a használati adatok, a katalógus adatait és a hello javaslat modell.
* Katalógus adatokat importálhat - katalógus hello cikkek metaadatai információkat tartalmaz. 
* Használati adatok importálása - használati adatok is feltölthetők a két módszer egyikével (vagy mindkettőt):
  * Hello használati adatokat tartalmazó fájl feltöltésével.
  * Küldött adatok megszerzésére események.
    Általában a rendelés toobe képes toocreate egy kezdeti javaslat modell (a rendszerindítási) használati-fájl feltöltése, és akkor használható, amíg hello rendszer elegendő adatot gyűjt hello adatok megszerzése formátum használatával.
* A javaslat modell létrehozása – Ez egy aszinkron művelet, mely hello javaslat rendszer összes hello használati adatokat fogad, és létrehoz egy javaslat modell. Ez a művelet több percet is igénybe vehet, vagy hello függően több órába hello adatok és hello méretének összeállíthatja konfigurációs paramétereket. Hello build váltanak, amikor elérhetővé válik egy build. Felhasználhatja azt toocheck hello felépítési folyamat véget ért tooconsume javaslatok megkezdése előtt.
* Használja a javaslatok - Get ajánlások egy adott elem vagy elemek listáját.

A fenti hello lépéseket hello Azure Machine Learning javaslatok API keresztül zajlik.  Letöltheti a mintaalkalmazás, amely megvalósítja az egyes lépéseket a hello [gyűjtemény is.](http://1drv.ms/1xeO2F3)

## <a name="limitations"></a>Korlátozások
* előfizetésenként modellek hello maximális száma érték a 10.
* a katalógus rendelkező elemek maximális számát hello 100 000.
* mindig használati pontok maximális számát hello ~ 5,000,000. Ha újakat feltöltött vagy fogja jelenteni a legrégebbi hello törlődni fog.
* hello maximális a FELADÁS egy vagy több (például katalógus adatokat importálhat, használati adatok importálása) elküldött adatok mérete 200MB.
* hello másodpercenkénti javaslat modell build nem aktív tranzakciók száma van ~ 2TPS. Aktív javaslat modell build mentése too20TPS tárolására képes.

## <a name="integration"></a>Integráció
### <a name="authentication"></a>Authentication
A Microsoft Azure piactérről vagy hello Basic vagy az OAuth hitelesítési módszer támogatja. Könnyedén megtalálhatja a hello kulcsait hello piactér toohello kulcsokat navigálva [fiókbeállításokban](https://datamarket.azure.com/account/keys). 

#### <a name="basic-authentication"></a>Az egyszerű hitelesítés
Hello engedélyezési fejléc hozzáadása:

    Authorization: Basic <creds>

    Where <creds> = ConvertToBase64("AccountKey:" + yourAccountKey);  

Átalakítás tooBase64 (C#)

    var bytes = Encoding.UTF8.GetBytes("AccountKey:" + yourAccountKey);
    var creds = Convert.ToBase64String(bytes);

Átalakítás tooBase64 (JavaScript)

    var creds = window.btoa("AccountKey" + ":" + yourAccountKey);




### <a name="service-uri"></a>Szolgáltatás URI-azonosítója
hello szolgáltatás gyökér URI hello Azure Machine Learning javaslatok API-k [itt.](https://api.datamarket.azure.com/amla/recommendations/v2/)

hello teljes szolgáltatás URI kifejezett elemek hello OData specifikáció használatával.

### <a name="api-version"></a>API-verzió
Minden API-hívás fog működni, hello végén apiVersion be kell állítania egy lekérdezési paraméter túl "1.0".

### <a name="ids-are-case-sensitive"></a>Azonosítók-és nagybetűk
Azonosítók, bármelyik hello API-k, visszaadott kis-és nagybetűket, és kell használni, így amikor későbbi API-hívásokban paraméterként. Például modellazonosítóját és a katalógus azonosítók nagybetűk között.

### <a name="create-a-model"></a>Modell létrehozása
Egy "létrehozása a modell" kérést hoz létre:

| HTTP-metódus | URI |
|:--- |:--- |
| POST |`<rootURI>/CreateModel?modelName=%27<model_name>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/CreateModel?modelName=%27MyFirstModel%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelName |Csak betűk (A-Z, a – z), szám (0-9), kötőjeleket (-) és aláhúzásjelet (_) megengedettek.<br>Maximális hossz: 20 |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

* `feed/entry/content/properties/id`-Hello modell eltérő azonosítót tartalmaz.
  Ne feledje, hogy hello Modellazonosító kis-és nagybetűket.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">a658c626-2baa-43a7-ac98-f6ee26120a12</d:Id>
        <d:Name m:type="Edm.String">MyFirstModel</d:Name>
        <d:Date m:type="Edm.String">10/5/2014 6:35:19 AM</d:Date>
        <d:Status m:type="Edm.String">Created</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:UserName>
        <d:Description m:type="Edm.String"></d:Description>
      </m:properties>
    </content>
      </entry>
    </feed>


### <a name="import-catalog-data"></a>Eseménykatalógus-adatok importálása
Ha több katalógus fájlok toohello ugyanaz a modell több hívások tölt fel, azt csak hello új katalóguselemek szúrja be. A meglévő elemek hello eredeti értékekkel marad.

| HTTP-metódus | URI |
|:--- |:--- |
| POST |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |Hello modell (kis-és nagybetűket) egyedi azonosítója |
| fájlnév |Hello katalógus szöveges azonosítója.<br>Csak betűk (A-Z, a – z), szám (0-9), kötőjeleket (-) és aláhúzásjelet (_) megengedettek.<br>Maximális hossz: 50 |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |Katalógus-adatokat. Formátum:<br>`<Item Id>,<Item Name>,<Item Category>[,<description>]`<br><br><table><tr><th>Név</th><th>Kötelező</th><th>Típus</th><th>Leírás</th></tr><tr><td>Cikk azonosítója</td><td>Igen</td><td>Alfanumerikus, a maximális hossz 50</td><td>Egy elem egyedi azonosítója</td></tr><tr><td>Elem neve</td><td>Igen</td><td>Alfanumerikus, a maximális hossz 255</td><td>Elem neve</td></tr><tr><td>Elem kategória</td><td>Igen</td><td>Alfanumerikus, a maximális hossz 255</td><td>Kategória toowhich ez elem tartozik (például főzés könyvek tragédiát...)</td></tr><tr><td>Leírás</td><td>Nem</td><td>Alfanumerikus, maximális hosszúság 4000</td><td>Ez az elem leírása</td></tr></table><br>Maximális fájlmérete 200MB.<br><br>Példa:<br><code>2406e770-769c-4189-89de-1c9283f93a96,Clara Callan,Book<br>21bf8088-b6c0-4509-870c-e1c7ac78304a,hello Forgetting Room: A Fiction (Byzantium Book),Book<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23,Spadework,Book<br>552a1940-21e4-4399-82bb-594b46d7ed54,Restraint of Beasts,Book</code> |

**Válasz**:

HTTP-állapotkód: 200

* `Feed\entry\content\properties\LineCount`-Elfogadott sorok száma.
* `Feed\entry\content\properties\ErrorCount`-Tooan hiba miatt nem beillesztett sorok száma.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
          <subtitle type="text">Import catalog file</subtitle>
          <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:58:04Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">ImportCatalogFileEntity2</title>
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">4</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
      </m:properties>
    </content>
      </entry>
    </feed>


### <a name="import-usage-data"></a>Használati adatok importálása
#### <a name="uploading-a-file"></a>A fájl feltöltése
Ez a szakasz bemutatja, hogyan tooupload használati adatok egy fájl segítségével. A használati adatokat tartalmazó többször API hívása. Minden használati adatokat a rendszer minden hívások menti.

| HTTP-metódus | URI |
|:--- |:--- |
| POST |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |Hello modell (kis-és nagybetűket) egyedi azonosítója |
| fájlnév |Hello katalógus szöveges azonosítója.<br>Csak betűk (A-Z, a – z), szám (0-9), kötőjeleket (-) és aláhúzásjelet (_) megengedettek.<br>Maximális hossz: 50 |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |Használati adatok. Formátum:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Név</th><th>Kötelező</th><th>Típus</th><th>Leírás</th></tr><tr><td>Felhasználói azonosító</td><td>Igen</td><td>Alfanumerikus</td><td>A felhasználó egyedi azonosítója</td></tr><tr><td>Cikk azonosítója</td><td>Igen</td><td>Alfanumerikus, a maximális hossz 50</td><td>Egy elem egyedi azonosítója</td></tr><tr><td>Time</td><td>Nem</td><td>Dátum formátumban: éééé/hh/nnTóó: pp: (06 például 2013/20T10:00:00)</td><td>Az adatok ideje</td></tr><tr><td>Esemény</td><td>Nem, ha a megadott, akkor is kell helyezni a dátum</td><td>Hello a következők egyike:<br>• Kattintson<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Beszerzési</td><td></td></tr></table><br>Maximális fájlmérete 200MB.<br><br>Példa:<br><pre>149452,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951,1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Válasz**:

HTTP-állapotkód: 200

* `Feed\entry\content\properties\LineCount`-Elfogadott sorok száma.
* `Feed\entry\content\properties\ErrorCount`-Tooan hiba miatt nem beillesztett sorok száma.
* `Feed\entry\content\properties\FileId`-Fájl azonosítója.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Add bulk usage data (usage file)</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T07:21:44Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
      </entry>
    </feed>


#### <a name="using-data-acquisition"></a>Adatgyűjtést használatával
Ez a szakasz bemutatja, hogyan toosend események valós időben tooAzure Machine Learning ajánlásokat, általában a webhelyről.

| HTTP-metódus | URI |
|:--- |:--- |
| POST |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| apiVersion |1.0 |
|  | |
| Kérés törzsében |Esemény adatbevitel minden esemény toosend keresi. Küldje el a hello ugyanazon felhasználó vagy a böngésző-munkamenet hello hello munkamenet-azonosító mezőben ugyanazzal az Azonosítóval. (Lásd az alábbi esemény törzsében mintáját.) |

* Példa a "Kattintson" esemény:
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
        <Name>Click</Name>
        <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
        </EventData>
        </Event>
* Példa a "RecommendationClick" esemény:
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>RecommendationClick</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* Példa a "AddShopCart" esemény:
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>AddShopCart</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* Példa a "RemoveShopCart" esemény:
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>RemoveShopCart</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
        </EventData>
          </EventData>
        </Event>
* Példa a "Vásárlás" esemény:
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
        <SessionId>11112222</SessionId>
        <EventData>
        <EventData>
            <Name>Purchase</Name> 
            <PurchaseItems>
            <PurchaseItems>
                <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
                <Count>3</Count>
            </PurchaseItems>
        </PurchaseItems>
        </EventData>
        </EventData>
        </Event>
* Példa 2 események "kattintson a" és "AddShopCart" küldésekor:
  
        <Event xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
          <ModelId>2779c063-48fb-46c1-bae3-74acddc8c1d1</ModelId>
          <SessionId>11112222</SessionId>
          <EventData>
        <EventData>
          <Name>Click</Name>
          <ItemId>21BF8088-B6C0-4509-870C-E1C7AC78304A</ItemId>
          <ItemName>itemName</ItemName>
          <ItemDescription>item description</ItemDescription>
          <ItemCategory>category</ItemCategory>
        </EventData>
        <EventData>
          <Name>AddShopCart</Name>
          <ItemId>552A1940-21E4-4399-82BB-594B46D7ED54</ItemId>
        </EventData>
          </EventData>
        </Event>

**Válasz**: HTTP-állapotkód: 200

### <a name="build-a-recommendation-model"></a>A javaslat modell létrehozása
| HTTP-metódus | URI |
|:--- |:--- |
| POST |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |Hello modell (kis-és nagybetűket) egyedi azonosítója |
| userDescription |Hello katalógus szöveges azonosítója. Vegye figyelembe, hogy ha a tárolóhelyek használata akkor kell kódolása % 20 helyette. Tekintse meg a fenti példa.<br>Maximális hossz: 50 |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

Ez az egy aszinkron API-t. Válaszként elérhetővé válik egy build azonosítója. Amikor véget ért a hello build tooknow, hello "Beolvasása buildek állapota, a modell" API hívása, és keresse meg a build azonosító hello válaszként. Ne feledje, hogy a build telhet-e a perc toohours hello hello adatok méretétől függően.

Javaslatok keretein belül hello build vége nem lehet felhasználni.

Érvényes létrehozásának állapota:

* Hozzon létre – modell lett létrehozva.
* Aszinkron – modell build lett elindítva, és azt a várólistára.
* Épület – modell éppen készül.
* Sikeres – a létrehozási művelet sikeresen befejeződött.
* Hiba – Build hibával ért véget.
* Visszavonva – Build megszakították.
* Visszavonása – Build megszakítása folyamatban van.

Vegye figyelembe, hogy hello build azonosító hello a következő elérési út alatt található:`Feed\entry\content\properties\Id`

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Build a Model with RequestBody</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T08:56:34Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">1000272</d:Id>
        <d:UserName m:type="Edm.String"></d:UserName>
        <d:ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:ModelId>
        <d:ModelName m:type="Edm.String">docTest</d:ModelName>
        <d:Type m:type="Edm.String">Recommendation</d:Type>
        <d:CreationTime m:type="Edm.String">2014-10-05T08:56:31.893</d:CreationTime>
        <d:Progress_BuildId m:type="Edm.String">1000272</d:Progress_BuildId>
        <d:Progress_ModelId m:type="Edm.String">9559872f-7a53-4076-a3c7-19d9385c1265</d:Progress_ModelId>
        <d:Progress_UserName m:type="Edm.String">5-4058-ab36-1fe254f05102@dm.com</d:Progress_UserName>
        <d:Progress_IsExecutionStarted m:type="Edm.String">false</d:Progress_IsExecutionStarted>
        <d:Progress_IsExecutionEnded m:type="Edm.String">false</d:Progress_IsExecutionEnded>
        <d:Progress_Percent m:type="Edm.String">0</d:Progress_Percent>
        <d:Progress_StartTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_StartTime>
        <d:Progress_EndTime m:type="Edm.String">0001-01-01T00:00:00</d:Progress_EndTime>
        <d:Progress_UpdateDateUTC m:type="Edm.String"></d:Progress_UpdateDateUTC>
        <d:Status m:type="Edm.String">Queued</d:Status>
        <d:Key1 m:type="Edm.String">UseFeaturesInModel</d:Key1>
        <d:Value1 m:type="Edm.String">False</d:Value1>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="get-build-status-of-a-model"></a>A modell létrehozási állapotának beolvasása
| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |Hello modell (kis-és nagybetűket) egyedi azonosítója |
| onlyLastBuild |Azt jelzi, hogy összes hello tooreturn build hello modell előzmények vagy hello legutóbbi build csak hello állapotát. |
| apiVersion |1.0 |

**Válasz**:

HTTP-állapotkód: 200

hello válasz tartalmazza a build egy tételt. Mindegyik bejegyzés rendelkezik hello a következő adatokat:

* `feed/entry/content/properties/UserName`– Hello felhasználó neve.
* `feed/entry/content/properties/ModelName`– Hello modell neve.
* `feed/entry/content/properties/ModelId`– Minta egyedi azonosítója.
* `feed/entry/content/properties/IsDeployed`– E hello buildverziót központilag telepítik (más néven aktív build).
* `feed/entry/content/properties/BuildId`– Az egyedi azonosító létrehozása.
* `feed/entry/content/properties/BuildType`-Hello build típusú.
* `feed/entry/content/properties/Status`– Létrehozási állapot. Hello a következők egyike lehet: Hiba történt, épület, várakozik, Canceling, visszavonva, sikeres
* `feed/entry/content/properties/StatusMessage`– Részletes állapotüzenet (csak toospecific állapotok vonatkozik).
* `feed/entry/content/properties/Progress`– Összeállítása folyamatban (%).
* `feed/entry/content/properties/StartTime`– A kezdő időpont felépítéséhez.
* `feed/entry/content/properties/EndTime`– Build befejezési időpontja.
* `feed/entry/content/properties/ExecutionTime`– Build időtartama.
* `feed/entry/content/properties/ProgressStep`– Hello jelenlegi fázisa, amely a folyamatban lévő build adatait.

Érvényes létrehozásának állapota:

* Létrehozott – Build kérelem tétel jött létre.
* Aszinkron – kérelmet lett elindítva, és azt a várólistára.
* Épület – összeállítása folyamatban van.
* Sikeres – a létrehozási művelet sikeresen befejeződött.
* Hiba – Build hibával ért véget.
* Visszavonva – Build megszakították.
* Visszavonása – Build megszakítása folyamatban van.

Build típusú engedélyezett érvényes értékek:

* Sorszám - dimenziószáma felépítéséhez. (Dimenziószáma összeállítása a részleteket, olvassa el "Machine Learning javaslat API dokumentációjában" dokumentum toohello.)
* A javaslat - javaslat build.
* FBT - együtt build gyakran vásárolt.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get builds status of a model</subtitle>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetBuildsStatusEntity</title>
    <updated>2014-11-05T17:51:10Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:UserName m:type="Edm.String">b-434e-b2c9-84935664ff20@dm.com</d:UserName>
        <d:ModelName m:type="Edm.String">ModelName</d:ModelName>
        <d:ModelId m:type="Edm.String">1d20c34f-dca1-4eac-8e5d-f299e4e4ad66</d:ModelId>
        <d:IsDeployed m:type="Edm.String">true</d:IsDeployed>
        <d:BuildId m:type="Edm.String">1000272</d:BuildId>
        <d:BuildType m:type="Edm.String">Recommendation</d:BuildType>
        <d:Status m:type="Edm.String">Success</d:Status>
        <d:StatusMessage m:type="Edm.String"></d:StatusMessage>
        <d:Progress m:type="Edm.String">0</d:Progress>
        <d:StartTime m:type="Edm.String">2014-11-02T13:43:51</d:StartTime>
        <d:EndTime m:type="Edm.String">2014-11-02T13:45:10</d:EndTime>
        <d:ExecutionTime m:type="Edm.String">00:01:19</d:ExecutionTime>
        <d:IsExecutionStarted m:type="Edm.String">false</d:IsExecutionStarted>
        <d:ProgressStep m:type="Edm.String"></d:ProgressStep>
      </m:properties>
     </content>
    </entry>
    </feed>


### <a name="get-recommendations"></a>Javaslatok beszerzése
| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |Hello modell (kis-és nagybetűket) egyedi azonosítója |
| hogy az elemazonosítók |Hello elemek toorecommend a vesszővel tagolt listája.<br>Maximális hossz: 1024 |
| numberOfResults |Szükséges eredmények száma |
| includeMetatadata |Jövőbeli használatra, mindig hamis |
| apiVersion |1.0 |

**Válasz:**

HTTP-állapotkód: 200

hello válasz egy tételt ajánlott cikk tartalmazza. Mindegyik bejegyzés rendelkezik hello a következő adatokat:

* `Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.
* `Feed\entry\content\properties\Name`-Hello elem nevét.
* `Feed\entry\content\properties\Rating`-Minősítés hello ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.
* `Feed\entry\content\properties\Reasoning`-Javaslat indoklást (például a javaslat magyarázatokat).

Az OData-XML

az alábbi hello példa egy válasz 10 ajánlott elemeket tartalmazza:

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get Recommendation</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T12:28:48Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">159</d:Id>
        <d:Name m:type="Edm.String">159</d:Name>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '159'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">52</d:Id>
        <d:Name m:type="Edm.String">52</d:Name>
        <d:Rating m:type="Edm.Double">0.539588900748721</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '52'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">35</d:Id>
        <d:Name m:type="Edm.String">35</d:Name>
        <d:Rating m:type="Edm.Double">0.53842946443853</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '35'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">124</d:Id>
        <d:Name m:type="Edm.String">124</d:Name>
        <d:Rating m:type="Edm.Double">0.536712832792886</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '124'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">120</d:Id>
        <d:Name m:type="Edm.String">120</d:Name>
        <d:Rating m:type="Edm.Double">0.533673023762878</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '120'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">96</d:Id>
        <d:Name m:type="Edm.String">96</d:Name>
        <d:Rating m:type="Edm.Double">0.532544826370521</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '96'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">69</d:Id>
        <d:Name m:type="Edm.String">69</d:Name>
        <d:Rating m:type="Edm.Double">0.531678607847759</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '69'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">172</d:Id>
        <d:Name m:type="Edm.String">172</d:Name>
        <d:Rating m:type="Edm.Double">0.530957821375951</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '172'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">155</d:Id>
        <d:Name m:type="Edm.String">155</d:Name>
        <d:Rating m:type="Edm.Double">0.529093541481333</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '155'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">32</d:Id>
        <d:Name m:type="Edm.String">32</d:Name>
        <d:Rating m:type="Edm.Double">0.528917978168322</d:Rating>
        <d:Reasoning m:type="Edm.String">People who like '1003' also like '32'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="update-model"></a>Frissítési modell
Frissítés hello modell leírása, vagy hello aktív build azonosítóját.
*Aktív build azonosító* -minden build minden modell van állítva egy összeállítási. hello aktív build azonosító: hello első sikeres build minden új modell. Amennyiben van egy aktív build ID azonosítója és további változata hello teszi ugyanannak a modellnek van szüksége tooexplicitly állítsa be úgy, mint hello alapértelmezett build azonosító Ha szeretné. Ha felhasznált ajánlásokat, ha nem adja meg a hello build azonosítója, amelyet toouse, hello alapértelmezett egy automatikusan fog történni.

Ez az eljárás lehetővé teszi - Miután javaslat modell éles - toobuild új modellek és tesztelését ahhoz, azok előlépteti tooproduction.

| HTTP-metódus | URI |
|:--- |:--- |
| A PUT |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| id |Hello modell (kis-és nagybetűket) egyedi azonosítója |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |`<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`   <Description>New Description</Description>`<br>`          <ActiveBuildId>-1</ActiveBuildId>`<br>`</ModelUpdateParams>`<br><br>Ne feledje, hogy hello XML-címkék leírás ActiveBuildId opcionálisak. Ha nem szeretné, hogy tooset leírás vagy ActiveBuildId, hello teljes címke eltávolítása. |

**Válasz**:

HTTP-állapotkód: 200

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Update an Existing Model</subtitle>
      <id>https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T10:27:17Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/Data.ashx/amla/recommendations/v2/UpdateModel?id='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;apiVersion='1.0'" />
    </feed>

## <a name="legal"></a>Jogi tudnivalók
Ez a dokumentum biztosított ",-van". Információk és nézetek ebben a dokumentumban, beleértve az URL-CÍMEK és más internetes webhelyet, értesítés nélkül változhatnak. A felhasznált példák némelyike csak illusztrációs célokat szolgálnak, és kitalált esetet szemléltet. Nincs valós association vagy a kapcsolat célja, vagy eseményekkel. Ez a dokumentum nem biztosít semmilyen jogot tooany semmilyen Microsoft-termékben található szellemi tulajdonhoz. Előfordulhat, hogy másolja és használja a dokumentum belső használatra, tájékoztatási céllal. © 2014 Microsoft. Minden jog fenntartva. 

