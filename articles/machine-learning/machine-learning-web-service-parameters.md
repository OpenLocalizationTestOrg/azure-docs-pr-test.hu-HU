---
title: "Azure Machine Learning webszolgáltatás-paramétereket aaaUse |} Microsoft Docs"
description: "Hogyan toouse Azure Machine Learning webszolgáltatás-paramétereket toomodify hello viselkedését a modell, hello webszolgáltatás történő hozzáféréskor."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: c49187db-b976-4731-89d6-11a0bf653db1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/12/2017
ms.author: raymondl;garye
ms.openlocfilehash: 214711eb819a6cea34db905abdf015da11e846d4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-machine-learning-web-service-parameters"></a>Az Azure Machine Learning webszolgáltatás paramétereinek használata
Az Azure Machine Learning webszolgáltatás közzététele, amely konfigurálható paraméterekkel lehetővé tevő modulokat tartalmaz a kísérlet hozta létre. Bizonyos esetekben érdemes lehet toochange hello modul viselkedés hello webes szolgáltatás futása közben. *Webszolgáltatási paramétereket* toodo lehetővé teszi ezt a feladatot. 

Ilyenek például hello beállítását [és adatokat importálhat] [ reader] modul hello felhasználó hello a közzétett webes szolgáltatás, megadhat egy másik adatforráshoz hello webszolgáltatás elérésekor. Vagy hello konfigurálása [adatok exportálása] [ writer] modul, hogy egy másik cél adható meg. Más például hello bitjei hello számának módosítása [Szolgáltatáskivonatolás] [ feature-hashing] hello kívánt funkcióinak modul vagy hello száma [szűrő-alapú szolgáltatás kiválasztása] [ filter-based-feature-selection] modul. 

Állítsa be a webszolgáltatás-paramétereket, és rendelje hozzá őket egy vagy több modulja paraméter a kísérletben, és megadhatja, hogy azok kötelező vagy választható. hello webszolgáltatás hello felhasználói is majd adja meg ezeket a paramétereket, amikor azok hello webszolgáltatás hívására. 

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="how-tooset-and-use-web-service-parameters"></a>Hogyan tooset és -felhasználási webszolgáltatás-paramétereket
A webszolgáltatási paraméter hello ikon következő toohello paraméter egy modul kattintva, majd válassza a "Web service paraméter beállítása" határozza meg. Ez létrehoz egy új webszolgáltatási paraméter, és kapcsolódik az toothat modul paraméter. Ezt követően amikor hello webszolgáltatás, hello felhasználó megadhatja a hello webszolgáltatási paraméter értékét, és alkalmazott toohello modul paramétere.

A webszolgáltatási paraméter határozza meg, ha elérhető tooany más modul paraméter hello kísérletben. Egy webszolgáltatási egy modul egy paraméterének társított paraméter megadása esetén használható, hogy ugyanazon webszolgáltatási paraméter bármely más modul mindaddig, amíg hello paraméter hello ugyanilyen típusú értéket vár. Például ha hello webszolgáltatási paraméter egy numerikus érték, majd csak használat modul paraméter, amely egy numerikus értéket várt. Hello felhasználó hello webszolgáltatási paraméter értéket állít be, akkor lesz társított alkalmazott tooall modul paraméterei.

Eldöntheti, hogy egy alapértelmezett tooprovide hello webszolgáltatási paraméter értékét. Ha, majd hello paraméter megadása nem kötelező hello felhasználó hello webszolgáltatás. Ha nem ad meg alapértelmezett értéket, majd hello felhasználó esetén szükséges tooenter értéket hello webszolgáltatás érhető el.

hello hello webszolgáltatás API dokumentációja tartalmaz adatokat hello webes szolgáltatás felhasználó hogyan toospecify hello webszolgáltatási paraméter programozott módon hello webszolgáltatás való hozzáféréskor.

