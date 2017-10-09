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
# <a name="azure-functions-external-table-binding-preview"></a><span data-ttu-id="558ec-103">Az Azure Functions külső táblakötéssel (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="558ec-103">Azure Functions External Table binding (Preview)</span></span>
<span data-ttu-id="558ec-104">Ez a cikk bemutatja, hogyan toomanipulate táblázatos adatok SaaS szolgáltatók (például a Sharepoint, a Dynamics) olyan beépített kötések a függvényen belül.</span><span class="sxs-lookup"><span data-stu-id="558ec-104">This article shows how toomanipulate tabular data on SaaS providers (e.g. Sharepoint, Dynamics) within your function with built-in bindings.</span></span> <span data-ttu-id="558ec-105">Az Azure Functions bemeneti és kimeneti kötések támogatja a külső táblákhoz.</span><span class="sxs-lookup"><span data-stu-id="558ec-105">Azure Functions supports input, and output bindings for external tables.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="api-connections"></a><span data-ttu-id="558ec-106">API-kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="558ec-106">API Connections</span></span>

<span data-ttu-id="558ec-107">Tábla kötések külső API kapcsolatok tooauthenticate 3. fél Szolgáltatottszoftver-szolgáltatókkal használja ki.</span><span class="sxs-lookup"><span data-stu-id="558ec-107">Table bindings leverage external API connections tooauthenticate with 3rd party SaaS providers.</span></span> 

<span data-ttu-id="558ec-108">A kötés hozzárendelésekor új API-kapcsolatot létrehozni, vagy belül hello meglévő API-kapcsolat használata ugyanabban az erőforráscsoportban</span><span class="sxs-lookup"><span data-stu-id="558ec-108">When assigning a binding you can either create a new API connection or use an existing API connection within hello same resource group</span></span>

### <a name="supported-api-connections-tables"></a><span data-ttu-id="558ec-109">Támogatott API-kapcsolatok (tábla) s</span><span class="sxs-lookup"><span data-stu-id="558ec-109">Supported API Connections (Table)s</span></span>

