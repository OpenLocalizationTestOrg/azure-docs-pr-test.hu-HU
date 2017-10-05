---
title: "Megvalósítása vész-helyreállítási használatával biztonsági mentése és visszaállítása az Azure API Management |} Microsoft Docs"
description: "Útmutató: biztonsági mentése és visszaállítása az Azure API Management katasztrófa utáni helyreállítás végrehajtásához."
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
ms.openlocfilehash: 07c0265490cfae733133b6e0c938f90f9b392da4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-implement-disaster-recovery-using-service-backup-and-restore-in-azure-api-management"></a><span data-ttu-id="10f18-103">Vész-helyreállítási szolgáltatás biztonsági mentéssel implementálja, és az Azure API Management visszaállítása</span><span class="sxs-lookup"><span data-stu-id="10f18-103">How to implement disaster recovery using service backup and restore in Azure API Management</span></span>
<span data-ttu-id="10f18-104">Közzétételét és kezelését az API-kat Azure API Management szolgáltatáson keresztül kiválasztásával készítésének sok hibatűrést és infrastruktúra képességeket, amelyeket egyébként külön kialakítása, megvalósítása és kezelése.</span><span class="sxs-lookup"><span data-stu-id="10f18-104">By choosing to publish and manage your APIs via Azure API Management you are taking advantage of many fault tolerance and infrastructure capabilities that you would otherwise have to design, implement, and manage.</span></span> <span data-ttu-id="10f18-105">Az Azure platformon csökkenti a nagy része egy azon részét, a költség, a lehetséges hibák.</span><span class="sxs-lookup"><span data-stu-id="10f18-105">The Azure platform mitigates a large fraction of potential failures at a fraction of the cost.</span></span>

<span data-ttu-id="10f18-106">Rendelkezésre állási helyreállítás érintő a régió, ahol az API Management szolgáltatás tárolása, hogy a szolgáltatás egy másik régióban pótlására bármikor készen kell lennie.</span><span class="sxs-lookup"><span data-stu-id="10f18-106">To recover from availability problems affecting the region where your API Management service is hosted you should be ready to reconstitute your service in a different region at any time.</span></span> <span data-ttu-id="10f18-107">Attól függően, hogy a rendelkezésre állási célokat és a helyreállítási idő célkitűzése érdemes lefoglalni egy biztonsági mentési szolgáltatás egy vagy több régióban, majd próbálja meg a konfigurációs és a tartalom szinkronban vannak a aktív szolgáltatás karbantartása.</span><span class="sxs-lookup"><span data-stu-id="10f18-107">Depending on your availability goals and recovery time objective  you might want to reserve a backup service in one or more regions and try to maintain their configuration and content in sync with the active service.</span></span> <span data-ttu-id="10f18-108">A szolgáltatás biztonsági mentési és visszaállítási funkciót biztosít a szükséges építőelem végrehajtási vész-helyreállítási stratégiát.</span><span class="sxs-lookup"><span data-stu-id="10f18-108">The service backup and restore feature provides the necessary building block for implementing your disaster recovery strategy.</span></span>

<span data-ttu-id="10f18-109">Ez az útmutató bemutatja, hogyan Azure Resource Manager-kérelmek hitelesítéséhez szükséges, és hogyan biztonsági mentése és visszaállítása az API Management szolgáltatáspéldány.</span><span class="sxs-lookup"><span data-stu-id="10f18-109">This guide shows how to authenticate Azure Resource Manager requests, and how to backup and restore your API Management service instances.</span></span>

> [!NOTE]
> <span data-ttu-id="10f18-110">A biztonsági mentése és visszaállítása egy API-kezelés szolgáltatáspéldány vész-helyreállítási folyamatot a API-kezelés szolgáltatás példányainak ahhoz hasonló forgatókönyvek esetén átmeneti replikálására is használható.</span><span class="sxs-lookup"><span data-stu-id="10f18-110">The process for backing up and restoring an API Management service instance for disaster recovery can also be used for replicating API Management service instances for scenarios such as staging.</span></span>
>
> <span data-ttu-id="10f18-111">Vegye figyelembe, hogy minden egyes biztonsági másolat 30 nap múlva lejár.</span><span class="sxs-lookup"><span data-stu-id="10f18-111">Note that each backup expires after 30 days.</span></span> <span data-ttu-id="10f18-112">Ha a biztonsági másolat visszaállítása a 30 napos lejárati időszak lejárta után kísérli meg, a visszaállítás sikertelen lesz, és egy `Cannot restore: backup expired` üzenet.</span><span class="sxs-lookup"><span data-stu-id="10f18-112">If you attempt to restore a backup after the 30 day expiration period has expired, the restore will fail with a `Cannot restore: backup expired` message.</span></span>
>
>

