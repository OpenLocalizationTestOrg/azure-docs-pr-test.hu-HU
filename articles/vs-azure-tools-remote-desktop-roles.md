---
title: "Távoli asztal Azure szerepkörök aaaUsing |} Microsoft Docs"
description: "A távoli asztal használata Azure szerepkörök"
services: visual-studio-online
documentationcenter: na
author: kraigb
manager: ghogen
editor: 
ms.assetid: f5727ebe-9f57-4d7d-aff1-58761e8de8c1
ms.service: multiple
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/11/2016
ms.author: kraigb
ms.openlocfilehash: d35fd421cde8be9e3caa474db95974a54e528bae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="using-remote-desktop-with-azure-roles"></a>A távoli asztal használata Azure szerepkörök
Hello Azure SDK-t és a távoli asztali szolgáltatások segítségével érheti el az Azure szerepkörök és az Azure által üzemeltetett virtuális gépeket. A Visual Studio a távoli asztali szolgáltatások egy Azure-felhőszolgáltatás-projekt t konfigurálhatja. tooenable a távoli asztali szolgáltatások, létre kell hoznia egy működő projekt, amely tartalmaz egy vagy több szerepkört és tooAzure közzététele.

> [!IMPORTANT]
> Hibaelhárítás vagy fejlesztői csak egy Azure szerepkörét hozzá kell férnie. minden virtuális gép céljának hello toorun egy adott szerepkör az Azure-alkalmazás nem toorun más olyan ügyfélalkalmazásokhoz van. Ha azt szeretné, hogy egy virtuális gép bármilyen célra használható Azure toohost toouse, tekintse meg az Azure virtuális gépek használata a Server Explorer.
> 
> 

## <a name="tooenable-and-use-remote-desktop-for-an-azure-role"></a>tooenable és a távoli asztal használata az Azure-szerepkör
1. A Solution Explorerben nyissa meg a felhőszolgáltatás-projekt helyi menüjének hello, és válassza **közzététel**.
   
    Hello **Azure-alkalmazás közzététele** varázsló jelenik meg.
   
    ![A parancs egy Felhőszolgáltatás-projekt közzététele](./media/vs-azure-tools-remote-desktop-roles/IC799161.png)
2. Hello aljához **Microsoft Azure közzétételi beállítási** hello varázsló, jelölje be hello **távoli asztal engedélyezése** az összes szerepkörök jelölőnégyzetet. 
   
    Hello **a távoli asztal konfigurálásának** párbeszédpanel jelenik meg.
3. Hello hello alján **a távoli asztal konfigurálásának** párbeszédpanelen válassza ki a hello **további beállítások** gombra. 
   
    Megjelenik egy legördülő lista, amely lehetővé teszi, hogy hozzon létre vagy válasszon egy tanúsítványt, így amikor a távoli asztali kapcsolatra hitelesítő adatok titkosíthatók.
4. Hello legördülő listában válassza ki a  **&lt;létrehozása >**, vagy válasszon egy meglévő hello listából. 
   
    Ha úgy dönt, hogy a meglévő tanúsítványt, hagyja ki a lépéseket követve hello.
   
   > [!NOTE]
   > hello tanúsítványokat, amelyekre szüksége van a távoli asztali kapcsolat eltérnek a többi Azure művelet használó hello tanúsítványokat. hello távelérési tanúsítványnak rendelkeznie kell egy titkos kulccsal.
   > 
   > 
   
    Hello **tanúsítvány létrehozása** párbeszédpanel jelenik meg.
   
   1. Adjon meg egy rövid nevet a hello új tanúsítványt, és válassza a hello **OK** gombra. Új tanúsítvány hello hello legördülő lista megjelenik.
   2. A hello **a távoli asztal konfigurálásának** párbeszédpanelen adja meg egy felhasználónevet és jelszót.
      
       Nem használhat egy meglévő fiókkal. Ne adjon meg rendszergazdai felhasználónév hello hello új fiókot.
      
      > [!NOTE]
      > Ha hello jelszó nem felel meg a hello összetettségi követelményeknek, piros ikon látható a következő toohello mezőbe. hello jelszónak kell tartalmaznia, nagybetűk, kisbetűk, és a számok vagy szimbólumok.
      > 
      > 
   3. Válassza ki a dátum mely hello fiók lejár és után, amely távoli asztali kapcsolatok le lesz tiltva.
   4. Miután megismerte a megadott összes hello szükséges adatokat, válassza ki azt a hello **OK** gombra.
      
       Számos beállítás, amelyek lehetővé teszik a távelérési szolgáltatások toohello .cscfg és az .csdef fájlok kerülnek.
5. A hello **Microsoft Azure közzétételi beállítási** varázsló, válassza ki a hello **OK** gombra kattint, ha már kész toopublish a felhőalapú szolgáltatás.
   
    Ha még nem áll készen toopublish, válassza ki a hello **Mégse** gombra. hello konfigurációs beállítások lesznek mentve, és később közzététele a felhőalapú szolgáltatás.

## <a name="connect-tooan-azure-role-by-using-remote-desktop"></a>A távoli asztal használatával kapcsolódnak a tooan Azure szerepkör
Miutá közzéteszi az Azure felhőalapú szolgáltatás, használható Server Explorer toolog hello virtuális gépekké, amely az Azure futtatja. 

1. A Server Explorer eszközben bontsa ki a hello **Azure** csomópontot, majd bontsa ki a hello csomópont egy felhőalapú szolgáltatás, és egyet a szerepkörök toodisplay példányok listáját.
2. Nyissa meg a helyi menü hello egy példány csomópont, és válassza **csatlakozzon a távoli asztal**.
   
    ![Távoli asztali kapcsolatra](./media/vs-azure-tools-remote-desktop-roles/IC799162.png)
3. Adja meg a hello felhasználónevet és jelszót, amelyet korábban hozott létre. Most jelentkezett be a távoli munkamenethez.

