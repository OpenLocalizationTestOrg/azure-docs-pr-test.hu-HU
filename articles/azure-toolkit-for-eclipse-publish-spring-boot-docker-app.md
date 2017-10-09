---
title: "a rugó rendszerindító alkalmazást egy Docker-tároló segítségével aaaPublish hello Azure eszköztára Eclipse |} Microsoft Docs"
description: "Megtudhatja, hogyan toopublish egy webes alkalmazás tooMicrosoft Azure-bA egy Docker-tároló segítségével hello Azure eszköztára eclipse-ben."
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
ms.openlocfilehash: 29390c49c339a1ebb87cb3951b21cea01c0da15f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="publish-a-spring-boot-app-as-a-docker-container-by-using-hello-azure-toolkit-for-eclipse"></a><span data-ttu-id="ec0a9-103">A rugó rendszerindító alkalmazás közzététele egy Docker-tároló, Eclipse hello Azure eszközkészlet használatával</span><span class="sxs-lookup"><span data-stu-id="ec0a9-103">Publish a Spring Boot app as a Docker container by using hello Azure Toolkit for Eclipse</span></span>

<span data-ttu-id="ec0a9-104">Hello [rugó keretrendszer] egy nyílt forráskódú megoldás, amely a fejlesztőket Java vállalati szintű alkalmazásokat.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-104">hello [Spring Framework] is an open-source solution that helps Java developers create enterprise-level applications.</span></span> <span data-ttu-id="ec0a9-105">Hello több népszerű projektekre beépített a platform egyik [rugó rendszerindító], amely lehetővé teszi egy egyszerűsített megközelítés önálló Java-alkalmazások létrehozása.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-105">One of hello more-popular projects that is built on top of that platform is [Spring Boot], which provides a simplified approach for creating standalone Java applications.</span></span>

<span data-ttu-id="ec0a9-106">[Docker] egy nyílt forráskódú megoldás, amely a fejlesztőket automatizálásához hello telepítési méretezés és a tárolók a futó alkalmazások kezelését.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-106">[Docker] is an open-source solution that helps developers automate hello deployment, scaling, and management of their applications that are running in containers.</span></span>

<span data-ttu-id="ec0a9-107">Ez az oktatóanyag bemutatja, hogyan hello lépéseket toodeploy egy rugó rendszerindító alkalmazásra, amely egy Docker-tároló tooMicrosoft Azure Eclipse hello Azure eszközkészlet használatával.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-107">This tutorial walks you through hello steps toodeploy a Spring Boot application as a Docker container tooMicrosoft Azure by using hello Azure Toolkit for Eclipse.</span></span>

[!INCLUDE [azure-toolkit-for-eclipse-prerequisites](../includes/azure-toolkit-for-eclipse-prerequisites.md)]

## <a name="clone-hello-default-spring-boot-docker-repository"></a><span data-ttu-id="ec0a9-108">Hello alapértelmezett rugó rendszerindító Docker tárház klónozása</span><span class="sxs-lookup"><span data-stu-id="ec0a9-108">Clone hello default Spring Boot Docker repository</span></span>

### <a name="import-hello-public-repository"></a><span data-ttu-id="ec0a9-109">Importálás hello nyilvános tárház</span><span class="sxs-lookup"><span data-stu-id="ec0a9-109">Import hello public repository</span></span>

<span data-ttu-id="ec0a9-110">hello következő lépések végigvezetik hello rugó rendszerindító Docker tárház tooyour helyi számítógép klónozása IntelliJ használatával.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-110">hello following steps walk you through cloning hello Spring Boot Docker repository tooyour local computer by using IntelliJ.</span></span> <span data-ttu-id="ec0a9-111">Ha azt szeretné, hogy a parancssor toouse, [rugó rendszerindító alkalmazást az Azure Tárolószolgáltatásban Linux központilag][Deploy Spring Boot on Linux in ACS].</span><span class="sxs-lookup"><span data-stu-id="ec0a9-111">If you want toouse a command line, see [Deploy a Spring Boot application on Linux in Azure Container Service][Deploy Spring Boot on Linux in ACS].</span></span>

1. <span data-ttu-id="ec0a9-112">Nyissa meg az eclipse-ben.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-112">Open Eclipse.</span></span>

1. <span data-ttu-id="ec0a9-113">Kattintson a **fájl** > **importálási**.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-113">Click **File** > **Import**.</span></span>

   ![Fájl importálása menü][CL01]

