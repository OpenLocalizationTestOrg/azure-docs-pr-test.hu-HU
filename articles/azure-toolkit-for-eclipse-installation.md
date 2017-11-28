---
title: "Az eclipse-ben az Azure eszközkészlet telepítése |} Microsoft Docs"
description: "Megtudhatja, hogyan telepítse az Azure-eszközkészlet az eclipse-ben."
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
ms.openlocfilehash: 35cddba38c364dfb2f6a8646b0014d48ca4cb795
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="installing-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="a2abd-103">Az eclipse-ben az Azure eszközkészlet telepítése</span><span class="sxs-lookup"><span data-stu-id="a2abd-103">Installing the Azure Toolkit for Eclipse</span></span>
<span data-ttu-id="a2abd-104">Az Eclipse Azure eszköztára és sablonok, amelyek lehetővé teszik könnyen létrehozása, fejlesztése és tesztelése, és központi telepítése az Azure-alkalmazások használata az Eclipse fejlesztői környezetet biztosít a.</span><span class="sxs-lookup"><span data-stu-id="a2abd-104">The Azure Toolkit for Eclipse provides templates and functionality that allow you to easily create, develop, test, and deploy Azure applications using the Eclipse development environment.</span></span> <span data-ttu-id="a2abd-105">Az Eclipse Azure eszköztára nyílt forráskódú projektként.</span><span class="sxs-lookup"><span data-stu-id="a2abd-105">The Azure Toolkit for Eclipse is an Open Source project.</span></span> <span data-ttu-id="a2abd-106">A forráskód érhető el a MIT licence alapján <https://github.com/microsoft/azure-tools-for-java>.</span><span class="sxs-lookup"><span data-stu-id="a2abd-106">The source code is available under the MIT License from <https://github.com/microsoft/azure-tools-for-java>.</span></span>

<span data-ttu-id="a2abd-107">A következő lépések bemutatják az eclipse-ben az Azure eszközkészlet telepítése.</span><span class="sxs-lookup"><span data-stu-id="a2abd-107">The following steps show you how to install the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="to-install-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="a2abd-108">Az eclipse-ben az Azure eszközkészlet telepítése</span><span class="sxs-lookup"><span data-stu-id="a2abd-108">To install the Azure Toolkit for Eclipse</span></span>
1. <span data-ttu-id="a2abd-109">Indítsa el az Eclipse.</span><span class="sxs-lookup"><span data-stu-id="a2abd-109">Start Eclipse.</span></span>
2. <span data-ttu-id="a2abd-110">Kattintson a **súgó** menüben, majd kattintson **új szoftverek telepítése**, a következő ábrán látható módon.</span><span class="sxs-lookup"><span data-stu-id="a2abd-110">Click the **Help** menu, and then click **Install New Software**, as shown in the following illustration.</span></span>
   
    ![Az eclipse-ben az Azure eszközkészlet telepítése][01]
3. <span data-ttu-id="a2abd-112">Az a **elérhető szoftverek** párbeszédpanel, melyhez a **együttműködve** szövegmezőben `http://dl.microsoft.com/eclipse` követ a **Enter** kulcs.</span><span class="sxs-lookup"><span data-stu-id="a2abd-112">In the **Available Software** dialog, within the **Work with** text box, type `http://dl.microsoft.com/eclipse` followed by the **Enter** key.</span></span>
4. <span data-ttu-id="a2abd-113">Az a **neve** ablaktáblán jelölje **Eclipse Azure eszköztára**, és törölje a jelet **lépjen kapcsolatba az összes frissítés hely található a szükséges szoftverek telepítése során**.</span><span class="sxs-lookup"><span data-stu-id="a2abd-113">In the **Name** pane, check **Azure Toolkit for Eclipse**, and uncheck **Contact all update sites during install to find required software**.</span></span> <span data-ttu-id="a2abd-114">A képernyőn a következőhöz hasonlóan kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="a2abd-114">Your screen should appear similar to the following:</span></span>
   
    ![Az eclipse-ben az Azure eszközkészlet telepítése][02]
