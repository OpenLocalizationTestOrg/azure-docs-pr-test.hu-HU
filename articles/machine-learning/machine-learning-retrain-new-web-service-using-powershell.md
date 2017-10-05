---
title: "A PowerShell-lel egy új Azure Machine Learning webszolgáltatás újratanítása |} Microsoft Docs"
description: "Megtudhatja, hogyan programozott módon modell működik, és frissíti a webszolgáltatás az újonnan betanított modell használatára az Azure Machine Learning a Machine Learning Management PowerShell-parancsmagok használatával."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondlaghaeian
editor: 
ms.assetid: 3953a398-6174-4d2d-8bbd-e55cf1639415
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/28/2017
ms.author: v-donglo
ms.openlocfilehash: 804dd59e62f38ee1878045d93211ee18e0d5bfce
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="retrain-a-new-resource-manager-based-web-service-using-the-machine-learning-management-powershell-cmdlets"></a><span data-ttu-id="dd668-103">A Machine Learning Management PowerShell-parancsmagok használatával egy új erőforrás-kezelő alapú webszolgáltatás újratanítása</span><span class="sxs-lookup"><span data-stu-id="dd668-103">Retrain a New Resource Manager based web service using the Machine Learning Management PowerShell cmdlets</span></span>
<span data-ttu-id="dd668-104">Ha újratanítása egy új webszolgáltatás-bővítmény, frissítenie kell hivatkoznia, az új betanított modell a prediktív webszolgáltatás-definíciójának.</span><span class="sxs-lookup"><span data-stu-id="dd668-104">When you retrain a New web service, you update the predictive web service definition to reference the new trained model.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="dd668-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="dd668-105">Prerequisites</span></span>
<span data-ttu-id="dd668-106">Be kell állítania egy tanítási kísérletet, és egy prediktív kísérletté látható módon [Machine Learning-modellek szoftveres](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="dd668-106">You must set up a training experiment and a predictive experiment as shown in [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="dd668-107">A prediktív kísérletté tanulás webszolgáltatás Azure Resource Manager (új) alapú gépként kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="dd668-107">The predictive experiment must be deployed as an Azure Resource Manager (New) based machine learning web service.</span></span> <span data-ttu-id="dd668-108">Egy új webszolgáltatás-bővítmény telepítése, megfelelő engedélyekkel kell rendelkeznie, amelyhez az előfizetést, a webszolgáltatás telepítése.</span><span class="sxs-lookup"><span data-stu-id="dd668-108">To deploy a New web service you must have sufficient permissions in the subscription to which you deploying the web service.</span></span> <span data-ttu-id="dd668-109">További információkért lásd: [kezelése az Azure Machine Learning webszolgáltatások portál használatával egy webszolgáltatás-bővítmény](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="dd668-109">For more information, see [Manage a Web service using the Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="dd668-110">A web Services további információkért lásd: [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="dd668-110">For additional information on Deploying web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="dd668-111">Ehhez a folyamathoz szükséges, hogy telepítette az Azure Machine Learning parancsmagok.</span><span class="sxs-lookup"><span data-stu-id="dd668-111">This process requires that you have installed the Azure Machine Learning Cmdlets.</span></span> <span data-ttu-id="dd668-112">A Machine Learning-parancsmagok telepítése információkért lásd: a [Azure Machine Learning parancsmagok](https://msdn.microsoft.com/library/azure/mt767952.aspx) hivatkozást az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="dd668-112">For information installing the Machine Learning cmdlets, see the [Azure Machine Learning Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) reference on MSDN.</span></span>

<span data-ttu-id="dd668-113">A következő adatokat másolni a megőrzési kimenete:</span><span class="sxs-lookup"><span data-stu-id="dd668-113">Copied the following information from the retraining output:</span></span>

* <span data-ttu-id="dd668-114">BaseLocation</span><span class="sxs-lookup"><span data-stu-id="dd668-114">BaseLocation</span></span>
* <span data-ttu-id="dd668-115">RelativeLocation</span><span class="sxs-lookup"><span data-stu-id="dd668-115">RelativeLocation</span></span>

<span data-ttu-id="dd668-116">Lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="dd668-116">The steps you take are:</span></span>

1. <span data-ttu-id="dd668-117">Jelentkezzen be Azure Resource Manager-fiókját.</span><span class="sxs-lookup"><span data-stu-id="dd668-117">Sign in to your Azure Resource Manager account.</span></span>
2. <span data-ttu-id="dd668-118">A webszolgáltatás-definíciójának beolvasása</span><span class="sxs-lookup"><span data-stu-id="dd668-118">Get the web service definition</span></span>
3. <span data-ttu-id="dd668-119">A webszolgáltatás-definíciójának JSON exportálása</span><span class="sxs-lookup"><span data-stu-id="dd668-119">Export the Web Service Definition as JSON</span></span>
4. <span data-ttu-id="dd668-120">Frissítse a JSON-ban a ilearner blobra mutató hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="dd668-120">Update the reference to the ilearner blob in the JSON.</span></span>
5. <span data-ttu-id="dd668-121">A JSON importálnia kell a webszolgáltatás-definíciójának</span><span class="sxs-lookup"><span data-stu-id="dd668-121">Import the JSON into a Web Service Definition</span></span>
6. <span data-ttu-id="dd668-122">A webszolgáltatás új webszolgáltatás-definíciójának frissítése</span><span class="sxs-lookup"><span data-stu-id="dd668-122">Update the web service with new Web Service Definition</span></span>

## <a name="sign-in-to-your-azure-resource-manager-account"></a><span data-ttu-id="dd668-123">Jelentkezzen be az Azure Resource Manager-fiókba</span><span class="sxs-lookup"><span data-stu-id="dd668-123">Sign in to your Azure Resource Manager account</span></span>
<span data-ttu-id="dd668-124">Ön először be kell jelentkeznie Azure-fiókjába a belül a PowerShell környezet használatával a [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="dd668-124">You must first sign in to your Azure account from within the PowerShell environment using the [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span>

## <a name="get-the-web-service-definition"></a><span data-ttu-id="dd668-125">A webszolgáltatás-definíciójának beolvasása</span><span class="sxs-lookup"><span data-stu-id="dd668-125">Get the Web Service Definition</span></span>
<span data-ttu-id="dd668-126">A következő beolvasni a webszolgáltatás meghívásával a [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="dd668-126">Next, get the Web Service by calling the [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span> <span data-ttu-id="dd668-127">A webszolgáltatás-definíciójának a betanított modell webszolgáltatás belső másolatát, és nincs közvetlen módosítható.</span><span class="sxs-lookup"><span data-stu-id="dd668-127">The Web Service Definition is an internal representation of the trained model of the web service and is not directly modifiable.</span></span> <span data-ttu-id="dd668-128">Győződjön meg arról, hogy vannak-e a webszolgáltatás-definíciójának beolvasása a prediktív kísérletté és nem a tanítási kísérletet.</span><span class="sxs-lookup"><span data-stu-id="dd668-128">Make sure that you are retrieving the Web Service Definition for your Predictive experiment and not your training experiment.</span></span>

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

<span data-ttu-id="dd668-129">Határozza meg az erőforráscsoport neve egy meglévő webszolgáltatás, futtassa a Get-AzureRmMlWebService parancsmagot a webes szolgáltatások megjelennek az előfizetés paraméter nélkül.</span><span class="sxs-lookup"><span data-stu-id="dd668-129">To determine the resource group name of an existing web service, run the Get-AzureRmMlWebService cmdlet without any parameters to display the web services in your subscription.</span></span> <span data-ttu-id="dd668-130">Keresse meg a webes szolgáltatás, és tekintse meg a webes szolgáltatás azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="dd668-130">Locate the web service, and then look at its web service ID.</span></span> <span data-ttu-id="dd668-131">Az erőforráscsoport neve az azonosító, a negyedik eleme után a *resourceGroups* elemet.</span><span class="sxs-lookup"><span data-stu-id="dd668-131">The name of the resource group is the fourth element in the ID, just after the *resourceGroups* element.</span></span> <span data-ttu-id="dd668-132">A következő példában az erőforráscsoport neve alapértelmezett-MachineLearning-SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="dd668-132">In the following example, the resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

<span data-ttu-id="dd668-133">Azt is megteheti területen megállapíthatja, hogy az erőforráscsoport neve egy meglévő webszolgáltatás jelentkezzen be a Microsoft Azure Machine Learning webszolgáltatások portálra.</span><span class="sxs-lookup"><span data-stu-id="dd668-133">Alternatively, to determine the resource group name of an existing web service, log on to the Microsoft Azure Machine Learning Web Services portal.</span></span> <span data-ttu-id="dd668-134">Válassza ki a webszolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="dd668-134">Select the web service.</span></span> <span data-ttu-id="dd668-135">Az erőforráscsoport neve a webszolgáltatás URL-CÍMÉT a ötödik eleme után a *resourceGroups* elemet.</span><span class="sxs-lookup"><span data-stu-id="dd668-135">The resource group name is the fifth element of the URL of the web service, just after the *resourceGroups* element.</span></span> <span data-ttu-id="dd668-136">A következő példában az erőforráscsoport neve alapértelmezett-MachineLearning-SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="dd668-136">In the following example, the resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-the-web-service-definition-as-json"></a><span data-ttu-id="dd668-137">A webszolgáltatás-definíciójának JSON exportálása</span><span class="sxs-lookup"><span data-stu-id="dd668-137">Export the Web Service Definition as JSON</span></span>
<span data-ttu-id="dd668-138">Módosítani a definíciót a betanított modell használatára az újonnan betanított modell, előbb használnia kell a [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) parancsmag használatával exportálja a JSON formátumú fájlba.</span><span class="sxs-lookup"><span data-stu-id="dd668-138">To modify the definition to the trained model to use the newly Trained Model, you must first use the [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet to export it to a JSON format file.</span></span>

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-the-reference-to-the-ilearner-blob-in-the-json"></a><span data-ttu-id="dd668-139">Frissítse a JSON-ban a ilearner blobra mutató hivatkozás.</span><span class="sxs-lookup"><span data-stu-id="dd668-139">Update the reference to the ilearner blob in the JSON.</span></span>
<span data-ttu-id="dd668-140">Az eszközök, keresse meg a [betanított modell], frissítse a *uri* értéket a *locationInfo* ilearner BLOB URI-azonosítójú csomópont.</span><span class="sxs-lookup"><span data-stu-id="dd668-140">In the assets, locate the [trained model], update the *uri* value in the *locationInfo* node with the URI of the ilearner blob.</span></span> <span data-ttu-id="dd668-141">Az URI egyesítésével létrejön a *BaseLocation* és a *RelativeLocation* a BES átképezési hívás a kimenetből.</span><span class="sxs-lookup"><span data-stu-id="dd668-141">The URI is generated by combining the *BaseLocation* and the *RelativeLocation* from the output of the BES retraining call.</span></span> <span data-ttu-id="dd668-142">Ekkor frissül az elérési út az új betanított modell.</span><span class="sxs-lookup"><span data-stu-id="dd668-142">This updates the path to reference the new trained model.</span></span>

     "asset3": {
        "name": "Retrain Samp.le [trained model]",
        "type": "Resource",
        "locationInfo": {
          "uri": "https://mltestaccount.blob.core.windows.net/azuremlassetscontainer/baca7bca650f46218633552c0bcbba0e.ilearner"
        },
        "outputPorts": {
          "Results dataset": {
            "type": "Dataset"
          }
        }
      },

## <a name="import-the-json-into-a-web-service-definition"></a><span data-ttu-id="dd668-143">A JSON importálnia kell a webszolgáltatás-definíciójának</span><span class="sxs-lookup"><span data-stu-id="dd668-143">Import the JSON into a Web Service Definition</span></span>
<span data-ttu-id="dd668-144">Kell használnia a [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) parancsmagot, hogy a módosított JSON-fájl átalakítása vissza egy webszolgáltatás-definíciójának frissítése a webszolgáltatás-definíciójának használható.</span><span class="sxs-lookup"><span data-stu-id="dd668-144">You must use the [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet to convert the modified JSON file back into a Web Service Definition that you can use to update the Web Service Definition.</span></span>

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-the-web-service-with-new-web-service-definition"></a><span data-ttu-id="dd668-145">A webszolgáltatás új webszolgáltatás-definíciójának frissítése</span><span class="sxs-lookup"><span data-stu-id="dd668-145">Update the web service with new Web Service Definition</span></span>
<span data-ttu-id="dd668-146">Végezetül használhatja [frissítés-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) parancsmagot, hogy a webszolgáltatás-definíciójának frissítése.</span><span class="sxs-lookup"><span data-stu-id="dd668-146">Finally, you use [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet to update the Web Service Definition.</span></span>

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a><span data-ttu-id="dd668-147">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="dd668-147">Summary</span></span>
<span data-ttu-id="dd668-148">A Machine Learning PowerShell parancsmagokat használva frissítheti a betanított modell egy prediktív webszolgáltatás forgatókönyvek például engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="dd668-148">Using the Machine Learning PowerShell management cmdlets, you can update the trained model of a predictive Web Service enabling scenarios such as:</span></span>

* <span data-ttu-id="dd668-149">Az új adatokat átképezési rendszeres modell.</span><span class="sxs-lookup"><span data-stu-id="dd668-149">Periodic model retraining with new data.</span></span>
* <span data-ttu-id="dd668-150">Terjesztési ügyfelek-modell, azzal a céllal, hogy működik a modell használatával saját adataikat.</span><span class="sxs-lookup"><span data-stu-id="dd668-150">Distribution of a model to customers with the goal of letting them retrain the model using their own data.</span></span>

