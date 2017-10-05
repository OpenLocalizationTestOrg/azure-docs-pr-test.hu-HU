---
title: "Számítógép tanulási javaslatok API-dokumentáció |} Microsoft Docs"
description: "Egy a Microsoft Azure piactéren elérhető javaslatok motor Azure Machine Learning javaslatok API dokumentációjában."
services: machine-learning
documentationcenter: 
author: LuisCabrer
manager: jhubbard
editor: cgronlun
ms.assetid: 32c3ab2f-fdd7-48cc-b501-ad55c79b87dc
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/31/2017
ms.author: LuisCa
ROBOTS: NOINDEX
redirect_url: machine-learning-datamarket-deprecation
redirect_document_id: TRUE
ms.openlocfilehash: 1fba64d78d779344e2895b0d54419186b7584865
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-machine-learning-recommendations-api-documentation"></a>Azure Machine Learning Recommendations – API-dokumentáció
> [!NOTE]
> El kell kezdenie a javaslatok API kognitív szolgáltatás helyett jelen verziójában. A javaslatok kognitív szolgáltatás adatokéval ezt a szolgáltatást, és az új szolgáltatások nincs fejlesztik ki. Rendelkezik új szolgáltatásokat, például a kötegelés támogatása, a megfelelőbb API Explorer, a tisztító API felület, egységesebb előfizetési/számlázási élményt, stb.
> További információ [áttelepítése az új kognitív szolgáltatáshoz](http://aka.ms/recomigrate)
> 
> 

Ez a dokumentum a Microsoft Azure Machine Learning javaslatok API-k ábrázol.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="1-general-overview"></a>1. Általános – áttekintés
Ez a dokumentum egy API-hivatkozás. Az "Azure Machine Learning javaslat – gyors üzembe" dokumentum kell kezdődnie.

Az Azure Machine Learning javaslatok API a következő logikai csoportokra osztható:

* <ins>Korlátozások</ins> -javaslatok API-korlátozásokkal.
* <ins>Általános információk</ins> -információkat a hitelesítés, szolgáltatás URI és versioning.
* <ins>Basic modell</ins> -API-k, amelyek lehetővé teszik a modellt az alapvető műveleteket (pl. létrehozása, frissítése és modell törlése).
* <ins>A modell speciális</ins> -API-k, amelyek lehetővé teszik a modellen adatok insights speciális.
* <ins>A modell az üzleti szabályok</ins> -API-k, amelyek lehetővé teszik, hogy a modell javaslat eredmények üzleti szabályok kezelése.
* <ins>Katalógus</ins> -API-k, amelyek lehetővé teszik a modell katalógus alapvető műveleteket. A katalógus metaadat-információkat a használati adatok elemek tartalmazza.
* <ins>A szolgáltatás</ins> -API-k, amelyek lehetővé teszik elemen insights feltölti a katalógus és hogyan lehet ezt az információt jobb javaslatok létrehozásához.
* <ins>Használati adatok</ins> -API-k, amelyek lehetővé teszik a modell használati adatok alapvető műveleteket. Használati adatok alapvető formájában pár &#60; userId &#62; tartalmazó sorok áll, &#60; itemId &#62;.
* <ins>Build</ins> -API-k, amelyek lehetővé teszik, hogy a modell build indíthat, és a build kapcsolatos alapvető műveleteket. Ha elvégezte a értékes használati adatok modell build indíthat el.
* <ins>A javaslat</ins> -API-k, amelyek lehetővé teszik, hogy javaslatokat felhasználását, a build-modell leteltével.
* <ins>Felhasználói adatok</ins> -API-k, amelyek lehetővé teszik az adatok beolvasása a felhasználó használati adatai.
* <ins>Értesítések</ins> -API-k, amelyek lehetővé teszik a problémák értesítést szeretne kapni a API műveleteivel kapcsolatos. (Például a használati adatok adatgyűjtést és az események feldolgozása sikertelen a legtöbb jelentik. Egy hiba értesítést generál.)

## <a name="2-limitations"></a>2. Korlátozások
* Előfizetésenként modellek maximális száma érték a 10.
* Egyes modellek buildek legfeljebb 20.
* A katalógus rendelkező elemek maximális száma: 100 000.
* Mindig használati pontok maximális számát ~ 5,000,000. A legrégebbi esetén törlendő újakat feltöltött vagy fogja jelenteni.
* A FELADÁS egy vagy több (pl. katalógus adatokat importálhat, használati adatok importálása) elküldött adatok maximális mérete 200MB.
* A maximális elemek száma, amelyek is meg kell adnia a javaslatok beolvasásakor 150.

## <a name="3-apis---general-information"></a>3. API-k – általános információk
### <a name="31-authentication"></a>3.1. Authentication
Kérjük, kövesse a Microsoft Azure piactérről irányelveket hitelesítési kapcsolatban. A piactér a Basic vagy az OAuth hitelesítési módszer támogatja.

### <a name="32-service-uri"></a>3.2. Szolgáltatás URI-azonosítója
A szolgáltatás legfelső szintű URI-azonosítóját az Azure Machine Learning javaslatok API-k [itt.](https://api.datamarket.azure.com/amla/recommendations/v3/)

A teljes szolgáltatás URI kifejezett elemek az OData specifikáció használatával.  

### <a name="33-api-version"></a>3.3. API-verzió
Minden API-hívás végén, lesz egy lekérdezési paraméter apiVersion, amely 1.0 értékre kell állítani.

### <a name="34-ids-are-case-sensitive"></a>3.4. Azonosítók különbözőnek számítanak a kis
Azonosítók, az API-k által visszaadott különbözőnek számítanak a kis és kell használni, így amikor későbbi API-hívásokban paraméterként. Például modellazonosítóját és a katalógus azonosítók különbözőnek számítanak a kis.

## <a name="4-recommendations-quality-and-cold-items"></a>4. Javaslatok minőségének és Cold elemek
### <a name="41-recommendation-quality"></a>4.1. A javaslat minősége
A javaslat modellek létrehozásához általában van elegendő, a rendszer tudjon ajánlani. Ettől függetlenül javaslat minőségi függ a használatát, feldolgozása és a katalógus körét. Például ha sok cold elem (elem jelentős használata nélkül), a rendszer rendelkezik-e egy ilyen elem ajánlást megadása, vagy egy ilyen elem használata javasolt egy nehézségek. Ahhoz, hogy a cold elem probléma megoldásához, a rendszer engedélyezi a metaadatok javítása érdekében a javaslatok elemek használatát. A metaadatok szolgáltatások nevezzük. Tipikus szolgáltatásokat egy könyv szerző vagy egy movie szereplő. A kulcs/érték karakterláncok formájában katalógus keresztül szolgáltatást kínál. A teljes fájl formátuma, a katalógus, tekintse meg a [katalógus szakasz importálása](#81-import-catalog-data). 

### <a name="42-rank-build"></a>4.2. Rangsorolt build
Szolgáltatások javíthatja a javaslat modell, de ehhez lekérdezhetik a fontos funkciók használatát igényli. Erre a célra egy új build bevezetett - build a sorrend első helyén. A build fog rangsorolja szolgáltatások használhatóságát. Egy olyan jelentéssel bíró szolgáltatása, 2, illetve a hierarchiában felfelé a sorrendet megadó pontszám szolgáltatás.
Után az ismertetése, a szolgáltatások ezek jelentéssel bíró, jelentéssel bíró szolgáltatások listája (vagy allista) javaslat build kiváltani. E funkció használatát a fejlesztésen esett át a meleg elemek és a cold elemeket is. Meleg elemek használatához a `UseFeatureInModel` build paramétert kell beállítani. Ahhoz, hogy szolgáltatások cold elemek, a `AllowColdItemPlacement` engedélyezni kell a build paramétert.
Megjegyzés: Nincs lehetőség engedélyezése `AllowColdItemPlacement` engedélyezése nélkül `UseFeatureInModel`.

### <a name="43-recommendation-reasoning"></a>4.3. A javaslat indoklást
A javaslat indoklást funkciók használata szerepet játszó másik tényező. Ezenkívül az Azure Machine Learning javaslatok motor szolgáltatások segítségével javaslat magyarázattal (más néven mintafelismerési), a javaslat fogyasztó vezető további abban, hogy a javasolt elemben.
Ahhoz, hogy indoklást, a `AllowFeatureCorrelation` és `ReasoningFeatureList` paraméterek beállítása előtt kérése ajánlás build kell lennie.

## <a name="5-model-basic"></a>5. Basic modell
### <a name="51-create-model"></a>5.1. Modell létrehozása
Kérést hoz létre "létrehozása a modell".

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

* `feed/entry/content/properties/id`-Tartalmazza a modell.
  **Megjegyzés:**: Modellazonosító a kis-és nagybetűket.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Create A New Model</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T06:35:21Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">CreateANewModelEntity2</title>
    <updated>2014-10-05T06:35:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/CreateModel?modelName='MyFirstModel'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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

### <a name="52-get-model"></a>5.2. Modell lekérése
A "get-modell" kérést hoz.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/GetModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br>Példa:<br>`<rootURI>/GetModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| id |A modell (kis-és nagybetűket) egyedi azonosítója |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

A modell adatai található a következő elemeket:

* `feed/entry/content/properties/Id`-Modell egyedi azonosítója.
* `feed/entry/content/properties/Name`-Modell neve.
* `feed/entry/content/properties/Date`-Modell létrehozásának dátumát.
* `feed/entry/content/properties/Status`-Modell állapota. A következők egyikét:
  * Létre - modell jön létre, és nem tartalmaz Alkalmazáskatalógus és a használati.
  * ReadyForBuild - modell jön létre, és a katalógus és használati tartalmazza.
* `feed/entry/content/properties/HasActiveBuild`-Azt jelzi, ha a modell sikeresen lett létrehozva.
* `feed/entry/content/properties/BuildId`-Modell aktív build azonosítóját.
* `feed/entry/content/properties/Mpr`-Modell átlagos PERCENTILIS rangsorolási (MPR - ModelInsight további információt talál).
* `feed/entry/content/properties/UserName`-Modell belső felhasználó felhasználóneve.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get A List Of All Models</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T14:35:51Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
        <d:Name m:type="Edm.String">vah-11111</d:Name>
        <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
        <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
        <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
        <d:BuildId m:type="Edm.String">-1</d:BuildId>
        <d:Mpr m:type="Edm.String">0</d:Mpr>
        <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
        <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
        <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
        <d:Description m:type="Edm.String">short description</d:Description>
        <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="53----get-all-models"></a>5.3.    Összes modell lekérése
Lekéri az összes modellt az aktuális felhasználó.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/GetAllModels?apiVersion=%271.0%27`<br>Példa:<br>`<rootURI>/GetAllModels?apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

* `feed/entry/content/properties/Id`-Modell egyedi azonosítója.
* `feed/entry/content/properties/Name`-Modell neve.
* `feed/entry/content/properties/Date`-Modell létrehozásának dátumát.
* `feed/entry/content/properties/Status`-Modell állapota. A következők egyikét:
  * Létre - modell jön létre, és nem tartalmaz Alkalmazáskatalógus és a használati.
  * ReadyForBuild - modell jön létre, és a katalógus és használati tartalmazza.
* `feed/entry/content/properties/HasActiveBuild`-Azt jelzi, ha a modell sikeresen lett létrehozva.
* `feed/entry/content/properties/BuildId`-Modell aktív build azonosítóját.
* `feed/entry/content/properties/Mpr`-Modell MPR (lásd a ModelInsight olvashat).
* `feed/entry/content/properties/UserName`-Modell belső felhasználó felhasználóneve.
* `feed/entry/content/properties/UsageFileNames`-Modell fájlok vesszővel elválasztott listája.
* `feed/entry/content/properties/CatalogId`-Modell katalógus azonosítója.
* `feed/entry/content/properties/Description`-Modell leírása.
* `feed/entry/content/properties/CatalogFileName`-Fájl modell katalógusnév.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get A List Of All Models</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-28T14:35:51Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfAllModelsEntity</title>
    <updated>2014-10-28T14:35:51Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetAllModels?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">68b232f2-1037-45f7-8f49-af822695ee63</d:Id>
                    <d:Name m:type="Edm.String">vah-11111</d:Name>
                    <d:Date m:type="Edm.String">10/28/2014 2:16:07 PM</d:Date>
                    <d:Status m:type="Edm.String">ReadyForBuild</d:Status>
                    <d:HasActiveBuild m:type="Edm.String">false</d:HasActiveBuild>
                    <d:BuildId m:type="Edm.String">-1</d:BuildId>
                    <d:Mpr m:type="Edm.String">0</d:Mpr>
                    <d:UsageFileNames m:type="Edm.String">ImplicitMatrix10_Guid_small.txt, ImplicitMatrix11_Guid_small.txt</d:UsageFileNames>
                    <d:CatalogId m:type="Edm.String">626babdb-cab6-43a6-82ef-4fb86345700c</d:CatalogId>
                    <d:UserName m:type="Edm.String">89dc4722-03ba-4f90-8821-b1db388408b5@dm.com</d:UserName>
                    <d:Description m:type="Edm.String">short description</d:Description>
                    <d:CatalogFileName m:type="Edm.String">catalog10_small.txt</d:CatalogFileName>
                </m:properties>
            </content>
        </entry>
    </feed>

### <a name="54----update-model"></a>5.4.    Frissítési modell
Frissítheti a modell leírása vagy az aktív build azonosítóval.<br>
<ins>Aktív build azonosító</ins> -minden build minden modell van állítva egy összeállítási. Az aktív build azonosító: az első sikeres build minden új modell. Amennyiben van egy aktív build ID azonosítója és teheti meg ugyanannak a modellnek további alkot, explicit módon állítsa be alapértelmezett build Azonosítóként, ha azt szeretné, hogy szüksége. Felhasznált ajánlásokat, ha nem adja meg a build Azonosítóját, amelynek szeretné használni, amikor az alapértelmezett automatikusan használható.<br>
Ez az eljárás lehetővé teszi - után egy javaslat modell éles - új modell összeállításához és teszteléséhez őket, mielőtt termelési támogatja azokat.

| HTTP-metódus | URI |
|:--- |:--- |
| A PUT |`<rootURI>/UpdateModel?id=%27<modelId>%27&apiVersion=%271.0%27`<br>Példa:<br>`<rootURI>/UpdateModel?id=%279559872f-7a53-4076-a3c7-19d9385c1265%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| id |A modell (kis-és nagybetűket) egyedi azonosítója |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |`<ModelUpdateParams xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">`<br>`<Description>New Description</Description>`<br>`<ActiveBuildId>-1</ActiveBuildId>`<br>` </ModelUpdateParams>`<br><br>Ne feledje, hogy az XML-címkék leírását és ActiveBuildId nem kötelező. Ha nem szeretné állítani a leírás vagy ActiveBuildId, távolítsa el a teljes címke. |

**Válasz**:

HTTP-állapotkód: 200

### <a name="55----delete-model"></a>5.5.    Modell törlése
Törli a meglévő modell által azonosítóját.

| HTTP-metódus | URI |
|:--- |:--- |
| TÖRLÉSE |`<rootURI>/DeleteModel?id=%27<model_id>%27&apiVersion=%271.0%27`<br>Példa:<br>`<rootURI>/DeleteModel?id=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| id |A modell (kis-és nagybetűket) egyedi azonosítója |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Delete Model by Id</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-28T10:39:33Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">DeleteModelByIdEntity</title>
    <updated>2014-10-28T10:39:33Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/DeleteModel?id='1cac7b76-def4-41f1-bc81-29b806adb1de'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:string m:type="Edm.String"></d:string>
      </m:properties>
    </content>
      </entry>
    </feed>

## <a name="6-model-advanced"></a>6. Speciális modell
### <a name="61----model-data-insight"></a>6.1.    Modell adatok felmérése
A használati adatok alapján ez a modell a következővel történt a statisztikai adatokat ad vissza.

Csak a javaslat build esetén érhető el.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/GetDataInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Példa:<br>`<rootURI>/GetDataInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

Az adatok tulajdonságainak gyűjteményét adja vissza a rendszer.

* `feed/entry/id/content/properties/key`-Érvényes tulajdonságnév.
* `feed/entry/id/content/properties/value`-A tulajdonság értékét fogja tárolni.

Az alábbi táblázat az egyes kulcsok értéket mutatja be.

| Kulcs | Leírás |
|:--- |:--- |
| AvgItemLength |Elemenként egyedi felhasználók átlagos száma. |
| AvgUserLength |Felhasználónként eltérő elemek átlagos száma. |
| DensificationNumberOfItems |Cikkek követően nem kell modellezni törlési elemek száma. |
| DensificationNumberOfUsers |Használati pontok után törlési felhasználók és nem kell modellezni elemek száma. |
| DensificationNumberOfRecords |Használati pontok után törlési felhasználók és nem kell modellezni elemek száma. |
| MaxItemLength |A legnépszerűbb elem egyedi felhasználók száma. |
| MaxUserLength |Egy felhasználó különálló elemek maximális száma. |
| MinItemLength |Egy elem egyedi felhasználók maximális száma. |
| MinUserLength |Egy felhasználó különálló elemek minimális száma. |
| RawNumberOfItems |A fájlok elemek száma. |
| RawNumberOfUsers |Mielőtt bármely karbantartási eltávolítási használati pontok száma. |
| RawNumberOfRecords |Mielőtt bármely karbantartási eltávolítási használati pontok száma. |
| SamplingNumberOfItems |N/A |
| SamplingNumberOfRecords |N/A |
| SamplingNumberOfUsers |N/A |

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get data insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgItemLength</d:Key>
        <d:Value m:type="Edm.String">18.936</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">AvgUserLength</d:Key>
        <d:Value m:type="Edm.String">51.879</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfItems</d:Key>
        <d:Value m:type="Edm.String">2,000</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Key m:type="Edm.String">DensificationNumberOfRecords</d:Key>
        <d:Value m:type="Edm.String">37,872</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetDataInsightStatisticsEntity</title>
    <updated>2014-10-27T14:21:21Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
    <content type="application/xml">
        <m:properties>
            <d:Key m:type="Edm.String">DensificationNumberOfUsers</d:Key>
            <d:Value m:type="Edm.String">730</d:Value>
      </m:properties>
    </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxItemLength</d:Key>
                <d:Value m:type="Edm.String">4,704</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MaxUserLength</d:Key>
                <d:Value m:type="Edm.String">190</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinItemLength</d:Key>
                <d:Value m:type="Edm.String">8</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">MinUserLength</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">RawNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfItems</d:Key>
                <d:Value m:type="Edm.String">2,000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=13&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfRecords</d:Key>
                <d:Value m:type="Edm.String">72,022</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1</id>
        <title type="text">GetDataInsightStatisticsEntity</title>
        <updated>2014-10-27T14:21:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetDataInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=14&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">SampelingNumberOfUsers</d:Key>
                <d:Value m:type="Edm.String">9,044</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="62----model-insight"></a>6.2.    Modell felmérése
A modell insight értéket ad vissza a aktív összeállítása a vagy (Ha a kapott) adott build.

Csak a javaslat build esetén érhető el.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |Aktív build azonosítójú:<br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/GetModelInsight?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br>Adott build azonosítójú:<br>`<rootURI>/GetModelInsight?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| buildId |Nem kötelező – sikeres build azonosító szám. |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

Az adatok tulajdonságainak gyűjteményét adja vissza a rendszer.

* `feed/entry/id/content/properties/key`
* `feed/entry/id/content/properties/value`

Az alábbi táblázat az egyes kulcsok értéket mutatja be.

| Kulcs | Leírás |
|:--- |:--- |
| CatalogCoverage |A katalógus részét modellezhető használati mintákat. A további elemeket kell tartalom-alapú szolgáltatásokat. |
| MPR-hez |Átlagos PERCENTILIS rangsorolási a modellnek. Jobb alsó. |
| NumberOfDimensions |A mátrix factorization algoritmus által használt dimenziók száma. |

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get model insight statistics</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">CatalogCoverage</d:Key>
                <d:Value m:type="Edm.String">1.000</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">Mpr</d:Key>
                <d:Value m:type="Edm.String">0.367</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetModelInsightStatisticsEntity</title>
        <updated>2014-10-27T14:27:11Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelInsight?modelId='6254b40d-0514-49cb-a298-b81256d2b3ca'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Key m:type="Edm.String">NumberOfDimensions</d:Key>
                <d:Value m:type="Edm.String">20</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="63----get-model-sample"></a>6.3.    Modell minta beolvasása
Lekérdezi a javaslat modell minta.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/GetModelSample?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Példa:<br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27`<br><br>Adott build azonosítójú:<br>`<rootURI>/GetModelSample?modelId=%27<model_id>%27&buildId=%27<build_id>%27&apiVersion=%271.0%27`<br>Példa:<br>`<rootURI>/GetModelSample?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&buildId=%271500068%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

Az OData-XML

Nyers szöveges formátumú választ küld vissza:

<pre>
Level 1
---------------
655fc955-a5a3-4a26-9723-3090859cb27b, Prey: A Novel
    655fc955-a5a3-4a26-9723-3090859cb27b, Prey: A Novel Rating: 0.5215
    3f471802-f84f-44a0-99c8-6d2e7418eec1, Black Hawk Down: A Story of Modern War Rating: 0.5151
    07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon Rating: 0.5148
    6afc18e4-8c2a-43d1-9021-57543d6b11d8, Imajica Rating: 0.5146
    e4cc5e69-3567-43ab-b00f-f0d8d0506870, Hit List Rating: 0.514
56b61441-0eed-46cc-a8f6-112775b81892, Life and Death in Shanghai
    56b61441-0eed-46cc-a8f6-112775b81892, Life and Death in Shanghai Rating: 0.5218
    53156702-cc0c-443d-b718-6fb74b2491d3, Son of \ Rating: 0.5212
    fb8cf7a6-8719-46ee-97d4-92f931d77a3a, Smoke and Mirrors: Short Fictions and Illusions Rating: 0.5188
    8f5fe006-79e4-4679-816b-950989d1db4b, A Place I've Never Been (Contemporary American Fiction) Rating: 0.5156
    d8db4583-cc0f-49ce-bc95-b7fa3491623f, Happiness: A Novel Rating: 0.5156
50471eec-9aeb-4900-84d7-21567ab18546, If the Buddha Dated: A Handbook for Finding Love on a Spiritual Path
    cfe922a1-7ca0-4f8d-ad9d-b7cc87bfe0ef, Divine Secrets of the Ya-Ya Sisterhood: A Novel Rating: 0.5266
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, The Poisonwood Bible: A Novel Rating: 0.5252
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5244
    e2cbf7ad-0636-4117-8b30-298da6df7077, Animal Dreams Rating: 0.5227
    6c818fd3-5a09-417d-9ab4-7ffe090f0fef, Confessions of an Ugly Stepsister: A Novel Rating: 0.5222
5e97148f-defb-4d74-af2d-80f4763bf531, The Deep End of the Ocean (Oprah's Book Club)
    5e97148f-defb-4d74-af2d-80f4763bf531, The Deep End of the Ocean (Oprah's Book Club) Rating: 0.537
    5dcbac37-2946-4f2a-a0b3-bbe710f9409a, Up Island: A Novel Rating: 0.5277
    bc5b69db-733b-4346-adde-3927544258f7, Downtown Rating: 0.5275
    31fe5c63-3e5a-48d0-802b-d3b0f989a634, Have a Nice Day: A Tale of Blood and Sweatsocks Rating: 0.5252
    0adf981a-b65b-4c11-b36b-78aca2f948a2, The Perfect Storm: A True Story of Men Against the Sea Rating: 0.5238
68f97068-ae1a-4163-9e94-396b800b743d, Modoc: The True Story of the Greatest Elephant That Ever Lived
    68f97068-ae1a-4163-9e94-396b800b743d, Modoc: The True Story of the Greatest Elephant That Ever Lived Rating: 0.5379
    6724862e-e4e7-4022-9614-1468d8b902ff, Little House on the Prairie Rating: 0.5345
    cdedb837-1620-496d-94c4-6ccfed888320, Little House in the Big Woods Rating: 0.5325
    382164ba-406b-4187-b726-d7a54b9d790d, The Tao of Pooh Rating: 0.5309
    6a068d6a-bb74-4ba3-b3f2-a956c4f9d1b5, On the Banks of Plum Creek Rating: 0.5285
37ef8e74-e348-44e5-aabc-1d7f9efcb25b, Men Are from Mars Women Are from Venus: A Practical Guide for Improving Communication and Getting What You Want in Your Relationships
    37ef8e74-e348-44e5-aabc-1d7f9efcb25b, Men Are from Mars, Women Are from Venus: A Practical Guide for Improving Communication and Getting What You Want in Your Relationships Rating: 0.5397
    f2be16d4-5faf-4d32-ab83-7ba74d29261e, Politically Correct Bedtime Stories: Modern Tales for Our Life and Times Rating: 0.5207
    ef732c5c-334b-4d6b-ab82-7255eb7286d0, Honor Among Thieves Rating: 0.5195
    0b209b8c-7cdd-47fd-b940-05c7ff7c60fc, The Giving Tree Rating: 0.5194
    883b360f-8b42-407f-b977-2f44ad840877, Scary Stories to Tell in the Dark: Collected from American Folklore (Scary Stories) Rating: 0.5184
ff51b67e-fa8e-4c5e-8f4d-02a928de735d, Men at Work: The Craft of Baseball
    d008dae9-c73a-40a1-9a9b-96d5cf546f36, The Gulag Archipelago 1918-1956: An Experiment in Literary Investigation I-II Rating: 0.5416
    ff51b67e-fa8e-4c5e-8f4d-02a928de735d, Men at Work: The Craft of Baseball Rating: 0.5403
    49dec30e-0adb-411a-b186-48eaabf6f8bc, Fatherland Rating: 0.5394
    cc7964fd-d30f-478e-a425-93ddbdf094ed, Magic the Gathering: Arena Vol. 1 Rating: 0.5379
    8a1e9f36-97af-4614-bed9-24e3940a05f3, More Sniglets: Any Word That Doesn't Appear in the Dictionary but Should Rating: 0.5377
12a6d988-be21-4a09-8143-9d5f4261ba16, A Dream of Eagles
    07b10e28-9e7c-4032-90b7-10acab7f2460, Cryptonomicon Rating: 0.5417
    e4cc5e69-3567-43ab-b00f-f0d8d0506870, Hit List Rating: 0.5416
    1f1a34c4-9781-49f5-a3cc-acec3ae3c71d, The Family Rating: 0.5371
    56daeffe-7d48-43cd-8ef8-7dffd0c103d3, Kilo Class Rating: 0.5366
    b2fe511e-5cb9-4a56-b823-2801e63e6a96, Legal Tender Rating: 0.5366
df87525b-e435-4bd6-8701-4e60ad344e28, Finding Fish
    56d33036-dfda-46b9-8e2a-76cb03921bb0, The X-Files: Ground Zero Rating: 0.5417
    0780cde8-6529-4e1d-b6c6-082c1b80e596, Twelve Red Herrings Rating: 0.5416
    df87525b-e435-4bd6-8701-4e60ad344e28, Finding Fish Rating: 0.5408
    400fe331-2c35-490c-adbc-b28b4b73d56c, Shall We Tell the President? Rating: 0.5383
    f86ad7d0-5c03-42b3-aebf-13d44aec8b30, Shades of Grace Rating: 0.5358
de1f62a4-89e6-44d2-aaee-992a4bf093f1, The Map That Changed the World: William Smith and the Birth of Modern Geology
    de1f62a4-89e6-44d2-aaee-992a4bf093f1, The Map That Changed the World: William Smith and the Birth of Modern Geology Rating: 0.5422
    b303538f-e2c6-4a2c-b425-8d21e684fc3e, My Uncle Oswald Rating: 0.5385
    34b84627-48af-4a4c-96c4-b26fb3863f56, Midnight In the Garden of Good and Evil Rating: 0.5379
    306cbaa7-b1a8-4142-9d55-e11b5018a7a8, The Street Lawyer Rating: 0.5376
    e53b4baa-8c09-45c4-95c0-b6a26b98770b, Miss Smillas Feeling for Snow Rating: 0.5367

Level 2
---------------
352aaea1-6b12-454d-a3d5-46379d9e4eb2, The Sinister Pig (Hillerman Tony)
    352aaea1-6b12-454d-a3d5-46379d9e4eb2, The Sinister Pig (Hillerman Tony) Rating: 0.5425
    74c49398-bc10-4af5-a658-a996a1201254, Children of the Storm (Peters Elizabeth) Rating: 0.5387
    9ba80080-196e-43fd-8025-391d963f77e7, The Floating Girl Rating: 0.5372
    e68f81d5-7745-4cc7-b943-fedb8fcc2ced, Killer Smile (Scottoline Lisa) Rating: 0.5353
    b2fe511e-5cb9-4a56-b823-2801e63e6a96, Legal Tender Rating: 0.5332
c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon days
    0adf981a-b65b-4c11-b36b-78aca2f948a2, The Perfect Storm: A True Story of Men Against the Sea Rating: 0.5433
    c65c3995-abf7-4c7b-bb3c-8eb5aa9be7a5, Lake Wobegon days Rating: 0.543
    a00ae6ad-4a7f-4211-9836-75ce8834eb11, Sniglets (Snig'lit: Any Word That Doesn't Appear in the Dictionary But Should) Rating: 0.5327
    6f6e192e-0d64-49ca-9b63-f09413ea1ee6, Politically Correct Holiday Stories: For an Enlightened Yuletide Season Rating: 0.5307
    798051a8-147d-4d46-b0dc-e836325029e6, AGE OF INNOCENCE (MOVIE TIE-IN) Rating: 0.5301
73f3e25a-e996-4162-9ed8-ff3d34075650, O Pioneers! (Penguin Twentieth-Century Classics)
    cba8163f-6536-436b-8130-47b4a43c827f, Trust No One (The Official Guide to the X-Files Vol. 2) Rating: 0.5434
    5708e4cb-2492-49c0-94a8-cc413eec5d89, Small Gods (Discworld Novels (Paperback)) Rating: 0.5406
    73f3e25a-e996-4162-9ed8-ff3d34075650, O Pioneers! (Penguin Twentieth-Century Classics) Rating: 0.5403
    d885b0bd-ae4b-452d-bdf2-faa90197dbc9, The Color of Magic Rating: 0.539
    b133a9c4-4784-4db3-b100-d0d6dffb94d2, The Truth Is Out There (The Official Guide to the X-Files Vol. 1) Rating: 0.5367
271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: Or I Know Why the Winged Whale Sings
    271700a5-854a-4d5a-8409-6b57a5ee4de4, Fluke: Or I Know Why the Winged Whale Sings Rating: 0.5445
    2de1c354-90ff-47c5-a0db-1bad7d88ef94, The Salaryman's Wife (Children of Violence Series) Rating: 0.5329
    d279416e-19c0-43f8-9ec9-a585947879ca, Zen Attitude Rating: 0.5316
    c8f854d7-3de3-4b23-8217-f4f851670fd4, Revenge of the Cootie Girls: A Robin Hudson Mystery (Robin Hudson Mysteries (Paperback)) Rating: 0.5305
    8ef4751c-7074-409e-a3ac-d49b222fc864, Where the Wild Things Are Rating: 0.5289
9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, Their Eyes Were Watching God
    9ad1b620-0a7b-4543-8673-66d4c3bcb2f1, Their Eyes Were Watching God Rating: 0.5446
    da45c4d5-aba1-413b-a9bd-50df98b1e1d2, The Bean Trees Rating: 0.5389
    65ecbdd1-131c-40c3-a3d6-d86ca281377a, The God of Small Things Rating: 0.5387
    c78743bf-7947-4a0c-8db7-8a3bfe69ba70, The Stone Diaries Rating: 0.5355
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5344
5f17d90a-2604-4fe8-8977-1a280b9098b1, One for the Money (Stephanie Plum Novels (Paperback))
    5f17d90a-2604-4fe8-8977-1a280b9098b1, One for the Money (Stephanie Plum Novels (Paperback)) Rating: 0.5446
    57169b2b-9a8a-486b-9aac-1ed98ce57168, Final Appeal Rating: 0.5332
    efcb1bc4-7278-4a8f-b491-befde02070d6, Moment of Truth Rating: 0.5329
    1efa91a2-993b-4c43-9f5c-3454fc12612d, Burn Factor Rating: 0.5309
    24c59962-458a-4ec8-b95d-d694e861919c, At Home in Mitford (The Mitford Years) Rating: 0.5303
4fd48c46-1a20-4c57-bc7f-a02ef123dc52, As Nature Made Him: The Boy Who Was Raised As a Girl
    4fd48c46-1a20-4c57-bc7f-a02ef123dc52, As Nature Made Him: The Boy Who Was Raised As a Girl Rating: 0.5449
    cd5f2c03-20cb-43be-a1fb-3b4233e63222, Pigs in Heaven Rating: 0.5329
    19985fdb-d07a-4a25-ae4a-97b9cb61e5d1, Love in the Time of Cholera (Penguin Great Books of the 20th Century) Rating: 0.5267
    15689d09-c711-4844-84d8-130a90237b26, Bel Canto Rating: 0.5245
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, The Poisonwood Bible: A Novel Rating: 0.5235
98df28ec-41e7-4fca-b77f-8b0d3109085d, Star Trek Memories
    f874b5a3-5d40-4436-94ff-0fa1c090ddf5, The Sun Also Rises (A Scribner classic) Rating: 0.5451
    98df28ec-41e7-4fca-b77f-8b0d3109085d, Star Trek Memories Rating: 0.5442
    0ce0014a-9a48-4013-a08a-7f2c11877930, H.M.S. Unseen Rating: 0.5421
    15316ca6-1e38-425f-893d-691944a47000, More Scary Stories To Tell In The Dark Rating: 0.5409
    329d5682-3dc3-4206-8aa2-eef4b1032258, Letters from the Earth Rating: 0.54
5b9445d5-c072-419c-8d49-6f669bb1b0a9, Daughter of Fortune: A Novel (Oprah's Book Club (Hardcover))
    5b9445d5-c072-419c-8d49-6f669bb1b0a9, Daughter of Fortune: A Novel (Oprah's Book Club (Hardcover)) Rating: 0.5462
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, The Poisonwood Bible: A Novel Rating: 0.5372
    604eb3bd-6026-4f51-bffd-9fb54f180400, Family Pictures: A Novel Rating: 0.5341
    8d06d01d-31cd-4678-b6b1-140a67987ce9, Songs in Ordinary Time (Oprah's Book Club (Paperback)) Rating: 0.5334
    da45c4d5-aba1-413b-a9bd-50df98b1e1d2, The Bean Trees Rating: 0.5319
d5358189-d70f-4e35-8add-34b83b4942b3, Pigs in Heaven
    d5358189-d70f-4e35-8add-34b83b4942b3, Pigs in Heaven Rating: 0.5491
    ff91a483-1ce5-4b37-a6fd-5ffcf21f8745, The Poisonwood Bible: A Novel Rating: 0.5401
    c78743bf-7947-4a0c-8db7-8a3bfe69ba70, The Stone Diaries Rating: 0.5393
    8d06d01d-31cd-4678-b6b1-140a67987ce9, Songs in Ordinary Time (Oprah's Book Club (Paperback)) Rating: 0.5382
    973f8cbd-0846-4f6b-9d28-4dd0d7dc3a19, Pigs in Heaven Rating: 0.5367

</pre>


## <a name="7-model-business-rules"></a>7. Modell az üzleti szabályok
A támogatott szabályok típusai a következők:

* <strong>BlockList</strong> -BlockList lehetővé teszi, hogy adja meg a javaslat eredményéből nem kívánt elemek listáját. 
* <strong>FeatureBlockList</strong> -funkció BlockList lehetővé teszi a blokkolása szolgáltatása értékek alapján.

*Ne küldjön 1000-nél több elemek egy egyetlen blocklist szabályban, vagy a hívás előfordulhat, hogy az időkorlát. Ha 1000-nél több elemet zárolnia kell, hogy több blocklist hívások.*

* <strong>Upsale</strong> -Upsale lehetővé teszi a javaslat eredmények visszaadandó elemek kényszerítéséhez.
* <strong>Engedélyezési lista</strong> -fehér lista lehetővé teszi csak elemek listájából javaslatokat.
* <strong>FeatureWhiteList</strong> -funkció fehér lista lehetővé teszi, hogy csak az elemek, amelyekhez az adott termékfejlesztő értékek ajánlani.
* <strong>PerSeedBlockList</strong> -/ Seed tiltólista lehetővé teszi elemenként javaslat eredményeként nem adható vissza elemek listáját.

### <a name="71----get-model-rules"></a>7.1.    Modell szabályok lekérése
| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/GetModelRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br>Példa:<br>`<rootURI>/GetModelRules?modelId=%271cac7b76-def4-41f1-bc81-29b806adb1de%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

* `feed/entry/content/properties/Id`-Ez a szabály egyedi azonosítója.
* `feed/entry/content/properties/Type`– A szabály típusa.
* `feed/entry/content/properties/Parameter`-Szabály paraméter.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get a list of rules for a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T12:58:57Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000043</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1000"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfRulesForAModelEntity</title>
        <updated>2014-11-05T12:58:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelRules?modelId='5e824626-50d3-469d-a824-564d38453103'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000044</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1001"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="72----add-rule"></a>7.2.    Szabály hozzáadása
| HTTP-metódus | URI |
|:--- |:--- |
| POST |`<rootURI>/AddRule?apiVersion=%271.0%27` |
| FEJLÉC |`"Content-Type", "text/xml"` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| apiVersion |1.0 |
|  | |
| A kérelem törzse | |

<ins>Amikor biztosít azonosítónak olyan üzleti szabályok esetén, ügyeljen arra, hogy használja a cikk (azonos azonosítóval, amelyet a katalógus fájlban használt) külső azonosítóját</ins><br>
<ins>BlockList szabály hozzáadása:</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>BlockList</Type><Value>{"ItemsToExclude":["2406E770-769C-4189-89DE-1C9283F93A96","3906E110-769C-4189-89DE-1C9283F98888"]}</Value></ApiFilter>`<br><br><ins>
<ins>FeatureBlockList szabály hozzáadása:</ins><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureBlockList</Type><Value>{"Name":"Movie_category","Values":["Adult","Drama"]}</Value></ApiFilter>`<br><br><ins>Upsale szabály hozzáadása:</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>Upsale</Type><Value>{"ItemsToUpsale":["2406E770-769C-4189-89DE-1C9283F93A96"],"NumberOfItemsToUpsale":5}</Value></ApiFilter>`<br><br>
<ins>Engedélyezési szabály hozzáadása:</ins><br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>WhiteList</Type><Value>{"ItemsToInclude":["2406E770-769C-4189-89DE-1C9283F93A96","1116E770-769C-4189-89DE-1C9283F88888"]}</Value></ApiFilter>`<br><br><ins>
<ins>FeatureWhiteList szabály hozzáadása:</ins><br>
<br>
`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>FeatureWhiteList</Type><Value>{"Name":"Movie_rating","Values":["PG13"]}</Value></ApiFilter>`<br><br><ins>PerSeedBlockList szabály hozzáadása:</ins><br>`<ApiFilter xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"><ModelId>24024f7e-b45c-419e-bfa2-dfd947e0d253</ModelId><Type>PerSeedBlockList</Type><Value>{"SeedItems":["9949"],"ItemsToExclude":["9862","8158","8244"]}</Value></ApiFilter>`|

**Válasz**:

HTTP-állapotkód: 200

Az API-t adja vissza az újonnan létrehozott szabály a hozzá tartozó részletek. A szabályok tulajdonság lekérhetők a következő elérési utak:

* `feed/entry/content/properties/Id`-Ez a szabály egyedi azonosítója.
* `feed/entry/content/properties/Type`-A szabály típusa: BlockList vagy Upsale.
* `feed/entry/content/properties/Parameter`-Szabály paraméter.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Add A Rule</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-05T11:13:28Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AddARuleEntity</title>
        <updated>2014-11-05T11:13:28Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/AddRule?apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">1000041</d:Id>
                <d:Type m:type="Edm.String">BlockList</d:Type>
                <d:Parameters m:type="Edm.String">{"ItemsToExclude":["1002"]}</d:Parameters>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="73----delete-rule"></a>7.3.    A szabály törlése
| HTTP-metódus | URI |
|:--- |:--- |
| TÖRLÉSE |`<rootURI>/DeleteRule?modelId=%27<model_id>%27&filterId=%27<filter_Id>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`DeleteRule?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&filterId=%271000011%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| filterId |A szűrő egyedi azonosítója |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

### <a name="74----delete-all-rules"></a>7.4.    A szabályok törlése
| HTTP-metódus | URI |
|:--- |:--- |
| TÖRLÉSE |`<rootURI>/DeleteAllRules?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`DeleteAllRules?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

## <a name="8-catalog"></a>8. Alkalmazáskatalógus
### <a name="81----import-catalog-data"></a>8.1.    Eseménykatalógus-adatok importálása
Ha ugyanannak a modellnek több hívások több katalógus fájlt feltölteni, azt csak az új katalóguselemek szúrja be. A meglévő elemek marad az eredeti értékekkel. Eseménykatalógus-adatok a metódus nem frissíthető.

A katalógus adatait a következő formátumot kell követnie:

* Szolgáltatások – nélkül`<Item Id>,<Item Name>,<Item Category>[,<Description>]`
* -Szolgáltatásokkal`<Item Id>,<Item Name>,<Item Category>,[<Description>],<Features list>`

Megjegyzés: A fájl maximális mérete 200MB.

** Részletek formázása **

| Név | Kötelező | Típus | Leírás |
|:--- |:--- |:--- |:--- |
| Cikk azonosítója |Igen |[A-z], [a-z], [0-9], [_] &#40; Aláhúzás &#41; [-] &#40; Dash &#41;<br> Maximális hossz: 50 |Egy elem egyedi azonosítója. |
| Elem neve |Igen |Bármely alfanumerikus karakterek<br> Maximális hossz: 255 |Elem neve. |
| Elem kategória |Igen |Bármely alfanumerikus karakterek <br> Maximális hossz: 255 |Kategória, amelyhez ez a cikk tartozik (pl. főzés könyvek, tragédiát...); üres is lehet. |
| Leírás |Nem, kivéve, ha a funkciók jelent-e (de üres lehet) |Bármely alfanumerikus karakterek <br> Maximális hossz: 4000 |Ez az elem leírása. |
| Szolgáltatások listája |Nem |Bármely alfanumerikus karakterek <br> Maximális hossz: 4000; Szolgáltatások: 20 maximális száma |Szolgáltatás neve vesszővel elválasztott listája modell ajánlás; javítására használható szolgáltatás érték = Lásd: [témakörök speciális](#2-advanced-topics) szakasz. |

| HTTP-metódus | URI |
|:--- |:--- |
| POST |`<rootURI>/ImportCatalogFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/ImportCatalogFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27catalog10_small.txt%27&apiVersion=%271.0%27` |
| FEJLÉC |`"Content-Type", "text/xml"` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| fájlnév |A katalógus szöveges azonosítója.<br>Csak betűk (A-Z, a – z), szám (0-9), kötőjeleket (-) és aláhúzásjelet (_) megengedettek.<br>Maximális hossz: 50 |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |(A szolgáltatások) például:<br/>2406e770-769c-4189-89de-1c9283f93a96 Clara Callan, megjelenik a könyv leírás szerzői = Richard Wright, közzétevő = Harper Flamingo Kanada, év = 2001<br>21bf8088-b6c0-4509-870c-e1c7ac78304a, elfelejti a helyiségben: A példa (Byzantium könyv), a könyv,, szerzői = Nick Bantock, közzétevő = Harpercollins, év = 1997<br>3bb5cb44-d143-4bdd-a55c-443964bf4b23 Spadework, könyv,, szerzői = komócsin Findley, közzétevő = HarperFlamingo Kanada év = 2001<br>552a1940-21e4-4399-82bb-594b46d7ed54 korlátozás a vadállatok, megjelenik a könyv leírás szerzői = Magnus Mills, közzétevő = Arcade közzétételi év = 1998</pre> |

**Válasz**:

HTTP-állapotkód: 200

Az API-t adja vissza az importálás jelentést.

* `feed\entry\content\properties\LineCount`-Elfogadott sorok száma.
* `feed\entry\content\properties\ErrorCount`-Hiba miatt nem beillesztett sorok száma.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Import catalog file</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-10-05T06:58:04Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'" />
    <entry>
       <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ImportCatalogFileEntity2</title>
        <updated>2014-10-05T06:58:04Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportCatalogFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='catalog10_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:LineCount m:type="Edm.String">4</d:LineCount>
                <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="82----get-catalog"></a>8.2.    Katalógus beolvasása
Lekéri az összes katalóguselemeket.
A katalógus lesz egyszerre egy lap lekérése. Ha azt szeretné, egy adott indexű beolvasásának, használhatja a $skip odata paraméter. Például ha le szeretné kérdezni 100 pozíciótól kezdődően elemet, adja hozzá a $skip paraméter = 100 kérésre.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/GetCatalog?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`GetCatalog?modelId=%2724024f7e-b45c-419e-bfa2-dfd947e0d253%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

A válasz tartalmazza az elemet egy tételt. Mindegyik bejegyzés rendelkezik a következő adatokat:

* `feed/entry/content/properties/ExternalId`-Katalógus külső azonosítója, az ügyfél által a megadottal.
* `feed/entry/content/properties/InternalId`-Katalógus belső azonosítója, azt, amelyik az Azure Machine Learning javaslatok hozott létre.
* `feed/entry/content/properties/Name`-Katalógus elem neve.
* `feed/entry/content/properties/Category`-Katalógus cikk kategóriát.
* `feed/entry/content/properties/Description`-Katalógus leírása.
* `feed/entry/content/properties/Metadata`-Katalógus elem metaadatait.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get All Catalog Items</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">552A1940-21E4-4399-82BB-594B46D7ED54</d:ExternalId>
                <d:InternalId m:type="Edm.String">060db26a-e6a6-464e-bb52-436d2da82a50</d:InternalId>
                <d:Name m:type="Edm.String">Restraint of Beasts</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:ExternalId>
                <d:InternalId m:type="Edm.String">209d7bfe-2eb9-4455-92a3-7c867a41a74a</d:InternalId>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">3BB5CB44-D143-4BDD-A55C-443964BF4B23</d:ExternalId>
                <d:InternalId m:type="Edm.String">913ff79b-059b-4d4d-8042-6fa4cc1391dd</d:InternalId>
                <d:Name m:type="Edm.String">Spadework</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">AllCatalogItemsEntity</title>
        <updated>2014-10-29T11:13:26Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalog?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:ExternalId m:type="Edm.String">21BF8088-B6C0-4509-870C-E1C7AC78304A</d:ExternalId>
                <d:InternalId m:type="Edm.String">ea65e4fa-768c-40b4-92c3-69d3e8178691</d:InternalId>
                <d:Name m:type="Edm.String">The Forgetting Room: A Fiction (Byzantium Book)</d:Name>
                <d:Category m:type="Edm.String">Book</d:Category>
                <d:Description m:type="Edm.String"></d:Description>
                <d:Metadata m:type="Edm.String"></d:Metadata>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="83----get-catalog-items-by-token"></a>8.3.    Beolvasni a Katalóguselemeket a Token által
| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/GetCatalogItemsByToken?modelId=%27<modelId>%27&token=%27<token>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`GetCatalogItemsByToken?modelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&token=%27Cla%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| Token |A Katalóguselem nevű token. Legalább 3 karaktert kell tartalmaznia. |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

A válasz tartalmazza az elemet egy tételt. Mindegyik bejegyzés rendelkezik a következő adatokat:

* `feed/entry/content/properties/InternalId`-Katalógus belső azonosítója, azt, amelyik az Azure Machine Learning javaslatok hozott létre.
* `feed/entry/content/properties/Name`-Katalógus elem neve.
* `feed/entry/content/properties/Rating`-(későbbi használatra)
* `feed/entry/content/properties/Reasoning`-(későbbi használatra)
* `feed/entry/content/properties/Metadata`-(későbbi használatra)
* `feed/entry/content/properties/FormattedRating`-(későbbi használatra)

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get Catalog items that contain a token</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-29T11:48:19Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">CatalogItemsThatContainATokenEntity</title>
            <updated>2014-10-29T11:48:19Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetCatalogItemsByToken?modelId='0dbb55fa-7f11-418d-8537-8ff2d9d1d9c6'&amp;token='Cla'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                  <m:properties>
                    <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                    <d:Name m:type="Edm.String">Clara Callan</d:Name>
                    <d:Rating m:type="Edm.Double">0</d:Rating>
                    <d:Reasoning m:type="Edm.String"></d:Reasoning>
                    <d:Metadata m:type="Edm.String"></d:Metadata>
                    <d:FormattedRating m:type="Edm.Double" m:null="true"></d:FormattedRating>
                  </m:properties>
            </content>
        </entry>
    </feed>

## <a name="9-usage-data"></a>9. Használati adatok
### <a name="91----import-usage-data"></a>9.1.    Használati adatok importálása
#### <a name="911-uploading-file"></a>9.1.1. Fájl feltöltése
Ez a szakasz bemutatja, hogyan használati adatok feltöltése egy fájl segítségével. A használati adatokat tartalmazó többször API hívása. Minden használati adatokat a rendszer minden hívások menti.

| HTTP-metódus | URI |
|:--- |:--- |
| POST |`<rootURI>/ImportUsageFile?modelId=%27<modelId>%27&filename=%27<fileName>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/ImportUsageFile?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&filename=%27ImplicitMatrix10_Guid_small.txt%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| fájlnév |A katalógus szöveges azonosítója.<br>Csak betűk (A-Z, a – z), szám (0-9), kötőjeleket (-) és aláhúzásjelet (_) megengedettek.<br>Maximális hossz: 50 |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |Használati adatok. Formátum:<br>`<User Id>,<Item Id>[,<Time>,<Event>]`<br><br><table><tr><th>Név</th><th>Kötelező</th><th>Típus</th><th>Leírás</th></tr><tr><td>Felhasználói azonosító</td><td>Igen</td><td>[A-z], [a-z], [0-9], [_] &#40; Aláhúzás &#41; [-] &#40; Dash &#41;<br> Maximális hossz: 255 </td><td>A felhasználó egyedi azonosítója.</td></tr><tr><td>Cikk azonosítója</td><td>Igen</td><td>[A-z], [a-z], [0-9], [&#95;] &#40; Aláhúzás &#41; [-] &#40; Dash &#41;<br> Maximális hossz: 50</td><td>Egy elem egyedi azonosítója.</td></tr><tr><td>Time</td><td>Nem</td><td>Dátum formátumban: éééé/hh/nnTóó: pp: (pl. 2013 06/20T10:00:00)</td><td>Az adatok időpontja.</td></tr><tr><td>Esemény</td><td>Nincs; Ha a megadott dátum is kell állítani</td><td>A következők egyikét:<br>• Kattintson<br>• RecommendationClick<br>• AddShopCart<br>• RemoveShopCart<br>• Beszerzési</td><td></td></tr></table><br>Maximális fájlméret: 200MB<br><br>Példa:<br><pre>149452,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>6360,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>50321,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>71285,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>224450,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>236645,1b3d95e2-84e4-414c-bb38-be9cf461c347<br>107951,1b3d95e2-84e4-414c-bb38-be9cf461c347</pre> |

**Válasz**:

HTTP-állapotkód: 200

* `Feed\entry\content\properties\LineCount`-Elfogadott sorok száma.
* `Feed\entry\content\properties\ErrorCount`-Hiba miatt nem beillesztett sorok száma.
* `Feed\entry\content\properties\FileId`-Fájl azonosítója.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Add bulk usage data (usage file)</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T07:21:44Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">AddBulkUsageDataUsageFileEntity2</title>
    <updated>2014-10-05T07:21:44Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ImportUsageFile?modelId='a658c626-2baa-43a7-ac98-f6ee26120a12'&amp;filename='ImplicitMatrix10_Guid_small.txt'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:LineCount m:type="Edm.String">33</d:LineCount>
        <d:ErrorCount m:type="Edm.String">0</d:ErrorCount>
        <d:FileId m:type="Edm.String">fead7c1c-db01-46c0-872f-65bcda36025d</d:FileId>
      </m:properties>
    </content>
      </entry>
    </feed>


#### <a name="912-using-data-acquisition"></a>9.1.2. Adatgyűjtést használatával
Ez a szakasz bemutatja, hogyan küldhet események valós idejű Azure Machine Learning ajánlásokat, általában a webhelyről.

| HTTP-metódus | URI |
|:--- |:--- |
| POST |`<rootURI>/AddUsageEvent?apiVersion=%271.0%27` |
| FEJLÉC |`"Content-Type", "text/xml"` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| apiVersion |1.0 |
| Kérés törzsében |Esemény esetében bevitt adat minden esemény szeretne küldeni. Küldje el a ugyanazon felhasználó vagy a böngésző-munkamenet ugyanazt az Azonosítót a munkamenet-azonosító mezőben. (Lásd az alábbi esemény törzsében mintáját.) |

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
                    <PurchaseItem>
                        <ItemId>ABBF8081-C5C0-4F09-9701-E1C7AC78304A</ItemId>
                        <Count>1</Count>
                    </PurchaseItem>
                    <PurchaseItem>
                        <ItemId>21BF8088-B6C0-4509-870C-11C0AC7F304B</ItemId>
                        <Count>3</Count>
                    </PurchaseItem>
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

### <a name="92----list-model-usage-files"></a>9.2.    Modell használati fájlok listázása
Lekéri az összes modell fájlok metaadatait.
A fájlok lesznek használati egy oldalt az attribútumböngészőben egyszerre beolvasott. Minden lap containes 100 elemeket. Ha azt szeretné, egy adott indexű beolvasásának, használhatja a $skip odata paraméter. Például ha le szeretné kérdezni 100 pozíciótól kezdődően elemet, adja hozzá a $skip paraméter = 100 kérésre.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/ListModelUsageFiles?forModelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/ListModelUsageFiles?forModelId=%270dbb55fa-7f11-418d-8537-8ff2d9d1d9c6%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| forModelId |A modell egyedi azonosítója |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

A válasz használati fájlonként egy bejegyzést tartalmaz. Mindegyik bejegyzés rendelkezik a következő adatokat:

* `feed\entry\content\properties\Id`-Használati azonosítót.
* `feed\entry\content\properties\Length`-MB használat a fájl hosszát.
* `feed\entry\content\properties\DateModified`-A használati fájl létrehozásának dátuma.
* `feed\entry\content\properties\UseInModel`-E a használati fájllal a modellben.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of model's usage files info</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
            <updated>2014-10-30T09:40:25Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Id m:type="Edm.String">4c067b42-e975-4cb2-8c98-a6ab80ed6d63</d:Id>
                    <d:Length m:type="Edm.Double">0</d:Length>
                    <d:DateModified m:type="Edm.String">10/30/2014 9:19:57 AM</d:DateModified>
                    <d:UseInModel m:type="Edm.String">true</d:UseInModel>
                </m:properties>
            </content>
        </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetAListOfModelsUsageFilesInfoEntity</title>
        <updated>2014-10-30T09:40:25Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ListModelUsageFiles?forModelId='1c1110f8-7d9f-4c64-a807-4c9c5329993a'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">3126d816-4e80-4248-8339-1ebbdb9d544d</d:Id>
                <d:Length m:type="Edm.Double">0.001</d:Length>
                <d:DateModified m:type="Edm.String">10/30/2014 9:21:44 AM</d:DateModified>
                <d:UseInModel m:type="Edm.String">true</d:UseInModel>
              </m:properties>
        </content>
    </entry>
</feed>

### <a name="93----get-usage-statistics"></a>9.3.    Használati statisztikák beolvasása
Lekéri a használati statisztikáit.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/GetUsageStatistics?modelId=%27<modelId>%27& startDate=%27<date>%27&endDate=%27<date>%27&eventTypes=%27<types>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/GetUsageStatistics?modelId=%271d20c34f-dca1-4eac-8e5d-f299e4e4ad66%27&startDate=%272014%2F10%2F17T00%3A00%3A00%27&endDate=%272014%2F11%2F16T00%3A00%3A00%27&eventTypes=%271%2C2%2C3%2C4%2C5%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| A StartDate |Kezdő dátum. Formátum: éééé/hh/nnTóó: pp: |
| endDate |Záró dátum. Formátum: éééé/hh/nnTóó: pp: |
| eventTypes |Vesszővel elválasztott karakterlánc eseménytípusok vagy null az összes eseményének lekérdezése |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

Kulcs/érték elemek gyűjteménye. Mindegyik tartalmazza egy adott esemény típusa óránként csoportosítva események.

* `feed\entry[i]\content\properties\Key`– Az idő (óra szerint csoportosítva) és az eseménytípus tartalmazza.
* `feed\entry[i]\content\properties\Value`-Esemény teljes száma.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get usage statistics</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2014-11-18T11:39:16Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;Click</d:Event>
                <d:Value m:type="Edm.String">5</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/9/2014 8:00:00 AM;RecommendationClick</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
              </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/1/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
        <title type="text">GetUsageStatisticsEntity</title>
        <updated>2014-11-18T11:39:16Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUsageStatistics?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;startDate='2014/10/12T00:00:00'&amp;endDate='2014/11/10T00:00:00'&amp;eventTypes=''&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Event m:type="Edm.String">11/5/2014 8:00:00 AM;RemoveShopCart</d:Event>
                <d:Value m:type="Edm.String">10</d:Value>
            </m:properties>
        </content>
    </entry>
    </feed>

### <a name="94----get-usage-file-sample"></a>9.4.    Használati fájl minta beolvasása
Lekéri az első 2KB használati fájl tartalma.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/GetUsageFileSample?modelId=%27<modelId>%27& fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/GetUsageFileSample?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fileId=%274c067b42-e975-4cb2-8c98-a6ab80ed6d63%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| fileId |A modellfájl használati egyedi azonosítója |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

Nyers szöveges formátumú választ küld vissza:

<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
235588,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
158254,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
271195,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
141157,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
171118,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
225087,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
</pre>


### <a name="95----get-model-usage-file"></a>9.5.    Használati modellfájl beolvasása
Lekéri a használati fájl teljes tartalmát.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/GetModelUsageFile?mid=%27<modelId>%27& fid=%27<fileId>%27&download=%27<download_value>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/GetModelUsageFile?mid=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&fid=%273126d816-4e80-4248-8339-1ebbdb9d544d%27&download=%271%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| Mid |A modell egyedi azonosítója |
| FID |A modellfájl használati egyedi azonosítója |
| Letöltése |1 |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

Nyers szöveges formátumú választ küld vissza:

<pre>
85526,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
210926,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
116866,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
177458,2406E770-769C-4189-89DE-1C9283F93A96,2014/11/02T13:40:15,True,1
274004,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
123883,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
37712,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
152249,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
250948,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
235588,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
158254,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
271195,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
141157,21BF8088-B6C0-4509-870C-E1C7AC78304A,2014/11/02T13:40:15,True,1
171118,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
225087,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
244881,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
50547,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
213090,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
260655,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
72214,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
189334,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
36326,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
189336,3BB5CB44-D143-4BDD-A55C-443964BF4B23,2014/11/02T13:40:15,True,1
189334,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
260655,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
162100,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
54946,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
260965,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
102758,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
112602,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
163925,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
262998,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
144717,552A1940-21E4-4399-82BB-594B46D7ED54,2014/11/02T13:40:15,True,1
</pre>

### <a name="96----delete-usage-file"></a>9.6.    Használati fájl törlése
A megadott modell használati fájl törlése.

| HTTP-metódus | URI |
|:--- |:--- |
| TÖRLÉSE |`<rootURI>/DeleteUsageFile?modelId=%27<modelId>%27&fileId=%27<fileId>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/DeleteUsageFile?modelId=%270f86d698-d0f4-4406-a684-d13d22c47a73%27&fileId=%27f2e0b09d-be5c-46b2-9ac2-c7f622e5e1a5%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| fileId |A fájl törlendő egyedi azonosítója |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

### <a name="97----delete-all-usage-files"></a>9.7.    Minden fájlok törlése
Törli az összes modell használati fájlt.

| HTTP-metódus | URI |
|:--- |:--- |
| TÖRLÉSE |`<rootURI>/DeleteAllUsageFiles?modelId=%27<modelId>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/DeleteAllUsageFiles?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

## <a name="10-features"></a>10. Szolgáltatások
Ez a szakasz bemutatja, hogyan adatbeolvasás funkció, például az importált szolgáltatások és azok értékeit, azok dimenziószáma és a dimenziószáma lett lefoglalva. Szolgáltatások részeként az eseménykatalógus-adatok importálása, és majd a rangsor tartozik amikor rank build hajtja végre.
A szolgáltatás dimenziószáma használati adatok és típusú elemeket a minta alapján módosíthatja. De konzisztens használati elemek, a dimenziószáma csak kis ingadozását kell rendelkeznie.
A szolgáltatások rangja egy nem negatív szám. A szám 0 azt jelenti, hogy a szolgáltatás nem lett-e rangsorolva (történik, ha ez az API a sorrendet megadó első build befejezése előtt indít). A dátum, amelyen a dimenziószáma attribútummal volt a pontszám frissesség nevezik.

### <a name="101-get-features-info-for-last-rank-build"></a>10.1. Található szolgáltatások adat (a sorrendet megadó utolsó Buildverziót)
A szolgáltatás információt, beleértve a prioritást, a legutóbbi sikeres rank buildjéhez kéri le.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| samplingSize |Az egyes szolgáltatásokhoz, a katalógusban szereplő adatok alapján is tartalmazzanak száma. <br/>Lehetséges értékek:<br> -1 - összes mintát. <br>0 - nem mintavételi. <br>N - N minták minden szolgáltatás nevét adja vissza. |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

A válasz szolgáltatás info bejegyzések listáját tartalmazza. Mindegyik bejegyzés tartalmazza:

* `feed/entry/content/m:properties/d:Name`-Szolgáltatás neve.
* `feed/entry/content/m:properties/d:RankUpdateDate`-A dátum, amelyen a dimenziószáma lett lefoglalva Ez a szolgáltatás, más néven pontszám frissesség szolgáltatást. Egy korábbi dátum ("0001-01-01T00:00:00") azt jelenti, hogy a sorrendet megadó építés elvégezték-e.
* `feed/entry/content/m:properties/d:Rank`-Funkció dimenziószáma (float). A sorrend első helyén, 2.0-s, illetve a hierarchiában felfelé a helyes szolgáltatásának tekinthető.
* `feed/entry/content/m:properties/d:SampleValues`-A kért mintavételi mérete legfeljebb értékek vesszővel tagolt listája.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get the features of a model</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-01-08T13:15:02Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Author</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Name m:type="Edm.String">Publisher</d:Name>
                <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
                <d:Rank m:type="Edm.String">0</d:Rank>
                <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
            </m:properties>
        </content>
    </entry>
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
        <title type="text">ModelFeaturesEntity</title>
        <updated>2015-01-08T13:15:02Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1"/>
        <contenttype="application/xml">
        <m:properties>
            <d:Name m:type="Edm.String">Year</d:Name>
            <d:RankUpdateDate m:type="Edm.String">0001-01-01T00:00:00</d:RankUpdateDate>
            <d:Rank m:type="Edm.String">0</d:Rank>
            <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
        </m:properties>
        </content>
    </entry>
</feed>

### <a name="102-get-features-info-for-specific-rank-build"></a>10.2. Található szolgáltatások adat (a megadott sorszám Build)
Például egy adott buildjénél a sorrendet megadó rangsorolási szolgáltatás adatainak beolvasása.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/GetModelFeatures?modelId=%27<modelId>%27&samplingSize=%27<samplingSize>%27&rankBuildId=<rankBuildId>&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/GetModelFeatures?modelId=%271c1110f8-7d9f-4c64-a807-4c9c5329993a%27&samplingSize=10%27&rankBuildId=1000551&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| samplingSize |Az egyes szolgáltatásokhoz, a katalógusban szereplő adatok alapján is tartalmazzanak száma.<br/> Lehetséges értékek:<br> -1 - összes mintát. <br>0 - nem mintavételi. <br>N - N minták minden szolgáltatás nevét adja vissza. |
| rankBuildId |A sorrendet megadó build vagy -1 -nek a sorrendet megadó utolsó buildverziót egyedi azonosítója |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

A válasz szolgáltatás info bejegyzések listáját tartalmazza. Mindegyik bejegyzés tartalmazza:

* `feed/entry/content/m:properties/d:Name`-Szolgáltatás neve.
* `feed/entry/content/m:properties/d:RankUpdateDate`-A dátum, amelyen a dimenziószáma lett lefoglalva Ez a szolgáltatás, más néven pontszám frissesség szolgáltatást. Egy korábbi dátum ("0001-01-01T00:00:00") azt jelenti, hogy a sorrendet megadó építés elvégezték-e.
* `feed/entry/content/m:properties/d:Rank`-Funkció dimenziószáma (float). A sorrend első helyén, 2.0-s, illetve a hierarchiában felfelé a helyes szolgáltatásának tekinthető.
* `feed/entry/content/m:properties/d:SampleValues`-A kért mintavételi mérete legfeljebb értékek vesszővel tagolt listája.

OData

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get the features of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:54:22Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Author</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">3.38867426</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. A. Attanasio, A. A. Milne, A. Bates, A. C. Bhaktivedanta Swami Prabhupada et al., A. C. Crispin, A. C. Doyle, A. C. H. Smith, A. E. Parker, A. J. Holt, A. J. Matthews</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Publisher</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.67839336</d:Rank>
                    <d:SampleValues m:type="Edm.String">A. Mondadori, Abacus, Abacus Press, Abacus Uk, Abstract Studio, Acacia Press, Academy Chicago Publishers, Ace Books, ACE Charter, Actar</d:SampleValues>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">ModelFeaturesEntity</title>
            <updated>2015-01-08T13:54:22Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelFeatures?modelId='f13ab2e8-b530-4aa1-86f7-2f4a24714765'&amp;samplingSize='10'&amp;rankBuildId=1000653&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Name m:type="Edm.String">Year</d:Name>
                    <d:RankUpdateDate m:type="Edm.String">2015-01-08T13:52:14.673</d:RankUpdateDate>
                    <d:Rank m:type="Edm.String">1.12352145</d:Rank>
                    <d:SampleValues m:type="Edm.String">0, 1920, 1926, 1927, 1929, 1930, 1932, 1942, 1943, 1946</d:SampleValues>
                </m:properties>
            </content>
        </entry>
    </feed>


## <a name="11-build"></a>11. Felépítés
  Ez a szakasz ismerteti a különböző API-k buildek kapcsolódik. 3 különböző típusú buildek: javaslat build, a sorrendet megadó build és egy (gyakran vásárolt együtt) FBT build.

A javaslat build célja egy előrejelzéseket használt javaslat modell generálásához. (Az ilyen típusú build) előrejelzéseket térjen két verziója:

* I2I – más néven A cikk ajánlottak - elem vagy egy elemlistát, ez a beállítás lesz előrejelzése, amelyek magas érdeklődésére elemek listáját.
* U2I – más néven Felhasználói elem javaslatok – a megadott felhasználói azonosító (és nem kötelezően elemek listáját) ezt a beállítást fogja előre jelezni, amelyek a megadott felhasználó (és a további elemeket választhat) magas érdeklődésére elemek listáját. A U2I javaslatok a felhasználó a modell készült idejével érdeklő elemeket előzményeinek alapulnak.

A sorrendet megadó build, amely lehetővé teszi további információk a szolgáltatások használhatóságát műszaki build. Általában a legjobb eredmények eléréséhez a szolgáltatásokat érintő javaslat modell, meg kell tegye a következőket:

* Aktiválhatja a sorrendet megadó build, (kivéve, ha a szolgáltatások a pontszám stabil), és várjon, amíg a szolgáltatás pontszámot kap.
* A szolgáltatások rangja meghívásával beolvasása a [szolgáltatások információ](#101-get-features-info-for-last-rank-build) API.
* A javaslat build konfigurálása a következő paraméterekkel:
  * `useFeatureInModel`-Igaz értékre kell beállítani.
  * `ModelingFeatureList`– Állítsa a 2.0-s vagy több (az egyes holtversenyekben az előző lépésben lekért) megfelelően pontszámot funkcióinak vesszővel tagolt listája.
  * `AllowColdItemPlacement`-Igaz értékre kell beállítani.
  * Opcionálisan megadhat `EnableFeatureCorrelation` TRUE és `ReasoningFeatureList` szolgáltatások listájához magyarázatot (általában ugyanaz a szolgáltatások listájában modellezés vagy allista) használni kívánt.
* Indítás, a javaslat összeállítása a konfigurált adatokkal.

Megjegyzés: Ha nem adja meg a paramétereket (pl. meghívása a javaslat build paraméterek nélkül) vagy nem explicit módon letiltja az funkciók használatát (pl. `UseFeatureInModel` értéke hamis), a rendszer a szolgáltatás kapcsolatos paramétereket a magyarázat beállításához a fenti értékek abban az esetben, ha a sorrend első helyén build létezik-e.

Nincs a sorrendet megadó build és egyidejűleg ugyanannak a modellnek az ajánlás build futó korlátozva. Ettől függetlenül ugyanannak a modellnek párhuzamosan azonos típusú két változata nem futtatható.

Egy (gyakran vásárolt együtt) FBT build még egy másik javaslatok algoritmus nevezik néha "konzervatív" ajánló, ami ideális, amelyek nincsenek homogén jellegű katalógusok (homogén: könyvek, filmek, néhány étele módon; nem homogén: számítógép és eszközök, a tartományok közötti, rendkívül sokféle).

Megjegyzés: Ha a feltöltött fájlok tartalmazzák a nem kötelező mező "eseménytípus" majd FBT a modellezési csak "a Vásárlás" események lesz. Ha nincs eseménytípus biztosítja az összes esemény beszerzési kell tekinteni.

#### <a name="111-build-parameters"></a>11.1 build paraméterek
Minden build típusú paraméterek (kitaláltak alább) segítségével konfigurálhatók. Ha nem adja meg a paraméterek, a rendszer automatikusan attribútumok értéke a paraméterek build indít el a jelen információ alapján.

##### <a name="1111-usage-condenser"></a>11.1.1. Használati hűtő
Felhasználók, illetve néhány használati pont cikkeket további zaj mint információkat is tartalmazhatnak. A rendszer megkísérli előrejelzése használati pontok modellben használandó felhasználói/elemenként minimális száma. Ez a szám lesz elemek ItemCutoffLowerBound és ItemCutoffUpperBound paramétereinek megadott tartományt, és a UserCutOffLowerBound és UserCutoffUpperBound paraméterek, a felhasználók által megadott tartományon belül. Az elemek vagy felhasználók hűtő hatással nulla megfelelő határain közül legalább egy beállítást minimálisra csökkenthető.

##### <a name="1112-rank-build-parameters"></a>11.1.2. Dimenziószáma build paraméterek
Az alábbi táblázat mutatja be, hogy a sorrendet megadó build build paramétereinek.

| Kulcs | Leírás | Típus | Érvényes értéket |
|:--- |:--- |:--- |:--- |
| NumberOfModelIterations |Kezeli a modell ismétlések száma megjelenik a teljes compute idejét és a modell pontosságát. Minél nagyobb a szám, a nagyobb pontosságot elérhetővé válik, de a számítási idejét hosszabb ideig tart. |Egész szám |10-50 |
| NumberOfModelDimensions |A dimenziók száma a modellben megkísérli az adatok belül található szolgáltatások száma vonatkozik. Növelje meg a dimenziók lehetővé teszi a nagyobb a pontosabb az eredmények csoportokba kisebb beállításra. Azonban túl sok dimenzióval megakadályozza, hogy a modell találjanak korrelációk elemek között. |Egész szám |10-40 |
| ItemCutOffLowerBound |Meghatározza a hűtő elem alsó korlátja. Tekintse meg a fenti használati hűtő. |Egész szám |2 vagy több (a 0 hűtő letiltása) |
| ItemCutOffUpperBound |Meghatározza az elem a hűtő felső korlátja. Tekintse meg a fenti használati hűtő. |Egész szám |2 vagy több (a 0 hűtő letiltása) |
| UserCutOffLowerBound |Meghatározza a hűtő felhasználói alsó korlátja. Tekintse meg a fenti használati hűtő. |Egész szám |2 vagy több (a 0 hűtő letiltása) |
| UserCutOffUpperBound |Határozza meg a felhasználó a hűtő felső korlátja. Tekintse meg a fenti használati hűtő. |Egész szám |2 vagy több (a 0 hűtő letiltása) |

##### <a name="1113-recommendation-build-parameters"></a>11.1.3. A javaslat build paraméterek
Az alábbi táblázat a javaslat build build paramétereinek ábrázol.

| Kulcs | Leírás | Típus | Érvényes értéket |
|:--- |:--- |:--- |:--- |
| NumberOfModelIterations |Kezeli a modell ismétlések száma megjelenik a teljes compute idejét és a modell pontosságát. Minél nagyobb a szám, a nagyobb pontosságot elérhetővé válik, de a számítási idejét hosszabb ideig tart. |Egész szám |10-50 |
| NumberOfModelDimensions |A dimenziók száma a modellben megkísérli az adatok belül található szolgáltatások száma vonatkozik. Növelje meg a dimenziók lehetővé teszi a nagyobb a pontosabb az eredmények csoportokba kisebb beállításra. Azonban túl sok dimenzióval megakadályozza, hogy a modell találjanak korrelációk elemek között. |Egész szám |10-40 |
| ItemCutOffLowerBound |Meghatározza a hűtő elem alsó korlátja. Tekintse meg a fenti használati hűtő. |Egész szám |2 vagy több (a 0 hűtő letiltása) |
| ItemCutOffUpperBound |Meghatározza az elem a hűtő felső korlátja. Tekintse meg a fenti használati hűtő. |Egész szám |2 vagy több (a 0 hűtő letiltása) |
| UserCutOffLowerBound |Meghatározza a hűtő felhasználói alsó korlátja. Tekintse meg a fenti használati hűtő. |Egész szám |2 vagy több (a 0 hűtő letiltása) |
| UserCutOffUpperBound |Határozza meg a felhasználó a hűtő felső korlátja. Tekintse meg a fenti használati hűtő. |Egész szám |2 vagy több (a 0 hűtő letiltása) |
| Leírás |Build leírása. |Karakterlánc |A szöveg, legfeljebb 512 karakter |
| EnableModelingInsights |A javaslat modell metrikák számítási teszi lehetővé. |Logikai érték |Igaz/hamis |
| UseFeaturesInModel |Azt jelzi, ha szolgáltatása használható a javaslat modell növelése érdekében. |Logikai érték |Igaz/hamis |
| ModelingFeatureList |A javaslat növelése érdekében a javaslat építés használandó neveinek vesszővel tagolt listája. |Karakterlánc |Funkcióneveket, legfeljebb 512 karakter |
| AllowColdItemPlacement |Azt jelzi, ha a javaslat is leküldéses cold elemek szolgáltatás hasonlóság keresztül. |Logikai érték |Igaz/hamis |
| EnableFeatureCorrelation |Azt jelzi, ha szolgáltatások indoklást is használható. |Logikai érték |Igaz/hamis |
| ReasoningFeatureList |Vesszővel tagolt listája mintafelismerési mondat (pl. javaslat magyarázatokat) használt. |Karakterlánc |Funkcióneveket, legfeljebb 512 karakter |
| EnableU2I |A személyre szabott javaslat más néven engedélyezése U2I (elem javaslatok felhasználó). |Logikai érték |Igaz/hamis (alapértelmezett értéke igaz) |

##### <a name="1114-fbt-build-parameters"></a>11.1.4. FBT build paraméterek
Az alábbi táblázat a javaslat build build paramétereinek ábrázol.

| Kulcs | Leírás | Típus | Érvénytelen érték (alapértelmezett) |
|:--- |:--- |:--- |:--- |
| FbtSupportThreshold |Hogyan konzervatív a modell van. Figyelembe kell venni modellezési elemek közös előfordulásainak száma. |Egész szám |3-50 (6) |
| FbtMaxItemSetSize |Bounds gyakori csoportban lévő elemek száma. |Egész szám |2-3 (2) |
| FbtMinimalScore |Egy gyakori készlet kell rendelkeznie kell ahhoz, hogy a keresés eredményeit minimális pontszámot. Minél nagyobb a megfelelőbb. |Dupla |0 és újabb (0) |
| FbtSimilarityFunction |Meghatározza a hasonlóság működnek, mint a build használják. Növekedési előtérbe serendipity, közös előfordulási előtérbe kiszámíthatóságot és Jaccard a két töltött kompromisszumot. |Karakterlánc |cooccurrence, növekedési, jaccard (növekedési) |

### <a name="112-trigger-a-recommendation-build"></a>11.2. A javaslat Build eseményindító
  Alapértelmezés szerint ez az API javaslat modell build vált. Elindítani a sorrendet megadó build (ahhoz, hogy szolgáltatások pontozása), a build API variant build típusú paraméterrel kell használni.

| HTTP-metódus | URI |
|:--- |:--- |
| POST |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&apiVersion=%271.0%27` |
| FEJLÉC |`"Content-Type", "text/xml"`(Ha a kérelem törzse küldése) |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| userDescription |A katalógus szöveges azonosítója. Vegye figyelembe, hogy ha a tárolóhelyek használata akkor kell kódolása % 20 helyette. Tekintse meg a fenti példa.<br>Maximális hossz: 50 |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |Ha üres majd a build fogja végrehajtani az alapértelmezett build paraméterekkel.<br><br>Ha be szeretné állítani a build paraméterek, küldje el a paraméterek XML törzsébe, ahogy az alábbi minta. (Lásd a "Paraméterek létrehozása" című szakasz annak magyarázatát, a paraméterek.)`<NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance><EnableModelingInsights>true</EnableModelingInsights><UseFeaturesInModel>false</UseFeaturesInModel><ModelingFeatureList>feature_name_1,feature_name_2,...</ModelingFeatureList><AllowColdItemPlacement>false</AllowColdItemPlacement><EnableFeatureCorrelation>false</EnableFeatureCorrelation><ReasoningFeatureList>feature_name_a,feature_name_b,...</ReasoningFeatureList></BuildParametersList>` |

**Válasz**:

HTTP-állapotkód: 200

Ez az egy aszinkron API-t. Válaszként elérhetővé válik egy build azonosítója. Ha a build véget ért, kell az "Első buildek állapota az egy modell" API, és keresse meg a választ a build Azonosítóját. Fontos tudni, hogy build is perces óra adatok méretétől függően.

Nem lehet felhasználni a build: javaslatok karakterlánccal végződik-e.

Érvényes létrehozásának állapota:

* Hozzon létre - létrehozási kérelem lett létrehozva.
* Aszinkron - Build kérést küld, és azt a várólistára.
* Épület - Build folyamatban van.
* Sikeres – a létrehozási művelet sikeresen befejeződött.
* Hiba – Build hibával ért véget.
* Megszakítva - Build megszakították.
* Kapcsolódó - a build megszakítási kérelmet küldött.

Vegye figyelembe, hogy megtalálható-e a build azonosítója a következő elérési úton:`Feed\entry\content\properties\Id`

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Build a Model with RequestBody</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T08:56:34Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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

### <a name="113-trigger-build-recommendation-rank-or-fbt"></a>11.3. Eseményindító Build (javaslat, dimenziószáma vagy FBT)
| HTTP-metódus | URI |
|:--- |:--- |
| POST |`<rootURI>/BuildModel?modelId=%27<modelId>%27&userDescription=%27<description>%27&buildType=%27<buildType>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/BuildModel?modelId=%27a658c626-2baa-43a7-ac98-f6ee26120a12%27&userDescription=%27First%20build%27&buildType=%27Ranking%27&apiVersion=%271.0%27` |
| FEJLÉC |`"Content-Type", "text/xml"`(Ha a kérelem törzse küldése) |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| userDescription |A katalógus szöveges azonosítója. Vegye figyelembe, hogy ha a tárolóhelyek használata akkor kell kódolása % 20 helyette. Tekintse meg a fenti példa.<br>Maximális hossz: 50 |
| buildType |A meghívni kívánt build típusú: <br/> -Javaslat build "ajánlott" <br> -"Prioritás" rank buildjéhez <br/> -FBT build "Fbt" |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |Ha üres majd a build fogja végrehajtani az alapértelmezett build paraméterekkel.<br><br>Ha be szeretné állítani a build paraméterek, küldje el XML formátumban a szervezet például az alábbi mintában azokat. (Lásd a "Paraméterek Build" Magyarázat és a paraméterek teljes listája.)`<BuildParametersList><NumberOfModelIterations>40</NumberOfModelIterations><NumberOfModelDimensions>20</NumberOfModelDimensions><MinItemAppearance>5</MinItemAppearance><MinUserAppearance>5</MinUserAppearance></BuildParametersList>` |

**Válasz**:

HTTP-állapotkód: 200

Ez az egy aszinkron API-t. Válaszként elérhetővé válik egy build azonosítója. Ha a build véget ért, kell az "Első buildek állapota az egy modell" API, és keresse meg a választ a build Azonosítóját. Fontos tudni, hogy build is perces óra adatok méretétől függően.

Nem lehet felhasználni a build: javaslatok karakterlánccal végződik-e.

Érvényes létrehozásának állapota:

* Hozzon létre - modell lett létrehozva.
* Aszinkron - modell build lett elindítva, és azt a várólistára.
* Épület - modell éppen készül.
* Sikeres – a létrehozási művelet sikeresen befejeződött.
* Hiba – Build hibával ért véget.
* Megszakítva - Build megszakították.
* Kapcsolódó - Build megszakítás alatt áll.

Vegye figyelembe, hogy megtalálható-e a build azonosítója a következő elérési úton:`Feed\entry\content\properties\Id`

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Build a Model with RequestBody</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T08:56:34Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">BuildAModelEntity2</title>
    <updated>2014-10-05T08:56:34Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/BuildModel?modelId='9559872f-7a53-4076-a3c7-19d9385c1265'&amp;userDescription='First%20build'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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




### <a name="114-get-builds-status-of-a-model"></a>11.4. A modell buildek állapotának beolvasása
Lekéri a buildek és azok állapotát a megadott modell.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/GetModelBuildsStatus?modelId=%27<modelId>%27&onlyLastBuild=<bool>&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/GetModelBuildsStatus?modelId=%279559872f-7a53-4076-a3c7-19d9385c1265%27&onlyLastBuild=true&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| onlyLastBuild |Azt jelzi, hogy a modell létrehozási előzmények vagy csak a legutóbbi build állapotának visszaadása |
| apiVersion |1.0 |

**Válasz**:

HTTP-állapotkód: 200

A válasz tartalmazza a build egy tételt. Mindegyik bejegyzés rendelkezik a következő adatokat:

* `feed/entry/content/properties/UserName`-Felhasználó nevét.
* `feed/entry/content/properties/ModelName`-A modell neve.
* `feed/entry/content/properties/ModelId`-Modell egyedi azonosítója.
* `feed/entry/content/properties/IsDeployed`-E a buildverziót központilag telepítik (más néven aktív build).
* `feed/entry/content/properties/BuildId`-Build egyedi azonosítója.
* `feed/entry/content/properties/BuildType`-A build típusú.
* `feed/entry/content/properties/Status`-Létrehozási állapot. A következők egyike lehet: Hiba történt, épület, várakozik, Cancelling, megszakítva, sikeres.
* `feed/entry/content/properties/StatusMessage`– Részletes állapotüzenet (csak bizonyos állapotaihoz vonatkozik).
* `feed/entry/content/properties/Progress`-Build (%) folyamatban van.
* `feed/entry/content/properties/StartTime`-Build kezdési időpontja.
* `feed/entry/content/properties/EndTime`-A befejezési idő felépítéséhez.
* `feed/entry/content/properties/ExecutionTime`-Build időtartama.
* `feed/entry/content/properties/ProgressStep`-A folyamatban lévő build aktuális állapotának részleteit.

Érvényes létrehozásának állapota:

* Létre - létrehozási kérelem tétel jött létre.
* Aszinkron - kérelmet lett elindítva, és azt a várólistára.
* Épület - Build folyamatban van.
* Sikeres – a létrehozási művelet sikeresen befejeződött.
* Hiba – Build hibával ért véget.
* Megszakítva - Build megszakították.
* Kapcsolódó - Build megszakítás alatt áll.

Build típusú engedélyezett érvényes értékek:

* Sorszám - dimenziószáma felépítéséhez.
* A javaslat - javaslat build.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a model</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T17:51:10Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T17:51:10Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetModelBuildsStatus?modelId='1d20c34f-dca1-4eac-8e5d-f299e4e4ad66'&amp;onlyLastBuild=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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


### <a name="115-get-builds-status"></a>11.5. Buildek állapotának beolvasása
Lekéri az összes modellt a felhasználói állapotok felépítéséhez.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=<bool>&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/GetUserBuildsStatus?onlyLastBuilds=true&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| onlyLastBuild |Azt jelzi, hogy a modell létrehozási előzményei, vagy csak a legutóbbi build állapotát. |
| apiVersion |1.0 |

**Válasz**:

HTTP-állapotkód: 200

A válasz tartalmazza a build egy tételt. Mindegyik bejegyzés rendelkezik a következő adatokat:

* `feed/entry/content/properties/UserName`-Felhasználó nevét.
* `feed/entry/content/properties/ModelName`-A modell neve.
* `feed/entry/content/properties/ModelId`-Modell egyedi azonosítója.
* `feed/entry/content/properties/IsDeployed`-E a buildverziót központilag telepítik.
* `feed/entry/content/properties/BuildId`-Build egyedi azonosítója.
* `feed/entry/content/properties/BuildType`-A build típusú.
* `feed/entry/content/properties/Status`-Létrehozási állapot. A következők egyike lehet: Hiba történt, épület, várakozik, megszakítva, Cancelling, sikeres.
* `feed/entry/content/properties/StatusMessage`– Részletes állapotüzenet (csak bizonyos állapotaihoz vonatkozik).
* `feed/entry/content/properties/Progress`-Build (%) folyamatban van.
* `feed/entry/content/properties/StartTime`-Build kezdési időpontja.
* `feed/entry/content/properties/EndTime`-A befejezési idő felépítéséhez.
* `feed/entry/content/properties/ExecutionTime`-Build időtartama.
* `feed/entry/content/properties/ProgressStep`-A folyamatban lévő build aktuális állapotának részleteit.

Érvényes létrehozásának állapota:

* Létre - létrehozási kérelem tétel jött létre.
* Aszinkron - kérelmet lett elindítva, és azt a várólistára.
* Épület - Build folyamatban van.
* Sikeres – a létrehozási művelet sikeresen befejeződött.
* Hiba – Build hibával ért véget.
* Megszakítva - Build megszakították.
* Kapcsolódó - Build megszakítás alatt áll.

Build típusú engedélyezett érvényes értékek:

* Sorszám - dimenziószáma felépítéséhez.
* A javaslat - javaslat build.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get builds status of a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-05T18:41:21Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildsStatusEntity</title>
            <updated>2014-11-05T18:41:21Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserBuildsStatus?onlyLastBuilds=False&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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


### <a name="116-delete-build"></a>11.6. Build törlése
Build törli.

MEGJEGYZÉS: <br>Egy aktív build nem törölhető. A modell frissíteni kell a különböző aktív buildre törlés előtt.<br>Egy folyamatban lévő build nem törölhető. Törölnie kell a build először meghívásával <strong>Szerkesztés megszakítása</strong>.

| HTTP-metódus | URI |
|:--- |:--- |
| TÖRLÉSE |`<rootURI>/DeleteBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/DeleteBuild?buildId=%271500068%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| buildId |A build egyedi azonosítója. |
| apiVersion |1.0 |

**Válasz:**

HTTP-állapotkód: 200

### <a name="117-cancel-build"></a>11.7. Szakítsa meg a Build
Állapot fejlesztése során build visszavonása.

| HTTP-metódus | URI |
|:--- |:--- |
| A PUT |`<rootURI>/CancelBuild?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/CancelBuild?buildId=%271500076%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| buildId |A build egyedi azonosítója. |
| apiVersion |1.0 |

**Válasz:**

HTTP-állapotkód: 200

### <a name="118-get-build-parameters"></a>11.8. Build paraméterek lekérése
Lekéri a paraméterek felépítéséhez.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/GetBuildParameters?buildId=%27<buildId>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/GetBuildParameters?buildId=%271000653%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| buildId |A build egyedi azonosítója. |
| apiVersion |1.0 |

**Válasz:**

HTTP-állapotkód: 200

Ez az API a kulcs/érték elemgyűjtemény adja vissza. Minden elem egy paraméter és az érték jelöli:

* `feed/entry/content/properties/Key`-Build paraméter neve.
* `feed/entry/content/properties/Value`-Build paraméter értékét.

Az alábbi táblázat az egyes kulcsok értéket mutatja be.

| Kulcs | Leírás | Típus | Érvényes értéket |
|:--- |:--- |:--- |:--- |
| NumberOfModelIterations |Kezeli a modell ismétlések száma megjelenik a teljes compute idejét és a modell pontosságát. Minél nagyobb a szám, a nagyobb pontosságot elérhetővé válik, de a számítási idejét hosszabb ideig tart. |Egész szám |10-50 |
| NumberOfModelDimensions |A dimenziók száma a modellben megkísérli az adatok belül található szolgáltatások száma vonatkozik. Növelje meg a dimenziók lehetővé teszi a nagyobb a pontosabb az eredmények csoportokba kisebb beállításra. Azonban túl sok dimenzióval megakadályozza, hogy a modell találjanak korrelációk elemek között. |Egész szám |10-40 |
| ItemCutOffLowerBound |Meghatározza a hűtő elem alsó korlátja. Tekintse meg a fenti használati hűtő. |Egész szám |2 vagy több (a 0 hűtő letiltása) |
| ItemCutOffUpperBound |Meghatározza az elem a hűtő felső korlátja. Tekintse meg a fenti használati hűtő. |Egész szám |2 vagy több (a 0 hűtő letiltása) |
| UserCutOffLowerBound |Meghatározza a hűtő felhasználói alsó korlátja. Tekintse meg a fenti használati hűtő. |Egész szám |2 vagy több (a 0 hűtő letiltása) |
| UserCutOffUpperBound |Határozza meg a felhasználó a hűtő felső korlátja. Tekintse meg a fenti használati hűtő. |Egész szám |2 vagy több (a 0 hűtő letiltása) |
| Leírás |Build leírása. |Karakterlánc |A szöveg, legfeljebb 512 karakter |
| EnableModelingInsights |A javaslat modell metrikák számítási teszi lehetővé. |Logikai érték |Igaz/hamis |
| UseFeaturesInModel |Azt jelzi, ha szolgáltatása használható a javaslat modell növelése érdekében. |Logikai érték |Igaz/hamis |
| ModelingFeatureList |A javaslat növelése érdekében a javaslat építés használandó neveinek vesszővel tagolt listája. |Karakterlánc |Funkcióneveket, legfeljebb 512 karakter |
| AllowColdItemPlacement |Azt jelzi, ha a javaslat is leküldéses cold elemek szolgáltatás hasonlóság keresztül. |Logikai érték |Igaz/hamis |
| EnableFeatureCorrelation |Azt jelzi, ha szolgáltatások indoklást is használható. |Logikai érték |Igaz/hamis |
| ReasoningFeatureList |Vesszővel tagolt listája mintafelismerési mondat (pl. javaslat magyarázatokat) használt. |Karakterlánc |Funkcióneveket, legfeljebb 512 karakter |

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get build parameters</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2015-01-08T13:50:57Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'" />
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UseFeaturesInModel</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">AllowColdItemPlacement</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableFeatureCorrelation</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">EnableModelingInsights</d:Key>
                    <d:Value m:type="Edm.String">False</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelIterations</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
            <titletype="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">NumberOfModelDimensions</d:Key>
                    <d:Value m:type="Edm.String">10</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <linkrel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ItemCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffLowerBound</d:Key>
                    <d:Value m:type="Edm.String">2</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">UserCutOffUpperBound</d:Key>
                    <d:Value m:type="Edm.String">2147483647</d:Value>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=10&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ModelingFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=11&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">ReasoningFeatureList</d:Key>
                    <d:Value m:type="Edm.String"/>
                </m:properties>
            </content>
        </entry>
        <entry>
            <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1</id>
            <title type="text">GetBuildParametersEntity</title>
            <updated>2015-01-08T13:50:57Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetBuildParameters?buildId='1000653'&amp;apiVersion='1.0'&amp;$skip=12&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:Key m:type="Edm.String">Description</d:Key>
                    <d:Value m:type="Edm.String">rankBuild</d:Value>
                </m:properties>
            </content>
        </entry>
    </feed>

## <a name="12-recommendation"></a>12. Ajánlás
### <a name="121-get-item-recommendations-for-active-build"></a>12.1. Elem javaslatok beolvasása (az aktív build)
A aktív build típusú javaslatok beszerzése "Ajánlás" vagy "Fbt" mag (bemeneti) elemek listája alapján.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| hogy az elemazonosítók |Az elemek javasolt vesszővel tagolt listája. <br>Ha az aktív build típusú FBT majd küldhet csak egy elemet. <br>Maximális hossz: 1024 |
| numberOfResults |Szükséges eredmények száma <br> Maximális: 150 |
| includeMetatadata |Jövőbeli használatra, mindig hamis |
| apiVersion |1.0 |

**Válasz:**

HTTP-állapotkód: 200

A válasz egy tételt ajánlott cikk tartalmazza. Mindegyik bejegyzés rendelkezik a következő adatokat:

* `Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.
* `Feed\entry\content\properties\Name`-Az elem nevét.
* `Feed\entry\content\properties\Rating`-Értékelése az ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.
* `Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).

Az alábbi példa egy válasz 10 ajánlott elemet tartalmaz.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get Recommendation</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T12:28:48Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=3&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=4&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=5&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=6&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=7&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=8&amp;$top=1" />
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
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1</id>
    <title type="text">GetRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=10&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=9&amp;$top=1" />
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

### <a name="122-get-item-recommendations-of-a-specific-build"></a>12.2. (Az adott build) elem javaslatok beszerzése
Egy adott build "Ajánlás" vagy "Fbt" típusú javaslatok beszerzése.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/ItemRecommend?modelId=%27<modelId>%27&itemIds=%27<itemId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/ItemRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=1234&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| hogy az elemazonosítók |Az elemek javasolt vesszővel tagolt listája. <br>Ha az aktív build típusú FBT majd küldhet csak egy elemet. <br>Maximális hossz: 1024 |
| numberOfResults |Szükséges eredmények száma <br> Maximális: 150 |
| includeMetatadata |Jövőbeli használatra, mindig hamis |
| buildId |a build azonosítót használja a javaslat kérelem |
| apiVersion |1.0 |

**Válasz:**

HTTP-állapotkód: 200

A válasz egy tételt ajánlott cikk tartalmazza. Mindegyik bejegyzés rendelkezik a következő adatokat:

* `Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.
* `Feed\entry\content\properties\Name`-Az elem nevét.
* `Feed\entry\content\properties\Rating`-Értékelése az ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.
* `Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).

A válasz példa a 12.1

### <a name="123-get-fbt-recommendations-for-active-build"></a>12.3. (Az aktív build) FBT javaslatok beszerzése
Tekintse át a javasolt a aktív build típusú, "Fbt" alapján a kezdőérték (bemeneti) elemet.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=<double>&includeMetadata=false&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| itemId |Ajánlott elemet. <br>Maximális hossz: 1024 |
| numberOfResults |Szükséges eredmények száma <br>Maximális: 150 |
| minimalScore |Minimális pontszám, amelyben egy gyakori készlet kell ahhoz, hogy a keresés eredményeit |
| includeMetatadata |Jövőbeli használatra, mindig hamis |
| apiVersion |1.0 |

**Válasz:**

HTTP-állapotkód: 200

A válasz egy tételt ajánlott elem beállítása (olyan elemek, amelyek általában a kezdőérték/bemeneti elemszintű együtt vannak vásárolt készlete) tartalmazza. Mindegyik bejegyzés rendelkezik a következő adatokat:

* `Feed\entry\content\properties\Id1`-Ajánlott elem azonosítóját.
* `Feed\entry\content\properties\Name1`-Az elem nevét.
* `Feed\entry\content\properties\Id2`– 2. ajánlott azonosítója (opcionális).
* `Feed\entry\content\properties\Name2`-A 2. (nem kötelező) elem nevét.
* `Feed\entry\content\properties\Rating`-Értékelése az ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.
* `Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).

Az alábbi példa egy válasz 3 ajánlott elem beállítása magában foglalja.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
      <title type="text" />
      <subtitle type="text">Get Recommendation</subtitle>
      <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemId='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'</id>
      <rights type="text" />
      <updated>2014-10-05T12:28:48Z</updated>
      <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'" />
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">159</d:Id1>
        <d:Name1 m:type="Edm.String">159</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.543343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '159'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=1&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">52</d:Id1>
        <d:Name1 m:type="Edm.String">52</d:Name1>
        <d:Id2 m:type="Edm.String"></d:Id2>
        <d:Name2 m:type="Edm.String"></d:Name2>
        <d:Rating m:type="Edm.Double">0.533343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '52'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
      <entry>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1</id>
    <title type="text">GetFbtRecommendationEntity</title>
    <updated>2014-10-05T12:28:48Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/ItemFbtRecommend?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;itemIds='1003'&amp;numberOfResults=3&amp;minimalScore=0.1&amp;includeMetadata=false&amp;apiVersion='1.0'&amp;$skip=2&amp;$top=1" />
    <content type="application/xml">
      <m:properties>
        <d:Id1 m:type="Edm.String">35</d:Id1>
        <d:Name1 m:type="Edm.String">35</d:Name1>
        <d:Id2 m:type="Edm.String">102</d:Id2>
        <d:Name2 m:type="Edm.String">102</d:Name2>
        <d:Rating m:type="Edm.Double">0.523343480387708</d:Rating>
        <d:Reasoning m:type="Edm.String">People who bought '1003' also bought '35' and '102'</d:Reasoning>
      </m:properties>
    </content>
      </entry>
    </feed>

### <a name="124-get-fbt-recommendations-of-a-specific-build"></a>12.4. (Az adott build) FBT javaslatok beszerzése
Egy adott build "Fbt" típusú javaslatok beszerzése.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/ItemFbtRecommend?modelId=%27<modelId>%27&itemId=%27<itemId>%27&numberOfResults=<int>&minimalScore=<double>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/ItemFbtRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&itemId=%271003%27&numberOfResults=10&minimalScore=0.1&includeMetadata=false&buildId=1234&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| itemId |Ajánlott elemet. <br>Maximális hossz: 1024 |
| numberOfResults |Szükséges eredmények száma <br>Maximális: 150 |
| minimalScore |Minimális pontszám, amelyben egy gyakori készlet kell ahhoz, hogy a keresés eredményeit |
| includeMetatadata |Jövőbeli használatra, mindig hamis |
| buildId |a build azonosítót használja a javaslat kérelem |
| apiVersion |1.0 |

**Válasz:**

HTTP-állapotkód: 200

A válasz egy tételt ajánlott elem beállítása (olyan elemek, amelyek általában a kezdőérték/bemeneti elemszintű együtt vannak vásárolt készlete) tartalmazza. Mindegyik bejegyzés rendelkezik a következő adatokat:

* `Feed\entry\content\properties\Id1`-Ajánlott elem azonosítóját.
* `Feed\entry\content\properties\Name1`-Az elem nevét.
* `Feed\entry\content\properties\Id2`– 2. ajánlott azonosítója (opcionális).
* `Feed\entry\content\properties\Name2`-A 2. (nem kötelező) elem nevét.
* `Feed\entry\content\properties\Rating`-Értékelése az ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.
* `Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).

A válasz példa a 12.3

### <a name="125-get-user-recommendations-for-active-build"></a>12.5. Felhasználói javaslatok beolvasása (az aktív build)
A build "Ajánlás" jelölésű aktív build típusú felhasználói javaslatok beszerzése.

Az API-t a felhasználó a használati előzményei alapján előre jelzett elem listáját adja vissza.

Megjegyzések: 

1. Nincs felhasználói javaslat FBT buildjéhez van.
2. Ha az aktív build ezt a módszert fog FBT hibát ad vissza.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| Felhasználói azonosítóját |A felhasználó egyedi azonosítója |
| numberOfResults |Szükséges eredmények száma |
| includeMetatadata |Jövőbeli használatra, mindig hamis |
| apiVersion |1.0 |

**Válasz:**

HTTP-állapotkód: 200

A válasz egy tételt ajánlott cikk tartalmazza. Mindegyik bejegyzés rendelkezik a következő adatokat:

* `Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.
* `Feed\entry\content\properties\Name`-Az elem nevét.
* `Feed\entry\content\properties\Rating`-Értékelése az ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.
* `Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).

