---
title: "A rugó rendszerindító alkalmazás közzététele egy Docker-tároló, az eclipse-ben az Azure-eszközkészlet használatával |} Microsoft Docs"
description: "Útmutató a webes alkalmazás a Microsoft Azure is közzé egy Docker-tároló az eclipse-ben az Azure-eszközkészlet használatával."
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
ms.date: 06/21/2017
ms.author: robmcm
ms.openlocfilehash: fcb60fcfbda26f5f37bfb0edcb01f8737188b6bc
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/03/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-the-azure-toolkit-for-eclipse"></a><span data-ttu-id="8b798-103">A rugó rendszerindító alkalmazás közzététele egy Docker-tároló, az eclipse-ben az Azure-eszközkészlet használatával</span><span class="sxs-lookup"><span data-stu-id="8b798-103">Publish a Spring Boot app as a Docker container by using the Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="8b798-104">A [rugó keretrendszer] egy nyílt forráskódú megoldás, amely a fejlesztőket Java vállalati szintű alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="8b798-104">The [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="8b798-105">A beépített több népszerű projektek a platform egyik [rugó rendszerindító], amely lehetővé teszi egy egyszerűsített megközelítés önálló Java-alkalmazások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="8b798-105">One of the more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="8b798-106">[Docker] egy nyílt forráskódú megoldás, amely a fejlesztőket automatizálja a központi telepítési méretezés és a tárolók a futó alkalmazások kezelését.</span><span class="sxs-lookup"><span data-stu-id="8b798-106">[Docker] is an open-source solution that helps developers automate the deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="8b798-107">Ez az oktatóanyag végigvezeti a központi telepítése egy rugó rendszerindító alkalmazásra, amely egy Docker-tároló Microsoft Azure az eclipse-ben az Azure-eszközkészlet használatával.</span><span class="sxs-lookup"><span data-stu-id="8b798-107">This tutorial walks you through the steps to deploy a Spring Boot application as a Docker container to Microsoft Azure by using the Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="clone-the-default-spring-boot-docker-repository"></a><span data-ttu-id="8b798-108">Az alapértelmezett rendszerindító Docker rugó tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="8b798-108">Clone the default Spring Boot Docker repository</span></span>

### <a name="import-the-public-repository"></a><span data-ttu-id="8b798-109">A nyilvános tárház importálása</span><span class="sxs-lookup"><span data-stu-id="8b798-109">Import the public repository</span></span>

<span data-ttu-id="8b798-110">A következő lépések végigvezetik a helyi számítógépen a rugó rendszerindító Docker-tárház klónozása IntelliJ használatával.</span><span class="sxs-lookup"><span data-stu-id="8b798-110">The following steps walk you through cloning the Spring Boot Docker repository to your local computer by using IntelliJ.</span></span> <span data-ttu-id="8b798-111">Ha azt szeretné, a parancssor használatára, lásd: [rugó rendszerindító alkalmazást az Azure Tárolószolgáltatásban Linux központilag][Deploy Spring Boot on Linux in ACS].</span><span class="sxs-lookup"><span data-stu-id="8b798-111">If you want to use a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in ACS].</span></span>

1. <span data-ttu-id="8b798-112">Nyissa meg az eclipse-ben.</span><span class="sxs-lookup"><span data-stu-id="8b798-112">Open Eclipse.</span></span>

1. <span data-ttu-id="8b798-113">Kattintson a **fájl** > **importálási**.</span><span class="sxs-lookup"><span data-stu-id="8b798-113">Click **File** > **Import**.</span></span>

   ![Fájl importálása menü][CL01]

