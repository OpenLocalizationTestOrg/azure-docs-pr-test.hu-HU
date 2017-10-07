---
title: "aaaTroubleshoot Azure Data Factory problémák"
description: "Ismerje meg, hogyan tootroubleshoot állít ki az Azure Data Factory használatával."
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
ms.openlocfilehash: cf65bcf3e1c3f061d3ac1dbf32e99cc7b014f9dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-data-factory-issues"></a><span data-ttu-id="c43ea-103">Data Factory-hibák elhárítása</span><span class="sxs-lookup"><span data-stu-id="c43ea-103">Troubleshoot Data Factory issues</span></span>
<span data-ttu-id="c43ea-104">Ez a cikk biztosítja a hibaelhárítási tippekért problémák Azure Data Factory használatával.</span><span class="sxs-lookup"><span data-stu-id="c43ea-104">This article provides troubleshooting tips for issues when using Azure Data Factory.</span></span> <span data-ttu-id="c43ea-105">Ez a cikk nem szerepel minden hello lehetséges problémákat hello szolgáltatás használatakor, de bizonyos problémák és általános hibaelhárítási tippek magában foglalja.</span><span class="sxs-lookup"><span data-stu-id="c43ea-105">This article does not list all hello possible issues when using hello service, but it covers some issues and general troubleshooting tips.</span></span>   

## <a name="troubleshooting-tips"></a><span data-ttu-id="c43ea-106">Hibaelhárítási tippek</span><span class="sxs-lookup"><span data-stu-id="c43ea-106">Troubleshooting tips</span></span>
### <a name="error-hello-subscription-is-not-registered-toouse-namespace-microsoftdatafactory"></a><span data-ttu-id="c43ea-107">Hiba: hello előfizetés nincs regisztrált toouse névtér "Microsoft.DataFactory"</span><span class="sxs-lookup"><span data-stu-id="c43ea-107">Error: hello subscription is not registered toouse namespace 'Microsoft.DataFactory'</span></span>
<span data-ttu-id="c43ea-108">Ha ez a hibaüzenet jelenik meg, hello Azure Data Factory erőforrás-szolgáltató nincs regisztrálva a számítógépen.</span><span class="sxs-lookup"><span data-stu-id="c43ea-108">If you receive this error, hello Azure Data Factory resource provider has not been registered on your machine.</span></span> <span data-ttu-id="c43ea-109">A következő hello:</span><span class="sxs-lookup"><span data-stu-id="c43ea-109">Do hello following:</span></span>

1. <span data-ttu-id="c43ea-110">Indítsa el az Azure PowerShellt.</span><span class="sxs-lookup"><span data-stu-id="c43ea-110">Launch Azure PowerShell.</span></span>
2. <span data-ttu-id="c43ea-111">Jelentkezzen be tooyour Azure-fiók hello a következő parancs használatával.</span><span class="sxs-lookup"><span data-stu-id="c43ea-111">Log in tooyour Azure account using hello following command.</span></span>

    ```powershell
    Login-AzureRmAccount
    ```
3. <span data-ttu-id="c43ea-112">Futtassa a következő parancs tooregister hello Azure Data Factory szolgáltató hello.</span><span class="sxs-lookup"><span data-stu-id="c43ea-112">Run hello following command tooregister hello Azure Data Factory provider.</span></span>

    ```powershell        
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.DataFactory
    ```

### <a name="problem-unauthorized-error-when-running-a-data-factory-cmdlet"></a><span data-ttu-id="c43ea-113">Probléma: Nem engedélyezett hiba a Data Factory parancsmag futtatásakor</span><span class="sxs-lookup"><span data-stu-id="c43ea-113">Problem: Unauthorized error when running a Data Factory cmdlet</span></span>
<span data-ttu-id="c43ea-114">Az Azure PowerShell hello hello jobb Azure-fiók vagy előfizetés valószínűleg nem használ.</span><span class="sxs-lookup"><span data-stu-id="c43ea-114">You are probably not using hello right Azure account or subscription with hello Azure PowerShell.</span></span> <span data-ttu-id="c43ea-115">A következő parancsmagok tooselect hello jobb hello Azure PowerShell-lel rendelkező Azure fiókja és -előfizetése toouse hello használata.</span><span class="sxs-lookup"><span data-stu-id="c43ea-115">Use hello following cmdlets tooselect hello right Azure account and subscription toouse with hello Azure PowerShell.</span></span>

