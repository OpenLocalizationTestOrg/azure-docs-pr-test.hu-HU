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
# <a name="create-dns-zones-and-record-sets-using-hello-net-sdk"></a>Hozzon létre DNS-zónák és a rekordhalmazok hello .NET SDK használatával

Műveletek toocreate, delete vagy update DNS-zónák, rekordhalmazokat és rekordokat a DNS-kezelési .NET library DNS SDK segítségével automatizálható. Teljes Visual Studio-projekt érhető [itt.](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True)

## <a name="create-a-service-principal-account"></a>Hozzon létre egy egyszerű szolgáltatás fiókja

Programozott hozzáférés tooAzure erőforrások általában a saját felhasználói hitelesítő adatokat, hanem egy külön fiókot keresztül kap. Ezeket a dedikált fiókokat "egyszerű" szolgáltatásfiókok nevezzük. toouse hello Azure DNS SDK minta projekt, akkor először toocreate egyszerű szolgáltatásfiók kell, és rendelje hozzá hello megfelelő engedélyekkel.

1. Hajtsa végre a [ezeket az utasításokat](../azure-resource-manager/resource-group-authenticate-service-principal.md) toocreate egyszerű szolgáltatás fiókja (hello Azure DNS SDK mintaprojektet jelszóalapú hitelesítés feltételezi.)
2. Hozzon létre egy erőforráscsoportot ([itt hogyan](../azure-resource-manager/resource-group-template-deploy-portal.md)).
3. Azure RBAC toogrant hello szolgáltatás egyszerű fiókja "DNS-zóna közreműködői" engedélyek toohello erőforrás csoport használatával ([itt hogyan](../active-directory/role-based-access-control-configure.md).)
4. Ha hello Azure DNS SDK minta projekt használja, szerkessze az alábbiak szerint hello "program.cs" fájlt:

   * Helyezze be a hello helyes az értékük hello tenantId, clientId (más néven Fiókazonosító), titkos kulcs (egyszerű szolgáltatásfiók jelszavának) és az 1. lépésben használt előfizetés-azonosító.
   * Adja meg az erőforráscsoport neve hello a 2.
   * Adjon meg egy DNS-zóna neve, az Ön által választott.

## <a name="nuget-packages-and-namespace-declarations"></a>NuGet-csomagok és a névtér-deklarációk

toouse hello Azure DNS .NET SDK, kell tooinstall hello **Azure DNS könyvtár** NuGet-csomagot, és más szükséges Azure csomagok.

1. A **Visual Studio**, vagy egy új projekt megnyitása.
2. Nyissa meg túl**eszközök** ** > ** **NuGet-Csomagkezelő** ** > ** **NuGet-csomagok kezelése a Megoldás... **.
3. Kattintson a **Tallózás**, hello engedélyezése **Include prerelease** jelölőnégyzetet, és írja be **Microsoft.Azure.Management.Dns** hello Keresés mezőbe.
4. Jelölje ki hello csomagot, kattintson a **telepítése** tooadd azt tooyour Visual Studio-projektet.
5. Ismételje meg a fenti tooalso telepítés hello csomagok a következő hello folyamat: **Microsoft.Rest.ClientRuntime.Azure.Authentication** és **Microsoft.Azure.Management.ResourceManager**.

## <a name="add-namespace-declarations"></a>Névtér-deklarációk hozzáadása

A következő névtér-deklarációk hello hozzáadása

```cs
using Microsoft.Rest.Azure.Authentication;
using Microsoft.Azure.Management.Dns;
using Microsoft.Azure.Management.Dns.Models;
```

## <a name="initialize-hello-dns-management-client"></a>Hello DNS-felügyeleti ügyfél inicializálása

Hello *DnsManagementClient* hello módszerek és a szükséges DNS-zónák és a rekordhalmazok kezelése tulajdonságait tartalmazza.  hello alábbira jelentkezik be toohello egyszerű szolgáltatás fiókja és egy DnsManagementClient objektumot hoz létre.

```cs
// Build hello service credentials and DNS management client
var serviceCreds = await ApplicationTokenProvider.LoginSilentAsync(tenantId, clientId, secret);
var dnsClient = new DnsManagementClient(serviceCreds);
dnsClient.SubscriptionId = subscriptionId;
```

## <a name="create-or-update-a-dns-zone"></a>Hozható létre vagy frissíthető a DNS-zóna

DNS-zóna toocreate, először egy "Zóna" objektum létrehozása toocontain hello DNS-zóna paramétereit. Mivel a DNS-zónák nem csatolt tooa adott régióban, hello helye beállítva too'global ". Ebben a példában egy [Azure Resource Manager "tag"](https://azure.microsoft.com/updates/organize-your-azure-resources-with-tags/) toohello zóna is megjelenik.

tooactually létrehozása vagy frissítése hello zóna Azure DNS-beli hello zóna paramétereit tartalmazó hello zóna objektum átadása toohello *DnsManagementClient.Zones.CreateOrUpdateAsyc* metódust.