1. <span data-ttu-id="8b798-115">Ha a **importálási** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="8b798-115">When the **Import** dialog box opens:</span></span>

   <span data-ttu-id="8b798-116">a.</span><span class="sxs-lookup"><span data-stu-id="8b798-116">a.</span></span> <span data-ttu-id="8b798-117">Bontsa ki a **Git**.</span><span class="sxs-lookup"><span data-stu-id="8b798-117">Expand **Git**.</span></span>

   <span data-ttu-id="8b798-118">b.</span><span class="sxs-lookup"><span data-stu-id="8b798-118">b.</span></span> <span data-ttu-id="8b798-119">Válassza ki **Projects from Git**.</span><span class="sxs-lookup"><span data-stu-id="8b798-119">Select **Projects from Git**.</span></span>
   
   <span data-ttu-id="8b798-120">c.</span><span class="sxs-lookup"><span data-stu-id="8b798-120">c.</span></span> <span data-ttu-id="8b798-121">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="8b798-121">Click **Next**.</span></span>

   ![Importálása párbeszédpanel][CL02]

1. <span data-ttu-id="8b798-123">Az a **tárház forrásának kijelölése** lap:</span><span class="sxs-lookup"><span data-stu-id="8b798-123">On the **Select Repository Source** page:</span></span>

   <span data-ttu-id="8b798-124">a.</span><span class="sxs-lookup"><span data-stu-id="8b798-124">a.</span></span> <span data-ttu-id="8b798-125">Válassza ki **URI klónozása**.</span><span class="sxs-lookup"><span data-stu-id="8b798-125">Select **Clone URI**.</span></span>
   
   <span data-ttu-id="8b798-126">b.</span><span class="sxs-lookup"><span data-stu-id="8b798-126">b.</span></span> <span data-ttu-id="8b798-127">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="8b798-127">Click **Next**.</span></span>

   ![Válassza ki a tárház forrás lap][CL03]

1. <span data-ttu-id="8b798-129">Az a **forrás Git-tárház** lap:</span><span class="sxs-lookup"><span data-stu-id="8b798-129">On the **Source Git Repository** page:</span></span>

   <span data-ttu-id="8b798-130">a.</span><span class="sxs-lookup"><span data-stu-id="8b798-130">a.</span></span> <span data-ttu-id="8b798-131">A **URI**, adja meg `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span><span class="sxs-lookup"><span data-stu-id="8b798-131">For **URI**, enter `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span></span> <span data-ttu-id="8b798-132">Ez a lépés automatikusan kitölti a **állomás** és **tárház elérési** mezők a megfelelő értékekkel.</span><span class="sxs-lookup"><span data-stu-id="8b798-132">This step should automatically populate the **Host** and **Repository path** fields with the correct values.</span></span>
   
   <span data-ttu-id="8b798-133">b.</span><span class="sxs-lookup"><span data-stu-id="8b798-133">b.</span></span> <span data-ttu-id="8b798-134">A rugó rendszerindító tárház nyilvános, így nem kell megadnia a Git-felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="8b798-134">The Spring Boot repository is public, so you should not have to enter your Git username and password.</span></span>
   
   <span data-ttu-id="8b798-135">c.</span><span class="sxs-lookup"><span data-stu-id="8b798-135">c.</span></span> <span data-ttu-id="8b798-136">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="8b798-136">Click **Next**.</span></span>

   ![Forrás Git-tárház lap][CL04]

1. <span data-ttu-id="8b798-138">Az a **ág kiválasztása** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="8b798-138">On the **Branch Selection** page, click **Next**.</span></span>

   ![Ág kiválasztása lap][CL05]

1. <span data-ttu-id="8b798-140">Az a **helyi cél** lap:</span><span class="sxs-lookup"><span data-stu-id="8b798-140">On the **Local Destination** page:</span></span>

   <span data-ttu-id="8b798-141">a.</span><span class="sxs-lookup"><span data-stu-id="8b798-141">a.</span></span> <span data-ttu-id="8b798-142">Adja meg a helyi mappát, ahol a helyi tárházban.</span><span class="sxs-lookup"><span data-stu-id="8b798-142">Specify the local folder where you want your local repo.</span></span>
   
   <span data-ttu-id="8b798-143">b.</span><span class="sxs-lookup"><span data-stu-id="8b798-143">b.</span></span> <span data-ttu-id="8b798-144">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="8b798-144">Click **Next**.</span></span>

   ![Helyi cél lap][CL06]

