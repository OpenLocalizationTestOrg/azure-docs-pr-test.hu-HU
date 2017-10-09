---
title: "aaaInstalling hello Azure eszköztára Eclipse |} Microsoft Docs"
description: "Ismerje meg, hogyan tooinstall hello Azure eszköztára eclipse-ben."
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
ms.openlocfilehash: 6c195fab2b47fb5c42541a8cf52be4ec88d27b5d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="installing-hello-azure-toolkit-for-eclipse"></a><span data-ttu-id="8aee2-103">Hello Azure eszköztára Eclipse telepítése</span><span class="sxs-lookup"><span data-stu-id="8aee2-103">Installing hello Azure Toolkit for Eclipse</span></span>
<span data-ttu-id="8aee2-104">sablonok és a funkciókat, amelyek lehetővé teszik tooeasily létrehozásával hello Azure eszköztára Eclipse foglalja össze, fejlesztése, tesztelése és hello Eclipse fejlesztői környezetet használó Azure alkalmazások telepítéséhez.</span><span class="sxs-lookup"><span data-stu-id="8aee2-104">hello Azure Toolkit for Eclipse provides templates and functionality that allow you tooeasily create, develop, test, and deploy Azure applications using hello Eclipse development environment.</span></span> <span data-ttu-id="8aee2-105">hello Azure eszköztára Eclipse nyílt forráskódú projektként.</span><span class="sxs-lookup"><span data-stu-id="8aee2-105">hello Azure Toolkit for Eclipse is an Open Source project.</span></span> <span data-ttu-id="8aee2-106">hello forráskód alatt hello MIT licenccel érhető el a <https://github.com/microsoft/azure-tools-for-java>.</span><span class="sxs-lookup"><span data-stu-id="8aee2-106">hello source code is available under hello MIT License from <https://github.com/microsoft/azure-tools-for-java>.</span></span>

<span data-ttu-id="8aee2-107">hello lépések bemutatják, hogyan tooinstall hello Azure eszköztára eclipse-ben.</span><span class="sxs-lookup"><span data-stu-id="8aee2-107">hello following steps show you how tooinstall hello Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="tooinstall-hello-azure-toolkit-for-eclipse"></a><span data-ttu-id="8aee2-108">tooinstall hello Eclipse Azure eszköztára</span><span class="sxs-lookup"><span data-stu-id="8aee2-108">tooinstall hello Azure Toolkit for Eclipse</span></span>
1. <span data-ttu-id="8aee2-109">Indítsa el az Eclipse.</span><span class="sxs-lookup"><span data-stu-id="8aee2-109">Start Eclipse.</span></span>
2. <span data-ttu-id="8aee2-110">Kattintson a hello **súgó** menüben, majd kattintson **új szoftverek telepítése**, ahogy az ábra a következő hello.</span><span class="sxs-lookup"><span data-stu-id="8aee2-110">Click hello **Help** menu, and then click **Install New Software**, as shown in hello following illustration.</span></span>
   
    ![Hello Azure eszköztára Eclipse telepítése][01]
3. <span data-ttu-id="8aee2-112">A hello **elérhető szoftverek** párbeszédpaneljén hello **együttműködve** szövegmezőben `http://dl.microsoft.com/eclipse` hello követ **Enter** kulcs.</span><span class="sxs-lookup"><span data-stu-id="8aee2-112">In hello **Available Software** dialog, within hello **Work with** text box, type `http://dl.microsoft.com/eclipse` followed by hello **Enter** key.</span></span>
4. <span data-ttu-id="8aee2-113">A hello **neve** ablaktáblán jelölje **Eclipse Azure eszköztára**, és törölje a jelet **minden frissítés során helyek forduljon toofind szükséges szoftver telepítéséhez**.</span><span class="sxs-lookup"><span data-stu-id="8aee2-113">In hello **Name** pane, check **Azure Toolkit for Eclipse**, and uncheck **Contact all update sites during install toofind required software**.</span></span> <span data-ttu-id="8aee2-114">A képernyő hasonló toohello következő kell megjelennie:</span><span class="sxs-lookup"><span data-stu-id="8aee2-114">Your screen should appear similar toohello following:</span></span>
   
    ![Hello Azure eszköztára Eclipse telepítése][02]
