---
title: "aaaEnable nyilvános olvasási hozzáférés a tárolók és blobok az Azure Blob storage |} Microsoft Docs"
description: "Megtudhatja, hogyan toomake tárolók és blobok névtelen hozzáférés, elérhető, és hogyan tooaccess őket programozott módon."
services: storage
documentationcenter: 
author: mmacy
manager: timlt
editor: tysonn
ms.assetid: a2cffee6-3224-4f2a-8183-66ca23b2d2d7
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: marsma
ms.openlocfilehash: 0675b5dc4d32a3a0a34376ae4c049542b07ba03a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-anonymous-read-access-toocontainers-and-blobs"></a><span data-ttu-id="c0912-103">Kezelheti a névtelen olvasási hozzáférés toocontainers és blobok</span><span class="sxs-lookup"><span data-stu-id="c0912-103">Manage anonymous read access toocontainers and blobs</span></span>
<span data-ttu-id="c0912-104">Névtelen, nyilvános olvasási hozzáférés tooa tároló és a benne található blobokat az Azure Blob storage használatával engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="c0912-104">You can enable anonymous, public read access tooa container and its blobs in Azure Blob storage.</span></span> <span data-ttu-id="c0912-105">Ezzel a módszerrel csak olvasási hozzáféréssel toothese erőforrások megosztása a fiókkulcs nélkül, és anélkül, hogy a közös hozzáférésű jogosultságkód (SAS) adhat meg.</span><span class="sxs-lookup"><span data-stu-id="c0912-105">By doing so, you can grant read-only access toothese resources without sharing your account key, and without requiring a shared access signature (SAS).</span></span>

<span data-ttu-id="c0912-106">Nyilvános olvasási hozzáférés esetén ajánlott használni kívánt egyes blobok tooalways érhetők el a névtelen olvasási hozzáférés forgatókönyvek esetén.</span><span class="sxs-lookup"><span data-stu-id="c0912-106">Public read access is best for scenarios where you want certain blobs tooalways be available for anonymous read access.</span></span> <span data-ttu-id="c0912-107">Részletesebb vezérlést létrehozhat egy közös hozzáférésű jogosultságkódot.</span><span class="sxs-lookup"><span data-stu-id="c0912-107">For more fine-grained control, you can create a shared access signature.</span></span> <span data-ttu-id="c0912-108">Közös hozzáférésű jogosultságkód lehetővé teszik a korlátozott tooprovide hozzáférés eltérő engedélyekkel, egy adott időszakra vonatkozóan.</span><span class="sxs-lookup"><span data-stu-id="c0912-108">Shared access signatures enable you tooprovide restricted access using different permissions, for a specific time period.</span></span> <span data-ttu-id="c0912-109">Megosztott létrehozásával kapcsolatos további információért aláírások hozzáférni, lásd: [használata közös hozzáférésű jogosultságkód (SAS) az Azure Storage](storage-dotnet-shared-access-signature-part-1.md).</span><span class="sxs-lookup"><span data-stu-id="c0912-109">For more information about creating shared access signatures, see [Using shared access signatures (SAS) in Azure Storage](storage-dotnet-shared-access-signature-part-1.md).</span></span>

## <a name="grant-anonymous-users-permissions-toocontainers-and-blobs"></a><span data-ttu-id="c0912-110">Adja meg a névtelen felhasználók engedélyek toocontainers és blobok</span><span class="sxs-lookup"><span data-stu-id="c0912-110">Grant anonymous users permissions toocontainers and blobs</span></span>
<span data-ttu-id="c0912-111">Alapértelmezés szerint a tároló és szereplő bármely BLOB érhetik csak el hello tárfiók hello tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="c0912-111">By default, a container and any blobs within it may be accessed only by hello owner of hello storage account.</span></span> <span data-ttu-id="c0912-112">toogive névtelen felhasználók olvasási engedéllyel a tooa és a benne található blobokat, hello tároló engedélyek tooallow nyilvános hozzáférés állíthatja be.</span><span class="sxs-lookup"><span data-stu-id="c0912-112">toogive anonymous users read permissions tooa container and its blobs, you can set hello container permissions tooallow public access.</span></span> <span data-ttu-id="c0912-113">Névtelen felhasználók olvashatják a nyilvánosan elérhető tárolóban található blobok hello kérelem hitelesítése nélkül.</span><span class="sxs-lookup"><span data-stu-id="c0912-113">Anonymous users can read blobs within a publicly accessible container without authenticating hello request.</span></span>