1. <span data-ttu-id="8b798-146">Az a **válassza ki a használandó projektek importálása varázsló** lap:</span><span class="sxs-lookup"><span data-stu-id="8b798-146">On the **Select a wizard to use for importing projects** page:</span></span>

   <span data-ttu-id="8b798-147">a.</span><span class="sxs-lookup"><span data-stu-id="8b798-147">a.</span></span> <span data-ttu-id="8b798-148">Válassza ki **általános projektként importálási**.</span><span class="sxs-lookup"><span data-stu-id="8b798-148">Select **Import as a general project**.</span></span>
   
   <span data-ttu-id="8b798-149">b.</span><span class="sxs-lookup"><span data-stu-id="8b798-149">b.</span></span> <span data-ttu-id="8b798-150">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="8b798-150">Click **Next**.</span></span>

   !["A használandó projektek importálása varázsló kiválasztása" című oldalt][CL07]

1. <span data-ttu-id="8b798-152">Az a **Import Projects** lap:</span><span class="sxs-lookup"><span data-stu-id="8b798-152">On the **Import Projects** page:</span></span>

   <span data-ttu-id="8b798-153">a.</span><span class="sxs-lookup"><span data-stu-id="8b798-153">a.</span></span> <span data-ttu-id="8b798-154">Írja be a projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="8b798-154">Specify your project name.</span></span>
   
   <span data-ttu-id="8b798-155">b.</span><span class="sxs-lookup"><span data-stu-id="8b798-155">b.</span></span> <span data-ttu-id="8b798-156">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="8b798-156">Click **Finish**.</span></span>

   ![Lap projektek importálása][CL08]

1. <span data-ttu-id="8b798-158">Ha a tárház sikeresen klónozták, megjelenik az eclipse-ben szereplő összes fájlt.</span><span class="sxs-lookup"><span data-stu-id="8b798-158">When the repository is cloned successfully, you see all the files listed in Eclipse.</span></span>

   ![Helyi tárház][CL09]

### <a name="create-a-maven-project-from-your-local-repository"></a><span data-ttu-id="8b798-160">A helyi tárházból Maven-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="8b798-160">Create a Maven project from your local repository</span></span>

<span data-ttu-id="8b798-161">A rugó rendszerindító Docker-tárház egy befejezett Maven project, mert ezzel fogja a jelen oktatóanyag tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="8b798-161">The Spring Boot Docker repository contains a completed Maven project, which you will use for this tutorial.</span></span> 

1. <span data-ttu-id="8b798-162">Kattintson a **fájl** > **importálási**.</span><span class="sxs-lookup"><span data-stu-id="8b798-162">Click **File** > **Import**.</span></span>

   ![Importálási parancs, a Fájl menü][CL01]

1. <span data-ttu-id="8b798-164">Ha a **importálási** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="8b798-164">When the **Import** dialog box opens:</span></span>

   <span data-ttu-id="8b798-165">a.</span><span class="sxs-lookup"><span data-stu-id="8b798-165">a.</span></span> <span data-ttu-id="8b798-166">Bontsa ki a **Maven**.</span><span class="sxs-lookup"><span data-stu-id="8b798-166">Expand **Maven**.</span></span>
   
   <span data-ttu-id="8b798-167">b.</span><span class="sxs-lookup"><span data-stu-id="8b798-167">b.</span></span> <span data-ttu-id="8b798-168">Válassza ki **meglévő Maven-projektek**.</span><span class="sxs-lookup"><span data-stu-id="8b798-168">Select **Existing Maven Projects**.</span></span>
   
   <span data-ttu-id="8b798-169">c.</span><span class="sxs-lookup"><span data-stu-id="8b798-169">c.</span></span> <span data-ttu-id="8b798-170">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="8b798-170">Click **Next**.</span></span>

   ![Importálása párbeszédpanel][MV01]