5. <span data-ttu-id="a2abd-116">Ha kibontja a **Eclipse Azure eszköztára**, látni fogja a következő elemek:</span><span class="sxs-lookup"><span data-stu-id="a2abd-116">If you expand the **Azure Toolkit for Eclipse**, you will see the following items:</span></span>
   
   * <span data-ttu-id="a2abd-117">**Javához készült Application Insights beépülő modul**: Ez az összetevő lehetővé teszi, hogy az Azure telemetriai naplózása és elemzése az alkalmazások és a kiszolgáló-példány szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="a2abd-117">**Application Insights Plugin for Java**: This component allows you to use Azure's telemetry logging and analysis services for your applications and server instances.</span></span>
   * <span data-ttu-id="a2abd-118">**Az Azure Access Control szolgáltatások szűrő**: Ez az összetevő-támogatást biztosít az alkalmazás használatához Azure ACS, engedélyezése egyszeri bejelentkezéshez forgatókönyvek és az alkalmazás identitását logika externalizing.</span><span class="sxs-lookup"><span data-stu-id="a2abd-118">**Azure Access Control Services Filter**: This component provides support for authenticating application users with Azure ACS, enabling single sign-on scenarios and externalizing identity logic from the application.</span></span>
   * <span data-ttu-id="a2abd-119">**Az Azure közös beépülő modul**: Ez az összetevő biztosítja a szükséges egyéb eszközkészlet összetevők közös funkciókat.</span><span class="sxs-lookup"><span data-stu-id="a2abd-119">**Azure Common Plugin**: This component provides the common functionality needed by other toolkit components.</span></span>
   * <span data-ttu-id="a2abd-120">**Az Azure-kezelőjét az Eclipse**: Ez az összetevő biztosítja a szükséges egyéb eszközkészlet összetevők közös funkciókat.</span><span class="sxs-lookup"><span data-stu-id="a2abd-120">**Azure Explorer for Eclipse**: This component provides the common functionality needed by other toolkit components.</span></span>
   * <span data-ttu-id="a2abd-121">**Azure Java Eclipse beépülő modul**: Ez az összetevő támogatást nyújt az projektek, amelyek segítenek a létrehozása, tesztelése és telepítése a Microsoft Azure felhőbe az eclipse-ben és a parancssorból Java-alkalmazások fejlesztésével.</span><span class="sxs-lookup"><span data-stu-id="a2abd-121">**Azure Plugin for Eclipse with Java**: This component provides support for developing projects that help build, test and deploy Java applications for the Microsoft Azure cloud in Eclipse and via command line.</span></span>
   * <span data-ttu-id="a2abd-122">**Az Azure Web Apps Plugin Java**: Ez az összetevő támogatást nyújt a Microsoft Azure Web Apps tárolók Java webes alkalmazások telepítése.</span><span class="sxs-lookup"><span data-stu-id="a2abd-122">**Azure Web Apps Plugin with Java**: This component provides support for deploying Java web applications to Microsoft Azure Web App containers.</span></span>
   * <span data-ttu-id="a2abd-123">**Microsoft SQL Server JDBC illesztőprogram 4.2**: Java Platform Enterprise Edition 8 Ez az összetevő biztosítja az SQL Server és a Microsoft Azure SQL Database JDBC API.</span><span class="sxs-lookup"><span data-stu-id="a2abd-123">**Microsoft JDBC Driver 4.2 for SQL Server**: This component provides JDBC API for SQL Server and Microsoft Azure SQL Database for Java Platform Enterprise Edition 8.</span></span>
   * <span data-ttu-id="a2abd-124">**Az Apache Qpid ügyfél függvénytárainak JMS csomag**: Ez az összetevő biztosítja az JMS ügyféloldali összetevők ahhoz, hogy az alkalmazás használhatja a Microsoft Azure messaging AMQP Apache Qpid projektből.</span><span class="sxs-lookup"><span data-stu-id="a2abd-124">**Package for Apache Qpid Client Libraries for JMS**: This component provides the JMS client component from the Apache Qpid project to enable your application to use AMQP messaging in Microsoft Azure.</span></span>
   * <span data-ttu-id="a2abd-125">**A Microsoft Azure-könyvtárakban Java csomag**: Ez az összetevő biztosítja API-k eléréséhez a Microsoft Azure-szolgáltatások, például a tárolás, a service bus, szolgáltatás futásideje, stb.</span><span class="sxs-lookup"><span data-stu-id="a2abd-125">**Package for Microsoft Azure Libraries for Java**: This component provides APIs for accessing Microsoft Azure services, such as storage, service bus, service runtime, etc.</span></span>
