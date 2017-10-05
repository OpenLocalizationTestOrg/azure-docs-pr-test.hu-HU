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
# <a name="azure-sign-in-instructions-for-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="016ea-103">Az Azure jelentkezzen be az Azure eszközkészlet utasításokat az eclipse-ben</span><span class="sxs-lookup"><span data-stu-id="016ea-103">Azure Sign In Instructions for the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="016ea-104">Az Azure-eszközkészlet az eclipse-ben az Azure-fiókjával bejelentkezik két módszert biztosít:</span><span class="sxs-lookup"><span data-stu-id="016ea-104">The Azure Toolkit for Eclipse provides two methods for signing into your Azure account:</span></span>

  * <span data-ttu-id="016ea-105">**Interaktív** – Ez a módszer használata esetén megadja a Azure hitelesítő adatait az Azure-fiókjába történő minden egyes bejelentkezéskor.</span><span class="sxs-lookup"><span data-stu-id="016ea-105">**Interactive** - when you are using this method, you will enter your Azure credentials each time you sign into your Azure account.</span></span>
  * <span data-ttu-id="016ea-106">**Automatikus** – Ez a módszer használata esetén létrehozhat egy hitelesítőadat-fájlt, amely a szolgáltatás egyszerű adatokat tartalmaz, amely után az a hitelesítő adatok fájl segítségével automatikusan jelentkezzen be az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="016ea-106">**Automated** - when you are using this method, you will create a credentials file which contains your service principal data, after which you can use the credentials file to automatically sign into your Azure account.</span></span>

<span data-ttu-id="016ea-107">A lépéseket a következő szakaszokban azt ismerteti, hogyan minden módszer használatát.</span><span class="sxs-lookup"><span data-stu-id="016ea-107">The steps in the following sections will describe how to use each method.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="signing-into-your-azure-account-interactively"></a><span data-ttu-id="016ea-108">Az Azure-fiókjával interaktívan bejelentkezni</span><span class="sxs-lookup"><span data-stu-id="016ea-108">Signing into your Azure account interactively</span></span>

<span data-ttu-id="016ea-109">Az alábbi lépéseket fogja bemutatják, hogyan lehet jelentkezzen be Azure hitelesítő adatait az Azure manuális megadásával.</span><span class="sxs-lookup"><span data-stu-id="016ea-109">The following steps will illustrate how to sign into Azure by manually entering your Azure credentials.</span></span>

1. <span data-ttu-id="016ea-110">Nyissa meg a projekt eclipse-ben.</span><span class="sxs-lookup"><span data-stu-id="016ea-110">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="016ea-111">Kattintson a **eszközök**, majd kattintson a **Azure**, és kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="016ea-111">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Az Azure bejelentkezési eclipse menü][I01]

1. <span data-ttu-id="016ea-113">Ha a **Azure bejelentkezés** kiválasztása párbeszédpanel jelenik meg, **interaktív**, és kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="016ea-113">When the **Azure Sign In** dialog box appears, select **Interactive**, and then click **Sign In**.</span></span>

   ![Jelentkezzen be a párbeszédpanelt][I02]

1. <span data-ttu-id="016ea-115">Ha a **Azure bejelentkezés** párbeszédpanel jelenik meg, az Azure hitelesítő adatait, és kattintson **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="016ea-115">When the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign In**.</span></span>

   ![Azure bejelentkezési párbeszédpanel][I03]

1. <span data-ttu-id="016ea-117">Ha a **válasszon előfizetések** párbeszédpanel jelenik meg, válassza ki, amelyet szeretne használni, és kattintson az előfizetések **OK**.</span><span class="sxs-lookup"><span data-stu-id="016ea-117">When the **Select Subscriptions** dialog box appears, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Az előfizetések kiválasztása párbeszédpanel][I04]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-interactively"></a><span data-ttu-id="016ea-119">Aláírási kívül az Azure-fiókjával, amikor interaktívan jelentkezik be</span><span class="sxs-lookup"><span data-stu-id="016ea-119">Signing out of your Azure account when you signed in interactively</span></span>

<span data-ttu-id="016ea-120">Miután konfigurálta a lépéseket az előző szakaszban, akkor lesz automatikusan kijelentkezteti az Eclipse újraindítását minden alkalommal, amikor Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="016ea-120">After you have configured the steps in the previous section, you will automatically signed out of your Azure account each time you restart Eclipse.</span></span> <span data-ttu-id="016ea-121">Azonban ha azt szeretné, az Azure-fiókjával kijelentkezik Eclipse újraindítása nélkül, az alábbi lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="016ea-121">However, if you want to sign out of your Azure account without restarting Eclipse, use the following steps.</span></span>