5. <span data-ttu-id="8aee2-116">Ha kibontja hello **Eclipse Azure eszköztára**, látni fogja a következő elemek hello:</span><span class="sxs-lookup"><span data-stu-id="8aee2-116">If you expand hello **Azure Toolkit for Eclipse**, you will see hello following items:</span></span>
   
   * <span data-ttu-id="8aee2-117">**Javához készült Application Insights beépülő modul**: Ez az összetevő lehetővé teszi a toouse Azure telemetriai naplózása és elemzése az alkalmazások és a kiszolgáló-példány szolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="8aee2-117">**Application Insights Plugin for Java**: This component allows you toouse Azure's telemetry logging and analysis services for your applications and server instances.</span></span>
   * <span data-ttu-id="8aee2-118">**Az Azure Access Control szolgáltatások szűrő**: Ez az összetevő-támogatást biztosít az alkalmazás használatához Azure ACS, engedélyezése egyszeri bejelentkezéshez forgatókönyvek és externalizing identitás logika hello alkalmazásból.</span><span class="sxs-lookup"><span data-stu-id="8aee2-118">**Azure Access Control Services Filter**: This component provides support for authenticating application users with Azure ACS, enabling single sign-on scenarios and externalizing identity logic from hello application.</span></span>
   * <span data-ttu-id="8aee2-119">**Az Azure közös beépülő modul**: Ez az összetevő hello más eszközkészlet összetevőkhöz szükséges közös funkciókat biztosítja.</span><span class="sxs-lookup"><span data-stu-id="8aee2-119">**Azure Common Plugin**: This component provides hello common functionality needed by other toolkit components.</span></span>
   * <span data-ttu-id="8aee2-120">**Az Azure-kezelőjét az Eclipse**: Ez az összetevő hello más eszközkészlet összetevőkhöz szükséges közös funkciókat biztosítja.</span><span class="sxs-lookup"><span data-stu-id="8aee2-120">**Azure Explorer for Eclipse**: This component provides hello common functionality needed by other toolkit components.</span></span>
   * <span data-ttu-id="8aee2-121">**Azure Java Eclipse beépülő modul**: Ez az összetevő létrehozása, tesztelése és hello Microsoft Azure felhőbe az eclipse-ben és a parancssorból a Java-alkalmazások központi telepítését megkönnyítő projektek fejlesztése támogatást nyújt.</span><span class="sxs-lookup"><span data-stu-id="8aee2-121">**Azure Plugin for Eclipse with Java**: This component provides support for developing projects that help build, test and deploy Java applications for hello Microsoft Azure cloud in Eclipse and via command line.</span></span>
   * <span data-ttu-id="8aee2-122">**Az Azure Web Apps Plugin Java**: Ez az összetevő támogatást nyújt a Java webes alkalmazások tooMicrosoft Azure Web Apps tárolók telepítése.</span><span class="sxs-lookup"><span data-stu-id="8aee2-122">**Azure Web Apps Plugin with Java**: This component provides support for deploying Java web applications tooMicrosoft Azure Web App containers.</span></span>
   * <span data-ttu-id="8aee2-123">**Microsoft SQL Server JDBC illesztőprogram 4.2**: Java Platform Enterprise Edition 8 Ez az összetevő biztosítja az SQL Server és a Microsoft Azure SQL Database JDBC API.</span><span class="sxs-lookup"><span data-stu-id="8aee2-123">**Microsoft JDBC Driver 4.2 for SQL Server**: This component provides JDBC API for SQL Server and Microsoft Azure SQL Database for Java Platform Enterprise Edition 8.</span></span>
   * <span data-ttu-id="8aee2-124">**Az Apache Qpid ügyfél függvénytárainak JMS csomag**: Ez az összetevő hello JMS ügyfél összetevőt hello Apache Qpid projekt tooenable az alkalmazás toouse AMQP üzenetküldést biztosít a Microsoft Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="8aee2-124">**Package for Apache Qpid Client Libraries for JMS**: This component provides hello JMS client component from hello Apache Qpid project tooenable your application toouse AMQP messaging in Microsoft Azure.</span></span>
   * <span data-ttu-id="8aee2-125">**A Microsoft Azure-könyvtárakban Java csomag**: Ez az összetevő biztosítja API-k eléréséhez a Microsoft Azure-szolgáltatások, például a tárolás, a service bus, szolgáltatás futásideje, stb.</span><span class="sxs-lookup"><span data-stu-id="8aee2-125">**Package for Microsoft Azure Libraries for Java**: This component provides APIs for accessing Microsoft Azure services, such as storage, service bus, service runtime, etc.</span></span>
6. <span data-ttu-id="8aee2-126">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="8aee2-126">Click **Next**.</span></span> <span data-ttu-id="8aee2-127">(Ha szokatlan késések hello eszközkészlet telepítése közben, győződjön meg arról, hogy **minden frissítés során helyek forduljon toofind szükséges szoftver telepítéséhez** nincs bejelölve.)</span><span class="sxs-lookup"><span data-stu-id="8aee2-127">(If you experience unusual delays when installing hello toolkit, ensure that **Contact all update sites during install toofind required software** is unchecked.)</span></span>
7. <span data-ttu-id="8aee2-128">A hello **telepítése részletek** párbeszédpanel, kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="8aee2-128">In hello **Install Details** dialog, click **Next**.</span></span>
   
    ![Tekintse át a telepítés részleteit][03]
