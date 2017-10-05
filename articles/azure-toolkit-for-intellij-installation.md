---
title: "Az intellij-t az Azure eszközkészlet telepítése |} Microsoft Docs"
description: "Útmutató az Azure-eszközkészlet telepítése az IntelliJ Idea."
services: 
documentationcenter: java
author: rmcmurray
manager: erikre
editor: 
ms.assetid: c6817c7b-f28c-4c06-8216-41c7a8117de3
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: Java
ms.topic: article
ms.date: 04/14/2017
ms.author: robmcm
ms.openlocfilehash: bf11a8580500f78c4a96a02953f221501eeffe6c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="installing-the-azure-toolkit-for-intellij"></a>Az intellij-t az Azure eszközkészlet telepítése
Az IntelliJ Azure eszköztára sablonok és engedélyezi, hogy könnyen létrehozása, fejlesztése és tesztelése, és az IntelliJ IDEA fejlesztési környezet használata Azure-alkalmazások telepítését biztosít. Az IntelliJ Azure eszköztára nyílt forráskódú projektként, amelynek forráskódját, akkor a MIT licenccel a Githubon a következő URL-címen a projekt helyről:

<https://github.com/Microsoft/Azure-Tools-for-Java>

Az intellij-t, a beállítások párbeszédpanelen megadott és a konfigurálás menüből kifejezésre a kezdőképernyőn; az Azure-eszközkészlet telepítése a két módszer Mindkét telepítési módszer a következő lépésekben bizonyítható.

[!INCLUDE [azure-toolkit-for-IntelliJ-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-settings-dialog-box"></a>A beállítások párbeszédpanelen megadott IntelliJ az Azure-eszközkészlet telepítése
1. Indítsa el az IntelliJ IDEA.
2. Az IntelliJ IDEA megnyitása után kattintson **fájl**, majd kattintson a **beállítások**.
   
    ![Nyissa meg az IntelliJ IDEA beállításai párbeszédpanel][01a]
3. A beállítások párbeszédpanelen kattintson a **beépülő modulok**, és kattintson a **adattárak Tallózás**.
   
    ![Az IntelliJ IDEA beállításai párbeszédpanel][02a]
4. Az a **Tallózás adattárak** párbeszédpanelen be a keresőmezőbe írja be a "Azure". Jelöljön ki **IntelliJ Azure eszköztára**, és kattintson a **telepítése**.
   
    ![Az intellij-t az Azure eszközkészlet keresése][03]
   
    IntelliJ IDEA egy párbeszédpanelen megjelenik a telepítési folyamat.
   
    ![Folyamatban lévő telepítés][04]
5. A telepítés befejezése után kattintson **indítsa újra az IntelliJ IDEA**.
   
    ![Indítsa újra az IntelliJ IDEA][05]
6. Kattintson a **OK** a beállítások párbeszédpanel bezárásához.
   
    ![Zárja be az IntelliJ IDEA beállításai párbeszédpanel][06]
7. Amikor a rendszer kéri, indítsa újra az IntelliJ IDEA vagy halassza el, kattintson **indítsa újra a**.
   
    ![Indítsa újra az IntelliJ IDEA][07]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-start-screen"></a>Az Azure-eszközkészlet telepítése az intellij-t a kezdőképernyőről
1. Indítsa el az IntelliJ IDEA.
2. Az IntelliJ IDEA indítási képernyő megjelenésekor kattintson **konfigurálása**, majd kattintson a **beépülő modulok**.
   
    ![Az IntelliJ IDEA beépülő modulok telepítése][01b]
3. Az a **beépülő modulok** párbeszédpanel, kattintson a **adattárak Tallózás**.
   
    ![Keresse meg az IntelliJ IDEA beépülő modul adattárak][02b]
4. Az a **Tallózás adattárak** párbeszédpanelen be a keresőmezőbe írja be a "Azure". Jelöljön ki **IntelliJ Azure eszköztára**, és kattintson a **telepítése**.
   
    ![Az intellij-t az Azure eszközkészlet keresése][03]
   
    IntelliJ IDEA egy párbeszédpanelen megjelenik a telepítési folyamat.
   
    ![Folyamatban lévő telepítés][04]
5. A telepítés befejezése után kattintson **indítsa újra az IntelliJ IDEA**.
   
    ![Indítsa újra az IntelliJ IDEA][05]
6. Amikor a rendszer kéri, indítsa újra az IntelliJ IDEA vagy halassza el, kattintson **indítsa újra a**.
   
    ![Indítsa újra az IntelliJ IDEA][07]

## <a name="see-also"></a>Lásd még:
A Java IDE környezetekhez készült Azure-eszközkészlettel kapcsolatos további információkért lásd az alábbi hivatkozásokat:

* [Eclipse Azure eszköztára]
  * [Az Eclipse-hez készült Azure-eszközkészlet újdonságai]
  * [Az Eclipse-hez készült Azure-eszközkészlet telepítése]
  * [Bejelentkezési utasítások az Eclipse-hez készült Azure-eszközkészlethez]
  * [Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]
* [Az IntelliJ-hez készült Azure-eszközkészlet]
  * [Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]
  * *Az intellij-t (Ez a cikk) az Azure eszközkészlet telepítése*
  * [Bejelentkezési utasítások az IntelliJ-hez készült Azure-eszközkészlethez]
  * [Hello World webalkalmazás létrehozása az intellij-t az Azure]

Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ].

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Az Eclipse-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-eclipse-installation.md
[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md
[Bejelentkezési utasítások az Eclipse-hez készült Azure-eszközkészlethez]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Bejelentkezési utasítások az IntelliJ-hez készült Azure-eszközkészlethez]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Az Eclipse-hez készült Azure-eszközkészlet újdonságai]: ./azure-toolkit-for-eclipse-whats-new.md
[Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01a]: ./media/azure-toolkit-for-intellij-installation/01-intellij-file-settings.png
[01b]: ./media/azure-toolkit-for-intellij-installation/01-intellij-configure-dropdown.png
[02a]: ./media/azure-toolkit-for-intellij-installation/02-intellij-settings-dialog.png
[02b]: ./media/azure-toolkit-for-intellij-installation/02-intellij-plugins-dialog.png
[03]: ./media/azure-toolkit-for-intellij-installation/03-intellij-browse-repositories.png
[04]: ./media/azure-toolkit-for-intellij-installation/04-install-progress.png
[05]: ./media/azure-toolkit-for-intellij-installation/05-restart-intellij.png
[06]: ./media/azure-toolkit-for-intellij-installation/06-intellij-settings-dialog.png
[07]: ./media/azure-toolkit-for-intellij-installation/07-restart-intellij.png
