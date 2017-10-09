---
title: "Azure-telepítésekre az eclipse-ben a távelérés aaaEnabling"
description: "Ismerje meg, hogyan tooenable távelérés az Azure-környezetekhez hello Azure eszközkészlet használata az eclipse-ben."
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
ms.openlocfilehash: 00c2bf22c1f3ec792098f154f771c87506e87881
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="enabling-remote-access-for-azure-deployments-in-eclipse"></a>Távelérés engedélyezése az Azure-környezetekhez az eclipse-ben
toohelp hibaelhárítása a központi telepítések, előfordulhat, hogy engedélyezi, és használja a távelérés tooconnect toohello virtuálisgép üzemeltetési a környezethez. Távelérés funkciót hello hello Remote Desktop Protocol (RDP) alapul. Konfigurálja a távoli hozzáférést az üzembe helyezéshez tooAzure van közzétéve, vagy ha az eclipse-ben a Windows operációs rendszert használ, konfigurálja a távoli hozzáférést tooAzure közzététele előtt után. Vegye figyelembe, hogy szüksége lesz a távoli asztali ügyfelek kompatibilis az operációs rendszer rendelés tooconnect tooyour telepítése virtuális gépre az Azure-ban.

## <a name="how-tooenable-remote-access-before-you-deploy-tooazure"></a>Hogyan telepítsék központilag az tooenable távelérési előtt tooAzure
> [!NOTE]
> tooenable távoli elérés az alkalmazás tooAzure központi telepítése előtt, meg kell eclipse-ben futó Windows toobe.
> 
> 

hello következő kép bemutatja hello **távelérési** tulajdonságai párbeszédpanelen tooenable távelérési használt.

![][ic719494]

Nincsenek a két módon toodisplay hello **távelérési** tulajdonságainak párbeszédpanelét jeleníti meg:

* Kattintson a hello **speciális** hello hivatkozásra **távelérési** hello szakasza **tooAzure közzététele** párbeszédpanel.

* Nyissa meg hello **tulajdonságok** a Azure projekt párbeszédpanel.

Amikor létrehoz egy új Azure-telepítés projektet, hello projekt nem lesz alapértelmezés szerint engedélyezett távoli hozzáférési. Azonban, könnyen engedélyezheti távelérési hello hello felhasználónév és jelszó megadásával **tooAzure közzététele** párbeszédpanel. hello távelérési jelszó titkosított X.509-tanúsítványokat használ. Ha nem adja meg a tanúsítvány, hello titkosítási támaszkodik hello Azure Eclipse beépülő modul rendszerrel szállított önaláírt tanúsítványt. Az önaláírt tanúsítvány van hello **cert** az Azure-projekt mappájában tárolt mindkét nyilvános tanúsítvány fájlként (SampleRemoteAccessPublic.cer) és a, a személyes információcseréhez kapcsolódó (PFX) tanúsítványa () SampleRemoteAccessPrivate.pfx). Ez utóbbi hello hello hello tanúsítvány titkos kulcsa tartalmazza, és nem rendelkezik, egy alapértelmezett jelszó **jelszó1**. Azonban mivel ezt a jelszót nyilvános Tudásbázis, hello alapértelmezett tanúsítványt kell használni csak tanulási célokra, nem az éles környezet. Így eltérő megismerésére céljából, ha azt szeretné, hogy tooenabled távoli munkamenetek a központi telepítése esetén kattintson hello **speciális** hello hivatkozásra **tooAzure közzététele** párbeszédpanel toospecify saját tanúsítvány. Vegye figyelembe, hogy szüksége lesz a tooupload hello PFX verziója hello tanúsítvány tooyour üzemeltetett szolgáltatás hello Azure felügyeleti portálon belül, úgy, hogy Azure vissza tudja fejteni hello felhasználói jelszavát.

