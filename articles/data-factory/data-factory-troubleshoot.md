---
title: "Azure Data Factory elhárítása"
description: "További tudnivalók az Azure Data Factory használatával kapcsolatos problémák elhárítása."
services: data-factory
documentationcenter: 
author: spelluru
manager: jhubbard
editor: monicar
ms.assetid: 38fd14c1-5bb7-4eef-a9f5-b289ff9a6942
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/14/2017
ms.author: spelluru
ms.openlocfilehash: 953a2703db7c8991f580a7c963d6cbd94265c213
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-data-factory-issues"></a><span data-ttu-id="a8be6-103">Data Factory-hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="a8be6-103">Troubleshoot Data Factory issues</span></span>
<span data-ttu-id="a8be6-104">Ez a cikk biztosítja a hibaelhárítási tippekért problémák Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="a8be6-104">This article provides troubleshooting tips for issues when using Azure Data Factory.</span></span> <span data-ttu-id="a8be6-105">Ez a cikk nem szerepel a lehetséges problémák, a szolgáltatás használata esetén, de bizonyos problémák és általános hibaelhárítási tippek magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="a8be6-105">This article does not list all the possible issues when using the service, but it covers some issues and general troubleshooting tips.</span></span>   

## <a name="troubleshooting-tips"></a><span data-ttu-id="a8be6-106">Hibaelhárítási tippek</span><span class="sxs-lookup"><span data-stu-id="a8be6-106">Troubleshooting tips</span></span>
### <a name="error-the-subscription-is-not-registered-to-use-namespace-microsoftdatafactory"></a><span data-ttu-id="a8be6-107">Hiba: Az előfizetés nincs regisztrálva a Microsoft.DataFactory névtér használatára</span><span class="sxs-lookup"><span data-stu-id="a8be6-107">Error: The subscription is not registered to use namespace 'Microsoft.DataFactory'</span></span>
<span data-ttu-id="a8be6-108">Amennyiben ezt a hibaüzenetet kapja, az Azure Data Factory erőforrás-szolgáltató nincs regisztrálva a számítógépén.</span><span class="sxs-lookup"><span data-stu-id="a8be6-108">If you receive this error, the Azure Data Factory resource provider has not been registered on your machine.</span></span> <span data-ttu-id="a8be6-109">Tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="a8be6-109">Do the following:</span></span>

1. <span data-ttu-id="a8be6-110">Indítsa el az Azure PowerShellt.</span><span class="sxs-lookup"><span data-stu-id="a8be6-110">Launch Azure PowerShell.</span></span>
2. <span data-ttu-id="a8be6-111">Jelentkezzen be az Azure-fiókjával az alábbi parancs segítségével.</span><span class="sxs-lookup"><span data-stu-id="a8be6-111">Log in to your Azure account using the following command.</span></span>

    ```powershell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="a8be6-112">A következő parancsot az Azure Data Factory-szolgáltató regisztrálásához.</span><span class="sxs-lookup"><span data-stu-id="a8be6-112">Run the following command to register the Azure Data Factory provider.</span></span>

    ```powershell        
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a><span data-ttu-id="a8be6-113">Probléma: Nem engedélyezett hiba a Data Factory parancsmag futtatásakor</span><span class="sxs-lookup"><span data-stu-id="a8be6-113">Problem: Unauthorized error when running a Data Factory cmdlet</span></span>
<span data-ttu-id="a8be6-114">Valószínűleg nem a megfelelő Azure-fiókot vagy előfizetést használja az Azure PowerShell futtatásához.</span><span class="sxs-lookup"><span data-stu-id="a8be6-114">You are probably not using the right Azure account or subscription with the Azure PowerShell.</span></span> <span data-ttu-id="a8be6-115">Az alábbi parancsmagokkal válassza ki a megfelelő Azure-fiókot és előfizetést az Azure PowerShell használatához.</span><span class="sxs-lookup"><span data-stu-id="a8be6-115">Use the following cmdlets to select the right Azure account and subscription to use with the Azure PowerShell.</span></span>

