---
title: "Az eclipse-ben az Azure-eszközkészlet használatával egy Docker-tároló közzététele |} Microsoft Docs"
description: "Útmutató a webes alkalmazás a Microsoft Azure is közzé egy Docker-tároló az eclipse-ben az Azure-eszközkészlet használatával."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 746bb0a073396b84fa8592adda6748a2a5692ee8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a>A webes alkalmazás is egy Docker-tároló közzé a eclipse-ben az Azure-eszközkészlet használatával

Docker-tároló széles körben használt módszerrel az webes alkalmazások. Docker-tárolók használatával a fejlesztők képes egyesíteni a project fájlok és a függőségek egy kiszolgáló üzembe helyezése egyetlen csomagban. Az Eclipse Azure eszköztára egyszerűbben a Java-fejlesztőknek hozzáadásával *Docker-tároló közzététel* központi telepítést, hogy a Microsoft Azure szolgáltatásainak. Ez a cikk végigvezeti az alkalmazások az Azure-bA Docker tárolóként közzétételéhez szükséges lépéseket.

> [!NOTE]
> További információ a Docker érhető el a [Docker webhely].
>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a>A webalkalmazás közzététele az Azure-bA egy Docker-tároló használatával

1. Nyissa meg a webalkalmazás-projektet az eclipse-ben.

2. Elindítani a **Docker-tároló közzététel** varázsló, tegye a következők egyikét:

   * Az a **Navigator** megtekinteni, kattintson jobb gombbal a projektre, kattintson a **Azure**, és kattintson a **Docker-tároló közzététel**.

      ![Közzététel Docker-tároló parancsként Navigator megtekintése][PUB01]

   * Az Eclipse eszköztáron kattintson a **közzététel** gombra, majd **Docker-tároló közzététel**.

      ![Közzététel Docker-tároló parancsként eclipse eszköztár][PUB02]
      
    A **Docker-tároló központi telepítése az Azure-on** varázsló megnyílik.

    ![A központi telepítése Docker-tároló Azure varázsló][PUB03]

3. Az a **adjon meg egy kép nevet, válassza ki a összetevő elérési útja, ellenőrizze a használt Docker állomás** ablakban tegye a következőket:

    a. Az a **Docker lemezképnév** mezőben adjon meg egy egyedi nevet a Docker-állomás. (A varázsló automatikusan létrehozza a nevét, de módosíthatja.)

    b. A **állomások** terület tartalmazza a Docker állomásokon, már létrehozott. A következő lehetőségek közül választhat:

    * Ha egy meglévő Docker-állomás, a webes alkalmazás telepíthet rá.
    * Hozzon létre egy új Docker-állomás, kattintson a **Hozzáadás**.  
      
    A **Docker állomás létrehozása** párbeszédpanel.

    ![Az Azure varázsló Docker-tároló telepítése][PUB04a]

4. Az a **állítsa be az új virtuális gép** ablakban adja meg a következő beállítások érhetők el a Docker-állomás. (A varázsló automatikusan létrehozza a lehetőségek a legtöbb, de egyikük módosíthatja.)

   a. **Név**: Adjon meg egy egyedi nevet a Docker-állomás. (Nincs ugyanaz, mint a korábban megadott Docker lemezképnév.)

   b. **Előfizetés**: Adja meg az Azure-előfizetés, amelyekkel az állomáshoz.

   c. **A régióban**: Adja meg a földrajzi régió, ahol az állomás.

   d. Az a **állomás operációs rendszer és a méret** lapon:
     * **Az operációs rendszer üzemeltetéséhez**: Adja meg az operációs rendszer a virtuális gép, amely tartalmazza a gazdagépet.
     * **Méret**: Adja meg a virtuális gép méretét az állomáshoz.

   e. Az a **erőforráscsoport** lapon:
     * **Új erőforráscsoport**: hozzon létre egy új erőforráscsoportot az állomáshoz.
     * **Meglévő erőforráscsoport**: Írjon be egy meglévő erőforráscsoportot az Azure-fiókjával.

   f. Az a **hálózati** lapon:
     * **Új virtuális hálózat**: új virtuális hálózat létrehozása az állomáshoz.
     * **Meglévő virtuális hálózat**: Írjon be egy meglévő virtuális hálózatot az Azure-fiókjával.

   g. Az a **tárolási** lapon:
     * **Új tárfiók**: hozzon létre egy új tárfiókot az állomáshoz.
     * **Meglévő tárfiók**: Írjon be egy meglévő tárfiókot az Azure-fiókjával.

