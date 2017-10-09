---
title: "aaaSign az utasításokat a hello Azure eszköztára Eclipse |} Microsoft Docs"
description: "Ismerje meg, hogyan használatával a Microsoft Azure toosign hello Azure eszköztára eclipse-ben."
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
ms.openlocfilehash: 95be64750ca0147f76dae8f364fad80cb9ccc969
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sign-in-instructions-for-hello-azure-toolkit-for-eclipse"></a>Az Azure bejelentkezési az utasításokat hello Eclipse Azure eszköztára

hello Azure eszköztára eclipse-ben az Azure-fiókjával bejelentkezik két módszert biztosít:

  * **Interaktív** – Ez a módszer használata esetén megadja a Azure hitelesítő adatait az Azure-fiókjába történő minden egyes bejelentkezéskor.
  * **Automatikus** – Ez a módszer használata esetén létrehozhat egy hitelesítőadat-fájlt, amely a szolgáltatás egyszerű adatokat tartalmaz, amely után használhatja hello hitelesítő adatok fájl tooautomatically jelentkezzen be Azure-fiókjába.

hello hello a következő részekben leírt lépéseket ismerteti, hogyan toouse mindegyik módszerről.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="signing-into-your-azure-account-interactively"></a>Az Azure-fiókjával interaktívan bejelentkezni

hello következőket mutatják be hogyan toosign az Azure kézzel írja be Azure hitelesítő adatait.

1. Nyissa meg a projekt eclipse-ben.

1. Kattintson a **eszközök**, majd kattintson a **Azure**, és kattintson a **bejelentkezés**.

   ![Az Azure bejelentkezési eclipse menü][I01]

1. Ha hello **Azure bejelentkezés** kiválasztása párbeszédpanel jelenik meg, **interaktív**, és kattintson a **bejelentkezés**.

   ![Jelentkezzen be a párbeszédpanelt][I02]

1. Ha hello **Azure bejelentkezés** párbeszédpanel jelenik meg, az Azure hitelesítő adatait, és kattintson **bejelentkezés**.

   ![Azure bejelentkezési párbeszédpanel][I03]

