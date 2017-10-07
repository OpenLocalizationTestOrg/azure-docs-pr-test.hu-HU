---
title: "Hibáinak elhárítása: Hozzon létre, és csatlakozzon a Machine Learning-munkaterület tooa |} Microsoft Docs"
description: "A gyakori problémákat hoz létre és tooan Azure Machine Learning-munkaterület csatlakoztatja a megoldások"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 1a8aec4b-35f9-44e8-9570-2575b8979ab1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: 965a0025e85ba4e22c2b037edfa923e7f7599069
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-create-and-connect-tooan-machine-learning-workspace"></a>Hibaelhárítási útmutató: hozzon létre, és csatlakozzon a Machine Learning-munkaterület tooan
Ez az útmutató egyes megoldások gyakran ütközött kihívást állítja be az Azure Machine Learning munkaterületek.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a>Munkaterület tulajdonosa
a Machine Learning Studio munkaterületének tooopen, kell lennie toohello Microsoft Account bejelentkezve toocreate hello munkaterület használt, vagy tooreceive hello tulajdonos toojoin hello munkaterületről meghívót van szüksége. Hello Azure-portálon kezelheti tartoznak a hello képességét tooconfigure hello munkaterületen.

A munkaterület kezeléséről további információkért lásd: [kezelése az Azure Machine Learning-munkaterület].

[kezelése az Azure Machine Learning-munkaterület]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a>Engedélyezett régiók
Gépi tanulás már régiók korlátozott számú érhető el. Az előfizetés nem tartalmazza a régiók egyikéhez sem, ha lehetséges, hogy hello hibaüzenet jelenik meg, "Nincs előfizetése van az engedélyezett régiók hello."

amely egy régiót kell toorequest hozzáadott tooyour előfizetés, egy új Microsoft-támogatási kérelem létrehozása hello Azure-portálon, majd a **számlázási** hello probléma típusa, és hajtsa végre hello kéri toosubmit a kérést.

## <a name="storage-account"></a>Tárfiók
hello Machine Learning szolgáltatás a tárolási fiók toostore adatokra van szüksége. Meglévő tárfiókot is használhatja, vagy létrehozhat egy új tárfiókot, (ha kvóta toocreate új tárfiók) hello új Machine Learning munkaterületének létrehozásakor.

Hello új Machine Learning-munkaterület létrehozása után által használt toocreate hello munkaterület hello Microsoft-fiókjával bejelentkezhet tooMachine Learning Studióban. Ha hello hibaüzenet jelenik meg, "Munkaterület nem található" (hasonló toohello a következő képernyőkép), használja a következő lépéseket toodelete hello a böngésző cookie-kat.

![Nem található munkaterület][screen3]

**toodelete böngésző cookie-k**

1. Ha az Internet Explorer, kattintson a hello **eszközök** hello jobb felső sarkában található gombra, majd az **Internetbeállítások**.  

![Internetbeállítások][screen4]

2. A hello **általános** lapra, majd **törlése...**

![Általános lap][screen5]

3. A hello **böngészési előzmények törlése** párbeszédpanelen győződjön meg arról, hogy **cookie-k és a webhely adatok** van kiválasztva, és kattintson a **törlése**.

![Törölje a cookie-k][screen6]

Miután hello cookie-k törlődnek, indítsa újra hello böngészőt, és folytassa a toohello [Microsoft Azure Machine Learning](https://studio.azureml.net) lap. Amikor a rendszer kéri a felhasználónevet és jelszót, írja be a hello toocreate hello munkaterület használt Microsoft-fiók.

## <a name="comments"></a>Megjegyzések

Célunk toomake hello Machine Learning élmény, a lehető zökkenőmentes. Tegye a megjegyzések és a problémák, hello [Azure Machine Learning fórum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) velünk kiszolgálására, jobb toohelp.

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
