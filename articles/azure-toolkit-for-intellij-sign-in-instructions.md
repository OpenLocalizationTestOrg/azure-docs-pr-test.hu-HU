---
title: "Bejelentkezési utasításokat az Azure-eszközkészlet az IntelliJ |} Microsoft Docs"
description: "Útmutató az intellij-t az Azure-eszközkészlet használatával a Microsoft Azure bejelentkezni."
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
ms.openlocfilehash: 4e2ed072bdaea0a71fef042c0c72b7656a42bbe8
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="sign-in-instructions-for-the-azure-toolkit-for-intellij"></a><span data-ttu-id="c9139-103">Bejelentkezési utasításokat az Azure-eszközkészlet az intellij-t</span><span class="sxs-lookup"><span data-stu-id="c9139-103">Sign-in instructions for the Azure Toolkit for IntelliJ</span></span>

<span data-ttu-id="c9139-104">Az Azure-eszközkészlet az intellij-t az Azure-fiókjába történő bejelentkezés két módszert biztosít:</span><span class="sxs-lookup"><span data-stu-id="c9139-104">The Azure Toolkit for IntelliJ provides two methods for signing in to your Azure account:</span></span>

  * <span data-ttu-id="c9139-105">**Interaktív**: Azure hitelesítő adatait az Azure-fiókjával bejelentkezik minden alkalommal megadniuk.</span><span class="sxs-lookup"><span data-stu-id="c9139-105">**Interactive**: You enter your Azure credentials each time you sign in to your Azure account.</span></span>
  * <span data-ttu-id="c9139-106">**Automatikus**: hoz létre egy hitelesítőadat-fájlt, amely segítségével automatikusan jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="c9139-106">**Automated**: You create a credentials file that you can use to automatically sign in to your Azure account.</span></span>

<span data-ttu-id="c9139-107">Az alábbi szakaszok azt ismertetik, hogyan minden módszer használatát.</span><span class="sxs-lookup"><span data-stu-id="c9139-107">The following sections describe how to use each method.</span></span>