hello maradéka hello az oktatóanyag bemutatja, hogyan tooenable távelérés távoli hozzáférés letiltva eredetileg létrehozott Azure-telepítés projekthez. Ebben az oktatóanyagban létre fogunk hozni egy új önaláírt tanúsítványt, és a .pfx-fájlt egy tetszőleges jelszót kell. Akkor is hello beállítással, a hitelesítésszolgáltató által kiállított tanúsítványt.

## <a name="how-tooenable-remote-access-after-you-have-deployed-tooazure"></a>Hogyan tooenable távelérési követően tooAzure telepített
tooenable távelérési tooAzure, használjon hello lépések telepítése után:

1. Jelentkezzen be Azure-fiókjával hello Azure felügyeleti portálra

2. A listájában **Felhőszolgáltatások**, válassza ki a központilag telepített a felhőalapú szolgáltatás

3. Hello cloud service weblapon, kattintson a hello **konfigurálása** hivatkozás

4. A hello a hello konfiguráció lap alján, kattintson a hello **távoli** hivatkozás

5. Hello felugró párbeszédpanel megjelenésekor:
   
   * Adja meg a szerepkör hello meg a legyen tooenable távelérés

   * Kattintson a tooselect hello **távoli asztal engedélyezése** jelölőnégyzet
   
   * Adjon meg egy felhasználói nevet és jelszót toouse szeretne a távoli hozzáféréshez
   
   * Válassza ki a hello tanúsítvány toouse

6. Kattintson az **OK** gombra 

Egy üzenet arról, hogy a konfiguráció módosítása folyamatban van, ami eltarthat néhány percig toocomplete jelenik meg. Hello konfigurációs módosítás befejezése után kövesse hello hello **a toolog távolról** szakasz a cikk későbbi részében.

## <a name="how-tooenable-remote-access-in-your-package"></a>Hogyan távelérés tooenable, ha a csomag
1. Belül meg Eclipse Project Explorer ablaktáblában kattintson a jobb gombbal az Azure-projekt, és kattintson a **tulajdonságok**.

2. A hello **tulajdonságok** párbeszédpanelen bontsa ki a **Azure** a hello bal oldali ablaktáblában kattintson **távelérési**.

3. A hello **távelérési** párbeszédpanelen győződjön meg arról **engedélyezése minden szerepkörök tooaccept a távoli asztali kapcsolatokat a bejelentkezési hitelesítő adatokkal rendelkező** be van jelölve.

4. Adjon meg egy felhasználónevet a távoli asztali kapcsolat hello.

5. Adja meg, és erősítse meg a hello hello felhasználó jelszavát. hello felhasználói nevet és jelszót értékek ezen a párbeszédpanelen állítsa be a távoli asztali kapcsolat létrehozásakor használható. (Vegye figyelembe, hogy ez a PFX-jelszót külön jelszó.)

6. Adja meg a lejárati dátum hello hello felhasználói fiókhoz.

7. Kattintson a **új** toocreate egy új önaláírt tanúsítványt. (Másik lehetőségként kiválaszthatja egy tanúsítványt a munkaterület vagy a fájl rendszerről hello keresztül **munkaterület** vagy **fájlrendszer** gomb, illetve, de ebben az oktatóanyagban létre fogunk hozni egy új alkalmazásában tanúsítvány.)

   * A hello **új tanúsítvány** párbeszédpanelen adja meg, majd erősítse meg fogja használni a PFX-fájljának hello jelszót.

   * Fogadja el a megadott érték hello **név (CN)**, vagy használjon egy egyéni nevet.

   * Adja meg a hello új tanúsítvány .cer formában menteni hello elérési útját és nevét. Ez és hello következő lépést, a hello használata **cert** mappában található az Azure-projekt, de most szabad toochoose egy másik helyre. Ez az oktatóanyag céljából, ezen **c:\mycert\mycert.cer**. (Hello létrehozása **c:\mycert** mappa előzetes tooproceeding, vagy használjon egy létező mappát, ha szükséges.)

   * Adja meg a hello új tanúsítványt és a titkos kulcsot .pfx formátumban szeretné menteni hello elérési útját és nevét. Ez az oktatóanyag céljából, ezen **c:\mycert\mycert.pfx**. A **új tanúsítvány** párbeszédpanel alábbihoz hasonló toohello következő (hello mappák elérési útjaiban frissítéséhez, ha nem használja **c:\mycert**):
     
      ![][ic712275]

   * Kattintson a **OK** tooclose hello **új tanúsítvány** párbeszédpanel.