1. <span data-ttu-id="8b798-172">Az a **Maven-projektek** lap:</span><span class="sxs-lookup"><span data-stu-id="8b798-172">On the **Maven Projects** page:</span></span>

   <span data-ttu-id="8b798-173">a.</span><span class="sxs-lookup"><span data-stu-id="8b798-173">a.</span></span> <span data-ttu-id="8b798-174">A **gyökérkönyvtár**, adja meg a **teljes** mappa a helyi tárházba.</span><span class="sxs-lookup"><span data-stu-id="8b798-174">For **Root Directory**, specify the **complete** folder in your local repository.</span></span>
   
   <span data-ttu-id="8b798-175">b.</span><span class="sxs-lookup"><span data-stu-id="8b798-175">b.</span></span> <span data-ttu-id="8b798-176">Bontsa ki a **speciális** szakaszt, és adjon meg egy egyéni nevet **sablon**.</span><span class="sxs-lookup"><span data-stu-id="8b798-176">Expand the **Advanced** section, and enter a custom name for **Name template**.</span></span>
   
   <span data-ttu-id="8b798-177">c.</span><span class="sxs-lookup"><span data-stu-id="8b798-177">c.</span></span> <span data-ttu-id="8b798-178">Jelölje be a **pom.xml** fájlt a projektben.</span><span class="sxs-lookup"><span data-stu-id="8b798-178">Select the box for the **pom.xml** file in the project.</span></span>
   
   <span data-ttu-id="8b798-179">d.</span><span class="sxs-lookup"><span data-stu-id="8b798-179">d.</span></span> <span data-ttu-id="8b798-180">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="8b798-180">Click **Finish**.</span></span>

   ![Maven-projektek lap][MV02]

1. <span data-ttu-id="8b798-182">Amikor a Maven project sikeresen meg van nyitva, a második projekt eclipse-ben szereplő látható.</span><span class="sxs-lookup"><span data-stu-id="8b798-182">When the Maven project is opened successfully, you see a second project listed in Eclipse.</span></span>

   ![Helyi Maven project][MV03]

## <a name="build-your-spring-boot-app-by-using-maven"></a><span data-ttu-id="8b798-184">A rugó rendszerindító alkalmazás elkészítésére Maven használatával</span><span class="sxs-lookup"><span data-stu-id="8b798-184">Build your Spring Boot app by using Maven</span></span>

1. <span data-ttu-id="8b798-185">Válassza ki a Maven projekt Eclipse Project Explorer.</span><span class="sxs-lookup"><span data-stu-id="8b798-185">In the Eclipse Project Explorer, select the Maven project.</span></span>

1. <span data-ttu-id="8b798-186">Kattintson a **futtatása** > **futtató** > **Maven build**.</span><span class="sxs-lookup"><span data-stu-id="8b798-186">Click **Run** > **Run As** > **Maven build**.</span></span>

   ![Parancsokat futtató Maven build][BU01]

1. <span data-ttu-id="8b798-188">Ha az alkalmazás sikeresen épül, a konzolablakban állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="8b798-188">When your application is successfully built, the console window shows the status.</span></span>

   ![Sikeres Maven build][BU02]

## <a name="publish-your-web-app-to-azure-by-using-a-docker-container"></a><span data-ttu-id="8b798-190">A webalkalmazás közzététele az Azure-bA egy Docker-tároló használatával</span><span class="sxs-lookup"><span data-stu-id="8b798-190">Publish your web app to Azure by using a Docker container</span></span>

1. <span data-ttu-id="8b798-191">Válassza ki a Maven projekt Eclipse Project Explorer.</span><span class="sxs-lookup"><span data-stu-id="8b798-191">In the Eclipse Project Explorer, select the Maven project.</span></span>

1. <span data-ttu-id="8b798-192">Kattintson az Azure **közzététel** menüben, majd kattintson **Docker-tároló közzététel**.</span><span class="sxs-lookup"><span data-stu-id="8b798-192">Click the Azure **Publish** menu, and then click **Publish as Docker Container**.</span></span>

   ![Docker-tároló parancsként közzététele][PU01]

