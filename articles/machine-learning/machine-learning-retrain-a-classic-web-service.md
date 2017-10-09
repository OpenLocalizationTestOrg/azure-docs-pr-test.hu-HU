---
title: "a klasszikus webszolgáltatás aaaRetrain |} Microsoft Docs"
description: "Ismerje meg, hogyan tooprogrammatically újratanítása a modell és a frissítés hello webes toouse hello újonnan betanított modell az Azure Machine Learning."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondlaghaeian
editor: 
ms.assetid: e36e1961-9e8b-4801-80ef-46d80b140452
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: d3ba21ed75f02868535cb2fcac607643303a9554
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="retrain-a-classic-web-service"></a>Klasszikus webszolgáltatás újratanítása
hello prediktív webszolgáltatás üzembe helyezett scoring-végpontja hello alapértelmezett beállítás. Alapértelmezett végpontok tartják hello szinkronban eredeti képzési és kísérletek pontozási, és ezért hello betanított modell hello alapértelmezett végpont nem cserélhető le. tooretrain hello webszolgáltatás, hozzá kell adnia egy új végpont toohello webszolgáltatás-bővítmény. 

## <a name="prerequisites"></a>Előfeltételek
Akkor be kell állítania egy tanítási kísérletet, és egy prediktív kísérletté látható módon [Machine Learning-modellek szoftveres](machine-learning-retrain-models-programmatically.md). 

> [!IMPORTANT]
> hello prediktív kísérletté kell telepíteni, mint a klasszikus machine learning-webszolgáltatás. 
> 
> 

A web Services további információkért lásd: [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).

## <a name="add-a-new-endpoint"></a>Új végpont hozzáadása
hello prediktív webszolgáltatás központilag telepített scoring-végpontja, hogy szinkronban hello eredeti képzési és pontozási kísérletek betanított modell alapértelmezett tartalmazza. tooupdate a webes szolgáltatás toowith új modell betanítását, létre kell hoznia egy új pontozási végpontot. 

új pontozási végpont, a betanított modell hello frissíthető prediktív webszolgáltatásból hello toocreate:

> [!NOTE]
> Győződjön meg arról, hogy hello végpont toohello prediktív webes szolgáltatás, a képzési webszolgáltatás hello nem ad hozzá. Ha megfelelően telepítette, a képzési és a prediktív webszolgáltatás, megtekintheti az felsorolt két külön webszolgáltatások. "[prediktív exp.]" Hello prediktív webszolgáltatás kell végződnie.
> 
> 

Háromféleképpen, amelyben hozzáadhat egy új végpont tooa webszolgáltatás:

1. Automatizáltan
2. Hello a Microsoft Azure Web Services portálon
3. A klasszikus Azure portálon hello használata

### <a name="programmatically-add-an-endpoint"></a>Programozott módon a végpont hozzáadása
Pontozó végpontjaitól jelen hello mintakód is hozzáadhat [github-tárházban](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs).

### <a name="use-hello-microsoft-azure-web-services-portal-tooadd-an-endpoint"></a>Hello Microsoft Azure Web Services portál tooadd végpont használata
1. A Machine Learning Studio hello bal oldali oszlopban kattintson a webszolgáltatásokat.
2. Hello web service irányítópult hello alul kattintson **kezelése végpontok előzetes**.
3. Kattintson az **Add** (Hozzáadás) parancsra.
4. Írja be nevét és leírását hello új végpont. Válassza ki a hello naplózási szint, és hogy engedélyezve van-e mintaadatokat. További információt a naplózást, [naplózását a Machine Learning webszolgáltatások](machine-learning-web-services-logging.md).