## <a name="authenticating-azure-resource-manager-requests"></a><span data-ttu-id="10f18-113">Hitelesítő Azure Resource Manager-kérelmek</span><span class="sxs-lookup"><span data-stu-id="10f18-113">Authenticating Azure Resource Manager requests</span></span>
> [!IMPORTANT]
> <span data-ttu-id="10f18-114">A REST API-t a biztonsági mentési és visszaállítási Azure Resource Managert használja, és egy különböző hitelesítési eljárást, mint a REST API-k az API Management entitások kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="10f18-114">The REST API for backup and restore uses Azure Resource Manager and has a different authentication mechanism than the REST APIs for managing your API Management entities.</span></span> <span data-ttu-id="10f18-115">Ebben a szakaszban a lépések azt ismertetik, hogyan Azure Resource Manager-kérelmek hitelesítéséhez szükséges.</span><span class="sxs-lookup"><span data-stu-id="10f18-115">The steps in this section describe how to authenticate Azure Resource Manager requests.</span></span> <span data-ttu-id="10f18-116">További információkért lásd: [kérelmek hitelesítéséhez az Azure Resource Manager](http://msdn.microsoft.com/library/azure/dn790557.aspx).</span><span class="sxs-lookup"><span data-stu-id="10f18-116">For more information, see [Authenticating Azure Resource Manager requests](http://msdn.microsoft.com/library/azure/dn790557.aspx).</span></span>
>
>

<span data-ttu-id="10f18-117">Minden olyan feladat, amelyeknek az Azure Resource Manager használatával erőforrások az Azure Active Directoryban az alábbi lépéseket követve hitelesíteni kell.</span><span class="sxs-lookup"><span data-stu-id="10f18-117">All of the tasks that you do on resources using the Azure Resource Manager must be authenticated with Azure Active Directory using the following steps.</span></span>

* <span data-ttu-id="10f18-118">Hozzáadhat egy alkalmazást az Azure Active Directory-bérlő.</span><span class="sxs-lookup"><span data-stu-id="10f18-118">Add an application to the Azure Active Directory tenant.</span></span>
* <span data-ttu-id="10f18-119">Állítsa be az alkalmazás hozzáadott.</span><span class="sxs-lookup"><span data-stu-id="10f18-119">Set permissions for the application that you added.</span></span>
* <span data-ttu-id="10f18-120">A token beolvasása az Azure Resource Manager kérések hitelesítése.</span><span class="sxs-lookup"><span data-stu-id="10f18-120">Get the token for authenticating requests to Azure Resource Manager.</span></span>

<span data-ttu-id="10f18-121">Az első lépés, ha az Azure Active Directory-alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="10f18-121">The first step is to create an Azure Active Directory application.</span></span> <span data-ttu-id="10f18-122">Jelentkezzen be a [klasszikus Azure portál](http://manage.windowsazure.com/) példányt, és keresse meg az előfizetést, amely tartalmazza az API-kezelés szolgáltatás a **alkalmazások** fülre az alapértelmezett Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="10f18-122">Log into the [Azure Classic Portal](http://manage.windowsazure.com/) using the subscription that contains your API Management service instance and navigate to the **Applications** tab for your default Azure Active Directory.</span></span>

> [!NOTE]
> <span data-ttu-id="10f18-123">Ha az Azure Active Directory alapértelmezett címtár nem látható a fiókjához, lépjen kapcsolatba a fiókhoz a szükséges engedélyek megadására az Azure-előfizetés rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="10f18-123">If the Azure Active Directory default directory is not visible to your account, contact the administrator of the Azure subscription to grant the required permissions to your account.</span></span>

![Azure Active Directory-alkalmazás létrehozása][api-management-add-aad-application]

<span data-ttu-id="10f18-125">Kattintson a **Hozzáadás**, **a szerveztem által fejlesztett alkalmazás hozzáadása**, és válassza a **natív ügyfélalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="10f18-125">Click **Add**, **Add an application my organization is developing**, and choose **Native client application**.</span></span> <span data-ttu-id="10f18-126">Adjon meg egy leíró nevet, és kattintson a Tovább nyílra.</span><span class="sxs-lookup"><span data-stu-id="10f18-126">Enter a descriptive name, and click the next arrow.</span></span> <span data-ttu-id="10f18-127">Írjon be egy helyőrző URL-címet, például `http://resources` a a **átirányítási URI-**, mert a mezőt kötelező kitölteni, de az érték nem használatos később.</span><span class="sxs-lookup"><span data-stu-id="10f18-127">Enter a placeholder URL such as `http://resources` for the **Redirect URI**, as it is a required field, but the value is not used later.</span></span> <span data-ttu-id="10f18-128">Jelölje be a jelölőnégyzetet az alkalmazás menteni.</span><span class="sxs-lookup"><span data-stu-id="10f18-128">Click the check box to save the application.</span></span>

<span data-ttu-id="10f18-129">Ha az alkalmazás menti, kattintson **konfigurálása**, görgessen le a **egyéb alkalmazások engedélyei** szakaszt, és kattintson a **alkalmazás hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="10f18-129">Once the application is saved, click **Configure**, scroll down to the **permissions to other applications** section, and click **Add application**.</span></span>

![Engedélyek hozzáadása][api-management-aad-permissions-add]

<span data-ttu-id="10f18-131">Válassza ki **Windows** **Azure szolgáltatásfelügyeleti API** és a jelölőnégyzet bejelölésével vegye fel az alkalmazást.</span><span class="sxs-lookup"><span data-stu-id="10f18-131">Select **Windows** **Azure Service Management API** and click the checkbox to add the application.</span></span>

![Engedélyek hozzáadása][api-management-aad-permissions]

<span data-ttu-id="10f18-133">Kattintson a **delegált engedélyek** mellett az újonnan hozzáadott **Windows** **Azure szolgáltatásfelügyeleti API** alkalmazást, jelölje be a **Azure szolgáltatás Management (preview)**, és kattintson a **mentése**.</span><span class="sxs-lookup"><span data-stu-id="10f18-133">Click **Delegated Permissions** beside the newly added **Windows** **Azure Service Management API** application, check the box for **Access Azure Service Management (preview)**, and click **Save**.</span></span>

![Engedélyek hozzáadása][api-management-aad-delegated-permissions]

<span data-ttu-id="10f18-135">Előtt hívja az API-kat, hogy a biztonsági mentés és állítsa vissza azt, fontos szolgáltatáshitelesítést egy token.</span><span class="sxs-lookup"><span data-stu-id="10f18-135">Prior to invoking the APIs that generate the backup and restore it, it is necessary to get a token.</span></span> <span data-ttu-id="10f18-136">Az alábbi példában a [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) nuget-csomagot a jogkivonatot lekérdezni.</span><span class="sxs-lookup"><span data-stu-id="10f18-136">The following example uses the [Microsoft.IdentityModel.Clients.ActiveDirectory](https://www.nuget.org/packages/Microsoft.IdentityModel.Clients.ActiveDirectory) nuget package to retrieve the token.</span></span>

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
                throw new InvalidOperationException("Failed to obtain the JWT token");
            }

            Console.WriteLine(result.AccessToken);

            Console.ReadLine();
        }
    }
}
```

<span data-ttu-id="10f18-137">Cserélje le `{tentand id}`, `{application id}`, és `{redirect uri}` az alábbi utasításokat.</span><span class="sxs-lookup"><span data-stu-id="10f18-137">Replace `{tentand id}`, `{application id}`, and `{redirect uri}` using the following instructions.</span></span>

<span data-ttu-id="10f18-138">Cserélje le `{tenant id}` együtt az imént létrehozott Azure Active Directory-alkalmazás a bérlő azonosítója.</span><span class="sxs-lookup"><span data-stu-id="10f18-138">Replace `{tenant id}` with the tenant id of the Azure Active Directory application you just created.</span></span> <span data-ttu-id="10f18-139">Az azonosító eléréséhez kattintson **végpontok megtekintése**.</span><span class="sxs-lookup"><span data-stu-id="10f18-139">You can access the id by clicking **View endpoints**.</span></span>

![Végpontok][api-management-aad-default-directory]

![Végpontok][api-management-endpoint]

<span data-ttu-id="10f18-142">Cserélje le `{application id}` és `{redirect uri}` használatával a **ügyfél-azonosító** és az URL-CÍMÉT a **átirányítási URI-azonosítók** szakaszban az Azure Active Directory-alkalmazás **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="10f18-142">Replace `{application id}` and `{redirect uri}` using the **Client Id** and  the URL from the **Redirect Uris** section from your Azure Active Directory application's **Configure** tab.</span></span>

![Erőforrások][api-management-aad-resources]

<span data-ttu-id="10f18-144">Ha az értékek vannak megadva, a Kódpélda kell visszaadnia jogkivonatot az alábbi példához hasonló.</span><span class="sxs-lookup"><span data-stu-id="10f18-144">Once the values are specified, the code example should return a token similar to the following example.</span></span>

![Token][api-management-arm-token]

<span data-ttu-id="10f18-146">Előtt hívja meg az alábbi szakaszok ismertetik a biztonsági mentési és visszaállítási műveletek, állítsa be a REST-hívást a hitelesítési kérelem fejlécéhez.</span><span class="sxs-lookup"><span data-stu-id="10f18-146">Before calling the backup and restore operations described in the following sections, set the authorization request header for your REST call.</span></span>

```c#
request.Headers.Add(HttpRequestHeader.Authorization, "Bearer " + token);
```

## <span data-ttu-id="10f18-147"><a name="step1"></a>Készítsen biztonsági másolatot egy API-kezelés szolgáltatás</span><span class="sxs-lookup"><span data-stu-id="10f18-147"><a name="step1"> </a>Back up an API Management service</span></span>
<span data-ttu-id="10f18-148">Biztonsági mentése egy API-kezelés szolgáltatás a probléma a következő HTTP-kérelem:</span><span class="sxs-lookup"><span data-stu-id="10f18-148">To back up an API Management service issue the following HTTP request:</span></span>

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/backup?api-version={api-version}`

