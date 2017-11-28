---
title: "aaaImplement vész-helyreállítási használatával biztonsági mentése és visszaállítása az Azure API Management |} Microsoft Docs"
description: "Megtudhatja, hogyan toouse biztonsági mentése és visszaállítása az Azure API Management tooperform katasztrófa utáni helyreállítás."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 6f10be3c-f796-4a6c-bacd-7931b6aa82af
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 058bfb579e3a3f51fb1dac8ea37eb4fdbc83a4ad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooimplement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a><span data-ttu-id="1d2be-103">Hogyan tooimplement vész-helyreállítási segítségével biztonsági mentés és a szolgáltatás visszaállítása az Azure API Management</span><span class="sxs-lookup"><span data-stu-id="1d2be-103">How tooimplement disaster recovery using service backup and restore in Azure API Management</span></span>
<span data-ttu-id="1d2be-104">Az toopublish kiválasztása és kezelése az API-k ki sok hiba tűréshatár és infrastruktúra képességeket, amelyeket egyébként külön toodesign, megvalósítása és kezelése Azure API Management szolgáltatáson keresztül.</span><span class="sxs-lookup"><span data-stu-id="1d2be-104">By choosing toopublish and manage your APIs via Azure API Management you are taking advantage of many fault tolerance and infrastructure capabilities that you would otherwise have toodesign, implement, and manage.</span></span> <span data-ttu-id="1d2be-105">hello Azure platformon csökkenti a lehetséges hibák hello költség töredéke alatt, nagy része.</span><span class="sxs-lookup"><span data-stu-id="1d2be-105">hello Azure platform mitigates a large fraction of potential failures at a fraction of hello cost.</span></span>

<span data-ttu-id="1d2be-106">rendelkezésre állási problémákhoz hello terület, ahol az API Management szolgáltatást érintő toorecover üzemeltetett meg kell tooreconstitute készen áll a szolgáltatás egy másik régióban bármikor.</span><span class="sxs-lookup"><span data-stu-id="1d2be-106">toorecover from availability problems affecting hello region where your API Management service is hosted you should be ready tooreconstitute your service in a different region at any time.</span></span> <span data-ttu-id="1d2be-107">Attól függően, hogy a rendelkezésre állási célokat és a helyreállítási idő célkitűzése előfordulhat, hogy szeretné, hogy egy biztonsági mentési szolgáltatás egy vagy több régióban tooreserve, és próbálja meg toomaintain, a konfiguráció és a tartalom szinkronban vannak hello aktív szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="1d2be-107">Depending on your availability goals and recovery time objective  you might want tooreserve a backup service in one or more regions and try toomaintain their configuration and content in sync with hello active service.</span></span> <span data-ttu-id="1d2be-108">hello szolgáltatás biztonsági mentési és visszaállítási funkciót biztosít a hello szükséges építőelem végrehajtási vész-helyreállítási stratégiát.</span><span class="sxs-lookup"><span data-stu-id="1d2be-108">hello service backup and restore feature provides hello necessary building block for implementing your disaster recovery strategy.</span></span>

<span data-ttu-id="1d2be-109">Ez az útmutató bemutatja, hogyan tooauthenticate Azure Resource Manager kér, és hogyan toobackup és az API Management szolgáltatáspéldány visszaállítása.</span><span class="sxs-lookup"><span data-stu-id="1d2be-109">This guide shows how tooauthenticate Azure Resource Manager requests, and how toobackup and restore your API Management service instances.</span></span>

> [!NOTE]
> <span data-ttu-id="1d2be-110">hello biztonsági mentése és visszaállítása egy API-kezelés szolgáltatáspéldány vész-helyreállítási folyamata a API-kezelés szolgáltatás példányainak ahhoz hasonló forgatókönyvek esetén átmeneti replikálására is használható.</span><span class="sxs-lookup"><span data-stu-id="1d2be-110">hello process for backing up and restoring an API Management service instance for disaster recovery can also be used for replicating API Management service instances for scenarios such as staging.</span></span>
>
> <span data-ttu-id="1d2be-111">Vegye figyelembe, hogy minden egyes biztonsági másolat 30 nap múlva lejár.</span><span class="sxs-lookup"><span data-stu-id="1d2be-111">Note that each backup expires after 30 days.</span></span> <span data-ttu-id="1d2be-112">Ha a biztonsági mentés toorestore hello 30 napos lejárati időszak lejárta után, hello visszaállítása sikertelen lesz, és egy `Cannot restore: backup expired` üzenet.</span><span class="sxs-lookup"><span data-stu-id="1d2be-112">If you attempt toorestore a backup after hello 30 day expiration period has expired, hello restore will fail with a `Cannot restore: backup expired` message.</span></span>
>
>

