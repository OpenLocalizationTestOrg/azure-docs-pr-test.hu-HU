---
title: "DNS-zóna létrehozása, és jegyezze fel beállítása az Azure DNS a .NET SDK használatával |} Microsoft Docs"
description: "DNS-zónák és rekord létrehozása az Azure DNS-beállítja a .NET SDK használatával."
services: dns
documentationcenter: na
author: jtuliani
manager: carmonm
ms.assetid: eed99b87-f4d4-4fbf-a926-263f7e30b884
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/19/2016
ms.author: jonatul
ms.openlocfilehash: c0fb0be8da1c0ca48a4d43ea027d30a0bc17fe30
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-dns-zones-and-record-sets-using-the-net-sdk"></a><span data-ttu-id="eb047-103">Hozzon létre DNS-zónák és a .NET SDK használatával rekordhalmazok</span><span class="sxs-lookup"><span data-stu-id="eb047-103">Create DNS zones and record sets using the .NET SDK</span></span>

<span data-ttu-id="eb047-104">Műveletek létrehozása, törlés vagy frissítés DNS zónák, rekordhalmazokat és rekordokat a DNS-kezelési .NET library DNS SDK segítségével automatizálható.</span><span class="sxs-lookup"><span data-stu-id="eb047-104">You can automate operations to create, delete, or update DNS zones, record sets, and records by using DNS SDK with .NET DNS Management library.</span></span> <span data-ttu-id="eb047-105">Teljes Visual Studio-projekt érhető [itt.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)</span><span class="sxs-lookup"><span data-stu-id="eb047-105">A full Visual Studio project is available [here.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)</span></span>

## <a name="create-a-service-principal-account"></a><span data-ttu-id="eb047-106">Hozzon létre egy egyszerű szolgáltatás fiókja</span><span class="sxs-lookup"><span data-stu-id="eb047-106">Create a service principal account</span></span>

<span data-ttu-id="eb047-107">Általában az Azure-erőforrások programozott hozzáférést kapnak a saját felhasználói hitelesítő adatokat, hanem egy külön fiókot keresztül.</span><span class="sxs-lookup"><span data-stu-id="eb047-107">Typically, programmatic access to Azure resources is granted via a dedicated account rather than your own user credentials.</span></span> <span data-ttu-id="eb047-108">Ezeket a dedikált fiókokat "egyszerű" szolgáltatásfiókok nevezzük.</span><span class="sxs-lookup"><span data-stu-id="eb047-108">These dedicated accounts are called 'service principal' accounts.</span></span> <span data-ttu-id="eb047-109">Az Azure DNS SDK minta projekt használatához először hozzon létre egy egyszerű szolgáltatásfiókot, és rendelje hozzá a megfelelő engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="eb047-109">To use the Azure DNS SDK sample project, you first need to create a service principal account and assign it the correct permissions.</span></span>