1. <span data-ttu-id="ec0a9-115">Ha hello **importálási** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="ec0a9-115">When hello **Import** dialog box opens:</span></span>

   <span data-ttu-id="ec0a9-116">a.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-116">a.</span></span> <span data-ttu-id="ec0a9-117">Bontsa ki a **Git**.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-117">Expand **Git**.</span></span>

   <span data-ttu-id="ec0a9-118">b.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-118">b.</span></span> <span data-ttu-id="ec0a9-119">Válassza ki **Projects from Git**.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-119">Select **Projects from Git**.</span></span>
   
   <span data-ttu-id="ec0a9-120">c.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-120">c.</span></span> <span data-ttu-id="ec0a9-121">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-121">Click **Next**.</span></span>

   ![Importálása párbeszédpanel][CL02]

1. <span data-ttu-id="ec0a9-123">A hello **tárház forrásának kijelölése** lap:</span><span class="sxs-lookup"><span data-stu-id="ec0a9-123">On hello **Select Repository Source** page:</span></span>

   <span data-ttu-id="ec0a9-124">a.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-124">a.</span></span> <span data-ttu-id="ec0a9-125">Válassza ki **URI klónozása**.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-125">Select **Clone URI**.</span></span>
   
   <span data-ttu-id="ec0a9-126">b.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-126">b.</span></span> <span data-ttu-id="ec0a9-127">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-127">Click **Next**.</span></span>

   ![Válassza ki a tárház forrás lap][CL03]

1. <span data-ttu-id="ec0a9-129">A hello **forrás Git-tárház** lap:</span><span class="sxs-lookup"><span data-stu-id="ec0a9-129">On hello **Source Git Repository** page:</span></span>

   <span data-ttu-id="ec0a9-130">a.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-130">a.</span></span> <span data-ttu-id="ec0a9-131">A **URI**, adja meg `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-131">For **URI**, enter `https://github.com/spring-guides/gs-spring-boot-docker.git`.</span></span> <span data-ttu-id="ec0a9-132">Ez a lépés automatikusan kitölti hello **állomás** és **tárház elérési** mezőket a hello javítsa az értékeket.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-132">This step should automatically populate hello **Host** and **Repository path** fields with hello correct values.</span></span>
   
   <span data-ttu-id="ec0a9-133">b.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-133">b.</span></span> <span data-ttu-id="ec0a9-134">hello rugó rendszerindító tárház nyilvános, így nem kell tooenter a Git-felhasználónevét és jelszavát.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-134">hello Spring Boot repository is public, so you should not have tooenter your Git username and password.</span></span>
   
   <span data-ttu-id="ec0a9-135">c.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-135">c.</span></span> <span data-ttu-id="ec0a9-136">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-136">Click **Next**.</span></span>

   ![Forrás Git-tárház lap][CL04]

1. <span data-ttu-id="ec0a9-138">A hello **ág kiválasztása** kattintson **következő**.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-138">On hello **Branch Selection** page, click **Next**.</span></span>

   ![Ág kiválasztása lap][CL05]

1. <span data-ttu-id="ec0a9-140">A hello **helyi cél** lap:</span><span class="sxs-lookup"><span data-stu-id="ec0a9-140">On hello **Local Destination** page:</span></span>

   <span data-ttu-id="ec0a9-141">a.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-141">a.</span></span> <span data-ttu-id="ec0a9-142">Adja meg a hello helyi mappát, ahol a helyi tárházban.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-142">Specify hello local folder where you want your local repo.</span></span>
   
   <span data-ttu-id="ec0a9-143">b.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-143">b.</span></span> <span data-ttu-id="ec0a9-144">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-144">Click **Next**.</span></span>

   ![Helyi cél lap][CL06]

1. <span data-ttu-id="ec0a9-146">A hello **válassza ki a projektek importálásához varázsló toouse** oldalon:</span><span class="sxs-lookup"><span data-stu-id="ec0a9-146">On hello **Select a wizard toouse for importing projects** page:</span></span>

   <span data-ttu-id="ec0a9-147">a.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-147">a.</span></span> <span data-ttu-id="ec0a9-148">Válassza ki **általános projektként importálási**.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-148">Select **Import as a general project**.</span></span>
   
   <span data-ttu-id="ec0a9-149">b.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-149">b.</span></span> <span data-ttu-id="ec0a9-150">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-150">Click **Next**.</span></span>

   !["A projektek importálásához varázsló toouse kiválasztása" című oldalt][CL07]