A válasz példa a 12.1

### <a name="126-get-user-recommendations-with-item-list-for-active-build"></a>12.6. Felhasználói javaslatok lekérdezni elemek listáját (az aktív build)
A build "Ajánlás" jelölésű elemek bejegyzés további listája az aktív build típusú felhasználói javaslatok beszerzése

Az API-t a felhasználó a használati előzményei alapján előre jelzett elem és a további megadott elemek listáját adja vissza.

Megjegyzések: 

1. Nincs felhasználói javaslat FBT buildjéhez van.
2. Ha az aktív build ezt a módszert fog FBT hibát ad vissza.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%2C1000%27&numberOfResults=10&includeMetadata=false&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| Felhasználói azonosítóját |A felhasználó egyedi azonosítója |
| itemsIds |Az elemek javasolt vesszővel tagolt listája. Maximális hossz: 1024 |
| numberOfResults |Szükséges eredmények száma |
| includeMetatadata |Jövőbeli használatra, mindig hamis |
| apiVersion |1.0 |

**Válasz:**

HTTP-állapotkód: 200

A válasz egy tételt ajánlott cikk tartalmazza. Mindegyik bejegyzés rendelkezik a következő adatokat:

* `Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.
* `Feed\entry\content\properties\Name`-Az elem nevét.
* `Feed\entry\content\properties\Rating`-Értékelése az ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.
* `Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).

