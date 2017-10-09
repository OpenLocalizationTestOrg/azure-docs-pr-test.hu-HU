---
title: "a Machine Learning webszolgáltatásba web app sablonnal aaaConsume |} Microsoft Docs"
description: "Web app sablon használata a Azure piactérről tooconsume az Azure Machine Learning a prediktív webes szolgáltatás."
keywords: "a gépi tanulás webszolgáltatás, operationalization, REST API-t"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: e0d71683-61b9-4675-8df5-09ddc2f0d92d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;raymondl
ms.openlocfilehash: 1199377bead470807d58ca7f7a667175cbb88450
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="consume-an-azure-machine-learning-web-service-with-a-web-app-template"></a>Azure Machine Learning webszolgáltatások használata webalkalmazás-sablonnal

Egyszer már a prediktív modell fejlesztett, és telepítve lett webszolgáltatásként az Azure Machine Learning Studio használatával, vagy használja az eszközök, például az R vagy Python, van-e hozzáférési hello operationalized modell egy REST API használatával.

Számos módon tooconsume hello REST API-t és hello webes szolgáltatás. Például egy alkalmazás írhatnak C# nyelven íródtak, R, vagy Python hello segítségével példakód létrehozza a hello webszolgáltatás üzembe helyezésekor (hello elérhető [Machine Learning Web Services portálra](https://services.azureml.net/quickstart) vagy hello web service irányítópult A Machine Learning Studio). Hello Microsoft Excel-munkafüzet minta létrehozza a hello, vagy ugyanannyi időt vesz igénybe.

