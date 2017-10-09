---
title: "aaaInstalling hello Azure eszköztára Eclipse |} Microsoft Docs"
description: "Ismerje meg, hogyan tooinstall hello Azure eszköztára eclipse-ben."
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
ms.openlocfilehash: 6c195fab2b47fb5c42541a8cf52be4ec88d27b5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="installing-hello-azure-toolkit-for-eclipse"></a>Hello Azure eszköztára Eclipse telepítése
sablonok és a funkciókat, amelyek lehetővé teszik tooeasily létrehozásával hello Azure eszköztára Eclipse foglalja össze, fejlesztése, tesztelése és hello Eclipse fejlesztői környezetet használó Azure alkalmazások telepítéséhez. hello Azure eszköztára Eclipse nyílt forráskódú projektként. hello forráskód alatt hello MIT licenccel érhető el a <https://github.com/microsoft/azure-tools-for-java>.

hello lépések bemutatják, hogyan tooinstall hello Azure eszköztára eclipse-ben.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="tooinstall-hello-azure-toolkit-for-eclipse"></a>tooinstall hello Eclipse Azure eszköztára
1. Indítsa el az Eclipse.
2. Kattintson a hello **súgó** menüben, majd kattintson **új szoftverek telepítése**, ahogy az ábra a következő hello.
   
    ![Hello Azure eszköztára Eclipse telepítése][01]
3. A hello **elérhető szoftverek** párbeszédpaneljén hello **együttműködve** szövegmezőben `http://dl.microsoft.com/eclipse` hello követ **Enter** kulcs.
4. A hello **neve** ablaktáblán jelölje **Eclipse Azure eszköztára**, és törölje a jelet **minden frissítés során helyek forduljon toofind szükséges szoftver telepítéséhez**. A képernyő hasonló toohello következő kell megjelennie:
   
    ![Hello Azure eszköztára Eclipse telepítése][02]
5. Ha kibontja hello **Eclipse Azure eszköztára**, látni fogja a következő elemek hello:
   
   * **Javához készült Application Insights beépülő modul**: Ez az összetevő lehetővé teszi a toouse Azure telemetriai naplózása és elemzése az alkalmazások és a kiszolgáló-példány szolgáltatások.
   * **Az Azure Access Control szolgáltatások szűrő**: Ez az összetevő-támogatást biztosít az alkalmazás használatához Azure ACS, engedélyezése egyszeri bejelentkezéshez forgatókönyvek és externalizing identitás logika hello alkalmazásból.
   * **Az Azure közös beépülő modul**: Ez az összetevő hello más eszközkészlet összetevőkhöz szükséges közös funkciókat biztosítja.
   * **Az Azure-kezelőjét az Eclipse**: Ez az összetevő hello más eszközkészlet összetevőkhöz szükséges közös funkciókat biztosítja.
   * **Azure Java Eclipse beépülő modul**: Ez az összetevő létrehozása, tesztelése és hello Microsoft Azure felhőbe az eclipse-ben és a parancssorból a Java-alkalmazások központi telepítését megkönnyítő projektek fejlesztése támogatást nyújt.
   * **Az Azure Web Apps Plugin Java**: Ez az összetevő támogatást nyújt a Java webes alkalmazások tooMicrosoft Azure Web Apps tárolók telepítése.
   * **Microsoft SQL Server JDBC illesztőprogram 4.2**: Java Platform Enterprise Edition 8 Ez az összetevő biztosítja az SQL Server és a Microsoft Azure SQL Database JDBC API.
   * **Az Apache Qpid ügyfél függvénytárainak JMS csomag**: Ez az összetevő hello JMS ügyfél összetevőt hello Apache Qpid projekt tooenable az alkalmazás toouse AMQP üzenetküldést biztosít a Microsoft Azure-ban.
   * **A Microsoft Azure-könyvtárakban Java csomag**: Ez az összetevő biztosítja API-k eléréséhez a Microsoft Azure-szolgáltatások, például a tárolás, a service bus, szolgáltatás futásideje, stb.
6. Kattintson a **Tovább** gombra. (Ha szokatlan késések hello eszközkészlet telepítése közben, győződjön meg arról, hogy **minden frissítés során helyek forduljon toofind szükséges szoftver telepítéséhez** nincs bejelölve.)
7. A hello **telepítése részletek** párbeszédpanel, kattintson a **következő**.
   
    ![Tekintse át a telepítés részleteit][03]
8. A hello **licencek áttekintése** párbeszédpanelen tekintse át a hello hello Licencszerződések feltételeit. Ha hello hello licencszerződés feltételeinek elfogadásához kattintson **hello hello szerződés feltételeinek elfogadása** majd **Befejezés**. (hello fennmaradó lépések azt feltételezik hello hello licencszerződés feltételeinek elfogadásához. Ha nem fogadja el a hello hello Licencszerződések feltételeit, lépjen ki a telepítési folyamat hello.)
   
    ![Licencek áttekintése][04]
   
    Eclipse letölti és telepíti a szükséges hello csomagok.
   
    ![Folyamatban lévő telepítés][05]
9. Ha toorestart Eclipse toocomplete telepítési kéri, kattintson az **Igen**.
   
    ![Indítsa újra a parancssorba][06]

## <a name="see-also"></a>Lásd még:
A Java IDEs hello Azure eszközök gazdag kapcsolatos további információkért tekintse meg a következő hivatkozások hello:

* [Eclipse Azure eszköztára]
  * [Újdonságok az Eclipse Azure eszköztára hello]
  * *Hello Azure eszköztára Eclipse (Ez a cikk) telepítése*
  * [Bejelentkezés a utasításokat hello Eclipse Azure eszköztára]
  * [Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]
* [Az IntelliJ-hez készült Azure-eszközkészlet]
  * [Újdonságok az intellij-t Azure eszköztára hello]
  * [Az IntelliJ hello Azure eszközkészlet telepítése]
  * [Bejelentkezés a utasításokat hello IntelliJ Azure eszköztára]
  * [Hello World webalkalmazás létrehozása az intellij-t az Azure]

Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból].

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installing hello Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Az IntelliJ hello Azure eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md
[Bejelentkezés a utasításokat hello Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Bejelentkezés a utasításokat hello IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Újdonságok az Eclipse Azure eszköztára hello]: ./azure-toolkit-for-eclipse-whats-new.md
[Újdonságok az intellij-t Azure eszköztára hello]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->