6. <span data-ttu-id="a2abd-126">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="a2abd-126">Click **Next**.</span></span> <span data-ttu-id="a2abd-127">(Ha az eszközkészlet telepítésével szokatlan késést tapasztal, ellenőrizze, hogy **lépjen kapcsolatba az összes frissítés hely található a szükséges szoftverek telepítése során** nincs bejelölve.)</span><span class="sxs-lookup"><span data-stu-id="a2abd-127">(If you experience unusual delays when installing the toolkit, ensure that **Contact all update sites during install to find required software** is unchecked.)</span></span>
7. <span data-ttu-id="a2abd-128">Az a **telepítése részletek** párbeszédpanel, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="a2abd-128">In the **Install Details** dialog, click **Next**.</span></span>
   
    ![Tekintse át a telepítés részleteit][03]
8. <span data-ttu-id="a2abd-130">Az a **licencek áttekintése** párbeszédpanelen tekintse át a Licencszerződések feltételeit.</span><span class="sxs-lookup"><span data-stu-id="a2abd-130">In the **Review Licenses** dialog, review the terms of the license agreements.</span></span> <span data-ttu-id="a2abd-131">Ha elfogadja a licencszerződés feltételeit, kattintson az **elfogadom a licencszerződés feltételeit** majd **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="a2abd-131">If you accept the terms of the license agreements, click **I accept the terms of the license agreements** and then click **Finish**.</span></span> <span data-ttu-id="a2abd-132">(A többi lépések feltételezik, fogadja el a licencszerződés feltételeit.</span><span class="sxs-lookup"><span data-stu-id="a2abd-132">(The remaining steps assume you do accept the terms of the license agreements.</span></span> <span data-ttu-id="a2abd-133">Amennyiben nem fogadja el a licencszerződés feltételeit, lépjen ki a telepítési folyamatot.)</span><span class="sxs-lookup"><span data-stu-id="a2abd-133">If you do not accept the terms of the license agreements, exit the installation process.)</span></span>
   
    ![Licencek áttekintése][04]
   
    <span data-ttu-id="a2abd-135">Eclipse letölti és telepíti a szükséges csomagokat.</span><span class="sxs-lookup"><span data-stu-id="a2abd-135">Eclipse will download and install the requisite packages.</span></span>
   
    ![Folyamatban lévő telepítés][05]
9. <span data-ttu-id="a2abd-137">Ha a telepítés befejezéséhez indítsa újra a Eclipse kéri, kattintson a **Igen**.</span><span class="sxs-lookup"><span data-stu-id="a2abd-137">If prompted to restart Eclipse to complete installation, click **Yes**.</span></span>
   
    ![Indítsa újra a parancssorba][06]

## <a name="see-also"></a><span data-ttu-id="a2abd-139">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="a2abd-139">See Also</span></span>
<span data-ttu-id="a2abd-140">A Java IDE környezetekhez készült Azure-eszközkészlettel kapcsolatos további információkért lásd az alábbi hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="a2abd-140">For more information about the Azure Toolkits for Java IDEs, see the following links:</span></span>

