---
title: "aaaCreate DNS-zónák és a rekordhalmazok az Azure DNS használatával hello .NET SDK |} Microsoft Docs"
description: "Hogyan toocreate DNS-zónák és rekord beállítása az Azure DNS-hello .NET SDK használatával."
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
ms.openlocfilehash: e3bab98b13b787427219acb7ec55e53490512fe7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-dns-zones-and-record-sets-using-hello-net-sdk"></a><span data-ttu-id="19e94-103">Hozzon létre DNS-zónák és a rekordhalmazok hello .NET SDK használatával</span><span class="sxs-lookup"><span data-stu-id="19e94-103">Create DNS zones and record sets using hello .NET SDK</span></span>

<span data-ttu-id="19e94-104">Műveletek toocreate, delete vagy update DNS-zónák, rekordhalmazokat és rekordokat a DNS-kezelési .NET library DNS SDK segítségével automatizálható.</span><span class="sxs-lookup"><span data-stu-id="19e94-104">You can automate operations toocreate, delete, or update DNS zones, record sets, and records by using DNS SDK with .NET DNS Management library.</span></span> <span data-ttu-id="19e94-105">Teljes Visual Studio-projekt érhető [itt.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)</span><span class="sxs-lookup"><span data-stu-id="19e94-105">A full Visual Studio project is available [here.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)</span></span>

## <a name="create-a-service-principal-account"></a><span data-ttu-id="19e94-106">Hozzon létre egy egyszerű szolgáltatás fiókja</span><span class="sxs-lookup"><span data-stu-id="19e94-106">Create a service principal account</span></span>

<span data-ttu-id="19e94-107">Programozott hozzáférés tooAzure erőforrások általában a saját felhasználói hitelesítő adatokat, hanem egy külön fiókot keresztül kap.</span><span class="sxs-lookup"><span data-stu-id="19e94-107">Typically, programmatic access tooAzure resources is granted via a dedicated account rather than your own user credentials.</span></span> <span data-ttu-id="19e94-108">Ezeket a dedikált fiókokat "egyszerű" szolgáltatásfiókok nevezzük.</span><span class="sxs-lookup"><span data-stu-id="19e94-108">These dedicated accounts are called 'service principal' accounts.</span></span> <span data-ttu-id="19e94-109">toouse hello Azure DNS SDK minta projekt, akkor először toocreate egyszerű szolgáltatásfiók kell, és rendelje hozzá hello megfelelő engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="19e94-109">toouse hello Azure DNS SDK sample project, you first need toocreate a service principal account and assign it hello correct permissions.</span></span>