1. <span data-ttu-id="c43ea-116">Login-AzureRmAccount - használata hello megfelelő felhasználói Azonosítót és jelszót</span><span class="sxs-lookup"><span data-stu-id="c43ea-116">Login-AzureRmAccount - Use hello right user ID and password</span></span>
2. <span data-ttu-id="c43ea-117">Get-AzureRmSubscription – összes hello hello fiókhoz előfizetések nézet.</span><span class="sxs-lookup"><span data-stu-id="c43ea-117">Get-AzureRmSubscription - View all hello subscriptions for hello account.</span></span>
3. <span data-ttu-id="c43ea-118">SELECT-AzureRmSubscription &lt;előfizetés neve&gt; -válasszon hello megfelelő előfizetést.</span><span class="sxs-lookup"><span data-stu-id="c43ea-118">Select-AzureRmSubscription &lt;subscription name&gt; - Select hello right subscription.</span></span> <span data-ttu-id="c43ea-119">Használjon hello azonos toocreate egy adat-előállító hello Azure-portált használja.</span><span class="sxs-lookup"><span data-stu-id="c43ea-119">Use hello same one you use toocreate a data factory on hello Azure portal.</span></span>

### <a name="problem-fail-toolaunch-data-management-gateway-express-setup-from-azure-portal"></a><span data-ttu-id="c43ea-120">Hiba: Sikertelen toolaunch adatok Management Gateway Express telepítése Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="c43ea-120">Problem: Fail toolaunch Data Management Gateway Express Setup from Azure portal</span></span>
<span data-ttu-id="c43ea-121">hello Expressz telepítés az adatkezelési átjáró hello Internet Explorer vagy a Microsoft ClickOnce kompatibilis böngésző szükséges.</span><span class="sxs-lookup"><span data-stu-id="c43ea-121">hello Express setup for hello Data Management Gateway requires Internet Explorer or a Microsoft ClickOnce compatible web browser.</span></span> <span data-ttu-id="c43ea-122">Ha hello Express telepítő toostart sikertelen, hello következő módokon:</span><span class="sxs-lookup"><span data-stu-id="c43ea-122">If hello Express Setup fails toostart, do one of hello following:</span></span>

