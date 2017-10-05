---
title: "Az eclipse-ben az Azure eszközkészlet telepítése |} Microsoft Docs"
description: "Megtudhatja, hogyan telepítse az Azure-eszközkészlet az eclipse-ben."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 9e93ff6a-f42b-4d99-b55b-624136b4a730
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: 35cddba38c364dfb2f6a8646b0014d48ca4cb795
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="installing-the-azure-toolkit-for-eclipse"></a>Az eclipse-ben az Azure eszközkészlet telepítése
Az Eclipse Azure eszköztára és sablonok, amelyek lehetővé teszik könnyen létrehozása, fejlesztése és tesztelése, és központi telepítése az Azure-alkalmazások használata az Eclipse fejlesztői környezetet biztosít a. Az Eclipse Azure eszköztára nyílt forráskódú projektként. A forráskód érhető el a MIT licence alapján <https://github.com/microsoft/azure-tools-for-java>.

A következő lépések bemutatják az eclipse-ben az Azure eszközkészlet telepítése.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a>Az eclipse-ben az Azure eszközkészlet telepítése
1. Indítsa el az Eclipse.
2. Kattintson a **súgó** menüben, majd kattintson **új szoftverek telepítése**, a következő ábrán látható módon.
   
    ![Az eclipse-ben az Azure eszközkészlet telepítése][01]
3. Az a **elérhető szoftverek** párbeszédpanel, melyhez a **együttműködve** szövegmezőben `http://dl.microsoft.com/eclipse` követ a **Enter** kulcs.
4. Az a **neve** ablaktáblán jelölje **Eclipse Azure eszköztára**, és törölje a jelet **lépjen kapcsolatba az összes frissítés hely található a szükséges szoftverek telepítése során**. A képernyőn a következőhöz hasonlóan kell megjelennie:
   
    ![Az eclipse-ben az Azure eszközkészlet telepítése][02]
5. Ha kibontja a **Eclipse Azure eszköztára**, látni fogja a következő elemek:
   
   * **Javához készült Application Insights beépülő modul**: Ez az összetevő lehetővé teszi, hogy az Azure telemetriai naplózása és elemzése az alkalmazások és a kiszolgáló-példány szolgáltatások.
   * **Az Azure Access Control szolgáltatások szűrő**: Ez az összetevő-támogatást biztosít az alkalmazás használatához Azure ACS, engedélyezése egyszeri bejelentkezéshez forgatókönyvek és az alkalmazás identitását logika externalizing.
   * **Az Azure közös beépülő modul**: Ez az összetevő biztosítja a szükséges egyéb eszközkészlet összetevők közös funkciókat.
   * **Az Azure-kezelőjét az Eclipse**: Ez az összetevő biztosítja a szükséges egyéb eszközkészlet összetevők közös funkciókat.
   * **Azure Java Eclipse beépülő modul**: Ez az összetevő támogatást nyújt az projektek, amelyek segítenek a létrehozása, tesztelése és telepítése a Microsoft Azure felhőbe az eclipse-ben és a parancssorból Java-alkalmazások fejlesztésével.
   * **Az Azure Web Apps Plugin Java**: Ez az összetevő támogatást nyújt a Microsoft Azure Web Apps tárolók Java webes alkalmazások telepítése.
   * **Microsoft SQL Server JDBC illesztőprogram 4.2**: Java Platform Enterprise Edition 8 Ez az összetevő biztosítja az SQL Server és a Microsoft Azure SQL Database JDBC API.
   * **Az Apache Qpid ügyfél függvénytárainak JMS csomag**: Ez az összetevő biztosítja az JMS ügyféloldali összetevők ahhoz, hogy az alkalmazás használhatja a Microsoft Azure messaging AMQP Apache Qpid projektből.
   * **A Microsoft Azure-könyvtárakban Java csomag**: Ez az összetevő biztosítja API-k eléréséhez a Microsoft Azure-szolgáltatások, például a tárolás, a service bus, szolgáltatás futásideje, stb.
6. Kattintson a **Tovább** gombra. (Ha az eszközkészlet telepítésével szokatlan késést tapasztal, ellenőrizze, hogy **lépjen kapcsolatba az összes frissítés hely található a szükséges szoftverek telepítése során** nincs bejelölve.)
7. Az a **telepítése részletek** párbeszédpanel, kattintson a **következő**.
   
    ![Tekintse át a telepítés részleteit][03]
8. Az a **licencek áttekintése** párbeszédpanelen tekintse át a Licencszerződések feltételeit. Ha elfogadja a licencszerződés feltételeit, kattintson az **elfogadom a licencszerződés feltételeit** majd **Befejezés**. (A többi lépések feltételezik, fogadja el a licencszerződés feltételeit. Amennyiben nem fogadja el a licencszerződés feltételeit, lépjen ki a telepítési folyamatot.)
   
    ![Licencek áttekintése][04]
   
    Eclipse letölti és telepíti a szükséges csomagokat.
   
    ![Folyamatban lévő telepítés][05]
9. Ha a telepítés befejezéséhez indítsa újra a Eclipse kéri, kattintson a **Igen**.
   
    ![Indítsa újra a parancssorba][06]

## <a name="see-also"></a>Lásd még:
A Java IDE környezetekhez készült Azure-eszközkészlettel kapcsolatos további információkért lásd az alábbi hivatkozásokat:

* [Eclipse Azure eszköztára]
  * [Az Eclipse-hez készült Azure-eszközkészlet újdonságai]
  * *Az Azure eszközkészlet telepítése az eclipse-ben (Ez a cikk)*
  * [Bejelentkezési utasítások az Eclipse-hez készült Azure-eszközkészlethez]
  * [Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]
* [Az IntelliJ-hez készült Azure-eszközkészlet]
  * [Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]
  * [Az IntelliJ-hez készült Azure-eszközkészlet telepítése]
  * [Bejelentkezési utasítások az IntelliJ-hez készült Azure-eszközkészlethez]
  * [Hello World webalkalmazás létrehozása az intellij-t az Azure]

Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ].

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md
[Bejelentkezési utasítások az Eclipse-hez készült Azure-eszközkészlethez]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Bejelentkezési utasítások az IntelliJ-hez készült Azure-eszközkészlethez]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Az Eclipse-hez készült Azure-eszközkészlet újdonságai]: ./azure-toolkit-for-eclipse-whats-new.md
[Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->