<span data-ttu-id="10f18-149">Ahol:</span><span class="sxs-lookup"><span data-stu-id="10f18-149">where:</span></span>

* <span data-ttu-id="10f18-150">`subscriptionId`-a kívánt API Management szolgáltatást tartalmazó előfizetés azonosítója a biztonsági mentés</span><span class="sxs-lookup"><span data-stu-id="10f18-150">`subscriptionId` - id of the subscription containing the API Management service you are attempting to backup</span></span>
* <span data-ttu-id="10f18-151">`resourceGroupName`-karakterlánc 'Api - alapértelmezett-{szolgáltatás-régió}' formájában ahol `service-region` azonosítja az Azure-régió, ahol az API Management szolgáltatást, akkor próbált biztonsági mentési üzemelteti, pl.`North-Central-US`</span><span class="sxs-lookup"><span data-stu-id="10f18-151">`resourceGroupName` - a string in the form of 'Api-Default-{service-region}' where `service-region` identifies the Azure region where the API Management service you are trying to backup is hosted, e.g. `North-Central-US`</span></span>
* <span data-ttu-id="10f18-152">`serviceName`-a szolgáltatás nevét, az API Management végez biztonsági másolatának meg egyszerre, ahol létrehozták</span><span class="sxs-lookup"><span data-stu-id="10f18-152">`serviceName` - the name of the API Management service you are making a backup of specified at the time of its creation</span></span>
* <span data-ttu-id="10f18-153">`api-version`-cseréje`2014-02-14`</span><span class="sxs-lookup"><span data-stu-id="10f18-153">`api-version` - replace  with `2014-02-14`</span></span>