* <span data-ttu-id="a2abd-141">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="a2abd-141">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a2abd-142">[Az Eclipse-hez készült Azure-eszközkészlet újdonságai]</span><span class="sxs-lookup"><span data-stu-id="a2abd-142">[What's New in the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a2abd-143">*Az Azure eszközkészlet telepítése az eclipse-ben (Ez a cikk)*</span><span class="sxs-lookup"><span data-stu-id="a2abd-143">*Installing the Azure Toolkit for Eclipse (This Article)*</span></span>
  * <span data-ttu-id="a2abd-144">[Bejelentkezési utasítások az Eclipse-hez készült Azure-eszközkészlethez]</span><span class="sxs-lookup"><span data-stu-id="a2abd-144">[Sign In Instructions for the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="a2abd-145">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="a2abd-145">[Create a Hello World Web App for Azure in Eclipse]</span></span>
* <span data-ttu-id="a2abd-146">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="a2abd-146">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a2abd-147">[Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]</span><span class="sxs-lookup"><span data-stu-id="a2abd-147">[What's New in the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a2abd-148">[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="a2abd-148">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a2abd-149">[Bejelentkezési utasítások az IntelliJ-hez készült Azure-eszközkészlethez]</span><span class="sxs-lookup"><span data-stu-id="a2abd-149">[Sign In Instructions for the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="a2abd-150">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="a2abd-150">[Create a Hello World Web App for Azure in IntelliJ]</span></span>

<span data-ttu-id="a2abd-151">Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ].</span><span class="sxs-lookup"><span data-stu-id="a2abd-151">For more information about using Azure with Java, see the [Azure Java Developer Center].</span></span>

<!-- URL List -->

<span data-ttu-id="a2abd-152">[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md</span><span class="sxs-lookup"><span data-stu-id="a2abd-152">[Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse.md</span></span>
<span data-ttu-id="a2abd-153">[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md</span><span class="sxs-lookup"><span data-stu-id="a2abd-153">[Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij.md</span></span>
<span data-ttu-id="a2abd-154">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="a2abd-154">[Create a Hello World Web App for Azure in Eclipse]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md</span></span>
<span data-ttu-id="a2abd-155">[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span><span class="sxs-lookup"><span data-stu-id="a2abd-155">[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md</span></span>
[Installing the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
<span data-ttu-id="a2abd-156">[Az IntelliJ-hez készült Azure-eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md</span><span class="sxs-lookup"><span data-stu-id="a2abd-156">[Installing the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-installation.md</span></span>
<span data-ttu-id="a2abd-157">[Bejelentkezési utasítások az Eclipse-hez készült Azure-eszközkészlethez]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="a2abd-157">[Sign In Instructions for the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-sign-in-instructions.md</span></span>
<span data-ttu-id="a2abd-158">[Bejelentkezési utasítások az IntelliJ-hez készült Azure-eszközkészlethez]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span><span class="sxs-lookup"><span data-stu-id="a2abd-158">[Sign In Instructions for the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-sign-in-instructions.md</span></span>
<span data-ttu-id="a2abd-159">[Az Eclipse-hez készült Azure-eszközkészlet újdonságai]: ./azure-toolkit-for-eclipse-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="a2abd-159">[What's New in the Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-whats-new.md</span></span>
<span data-ttu-id="a2abd-160">[Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]: ./azure-toolkit-for-intellij-whats-new.md</span><span class="sxs-lookup"><span data-stu-id="a2abd-160">[What's New in the Azure Toolkit for IntelliJ]: ./azure-toolkit-for-intellij-whats-new.md</span></span>

<span data-ttu-id="a2abd-161">[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/</span><span class="sxs-lookup"><span data-stu-id="a2abd-161">[Azure Java Developer Center]: https://azure.microsoft.com/develop/java/</span></span>

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->
