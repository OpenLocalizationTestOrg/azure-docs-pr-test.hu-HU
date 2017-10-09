---
title: az Azure Data Lake Store aaaCopy adatok tooand |} Microsoft Docs
description: "Megtudhatja, hogyan toocopy adatok tooand Data Lake Store az Azure Data Factory használatával"
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: monicar
ms.assetid: 25b1ff3c-b2fd-48e5-b759-bb2112122e30
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: jingwang
ms.openlocfilehash: 2e78f75f3821738332dacf70f6bf2c16f0136408
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="copy-data-tooand-from-data-lake-store-by-using-data-factory"></a>Adatok tooand másolása Data Lake Store a Data Factory használatával
Ez a cikk azt ismerteti, hogyan toouse másolási tevékenység az Azure Data Factory toomove adatok tooand az Azure Data Lake Store. -Buildekről nyújtanak a hello [adatok mozgása tevékenységek](data-factory-data-movement-activities.md) cikk, a másolási tevékenység során adatátvitel áttekintését.

## <a name="supported-scenarios"></a>Támogatott helyzetek
Adatokat másolhat **az Azure Data Lake Store** toohello a következő adatokat tárolja:

[!INCLUDE [data-factory-supported-sinks](../../includes/data-factory-supported-sinks.md)]

Adatok másolása a következő adatokat tárolja hello **tooAzure Data Lake Store**:

[!INCLUDE [data-factory-supported-sources](../../includes/data-factory-supported-sources.md)]

> [!NOTE]
> Data Lake Store-fiók létrehozása a másolási tevékenység során a folyamat létrehozása előtt. További információkért lásd: [Ismerkedés az Azure Data Lake Store](../data-lake-store/data-lake-store-get-started-portal.md).

## <a name="supported-authentication-types"></a>Támogatott hitelesítési típusok
hello Data Lake Store összekötő ezeket hitelesítési típusokat támogatja:
* Egyszerű szolgáltatásnév hitelesítése
* Felhasználói hitelesítő adatok (OAuth) hitelesítés 