<span data-ttu-id="10f18-154">A kérelem törzsében adja meg a cél az Azure storage-fiók neve, a hozzáférési kulcsot, a blob-tároló neve és a biztonsági másolat neve:</span><span class="sxs-lookup"><span data-stu-id="10f18-154">In the body of the request, specify the target Azure storage account name, access key, blob container name, and backup name:</span></span>

```
'{  
    storageAccount : {storage account name for the backup},  
    accessKey : {access key for the account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

<span data-ttu-id="10f18-155">Állítsa a `Content-Type` a kérelem fejlécében `application/json`.</span><span class="sxs-lookup"><span data-stu-id="10f18-155">Set the value of the `Content-Type` request header to `application/json`.</span></span>

<span data-ttu-id="10f18-156">Biztonsági mentés egy hosszú ideig futó művelet, amely több percet is igénybe vehet.</span><span class="sxs-lookup"><span data-stu-id="10f18-156">Backup is a long running operation that may take multiple minutes to complete.</span></span>  <span data-ttu-id="10f18-157">Ha a kérelem sikeres volt, és a biztonsági mentési folyamat kezdeményezett kapni fog egy `202 Accepted` a válasz állapotkódja egy `Location` fejléc.</span><span class="sxs-lookup"><span data-stu-id="10f18-157">If the request was successful and the backup process was initiated you’ll receive a `202 Accepted` response status code with a `Location` header.</span></span>  <span data-ttu-id="10f18-158">Ellenőrizze a "GET" kéréseket az URL-címet a `Location` fejlécet a művelet állapotának megállapítása.</span><span class="sxs-lookup"><span data-stu-id="10f18-158">Make 'GET' requests to the URL in the `Location` header to find out the status of the operation.</span></span> <span data-ttu-id="10f18-159">Miközben folyamatban van a biztonsági mentés fog továbbra is megjelenik egy '202 elfogadott' állapotkód.</span><span class="sxs-lookup"><span data-stu-id="10f18-159">While the backup is in progress you will continue to receive a '202 Accepted' status code.</span></span> <span data-ttu-id="10f18-160">Válaszkód `200 OK` jelzi a biztonsági mentési művelet sikeres befejezését.</span><span class="sxs-lookup"><span data-stu-id="10f18-160">A Response code of `200 OK` will indicate successful completion of the backup operation.</span></span>

<span data-ttu-id="10f18-161">Amikor egy biztonsági mentési kérést vegye figyelembe a következő korlátozások vonatkoznak.</span><span class="sxs-lookup"><span data-stu-id="10f18-161">Please note the following constraints when making a backup request.</span></span>

* <span data-ttu-id="10f18-162">**Tároló** a kérés törzsében megadott **léteznie kell**.</span><span class="sxs-lookup"><span data-stu-id="10f18-162">**Container** specified in the request body **must exist**.</span></span>
* <span data-ttu-id="10f18-163">Biztonsági mentés folyamatban, amíg **nem szabadna megpróbálniuk a szolgáltatás felügyeleti műveleteket sem** SKU frissítése vagy alacsonyabb szintre való visszalépést, a tartomány kiszolgálónév-változás, például stb.</span><span class="sxs-lookup"><span data-stu-id="10f18-163">While backup is in progress you **should not attempt any service management operations** such as SKU upgrade or downgrade, domain name change, etc.</span></span>
* <span data-ttu-id="10f18-164">A visszaállítás egy **biztonsági mentés csak 30 napig garantáltan** időpontjában a létrehozása óta.</span><span class="sxs-lookup"><span data-stu-id="10f18-164">Restore of a **backup is guaranteed only for 30 days** since the moment of its creation.</span></span>
* <span data-ttu-id="10f18-165">**Használati adatok** elemzési jelentések létrehozásához használt **nincs megadva** a biztonsági mentésben.</span><span class="sxs-lookup"><span data-stu-id="10f18-165">**Usage data** used for creating analytics reports **is not included** in the backup.</span></span> <span data-ttu-id="10f18-166">Használjon [Azure API Management REST API] [ Azure API Management REST API] rendszeresen beolvasása az analytics-jelentések meg van őrizve.</span><span class="sxs-lookup"><span data-stu-id="10f18-166">Use [Azure API Management REST API][Azure API Management REST API] to periodically retrieve analytics reports for safekeeping.</span></span>
* <span data-ttu-id="10f18-167">A gyakoriság, amellyel elvégezhető a szolgáltatás biztonsági mentések hatással lesz a helyreállításipont-célkitűzést.</span><span class="sxs-lookup"><span data-stu-id="10f18-167">The frequency with which you perform service backups will affect your recovery point objective.</span></span> <span data-ttu-id="10f18-168">Minimalizálása érdekében azt javasoljuk a rendszeres biztonsági mentések végrehajtására, valamint a igény szerinti biztonsági mentést végez az API Management szolgáltatás fontos módosítások elvégzése után.</span><span class="sxs-lookup"><span data-stu-id="10f18-168">To minimize it we advise implementing regular backups as well as performing on-demand backups after making important changes to your API Management service.</span></span>
* <span data-ttu-id="10f18-169">**Módosítások** a szolgáltatás konfigurációs (pl. API-k, házirendek, fejlesztői portál megjelenését) biztonsági mentés közben végzett művelet van folyamatban **előfordulhat, hogy nem kell a biztonsági mentésben szereplő, és ezért elvész**.</span><span class="sxs-lookup"><span data-stu-id="10f18-169">**Changes** made to the service configuration (e.g. APIs, policies, developer portal appearance) while backup operation is in process **might not be included in the backup and therefore will be lost**.</span></span>

## <span data-ttu-id="10f18-170"><a name="step2"></a>Egy API-kezelés szolgáltatás visszaállítása</span><span class="sxs-lookup"><span data-stu-id="10f18-170"><a name="step2"> </a>Restore an API Management service</span></span>
<span data-ttu-id="10f18-171">Az API Management visszaállítása egy korábban létrehozott biztonsági másolat szolgáltatás ellenőrizze a következő HTTP-kérelem:</span><span class="sxs-lookup"><span data-stu-id="10f18-171">To restore an API Management service from a previously created backup make the following HTTP request:</span></span>

`POST https://management.azure.com/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.ApiManagement/service/{serviceName}/restore?api-version={api-version}`

