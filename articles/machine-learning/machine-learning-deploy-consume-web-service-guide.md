---
title: "Az Azure Machine Learning webszolgáltatások: Telepítés és használat |} Microsoft Docs"
description: "Erőforrások üzembe helyezéséhez és webszolgáltatások felhasználása."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: 47635376-d1f4-4ea4-a6af-bd1f99f69a69
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 539c2abb053a0f981be0374defe45cf4d96b740b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-machine-learning-web-services-deployment-and-consumption"></a>Azure Machine Learning webszolgáltatások: telepítés és használat
Azure Machine Learning toodeploy gépi tanulásra munkafolyamatok és modellek webszolgáltatásként használhatja. Ezek a webszolgáltatások majd lehet használt toocall hello gépi tanulási modelljeit alkalmazásokból keresztül hello Internet toodo előrejelzéseket valós időben vagy kötegelt módban. Mivel hello webszolgáltatások RESTful, hívása azokat a különböző programozási nyelveket és platformok, például a .NET és a Java, és az alkalmazások, például az Excel.

hello következő szakaszokban hivatkozások toowalkthroughs, a kód és a dokumentáció toohelp első lépések megtételéhez.

## <a name="deploy-a-web-service"></a>Webszolgáltatás üzembe helyezése
### <a name="with-azure-machine-learning-studio"></a>Az Azure Machine Learning Studio
A Machine Learning Studio és hello Microsoft Azure Machine Learning webszolgáltatások portal segítségével telepítheti és kezelheti egy webszolgáltatás-bővítmény kód írása nélkül.

hello következő hivatkozásokra kattintva kapcsolatos általános tudnivalók toodeploy egy új webszolgáltatás-bővítmény:

* Az áttekintést arról, hogyan toodeploy egy új webes szolgáltatás, amely az Azure Resource Manageren alapul: [egy új webszolgáltatás-bővítmény telepítése](machine-learning-webservice-deploy-a-web-service.md).
* A forgatókönyv leírja, toodeploy egy webszolgáltatás-bővítmény, lásd: [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).
* Egy kapcsolatos teljes forgatókönyv toocreate és egy webszolgáltatás-bővítmény telepítése, lásd: [forgatókönyv 1. lépés: a Machine Learning-munkaterület létrehozása](machine-learning-walkthrough-1-create-ml-workspace.md).
* Adott, amely egy webszolgáltatás-bővítmény telepítése című részben talál példákat:

  * [Útmutató 5. lépés: Hello Azure Machine Learning webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md)
  * [Hogyan toodeploy egy webes szolgáltatás toomultiple régiók](machine-learning-how-to-deploy-to-multiple-regions.md)

### <a name="with-web-services-resource-provider-apis-azure-resource-manager-apis"></a>A web services erőforrás-szolgáltató API-k (Azure Resource Manager API-k)
hello Azure Machine Learning erőforrás-szolgáltató web Services lehetővé teszi, hogy üzembe helyezési és kezelési webes szolgáltatás REST API-hívásokkal. További részletekért lásd: a [Machine Learning Web Service (REST)](/rest/api/machinelearning/index) hivatkozás.

<!-- [Machine Learning Web Service (REST)](https://msdn.microsoft.com/library/azure/mt767538.aspx) reference. -->


### <a name="with-powershell-cmdlets"></a>A PowerShell-parancsmagokkal
Az Azure Machine Learning erőforrás-szolgáltató web Services lehetővé teszi, hogy üzembe helyezési és kezelési webszolgáltatások PowerShell-parancsmagok használatával.

toouse hello parancsmagok kell először bejelentkezik tooyour hello PowerShell környezetből Azure fiók hello segítségével [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag. Ha nincs tisztában a hogyan toocall PowerShell parancsokat, amelyek a erőforrás-kezelő alapja című [az Azure PowerShell használata Azure Resource Managerrel](../azure-resource-manager/powershell-azure-resource-manager.md#log-in-to-your-azure-account).

tooexport a prediktív kísérletezhet, használja a [a mintakód](https://github.com/ritwik20/AzureML-WebServices). Miután létrehozta a hello .exe fájl hello kódból, adhatja meg:

    C:\<folder>\GetWSD <experiment-url> <workspace-auth-token>

Egy webes szolgáltatás JSON-sablon hello alkalmazást futtató hoz létre. toouse hello sablon toodeploy egy webszolgáltatás, hozzá kell adnia a következő információ hello:

* A tárfiók neve vagy a kulcs

    Hello tárfiók neve vagy a kulcs letölthető vagy hello [Azure-portálon](https://portal.azure.com/) vagy hello [a klasszikus Azure portálon](http://manage.windowsazure.com/).
* Előfizetési csomag azonosítója

    Hello terv azonosítója letölthető hello [Azure Machine Learning webszolgáltatások](https://services.azureml.net) portál jelentkezik be, majd kattintson a csomag neve.

Adja hozzá toohello JSON-sablon hello gyermekeként *tulajdonságok* hello csomóponthoz azonos szinten, hello *MachineLearningWorkspace* csomópont.

Íme egy példa:

    "StorageAccount": {
            "name": "YourStorageAccountName",
            "key": "YourStorageAccountKey"
    },
    "CommitmentPlan": {
        "id": "subscriptions/YouSubscriptionID/resourceGroups/YourResourceGroupID/providers/Microsoft.MachineLearning/commitmentPlans/YourPlanName"
    }

Tekintse meg a következő cikkek hello és a mintakódot további részletek:

* [Az Azure Machine Learning parancsmagok](https://msdn.microsoft.com/library/azure/mt767952.aspx) hivatkozást az MSDN webhelyen
* A minta [forgatókönyv](https://github.com/raymondlaghaeian/azureml-webservices-arm-powershell/blob/master/sample-commands.txt) a Githubon

## <a name="consume-hello-web-services"></a>Hello webszolgáltatások felhasználása
### <a name="from-hello-azure-machine-learning-web-services-ui-testing"></a>A hello Azure Machine Learning Web Services felhasználói felületén (tesztelés)
A webszolgáltatás hello Azure Machine Learning webszolgáltatások portálról tesztelheti. Ez magában foglalja a tesztelés hello kérés-válasz szolgáltatás (RR-EKET), és hogyan kötegelt végrehajtási szolgáltatás (BES).

* [Új webszolgáltatás üzembe helyezése](machine-learning-webservice-deploy-a-web-service.md)
* [Az Azure Machine Learning webszolgáltatás telepítése](machine-learning-publish-a-machine-learning-web-service.md)
* [Útmutató 5. lépés: Hello Azure Machine Learning webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md)

### <a name="from-excel"></a>Az Excelből
Egy Excel-sablont, amely akkor hello webszolgáltatás tölthető le:

* [Az Azure Machine Learning webszolgáltatásba az Excelből felhasználása](machine-learning-consuming-from-excel.md)
* [Az Excel beépülő modul az Azure Machine Learning webszolgáltatások](machine-learning-excel-add-in-for-web-services.md)

### <a name="from-a-rest-based-client"></a>Egy REST-alapú ügyfél
Az Azure Machine Learning webszolgáltatások RESTful API-k. Használatba vehetné a különböző platformokon, például a .NET, Python, R, Java, stb. hello API- **felhasználás** lap a web Service hello [Microsoft Azure Machine Learning webszolgáltatások portal](https://services.azureml.net) minta rendelkezik Ismerkedés a kódot, amely segítségével. További információkért lásd: [hogyan tooconsume az Azure Machine Learning Web service](machine-learning-consume-web-services.md).