1. <span data-ttu-id="eb047-110">Hajtsa végre a [ezeket az utasításokat](../azure-resource-manager/resource-group-authenticate-service-principal.md) létrehozni egy egyszerű szolgáltatás fiókja (az Azure DNS SDK minta projekt jelszóalapú hitelesítés feltételezi.)</span><span class="sxs-lookup"><span data-stu-id="eb047-110">Follow [these instructions](../azure-resource-manager/resource-group-authenticate-service-principal.md) to create a service principal account (the Azure DNS SDK sample project assumes password-based authentication.)</span></span>
2. <span data-ttu-id="eb047-111">Hozzon létre egy erőforráscsoportot ([itt hogyan](../azure-resource-manager/resource-group-template-deploy-portal.md)).</span><span class="sxs-lookup"><span data-stu-id="eb047-111">Create a resource group ([here's how](../azure-resource-manager/resource-group-template-deploy-portal.md)).</span></span>
3. <span data-ttu-id="eb047-112">Használja a Azure RBAC egyszerű szolgáltatás fiókja engedélyekkel "DNS-zóna közreműködői" az erőforráscsoport ([itt hogyan](../active-directory/role-based-access-control-configure.md).)</span><span class="sxs-lookup"><span data-stu-id="eb047-112">Use Azure RBAC to grant the service principal account 'DNS Zone Contributor' permissions to the resource group ([here's how](../active-directory/role-based-access-control-configure.md).)</span></span>
4. <span data-ttu-id="eb047-113">Ha az Azure DNS SDK minta projekt használja, szerkessze az alábbiak szerint a "program.cs" fájlt:</span><span class="sxs-lookup"><span data-stu-id="eb047-113">If using the Azure DNS SDK sample project, edit the 'program.cs' file as follows:</span></span>

   * <span data-ttu-id="eb047-114">Helyezze be a megfelelő értékeket a tenantId, clientId (más néven Fiókazonosító), titkos kulcs (egyszerű szolgáltatásfiók jelszavának) és az 1. lépésben használt előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="eb047-114">Insert the correct values for the tenantId, clientId (also known as account ID), secret (service principal account password) and subscriptionId as used in step 1.</span></span>
   * <span data-ttu-id="eb047-115">Adja meg az erőforráscsoport neve a 2.</span><span class="sxs-lookup"><span data-stu-id="eb047-115">Enter the resource group name chosen in step 2.</span></span>
   * <span data-ttu-id="eb047-116">Adjon meg egy DNS-zóna neve, az Ön által választott.</span><span class="sxs-lookup"><span data-stu-id="eb047-116">Enter a DNS zone name of your choice.</span></span>

## <a name="nuget-packages-and-namespace-declarations"></a><span data-ttu-id="eb047-117">NuGet-csomagok és a névtér-deklarációk</span><span class="sxs-lookup"><span data-stu-id="eb047-117">NuGet packages and namespace declarations</span></span>

<span data-ttu-id="eb047-118">Az Azure DNS .NET SDK használatához telepíteni kell a **Azure DNS könyvtár** NuGet-csomagot, és más szükséges Azure csomagok.</span><span class="sxs-lookup"><span data-stu-id="eb047-118">To use the Azure DNS .NET SDK, you need to install the **Azure DNS Management Library** NuGet package and other required Azure packages.</span></span>

1. <span data-ttu-id="eb047-119">A **Visual Studio**, vagy egy új projekt megnyitása.</span><span class="sxs-lookup"><span data-stu-id="eb047-119">In **Visual Studio**, open a project or new project.</span></span>
2. <span data-ttu-id="eb047-120">Ugrás a **eszközök**  **>**  **NuGet-Csomagkezelő**  **>**  **megoldás NuGet-csomagok kezelése...** .</span><span class="sxs-lookup"><span data-stu-id="eb047-120">Go to **Tools** **>** **NuGet Package Manager** **>** **Manage NuGet Packages for Solution...**.</span></span>
3. <span data-ttu-id="eb047-121">Kattintson **Tallózás**, engedélyezze a **Include prerelease** jelölőnégyzetet, és írja be **Microsoft.Azure.Management.Dns** be a keresőmezőbe.</span><span class="sxs-lookup"><span data-stu-id="eb047-121">Click **Browse**, enable the **Include prerelease** checkbox, and type **Microsoft.Azure.Management.Dns** in the search box.</span></span>
4. <span data-ttu-id="eb047-122">Válassza ki a csomagot, és kattintson a **telepítése** veheti fel a Visual Studio-projektet.</span><span class="sxs-lookup"><span data-stu-id="eb047-122">Select the package and click **Install** to add it to your Visual Studio project.</span></span>
5. <span data-ttu-id="eb047-123">Ismételje meg a következő csomagok telepítése is a fenti folyamatot: **Microsoft.Rest.ClientRuntime.Azure.Authentication** és **Microsoft.Azure.Management.ResourceManager**.</span><span class="sxs-lookup"><span data-stu-id="eb047-123">Repeat the process above to also install the following packages: **Microsoft.Rest.ClientRuntime.Azure.Authentication** and **Microsoft.Azure.Management.ResourceManager**.</span></span>

## <a name="add-namespace-declarations"></a><span data-ttu-id="eb047-124">Névtér-deklarációk hozzáadása</span><span class="sxs-lookup"><span data-stu-id="eb047-124">Add namespace declarations</span></span>

<span data-ttu-id="eb047-125">A következő névtér-deklarációk hozzáadása</span><span class="sxs-lookup"><span data-stu-id="eb047-125">Add the following namespace declarations</span></span>

```cs
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.Dns;
using Microsoft.Azure.Management.Dns.Models;
```

## <a name="initialize-the-dns-management-client"></a><span data-ttu-id="eb047-126">A DNS-felügyeleti ügyfél inicializálása</span><span class="sxs-lookup"><span data-stu-id="eb047-126">Initialize the DNS management client</span></span>

<span data-ttu-id="eb047-127">A *DnsManagementClient* a módszerek és a szükséges DNS-zónák és a rekordhalmazok kezelése tulajdonságait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="eb047-127">The *DnsManagementClient* contains the methods and properties necessary for managing DNS zones and recordsets.</span></span>  <span data-ttu-id="eb047-128">Az alábbi kód egyszerű szolgáltatás fiókja bejelentkezik, és egy DnsManagementClient objektumot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="eb047-128">The following code logs in to the service principal account and creates a DnsManagementClient object.</span></span>

```cs
// Build the service credentials and DNS management client
var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
var dnsClient = new DnsManagementClient(serviceCreds);
dnsClient.SubscriptionId = subscriptionId;
```

## <a name="create-or-update-a-dns-zone"></a><span data-ttu-id="eb047-129">Hozható létre vagy frissíthető a DNS-zóna</span><span class="sxs-lookup"><span data-stu-id="eb047-129">Create or update a DNS zone</span></span>

<span data-ttu-id="eb047-130">DNS-zóna létrehozása, először egy "Zóna" objektumot hoz létre a DNS-zóna paramétereit.</span><span class="sxs-lookup"><span data-stu-id="eb047-130">To create a DNS zone, first a "Zone" object is created to contain the DNS zone parameters.</span></span> <span data-ttu-id="eb047-131">DNS-zónák nem kapcsolódnak egy adott területre, mert a hely értéke "global".</span><span class="sxs-lookup"><span data-stu-id="eb047-131">Because DNS zones are not linked to a specific region, the location is set to 'global'.</span></span> <span data-ttu-id="eb047-132">Ebben a példában egy [Azure Resource Manager "tag"](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) felvétele a zónába is megjelenik.</span><span class="sxs-lookup"><span data-stu-id="eb047-132">In this example, an [Azure Resource Manager 'tag'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) is also added to the zone.</span></span>

<span data-ttu-id="eb047-133">Ténylegesen létrehozásához, vagy az Azure DNS-zóna frissítéséhez, a zóna objektum a zóna paramétereit tartalmazó van lett átadva a *DnsManagementClient.Zones.CreateOrUpdateAsyc* metódust.</span><span class="sxs-lookup"><span data-stu-id="eb047-133">To actually create or update the zone in Azure DNS, the zone object containing the zone parameters is passed to the *DnsManagementClient.Zones.CreateOrUpdateAsyc* method.</span></span>

> [!NOTE]
> <span data-ttu-id="eb047-134">DnsManagementClient művelet három módot támogat: szinkron ("CreateOrUpdate"), aszinkron ("CreateOrUpdateAsync"), vagy aszinkron, aki hozzáféréssel rendelkezik a HTTP-válasz (CreateOrUpdateWithHttpMessagesAsync).</span><span class="sxs-lookup"><span data-stu-id="eb047-134">DnsManagementClient supports three modes of operation: synchronous ('CreateOrUpdate'), asynchronous ('CreateOrUpdateAsync'), or asynchronous with access to the HTTP response ('CreateOrUpdateWithHttpMessagesAsync').</span></span>  <span data-ttu-id="eb047-135">Attól függően, hogy az alkalmazások igényeihez ezek mód választható.</span><span class="sxs-lookup"><span data-stu-id="eb047-135">You can choose any of these modes, depending on your application needs.</span></span>

<span data-ttu-id="eb047-136">Az Azure DNS támogatja az egyidejű hozzáférések optimista, úgynevezett [ETag-EK](dns-getstarted-create-dnszone.md).</span><span class="sxs-lookup"><span data-stu-id="eb047-136">Azure DNS supports optimistic concurrency, called [Etags](dns-getstarted-create-dnszone.md).</span></span> <span data-ttu-id="eb047-137">Ebben a példában megadása "*" a "If-None-Match" a fejléc közli az Azure DNS számára hozzon létre egy DNS-zónát, ha egy nem létezik.</span><span class="sxs-lookup"><span data-stu-id="eb047-137">In this example, specifying "*" for the 'If-None-Match' header tells Azure DNS to create a DNS zone if one does not already exist.</span></span>  <span data-ttu-id="eb047-138">A hívás sikertelen lesz, ha egy zónát a megadott névvel már létezik a megadott erőforráscsoportban.</span><span class="sxs-lookup"><span data-stu-id="eb047-138">The call fails if a zone with the given name already exists in the given resource group.</span></span>

```cs
// Create zone parameters
var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"

// Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
dnsZoneParams.Tags = new Dictionary<string, string>();
dnsZoneParams.Tags.Add("dept", "finance");

// Create the actual zone.
// Note: Uses 'If-None-Match *' ETAG check, so will fail if the zone exists already.
// Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
// Note: For getting the http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");
```

## <a name="create-dns-record-sets-and-records"></a><span data-ttu-id="eb047-139">DNS-rekordhalmazok és rekordok létrehozása</span><span class="sxs-lookup"><span data-stu-id="eb047-139">Create DNS record sets and records</span></span>

<span data-ttu-id="eb047-140">DNS-rekordokat a rendszer kezeli, a rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="eb047-140">DNS records are managed as a record set.</span></span> <span data-ttu-id="eb047-141">Rekordhalmaz olyan rekord az azonos nevű és típusú rekordok a zónán belül.</span><span class="sxs-lookup"><span data-stu-id="eb047-141">A record set is a set of records with the same name and record type within a zone.</span></span>  <span data-ttu-id="eb047-142">A rekordhalmaz nevének megadása képest a zóna nevét, nem teljesen minősített DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="eb047-142">The record set name is relative to the zone name, not the fully qualified DNS name.</span></span>

<span data-ttu-id="eb047-143">Hozzon létre vagy frissíthető, a "Rekordhalmaz" paraméterek objektum létrehozása és átadott *DnsManagementClient.RecordSets.CreateOrUpdateAsync*.</span><span class="sxs-lookup"><span data-stu-id="eb047-143">To create or update a record set, a "RecordSet" parameters object is created and passed to *DnsManagementClient.RecordSets.CreateOrUpdateAsync*.</span></span> <span data-ttu-id="eb047-144">Mivel a DNS-zóna esetén három működési módokat támadja: szinkron ("CreateOrUpdate"), aszinkron ("CreateOrUpdateAsync"), vagy aszinkron, aki hozzáféréssel rendelkezik a HTTP-válasz (CreateOrUpdateWithHttpMessagesAsync).</span><span class="sxs-lookup"><span data-stu-id="eb047-144">As with DNS zones, there are three modes of operation: synchronous ('CreateOrUpdate'), asynchronous ('CreateOrUpdateAsync'), or asynchronous with access to the HTTP response ('CreateOrUpdateWithHttpMessagesAsync').</span></span>

<span data-ttu-id="eb047-145">Csakúgy, mint a DNS-zónák rekordhalmazok műveletek közé tartozik a hozzáférések optimista támogatása.</span><span class="sxs-lookup"><span data-stu-id="eb047-145">As with DNS zones, operations on record sets include support for optimistic concurrency.</span></span>  <span data-ttu-id="eb047-146">Ebben a példában mivel sem az "If-Match", sem az "If-None-Match" meg van adva, a rekordhalmaz mindig létrejön.</span><span class="sxs-lookup"><span data-stu-id="eb047-146">In this example, since neither 'If-Match' nor 'If-None-Match' are specified, the record set is always created.</span></span>  <span data-ttu-id="eb047-147">Ez a hívás felülírja a meglévő rekordhalmazt azonos nevű és rekordtípust a DNS-zóna.</span><span class="sxs-lookup"><span data-stu-id="eb047-147">This call overwrites any existing record set with the same name and record type in this DNS zone.</span></span>

```cs
// Create record set parameters
var recordSetParams = new RecordSet();
recordSetParams.TTL = 3600;

// Add records to the record set parameter object.  In this case, we'll add a record of type 'A'
recordSetParams.ARecords = new List<ARecord>();
recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

// Add metadata to the record set.  Similar to Azure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
recordSetParams.Metadata = new Dictionary<string, string>();
recordSetParams.Metadata.Add("user", "Mary");

// Create the actual record set in Azure DNS
// Note: no ETAG checks specified, will overwrite existing record set if one exists
var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);
```

## <a name="get-zones-and-record-sets"></a><span data-ttu-id="eb047-148">Zónák és a rekordhalmazok</span><span class="sxs-lookup"><span data-stu-id="eb047-148">Get zones and record sets</span></span>

<span data-ttu-id="eb047-149">A *DnsManagementClient.Zones.Get* és *DnsManagementClient.RecordSets.Get* módszerek beolvasása az egyes zónák és a rekordhalmazok, illetve.</span><span class="sxs-lookup"><span data-stu-id="eb047-149">The *DnsManagementClient.Zones.Get* and *DnsManagementClient.RecordSets.Get* methods retrieve individual zones and record sets, respectively.</span></span> <span data-ttu-id="eb047-150">Rekordhalmazok típusuk, nevét és a zóna és a erőforrás csoport léteznek az azonosítják.</span><span class="sxs-lookup"><span data-stu-id="eb047-150">RecordSets are identified by their type, name, and the zone and resource group they exist in.</span></span> <span data-ttu-id="eb047-151">A név és az erőforráscsoport léteznek a zónák azonosítja.</span><span class="sxs-lookup"><span data-stu-id="eb047-151">Zones are identified by their name and the resource group they exist in.</span></span>

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
```

## <a name="update-an-existing-record-set"></a><span data-ttu-id="eb047-152">Egy meglévő rekordhalmaz frissítése</span><span class="sxs-lookup"><span data-stu-id="eb047-152">Update an existing record set</span></span>

<span data-ttu-id="eb047-153">Egy meglévő DNS-rekordhalmaz frissíteni, először a rekordhalmaz beolvasása, majd frissítik rekordhalmaz tartalmát, majd küldje el a módosítást.</span><span class="sxs-lookup"><span data-stu-id="eb047-153">To update an existing DNS record set, first retrieve the record set, then update the record set contents, then submit the change.</span></span>  <span data-ttu-id="eb047-154">Ebben a példában a "Etag" a "If-Match" paraméter, a beolvasott rekord készletből adtuk meg.</span><span class="sxs-lookup"><span data-stu-id="eb047-154">In this example, we specify the 'Etag' from the retrieved record set in the 'If-Match' parameter.</span></span> <span data-ttu-id="eb047-155">A hívás sikertelen lesz, ha egy párhuzamos művelet módosította a rekordhalmaz időközben.</span><span class="sxs-lookup"><span data-stu-id="eb047-155">The call fails if a concurrent operation has modified the record set in the meantime.</span></span>

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

// Add a new record to the local object.  Note that records in a record set must be unique/distinct
recordSet.ARecords.Add(new ARecord("5.6.7.8"));

// Update the record set in Azure DNS
// Note: ETAG check specified, update will be rejected if the record set has changed in the meantime
recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);
```

## <a name="list-zones-and-record-sets"></a><span data-ttu-id="eb047-156">Lista zóna és a rekordhalmazok</span><span class="sxs-lookup"><span data-stu-id="eb047-156">List zones and record sets</span></span>

<span data-ttu-id="eb047-157">A zónák listában használja a *DnsManagementClient.Zones.List...*  módszereket támogatja, vagy egy adott erőforráscsoportban található összes zóna vagy egy adott Azure-előfizetésben minden zóna listázása (között erőforráscsoportok.) A rekordhalmazok listában használja *DnsManagementClient.RecordSets.List...*  módszereket támogatja, vagy egy adott zónában minden rekordkészletet vagy csak adott típusú ezen rekordhalmazok listázása.</span><span class="sxs-lookup"><span data-stu-id="eb047-157">To list zones, use the *DnsManagementClient.Zones.List...* methods, which support listing either all zones in a given resource group or all zones in a given Azure subscription (across resource groups.) To list record sets, use *DnsManagementClient.RecordSets.List...* methods, which support either listing all record sets in a given zone or only those record sets of a specific type.</span></span>

<span data-ttu-id="eb047-158">Jegyezze fel a zónák listázásakor, és a rekordhalmazok eredmények lehet, hogy paginated.</span><span class="sxs-lookup"><span data-stu-id="eb047-158">Note when listing zones and record sets that results may be paginated.</span></span>  <span data-ttu-id="eb047-159">A következő példa bemutatja, hogyan iterációt az eredmények oldalát.</span><span class="sxs-lookup"><span data-stu-id="eb047-159">The following example shows how to iterate through the pages of results.</span></span> <span data-ttu-id="eb047-160">(Egy mesterségesen kis oldalméret "2" használatával kényszerítheti a lapozás; a gyakorlatban ez a paraméter nincs megadva és az alapértelmezett oldalméret használt.)</span><span class="sxs-lookup"><span data-stu-id="eb047-160">(An artificially small page size of '2' is used to force paging; in practice this parameter should be omitted and the default page size used.)</span></span>

```cs
// Note: in this demo, we'll use a very small page size (2 record sets) to demonstrate paging
// In practice, to improve performance you would use a large page size or just use the system default
int recordSets = 0;
var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
recordSets += page.Count();

while (page.NextPageLink != null)
{
    page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
    recordSets += page.Count();
}
```

## <a name="next-steps"></a><span data-ttu-id="eb047-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eb047-161">Next steps</span></span>

<span data-ttu-id="eb047-162">Töltse le a [Azure DNS .NET SDK mintaprojektet](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), mely tartalmazza az Azure DNS .NET SDK-val, többek között például a más DNS-rekord típust további példákat.</span><span class="sxs-lookup"><span data-stu-id="eb047-162">Download the [Azure DNS .NET SDK sample project](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), which includes further examples of how to use the Azure DNS .NET SDK, including examples for other DNS record types.</span></span>
