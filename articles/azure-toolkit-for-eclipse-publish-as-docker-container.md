---
title: "egy Docker-tároló segítségével aaaPublish hello Azure eszköztára Eclipse |} Microsoft Docs"
description: "Megtudhatja, hogyan toopublish egy webes alkalmazás tooMicrosoft Azure-bA egy Docker-tároló segítségével hello Azure eszköztára eclipse-ben."
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
ms.openlocfilehash: 53ec3a7f7a171691024e03622fd48d6f1e257b50
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-web-app-as-a-docker-container-by-using-hello-azure-toolkit-for-eclipse"></a>A webes alkalmazás is egy Docker-tároló közzé a Eclipse hello Azure eszközkészlet használatával

Docker-tároló széles körben használt módszerrel az webes alkalmazások. Docker-tárolók használatával a fejlesztők képes egyesíteni a project fájlok és a függőségek telepítési tooa kiszolgáló egyetlen csomagban. hello Azure eszköztára Eclipse egyszerűbben a Java-fejlesztőknek hozzáadásával *Docker-tároló közzététel* telepítési tooMicrosoft Azure szolgáltatásai. Ez a cikk bemutatja, hogyan hello lépéseket szükséges toopublish az alkalmazások tooAzure Docker tárolóként.

> [!NOTE]
> További információ a Docker érhető el hello [Docker webhely].
>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a>A webes alkalmazás tooAzure közzététele egy Docker-tároló használatával

1. Nyissa meg a webalkalmazás-projektet az eclipse-ben.

2. toostart hello **Docker-tároló közzététel** varázsló hello alábbi módszerekkel:

   * A hello **Navigator** megtekinteni, kattintson jobb gombbal a projektre, kattintson a **Azure**, és kattintson a **Docker-tároló közzététel**.

      ![Közzététel Docker-tároló parancsként Navigator megtekintése][PUB01]

   * Hello Eclipse eszköztáron kattintson a hello **közzététel** gombra, majd **Docker-tároló közzététel**.

      ![Közzététel Docker-tároló parancsként eclipse eszköztár][PUB02]
      
    Hello **Docker-tároló központi telepítése az Azure-on** varázsló megnyílik.

    ![hello Azure varázsló a Docker-tároló központi telepítése][PUB03]

3. A hello **írja be a lemezkép nevét, válassza ki a hello összetevő elérési útja, ellenőrizze a használt Docker állomás toobe** ablakban, a következő hello:

    a. A hello **Docker lemezképnév** mezőben adjon meg egy egyedi nevet a Docker-állomás. (hello varázsló automatikusan létrehozza a nevét, de módosíthatja.)

    b. Hello **állomások** terület tartalmazza a Docker állomásokon, már létrehozott. Hello alábbi módszerekkel:

    * Ha egy meglévő Docker-állomás, a webes alkalmazás tooit telepítheti.
    * Kattintson egy új Docker állomás toocreate **Hozzáadás**.  
      
    Hello **Docker állomás létrehozása** párbeszédpanel.

    ![Az Azure varázsló Docker-tároló telepítése][PUB04a]

4. A hello **hello új virtuális gép konfigurálása** ablakban adja meg az alábbi beállítások a Docker állomás hello. (hello varázsló automatikusan létrehozza a legtöbb hello lehetőségek, de egyikük módosíthatja.)

   a. **Név**: Adjon meg egy egyedi nevet hello Docker állomás. (Fontos nem ugyanaz, mint a korábban megadott Docker lemezképnév hello hello.)

   b. **Előfizetés**: Adja meg a hello Azure-előfizetéssel, amelyekkel az állomáshoz.

   c. **A régióban**: hello földrajzi régiót, ahol az állomás-e meg.

   d. A hello **állomás operációs rendszer és a méret** lapon:
     * **Az operációs rendszer üzemeltetéséhez**: hello operációs rendszer hello virtuális gépet, amely tartalmazza a gazdagépet adjon meg.
     * **Méret**: hello virtuális gép méretét adja meg a gazdagépen.

   e. A hello **erőforráscsoport** lapon:
     * **Új erőforráscsoport**: hozzon létre egy új erőforráscsoportot az állomáshoz.
     * **Meglévő erőforráscsoport**: Írjon be egy meglévő erőforráscsoportot az Azure-fiókjával.

   f. A hello **hálózati** lapon:
     * **Új virtuális hálózat**: új virtuális hálózat létrehozása az állomáshoz.
     * **Meglévő virtuális hálózat**: Írjon be egy meglévő virtuális hálózatot az Azure-fiókjával.

   g. A hello **tárolási** lapon:
     * **Új tárfiók**: hozzon létre egy új tárfiókot az állomáshoz.
     * **Meglévő tárfiók**: Írjon be egy meglévő tárfiókot az Azure-fiókjával.

5. Kattintson a **Tovább** gombra.

