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
# <a name="azure-sign-in-instructions-for-hello-azure-toolkit-for-eclipse"></a><span data-ttu-id="1a7a2-103">Az Azure bejelentkezési az utasításokat hello Eclipse Azure eszköztára</span><span class="sxs-lookup"><span data-stu-id="1a7a2-103">Azure Sign In Instructions for hello Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="1a7a2-104">hello Azure eszköztára eclipse-ben az Azure-fiókjával bejelentkezik két módszert biztosít:</span><span class="sxs-lookup"><span data-stu-id="1a7a2-104">hello Azure Toolkit for Eclipse provides two methods for signing into your Azure account:</span></span>

  * <span data-ttu-id="1a7a2-105">**Interaktív** – Ez a módszer használata esetén megadja a Azure hitelesítő adatait az Azure-fiókjába történő minden egyes bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-105">**Interactive** - when you are using this method, you will enter your Azure credentials each time you sign into your Azure account.</span></span>
  * <span data-ttu-id="1a7a2-106">**Automatikus** – Ez a módszer használata esetén létrehozhat egy hitelesítőadat-fájlt, amely a szolgáltatás egyszerű adatokat tartalmaz, amely után használhatja hello hitelesítő adatok fájl tooautomatically jelentkezzen be Azure-fiókjába.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-106">**Automated** - when you are using this method, you will create a credentials file which contains your service principal data, after which you can use hello credentials file tooautomatically sign into your Azure account.</span></span>

<span data-ttu-id="1a7a2-107">hello hello a következő részekben leírt lépéseket ismerteti, hogyan toouse mindegyik módszerről.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-107">hello steps in hello following sections will describe how toouse each method.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="signing-into-your-azure-account-interactively"></a><span data-ttu-id="1a7a2-108">Az Azure-fiókjával interaktívan bejelentkezni</span><span class="sxs-lookup"><span data-stu-id="1a7a2-108">Signing into your Azure account interactively</span></span>

<span data-ttu-id="1a7a2-109">hello következőket mutatják be hogyan toosign az Azure kézzel írja be Azure hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-109">hello following steps will illustrate how toosign into Azure by manually entering your Azure credentials.</span></span>

1. <span data-ttu-id="1a7a2-110">Nyissa meg a projekt eclipse-ben.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-110">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="1a7a2-111">Kattintson a **eszközök**, majd kattintson a **Azure**, és kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-111">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Az Azure bejelentkezési eclipse menü][I01]

1. <span data-ttu-id="1a7a2-113">Ha hello **Azure bejelentkezés** kiválasztása párbeszédpanel jelenik meg, **interaktív**, és kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-113">When hello **Azure Sign In** dialog box appears, select **Interactive**, and then click **Sign In**.</span></span>

   ![Jelentkezzen be a párbeszédpanelt][I02]

1. <span data-ttu-id="1a7a2-115">Ha hello **Azure bejelentkezés** párbeszédpanel jelenik meg, az Azure hitelesítő adatait, és kattintson **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-115">When hello **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign In**.</span></span>

   ![Azure bejelentkezési párbeszédpanel][I03]

1. <span data-ttu-id="1a7a2-117">Ha hello **válasszon előfizetések** párbeszédpanel jelenik meg, jelölje be hello előfizetések toouse szeretne, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-117">When hello **Select Subscriptions** dialog box appears, select hello subscriptions that you want toouse, and then click **OK**.</span></span>

   ![Az előfizetések kiválasztása párbeszédpanel][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a><span data-ttu-id="1a7a2-119">Aláírási kívül az Azure-fiókjával, amikor interaktívan jelentkezik be</span><span class="sxs-lookup"><span data-stu-id="1a7a2-119">Signing out of your Azure account when you signed in interactively</span></span>

<span data-ttu-id="1a7a2-120">Miután konfigurálta a hello lépéseket hello előző szakaszban, fogja automatikusan kijelentkezteti az Eclipse újraindítását minden alkalommal, amikor Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-120">After you have configured hello steps in hello previous section, you will automatically signed out of your Azure account each time you restart Eclipse.</span></span> <span data-ttu-id="1a7a2-121">Azonban toosign kívül az Azure-fiókjával Eclipse újraindítása nélkül, használja a következő lépéseket hello.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-121">However, if you want toosign out of your Azure account without restarting Eclipse, use hello following steps.</span></span>

1. <span data-ttu-id="1a7a2-122">Az eclipse-ben kattintson **eszközök**, majd kattintson a **Azure**, és kattintson a **Kijelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-122">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Az Azure kijelentkezés eclipse menü][L01]

1. <span data-ttu-id="1a7a2-124">Ha hello **Azure Kijelentkezés** párbeszédpanel, kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-124">When hello **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![Jelentkezzen ki párbeszédpanel][L02]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-toouse-in-hello-future"></a><span data-ttu-id="1a7a2-126">Automatikusan jelentkezik be az Azure-fiókjával, és a hitelesítő adatok egy fájlt toouse hello jövőbeli</span><span class="sxs-lookup"><span data-stu-id="1a7a2-126">Signing into your Azure account automatically and creating a credentials file toouse in hello future</span></span>

<span data-ttu-id="1a7a2-127">hello következő lépésekkel haladhat végig vezeti egy hitelesítőadat-fájlt, amely tartalmazza a szolgáltatás egyszerű adatait.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-127">hello following steps will walk you through creating a credentials file which contains your service principal data.</span></span> <span data-ttu-id="1a7a2-128">Miután végrehajtotta ezeket a lépéseket, Eclipse lesz automatikusan használata hello hitelesítő adatok fájl tooautomatically bejelentkezési meg az Azure minden alkalommal, amikor nyissa meg a projektet.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-128">Once you have completed these steps, Eclipse will automatically use hello credentials file tooautomatically sign you into Azure each time you open your project.</span></span>

1. <span data-ttu-id="1a7a2-129">Nyissa meg a projekt eclipse-ben.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-129">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="1a7a2-130">Kattintson a **eszközök**, majd kattintson a **Azure**, és kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-130">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Az Azure bejelentkezési eclipse menü][A01]

