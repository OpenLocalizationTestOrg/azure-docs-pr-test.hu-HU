---
title: "Az Azure Machine Learning automatikus adatok csővezeték-Adatlap |} Microsoft Docs"
description: "Nyomtatható Adatlap, amely azt ismerteti, hogyan beállítása az Azure Machine Learning webszolgáltatásba az automatikus csővezeték-e az adatok a helyszínen, streamelés esetén az Azure-ban vagy harmadik fél felhőszolgáltatásban."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 22674d6b-4491-4805-a3ac-d423611177bb
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: mithal;garye
ms.openlocfilehash: e212b2c935eb0ae64ed1cd2e6dc1564f8fcd503b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="cheat-sheet-for-an-automated-data-pipeline-for-azure-machine-learning-predictions"></a><span data-ttu-id="8628b-103">Gyorsreferencia az Azure Machine Learning-előrejelzések automatikus folyamatához</span><span class="sxs-lookup"><span data-stu-id="8628b-103">Cheat sheet for an automated data pipeline for Azure Machine Learning predictions</span></span>
<span data-ttu-id="8628b-104">A **Microsoft Azure Machine Learning automatikus adatok csővezeték-adatlap** segít a technológia beolvasni az adatokat, hogy a Machine Learning segítségével történő navigálás webszolgáltatás, ahol azt is pontozni a prediktív elemzési modell.</span><span class="sxs-lookup"><span data-stu-id="8628b-104">The **Microsoft Azure Machine Learning automated data pipeline cheat sheet** helps you navigate through the technology you can use to get your data to your Machine Learning web service where it can be scored by your predictive analytics model.</span></span>

<span data-ttu-id="8628b-105">Attól függően az adatok-e a helyszínen, a felhőben, vagy streaming valós idejű, az adatok áthelyezése a webszolgáltatási végpontot pontozó elérhető különböző eljárásokat.</span><span class="sxs-lookup"><span data-stu-id="8628b-105">Depending on whether your data is on-premises, in the cloud, or streaming real-time, there are different mechanisms available to move the data to your web service endpoint for scoring.</span></span>
<span data-ttu-id="8628b-106">Ez Adatlap végigvezeti a döntést, meg kell győződnie, és hivatkozásokat segíthetnek a megoldást kínál.</span><span class="sxs-lookup"><span data-stu-id="8628b-106">This cheat sheet walks you through the decisions you need to make, and it offers links to articles that can help you develop your solution.</span></span>

## <a name="download-the-machine-learning-automated-data-pipeline-cheat-sheet"></a><span data-ttu-id="8628b-107">Töltse le a Machine Learning automatikus folyamat Adatlap</span><span class="sxs-lookup"><span data-stu-id="8628b-107">Download the Machine Learning automated data pipeline cheat sheet</span></span>
<span data-ttu-id="8628b-108">Miután letöltötte a Adatlap, nyomtathatja tabloid méretben (11 x 17).</span><span class="sxs-lookup"><span data-stu-id="8628b-108">Once you download the cheat sheet, you can print it in tabloid size (11 x 17 in.).</span></span>

<span data-ttu-id="8628b-109">Töltse le a Adatlap:  **[Microsoft Azure Machine Learning automatikus adatok csővezeték-Adatlap](http://download.microsoft.com/download/C/C/7/CC726F8B-2E6F-4C20-9B6F-AFBEE8253023/microsoft-machine-learning-operationalization-cheat-sheet_v1.pdf)**</span><span class="sxs-lookup"><span data-stu-id="8628b-109">Download the cheat sheet here: **[Microsoft Azure Machine Learning automated data pipeline cheat sheet](http://download.microsoft.com/download/C/C/7/CC726F8B-2E6F-4C20-9B6F-AFBEE8253023/microsoft-machine-learning-operationalization-cheat-sheet_v1.pdf)**</span></span>

![Microsoft Azure Machine Learning Studio képességek áttekintése][op-cheat-sheet]

[op-cheat-sheet]: ./media/machine-learning-automated-data-pipeline-cheat-sheet/machine-learning-automated-data-pipeline-cheat-sheet_v1.1.png


## <a name="more-help-with-machine-learning-studio"></a><span data-ttu-id="8628b-111">További segítség a Machine Learning Studio használatához</span><span class="sxs-lookup"><span data-stu-id="8628b-111">More help with Machine Learning Studio</span></span>
* <span data-ttu-id="8628b-112">Microsoft Azure Machine Learning áttekintését lásd: [Bevezetés a Microsoft Azure machine learning](machine-learning-what-is-machine-learning.md).</span><span class="sxs-lookup"><span data-stu-id="8628b-112">For an overview of Microsoft Azure Machine Learning, see [Introduction to machine learning on Microsoft Azure](machine-learning-what-is-machine-learning.md).</span></span>
* <span data-ttu-id="8628b-113">Egy pontozási webszolgáltatás-bővítmény telepítése egy ismertetése [központi telepítése az Azure Machine Learning webszolgáltatás](machine-learning-publish-a-machine-learning-web-service.md).</span><span class="sxs-lookup"><span data-stu-id="8628b-113">For an explanation of how to deploy a scoring web service, see [Deploy an Azure Machine Learning web service](machine-learning-publish-a-machine-learning-web-service.md).</span></span>
* <span data-ttu-id="8628b-114">Hogyan pontozási webszolgáltatás felhasználásához tárgyalását lásd: [hogyan kell használni az Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span><span class="sxs-lookup"><span data-stu-id="8628b-114">For a discussion of how to consume a scoring web service, see [How to consume an Azure Machine Learning Web service](machine-learning-consume-web-services.md).</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