6. A hello **napló konfigurálja a hitelesítő adatokat, és a portbeállítások** ablak, válasszon egyet az alábbi beállítások hello:

    * **Hitelesítő adatok importálása az Azure Key Vault**: egy korábban mentett kumulatív hitelesítő adatokat, amelyek az Azure-előfizetéshez vannak tárolva.

      >[!NOTE]
      >Egy Azure Key Vault, amely egy meghatározott fiók vagy a szolgáltatás egyszerű nincs egy másik fiókot vagy egyszerű szolgáltatásnév osztja meg a hello előfizetés automatikusan elérhető. tooallow egy másik fiókot, vagy a szolgáltatás egyszerű toouse hello Key Vault, hello Azure portál tooadd hello fiók vagy a szolgáltatásnevet kell használnia.

    * **Új bejelentkezési hitelesítő adatok**: hoz létre új bejelentkezési hitelesítő adatokat. Ha ezt a lehetőséget választja, a hello, a következő:
    
      * A hello **VM hitelesítő adatok** lapra, majd a hello következő hello virtuális gép bejelentkezési hitelesítő adatokat a Docker-állomás beállításai közül:

          * **Felhasználónév**: hello felhasználónév megadása a virtuális gép bejelentkezési hitelesítő adatait.
          * **Jelszó** és **megerősítése**: hello jelszó megadása a virtuális gép bejelentkezési hitelesítő adatait.
          * **SSH**: Adja meg a Docker állomás hello Secure Shell (SSH) beállításait. Hello alábbi beállítások közül választhat:
            * **Nincs**: meghatározza, hogy a virtuális gép nem teszi lehetővé SSH-kapcsolatokhoz.
            * **Automatikus generálása**: hozza létre automatikusan hello szükséges beállítások SSH-kapcsolaton keresztül csatlakozni.
            * **Importálás a directory**: Adja meg a korábban mentett SSH-beállítások készletét tartalmazó könyvtár. hello directory tartalmaznia kell a következő két fájlok hello:
                * *id_rsa*: hello RSA azonosítás engedélyezése a felhasználó a tartalmazza.
                * *id_rsa.pub*: hello RSA nyilvános kulcsot tartalmaz, amelyek használják a hitelesítéshez.
        
        ![Docker állomás létrehozása][PUB05]

      * A hello **Docker démon hitelesítő adatok** lapra, adja meg az alábbi beállítások hello:

          * **Docker démon port**: Adjon meg egyedi hello TCP-port a Docker-állomás.
          * **A TLS biztonsági**: Adja meg a Docker állomás hello Transport Layer Security-beállításokat. Hello alábbi beállítások közül választhat:
            * **Nincs**: Megadja, hogy a virtuális gép nem engedélyezi TLS-kapcsolatokhoz.
            * **Automatikus generálása**: hozza létre automatikusan hello szükséges beállításokat a TLS keresztül kapcsolódik.
            * **Importálás a directory**: Adja meg a korábban mentett TLS beállítások készletét tartalmazó könyvtár. Pontosabban hello directory következő hat fájlok hello kell tartalmaznia:
                * *CA.PEM* és *ca-key.pem*: hello tanúsítvány és nyilvános kulcs a hello TLS hitelesítésszolgáltatót tartalmaz.
                * *CERT.PEM* és *key.pem*: hello ügyféltanúsítványok és a TLS-hitelesítéshez használt nyilvános kulcsot tartalmazza.
                * *Server.PEM* és *kiszolgáló-key.pem*: hello kiszolgálói tanúsítvány és nyilvános kulcs hello állomás.

        ![Docker állomás létrehozása][PUB06]

7. Miután megadta az összes információt megelőző hello, kattintson **Befejezés**.

8. A hello **Docker-tároló központi telepítése az Azure-on** varázsló, kattintson a **következő**.

   ![hello Azure varázsló a Docker-tároló központi telepítése][PUB07]

9. A hello **hello Docker-tároló toobe létrehozott konfigurálása** ablakban, a következő hello:

   a. A hello **Docker-tároló neve** mezőben adjon meg egy egyedi nevet a Docker-tároló.

   b. A következő Docker képek hello közül választhat:
     * **Előre definiált Docker kép**: Megadja a már meglévő Docker-lemezkép az Azure-ból.

       >[!NOTE]
       >hello Docker képek ebben a mezőben a lista az számos olyan rendszerkép található, hogy hello Azure eszközkészlet lett konfigurálva toopatch, hogy a rendszer automatikusan telepíti az összetevő.

     * **Egyéni Dockerfile**: a helyi számítógépről egy korábban mentett Dockerfile határozza meg.

       >[!NOTE]
       >Ez a tulajdonság speciális fejlesztők számára toodeploy saját Dockerfile. Azt azonban ez a beállítás tooensure azok Dockerfile megfelelően részét képező használó toodevelopers legfeljebb lesz. hello Azure eszközkészlet nem felel meg egy egyéni Dockerfile hello tartalma, hello telepítés esetleg sikertelen lesz, ha hello Dockerfile problémákkal rendelkezik. Ezenkívül hello Azure eszközkészlet vár hello egyéni Dockerfile toocontain egy webes alkalmazás-összetevő, és megkísérel tooopen HTTP-kapcsolatot. Ha a fejlesztők közzététele összetevő más típusú, a telepítés utáni jelenhet ártalmatlan hibák.

   c. **Portbeállításokat**: hello egyedi TCP port kötést a Docker-tároló adja meg.

     ![hello konfigurálása hello Docker tároló létrehozott toobe ablak][PUB08]

10. Kattintson a befejezését követően az összes lépést megelőző hello, **Befejezés**.

hello Azure eszközkészlet megkezdi a központi telepítését a webes alkalmazás tooAzure egy Docker-tároló. 

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

Azure Java használatával kapcsolatos további információkért lásd: [Azure Java fejlesztői központból] és [Java Tools for Visual Studio Team Services].

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