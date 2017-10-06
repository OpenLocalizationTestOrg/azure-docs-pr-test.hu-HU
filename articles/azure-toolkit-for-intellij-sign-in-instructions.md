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
# <a name="sign-in-instructions-for-hello-azure-toolkit-for-intellij"></a><span data-ttu-id="6cd43-103">Bejelentkezési utasításokat hello Azure eszközkészlet az intellij-t</span><span class="sxs-lookup"><span data-stu-id="6cd43-103">Sign-in instructions for hello Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="6cd43-104">hello Azure eszköztára IntelliJ bejelentkezés az Azure-fiók tooyour két módszert biztosít:</span><span class="sxs-lookup"><span data-stu-id="6cd43-104">hello Azure Toolkit for IntelliJ provides two methods for signing in tooyour Azure account:</span></span>

  * <span data-ttu-id="6cd43-105">**Interaktív**: meg hitelesítő adatait az Azure minden egyes bejelentkezéskor tooyour Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="6cd43-105">**Interactive**: You enter your Azure credentials each time you sign in tooyour Azure account.</span></span>
  * <span data-ttu-id="6cd43-106">**Automatikus**: használható tooautomatically bejelentkezési tooyour Azure-fiók a hitelesítő adatok fájlt hoz létre.</span><span class="sxs-lookup"><span data-stu-id="6cd43-106">**Automated**: You create a credentials file that you can use tooautomatically sign in tooyour Azure account.</span></span>

<span data-ttu-id="6cd43-107">hello alábbi szakaszok azt ismertetik, hogyan toouse mindegyik módszerről.</span><span class="sxs-lookup"><span data-stu-id="6cd43-107">hello following sections describe how toouse each method.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="sign-in-tooyour-azure-account-interactively"></a><span data-ttu-id="6cd43-108">Interaktív bejelentkezés tooyour Azure-fiók</span><span class="sxs-lookup"><span data-stu-id="6cd43-108">Sign in tooyour Azure account interactively</span></span>

<span data-ttu-id="6cd43-109">az Azure hitelesítő adatait, a manuális megadásával tooAzure toosign hello a következő:</span><span class="sxs-lookup"><span data-stu-id="6cd43-109">toosign in tooAzure by manually entering your Azure credentials, do hello following:</span></span>

1. <span data-ttu-id="6cd43-110">Nyissa meg a projektet az IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="6cd43-110">Open your project with IntelliJ IDEA.</span></span>

2. <span data-ttu-id="6cd43-111">Kattintson a **eszközök**, pont túl**Azure**, és kattintson a **Azure bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-111">Click **Tools**, point too**Azure**, and then click **Azure Sign In**.</span></span>

   ![hello IntelliJ Azure bejelentkezés parancs][I01]

3. <span data-ttu-id="6cd43-113">A hello **Azure bejelentkezés** ablakban válassza ki **interaktív**, és kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-113">In hello **Azure Sign In** window, select **Interactive**, and then click **Sign in**.</span></span>

   ![a kiválasztott interaktív hello Azure bejelentkezés ablak][I02]

4. <span data-ttu-id="6cd43-115">A hello **Azure bejelentkezés** párbeszédpanel jelenik meg, az Azure hitelesítő adatait, és kattintson **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-115">In hello **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![hello Azure bejelentkezési párbeszédpanel][I03]

5. <span data-ttu-id="6cd43-117">A hello **válasszon előfizetések** párbeszédpanel megnyitásához, jelölje be hello előfizetés toouse szeretne, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-117">In hello **Select Subscriptions** dialog box, select hello subscriptions that you want toouse, and then click **OK**.</span></span>

   ![hello előfizetések kiválasztása párbeszédpanel][I04]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-interactively"></a><span data-ttu-id="6cd43-119">Jelentkezzen ki az Azure-fiókjával, miután bejelentkezett interaktív módon</span><span class="sxs-lookup"><span data-stu-id="6cd43-119">Sign out of your Azure account after you have signed in interactively</span></span>

<span data-ttu-id="6cd43-120">Miután konfigurálta a fiókját előző lépésekben hello segítségével, akkor rendszer automatikusan kijelentkezteti az Azure-fiókjával IntelliJ IDEA minden újraindításakor.</span><span class="sxs-lookup"><span data-stu-id="6cd43-120">After you have configured your account by using hello preceding steps, you will be automatically signed out of your Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="6cd43-121">Azonban ha azt szeretné, toosign kívül az Azure-fiókjával *nélkül* IntelliJ IDEA újraindítása a következő hello.</span><span class="sxs-lookup"><span data-stu-id="6cd43-121">However, if you want toosign out of your Azure account *without* restarting IntelliJ IDEA, do hello following.</span></span>

1. <span data-ttu-id="6cd43-122">Az IntelliJ IDEA a hello **eszközök** menüben mutasson túl**Azure**, és kattintson a **Azure Kijelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-122">In IntelliJ IDEA, on hello **Tools** menu, point too**Azure**, and then click **Azure Sign Out**.</span></span>

   ![hello IntelliJ Azure Kijelentkezés parancs][L01]