A válasz példa a 12.1

### <a name="127-get-user-recommendations--of-a-specific-build"></a>12.7. (Az adott build) felhasználói javaslatok beszerzése
Egy adott build "Ajánlás" típusú felhasználói javaslatok beszerzése.

Az API-t (az adott build szerepel) felhasználó a használati előzményei alapján előre jelzett elem listáját adja vissza.

Megjegyzés: Nincs FBT buildjéhez nincs felhasználói javaslat.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| Felhasználói azonosítóját |A felhasználó egyedi azonosítója |
| numberOfResults |Szükséges eredmények száma |
| includeMetatadata |Jövőbeli használatra, mindig hamis |
| buildId |a build azonosítót használja a javaslat kérelem |
| apiVersion |1.0 |

**Válasz:**

HTTP-állapotkód: 200

A válasz egy tételt ajánlott cikk tartalmazza. Mindegyik bejegyzés rendelkezik a következő adatokat:

* `Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.
* `Feed\entry\content\properties\Name`-Az elem nevét.
* `Feed\entry\content\properties\Rating`-Értékelése az ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.
* `Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).

A válasz példa a 12.1

### <a name="128-get-user-recommendations-with-item-list-of-a-specific-build"></a>12.8. Felhasználói javaslatok beszerzése elem listáját (adott build)
Egy adott build "Ajánlás" típusú felhasználói javaslatok és további elemek listájának kapják meg.