8. <span data-ttu-id="8aee2-130">A hello **licencek áttekintése** párbeszédpanelen tekintse át a hello hello Licencszerződések feltételeit.</span><span class="sxs-lookup"><span data-stu-id="8aee2-130">In hello **Review Licenses** dialog, review hello terms of hello license agreements.</span></span> <span data-ttu-id="8aee2-131">Ha hello hello licencszerződés feltételeinek elfogadásához kattintson **hello hello szerződés feltételeinek elfogadása** majd **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="8aee2-131">If you accept hello terms of hello license agreements, click **I accept hello terms of hello license agreements** and then click **Finish**.</span></span> <span data-ttu-id="8aee2-132">(hello fennmaradó lépések azt feltételezik hello hello licencszerződés feltételeinek elfogadásához.</span><span class="sxs-lookup"><span data-stu-id="8aee2-132">(hello remaining steps assume you do accept hello terms of hello license agreements.</span></span> <span data-ttu-id="8aee2-133">Ha nem fogadja el a hello hello Licencszerződések feltételeit, lépjen ki a telepítési folyamat hello.)</span><span class="sxs-lookup"><span data-stu-id="8aee2-133">If you do not accept hello terms of hello license agreements, exit hello installation process.)</span></span>
   
    ![Licencek áttekintése][04]
   
    <span data-ttu-id="8aee2-135">Eclipse letölti és telepíti a szükséges hello csomagok.</span><span class="sxs-lookup"><span data-stu-id="8aee2-135">Eclipse will download and install hello requisite packages.</span></span>
   
    ![Folyamatban lévő telepítés][05]
9. <span data-ttu-id="8aee2-137">Ha toorestart Eclipse toocomplete telepítési kéri, kattintson az **Igen**.</span><span class="sxs-lookup"><span data-stu-id="8aee2-137">If prompted toorestart Eclipse toocomplete installation, click **Yes**.</span></span>
   
    ![Indítsa újra a parancssorba][06]

## <a name="see-also"></a><span data-ttu-id="8aee2-139">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="8aee2-139">See Also</span></span>
<span data-ttu-id="8aee2-140">A Java IDEs hello Azure eszközök gazdag kapcsolatos további információkért tekintse meg a következő hivatkozások hello:</span><span class="sxs-lookup"><span data-stu-id="8aee2-140">For more information about hello Azure Toolkits for Java IDEs, see hello following links:</span></span>

* <span data-ttu-id="8aee2-141">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="8aee2-141">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="8aee2-142">[Újdonságok az Eclipse Azure eszköztára hello]</span><span class="sxs-lookup"><span data-stu-id="8aee2-142">[What's New in hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="8aee2-143">*Hello Azure eszköztára Eclipse (Ez a cikk) telepítése*</span><span class="sxs-lookup"><span data-stu-id="8aee2-143">*Installing hello Azure Toolkit for Eclipse (This Article)*</span></span>
  * <span data-ttu-id="8aee2-144">[Bejelentkezés a utasításokat hello Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="8aee2-144">[Sign In Instructions for hello Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="8aee2-145">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="8aee2-145">[Create a Hello World Web App for Azure in Eclipse]</span></span>
* <span data-ttu-id="8aee2-146">[Az IntelliJ-hez készült Azure-eszközkészlet]</span><span class="sxs-lookup"><span data-stu-id="8aee2-146">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="8aee2-147">[Újdonságok az intellij-t Azure eszköztára hello]</span><span class="sxs-lookup"><span data-stu-id="8aee2-147">[What's New in hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="8aee2-148">[Az IntelliJ hello Azure eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="8aee2-148">[Installing hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="8aee2-149">[Bejelentkezés a utasításokat hello IntelliJ Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="8aee2-149">[Sign In Instructions for hello Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="8aee2-150">[Hello World webalkalmazás létrehozása az intellij-t az Azure]</span><span class="sxs-lookup"><span data-stu-id="8aee2-150">[Create a Hello World Web App for Azure in IntelliJ]</span></span>

<span data-ttu-id="8aee2-151">Azure Java használatával kapcsolatos további információkért lásd: hello [Azure Java fejlesztői központból].</span><span class="sxs-lookup"><span data-stu-id="8aee2-151">For more information about using Azure with Java, see hello [Azure Java Developer Center].</span></span>

<!-- URL List -->

[Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse.md
[Az IntelliJ-hez készült Azure-eszközkészlet]: ./azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web/app-service-web-eclipse-create-hello-world-web-app.md
[Hello World webalkalmazás létrehozása az intellij-t az Azure]: ./app-service-web/app-service-web-intellij-create-hello-world-web-app.md
[Installing hello Azure Toolkit for Eclipse]: ./azure-toolkit-for-eclipse-installation.md
[Az IntelliJ hello Azure eszközkészlet telepítése]: ./azure-toolkit-for-intellij-installation.md
[Bejelentkezés a utasításokat hello Eclipse Azure eszköztára]: ./azure-toolkit-for-eclipse-sign-in-instructions.md
[Bejelentkezés a utasításokat hello IntelliJ Azure eszköztára]: ./azure-toolkit-for-intellij-sign-in-instructions.md
[Újdonságok az Eclipse Azure eszköztára hello]: ./azure-toolkit-for-eclipse-whats-new.md
[Újdonságok az intellij-t Azure eszköztára hello]: ./azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központból]: https://azure.microsoft.com/develop/java/

<!-- IMG List -->

[01]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-01.png
[02]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-02.png
[03]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-03.png
[04]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-04.png
[05]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-05.png
[06]: ./media/azure-toolkit-for-eclipse-installation/eclipse-installation-06.png

<!-- Legacy MSDN URL = https://msdn.microsoft.com/library/azure/hh690946.aspx -->
