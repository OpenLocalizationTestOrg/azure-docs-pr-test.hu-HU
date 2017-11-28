---
title: "Az Azure Functions külső táblakötéssel (előzetes verzió) |} Microsoft Docs"
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
ms.openlocfilehash: 716438e5ea490f6716999813112305499dbe61a8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-functions-external-table-binding-preview"></a><span data-ttu-id="4f172-103">Az Azure Functions külső táblakötéssel (előzetes verzió)</span><span class="sxs-lookup"><span data-stu-id="4f172-103">Azure Functions External Table binding (Preview)</span></span>
<span data-ttu-id="4f172-104">Ez a cikk bemutatja, hogyan kezelheti a Szolgáltatottszoftver-szolgáltatók (például a Sharepoint, a Dynamics) táblázatos adatok a függvényen belül beépített definiált kötésekkel.</span><span class="sxs-lookup"><span data-stu-id="4f172-104">This article shows how to manipulate tabular data on SaaS providers (e.g. Sharepoint, Dynamics) within your function with built-in bindings.</span></span> <span data-ttu-id="4f172-105">Az Azure Functions bemeneti és kimeneti kötések támogatja a külső táblákhoz.</span><span class="sxs-lookup"><span data-stu-id="4f172-105">Azure Functions supports input, and output bindings for external tables.</span></span>

[!INCLUDE [intro](../../includes/functions-bindings-intro.md)]

## <a name="api-connections"></a><span data-ttu-id="4f172-106">API-kapcsolatok</span><span class="sxs-lookup"><span data-stu-id="4f172-106">API Connections</span></span>

<span data-ttu-id="4f172-107">Tábla kötések használja ki a 3. fél Szolgáltatottszoftver-szolgáltatók való hitelesítéshez szükséges külső API-kapcsolatokat.</span><span class="sxs-lookup"><span data-stu-id="4f172-107">Table bindings leverage external API connections to authenticate with 3rd party SaaS providers.</span></span> 

<span data-ttu-id="4f172-108">A kötés hozzárendelésekor új API-kapcsolatot létrehozni, vagy belül ugyanabban az erőforráscsoportban meglévő API-kapcsolat használata</span><span class="sxs-lookup"><span data-stu-id="4f172-108">When assigning a binding you can either create a new API connection or use an existing API connection within the same resource group</span></span>

### <a name="supported-api-connections-tables"></a><span data-ttu-id="4f172-109">Támogatott API-kapcsolatok (tábla) s</span><span class="sxs-lookup"><span data-stu-id="4f172-109">Supported API Connections (Table)s</span></span>