<span data-ttu-id="c0912-114">Az alábbi engedélyek használata hello egy tárolót is konfigurálhatja:</span><span class="sxs-lookup"><span data-stu-id="c0912-114">You can configure a container with hello following permissions:</span></span>

* <span data-ttu-id="c0912-115">**Nincs nyilvános olvasási hozzáférés:** hello tároló és a benne található blobokat hozzáférhet csak hello tárfiók tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="c0912-115">**No public read access:** hello container and its blobs can be accessed only by hello storage account owner.</span></span> <span data-ttu-id="c0912-116">Ez a hello alapértelmezett az összes új tárolókat.</span><span class="sxs-lookup"><span data-stu-id="c0912-116">This is hello default for all new containers.</span></span>
* <span data-ttu-id="c0912-117">**Nyilvános olvasási hozzáférés a blobok csak:** hello tárolóban található Blobok névtelen kérelem által is olvasható, de adatai nem érhető el.</span><span class="sxs-lookup"><span data-stu-id="c0912-117">**Public read access for blobs only:** Blobs within hello container can be read by anonymous request, but container data is not available.</span></span> <span data-ttu-id="c0912-118">Névtelen ügyfeleket nem tudja felsorolni hello blobok hello tárolóban.</span><span class="sxs-lookup"><span data-stu-id="c0912-118">Anonymous clients cannot enumerate hello blobs within hello container.</span></span>
* <span data-ttu-id="c0912-119">**Teljes nyilvános olvasási hozzáférés:** minden tároló és a blob adatainak névtelen kérelem által olvasható.</span><span class="sxs-lookup"><span data-stu-id="c0912-119">**Full public read access:** All container and blob data can be read by anonymous request.</span></span> <span data-ttu-id="c0912-120">Ügyfelek hello tárolóban található blobok névtelen kérelem enumerálása, de nem tudja felsorolni a tárolók hello tárfiókon belül.</span><span class="sxs-lookup"><span data-stu-id="c0912-120">Clients can enumerate blobs within hello container by anonymous request, but cannot enumerate containers within hello storage account.</span></span>

<span data-ttu-id="c0912-121">Használhatja az alábbi tooset tároló engedélyek hello:</span><span class="sxs-lookup"><span data-stu-id="c0912-121">You can use hello following tooset container permissions:</span></span>

