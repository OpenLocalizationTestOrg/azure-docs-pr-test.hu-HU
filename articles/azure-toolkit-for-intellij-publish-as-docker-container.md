---
title: "egy Docker-tároló segítségével aaaPublish hello Azure eszközkészlet IntelliJ |} Microsoft Docs"
description: "Megtudhatja, hogyan toopublish egy webes alkalmazás tooMicrosoft Azure-bA egy Docker-tároló segítségével hello Azure eszközkészlet intellij-t."
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
ms.openlocfilehash: bee94cb269ea707ae7ad55232e23e915aec48c34
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-hello-azure-toolkit-for-intellij"></a>A webes alkalmazás is egy Docker-tároló közzé a IntelliJ hello Azure eszközkészlet használatával

Docker-tároló széles körben használt módszerrel az webes alkalmazások. Docker-tárolók használatával a fejlesztők képes egyesíteni a project fájlok és a függőségek telepítési tooa kiszolgáló egyetlen csomagban. hello Azure eszköztára IntelliJ egyszerűbben a Java-fejlesztőknek hozzáadásával *Docker-tároló közzététel* telepítési tooMicrosoft Azure szolgáltatásai. Ez a cikk bemutatja, hogyan hello lépéseket szükséges toopublish az alkalmazások tooAzure Docker tárolóként.

> [!NOTE]
>
> További információ a Docker érhető el hello [Docker webhely].
>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a>A webes alkalmazás tooAzure közzététele egy Docker-tároló használatával