Az API-t visszaállítja az előre jelzett elem a használati előzményei alapján a felhasználó listáját, valamint a további elemek listáját.

Megjegyzés: A Tthere nincs felhasználói javaslat FBT buildjéhez.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/UserRecommend?modelId=%27<modelId>%27&userId=%27<userId>%27&itemsIds=%27<itemsIds>%27&numberOfResults=<int>&includeMetadata=<bool>&buildId=<int>&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/UserRecommend?modelId=%272779c063-48fb-46c1-bae3-74acddc8c1d1%27&userId=%27u1101%27&itemsIds=%271003%27&numberOfResults=10&includeMetadata=false&buildId=50012&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| Felhasználói azonosítóját |A felhasználó egyedi azonosítója |
| hogy az elemazonosítók |Az elemek javasolt vesszővel tagolt listája. Maximális hossz: 1024 |
| numberOfResults |Szükséges eredmények száma |
| includeMetatadata |Jövőbeli használatra, mindig hamis |
| buildId |a build azonosítót használja a javaslat kérelem |
| apiVersion |1.0 |

**Válasz:**

HTTP-állapotkód: 200

A válasz egy tételt ajánlott cikk tartalmazza. Mindegyik bejegyzés rendelkezik a következő adatokat:

* `Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.
* `Feed\entry\content\properties\Name`-Az elem nevét.
* `Feed\entry\content\properties\Rating`-Értékelése az ajánlás; nagyobb szám azt jelenti, hogy magasabb vetett bizalmat.
* `Feed\entry\content\properties\Reasoning`-A javaslat mintafelismerési (pl. javaslat magyarázatokat).

A válasz példa a 12.1

## <a name="13-user-usage-history"></a>13. A felhasználó használati előzményei
Miután egy javaslat modell lett létrehozva a rendszer engedélyezi a felhasználók előzményei (egy adott felhasználóhoz tartozó elemeket) beolvasása a build használatos.
Ez az API engedélyezése a felhasználók előzményei beolvasása

Megjegyzés: a felhasználók előzményei érhető el jelenleg csak a javaslat buildek.

### <a name="131-retrieve-user-history"></a>13.1 beolvasása felhasználók előzményei
A megadott felhasználói azonosító az aktív build vagy a megadott build használt elem listájának beolvasása.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |A felhasználók előzményei beolvasása az active build.<br/>`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&apiVersion=%271.0%27`<br/><br/>A felhasználó előzményeinek lekérése a megadott build`<rootURI>/GetUserHistory?modelId=%27<model_id>%27&userId=%27<userId>%27&buildId=<int>&apiVersion=%271.0%27`<br/><br/>Példa:`<rootURI>/GetUserHistory?modelId=%2727967136e8-f868-4258-9331-10d567f87fae%27&&userId=%27u_1013%27&apiVersion=%271.0%277` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |a modell egyedi azonosítóját. |
| Felhasználói azonosítóját |a felhasználó egyedi azonosítója. |
| buildId |nem kötelező paraméter, annak jelzésére, hogy mely build a a felhasználók előzményei kell fetch engedélyezése |
| apiVersion |1.0 |

**Válasz:**

HTTP-állapotkód: 200

A válasz egy tételt ajánlott cikk tartalmazza. Mindegyik bejegyzés rendelkezik a következő adatokat:

* `Feed\entry\content\properties\Id`-Ajánlott elem azonosítóját.
* `Feed\entry\content\properties\Name`-Az elem nevét.
* `Feed\entry\content\properties\Rating`– NEM ÁLL RENDELKEZÉSRE.
* `Feed\entry\content\properties\Reasoning`– NEM ÁLL RENDELKEZÉSRE.

Az OData-XML

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
    <title type="text" />
    <subtitle type="text">Get User History</subtitle>
    <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'</id>
    <rights type="text" />
    <updated>2015-05-26T15:32:47Z</updated>
    <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'" />
    <entry>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
        <title type="text">CatalogItemsThatContainATokenEntity</title>
        <updated>2015-05-26T15:32:47Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetUserHistory?modelId='2779c063-48fb-46c1-bae3-74acddc8c1d1'&amp;userId='u_1013'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
        <content type="application/xml">
            <m:properties>
                <d:Id m:type="Edm.String">2406E770-769C-4189-89DE-1C9283F93A96</d:Id>
                <d:Name m:type="Edm.String">Clara Callan</d:Name>
                <d:Rating m:type="Edm.Double">0</d:Rating>
                <d:Reasoning m:type="Edm.String"/>
                <d:Metadata m:type="Edm.String"/>
                <d:FormattedRating m:type="Edm.Double" m:null="true"/>
            </m:properties>
        </content>
    </entry>
</feed>

## <a name="14-notifications"></a>14. Értesítések
Az Azure Machine Learning javaslatok létrehozza értesítések, amikor állandó hiba fordulhat elő, a rendszer. Az értesítések 3 típusa van:

1. Build hiba – ezt az értesítést akkor váltódik ki, minden összeállítási hiba.
2. Hiba – az értesítés feldolgozása adatgyűjtést lesz kiváltva, ha több mint 100 hiba van a használati események száma modell feldolgozása az elmúlt 5 percben.
3. Javaslat fogyasztás hiba – ezt az értesítést lesz kiváltva, ha van 100-nál több hibák az elmúlt 5 percben az ajánlás kérelmek / modell feldolgozása során.

### <a name="141-get-notifications"></a>14.1. Értesítéseket
Lekéri az összes vagy egy egyetlen modell minden értesítést.

| HTTP-metódus | URI |
|:--- |:--- |
| GET |`<rootURI>/GetNotifications?modelId=%27<model_id>%27&apiVersion=%271.0%27`<br><br>Első összes minden értesítés:<br>`<rootURI>/GetNotifications?apiVersion=%271.0%27`<br><br>Példa egy adott modell értesítések kapcsolódnak:<br>`<rootURI>/GetNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%277` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |Nem kötelező paraméter. Ha nincs megadva, az összes modell összes értesítéseket fog kapni. <br>Érvénytelen érték: a modell egyedi azonosítóját. |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz:**

HTTP-állapotkód: 200

Az OData-XML

    The response includes one entry per notification. Each entry has the following data:
        * feed\entry\content\properties\UserName - Internal user name identification.
        * feed\entry\content\properties\ModelId - Model ID.
        * feed\entry\content\properties\Message - Notification message.
        * feed\entry\content\properties\DateCreated - Date that this notification was created in UTC format.
        * feed\entry\content\properties\NotificationType - Notification types. Values are BuildFailure, RecommendationFailure, and DataAquisitionFailure.

    <feed xmlns:base="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications" xmlns:d="http://schemas.microsoft.com/ado/2007/08/dataservices" xmlns:m="http://schemas.microsoft.com/ado/2007/08/dataservices/metadata" xmlns="http://www.w3.org/2005/Atom">
        <title type="text" />
        <subtitle type="text">Get a list of Notifications for a user</subtitle>
        <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'</id>
        <rights type="text" />
        <updated>2014-11-04T13:24:23Z</updated>
        <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'" />
        <entry>
                <id>https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1</id>
            <title type="text">GetAListOfNotificationsForAUserEntity</title>
            <updated>2014-11-04T13:24:23Z</updated>
            <link rel="self" href="https://api.datamarket.azure.com/amla/recommendations/v3/GetNotifications?modelId='967136e8-f868-4258-9331-10d567f87fae'&amp;apiVersion='1.0'&amp;$skip=0&amp;$top=1" />
            <content type="application/xml">
                <m:properties>
                    <d:UserName m:type="Edm.String">515506bc-3693-4dce-a5e2-81cb3e8efb56@dm.com</d:UserName>
                    <d:ModelId m:type="Edm.String">967136e8-f868-4258-9331-10d567f87fae</d:ModelId>
                    <d:Message m:type="Edm.String">Build failed for user</d:Message>
                    <d:DateCreated m:type="Edm.String">2014-11-04T13:23:14.383</d:DateCreated>
                    <d:NotificationType m:type="Edm.String">BuildFailure</d:NotificationType>
                </m:properties>
            </content>
        </entry>
    </feed>

### <a name="142-delete-model-notifications"></a>14.2. Modell értesítések törlése
Törli az összes olvasási értesítések levő modell esetén.

| HTTP-metódus | URI |
|:--- |:--- |
| TÖRLÉSE |`<rootURI>/DeleteModelNotifications?modelId=%<model_id>%27&apiVersion=%271.0%27`<br><br>Példa:<br>`<rootURI>/DeleteModelNotifications?modelId=%27967136e8-f868-4258-9331-10d567f87fae%27&apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| modelId |A modell egyedi azonosítója |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