* [<span data-ttu-id="c0912-122">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c0912-122">Azure portal</span></span>](https://portal.azure.com)
* [<span data-ttu-id="c0912-123">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="c0912-123">Azure PowerShell</span></span>](storage-powershell-guide-full.md#how-to-manage-azure-blobs)
* [<span data-ttu-id="c0912-124">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="c0912-124">Azure CLI 2.0</span></span>](storage-azure-cli.md#create-and-manage-blobs)
* <span data-ttu-id="c0912-125">Programozott módon egyikével hello storage ügyfélkódtáraival vagy hello REST API-n</span><span class="sxs-lookup"><span data-stu-id="c0912-125">Programmatically, by using one of hello storage client libraries or hello REST API</span></span>

### <a name="set-container-permissions-in-hello-azure-portal"></a><span data-ttu-id="c0912-126">Hello Azure-portálon a tároló engedélyeinek beállítása</span><span class="sxs-lookup"><span data-stu-id="c0912-126">Set container permissions in hello Azure portal</span></span>
<span data-ttu-id="c0912-127">hello tooset tároló engedélyek [Azure-portálon](https://portal.azure.com), kövesse az alábbi lépéseket:</span><span class="sxs-lookup"><span data-stu-id="c0912-127">tooset container permissions in hello [Azure portal](https://portal.azure.com), follow these steps:</span></span>

1. <span data-ttu-id="c0912-128">Nyissa meg a **tárfiók** hello portál panel.</span><span class="sxs-lookup"><span data-stu-id="c0912-128">Open your **Storage account** blade in hello portal.</span></span> <span data-ttu-id="c0912-129">A tárfiók található kiválasztásával **tárfiókok** hello portál főmenü panelen.</span><span class="sxs-lookup"><span data-stu-id="c0912-129">You can find your storage account by selecting **Storage accounts** in hello main portal menu blade.</span></span>
1. <span data-ttu-id="c0912-130">A **BLOB szolgáltatás** hello menü paneljén válassza **tárolók**.</span><span class="sxs-lookup"><span data-stu-id="c0912-130">Under **BLOB SERVICE** on hello menu blade, select **Containers**.</span></span>
1. <span data-ttu-id="c0912-131">Kattintson a jobb gombbal a hello tároló sor- vagy select hello három pont tooopen hello tároló **helyi menü**.</span><span class="sxs-lookup"><span data-stu-id="c0912-131">Right-click on hello container row or select hello ellipsis tooopen hello container's **Context menu**.</span></span>
1. <span data-ttu-id="c0912-132">Válassza ki **házirendhez** hello helyi menü.</span><span class="sxs-lookup"><span data-stu-id="c0912-132">Select **Access policy** in hello context menu.</span></span>
1. <span data-ttu-id="c0912-133">Válasszon egy **hozzáférési típus** hello a legördülő menü.</span><span class="sxs-lookup"><span data-stu-id="c0912-133">Select an **Access type** from hello drop down menu.</span></span>

    ![Szerkessze a Csomagtároló metaadatai párbeszédpanel](./media/storage-manage-access-to-resources/storage-manage-access-to-resources-0.png)

### <a name="set-container-permissions-with-net"></a><span data-ttu-id="c0912-135">A .NET tároló engedélyeinek beállítása</span><span class="sxs-lookup"><span data-stu-id="c0912-135">Set container permissions with .NET</span></span>
<span data-ttu-id="c0912-136">C# és a Storage ügyféloldali kódtára hello használata a .NET-hez, a tároló engedélyeinek tooset hello tároló meglévő engedélyek először lekérdezése által hívó hello **GetPermissions** metódust.</span><span class="sxs-lookup"><span data-stu-id="c0912-136">tooset permissions for a container using C# and hello Storage Client Library for .NET, first retrieve hello container's existing permissions by calling hello **GetPermissions** method.</span></span> <span data-ttu-id="c0912-137">Majd a készlet hello **PublicAccess** hello tulajdonsága **BlobContainerPermissions** hello által visszaadott objektum **GetPermissions** metódust.</span><span class="sxs-lookup"><span data-stu-id="c0912-137">Then set hello **PublicAccess** property for hello **BlobContainerPermissions** object that is returned by hello **GetPermissions** method.</span></span> <span data-ttu-id="c0912-138">Végezetül hívás hello **SetPermissions** hello metódus engedélyek frissítése.</span><span class="sxs-lookup"><span data-stu-id="c0912-138">Finally, call hello **SetPermissions** method with hello updated permissions.</span></span>

<span data-ttu-id="c0912-139">a következő példa hello toofull nyilvános olvasási hozzáférés hello tároló engedélyeket állít be.</span><span class="sxs-lookup"><span data-stu-id="c0912-139">hello following example sets hello container's permissions toofull public read access.</span></span> <span data-ttu-id="c0912-140">tooset engedélyek toopublic olvasási hozzáféréssel a blobok csak, állítsa be a hello **PublicAccess** tulajdonság túl**BlobContainerPublicAccessType.Blob**.</span><span class="sxs-lookup"><span data-stu-id="c0912-140">tooset permissions toopublic read access for blobs only, set hello **PublicAccess** property too**BlobContainerPublicAccessType.Blob**.</span></span> <span data-ttu-id="c0912-141">tooremove névtelen felhasználók minden engedélyeinek beállítása tulajdonság túl hello**BlobContainerPublicAccessType.Off**.</span><span class="sxs-lookup"><span data-stu-id="c0912-141">tooremove all permissions for anonymous users, set hello property too**BlobContainerPublicAccessType.Off**.</span></span>

```csharp
public static void SetPublicContainerPermissions(CloudBlobContainer container)
{
    BlobContainerPermissions permissions = container.GetPermissions();
    permissions.PublicAccess = BlobContainerPublicAccessType.Container;
    container.SetPermissions(permissions);
}
```

## <a name="access-containers-and-blobs-anonymously"></a><span data-ttu-id="c0912-142">Tárolók és blobok névtelen eléréséhez</span><span class="sxs-lookup"><span data-stu-id="c0912-142">Access containers and blobs anonymously</span></span>
<span data-ttu-id="c0912-143">Ügyfél tárolók és blobok névtelen hozzáféréshez használhatja a konstruktorok, amelyekhez nem szükséges hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="c0912-143">A client that accesses containers and blobs anonymously can use constructors that do not require credentials.</span></span> <span data-ttu-id="c0912-144">a következő példák hello többféleképpen tooreference Blob szolgáltatás erőforrások megjelenítése névtelenül.</span><span class="sxs-lookup"><span data-stu-id="c0912-144">hello following examples show a few different ways tooreference Blob service resources anonymously.</span></span>

### <a name="create-an-anonymous-client-object"></a><span data-ttu-id="c0912-145">Névtelen ügyfél objektum létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0912-145">Create an anonymous client object</span></span>
<span data-ttu-id="c0912-146">Létrehozhat egy új ügyfél szolgáltatásobjektum névtelen hozzáférés hello Blob-szolgáltatásvégpont hello fiók megadásával.</span><span class="sxs-lookup"><span data-stu-id="c0912-146">You can create a new service client object for anonymous access by providing hello Blob service endpoint for hello account.</span></span> <span data-ttu-id="c0912-147">Azonban is ismernie kell a tároló, amelyet a névtelen hozzáférés fiókhoz tartozó hello nevének.</span><span class="sxs-lookup"><span data-stu-id="c0912-147">However, you must also know hello name of a container in that account that's available for anonymous access.</span></span>

```csharp
public static void CreateAnonymousBlobClient()
{
    // Create hello client object using hello Blob service endpoint.
    CloudBlobClient blobClient = new CloudBlobClient(new Uri(@"https://storagesample.blob.core.windows.net"));

    // Get a reference tooa container that's available for anonymous access.
    CloudBlobContainer container = blobClient.GetContainerReference("sample-container");

    // Read hello container's properties. Note this is only possible when hello container supports full public read access.
    container.FetchAttributes();
    Console.WriteLine(container.Properties.LastModified);
    Console.WriteLine(container.Properties.ETag);
}
```

### <a name="reference-a-container-anonymously"></a><span data-ttu-id="c0912-148">Egy tároló névtelenül hivatkozik</span><span class="sxs-lookup"><span data-stu-id="c0912-148">Reference a container anonymously</span></span>
<span data-ttu-id="c0912-149">Ha hello URL-cím tooa tároló, amely névtelenül érhető el, használhatja tooreference hello tároló közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="c0912-149">If you have hello URL tooa container that is anonymously available, you can use it tooreference hello container directly.</span></span>

```csharp
public static void ListBlobsAnonymously()
{
    // Get a reference tooa container that's available for anonymous access.
    CloudBlobContainer container = new CloudBlobContainer(new Uri(@"https://storagesample.blob.core.windows.net/sample-container"));

    // List blobs in hello container.
    foreach (IListBlobItem blobItem in container.ListBlobs())
    {
        Console.WriteLine(blobItem.Uri);
    }
}
```

### <a name="reference-a-blob-anonymously"></a><span data-ttu-id="c0912-150">Egy blob névtelenül hivatkozik</span><span class="sxs-lookup"><span data-stu-id="c0912-150">Reference a blob anonymously</span></span>
<span data-ttu-id="c0912-151">Ha hello URL-cím tooa blob, amelyet a névtelen hozzáférés, melyeket referenciaként használhat hello blob közvetlenül az adott URL-cím segítségével:</span><span class="sxs-lookup"><span data-stu-id="c0912-151">If you have hello URL tooa blob that is available for anonymous access, you can reference hello blob directly using that URL:</span></span>

```csharp
public static void DownloadBlobAnonymously()
{
    CloudBlockBlob blob = new CloudBlockBlob(new Uri(@"https://storagesample.blob.core.windows.net/sample-container/logfile.txt"));
    blob.DownloadToFile(@"C:\Temp\logfile.txt", System.IO.FileMode.Create);
}
```

## <a name="features-available-tooanonymous-users"></a><span data-ttu-id="c0912-152">Szolgáltatások rendelkezésre tooanonymous felhasználók</span><span class="sxs-lookup"><span data-stu-id="c0912-152">Features available tooanonymous users</span></span>
<span data-ttu-id="c0912-153">a következő táblázat hello jeleníti meg, milyen műveletek előfordulhat, hogy hívható névtelen felhasználók számára, ha a tároló hozzáférés-vezérlési lista tooallow nyilvános hozzáférés van beállítva.</span><span class="sxs-lookup"><span data-stu-id="c0912-153">hello following table shows which operations may be called by anonymous users when a container's ACL is set tooallow public access.</span></span>

| <span data-ttu-id="c0912-154">REST-művelet</span><span class="sxs-lookup"><span data-stu-id="c0912-154">REST Operation</span></span> | <span data-ttu-id="c0912-155">Teljes körű nyilvános olvasási joggal rendelkező engedély</span><span class="sxs-lookup"><span data-stu-id="c0912-155">Permission with full public read access</span></span> | <span data-ttu-id="c0912-156">A blobok csak nyilvános olvasási joggal rendelkező engedély</span><span class="sxs-lookup"><span data-stu-id="c0912-156">Permission with public read access for blobs only</span></span> |
| --- | --- | --- |
| <span data-ttu-id="c0912-157">Tárolók listája</span><span class="sxs-lookup"><span data-stu-id="c0912-157">List Containers</span></span> |<span data-ttu-id="c0912-158">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-158">Owner only</span></span> |<span data-ttu-id="c0912-159">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-159">Owner only</span></span> |
| <span data-ttu-id="c0912-160">Tároló létrehozása</span><span class="sxs-lookup"><span data-stu-id="c0912-160">Create Container</span></span> |<span data-ttu-id="c0912-161">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-161">Owner only</span></span> |<span data-ttu-id="c0912-162">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-162">Owner only</span></span> |
| <span data-ttu-id="c0912-163">A tároló tulajdonságainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="c0912-163">Get Container Properties</span></span> |<span data-ttu-id="c0912-164">Összes</span><span class="sxs-lookup"><span data-stu-id="c0912-164">All</span></span> |<span data-ttu-id="c0912-165">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-165">Owner only</span></span> |
| <span data-ttu-id="c0912-166">Tároló metaadatot beszerezni</span><span class="sxs-lookup"><span data-stu-id="c0912-166">Get Container Metadata</span></span> |<span data-ttu-id="c0912-167">Összes</span><span class="sxs-lookup"><span data-stu-id="c0912-167">All</span></span> |<span data-ttu-id="c0912-168">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-168">Owner only</span></span> |
| <span data-ttu-id="c0912-169">Állítsa be a Csomagtároló metaadatai</span><span class="sxs-lookup"><span data-stu-id="c0912-169">Set Container Metadata</span></span> |<span data-ttu-id="c0912-170">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-170">Owner only</span></span> |<span data-ttu-id="c0912-171">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-171">Owner only</span></span> |
| <span data-ttu-id="c0912-172">Tároló hozzáférés-vezérlési lista beolvasása</span><span class="sxs-lookup"><span data-stu-id="c0912-172">Get Container ACL</span></span> |<span data-ttu-id="c0912-173">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-173">Owner only</span></span> |<span data-ttu-id="c0912-174">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-174">Owner only</span></span> |
| <span data-ttu-id="c0912-175">Tároló hozzáférés-vezérlési lista beállítása</span><span class="sxs-lookup"><span data-stu-id="c0912-175">Set Container ACL</span></span> |<span data-ttu-id="c0912-176">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-176">Owner only</span></span> |<span data-ttu-id="c0912-177">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-177">Owner only</span></span> |
| <span data-ttu-id="c0912-178">Törli a tárolót</span><span class="sxs-lookup"><span data-stu-id="c0912-178">Delete Container</span></span> |<span data-ttu-id="c0912-179">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-179">Owner only</span></span> |<span data-ttu-id="c0912-180">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-180">Owner only</span></span> |
| <span data-ttu-id="c0912-181">Lista Blobok</span><span class="sxs-lookup"><span data-stu-id="c0912-181">List Blobs</span></span> |<span data-ttu-id="c0912-182">Összes</span><span class="sxs-lookup"><span data-stu-id="c0912-182">All</span></span> |<span data-ttu-id="c0912-183">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-183">Owner only</span></span> |
| <span data-ttu-id="c0912-184">Helyezze a Blob</span><span class="sxs-lookup"><span data-stu-id="c0912-184">Put Blob</span></span> |<span data-ttu-id="c0912-185">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-185">Owner only</span></span> |<span data-ttu-id="c0912-186">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-186">Owner only</span></span> |
| <span data-ttu-id="c0912-187">A Blob beolvasása</span><span class="sxs-lookup"><span data-stu-id="c0912-187">Get Blob</span></span> |<span data-ttu-id="c0912-188">Összes</span><span class="sxs-lookup"><span data-stu-id="c0912-188">All</span></span> |<span data-ttu-id="c0912-189">Összes</span><span class="sxs-lookup"><span data-stu-id="c0912-189">All</span></span> |
| <span data-ttu-id="c0912-190">A Blob tulajdonságainak beolvasása</span><span class="sxs-lookup"><span data-stu-id="c0912-190">Get Blob Properties</span></span> |<span data-ttu-id="c0912-191">Összes</span><span class="sxs-lookup"><span data-stu-id="c0912-191">All</span></span> |<span data-ttu-id="c0912-192">Összes</span><span class="sxs-lookup"><span data-stu-id="c0912-192">All</span></span> |
| <span data-ttu-id="c0912-193">A Blob tulajdonságainak beállítása</span><span class="sxs-lookup"><span data-stu-id="c0912-193">Set Blob Properties</span></span> |<span data-ttu-id="c0912-194">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-194">Owner only</span></span> |<span data-ttu-id="c0912-195">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-195">Owner only</span></span> |
| <span data-ttu-id="c0912-196">A Blob metaadatot beszerezni</span><span class="sxs-lookup"><span data-stu-id="c0912-196">Get Blob Metadata</span></span> |<span data-ttu-id="c0912-197">Összes</span><span class="sxs-lookup"><span data-stu-id="c0912-197">All</span></span> |<span data-ttu-id="c0912-198">Összes</span><span class="sxs-lookup"><span data-stu-id="c0912-198">All</span></span> |
| <span data-ttu-id="c0912-199">Állítsa be a Blob metaadatai</span><span class="sxs-lookup"><span data-stu-id="c0912-199">Set Blob Metadata</span></span> |<span data-ttu-id="c0912-200">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-200">Owner only</span></span> |<span data-ttu-id="c0912-201">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-201">Owner only</span></span> |
| <span data-ttu-id="c0912-202">PUT letiltása</span><span class="sxs-lookup"><span data-stu-id="c0912-202">Put Block</span></span> |<span data-ttu-id="c0912-203">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-203">Owner only</span></span> |<span data-ttu-id="c0912-204">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-204">Owner only</span></span> |
| <span data-ttu-id="c0912-205">(Csak a véglegesített blokkolja) tiltólista beolvasása</span><span class="sxs-lookup"><span data-stu-id="c0912-205">Get Block List (committed blocks only)</span></span> |<span data-ttu-id="c0912-206">Összes</span><span class="sxs-lookup"><span data-stu-id="c0912-206">All</span></span> |<span data-ttu-id="c0912-207">Összes</span><span class="sxs-lookup"><span data-stu-id="c0912-207">All</span></span> |
| <span data-ttu-id="c0912-208">(Csak a nem véglegesített blokkok vagy blokkok) tiltólista beolvasása</span><span class="sxs-lookup"><span data-stu-id="c0912-208">Get Block List (uncommitted blocks only or all blocks)</span></span> |<span data-ttu-id="c0912-209">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-209">Owner only</span></span> |<span data-ttu-id="c0912-210">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-210">Owner only</span></span> |
| <span data-ttu-id="c0912-211">PUT tiltólista</span><span class="sxs-lookup"><span data-stu-id="c0912-211">Put Block List</span></span> |<span data-ttu-id="c0912-212">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-212">Owner only</span></span> |<span data-ttu-id="c0912-213">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-213">Owner only</span></span> |
| <span data-ttu-id="c0912-214">A Blob törlése</span><span class="sxs-lookup"><span data-stu-id="c0912-214">Delete Blob</span></span> |<span data-ttu-id="c0912-215">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-215">Owner only</span></span> |<span data-ttu-id="c0912-216">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-216">Owner only</span></span> |
| <span data-ttu-id="c0912-217">A Blob másolása</span><span class="sxs-lookup"><span data-stu-id="c0912-217">Copy Blob</span></span> |<span data-ttu-id="c0912-218">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-218">Owner only</span></span> |<span data-ttu-id="c0912-219">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-219">Owner only</span></span> |
| <span data-ttu-id="c0912-220">Pillanatkép Blob</span><span class="sxs-lookup"><span data-stu-id="c0912-220">Snapshot Blob</span></span> |<span data-ttu-id="c0912-221">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-221">Owner only</span></span> |<span data-ttu-id="c0912-222">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-222">Owner only</span></span> |
| <span data-ttu-id="c0912-223">Címbérlet Blob</span><span class="sxs-lookup"><span data-stu-id="c0912-223">Lease Blob</span></span> |<span data-ttu-id="c0912-224">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-224">Owner only</span></span> |<span data-ttu-id="c0912-225">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-225">Owner only</span></span> |
| <span data-ttu-id="c0912-226">Helyezze a lap</span><span class="sxs-lookup"><span data-stu-id="c0912-226">Put Page</span></span> |<span data-ttu-id="c0912-227">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-227">Owner only</span></span> |<span data-ttu-id="c0912-228">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-228">Owner only</span></span> |
| <span data-ttu-id="c0912-229">Get tartományokat</span><span class="sxs-lookup"><span data-stu-id="c0912-229">Get Page Ranges</span></span> |<span data-ttu-id="c0912-230">Összes</span><span class="sxs-lookup"><span data-stu-id="c0912-230">All</span></span> |<span data-ttu-id="c0912-231">Összes</span><span class="sxs-lookup"><span data-stu-id="c0912-231">All</span></span> |
| <span data-ttu-id="c0912-232">Hozzáfűző Blob</span><span class="sxs-lookup"><span data-stu-id="c0912-232">Append Blob</span></span> |<span data-ttu-id="c0912-233">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-233">Owner only</span></span> |<span data-ttu-id="c0912-234">Csak a tulajdonos</span><span class="sxs-lookup"><span data-stu-id="c0912-234">Owner only</span></span> |

## <a name="next-steps"></a><span data-ttu-id="c0912-235">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c0912-235">Next steps</span></span>

* [<span data-ttu-id="c0912-236">Hello Azure Storage szolgáltatásainak hitelesítése</span><span class="sxs-lookup"><span data-stu-id="c0912-236">Authentication for hello Azure Storage Services</span></span>](https://msdn.microsoft.com/library/azure/dd179428.aspx)
* [<span data-ttu-id="c0912-237">Közös hozzáférésű Jogosultságkód (SAS) használatával</span><span class="sxs-lookup"><span data-stu-id="c0912-237">Using Shared Access Signatures (SAS)</span></span>](storage-dotnet-shared-access-signature-part-1.md)
* [<span data-ttu-id="c0912-238">Hozzáférés delegálása közös hozzáférésű jogosultságkód használatával</span><span class="sxs-lookup"><span data-stu-id="c0912-238">Delegating Access with a Shared Access Signature</span></span>](https://msdn.microsoft.com/library/azure/ee395415.aspx)
