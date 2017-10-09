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
# <a name="retrain-a-new-resource-manager-based-web-service-using-hello-machine-learning-management-powershell-cmdlets"></a>Egy új erőforrás-kezelő alapú webszolgáltatás hello Machine Learning Management PowerShell-parancsmagok használatával újratanítása
Ha újratanítása egy új webszolgáltatás-bővítmény, hello prediktív webes szolgáltatás definíciós tooreference hello új betanított modell frissítenie.  

## <a name="prerequisites"></a>Előfeltételek
Be kell állítania egy tanítási kísérletet, és egy prediktív kísérletté látható módon [Machine Learning-modellek szoftveres](machine-learning-retrain-models-programmatically.md). 

> [!IMPORTANT]
> hello prediktív kísérletté tanulás webszolgáltatás Azure Resource Manager (új) alapú gépként kell telepíteni. toodeploy egy új webszolgáltatás-bővítmény, megfelelő engedélyekkel kell rendelkeznie a hello előfizetés toowhich meg hello webes szolgáltatás telepítéséhez. További információkért lásd: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md). 

A web Services további információkért lásd: [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).

Ehhez a folyamathoz szükséges, hogy telepítette-e az Azure Machine Learning parancsmagok hello. Hello Machine Learning-parancsmagok telepítése információkért lásd: hello [Azure Machine Learning parancsmagok](https://msdn.microsoft.com/library/azure/mt767952.aspx) hivatkozást az MSDN Webhelyén.

Kimeneti átképezési hello a következő információ másolt hello:

* BaseLocation
* RelativeLocation

hello lépései a következők:

1. Jelentkezzen be Azure Resource Manager fiók tooyour.
2. Hello webszolgáltatás-definíciójának beolvasása
3. Webszolgáltatás-definíciójának hello JSON exportálása
4. Frissítés hello hivatkozás toohello ilearner blob hello JSON.
5. A JSON-kódot egy webszolgáltatás-definíciójának importálása hello
6. Új webszolgáltatás-definíciójának hello webes szolgáltatás frissítése

## <a name="sign-in-tooyour-azure-resource-manager-account"></a>Jelentkezzen be Azure Resource Manager fiók tooyour
Be kell jelentkezni az Azure-hello PowerShell környezetben hello segítségével a fiók tooyour [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag.

## <a name="get-hello-web-service-definition"></a>Hello webszolgáltatás-definíciójának beolvasása
A következő get hello webszolgáltatás hívó hello által [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag. hello webszolgáltatás-definíciójának hello betanított modell hello webszolgáltatás belső másolatát, és nem módosítható közvetlenül. Győződjön meg arról, hogy hello webszolgáltatás-definíciójának keres a prediktív kísérletté és nem a tanítási kísérletet.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

toodetermine hello erőforráscsoport neve egy meglévő webszolgáltatás hello Get-AzureRmMlWebService parancsmag nélkül paraméterek toodisplay hello webes szolgáltatások futtatása az előfizetésben. Keresse meg a hello webes szolgáltatás, és tekintse meg a webes szolgáltatás azonosítóját. hello erőforráscsoport hello neve nem a hello azonosítója hello eleme után hello *resourceGroups* elemet. A következő példa hello hello erőforráscsoport neve alapértelmezett-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Másik lehetőségként toodetermine hello erőforráscsoport neve egy meglévő webszolgáltatás, toohello Microsoft Azure Machine Learning webszolgáltatások portal bejelentkezés. Válassza ki a hello webszolgáltatáshoz. hello erőforráscsoport neve hello webszolgáltatás, hello URL-CÍMÉT hello ötödik eleme után hello *resourceGroups* elemet. A következő példa hello hello erőforráscsoport neve alapértelmezett-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-hello-web-service-definition-as-json"></a>Webszolgáltatás-definíciójának hello JSON exportálása
toomodify hello definition betanítása toohello modell toouse hello újonnan Trained Model, előbb használnia kell hello [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) parancsmag tooexport azt tooa JSON formátumú fájlba.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-hello-reference-toohello-ilearner-blob-in-hello-json"></a>Frissítés hello hivatkozás toohello ilearner blob hello JSON.
Az eszközök, a hello keresse meg a hello [betanított modell], frissítés hello *uri* hello érték *locationInfo* hello hello ilearner BLOB URI-csomópont. hello URI létrejön hello kombinálásával *BaseLocation* és hello *RelativeLocation* a BES megőrzési hívás hello hello kimenetét. Ekkor frissül hello elérési tooreference hello új betanított modell.

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

## <a name="import-hello-json-into-a-web-service-definition"></a>A JSON-kódot egy webszolgáltatás-definíciójának importálása hello
Hello kell használnia [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) parancsmag tooconvert hello módosított JSON-fájlt egy webszolgáltatás-definíciójának használható tooupdate hello webszolgáltatás-definíciójának programba.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-hello-web-service-with-new-web-service-definition"></a>Új webszolgáltatás-definíciójának hello webes szolgáltatás frissítése
Végezetül használhatja [frissítés-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) parancsmag tooupdate hello webszolgáltatás-definíciójának.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'  -ServiceUpdates $wsd

## <a name="summary"></a>Összefoglalás
PowerShell-parancsmagokkal hello Machine Learning felügyeleti, frissítheti az hello betanított modell egy prediktív webszolgáltatás forgatókönyvek például engedélyezése:

* Az új adatokat átképezési rendszeres modell.
* A modell toocustomers terjesztési azzal hello céllal, hogy újratanítása hello modell használatával saját adataikat.