1. <span data-ttu-id="8b798-194">Ha a **Docker-tároló üzembe helyezése az Azure-on** párbeszédpanel jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="8b798-194">When the **Deploying Docker Container on Azure** dialog box appears:</span></span>

   <span data-ttu-id="8b798-195">a.</span><span class="sxs-lookup"><span data-stu-id="8b798-195">a.</span></span> <span data-ttu-id="8b798-196">Adja meg a Docker egyéni rendszerkép neve.</span><span class="sxs-lookup"><span data-stu-id="8b798-196">Enter a custom Docker image name.</span></span>
   
   <span data-ttu-id="8b798-197">b.</span><span class="sxs-lookup"><span data-stu-id="8b798-197">b.</span></span> <span data-ttu-id="8b798-198">A **összetevő telepítéséhez**, adja meg az elérési útját a **gs-rugó-rendszerindítás – docker-0.1.0.jar** csak beépített fájlt.</span><span class="sxs-lookup"><span data-stu-id="8b798-198">For **Artifact to deploy**, specify the path to the **gs-spring-boot-docker-0.1.0.jar** file you just built.</span></span>

   ![Adja meg a Docker-beállítások][PU02]

   <span data-ttu-id="8b798-200">Bármely létező Docker-gazdagépekből jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="8b798-200">Any existing Docker hosts are displayed.</span></span> 