1. <span data-ttu-id="a8be6-116">Login-AzureRmAccount - használja a megfelelő felhasználói Azonosítót és jelszót</span><span class="sxs-lookup"><span data-stu-id="a8be6-116">Login-AzureRmAccount - Use the right user ID and password</span></span>
2. <span data-ttu-id="a8be6-117">Get-AzureRmSubscription – a fiókhoz tartozó előfizetések megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="a8be6-117">Get-AzureRmSubscription - View all the subscriptions for the account.</span></span>
3. <span data-ttu-id="a8be6-118">SELECT-AzureRmSubscription &lt;előfizetés neve&gt; -válassza ki a megfelelő előfizetést.</span><span class="sxs-lookup"><span data-stu-id="a8be6-118">Select-AzureRmSubscription &lt;subscription name&gt; - Select the right subscription.</span></span> <span data-ttu-id="a8be6-119">Használja ugyanazt, adat-előállító létrehozása az Azure portál használatával.</span><span class="sxs-lookup"><span data-stu-id="a8be6-119">Use the same one you use to create a data factory on the Azure portal.</span></span>

### <a name="problem-fail-to-launch-data-management-gateway-express-setup-from-azure-portal"></a><span data-ttu-id="a8be6-120">Probléma: Nem sikerült indítsa el az adatok Management Gateway Express telepítése Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="a8be6-120">Problem: Fail to launch Data Management Gateway Express Setup from Azure portal</span></span>
<span data-ttu-id="a8be6-121">Az adatkezelési átjáró expressz telepítéséhez az Internet Explorer vagy egy Microsoft ClickOnce-kompatiblis webböngésző szükséges.</span><span class="sxs-lookup"><span data-stu-id="a8be6-121">The Express setup for the Data Management Gateway requires Internet Explorer or a Microsoft ClickOnce compatible web browser.</span></span> <span data-ttu-id="a8be6-122">Amennyiben az expressz telepítés nem indul el, tegye a következők egyikét:</span><span class="sxs-lookup"><span data-stu-id="a8be6-122">If the Express Setup fails to start, do one of the following:</span></span>

