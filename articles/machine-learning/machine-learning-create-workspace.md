---
title: "a Machine Learning-munkaterület aaaCreate |} Microsoft Docs"
description: "Hogyan toocreate az Azure Machine Learning Studio munkaterület"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: aa96b784-ac6c-44bc-a28a-85d49fbe90a2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye;bradsev;ahgyger
ms.openlocfilehash: 178293af222365993fade666124f34269d892325
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-share-an-azure-machine-learning-workspace"></a>Azure Machine Learning-munkaterület létrehozása és megosztása
Ebben a menüben be hello tooset különböző adatok tudományos környezetekben általi felhasználásáról hello Cortana Analytics folyamat (CAP-ok) leíró tootopics hivatkozásokat tartalmaz.

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Azure Machine Learning Studio toouse, toohave Machine Learning-munkaterület szükséges. A munkaterület toocreate kell hello eszközöket tartalmazza, kezelése és kísérletek közzététele.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

### <a name="toocreate-a-workspace"></a>a munkaterület toocreate
1. Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/)

    > [!NOTE]
    > a toosign és munkaterület létrehozása, az Azure-előfizetési rendszergazda toobe van szüksége. 
    >
    > 

2. Kattintson a **+ új**

3. Válassza ki **Eszközintelligencia + analitika**, kattintson a **Machine Learning-munkaterület**, majd kattintson a **létrehozása**

4. Adja meg a munkaterület adatokat

    - Hello *munkaterületnevet* too260 karakterből állhat, nem adhatja végződése fel lehet. hello neve nem tartalmazhatja a következő karaktereket:`< > * % & : \ ? + /`
    - Hello *web service-csomag* akkor válasszon (vagy hozzon létre), és társított hello *tarifacsomag* válassza ki, akkor használatos, ha a munkaterület webszolgáltatások telepítése.

    ![Új munkaterület létrehozása](media/machine-learning-create-workspace/create-new-workspace.png)

5. Kattintson a **Create** (Létrehozás) gombra

Miután hello munkaterület van telepítve, a Machine Learning Studióban indíthatja el.

1. Keresse meg a tooMachine Learning Studio: [https://studio.azureml.net/](https://studio.azureml.net/).

2. Válassza ki a munkaterület hello felső – jobb sarkában található.

    ![Munkaterület kiválasztása](media/machine-learning-create-workspace/open-workspace.png)

3. Kattintson a **saját kísérletek opcióra**.

    ![Nyissa meg benne](media/machine-learning-create-workspace/my-experiments.png)

A munkaterület kezelésével kapcsolatos információkért lásd: [kezelése az Azure Machine Learning-munkaterület](machine-learning-manage-workspace.md).
Ha a munkaterület létrehozását probléma adódik, tekintse meg [hibaelhárítási útmutatója: hozzon létre, és csatlakozzon a Machine Learning-munkaterület tooa](machine-learning-troubleshooting-creating-ml-workspace.md).


## <a name="sharing-an-azure-machine-learning-workspace"></a>Egy Azure Machine Learning munkaterülettel megosztása
A Machine Learning-munkaterület létrehozása után felajánlhatja a felhasználóknak tooyour munkaterület tooshare hozzáférés tooyour munkaterület és minden a kísérletek, adathalmazokat, notebookok stb. Hozzáadhat felhasználókat két szerepkör egyikében:

* **Felhasználói** -munkaterület felhasználó is létrehozása, nyissa meg a, módosítása és törlése kísérleteket, adatkészleteket, hello munkaterületen stb.
* **Tulajdonos** - tulajdonos kérhetnek és eltávolíthasson felhasználókat hello munkaterületén, a felhasználó hozzáadása toowhat teheti meg.

> [!NOTE]
> hello rendszergazdai fiók hello munkaterület létrehozó tulajdonos munkaterületként automatikusan kerül toohello munkaterület. Azonban más rendszergazdák vagy a felhasználók, előfizetés nem automatikusan hozzáféréssel toohello munkaterület - tooinvite kell azokat explicit módon.
> 
> 

### <a name="tooshare-a-workspace"></a>a munkaterület tooshare

1. Jelentkezzen be tooMachine Learning Studióba [https://studio.azureml.net/Home](https://studio.azureml.net/Home)

2. A hello bal oldali panelen kattintson a **beállítások**

3. Kattintson a hello **felhasználók** lap

4. Kattintson a **több felhasználók MEGHÍVÁSA** hello lap hello aljához

    ![Studio beállításai](media/machine-learning-create-workspace/settings.png)

5. Adjon meg egy vagy több e-mail címet. hello felhasználók kell egy érvényes Microsoft-fiókkal vagy szervezeti fiókkal (az Azure Active Directory).

6. Válassza ki, hogy tooadd hello felhasználók tulajdonosa vagy a felhasználó.

7. Kattintson a hello **OK** pipa gombra.

Minden felhasználóhoz hozzá lépéseit hogyan toosign toohello a megosztott munkaterület e-mailt fog kapni.

> [!NOTE]
> A felhasználók toobe képes toodeploy vagy a munkaterület webszolgáltatások kezelése, a közreműködői vagy hello Azure-előfizetés rendszergazdája kell lenniük. 



