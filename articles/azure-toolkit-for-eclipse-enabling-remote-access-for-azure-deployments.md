---
title: "Távelérés engedélyezése az Azure-környezetekhez az eclipse-ben"
description: "Útmutató: a távelérés engedélyezése az Azure üzembe helyezése az Azure-eszközkészlet az eclipse-ben."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: b6150006-9a7f-4d83-be18-d35ec780c7c5
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 654d511bd5a62341f87569317e97360c94a6f26c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a>Távelérés engedélyezése az Azure-környezetekhez az eclipse-ben
Megoldhatja a központi telepítések, előfordulhat, hogy engedélyezése, és csatlakozzon a virtuális géphez, a központi telepítés üzemeltető távelérési segítségével. A távelérés funkciót a távoli asztal protokoll (RDP) a támaszkodik. Konfigurálja a távoli hozzáférést a központi telepítés után az Azure-bA rendelkezik közzétett, vagy Eclipse használatakor a Windows operációs rendszer konfigurálja a távoli hozzáférést az Azure-bA közzététele előtt. Vegye figyelembe, hogy szüksége lesz a távoli asztali ügyfél, amely kompatibilis az operációs rendszer a központi telepítés az Azure virtuális géphez való csatlakozás érdekében.

## <a name="how-to-enable-remote-access-before-you-deploy-to-azure"></a>Távoli hozzáférés engedélyezése előtt telepítse az Azure
> [!NOTE]
> Távelérés engedélyezése az Azure-bA az alkalmazás központi telepítése előtt, szüksége lehet az eclipse-ben futó Windows.
> 
> 

Az alábbi képen látható a **távelérési** távoli hozzáférésének engedélyezésére szolgáló tulajdonságai párbeszédpanelen.

![][ic719494]

Két módon való megjelenítése a **távelérési** tulajdonságainak párbeszédpanelét jeleníti meg:

* Kattintson a **speciális** hivatkozásra a **távelérési** szakasza a **Azure közzététel** párbeszédpanel.

* Nyissa meg a **tulajdonságok** a Azure projekt párbeszédpanel.

Amikor létrehoz egy új Azure-telepítés projektet, a projekt nem lesz alapértelmezés szerint engedélyezett távoli hozzáférési. Azonban könnyen engedélyezheti távoli hozzáférést a felhasználónév és jelszó megadásával a **Azure közzététel** párbeszédpanel. A távelérés jelszó titkosított X.509-tanúsítványokat használ. Ha nem adja meg a tanúsítvány, a titkosítást egy önaláírt tanúsítványt, az Azure Eclipse beépülő modul rendszerrel szállított támaszkodik. Az önaláírt tanúsítvány van a **cert** mappában található az Azure-projekt, egy nyilvános tanúsítványfájlt (SampleRemoteAccessPublic.cer) mind a személyes információcseréhez kapcsolódó (PFX) Tanúsítványfájl (SampleRemoteAccessPrivate.pfx) tárolja. Ez utóbbi tartalmazza a tanúsítvány titkos kulcsát, és nem rendelkezik, egy alapértelmezett jelszó **jelszó1**. Azonban mivel a jelszó nyilvános Tudásbázis, az alapértelmezett tanúsítvány használható csak tanulási célokra, nem az éles környezet. Így eltérő megismerésére céljából, ha szeretné engedélyezni a központi telepítés távoli munkamenetek kattintson a **speciális** hivatkozásra a **Azure közzététel** párbeszédpanelen adja meg a saját tanúsítványt. Vegye figyelembe, hogy szeretné-e a tanúsítvány PFX verziója feltölteni az Azure felügyeleti portálon, az üzemeltetett szolgáltatás úgy, hogy Azure vissza tudja fejteni a felhasználó jelszavát.

A hátralévő részét az oktatóanyag bemutatja, hogyan engedélyezze a távoli hozzáférést egy Azure-telepítés projekt, amely eredetileg hoztak létre a távoli hozzáférés le van tiltva. Ebben az oktatóanyagban létre fogunk hozni egy új önaláírt tanúsítványt, és a .pfx-fájlt egy tetszőleges jelszót kell. Akkor is használhatja a hitelesítésszolgáltató által kiállított tanúsítványt.

## <a name="how-to-enable-remote-access-after-you-have-deployed-to-azure"></a>Távoli hozzáférés engedélyezése, miután telepítette az Azure-bA
Engedélyezze a távelérést, miután telepítette az Azure-ba, használja az alábbi lépéseket:

1. Jelentkezzen be az Azure felügyeleti portálra, az Azure-fiókjával

2. A listájában **Felhőszolgáltatások**, válassza ki a központilag telepített a felhőalapú szolgáltatás

3. A felhőalapú szolgáltatás weblapon kattintson a **konfigurálása** hivatkozás

4. A lap alján, kattintson a **távoli** hivatkozás

5. A felugró párbeszédpanel megjelenésekor:
   
   * Adja meg a legyen az engedélyezi a távoli hozzáférést

   * Kattintással jelölje ki a **távoli asztal engedélyezése** jelölőnégyzet
   
   * Adjon meg egy felhasználónevet és a távoli hozzáféréshez használni kívánt jelszót
   
   * Válassza ki a használni kívánt tanúsítványt

6. Kattintson az **OK** gombra 