[!INCLUDE [azure-toolkit-for-intellij-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="sign-in-to-your-azure-account-interactively"></a><span data-ttu-id="c9139-108">Interaktív bejelentkezés az Azure-fiókjába</span><span class="sxs-lookup"><span data-stu-id="c9139-108">Sign in to your Azure account interactively</span></span>

<span data-ttu-id="c9139-109">Jelentkezzen be Azure hitelesítő adatait az Azure manuális megadásával, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="c9139-109">To sign in to Azure by manually entering your Azure credentials, do the following:</span></span>

1. <span data-ttu-id="c9139-110">Nyissa meg a projektet az IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="c9139-110">Open your project with IntelliJ IDEA.</span></span>

2. <span data-ttu-id="c9139-111">Kattintson a **eszközök**, mutasson a **Azure**, és kattintson a **Azure bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c9139-111">Click **Tools**, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![Az intellij-t Azure bejelentkezés parancs][I01]

3. <span data-ttu-id="c9139-113">Az a **Azure bejelentkezés** ablakban válassza ki **interaktív**, és kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c9139-113">In the **Azure Sign In** window, select **Interactive**, and then click **Sign in**.</span></span>

   ![A kiválasztott interaktív Azure bejelentkezés ablakot][I02]

4. <span data-ttu-id="c9139-115">Az a **Azure bejelentkezés** párbeszédpanel jelenik meg, az Azure hitelesítő adatait, és kattintson **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c9139-115">In the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![Az Azure bejelentkezési párbeszédpanel][I03]

5. <span data-ttu-id="c9139-117">Az a **előfizetések kiválasztása** párbeszédpanel megnyitásához, válassza ki a használni kívánt előfizetést, és végül **OK**.</span><span class="sxs-lookup"><span data-stu-id="c9139-117">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Az előfizetések kiválasztása párbeszédpanel][I04]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-interactively"></a><span data-ttu-id="c9139-119">Jelentkezzen ki az Azure-fiókjával, miután bejelentkezett interaktív módon</span><span class="sxs-lookup"><span data-stu-id="c9139-119">Sign out of your Azure account after you have signed in interactively</span></span>

<span data-ttu-id="c9139-120">Miután konfigurálta a fiókját a fenti lépések segítségével, akkor rendszer automatikusan kijelentkezteti az Azure-fiókjával IntelliJ IDEA minden újraindításakor.</span><span class="sxs-lookup"><span data-stu-id="c9139-120">After you have configured your account by using the preceding steps, you will be automatically signed out of your Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="c9139-121">Azonban hogy szeretné-e az Azure-fiókjával kijelentkezni *nélkül* újraindítása IntelliJ IDEA, tegye a következőket.</span><span class="sxs-lookup"><span data-stu-id="c9139-121">However, if you want to sign out of your Azure account *without* restarting IntelliJ IDEA, do the following.</span></span>

1. <span data-ttu-id="c9139-122">Az IntelliJ IDEA a a **eszközök** menüben mutasson a **Azure**, és kattintson a **Azure Kijelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c9139-122">In IntelliJ IDEA, on the **Tools** menu, point to **Azure**, and then click **Azure Sign Out**.</span></span>

   ![Az intellij-t Azure Kijelentkezés parancs][L01]

2. <span data-ttu-id="c9139-124">Az a **Azure Kijelentkezés** ablak, kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="c9139-124">In the **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![Az Azure kijelentkezés ablak][L02]

## <a name="sign-in-to-your-azure-account-automatically"></a><span data-ttu-id="c9139-126">Automatikus bejelentkezés az Azure-fiókjába</span><span class="sxs-lookup"><span data-stu-id="c9139-126">Sign in to your Azure account automatically</span></span>

<span data-ttu-id="c9139-127">Ez a szakasz végigvezeti egy hitelesítő adatait tartalmazó fájlt, a szolgáltatás egyszerű adatainak létrehozása.</span><span class="sxs-lookup"><span data-stu-id="c9139-127">This section walks you through creating a credentials file that contains your service principal data.</span></span> <span data-ttu-id="c9139-128">Ez a folyamat befejezését követően eclipse-ben a hitelesítőadat-fájlt használatával automatikusan bejelentkezés az Azure a projekt minden egyes megnyitásakor.</span><span class="sxs-lookup"><span data-stu-id="c9139-128">After you have completed this process, Eclipse uses the credentials file to automatically sign you in to Azure each time you open your project.</span></span>

1. <span data-ttu-id="c9139-129">Nyissa meg a projektet az IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="c9139-129">Open your project with IntelliJ IDEA.</span></span>

2. <span data-ttu-id="c9139-130">Az a **eszközök** menüben mutasson a **Azure**, és kattintson a **Azure bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c9139-130">On the **Tools** menu, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![Az intellij-t Azure bejelentkezés parancs][A01]

3. <span data-ttu-id="c9139-132">Az a **Azure bejelentkezés** ablakban válassza ki **automatikus**, és kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="c9139-132">In the **Azure Sign In** window, select **Automated**, and then click **New**.</span></span>

   ![A kiválasztott automatikus Azure bejelentkezés ablakot][A02]

4. <span data-ttu-id="c9139-134">Az a **Azure bejelentkezési párbeszédpanel** ablakban adja meg Azure hitelesítő adatait, és kattintson **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c9139-134">In the **Azure Login Dialog** window, enter your Azure credentials, and then click **Sign in**.</span></span>

   ![Az Azure bejelentkezési párbeszédpanel][A03]

5. <span data-ttu-id="c9139-136">A a **hitelesítési fájlok létrehozása** ablakban válassza ki, amelyet szeretne használni, válassza ki a célkönyvtárat, és kattintson az előfizetések **Start**.</span><span class="sxs-lookup"><span data-stu-id="c9139-136">In the **Create Authentication Files** window, select the subscriptions that you want to use, choose your destination directory, and then click **Start**.</span></span>

   ![A hitelesítés-fájlok létrehozása ablak][A04]

6. <span data-ttu-id="c9139-138">Az a **egyszerű szolgáltatás létrehozása állapota** párbeszédpanel, miután a fájlok sikeresen létrejött, kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="c9139-138">In the **Service Principal Creation Status** dialog box, after your files have been created successfully, click **OK**.</span></span>

   ![Az egyszerű szolgáltatás létrehozása párbeszédpanel][A05]

7. <span data-ttu-id="c9139-140">Az a **Azure bejelentkezés** ablak, kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c9139-140">In the **Azure Sign In** window, click **Sign in**.</span></span>

   ![Azure bejelentkezési párbeszédpanel][A06]

8. <span data-ttu-id="c9139-142">Az a **előfizetések kiválasztása** párbeszédpanel megnyitásához, válassza ki a használni kívánt előfizetést, és végül **OK**.</span><span class="sxs-lookup"><span data-stu-id="c9139-142">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Az előfizetések kiválasztása párbeszédpanel][A07]

