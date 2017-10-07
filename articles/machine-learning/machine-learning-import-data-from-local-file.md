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
# <a name="import-training-data-from-a-file-on-your-hard-drive-into-machine-learning-studio"></a>Tanítási adatokat importálhat egy fájlt a merevlemezen a Machine Learning Studióhoz
[!INCLUDE [import-data-into-aml-studio-selector](../../includes/machine-learning-import-data-into-aml-studio.md)]

Ismerje meg, hogyan tooupload egy fájlt a merevlemez-meghajtóról toouse tanítási adatokat az Azure Machine Learning Studióban. Hello adatfájl importálásával vannak a munkaterület egy adatkészlet modul használatra kész.

## <a name="steps-tooimport-data-from-a-local-file"></a>Egy helyi fájl lépéseket tooimport adatait
a helyi merevlemez-meghajtóról, tooimport adatok hello a következő:

1. Kattintson a **+ új** hello hello Machine Learning Studio ablakának alsó részén.
2. Válassza ki **DATASET** és **helyi FÁJLBÓL**.
3. A hello **töltse fel az új adatkészlet** párbeszédpanelen Tallózás toohello fájlt tooupload
4. Adjon meg egy nevet, hello adattípus azonosítása és ismertetésének. Olyan leírást ajánlott - toorecord lehetővé teszi bármely jellemzőit, amelyet tooremember hello adatok használata a jövőben hello hello adatokról.
5. jelölőnégyzet hello **hello új verziója, amelyet egy meglévő adatkészlet** lehetővé teszi a tooupdate egy meglévő adatkészlet az új adatokat. Ezt a jelölőnégyzetet, és írja be egy meglévő adatkészlet hello nevét.

![Töltse fel az új adatkészlet](media/machine-learning-import-data-from-local-file/upload-dataset.png)

Feltöltés közben megjelenik egy üzenet, hogy a fájl feltöltése van folyamatban. Töltse fel idő az adatok és hello sebessége a kapcsolat toohello szolgáltatás hello méretétől függ. Ha ismeri az hello fájl hosszú időt vesz igénybe, megteheti belül a Machine Learning Studio egyebek, várakozás közben. Azonban hello böngésző bezárása hello adatok feltöltése toofail okoz.

## <a name="dataset-module-is-ready-for-use"></a>A DataSet modul az használatra kész
Az adatok a feltöltést követően dataset modulban tárolt, és elérhető tooany kísérlet a munkaterületen.

A kísérlet szerkesztésekor a létrehozott hello hello adatkészletek található **saját adatkészletek** hello alatti listában **mentett adatkészletek** hello modulpalettán listájában. Húzza és a hello dataset hello kísérlet vászonra dobja el, ha azt szeretné, toouse hello dataset további elemzés és a gépi tanulás.