2. <span data-ttu-id="6cd43-124">A hello **Azure Kijelentkezés** ablak, kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-124">In hello **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![hello Azure kijelentkezés ablak][L02]

## <a name="sign-in-tooyour-azure-account-automatically"></a><span data-ttu-id="6cd43-126">Jelentkezzen be Azure-fiók tooyour automatikusan</span><span class="sxs-lookup"><span data-stu-id="6cd43-126">Sign in tooyour Azure account automatically</span></span>

<span data-ttu-id="6cd43-127">Ez a szakasz végigvezeti egy hitelesítő adatait tartalmazó fájlt, a szolgáltatás egyszerű adatainak létrehozása.</span><span class="sxs-lookup"><span data-stu-id="6cd43-127">This section walks you through creating a credentials file that contains your service principal data.</span></span> <span data-ttu-id="6cd43-128">Ez a folyamat befejezését követően az eclipse-ben használt hello hitelesítő adatok fájl tooautomatically bejelentkezési a tooAzure minden alkalommal, amikor nyissa meg a projektet.</span><span class="sxs-lookup"><span data-stu-id="6cd43-128">After you have completed this process, Eclipse uses hello credentials file tooautomatically sign you in tooAzure each time you open your project.</span></span>

1. <span data-ttu-id="6cd43-129">Nyissa meg a projektet az IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="6cd43-129">Open your project with IntelliJ IDEA.</span></span>

2. <span data-ttu-id="6cd43-130">A hello **eszközök** menüben mutasson túl**Azure**, és kattintson a **Azure bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-130">On hello **Tools** menu, point too**Azure**, and then click **Azure Sign In**.</span></span>

   ![hello IntelliJ Azure bejelentkezés parancs][A01]

3. <span data-ttu-id="6cd43-132">A hello **Azure bejelentkezés** ablakban válassza ki **automatikus**, és kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-132">In hello **Azure Sign In** window, select **Automated**, and then click **New**.</span></span>

   ![a kiválasztott automatikus hello Azure bejelentkezés ablak][A02]

4. <span data-ttu-id="6cd43-134">A hello **Azure bejelentkezési párbeszédpanel** ablakban adja meg Azure hitelesítő adatait, és kattintson **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-134">In hello **Azure Login Dialog** window, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![hello Azure bejelentkezési párbeszédpanel][A03]

5. <span data-ttu-id="6cd43-136">A hello **hitelesítési fájlok létrehozása** ablakban, a select hello előfizetések, hogy azt szeretné, hogy toouse, válassza ki a célkönyvtárat, és kattintson **Start**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-136">In hello **Create Authentication Files** window, select hello subscriptions that you want toouse, choose your destination directory, and then click **Start**.</span></span>

   ![hello hitelesítési fájlok létrehozása ablak][A04]

6. <span data-ttu-id="6cd43-138">A hello **egyszerű szolgáltatás létrehozása állapot** párbeszédpanel, miután a fájlok sikeresen létrejött, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-138">In hello **Service Principal Creation Status** dialog box, after your files have been created successfully, click **OK**.</span></span>

   ![hello egyszerű szolgáltatás létrehozása párbeszédpanel][A05]

7. <span data-ttu-id="6cd43-140">A hello **Azure bejelentkezés** ablak, kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-140">In hello **Azure Sign In** window, click **Sign in**.</span></span>

   ![Azure bejelentkezési párbeszédpanel][A06]

8. <span data-ttu-id="6cd43-142">A hello **válasszon előfizetések** párbeszédpanel megnyitásához, jelölje be hello előfizetés toouse szeretne, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-142">In hello **Select Subscriptions** dialog box, select hello subscriptions that you want toouse, and then click **OK**.</span></span>

   ![hello előfizetések kiválasztása párbeszédpanel][A07]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-automatically"></a><span data-ttu-id="6cd43-144">Jelentkezzen ki az Azure-fiókjával, miután bejelentkezett automatikusan</span><span class="sxs-lookup"><span data-stu-id="6cd43-144">Sign out of your Azure account after you have signed in automatically</span></span>

<span data-ttu-id="6cd43-145">Miután konfigurálta a fiókját előző lépésekben hello segítségével, hello Azure eszközkészlet automatikusan bejelentkezik a tooyour IntelliJ IDEA minden újraindításakor Azure-fiók.</span><span class="sxs-lookup"><span data-stu-id="6cd43-145">After you have configured your account by using hello preceding steps, hello Azure Toolkit automatically signs you in tooyour Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="6cd43-146">Azonban a toosign kívüli az Azure-fiókjával, és megelőzheti a hello Azure eszközkészlet automatikus bejelentkezés, a hello a következő:</span><span class="sxs-lookup"><span data-stu-id="6cd43-146">However, toosign out of your Azure account and prevent hello Azure Toolkit from signing you in automatically, do hello following:</span></span>

