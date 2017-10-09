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
# <a name="change-a-blob-path-from-hello-default-path-private-preview"></a>Egy blob elérési utat módosítsa hello alapértelmezett elérési út (magán előnézetben)

Ez a cikk ismerteti, hogyan tooset be egy Azure működni toorename egy alapértelmezett blob fájl elérési útját. 

## <a name="prerequisites"></a>Előfeltételek

Győződjön meg arról, hogy rendelkezik-e olyan feladatdefinícióban erőforráscsoporton belül egy hibrid adatforrás, melyhez a megfelelően konfigurált.

## <a name="create-an-azure-function"></a>Egy Azure-függvény létrehozása

egy Azure függvény toocreate hello a következő:

1. Nyissa meg toohello [Azure-portálon](http://portal.azure.com/).

2. A bal oldali ablaktáblán hello hello tetején kattintson **új**. 

3. A hello **keresési** mezőbe írja be **függvény App**, és nyomja le az ENTER billentyűt.

    ![Hello keresőmezőbe írja be a "Függvény alkalmazás"](./media/storsimple-data-manager-change-default-blob-path/goto-function-app-resource.png)

4. A hello **eredmények** listában, kattintson **függvény App**.

    ![Válassza ki a hello függvény alkalmazás-erőforrást hello eredmények listájában](./media/storsimple-data-manager-change-default-blob-path/select-function-app-resource.png)

    Hello **függvény App** ablak nyílik meg.

5. Kattintson a **Create** (Létrehozás) gombra.

    ![hello függvény App ablak "Létrehozás" gombra](./media/storsimple-data-manager-change-default-blob-path/create-new-function-app.png)

6. A hello **függvény App** konfigurációs panelen a következő hello:

    a. A hello **alkalmazásnév** mezőjébe hello alkalmazás neve.
    
    b. A hello **előfizetés** mezőjébe hello előfizetés hello nevét.

    c. A **erőforráscsoport**, kattintson a **hozzon létre új**, és majd hello típusnév hello erőforráscsoport.

    d. A hello **üzemeltetési terv** mezőbe írja be **fogyasztás megtervezése**.

    e. A hello **hely** mezőbe típusú hello helyet.

    f. A **tárfiók**, válasszon egy meglévő tárfiókot, vagy hozzon létre egy új tárfiókot. A tárfiók belső használatra készült a hello függvény.

    ![Adja meg az új függvény alkalmazás-konfigurációs adatokat](./media/storsimple-data-manager-change-default-blob-path/enter-new-funcion-app-data.png)

7. Kattintson a **Create** (Létrehozás) gombra.  
    hello függvény alkalmazás létrehozása.

8. Hello bal oldali ablaktáblában kattintson **további szolgáltatások**, és ezután hello a következő:
    
    a. A hello **szűrő** mezőbe írja be **alkalmazásszolgáltatások**.
    
    b. Kattintson a **alkalmazásszolgáltatások**. 

    !["További szolgáltatások" hivatkozás hello bal oldali panelen](./media/storsimple-data-manager-change-default-blob-path/more-services.png)

9. App Service szolgáltatások hello listájában kattintson hello függvény alkalmazás hello nevét.  
    Megnyílik a hello függvény oldalra.

10. Hello bal oldali ablaktáblában kattintson **új függvény**, és ezután hello a következő: 

    a. A hello **nyelvi** listáról válassza ki **C#**.
    
    b. Hello tömb sablon csempék, válassza ki a **QueueTrigger-c Sharp**.

    c. A hello **a függvény neve** mezőbe írja be a függvény nevét.

    d. A hello **üzenetsornév** mezőbe írja be az adat-átalakítási feladat definition nevét.

    e. A **fiók tárolókapcsolat**, kattintson a **új**, majd válassza ki a megfelelő toohello adat-átalakítási feladat hello fiók.  
        Jegyezze fel a hello kapcsolat neve. a későbbi hello Azure függvény, meg kell adni a hello nevet.

       ![Egy új C#-függvény létrehozása](./media/storsimple-data-manager-change-default-blob-path/create-new-csharp-function.png)

    f. Kattintson a **Create** (Létrehozás) gombra.  
    Hello **függvény** ablak nyílik meg.

11. A hello **függvény** ablakban futtassa _.csx_ fájlt, és ezután hello a következő:

    a. Illessze be a kódját a következő hello:

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

    b. Cserélje le **STORAGE_CONNECTIONNAME** a sor a tárolási fiók kapcsolattal 11 (lásd a pont 8 c).

    c. Kattintson a lap tetején hello balra **mentése**.

    ![Függvény mentése](./media/storsimple-data-manager-change-default-blob-path/save-function.png)

12. toocomplete hello függvény, adjon hozzá egy további fájlt hello következő tevékenységek végrehajtásával:

    a. Kattintson a **-fájlokat tekinthetnek meg**.

       ![hello "Fájlok megtekintése" hivatkozásra](./media/storsimple-data-manager-change-default-blob-path/view-files.png)

    b. Kattintson az **Add** (Hozzáadás) parancsra.
    
    c. Típus **project.json**, és nyomja le az ENTER billentyűt.
    
    d. A hello **project.json** fájlt, illessze be a kódját a következő hello:

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

    e. Kattintson a **Save** (Mentés) gombra.

Egy Azure függvény hozott létre. Ez a funkció akkor váltódik ki, minden alkalommal, amikor egy új blob hello adat-átalakítási feladat által generált.

## <a name="next-steps"></a>Következő lépések

[StorSimple adatokat kezelő felhasználói felületén tootransform az adatok használata](storsimple-data-manager-ui.md)