1. <span data-ttu-id="8b798-201">Ha egy meglévő gazdagépre telepíti, kihagyhatja az 5.</span><span class="sxs-lookup"><span data-stu-id="8b798-201">If you choose to deploy to an existing host, you can skip to step 5.</span></span> <span data-ttu-id="8b798-202">Ellenkező esetben a következő lépések segítségével gazdagép létrehozása:</span><span class="sxs-lookup"><span data-stu-id="8b798-202">Otherwise, use the following steps to create a host:</span></span>

   <span data-ttu-id="8b798-203">a.</span><span class="sxs-lookup"><span data-stu-id="8b798-203">a.</span></span> <span data-ttu-id="8b798-204">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="8b798-204">Click **Add**.</span></span>

      ![Új Docker-gazdagép hozzáadása][PU03]

   <span data-ttu-id="8b798-206">b.</span><span class="sxs-lookup"><span data-stu-id="8b798-206">b.</span></span> <span data-ttu-id="8b798-207">Ha a **Docker állomás létrehozása** párbeszédpanel jelenik meg, dönthet úgy, hogy fogadja el az alapértelmezett vagy az egyéni beállításait az új Docker-állomáshoz is megadhat.</span><span class="sxs-lookup"><span data-stu-id="8b798-207">When the **Create Docker Host** dialog box appears, you can choose to accept the defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="8b798-208">(Különböző beállításának részletes leírását lásd: [egy Docker-tároló egy webalkalmazást az intellij-t az Azure-eszközkészlet használatával közzététel][Publish Container with Azure Toolkit].) Kattintson a **következő** amikor megadott mely beállításokat kívánják használni.</span><span class="sxs-lookup"><span data-stu-id="8b798-208">(For detailed descriptions of the various settings, see [Publish a web app as a Docker container by using the Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings to use.</span></span>

      ![Adja meg a Docker állomás beállításai][PU04]

   <span data-ttu-id="8b798-210">c.</span><span class="sxs-lookup"><span data-stu-id="8b798-210">c.</span></span> <span data-ttu-id="8b798-211">Választhat egy az Azure key vault a meglévő bejelentkezési hitelesítő adatok használatát, vagy dönthet úgy, új Docker bejelentkezési hitelesítő adatok megadásához.</span><span class="sxs-lookup"><span data-stu-id="8b798-211">You can choose to use existing login credentials from an Azure key vault, or you can choose to enter new Docker login credentials.</span></span> <span data-ttu-id="8b798-212">Kattintson a **Befejezés** Ha megadta a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="8b798-212">Click **Finish** when you have specified your options.</span></span>

      ![Docker állomás hitelesítő adatok megadása][PU05]

1. <span data-ttu-id="8b798-214">Válassza ki a Docker-állomás, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="8b798-214">Select your Docker host, and then click **Next**.</span></span>

   ![Válassza ki a Docker állomás használata][PU06]

1. <span data-ttu-id="8b798-216">Az utolsó oldalán a **Docker-tároló üzembe helyezése az Azure-on** párbeszédpanelen adja meg a következő beállításokat:</span><span class="sxs-lookup"><span data-stu-id="8b798-216">On the last page of the **Deploying Docker Container on Azure** dialog box, specify the following options:</span></span>

   <span data-ttu-id="8b798-217">a.</span><span class="sxs-lookup"><span data-stu-id="8b798-217">a.</span></span> <span data-ttu-id="8b798-218">Adjon meg egy egyéni nevet, a tároló, amely üzemelteti a Docker-tároló választhat, vagy elfogadhatja az alapértelmezett.</span><span class="sxs-lookup"><span data-stu-id="8b798-218">You can choose to specify a custom name for the container that will host your Docker container, or you can accept the default.</span></span>

   <span data-ttu-id="8b798-219">b.</span><span class="sxs-lookup"><span data-stu-id="8b798-219">b.</span></span> <span data-ttu-id="8b798-220">Adja meg a TCP-portok a docker-állomás a következő szintaxissal: *[külső port]*:*[belső port]*.</span><span class="sxs-lookup"><span data-stu-id="8b798-220">Enter the TCP ports for your docker host by using the following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="8b798-221">Például **80:8080** megadja egy külső port a 80-as és az alapértelmezett belső rugó rendszerindító port a 8080-as.</span><span class="sxs-lookup"><span data-stu-id="8b798-221">For example, **80:8080** specifies an external port of 80 and the default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="8b798-222">Ha testreszabta a belső port (például a a application.yml fájl szerkesztésével), meg kell adnia a portszámot a megfelelő irányításához megtörténik az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="8b798-222">If you have customized your internal port (for example, by editing the application.yml file), you need to specify the port number for the correct routing to occur in Azure.</span></span>

   <span data-ttu-id="8b798-223">c.</span><span class="sxs-lookup"><span data-stu-id="8b798-223">c.</span></span> <span data-ttu-id="8b798-224">Ezek a beállítások megadása után kattintson az **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="8b798-224">After you configure these options, click **Finish**.</span></span>

   ![Egy Docker-tároló az Azure telepítéséhez][PU07]

1. <span data-ttu-id="8b798-226">Ha az Azure-eszközkészlet közzététele befejeződött, az Azure tevékenységnapló megjeleníti **közzétett** az állapot.</span><span class="sxs-lookup"><span data-stu-id="8b798-226">When the Azure Toolkit has finished publishing, the Azure Activity Log displays **Published** for the status.</span></span>

   ![Docker-gazdagép telepítése sikeres volt][PU08]

## <a name="next-steps"></a><span data-ttu-id="8b798-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8b798-228">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
<span data-ttu-id="8b798-229">[Docker]: https://www.docker.com/</span><span class="sxs-lookup"><span data-stu-id="8b798-229">[Docker]: https://www.docker.com/</span></span>
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
<span data-ttu-id="8b798-230">[rugó rendszerindító]: http://projects.spring.io/spring-boot/</span><span class="sxs-lookup"><span data-stu-id="8b798-230">[Spring Boot]: http://projects.spring.io/spring-boot/</span></span>
<span data-ttu-id="8b798-231">[rugó keretrendszer]: https://spring.io/</span><span class="sxs-lookup"><span data-stu-id="8b798-231">[Spring Framework]: https://spring.io/</span></span>

<!-- IMG List -->

[CL01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL01.png
[CL02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL02.png
[CL03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL03.png
[CL04]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL04.png
[CL05]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL05.png
[CL06]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL06.png
[CL07]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL07.png
[CL08]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL08.png
[CL09]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/CL09.png

[MV01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV01.png
[MV02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV02.png
[MV03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/MV03.png

[BU01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU01.png
[BU02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/BU02.png

[PU01]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU01.png
[PU02]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU02.png
[PU03]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU03.png
[PU04]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU04.png
[PU05]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU05.png
[PU06]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU06.png
[PU07]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU07.png
[PU08]: ./media/azure-toolkit-for-eclipse-publish-spring-boot-docker-app/PU08.png