De hello Web App hello elérhető sablonok keresztül van a webszolgáltatás leggyorsabb és legegyszerűbb módja tooaccess hello [Azure Web App piactér](https://azure.microsoft.com/marketplace/web-applications/all/).

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="hello-azure-machine-learning-web-app-templates"></a>hello Azure Machine Learning Web App sablonok
hello web app hello Azure piactéren elérhető sablonok hozhat létre egy egyéni webalkalmazást, hogy ismeri a webszolgáltatás bemeneti adatok és a kívánt eredmény elérése érdekében. Mindössze annyit kell toodo hello web access tooyour webszolgáltatás és az adatokat, és hello sablon hello rest.

Két sablonok érhetők el:

* [Az Azure ML kérés-válasz szolgáltatás Web App-sablon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/)
* [Azure ML kötegelt végrehajtási szolgáltatás Web App-sablon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/)

Minden sablon egy minta ASP.NET alkalmazást, a webszolgáltatás hello API URI és kulcs használatával hoz létre, és telepíti, a webhely tooAzure. hello kérés-válasz szolgáltatás (RR-EKET) sablon létrehoz egy webalkalmazást, amely lehetővé teszi az adatok toohello web service tooget egyetlen eredmény egyetlen sor toosend. hello kötegelt végrehajtási szolgáltatás (BES) sablon létrehoz egy webalkalmazást, amely lehetővé teszi toosend adatok tooget hány sornyi több eredményt.

Nincs kódolási van szükség toouse ezeket a sablonokat. Csak akkor adja meg, hello API-kulcsot és URI-t, és hello sablon buildek hello alkalmazását.

tooget hello API-kulcs és a kérelem URI-azonosítója az adott webszolgáltatás:

1. A hello [Web Services portálra](https://services.azureml.net/quickstart), egy új webszolgáltatás kattintson **webszolgáltatások** hello tetején. Vagy a klasszikus web service kattintson **klasszikus webszolgáltatások**.
2. Kattintson a kívánt tooaccess hello webszolgáltatáshoz.
3. Klasszikus webszolgáltatáshoz kattintson a kívánt tooaccess hello végpont.
4. Kattintson a **felhasználás** hello tetején.
5. Másolás hello **elsődleges** vagy **másodlagos kulcs** és mentse azt.
6. A kérés-válasz szolgáltatás (RR-EKET) sablon létrehozásakor, másolja a hello **kérés-válasz** URI és mentse azt. Ha egy kötegelt végrehajtási szolgáltatás (BES) sablont hoz létre, másolja a hello **kötegelt kérelmekben** URI és mentse azt.


## <a name="how-toouse-hello-request-response-service-rrs-template"></a>Hogyan toouse hello kérés-válasz szolgáltatás (RR-EKET) sablon
Kövesse ezeket lépéseket toouse hello RR-EKET web app-sablon, a hello a következő ábrán látható módon.

![Folyamat toouse RRS webes sablon][image1]


<!--    ![API Key][image3] -->

<!-- This value will look like this:
   
        https://ussouthcentral.services.azureml.net/workspaces/<workspace-id>/services/<service-id>/execute?api-version=2.0&details=true
   
    ![Request URI][image4] -->

1. Nyissa meg toohello [Azure-portálon](https://portal.azure.com), **bejelentkezési**, kattintson **új**, és adja meg **Azure ML kérés-válasz szolgáltatás Web App**, kattintson a **Létrehozása**. 
   
   * Adjon meg egy egyedi nevet a webalkalmazás. hello webalkalmazás URL-címe hello lesz a nevét, majd `.azurewebsites.net.` például`http://carprediction.azurewebsites.net.`
   * Válassza ki a hello Azure-előfizetés és a szolgáltatások alatt a webszolgáltatás fut..
   * Kattintson a **Create** (Létrehozás) gombra.
     
     ![Webalkalmazás létrehozása][image5]

4. Ha Azure hello webalkalmazás telepítése befejeződött, kattintson a hello **URL-cím** a webes alkalmazás fiókbeállítási oldalára Azure hello, vagy meg hello URL-címet egy webböngészőben. Például: `http://carprediction.azurewebsites.net.`
5. Ha a webes alkalmazás első futtatásakor, ekkor megkérdezi, hogy a hello hello **API Post URL-címe** és **API-kulcs**.
   Adja meg a korábban mentett hello értékek (**kérelem URI-azonosítója** és **API-kulcs**, illetve).
     
     Kattintson a **nyújt**.
     
     ![Adja meg a feladás egy vagy több URI és az API-kulcs][image6]

6. hello webes alkalmazás megjelenik a **Web App konfigurációs** hello aktuális webszolgáltatások beállításai lapon. Itt módosíthatja hello webes alkalmazás által használt toohello beállításokat.
   
   > [!NOTE]
   > Itt hello szolgáltatás beállításainak módosítása csak akkor változik meg azokat a webalkalmazás. Hello alapértelmezett beállítások a webszolgáltatás nem módosítja. Például, ha módosítja a hello **leírás** itt látható hello webes szolgáltatás irányítópultján a Machine Learning Studióban hello leírás nem módosítja.
   > 
   > 
   
    Amikor elkészült, kattintson a **módosítások mentése**, és kattintson a **tooHome lapon lépjen**.

7. A hello kezdőlap adhatja értékek toosend tooyour webszolgáltatás. Kattintson a **Submit** amikor elkészült, és hello eredményt adja vissza.

Ha azt szeretné, hogy tooreturn toohello **konfigurációs** lapon, nyissa meg toohello `setting.aspx` hello webalkalmazás oldalán. Például: `http://carprediction.azurewebsites.net/setting.aspx.` újra lesz felszólító tooenter hello API-kulcs –, hogy tooaccess hello lapot, és hello-beállítások frissítése van szükség.

Állítsa le, indítsa újra, vagy az Azure portálon, mint bármely más webalkalmazás hello hello a webalkalmazás törlése. Mindaddig, amíg fut-e otthoni webcímet toohello navigálhat, és adja meg új értékeket.

## <a name="how-toouse-hello-batch-execution-service-bes-template"></a>Hogyan toouse hello kötegelt végrehajtási szolgáltatás (BES) sablon
Használhatja a hello BES azonos módon-sablonként hello RR-EKET, kivéve létrehozott hello webes alkalmazást hello web app-sablon lehetővé teszi a toosubmit több sornyi adatot és több eredményeket.

hello bemeneti értékeket a kötegelt végrehajtási webszolgáltatáshoz származhatnak az Azure storage vagy a helyi fájl; hello eredményeket egy az Azure storage-tároló tárolja.
Igen, akkor lesz szüksége egy Azure storage-tároló toohold hello hello webalkalmazás által visszaadott eredmények, és frissítenie kell tooget a bemeneti adatok készen áll.

![Toouse BES feldolgozni webes sablon][image2]

1. Hajtsa végre az azonos hello eljárás toocreate hello BES webalkalmazás mint hello RR-EKET sablon, kivéve Ugrás túl[Azure ML kötegelt végrehajtási szolgáltatás Web App sablon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/) tooopen hello BES sablon Azure piactéren, és kattintson a **webalkalmazás létrehozása** .

2. hello eredmények tárolt, ahová toospecify hello cél tároló adatokat be hello web app kezdőlap. Ha hello webalkalmazás kérheti le a hello bemeneti értékeket, vagy egy helyi fájl vagy egy Azure storage-tárolót is megadhat.
   Kattintson a **nyújt**.
   
    ![Szolgáltatás adatai][image7]

hello web app egy feladat állapota lapon jelenik meg.
Hello feladat befejezése fogja az Azure blob storage hello eredmények hello helyét adni. Akkor is hello eredmények tooa helyi fájl letöltése hello lehetőségét.

## <a name="for-more-information"></a>További információ
További információk toolearn...

* Tekintse meg a Machine Learning Studio, a gépi tanulási kísérlet létrehozása [az első kísérlet létrehozása az Azure Machine Learning Studióban](machine-learning-create-experiment.md)
* Hogyan a machine learning webszolgáltatásként, kísérletezhet toodeploy: [az Azure Machine Learning webszolgáltatás telepítése](machine-learning-publish-a-machine-learning-web-service.md)
* más módokon tooaccess a webszolgáltatás, lásd: [hogyan tooconsume az Azure Machine Learning Web service](machine-learning-consume-web-services.md)

[image1]: media/machine-learning-consume-web-service-with-web-app-template/rrs-web-template-flow.png
[image2]: media/machine-learning-consume-web-service-with-web-app-template/bes-web-template-flow.png
[image3]: media/machine-learning-consume-web-service-with-web-app-template/api-key.png
[image4]: media/machine-learning-consume-web-service-with-web-app-template/post-uri.png
[image5]: media/machine-learning-consume-web-service-with-web-app-template/create-web-app.png
[image6]: media/machine-learning-consume-web-service-with-web-app-template/web-service-info.png
[image7]: media/machine-learning-consume-web-service-with-web-app-template/storage.png