> [!NOTE]
> API-JÁNAK dokumentációja hello a klasszikus webszolgáltatás hello keresztül valósul meg **API súgólap** hello webszolgáltatás hivatkozásra **IRÁNYÍTÓPULT** a Machine Learning Studióban. hello API-JÁNAK dokumentációja, az új webszolgáltatás hello keresztül valósul meg [Azure Machine Learning webszolgáltatások](https://services.azureml.net/Quickstart) hello portált **felhasználás** és **Swagger API** lapok: az a webes szolgáltatás.
> 
> 

## <a name="example"></a>Példa
Tegyük fel, tételezzük fel, hogy az a kísérlet van egy [adatok exportálása] [ writer] modul által küldött adatokat tooAzure blob Storage tárolóban. A webszolgáltatási nevű, "Blob elérési út" paraméter fogunk meghatározni, amely lehetővé teszi, hogy hello webes szolgáltatás felhasználói toochange hello elérési toohello a blob storage hello szolgáltatás elérésekor.

1. A Machine Learning Studióban, kattintson a hello [adatok exportálása] [ writer] modul tooselect azt. A Tulajdonságok hello tulajdonságai panelen toohello sarkában hello kísérletvászonra láthatók.
2. Adja meg a hello tárolási típusát:
   
   * A **adja meg az adatok cél**, jelölje be az "Azure Blob Storage".
   * A **adja meg a hitelesítési típus**, jelölje be az "Account".
   * Hello Azure blob storage hello fiók adatainak megadása. 
     <p />
3.Kattintson a hello ikon toohello sarkában hello **tároló paraméter tooblob kezdetű elérési**. Néz ki:
   
   ![Webes szolgáltatás paraméter ikon][icon]
   
   Válassza ki a "Web service paraméter beállítása".
   
   Bejegyzés hozzáadása az **webszolgáltatás-paramétereket** hello tulajdonságok panelen az hello "elérési út tooblob kezdődő neveket tároló" hello alján. Ez a hello webszolgáltatási paraméter, amelyik most már a társított [adatok exportálása] [ writer] modul paraméter.
4. toorename webszolgáltatási paraméter hello kattintson hello nevét, adja meg a "Blob elérési út", és nyomja le az ENTER hello **Enter** kulcs. 
5. tooprovide hello webszolgáltatási paraméter, az alapértelmezett értéket hello ikon toohello hello neve sarkában kattintson, válassza a "Megadása az alapértelmezett érték", adjon meg egy értéket (például "container1/output1.csv"), és nyomja le az ENTER hello **Enter** kulcs.
   
   ![Webszolgáltatási paraméter][parameter]
6. Kattintson a **Run** (Futtatás) parancsra. 
7. Kattintson a **webes szolgáltatás telepítése** válassza **webes szolgáltatás telepítése [klasszikus]** vagy **[Új] webes szolgáltatás telepítése** toodeploy hello webes szolgáltatás.

> [!NOTE] 
> toodeploy egy új webszolgáltatás-bővítmény, megfelelő engedélyekkel kell rendelkeznie a hello előfizetés toowhich meg hello webes szolgáltatás telepítéséhez. További információ: [egy webszolgáltatás-bővítmény hello Azure Machine Learning webszolgáltatások portálon kezelheti](machine-learning-manage-new-webservice.md). 

hello felhasználói hello webszolgáltatás most adja meg a hello új célhelyét [adatok exportálása] [ writer] modul hello webszolgáltatás való hozzáféréskor.

## <a name="more-information"></a>További információ
Részletes példa, lásd: hello [webszolgáltatás-paramétereket](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx) hello bejegyzést [Machine Learning Blog](http://blogs.technet.com/b/machinelearning/archive/2014/11/25/azureml-web-service-parameters.aspx).

A Machine Learning webszolgáltatás elérése további információkért lásd: [hogyan tooconsume az Azure Machine Learning Web service](machine-learning-consume-web-services.md).

<!-- Images -->
[icon]: ./media/machine-learning-web-service-parameters/icon.png
[parameter]: ./media/machine-learning-web-service-parameters/parameter.png


<!-- Module References -->
[feature-hashing]: https://msdn.microsoft.com/library/azure/c9a82660-2d9c-411d-8122-4d9e0b3ce92a/
[filter-based-feature-selection]: https://msdn.microsoft.com/library/azure/918b356b-045c-412b-aa12-94a1d2dad90f/
[reader]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
[writer]: https://msdn.microsoft.com/library/azure/7a391181-b6a7-4ad4-b82d-e419c0d6522c/