### <a name="use-hello-azure-classic-portal-tooadd-an-endpoint"></a>Az Azure klasszikus portál tooadd végpont hello használata
1. Jelentkezzen be toohello [klasszikus Azure portálon](https://manage.windowsazure.com).
2. Hello bal oldali menüben kattintson a **Machine Learning**.
3. A név, kattintson a munkaterület majd **webszolgáltatások**.
4. A név, kattintson a **nyilvántartásba modell [prediktív exp].** .
5. Hello a hello lap alján, kattintson **végpont hozzáadása**. Végpontok hozzáadásával kapcsolatos további információkért lásd: [végpontok létrehozása](machine-learning-create-endpoint.md). 

## <a name="update-hello-added-endpoints-trained-model"></a>Frissítés hello hozzáadott végpont betanított modell
toocomplete hello megőrzési folyamat, frissítenie kell a betanított modell hello hello új végpont hozzáadott.

* Ha új végpont hello hello a klasszikus Azure portál használatával adott hozzá, továbbra is hello portal hello új végpont nevére kattint, majd hello **UpdateResource** tooget hello URL-cím tooupdate hello végpont modell kell kapcsolni.
* Hello végpont hello mintakód használatával adott hozzá, ha ez magában foglalja a hely hello súgó URL-címének hello által azonosított *HelpLocationURL* hello kimeneti értéket.

tooretrieve hello elérési URL-címe:

1. Másolással illessze be a hello URL-CÍMÉT a böngésző címsorába.
2. Hello frissítés erőforrás hivatkozásra.
3. Másolja a hello hello a JAVÍTÁSI kérelemben POST URL-CÍMÉT. Példa:
   
     JAVÍTÁS URL-CÍM: HTTPS://MANAGEMENT.AZUREML.NET/WORKSPACES/00BF70534500B34REBFA1843D6/WEBSERVICES/AF3ER32AD393852F9B30AC9A35B/ENDPOINTS/NEWENDPOINT2

Most már használhatja a hello betanított modell tooupdate hello scoring-végpontja, amelyet korábban hozott létre.

hello a következő mintakód bemutatja, hogyan toouse hello *BaseLocation*, *RelativeLocation*, *SasBlobToken*, és a javítás URL-cím tooupdate hello végpont.

    private async Task OverwriteModel()
    {
        var resourceLocations = new
        {
            Resources = new[]
            {
                new
                {
                    Name = "Census Model [trained model]",
                    Location = new AzureBlobDataReference()
                    {
                        BaseLocation = "https://esintussouthsus.blob.core.windows.net/",
                        RelativeLocation = "your endpoint relative location", //from hello output, for example: “experimentoutput/8946abfd-79d6-4438-89a9-3e5d109183/8946abfd-79d6-4438-89a9-3e5d109183.ilearner”
                        SasBlobToken = "your endpoint SAS blob token" //from hello output, for example: “?sv=2013-08-15&sr=c&sig=37lTTfngRwxCcf94%3D&st=2015-01-30T22%3A53%3A06Z&se=2015-01-31T22%3A58%3A06Z&sp=rl”
                    }
                }
            }
        };

        using (var client = new HttpClient())
        {
            client.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Bearer", apiKey);

            using (var request = new HttpRequestMessage(new HttpMethod("PATCH"), endpointUrl))
            {
                request.Content = new StringContent(JsonConvert.SerializeObject(resourceLocations), System.Text.Encoding.UTF8, "application/json");
                HttpResponseMessage response = await client.SendAsync(request);

                if (!response.IsSuccessStatusCode)
                {
                    await WriteFailedResponse(response);
                }

                // Do what you want with a successful response here.
            }
        }
    }

Hello *apiKey* és hello *végponti URL-cím* a hello hívás végpont irányítópult lehet lekérni.

hello értékének hello *neve* paraméterének *erőforrások* kell hello erőforrás neve egyezik hello mentett Trained Model hello prediktív kísérletté. tooget hello erőforrás neve:

1. Jelentkezzen be toohello [klasszikus Azure portálon](https://manage.windowsazure.com).
2. Hello bal oldali menüben kattintson a **Machine Learning**.
3. A név, kattintson a munkaterület majd **webszolgáltatások**.
4. A név, kattintson a **nyilvántartásba modell [prediktív exp].** .
5. Kattintson a hello hozzáadott új végpont.
6. Az hello végpont irányítópultján kattintson **frissítés erőforrás**.
7. Hello webszolgáltatás hello frissítés erőforrás API-JÁNAK dokumentációja lapján található hello **erőforrásnév** alatt **frissíthető erőforrások**.

Ha befejezte a hello végpont frissítése a SAS-jogkivonat nem, végre kell hajtania a hello feladatazonosító tooobtain friss jogkivonat GET.

Hello kódot már sikeresen futott, amikor új végpont hello kell kezdődnie, körülbelül 30 másodpercet hello retrained modellt használja.

## <a name="summary"></a>Összefoglalás
Használatával hello Átképezési API-kat, frissítheti az hello betanított modell egy prediktív webszolgáltatás forgatókönyvek például engedélyezése:

* Az új adatokat átképezési rendszeres modell.
* A modell toocustomers terjesztési azzal hello céllal, hogy újratanítása hello modell használatával saját adataikat.

## <a name="next-steps"></a>Következő lépések
[Hibaelhárítás a klasszikus Azure Machine Learning webszolgáltatás átképezési hello](machine-learning-troubleshooting-retraining-models.md)