## <a name="sign-out-of-your-azure-account-after-you-have-signed-in-automatically"></a><span data-ttu-id="c9139-144">Jelentkezzen ki az Azure-fiókjával, miután bejelentkezett automatikusan</span><span class="sxs-lookup"><span data-stu-id="c9139-144">Sign out of your Azure account after you have signed in automatically</span></span>

<span data-ttu-id="c9139-145">Miután konfigurálta a fiókját a fenti lépések használatával, az Azure-eszközkészlet automatikusan bejelentkezik, az Azure-fiókjával IntelliJ IDEA minden újraindításakor.</span><span class="sxs-lookup"><span data-stu-id="c9139-145">After you have configured your account by using the preceding steps, the Azure Toolkit automatically signs you in to your Azure account each time you restart IntelliJ IDEA.</span></span> <span data-ttu-id="c9139-146">Azonban jelentkezzen ki az Azure-fiókjával, és az Azure-eszközkészlet megakadályozza az automatikus bejelentkezés, tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="c9139-146">However, to sign out of your Azure account and prevent the Azure Toolkit from signing you in automatically, do the following:</span></span>

1. <span data-ttu-id="c9139-147">Az IntelliJ IDEA a a **eszközök** menüben mutasson a **Azure**, és kattintson a **Azure Kijelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c9139-147">In IntelliJ IDEA, on the **Tools** menu, point to **Azure**, and then click **Azure Sign Out**.</span></span>

   ![Az intellij-t Azure Kijelentkezés parancs][L01]

2. <span data-ttu-id="c9139-149">Az a **Azure Kijelentkezés** ablak, kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="c9139-149">In the **Azure Sign Out** confirmation window, click **Yes**.</span></span>

   ![Az Azure kijelentkezés ablak][L03]

## <a name="sign-in-to-your-azure-account-automatically-by-using-an-existing-credentials-file"></a><span data-ttu-id="c9139-151">Jelentkezzen be az Azure-fiókjával automatikusan meglévő hitelesítő adatok fájl segítségével</span><span class="sxs-lookup"><span data-stu-id="c9139-151">Sign in to your Azure account automatically by using an existing credentials file</span></span>

<span data-ttu-id="c9139-152">IntelliJ IDEA használatakor kijelentkezni az Azure-fiókjával, ha egy meglévő hitelesítő adatok fájl automatikusan jelentkezzen be újra a a fiókot kell használnia.</span><span class="sxs-lookup"><span data-stu-id="c9139-152">If you sign out of your Azure account when you are using IntelliJ IDEA, you must use an existing credentials file to automatically sign back in to the account.</span></span> <span data-ttu-id="c9139-153">A meglévő hitelesítő adatok fájl az eclipse-ben az Azure eszközkészlet konfigurálásához tegye a következőket:</span><span class="sxs-lookup"><span data-stu-id="c9139-153">To configure the Azure Toolkit for Eclipse to use an existing credentials file, do the following:</span></span>

1. <span data-ttu-id="c9139-154">Nyissa meg a projektet az IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="c9139-154">Open your project with IntelliJ IDEA.</span></span>