1. <span data-ttu-id="016ea-122">Az eclipse-ben kattintson **eszközök**, majd kattintson a **Azure**, és kattintson a **Kijelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="016ea-122">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Az Azure kijelentkezés eclipse menü][L01]

1. <span data-ttu-id="016ea-124">Ha a **Azure Kijelentkezés** párbeszédpanel, kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="016ea-124">When the **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![Jelentkezzen ki párbeszédpanel][L02]

## <a name="signing-into-your-azure-account-automatically-and-creating-a-credentials-file-to-use-in-the-future"></a><span data-ttu-id="016ea-126">Automatikusan jelentkezik be Azure-fiókja és a hitelesítő adatok fájl létrehozásakor a jövőben használni</span><span class="sxs-lookup"><span data-stu-id="016ea-126">Signing into your Azure account automatically and creating a credentials file to use in the future</span></span>

<span data-ttu-id="016ea-127">A következő lépésekkel haladhat végig létrehozása egy hitelesítőadat-fájlt, amely tartalmazza a szolgáltatás egyszerű adatait.</span><span class="sxs-lookup"><span data-stu-id="016ea-127">The following steps will walk you through creating a credentials file which contains your service principal data.</span></span> <span data-ttu-id="016ea-128">Ha az alábbi lépéseket, automatikusan bejelentkezik az Azure minden alkalommal, amikor megnyitja a projekt Eclipse automatikusan az a hitelesítőadat-fájlt használja.</span><span class="sxs-lookup"><span data-stu-id="016ea-128">Once you have completed these steps, Eclipse will automatically use the credentials file to automatically sign you into Azure each time you open your project.</span></span>

1. <span data-ttu-id="016ea-129">Nyissa meg a projekt eclipse-ben.</span><span class="sxs-lookup"><span data-stu-id="016ea-129">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="016ea-130">Kattintson a **eszközök**, majd kattintson a **Azure**, és kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="016ea-130">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Az Azure bejelentkezési eclipse menü][A01]

1. <span data-ttu-id="016ea-132">Ha a **Azure bejelentkezés** kiválasztása párbeszédpanel jelenik meg, **automatikus**, és kattintson a **új**.</span><span class="sxs-lookup"><span data-stu-id="016ea-132">When the **Azure Sign In** dialog box appears, select **Automated**, and then click **New**.</span></span>

   ![Jelentkezzen be a párbeszédpanelt][A02]

1. <span data-ttu-id="016ea-134">Ha a **Azure bejelentkezés** párbeszédpanel jelenik meg, az Azure hitelesítő adatait, és kattintson **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="016ea-134">When the **Azure Log In** dialog box appears, enter your Azure credentials, and then click **Sign In**.</span></span>

   ![Azure bejelentkezési párbeszédpanel][A03]

1. <span data-ttu-id="016ea-136">Ha a **hitelesítési-fájlok létrehozása** párbeszédpanel jelenik meg, válassza ki, amelyet szeretne használni, válassza ki a célkönyvtárat, és kattintson az előfizetések **Start**.</span><span class="sxs-lookup"><span data-stu-id="016ea-136">When the **Create authentication files** dialog box appears, select the subscriptions that you want to use, choose your destination directory, and then click **Start**.</span></span>

   ![Azure bejelentkezési párbeszédpanel][A04]

1. <span data-ttu-id="016ea-138">A **egyszerű Creatation állapot** párbeszédpanel jelenik meg, miután a fájlok sikeresen létrejött, majd **OK**.</span><span class="sxs-lookup"><span data-stu-id="016ea-138">The **Service Principal Creatation Status** dialog box will be displayed, and after your files have been created successfully, click **OK**.</span></span>

   ![Szolgáltatás egyszerű Creatation állapota párbeszédpanel][A05]

1. <span data-ttu-id="016ea-140">Ha a **Azure bejelentkezés** párbeszédpanel, kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="016ea-140">When the **Azure Sign In** dialog box appears, click **Sign In**.</span></span>

   ![Azure bejelentkezési párbeszédpanel][A06]

