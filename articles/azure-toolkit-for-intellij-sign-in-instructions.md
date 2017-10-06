---
title: "aaaSign az utasításokat a hello Azure eszközkészlet IntelliJ |} Microsoft Docs"
description: "Ismerje meg, hogyan toosign tooMicrosoft Azure használatával a hello Azure eszközkészlet intellij-t."
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
ms.openlocfilehash: 2de781fc19267cce133b1e6456481497e165fce4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="sign-in-instructions-for-hello-azure-toolkit-for-intellij"></a>Bejelentkezési utasításokat hello Azure eszközkészlet az intellij-t

hello Azure eszköztára IntelliJ bejelentkezés az Azure-fiók tooyour két módszert biztosít:

  * **Interaktív**: meg hitelesítő adatait az Azure minden egyes bejelentkezéskor tooyour Azure-fiók.
  * **Automatikus**: használható tooautomatically bejelentkezési tooyour Azure-fiók a hitelesítő adatok fájlt hoz létre.

hello alábbi szakaszok azt ismertetik, hogyan toouse mindegyik módszerről.

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="sign-in-tooyour-azure-account-interactively"></a>Interaktív bejelentkezés tooyour Azure-fiók

az Azure hitelesítő adatait, a manuális megadásával tooAzure toosign hello a következő:

1. Nyissa meg a projektet az IntelliJ IDEA.

2. Kattintson a **eszközök**, pont túl**Azure**, és kattintson a **Azure bejelentkezés**.

   ![hello IntelliJ Azure bejelentkezés parancs][I01]

3. A hello **Azure bejelentkezés** ablakban válassza ki **interaktív**, és kattintson a **bejelentkezés**.

   ![a kiválasztott interaktív hello Azure bejelentkezés ablak][I02]

4. A hello **Azure bejelentkezés** párbeszédpanel jelenik meg, az Azure hitelesítő adatait, és kattintson **bejelentkezés**.

   ![hello Azure bejelentkezési párbeszédpanel][I03]

5. A hello **válasszon előfizetések** párbeszédpanel megnyitásához, jelölje be hello előfizetés toouse szeretne, és kattintson **OK**.

   ![hello előfizetések kiválasztása párbeszédpanel][I04]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-interactively"></a>Jelentkezzen ki az Azure-fiókjával, miután bejelentkezett interaktív módon

Miután konfigurálta a fiókját előző lépésekben hello segítségével, akkor rendszer automatikusan kijelentkezteti az Azure-fiókjával IntelliJ IDEA minden újraindításakor. Azonban ha azt szeretné, toosign kívül az Azure-fiókjával *nélkül* IntelliJ IDEA újraindítása a következő hello.

1. Az IntelliJ IDEA a hello **eszközök** menüben mutasson túl**Azure**, és kattintson a **Azure Kijelentkezés**.

   ![hello IntelliJ Azure Kijelentkezés parancs][L01]

2. A hello **Azure Kijelentkezés** ablak, kattintson a **Igen**.

   ![hello Azure kijelentkezés ablak][L02]

## <a name="sign-in-tooyour-azure-account-automatically"></a>Jelentkezzen be Azure-fiók tooyour automatikusan

Ez a szakasz végigvezeti egy hitelesítő adatait tartalmazó fájlt, a szolgáltatás egyszerű adatainak létrehozása. Ez a folyamat befejezését követően az eclipse-ben használt hello hitelesítő adatok fájl tooautomatically bejelentkezési a tooAzure minden alkalommal, amikor nyissa meg a projektet.

1. Nyissa meg a projektet az IntelliJ IDEA.

2. A hello **eszközök** menüben mutasson túl**Azure**, és kattintson a **Azure bejelentkezés**.

   ![hello IntelliJ Azure bejelentkezés parancs][A01]

3. A hello **Azure bejelentkezés** ablakban válassza ki **automatikus**, és kattintson a **új**.

   ![a kiválasztott automatikus hello Azure bejelentkezés ablak][A02]

4. A hello **Azure bejelentkezési párbeszédpanel** ablakban adja meg Azure hitelesítő adatait, és kattintson **bejelentkezés**.

   ![hello Azure bejelentkezési párbeszédpanel][A03]

5. A hello **hitelesítési fájlok létrehozása** ablakban, a select hello előfizetések, hogy azt szeretné, hogy toouse, válassza ki a célkönyvtárat, és kattintson **Start**.

   ![hello hitelesítési fájlok létrehozása ablak][A04]

6. A hello **egyszerű szolgáltatás létrehozása állapot** párbeszédpanel, miután a fájlok sikeresen létrejött, kattintson a **OK**.

   ![hello egyszerű szolgáltatás létrehozása párbeszédpanel][A05]

