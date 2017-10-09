---
title: "aaaAzure funkciók külső táblakötéssel (előzetes verzió) |} Microsoft Docs"
description: "A külső tábla kötések az Azure Functions használatával"
services: functions
documentationcenter: 
author: alexkarcher-msft
manager: erikre
editor: 
ms.assetid: 
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 04/12/2017
ms.author: alkarche
ms.openlocfilehash: bf19d7d377232edc91087d5f4110602bb82c67ef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-functions-external-table-binding-preview"></a>Az Azure Functions külső táblakötéssel (előzetes verzió)
Ez a cikk bemutatja, hogyan toomanipulate táblázatos adatok SaaS szolgáltatók (például a Sharepoint, a Dynamics) olyan beépített kötések a függvényen belül. Az Azure Functions bemeneti és kimeneti kötések támogatja a külső táblákhoz.

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="api-connections"></a>API-kapcsolatok

Tábla kötések külső API kapcsolatok tooauthenticate 3. fél Szolgáltatottszoftver-szolgáltatókkal használja ki. 

A kötés hozzárendelésekor új API-kapcsolatot létrehozni, vagy belül hello meglévő API-kapcsolat használata ugyanabban az erőforráscsoportban

### <a name="supported-api-connections-tables"></a>Támogatott API-kapcsolatok (tábla) s