1. <span data-ttu-id="6cd43-147">Az IntelliJ IDEA a hello **eszközök** menüben mutasson túl**Azure**, és kattintson a **Azure Kijelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-147">In IntelliJ IDEA, on hello **Tools** menu, point too**Azure**, and then click **Azure Sign Out**.</span></span>

   ![hello IntelliJ Azure Kijelentkezés parancs][L01]

2. <span data-ttu-id="6cd43-149">A hello **Azure Kijelentkezés** ablak, kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-149">In hello **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![hello Azure kijelentkezés ablak][L03]

## <a name="sign-in-tooyour-azure-account-automatically-by-using-an-existing-credentials-file"></a><span data-ttu-id="6cd43-151">Tooyour Azure-fiók automatikus bejelentkezés meglévő hitelesítő adatok fájl segítségével</span><span class="sxs-lookup"><span data-stu-id="6cd43-151">Sign in tooyour Azure account automatically by using an existing credentials file</span></span>

<span data-ttu-id="6cd43-152">IntelliJ IDEA használatakor kijelentkezni az Azure-fiókjával, ha egy meglévő hitelesítő adatok tooautomatically bejelentkezési vissza a toohello fiókot kell használnia.</span><span class="sxs-lookup"><span data-stu-id="6cd43-152">If you sign out of your Azure account when you are using IntelliJ IDEA, you must use an existing credentials file tooautomatically sign back in toohello account.</span></span> <span data-ttu-id="6cd43-153">az Eclipse toouse meglévő hitelesítőadat-fájlt tooconfigure hello Azure eszközkészlet hello a következő:</span><span class="sxs-lookup"><span data-stu-id="6cd43-153">tooconfigure hello Azure Toolkit for Eclipse toouse an existing credentials file, do hello following:</span></span>

1. <span data-ttu-id="6cd43-154">Nyissa meg a projektet az IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="6cd43-154">Open your project with IntelliJ IDEA.</span></span>

2. <span data-ttu-id="6cd43-155">A hello **eszközök** menüben mutasson túl**Azure**, és kattintson a **Azure bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-155">On hello **Tools** menu, point too**Azure**, and then click **Azure Sign In**.</span></span>

   ![hello IntelliJ Azure bejelentkezés parancs][A01]

3. <span data-ttu-id="6cd43-157">A hello **Azure bejelentkezés** ablakban válassza ki **automatikus**, és kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-157">In hello **Azure Sign In** window, select **Automated**, and then click **Browse**.</span></span>

   ![a kiválasztott automatikus hello Azure bejelentkezés ablak][A02]

4. <span data-ttu-id="6cd43-159">A hello **hitelesítési fájl kiválasztása** párbeszédpanelen válassza ki a korábban létrehozott hitelesítő adatokat tartalmazó fájlt, és kattintson **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-159">In hello **Select Authentication File** dialog box, select a previously created credentials file, and then click **Select**.</span></span>

   ![hello hitelesítési fájl kiválasztása párbeszédpanel][A08]

5. <span data-ttu-id="6cd43-161">A hello **Azure bejelentkezés** ablak, kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-161">In hello **Azure Sign In** window, click **Sign in**.</span></span>

   ![a kiválasztott automatikus hello Azure bejelentkezés ablak][A06]

6. <span data-ttu-id="6cd43-163">A hello **válasszon előfizetések** párbeszédpanel megnyitásához, jelölje be hello előfizetés toouse szeretne, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="6cd43-163">In hello **Select Subscriptions** dialog box, select hello subscriptions that you want toouse, and then click **OK**.</span></span>

   ![hello előfizetések kiválasztása párbeszédpanel][A07]

## <a name="next-steps"></a><span data-ttu-id="6cd43-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="6cd43-165">Next steps</span></span>
<span data-ttu-id="6cd43-166">A Java IDEs hello Azure eszközök gazdag kapcsolatos további információkért tekintse meg a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="6cd43-166">For more information about hello Azure Toolkits for Java IDEs, see hello following links:</span></span>

* <span data-ttu-id="6cd43-167">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="6cd43-167">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="6cd43-168">[What's new in hello Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="6cd43-168">[What's new in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="6cd43-169">[Hello Azure eszköztára Eclipse telepítése]</span><span class="sxs-lookup"><span data-stu-id="6cd43-169">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="6cd43-170">[Hello Azure eszköztára Eclipse bejelentkezési utasítások]</span><span class="sxs-lookup"><span data-stu-id="6cd43-170">[Sign-in instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="6cd43-171">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="6cd43-171">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="6cd43-172">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="6cd43-172">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="6cd43-173">[What's new in hello IntelliJ Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="6cd43-173">[What's new in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="6cd43-174">[Az IntelliJ hello Azure eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="6cd43-174">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="6cd43-175">*Bejelentkezési utasításokat hello Azure eszközkészlet az IntelliJ* (Ez a cikk)</span><span class="sxs-lookup"><span data-stu-id="6cd43-175">*Sign-in instructions for hello Azure Toolkit for IntelliJ* (this article)</span></span>
  * <span data-ttu-id="6cd43-176">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="6cd43-176">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="6cd43-177">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból] és hello [Java Tools for Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="6cd43-177">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

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
