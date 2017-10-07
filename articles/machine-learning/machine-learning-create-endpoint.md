---
title: "aaaCreating webszolgáltatás-végpontok a Machine Learning |} Microsoft Docs"
description: "Az Azure Machine Learning webszolgáltatás-végpontok létrehozása"
services: machine-learning
documentationcenter: 
author: hiteshmadan
manager: padou
editor: cgronlun
ms.assetid: 4657fc1b-5228-4950-a29e-bc709259f728
ms.service: machine-learning
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: tbd
ms.date: 10/04/2016
ms.author: himad
ms.openlocfilehash: 10a2bc586c6fe35e28d8bf0293854c578827c453
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-endpoints"></a>Végpontok létrehozása
> [!NOTE]
>  Ez a témakör ismerteti a technikák alkalmazható tooa **klasszikus** Machine Learning webszolgáltatáshoz.
> 
> 

Webalapú alkalmazásokhoz, előre tooyour ügyfelek értékesít létrehozásakor kell tooprovide betanított modellek tooeach ügyfelek, amelyek továbbra is csatolt toohello kísérlet mely hello webes szolgáltatás létrehozásához. Ezenkívül toohello kísérlet kell frissítéseket alkalmazza a szelektív tooan végpont hello testreszabások felülírása nélkül.

tooaccomplish az Azure Machine Learning toocreate lehetővé teszi az adott üzembe helyezett webszolgáltatás több végpontot. A webszolgáltatás hello végpontok egymástól függetlenül címzett, szabályozva, és felügyelt. Minden egyes végpont tooyour ügyfelek terjeszthető egyedi URL-cím és a hitelesítési kulcs esetében.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="adding-endpoints-tooa-web-service"></a>Végpontok tooa webszolgáltatás hozzáadása
Nincsenek három módon tooadd egy végpont tooa webszolgáltatáshoz.

* Automatizáltan
* Hello Azure Machine Learning webszolgáltatások portálon keresztül
* Ha a klasszikus Azure portálon hello

Hello végpont létrehozását követően szokásokra is szinkron API-k, kötegelt API-k, segítségével, és az excel-munkalapokat. Továbbá a felhasználói felületen keresztüli végpontokra tooadding, használhatja hello végpont felügyeleti API-k tooprogrammatically végpont-hozzáadáshoz.

> [!NOTE]
> Ha további végpontok toohello webszolgáltatás hozzáadott, hello alapértelmezett végpont nem törölhető.
> 
> 

## <a name="adding-an-endpoint-programmatically"></a>A végpont programozott módon hozzáadása
Hozzáadhat egy végpont tooyour webszolgáltatás programozott módon hello [AddEndpoint](https://github.com/raymondlaghaeian/AML_EndpointMgmt/blob/master/Program.cs) példakód.

## <a name="adding-an-endpoint-using-hello-azure-machine-learning-web-services-portal"></a>Hello Azure Machine Learning webszolgáltatások portálon végpont hozzáadása
1. A Machine Learning Studio hello bal oldali oszlopban kattintson a webszolgáltatásokat.
2. Hello Web service irányítópult hello alul kattintson **végpontok kezelése**. hello Azure Machine Learning webszolgáltatások portál megnyitja a webszolgáltatás hello toohello végpontok oldalon.
3. Kattintson az **Új** lehetőségre.
4. Írja be nevét és leírását hello új végpont. Végpontneveit 24 karakter vagy kevesebb hosszúságúnak kell lennie, és a magyar ábécét kisbetűket és számokat kell készíteni. Válassza ki a hello naplózási szint, és hogy engedélyezve van-e mintaadatokat. További információt a naplózást, [naplózását a Machine Learning webszolgáltatások](machine-learning-web-services-logging.md).

## <a name="adding-an-endpoint-using-hello-azure-classic-portal"></a>A klasszikus Azure portálon hello segítségével a végpont hozzáadása
1. Jelentkezzen be toohello [a klasszikus Azure portálon](http://manage.windowsazure.com), kattintson a **Machine Learning** hello bal oldali oszlopban. Kattintson a hello webes szolgáltatás, amelyben érdekli tartalmazó hello munkaterületen.
   
    ![Keresse meg a tooworkspace](./media/machine-learning-create-endpoint/figure-1.png)
2. Kattintson a **webszolgáltatások**.
   
    ![Keresse meg a tooWeb szolgáltatások](./media/machine-learning-create-endpoint/figure-2.png)
3. Kattintson az elérhető végpontok listáját toosee hello kíváncsiak vagyunk hello webszolgáltatáshoz.
   
    ![Keresse meg a tooendpoint](./media/machine-learning-create-endpoint/figure-3.png)
4. Hello a hello lap alján, kattintson **végpont hozzáadása**. Írja be a nevet és leírást, ellenőrizze, hogy nincsenek az azonos név ennek a webszolgáltatásnak a hello nincsenek végpontok. Amennyiben nincsenek különleges követelmények hello késleltetési szintjét hagyja az alapértelmezett értékre. toolearn szabályozással kapcsolatos további információkért lásd: [API-végpontok méretezése](machine-learning-scaling-webservice.md).
   
    ![-Végpont létrehozása](./media/machine-learning-create-endpoint/figure-4.png)

## <a name="next-steps"></a>Következő lépések
[Hogyan tooconsume az Azure Machine Learning Web service](machine-learning-consume-web-services.md).

