---
title: "aaaChange blob elérési út hello alapértelmezett |} Microsoft Docs"
description: "Ismerje meg, hogyan tooset be egy Azure működni toorename egy blob fájl elérési útját (magán előnézetben)"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 03/16/2017
ms.author: vidarmsft
ms.openlocfilehash: 2c414603514223c701ab1a3bd0b81ee18f1af666
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="change-a-blob-path-from-hello-default-path-private-preview"></a><span data-ttu-id="2a253-103">Egy blob elérési utat módosítsa hello alapértelmezett elérési út (magán előnézetben)</span><span class="sxs-lookup"><span data-stu-id="2a253-103">Change a blob path from hello default path (private preview)</span></span>

<span data-ttu-id="2a253-104">Ez a cikk ismerteti, hogyan tooset be egy Azure működni toorename egy alapértelmezett blob fájl elérési útját.</span><span class="sxs-lookup"><span data-stu-id="2a253-104">This article describes how tooset up an Azure function toorename a default blob file path.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="2a253-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2a253-105">Prerequisites</span></span>

<span data-ttu-id="2a253-106">Győződjön meg arról, hogy rendelkezik-e olyan feladatdefinícióban erőforráscsoporton belül egy hibrid adatforrás, melyhez a megfelelően konfigurált.</span><span class="sxs-lookup"><span data-stu-id="2a253-106">Ensure that you have a job definition that has been correctly configured in a hybrid data resource within a resource group.</span></span>

## <a name="create-an-azure-function"></a><span data-ttu-id="2a253-107">Egy Azure-függvény létrehozása</span><span class="sxs-lookup"><span data-stu-id="2a253-107">Create an Azure function</span></span>

<span data-ttu-id="2a253-108">egy Azure függvény toocreate hello a következő:</span><span class="sxs-lookup"><span data-stu-id="2a253-108">toocreate an Azure function, do hello following:</span></span>