1. <span data-ttu-id="1a7a2-132">Ha hello **Azure bejelentkezés** kiválasztása párbeszédpanel jelenik meg, **automatikus**, és kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-132">When hello **Azure Sign In** dialog box appears, select **Automated**, and then click **New**.</span></span>

   ![Jelentkezzen be a párbeszédpanelt][A02]

1. <span data-ttu-id="1a7a2-134">Ha hello **Azure bejelentkezés** párbeszédpanel jelenik meg, az Azure hitelesítő adatait, és kattintson **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-134">When hello **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign In**.</span></span>

   ![Azure bejelentkezési párbeszédpanel][A03]

1. <span data-ttu-id="1a7a2-136">Ha hello **hitelesítési-fájlok létrehozása** párbeszédpanel jelenik meg, hogy azt szeretné, hogy toouse, válassza ki a célkönyvtárat, és kattintson az előfizetések válassza hello **Start**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-136">When hello **Create authentication files** dialog box appears, select hello subscriptions that you want toouse, choose your destination directory, and then click **Start**.</span></span>

   ![Azure bejelentkezési párbeszédpanel][A04]

1. <span data-ttu-id="1a7a2-138">Hello **egyszerű Creatation állapot** párbeszédpanel jelenik meg, miután a fájlok sikeresen létrejött, majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-138">hello **Service Principal Creatation Status** dialog box will be displayed, and after your files have been created successfully, click **OK**.</span></span>

   ![Szolgáltatás egyszerű Creatation állapota párbeszédpanel][A05]

1. <span data-ttu-id="1a7a2-140">Ha hello **Azure bejelentkezés** párbeszédpanel, kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-140">When hello **Azure Sign In** dialog box appears, click **Sign In**.</span></span>

   ![Azure bejelentkezési párbeszédpanel][A06]

1. <span data-ttu-id="1a7a2-142">Ha hello **válasszon előfizetések** párbeszédpanel jelenik meg, jelölje be hello előfizetések toouse szeretne, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-142">When hello **Select Subscriptions** dialog box appears, select hello subscriptions that you want toouse, and then click **OK**.</span></span>

   ![Az előfizetések kiválasztása párbeszédpanel][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a><span data-ttu-id="1a7a2-144">Aláírási kívül az Azure-fiókjával, ha automatikusan jelentkezett be</span><span class="sxs-lookup"><span data-stu-id="1a7a2-144">Signing out of your Azure account when you signed in automatically</span></span>

<span data-ttu-id="1a7a2-145">Miután konfigurálta a hello lépéseket hello előző szakaszban, hello Azure eszközkészlet, automatikusan bejelentkezik az Eclipse újraindítását minden alkalommal, amikor az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-145">After you have configured hello steps in hello previous section, hello Azure Toolkit will automatically sign you into your Azure account each time you restart Eclipse.</span></span> <span data-ttu-id="1a7a2-146">Azonban toosign kívüli az Azure-fiókjával, és megakadályozza, hogy a hello Azure eszközkészlet automatikusan, a következő lépéseket használata hello bejelentkeztetése közben.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-146">However, toosign out of your Azure account and prevent hello Azure Toolkit from signing you in automatically, use hello following steps.</span></span>

1. <span data-ttu-id="1a7a2-147">Az eclipse-ben kattintson **eszközök**, majd kattintson a **Azure**, és kattintson a **Kijelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-147">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Az Azure kijelentkezés eclipse menü][L01]

1. <span data-ttu-id="1a7a2-149">Ha hello **Azure Kijelentkezés** párbeszédpanel, kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-149">When hello **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![Jelentkezzen ki párbeszédpanel][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a><span data-ttu-id="1a7a2-151">Az Azure-fiók használatával automatikusan egy hitelesítőadat-fájlt, amely már létrehozott bejelentkezni</span><span class="sxs-lookup"><span data-stu-id="1a7a2-151">Signing into your Azure account automatically using a credentials file which you have already created</span></span>

<span data-ttu-id="1a7a2-152">Ha regisztrál az Azure-ból Eclipse használata esetén, tooreconfigure hello Azure eszközkészlet Eclipse toouse egy hitelesítőadat-fájlt, amely hozott létre automatikusan az Azure-fiókot a következő bejelentkezés előtt szüksége lesz.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-152">If you sign out of Azure when you are using Eclipse, you will need tooreconfigure hello Azure Toolkit for Eclipse toouse a credentials file which have created before you can automatically sign into your Azure acccount.</span></span> <span data-ttu-id="1a7a2-153">hello lépések haladhat végig hello Azure eszközkészlet toouse konfigurálása egy meglévő hitelesítőadat-fájlt.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-153">hello following steps will walk you through configuring hello Azure Toolkit toouse an existing credentials file.</span></span>

1. <span data-ttu-id="1a7a2-154">Nyissa meg a projekt eclipse-ben.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-154">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="1a7a2-155">Kattintson a **eszközök**, majd kattintson a **Azure**, és kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-155">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Az Azure bejelentkezési eclipse menü][A01]

