---
title: "1. lépés: A Machine Learning-munkaterület létrehozása |} Microsoft Docs"
description: "1. lépésében hello fejlesztése egy prediktív megoldás forgatókönyv: megtudhatja, hogyan tooset egy új Azure Machine Learning Studio munkaterületet."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b3c97e3d-16ba-4e42-9657-2562854a1e04
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/23/2017
ms.author: garye
ms.openlocfilehash: 93d2e240826db9768e85b00cab0eb62510b4efb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-step-1-create-a-machine-learning-workspace"></a>Az útmutató 1. lépése: Machine Learning-munkaterület létrehozása
Ez az első lépés hello hello forgatókönyv [az Azure Machine Learning a prediktív elemzési megoldás fejlesztése](machine-learning-walkthrough-develop-predictive-solution.md).

1. **Machine Learning-munkaterület létrehozása**
2. [Meglévő adatok feltöltése](machine-learning-walkthrough-2-upload-data.md)
3. [Új kísérlet létrehozása](machine-learning-walkthrough-3-create-new-experiment.md)
4. [Betanítása és kiértékelése hello modellek](machine-learning-walkthrough-4-train-and-evaluate-models.md)
5. [Hello webszolgáltatás telepítése](machine-learning-walkthrough-5-publish-web-service.md)
6. [Hozzáférés hello webszolgáltatás](machine-learning-walkthrough-6-access-web-service.md)

- - -
<!-- This needs toobe updated toorefer toohello new way of creating workspaces in hello Ibiza portal -->

a Machine Learning Studio toouse, kell toohave Microsoft Azure Machine Learning munkaterülettel. A munkaterület toocreate kell hello eszközöket tartalmazza, kezelése és kísérletek közzététele.  

<!--
## toocreate a workspace
1. Sign in toohello [Azure classic portal](https://manage.windowsazure.com).
2. In hello  Azure services panel, click **MACHINE LEARNING**.  
   ![Create workspace][1]
3. Click **CREATE AN ML WORKSPACE**.
4. On hello **QUICK CREATE** page, enter your workspace information and then click **CREATE AN ML WORKSPACE**.
-->

az Azure-előfizetése rendszergazdája hello toocreate hello munkaterület kell, és adja meg a tulajdonos vagy közreműködő. További információkért lásd: [létrehozása és a megosztáshoz egy Azure Machine Learning munkaterülettel](machine-learning-create-workspace.md).

A munkaterület létrehozását követően nyissa meg a Machine Learning Studio ([https://studio.azureml.net/Home](https://studio.azureml.net/Home)). Ha egynél több munkaterület, igény szerint hello munkaterület a hello eszköztár hello ablak jobb felső sarkában hello.

![A Studio munkaterület kiválasztása][2]

> [!TIP]
> Ha hello munkaterület tulajdonos történt, azzal mások dolgozik hello kísérletek megoszthatja toohello munkaterületen. Ehhez a Machine Learning Studióban a hello **beállítások** lap. Egyszerűen hello Microsoft-fiókkal vagy szervezeti fiókkal minden felhasználó számára.
> 
> A hello **beállítások** kattintson **felhasználók**, kattintson a **több felhasználók MEGHÍVÁSA** hello ablak hello alján.
> 
> 

- - -
**Következő: [meglévő adatok feltöltése](machine-learning-walkthrough-2-upload-data.md)**

[1]: ./media/machine-learning-walkthrough-1-create-ml-workspace/create1.png
[2]: ./media/machine-learning-walkthrough-1-create-ml-workspace/open-workspace.png