<span data-ttu-id="10f18-172">Ahol:</span><span class="sxs-lookup"><span data-stu-id="10f18-172">where:</span></span>

* <span data-ttu-id="10f18-173">`subscriptionId`-a biztonsági másolatból történő visszaállítását végzi az API Management szolgáltatást tartalmazó előfizetés azonosítója</span><span class="sxs-lookup"><span data-stu-id="10f18-173">`subscriptionId` - id of the subscription containing the API Management service you are restoring a backup into</span></span>
* <span data-ttu-id="10f18-174">`resourceGroupName`-karakterlánc 'Api - alapértelmezett-{szolgáltatás-régió}' formájában ahol `service-region` pl. azonosítja az Azure-régió, ahol az API Management szolgáltatás visszaállítását végzi, a biztonsági másolat tárolása,`North-Central-US`</span><span class="sxs-lookup"><span data-stu-id="10f18-174">`resourceGroupName` - a string in the form of 'Api-Default-{service-region}' where `service-region` identifies the Azure region where the API Management service you are restoring a backup into is hosted, e.g. `North-Central-US`</span></span>
* <span data-ttu-id="10f18-175">`serviceName`-a API-kezelés szolgáltatás a visszaállítandó meg, ahol létrehozták egyszerre neve</span><span class="sxs-lookup"><span data-stu-id="10f18-175">`serviceName` - the name of the API Management service being restored into specified at the time of its creation</span></span>
* <span data-ttu-id="10f18-176">`api-version`-cseréje`2014-02-14`</span><span class="sxs-lookup"><span data-stu-id="10f18-176">`api-version` - replace  with `2014-02-14`</span></span>

