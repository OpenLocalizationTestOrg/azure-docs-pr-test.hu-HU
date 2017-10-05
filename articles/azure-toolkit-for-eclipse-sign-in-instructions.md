---
title: "Jelentkezzen be az Azure eszközkészlet utasításokat az Eclipse |} Microsoft Docs"
description: "Útmutató a Microsoft Azure az eclipse-ben az Azure-eszközkészlet használatával írja alá."
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
ms.openlocfilehash: 02dd9935086c4c40d9ed54cc9ff2412ca96889f5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a>Az Azure jelentkezzen be az Azure eszközkészlet utasításokat az eclipse-ben

Az Azure-eszközkészlet az eclipse-ben az Azure-fiókjával bejelentkezik két módszert biztosít:

  * **Interaktív** – Ez a módszer használata esetén megadja a Azure hitelesítő adatait az Azure-fiókjába történő minden egyes bejelentkezéskor.
  * **Automatikus** – Ez a módszer használata esetén létrehozhat egy hitelesítőadat-fájlt, amely a szolgáltatás egyszerű adatokat tartalmaz, amely után az a hitelesítő adatok fájl segítségével automatikusan jelentkezzen be az Azure-fiókjával.

A lépéseket a következő szakaszokban azt ismerteti, hogyan minden módszer használatát.

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="signing-into-your-azure-account-interactively"></a>Az Azure-fiókjával interaktívan bejelentkezni

Az alábbi lépéseket fogja bemutatják, hogyan lehet jelentkezzen be Azure hitelesítő adatait az Azure manuális megadásával.

1. Nyissa meg a projekt eclipse-ben.

1. Kattintson a **eszközök**, majd kattintson a **Azure**, és kattintson a **bejelentkezés**.

   ![Az Azure bejelentkezési eclipse menü][I01]

1. Ha a **Azure bejelentkezés** kiválasztása párbeszédpanel jelenik meg, **interaktív**, és kattintson a **bejelentkezés**.

   ![Jelentkezzen be a párbeszédpanelt][I02]

1. Ha a **Azure bejelentkezés** párbeszédpanel jelenik meg, az Azure hitelesítő adatait, és kattintson **bejelentkezés**.

   ![Azure bejelentkezési párbeszédpanel][I03]