Azt javasoljuk, hogy a szolgáltatás egyszerű hitelesítést, különösen egy ütemezett adatok másolását a használja. Jogkivonat lejáratáról fordulhat elő a felhasználói hitelesítő adatok hitelesítéssel. További konfigurációs információkért lásd: hello [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.

## <a name="get-started"></a>Bevezetés
A másolási tevékenység, amely helyezi át az adatokat az Azure Data Lake Store vagy a különböző eszközök/API-k használatával létrehozhat egy folyamatot.

hello legegyszerűbb módja toocreate egy folyamat toocopy adatok toouse hello **másolása varázsló**. Az adatcsatorna létrehozásával hello másolása varázsló használatával oktatóanyagok esetén lásd: [oktatóanyag: hozzon létre egy folyamatot, másolása varázslóval](data-factory-copy-data-wizard-tutorial.md).

Használhatja a következő eszközök toocreate adatcsatorna hello: **Azure-portálon**, **Visual Studio**, **Azure PowerShell**, **Azure Resource Manager-sablon** , **.NET API**, és **REST API-t**. Lásd: [másolási tevékenység oktatóanyag](data-factory-copy-data-from-azure-blob-storage-to-sql-database.md) részletesen toocreate a másolási tevékenység az adatcsatorna számára.

Akár hello eszközök vagy API-k, hajtsa végre a következő lépéseket toocreate egy folyamatot, amely áthelyezi a forrásadatok az adattároló tooa fogadó adattár hello:

1. Hozzon létre egy **adat-előállító**. Egy adat-előállító tartalmazhat egy vagy több folyamatok. 
2. Hozzon létre **összekapcsolt szolgáltatások** toolink bemeneti és kimeneti adatok tárolók tooyour adat-előállítóban. Adatok másolása az Azure blob storage tooan Azure Data Lake Store az, akkor hozzon létre például két összekapcsolt szolgáltatások toolink az Azure storage-fiók és az Azure Data Lake store tooyour adat-előállítóban. Csatolt szolgáltatás tulajdonságait, amelyek adott tooAzure Data Lake Store, lásd: [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz. 
2. Hozzon létre **adatkészletek** toorepresent bemeneti és kimeneti adatok hello a másolási művelet. Hello utolsó lépésében említett hello például létrehoz egy adatkészlet toospecify hello blobtárolót és hello bemeneti adatokat tartalmazó mappát. És egy másik dataset toospecify hello mappa és a fájlelérési út hello Data Lake Store-ban hello blob-tároló átmásolva hello adatokat tartalmazó hozzon létre. Adatkészlet tulajdonságai, amelyek adott tooAzure Data Lake Store, lásd: [adatkészlet tulajdonságai](#dataset-properties) szakasz.
3. Hozzon létre egy **csővezeték** , amely fogad egy bemeneti adatkészlet és egy kimeneti adatkészletet másolási tevékenységgel. A korábban említett hello példában BlobSource forrás-és AzureDataLakeStoreSink akár használhatja a fogadó hello másolási tevékenységhez. Ehhez hasonlóan az Azure Data Lake Store tooAzure Blob Storage másolása, használható AzureDataLakeStoreSource és BlobSink hello másolási tevékenység. A másolási tevékenység tulajdonságait, amelyek adott tooAzure Data Lake Store, lásd: [tevékenység Tulajdonságok másolása](#copy-activity-properties) szakasz. További információkért hogyan toouse egy adatok tárolót, mint a forrás- és a fogadó hivatkozásra hello az adattároló hello előző szakaszban.  

Hello varázsló használatakor a Data Factory entitások (összekapcsolt szolgáltatások adatkészletek és hello pipeline) JSON-definíciók automatikusan létrejönnek. Eszközök/API-k (kivéve a .NET API-t) használata esetén adja meg a Data Factory entitások hello JSON formátumban.  JSON-definíciók, amelyek használt toocopy adatokat az Azure Data Lake Store az adat-előállító entitások minták, lásd: [JSON példák](#json-examples-for-copying-data-to-and-from-data-lake-store) című szakaszát.

a következő szakaszok hello JSON-tulajdonságok esetében használt toodefine adat-előállító entitások adott tooData Lake Store részleteit tartalmazzák.

## <a name="linked-service-properties"></a>A kapcsolódószolgáltatás-tulajdonságok
A társított szolgáltatás adatokat tároló tooa adat-előállító hivatkozásokat tartalmaz. Típusú társított szolgáltatás létrehozása **AzureDataLakeStore** toolink a Data Lake Store adatok tooyour adat-előállítóban. a következő táblázat hello JSON elemek adott tooData Lake Store kapcsolódó szolgáltatások ismerteti. Egyszerű szolgáltatásnév és a felhasználói hitelesítő adatok hitelesítése közül választhat.

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| **típusa** | hello type tulajdonság túl be kell állítani**AzureDataLakeStore**. | Igen |
| **dataLakeStoreUri** | Hello Azure Data Lake Store-fiók adatait. Ezt az információt fogad hello a következő formátumok egyikét: `https://[accountname].azuredatalakestore.net/webhdfs/v1` vagy `adl://[accountname].azuredatalakestore.net/`. | Igen |
| **előfizetés-azonosító** | Azure-előfizetés azonosítója toowhich hello Data Lake Store-fiók tartozik. | A fogadó szükséges |
| **erőforráscsoport-név** | Azure-erőforrás csoport neve toowhich hello Data Lake Store-fiók tartozik. | A fogadó szükséges |

### <a name="service-principal-authentication-recommended"></a>Szolgáltatás egyszerű hitelesítés (ajánlott)
toouse szolgáltatás egyszerű hitelesítési regisztrálása az Azure Active Directory (Azure AD) és támogatás azt hello alkalmazás entitás tooData Lake áruház eléréséhez. Részletes útmutató: [szolgáltatások közötti hitelesítési](../data-lake-store/data-lake-store-authenticate-using-active-directory.md). Jegyezze fel az értéket, amely a következő hello toodefine hello társított szolgáltatáshoz:
* Alkalmazásazonosító
* Alkalmazás kulcs 
* Bérlőazonosító

> [!IMPORTANT]
> Ha hello másolása varázsló tooauthor adatok folyamatok használ, győződjön meg arról, hogy megadta-e egyszerű hello szolgáltatást legalább egy **olvasó** hello Data Lake Store-fiók hozzáférés-vezérlés (identitások és hozzáférések felügyeletéhez) szerepkörhöz. Is, legalább az egyszerű hello szolgáltatást biztosítania **olvasási + Execute** engedély tooyour Data Lake Store gyökér ("/") és gyermekét. Ellenkező esetben láthatja üdvözlőüzenetére "hello megadott hitelesítő adatok érvénytelenek."<br/><br/>
Miután létrehozása vagy frissítése egy egyszerű szolgáltatást az Azure ad-ben, hello módosítások tootake hatás néhány percig is tarthat. Ellenőrizze a hello egyszerű szolgáltatásnév és a Data Lake Store access control lista (ACL) konfigurációkat. Ha továbbra is megjelenik az "a megadott hitelesítő adatok érvénytelenek hello" hello üzenet, várjon egy kicsit, és próbálkozzon újra.

Szolgáltatás egyszerű hitelesítés használata a következő tulajdonságok hello megadásával:

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| **servicePrincipalId** | Adja meg a hello alkalmazás ügyfél-azonosítót. | Igen |
| **servicePrincipalKey** | Adja meg a hello kulcsát. | Igen |
| **Bérlői** | Adja meg a hello bérlői adatokat (tartomány nevét vagy a bérlő azonosító) alatt az alkalmazás található. Ez által rámutató hello egér hello Azure-portál jobb felső sarkában hello kérheti le. | Igen |

**Példa: Szolgáltatás egyszerű hitelesítés**
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

### <a name="user-credential-authentication"></a>Felhasználói hitelesítő adatok hitelesítése
Másik megoldásként használhatja felhasználói hitelesítő adatok hitelesítése toocopy a vagy tooData Lake Store megadásával következő tulajdonságai hello:

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| **engedélyezési** | Kattintson a hello **engedélyezés** hello Data Factory Editor gombra, és írja be a hitelesítő adat, amelyet hozzárendel hello automatikusan létrehozott engedélyezési URL-cím toothis tulajdonság. | Igen |
| **munkamenet-azonosító** | OAuth munkamenet-azonosító hello OAuth hitelesítési munkamenetből. Minden munkamenet-azonosító egyedi, és csak egyszer használható. Ez a beállítás automatikusan létrejön a Data Factory Editor hello használatakor. | Igen |

**Példa: Felhasználók hitelesítő adatok hitelesítése**
```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "sessionId": "<session ID>",
            "authorization": "<authorization URL>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

#### <a name="token-expiration"></a>Jogkivonat lejáratáról
engedélyezési kód hello segítségével generáló hello **engedélyezés** gombra egy bizonyos idő elteltével lejár. a következő üzenet hello azt jelenti, hogy adott hello hitelesítésére szolgáló jogkivonat érvényessége lejárt:

Hitelesítőadat-műveleti hiba: invalid_grant - AADSTS70002: Hiba történt a hitelesítő adatok ellenőrzése. AADSTS70008: hello megadott hozzáférés biztosítása lejárt vagy visszavonták. Nyomkövetési azonosító: d18629e8-af88-43c5-88e3-d8419eb1fca1 Korrelációazonosító: fac30a0c-6be6-4e02-8d69-a776d2ffefd7 időbélyegző: 2015-12-15 21-09-31Z.

hello következő táblázatban a különböző típusú felhasználói fiókok hello lejárati ideje:


| Felhasználó típusa | Után lejár |
|:--- |:--- |
| Felhasználói fiókok *nem* kezeli az Azure Active Directoryban (például @hotmail.com vagy @live.com) |12 óra |
| Felhasználói fiókok Azure Active Directory által felügyelt |14 napja után utolsó szelet hello futtatása <br/><br/>90 nap, ha egy olyan OAuth-alapú társított szolgáltatás szelet 14 naponta legalább egyszer fut |

Ha a jelszó módosítása előtt hello jogkivonat lejárati ideje, hello jogkivonat azonnal lejár. Ebben a szakaszban korábban említett hello üzenet jelenik meg.

Hello segítségével tudja ismét engedélyezheti hello fiók **engedélyezés** gomb, amikor hello jogkivonat lejár tooredeploy hello társított szolgáltatás. Hello értékeket is létrehozhat **sessionId** és **engedélyezési** programozott módon használatával tulajdonságok hello következő kódot:


```csharp
if (linkedService.Properties.TypeProperties is AzureDataLakeStoreLinkedService ||
    linkedService.Properties.TypeProperties is AzureDataLakeAnalyticsLinkedService)
{
    AuthorizationSessionGetResponse authorizationSession = this.Client.OAuth.Get(this.ResourceGroupName, this.DataFactoryName, linkedService.Properties.Type);

    WindowsFormsWebAuthenticationDialog authenticationDialog = new WindowsFormsWebAuthenticationDialog(null);
    string authorization = authenticationDialog.AuthenticateAAD(authorizationSession.AuthorizationSession.Endpoint, new Uri("urn:ietf:wg:oauth:2.0:oob"));

    AzureDataLakeStoreLinkedService azureDataLakeStoreProperties = linkedService.Properties.TypeProperties as AzureDataLakeStoreLinkedService;
    if (azureDataLakeStoreProperties != null)
    {
        azureDataLakeStoreProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeStoreProperties.Authorization = authorization;
    }

    AzureDataLakeAnalyticsLinkedService azureDataLakeAnalyticsProperties = linkedService.Properties.TypeProperties as AzureDataLakeAnalyticsLinkedService;
    if (azureDataLakeAnalyticsProperties != null)
    {
        azureDataLakeAnalyticsProperties.SessionId = authorizationSession.AuthorizationSession.SessionId;
        azureDataLakeAnalyticsProperties.Authorization = authorization;
    }
}
```
Hello kódban használt hello adat-előállító osztályok, lásd: hello [AzureDataLakeStoreLinkedService osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakestorelinkedservice.aspx), [AzureDataLakeAnalyticsLinkedService osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.azuredatalakeanalyticslinkedservice.aspx), és [ AuthorizationSessionGetResponse osztály](https://msdn.microsoft.com/library/microsoft.azure.management.datafactories.models.authorizationsessiongetresponse.aspx) témaköröket. Adja hozzá egy hivatkozást tooversion `2.9.10826.1824` a `Microsoft.IdentityModel.Clients.ActiveDirectory.WindowsForms.dll` a hello `WindowsFormsWebAuthenticationDialog` hello kódban használt osztály.

## <a name="dataset-properties"></a>Adatkészlet tulajdonságai
hello beállítása toospecify egy adatkészlet toorepresent bemeneti adatok egy Data Lake Store-ban, **típus** hello dataset tulajdonsága túl**AzureDataLakeStore**. Set hello **linkedServiceName** hello dataset toohello nevének hello Data Lake Store tulajdonság társított szolgáltatás. JSON-szakaszok és meghatározása adatkészletek esetében elérhető tulajdonságok teljes listáját lásd: hello [adatkészletek létrehozása](data-factory-create-datasets.md) cikk. A JSON-ban, a DataSet adatkészlet szakaszok például **struktúra**, **rendelkezésre állási**, és **házirend**, minden adatkészlet esetében hasonló (Azure SQL-adatbázis, az Azure blob és az Azure tábla a Példa). Hello **typeProperties** szakaszban nem egyezik az adatkészlet egyes típusú, és megjelenik többek között a helyét és hello adattár hello adatok formátuma. 

Hello **typeProperties** szakasz egy adatkészlet típusú **AzureDataLakeStore** következő tulajdonságai hello tartalmazza:

| Tulajdonság | Leírás | Szükséges |
|:--- |:--- |:--- |
| **folderPath** |Elérési út toohello tároló és mappa Data Lake Store-ban. |Igen |
| **Fájlnév** |Az Azure Data Lake Store hello fájl neve. Hello **Fájlnév** tulajdonság nem kötelező, és a kis-és nagybetűket. <br/><br/>Ha megad **Fájlnév**, hello tevékenység (például a Másolás) működik-e az adott fájl hello.<br/><br/>Ha **Fájlnév** nincs megadva, másolása tartalmazza az összes fájl **folderPath** hello bemeneti adatkészlet.<br/><br/>Ha **Fájlnév** nincs megadva egy kimeneti adatkészlet és **preserveHierarchy** nincs megadva a tevékenység fogadó hello hello létrehozott fájl neve nem hello formátumú adatok. _GUID_.txt ". Például: Data.0a405f8a-93ff-4c6f-b3be-f69616f1df7a.txt. |Nem |
| **partitionedBy** |Hello **partitionedBy** tulajdonság nem kötelező megadni. Toospecify egy dinamikus elérési útját és fájlnevét-idősoros adatok használhatja. Például **folderPath** adatok órára is lehet paraméterezett. Részletek és példák: [partitionedBy tulajdonság hello](#using-partitionedby-property). |Nem |
| **formátumban** | a következő formátumban típusok hello támogatottak: **szöveges**, **JsonFormat**, **AvroFormat**, **OrcFormat**, és  **ParquetFormat**. Set hello **típus** tulajdonság alapján **formátum** tooone ezeket az értékeket. További információkért lásd: hello [szövegformátum](data-factory-supported-file-and-compression-formats.md#text-format), [JSON formátumban](data-factory-supported-file-and-compression-formats.md#json-format), [az Avro formátum](data-factory-supported-file-and-compression-formats.md#avro-format), [ORC formátum](data-factory-supported-file-and-compression-formats.md#orc-format), és [Parquet formátumban ](data-factory-supported-file-and-compression-formats.md#parquet-format) hello részeiben [Azure Data Factory által támogatott formátumú és tömörítést](data-factory-supported-file-and-compression-formats.md) cikk. <br><br> Ha azt szeretné, hogy toocopy fájlok ",-van" közötti fájlalapú tárolók (bináris másolhatja azokat), hagyja ki a hello `format` mindkét bemeneti és kimeneti adatkészlet-definíciókban szakasz. |Nem |
| **tömörítés** | Adja meg a hello típusát és hello adatok tömörítése szintjét. Támogatott típusok a következők **GZip**, **Deflate**, **BZip2**, és **ZipDeflate**. Támogatott szintek a következők **Optimal** és **leggyorsabb**. További információkért lásd: [Azure Data Factory által támogatott formátumú és tömörítést](data-factory-supported-file-and-compression-formats.md#compression-support). |Nem |

### <a name="hello-partitionedby-property"></a>hello partitionedBy tulajdonság
Megadhatja, hogy a dinamikus **folderPath** és **Fájlnév** hello idősorozat adatokhoz tulajdonságok **partitionedBy** tulajdonság, a Data Factory funkciók és a rendszer változók. További információkért lásd: hello [Azure Data Factory - funkciók és rendszerváltozók](data-factory-functions-variables.md) cikk.


A példában a következő hello `{Slice}` hello érték hello adat-előállító rendszer változó helyére `SliceStart` megadott hello formátumban (`yyyyMMddHH`). hello neve `SliceStart` toohello kezdő időpontjának hello szelet hivatkozik. Hello `folderPath` tulajdonság nem az egyes szeletek, mint az azonos `wikidatagateway/wikisampledataout/2014100103` vagy `wikidatagateway/wikisampledataout/2014100104`.

```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Slice}",
"partitionedBy":
[
    { "name": "Slice", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyyMMddHH" } },
],
```

A következő példa, hello év, hónap, nap és idején hello a `SliceStart` ki kell olvasni a külön változók hello által használt `folderPath` és `fileName` tulajdonságok:
```JSON
"folderPath": "wikidatagateway/wikisampledataout/{Year}/{Month}/{Day}",
"fileName": "{Hour}.csv",
"partitionedBy":
 [
    { "name": "Year", "value": { "type": "DateTime", "date": "SliceStart", "format": "yyyy" } },
    { "name": "Month", "value": { "type": "DateTime", "date": "SliceStart", "format": "MM" } },
    { "name": "Day", "value": { "type": "DateTime", "date": "SliceStart", "format": "dd" } },
    { "name": "Hour", "value": { "type": "DateTime", "date": "SliceStart", "format": "hh" } }
],
```
Idősorozat adatkészletek olvashat, ütemezés és a szeletek, lásd: hello [Azure Data Factory adathalmazok](data-factory-create-datasets.md) és [adat-előállító ütemezés és a végrehajtási](data-factory-scheduling-and-execution.md) cikkeket. 


## <a name="copy-activity-properties"></a>Másolási tevékenység tulajdonságai
Szakaszok és a rendelkezésre álló tevékenységek meghatározó tulajdonságok teljes listáját lásd: hello [folyamatok létrehozása](data-factory-create-pipelines.md) cikk. Például a nevét, leírását, valamint bemeneti és kimeneti táblák és házirend tulajdonságai minden típusú tevékenységek érhetők el.

tulajdonságok érhetők el hello hello **typeProperties** tevékenység szakasz tevékenységek minden típusának függenek. A másolási tevékenységhez változik források és mosdók hello típusától függően.

**AzureDataLakeStoreSource** támogatja a következő tulajdonság a hello hello **typeProperties** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| **rekurzív** |Azt jelzi, hogy hello adatolvasás rekurzív módon hello almappák vagy csak a megadott mappa hello. |TRUE hamis (alapértelmezés) |Nem |


**AzureDataLakeStoreSink** támogatja a következő tulajdonságai a hello hello **typeProperties** szakasz:

| Tulajdonság | Leírás | Megengedett értékek | Szükséges |
| --- | --- | --- | --- |
| **copyBehavior** |Meghatározza a hello másolási viselkedését. |<b>PreserveHierarchy</b>: hello fájl hierarchia hello célmappában megőrzi. hello relatív fájl toosource forrásmappa elérési út azonos toohello fájl tootarget célmappa relatív elérési útját.<br/><br/><b>FlattenHierarchy</b>: hello forrásmappából minden fájl első szintű hello hello célmappában jönnek létre. hello fájljaira jönnek létre automatikusan létrehozott névvel.<br/><br/><b>Mergefiles típusú</b>: összes fájl forrásfájlból hello mappa tooone egyesíti. Ha hello fájl vagy a blob neve meg van adva, a hello egyesített fájl neve az hello megadott név. Ellenkező esetben hello fájlnév rendszer automatikusan elnevezi. |Nem |

### <a name="recursive-and-copybehavior-examples"></a>rekurzív és copyBehavior példák
Ez a szakasz ismerteti a hello viselkedésről hello másolási műveletek kombinációk rekurzív és copybehavior tulajdonságot.

| Rekurzív | copyBehavior | Viselkedésről |
| --- | --- | --- |
| Igaz |preserveHierarchy |A forrásmappa mappa1 a struktúra a következő hello: <br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>hello tároló mappa mappa1 azonos szerkezeti hello forrásaként hello jön létre<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5. |
| Igaz |flattenHierarchy |A forrásmappa mappa1 a struktúra a következő hello: <br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>a következő struktúra hello hello cél mappa1 jön létre: <br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet a file1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File5 |
| Igaz |mergefiles típusú |A forrásmappa mappa1 a struktúra a következő hello: <br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>a következő struktúra hello hello cél mappa1 jön létre: <br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 + File2 + fájl3 + File4 + 5 fájl tartalmát egy fájl automatikusan létrehozott fájlnévvel egyesülnek |
| hamis |preserveHierarchy |A forrásmappa mappa1 a struktúra a következő hello: <br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>a következő struktúra hello hello tároló mappa mappa1 jön létre<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/><br/><br/>Fájl3, File4 és File5 Subfolder1 nem átveszik. |
| hamis |flattenHierarchy |A forrásmappa mappa1 a struktúra a következő hello:<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>a következő struktúra hello hello tároló mappa mappa1 jön létre<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet a file1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;automatikusan létrehozott nevet File2<br/><br/><br/>Fájl3, File4 és File5 Subfolder1 nem átveszik. |
| hamis |mergefiles típusú |A forrásmappa mappa1 a struktúra a következő hello:<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;File1 kiszolgálón<br/>&nbsp;&nbsp;&nbsp;&nbsp;File2<br/>&nbsp;&nbsp;&nbsp;&nbsp;Subfolder1<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Fájl3<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File4<br/>&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;File5<br/><br/>a következő struktúra hello hello tároló mappa mappa1 jön létre<br/><br/>Mappa1<br/>&nbsp;&nbsp;&nbsp;&nbsp;Egy fájl automatikusan létrehozott fájlnévvel egyesített file1 + File2 tartalma. automatikusan létrehozott nevet a file1 kiszolgálón<br/><br/>Fájl3, File4 és File5 Subfolder1 nem átveszik. |

## <a name="supported-file-and-compression-formats"></a>Támogatott formátumú és tömörítés
További információkért lásd: hello [formátumú és tömörítést az Azure Data Factory](data-factory-supported-file-and-compression-formats.md) cikk.

## <a name="json-examples-for-copying-data-tooand-from-data-lake-store"></a>JSON például a Data Lake Store-ból adatokat tooand másolása
a következő példák hello adja meg a minta JSON-definíciók. Hello mezővel használhat ezen minta definíciók toocreate adatcsatorna [Azure-portálon](data-factory-copy-activity-tutorial-using-azure-portal.md), [Visual Studio](data-factory-copy-activity-tutorial-using-visual-studio.md), vagy [Azure PowerShell](data-factory-copy-activity-tutorial-using-powershell.md). Példák megjelenítése hogyan hello toocopy adatok tooand Data Lake Store és az Azure Blob-tárolóból. Azonban az adatok átmásolhatók _közvetlenül_ bármelyik hello források tooany hello támogatott nyelő. További információkért lásd: hello szakasz "támogatott adattárolókhoz és formátumok" hello [adatok áthelyezése a másolási tevékenység segítségével](data-factory-data-movement-activities.md) cikk.  

### <a name="example-copy-data-from-azure-blob-storage-tooazure-data-lake-store"></a>Példa: Adatok másolása az Azure Blob Storage tooAzure Data Lake Store
Ebben a szakaszban hello példakód mutatja:

* A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* A társított szolgáltatás típusa [AzureDataLakeStore](#linked-service-properties).
* Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureDataLakeStore](#dataset-properties).
* A [csővezeték](data-factory-create-pipelines.md) , a másolási tevékenység által használt [BlobSource](data-factory-azure-blob-connector.md#copy-activity-properties) és [AzureDataLakeStoreSink](#copy-activity-properties).

hello példák azt szemléltetik, hogyan idősorozat adatokat az Azure Blob Storage van másolt tooData Lake Store óránként. 

**Azure Storage társított szolgáltatás**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```

**Azure Data Lake Store társított szolgáltatás**

```JSON
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>",
            "subscriptionId": "<subscription of ADLS>",
            "resourceGroupName": "<resource group of ADLS>"
        }
    }
}
```

> [!NOTE]
> További konfigurációs információkért lásd: hello [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.
>

**Azure blobbemeneti adatkészlet**

A következő példa hello, adatok van felvett egy új blobból minden órában (`"frequency": "Hour", "interval": 1`). hello mappa elérési útját és nevét hello blob dinamikusan értékeli ki a rendszer által feldolgozott hello szelet hello kezdési ideje alapján. hello mappa elérési útját használja hello év, hónap és nap részéhez hello kezdési ideje. hello fájlnév hello része kezdési időpont hello óra használja. Hello `"external": true` beállítás arról értesíti az hello tábla külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák hello Data Factory szolgáltatásnak.

```JSON
{
  "name": "AzureBlobInput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ]
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      "externalData": {
        "retryInterval": "00:01:00",
        "retryTimeout": "00:10:00",
        "maximumRetry": 3
      }
    }
  }
}
```

**Azure Data Lake Store kimeneti adatkészlet**

a következő példa másolatok tooData Lake adattárban hello. Új adatok másolásakor tooData Lake Store óránként.

```JSON
{
    "name": "AzureDataLakeStoreOutput",
      "properties": {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/output/"
        },
        "availability": {
              "frequency": "Hour",
              "interval": 1
        }
      }
}
```


**A folyamat egy blob-forrás és a Data Lake Store fogadó másolási tevékenység**

A következő példa hello, hello feldolgozási sor tartalmazza a másolási tevékenység során konfigurált toouse hello bemeneti és kimeneti adathalmazokat. hello másolási tevékenység az ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello `source` típusuk értéke túl`BlobSource`, és hello `sink` típusuk értéke túl`AzureDataLakeStoreSink`.

```json
{  
    "name":"SamplePipeline",
    "properties":
    {  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":
        [  
              {
                "name": "AzureBlobtoDataLake",
                "description": "Copy Activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureBlobInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureDataLakeStoreOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "BlobSource"
                      },
                      "sink": {
                        "type": "AzureDataLakeStoreSink"
                      }
                },
                   "scheduler": {
                      "frequency": "Hour",
                      "interval": 1
                },
                "policy": {
                      "concurrency": 1,
                      "executionPriorityOrder": "OldestFirst",
                      "retry": 0,
                      "timeout": "01:00:00"
                }
              }
        ]
    }
}
```

### <a name="example-copy-data-from-azure-data-lake-store-tooan-azure-blob"></a>Példa: Adatok másolása az Azure Data Lake Store tooan Azure blob
Ebben a szakaszban hello példakód mutatja:

* A társított szolgáltatás típusa [AzureDataLakeStore](#linked-service-properties).
* A társított szolgáltatás típusa [AzureStorage](data-factory-azure-blob-connector.md#linked-service-properties).
* Bemeneti [dataset](data-factory-create-datasets.md) típusú [AzureDataLakeStore](#dataset-properties).
* Egy kimeneti [dataset](data-factory-create-datasets.md) típusú [AzureBlob](data-factory-azure-blob-connector.md#dataset-properties).
* A [csővezeték](data-factory-create-pipelines.md) , a másolási tevékenység által használt [AzureDataLakeStoreSource](#copy-activity-properties) és [BlobSink](data-factory-azure-blob-connector.md#copy-activity-properties).

hello kód átmásolja-idősoros adatok Data Lake Store tooan Azure blob minden órában. 

**Azure Data Lake Store társított szolgáltatás**

```json
{
    "name": "AzureDataLakeStoreLinkedService",
    "properties": {
        "type": "AzureDataLakeStore",
        "typeProperties": {
            "dataLakeStoreUri": "https://<accountname>.azuredatalakestore.net/webhdfs/v1",
            "servicePrincipalId": "<service principal id>",
            "servicePrincipalKey": "<service principal key>",
            "tenant": "<tenant info, e.g. microsoft.onmicrosoft.com>"
        }
    }
}
```

> [!NOTE]
> További konfigurációs információkért lásd: hello [szolgáltatástulajdonságok kapcsolódó](#linked-service-properties) szakasz.
>

**Azure Storage társított szolgáltatás**

```JSON
{
  "name": "StorageLinkedService",
  "properties": {
    "type": "AzureStorage",
    "typeProperties": {
      "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
    }
  }
}
```
**Azure Data Lake bemeneti adatkészlet**

Ebben a példában, beállítása `"external"` túl`true` hello tábla külső toohello adat-előállítót, és egy tevékenység hello adat-előállítóban nem hozzák arról tájékoztatja a hello Data Factory szolgáltatásnak.

```json
{
    "name": "AzureDataLakeStoreInput",
      "properties":
    {
        "type": "AzureDataLakeStore",
        "linkedServiceName": "AzureDataLakeStoreLinkedService",
        "typeProperties": {
            "folderPath": "datalake/input/",
            "fileName": "SearchLog.tsv",
            "format": {
                "type": "TextFormat",
                "rowDelimiter": "\n",
                "columnDelimiter": "\t"
            }
        },
        "external": true,
        "availability": {
            "frequency": "Hour",
              "interval": 1
        },
        "policy": {
              "externalData": {
                "retryInterval": "00:01:00",
                "retryTimeout": "00:10:00",
                "maximumRetry": 3
              }
        }
      }
}
```
**Azure blobkimeneti adatkészlet**

A következő példa hello, adatot ír tooa új blob minden órában (`"frequency": "Hour", "interval": 1`). hello mappa elérési útja hello BLOB dinamikusan értékeli hello szelet által feldolgozott hello kezdési ideje alapján. hello mappa elérési útja hello év, hónap, nap és hello kezdő időpontja óra részét használja.

```JSON
{
  "name": "AzureBlobOutput",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "StorageLinkedService",
    "typeProperties": {
      "folderPath": "mycontainer/myfolder/yearno={Year}/monthno={Month}/dayno={Day}/hourno={Hour}",
      "partitionedBy": [
        {
          "name": "Year",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "yyyy"
          }
        },
        {
          "name": "Month",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "MM"
          }
        },
        {
          "name": "Day",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "dd"
          }
        },
        {
          "name": "Hour",
          "value": {
            "type": "DateTime",
            "date": "SliceStart",
            "format": "HH"
          }
        }
      ],
      "format": {
        "type": "TextFormat",
        "columnDelimiter": "\t",
        "rowDelimiter": "\n"
      }
    },
    "availability": {
      "frequency": "Hour",
      "interval": 1
    }
  }
}
```

**A másolási tevékenység során az Azure Data Lake Store és egy blob fogadó egy folyamaton belül**

A következő példa hello, hello feldolgozási sor tartalmazza a másolási tevékenység során konfigurált toouse hello bemeneti és kimeneti adathalmazokat. hello másolási tevékenység az ütemezett toorun óránként. Hello adatcsatorna JSON-definícióból, hello `source` típusuk értéke túl`AzureDataLakeStoreSource`, és hello `sink` típusuk értéke túl`BlobSink`.

```json
{  
    "name":"SamplePipeline",
    "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline for copy activity",
        "activities":[  
              {
                "name": "AzureDakeLaketoBlob",
                "description": "copy activity",
                "type": "Copy",
                "inputs": [
                  {
                    "name": "AzureDataLakeStoreInput"
                  }
                ],
                "outputs": [
                  {
                    "name": "AzureBlobOutput"
                  }
                ],
                "typeProperties": {
                    "source": {
                        "type": "AzureDataLakeStoreSource",
                      },
                      "sink": {
                        "type": "BlobSink"
                      }
                },
                   "scheduler": {
                      "frequency": "Hour",
                      "interval": 1
                },
                "policy": {
                      "concurrency": 1,
                      "executionPriorityOrder": "OldestFirst",
                      "retry": 0,
                      "timeout": "01:00:00"
                }
              }
         ]
    }
}
```

Hello másolási tevékenység definíciójának hello forrás adatkészlet toocolumns hello fogadó adatkészlet oszlopokat is leképezheti. További információkért lásd: [Azure Data Factory dataset oszlopai leképezési](data-factory-map-columns.md).

## <a name="performance-and-tuning"></a>Teljesítmény és finomhangolás
Másolási tevékenység teljesítményét befolyásoló tényezők hello kapcsolatos toolearn és hogyan toooptimize, lásd: hello [másolási tevékenység teljesítmény- és hangolási útmutató](data-factory-copy-activity-performance.md) cikk.