8. A **távelérési** párbeszédpanel hasonló toohello következő kell kinéznie:</p>
   
   ![][ic719495]

9. Kattintson a **OK** tooclose hello **távelérési** párbeszédpanel.

Az alkalmazás, a hello összeállítása a központi telepítés toocloud meg van adva.

## <a name="toolog-in-remotely"></a>a toolog távolról
Ha készen áll a szerepkör példánya, távolról bejelentkezhet toohello virtuális gép, amelyen az alkalmazást.

* Ha az eclipse-ben a Windows és a kijelölt hello használ **Start távoli asztal a telepítése** beállítást a központi telepítés tooAzure során választhat egy távoli asztali kapcsolat bejelentkezési képernyőt, amikor a telepítés megkezdése. Amikor hello felhasználónevet és jelszót kéri, adja meg a távoli felhasználó hello megadott hello értékeket, és képes toolog a.

* Hello keresztül távolról van egy másik módja toolog a <a href="http://go.microsoft.com/fwlink/?LinkID=512959">Azure felügyeleti portálon</a>:
  
  * Hello belül **Felhőszolgáltatások** ábrázolása hello Azure felügyeleti portálon, kattintson a felhőalapú szolgáltatás, majd **példányok**, kattintson egy adott példányt, és kattintson a hello **Connect**gombra. Hello **Connect** gomb hello parancssáv hello következő jelenik meg:
    
      ![][ic659273]

  * Hello kattintás után **Connect** gomb, felszólító tooopen RDP-fájlba fogja. Nyissa meg a hello fájlt, és hello utasításokat követve. (Képes is mentheti, a fájl tooyour helyi számítógép, és futtassa hello fájl dupla kattintással tooremote napló tooyour a virtuális gép toofirst anélkül hello felügyeleti portálon lépjen.)

  * Amikor hello felhasználónevet és jelszót kéri, adja meg a távoli felhasználó hello megadott hello értékeket, és képes toolog a.

> [!NOTE]
> Ha nem Windows operációs rendszeren, meg kell toouse egy távoli asztali ügyfél, amely az operációs rendszer kompatibilis, és kövesse a hello lépéseket tooconfigure, hogy az ügyfélszámítógépek hello beállításokkal letöltött hello RDP-fájlban.
> 
> 

## <a name="see-also"></a>Lásd még:
[Eclipse Azure eszköztára][Azure Toolkit for Eclipse]

[Hozzon létre egy Hello World alkalmazásról egy Azure az eclipse-ben][Creating a Hello World Application for Azure in Eclipse]

[Hello Azure eszköztára Eclipse telepítése][Installing hello Azure Toolkit for Eclipse] 

Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból][Azure Java Developer Center].

<!-- URL List -->

[Azure Java Developer Center]: http://go.microsoft.com/fwlink/?LinkID=699547
[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699529
[Creating a Hello World Application for Azure in Eclipse]: http://go.microsoft.com/fwlink/?LinkID=699533
[Installing hello Azure Toolkit for Eclipse]: http://go.microsoft.com/fwlink/?LinkId=699546

<!-- IMG List -->

[ic712275]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic712275.png
[ic719495]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719495.png
[ic719494]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic719494.png
[ic659273]: ./media/azure-toolkit-for-eclipse-enabling-remote-access-for-azure-deployments/ic659273.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690951.aspx -->