1. Ha a **válasszon előfizetések** párbeszédpanel jelenik meg, válassza ki, amelyet szeretne használni, és kattintson az előfizetések **OK**.

   ![Az előfizetések kiválasztása párbeszédpanel][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a>Aláírási kívül az Azure-fiókjával, amikor interaktívan jelentkezik be

Miután konfigurálta a lépéseket az előző szakaszban, akkor lesz automatikusan kijelentkezteti az Eclipse újraindítását minden alkalommal, amikor Azure-fiókjával. Azonban ha azt szeretné, az Azure-fiókjával kijelentkezik Eclipse újraindítása nélkül, az alábbi lépésekkel.

1. Az eclipse-ben kattintson **eszközök**, majd kattintson a **Azure**, és kattintson a **Kijelentkezés**.

   ![Az Azure kijelentkezés eclipse menü][L01]

1. Ha a **Azure Kijelentkezés** párbeszédpanel, kattintson a **Igen**.

   ![Jelentkezzen ki párbeszédpanel][L02]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-to-use-in-the-future"></a>Automatikusan jelentkezik be Azure-fiókja és a hitelesítő adatok fájl létrehozásakor a jövőben használni

A következő lépésekkel haladhat végig létrehozása egy hitelesítőadat-fájlt, amely tartalmazza a szolgáltatás egyszerű adatait. Ha az alábbi lépéseket, automatikusan bejelentkezik az Azure minden alkalommal, amikor megnyitja a projekt Eclipse automatikusan az a hitelesítőadat-fájlt használja.

1. Nyissa meg a projekt eclipse-ben.

1. Kattintson a **eszközök**, majd kattintson a **Azure**, és kattintson a **bejelentkezés**.

   ![Az Azure bejelentkezési eclipse menü][A01]

1. Ha a **Azure bejelentkezés** kiválasztása párbeszédpanel jelenik meg, **automatikus**, és kattintson a **új**.

   ![Jelentkezzen be a párbeszédpanelt][A02]

1. Ha a **Azure bejelentkezés** párbeszédpanel jelenik meg, az Azure hitelesítő adatait, és kattintson **bejelentkezés**.

   ![Azure bejelentkezési párbeszédpanel][A03]

1. Ha a **hitelesítési-fájlok létrehozása** párbeszédpanel jelenik meg, válassza ki, amelyet szeretne használni, válassza ki a célkönyvtárat, és kattintson az előfizetések **Start**.

   ![Azure bejelentkezési párbeszédpanel][A04]

1. A **egyszerű Creatation állapot** párbeszédpanel jelenik meg, miután a fájlok sikeresen létrejött, majd **OK**.

   ![Szolgáltatás egyszerű Creatation állapota párbeszédpanel][A05]

1. Ha a **Azure bejelentkezés** párbeszédpanel, kattintson a **bejelentkezés**.

   ![Azure bejelentkezési párbeszédpanel][A06]

1. Ha a **válasszon előfizetések** párbeszédpanel jelenik meg, válassza ki, amelyet szeretne használni, és kattintson az előfizetések **OK**.

   ![Az előfizetések kiválasztása párbeszédpanel][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a>Aláírási kívül az Azure-fiókjával, ha automatikusan jelentkezett be

Miután konfigurálta a lépéseket az előző szakaszban, az Azure-eszközkészlet, automatikusan bejelentkezik az Eclipse újraindítását minden alkalommal, amikor az Azure-fiókjával. Azonban jelentkezzen ki az Azure-fiókjával, és az Azure-eszközkészlet megakadályozza az automatikus bejelentkezés, az alábbi lépésekkel.

1. Az eclipse-ben kattintson **eszközök**, majd kattintson a **Azure**, és kattintson a **Kijelentkezés**.

   ![Az Azure kijelentkezés eclipse menü][L01]

1. Ha a **Azure Kijelentkezés** párbeszédpanel, kattintson a **Igen**.

   ![Jelentkezzen ki párbeszédpanel][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a>Az Azure-fiók használatával automatikusan egy hitelesítőadat-fájlt, amely már létrehozott bejelentkezni

Ha regisztrál az Azure-ból Eclipse használata esetén, szüksége lesz újrakonfigurálása az Azure-eszközkészlet az eclipse-ben a hitelesítőadat-fájlt, amely hozott létre, mielőtt automatikus bejelentkezés az Azure-fiókot használni. A következő lépésekkel haladhat végig konfigurálása az Azure-eszközkészlet meglévő hitelesítő adatok fájl.

1. Nyissa meg a projekt eclipse-ben.

1. Kattintson a **eszközök**, majd kattintson a **Azure**, és kattintson a **bejelentkezés**.

   ![Az Azure bejelentkezési eclipse menü][A01]

1. Ha a **Azure bejelentkezés** kiválasztása párbeszédpanel jelenik meg, **automatikus**, és kattintson a **Tallózás**.

   ![Jelentkezzen be a párbeszédpanelt][A02]

1. Ha a **hitelesített fájl kiválasztása** párbeszédpanel jelenik meg, válassza ki a hitelesítőadat-fájlt, amely a korábban létrehozott, és kattintson a **válasszon**.

   ![Jelentkezzen be a párbeszédpanelt][A08]

1. Ha a **Azure bejelentkezés** párbeszédpanel, kattintson a **bejelentkezés**.

   ![Azure bejelentkezési párbeszédpanel][A06]

1. Ha a **válasszon előfizetések** párbeszédpanel jelenik meg, válassza ki, amelyet szeretne használni, és kattintson az előfizetések **OK**.

   ![Az előfizetések kiválasztása párbeszédpanel][A07]

## <a name="see-also"></a>Lásd még:
A Java IDE környezetekhez készült Azure-eszközkészlettel kapcsolatos további információkért lásd az alábbi hivatkozásokat:

* [Eclipse Azure eszköztára]
  * [Az Eclipse-hez készült Azure-eszközkészlet újdonságai]
  * [Az Eclipse-hez készült Azure-eszközkészlet telepítése]
  * *Jelentkezzen be az Azure eszközkészlet utasításokat az eclipse-ben (Ez a cikk)*
  * [Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]
* [Az IntelliJ-hez készült Azure-eszközkészlet]
  * [Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]
  * [Az IntelliJ-hez készült Azure-eszközkészlet telepítése]
  * [Bejelentkezési utasítások az IntelliJ-hez készült Azure-eszközkészlethez]
  * [Hello World webalkalmazás létrehozása az intellij-t az Azure]

Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ] és [Java-eszközök a Visual Studio Team Serviceshez].

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Az Eclipse-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-eclipse-installation.md
[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md
[Sign In Instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Bejelentkezési utasítások az IntelliJ-hez készült Azure-eszközkészlethez]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Az Eclipse-hez készült Azure-eszközkészlet újdonságai]: ./azure-toolkit-for-eclipse-whats-new.md
[Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/
[Java-eszközök a Visual Studio Team Serviceshez]: https://java.visualstudio.com/

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