2. <span data-ttu-id="c9139-155">Az a **eszközök** menüben mutasson a **Azure**, és kattintson a **Azure bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c9139-155">On the **Tools** menu, point to **Azure**, and then click **Azure Sign In**.</span></span>

   ![Az intellij-t Azure bejelentkezés parancs][A01]

3. <span data-ttu-id="c9139-157">Az a **Azure bejelentkezés** ablakban válassza ki **automatikus**, és kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="c9139-157">In the **Azure Sign In** window, select **Automated**, and then click **Browse**.</span></span>

   ![A kiválasztott automatikus Azure bejelentkezés ablakot][A02]

4. <span data-ttu-id="c9139-159">Az a **hitelesítési fájl kiválasztása** párbeszédpanelen válassza ki a korábban létrehozott hitelesítő adatokat tartalmazó fájlt, és kattintson **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="c9139-159">In the **Select Authentication File** dialog box, select a previously created credentials file, and then click **Select**.</span></span>

   ![A hitelesítési fájl kiválasztása párbeszédpanel][A08]

5. <span data-ttu-id="c9139-161">Az a **Azure bejelentkezés** ablak, kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="c9139-161">In the **Azure Sign In** window, click **Sign in**.</span></span>

   ![A kiválasztott automatikus Azure bejelentkezés ablakot][A06]

6. <span data-ttu-id="c9139-163">Az a **előfizetések kiválasztása** párbeszédpanel megnyitásához, válassza ki a használni kívánt előfizetést, és végül **OK**.</span><span class="sxs-lookup"><span data-stu-id="c9139-163">In the **Select Subscriptions** dialog box, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Az előfizetések kiválasztása párbeszédpanel][A07]

## <a name="next-steps"></a><span data-ttu-id="c9139-165">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c9139-165">Next steps</span></span>
<span data-ttu-id="c9139-166">A Java IDE környezetekhez készült Azure-eszközkészlettel kapcsolatos további információkért lásd az alábbi hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="c9139-166">For more information about the Azure Toolkits for Java IDEs, see the following links:</span></span>

* <span data-ttu-id="c9139-167">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="c9139-167">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="c9139-168">[What's new in Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="c9139-168">[What's new in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="c9139-169">[Az Eclipse-hez készült Azure-eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="c9139-169">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="c9139-170">[Az Eclipse Azure eszköztára bejelentkezési utasítások]</span><span class="sxs-lookup"><span data-stu-id="c9139-170">[Sign-in instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="c9139-171">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="c9139-171">[Create a Hello World web app for Azure in Eclipse]</span></span>
* <span data-ttu-id="c9139-172">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="c9139-172">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="c9139-173">[What's new in IntelliJ Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="c9139-173">[What's new in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="c9139-174">[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="c9139-174">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="c9139-175">*Bejelentkezési utasításokat az Azure-eszközkészlet az IntelliJ* (Ez a cikk)</span><span class="sxs-lookup"><span data-stu-id="c9139-175">*Sign-in instructions for the Azure Toolkit for IntelliJ* (this article)</span></span>
  * <span data-ttu-id="c9139-176">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="c9139-176">[Create a Hello World web app for Azure in IntelliJ]</span></span>

<span data-ttu-id="c9139-177">Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ] és [Java-eszközök a Visual Studio Team Serviceshez].</span><span class="sxs-lookup"><span data-stu-id="c9139-177">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

<span data-ttu-id="c9139-178">[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="c9139-178">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="c9139-179">[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="c9139-179">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="c9139-180">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="c9139-180">[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="c9139-181">[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="c9139-181">[Create a Hello World web app for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="c9139-182">[Az Eclipse-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="c9139-182">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="c9139-183">[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="c9139-183">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="c9139-184">[Az Eclipse Azure eszköztára bejelentkezési utasítások]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="c9139-184">[Sign-in instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
[Sign-in instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md
<span data-ttu-id="c9139-185">[What's new in Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="c9139-185">[What's new in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="c9139-186">[What's new in IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="c9139-186">[What's new in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="c9139-187">[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="c9139-187">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="c9139-188">[Java-eszközök a Visual Studio Team Serviceshez]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="c9139-188">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>

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
