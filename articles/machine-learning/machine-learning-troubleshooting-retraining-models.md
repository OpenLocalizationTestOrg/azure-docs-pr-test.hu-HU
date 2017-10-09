---
title: "az Azure Machine Learning klasszikus átképezési aaaTroubleshoot webszolgáltatás |} Microsoft Docs"
description: "Határozza meg és javítsa ki a gyakori problémák közben, amikor az Azure Machine Learning webszolgáltatás vannak mind hello modell."
services: machine-learning
documentationcenter: 
author: VDonGlover
manager: raymondl
editor: 
ms.assetid: 75cac53c-185c-437d-863a-5d66d871921e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 2b6a78eaba161877106dccdc23437b5e454fca7b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-hello-retraining-of-an-azure-machine-learning-classic-web-service"></a>Az Azure Machine Learning klasszikus webszolgáltatás az átképezési hello hibaelhárítása
## <a name="retraining-overview"></a>Megőrzési áttekintése
Egy prediktív kísérletté pontozási webszolgáltatásként központi telepítésekor a rendszer egy olyan statikus modell. Amint az elérhetővé válik az új adatok, vagy ha hello felhasználóinak azon hello API rendelkezik saját adataikat, hello modellnek támogatnia kell a toobe retrained. 

A folyamat egy klasszikus webszolgáltatás átképezési hello részletes útmutatást lásd: [újratanítása Machine Learning modellek szoftveres](machine-learning-retrain-models-programmatically.md).

## <a name="retraining-process"></a>Megőrzési folyamat
Ha tooretrain hello webszolgáltatás van szüksége, hozzá kell adnia néhány további eleme:

* A webszolgáltatás telepítve hello tanítási kísérletet. hello kísérlet rendelkeznie kell egy **webes szolgáltatás kimeneti** modul csatlakoztatva hello toohello kimenete **tanítási modell** modul.  
  
    ![Hello webes szolgáltatás kimeneti toohello tanítási modell csatolni.][image1]
* Új végpont webszolgáltatás pontozási tooyour hozzá.  Programozott módon adhat hozzá hello végpont használatával hello hivatkozott hello mintakód Machine Learning-modellek szoftveres témakör vagy hello a klasszikus Azure portálon keresztül.

Hello C# mintakód hello képzési Web Service API Súgó lap tooretrain modellből használhatja. Miután kiértékelték hello eredményeit, és elégedett őket, hello betanított modell hello új végpont hozzáadott webszolgáltatás pontozási frissítenie.

A hely összes hello darabot hello fő lépést kell végrehajtania, tooretrain hello modell a következők:

1. Hello képzési webes szolgáltatás hívása: hello hívás toohello kötegelt végrehajtási szolgáltatás (BES), nem hello kérelem válasz szolgáltatás (RR-EKET). Hello példakód C# hello API Súgó lap toomake hello hívásakor használható. 
2. Hello értékek keresése hello *BaseLocation*, *RelativeLocation*, és *SasBlobToken*: ezeket az értékeket ad vissza a hello kimeneti a hívás toohello képzési webes A szolgáltatás. 
   ![jelenít meg a minta és az hello BaseLocation RelativeLocation és SasBlobToken értékek átképezési hello hello kimenetét.][image6]
3. Frissítés hello hozzáadni az új pontozási hello webszolgáltatás hello végpont modell betanítása: hello mintakód használatával megadott hello újratanítása Machine Learning modellek szoftveres, frissítse hello új végpont toohello hello modell pontozása újonnan hozzáadott betanított modell a tanítási webszolgáltatás hello.

## <a name="common-obstacles"></a>Közös akadályok
### <a name="check-toosee-if-you-have-hello-correct-patch-url"></a>Toosee ellenőrizze, hogy javítsa ki az URL-cím javítás hello rendelkezik
hello javítás URL-címet használ kell lennie egy hello társított hello webszolgáltatás pontozási toohello hozzá új scoring-végpontja. Számos módon tooobtain hello javítás URL-címe:

**1. lehetőség: programozottan**

tooget hello javítsa ki a javítás URL-címe:

1. Futtassa a hello [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) példakód.
2. A AddEndpoint hello kimenetből megkeresi a hello *HelpLocation* értékét, és másolja hello URL-címet.
   
   ![HelpLocation hello kimenet hello addEndpoint minta.][image2]