<span data-ttu-id="10f18-177">A kérelem törzsében adja meg a biztonságimásolat-fájl helyét, azaz az Azure storage-fiók neve, a hozzáférési kulcsot, a blob-tároló neve és biztonsági másolat neve:</span><span class="sxs-lookup"><span data-stu-id="10f18-177">In the body of the request, specify the backup file location, i.e. Azure storage account name, access key, blob container name, and backup name:</span></span>

```
'{  
    storageAccount : {storage account name for the backup},  
    accessKey : {access key for the account},  
    containerName : {backup container name},  
    backupName : {backup blob name}  
}'
```

<span data-ttu-id="10f18-178">Állítsa a `Content-Type` a kérelem fejlécében `application/json`.</span><span class="sxs-lookup"><span data-stu-id="10f18-178">Set the value of the `Content-Type` request header to `application/json`.</span></span>

<span data-ttu-id="10f18-179">Visszaállítás egy hosszú ideig futó művelet, amely előfordulhat, hogy 30 vagy több percig tarthat befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="10f18-179">Restore is a long running operation that may take up to 30 or more minutes to complete.</span></span>  <span data-ttu-id="10f18-180">Ha a kérelem sikeres volt, és a visszaállítási folyamat kezdeményezett kapni fog egy `202 Accepted` a válasz állapotkódja egy `Location` fejléc.</span><span class="sxs-lookup"><span data-stu-id="10f18-180">If the request was successful and the restore process was initiated you’ll receive a `202 Accepted` response status code with a `Location` header.</span></span>  <span data-ttu-id="10f18-181">Ellenőrizze a "GET" kéréseket az URL-címet a `Location` fejlécet a művelet állapotának megállapítása.</span><span class="sxs-lookup"><span data-stu-id="10f18-181">Make 'GET' requests to the URL in the `Location` header to find out the status of the operation.</span></span> <span data-ttu-id="10f18-182">A visszaállítás közben, továbbra is megkapják '202 elfogadott' állapotkód.</span><span class="sxs-lookup"><span data-stu-id="10f18-182">While the restore is in progress you will continue to receive '202 Accepted' status code.</span></span> <span data-ttu-id="10f18-183">Válaszkód `200 OK` tájékoztatja a visszaállítási művelet sikeres befejezését.</span><span class="sxs-lookup"><span data-stu-id="10f18-183">A response code of `200 OK` will indicate successful completion of the restore operation.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="10f18-184">**A Termékváltozat** a szolgáltatás a visszaállítandó **egyeznie kell** a visszaállítandó biztonsági másolat szolgáltatás a Termékváltozat.</span><span class="sxs-lookup"><span data-stu-id="10f18-184">**The SKU** of the service being restored into **must match** the SKU of the backed up service being restored.</span></span>
>
> <span data-ttu-id="10f18-185">**Módosítások** végzett a szolgáltatási konfiguráció (pl. API-k, házirendek, fejlesztői portál megjelenését) visszaállítása közben a művelet van folyamatban **felülíródnak**.</span><span class="sxs-lookup"><span data-stu-id="10f18-185">**Changes** made to the service configuration (e.g. APIs, policies, developer portal appearance) while restore operation is in progress **could be overwritten**.</span></span>
>
>