> [!NOTE]
> DnsManagementClient művelet három módot támogat: szinkron ("CreateOrUpdate"), aszinkron ("CreateOrUpdateAsync"), vagy aszinkron hozzáférés toohello HTTP-válasz (CreateOrUpdateWithHttpMessagesAsync).  Attól függően, hogy az alkalmazások igényeihez ezek mód választható.

Az Azure DNS támogatja az egyidejű hozzáférések optimista, úgynevezett [ETag-EK](dns-getstarted-create-dnszone.md). A jelen példában adja meg "*" hello "If-None-Match" fejlécet közli az Azure DNS toocreate DNS-zóna Ha egy nem létezik a.  hello hívás sikertelen lesz, ha a zóna hello megadott névvel már létezik a megadott erőforráscsoport hello.

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

## <a name="create-dns-record-sets-and-records"></a>DNS-rekordhalmazok és rekordok létrehozása

DNS-rekordokat a rendszer kezeli, a rekordhalmaz. Rekordhalmaz olyan rekordokat hello ugyanazt a nevet, és jegyezze fel a zónán belül típusa.  hello rekordhalmaz nevébe relatív toohello zóna nevét, nem hello teljesen minősített DNS-nevét.

rekordhalmaz toocreate vagy frissítés, a "Rekordhalmaz" paraméterek objektum létrehozása és átadott túl*DnsManagementClient.RecordSets.CreateOrUpdateAsync*. Mivel a DNS-zóna esetén három működési módokat támadja: szinkron ("CreateOrUpdate"), aszinkron ("CreateOrUpdateAsync"), vagy aszinkron hozzáférés toohello HTTP-válasz (CreateOrUpdateWithHttpMessagesAsync).

Csakúgy, mint a DNS-zónák rekordhalmazok műveletek közé tartozik a hozzáférések optimista támogatása.  Ebben a példában sem az "If-Match", sem az "If-None-Match" meg van adva, mivel hello rekordhalmaz mindig létrejön.  A hívás felülírja hello ugyanazt a nevet, és jegyezze fel a bármely létező rekordot a DNS-zóna típusa.

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

## <a name="get-zones-and-record-sets"></a>Zónák és a rekordhalmazok

Hello *DnsManagementClient.Zones.Get* és *DnsManagementClient.RecordSets.Get* módszerek beolvasása az egyes zónák és a rekordhalmazok, illetve. Rekordhalmazok a típusa, a név és a léteznek az hello zóna, illetve erőforrás csoport azonosítják. A név és hello erőforráscsoport léteznek a zónák azonosítja.

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);
```

## <a name="update-an-existing-record-set"></a>Egy meglévő rekordhalmaz frissítése

egy meglévő DNS-rekordot tooupdate beállításához először kérjen le hello rekordhalmaz, majd a frissítés hello rekord tartalmát, akkor a hello módosítás elküldése.  Ebben a példában az "Etag" hello a lekért hello "If-Match" paraméter rekordhalmazt hello adtuk meg. hello hívás sikertelen lesz, ha egy párhuzamos művelet módosította hello rekordhalmazt hello addig is.

```cs
var recordSet = dnsClient.RecordSets.Get(resourceGroupName, zoneName, recordSetName, RecordType.A);

// Add a new record toohello local object.  Note that records in a record set must be unique/distinct
recordSet.ARecords.Add(new ARecord("5.6.7.8"));

// Update hello record set in Azure DNS
// Note: ETAG check specified, update will be rejected if hello record set has changed in hello meantime
recordSet = await dnsClient.RecordSets.CreateOrUpdateAsync(resourceGroupName, zoneName, recordSetName, RecordType.A, recordSet, recordSet.Etag);
```

## <a name="list-zones-and-record-sets"></a>Lista zóna és a rekordhalmazok

toolist zónák használatára hello *DnsManagementClient.Zones.List... * módszerek, amely támogatja az egy adott erőforráscsoportban található összes zónákat, vagy az adott Azure-előfizetés (között erőforrás csoportok.) toolist rekordhalmazok minden zóna listázása, használjon *DnsManagementClient.RecordSets.List... * módszereket támogatja, vagy egy adott zónában minden rekordkészletet vagy csak adott típusú ezen rekordhalmazok listázása.

Jegyezze fel a zónák listázásakor, és a rekordhalmazok eredmények lehet, hogy paginated.  a következő példa azt mutatja meg hogyan hello tooiterate hello oldalak az eredmények között. (Egy mesterségesen kis oldalméret "2" használt tooforce lapozás, a gyakorlatban ez a paraméter megadni, és használt alapértelmezett oldalméret hello.)

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

## <a name="next-steps"></a>Következő lépések

Töltse le a hello [Azure DNS .NET SDK mintaprojektet](https://www.microsoft.com/en-us/download/details.aspx?id=47268&WT.mc_id=DX_MVP4025064&e6b34bbe-475b-1abd-2c51-b5034bcdd6d2=True), mely tartalmazza a további példákat is, hogyan toouse hello Azure DNS .NET SDK-t, beleértve a példákat a más DNS-rekord típust.
