---
title: "6. lépés: Hozzáférési hello Machine Learning webszolgáltatás |} Microsoft Docs"
description: "6. lépésében hello fejlesztése egy prediktív megoldás forgatókönyv: egy aktív Azure Machine Learning Web service eléréséhez."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 6a65c89a-40ab-4673-8dd8-8eee0a150e3b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 211de0294092c6a6b5e6eb608d5d3b88107674c6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-6-access-hello-azure-machine-learning-web-service"></a>A forgatókönyv 6. lépés: Hozzáférési hello Azure Machine Learning webszolgáltatás

Ez az utolsó lépésében hello hello forgatókönyv [az Azure Machine Learning a prediktív elemzési megoldás fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md)

1. [Machine Learning-munkaterület létrehozása](machine-learning-walkthrough-1-create-ml-workspace.md)
2. [Meglévő adatok feltöltése](machine-learning-walkthrough-2-upload-data.md)
3. [Új kísérlet létrehozása](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Betanítása és kiértékelése hello modellek](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Hello webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md)
6. **Hozzáférés hello webszolgáltatás**

- - -
A forgatókönyv hello előző lépésben helyeztünk üzembe egy webszolgáltatás, amelyet a jóváírás kockázat előrejelzési modellt használ. A felhasználók most már tudja toosend adatok tooit és eredményeket. 

hello webszolgáltatás egy Azure webes szolgáltatás, amely kaphat, és térjen vissza az adatokat, és REST API-k az alábbi két módszer egyikével:  

* **Kérelem/válasz** – hello felhasználó elküldi egy vagy több sornyi jóváírás adatok toohello egy HTTP protokoll használatával szolgáltatásra, és hello szolgáltatás válasza eredményeket egy vagy több készletekkel.
* **Kötegelt végrehajtás** – hello felhasználói tárolja egy vagy több sort jóváírás adatok az Azure blob-, és ezután elküldi a hello blob hely toohello szolgáltatás. hello szolgáltatás pontszámok összes hello adatsorokat hello bemeneti blob, tárolók hello egy másik blob eredményez, és visszaadja hello tároló URL-CÍMÉT.  

hello keresztül történik egy klasszikus webszolgáltatás leggyorsabb és legegyszerűbb módja tooaccess hello [Azure ML kérés-válasz szolgáltatás webalkalmazás](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlaspnettemplateforrrs/) vagy [Azure ML kötegelt végrehajtási szolgáltatás Web App sablon](https://azure.microsoft.com/marketplace/partners/microsoft/azuremlbeswebapptemplate/).

Ezeket a webes alkalmazás sablonokat hozhat létre egy egyéni webalkalmazást, hogy ismeri a webszolgáltatás bemeneti adatokat, és mi ad vissza. Toodo szüksége, adja meg a hozzáférés tooyour webszolgáltatás és az adatokat, és hello sablon hello rest.

Hello web app sablonok használatával kapcsolatos további információkért lásd: [egy Azure Machine Learning Web service web app sablonnal felhasználásához](machine-learning-consume-web-service-with-web-app-template.md).

Egy egyéni alkalmazást tooaccess hello webszolgáltatás előírt, R, C# és Python programozási nyelvek starter kód használatával is készíthet.

A teljes részletei [hogyan tooconsume az Azure Machine Learning Web service](machine-learning-consume-web-services.md).