1. <span data-ttu-id="016ea-142">Ha a **válasszon előfizetések** párbeszédpanel jelenik meg, válassza ki, amelyet szeretne használni, és kattintson az előfizetések **OK**.</span><span class="sxs-lookup"><span data-stu-id="016ea-142">When the **Select Subscriptions** dialog box appears, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Az előfizetések kiválasztása párbeszédpanel][A07]

## <a name="signing-out-of-your-azure-account-when-you-signed-in-automatically"></a><span data-ttu-id="016ea-144">Aláírási kívül az Azure-fiókjával, ha automatikusan jelentkezett be</span><span class="sxs-lookup"><span data-stu-id="016ea-144">Signing out of your Azure account when you signed in automatically</span></span>

<span data-ttu-id="016ea-145">Miután konfigurálta a lépéseket az előző szakaszban, az Azure-eszközkészlet, automatikusan bejelentkezik az Eclipse újraindítását minden alkalommal, amikor az Azure-fiókjával.</span><span class="sxs-lookup"><span data-stu-id="016ea-145">After you have configured the steps in the previous section, the Azure Toolkit will automatically sign you into your Azure account each time you restart Eclipse.</span></span> <span data-ttu-id="016ea-146">Azonban jelentkezzen ki az Azure-fiókjával, és az Azure-eszközkészlet megakadályozza az automatikus bejelentkezés, az alábbi lépésekkel.</span><span class="sxs-lookup"><span data-stu-id="016ea-146">However, to sign out of your Azure account and prevent the Azure Toolkit from signing you in automatically, use the following steps.</span></span>

1. <span data-ttu-id="016ea-147">Az eclipse-ben kattintson **eszközök**, majd kattintson a **Azure**, és kattintson a **Kijelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="016ea-147">In Eclipse, click **Tools**, then click **Azure**, and then click **Sign Out**.</span></span>

   ![Az Azure kijelentkezés eclipse menü][L01]

1. <span data-ttu-id="016ea-149">Ha a **Azure Kijelentkezés** párbeszédpanel, kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="016ea-149">When the **Azure Sign Out** dialog box appears, click **Yes**.</span></span>

   ![Jelentkezzen ki párbeszédpanel][L03]

## <a name="signing-into-your-azure-account-automatically-using-a-credentials-file-which-you-have-already-created"></a><span data-ttu-id="016ea-151">Az Azure-fiók használatával automatikusan egy hitelesítőadat-fájlt, amely már létrehozott bejelentkezni</span><span class="sxs-lookup"><span data-stu-id="016ea-151">Signing into your Azure account automatically using a credentials file which you have already created</span></span>

<span data-ttu-id="016ea-152">Ha regisztrál az Azure-ból Eclipse használata esetén, szüksége lesz újrakonfigurálása az Azure-eszközkészlet az eclipse-ben a hitelesítőadat-fájlt, amely hozott létre, mielőtt automatikus bejelentkezés az Azure-fiókot használni.</span><span class="sxs-lookup"><span data-stu-id="016ea-152">If you sign out of Azure when you are using Eclipse, you will need to reconfigure the Azure Toolkit for Eclipse to use a credentials file which have created before you can automatically sign into your Azure acccount.</span></span> <span data-ttu-id="016ea-153">A következő lépésekkel haladhat végig konfigurálása az Azure-eszközkészlet meglévő hitelesítő adatok fájl.</span><span class="sxs-lookup"><span data-stu-id="016ea-153">The following steps will walk you through configuring the Azure Toolkit to use an existing credentials file.</span></span>

1. <span data-ttu-id="016ea-154">Nyissa meg a projekt eclipse-ben.</span><span class="sxs-lookup"><span data-stu-id="016ea-154">Open your project with Eclipse.</span></span>

1. <span data-ttu-id="016ea-155">Kattintson a **eszközök**, majd kattintson a **Azure**, és kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="016ea-155">Click **Tools**, then click **Azure**, and then click **Sign In**.</span></span>

   ![Az Azure bejelentkezési eclipse menü][A01]

1. <span data-ttu-id="016ea-157">Ha a **Azure bejelentkezés** kiválasztása párbeszédpanel jelenik meg, **automatikus**, és kattintson a **Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="016ea-157">When the **Azure Sign In** dialog box appears, select **Automated**, and then click **Browse**.</span></span>

   ![Jelentkezzen be a párbeszédpanelt][A02]

