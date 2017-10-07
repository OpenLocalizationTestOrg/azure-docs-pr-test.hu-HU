---
title: "egy Azure közzétett aaaDebugging felhőalapú szolgáltatás, a Visual Studio és az IntelliTrace |} Microsoft Docs"
description: "Ismerje meg, hogyan toodebug egy felhőalapú szolgáltatás, a Visual Studio és az IntelliTrace"
services: visual-studio-online
documentationcenter: n/a
author: kraigb
manager: ghogen
editor: 
ms.assetid: 5e6662fc-b917-43ea-bf2b-4f2fc3d213dc
ms.service: visual-studio-online
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/21/2017
ms.author: kraigb
ms.openlocfilehash: 60734a71d5499de72e1ca81a3ab0ab9f34bc303a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="debugging-a-published-azure-cloud-service-with-visual-studio-and-intellitrace"></a>A közzétett Azure-felhőszolgáltatásban a Visual Studio és az IntelliTrace-hibakeresés
Intellitrace bejelentkezhet a szerepkör példánya nagy mennyiségű hibakeresési adatok az Azure-ban futtatott. Ha a probléma okát toofind hello van szüksége, használhatja hello IntelliTrace-naplók toostep a kód a Visual Studio eszközből, mintha csak az Azure-ban futó. IntelliTrace kulcs kód végrehajtása és a környezet adatait rögzíti, amikor az Azure-alkalmazásokban az Azure-ban felhőszolgáltatásként fut, és lehetővé teszi, hogy érvényben, a Visual Studio eszközből hello rögzített adatok visszajátszásos. 

Ha rendelkezik telepített Visual Studio Enterprise IntelliTrace és az Azure-alkalmazásokban célok .NET-keretrendszer 4 vagy újabb verzió használható. IntelliTrace az Azure-szerepkörök adatokat gyűjt. Ezeket a szerepköröket mindig a 64 bites operációs rendszereket futtató virtuális gépek hello.

Alternatív megoldásként használható [távoli hibakeresés](http://go.microsoft.com/fwlink/p/?LinkId=623041) tooattach közvetlenül tooa felhőalapú szolgáltatás, hogy fut az Azure-ban.

> [!IMPORTANT]
> IntelliTrace csak hibakeresési forgatókönyvekhez készült, és nem használható éles környezet.
> 

## <a name="configure-an-azure-application-for-intellitrace"></a>Az Azure-alkalmazások konfigurálása az IntelliTrace
tooenable IntelliTrace Azure alkalmazás esetén létre kell hoznia és tegye közzé a Visual Studio Azure projektből hello alkalmazást. Közzététel előtt tooAzure IntelliTrace kell konfigurálása az Azure-alkalmazásában. Ha az alkalmazás közzétételére IntelliTrace beállítása nélkül, toorepublish hello projekt kell. További információkért lásd: [közzététele egy Azure cloud services projekteket a Visual Studio használatával](http://go.microsoft.com/fwlink/p/?LinkId=623012).

1. Amikor készen áll a toodeploy az Azure-alkalmazásában, győződjön meg arról, hogy túl van-e beállítva a project build célkitűzések**Debug**.

1. A **Megoldáskezelőben**, kattintson a jobb gombbal a projekt, és hello helyi menüből válassza ki a **közzététel**.
   
1. A hello **Azure-alkalmazás közzététele** párbeszédpanel, jelölje be hello Azure-előfizetéssel, válassza ki **következő**.

1. A hello **beállítások** lapra, jelölje be hello **speciális beállítások** fülre.

1. Kapcsolja be a hello **engedélyezése IntelliTrace** toocollect IntelliTrace-naplók az alkalmazás lehetőséget hello felhőben van közzétéve.
   
1. toocustomize hello IntelliTrace alapkonfiguráció, jelölje be **beállítások** következő túl**engedélyezése IntelliTrace**.

    ![IntelliTrace beállítások hivatkozására](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/intellitrace-settings-link.png)
   
1. A hello **IntelliTrace beállítások** párbeszédpanelen adhat meg, milyen események toolog, e toocollect hívja fel információt, mely modulok és a folyamatok toocollect naplóit, és mekkora terület tooallocate toohello rögzítése. Intellitrace alkalmazással kapcsolatban további információkért lásd: [intellitrace-hibakeresés](http://go.microsoft.com/fwlink/?LinkId=214468).
   
    ![IntelliTrace-beállítások](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC519063.png)

hello IntelliTrace-naplót hello megadott hello IntelliTrace beállítások (hello alapértelmezett mérete 250 MB) maximális méretének körkörös naplófájl. IntelliTrace-naplók összegyűjtött tooa fájl hello fájlrendszer hello virtuális gép. Hello naplók igénylésekor pillanatkép ezen a ponton az időben lesz végrehajtva, és letöltötte a helyi számítógép tooyour.

Azure-felhőszolgáltatásban hello közzétett tooAzure után azt is meghatározhatja, ha az IntelliTrace a hello Azure engedélyezett-e a csomópont **Server Explorer**, ahogy az a következő kép hello:

![Server Explorer - IntelliTrace engedélyezve](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC744134.png)

## <a name="download-intellitrace-logs-for-a-role-instance"></a>Töltse le az IntelliTrace-naplókat a szerepkörpéldányhoz
Visual Studio használatával, letöltheti a IntelliTrace-naplókat a szerepkör példánya a következő lépések végrehajtásával:

1. A **Server Explorer**, bontsa ki a hello **Felhőszolgáltatások** csomópont, és keresse meg a szerepkörpéldányt, amelynek a naplói toodownload kívánja. 

1. Kattintson a jobb gombbal a hello szerepkörpéldányt, és hello s helyi menüben válassza a **IntelliTrace-naplók megtekintése**. 

    ![IntelliTrace-naplók menüpont megjelenítése](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/view-intellitrace-logs.png)

1. hello IntelliTrace-naplók letöltött tooa fájl a könyvtárban található a helyi számítógépen. Minden alkalommal, amikor hello IntelliTrace-naplók kérnie egy új pillanatkép jön létre. Hello naplók letöltése folyamatban van, amíg a Visual Studio megjeleníti hello művelet előrehaladását hello hello **Azure tevékenységnapló** ablak. Ahogy az ábra a következő hello, bővítheti hello sor eleme hello művelet toosee további információkhoz juthat.

![VST_IntelliTraceDownloadProgress](./media/vs-azure-tools-intellitrace-debug-published-cloud-services/IC745551.png)

A Visual Studio toowork folytatható, amíg hello IntelliTrace-naplók letöltése. Amikor hello napló letöltése befejeződött, a Visual Studio nyílik meg.

> [!NOTE]
> hello IntelliTrace-naplók tartalmazhat kivételek adott hello keretrendszer hoz létre, és ezt követően kezeli. Belső keretrendszer kódot állít elő, ezeket a kivételeket, így biztonságosan figyelmen kívül hagyhatja őket egy szerepkör indítása normál részeként.
> 
> 

## <a name="next-steps"></a>Következő lépések
- [Azure felhőszolgáltatások hibakeresési lehetőségek](vs-azure-tools-debugging-cloud-services-overview.md)
- [Egy Visual Studio használatával Azure-felhőszolgáltatásban közzététele](vs-azure-tools-publishing-a-cloud-service.md)