---
title: "a PowerShell-lel egy új Azure Machine Learning webszolgáltatás aaaRetrain |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprogrammatically újratanítása egy modell és a frissítés hello web service toouse hello újonnan betanított modell az Azure Machine Learning hello Machine Learning Management PowerShell-parancsmagok használatával."
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
ms.openlocfilehash: 5b77fa82cfe17f0b4e90007ef81c506ab712475b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-a-new-resource-manager-based-web-service-using-hello-machine-learning-management-powershell-cmdlets"></a><span data-ttu-id="6a8ef-103">Egy új erőforrás-kezelő alapú webszolgáltatás hello Machine Learning Management PowerShell-parancsmagok használatával újratanítása</span><span class="sxs-lookup"><span data-stu-id="6a8ef-103">Retrain a New Resource Manager based web service using hello Machine Learning Management PowerShell cmdlets</span></span>
<span data-ttu-id="6a8ef-104">Ha újratanítása egy új webszolgáltatás-bővítmény, hello prediktív webes szolgáltatás definíciós tooreference hello új betanított modell frissítenie.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-104">When you retrain a New web service, you update hello predictive web service definition tooreference hello new trained model.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="6a8ef-105">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="6a8ef-105">Prerequisites</span></span>
<span data-ttu-id="6a8ef-106">Be kell állítania egy tanítási kísérletet, és egy prediktív kísérletté látható módon [Machine Learning-modellek szoftveres](machine-learning-retrain-models-programmatically.md).</span><span class="sxs-lookup"><span data-stu-id="6a8ef-106">You must set up a training experiment and a predictive experiment as shown in [Retrain Machine Learning models programmatically](machine-learning-retrain-models-programmatically.md).</span></span> 

> [!IMPORTANT]
> <span data-ttu-id="6a8ef-107">hello prediktív kísérletté tanulás webszolgáltatás Azure Resource Manager (új) alapú gépként kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-107">hello predictive experiment must be deployed as an Azure Resource Manager (New) based machine learning web service.</span></span> <span data-ttu-id="6a8ef-108">toodeploy egy új webszolgáltatás-bővítmény, megfelelő engedélyekkel kell rendelkeznie a hello előfizetés toowhich meg hello webes szolgáltatás telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-108">toodeploy a New web service you must have sufficient permissions in hello subscription toowhich you deploying hello web service.</span></span> <span data-ttu-id="6a8ef-109">További információkért lásd: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md).</span><span class="sxs-lookup"><span data-stu-id="6a8ef-109">For more information, see [Manage a Web service using hello Azure Machine Learning Web Services portal](machine-learning-manage-new-webservice.md).</span></span> 

