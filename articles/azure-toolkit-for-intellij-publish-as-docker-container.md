---
title: "Az intellij-t az Azure-eszközkészlet használatával egy Docker-tároló közzététele |} Microsoft Docs"
description: "Útmutató a webes alkalmazás a Microsoft Azure is közzé egy Docker-tároló az intellij-t az Azure-eszközkészlet használatával."
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
ms.openlocfilehash: 96680319a6c4c0f0a4673cd6303a5b172f428797
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-the-azure-toolkit-for-intellij"></a>A webes alkalmazás is egy Docker-tároló közzé a intellij-t az Azure-eszközkészlet használatával

Docker-tároló széles körben használt módszerrel az webes alkalmazások. Docker-tárolók használatával a fejlesztők képes egyesíteni a project fájlok és a függőségek egy kiszolgáló üzembe helyezése egyetlen csomagban. Az IntelliJ Azure eszköztára egyszerűbben a Java-fejlesztőknek hozzáadásával *Docker-tároló közzététel* központi telepítést, hogy a Microsoft Azure szolgáltatásainak. Ez a cikk végigvezeti az alkalmazások az Azure-bA Docker tárolóként közzétételéhez szükséges lépéseket.

> [!NOTE]
>
> További információ a Docker érhető el a [Docker webhely].
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a>A webalkalmazás közzététele az Azure-bA egy Docker-tároló használatával