* <span data-ttu-id="c43ea-123">Az Internet Explorer vagy a Microsoft ClickOnce kompatibilis böngésző használata.</span><span class="sxs-lookup"><span data-stu-id="c43ea-123">Use Internet Explorer or a Microsoft ClickOnce compatible web browser.</span></span>

    <span data-ttu-id="c43ea-124">Ha a Chrome használ, lépjen a toohello [Chrome webes tároló](https://chrome.google.com/webstore/), keressen a "ClickOnce" kulcsszóval, válasszon egyet a hello ClickOnce kiterjesztések, és telepítse.</span><span class="sxs-lookup"><span data-stu-id="c43ea-124">If you are using Chrome, go toohello [Chrome web store](https://chrome.google.com/webstore/), search with "ClickOnce" keyword, choose one of hello ClickOnce extensions, and install it.</span></span>

    <span data-ttu-id="c43ea-125">Hello azonos Firefox (install-bővítmény).</span><span class="sxs-lookup"><span data-stu-id="c43ea-125">Do hello same for Firefox (install add-in).</span></span> <span data-ttu-id="c43ea-126">Hello eszköztáron (három vízszintes vonal hello jobb felső sarokban) menü megnyitása gombra, kattintson a bővítmények, keressen a "ClickOnce" kulcsszóval egyikével hello ClickOnce kiválasztása, és telepítse.</span><span class="sxs-lookup"><span data-stu-id="c43ea-126">Click Open Menu button on hello toolbar (three horizontal lines in hello top-right corner), click Add-ons, search with "ClickOnce" keyword, choose one of hello ClickOnce extensions, and install it.</span></span>
* <span data-ttu-id="c43ea-127">Használjon hello **manuális telepítés** hivatkozás jelenik meg a hello hello portálon azonos panelen.</span><span class="sxs-lookup"><span data-stu-id="c43ea-127">Use hello **Manual Setup** link shown on hello same blade in hello portal.</span></span> <span data-ttu-id="c43ea-128">Ez a megközelítés toodownload telepítőfájlt használ, és manuális futtatása.</span><span class="sxs-lookup"><span data-stu-id="c43ea-128">You use this approach toodownload installation file and run it manually.</span></span> <span data-ttu-id="c43ea-129">Hello telepítés befejezését követően hello adatok adatkezelési átjáró konfigurálása párbeszédpanel jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="c43ea-129">After hello installation is successful, you see hello Data Management Gateway Configuration dialog box.</span></span> <span data-ttu-id="c43ea-130">Másolás hello **kulcs** hello Letöltés gombra, és használja azt a hello configuration manager toomanually hello átjáró regisztrálása hello szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="c43ea-130">Copy hello **key** from hello portal screen and use it in hello configuration manager toomanually register hello gateway with hello service.</span></span>  

### <a name="problem-fail-tooconnect-tooon-premises-sql-server"></a><span data-ttu-id="c43ea-131">Hiba: Sikertelen tooconnect tooon helyszíni SQL Server</span><span class="sxs-lookup"><span data-stu-id="c43ea-131">Problem: Fail tooconnect tooon-premises SQL Server</span></span>
<span data-ttu-id="c43ea-132">Indítsa el **az adatkezelési átjáró konfigurációkezelőjének** az átjárót működtető gépen hello és hello használata **hibaelhárítás** tootest hello kapcsolat tooSQL Server hello átjáró gépről fülre.</span><span class="sxs-lookup"><span data-stu-id="c43ea-132">Launch **Data Management Gateway Configuration Manager** on hello gateway machine and use hello **Troubleshooting** tab tootest hello connection tooSQL Server from hello gateway machine.</span></span> <span data-ttu-id="c43ea-133">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) kapcsolati/átjáró hibaelhárítási tippek a kapcsolódó problémákat.</span><span class="sxs-lookup"><span data-stu-id="c43ea-133">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for tips on troubleshooting connection/gateway related issues.</span></span>   

### <a name="problem-input-slices-are-in-waiting-state-for-ever"></a><span data-ttu-id="c43ea-134">Probléma: Ha valaha is várakozó állapotban vannak bemeneti szeletek</span><span class="sxs-lookup"><span data-stu-id="c43ea-134">Problem: Input slices are in Waiting state for ever</span></span>
<span data-ttu-id="c43ea-135">lehet, hogy hello szeletek a **Várakozás** állapot toovarious okok miatt.</span><span class="sxs-lookup"><span data-stu-id="c43ea-135">hello slices could be in **Waiting** state due toovarious reasons.</span></span> <span data-ttu-id="c43ea-136">Hello gyakori okai egyik adott hello **külső** tulajdonság értéke nem túl**igaz**.</span><span class="sxs-lookup"><span data-stu-id="c43ea-136">One of hello common reasons is that hello **external** property is not set too**true**.</span></span> <span data-ttu-id="c43ea-137">Bármely adatkészlet hatókörében létrehozott külső hello Azure Data Factory, fel kell tüntetni **külső** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="c43ea-137">Any dataset that is produced outside hello scope of Azure Data Factory should be marked with **external** property.</span></span> <span data-ttu-id="c43ea-138">Ez a tulajdonság jelzi, hogy hello adatok-e a külső és a nem mentett minden belül hello adat-előállító adatcsatornák által.</span><span class="sxs-lookup"><span data-stu-id="c43ea-138">This property indicates that hello data is external and not backed by any pipelines within hello data factory.</span></span> <span data-ttu-id="c43ea-139">hello adatszeletek fel van tüntetve **készen** hello adatok elérhetővé válik a hello megfelelő áruházban.</span><span class="sxs-lookup"><span data-stu-id="c43ea-139">hello data slices are marked as **Ready** once hello data is available in hello respective store.</span></span>