<span data-ttu-id="6a8ef-110">A web Services további információkért lásd: [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="6a8ef-110">For additional information on Deploying web services, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>

<span data-ttu-id="6a8ef-111">Ehhez a folyamathoz szükséges, hogy telepítette-e az Azure Machine Learning parancsmagok hello.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-111">This process requires that you have installed hello Azure Machine Learning Cmdlets.</span></span> <span data-ttu-id="6a8ef-112">Hello Machine Learning-parancsmagok telepítése információkért lásd: hello [Azure Machine Learning parancsmagok](https://msdn.microsoft.com/library/azure/mt767952.aspx) hivatkozást az MSDN Webhelyén.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-112">For information installing hello Machine Learning cmdlets, see hello [Azure Machine Learning Cmdlets](https://msdn.microsoft.com/library/azure/mt767952.aspx) reference on MSDN.</span></span>

<span data-ttu-id="6a8ef-113">Kimeneti átképezési hello a következő információ másolt hello:</span><span class="sxs-lookup"><span data-stu-id="6a8ef-113">Copied hello following information from hello retraining output:</span></span>

* <span data-ttu-id="6a8ef-114">BaseLocation</span><span class="sxs-lookup"><span data-stu-id="6a8ef-114">BaseLocation</span></span>
* <span data-ttu-id="6a8ef-115">RelativeLocation</span><span class="sxs-lookup"><span data-stu-id="6a8ef-115">RelativeLocation</span></span>

<span data-ttu-id="6a8ef-116">hello lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="6a8ef-116">hello steps you take are:</span></span>

1. <span data-ttu-id="6a8ef-117">Jelentkezzen be Azure Resource Manager fiók tooyour.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-117">Sign in tooyour Azure Resource Manager account.</span></span>
2. <span data-ttu-id="6a8ef-118">Hello webszolgáltatás-definíciójának beolvasása</span><span class="sxs-lookup"><span data-stu-id="6a8ef-118">Get hello web service definition</span></span>
3. <span data-ttu-id="6a8ef-119">Webszolgáltatás-definíciójának hello JSON exportálása</span><span class="sxs-lookup"><span data-stu-id="6a8ef-119">Export hello Web Service Definition as JSON</span></span>
4. <span data-ttu-id="6a8ef-120">Frissítés hello hivatkozás toohello ilearner blob hello JSON.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-120">Update hello reference toohello ilearner blob in hello JSON.</span></span>
5. <span data-ttu-id="6a8ef-121">A JSON-kódot egy webszolgáltatás-definíciójának importálása hello</span><span class="sxs-lookup"><span data-stu-id="6a8ef-121">Import hello JSON into a Web Service Definition</span></span>
6. <span data-ttu-id="6a8ef-122">Új webszolgáltatás-definíciójának hello webes szolgáltatás frissítése</span><span class="sxs-lookup"><span data-stu-id="6a8ef-122">Update hello web service with new Web Service Definition</span></span>

## <a name="sign-in-tooyour-azure-resource-manager-account"></a><span data-ttu-id="6a8ef-123">Jelentkezzen be Azure Resource Manager fiók tooyour</span><span class="sxs-lookup"><span data-stu-id="6a8ef-123">Sign in tooyour Azure Resource Manager account</span></span>
<span data-ttu-id="6a8ef-124">Be kell jelentkezni az Azure-hello PowerShell környezetben hello segítségével a fiók tooyour [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-124">You must first sign in tooyour Azure account from within hello PowerShell environment using hello [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span>

## <a name="get-hello-web-service-definition"></a><span data-ttu-id="6a8ef-125">Hello webszolgáltatás-definíciójának beolvasása</span><span class="sxs-lookup"><span data-stu-id="6a8ef-125">Get hello Web Service Definition</span></span>
<span data-ttu-id="6a8ef-126">A következő get hello webszolgáltatás hívó hello által [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-126">Next, get hello Web Service by calling hello [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) cmdlet.</span></span> <span data-ttu-id="6a8ef-127">hello webszolgáltatás-definíciójának hello betanított modell hello webszolgáltatás belső másolatát, és nem módosítható közvetlenül.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-127">hello Web Service Definition is an internal representation of hello trained model of hello web service and is not directly modifiable.</span></span> <span data-ttu-id="6a8ef-128">Győződjön meg arról, hogy hello webszolgáltatás-definíciójának keres a prediktív kísérletté és nem a tanítási kísérletet.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-128">Make sure that you are retrieving hello Web Service Definition for your Predictive experiment and not your training experiment.</span></span>

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

<span data-ttu-id="6a8ef-129">toodetermine hello erőforráscsoport neve egy meglévő webszolgáltatás hello Get-AzureRmMlWebService parancsmag nélkül paraméterek toodisplay hello webes szolgáltatások futtatása az előfizetésben.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-129">toodetermine hello resource group name of an existing web service, run hello Get-AzureRmMlWebService cmdlet without any parameters toodisplay hello web services in your subscription.</span></span> <span data-ttu-id="6a8ef-130">Keresse meg a hello webes szolgáltatás, és tekintse meg a webes szolgáltatás azonosítóját.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-130">Locate hello web service, and then look at its web service ID.</span></span> <span data-ttu-id="6a8ef-131">hello erőforráscsoport hello neve nem a hello azonosítója hello eleme után hello *resourceGroups* elemet.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-131">hello name of hello resource group is hello fourth element in hello ID, just after hello *resourceGroups* element.</span></span> <span data-ttu-id="6a8ef-132">A következő példa hello hello erőforráscsoport neve alapértelmezett-MachineLearning-SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-132">In hello following example, hello resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

<span data-ttu-id="6a8ef-133">Másik lehetőségként toodetermine hello erőforráscsoport neve egy meglévő webszolgáltatás, toohello Microsoft Azure Machine Learning webszolgáltatások portal bejelentkezés.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-133">Alternatively, toodetermine hello resource group name of an existing web service, log on toohello Microsoft Azure Machine Learning Web Services portal.</span></span> <span data-ttu-id="6a8ef-134">Válassza ki a hello webszolgáltatáshoz.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-134">Select hello web service.</span></span> <span data-ttu-id="6a8ef-135">hello erőforráscsoport neve hello webszolgáltatás, hello URL-CÍMÉT hello ötödik eleme után hello *resourceGroups* elemet.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-135">hello resource group name is hello fifth element of hello URL of hello web service, just after hello *resourceGroups* element.</span></span> <span data-ttu-id="6a8ef-136">A következő példa hello hello erőforráscsoport neve alapértelmezett-MachineLearning-SouthCentralUS.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-136">In hello following example, hello resource group name is Default-MachineLearning-SouthCentralUS.</span></span>

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-hello-web-service-definition-as-json"></a><span data-ttu-id="6a8ef-137">Webszolgáltatás-definíciójának hello JSON exportálása</span><span class="sxs-lookup"><span data-stu-id="6a8ef-137">Export hello Web Service Definition as JSON</span></span>
<span data-ttu-id="6a8ef-138">toomodify hello definition betanítása toohello modell toouse hello újonnan Trained Model, előbb használnia kell hello [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) parancsmag tooexport azt tooa JSON formátumú fájlba.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-138">toomodify hello definition toohello trained model toouse hello newly Trained Model, you must first use hello [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) cmdlet tooexport it tooa JSON format file.</span></span>

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-hello-reference-toohello-ilearner-blob-in-hello-json"></a><span data-ttu-id="6a8ef-139">Frissítés hello hivatkozás toohello ilearner blob hello JSON.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-139">Update hello reference toohello ilearner blob in hello JSON.</span></span>
<span data-ttu-id="6a8ef-140">Az eszközök, a hello keresse meg a hello [betanított modell], frissítés hello *uri* hello érték *locationInfo* hello hello ilearner BLOB URI-csomópont.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-140">In hello assets, locate hello [trained model], update hello *uri* value in hello *locationInfo* node with hello URI of hello ilearner blob.</span></span> <span data-ttu-id="6a8ef-141">hello URI létrejön hello kombinálásával *BaseLocation* és hello *RelativeLocation* a BES megőrzési hívás hello hello kimenetét.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-141">hello URI is generated by combining hello *BaseLocation* and hello *RelativeLocation* from hello output of hello BES retraining call.</span></span> <span data-ttu-id="6a8ef-142">Ekkor frissül hello elérési tooreference hello új betanított modell.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-142">This updates hello path tooreference hello new trained model.</span></span>

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

## <a name="import-hello-json-into-a-web-service-definition"></a><span data-ttu-id="6a8ef-143">A JSON-kódot egy webszolgáltatás-definíciójának importálása hello</span><span class="sxs-lookup"><span data-stu-id="6a8ef-143">Import hello JSON into a Web Service Definition</span></span>
<span data-ttu-id="6a8ef-144">Hello kell használnia [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) parancsmag tooconvert hello módosított JSON-fájlt egy webszolgáltatás-definíciójának használható tooupdate hello webszolgáltatás-definíciójának programba.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-144">You must use hello [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) cmdlet tooconvert hello modified JSON file back into a Web Service Definition that you can use tooupdate hello Web Service Definition.</span></span>

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-hello-web-service-with-new-web-service-definition"></a><span data-ttu-id="6a8ef-145">Új webszolgáltatás-definíciójának hello webes szolgáltatás frissítése</span><span class="sxs-lookup"><span data-stu-id="6a8ef-145">Update hello web service with new Web Service Definition</span></span>
<span data-ttu-id="6a8ef-146">Végezetül használhatja [frissítés-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) parancsmag tooupdate hello webszolgáltatás-definíciójának.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-146">Finally, you use [Update-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) cmdlet tooupdate hello Web Service Definition.</span></span>

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a><span data-ttu-id="6a8ef-147">Összefoglalás</span><span class="sxs-lookup"><span data-stu-id="6a8ef-147">Summary</span></span>
<span data-ttu-id="6a8ef-148">PowerShell-parancsmagokkal hello Machine Learning felügyeleti, frissítheti az hello betanított modell egy prediktív webszolgáltatás forgatókönyvek például engedélyezése:</span><span class="sxs-lookup"><span data-stu-id="6a8ef-148">Using hello Machine Learning PowerShell management cmdlets, you can update hello trained model of a predictive Web Service enabling scenarios such as:</span></span>

* <span data-ttu-id="6a8ef-149">Az új adatokat átképezési rendszeres modell.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-149">Periodic model retraining with new data.</span></span>
* <span data-ttu-id="6a8ef-150">A modell toocustomers terjesztési azzal hello céllal, hogy újratanítása hello modell használatával saját adataikat.</span><span class="sxs-lookup"><span data-stu-id="6a8ef-150">Distribution of a model toocustomers with hello goal of letting them retrain hello model using their own data.</span></span>