1. <span data-ttu-id="19e94-110">Hajtsa végre a [ezeket az utasításokat](../azure-resource-manager/resource-group-authenticate-service-principal.md) toocreate egyszerű szolgáltatás fiókja (hello Azure DNS SDK mintaprojektet jelszóalapú hitelesítés feltételezi.)</span><span class="sxs-lookup"><span data-stu-id="19e94-110">Follow [these instructions](../azure-resource-manager/resource-group-authenticate-service-principal.md) toocreate a service principal account (hello Azure DNS SDK sample project assumes password-based authentication.)</span></span>
2. <span data-ttu-id="19e94-111">Hozzon létre egy erőforráscsoportot ([itt hogyan](../azure-resource-manager/resource-group-template-deploy-portal.md)).</span><span class="sxs-lookup"><span data-stu-id="19e94-111">Create a resource group ([here's how](../azure-resource-manager/resource-group-template-deploy-portal.md)).</span></span>
3. <span data-ttu-id="19e94-112">Azure RBAC toogrant hello szolgáltatás egyszerű fiókja "DNS-zóna közreműködői" engedélyek toohello erőforrás csoport használatával ([itt hogyan](../active-directory/role-based-access-control-configure.md).)</span><span class="sxs-lookup"><span data-stu-id="19e94-112">Use Azure RBAC toogrant hello service principal account 'DNS Zone Contributor' permissions toohello resource group ([here's how](../active-directory/role-based-access-control-configure.md).)</span></span>
4. <span data-ttu-id="19e94-113">Ha hello Azure DNS SDK minta projekt használja, szerkessze az alábbiak szerint hello "program.cs" fájlt:</span><span class="sxs-lookup"><span data-stu-id="19e94-113">If using hello Azure DNS SDK sample project, edit hello 'program.cs' file as follows:</span></span>

   * <span data-ttu-id="19e94-114">Helyezze be a hello helyes az értékük hello tenantId, clientId (más néven Fiókazonosító), titkos kulcs (egyszerű szolgáltatásfiók jelszavának) és az 1. lépésben használt előfizetés-azonosító.</span><span class="sxs-lookup"><span data-stu-id="19e94-114">Insert hello correct values for hello tenantId, clientId (also known as account ID), secret (service principal account password) and subscriptionId as used in step 1.</span></span>
   * <span data-ttu-id="19e94-115">Adja meg az erőforráscsoport neve hello a 2.</span><span class="sxs-lookup"><span data-stu-id="19e94-115">Enter hello resource group name chosen in step 2.</span></span>
   * <span data-ttu-id="19e94-116">Adjon meg egy DNS-zóna neve, az Ön által választott.</span><span class="sxs-lookup"><span data-stu-id="19e94-116">Enter a DNS zone name of your choice.</span></span>

## <a name="nuget-packages-and-namespace-declarations"></a><span data-ttu-id="19e94-117">NuGet-csomagok és a névtér-deklarációk</span><span class="sxs-lookup"><span data-stu-id="19e94-117">NuGet packages and namespace declarations</span></span>

<span data-ttu-id="19e94-118">toouse hello Azure DNS .NET SDK, kell tooinstall hello **Azure DNS könyvtár** NuGet-csomagot, és más szükséges Azure csomagok.</span><span class="sxs-lookup"><span data-stu-id="19e94-118">toouse hello Azure DNS .NET SDK, you need tooinstall hello **Azure DNS Management Library** NuGet package and other required Azure packages.</span></span>

1. <span data-ttu-id="19e94-119">A **Visual Studio**, vagy egy új projekt megnyitása.</span><span class="sxs-lookup"><span data-stu-id="19e94-119">In **Visual Studio**, open a project or new project.</span></span>
2. <span data-ttu-id="19e94-120">Nyissa meg túl**eszközök** ** > ** **NuGet-Csomagkezelő** ** > ** **NuGet-csomagok kezelése a Megoldás... **.</span><span class="sxs-lookup"><span data-stu-id="19e94-120">Go too**Tools** **>** **NuGet Package Manager** **>** **Manage NuGet Packages for Solution...**.</span></span>
3. <span data-ttu-id="19e94-121">Kattintson a **Tallózás**, hello engedélyezése **Include prerelease** jelölőnégyzetet, és írja be **Microsoft.Azure.Management.Dns** hello Keresés mezőbe.</span><span class="sxs-lookup"><span data-stu-id="19e94-121">Click **Browse**, enable hello **Include prerelease** checkbox, and type **Microsoft.Azure.Management.Dns** in hello search box.</span></span>
4. <span data-ttu-id="19e94-122">Jelölje ki hello csomagot, kattintson a **telepítése** tooadd azt tooyour Visual Studio-projektet.</span><span class="sxs-lookup"><span data-stu-id="19e94-122">Select hello package and click **Install** tooadd it tooyour Visual Studio project.</span></span>
5. <span data-ttu-id="19e94-123">Ismételje meg a fenti tooalso telepítés hello csomagok a következő hello folyamat: **Microsoft.Rest.ClientRuntime.Azure.Authentication** és **Microsoft.Azure.Management.ResourceManager**.</span><span class="sxs-lookup"><span data-stu-id="19e94-123">Repeat hello process above tooalso install hello following packages: **Microsoft.Rest.ClientRuntime.Azure.Authentication** and **Microsoft.Azure.Management.ResourceManager**.</span></span>

## <a name="add-namespace-declarations"></a><span data-ttu-id="19e94-124">Névtér-deklarációk hozzáadása</span><span class="sxs-lookup"><span data-stu-id="19e94-124">Add namespace declarations</span></span>

<span data-ttu-id="19e94-125">A következő névtér-deklarációk hello hozzáadása</span><span class="sxs-lookup"><span data-stu-id="19e94-125">Add hello following namespace declarations</span></span>

```cs
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.Dns;
using Microsoft.Azure.Management.Dns.Models;
```

## <a name="initialize-hello-dns-management-client"></a><span data-ttu-id="19e94-126">Hello DNS-felügyeleti ügyfél inicializálása</span><span class="sxs-lookup"><span data-stu-id="19e94-126">Initialize hello DNS management client</span></span>

<span data-ttu-id="19e94-127">Hello *DnsManagementClient* hello módszerek és a szükséges DNS-zónák és a rekordhalmazok kezelése tulajdonságait tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="19e94-127">hello *DnsManagementClient* contains hello methods and properties necessary for managing DNS zones and recordsets.</span></span>  <span data-ttu-id="19e94-128">hello alábbira jelentkezik be toohello egyszerű szolgáltatás fiókja és egy DnsManagementClient objektumot hoz létre.</span><span class="sxs-lookup"><span data-stu-id="19e94-128">hello following code logs in toohello service principal account and creates a DnsManagementClient object.</span></span>

```cs
// Build hello service credentials and DNS management client
var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
var dnsClient = new DnsManagementClient(serviceCreds);
dnsClient.SubscriptionId = subscriptionId;
```

## <a name="create-or-update-a-dns-zone"></a><span data-ttu-id="19e94-129">Hozható létre vagy frissíthető a DNS-zóna</span><span class="sxs-lookup"><span data-stu-id="19e94-129">Create or update a DNS zone</span></span>

<span data-ttu-id="19e94-130">DNS-zóna toocreate, először egy "Zóna" objektum létrehozása toocontain hello DNS-zóna paramétereit.</span><span class="sxs-lookup"><span data-stu-id="19e94-130">toocreate a DNS zone, first a "Zone" object is created toocontain hello DNS zone parameters.</span></span> <span data-ttu-id="19e94-131">Mivel a DNS-zónák nem csatolt tooa adott régióban, hello helye beállítva too'global ".</span><span class="sxs-lookup"><span data-stu-id="19e94-131">Because DNS zones are not linked tooa specific region, hello location is set too'global'.</span></span> <span data-ttu-id="19e94-132">Ebben a példában egy [Azure Resource Manager "tag"](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) toohello zóna is megjelenik.</span><span class="sxs-lookup"><span data-stu-id="19e94-132">In this example, an [Azure Resource Manager 'tag'](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) is also added toohello zone.</span></span>

<span data-ttu-id="19e94-133">tooactually létrehozása vagy frissítése hello zóna Azure DNS-beli hello zóna paramétereit tartalmazó hello zóna objektum átadása toohello *DnsManagementClient.Zones.CreateOrUpdateAsyc* metódust.</span><span class="sxs-lookup"><span data-stu-id="19e94-133">tooactually create or update hello zone in Azure DNS, hello zone object containing hello zone parameters is passed toohello *DnsManagementClient.Zones.CreateOrUpdateAsyc* method.</span></span>

> [!NOTE]
> <span data-ttu-id="19e94-134">DnsManagementClient művelet három módot támogat: szinkron ("CreateOrUpdate"), aszinkron ("CreateOrUpdateAsync"), vagy aszinkron hozzáférés toohello HTTP-válasz (CreateOrUpdateWithHttpMessagesAsync).</span><span class="sxs-lookup"><span data-stu-id="19e94-134">DnsManagementClient supports three modes of operation: synchronous ('CreateOrUpdate'), asynchronous ('CreateOrUpdateAsync'), or asynchronous with access toohello HTTP response ('CreateOrUpdateWithHttpMessagesAsync').</span></span>  <span data-ttu-id="19e94-135">Attól függően, hogy az alkalmazások igényeihez ezek mód választható.</span><span class="sxs-lookup"><span data-stu-id="19e94-135">You can choose any of these modes, depending on your application needs.</span></span>

<span data-ttu-id="19e94-136">Az Azure DNS támogatja az egyidejű hozzáférések optimista, úgynevezett [ETag-EK](dns-getstarted-create-dnszone.md).</span><span class="sxs-lookup"><span data-stu-id="19e94-136">Azure DNS supports optimistic concurrency, called [Etags](dns-getstarted-create-dnszone.md).</span></span> <span data-ttu-id="19e94-137">A jelen példában adja meg "*" hello "If-None-Match" fejlécet közli az Azure DNS toocreate DNS-zóna Ha egy nem létezik a.</span><span class="sxs-lookup"><span data-stu-id="19e94-137">In this example, specifying "*" for hello 'If-None-Match' header tells Azure DNS toocreate a DNS zone if one does not already exist.</span></span>  <span data-ttu-id="19e94-138">hello hívás sikertelen lesz, ha a zóna hello megadott névvel már létezik a megadott erőforráscsoport hello.</span><span class="sxs-lookup"><span data-stu-id="19e94-138">hello call fails if a zone with hello given name already exists in hello given resource group.</span></span>

```cs
// Create zone parameters
var dnsZoneParams = new Zone("global"); // All DNS zones must have location = "global"

// Create a Azure Resource Manager 'tag'.  This is optional.  You can add multiple tags
dnsZoneParams.Tags = new Dictionary<string, string>();
dnsZoneParams.Tags.Add("dept", "finance");

// Create hello actual zone.
// Note: Uses 'If-None-Match *' ETAG check, so will fail if hello zone exists already.
// Note: For non-async usage, call dnsClient.Zones.CreateOrUpdate(resourceGroupName, zoneName, dnsZoneParams, null, "*")
// Note: For getting hello http response, call dnsClient.Zones.CreateOrUpdateWithHttpMessagesAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*")
var dnsZone = await dnsClient.Zones.CreateOrUpdateAsync(resourceGroupName, zoneName, dnsZoneParams, null, "*");
```

## <a name="create-dns-record-sets-and-records"></a><span data-ttu-id="19e94-139">DNS-rekordhalmazok és rekordok létrehozása</span><span class="sxs-lookup"><span data-stu-id="19e94-139">Create DNS record sets and records</span></span>

<span data-ttu-id="19e94-140">DNS-rekordokat a rendszer kezeli, a rekordhalmaz.</span><span class="sxs-lookup"><span data-stu-id="19e94-140">DNS records are managed as a record set.</span></span> <span data-ttu-id="19e94-141">Rekordhalmaz olyan rekordokat hello ugyanazt a nevet, és jegyezze fel a zónán belül típusa.</span><span class="sxs-lookup"><span data-stu-id="19e94-141">A record set is a set of records with hello same name and record type within a zone.</span></span>  <span data-ttu-id="19e94-142">hello rekordhalmaz nevébe relatív toohello zóna nevét, nem hello teljesen minősített DNS-nevét.</span><span class="sxs-lookup"><span data-stu-id="19e94-142">hello record set name is relative toohello zone name, not hello fully qualified DNS name.</span></span>

<span data-ttu-id="19e94-143">rekordhalmaz toocreate vagy frissítés, a "Rekordhalmaz" paraméterek objektum létrehozása és átadott túl*DnsManagementClient.RecordSets.CreateOrUpdateAsync*.</span><span class="sxs-lookup"><span data-stu-id="19e94-143">toocreate or update a record set, a "RecordSet" parameters object is created and passed too*DnsManagementClient.RecordSets.CreateOrUpdateAsync*.</span></span> <span data-ttu-id="19e94-144">Mivel a DNS-zóna esetén három működési módokat támadja: szinkron ("CreateOrUpdate"), aszinkron ("CreateOrUpdateAsync"), vagy aszinkron hozzáférés toohello HTTP-válasz (CreateOrUpdateWithHttpMessagesAsync).</span><span class="sxs-lookup"><span data-stu-id="19e94-144">As with DNS zones, there are three modes of operation: synchronous ('CreateOrUpdate'), asynchronous ('CreateOrUpdateAsync'), or asynchronous with access toohello HTTP response ('CreateOrUpdateWithHttpMessagesAsync').</span></span>

<span data-ttu-id="19e94-145">Csakúgy, mint a DNS-zónák rekordhalmazok műveletek közé tartozik a hozzáférések optimista támogatása.</span><span class="sxs-lookup"><span data-stu-id="19e94-145">As with DNS zones, operations on record sets include support for optimistic concurrency.</span></span>  <span data-ttu-id="19e94-146">Ebben a példában sem az "If-Match", sem az "If-None-Match" meg van adva, mivel hello rekordhalmaz mindig létrejön.</span><span class="sxs-lookup"><span data-stu-id="19e94-146">In this example, since neither 'If-Match' nor 'If-None-Match' are specified, hello record set is always created.</span></span>  <span data-ttu-id="19e94-147">A hívás felülírja hello ugyanazt a nevet, és jegyezze fel a bármely létező rekordot a DNS-zóna típusa.</span><span class="sxs-lookup"><span data-stu-id="19e94-147">This call overwrites any existing record set with hello same name and record type in this DNS zone.</span></span>

```cs
// Create record set parameters
var recordSetParams = new RecordSet();
recordSetParams.TTL = 3600;

// Add records toohello record set parameter object.  In this case, we'll add a record of type 'A'
recordSetParams.ARecords = new List<ARecord>();
recordSetParams.ARecords.Add(new ARecord("1.2.3.4"));

// Add metadata toohello record set.  Similar tooAzure Resource Manager tags, this is optional and you can add multiple metadata name/value pairs
recordSetParams.Metadata = new Dictionary<string, string>();
recordSetParams.Metadata.Add("user", "Mary");

// Create hello actual record set in Azure DNS
// Note: no ETAG checks specified, will overwrite existing record set if one exists
var recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSetParams);
```

## <a name="get-zones-and-record-sets"></a><span data-ttu-id="19e94-148">Zónák és a rekordhalmazok</span><span class="sxs-lookup"><span data-stu-id="19e94-148">Get zones and record sets</span></span>

<span data-ttu-id="19e94-149">Hello *DnsManagementClient.Zones.Get* és *DnsManagementClient.RecordSets.Get* módszerek beolvasása az egyes zónák és a rekordhalmazok, illetve.</span><span class="sxs-lookup"><span data-stu-id="19e94-149">hello *DnsManagementClient.Zones.Get* and *DnsManagementClient.RecordSets.Get* methods retrieve individual zones and record sets, respectively.</span></span> <span data-ttu-id="19e94-150">Rekordhalmazok a típusa, a név és a léteznek az hello zóna, illetve erőforrás csoport azonosítják.</span><span class="sxs-lookup"><span data-stu-id="19e94-150">RecordSets are identified by their type, name, and hello zone and resource group they exist in.</span></span> <span data-ttu-id="19e94-151">A név és hello erőforráscsoport léteznek a zónák azonosítja.</span><span class="sxs-lookup"><span data-stu-id="19e94-151">Zones are identified by their name and hello resource group they exist in.</span></span>

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
```

## <a name="update-an-existing-record-set"></a><span data-ttu-id="19e94-152">Egy meglévő rekordhalmaz frissítése</span><span class="sxs-lookup"><span data-stu-id="19e94-152">Update an existing record set</span></span>

<span data-ttu-id="19e94-153">egy meglévő DNS-rekordot tooupdate beállításához először kérjen le hello rekordhalmaz, majd a frissítés hello rekord tartalmát, akkor a hello módosítás elküldése.</span><span class="sxs-lookup"><span data-stu-id="19e94-153">tooupdate an existing DNS record set, first retrieve hello record set, then update hello record set contents, then submit hello change.</span></span>  <span data-ttu-id="19e94-154">Ebben a példában az "Etag" hello a lekért hello "If-Match" paraméter rekordhalmazt hello adtuk meg.</span><span class="sxs-lookup"><span data-stu-id="19e94-154">In this example, we specify hello 'Etag' from hello retrieved record set in hello 'If-Match' parameter.</span></span> <span data-ttu-id="19e94-155">hello hívás sikertelen lesz, ha egy párhuzamos művelet módosította hello rekordhalmazt hello addig is.</span><span class="sxs-lookup"><span data-stu-id="19e94-155">hello call fails if a concurrent operation has modified hello record set in hello meantime.</span></span>

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

// Add a new record toohello local object.  Note that records in a record set must be unique/distinct
recordSet.ARecords.Add(new ARecord("5.6.7.8"));

// Update hello record set in Azure DNS
// Note: ETAG check specified, update will be rejected if hello record set has changed in hello meantime
recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);
```

## <a name="list-zones-and-record-sets"></a><span data-ttu-id="19e94-156">Lista zóna és a rekordhalmazok</span><span class="sxs-lookup"><span data-stu-id="19e94-156">List zones and record sets</span></span>

<span data-ttu-id="19e94-157">toolist zónák használatára hello *DnsManagementClient.Zones.List... * módszerek, amely támogatja az egy adott erőforráscsoportban található összes zónákat, vagy az adott Azure-előfizetés (között erőforrás csoportok.) toolist rekordhalmazok minden zóna listázása, használjon *DnsManagementClient.RecordSets.List... * módszereket támogatja, vagy egy adott zónában minden rekordkészletet vagy csak adott típusú ezen rekordhalmazok listázása.</span><span class="sxs-lookup"><span data-stu-id="19e94-157">toolist zones, use hello *DnsManagementClient.Zones.List...* methods, which support listing either all zones in a given resource group or all zones in a given Azure subscription (across resource groups.) toolist record sets, use *DnsManagementClient.RecordSets.List...* methods, which support either listing all record sets in a given zone or only those record sets of a specific type.</span></span>

<span data-ttu-id="19e94-158">Jegyezze fel a zónák listázásakor, és a rekordhalmazok eredmények lehet, hogy paginated.</span><span class="sxs-lookup"><span data-stu-id="19e94-158">Note when listing zones and record sets that results may be paginated.</span></span>  <span data-ttu-id="19e94-159">a következő példa azt mutatja meg hogyan hello tooiterate hello oldalak az eredmények között.</span><span class="sxs-lookup"><span data-stu-id="19e94-159">hello following example shows how tooiterate through hello pages of results.</span></span> <span data-ttu-id="19e94-160">(Egy mesterségesen kis oldalméret "2" használt tooforce lapozás, a gyakorlatban ez a paraméter megadni, és használt alapértelmezett oldalméret hello.)</span><span class="sxs-lookup"><span data-stu-id="19e94-160">(An artificially small page size of '2' is used tooforce paging; in practice this parameter should be omitted and hello default page size used.)</span></span>

```cs
// Note: in this demo, we'll use a very small page size (2 record sets) toodemonstrate paging
// In practice, tooimprove performance you would use a large page size or just use hello system default
int recordSets = 0;
var page = await dnsClient.RecordSets.ListAllInResourceGroupAsync(resourceGroupName, zoneName, "2");
recordSets += page.Count();

while (page.NextPageLink != null)
{
    page = await dnsClient.RecordSets.ListAllInResourceGroupNextAsync(page.NextPageLink);
    recordSets += page.Count();
}
```

## <a name="next-steps"></a><span data-ttu-id="19e94-161">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="19e94-161">Next steps</span></span>

<span data-ttu-id="19e94-162">Töltse le a hello [Azure DNS .NET SDK mintaprojektet](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), mely tartalmazza a további példákat is, hogyan toouse hello Azure DNS .NET SDK-t, beleértve a példákat a más DNS-rekord típust.</span><span class="sxs-lookup"><span data-stu-id="19e94-162">Download hello [Azure DNS .NET SDK sample project](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), which includes further examples of how toouse hello Azure DNS .NET SDK, including examples for other DNS record types.</span></span>