* <span data-ttu-id="a8be6-123">Az Internet Explorer vagy a Microsoft ClickOnce kompatibilis böngésző használata.</span><span class="sxs-lookup"><span data-stu-id="a8be6-123">Use Internet Explorer or a Microsoft ClickOnce compatible web browser.</span></span>

    <span data-ttu-id="a8be6-124">Amennyiben Chrome böngészőt használ, keresse fel a [Chrome webáruházát](https://chrome.google.com/webstore/), keressen rá a „ClickOnce” kulcsszóra, válassza ki az egyik ClickOnce-bővítményt, majd telepítse azt.</span><span class="sxs-lookup"><span data-stu-id="a8be6-124">If you are using Chrome, go to the [Chrome web store](https://chrome.google.com/webstore/), search with "ClickOnce" keyword, choose one of the ClickOnce extensions, and install it.</span></span>

    <span data-ttu-id="a8be6-125">Tegye meg ugyanezt a Firefox (install-bővítmény).</span><span class="sxs-lookup"><span data-stu-id="a8be6-125">Do the same for Firefox (install add-in).</span></span> <span data-ttu-id="a8be6-126">Kattintson az eszköztár Menü megnyitása gombjára (három vízszintes csík a jobb felső sarokban). Kattintson a Kiegészítők menüpontra, keressen a „ClickOnce” kulcsszóra, jelöljön ki egy ClickOnce-bővítmény, és telepítse azt.</span><span class="sxs-lookup"><span data-stu-id="a8be6-126">Click Open Menu button on the toolbar (three horizontal lines in the top-right corner), click Add-ons, search with "ClickOnce" keyword, choose one of the ClickOnce extensions, and install it.</span></span>
* <span data-ttu-id="a8be6-127">Használja a **manuális telepítés** a portálon azonos panelen látható hivatkozásra.</span><span class="sxs-lookup"><span data-stu-id="a8be6-127">Use the **Manual Setup** link shown on the same blade in the portal.</span></span> <span data-ttu-id="a8be6-128">Ezt a módszert használja a telepítőfájl letöltéséhez, és manuális futtatása.</span><span class="sxs-lookup"><span data-stu-id="a8be6-128">You use this approach to download installation file and run it manually.</span></span> <span data-ttu-id="a8be6-129">A telepítés befejezését követően az adatok adatkezelési átjáró konfigurálása párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a8be6-129">After the installation is successful, you see the Data Management Gateway Configuration dialog box.</span></span> <span data-ttu-id="a8be6-130">Másolja ki a **kulcsot** a Portál képernyőről, és a konfigurációkezelőben ezt használva regisztrálja az átjárót a szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="a8be6-130">Copy the **key** from the portal screen and use it in the configuration manager to manually register the gateway with the service.</span></span>  

### <a name="problem-fail-to-connect-to-on-premises-sql-server"></a><span data-ttu-id="a8be6-131">Probléma: Nem sikerült kapcsolódni a helyszíni SQL Server</span><span class="sxs-lookup"><span data-stu-id="a8be6-131">Problem: Fail to connect to on-premises SQL Server</span></span>
<span data-ttu-id="a8be6-132">Indítsa el **az adatkezelési átjáró konfigurációkezelőjének** az átjárót működtető gépen, és használja a **hibaelhárítás** lapon, a kapcsolat tesztelése az SQL-kiszolgálón az átjárót működtető gépen.</span><span class="sxs-lookup"><span data-stu-id="a8be6-132">Launch **Data Management Gateway Configuration Manager** on the gateway machine and use the **Troubleshooting** tab to test the connection to SQL Server from the gateway machine.</span></span> <span data-ttu-id="a8be6-133">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="a8be6-133">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>   

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a><span data-ttu-id="a8be6-134">Probléma: Ha valaha is várakozó állapotban vannak bemeneti szeletek</span><span class="sxs-lookup"><span data-stu-id="a8be6-134">Problem: Input slices are in Waiting state for ever</span></span>
<span data-ttu-id="a8be6-135">A szeletek lehet a **Várakozás** állapot különféle okok miatt.</span><span class="sxs-lookup"><span data-stu-id="a8be6-135">The slices could be in **Waiting** state due to various reasons.</span></span> <span data-ttu-id="a8be6-136">A gyakori okai, hogy a **külső** tulajdonság értéke nem **igaz**.</span><span class="sxs-lookup"><span data-stu-id="a8be6-136">One of the common reasons is that the **external** property is not set to **true**.</span></span> <span data-ttu-id="a8be6-137">Bármely Azure Data Factory hatókörén kívül előállított dataset fel kell tüntetni **külső** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="a8be6-137">Any dataset that is produced outside the scope of Azure Data Factory should be marked with **external** property.</span></span> <span data-ttu-id="a8be6-138">Ez a tulajdonság jelzi, hogy az adatok külső és nem mentett adat-előállító belül bármely folyamatok által-e.</span><span class="sxs-lookup"><span data-stu-id="a8be6-138">This property indicates that the data is external and not backed by any pipelines within the data factory.</span></span> <span data-ttu-id="a8be6-139">Az adatszeletek **Készként** vannak jelölve, amint elérhetőek az adatok a megfelelő tárban.</span><span class="sxs-lookup"><span data-stu-id="a8be6-139">The data slices are marked as **Ready** once the data is available in the respective store.</span></span>

<span data-ttu-id="a8be6-140">Tekintse meg a következő példát az **external** tulajdonság használatáról.</span><span class="sxs-lookup"><span data-stu-id="a8be6-140">See the following example for the usage of the **external** property.</span></span> <span data-ttu-id="a8be6-141">Opcionálisan megadhat **externalData*** beállításakor külső igaz értékre.</span><span class="sxs-lookup"><span data-stu-id="a8be6-141">You can optionally specify **externalData*** when you set external to true.</span></span>

<span data-ttu-id="a8be6-142">Lásd: [adatkészletek](data-factory-create-datasets.md) cikk részletesebb ezt a tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="a8be6-142">See [Datasets](data-factory-create-datasets.md) article for more details about this property.</span></span>

```json
{
  "name": "CustomerTable",
  "properties": {
    "type": "AzureBlob",
    "linkedServiceName": "MyLinkedService",
    "typeProperties": {
      "folderPath": "MyContainer/MySubFolder/",
      "format": {
        "type": "TextFormat",
        "columnDelimiter": ",",
        "rowDelimiter": ";"
      }
    },
    "external": true,
    "availability": {
      "frequency": "Hour",
      "interval": 1
    },
    "policy": {
      }
    }
  }
}
```

<span data-ttu-id="a8be6-143">A hiba megoldásához adja hozzá az **external** tulajdonságot és a választható **externalData** szakaszt a bemeneti tábla JSON-definíciójához, és hozza létre ismét a táblát.</span><span class="sxs-lookup"><span data-stu-id="a8be6-143">To resolve the error, add the **external** property and the optional **externalData** section to the JSON definition of the input table and recreate the table.</span></span>

### <a name="problem-hybrid-copy-operation-fails"></a><span data-ttu-id="a8be6-144">Probléma: Hibrid másolás nem lehetséges</span><span class="sxs-lookup"><span data-stu-id="a8be6-144">Problem: Hybrid copy operation fails</span></span>
<span data-ttu-id="a8be6-145">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) lépéseket elhárítása onnan egy a helyszíni adatok másolása az adatkezelési átjáró használatával tárolja.</span><span class="sxs-lookup"><span data-stu-id="a8be6-145">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for steps to troubleshoot issues with copying to/from an on-premises data store using the Data Management Gateway.</span></span>

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a><span data-ttu-id="a8be6-146">Probléma: Igény szerinti HDInsight kiépítése sikertelen</span><span class="sxs-lookup"><span data-stu-id="a8be6-146">Problem: On-demand HDInsight provisioning fails</span></span>
<span data-ttu-id="a8be6-147">HDInsightOnDemand típusú társított szolgáltatás használata esetén meg kell adnia egy linkedServiceName, amely egy Azure Blob Storage mutat.</span><span class="sxs-lookup"><span data-stu-id="a8be6-147">When using a linked service of type HDInsightOnDemand, you need to specify a linkedServiceName that points to an Azure Blob Storage.</span></span> <span data-ttu-id="a8be6-148">A Data Factory szolgáltatás ezt a tárolót használja az igény szerinti HDInsight-fürt naplófájljainak és a kapcsolódó fájloknak a tárolásához.</span><span class="sxs-lookup"><span data-stu-id="a8be6-148">Data Factory service uses this storage to store logs and supporting files for your on-demand HDInsight cluster.</span></span>  <span data-ttu-id="a8be6-149">Néha az igény szerinti HDInsight-fürt kiépítése meghiúsul, és a következő hibaüzenet jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="a8be6-149">Sometimes provisioning of an on-demand HDInsight cluster fails with the following error:</span></span>

