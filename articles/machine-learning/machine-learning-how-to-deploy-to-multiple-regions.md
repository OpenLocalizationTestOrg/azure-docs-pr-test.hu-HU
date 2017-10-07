---
title: "egy webszolgáltatás-bővítmény aaaHow toodeploy toomultiple régiók |} Microsoft Docs"
description: "Lépéseket toodeploy (Másolás) egy új webszolgáltatás tooother régiók."
services: machine-learning
documentationcenter: 
author: vDonGlover
manager: raymondl
editor: cgronlun
ms.assetid: 36c60411-f2db-4ee2-9b66-b1f1d77a8f44
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/19/2017
ms.author: v-donglo
ms.openlocfilehash: 21fcdb96f118c60ed98b60b1b2df833766c7c8bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="how-toodeploy-a-web-service-toomultiple-regions"></a>Hogyan toodeploy egy webszolgáltatás-bővítmény toomultiple régiók
Új Azure Web Services hello lehetővé teszik tooeasily telepítése egy webes szolgáltatás toomultiple régiók több előfizetések vagy munkaterületek anélkül. 

Tarifacsomag az régió adott, ezért meg kell adnia mindegyik régióhoz fog üzembe helyezésének hello webszolgáltatás egy számlázási csomagot.

## <a name="toocreate-a-plan-in-another-region"></a>a csomag egy másik régióban toocreate
1. Jelentkezzen be a [Microsoft Azure Machine Learning webszolgáltatások](https://services.azureml.net/).
2. Kattintson a hello **tervek** menüjét.
3. Hello csomagok keresztül nézet lap, kattintson a **új**.
4. A hello **előfizetés** legördülő menüben válassza hello előfizetés mely hello új terv legyen elhelyezve.
5. A hello **régió** legördülő menüben válasszon ki egy régiót hello új csomag. hello beállítások megtervezése hello kijelölt terület jelennek majd meg hello **beállítások megtervezése** hello lap szakasza.
6. A hello **erőforráscsoport** legördülő menüben válassza a erőforráscsoport hello terv. További információ az erőforráscsoportokkal látható [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).
7. A **neve** hello terv hello típusnév.
8. A **terv beállítások**, hello számlázási szint hello új csomag gombra.
9. Kattintson a **Create** (Létrehozás) gombra.

## <a name="deploying-hello-web-service-tooanother-region"></a>Hello web service tooanother régióban üzembe helyezése
1. Kattintson a hello **webszolgáltatások** menüjét.
2. Válassza ki a hello webszolgáltatás tooa új régió telepít.
3. Kattintson a **másolási**.
4. A **webszolgáltatás neve**, adjon meg egy új nevet hello webszolgáltatáshoz.
5. A **szolgáltatásleírás webes**, hello webszolgáltatás leírását.
6. A hello **előfizetés** legördülő menüben válassza hello előfizetés mely hello új webszolgáltatás legyen elhelyezve.
7. A hello **erőforráscsoport** legördülő menüben válassza a erőforráscsoport hello webszolgáltatáshoz. További információ az erőforráscsoportokkal látható [Azure Resource Manager áttekintése](../azure-resource-manager/resource-group-overview.md).
8. A hello **régió** legördülő menüben válassza hello régióban mely toodeploy hello webszolgáltatás.
9. A hello **tárfiók** legördülő menüben válassza a tárfiókokat mely toostore hello webes szolgáltatás.
10. A hello **ár terv** legördülő menüben válasszon ki egy tervet a 8. lépésben kiválasztott hello régióban.
11. Kattintson a **másolási**.