### <a name="143-delete-user-notifications"></a>14.3. Felhasználó értesítései törlése
Törli az összes minden értesítést.

| HTTP-metódus | URI |
|:--- |:--- |
| TÖRLÉSE |`<rootURI>/DeleteUserNotifications?apiVersion=%271.0%27` |

| Paraméter neve | Érvényes értékek |
|:--- |:--- |
| apiVersion |1.0 |
|  | |
| A kérelem törzse |EGYIK SEM |

**Válasz**:

HTTP-állapotkód: 200

## <a name="15-legal"></a>15. Jogi tudnivalók
Ez a dokumentum biztosított ",-van". Információk és nézetek ebben a dokumentumban, beleértve az URL-cím és az egyéb webhelyhivatkozásokat, értesítés nélkül változhatnak.<br><br>
A felhasznált példák némelyike csak illusztrációs célokat szolgálnak, és kitalált esetet szemléltet. Nincs valós association vagy a kapcsolat célja, vagy eseményekkel.<br><br>
Jelen dokumentum nem biztosít semmilyen jogot semmilyen Microsoft-termékben található szellemi tulajdonhoz. Előfordulhat, hogy másolja és használja a dokumentum belső használatra, tájékoztatási céllal.<br><br>
© 2015 Microsoft. Minden jog fenntartva.