```
Failed to create cluster. Exception: Unable to complete the cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.
```

<span data-ttu-id="a8be6-150">Ez a hiba többnyire azt jelzi, hogy a linkedServiceName szolgáltatásban megadott tárfiók nem ugyanazon az adatközpont-helyen található, ahol a HDInsight kiépítése történik.</span><span class="sxs-lookup"><span data-stu-id="a8be6-150">This error usually indicates that the location of the storage account specified in the linkedServiceName is not in the same data center location where the HDInsight provisioning is happening.</span></span> <span data-ttu-id="a8be6-151">Példa: Ha a data factory USA nyugati régiója és az Azure storage egy, az USA keleti régiója, az igény szerinti kiépítés sikertelen lesz az USA nyugati régiója.</span><span class="sxs-lookup"><span data-stu-id="a8be6-151">Example: if your data factory is in West US and the Azure storage is in East US, the on-demand provisioning fails in West US.</span></span>

<span data-ttu-id="a8be6-152">Az additionalLinkedServiceNames egy másik JSON-tulajdonság, amelyben további tárfiókok adhatók meg az igény szerinti HDInsight szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="a8be6-152">Additionally, there is a second JSON property additionalLinkedServiceNames where additional storage accounts may be specified in on-demand HDInsight.</span></span> <span data-ttu-id="a8be6-153">Ezen további társított tárfiókokat és a HDInsight-fürt ugyanazon a helyen kell lennie, vagy a azonos hiba miatt sikertelen.</span><span class="sxs-lookup"><span data-stu-id="a8be6-153">Those additional linked storage accounts should be in the same location as the HDInsight cluster, or it fails with the same error.</span></span>

