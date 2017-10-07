---
title: "a Visual Studio egy Azure-felhőszolgáltatás-projekt aaaCreating |} Microsoft Docs"
description: "Ismerje meg, most toocreate a Visual Studio egy Azure-felhőszolgáltatás-projekt"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: ec580df7-3dcc-45a9-a1d9-8c110678dfb5
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 3c357016aa423688199a7ab3a670115e33a98fe9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="creating-an-azure-cloud-service-project-with-visual-studio"></a>Egy Azure-felhőszolgáltatás-projekt létrehozása a Visual Studio
hello Azure Tools for Visual Studio biztosít a projekt sablont, amely lehetővé teszi, hogy hozzon létre egy Azure felhőszolgáltatást. Hello projekt létrehozása után a Visual Studio lehetővé teszi a tooconfigure, hibakeresését és hello cloud service tooAzure telepítése.

## <a name="steps-toocreate-an-azure-cloud-service-project-in-visual-studio"></a>A Visual Studio egy Azure-felhőszolgáltatás-projekt lépéseket toocreate
Ez a szakasz végigvezeti az Azure-felhőszolgáltatás-projekt létrehozása a Visual Studióban legalább egy webes szerepkörök.  

1. Indítsa el a Visual Studio rendszergazdaként.

1. A hello fő menüben válassza **fájl** > **új** > **projekt**.

1. Válassza ki **felhő** hello Visual C# vagy a Visual Basic sablon csomópontok projektre, és válassza a **Azure Cloud Service** hello sablonok közül.

    ![Új Azure-felhőszolgáltatás](./media/vs-azure-tools-azure-project-create/new-project-wizard-for-cloud-service.png)

1. Adja meg, melyik hello a projekt toouse toodevelop kívánt .NET-keretrendszer verzióját.

1. Adjon nevet és a projekt helyét és hello megoldás nevét. 

1. Kattintson az **OK** gombra.

1. A hello **új Microsoft Azure Cloud Service** párbeszédpanelen jelölje be hello szerepkörök tooadd szeretné, és válassza ki a hello jobbra mutató nyíl gombra tooadd őket tooyour megoldás.

    ![Új Azure cloud service szerepkörök kiválasztása](./media/vs-azure-tools-azure-project-create/new-cloud-service.png)

1. szerepkör hozzáadott, a hello hello szerepkör rámutatáskor toorename **új Microsoft Azure Cloud Service** párbeszédpanel, és hello helyi menüből válassza ki a **átnevezése**. Egy szerepkör nevezze át a megoldás belül (a hello **Megoldáskezelőben**) után már hozzá lett adva.

    ![Azure cloud service szerepkör átnevezése](./media/vs-azure-tools-azure-project-create/new-cloud-service-rename.png)

hello Visual Studio Azure projekt társítások toohello projektek hello megoldásban rendelkezik. hello projekt is hello *szolgáltatásdefiníciós fájl* és *szolgáltatás konfigurációs fájlja*:

- **Szolgáltatásdefiníciós fájl** -hello futásidejű beállításait az alkalmazás, például milyen-szerepkörök szükségesek, a végpontok és a virtuális gép méretét határozza meg. 
- **Szolgáltatáskonfigurációs fájlt** -hány példánya egy futnak, és az egy szerepkörhöz definiált hello-beállítások értékeit hello konfigurálja. 

További információ ezen fájlokkal kapcsolatban, tekintse meg [hello szerepkörök az Azure-felhőszolgáltatás konfigurálása a Visual Studio](vs-azure-tools-configure-roles-for-cloud-service.md).

## <a name="next-steps"></a>Következő lépések
- [Az Azure cloud service projektek a Visual Studio szerepkörök kezelése](./vs-azure-tools-cloud-service-project-managing-roles.md)
