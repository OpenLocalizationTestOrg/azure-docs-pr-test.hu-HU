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
# <a name="installing-the-azure-toolkit-for-intellij"></a><span data-ttu-id="0527c-103">Az intellij-t az Azure eszközkészlet telepítése</span><span class="sxs-lookup"><span data-stu-id="0527c-103">Installing the Azure Toolkit for IntelliJ</span></span>
<span data-ttu-id="0527c-104">Az IntelliJ Azure eszköztára sablonok és engedélyezi, hogy könnyen létrehozása, fejlesztése és tesztelése, és az IntelliJ IDEA fejlesztési környezet használata Azure-alkalmazások telepítését biztosít.</span><span class="sxs-lookup"><span data-stu-id="0527c-104">The Azure Toolkit for IntelliJ provides templates and functionality that allow you to easily create, develop, test, and deploy Azure applications using the IntelliJ IDEA development environment.</span></span> <span data-ttu-id="0527c-105">Az IntelliJ Azure eszköztára nyílt forráskódú projektként, amelynek forráskódját, akkor a MIT licenccel a Githubon a következő URL-címen a projekt helyről:</span><span class="sxs-lookup"><span data-stu-id="0527c-105">The Azure Toolkit for IntelliJ is an Open Source project, whose source code is available under the MIT License from the project's site on GitHub at the following URL:</span></span>

<span data-ttu-id="0527c-106"><https://github.com/Microsoft/Azure-Tools-for-Java></span><span class="sxs-lookup"><span data-stu-id="0527c-106"><https://github.com/microsoft/azure-tools-for-java></span></span>

<span data-ttu-id="0527c-107">Az intellij-t, a beállítások párbeszédpanelen megadott és a konfigurálás menüből kifejezésre a kezdőképernyőn; az Azure-eszközkészlet telepítése a két módszer Mindkét telepítési módszer a következő lépésekben bizonyítható.</span><span class="sxs-lookup"><span data-stu-id="0527c-107">There are two methods of installing the Azure Toolkit for IntelliJ, from the Settings dialog box and from the Configure menu on the start screen; both installation methods will be demonstrated in the following steps.</span></span>