1. <span data-ttu-id="1a7a2-157">Ha hello **Azure bejelentkezés** kiválasztása párbeszédpanel jelenik meg, **automatikus**, és kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-157">When hello **Azure Sign In** dialog box appears, select **Automated**, and then click **Browse**.</span></span>

   ![Jelentkezzen be a párbeszédpanelt][A02]

1. <span data-ttu-id="1a7a2-159">Ha hello **hitelesített fájl kiválasztása** párbeszédpanel jelenik meg, válassza ki a hitelesítőadat-fájlt, amely a korábban létrehozott, és kattintson a **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-159">When hello **Select Authenticated File** dialog box appears, select a credentials file which you created earlier, and then click **Select**.</span></span>

   ![Jelentkezzen be a párbeszédpanelt][A08]

1. <span data-ttu-id="1a7a2-161">Ha hello **Azure bejelentkezés** párbeszédpanel, kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-161">When hello **Azure Sign In** dialog box appears, click **Sign In**.</span></span>

   ![Azure bejelentkezési párbeszédpanel][A06]

1. <span data-ttu-id="1a7a2-163">Ha hello **válasszon előfizetések** párbeszédpanel jelenik meg, jelölje be hello előfizetések toouse szeretne, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="1a7a2-163">When hello **Select Subscriptions** dialog box appears, select hello subscriptions that you want toouse, and then click **OK**.</span></span>

   ![Az előfizetések kiválasztása párbeszédpanel][A07]

## <a name="see-also"></a><span data-ttu-id="1a7a2-165">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="1a7a2-165">See Also</span></span>
<span data-ttu-id="1a7a2-166">A Java IDEs hello Azure eszközök gazdag kapcsolatos további információkért tekintse meg a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="1a7a2-166">For more information about hello Azure Toolkits for Java IDEs, see hello following links:</span></span>

* <span data-ttu-id="1a7a2-167">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="1a7a2-167">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="1a7a2-168">[Újdonságok az Eclipse Azure eszköztára hello]</span><span class="sxs-lookup"><span data-stu-id="1a7a2-168">[What's New in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="1a7a2-169">[Hello Azure eszköztára Eclipse telepítése]</span><span class="sxs-lookup"><span data-stu-id="1a7a2-169">[Installing hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="1a7a2-170">*Bejelentkezés az utasításokat hello Azure eszköztára Eclipse (Ez a cikk)*</span><span class="sxs-lookup"><span data-stu-id="1a7a2-170">*Sign In Instructions for hello Azure Toolkit for Eclipse (This Article)*</span></span>
  * <span data-ttu-id="1a7a2-171">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="1a7a2-171">[Create a Hello World Web App for Azure in Eclipse]</span></span>
* <span data-ttu-id="1a7a2-172">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="1a7a2-172">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="1a7a2-173">[Újdonságok az intellij-t Azure eszköztára hello]</span><span class="sxs-lookup"><span data-stu-id="1a7a2-173">[What's New in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="1a7a2-174">[Az IntelliJ hello Azure eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="1a7a2-174">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="1a7a2-175">[Bejelentkezés a utasításokat hello IntelliJ Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="1a7a2-175">[Sign In Instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="1a7a2-176">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="1a7a2-176">[Create a Hello World Web App for Azure in IntelliJ]</span></span>

<span data-ttu-id="1a7a2-177">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból] és hello [Java Tools for Visual Studio Team Services].</span><span class="sxs-lookup"><span data-stu-id="1a7a2-177">For more information about using Azure with Java, see hello [Azure Java Developer Center] and hello [Java Tools for Visual Studio Team Services].</span></span>

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