1. <span data-ttu-id="2a253-109">Nyissa meg toohello [Azure-portálon](http://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="2a253-109">Go toohello [Azure portal](http://portal.azure.com/).</span></span>

2. <span data-ttu-id="2a253-110">A bal oldali ablaktáblán hello hello tetején kattintson **új**.</span><span class="sxs-lookup"><span data-stu-id="2a253-110">At hello top of hello left pane, click **New**.</span></span> 

3. <span data-ttu-id="2a253-111">A hello **keresési** mezőbe írja be **függvény App**, és nyomja le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="2a253-111">In hello **Search** box, type **Function App**, and then press Enter.</span></span>

    ![Hello keresőmezőbe írja be a "Függvény alkalmazás"](./media/storsimple-data-manager-change-default-blob-path/goto-function-app-resource.png)

4. <span data-ttu-id="2a253-113">A hello **eredmények** listában, kattintson **függvény App**.</span><span class="sxs-lookup"><span data-stu-id="2a253-113">In hello **Results** list, click **Function App**.</span></span>

    ![Válassza ki a hello függvény alkalmazás-erőforrást hello eredmények listájában](./media/storsimple-data-manager-change-default-blob-path/select-function-app-resource.png)

    <span data-ttu-id="2a253-115">Hello **függvény App** ablak nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2a253-115">hello **Function App** window opens.</span></span>

5. <span data-ttu-id="2a253-116">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2a253-116">Click **Create**.</span></span>

    ![hello függvény App ablak "Létrehozás" gombra](./media/storsimple-data-manager-change-default-blob-path/create-new-function-app.png)

6. <span data-ttu-id="2a253-118">A hello **függvény App** konfigurációs panelen a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2a253-118">On hello **Function App** configuration blade, do hello following:</span></span>

    <span data-ttu-id="2a253-119">a.</span><span class="sxs-lookup"><span data-stu-id="2a253-119">a.</span></span> <span data-ttu-id="2a253-120">A hello **alkalmazásnév** mezőjébe hello alkalmazás neve.</span><span class="sxs-lookup"><span data-stu-id="2a253-120">In hello **App name** box, type hello app name.</span></span>
    
    <span data-ttu-id="2a253-121">b.</span><span class="sxs-lookup"><span data-stu-id="2a253-121">b.</span></span> <span data-ttu-id="2a253-122">A hello **előfizetés** mezőjébe hello előfizetés hello nevét.</span><span class="sxs-lookup"><span data-stu-id="2a253-122">In hello **Subscription** box, type hello name of hello subscription.</span></span>

    <span data-ttu-id="2a253-123">c.</span><span class="sxs-lookup"><span data-stu-id="2a253-123">c.</span></span> <span data-ttu-id="2a253-124">A **erőforráscsoport**, kattintson a **hozzon létre új**, és majd hello típusnév hello erőforráscsoport.</span><span class="sxs-lookup"><span data-stu-id="2a253-124">Under **Resource Group**, click **Create new**, and then type hello name of hello resource group.</span></span>

    <span data-ttu-id="2a253-125">d.</span><span class="sxs-lookup"><span data-stu-id="2a253-125">d.</span></span> <span data-ttu-id="2a253-126">A hello **üzemeltetési terv** mezőbe írja be **fogyasztás megtervezése**.</span><span class="sxs-lookup"><span data-stu-id="2a253-126">In hello **Hosting Plan** box, type **Consumption Plan**.</span></span>

    <span data-ttu-id="2a253-127">e.</span><span class="sxs-lookup"><span data-stu-id="2a253-127">e.</span></span> <span data-ttu-id="2a253-128">A hello **hely** mezőbe típusú hello helyet.</span><span class="sxs-lookup"><span data-stu-id="2a253-128">In hello **Location** box, type hello location.</span></span>

    <span data-ttu-id="2a253-129">f.</span><span class="sxs-lookup"><span data-stu-id="2a253-129">f.</span></span> <span data-ttu-id="2a253-130">A **tárfiók**, válasszon egy meglévő tárfiókot, vagy hozzon létre egy új tárfiókot.</span><span class="sxs-lookup"><span data-stu-id="2a253-130">Under **Storage account**, select an existing storage account or create a new storage account.</span></span> <span data-ttu-id="2a253-131">A tárfiók belső használatra készült a hello függvény.</span><span class="sxs-lookup"><span data-stu-id="2a253-131">A storage account is used internally for hello function.</span></span>

    ![Adja meg az új függvény alkalmazás-konfigurációs adatokat](./media/storsimple-data-manager-change-default-blob-path/enter-new-funcion-app-data.png)

7. <span data-ttu-id="2a253-133">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2a253-133">Click **Create**.</span></span>  
    <span data-ttu-id="2a253-134">hello függvény alkalmazás létrehozása.</span><span class="sxs-lookup"><span data-stu-id="2a253-134">hello function app is created.</span></span>

8. <span data-ttu-id="2a253-135">Hello bal oldali ablaktáblában kattintson **további szolgáltatások**, és ezután hello a következő:</span><span class="sxs-lookup"><span data-stu-id="2a253-135">In hello left pane, click **More services**, and then do hello following:</span></span>
    
    <span data-ttu-id="2a253-136">a.</span><span class="sxs-lookup"><span data-stu-id="2a253-136">a.</span></span> <span data-ttu-id="2a253-137">A hello **szűrő** mezőbe írja be **alkalmazásszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="2a253-137">In hello **Filter** box, type **App services**.</span></span>
    
    <span data-ttu-id="2a253-138">b.</span><span class="sxs-lookup"><span data-stu-id="2a253-138">b.</span></span> <span data-ttu-id="2a253-139">Kattintson a **alkalmazásszolgáltatások**.</span><span class="sxs-lookup"><span data-stu-id="2a253-139">Click **App Services**.</span></span> 

    !["További szolgáltatások" hivatkozás hello bal oldali panelen](./media/storsimple-data-manager-change-default-blob-path/more-services.png)

9. <span data-ttu-id="2a253-141">App Service szolgáltatások hello listájában kattintson hello függvény alkalmazás hello nevét.</span><span class="sxs-lookup"><span data-stu-id="2a253-141">In hello list of app services, click hello name of hello function app.</span></span>  
    <span data-ttu-id="2a253-142">Megnyílik a hello függvény oldalra.</span><span class="sxs-lookup"><span data-stu-id="2a253-142">hello function app page opens.</span></span>

10. <span data-ttu-id="2a253-143">Hello bal oldali ablaktáblában kattintson **új függvény**, és ezután hello a következő:</span><span class="sxs-lookup"><span data-stu-id="2a253-143">In hello left pane, click **New Function**, and then do hello following:</span></span> 

    <span data-ttu-id="2a253-144">a.</span><span class="sxs-lookup"><span data-stu-id="2a253-144">a.</span></span> <span data-ttu-id="2a253-145">A hello **nyelvi** listáról válassza ki **C#**.</span><span class="sxs-lookup"><span data-stu-id="2a253-145">In hello **Language** list, select **C#**.</span></span>
    
    <span data-ttu-id="2a253-146">b.</span><span class="sxs-lookup"><span data-stu-id="2a253-146">b.</span></span> <span data-ttu-id="2a253-147">Hello tömb sablon csempék, válassza ki a **QueueTrigger-c Sharp**.</span><span class="sxs-lookup"><span data-stu-id="2a253-147">In hello array of template tiles, select **QueueTrigger-CSharp**.</span></span>

    <span data-ttu-id="2a253-148">c.</span><span class="sxs-lookup"><span data-stu-id="2a253-148">c.</span></span> <span data-ttu-id="2a253-149">A hello **a függvény neve** mezőbe írja be a függvény nevét.</span><span class="sxs-lookup"><span data-stu-id="2a253-149">In hello **Name your function** box, type a name for your function.</span></span>

    <span data-ttu-id="2a253-150">d.</span><span class="sxs-lookup"><span data-stu-id="2a253-150">d.</span></span> <span data-ttu-id="2a253-151">A hello **üzenetsornév** mezőbe írja be az adat-átalakítási feladat definition nevét.</span><span class="sxs-lookup"><span data-stu-id="2a253-151">In hello **Queue name** box, type your data-transformation job definition name.</span></span>

    <span data-ttu-id="2a253-152">e.</span><span class="sxs-lookup"><span data-stu-id="2a253-152">e.</span></span> <span data-ttu-id="2a253-153">A **fiók tárolókapcsolat**, kattintson a **új**, majd válassza ki a megfelelő toohello adat-átalakítási feladat hello fiók.</span><span class="sxs-lookup"><span data-stu-id="2a253-153">Under **Storage account connection**, click **new**, and then select hello account that corresponds toohello data-transformation job.</span></span>  
        <span data-ttu-id="2a253-154">Jegyezze fel a hello kapcsolat neve.</span><span class="sxs-lookup"><span data-stu-id="2a253-154">Make a note of hello connection name.</span></span> <span data-ttu-id="2a253-155">a későbbi hello Azure függvény, meg kell adni a hello nevet.</span><span class="sxs-lookup"><span data-stu-id="2a253-155">hello name is required later in hello Azure function.</span></span>

       ![Egy új C#-függvény létrehozása](./media/storsimple-data-manager-change-default-blob-path/create-new-csharp-function.png)

    <span data-ttu-id="2a253-157">f.</span><span class="sxs-lookup"><span data-stu-id="2a253-157">f.</span></span> <span data-ttu-id="2a253-158">Kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="2a253-158">Click **Create**.</span></span>  
    <span data-ttu-id="2a253-159">Hello **függvény** ablak nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="2a253-159">hello **Function** window opens.</span></span>

11. <span data-ttu-id="2a253-160">A hello **függvény** ablakban futtassa _.csx_ fájlt, és ezután hello a következő:</span><span class="sxs-lookup"><span data-stu-id="2a253-160">In hello **Function** window, run _.csx_ file, and then do hello following:</span></span>

    <span data-ttu-id="2a253-161">a.</span><span class="sxs-lookup"><span data-stu-id="2a253-161">a.</span></span> <span data-ttu-id="2a253-162">Illessze be a kódját a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2a253-162">Paste hello following code:</span></span>

    ```
    using System;
    using System.Configuration;
    using Microsoft.WindowsAzure.Storage.Blob;
    using Microsoft.WindowsAzure.Storage.Queue;
    using Microsoft.WindowsAzure.Storage;
    using System.Collections.Generic;
    using System.Linq;

    public static void Run(QueueItem myQueueItem, TraceWriter log)
    {
        CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConfigurationManager.AppSettings["STORAGE_CONNECTIONNAME"]);

        string storageAccUriEndswith = "windows.net/";
        string uri = myQueueItem.TargetLocation.Replace("%20", " ");
        log.Info($"Blob Uri: {uri}");

        // Remove storage account uri string
        uri = uri.Substring(uri.IndexOf(storageAccUriEndswith) + storageAccUriEndswith.Length);

        string containerName = uri.Substring(0, uri.IndexOf("/")); 

        // Remove container name string
        uri = uri.Substring(containerName.Length + 1);

        // Current blob path
        string blobName = uri; 

        string volumeName = uri.Substring(containerName.Length + 1);
        volumeName = uri.Substring(0, uri.IndexOf("/"));

        // Remove volume name string
        uri = uri.Substring(volumeName.Length + 1);

        string newContainerName = uri.Substring(0, uri.IndexOf("/")).ToLower();
        string newBlobName = uri.Substring(newContainerName.Length + 1);

        log.Info($"Container name: {containerName}");
        log.Info($"Volume name: {volumeName}");
        log.Info($"New container name: {newContainerName}");

        log.Info($"Blob name: {blobName}");
        log.Info($"New blob name: {newBlobName}");

        // Create hello blob client.
        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

        // Container reference
        CloudBlobContainer container = blobClient.GetContainerReference(containerName);
        CloudBlobContainer newContainer = blobClient.GetContainerReference(newContainerName);
        newContainer.CreateIfNotExists();

        if(!container.Exists())
        {
            log.Info($"Container - {containerName} not exists");
            return;
        }

        if(!newContainer.Exists())
        {
            log.Info($"Container - {newContainerName} not exists");
            return;
        }

        CloudBlockBlob blob = container.GetBlockBlobReference(blobName);
        if (!blob.Exists())
        {
            // Skip toocopy hello blob toonew container, if source blob doesn't exist
            log.Info($"hello specified blob does not exist.");
            log.Info($"Blob Uri: {blob.Uri}");
            return;
        }

        CloudBlockBlob blobCopy = newContainer.GetBlockBlobReference(newBlobName);
        if (!blobCopy.Exists())
        {
            blobCopy.StartCopy(blob);
            // Delete old blob, after copy toonew container
            blob.DeleteIfExists();
            log.Info($"Blob file path renamed completed successfully");
        }
        else
        {
            log.Info($"Blob file path renamed already done");
            // Delete old blob, if already exists.
            blob.DeleteIfExists();
        }
    }

    public class QueueItem
    {
        public string SourceLocation {get;set;}
        public long SizeInBytes {get;set;}
        public string Status {get;set;}
        public string JobID {get;set;}
        public string TargetLocation {get; set;}
    }

    ```

    <span data-ttu-id="2a253-163">b.</span><span class="sxs-lookup"><span data-stu-id="2a253-163">b.</span></span> <span data-ttu-id="2a253-164">Cserélje le **STORAGE_CONNECTIONNAME** a sor a tárolási fiók kapcsolattal 11 (lásd a pont 8 c).</span><span class="sxs-lookup"><span data-stu-id="2a253-164">Replace **STORAGE_CONNECTIONNAME** on line 11 with your storage account connection (refer point 8c).</span></span>

    <span data-ttu-id="2a253-165">c.</span><span class="sxs-lookup"><span data-stu-id="2a253-165">c.</span></span> <span data-ttu-id="2a253-166">Kattintson a lap tetején hello balra **mentése**.</span><span class="sxs-lookup"><span data-stu-id="2a253-166">At hello top left, click **Save**.</span></span>

    ![Függvény mentése](./media/storsimple-data-manager-change-default-blob-path/save-function.png)

12. <span data-ttu-id="2a253-168">toocomplete hello függvény, adjon hozzá egy további fájlt hello következő tevékenységek végrehajtásával:</span><span class="sxs-lookup"><span data-stu-id="2a253-168">toocomplete hello function, add one more file by doing hello following:</span></span>

    <span data-ttu-id="2a253-169">a.</span><span class="sxs-lookup"><span data-stu-id="2a253-169">a.</span></span> <span data-ttu-id="2a253-170">Kattintson a **-fájlokat tekinthetnek meg**.</span><span class="sxs-lookup"><span data-stu-id="2a253-170">Click **View files**.</span></span>

       ![hello "Fájlok megtekintése" hivatkozásra](./media/storsimple-data-manager-change-default-blob-path/view-files.png)

    <span data-ttu-id="2a253-172">b.</span><span class="sxs-lookup"><span data-stu-id="2a253-172">b.</span></span> <span data-ttu-id="2a253-173">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="2a253-173">Click **Add**.</span></span>
    
    <span data-ttu-id="2a253-174">c.</span><span class="sxs-lookup"><span data-stu-id="2a253-174">c.</span></span> <span data-ttu-id="2a253-175">Típus **project.json**, és nyomja le az ENTER billentyűt.</span><span class="sxs-lookup"><span data-stu-id="2a253-175">Type **project.json**, and then press Enter.</span></span>
    
    <span data-ttu-id="2a253-176">d.</span><span class="sxs-lookup"><span data-stu-id="2a253-176">d.</span></span> <span data-ttu-id="2a253-177">A hello **project.json** fájlt, illessze be a kódját a következő hello:</span><span class="sxs-lookup"><span data-stu-id="2a253-177">In hello **project.json** file, paste hello following code:</span></span>

    ```
    {
    "frameworks": {
        "net46":{
        "dependencies": {
            "windowsazure.storage": "8.1.1"
        }
        }
    }
    }

    ```

    <span data-ttu-id="2a253-178">e.</span><span class="sxs-lookup"><span data-stu-id="2a253-178">e.</span></span> <span data-ttu-id="2a253-179">Kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="2a253-179">Click **Save**.</span></span>

<span data-ttu-id="2a253-180">Egy Azure függvény hozott létre.</span><span class="sxs-lookup"><span data-stu-id="2a253-180">You have created an Azure function.</span></span> <span data-ttu-id="2a253-181">Ez a funkció akkor váltódik ki, minden alkalommal, amikor egy új blob hello adat-átalakítási feladat által generált.</span><span class="sxs-lookup"><span data-stu-id="2a253-181">This function is triggered each time a new blob is generated by hello data-transformation job.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2a253-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2a253-182">Next steps</span></span>

[<span data-ttu-id="2a253-183">StorSimple adatokat kezelő felhasználói felületén tootransform az adatok használata</span><span class="sxs-lookup"><span data-stu-id="2a253-183">Use StorSimple Data Manager UI tootransform your data</span></span>](storsimple-data-manager-ui.md)