5. Kattintson a **Tovább** gombra.

6. Az a **napló konfigurálja a hitelesítő adatokat, és a portbeállítások** ablakban, a következő lehetőségek közül:

    * **Hitelesítő adatok importálása az Azure Key Vault**: egy korábban mentett kumulatív hitelesítő adatokat, amelyek az Azure-előfizetéshez vannak tárolva.

      >[!NOTE]
      >Egy Azure Key Vault, amely egy meghatározott fiók vagy a szolgáltatás egyszerű nincs egy másik fiókot vagy egyszerű szolgáltatásnév osztja meg az előfizetés által automatikusan elérhető. Egy másik fiókot, vagy a szolgáltatás lehetővé teszi a Key Vault használata egyszerű, az Azure-portálon kell használnia a fiókot, vagy az egyszerű szolgáltatásnév hozzáadásához.

    * **Új bejelentkezési hitelesítő adatok**: hoz létre új bejelentkezési hitelesítő adatokat. Ha ezt a lehetőséget választja, tegye a következőket:
    
      * Az a **VM hitelesítő adatok** adja meg a virtuális gép bejelentkezési hitelesítő adatok a Docker-állomás beállítások a következők egyikét:

          * **Felhasználónév**: Adja meg a felhasználónevet a virtuális gép bejelentkezési hitelesítő adatokat.
          * **Jelszó** és **megerősítése**: Adja meg a jelszót a virtuális gép bejelentkezési hitelesítő adatokat.
          * **SSH**: Adja meg a Secure Shell (SSH) beállításait a Docker-állomás. Az alábbi lehetőségek közül választhat:
            * **Nincs**: meghatározza, hogy a virtuális gép nem teszi lehetővé SSH-kapcsolatokhoz.
            * **Automatikus generálása**: automatikusan létrehozza a szükséges beállítások SSH-kapcsolaton keresztül csatlakozni.
            * **Importálás a directory**: Adja meg a korábban mentett SSH-beállítások készletét tartalmazó könyvtár. A könyvtár a következő két fájlt kell tartalmaznia:
                * *id_rsa*: a felhasználó RSA azonosítóját tartalmazza.
                * *id_rsa.pub*: a hitelesítéshez használt RSA nyilvános kulcsot tartalmazza.
        
        ![Docker állomás létrehozása][PUB05]

      * Az a **Docker démon hitelesítő adatok** lapra, adja meg a következő beállításokat:

          * **Docker démon port**: Adja meg az egyedi TCP-port a Docker-állomás.
          * **A TLS biztonsági**: Adja meg a Transport Layer Security-beállításokat a Docker-állomás. Az alábbi lehetőségek közül választhat:
            * **Nincs**: Megadja, hogy a virtuális gép nem engedélyezi TLS-kapcsolatokhoz.
            * **Automatikus generálása**: automatikusan létrehozza a szükséges beállítások TLS keresztül kapcsolódik.
            * **Importálás a directory**: Adja meg a korábban mentett TLS beállítások készletét tartalmazó könyvtár. Pontosabban a könyvtár a következő hat fájlokat kell tartalmaznia:
                * *CA.PEM* és *ca-key.pem*: az a TLS-hitelesítésszolgáltató a tanúsítvány és nyilvános kulcs található.
                * *CERT.PEM* és *key.pem*: az ügyfél-tanúsítványt és a TLS hitelesítésre használt nyilvános kulcsot tartalmazza.
                * *Server.PEM* és *kiszolgáló-key.pem*: a kiszolgálói tanúsítvány és nyilvános kulcs tartalmaznia ahhoz, hogy a gazdagép.

        ![Docker állomás létrehozása][PUB06]