<span data-ttu-id="c43ea-140">Tekintse meg a következő példa hello használati hello hello **külső** tulajdonság.</span><span class="sxs-lookup"><span data-stu-id="c43ea-140">See hello following example for hello usage of hello **external** property.</span></span> <span data-ttu-id="c43ea-141">Opcionálisan megadhat **externalData*** külső tootrue beállításakor.</span><span class="sxs-lookup"><span data-stu-id="c43ea-141">You can optionally specify **externalData*** when you set external tootrue.</span></span>

<span data-ttu-id="c43ea-142">Lásd: [adatkészletek](data-factory-create-datasets.md) cikk részletesebb ezt a tulajdonságot.</span><span class="sxs-lookup"><span data-stu-id="c43ea-142">See [Datasets](data-factory-create-datasets.md) article for more details about this property.</span></span>

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

<span data-ttu-id="c43ea-143">tooresolve hello hiba, vegye fel a hello **külső** tulajdonság és az opcionális hello **externalData** toohello JSON-definícióból hello bemeneti tábla szakaszt, és hozza létre újra a hello tábla.</span><span class="sxs-lookup"><span data-stu-id="c43ea-143">tooresolve hello error, add hello **external** property and hello optional **externalData** section toohello JSON definition of hello input table and recreate hello table.</span></span>