Látni fogja üzenet jelenik meg, hogy a konfiguráció módosítását van folyamatban, néhány percet is igénybe vehet. A konfigurációs módosítás befejezése után kövesse a lépéseket a **távoli bejelentkezési** szakasz a cikk későbbi részében.

## <a name="how-to-enable-remote-access-in-your-package"></a>A csomagban lévő távoli hozzáférés engedélyezése
1. Belül meg Eclipse Project Explorer ablaktáblában kattintson a jobb gombbal az Azure-projekt, és kattintson a **tulajdonságok**.

2. Az a **tulajdonságok** párbeszédpanelen bontsa ki a **Azure** a bal oldali ablaktáblán, majd kattintson a **távelérési**.

3. Az a **távelérési** párbeszédpanelen győződjön meg arról **engedélyezése a távoli asztali kapcsolatok fogadására a bejelentkezési hitelesítő adatokat az összes szerepkör** be van jelölve.

4. Adjon meg egy felhasználónevet, a távoli asztali kapcsolat.

5. Adja meg, és erősítse meg a felhasználó jelszavát. Ezen a párbeszédpanelen beállított felhasználó nevét és jelszavát értékeket fogja használni, amikor a távoli asztali kapcsolat. (Vegye figyelembe, hogy ez a PFX-jelszót külön jelszó.)

6. Adja meg a felhasználói fiók lejárati dátumát.

7. Kattintson a **új** egy új önaláírt tanúsítvány létrehozásához. (Másik lehetőségként a munkaterület vagy a fájl rendszerről keresztül kiválaszthatja egy tanúsítványt a **munkaterület** vagy **fájlrendszer** gomb, illetve, de célokra, ebben az oktatóanyagban létre fogunk hozni egy új tanúsítványt.)

   * Az a **új tanúsítvány** párbeszédpanelen adja meg, és erősítse meg a jelszót a PFX-fájljának fogja használni.

   * Fogadja el a megadott érték **név (CN)**, vagy használjon egy egyéni nevet.

   * Adja meg az új tanúsítvány .cer formában menteni elérési útját és nevét. Ezt a lépést, és a következő lépés, használhatja a **cert** mappában található az Azure-projekt, de szabadon válasszon másik helyet. Ez az oktatóanyag céljából, ezen **c:\mycert\mycert.cer**. (Létrehozása a **c:\mycert** mappa eljárás, vagy használjon egy létező mappát, ha szükséges.)

   * Adja meg a elérési útját és nevét, ahová az új tanúsítványt és annak titkos kulcsát, .pfx formátumban menti. Ez az oktatóanyag céljából, ezen **c:\mycert\mycert.pfx**. A **új tanúsítvány** párbeszédpanelen a következőhöz hasonlóan kell kinéznie (frissítse a mappák elérési útjában, ha nem használja **c:\mycert**):
     
      ![][ic712275]

   * Kattintson a **OK** bezárásához a **új tanúsítvány** párbeszédpanel.

8. A **távelérési** párbeszédpanelen a következőhöz hasonlóan kell kinéznie:</p>
   
   ![][ic719495]

9. Kattintson a **OK** bezárásához a **távelérési** párbeszédpanel.

Az alkalmazás építenie a központi telepítés felhőbe összeállítása.

## <a name="to-log-in-remotely"></a>A távoli bejelentkezéshez
Ha készen áll a szerepkör példánya, távolról bejelentkezhet a virtuális géphez, amelyen az alkalmazást.

* Ha az eclipse-ben a Windows és a kiválasztott használ a **Start távoli asztal a telepítése** lehetőséget az Azure-ba való üzembe helyezés során választhat egy távoli asztali kapcsolat bejelentkezési képernyőt, ha a telepítés megkezdése. Amikor a felhasználónevet és jelszót kéri, adja meg a távoli felhasználó számára megadott értékeket, és fog tudni jelentkezni.

* A távoli bejelentkezéshez egy másik lehetőség az <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure felügyeleti portálon</a>:
  
  * Belül a **Felhőszolgáltatások** az Azure felügyeleti portál, kattintson a felhőalapú szolgáltatás megtekintéséhez kattintson **példányok**, kattintson egy adott példányt, majd a **Connect** gombra. A **Connect** gombja megjelenik ugyan a parancssávon az alábbiak szerint:
    
      ![][ic659273]

  * Miután rákattintott a **Connect** gomb kérni fogja az RDP-fájl megnyitásához. Nyissa meg a fájlt, és kövesse az utasításokat. (Ön sikerült is mentse a fájlt a helyi számítógépen, és futtassa a fájl kattintson duplán a virtuális géphez távoli bejelentkezési anélkül, hogy nyissa meg a felügyeleti portálon.)

  * Amikor a felhasználónevet és jelszót kéri, adja meg a távoli felhasználó számára megadott értékeket, és fog tudni jelentkezni.

> [!NOTE]
> Ha nem Windows operációs rendszeren, szüksége egy távoli asztali ügyfél, amely kompatibilis az operációs rendszer, és kövesse a lépéseket, hogy az ügyfél konfigurálása az RDP-fájlban letöltött beállításokkal.
> 
> 

## <a name="see-also"></a>Lásd még:
[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]

[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]

[Az eclipse-ben az Azure eszközkészlet telepítése][Installing the Azure Toolkit for Eclipse] 

Azure Java használatával kapcsolatos további információkért lásd: a [Azure Java fejlesztői központból][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing the Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->