> [!NOTE]
> * A webalkalmazás közzétételét, létre kell hoznia egy központi telepítés kész összetevő. További tudnivalókért tekintse meg a [összetevők létrehozásával kapcsolatos további információért](#artifacts) szakasz.
>
> * Befejezését követően a központi telepítési varázsló legalább egyszer, a beállítások a legtöbb alapértelmezett használata, amikor a varázsló ismételt futtatása.
>

1. Nyissa meg a webalkalmazás-projektet az intellij-t.

2. Elindítani a **Docker-tároló közzététel** varázsló, tegye a következők egyikét:

   * Az a **projekt** eszköz ablak, kattintson jobb gombbal a projektre, kattintson a **Azure**, és kattintson a **Docker-tároló közzététel**:

      ![A közzététel Docker-tároló parancsot][PUB01]

   * Az IntelliJ eszköztáron kattintson a **közzététel csoport** gombra, majd **Docker-tároló közzététel**:

      ![A közzététel Docker-tároló parancsot][PUB02]  
    A **Docker-tároló központi telepítése az Azure-on** varázsló megnyílik.

   ![A központi telepítése Docker-tároló Azure varázsló][PUB03]

3. Az a **adjon meg egy kép nevet, válassza ki a összetevő elérési útja, ellenőrizze a használt Docker állomás** ablakban tegye a következőket: 

   a. Az a **Docker lemezképnév** mezőben adjon meg egy egyedi nevet a Docker-állomás. (A varázsló automatikusan létrehozza a nevét, de módosíthatja.) 

   b. A **állomások** terület tartalmazza a Docker állomásokon, már létrehozott. A következő lehetőségek közül választhat: 
      * Ha egy meglévő Docker-állomás, a webes alkalmazás telepíthet rá.
      * Hozzon létre egy Docker-állomás, kattintson a zöld plusz jel (**+**).  
       A **Docker állomás létrehozása** párbeszédpanel. 

      ![Az Azure varázsló Docker-tároló telepítése][PUB04a]

4. Az a **állítsa be az új virtuális gép** ablakban adja meg a következő információkat a Docker-állomás. (A varázsló automatikusan létrehozza az adatokat a legtöbb, de egyikük módosíthatja.) 

   a. Az a **neve** mezőben adjon meg egy egyedi nevet a Docker-állomás. (Nincs ugyanaz, mint a korábban megadott Docker lemezképnév.) 
    
   b. Az a **előfizetés** mezőbe írja be az Azure-előfizetés, amelyekkel az állomáshoz. 
      
   c. Az a **régió** mezőben adja meg a földrajzi régió, ahol az állomás.
      
   d. Az a **az operációs rendszer és a méret** lapján tegye a következőket:      
      * **Az operációs rendszer üzemeltetéséhez**: Adja meg az operációs rendszer a virtuális gép, amely tartalmazza a gazdagépet. 
      * **Méret**: Adja meg a virtuális gép méretét az állomáshoz.   
       
   e. Az a **erőforráscsoport** lapra, válassza ki a következők egyikét:      
      * **Új erőforráscsoport**: hozzon létre egy erőforráscsoportot az állomáshoz.
      * **Meglévő erőforráscsoport**: Adjon meg egy meglévő erőforráscsoportot az Azure-fiókjába. 
       
   f. Az a **hálózati** lapra, válassza ki a következők egyikét:      
      * **Új virtuális hálózat**: virtuális hálózat létrehozása az állomáshoz.
      * **Meglévő virtuális hálózat**: Adjon meg egy meglévő virtuális hálózatot az Azure-fiókjába. 
       
   g. Az a **tárolási** lapra, válassza ki a következők egyikét:      
      * **Új tárfiók**: hozzon létre egy tárfiókot az állomáshoz.
      * **Meglévő tárfiók**: Adja meg az Azure-fiókjába meglévő tárfiókot.
       
5. Kattintson a **Tovább** gombra.  
     A **napló konfigurálja a hitelesítő adatokat, és a portbeállítások** ablak nyílik meg.

      ![A konfigurálás bejelentkezési hitelesítő adatok és a port beállításai ablak][PUB05]

6. A következő lehetőségek közül:

      * **Hitelesítő adatok importálása az Azure Key Vault**: Adjon meg egy korábban mentett hitelesítő adatokat, amelyek az Azure-előfizetéshez vannak tárolva.

          > [!NOTE]
          > Az az Azure key vault, amely egy meghatározott fiók vagy a szolgáltatás egyszerű nincs egy másik fiókot vagy egyszerű szolgáltatásnév osztja meg az előfizetés által automatikusan elérhető. Egy másik fiókot, vagy a szolgáltatás lehetővé teszi a key vault használata egyszerű, az Azure-portálon kell használnia a fiókot, vagy az egyszerű szolgáltatásnév hozzáadásához.

      * **Új bejelentkezési hitelesítő adatok**: létrehozhat egy új bejelentkezési hitelesítő adatokat. Ha ezt a lehetőséget választja, tegye a következőket:

        a. Az a **VM hitelesítő adatok** lapra, adja meg a következő információkat a virtuális gép bejelentkezési hitelesítő adatok a Docker-állomás: * **felhasználónév**: be a felhasználónevet a virtuális gép bejelentkezési adatait hitelesítő adatok.
             * **Jelszó** és **megerősítése**: Adja meg a jelszót a virtuális gép bejelentkezési hitelesítő adatokat.
             * **SSH**: Adja meg a Secure Shell (SSH) beállításait a Docker-állomás. Az alábbi lehetőségek közül választhat: * **nincs**: meghatározza, hogy a virtuális gép nem teszi lehetővé SSH-kapcsolatokhoz.
                * **Automatikus generálása**: automatikusan létrehozza a szükséges beállítások SSH-kapcsolaton keresztül csatlakozni.
                * **Importálás a directory**: lehetővé teszi egy korábban mentett SSH-beállítások készletét tartalmazó könyvtárat. A könyvtár a következő két fájlt kell tartalmaznia:
                
                  * *id_rsa*: Contains the RSA identification for a user.
                  * *id_rsa.pub*: Contains the RSA public key that is used for authentication.
            
        b. Az a **Docker démon hozzáférés** lapra, adja meg a következő információkat:

          ![Docker állomás létrehozása][PUB06]
    
             * **Docker Daemon port**: Enter the unique TCP port for your Docker host.
             * **TLS Security**: Enter the Transport Layer Security settings for your Docker host. You can choose from the following options:
                * **None**: Specifies that your virtual machine does not allow TLS connections.
                * **Auto-generate**: Automatically creates the requisite settings for connecting via TLS.
                * **Import from directory**: Specifies a directory that contains a set of previously saved TLS settings. The directory must contain the following six files: 
                   * *ca.pem* and *ca-key.pem*: Contain the certificate and public key for the TLS Certificate Authority.
                   * *cert.pem* and *key.pem*: Contain client certificate and public key which will be used for TLS authentication.
                   * *server.pem* and *server-key.pem*: Contain the client certificate and public key that is used for TLS authentication.

7. Miután megadta a szükséges adatokat, kattintson a **Befejezés**.  
    A **Docker-tároló központi telepítése az Azure-on** varázsló ismét megjelenik.

   ![Az Azure varázsló Docker-tároló telepítése][PUB07]

8. Kattintson a **Tovább** gombra.  
    A **létrehozni a Docker-tároló konfigurálása** ablak nyílik meg.

   ![A létrehozandó ablakban a Docker-tároló konfigurálása][PUB08]

9. Az a **létrehozni a Docker-tároló konfigurálása** ablakban adja meg a következő információkat: 

   a. Az a **Docker-tároló neve** mezőben adjon meg egy egyedi nevet a Docker-tároló.

   b. A következő Docker-rendszerképek közül választhat: 

      * **Előre definiált Docker kép**: Adjon meg egy már meglévő Docker-lemezképet az Azure-ból. 

        > [!NOTE]
        > A Docker képek ebben a mezőben a lista az számos olyan rendszerkép található, amely az Azure-eszközkészlet lett konfigurálva, hogy a rendszer automatikusan telepíti az összetevő javítás. 

      * **Egyéni Dockerfile**: Adjon meg egy korábban mentett Dockerfile a helyi számítógépről.

        > [!NOTE]
        > Ez a tulajdonság speciális fejlesztők számára a saját Dockerfile telepíteni kívánja. Azonban esetén a fejlesztők számára ez a beállítás segítségével győződjön meg arról, hogy azok Dockerfile van megfelelően lett felépítve. Az Azure-eszközkészlet nem felel meg egy egyéni Dockerfile tartalmát, mert a telepítés meghiúsulhat, ha a Dockerfile problémákkal rendelkezik. Ezenkívül az Azure Toolkit tartalmaz egy webes alkalmazás összetevő egyéni Dockerfile vár, mert megpróbálja nyissa meg a HTTP-kapcsolatot. Fejlesztők összetevő más típusú közzé, ha azok ártalmatlan hibák fordulhat elő, a telepítés utáni.

   c. Az a **portbeállításokat** mezőbe írja be a Docker-tároló egyedi TCP-port kötés. 

10. Kattintson a fenti lépések végrehajtása után **Befejezés**. 

Az Azure-eszközkészlet megkezdi a központi telepítését a webalkalmazás Azure egy Docker-tároló. Ha beállította az intellij-t a háttérben telepíteni egy **üzembe helyezése az Azure-bA** folyamatjelző sáv jelenik meg. 

![A telepítés folyamatjelző][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a>Az összetevők létrehozásával kapcsolatos további információért

A telepítés kész összetevő létrehozásához tegye a következőket:

1. Nyissa meg a webalkalmazás-projektet az intellij-t.

2. Kattintson a **fájl**, és kattintson a **szerkezetének**.

   ![A projekt struktúra parancs][ART01]

3. Összetevő hozzáadásához kattintson a zöld plusz jel (**+**), és kattintson a **webalkalmazás: archív**.

   ![A ": webalkalmazások archívumából" parancs][ART02]

4. Az a **neve** mezőbe írja be a adatösszetevőt nevét (nem tartalmaznak a *.war* bővítmény), és kattintson a **OK**.

   ![Az összetevő neve mező][ART03]

Az IntelliJ összetevők létrehozásával kapcsolatos további információkért lásd: [összetevők konfigurálása] a JetBrains webhelyen.

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

Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ] és [Java-eszközök a Visual Studio Team Serviceshez].

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

[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/
[Java-eszközök a Visual Studio Team Serviceshez]: https://java.visualstudio.com/

[Docker webhely]: https://www.docker.com/
[összetevők konfigurálása]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

<!-- IMG List -->

[PUB01]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB01.png
[PUB02]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB02.png
[PUB03]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB03.png
[PUB04a]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04a.png
[PUB04b]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04b.png
[PUB04c]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04c.png
[PUB04d]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB04d.png
[PUB05]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB05.png
[PUB06]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB06.png
[PUB07]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB07.png
[PUB08]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB08.png
[PUB09]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/PUB09.png

[ART01]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART01.png
[ART02]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART02.png
[ART03]: ./media/azure-toolkit-for-intellij-publish-as-docker-container/ART03.png