### <a name="problem-hybrid-copy-operation-fails"></a><span data-ttu-id="c43ea-144">Probléma: Hibrid másolás nem lehetséges</span><span class="sxs-lookup"><span data-stu-id="c43ea-144">Problem: Hybrid copy operation fails</span></span>
<span data-ttu-id="c43ea-145">Lásd: [átjáró elhárítása](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) vonatkozó lépéseket tootroubleshoot problémákat másolása onnan egy a helyszíni adatok tárolására használatával hello az adatkezelési átjáró.</span><span class="sxs-lookup"><span data-stu-id="c43ea-145">See [Troubleshoot gateway issues](data-factory-data-management-gateway.md#troubleshooting-gateway-issues) for steps tootroubleshoot issues with copying to/from an on-premises data store using hello Data Management Gateway.</span></span>

### <a name="problem-on-demand-hdinsight-provisioning-fails"></a><span data-ttu-id="c43ea-146">Probléma: Igény szerinti HDInsight kiépítése sikertelen</span><span class="sxs-lookup"><span data-stu-id="c43ea-146">Problem: On-demand HDInsight provisioning fails</span></span>
<span data-ttu-id="c43ea-147">HDInsightOnDemand típusú társított szolgáltatás használatakor a linkedServiceName mutat, tooan Azure Blob Storage toospecify kell.</span><span class="sxs-lookup"><span data-stu-id="c43ea-147">When using a linked service of type HDInsightOnDemand, you need toospecify a linkedServiceName that points tooan Azure Blob Storage.</span></span> <span data-ttu-id="c43ea-148">Data Factory szolgáltatásnak használ a tárolási toostore naplók és egyéb szükséges fájlokat a igény szerinti HDInsight-fürthöz.</span><span class="sxs-lookup"><span data-stu-id="c43ea-148">Data Factory service uses this storage toostore logs and supporting files for your on-demand HDInsight cluster.</span></span>  <span data-ttu-id="c43ea-149">Egyes esetekben az igény szerinti HDInsight-fürtök kiépítése meghiúsul, és a következő hiba hello:</span><span class="sxs-lookup"><span data-stu-id="c43ea-149">Sometimes provisioning of an on-demand HDInsight cluster fails with hello following error:</span></span>

```
Failed toocreate cluster. Exception: Unable toocomplete hello cluster create operation. Operation failed with code '400'. Cluster left behind state: 'Error'. Message: 'StorageAccountNotColocated'.
```

<span data-ttu-id="c43ea-150">Ez a hiba általában azt jelzi, hogy hello linkedServiceName megadott hello tárfiók hello helye nem a hello azonos adatközpont-helyet, ahol hello HDInsight kiépítése történik.</span><span class="sxs-lookup"><span data-stu-id="c43ea-150">This error usually indicates that hello location of hello storage account specified in hello linkedServiceName is not in hello same data center location where hello HDInsight provisioning is happening.</span></span> <span data-ttu-id="c43ea-151">Példa: Ha a data factory USA nyugati régiója és az Azure storage hello az USA keleti régiója, hello igény létesítési meghiúsul az USA nyugati régiója.</span><span class="sxs-lookup"><span data-stu-id="c43ea-151">Example: if your data factory is in West US and hello Azure storage is in East US, hello on-demand provisioning fails in West US.</span></span>

<span data-ttu-id="c43ea-152">Az additionalLinkedServiceNames egy másik JSON-tulajdonság, amelyben további tárfiókok adhatók meg az igény szerinti HDInsight szolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="c43ea-152">Additionally, there is a second JSON property additionalLinkedServiceNames where additional storage accounts may be specified in on-demand HDInsight.</span></span> <span data-ttu-id="c43ea-153">Ezen további kapcsolt tárfiókot kell hello ugyanazon a helyen hello HDInsight-fürtöt, vagy nem sikerül a hello ugyanezt a hibaüzenetet.</span><span class="sxs-lookup"><span data-stu-id="c43ea-153">Those additional linked storage accounts should be in hello same location as hello HDInsight cluster, or it fails with hello same error.</span></span>

### <a name="problem-custom-net-activity-fails"></a><span data-ttu-id="c43ea-154">Probléma: Egyéni .NET tevékenység sikertelen</span><span class="sxs-lookup"><span data-stu-id="c43ea-154">Problem: Custom .NET activity fails</span></span>
<span data-ttu-id="c43ea-155">Lásd: [folyamat hibakeresése egyéni tevékenységeket](data-factory-use-custom-activities.md#troubleshoot-failures) a részletes lépéseket.</span><span class="sxs-lookup"><span data-stu-id="c43ea-155">See [Debug a pipeline with custom activity](data-factory-use-custom-activities.md#troubleshoot-failures) for detailed steps.</span></span>

## <a name="use-azure-portal-tootroubleshoot"></a><span data-ttu-id="c43ea-156">Az Azure portál tootroubleshoot használata</span><span class="sxs-lookup"><span data-stu-id="c43ea-156">Use Azure portal tootroubleshoot</span></span>
### <a name="using-portal-blades"></a><span data-ttu-id="c43ea-157">Portál paneleken használatával</span><span class="sxs-lookup"><span data-stu-id="c43ea-157">Using portal blades</span></span>
<span data-ttu-id="c43ea-158">Lásd: [figyelő folyamat](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) lépéseket.</span><span class="sxs-lookup"><span data-stu-id="c43ea-158">See [Monitor pipeline](data-factory-build-your-first-pipeline-using-editor.md#monitor-pipeline) for steps.</span></span>

### <a name="using-monitor-and-manage-app"></a><span data-ttu-id="c43ea-159">A Megfigyelés és kezelés alkalmazás használata</span><span class="sxs-lookup"><span data-stu-id="c43ea-159">Using Monitor and Manage App</span></span>
<span data-ttu-id="c43ea-160">Lásd: [figyelése és kezelése a data factory folyamatok figyelése és kezelése App](data-factory-monitor-manage-app.md) részleteket.</span><span class="sxs-lookup"><span data-stu-id="c43ea-160">See [Monitor and manage data factory pipelines using Monitor and Manage App](data-factory-monitor-manage-app.md) for details.</span></span>

## <a name="use-azure-powershell-tootroubleshoot"></a><span data-ttu-id="c43ea-161">Azure PowerShell tootroubleshoot használata</span><span class="sxs-lookup"><span data-stu-id="c43ea-161">Use Azure PowerShell tootroubleshoot</span></span>
### <a name="use-azure-powershell-tootroubleshoot-an-error"></a><span data-ttu-id="c43ea-162">Használja az Azure PowerShell tootroubleshoot hiba</span><span class="sxs-lookup"><span data-stu-id="c43ea-162">Use Azure PowerShell tootroubleshoot an error</span></span>
<span data-ttu-id="c43ea-163">Lásd: [képernyő adat-előállító folyamatok, az Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="c43ea-163">See [Monitor Data Factory pipelines using Azure PowerShell](data-factory-build-your-first-pipeline-using-powershell.md#monitor-pipeline) for details.</span></span>

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
