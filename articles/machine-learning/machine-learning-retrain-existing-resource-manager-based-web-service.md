---
title: "egy meglévő prediktív aaaRetrain webszolgáltatás |} Microsoft Docs"
description: "Ismerje meg, hogyan tooretrain modell és a frissítés hello web service toouse hello újonnan betanított modell az Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: 
ms.assetid: cc4c26a2-5672-4255-a767-cfd971e46775
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/11/2017
ms.author: v-donglo
ms.openlocfilehash: fb0760d0a2adc34fc5f3df1ae41bdac075f91bf4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-an-existing-predictive-web-service"></a>Meglévő prediktív webszolgáltatás újratanítása
Ez a dokumentum ismerteti a folyamatot a következő forgatókönyv hello átképezési hello:

* Rendelkezik egy tanítási kísérletet, és egy prediktív kísérletté központilag telepített operationalized webszolgáltatásként.
* Új adatokat, hogy a prediktív web service toouse tooperform a pontozási rendelkezik.

> [!NOTE] 
> toodeploy egy új webszolgáltatás-bővítmény, megfelelő engedélyekkel kell rendelkeznie a hello előfizetés toowhich meg hello webes szolgáltatás telepítéséhez. További információ: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md). 

A meglévő webszolgáltatás és kísérletek verziótól kezdődően kell toofollow ezeket a lépéseket:

1. Frissítési hello modell.
   1. A webszolgáltatás bemenetei és kimenetei a tanítási kísérletet tooallow módosítása.
   2. Hello tanítási kísérletet egy megőrzési webszolgáltatás-bővítmény telepítése.
   3. Hello tanítási kísérletet kötegelt végrehajtási szolgáltatás (BES) tooretrain hello modellt használja.
2. Hello Azure Machine Learning PowerShell parancsmagok tooupdate hello prediktív kísérletté használja.
   1. Jelentkezzen be Azure Resource Manager fiók tooyour.
   2. Hello webszolgáltatás-definíciójának beolvasása.
   3. Exportálja a hello webszolgáltatás-definíciójának JSON-ként.
   4. Frissítés hello hivatkozás toohello ilearner blob hello JSON.
   5. Hello JSON importálnia kell a webszolgáltatás-definíciójának.
   6. Hello webszolgáltatás frissítése egy új webszolgáltatás-definíciójának.

## <a name="deploy-hello-training-experiment"></a>Hello tanítási kísérletet telepítése
toodeploy hello tanítási kísérletet megőrzési webszolgáltatásként, hozzá kell adnia a webszolgáltatás bemenetei és kimenetei toohello szolgáltatásmodellt. Csatlakozzon egy *webes szolgáltatás kimeneti* modul toohello kísérlet  *[tanítási modell] [ train-model]*  hello kísérlet képzési modul, engedélyezése tooproduce új betanított modell, amely a prediktív kísérletté is használhatja. Ha rendelkezik egy *modell kiértékelése* modul, webes szolgáltatás kimeneti tooget hello kiértékelésének eredménye kimenetként is csatolható.

tooupdate a tanítási kísérletet:

1. Csatlakozás egy *Web Service bemeneti* bemeneti modul tooyour adatokat (például egy *Clean Missing Data* modul). Általában azt szeretné, hogy a bemeneti adatok feldolgozása a tooensure hello ugyanaz, mint az eredeti betanítási adatok.
2. Csatlakozás egy *webes szolgáltatás kimeneti* modul toohello kimenetét a *tanítási modell* modul.
3. Ha rendelkezik egy *modell kiértékelése* modul, és szeretné, hogy toooutput hello kiértékelésének eredménye, csatlakozzon a *webes szolgáltatás kimeneti* modul toohello kimenetét a *modell kiértékelése* a modul.

Futtassa a kísérletet.

A következő hello tanítási kísérletet kell telepítenie egy webszolgáltatás, amely létrehozza a modell betanítását és modell kiértékelésének eredménye.  

A kísérletvászonra hello hello alján kattintson **webes szolgáltatások beállítása**, majd válassza ki **[Új] webes szolgáltatás telepítése**. hello Azure Machine Learning webszolgáltatások portál megnyitja toohello **webes szolgáltatás telepítése** lap. Adjon meg egy nevet a webszolgáltatáshoz, fizetési csomag kiválasztása, és kattintson **telepítés**. Hello kötegelt végrehajtási metódus csak a betanított modellek létrehozásához használható.

## <a name="retrain-hello-model-with-new-data-by-using-bes"></a>Az új adatokat hello modell újratanítása BES használatával
Ebben a példában a C# toocreate hello alkalmazás átképezési használunk. Python vagy R minta kód tooaccomplish ezt a feladatot is használjon.

toocall hello átképezési API-kat:

1. Hozzon létre egy C# konzolalkalmazást a Visual Studio: **új** > **projekt** > **Visual C#** > **klasszikus Windows asztal** > **Konzolalkalmazás (.NET-keretrendszer)**.
2. Jelentkezzen be toohello a Machine Learning webszolgáltatások portálon.
3. Kattintson a hello webszolgáltatás, amelyet dolgozunk.
4. Kattintson a **felhasználásához**.
5. Hello hello alján **felhasználás** lap hello **mintakód** kattintson **kötegelt**.
6. A kötegelt végrehajtás hello C# mintakód másolja, majd illessze be hello Program.cs fájlra. Győződjön meg arról, hogy hello névtér nem módosulnak.

Adja hozzá a hello Microsoft.AspNet.WebApi.Client, NuGet-csomagot a hello megjegyzések. tooadd hello hivatkozás tooMicrosoft.WindowsAzure.Storage.dll, először meg kell tooinstall hello [ügyféloldali kódtára a Azure Storage szolgáltatás](https://www.nuget.org/packages/WindowsAzure.Storage).

hello alábbi képernyőfelvételen látható hello **felhasználás** hello Azure Machine Learning webszolgáltatások lapjára.

![Lap felhasználása][1]

### <a name="update-hello-apikey-declaration"></a>Hello apikey nyilatkozat frissítése
Keresse meg a hello **apikey** deklarációjában:

    const string apiKey = "abc123"; // Replace this with hello API key for hello web service

A hello **alapvető fogyasztási adatai** hello szakasza **felhasználás** lapon hello elsődleges kulcs keresse meg és másolja azt toohello **apikey** nyilatkozatot.

### <a name="update-hello-azure-storage-information"></a>Hello Azure Storage-adatainak módosítása
hello BES mintakód feltölt egy fájlt egy helyi meghajtó (például "C:\temp\CensusIpnput.csv") tooAzure tárolási, folyamatokat engedélyez, és írja a hello eredmények hátsó tooAzure tárolási.  

tooupdate hello Azure tárolással kapcsolatos, le kell kérni a tárfiók a klasszikus Azure portálon hello nevét, a kulcs és a tároló információkat, majd a frissítés hello correspondi a kísérlet futtatása után hello eredő hello tárfiók munkafolyamat hasonló toohello következő legyen:

![Eredményül kapott munkafolyamat futtatása után][4]hello kód ng értékek.

1. Jelentkezzen be toohello a klasszikus Azure portálon.
2. Kattintson a bal oldali oszlopban hello **tárolási**.
3. Hello listában tárfiókok jelölje be egy toostore hello retrained modell.
4. Hello a hello lap alján, kattintson **elérési kulcsok kezelése**.
5. Másolja ki és mentse a hello **elsődleges elérési kulcsot** és Bezárás hello párbeszédpanel.
6. Hello hello oldal tetején, kattintson a **tárolók**.
7. Egy meglévő tárolóhoz, válassza ki vagy hozzon létre egy újat, és mentse hello nevét.

Keresse meg a hello *StorageAccountName*, *StorageAccountKey*, és *StorageContainerName* nyilatkozatok, és a frissítés hello értékek mentett hello klasszikus portálon .

    const string StorageAccountName = "mystorageacct"; // Replace this with your Azure storage account name
    const string StorageAccountKey = "a_storage_account_key"; // Replace this with your Azure Storage key
    const string StorageContainerName = "mycontainer"; // Replace this with your Azure Storage container name

Is gondoskodnia kell arról, hogy hello bemeneti fájl áll rendelkezésre a hello kódban megadott hello helyen.

### <a name="specify-hello-output-location"></a>Hello kimeneti helyének megadása
A kérelem hasznos hello hello kimeneti hely megadása esetén a megadott hello fájl kiterjesztése hello *RelativeLocation* kell megadni, `ilearner`. Tekintse meg a következő példa hello:

    Outputs = new Dictionary<string, AzureBlobDataReference>() {
        {
            "output1",
            new AzureBlobDataReference()
            {
                ConnectionString = storageConnectionString,
                RelativeLocation = string.Format("{0}/output1results.ilearner", StorageContainerName) /*Replace this with hello location you want toouse for your output file and a valid file extension (usually .csv for scoring results or .ilearner for trained models)*/
            }
        },

hello az alábbiakban látható egy példa kimenet átképezési: ![kimeneti Átképezési][6]

## <a name="evaluate-hello-retraining-results"></a>Eredmények átképezési hello kiértékelése
Hello alkalmazás futtatásakor hello kimenete hello URL-cím és a megosztott hozzáférési aláírásokkal jogkivonat, amelyek a szükséges tooaccess hello kiértékelésének eredménye.

Megtekintheti a hello teljesítménymérési eredményeket hello retrained modell hello kombinálásával *BaseLocation*, *RelativeLocation*, és *SasBlobToken* hello kimeneti eredmények a *output2* (ahogy az az előző kimeneti kép átképezési hello szerepel), és be hello teljes URL-CÍMÉT a böngésző címsorában hello.  

Hello eredmények toodetermine akkor megvizsgálja, hogy hello újonnan betanított modell is elegendő egy meglévő tooreplace hello hajt végre.

Másolás hello *BaseLocation*, *RelativeLocation*, és *SasBlobToken* hello kimeneti eredmények közül.

## <a name="retrain-hello-web-service"></a>Hello webszolgáltatás újratanítása
Ha újratanítása egy új webszolgáltatás-bővítmény, hello prediktív webes szolgáltatás definíciós tooreference hello új betanított modell frissítenie. hello webszolgáltatás-definíciójának hello betanított modell hello webszolgáltatás belső másolatát, és nem módosítható közvetlenül. Győződjön meg arról, hogy vannak-e hello webszolgáltatás-definíciójának beolvasása a prediktív kísérletté és nem a tanítási kísérletet.

## <a name="sign-in-tooazure-resource-manager"></a>Jelentkezzen be tooAzure erőforrás-kezelő
Akkor először be kell jelentkeznie az Azure-hello PowerShell környezetben a fiók tooyour hello segítségével [Add-AzureRmAccount](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag.

## <a name="get-hello-web-service-definition-object"></a>Hello webszolgáltatás-definíciójának objektum
Következő lépésként hozza hello webszolgáltatás-definíciójának objektum hívó hello [Get-AzureRmMlWebService](https://msdn.microsoft.com/library/mt619267.aspx) parancsmag.

    $wsd = Get-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

toodetermine hello erőforráscsoport neve egy meglévő webszolgáltatás hello Get-AzureRmMlWebService parancsmag nélkül paraméterek toodisplay hello webes szolgáltatások futtatása az előfizetésben. Keresse meg a hello webes szolgáltatás, és tekintse meg a webes szolgáltatás azonosítóját. hello erőforráscsoport hello neve nem a hello azonosítója hello eleme után hello *resourceGroups* elemet. A következő példa hello hello erőforráscsoport neve alapértelmezett-MachineLearning-SouthCentralUS.

    Properties : Microsoft.Azure.Management.MachineLearning.WebServices.Models.WebServicePropertiesForGraph
    Id : /subscriptions/<subscription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237
    Name : RetrainSamplePre.2016.8.17.0.3.51.237
    Location : South Central US
    Type : Microsoft.MachineLearning/webServices
    Tags : {}

Másik lehetőségként toodetermine hello erőforráscsoport-név egy létező webszolgáltatás, jelentkezzen be Azure Machine Learning webszolgáltatások portal toohello. Válassza ki a hello webszolgáltatáshoz. hello erőforráscsoport neve hello webszolgáltatás, hello URL-CÍMÉT hello ötödik eleme után hello *resourceGroups* elemet. A következő példa hello hello erőforráscsoport neve alapértelmezett-MachineLearning-SouthCentralUS.

    https://services.azureml.net/subscriptions/<subcription ID>/resourceGroups/Default-MachineLearning-SouthCentralUS/providers/Microsoft.MachineLearning/webServices/RetrainSamplePre.2016.8.17.0.3.51.237


## <a name="export-hello-web-service-definition-object-as-json"></a>JSON-ként hello webszolgáltatás-definíciójának objektum exportálása
hello betanított modell toouse hello toomodify hello definíciója újonnan modell betanítása, előbb használnia kell hello [Export-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767935.aspx) parancsmag tooexport azt tooa JSON-formátumú fájlt.

    Export-AzureRmMlWebService -WebService $wsd -OutputFile "C:\temp\mlservice_export.json"

## <a name="update-hello-reference-toohello-ilearner-blob"></a>Hello hivatkozás toohello ilearner blob frissítése
Az eszközök, a hello keresse meg a hello [betanított modell], frissítés hello *uri* hello érték *locationInfo* hello hello ilearner BLOB URI-csomópont. hello URI létrejön hello kombinálásával *BaseLocation* és hello *RelativeLocation* a BES megőrzési hívás hello hello kimenetét.

     "asset3": {
        "name": "Retrain Sample [trained model]",
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

## <a name="import-hello-json-into-a-web-service-definition-object"></a>A webszolgáltatás-definíciójának objektum hello JSON importálása
Hello kell használnia [Import-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767925.aspx) parancsmag tooconvert hello JSON-fájl módosító vissza objektummá egy webszolgáltatás-definíciójának használható tooupdate hello predicative kísérlet.

    $wsd = Import-AzureRmMlWebService -InputFile "C:\temp\mlservice_export.json"


## <a name="update-hello-web-service"></a>Hello webes szolgáltatás frissítése
Végül, használja a hello [frissítés-AzureRmMlWebService](https://msdn.microsoft.com/library/azure/mt767922.aspx) parancsmag tooupdate hello prediktív kísérletté.

    Update-AzureRmMlWebService -Name 'RetrainSamplePre.2016.8.17.0.3.51.237' -ResourceGroupName 'Default-MachineLearning-SouthCentralUS'

[1]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-consume-page.png
[4]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE04.png
[6]: ./media/machine-learning-retrain-existing-arm-web-service/machine-learning-retrain-models-programmatically-IMAGE06.png

<!-- Module References -->
[train-model]: https://msdn.microsoft.com/library/azure/5cc7053e-aa30-450d-96c0-dae4be720977/