[!INCLUDE [azure-toolkit-for-IntelliJ-prerequisites](../includes/azure-toolkit-for-intellij-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-settings-dialog-box"></a><span data-ttu-id="0527c-108">A beállítások párbeszédpanelen megadott IntelliJ az Azure-eszközkészlet telepítése</span><span class="sxs-lookup"><span data-stu-id="0527c-108">To install the Azure Toolkit for IntelliJ from the settings dialog box</span></span>
1. <span data-ttu-id="0527c-109">Indítsa el az IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="0527c-109">Start IntelliJ IDEA.</span></span>
2. <span data-ttu-id="0527c-110">Az IntelliJ IDEA megnyitása után kattintson **fájl**, majd kattintson a **beállítások**.</span><span class="sxs-lookup"><span data-stu-id="0527c-110">When the IntelliJ IDEA opens, click **File**, then click **Settings**.</span></span>
   
    ![Nyissa meg az IntelliJ IDEA beállításai párbeszédpanel][01a]
3. <span data-ttu-id="0527c-112">A beállítások párbeszédpanelen kattintson a **beépülő modulok**, és kattintson a **adattárak Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="0527c-112">In the Settings dialog box, click **Plugins**, and then click **Browse repositories**.</span></span>
   
    ![Az IntelliJ IDEA beállításai párbeszédpanel][02a]
4. <span data-ttu-id="0527c-114">Az a **Tallózás adattárak** párbeszédpanelen be a keresőmezőbe írja be a "Azure".</span><span class="sxs-lookup"><span data-stu-id="0527c-114">In the **Browse Repositories** dialog box, type "Azure" in the search box.</span></span> <span data-ttu-id="0527c-115">Jelöljön ki **IntelliJ Azure eszköztára**, és kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="0527c-115">Highlight **Azure Toolkit for IntelliJ**, and then click **Install**.</span></span>
   
    ![Az intellij-t az Azure eszközkészlet keresése][03]
   
    <span data-ttu-id="0527c-117">IntelliJ IDEA egy párbeszédpanelen megjelenik a telepítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="0527c-117">IntelliJ IDEA will display the installation progress in a dialog box.</span></span>
   
    ![Folyamatban lévő telepítés][04]
5. <span data-ttu-id="0527c-119">A telepítés befejezése után kattintson **indítsa újra az IntelliJ IDEA**.</span><span class="sxs-lookup"><span data-stu-id="0527c-119">When the installation has completed, click **Restart IntelliJ IDEA**.</span></span>
   
    ![Indítsa újra az IntelliJ IDEA][05]
6. <span data-ttu-id="0527c-121">Kattintson a **OK** a beállítások párbeszédpanel bezárásához.</span><span class="sxs-lookup"><span data-stu-id="0527c-121">Click **OK** to close the Settings dialog box.</span></span>
   
    ![Zárja be az IntelliJ IDEA beállításai párbeszédpanel][06]
7. <span data-ttu-id="0527c-123">Amikor a rendszer kéri, indítsa újra az IntelliJ IDEA vagy halassza el, kattintson **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="0527c-123">When prompted to restart IntelliJ IDEA or postpone, click **Restart**.</span></span>
   
    ![Indítsa újra az IntelliJ IDEA][07]

## <a name="to-install-the-azure-toolkit-for-intellij-from-the-start-screen"></a><span data-ttu-id="0527c-125">Az Azure-eszközkészlet telepítése az intellij-t a kezdőképernyőről</span><span class="sxs-lookup"><span data-stu-id="0527c-125">To install the Azure Toolkit for IntelliJ from the start screen</span></span>
1. <span data-ttu-id="0527c-126">Indítsa el az IntelliJ IDEA.</span><span class="sxs-lookup"><span data-stu-id="0527c-126">Start IntelliJ IDEA.</span></span>
2. <span data-ttu-id="0527c-127">Az IntelliJ IDEA indítási képernyő megjelenésekor kattintson **konfigurálása**, majd kattintson a **beépülő modulok**.</span><span class="sxs-lookup"><span data-stu-id="0527c-127">When the IntelliJ IDEA start screen appears, click **Configure**, then click **Plugins**.</span></span>
   
    ![Az IntelliJ IDEA beépülő modulok telepítése][01b]
3. <span data-ttu-id="0527c-129">Az a **beépülő modulok** párbeszédpanel, kattintson a **adattárak Tallózás**.</span><span class="sxs-lookup"><span data-stu-id="0527c-129">In the **Plugins** dialog box, click **Browse repositories**.</span></span>
   
    ![Keresse meg az IntelliJ IDEA beépülő modul adattárak][02b]
4. <span data-ttu-id="0527c-131">Az a **Tallózás adattárak** párbeszédpanelen be a keresőmezőbe írja be a "Azure".</span><span class="sxs-lookup"><span data-stu-id="0527c-131">In the **Browse Repositories** dialog box, type "Azure" in the search box.</span></span> <span data-ttu-id="0527c-132">Jelöljön ki **IntelliJ Azure eszköztára**, és kattintson a **telepítése**.</span><span class="sxs-lookup"><span data-stu-id="0527c-132">Highlight **Azure Toolkit for IntelliJ**, and then click **Install**.</span></span>
   
    ![Az intellij-t az Azure eszközkészlet keresése][03]
   
    <span data-ttu-id="0527c-134">IntelliJ IDEA egy párbeszédpanelen megjelenik a telepítési folyamat.</span><span class="sxs-lookup"><span data-stu-id="0527c-134">IntelliJ IDEA will display the installation progress in a dialog box.</span></span>
   
    ![Folyamatban lévő telepítés][04]
5. <span data-ttu-id="0527c-136">A telepítés befejezése után kattintson **indítsa újra az IntelliJ IDEA**.</span><span class="sxs-lookup"><span data-stu-id="0527c-136">When the installation has completed, click **Restart IntelliJ IDEA**.</span></span>
   
    ![Indítsa újra az IntelliJ IDEA][05]
6. <span data-ttu-id="0527c-138">Amikor a rendszer kéri, indítsa újra az IntelliJ IDEA vagy halassza el, kattintson **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="0527c-138">When prompted to restart IntelliJ IDEA or postpone, click **Restart**.</span></span>
   
    ![Indítsa újra az IntelliJ IDEA][07]

## <a name="see-also"></a><span data-ttu-id="0527c-140">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="0527c-140">See Also</span></span>
<span data-ttu-id="0527c-141">A Java IDE környezetekhez készült Azure-eszközkészlettel kapcsolatos további információkért lásd az alábbi hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="0527c-141">For more information about the Azure Toolkits for Java IDEs, see the following links:</span></span>

* <span data-ttu-id="0527c-142">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="0527c-142">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="0527c-143">[Az Eclipse-hez készült Azure-eszközkészlet újdonságai]</span><span class="sxs-lookup"><span data-stu-id="0527c-143">[What's New in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="0527c-144">[Az Eclipse-hez készült Azure-eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="0527c-144">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="0527c-145">[Bejelentkezési utasítások az Eclipse-hez készült Azure-eszközkészlethez]</span><span class="sxs-lookup"><span data-stu-id="0527c-145">[Sign In Instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="0527c-146">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="0527c-146">[Create a Hello World Web App for Azure in Eclipse]</span></span>
* <span data-ttu-id="0527c-147">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="0527c-147">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="0527c-148">[Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]</span><span class="sxs-lookup"><span data-stu-id="0527c-148">[What's New in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="0527c-149">*Az intellij-t (Ez a cikk) az Azure eszközkészlet telepítése*</span><span class="sxs-lookup"><span data-stu-id="0527c-149">*Installing the Azure Toolkit for IntelliJ (This Article)*</span></span>
  * <span data-ttu-id="0527c-150">[Bejelentkezési utasítások az IntelliJ-hez készült Azure-eszközkészlethez]</span><span class="sxs-lookup"><span data-stu-id="0527c-150">[Sign In Instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="0527c-151">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="0527c-151">[Create a Hello World Web App for Azure in IntelliJ]</span></span>

<span data-ttu-id="0527c-152">Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ].</span><span class="sxs-lookup"><span data-stu-id="0527c-152">For more information about using Azure with Java, see the [Azure Java Developer Center].</span></span>

<!-- URL List -->

<span data-ttu-id="0527c-153">[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="0527c-153">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="0527c-154">[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="0527c-154">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="0527c-155">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="0527c-155">[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="0527c-156">[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="0527c-156">[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
<span data-ttu-id="0527c-157">[Az Eclipse-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-eclipse-installation.md</span><span class="sxs-lookup"><span data-stu-id="0527c-157">[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md</span></span>
[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md
<span data-ttu-id="0527c-158">[Bejelentkezési utasítások az Eclipse-hez készült Azure-eszközkészlethez]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="0527c-158">[Sign In Instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
<span data-ttu-id="0527c-159">[Bejelentkezési utasítások az IntelliJ-hez készült Azure-eszközkészlethez]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="0527c-159">[Sign In Instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="0527c-160">[Az Eclipse-hez készült Azure-eszközkészlet újdonságai]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="0527c-160">[What's New in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="0527c-161">[Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="0527c-161">[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="0527c-162">[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="0527c-162">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>

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