## <a name="authenticating-azure-resource-manager-requests"></a><span data-ttu-id="1d2be-113">Hitelesítő Azure Resource Manager-kérelmek</span><span class="sxs-lookup"><span data-stu-id="1d2be-113">Authenticating Azure Resource Manager requests</span></span>
> [!IMPORTANT]
> <span data-ttu-id="1d2be-114">hello REST API-t a biztonsági mentési és visszaállítási Azure Resource Managert használja, és különböző hitelesítési mechanizmussal rendelkezik, mint a REST API-k kezelése az API Management entitások hello.</span><span class="sxs-lookup"><span data-stu-id="1d2be-114">hello REST API for backup and restore uses Azure Resource Manager and has a different authentication mechanism than hello REST APIs for managing your API Management entities.</span></span> <span data-ttu-id="1d2be-115">Ebben a szakaszban hello lépések bemutatják, hogyan tooauthenticate Azure Resource Manager kéri.</span><span class="sxs-lookup"><span data-stu-id="1d2be-115">hello steps in this section describe how tooauthenticate Azure Resource Manager requests.</span></span> <span data-ttu-id="1d2be-116">További információkért lásd: [kérelmek hitelesítéséhez az Azure Resource Manager](http://msdn.microsoft.com/library/azure/dn790557.aspx).</span><span class="sxs-lookup"><span data-stu-id="1d2be-116">For more information, see [Authenticating Azure Resource Manager requests](http://msdn.microsoft.com/library/azure/dn790557.aspx).</span></span>
>
>

<span data-ttu-id="1d2be-117">Arról, hogy a hello Azure Resource Manager eszközzel hello feladatokat az Azure Active Directoryban a lépéseket követve hello hitelesíteni kell.</span><span class="sxs-lookup"><span data-stu-id="1d2be-117">All of hello tasks that you do on resources using hello Azure Resource Manager must be authenticated with Azure Active Directory using hello following steps.</span></span>

* <span data-ttu-id="1d2be-118">Adja hozzá az alkalmazás toohello Azure Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="1d2be-118">Add an application toohello Azure Active Directory tenant.</span></span>
* <span data-ttu-id="1d2be-119">Hello alkalmazáshoz hozzáadott engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="1d2be-119">Set permissions for hello application that you added.</span></span>
* <span data-ttu-id="1d2be-120">Hello jogkivonat lekérése kérelmek tooAzure erőforrás-kezelő hitelesítéséhez.</span><span class="sxs-lookup"><span data-stu-id="1d2be-120">Get hello token for authenticating requests tooAzure Resource Manager.</span></span>

<span data-ttu-id="1d2be-121">első lépés hello toocreate egy Azure Active Directory-alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="1d2be-121">hello first step is toocreate an Azure Active Directory application.</span></span> <span data-ttu-id="1d2be-122">Jelentkezzen be a hello [klasszikus Azure portál](http://manage.windowsazure.com/) hello az előfizetést, amely tartalmazza az API Management service-példány, és keresse meg a toohello **alkalmazások** fülre az alapértelmezett Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="1d2be-122">Log into hello [Azure Classic Portal](http://manage.windowsazure.com/) using hello subscription that contains your API Management service instance and navigate toohello **Applications** tab for your default Azure Active Directory.</span></span>

> [!NOTE]
> <span data-ttu-id="1d2be-123">Ha nem látható tooyour fiók hello Azure Active Directory alapértelmezett címtár, hello Azure-előfizetés toogrant hello kapcsolattartási hello rendszergazdája szükséges engedélyek tooyour fiók.</span><span class="sxs-lookup"><span data-stu-id="1d2be-123">If hello Azure Active Directory default directory is not visible tooyour account, contact hello administrator of hello Azure subscription toogrant hello required permissions tooyour account.</span></span>

![Azure Active Directory-alkalmazás létrehozása][api-management-add-aad-application]

<span data-ttu-id="1d2be-125">Kattintson a **Hozzáadás**, **a szerveztem által fejlesztett alkalmazás hozzáadása**, és válassza a **natív ügyfélalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="1d2be-125">Click **Add**, **Add an application my organization is developing**, and choose **Native client application**.</span></span> <span data-ttu-id="1d2be-126">Adjon meg egy leíró nevet, majd kattintson a Tovább nyílra hello.</span><span class="sxs-lookup"><span data-stu-id="1d2be-126">Enter a descriptive name, and click hello next arrow.</span></span> <span data-ttu-id="1d2be-127">Írjon be egy helyőrző URL-címet, például `http://resources` a hello **átirányítási URI-**, mert a mezőt kötelező kitölteni, de hello értéket később nem használja.</span><span class="sxs-lookup"><span data-stu-id="1d2be-127">Enter a placeholder URL such as `http://resources` for hello **Redirect URI**, as it is a required field, but hello value is not used later.</span></span> <span data-ttu-id="1d2be-128">Kattintson a hello jelölőnégyzetet toosave hello alkalmazásra.</span><span class="sxs-lookup"><span data-stu-id="1d2be-128">Click hello check box toosave hello application.</span></span>

<span data-ttu-id="1d2be-129">Miután hello alkalmazás mentettük, kattintson a **konfigurálása**, toohello görgetve **engedélyek tooother alkalmazások** szakaszt, és kattintson a **alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="1d2be-129">Once hello application is saved, click **Configure**, scroll down toohello **permissions tooother applications** section, and click **Add application**.</span></span>

![Engedélyek hozzáadása][api-management-aad-permissions-add]

<span data-ttu-id="1d2be-131">Válassza ki **Windows** **Azure szolgáltatásfelügyeleti API** kattintson hello jelölőnégyzet tooadd hello alkalmazásra.</span><span class="sxs-lookup"><span data-stu-id="1d2be-131">Select **Windows** **Azure Service Management API** and click hello checkbox tooadd hello application.</span></span>

![Engedélyek hozzáadása][api-management-aad-permissions]

<span data-ttu-id="1d2be-133">Kattintson a **delegált engedélyek** mellett az újonnan hozzáadott hello **Windows** **Azure szolgáltatásfelügyeleti API** alkalmazás, hello jelölőnégyzetét **Access Azure Service Management (preview)**, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="1d2be-133">Click **Delegated Permissions** beside hello newly added **Windows** **Azure Service Management API** application, check hello box for **Access Azure Service Management (preview)**, and click **Save**.</span></span>

![Engedélyek hozzáadása][api-management-aad-delegated-permissions]

<span data-ttu-id="1d2be-135">Előzetes tooinvoking hello API-k, amelyek létrehozzák hello biztonsági mentési, majd állítsa vissza azt, a szükséges tooget jogkivonatot.</span><span class="sxs-lookup"><span data-stu-id="1d2be-135">Prior tooinvoking hello APIs that generate hello backup and restore it, it is necessary tooget a token.</span></span> <span data-ttu-id="1d2be-136">hello alábbi példában hello [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) nuget csomag tooretrieve hello jogkivonat.</span><span class="sxs-lookup"><span data-stu-id="1d2be-136">hello following example uses hello [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) nuget package tooretrieve hello token.</span></span>

```c#
using Microsoft.IdentityModel.Clients.ActiveDirectory;
using System;

namespace GetTokenResourceManagerRequests
{
    class Program
    {
        static void Main(string[] args)
        {
            var authenticationContext = new AuthenticationContext("https://login.microsoftonline.com/{tenant id}");
            var result = authenticationContext.AcquireToken("https://management.azure.com/", {application id}, new Uri({redirect uri});

            if (result == null) {
                throw new InvalidOperationException("Failed tooobtain hello JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

<span data-ttu-id="1d2be-137">Cserélje le `{tentand id}`, `{application id}`, és `{redirect uri}` utasításai hello segítségével.</span><span class="sxs-lookup"><span data-stu-id="1d2be-137">Replace `{tentand id}`, `{application id}`, and `{redirect uri}` using hello following instructions.</span></span>

<span data-ttu-id="1d2be-138">Cserélje le `{tenant id}` hello bérlői azonosítójú hello Azure Active Directory-alkalmazás imént létrehozott.</span><span class="sxs-lookup"><span data-stu-id="1d2be-138">Replace `{tenant id}` with hello tenant id of hello Azure Active Directory application you just created.</span></span> <span data-ttu-id="1d2be-139">Hello azonosító eléréséhez kattintson **végpontok megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="1d2be-139">You can access hello id by clicking **View endpoints**.</span></span>

![Végpontok][api-management-aad-default-directory]

![Végpontok][api-management-endpoint]

<span data-ttu-id="1d2be-142">Cserélje le `{application id}` és `{redirect uri}` hello segítségével **ügyfél-azonosító** és URL-címet a hello hello **átirányítási URI-azonosítók** szakaszban az Azure Active Directory-alkalmazás **konfigurálása**  fülre.</span><span class="sxs-lookup"><span data-stu-id="1d2be-142">Replace `{application id}` and `{redirect uri}` using hello **Client Id** and  hello URL from hello **Redirect Uris** section from your Azure Active Directory application's **Configure** tab.</span></span>

![Erőforrások][api-management-aad-resources]

<span data-ttu-id="1d2be-144">Miután hello értékek vannak megadva, hello példakód a következő példa egy token hasonló toohello kell visszaadnia.</span><span class="sxs-lookup"><span data-stu-id="1d2be-144">Once hello values are specified, hello code example should return a token similar toohello following example.</span></span>

![Token][api-management-arm-token]

<span data-ttu-id="1d2be-146">Mielőtt hívása hello biztonsági mentési és visszaállítási hello a következő részekben leírt műveleteket, állítsa be a hello engedélyezési kérelem fejléce a REST-hívást.</span><span class="sxs-lookup"><span data-stu-id="1d2be-146">Before calling hello backup and restore operations described in hello following sections, set hello authorization request header for your REST call.</span></span>

```c#
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

## <span data-ttu-id="1d2be-147"><a name="step1"></a>Készítsen biztonsági másolatot egy API-kezelés szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="1d2be-147"><a name="step1"> </a>Back up an API Management service</span></span>
<span data-ttu-id="1d2be-148">tooback fel az API Management szolgáltatás probléma hello HTTP-kérelem a következő:</span><span class="sxs-lookup"><span data-stu-id="1d2be-148">tooback up an API Management service issue hello following HTTP request:</span></span>

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

<span data-ttu-id="1d2be-149">Ahol:</span><span class="sxs-lookup"><span data-stu-id="1d2be-149">where:</span></span>

* <span data-ttu-id="1d2be-150">`subscriptionId`-toobackup próbált hello API Management szolgáltatást tartalmazó hello előfizetés azonosítója</span><span class="sxs-lookup"><span data-stu-id="1d2be-150">`subscriptionId` - id of hello subscription containing hello API Management service you are attempting toobackup</span></span>
* <span data-ttu-id="1d2be-151">`resourceGroupName`-hello képernyőn "API - alapértelmezett-{szolgáltatás-régió}" karakterlánc ahol `service-region` hello Azure-régió, ahol hello API-kezelés szolgáltatás, amelyhez toobackup tárolása, pl. azonosítja`North-Central-US`</span><span class="sxs-lookup"><span data-stu-id="1d2be-151">`resourceGroupName` - a string in hello form of 'Api-Default-{service-region}' where `service-region` identifies hello Azure region where hello API Management service you are trying toobackup is hosted, e.g. `North-Central-US`</span></span>
* <span data-ttu-id="1d2be-152">`serviceName`-hello hello végez biztonsági másolatot, ahol létrehozták hello időpontban megadott API-kezelés szolgáltatás neve</span><span class="sxs-lookup"><span data-stu-id="1d2be-152">`serviceName` - hello name of hello API Management service you are making a backup of specified at hello time of its creation</span></span>
* <span data-ttu-id="1d2be-153">`api-version`-cseréje`2014-02-14`</span><span class="sxs-lookup"><span data-stu-id="1d2be-153">`api-version` - replace  with `2014-02-14`</span></span>

<span data-ttu-id="1d2be-154">A hello hello kérelem törzse adja meg a hello cél az Azure storage-fiók neve, a hozzáférési kulcsot, a blob-tároló neve és a biztonsági másolat neve:</span><span class="sxs-lookup"><span data-stu-id="1d2be-154">In hello body of hello request, specify hello target Azure storage account name, access key, blob container name, and backup name:</span></span>

```
'{  
    storageAccount : {storage account name for hello backup},  
    accessKey : {access key for hello account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

<span data-ttu-id="1d2be-155">Állítsa be hello hello `Content-Type` kérelemfejléc túl`application/json`.</span><span class="sxs-lookup"><span data-stu-id="1d2be-155">Set hello value of hello `Content-Type` request header too`application/json`.</span></span>

<span data-ttu-id="1d2be-156">Biztonsági mentés egy hosszú ideig futó művelet, amely több percig toocomplete is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="1d2be-156">Backup is a long running operation that may take multiple minutes toocomplete.</span></span>  <span data-ttu-id="1d2be-157">Ha hello kérés sikeres volt, és biztonsági mentési folyamat hello kezdeményezett kapni fog egy `202 Accepted` a válasz állapotkódja egy `Location` fejléc.</span><span class="sxs-lookup"><span data-stu-id="1d2be-157">If hello request was successful and hello backup process was initiated you’ll receive a `202 Accepted` response status code with a `Location` header.</span></span>  <span data-ttu-id="1d2be-158">Ellenőrizze a "GET" kérelmek hello toohello URL-címet `Location` fejléc toofind kimenő hello művelet hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="1d2be-158">Make 'GET' requests toohello URL in hello `Location` header toofind out hello status of hello operation.</span></span> <span data-ttu-id="1d2be-159">Hello biztonsági mentés közben továbbra tooreceive 202 elfogadott állapotkódot.</span><span class="sxs-lookup"><span data-stu-id="1d2be-159">While hello backup is in progress you will continue tooreceive a '202 Accepted' status code.</span></span> <span data-ttu-id="1d2be-160">Válaszkód `200 OK` fogja jelezni hello biztonsági mentési művelet sikeres befejezését.</span><span class="sxs-lookup"><span data-stu-id="1d2be-160">A Response code of `200 OK` will indicate successful completion of hello backup operation.</span></span>

<span data-ttu-id="1d2be-161">Vegye figyelembe a következő korlátozások, amikor egy biztonsági mentési kérést hello.</span><span class="sxs-lookup"><span data-stu-id="1d2be-161">Please note hello following constraints when making a backup request.</span></span>

* <span data-ttu-id="1d2be-162">**Tároló** hello kérés törzsében megadott **léteznie kell**.</span><span class="sxs-lookup"><span data-stu-id="1d2be-162">**Container** specified in hello request body **must exist**.</span></span>
* <span data-ttu-id="1d2be-163">Biztonsági mentés folyamatban, amíg **nem szabadna megpróbálniuk a szolgáltatás felügyeleti műveleteket sem** SKU frissítése vagy alacsonyabb szintre való visszalépést, a tartomány kiszolgálónév-változás, például stb.</span><span class="sxs-lookup"><span data-stu-id="1d2be-163">While backup is in progress you **should not attempt any service management operations** such as SKU upgrade or downgrade, domain name change, etc.</span></span>
* <span data-ttu-id="1d2be-164">A visszaállítás egy **biztonsági mentés csak 30 napig garantáltan** hello pillanattól a létrehozása óta.</span><span class="sxs-lookup"><span data-stu-id="1d2be-164">Restore of a **backup is guaranteed only for 30 days** since hello moment of its creation.</span></span>
* <span data-ttu-id="1d2be-165">**Használati adatok** elemzési jelentések létrehozásához használt **nincs megadva** hello biztonsági mentése.</span><span class="sxs-lookup"><span data-stu-id="1d2be-165">**Usage data** used for creating analytics reports **is not included** in hello backup.</span></span> <span data-ttu-id="1d2be-166">Használjon [Azure API Management REST API] [ Azure API Management REST API] tooperiodically beolvasni elemzés jelentések meg van őrizve.</span><span class="sxs-lookup"><span data-stu-id="1d2be-166">Use [Azure API Management REST API][Azure API Management REST API] tooperiodically retrieve analytics reports for safekeeping.</span></span>
* <span data-ttu-id="1d2be-167">hello gyakoriság, amellyel elvégezhető a szolgáltatás biztonsági mentések hatással lesz a helyreállításipont-célkitűzést.</span><span class="sxs-lookup"><span data-stu-id="1d2be-167">hello frequency with which you perform service backups will affect your recovery point objective.</span></span> <span data-ttu-id="1d2be-168">azt javasoljuk van, rendszeres biztonsági mentések végrehajtására, valamint a fontos elvégzése után igény szerinti biztonsági mentést végez toominimize tooyour API-kezelés szolgáltatás változik.</span><span class="sxs-lookup"><span data-stu-id="1d2be-168">toominimize it we advise implementing regular backups as well as performing on-demand backups after making important changes tooyour API Management service.</span></span>
* <span data-ttu-id="1d2be-169">**Módosítások** készült toohello szolgáltatáskonfiguráció (pl. API-k, házirendek, fejlesztői portál megjelenését) biztonsági mentési művelet során van folyamatban **hello biztonsági mentés nem tartalmazhatja, és ezért elvész**.</span><span class="sxs-lookup"><span data-stu-id="1d2be-169">**Changes** made toohello service configuration (e.g. APIs, policies, developer portal appearance) while backup operation is in process **might not be included in hello backup and therefore will be lost**.</span></span>

## <span data-ttu-id="1d2be-170"><a name="step2"></a>Egy API-kezelés szolgáltatás visszaállítása</span><span class="sxs-lookup"><span data-stu-id="1d2be-170"><a name="step2"> </a>Restore an API Management service</span></span>
<span data-ttu-id="1d2be-171">az API Management szolgáltatásnak, egy korábban létrehozott biztonsági másolatból toorestore győződjön hello HTTP-kérelem a következő:</span><span class="sxs-lookup"><span data-stu-id="1d2be-171">toorestore an API Management service from a previously created backup make hello following HTTP request:</span></span>

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

<span data-ttu-id="1d2be-172">Ahol:</span><span class="sxs-lookup"><span data-stu-id="1d2be-172">where:</span></span>

* <span data-ttu-id="1d2be-173">`subscriptionId`-hello API-kezelés szolgáltatás visszaállítását végzi, a biztonsági másolatot tartalmazó hello előfizetés azonosítója</span><span class="sxs-lookup"><span data-stu-id="1d2be-173">`subscriptionId` - id of hello subscription containing hello API Management service you are restoring a backup into</span></span>
* <span data-ttu-id="1d2be-174">`resourceGroupName`-hello képernyőn "API - alapértelmezett-{szolgáltatás-régió}" karakterlánc ahol `service-region` hello Azure-régió, ahol hello API-kezelés szolgáltatás visszaállítását végzi, a biztonsági másolat tárolása, pl. azonosítja`North-Central-US`</span><span class="sxs-lookup"><span data-stu-id="1d2be-174">`resourceGroupName` - a string in hello form of 'Api-Default-{service-region}' where `service-region` identifies hello Azure region where hello API Management service you are restoring a backup into is hosted, e.g. `North-Central-US`</span></span>
* <span data-ttu-id="1d2be-175">`serviceName`-hello API-kezelés szolgáltatás a visszaállítandó megadott hello időpontban, ahol létrehozták hello neve</span><span class="sxs-lookup"><span data-stu-id="1d2be-175">`serviceName` - hello name of hello API Management service being restored into specified at hello time of its creation</span></span>
* <span data-ttu-id="1d2be-176">`api-version`-cseréje`2014-02-14`</span><span class="sxs-lookup"><span data-stu-id="1d2be-176">`api-version` - replace  with `2014-02-14`</span></span>

<span data-ttu-id="1d2be-177">Hello hello kérelem törzse adja meg a hello biztonságimásolat-fájl helyét, azaz az Azure storage-fiók neve, a hozzáférési kulcsot, a blob-tároló neve és biztonsági másolat neve:</span><span class="sxs-lookup"><span data-stu-id="1d2be-177">In hello body of hello request, specify hello backup file location, i.e. Azure storage account name, access key, blob container name, and backup name:</span></span>

```
'{  
    storageAccount : {storage account name for hello backup},  
    accessKey : {access key for hello account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

<span data-ttu-id="1d2be-178">Állítsa be hello hello `Content-Type` kérelemfejléc túl`application/json`.</span><span class="sxs-lookup"><span data-stu-id="1d2be-178">Set hello value of hello `Content-Type` request header too`application/json`.</span></span>

<span data-ttu-id="1d2be-179">Visszaállítás egy hosszú ideig futó művelet, amely too30 vagy több percig toocomplete is tarthat.</span><span class="sxs-lookup"><span data-stu-id="1d2be-179">Restore is a long running operation that may take up too30 or more minutes toocomplete.</span></span>  <span data-ttu-id="1d2be-180">Ha hello kérés sikeres volt, és hello visszaállítási folyamat kezdeményezett kapni fog egy `202 Accepted` a válasz állapotkódja egy `Location` fejléc.</span><span class="sxs-lookup"><span data-stu-id="1d2be-180">If hello request was successful and hello restore process was initiated you’ll receive a `202 Accepted` response status code with a `Location` header.</span></span>  <span data-ttu-id="1d2be-181">Ellenőrizze a "GET" kérelmek hello toohello URL-címet `Location` fejléc toofind kimenő hello művelet hello állapotát.</span><span class="sxs-lookup"><span data-stu-id="1d2be-181">Make 'GET' requests toohello URL in hello `Location` header toofind out hello status of hello operation.</span></span> <span data-ttu-id="1d2be-182">Hello visszaállítása közben továbbra tooreceive '202 elfogadott' állapotkód.</span><span class="sxs-lookup"><span data-stu-id="1d2be-182">While hello restore is in progress you will continue tooreceive '202 Accepted' status code.</span></span> <span data-ttu-id="1d2be-183">Válaszkód `200 OK` fogja jelezni hello visszaállítási művelet sikeres befejezését.</span><span class="sxs-lookup"><span data-stu-id="1d2be-183">A response code of `200 OK` will indicate successful completion of hello restore operation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1d2be-184">**hello SKU** hello szolgáltatást a visszaállítandó **meg kell egyeznie** hello hello Termékváltozata biztonsági másolat szolgáltatás visszaállítása folyamatban.</span><span class="sxs-lookup"><span data-stu-id="1d2be-184">**hello SKU** of hello service being restored into **must match** hello SKU of hello backed up service being restored.</span></span>
>
> <span data-ttu-id="1d2be-185">**Módosítások** készült toohello szolgáltatáskonfiguráció (pl. API-k, házirendek, fejlesztői portál megjelenését) visszaállítási művelet során folyamatban van a **felülíródnak**.</span><span class="sxs-lookup"><span data-stu-id="1d2be-185">**Changes** made toohello service configuration (e.g. APIs, policies, developer portal appearance) while restore operation is in progress **could be overwritten**.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="1d2be-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1d2be-186">Next steps</span></span>
<span data-ttu-id="1d2be-187">Tekintse meg a következő Microsoft blogok hello biztonsági mentési/visszaállítási folyamat két különböző forgatókönyvek a hello.</span><span class="sxs-lookup"><span data-stu-id="1d2be-187">Check out hello following Microsoft blogs for two different walkthroughs of hello backup/restore process.</span></span>

* [<span data-ttu-id="1d2be-188">Az Azure API Management fiókok replikálása</span><span class="sxs-lookup"><span data-stu-id="1d2be-188">Replicate Azure API Management Accounts</span></span>](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
  * <span data-ttu-id="1d2be-189">Köszönjük, hogy tooGisela saját hozzájárulás toothis cikk.</span><span class="sxs-lookup"><span data-stu-id="1d2be-189">Thank you tooGisela for her contribution toothis article.</span></span>
* [<span data-ttu-id="1d2be-190">Az Azure API Management: Biztonsági mentése, és konfiguráció visszaállítása</span><span class="sxs-lookup"><span data-stu-id="1d2be-190">Azure API Management: Backing Up and Restoring Configuration</span></span>](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
  * <span data-ttu-id="1d2be-191">Stuart által részletes hello módszer nem egyezik meg a hello hivatalos útmutatást de nagyon fontos.</span><span class="sxs-lookup"><span data-stu-id="1d2be-191">hello approach detailed by Stuart does not match hello official guidance but it is very interesting.</span></span>

[Backup an API Management service]: #step1
[Restore an API Management service]: #step2


[Azure API Management REST API]: http://msdn.microsoft.com/library/azure/dn781421.aspx

[api-management-add-aad-application]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-add-aad-application.png

[api-management-aad-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions.png
[api-management-aad-permissions-add]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-permissions-add.png
[api-management-aad-delegated-permissions]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-delegated-permissions.png
[api-management-aad-default-directory]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-default-directory.png
[api-management-aad-resources]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-aad-resources.png
[api-management-arm-token]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-arm-token.png
[api-management-endpoint]: ./media/api-management-howto-disaster-recovery-backup-restore/api-management-endpoint.png