7. A hello **Azure bejelentkezés** ablak, kattintson a **bejelentkezés**.

   ![Azure bejelentkezési párbeszédpanel][A06]

8. A hello **válasszon előfizetések** párbeszédpanel megnyitásához, jelölje be hello előfizetés toouse szeretne, és kattintson **OK**.

   ![hello előfizetések kiválasztása párbeszédpanel][A07]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-automatically"></a>Jelentkezzen ki az Azure-fiókjával, miután bejelentkezett automatikusan

Miután konfigurálta a fiókját előző lépésekben hello segítségével, hello Azure eszközkészlet automatikusan bejelentkezik a tooyour IntelliJ IDEA minden újraindításakor Azure-fiók. Azonban a toosign kívüli az Azure-fiókjával, és megelőzheti a hello Azure eszközkészlet automatikus bejelentkezés, a hello a következő:

1. Az IntelliJ IDEA a hello **eszközök** menüben mutasson túl**Azure**, és kattintson a **Azure Kijelentkezés**.

   ![hello IntelliJ Azure Kijelentkezés parancs][L01]

2. A hello **Azure Kijelentkezés** ablak, kattintson a **Igen**.

   ![hello Azure kijelentkezés ablak][L03]

## <a name="sign-in-tooyour-azure-account-automatically-by-using-an-existing-credentials-file"></a>Tooyour Azure-fiók automatikus bejelentkezés meglévő hitelesítő adatok fájl segítségével

IntelliJ IDEA használatakor kijelentkezni az Azure-fiókjával, ha egy meglévő hitelesítő adatok tooautomatically bejelentkezési vissza a toohello fiókot kell használnia. az Eclipse toouse meglévő hitelesítőadat-fájlt tooconfigure hello Azure eszközkészlet hello a következő:

1. Nyissa meg a projektet az IntelliJ IDEA.

2. A hello **eszközök** menüben mutasson túl**Azure**, és kattintson a **Azure bejelentkezés**.

   ![hello IntelliJ Azure bejelentkezés parancs][A01]

3. A hello **Azure bejelentkezés** ablakban válassza ki **automatikus**, és kattintson a **Tallózás**.

   ![a kiválasztott automatikus hello Azure bejelentkezés ablak][A02]

4. A hello **hitelesítési fájl kiválasztása** párbeszédpanelen válassza ki a korábban létrehozott hitelesítő adatokat tartalmazó fájlt, és kattintson **válasszon**.

   ![hello hitelesítési fájl kiválasztása párbeszédpanel][A08]

5. A hello **Azure bejelentkezés** ablak, kattintson a **bejelentkezés**.

   ![a kiválasztott automatikus hello Azure bejelentkezés ablak][A06]

6. A hello **válasszon előfizetések** párbeszédpanel megnyitásához, jelölje be hello előfizetés toouse szeretne, és kattintson **OK**.

   ![hello előfizetések kiválasztása párbeszédpanel][A07]

## <a name="next-steps"></a>Következő lépések
A Java IDEs hello Azure eszközök gazdag kapcsolatos további információkért tekintse meg a következő hivatkozások hello:

* [Eclipse Azure eszköztára]
  * [What's new in hello Eclipse Azure eszköztára]
  * [Hello Azure eszköztára Eclipse telepítése]
  * [Hello Azure eszköztára Eclipse bejelentkezési utasítások]
  * [Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]
* [Az IntelliJ-hez készült Azure-eszközkészlet]
  * [What's new in hello IntelliJ Azure eszköztára]
  * [Az IntelliJ hello Azure eszközkészlet telepítése]
  * *Bejelentkezési utasításokat hello Azure eszközkészlet az IntelliJ* (Ez a cikk)
  * [Hello World webalkalmazás létrehozása az intellij-t az Azure]

Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból] és hello [Java Tools for Visual Studio Team Services].

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Hello Azure eszköztára Eclipse telepítése]: ./azure-toolkit-for-eclipse-installation.md
[Az IntelliJ hello Azure eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md
[Hello Azure eszköztára Eclipse bejelentkezési utasítások]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Sign-in instructions for hello Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[What's new in hello Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-whats-new.md
[What's new in hello IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/
[A Visual Studio Team Services Java-eszközök]: https://java.visualstudio.com/

<!-- IMG List -->

[I01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I01.png
[I02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I02.png
[I03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I03.png
[I04]: ./media/azure-toolkit-for-intellij-sign-in-instructions/I04.png

[A01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A01.png
[A02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A02.png
[A03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A03.png
[A04]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A04.png
[A05]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A05.png
[A06]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A06.png
[A07]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A07.png
[A08]: ./media/azure-toolkit-for-intellij-sign-in-instructions/A08.png

[L01]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L01.png
[L02]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L02.png
[L03]: ./media/azure-toolkit-for-intellij-sign-in-instructions/L03.png