|<span data-ttu-id="4f172-110">összekötő</span><span class="sxs-lookup"><span data-stu-id="4f172-110">Connector</span></span>|<span data-ttu-id="4f172-111">Eseményindító</span><span class="sxs-lookup"><span data-stu-id="4f172-111">Trigger</span></span>|<span data-ttu-id="4f172-112">Input (Bemenet)</span><span class="sxs-lookup"><span data-stu-id="4f172-112">Input</span></span>|<span data-ttu-id="4f172-113">Kimenet</span><span class="sxs-lookup"><span data-stu-id="4f172-113">Output</span></span>|
|:-----|:---:|:---:|:---:|
|[<span data-ttu-id="4f172-114">DB2</span><span class="sxs-lookup"><span data-stu-id="4f172-114">DB2</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-db2)||<span data-ttu-id="4f172-115">x</span><span class="sxs-lookup"><span data-stu-id="4f172-115">x</span></span>|<span data-ttu-id="4f172-116">x</span><span class="sxs-lookup"><span data-stu-id="4f172-116">x</span></span>
|[<span data-ttu-id="4f172-117">Dynamics 365 műveleteihez</span><span class="sxs-lookup"><span data-stu-id="4f172-117">Dynamics 365 for Operations</span></span>](https://ax.help.dynamics.com/wiki/install-and-configure-dynamics-365-for-operations-warehousing/)||<span data-ttu-id="4f172-118">x</span><span class="sxs-lookup"><span data-stu-id="4f172-118">x</span></span>|<span data-ttu-id="4f172-119">x</span><span class="sxs-lookup"><span data-stu-id="4f172-119">x</span></span>
|[<span data-ttu-id="4f172-120">Dynamics 365</span><span class="sxs-lookup"><span data-stu-id="4f172-120">Dynamics 365</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||<span data-ttu-id="4f172-121">x</span><span class="sxs-lookup"><span data-stu-id="4f172-121">x</span></span>|<span data-ttu-id="4f172-122">x</span><span class="sxs-lookup"><span data-stu-id="4f172-122">x</span></span>
|[<span data-ttu-id="4f172-123">Dynamics NAV</span><span class="sxs-lookup"><span data-stu-id="4f172-123">Dynamics NAV</span></span>](https://msdn.microsoft.com/library/gg481835.aspx)||<span data-ttu-id="4f172-124">x</span><span class="sxs-lookup"><span data-stu-id="4f172-124">x</span></span>|<span data-ttu-id="4f172-125">x</span><span class="sxs-lookup"><span data-stu-id="4f172-125">x</span></span>
|[<span data-ttu-id="4f172-126">Google Táblázatok</span><span class="sxs-lookup"><span data-stu-id="4f172-126">Google Sheets</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-googledrive)||<span data-ttu-id="4f172-127">x</span><span class="sxs-lookup"><span data-stu-id="4f172-127">x</span></span>|<span data-ttu-id="4f172-128">x</span><span class="sxs-lookup"><span data-stu-id="4f172-128">x</span></span>
|[<span data-ttu-id="4f172-129">Informix</span><span class="sxs-lookup"><span data-stu-id="4f172-129">Informix</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-informix)||<span data-ttu-id="4f172-130">x</span><span class="sxs-lookup"><span data-stu-id="4f172-130">x</span></span>|<span data-ttu-id="4f172-131">x</span><span class="sxs-lookup"><span data-stu-id="4f172-131">x</span></span>
|[<span data-ttu-id="4f172-132">Dynamics 365 a Pénzügy</span><span class="sxs-lookup"><span data-stu-id="4f172-132">Dynamics 365 for Financials</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-crmonline)||<span data-ttu-id="4f172-133">x</span><span class="sxs-lookup"><span data-stu-id="4f172-133">x</span></span>|<span data-ttu-id="4f172-134">x</span><span class="sxs-lookup"><span data-stu-id="4f172-134">x</span></span>
|[<span data-ttu-id="4f172-135">MySQL</span><span class="sxs-lookup"><span data-stu-id="4f172-135">MySQL</span></span>](https://docs.microsoft.com/azure/store-php-create-mysql-database)||<span data-ttu-id="4f172-136">x</span><span class="sxs-lookup"><span data-stu-id="4f172-136">x</span></span>|<span data-ttu-id="4f172-137">x</span><span class="sxs-lookup"><span data-stu-id="4f172-137">x</span></span>
|[<span data-ttu-id="4f172-138">Oracle Database</span><span class="sxs-lookup"><span data-stu-id="4f172-138">Oracle Database</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-oracledatabase)||<span data-ttu-id="4f172-139">x</span><span class="sxs-lookup"><span data-stu-id="4f172-139">x</span></span>|<span data-ttu-id="4f172-140">x</span><span class="sxs-lookup"><span data-stu-id="4f172-140">x</span></span>
|[<span data-ttu-id="4f172-141">Közös adatszolgáltatás</span><span class="sxs-lookup"><span data-stu-id="4f172-141">Common Data Service</span></span>](https://docs.microsoft.com/common-data-service/entity-reference/introduction)||<span data-ttu-id="4f172-142">x</span><span class="sxs-lookup"><span data-stu-id="4f172-142">x</span></span>|<span data-ttu-id="4f172-143">x</span><span class="sxs-lookup"><span data-stu-id="4f172-143">x</span></span>
|[<span data-ttu-id="4f172-144">Salesforce</span><span class="sxs-lookup"><span data-stu-id="4f172-144">Salesforce</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-salesforce)||<span data-ttu-id="4f172-145">x</span><span class="sxs-lookup"><span data-stu-id="4f172-145">x</span></span>|<span data-ttu-id="4f172-146">x</span><span class="sxs-lookup"><span data-stu-id="4f172-146">x</span></span>
|[<span data-ttu-id="4f172-147">SharePoint</span><span class="sxs-lookup"><span data-stu-id="4f172-147">SharePoint</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-sharepointonline)||<span data-ttu-id="4f172-148">x</span><span class="sxs-lookup"><span data-stu-id="4f172-148">x</span></span>|<span data-ttu-id="4f172-149">x</span><span class="sxs-lookup"><span data-stu-id="4f172-149">x</span></span>
|[<span data-ttu-id="4f172-150">SQL Server</span><span class="sxs-lookup"><span data-stu-id="4f172-150">SQL Server</span></span>](https://docs.microsoft.com/azure/connectors/connectors-create-api-sqlazure)||<span data-ttu-id="4f172-151">x</span><span class="sxs-lookup"><span data-stu-id="4f172-151">x</span></span>|<span data-ttu-id="4f172-152">x</span><span class="sxs-lookup"><span data-stu-id="4f172-152">x</span></span>
|[<span data-ttu-id="4f172-153">Teradata</span><span class="sxs-lookup"><span data-stu-id="4f172-153">Teradata</span></span>](http://www.teradata.com/products-and-services/azure/products/)||<span data-ttu-id="4f172-154">x</span><span class="sxs-lookup"><span data-stu-id="4f172-154">x</span></span>|<span data-ttu-id="4f172-155">x</span><span class="sxs-lookup"><span data-stu-id="4f172-155">x</span></span>
|<span data-ttu-id="4f172-156">UserVoice</span><span class="sxs-lookup"><span data-stu-id="4f172-156">UserVoice</span></span>||<span data-ttu-id="4f172-157">x</span><span class="sxs-lookup"><span data-stu-id="4f172-157">x</span></span>|<span data-ttu-id="4f172-158">x</span><span class="sxs-lookup"><span data-stu-id="4f172-158">x</span></span>
|<span data-ttu-id="4f172-159">Zendesk</span><span class="sxs-lookup"><span data-stu-id="4f172-159">Zendesk</span></span>||<span data-ttu-id="4f172-160">x</span><span class="sxs-lookup"><span data-stu-id="4f172-160">x</span></span>|<span data-ttu-id="4f172-161">x</span><span class="sxs-lookup"><span data-stu-id="4f172-161">x</span></span>


> [!NOTE]
> <span data-ttu-id="4f172-162">Külső tábla kapcsolatok is használható a [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)</span><span class="sxs-lookup"><span data-stu-id="4f172-162">External Table connections can also be used in [Azure Logic Apps](https://docs.microsoft.com/azure/connectors/apis-list)</span></span>

### <a name="creating-an-api-connection-step-by-step"></a><span data-ttu-id="4f172-163">Az API-kapcsolat létrehozása: lépésről lépésre</span><span class="sxs-lookup"><span data-stu-id="4f172-163">Creating an API connection: step by step</span></span>

1. <span data-ttu-id="4f172-164">Hozzon létre egy függvényt > egyéni függvény ![egyéni függvény létrehozása](./media/functions-bindings-storage-table/create-custom-function.jpg)</span><span class="sxs-lookup"><span data-stu-id="4f172-164">Create a function > custom function ![Create a custom function](./media/functions-bindings-storage-table/create-custom-function.jpg)</span></span>
1. <span data-ttu-id="4f172-165">A forgatókönyv `Experimental`  >  `ExternalTable-CSharp` sablon > hozzon létre egy új `External Table connection` 
 ![bemeneti sablon kiválasztása](./media/functions-bindings-storage-table/create-template-table.jpg)</span><span class="sxs-lookup"><span data-stu-id="4f172-165">Scenario `Experimental` > `ExternalTable-CSharp` template > Create a new `External Table connection`
![Choose table input template](./media/functions-bindings-storage-table/create-template-table.jpg)</span></span>
1. <span data-ttu-id="4f172-166">Válassza ki azt a Szolgáltatottszoftver-szolgáltatót > Válassza ki vagy hozzon létre kapcsolatot ![konfigurálása Szolgáltatottszoftver-kapcsolat](./media/functions-bindings-storage-table/authorize-API-connection.jpg)</span><span class="sxs-lookup"><span data-stu-id="4f172-166">Choose your SaaS provider > choose/create a connection ![Configure SaaS connection](./media/functions-bindings-storage-table/authorize-API-connection.jpg)</span></span>
1. <span data-ttu-id="4f172-167">Válassza ki az API-kapcsolatot > a függvény létrehozása ![tábla függvény létrehozása](./media/functions-bindings-storage-table/table-template-options.jpg)</span><span class="sxs-lookup"><span data-stu-id="4f172-167">Select your API connection > create the function ![Create table function](./media/functions-bindings-storage-table/table-template-options.jpg)</span></span>
1. <span data-ttu-id="4f172-168">Válassza ki`Integrate` > `External Table`</span><span class="sxs-lookup"><span data-stu-id="4f172-168">Select `Integrate` > `External Table`</span></span>
    1. <span data-ttu-id="4f172-169">Állítsa be a kapcsolatot a céloldali tábla használja.</span><span class="sxs-lookup"><span data-stu-id="4f172-169">Configure the connection to use your target table.</span></span> <span data-ttu-id="4f172-170">Ezek a beállítások nagyon Szolgáltatottszoftver-szolgáltatók között lesz.</span><span class="sxs-lookup"><span data-stu-id="4f172-170">These settings will very between SaaS providers.</span></span> <span data-ttu-id="4f172-171">-E az alábbi vázlat [adatforrás-beállítások](#datasourcesettings)
![konfigurálása tábla](./media/functions-bindings-storage-table/configure-API-connection.jpg)</span><span class="sxs-lookup"><span data-stu-id="4f172-171">They are outline below in [data source settings](#datasourcesettings)
![Configure table](./media/functions-bindings-storage-table/configure-API-connection.jpg)</span></span>

## <a name="usage"></a><span data-ttu-id="4f172-172">Használat</span><span class="sxs-lookup"><span data-stu-id="4f172-172">Usage</span></span>

<span data-ttu-id="4f172-173">Ebben a példában az azonosító, a Vezetéknév és a Vezetéknév oszlopokat "Ügyfél" nevű tábla kapcsolódik.</span><span class="sxs-lookup"><span data-stu-id="4f172-173">This example connects to a table named "Contact" with Id, LastName, and FirstName columns.</span></span> <span data-ttu-id="4f172-174">A kód a Contact entitásokat a táblázat sorolja fel, és a vezeték- és keresztneveket naplózza.</span><span class="sxs-lookup"><span data-stu-id="4f172-174">The code lists the Contact entities in the table and logs the first and last names.</span></span>

### <a name="bindings"></a><span data-ttu-id="4f172-175">Kötések</span><span class="sxs-lookup"><span data-stu-id="4f172-175">Bindings</span></span>
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
<span data-ttu-id="4f172-176">`entityId`a tábla kötések üresnek kell lennie.</span><span class="sxs-lookup"><span data-stu-id="4f172-176">`entityId` must be empty for table bindings.</span></span>

<span data-ttu-id="4f172-177">`ConnectionAppSettingsKey`az Alkalmazásbeállítás, amely tárolja az API-kapcsolati karakterlánc azonosítja.</span><span class="sxs-lookup"><span data-stu-id="4f172-177">`ConnectionAppSettingsKey` identifies the app setting that stores the API connection string.</span></span> <span data-ttu-id="4f172-178">Az Alkalmazásbeállítás automatikusan létrejön, amikor hozzáadja az API-kapcsolat az integráció felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="4f172-178">The app setting is created automatically when you add an API connection in the integrate UI.</span></span>

<span data-ttu-id="4f172-179">A táblázatos összekötő adatkészletek biztosít, és minden egyes táblát tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="4f172-179">A tabular connector provides data sets, and each data set contains tables.</span></span> <span data-ttu-id="4f172-180">Az alapértelmezett adatkészlet neve nem "alapértelmezett".</span><span class="sxs-lookup"><span data-stu-id="4f172-180">The name of the default data set is “default.”</span></span> <span data-ttu-id="4f172-181">A DataSet adatkészlet és a táblázat a különböző Szolgáltatottszoftver-szolgáltatók címeit az alábbiak:</span><span class="sxs-lookup"><span data-stu-id="4f172-181">The titles for a dataset and a table in various SaaS providers are listed below:</span></span>

|<span data-ttu-id="4f172-182">összekötő</span><span class="sxs-lookup"><span data-stu-id="4f172-182">Connector</span></span>|<span data-ttu-id="4f172-183">Adatkészlet</span><span class="sxs-lookup"><span data-stu-id="4f172-183">Dataset</span></span>|<span data-ttu-id="4f172-184">Tábla</span><span class="sxs-lookup"><span data-stu-id="4f172-184">Table</span></span>|
|:-----|:---|:---| 
|<span data-ttu-id="4f172-185">**SharePoint**</span><span class="sxs-lookup"><span data-stu-id="4f172-185">**SharePoint**</span></span>|<span data-ttu-id="4f172-186">Helykiszolgáló</span><span class="sxs-lookup"><span data-stu-id="4f172-186">Site</span></span>|<span data-ttu-id="4f172-187">SharePoint-lista</span><span class="sxs-lookup"><span data-stu-id="4f172-187">SharePoint List</span></span>
|<span data-ttu-id="4f172-188">**SQL**</span><span class="sxs-lookup"><span data-stu-id="4f172-188">**SQL**</span></span>|<span data-ttu-id="4f172-189">Adatbázis</span><span class="sxs-lookup"><span data-stu-id="4f172-189">Database</span></span>|<span data-ttu-id="4f172-190">Tábla</span><span class="sxs-lookup"><span data-stu-id="4f172-190">Table</span></span> 
|<span data-ttu-id="4f172-191">**Google lap**</span><span class="sxs-lookup"><span data-stu-id="4f172-191">**Google Sheet**</span></span>|<span data-ttu-id="4f172-192">Táblázat</span><span class="sxs-lookup"><span data-stu-id="4f172-192">Spreadsheet</span></span>|<span data-ttu-id="4f172-193">Munkalap</span><span class="sxs-lookup"><span data-stu-id="4f172-193">Worksheet</span></span> 
|<span data-ttu-id="4f172-194">**Excel**</span><span class="sxs-lookup"><span data-stu-id="4f172-194">**Excel**</span></span>|<span data-ttu-id="4f172-195">Excel-fájl</span><span class="sxs-lookup"><span data-stu-id="4f172-195">Excel file</span></span>|<span data-ttu-id="4f172-196">Lap</span><span class="sxs-lookup"><span data-stu-id="4f172-196">Sheet</span></span> 

<!--
See the language-specific sample that copies the input file to the output file.

* [C#](#incsharp)
* [Node.js](#innodejs)

-->
<a name="incsharp"></a>

### <a name="usage-in-c"></a><span data-ttu-id="4f172-197">A C# szintaxis</span><span class="sxs-lookup"><span data-stu-id="4f172-197">Usage in C#</span></span> #

```cs
#r "Microsoft.Azure.ApiHub.Sdk"
#r "Newtonsoft.Json"

using System;
using Microsoft.Azure.ApiHub;

//Variable name must match column type
//Variable type is dynamically bound to the incoming data
public class Contact
{
    public string Id { get; set; }
    public string LastName { get; set; }
    public string FirstName { get; set; }
}

public static async Task Run(string input, ITable<Contact> table, TraceWriter log)
{
    //Iterate over every value in the source table
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

<span data-ttu-id="4f172-198"><!--
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
##Az adatforrás-beállítások</span><span class="sxs-lookup"><span data-stu-id="4f172-198"><!--
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

### <a name="sql-server"></a><span data-ttu-id="4f172-199">SQL Server</span><span class="sxs-lookup"><span data-stu-id="4f172-199">SQL Server</span></span>

<span data-ttu-id="4f172-200">A parancsfájl létrehozása és feltöltése az ügyfél tábla nem éri el.</span><span class="sxs-lookup"><span data-stu-id="4f172-200">The script to create and populate the Contact table is below.</span></span> <span data-ttu-id="4f172-201">dataSetName az "alapértelmezett".</span><span class="sxs-lookup"><span data-stu-id="4f172-201">dataSetName is “default.”</span></span>

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

### <a name="google-sheets"></a><span data-ttu-id="4f172-202">Google Táblázatok</span><span class="sxs-lookup"><span data-stu-id="4f172-202">Google Sheets</span></span>
<span data-ttu-id="4f172-203">A Google Docs, hozzon létre egy táblázatot nevű `Contact`.</span><span class="sxs-lookup"><span data-stu-id="4f172-203">In Google Docs, create a spreadsheet with a worksheet named `Contact`.</span></span> <span data-ttu-id="4f172-204">Az összekötő számolótábla megjelenítési név nem használható.</span><span class="sxs-lookup"><span data-stu-id="4f172-204">The connector cannot use the spreadsheet display name.</span></span> <span data-ttu-id="4f172-205">A belső nevét (félkövér) kell használható dataSetName, például: `docs.google.com/spreadsheets/d/`  **`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`**  adja hozzá az oszlopnevek `Id`, `LastName`, `FirstName` első sorának, majd töltse fel a következő sorokban adatokat.</span><span class="sxs-lookup"><span data-stu-id="4f172-205">The internal name (in bold) needs to be used as dataSetName, for example: `docs.google.com/spreadsheets/d/`**`1UIz545JF_cx6Chm_5HpSPVOenU4DZh4bDxbFgJOSMz0`** Add the column names `Id`, `LastName`, `FirstName` to the first row, then populate data on subsequent rows.</span></span>

### <a name="salesforce"></a><span data-ttu-id="4f172-206">Salesforce</span><span class="sxs-lookup"><span data-stu-id="4f172-206">Salesforce</span></span>
<span data-ttu-id="4f172-207">dataSetName az "alapértelmezett".</span><span class="sxs-lookup"><span data-stu-id="4f172-207">dataSetName is “default.”</span></span>

## <a name="next-steps"></a><span data-ttu-id="4f172-208">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4f172-208">Next steps</span></span>
[!INCLUDE [next steps](../../includes/functions-bindings-next-steps.md)]
