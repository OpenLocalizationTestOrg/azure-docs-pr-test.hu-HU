---
title: "a fájl az Azure Machine Learning Studio aaaImport adatait |} Microsoft Docs"
description: "Ismerje meg, hogyan tooupload a betanítási adatok fájlt a merevlemez-meghajtóról tooAzure Machine Learning Studio. Ez létrehoz egy olyan adatkészlet modult hello munkaterületen."
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
ms.openlocfilehash: 636facd9042145382c953a1c75969149ede6f6fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-machine-learning-studio"></a><span data-ttu-id="285ad-105">Tanítási adatokat importálhat egy fájlt a merevlemezen a Machine Learning Studióhoz</span><span class="sxs-lookup"><span data-stu-id="285ad-105">Import training data from a file on your hard drive into Machine Learning Studio</span></span>
[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

<span data-ttu-id="285ad-106">Ismerje meg, hogyan tooupload egy fájlt a merevlemez-meghajtóról toouse tanítási adatokat az Azure Machine Learning Studióban.</span><span class="sxs-lookup"><span data-stu-id="285ad-106">Learn how tooupload a data file from your hard drive toouse as training data in Azure Machine Learning Studio.</span></span> <span data-ttu-id="285ad-107">Hello adatfájl importálásával vannak a munkaterület egy adatkészlet modul használatra kész.</span><span class="sxs-lookup"><span data-stu-id="285ad-107">By importing hello data file, you have a dataset module ready for use in your workspace.</span></span>

## <a name="steps-tooimport-data-from-a-local-file"></a><span data-ttu-id="285ad-108">Egy helyi fájl lépéseket tooimport adatait</span><span class="sxs-lookup"><span data-stu-id="285ad-108">Steps tooimport data from a local file</span></span>
<span data-ttu-id="285ad-109">a helyi merevlemez-meghajtóról, tooimport adatok hello a következő:</span><span class="sxs-lookup"><span data-stu-id="285ad-109">tooimport data from a local hard drive, do hello following:</span></span>

1. <span data-ttu-id="285ad-110">Kattintson a **+ új** hello hello Machine Learning Studio ablakának alsó részén.</span><span class="sxs-lookup"><span data-stu-id="285ad-110">Click **+NEW** at hello bottom of hello Machine Learning Studio window.</span></span>
2. <span data-ttu-id="285ad-111">Válassza ki **DATASET** és **helyi FÁJLBÓL**.</span><span class="sxs-lookup"><span data-stu-id="285ad-111">Select **DATASET** and **FROM LOCAL FILE**.</span></span>
3. <span data-ttu-id="285ad-112">A hello **töltse fel az új adatkészlet** párbeszédpanelen Tallózás toohello fájlt tooupload</span><span class="sxs-lookup"><span data-stu-id="285ad-112">In hello **Upload a new dataset** dialog, browse toohello file you want tooupload</span></span>
4. <span data-ttu-id="285ad-113">Adjon meg egy nevet, hello adattípus azonosítása és ismertetésének.</span><span class="sxs-lookup"><span data-stu-id="285ad-113">Enter a name, identify hello data type, and optionally enter a description.</span></span> <span data-ttu-id="285ad-114">Olyan leírást ajánlott - toorecord lehetővé teszi bármely jellemzőit, amelyet tooremember hello adatok használata a jövőben hello hello adatokról.</span><span class="sxs-lookup"><span data-stu-id="285ad-114">A description is recommended - it allows you toorecord any characteristics about hello data that you want tooremember when using hello data in hello future.</span></span>
5. <span data-ttu-id="285ad-115">jelölőnégyzet hello **hello új verziója, amelyet egy meglévő adatkészlet** lehetővé teszi a tooupdate egy meglévő adatkészlet az új adatokat.</span><span class="sxs-lookup"><span data-stu-id="285ad-115">hello checkbox **This is hello new version of an existing dataset** allows you tooupdate an existing dataset with new data.</span></span> <span data-ttu-id="285ad-116">Ezt a jelölőnégyzetet, és írja be egy meglévő adatkészlet hello nevét.</span><span class="sxs-lookup"><span data-stu-id="285ad-116">Click this checkbox and then enter hello name of an existing dataset.</span></span>

![Töltse fel az új adatkészlet](media/machine-learning-import-data-from-local-file/upload-dataset.png)

<span data-ttu-id="285ad-118">Feltöltés közben megjelenik egy üzenet, hogy a fájl feltöltése van folyamatban.</span><span class="sxs-lookup"><span data-stu-id="285ad-118">During upload, you'll see a message that your file is being uploaded.</span></span> <span data-ttu-id="285ad-119">Töltse fel idő az adatok és hello sebessége a kapcsolat toohello szolgáltatás hello méretétől függ.</span><span class="sxs-lookup"><span data-stu-id="285ad-119">Upload time depends on hello size of your data and hello speed of your connection toohello service.</span></span> <span data-ttu-id="285ad-120">Ha ismeri az hello fájl hosszú időt vesz igénybe, megteheti belül a Machine Learning Studio egyebek, várakozás közben.</span><span class="sxs-lookup"><span data-stu-id="285ad-120">If you know hello file will take a long time, you can do other things inside Machine Learning Studio while you wait.</span></span> <span data-ttu-id="285ad-121">Azonban hello böngésző bezárása hello adatok feltöltése toofail okoz.</span><span class="sxs-lookup"><span data-stu-id="285ad-121">However, closing hello browser causes hello data upload toofail.</span></span>

## <a name="dataset-module-is-ready-for-use"></a><span data-ttu-id="285ad-122">A DataSet modul az használatra kész</span><span class="sxs-lookup"><span data-stu-id="285ad-122">Dataset module is ready for use</span></span>
<span data-ttu-id="285ad-123">Az adatok a feltöltést követően dataset modulban tárolt, és elérhető tooany kísérlet a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="285ad-123">Once your data is uploaded, it's stored in a dataset module and is available tooany experiment in your workspace.</span></span>

<span data-ttu-id="285ad-124">A kísérlet szerkesztésekor a létrehozott hello hello adatkészletek található **saját adatkészletek** hello alatti listában **mentett adatkészletek** hello modulpalettán listájában.</span><span class="sxs-lookup"><span data-stu-id="285ad-124">When you're editing an experiment, you can find hello datasets you've created in hello **My Datasets** list under hello **Saved Datasets** list in hello module palette.</span></span> <span data-ttu-id="285ad-125">Húzza és a hello dataset hello kísérlet vászonra dobja el, ha azt szeretné, toouse hello dataset további elemzés és a gépi tanulás.</span><span class="sxs-lookup"><span data-stu-id="285ad-125">You can drag and drop hello dataset onto hello experiment canvas when you want toouse hello dataset for further analytics and machine learning.</span></span>