1. <span data-ttu-id="ec0a9-152">A hello **Import Projects** lap:</span><span class="sxs-lookup"><span data-stu-id="ec0a9-152">On hello **Import Projects** page:</span></span>

   <span data-ttu-id="ec0a9-153">a.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-153">a.</span></span> <span data-ttu-id="ec0a9-154">Írja be a projekt nevét.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-154">Specify your project name.</span></span>
   
   <span data-ttu-id="ec0a9-155">b.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-155">b.</span></span> <span data-ttu-id="ec0a9-156">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-156">Click **Finish**.</span></span>

   ![Lap projektek importálása][CL08]

1. <span data-ttu-id="ec0a9-158">Ha hello tárház sikeresen klónozták, megjelenik az eclipse-ben szereplő összes hello fájlokat.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-158">When hello repository is cloned successfully, you see all hello files listed in Eclipse.</span></span>

   ![Helyi tárház][CL09]

### <a name="create-a-maven-project-from-your-local-repository"></a><span data-ttu-id="ec0a9-160">A helyi tárházból Maven-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="ec0a9-160">Create a Maven project from your local repository</span></span>

<span data-ttu-id="ec0a9-161">hello rugó rendszerindító Docker-tárház egy befejezett Maven project, mert ezzel fogja a jelen oktatóanyag tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-161">hello Spring Boot Docker repository contains a completed Maven project, which you will use for this tutorial.</span></span> 

1. <span data-ttu-id="ec0a9-162">Kattintson a **fájl** > **importálási**.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-162">Click **File** > **Import**.</span></span>

   ![Az import parancs hello fájl menü][CL01]

1. <span data-ttu-id="ec0a9-164">Ha hello **importálási** párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="ec0a9-164">When hello **Import** dialog box opens:</span></span>

   <span data-ttu-id="ec0a9-165">a.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-165">a.</span></span> <span data-ttu-id="ec0a9-166">Bontsa ki a **Maven**.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-166">Expand **Maven**.</span></span>
   
   <span data-ttu-id="ec0a9-167">b.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-167">b.</span></span> <span data-ttu-id="ec0a9-168">Válassza ki **meglévő Maven-projektek**.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-168">Select **Existing Maven Projects**.</span></span>
   
   <span data-ttu-id="ec0a9-169">c.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-169">c.</span></span> <span data-ttu-id="ec0a9-170">Kattintson a **Tovább** gombra.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-170">Click **Next**.</span></span>

   ![Importálása párbeszédpanel][MV01]

1. <span data-ttu-id="ec0a9-172">A hello **Maven-projektek** lap:</span><span class="sxs-lookup"><span data-stu-id="ec0a9-172">On hello **Maven Projects** page:</span></span>

   <span data-ttu-id="ec0a9-173">a.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-173">a.</span></span> <span data-ttu-id="ec0a9-174">A **gyökérkönyvtár**, adja meg a hello **teljes** mappa a helyi tárházba.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-174">For **Root Directory**, specify hello **complete** folder in your local repository.</span></span>
   
   <span data-ttu-id="ec0a9-175">b.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-175">b.</span></span> <span data-ttu-id="ec0a9-176">Bontsa ki a hello **speciális** szakaszt, és adjon meg egy egyéni nevet **sablon**.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-176">Expand hello **Advanced** section, and enter a custom name for **Name template**.</span></span>
   
   <span data-ttu-id="ec0a9-177">c.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-177">c.</span></span> <span data-ttu-id="ec0a9-178">Jelölje be hello be a hello **pom.xml** fájl hello projektben.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-178">Select hello box for hello **pom.xml** file in hello project.</span></span>
   
   <span data-ttu-id="ec0a9-179">d.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-179">d.</span></span> <span data-ttu-id="ec0a9-180">Kattintson a **Befejezés** gombra.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-180">Click **Finish**.</span></span>

   ![Maven-projektek lap][MV02]

1. <span data-ttu-id="ec0a9-182">Amikor a hello Maven project sikeresen meg van nyitva, a második projekt eclipse-ben szereplő látható.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-182">When hello Maven project is opened successfully, you see a second project listed in Eclipse.</span></span>

   ![Helyi Maven project][MV03]

## <a name="build-your-spring-boot-app-by-using-maven"></a><span data-ttu-id="ec0a9-184">A rugó rendszerindító alkalmazás elkészítésére Maven használatával</span><span class="sxs-lookup"><span data-stu-id="ec0a9-184">Build your Spring Boot app by using Maven</span></span>