|<span data-ttu-id="558ec-110">összekötő</span><span class="sxs-lookup"><span data-stu-id="558ec-110">Connector</span></span>|<span data-ttu-id="558ec-111">Eseményindító</span><span class="sxs-lookup"><span data-stu-id="558ec-111">Trigger</span></span>|<span data-ttu-id="558ec-112">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="558ec-112">Input</span></span>|<span data-ttu-id="558ec-113">Kimenet</span><span class="sxs-lookup"><span data-stu-id="558ec-113">Output</span></span>|
|:-----|:---:|:---:|:---:|
|[<span data-ttu-id="558ec-114">DB2</span><span class="sxs-lookup"><span data-stu-id="558ec-114">DB2</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-db2)||<span data-ttu-id="558ec-115">x</span><span class="sxs-lookup"><span data-stu-id="558ec-115">x</span></span>|<span data-ttu-id="558ec-116">x</span><span class="sxs-lookup"><span data-stu-id="558ec-116">x</span></span>
|[<span data-ttu-id="558ec-117">Dynamics 365 műveleteihez</span><span class="sxs-lookup"><span data-stu-id="558ec-117">Dynamics 365 for Operations</span></span>](https://ax.help.dynamics.com/wiki/install-and-configure-dynamics-365-for-operations-warehousing/)||<span data-ttu-id="558ec-118">x</span><span class="sxs-lookup"><span data-stu-id="558ec-118">x</span></span>|<span data-ttu-id="558ec-119">x</span><span class="sxs-lookup"><span data-stu-id="558ec-119">x</span></span>
|[<span data-ttu-id="558ec-120">Dynamics 365</span><span class="sxs-lookup"><span data-stu-id="558ec-120">Dynamics 365</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||<span data-ttu-id="558ec-121">x</span><span class="sxs-lookup"><span data-stu-id="558ec-121">x</span></span>|<span data-ttu-id="558ec-122">x</span><span class="sxs-lookup"><span data-stu-id="558ec-122">x</span></span>
|[<span data-ttu-id="558ec-123">Dynamics NAV</span><span class="sxs-lookup"><span data-stu-id="558ec-123">Dynamics NAV</span></span>](https://msdn.microsoft.com/library/gg481835.aspx)||<span data-ttu-id="558ec-124">x</span><span class="sxs-lookup"><span data-stu-id="558ec-124">x</span></span>|<span data-ttu-id="558ec-125">x</span><span class="sxs-lookup"><span data-stu-id="558ec-125">x</span></span>
|[<span data-ttu-id="558ec-126">Google Táblázatok</span><span class="sxs-lookup"><span data-stu-id="558ec-126">Google Sheets</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-googledrive)||<span data-ttu-id="558ec-127">x</span><span class="sxs-lookup"><span data-stu-id="558ec-127">x</span></span>|<span data-ttu-id="558ec-128">x</span><span class="sxs-lookup"><span data-stu-id="558ec-128">x</span></span>
|[<span data-ttu-id="558ec-129">Informix</span><span class="sxs-lookup"><span data-stu-id="558ec-129">Informix</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-informix)||<span data-ttu-id="558ec-130">x</span><span class="sxs-lookup"><span data-stu-id="558ec-130">x</span></span>|<span data-ttu-id="558ec-131">x</span><span class="sxs-lookup"><span data-stu-id="558ec-131">x</span></span>
|[<span data-ttu-id="558ec-132">Dynamics 365 a Pénzügy</span><span class="sxs-lookup"><span data-stu-id="558ec-132">Dynamics 365 for Financials</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||<span data-ttu-id="558ec-133">x</span><span class="sxs-lookup"><span data-stu-id="558ec-133">x</span></span>|<span data-ttu-id="558ec-134">x</span><span class="sxs-lookup"><span data-stu-id="558ec-134">x</span></span>
|[<span data-ttu-id="558ec-135">MySQL</span><span class="sxs-lookup"><span data-stu-id="558ec-135">MySQL</span></span>](https://docs.microsoft.com/azure/store-php-create-mysql-database)||<span data-ttu-id="558ec-136">x</span><span class="sxs-lookup"><span data-stu-id="558ec-136">x</span></span>|<span data-ttu-id="558ec-137">x</span><span class="sxs-lookup"><span data-stu-id="558ec-137">x</span></span>
|[<span data-ttu-id="558ec-138">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="558ec-138">Oracle Database</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-oracledatabase)||<span data-ttu-id="558ec-139">x</span><span class="sxs-lookup"><span data-stu-id="558ec-139">x</span></span>|<span data-ttu-id="558ec-140">x</span><span class="sxs-lookup"><span data-stu-id="558ec-140">x</span></span>
|[<span data-ttu-id="558ec-141">Közös adatszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="558ec-141">Common Data Service</span></span>](https://docs.microsoft.com/common-data-service/entity-reference/introduction)||<span data-ttu-id="558ec-142">x</span><span class="sxs-lookup"><span data-stu-id="558ec-142">x</span></span>|<span data-ttu-id="558ec-143">x</span><span class="sxs-lookup"><span data-stu-id="558ec-143">x</span></span>
|[<span data-ttu-id="558ec-144">Salesforce</span><span class="sxs-lookup"><span data-stu-id="558ec-144">Salesforce</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-salesforce)||<span data-ttu-id="558ec-145">x</span><span class="sxs-lookup"><span data-stu-id="558ec-145">x</span></span>|<span data-ttu-id="558ec-146">x</span><span class="sxs-lookup"><span data-stu-id="558ec-146">x</span></span>
|[<span data-ttu-id="558ec-147">SharePoint</span><span class="sxs-lookup"><span data-stu-id="558ec-147">SharePoint</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-sharepointonline)||<span data-ttu-id="558ec-148">x</span><span class="sxs-lookup"><span data-stu-id="558ec-148">x</span></span>|<span data-ttu-id="558ec-149">x</span><span class="sxs-lookup"><span data-stu-id="558ec-149">x</span></span>
|[<span data-ttu-id="558ec-150">SQL Server</span><span class="sxs-lookup"><span data-stu-id="558ec-150">SQL Server</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-sqlazure)||<span data-ttu-id="558ec-151">x</span><span class="sxs-lookup"><span data-stu-id="558ec-151">x</span></span>|<span data-ttu-id="558ec-152">x</span><span class="sxs-lookup"><span data-stu-id="558ec-152">x</span></span>
|[<span data-ttu-id="558ec-153">Teradata</span><span class="sxs-lookup"><span data-stu-id="558ec-153">Teradata</span></span>](http://www.teradata.com/products-and-services/azure/products/)||<span data-ttu-id="558ec-154">x</span><span class="sxs-lookup"><span data-stu-id="558ec-154">x</span></span>|<span data-ttu-id="558ec-155">x</span><span class="sxs-lookup"><span data-stu-id="558ec-155">x</span></span>
|<span data-ttu-id="558ec-156">UserVoice</span><span class="sxs-lookup"><span data-stu-id="558ec-156">UserVoice</span></span>||<span data-ttu-id="558ec-157">x</span><span class="sxs-lookup"><span data-stu-id="558ec-157">x</span></span>|<span data-ttu-id="558ec-158">x</span><span class="sxs-lookup"><span data-stu-id="558ec-158">x</span></span>
|<span data-ttu-id="558ec-159">Zendesk</span><span class="sxs-lookup"><span data-stu-id="558ec-159">Zendesk</span></span>||<span data-ttu-id="558ec-160">x</span><span class="sxs-lookup"><span data-stu-id="558ec-160">x</span></span>|<span data-ttu-id="558ec-161">x</span><span class="sxs-lookup"><span data-stu-id="558ec-161">x</span></span>


> [!NOTE]
> <span data-ttu-id="558ec-162">Külső tábla kapcsolatok is használható a [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)</span><span class="sxs-lookup"><span data-stu-id="558ec-162">External Table connections can also be used in [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)</span></span>

### <a name="creating-an-api-connection-step-by-step"></a><span data-ttu-id="558ec-163">Az API-kapcsolat létrehozása: lépésről lépésre</span><span class="sxs-lookup"><span data-stu-id="558ec-163">Creating an API connection: step by step</span></span>

1. <span data-ttu-id="558ec-164">Hozzon létre egy függvényt > egyéni függvény ![egyéni függvény létrehozása](./media/functions-bindings-storage-table/create-custom-function.jpg)</span><span class="sxs-lookup"><span data-stu-id="558ec-164">Create a function > custom function ![Create a custom function](./media/functions-bindings-storage-table/create-custom-function.jpg)</span></span>
1. <span data-ttu-id="558ec-165">A forgatókönyv `Experimental`  >  `ExternalTable-CSharp` sablon > hozzon létre egy új `External Table connection` 
 ![bemeneti sablon kiválasztása](./media/functions-bindings-storage-table/create-template-table.jpg)</span><span class="sxs-lookup"><span data-stu-id="558ec-165">Scenario `Experimental` > `ExternalTable-CSharp` template > Create a new `External Table connection`
![Choose table input template](./media/functions-bindings-storage-table/create-template-table.jpg)</span></span>
1. <span data-ttu-id="558ec-166">Válassza ki azt a Szolgáltatottszoftver-szolgáltatót > Válassza ki vagy hozzon létre kapcsolatot ![konfigurálása Szolgáltatottszoftver-kapcsolat](./media/functions-bindings-storage-table/authorize-API-connection.jpg)</span><span class="sxs-lookup"><span data-stu-id="558ec-166">Choose your SaaS provider > choose/create a connection ![Configure SaaS connection](./media/functions-bindings-storage-table/authorize-API-connection.jpg)</span></span>
1. <span data-ttu-id="558ec-167">Válassza ki az API-kapcsolatot > hello függvény létrehozása ![tábla függvény létrehozása](./media/functions-bindings-storage-table/table-template-options.jpg)</span><span class="sxs-lookup"><span data-stu-id="558ec-167">Select your API connection > create hello function ![Create table function](./media/functions-bindings-storage-table/table-template-options.jpg)</span></span>
1. <span data-ttu-id="558ec-168">Válassza ki`Integrate` > `External Table`</span><span class="sxs-lookup"><span data-stu-id="558ec-168">Select `Integrate` > `External Table`</span></span>
    1. <span data-ttu-id="558ec-169">A céloldali tábla hello kapcsolat toouse konfigurálása</span><span class="sxs-lookup"><span data-stu-id="558ec-169">Configure hello connection toouse your target table.</span></span> <span data-ttu-id="558ec-170">Ezek a beállítások nagyon Szolgáltatottszoftver-szolgáltatók között lesz.</span><span class="sxs-lookup"><span data-stu-id="558ec-170">These settings will very between SaaS providers.</span></span> <span data-ttu-id="558ec-171">-E az alábbi vázlat [adatforrás-beállítások](#datasourcesettings)
![konfigurálása tábla](./media/functions-bindings-storage-table/configure-API-connection.jpg)</span><span class="sxs-lookup"><span data-stu-id="558ec-171">They are outline below in [data source settings](#datasourcesettings)
![Configure table](./media/functions-bindings-storage-table/configure-API-connection.jpg)</span></span>

## <a name="usage"></a><span data-ttu-id="558ec-172">Használat</span><span class="sxs-lookup"><span data-stu-id="558ec-172">Usage</span></span>

<span data-ttu-id="558ec-173">Ebben a példában a "Ügyfél" nevű azonosítót, Utónév és Vezetéknév oszlopokkal tooa táblázat kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="558ec-173">This example connects tooa table named "Contact" with Id, LastName, and FirstName columns.</span></span> <span data-ttu-id="558ec-174">hello kód hello Contact entitásokat hello táblázat sorolja fel, és a naplók hello vezeték- és keresztneveket.</span><span class="sxs-lookup"><span data-stu-id="558ec-174">hello code lists hello Contact entities in hello table and logs hello first and last names.</span></span>

### <a name="bindings"></a><span data-ttu-id="558ec-175">Kötések</span><span class="sxs-lookup"><span data-stu-id="558ec-175">Bindings</span></span>
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
<span data-ttu-id="558ec-176">`entityId`a tábla kötések üresnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="558ec-176">`entityId` must be empty for table bindings.</span></span>

<span data-ttu-id="558ec-177">`ConnectionAppSettingsKey`hello Alkalmazásbeállítás hello API kapcsolati karakterlánc tároló azonosítja.</span><span class="sxs-lookup"><span data-stu-id="558ec-177">`ConnectionAppSettingsKey` identifies hello app setting that stores hello API connection string.</span></span> <span data-ttu-id="558ec-178">hello Alkalmazásbeállítás automatikusan jön létre az API-k felvételekor hello kapcsolat integrálható a felhasználói felület.</span><span class="sxs-lookup"><span data-stu-id="558ec-178">hello app setting is created automatically when you add an API connection in hello integrate UI.</span></span>

<span data-ttu-id="558ec-179">A táblázatos összekötő adatkészletek biztosít, és minden egyes táblát tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="558ec-179">A tabular connector provides data sets, and each data set contains tables.</span></span> <span data-ttu-id="558ec-180">hello hello alapértelmezett adatkészlet értéke "alapértelmezett".</span><span class="sxs-lookup"><span data-stu-id="558ec-180">hello name of hello default data set is “default.”</span></span> <span data-ttu-id="558ec-181">a DataSet adatkészlet és a táblázat a különböző Szolgáltatottszoftver-szolgáltatók hello címeinek az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="558ec-181">hello titles for a dataset and a table in various SaaS providers are listed below:</span></span>

|<span data-ttu-id="558ec-182">összekötő</span><span class="sxs-lookup"><span data-stu-id="558ec-182">Connector</span></span>|<span data-ttu-id="558ec-183">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="558ec-183">Dataset</span></span>|<span data-ttu-id="558ec-184">Tábla</span><span class="sxs-lookup"><span data-stu-id="558ec-184">Table</span></span>|
|:-----|:---|:---| 
|<span data-ttu-id="558ec-185">**SharePoint**</span><span class="sxs-lookup"><span data-stu-id="558ec-185">**SharePoint**</span></span>|<span data-ttu-id="558ec-186">Helykiszolgáló</span><span class="sxs-lookup"><span data-stu-id="558ec-186">Site</span></span>|<span data-ttu-id="558ec-187">SharePoint-lista</span><span class="sxs-lookup"><span data-stu-id="558ec-187">SharePoint List</span></span>
|<span data-ttu-id="558ec-188">**SQL**</span><span class="sxs-lookup"><span data-stu-id="558ec-188">**SQL**</span></span>|<span data-ttu-id="558ec-189">Adatbázis</span><span class="sxs-lookup"><span data-stu-id="558ec-189">Database</span></span>|<span data-ttu-id="558ec-190">Tábla</span><span class="sxs-lookup"><span data-stu-id="558ec-190">Table</span></span> 
|<span data-ttu-id="558ec-191">**Google lap**</span><span class="sxs-lookup"><span data-stu-id="558ec-191">**Google Sheet**</span></span>|<span data-ttu-id="558ec-192">Táblázat</span><span class="sxs-lookup"><span data-stu-id="558ec-192">Spreadsheet</span></span>|<span data-ttu-id="558ec-193">Munkalap</span><span class="sxs-lookup"><span data-stu-id="558ec-193">Worksheet</span></span> 
|<span data-ttu-id="558ec-194">**Excel**</span><span class="sxs-lookup"><span data-stu-id="558ec-194">**Excel**</span></span>|<span data-ttu-id="558ec-195">Excel-fájl</span><span class="sxs-lookup"><span data-stu-id="558ec-195">Excel file</span></span>|<span data-ttu-id="558ec-196">Lap</span><span class="sxs-lookup"><span data-stu-id="558ec-196">Sheet</span></span> 

<!--
See hello language-specific sample that copies hello input file toohello output file.

* [C#](#incsharp)
* [Node.js](#innodejs)

-->
<a name="incsharp"></a>

### <a name="usage-in-c"></a><span data-ttu-id="558ec-197">A C# szintaxis</span><span class="sxs-lookup"><span data-stu-id="558ec-197">Usage in C#</span></span> #

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

<span data-ttu-id="558ec-198"><!--
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
##Az adatforrás-beállítások</span><span class="sxs-lookup"><span data-stu-id="558ec-198"><!--
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
## Data Source Settings</span></span>

### <a name="sql-server"></a><span data-ttu-id="558ec-199">SQL Server</span><span class="sxs-lookup"><span data-stu-id="558ec-199">SQL Server</span></span>

<span data-ttu-id="558ec-200">parancsfájl toocreate hello és tölthet hello ügyfél tábla nem éri el.</span><span class="sxs-lookup"><span data-stu-id="558ec-200">hello script toocreate and populate hello Contact table is below.</span></span> <span data-ttu-id="558ec-201">dataSetName az "alapértelmezett".</span><span class="sxs-lookup"><span data-stu-id="558ec-201">dataSetName is “default.”</span></span>

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

### <a name="google-sheets"></a><span data-ttu-id="558ec-202">Google Táblázatok</span><span class="sxs-lookup"><span data-stu-id="558ec-202">Google Sheets</span></span>
<span data-ttu-id="558ec-203">A Google Docs, hozzon létre egy táblázatot nevű `Contact`.</span><span class="sxs-lookup"><span data-stu-id="558ec-203">In Google Docs, create a spreadsheet with a worksheet named `Contact`.</span></span> <span data-ttu-id="558ec-204">hello összekötő hello számolótábla megjelenítési név nem használható.</span><span class="sxs-lookup"><span data-stu-id="558ec-204">hello connector cannot use hello spreadsheet display name.</span></span> <span data-ttu-id="558ec-205">hello belső nevét (félkövér) kell toobe használt dataSetName, mint például: `docs.google.com/spreadsheets/d/`  **`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`**  hello oszlopnevek hozzáadása `Id`, `LastName`, `FirstName` toohello első sor, majd az adatok feltöltése további sorokat.</span><span class="sxs-lookup"><span data-stu-id="558ec-205">hello internal name (in bold) needs toobe used as dataSetName, for example: `docs.google.com/spreadsheets/d/`**`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`** Add hello column names `Id`, `LastName`, `FirstName` toohello first row, then populate data on subsequent rows.</span></span>

### <a name="salesforce"></a><span data-ttu-id="558ec-206">Salesforce</span><span class="sxs-lookup"><span data-stu-id="558ec-206">Salesforce</span></span>
<span data-ttu-id="558ec-207">dataSetName az "alapértelmezett".</span><span class="sxs-lookup"><span data-stu-id="558ec-207">dataSetName is “default.”</span></span>

## <a name="next-steps"></a><span data-ttu-id="558ec-208">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="558ec-208">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