7. Miután megadta a fenti adatokat, kattintson a **Befejezés**.

8. Az a **Docker-tároló központi telepítése az Azure-on** varázsló, kattintson a **következő**.

   ![A központi telepítése Docker-tároló Azure varázsló][PUB07]

9. Az a **létrehozni a Docker-tároló konfigurálása** ablakban tegye a következőket:

   a. Az a **Docker-tároló neve** mezőben adjon meg egy egyedi nevet a Docker-tároló.

   b. A következő Docker-rendszerképek közül választhat:
     * **Előre definiált Docker kép**: Megadja a már meglévő Docker-lemezkép az Azure-ból.

       >[!NOTE]
       >A Docker képek ebben a mezőben a lista az számos olyan rendszerkép található, amely az Azure-eszközkészlet lett konfigurálva, hogy a rendszer automatikusan telepíti az összetevő javítás.

     * **Egyéni Dockerfile**: a helyi számítógépről egy korábban mentett Dockerfile határozza meg.

       >[!NOTE]
       >Ez a tulajdonság speciális fejlesztők számára a saját Dockerfile telepíteni kívánja. Azonban esetén a fejlesztők számára ez a beállítás segítségével győződjön meg arról, hogy azok Dockerfile van megfelelően lett felépítve. Az Azure-eszközkészlet nem felel meg egy egyéni Dockerfile tartalma, a telepítés esetleg sikertelen lesz, ha a Dockerfile problémákkal rendelkezik. Ezenkívül az Azure-eszközkészlet vár az egyéni Dockerfile magában foglalja a webes alkalmazás-összetevő, és megkísérli nyissa meg a HTTP-kapcsolatot. Ha a fejlesztők közzététele összetevő más típusú, a telepítés utáni jelenhet ártalmatlan hibák.

   c. **Portbeállításokat**: Adja meg a Docker-tároló egyedi TCP-port kötés.

     ![A létrehozandó ablakban a Docker-tároló konfigurálása][PUB08]

10. Miután elvégezte az előző lépéseket, kattintson a **Befejezés**.

Az Azure-eszközkészlet megkezdi a központi telepítését a webalkalmazás Azure egy Docker-tároló. 

## <a name="next-steps"></a>Következő lépések
A Java IDEs az Azure eszközök gazdag kapcsolatos további információkért lásd a következőket:

* [Eclipse Azure eszköztára]
  * [What's new in Eclipse Azure eszköztára]
  * [Az Eclipse-hez készült Azure-eszközkészlet telepítése]
  * [Az Eclipse Azure eszköztára bejelentkezési utasítások]
  * [Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]
* [Az IntelliJ-hez készült Azure-eszközkészlet]
  * [What's new in IntelliJ Azure eszköztára]
  * [Az IntelliJ-hez készült Azure-eszközkészlet telepítése]
  * [Bejelentkezési utasításokat az Azure-eszközkészlet az intellij-t]
  * [Hello World webalkalmazás létrehozása az intellij-t az Azure]

Azure Java használatával kapcsolatos további információkért lásd: [Azure Java fejlesztői központból] és [Java Tools for Visual Studio Team Services].

További erőforrásokat a Docker, tekintse meg a hivatalos [Docker webhely].

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Az Eclipse-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-eclipse-installation.md
[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md
[Az Eclipse Azure eszköztára bejelentkezési utasítások]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Bejelentkezési utasításokat az Azure-eszközkészlet az intellij-t]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[What's new in Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-whats-new.md
[What's new in IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/

[Docker webhely]: https://www.docker.com/

<!-- IMG List -->

[PUB01]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB01.png
[PUB02]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB02.png
[PUB03]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB03.png
[PUB04a]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04a.png
[PUB04b]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04b.png
[PUB04c]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04c.png
[PUB04d]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB04d.png
[PUB05]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB05.png
[PUB06]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB06.png
[PUB07]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB07.png
[PUB08]: ./media/azure-toolkit-for-eclipse-publish-as-docker-container/PUB08.png