|összekötő|Eseményindító|Input (Bemenet)|Kimenet|
|:-----|:---:|:---:|:---:|
|[DB2](https://docs.microsoft.com/azure/connectors/connectors-create-api-db2)||x|x
|[Dynamics 365 műveleteihez](https://ax.help.dynamics.com/wiki/install-and-configure-dynamics-365-for-operations-warehousing/)||x|x
|[Dynamics 365](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[Dynamics NAV](https://msdn.microsoft.com/library/gg481835.aspx)||x|x
|[Google Táblázatok](https://docs.microsoft.com/azure/connectors/connectors-create-api-googledrive)||x|x
|[Informix](https://docs.microsoft.com/azure/connectors/connectors-create-api-informix)||x|x
|[Dynamics 365 a Pénzügy](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||x|x
|[MySQL](https://docs.microsoft.com/azure/store-php-create-mysql-database)||x|x
|[Oracle Database](https://docs.microsoft.com/azure/connectors/connectors-create-api-oracledatabase)||x|x
|[Közös adatszolgáltatás](https://docs.microsoft.com/common-data-service/entity-reference/introduction)||x|x
|[Salesforce](https://docs.microsoft.com/azure/connectors/connectors-create-api-salesforce)||x|x
|[SharePoint](https://docs.microsoft.com/azure/connectors/connectors-create-api-sharepointonline)||x|x
|[SQL Server](https://docs.microsoft.com/azure/connectors/connectors-create-api-sqlazure)||x|x
|[Teradata](http://www.teradata.com/products-and-services/azure/products/)||x|x
|UserVoice||x|x
|Zendesk||x|x


> [!NOTE]
> Külső tábla kapcsolatok is használható a [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)

### <a name="creating-an-api-connection-step-by-step"></a>Az API-kapcsolat létrehozása: lépésről lépésre

1. Hozzon létre egy függvényt > egyéni függvény ![egyéni függvény létrehozása](./media/functions-bindings-storage-table/create-custom-function.jpg)
1. A forgatókönyv `Experimental`  >  `ExternalTable-CSharp` sablon > hozzon létre egy új `External Table connection` 
 ![bemeneti sablon kiválasztása](./media/functions-bindings-storage-table/create-template-table.jpg)
1. Válassza ki azt a Szolgáltatottszoftver-szolgáltatót > Válassza ki vagy hozzon létre kapcsolatot ![konfigurálása Szolgáltatottszoftver-kapcsolat](./media/functions-bindings-storage-table/authorize-API-connection.jpg)
1. Válassza ki az API-kapcsolatot > hello függvény létrehozása ![tábla függvény létrehozása](./media/functions-bindings-storage-table/table-template-options.jpg)
1. Válassza ki`Integrate` > `External Table`
    1. A céloldali tábla hello kapcsolat toouse konfigurálása Ezek a beállítások nagyon Szolgáltatottszoftver-szolgáltatók között lesz. -E az alábbi vázlat [adatforrás-beállítások](#datasourcesettings)
![konfigurálása tábla](./media/functions-bindings-storage-table/configure-API-connection.jpg)

## <a name="usage"></a>Használat

Ebben a példában a "Ügyfél" nevű azonosítót, Utónév és Vezetéknév oszlopokkal tooa táblázat kapcsolatot. hello kód hello Contact entitásokat hello táblázat sorolja fel, és a naplók hello vezeték- és keresztneveket.

### <a name="bindings"></a>Kötések
```json
{
  "bindings": [
    {
      "type": "manualTrigger",
      "direction": "in",
      "name": "input"
    },
    {
      "type": "apiHubTable",
      "direction": "in",
      "name": "table",
      "connection": "ConnectionAppSettingsKey",
      "dataSetName": "default",
      "tableName": "Contact",
      "entityId": "",
    }
  ],
  "disabled": false
}
```
`entityId`a tábla kötések üresnek kell lennie.

`ConnectionAppSettingsKey`hello Alkalmazásbeállítás hello API kapcsolati karakterlánc tároló azonosítja. hello Alkalmazásbeállítás automatikusan jön létre az API-k felvételekor hello kapcsolat integrálható a felhasználói felület.

A táblázatos összekötő adatkészletek biztosít, és minden egyes táblát tartalmaz. hello hello alapértelmezett adatkészlet értéke "alapértelmezett". a DataSet adatkészlet és a táblázat a különböző Szolgáltatottszoftver-szolgáltatók hello címeinek az alábbiak:

|összekötő|Adatkészlet|Tábla|
|:-----|:---|:---| 
|**SharePoint**|Helykiszolgáló|SharePoint-lista
|**SQL**|Adatbázis|Tábla 
|**Google lap**|Táblázat|Munkalap 
|**Excel**|Excel-fájl|Lap 

<!--
See hello language-specific sample that copies hello input file toohello output file.

* [C#](#incsharp)
* [Node.js](#innodejs)

-->
<a name="incsharp"></a>

### <a name="usage-in-c"></a>A C# szintaxis #

```cs
#r "Microsoft.Azure.ApiHub.Sdk"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.ApiHub;

//Variable name must match column type
//Variable type is dynamically bound toohello incoming data
public class Contact
{
    public string Id { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
}

public static async Task Run(string input, ITable<Contact> table, TraceWriter log)
{
    //Iterate over every value in hello source table
    ContinuationToken continuationToken = null;
    do
    {   
        //retreive table values
        var contactsSegment = await table.ListEntitiesAsync(
            continuationToken: continuationToken);

        foreach (var contact in contactsSegment.Items)
        {   
            log.Info(string.Format("{0} {1}", contact.FirstName, contact.LastName));
        }

        continuationToken = contactsSegment.ContinuationToken;
    }
    while (continuationToken != null);
}
```

<!--
<a name="innodejs"></a>

### Usage in Node.js

```javascript
module.exports = function(context) {
    context.log('Node.js Queue trigger function processed', context.bindings.myQueueItem);
    context.bindings.myOutputFile = context.bindings.myInputFile;
    context.done();
};
```
-->
<a name="datasourcesettings"></a>
##Az adatforrás-beállítások

### <a name="sql-server"></a>SQL Server

parancsfájl toocreate hello és tölthet hello ügyfél tábla nem éri el. dataSetName az "alapértelmezett".

```sql
CREATE TABLE Contact
(
    Id int NOT NULL,
    LastName varchar(20) NOT NULL,
    FirstName varchar(20) NOT NULL,
    CONSTRAINT PK_Contact_Id PRIMARY KEY (Id)
)
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (1, 'Bitt', 'Prad') 
GO
INSERT INTO Contact(Id, LastName, FirstName)
     VALUES (2, 'Glooney', 'Ceorge') 
GO
```

### <a name="google-sheets"></a>Google Táblázatok
A Google Docs, hozzon létre egy táblázatot nevű `Contact`. hello összekötő hello számolótábla megjelenítési név nem használható. hello belső nevét (félkövér) kell toobe használt dataSetName, mint például: `docs.google.com/spreadsheets/d/`  **`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`**  hello oszlopnevek hozzáadása `Id`, `LastName`, `FirstName` toohello első sor, majd az adatok feltöltése további sorokat.

### <a name="salesforce"></a>Salesforce
dataSetName az "alapértelmezett".

## <a name="next-steps"></a>Következő lépések
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
