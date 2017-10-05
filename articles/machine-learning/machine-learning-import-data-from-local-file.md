---
title: "Adatokat importálhat egy fájlt Azure Machine Learning Studio |} Microsoft Docs"
description: "Útmutató: Azure Machine Learning Studio a merevlemez-meghajtóról adatok képzési fájl feltöltéséhez. Ez létrehoz egy olyan adatkészlet modult a munkaterületen."
keywords: "adatok, adatformátum, adattípusok, adatforrások, betanítási adatok importálása"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: c0dd9e90-23c4-4f64-8b8f-489ad79f047b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye;bradsev
ms.openlocfilehash: 18010864160ceb2d76aea37196e6944bbe426457
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-machine-learning-studio"></a><span data-ttu-id="b6ed1-105">Tanítási adatokat importálhat egy fájlt a merevlemezen a Machine Learning Studióhoz</span><span class="sxs-lookup"><span data-stu-id="b6ed1-105">Import training data from a file on your hard drive into Machine Learning Studio</span></span>
[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

<span data-ttu-id="b6ed1-106">Ismerje meg, hogyan tölthet fel a merevlemez-meghajtóról kívánja használni, mint az Azure Machine Learning Studióban betanítási adatok adatfájlt.</span><span class="sxs-lookup"><span data-stu-id="b6ed1-106">Learn how to upload a data file from your hard drive to use as training data in Azure Machine Learning Studio.</span></span> <span data-ttu-id="b6ed1-107">Importálja az adatfájlban, rendelkezik egy adatkészlet modul használatra kész a munkaterület.</span><span class="sxs-lookup"><span data-stu-id="b6ed1-107">By importing the data file, you have a dataset module ready for use in your workspace.</span></span>

## <a name="steps-to-import-data-from-a-local-file"></a><span data-ttu-id="b6ed1-108">Adatokat importálhat egy helyi fájl lépései</span><span class="sxs-lookup"><span data-stu-id="b6ed1-108">Steps to import data from a local file</span></span>
<span data-ttu-id="b6ed1-109">Adatokat importálhat egy helyi merevlemezen, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="b6ed1-109">To import data from a local hard drive, do the following:</span></span>

1. <span data-ttu-id="b6ed1-110">Kattintson a **+ új** a Machine Learning Studio ablakának alján.</span><span class="sxs-lookup"><span data-stu-id="b6ed1-110">Click **+NEW** at the bottom of the Machine Learning Studio window.</span></span>
2. <span data-ttu-id="b6ed1-111">Válassza ki **DATASET** és **helyi FÁJLBÓL**.</span><span class="sxs-lookup"><span data-stu-id="b6ed1-111">Select **DATASET** and **FROM LOCAL FILE**.</span></span>
3. <span data-ttu-id="b6ed1-112">Az a **töltse fel az új adatkészlet** párbeszédpanelen keresse meg a feltölteni kívánt fájl</span><span class="sxs-lookup"><span data-stu-id="b6ed1-112">In the **Upload a new dataset** dialog, browse to the file you want to upload</span></span>
4. <span data-ttu-id="b6ed1-113">Adjon meg egy nevet, és azonosíthatja a adattípus ismertetésének.</span><span class="sxs-lookup"><span data-stu-id="b6ed1-113">Enter a name, identify the data type, and optionally enter a description.</span></span> <span data-ttu-id="b6ed1-114">Olyan leírást ajánlott - lehetővé teszi bármely jellemzőinek, amelyet a későbbiekben az adatok használatakor vegye figyelembe az adatok rögzítéséhez.</span><span class="sxs-lookup"><span data-stu-id="b6ed1-114">A description is recommended - it allows you to record any characteristics about the data that you want to remember when using the data in the future.</span></span>
5. <span data-ttu-id="b6ed1-115">A jelölőnégyzet **egyik meglévő adatkészletét új verziója** lehetővé teszi egy meglévő adatkészlet frissítése az új adatokat.</span><span class="sxs-lookup"><span data-stu-id="b6ed1-115">The checkbox **This is the new version of an existing dataset** allows you to update an existing dataset with new data.</span></span> <span data-ttu-id="b6ed1-116">Ezt a jelölőnégyzetet, és írja be egy meglévő adatkészlet nevét.</span><span class="sxs-lookup"><span data-stu-id="b6ed1-116">Click this checkbox and then enter the name of an existing dataset.</span></span>

![Töltse fel az új adatkészlet](media/machine-learning-import-data-from-local-file/upload-dataset.png)

<span data-ttu-id="b6ed1-118">Feltöltés közben megjelenik egy üzenet, hogy a fájl feltöltése van folyamatban.</span><span class="sxs-lookup"><span data-stu-id="b6ed1-118">During upload, you'll see a message that your file is being uploaded.</span></span> <span data-ttu-id="b6ed1-119">Töltse fel idő az adatok méretétől és a szolgáltatásnak a kapcsolat sebességétől függ.</span><span class="sxs-lookup"><span data-stu-id="b6ed1-119">Upload time depends on the size of your data and the speed of your connection to the service.</span></span> <span data-ttu-id="b6ed1-120">Ha tudja, hogy a fájl hosszú időt vesz igénybe, megteheti a Machine Learning Studio belül egyebek, várakozás közben.</span><span class="sxs-lookup"><span data-stu-id="b6ed1-120">If you know the file will take a long time, you can do other things inside Machine Learning Studio while you wait.</span></span> <span data-ttu-id="b6ed1-121">Azonban a böngésző bezárásával hatására az adatok feltöltése sikertelen.</span><span class="sxs-lookup"><span data-stu-id="b6ed1-121">However, closing the browser causes the data upload to fail.</span></span>

## <a name="dataset-module-is-ready-for-use"></a><span data-ttu-id="b6ed1-122">A DataSet modul az használatra kész</span><span class="sxs-lookup"><span data-stu-id="b6ed1-122">Dataset module is ready for use</span></span>
<span data-ttu-id="b6ed1-123">Az adatok a feltöltést követően egy adatkészlet modulban tárolja, és bármilyen kísérlet a munkaterület érhető el.</span><span class="sxs-lookup"><span data-stu-id="b6ed1-123">Once your data is uploaded, it's stored in a dataset module and is available to any experiment in your workspace.</span></span>

<span data-ttu-id="b6ed1-124">A kísérlet szerkesztésekor a létrehozott adathalmazok található a **saját adatkészletek** listában az a **mentett adatkészletek** a modulpalettán lista.</span><span class="sxs-lookup"><span data-stu-id="b6ed1-124">When you're editing an experiment, you can find the datasets you've created in the **My Datasets** list under the **Saved Datasets** list in the module palette.</span></span> <span data-ttu-id="b6ed1-125">Húzza, és dobja el az adatkészletet a kísérlet vászonra, ha meg szeretné használni a dataset további elemzés és a gépi tanulás.</span><span class="sxs-lookup"><span data-stu-id="b6ed1-125">You can drag and drop the dataset onto the experiment canvas when you want to use the dataset for further analytics and machine learning.</span></span>