1. <span data-ttu-id="016ea-159">Ha a **hitelesített fájl kiválasztása** párbeszédpanel jelenik meg, válassza ki a hitelesítőadat-fájlt, amely a korábban létrehozott, és kattintson a **válasszon**.</span><span class="sxs-lookup"><span data-stu-id="016ea-159">When the **Select Authenticated File** dialog box appears, select a credentials file which you created earlier, and then click **Select**.</span></span>

   ![Jelentkezzen be a párbeszédpanelt][A08]

1. <span data-ttu-id="016ea-161">Ha a **Azure bejelentkezés** párbeszédpanel, kattintson a **bejelentkezés**.</span><span class="sxs-lookup"><span data-stu-id="016ea-161">When the **Azure Sign In** dialog box appears, click **Sign In**.</span></span>

   ![Azure bejelentkezési párbeszédpanel][A06]

1. <span data-ttu-id="016ea-163">Ha a **válasszon előfizetések** párbeszédpanel jelenik meg, válassza ki, amelyet szeretne használni, és kattintson az előfizetések **OK**.</span><span class="sxs-lookup"><span data-stu-id="016ea-163">When the **Select Subscriptions** dialog box appears, select the subscriptions that you want to use, and then click **OK**.</span></span>

   ![Az előfizetések kiválasztása párbeszédpanel][A07]

## <a name="see-also"></a><span data-ttu-id="016ea-165">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="016ea-165">See Also</span></span>
<span data-ttu-id="016ea-166">A Java IDE környezetekhez készült Azure-eszközkészlettel kapcsolatos további információkért lásd az alábbi hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="016ea-166">For more information about the Azure Toolkits for Java IDEs, see the following links:</span></span>

* <span data-ttu-id="016ea-167">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="016ea-167">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="016ea-168">[Az Eclipse-hez készült Azure-eszközkészlet újdonságai]</span><span class="sxs-lookup"><span data-stu-id="016ea-168">[What's New in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="016ea-169">[Az Eclipse-hez készült Azure-eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="016ea-169">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="016ea-170">*Jelentkezzen be az Azure eszközkészlet utasításokat az eclipse-ben (Ez a cikk)*</span><span class="sxs-lookup"><span data-stu-id="016ea-170">*Sign In Instructions for the Azure Toolkit for Eclipse (This Article)*</span></span>
  * <span data-ttu-id="016ea-171">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="016ea-171">[Create a Hello World Web App for Azure in Eclipse]</span></span>
* <span data-ttu-id="016ea-172">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="016ea-172">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="016ea-173">[Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]</span><span class="sxs-lookup"><span data-stu-id="016ea-173">[What's New in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="016ea-174">[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="016ea-174">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="016ea-175">[Bejelentkezési utasítások az IntelliJ-hez készült Azure-eszközkészlethez]</span><span class="sxs-lookup"><span data-stu-id="016ea-175">[Sign In Instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="016ea-176">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="016ea-176">[Create a Hello World Web App for Azure in IntelliJ]</span></span>

<span data-ttu-id="016ea-177">Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ] és [Java-eszközök a Visual Studio Team Serviceshez].</span><span class="sxs-lookup"><span data-stu-id="016ea-177">For more information about using Azure with Java, see the [Azure Java Developer Center] and the [Java Tools for Visual Studio Team Services].</span></span>

<!-- URL List -->

<span data-ttu-id="016ea-178">[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="016ea-178">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="016ea-179">[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="016ea-179">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="016ea-180">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="016ea-180">[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="016ea-181">[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="016ea-181">[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="016ea-182">[Az Eclipse-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="016ea-182">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
<span data-ttu-id="016ea-183">[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="016ea-183">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
[Sign In Instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
<span data-ttu-id="016ea-184">[Bejelentkezési utasítások az IntelliJ-hez készült Azure-eszközkészlethez]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="016ea-184">[Sign In Instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="016ea-185">[Az Eclipse-hez készült Azure-eszközkészlet újdonságai]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="016ea-185">[What's New in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="016ea-186">[Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="016ea-186">[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="016ea-187">[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="016ea-187">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>
<span data-ttu-id="016ea-188">[Java-eszközök a Visual Studio Team Serviceshez]: https://java.visualstudio.com/</span><span class="sxs-lookup"><span data-stu-id="016ea-188">[Java Tools for Visual Studio Team Services]: https://java.visualstudio.com/</span></span>

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