3. Hello URL-cím illessze be egy böngésző toonavigate tooa lap, amely súgóhivatkozások biztosít hello webszolgáltatás.
4. Kattintson a hello **frissítés erőforrás** hivatkozás tooopen hello javítás súgólap.

**2. lehetőség: Hello klasszikus Azure portál használata**

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Nyissa meg hello Machine Learning fülre. ![Gép leaning lapján.][image4]
3. A munkaterület neve, majd kattintson **webszolgáltatások**.
4. Kattintson a hello pontozási webszolgáltatás dolgozik. (Ha Ön nem módosította a következő hello webszolgáltatás hello alapértelmezett nevét, akkor lejár [pontozási Exp.].)
5. Kattintson a **végpont hozzáadása**.
6. Miután hello végpontot hoz létre, kattintson a hello végpont nevét. Kattintson a **frissítés erőforrás** tooopen hello javítási súgólap.

> [!NOTE]
> Hello végpont toohello képzési webszolgáltatás helyett hello prediktív webszolgáltatás felvett, kapni fog a hello hello kattintva a következő hiba **frissítés erőforrás** hivatkozás: Sajnáljuk, ez a funkció nem támogatott, de vagy érhető el ebben a környezetben. Ez a webszolgáltatás nem frissíthető erőforrással rendelkezik. A Microsoft hello kellemetlenségért elnézését kérjük, és javítása a munkafolyamat dolgozik.
> 
> 

![Új endpoint irányítópult.][image3]

hello javítás súgólap tartalmaz hello javítás URL-címet kell használnia, és használhatja példakódot tartalmaz toocall azt.

![Javítás URL-címe.][image5]

### <a name="check-toosee-that-you-are-updating-hello-correct-scoring-endpoint"></a>Ellenőrizze, hogy a frissítő hello megfelelő pontozási végpont toosee
* A javítás nem hello képzési webszolgáltatás: hello webszolgáltatás pontozási hello javítási műveletet kell végrehajtani.
* A javítás nem hello alapértelmezett végpont a webszolgáltatás: hello webszolgáltatási végpontot hozzáadott új pontozási hello javítási műveletet kell végrehajtani.

Ellenőrizheti, hogy melyik webes szolgáltatásvégpont hello szerint be kapcsolva látogató hello a klasszikus Azure portálon. 

> [!NOTE]
> Győződjön meg arról, hogy hello végpont toohello prediktív webes szolgáltatás, a képzési webszolgáltatás hello nem ad hozzá. Ha megfelelően telepítette, a képzési és a prediktív webszolgáltatás, megtekintheti az felsorolt két külön webszolgáltatások. "[prediktív exp.]" Hello prediktív webszolgáltatás kell végződnie.
> 
> 

1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Nyissa meg hello Machine Learning fülre. ![Machine learning-munkaterület felhasználói felületén.][image4]
3. A munkaterület kiválasztása.
4. Kattintson a **webszolgáltatások**.
5. Válassza ki a prediktív webszolgáltatás.
6. Győződjön meg arról, hogy az új végpont toohello webszolgáltatás lett hozzáadva.

### <a name="check-hello-workspace-that-your-web-service-is-in-tooensure-it-is-in-hello-correct-region"></a>Ellenőrizze, hogy a webszolgáltatás megtalálható-e megfelelő régióban található hello tooensure hello munkaterület
1. Jelentkezzen be toohello [a klasszikus Azure portálon](https://manage.windowsazure.com).
2. Válassza ki a Machine Learning hello menüből.
   ![Machine learning régió felhasználói felületén.][image4]
3. Ellenőrizze a munkaterület hello helyét.

<!-- Image Links -->

[image1]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-studio-tm-connnected-to-web-service-out.png
[image2]: ./media/machine-learning-troubleshooting-retraining-a-model/addEndpoint-output.png
[image3]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-update-resource.png
[image4]: ./media/machine-learning-troubleshooting-retraining-a-model/azure-portal-machine-learning-tab.png
[image5]: ./media/machine-learning-troubleshooting-retraining-a-model/ml-help-page-patch-url.png
[image6]: ./media/machine-learning-troubleshooting-retraining-a-model/retraining-output.png
[image7]: ./media/machine-learning-troubleshooting-retraining-a-model/web-services-tab.png
