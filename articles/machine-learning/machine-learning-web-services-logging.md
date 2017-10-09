---
title: "a Machine Learning webszolgáltatások aaaLogging |} Microsoft Docs"
description: "Megtudhatja, hogyan tooenable naplózási Machine Learning webszolgáltatások. Naplózási toohelp hibaelhárítása hello API-k további információt tartalmaz."
services: machine-learning
documentationcenter: 
author: raymondlaghaeian
manager: jhubbard
editor: cgronlun
ms.assetid: c54d41e1-0300-46ef-bbfc-d6f7dca85086
ms.service: machine-learning
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/15/2017
ms.author: raymondl;garye
ms.openlocfilehash: ed23933d52d2151af658af2307d7df8743071f65
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enable-logging-for-machine-learning-web-services"></a>A naplózás engedélyezése a Machine Learning webszolgáltatásokhoz
Ez a dokumentum információkat biztosít a naplózási képesség Machine Learning webszolgáltatások hello. Naplózási nyújt további információkat, csak olyan hiba száma és egy üzenet, a hívások toohello Machine Learning API-k hibaelhárításához nyújt segítséget.  

## <a name="how-tooenable-logging-for-a-web-service"></a>Hogyan tooenable naplózási az adott webszolgáltatás

Engedélyezi a naplózást a hello [Azure Machine Learning webszolgáltatások](https://services.azureml.net) portálon. 

1. Jelentkezzen be Azure Machine Learning webszolgáltatások portálon toohello, [https://services.azureml.net](https://services.azureml.net). A klasszikus webszolgáltatáshoz is megkapható toohello portal kattintva **új webes felhasználói élményt** hello Machine Learning webszolgáltatások oldalon a Machine Learning Studióban.

   ![Új webes felhasználói élményt hivatkozás](media/machine-learning-web-services-logging/new-web-services-experience-link.png)

2. Hello felső menüsávban kattintson **webszolgáltatások** egy új a webes szolgáltatás, vagy kattintson a **klasszikus webszolgáltatások** klasszikus webszolgáltatás.

   ![Válassza ki az új vagy a klasszikus webszolgáltatások](media/machine-learning-web-services-logging/select-web-service.png)

3. Új webszolgáltatás kattintson a hello webszolgáltatás nevét. Az adott klasszikus webszolgáltatás kattintson hello webszolgáltatás neve majd a következő oldalon hello hello megfelelő végpont.

4. Hello felső menüsávban kattintson **konfigurálása**.

5. Set hello **naplózás engedélyezése** beállítás túl*hiba* (toolog csak a hibák) vagy *összes* (a teljes körű naplózás).

   ![Válassza ki a naplózási szint](media/machine-learning-web-services-logging/enable-logging.png)

6. Kattintson a **Save** (Mentés) gombra.

7. Klasszikus webszolgáltatások, hozza létre a hello **ml-diagnosztika** tároló.

   Minden web service naplóit őrizze meg a következő nevű blobtárolóban **ml-diagnosztika** hello webszolgáltatás társított hello tárfiókban. Az új webszolgáltatások, a tároló jön létre hello hello webszolgáltatás első megnyitásakor. A klasszikus webszolgáltatások kell toocreate hello tároló Ha még nem létezik. 

   1. A hello [Azure-portálon](https://portal.azure.com)lépjen hello webszolgáltatás társított toohello tárfiók.

   2. A **Blob szolgáltatás**, kattintson a **tárolók**.

   3. Ha hello tároló **ml-diagnosztika** nem létezik, kattintson a **+ tároló**, hello tároló hello neve "ml-diagnosztika" ad, és válassza ki a hello **hozzáférési típus** mint "Blob". Kattintson az **OK** gombra.

      ![Válassza ki a naplózási szint](media/machine-learning-web-services-logging/create-ml-diagnostics-container.png)

> [!TIP]
>
> Az adott klasszikus webszolgáltatás hello Web Services irányítópult a Machine Learning Studióban egy kapcsoló tooenable naplózás is rendelkezik. Azonban mivel naplózási most már felügyelt hello Web Services portálon keresztül, meg kell tooenable naplózás hello portálon ebben a cikkben leírtak szerint. Ha már engedélyezte Studio van bejelentkezve, hello Web Services portálra, tiltsa le a naplózást, és engedélyezze újra.


## <a name="hello-effects-of-enabling-logging"></a>hello hatásait naplózás engedélyezése
Ha naplózása engedélyezve van, hello diagnosztika és hello webszolgáltatási végpontot hibákat naplózza hello **ml-diagnosztika** hello Azure-Tárfiók a blob-tároló hello felhasználói munkaterület kapcsolódik. Ez a tároló összes hello webszolgáltatás végpontjainak tároló fiókhoz társított összes hello munkaterületek tartozó összes hello diagnosztikai adatokat tartalmazza.

hello naplók bármely hello több eszközök elérhető tooexplore egy Azure Storage-fiók használatával tekintheti meg. hello legegyszerűbb előfordulhat, hogy toonavigate toohello tárfiók hello Azure-portálon kell, kattintson a **tárolók**, majd kattintson a hello tároló **ml-diagnosztika**.  

## <a name="log-blob-detail-information"></a>Naplóadatok blob részletei
Minden egyes blob hello tárolóban hello diagnosztikai adatokat a hello a következő műveletek egyikét tartalmazza:

* Egy hello Batch-végrehajtási mód végrehajtását  
* Egy kérés-válasz hello metódus végrehajtása  
* A kérés-válasz tároló inicializálása

minden egyes blob hello nevét a következő űrlap hello előtaggal van: 


`{Workspace Id}-{Web service Id}-{Endpoint Id}/{Log type}`


Ha _típusú naplózása_ hello a következő értékek egyike:  

* Kötegelt  
* pontszám/kérelem  
* pontszám/init  