1. <span data-ttu-id="ec0a9-185">Az Eclipse Project Explorer hello válassza ki a hello Maven project.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-185">In hello Eclipse Project Explorer, select hello Maven project.</span></span>

1. <span data-ttu-id="ec0a9-186">Kattintson a **futtatása** > **futtató** > **Maven build**.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-186">Click **Run** > **Run As** > **Maven build**.</span></span>

   ![Parancsok toorun Maven build szerint][BU01]

1. <span data-ttu-id="ec0a9-188">Ha az alkalmazás sikeresen épül, a konzolablakban hello hello állapotát jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-188">When your application is successfully built, hello console window shows hello status.</span></span>

   ![Sikeres Maven build][BU02]

## <a name="publish-your-web-app-tooazure-by-using-a-docker-container"></a><span data-ttu-id="ec0a9-190">A webes alkalmazás tooAzure közzététele egy Docker-tároló használatával</span><span class="sxs-lookup"><span data-stu-id="ec0a9-190">Publish your web app tooAzure by using a Docker container</span></span>

1. <span data-ttu-id="ec0a9-191">Az Eclipse Project Explorer hello válassza ki a hello Maven project.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-191">In hello Eclipse Project Explorer, select hello Maven project.</span></span>

1. <span data-ttu-id="ec0a9-192">Kattintson a hello Azure **közzététel** menüben, majd kattintson **Docker-tároló közzététel**.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-192">Click hello Azure **Publish** menu, and then click **Publish as Docker Container**.</span></span>

   ![Docker-tároló parancsként közzététele][PU01]

1. <span data-ttu-id="ec0a9-194">Ha hello **Docker-tároló üzembe helyezése az Azure-on** párbeszédpanel jelenik meg:</span><span class="sxs-lookup"><span data-stu-id="ec0a9-194">When hello **Deploying Docker Container on Azure** dialog box appears:</span></span>

   <span data-ttu-id="ec0a9-195">a.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-195">a.</span></span> <span data-ttu-id="ec0a9-196">Adja meg a Docker egyéni rendszerkép neve.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-196">Enter a custom Docker image name.</span></span>
   
   <span data-ttu-id="ec0a9-197">b.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-197">b.</span></span> <span data-ttu-id="ec0a9-198">A **összetevő toodeploy**, adja meg a hello elérési toohello **gs-rugó-rendszerindítás – docker-0.1.0.jar** csak beépített fájlt.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-198">For **Artifact toodeploy**, specify hello path toohello **gs-spring-boot-docker-0.1.0.jar** file you just built.</span></span>

   ![Adja meg a Docker-beállítások][PU02]

   <span data-ttu-id="ec0a9-200">Bármely létező Docker-gazdagépekből jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-200">Any existing Docker hosts are displayed.</span></span> 

1. <span data-ttu-id="ec0a9-201">Ha úgy dönt, hogy toodeploy tooan meglévő állomás, kihagyhatja toostep 5.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-201">If you choose toodeploy tooan existing host, you can skip toostep 5.</span></span> <span data-ttu-id="ec0a9-202">Ellenkező esetben használja a következő lépéseket toocreate állomás hello:</span><span class="sxs-lookup"><span data-stu-id="ec0a9-202">Otherwise, use hello following steps toocreate a host:</span></span>

   <span data-ttu-id="ec0a9-203">a.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-203">a.</span></span> <span data-ttu-id="ec0a9-204">Kattintson az **Add** (Hozzáadás) parancsra.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-204">Click **Add**.</span></span>

      ![Új Docker-gazdagép hozzáadása][PU03]

   <span data-ttu-id="ec0a9-206">b.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-206">b.</span></span> <span data-ttu-id="ec0a9-207">Ha hello **Docker állomás létrehozása** párbeszédpanel jelenik meg, dönthet, hogy tooaccept hello alapértelmezett értékeket, vagy megadhatja az egyéni beállításait az új Docker-állomáshoz.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-207">When hello **Create Docker Host** dialog box appears, you can choose tooaccept hello defaults, or you can specify any custom settings for your new Docker host.</span></span> <span data-ttu-id="ec0a9-208">(A részletes leírását hello különböző beállításairól [a webes alkalmazás is egy Docker-tároló közzé a IntelliJ hello Azure eszközkészlet használatával][Publish Container with Azure Toolkit].) Kattintson a **következő** esetén mely beállítások toouse megadott.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-208">(For detailed descriptions of hello various settings, see [Publish a web app as a Docker container by using hello Azure Toolkit for IntelliJ][Publish Container with Azure Toolkit].) Click **Next** when you have specified which settings toouse.</span></span>

      ![Adja meg a Docker állomás beállításai][PU04]

   <span data-ttu-id="ec0a9-210">c.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-210">c.</span></span> <span data-ttu-id="ec0a9-211">Az az Azure key vault közül választhat a meglévő toouse bejelentkezési hitelesítő adatok, vagy dönthet úgy tooenter új Docker bejelentkezési hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-211">You can choose toouse existing login credentials from an Azure key vault, or you can choose tooenter new Docker login credentials.</span></span> <span data-ttu-id="ec0a9-212">Kattintson a **Befejezés** Ha megadta a beállításokat.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-212">Click **Finish** when you have specified your options.</span></span>

      ![Docker állomás hitelesítő adatok megadása][PU05]