> [!NOTE]
> * toopublish webalkalmazását, létre kell hoznia egy központi telepítés kész összetevő. toolearn több, tekintse meg a hello [összetevők létrehozásával kapcsolatos további információért](#artifacts) szakasz.
>
> * Befejezését követően hello telepítési varázsló legalább egyszer, a beállítások a legtöbb alapértelmezett szolgálnak, hogy hello varázsló futtatásakor ismét.
>

1. Nyissa meg a webalkalmazás-projektet az intellij-t.

2. toostart hello **Docker-tároló közzététel** varázsló hello alábbi módszerekkel:

   * A hello **projekt** eszköz ablak, kattintson jobb gombbal a projektre, kattintson a **Azure**, és kattintson a **Docker-tároló közzététel**:

      ![hello Docker-tároló parancsként közzététele][PUB01]

   * Hello IntelliJ eszköztáron kattintson a hello **közzététel csoport** gombra, majd **Docker-tároló közzététel**:

      ![hello Docker-tároló parancsként közzététele][PUB02]  
    Hello **Docker-tároló központi telepítése az Azure-on** varázsló megnyílik.

   ![hello Azure varázsló a Docker-tároló központi telepítése][PUB03]

3. A hello **írja be a lemezkép nevét, válassza ki a hello összetevő elérési útja, ellenőrizze a használt Docker állomás toobe** ablakban, a következő hello: 

   a. A hello **Docker lemezképnév** mezőben adjon meg egy egyedi nevet a Docker-állomás. (hello varázsló automatikusan létrehozza a nevét, de módosíthatja.) 

   b. Hello **állomások** terület tartalmazza a Docker állomásokon, már létrehozott. Hello alábbi módszerekkel: 
      * Ha egy meglévő Docker-állomás, a webes alkalmazás tooit telepítheti.
      * toocreate egy Docker-állomás, kattintson a hello zöld plusz jel (**+**).  
       Hello **Docker állomás létrehozása** párbeszédpanel. 

      ![Az Azure varázsló Docker-tároló telepítése][PUB04a]

4. A hello **hello új virtuális gép konfigurálása** ablakban adja meg a következő információ a Docker állomás hello. (hello varázsló automatikusan létrehozza a legtöbb hello az adatokat, de egyikük módosíthatja.) 

   a. A hello **neve** hello Docker állomás egyedi nevét adja meg. (Fontos nem ugyanaz, mint a korábban megadott Docker lemezképnév hello hello.) 
    
   b. A hello **előfizetés** mezőbe írja be a hello Azure-előfizetéssel, amelyekkel az állomáshoz. 
      
   c. A hello **régió** mezőbe írja be a hello földrajzi terület, ahol a gazdagépen található.
      
   d. A hello **az operációs rendszer és a méret** lapon, a következő hello:      
      * **Az operációs rendszer üzemeltetéséhez**: hello operációs rendszer hello virtuális gépet, amely tartalmazza a gazdagépet adjon meg. 
      * **Méret**: hello virtuális gép méretét adja meg a gazdagépen.   
       
   e. A hello **erőforráscsoport** lapra, válassza ki a hello a következők egyikét:      
      * **Új erőforráscsoport**: hozzon létre egy erőforráscsoportot az állomáshoz.
      * **Meglévő erőforráscsoport**: Adjon meg egy meglévő erőforráscsoportot az Azure-fiókjába. 
       
   f. A hello **hálózati** lapra, válassza ki a hello a következők egyikét:      
      * **Új virtuális hálózat**: virtuális hálózat létrehozása az állomáshoz.
      * **Meglévő virtuális hálózat**: Adjon meg egy meglévő virtuális hálózatot az Azure-fiókjába. 
       
   g. A hello **tárolási** lapra, válassza ki a hello a következők egyikét:      
      * **Új tárfiók**: hozzon létre egy tárfiókot az állomáshoz.
      * **Meglévő tárfiók**: Adja meg az Azure-fiókjába meglévő tárfiókot.
       
5. Kattintson a **Tovább** gombra.  
     Hello **napló konfigurálja a hitelesítő adatokat, és a portbeállítások** ablak nyílik meg.

      ![hello konfigurálása bejelentkezési hitelesítő adatok és a port beállításai ablak][PUB05]

6. Válasszon egyet az alábbi beállítások hello:

      * **Hitelesítő adatok importálása az Azure Key Vault**: Adjon meg egy korábban mentett hitelesítő adatokat, amelyek az Azure-előfizetéshez vannak tárolva.

          > [!NOTE]
          > Az az Azure key vault, amely egy meghatározott fiók vagy a szolgáltatás egyszerű nincs egy másik fiókot vagy egyszerű szolgáltatásnév osztja meg a hello előfizetés automatikusan elérhető. tooallow egy másik fiókot, vagy a szolgáltatás egyszerű toouse hello kulcsot tároló, hello Azure portál tooadd hello fiók vagy a szolgáltatásnevet kell használnia.

      * **Új bejelentkezési hitelesítő adatok**: létrehozhat egy új bejelentkezési hitelesítő adatokat. Ha ezt a lehetőséget választja, a hello, a következő:

        a. A hello **VM hitelesítő adatok** lapra, adja meg a következő információ hello virtuális gép bejelentkezési hitelesítő adatokat a Docker-állomás hello: * **felhasználónév**: Adja meg a virtuális gép bejelentkezési hello felhasználónév hitelesítő adatok.
             * **Jelszó** és **megerősítése**: hello jelszó megadása a virtuális gép bejelentkezési hitelesítő adatait.
             * **SSH**: Adja meg a Docker állomás hello Secure Shell (SSH) beállításait. Hello alábbi beállítások közül választhat: * **nincs**: meghatározza, hogy a virtuális gép nem teszi lehetővé SSH-kapcsolatokhoz.
                * **Automatikus generálása**: hozza létre automatikusan hello szükséges beállítások SSH-kapcsolaton keresztül csatlakozni.
                * **Importálás a directory**: lehetővé teszi egy korábban mentett SSH-beállítások készletét tartalmazó könyvtár toospecify. hello directory tartalmaznia kell a következő két fájlok hello:
                
                  * *id_rsa*: Contains hello RSA identification for a user.
                  * *id_rsa.pub*: Contains hello RSA public key that is used for authentication.
            
        b. A hello **Docker démon hozzáférés** lapra, adja meg a következő információ hello:

          ![Docker állomás létrehozása][PUB06]
    
             * **Docker Daemon port**: Enter hello unique TCP port for your Docker host.
             * **TLS Security**: Enter hello Transport Layer Security settings for your Docker host. You can choose from hello following options:
                * **None**: Specifies that your virtual machine does not allow TLS connections.
                * **Auto-generate**: Automatically creates hello requisite settings for connecting via TLS.
                * **Import from directory**: Specifies a directory that contains a set of previously saved TLS settings. hello directory must contain hello following six files: 
                   * *ca.pem* and *ca-key.pem*: Contain hello certificate and public key for hello TLS Certificate Authority.
                   * *cert.pem* and *key.pem*: Contain client certificate and public key which will be used for TLS authentication.
                   * *server.pem* and *server-key.pem*: Contain hello client certificate and public key that is used for TLS authentication.

7. Miután megadta a hello szükséges információkat, kattintson a **Befejezés**.  
    Hello **Docker-tároló központi telepítése az Azure-on** varázsló ismét megjelenik.

   ![Az Azure varázsló Docker-tároló telepítése][PUB07]

8. Kattintson a **Tovább** gombra.  
    Hello **hello Docker-tároló toobe létrehozott konfigurálása** ablak nyílik meg.

   ![hello konfigurálása hello Docker tároló létrehozott toobe ablak][PUB08]

9. A hello **hello Docker-tároló toobe létrehozott konfigurálása** ablakban adja meg a következő információ hello: 

   a. A hello **Docker-tároló neve** mezőben adjon meg egy egyedi nevet a Docker-tároló.

   b. A következő Docker képek hello közül választhat: 

      * **Előre definiált Docker kép**: Adjon meg egy már meglévő Docker-lemezképet az Azure-ból. 

        > [!NOTE]
        > hello Docker képek ebben a mezőben a lista az számos olyan rendszerkép található, hogy hello Azure eszközkészlet lett konfigurálva toopatch, hogy a rendszer automatikusan telepíti az összetevő. 

      * **Egyéni Dockerfile**: Adjon meg egy korábban mentett Dockerfile a helyi számítógépről.

        > [!NOTE]
        > Ez a tulajdonság speciális fejlesztők számára toodeploy saját Dockerfile. Azt azonban ez a beállítás tooensure azok Dockerfile megfelelően részét képező használó toodevelopers legfeljebb lesz. Hello Azure eszközkészlet nem felel meg egy egyéni Dockerfile hello tartalma, mert hello telepítése meghiúsulhat, ha hello Dockerfile problémákkal rendelkezik. Ezenkívül hello Azure eszközkészlet hello egyéni Dockerfile toocontain egy webes alkalmazás összetevő vár, mert az megpróbál tooopen HTTP-kapcsolatot. Fejlesztők összetevő más típusú közzé, ha azok ártalmatlan hibák fordulhat elő, a telepítés utáni.

   c. A hello **portbeállításokat** mezőbe írja be a hello egyedi TCP port kötést a Docker-tároló. 

10. Kattintson az előző lépésekben hello befejezését követően **Befejezés**. 

hello Azure eszközkészlet megkezdi a központi telepítését a webes alkalmazás tooAzure egy Docker-tároló. Ha beállította az IntelliJ toobe hello háttérben telepített egy **tooAzure telepítése** folyamatjelző sáv jelenik meg. 

![hello telepítés folyamatjelző][PUB09]

<a name="artifacts"></a>
## <a name="additional-information-about-creating-artifacts"></a>Az összetevők létrehozásával kapcsolatos további információért

a telepítés kész összetevő toocreate hello a következő:

1. Nyissa meg a webalkalmazás-projektet az intellij-t.

2. Kattintson a **fájl**, és kattintson a **szerkezetének**.

   ![hello szerkezetének parancs][ART01]

3. az összetevő, tooadd kattintson hello zöld plusz jel (**+**), és kattintson a **webalkalmazás: archív**.

   ![hello ": webalkalmazások archívumából" parancs][ART02]

4. A hello **neve** mezőbe írja be a adatösszetevőt nevét (nem tartalmaznak hello *.war* bővítmény), és kattintson a **OK**.

   ![hello összetevő neve mező][ART03]

Az IntelliJ összetevők létrehozásával kapcsolatos további információkért lásd: [összetevők konfigurálása] hello JetBrains webhelyen.

## <a name="next-steps"></a>Következő lépések
A Java IDEs hello Azure eszközök gazdag kapcsolatos további információkért tekintse meg a következő erőforrások hello:

* [Eclipse Azure eszköztára]
  * [What's new in hello Eclipse Azure eszköztára]
  * [Hello Azure eszköztára Eclipse telepítése]
  * [Hello Azure eszköztára Eclipse bejelentkezési utasítások]
  * [Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]
* [Az IntelliJ-hez készült Azure-eszközkészlet]
  * [What's new in hello IntelliJ Azure eszköztára]
  * [Az IntelliJ hello Azure eszközkészlet telepítése]
  * [Bejelentkezési utasításokat hello Azure eszközkészlet az intellij-t]
  * [Hello World webalkalmazás létrehozása az intellij-t az Azure]

Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból] és hello [Java Tools for Visual Studio Team Services].

További erőforrásokat a Docker, lásd: hello hivatalos [Docker webhely].

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Hello Azure eszköztára Eclipse telepítése]: ./azure-toolkit-for-eclipse-installation.md
[Az IntelliJ hello Azure eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md
[Hello Azure eszköztára Eclipse bejelentkezési utasítások]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Bejelentkezési utasításokat hello Azure eszközkészlet az intellij-t]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[What's new in hello Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-whats-new.md
[What's new in hello IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/
[A Visual Studio Team Services Java-eszközök]: https://java.visualstudio.com/

[Docker-webhely]: https://www.docker.com/
[Az összetevők konfigurálása]: https://www.jetbrains.com/help/idea/2016.1/configuring-artifacts.html

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