### <a name="problem-custom-net-activity-fails"></a><span data-ttu-id="a8be6-154">Probléma: Egyéni .NET tevékenység sikertelen</span><span class="sxs-lookup"><span data-stu-id="a8be6-154">Problem: Custom .NET activity fails</span></span>
<span data-ttu-id="a8be6-155">Lásd: [folyamat hibakeresése egyéni tevékenységeket](data-factory-use-custom-activities.md#troubleshoot-failures) a részletes lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a8be6-155">See [Debug a pipeline with custom activity](data-factory-use-custom-activities.md#troubleshoot-failures) for detailed steps.</span></span>

## <a name="use-azure-portal-to-troubleshoot"></a><span data-ttu-id="a8be6-156">Az Azure portál használatával hibáinak elhárítása</span><span class="sxs-lookup"><span data-stu-id="a8be6-156">Use Azure portal to troubleshoot</span></span>
### <a name="using-portal-blades"></a><span data-ttu-id="a8be6-157">Portál paneleken használatával</span><span class="sxs-lookup"><span data-stu-id="a8be6-157">Using portal blades</span></span>
<span data-ttu-id="a8be6-158">Lásd: [figyelő folyamat](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) lépéseket.</span><span class="sxs-lookup"><span data-stu-id="a8be6-158">See [Monitor pipeline](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) for steps.</span></span>

### <a name="using-monitor-and-manage-app"></a><span data-ttu-id="a8be6-159">A Megfigyelés és kezelés alkalmazás használata</span><span class="sxs-lookup"><span data-stu-id="a8be6-159">Using Monitor and Manage App</span></span>
<span data-ttu-id="a8be6-160">Lásd: [figyelése és kezelése a data factory folyamatok figyelése és kezelése App](data-factory-monitor-manage-app.md) részleteket.</span><span class="sxs-lookup"><span data-stu-id="a8be6-160">See [Monitor and manage data factory pipelines using Monitor and Manage App](data-factory-monitor-manage-app.md) for details.</span></span>

## <a name="use-azure-powershell-to-troubleshoot"></a><span data-ttu-id="a8be6-161">Hibaelhárítás az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="a8be6-161">Use Azure PowerShell to troubleshoot</span></span>
### <a name="use-azure-powershell-to-troubleshoot-an-error"></a><span data-ttu-id="a8be6-162">Egy hiba elhárítása az Azure PowerShell használatával</span><span class="sxs-lookup"><span data-stu-id="a8be6-162">Use Azure PowerShell to troubleshoot an error</span></span>
<span data-ttu-id="a8be6-163">Lásd: [képernyő adat-előállító folyamatok, az Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="a8be6-163">See [Monitor Data Factory pipelines using Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) for details.</span></span>

[adfgetstarted]: data-factory-copy-data-from-azure-blob-storage-to-sql-database.md
[use-custom-activities]: data-factory-use-custom-activities.md
[troubleshoot]: data-factory-troubleshoot.md
[developer-reference]: http://go.microsoft.com/fwlink/?LinkId=516908
[cmdlet-reference]: http://go.microsoft.com/fwlink/?LinkId=517456
[json-scripting-reference]: http://go.microsoft.com/fwlink/?LinkId=516971

[azure-portal]: https://portal.azure.com/

[image-data-factory-troubleshoot-with-error-link]: ./media/data-factory-troubleshoot/DataFactoryWithErrorLink.png

[image-data-factory-troubleshoot-datasets-with-errors-blade]: ./media/data-factory-troubleshoot/DatasetsWithErrorsBlade.png

[image-data-factory-troubleshoot-table-blade-with-problem-slices]: ./media/data-factory-troubleshoot/TableBladeWithProblemSlices.png

[image-data-factory-troubleshoot-activity-run-with-error]: ./media/data-factory-troubleshoot/ActivityRunDetailsWithError.png

[image-data-factory-troubleshoot-dataslice-blade-with-active-runs]: ./media/data-factory-troubleshoot/DataSliceBladeWithActivityRuns.png

[image-data-factory-troubleshoot-walkthrough2-with-errors-link]: ./media/data-factory-troubleshoot/Walkthrough2WithErrorsLink.png

[image-data-factory-troubleshoot-walkthrough2-datasets-with-errors]: ./media/data-factory-troubleshoot/Walkthrough2DataSetsWithErrors.png

[image-data-factory-troubleshoot-walkthrough2-table-with-problem-slices]: ./media/data-factory-troubleshoot/Walkthrough2TableProblemSlices.png

[image-data-factory-troubleshoot-walkthrough2-slice-activity-runs]: ./media/data-factory-troubleshoot/Walkthrough2DataSliceActivityRuns.png

[image-data-factory-troubleshoot-activity-run-details]: ./media/data-factory-troubleshoot/Walkthrough2ActivityRunDetails.png