1. Ha hello **válasszon előfizetések** párbeszédpanel jelenik meg, jelölje be hello előfizetések toouse szeretne, és kattintson **OK**.

   ![Az előfizetések kiválasztása párbeszédpanel][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a>Aláírási kívül az Azure-fiókjával, amikor interaktívan jelentkezik be

Miután konfigurálta a hello lépéseket hello előző szakaszban, fogja automatikusan kijelentkezteti az Eclipse újraindítását minden alkalommal, amikor Azure-fiókjával. Azonban toosign kívül az Azure-fiókjával Eclipse újraindítása nélkül, használja a következő lépéseket hello.

1. Az eclipse-ben kattintson **eszközök**, majd kattintson a **Azure**, és kattintson a **Kijelentkezés**.

   ![Az Azure kijelentkezés eclipse menü][L01]

1. Ha hello **Azure Kijelentkezés** párbeszédpanel, kattintson a **Igen**.

   ![Jelentkezzen ki párbeszédpanel][L02]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-toouse-in-hello-future"></a>Automatikusan jelentkezik be az Azure-fiókjával, és a hitelesítő adatok egy fájlt toouse hello jövőbeli

hello következő lépésekkel haladhat végig vezeti egy hitelesítőadat-fájlt, amely tartalmazza a szolgáltatás egyszerű adatait. Miután végrehajtotta ezeket a lépéseket, Eclipse lesz automatikusan használata hello hitelesítő adatok fájl tooautomatically bejelentkezési meg az Azure minden alkalommal, amikor nyissa meg a projektet.

1. Nyissa meg a projekt eclipse-ben.

1. Kattintson a **eszközök**, majd kattintson a **Azure**, és kattintson a **bejelentkezés**.

   ![Az Azure bejelentkezési eclipse menü][A01]

1. Ha hello **Azure bejelentkezés** kiválasztása párbeszédpanel jelenik meg, **automatikus**, és kattintson a **új**.

   ![Jelentkezzen be a párbeszédpanelt][A02]

1. Ha hello **Azure bejelentkezés** párbeszédpanel jelenik meg, az Azure hitelesítő adatait, és kattintson **bejelentkezés**.

   ![Azure bejelentkezési párbeszédpanel][A03]

1. Ha hello **hitelesítési-fájlok létrehozása** párbeszédpanel jelenik meg, hogy azt szeretné, hogy toouse, válassza ki a célkönyvtárat, és kattintson az előfizetések válassza hello **Start**.

   ![Azure bejelentkezési párbeszédpanel][A04]

1. Hello **egyszerű Creatation állapot** párbeszédpanel jelenik meg, miután a fájlok sikeresen létrejött, majd **OK**.

   ![Szolgáltatás egyszerű Creatation állapota párbeszédpanel][A05]

1. Ha hello **Azure bejelentkezés** párbeszédpanel, kattintson a **bejelentkezés**.

   ![Azure bejelentkezési párbeszédpanel][A06]

1. Ha hello **válasszon előfizetések** párbeszédpanel jelenik meg, jelölje be hello előfizetések toouse szeretne, és kattintson **OK**.

   ![Az előfizetések kiválasztása párbeszédpanel][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a>Aláírási kívül az Azure-fiókjával, ha automatikusan jelentkezett be

Miután konfigurálta a hello lépéseket hello előző szakaszban, hello Azure eszközkészlet, automatikusan bejelentkezik az Eclipse újraindítását minden alkalommal, amikor az Azure-fiókjával. Azonban toosign kívüli az Azure-fiókjával, és megakadályozza, hogy a hello Azure eszközkészlet automatikusan, a következő lépéseket használata hello bejelentkeztetése közben.

1. Az eclipse-ben kattintson **eszközök**, majd kattintson a **Azure**, és kattintson a **Kijelentkezés**.

   ![Az Azure kijelentkezés eclipse menü][L01]

1. Ha hello **Azure Kijelentkezés** párbeszédpanel, kattintson a **Igen**.

   ![Jelentkezzen ki párbeszédpanel][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a>Az Azure-fiók használatával automatikusan egy hitelesítőadat-fájlt, amely már létrehozott bejelentkezni

Ha regisztrál az Azure-ból Eclipse használata esetén, tooreconfigure hello Azure eszközkészlet Eclipse toouse egy hitelesítőadat-fájlt, amely hozott létre automatikusan az Azure-fiókot a következő bejelentkezés előtt szüksége lesz. hello lépések haladhat végig hello Azure eszközkészlet toouse konfigurálása egy meglévő hitelesítőadat-fájlt.

1. Nyissa meg a projekt eclipse-ben.

1. Kattintson a **eszközök**, majd kattintson a **Azure**, és kattintson a **bejelentkezés**.

   ![Az Azure bejelentkezési eclipse menü][A01]

1. Ha hello **Azure bejelentkezés** kiválasztása párbeszédpanel jelenik meg, **automatikus**, és kattintson a **Tallózás**.

   ![Jelentkezzen be a párbeszédpanelt][A02]

1. Ha hello **hitelesített fájl kiválasztása** párbeszédpanel jelenik meg, válassza ki a hitelesítőadat-fájlt, amely a korábban létrehozott, és kattintson a **válasszon**.

   ![Jelentkezzen be a párbeszédpanelt][A08]

1. Ha hello **Azure bejelentkezés** párbeszédpanel, kattintson a **bejelentkezés**.

   ![Azure bejelentkezési párbeszédpanel][A06]

1. Ha hello **válasszon előfizetések** párbeszédpanel jelenik meg, jelölje be hello előfizetések toouse szeretne, és kattintson **OK**.

   ![Az előfizetések kiválasztása párbeszédpanel][A07]

## <a name="see-also"></a>Lásd még:
A Java IDEs hello Azure eszközök gazdag kapcsolatos további információkért tekintse meg a következő hivatkozások hello:

* [Eclipse Azure eszköztára]
  * [Újdonságok az Eclipse Azure eszköztára hello]
  * [Hello Azure eszköztára Eclipse telepítése]
  * *Bejelentkezés az utasításokat hello Azure eszköztára Eclipse (Ez a cikk)*
  * [Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]
* [Az IntelliJ-hez készült Azure-eszközkészlet]
  * [Újdonságok az intellij-t Azure eszköztára hello]
  * [Az IntelliJ hello Azure eszközkészlet telepítése]
  * [Bejelentkezés a utasításokat hello IntelliJ Azure eszköztára]
  * [Hello World webalkalmazás létrehozása az intellij-t az Azure]

Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból] és hello [Java Tools for Visual Studio Team Services].

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Hello Azure eszköztára Eclipse telepítése]: ./azure-toolkit-for-eclipse-installation.md
[Az IntelliJ hello Azure eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md
[Sign In Instructions for hello Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Bejelentkezés a utasításokat hello IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Újdonságok az Eclipse Azure eszköztára hello]: ./azure-toolkit-for-eclipse-whats-new.md
[Újdonságok az intellij-t Azure eszköztára hello]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/
[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/

<!-- IMG List -->

[I01]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I01.png
[I02]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I02.png
[I03]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I03.png
[I04]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/I04.png

[A01]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A01.png
[A02]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A02.png
[A03]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A03.png
[A04]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A04.png
[A05]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A05.png
[A06]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A06.png
[A07]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A07.png
[A08]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/A08.png

[L01]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/L01.png
[L02]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/L02.png
[L03]: ./media/azure-toolkit-for-eclipse-sign-in-instructions/L03.png