## <a name="next-steps"></a><span data-ttu-id="10f18-186">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="10f18-186">Next steps</span></span>
<span data-ttu-id="10f18-187">Tekintse meg a biztonsági mentési/visszaállítási folyamat két különböző forgatókönyvek a következő Microsoft-blogjaihoz.</span><span class="sxs-lookup"><span data-stu-id="10f18-187">Check out the following Microsoft blogs for two different walkthroughs of the backup/restore process.</span></span>

* [<span data-ttu-id="10f18-188">Az Azure API Management fiókok replikálása</span><span class="sxs-lookup"><span data-stu-id="10f18-188">Replicate Azure API Management Accounts</span></span>](https://www.returngis.net/en/2015/06/replicate-azure-api-management-accounts/)
  * <span data-ttu-id="10f18-189">Köszönjük, hogy a Gisela saját hozzájárulás a cikkhez.</span><span class="sxs-lookup"><span data-stu-id="10f18-189">Thank you to Gisela for her contribution to this article.</span></span>
* [<span data-ttu-id="10f18-190">Az Azure API Management: Biztonsági mentése, és konfiguráció visszaállítása</span><span class="sxs-lookup"><span data-stu-id="10f18-190">Azure API Management: Backing Up and Restoring Configuration</span></span>](http://blogs.msdn.com/b/stuartleeks/archive/2015/04/29/azure-api-management-backing-up-and-restoring-configuration.aspx)
  * <span data-ttu-id="10f18-191">Stuart által részletes módszer nem egyezik meg a hivatalos útmutatást de nagyon fontos.</span><span class="sxs-lookup"><span data-stu-id="10f18-191">The approach detailed by Stuart does not match the official guidance but it is very interesting.</span></span>

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