1. <span data-ttu-id="ec0a9-214">Válassza ki a Docker-állomás, és kattintson a **következő**.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-214">Select your Docker host, and then click **Next**.</span></span>

   ![Válassza ki a Docker állomás toouse][PU06]

1. <span data-ttu-id="ec0a9-216">Hello utolsó oldalán hello **Docker-tároló üzembe helyezése az Azure-on** párbeszédpanelen adja meg az alábbi beállítások hello:</span><span class="sxs-lookup"><span data-stu-id="ec0a9-216">On hello last page of hello **Deploying Docker Container on Azure** dialog box, specify hello following options:</span></span>

   <span data-ttu-id="ec0a9-217">a.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-217">a.</span></span> <span data-ttu-id="ec0a9-218">Dönthet toospecify hello tárolót a Docker-tároló futtató egyéni nevet, vagy elfogadhatja az alapértelmezett hello.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-218">You can choose toospecify a custom name for hello container that will host your Docker container, or you can accept hello default.</span></span>

   <span data-ttu-id="ec0a9-219">b.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-219">b.</span></span> <span data-ttu-id="ec0a9-220">Adja meg a TCP-portok hello a docker állomás hello a következő szintaxis használatával: *[külső port]*:*[belső port]*.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-220">Enter hello TCP ports for your docker host by using hello following syntax: *[external port]*:*[internal port]*.</span></span> <span data-ttu-id="ec0a9-221">Például **80:8080** egy külső port a 80-as és hello alapértelmezett belső rugó rendszerindító port a 8080-as határozza meg.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-221">For example, **80:8080** specifies an external port of 80 and hello default internal Spring Boot port of 8080.</span></span>
   
      <span data-ttu-id="ec0a9-222">Ha a belső port testreszabott (például a hello application.yml fájl szerkesztésével), szüksége toospecify hello portszám hello megfelelő útválasztási toooccur az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-222">If you have customized your internal port (for example, by editing hello application.yml file), you need toospecify hello port number for hello correct routing toooccur in Azure.</span></span>

   <span data-ttu-id="ec0a9-223">c.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-223">c.</span></span> <span data-ttu-id="ec0a9-224">Ezek a beállítások megadása után kattintson az **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-224">After you configure these options, click **Finish**.</span></span>

   ![Egy Docker-tároló az Azure telepítéséhez][PU07]

1. <span data-ttu-id="ec0a9-226">Amikor hello Azure eszközkészlet közzététele befejeződött, hello Azure tevékenységnapló megjeleníti **közzétett** hello állapot.</span><span class="sxs-lookup"><span data-stu-id="ec0a9-226">When hello Azure Toolkit has finished publishing, hello Azure Activity Log displays **Published** for hello status.</span></span>

   ![Docker-gazdagép telepítése sikeres volt][PU08]

## <a name="next-steps"></a><span data-ttu-id="ec0a9-228">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ec0a9-228">Next steps</span></span>

[!INCLUDE [azure-toolkit-additional-resources](../includes/azure-toolkit-additional-resources.md)]

<!-- URL List -->

[Azure Management Portal]: http://go.microsoft.com/fwlink/?LinkID=512959
[Deploy Spring Boot on Linux in ACS]:container-service/kubernetes/container-service-deploy-spring-boot-app-on-linux.md
[Docker]: https://www.docker.com/
[Publish Container with Azure Toolkit]: ./azure-toolkit-for-intellij-publish-as-docker-container.md
[rugó rendszerindító]: http://projects.spring.io/spring-boot/
[rugó keretrendszer]: https://spring.io/

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
