---
title: "Egy alapszintű Azure-webalkalmazás létrehozása az IntelliJ |} Microsoft Docs"
description: "Az oktatóanyag bemutatja, hogyan használható az IntelliJ Azure eszköztára Hello World webalkalmazás létrehozása az Azure."
services: app-service\web
documentationcenter: java
author: selvasingh
manager: erikre
editor: 
ms.assetid: 75ce7b36-e3ae-491d-8305-4b42ce37db4e
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 04/25/2017
ms.author: robmcm
ms.openlocfilehash: 20b2c3d86f5bde9302647f345aa99b030d78613a
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="create-a-basic-azure-web-app-in-intellij"></a><span data-ttu-id="2b66f-103">Egy alapszintű Azure-webalkalmazás létrehozása az intellij-t</span><span class="sxs-lookup"><span data-stu-id="2b66f-103">Create a basic Azure web app in IntelliJ</span></span>
<span data-ttu-id="2b66f-104">Ez az oktatóanyag bemutatja, hogyan hozhat létre és telepíthet egy alapszintű Hello World alkalmazásról az Azure-ba, a webes alkalmazás segítségével a [IntelliJ Azure eszköztára].</span><span class="sxs-lookup"><span data-stu-id="2b66f-104">This tutorial shows how to create and deploy a basic Hello World application to Azure as a Web App by using the [Azure Toolkit for IntelliJ].</span></span> <span data-ttu-id="2b66f-105">Egy alapszintű JSP példa az egyszerűség, de hasonló lépésekkel akkor lehet megfelelő, egy Java servlet, az Azure-telepítés illetően.</span><span class="sxs-lookup"><span data-stu-id="2b66f-105">A basic JSP example is shown for simplicity, but similar steps would be appropriate for a Java servlet, as far as Azure deployment is concerned.</span></span>

<span data-ttu-id="2b66f-106">Ez az oktatóanyag befejezése után, az alkalmazás majd meg az alábbi ábrához hasonlóan egy webböngészőben megtekintheti:</span><span class="sxs-lookup"><span data-stu-id="2b66f-106">When you have completed this tutorial, your application will look similar to the following illustration when you view it in a web browser:</span></span>

![A minta-weblap][01]

## <a name="prerequisites"></a><span data-ttu-id="2b66f-108">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="2b66f-108">Prerequisites</span></span>
* <span data-ttu-id="2b66f-109">A Java fejlesztői készlet (JDK), v 1,8 vagy újabb.</span><span class="sxs-lookup"><span data-stu-id="2b66f-109">A Java Developer Kit (JDK), v 1.8 or later.</span></span>
* <span data-ttu-id="2b66f-110">Az IntelliJ IDEA Ultimate Edition.</span><span class="sxs-lookup"><span data-stu-id="2b66f-110">IntelliJ IDEA Ultimate Edition.</span></span> <span data-ttu-id="2b66f-111">Ez letölthető <https://www.jetbrains.com/idea/download/index.html>.</span><span class="sxs-lookup"><span data-stu-id="2b66f-111">This can be downloaded from <https://www.jetbrains.com/idea/download/index.html>.</span></span>
* <span data-ttu-id="2b66f-112">A Java-alapú webkiszolgáló vagy a kiszolgáló, terjesztési például [Apache Tomcat] vagy [Jetty].</span><span class="sxs-lookup"><span data-stu-id="2b66f-112">A distribution of a Java-based web server or application server, such as [Apache Tomcat] or [Jetty].</span></span>
* <span data-ttu-id="2b66f-113">Azure-előfizetéssel, amely szerezhető: a <https://azure.microsoft.com/free/> vagy <http://azure.microsoft.com/pricing/purchase-options/>.</span><span class="sxs-lookup"><span data-stu-id="2b66f-113">An Azure subscription, which can be acquired from <https://azure.microsoft.com/free/> or <http://azure.microsoft.com/pricing/purchase-options/>.</span></span>
* <span data-ttu-id="2b66f-114">A [IntelliJ Azure eszköztára].</span><span class="sxs-lookup"><span data-stu-id="2b66f-114">The [Azure Toolkit for IntelliJ].</span></span> <span data-ttu-id="2b66f-115">Az Azure-eszközkészlet telepítésével kapcsolatos információkért lásd: [az intellij-t az Azure eszközkészlet telepítésével].</span><span class="sxs-lookup"><span data-stu-id="2b66f-115">For information about installing the Azure Toolkit, see [Installing the Azure Toolkit for IntelliJ].</span></span>

## <a name="to-create-a-hello-world-application"></a><span data-ttu-id="2b66f-116">Hello World alkalmazás létrehozása</span><span class="sxs-lookup"><span data-stu-id="2b66f-116">To create a Hello World application</span></span>
<span data-ttu-id="2b66f-117">Először először foglalkozunk létrehoz egy Java-projektet.</span><span class="sxs-lookup"><span data-stu-id="2b66f-117">First, we'll start off with creating a Java project.</span></span>

1. <span data-ttu-id="2b66f-118">Indítsa el az intellij-t, és kattintson a **fájl** menü, majd kattintson a **új**, és kattintson a **projekt**.</span><span class="sxs-lookup"><span data-stu-id="2b66f-118">Start IntelliJ and click the **File** menu, then click **New**, and then click **Project**.</span></span>
   
    ![Fájl új projekt][02]
2. <span data-ttu-id="2b66f-120">Jelölje ki az új projekt párbeszédpanel **Java**, majd **webalkalmazás**, és kattintson a **új** hozzáadása a projekthez SDK.</span><span class="sxs-lookup"><span data-stu-id="2b66f-120">In the New Project dialog box, select **Java**, then **Web Application**, and then click **New** to add a Project SDK.</span></span>
   
    ![Új projekt párbeszédpanel][03a]
   
3. <span data-ttu-id="2b66f-122">A Kezdőkönyvtár kiválasztása a JDK párbeszédpanel megnyitásához, válassza ki a mappát, ahol a JDK telepítve van, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="2b66f-122">In the Select Home Directory for JDK dialog box, select the folder where your JDK is installed, and then click **OK**.</span></span> <span data-ttu-id="2b66f-123">Kattintson a **következő** folytatja az új projekt párbeszédpanel.</span><span class="sxs-lookup"><span data-stu-id="2b66f-123">Click **Next** in the New Project dialog box to continue.</span></span>
   
    ![Adja meg a JDK kezdőkönyvtára][03b]
4. <span data-ttu-id="2b66f-125">Ebben az oktatóanyagban a nevet a projektnek **Java-webalkalmazás-alkalmazás-a-Azure**, és kattintson a **Befejezés**.</span><span class="sxs-lookup"><span data-stu-id="2b66f-125">For purposes of this tutorial, name the project **Java-Web-App-On-Azure**, and then click **Finish**.</span></span>
   
    ![Új projekt párbeszédpanel][04]
5. <span data-ttu-id="2b66f-127">Az IntelliJ a Project Explorer nézet, bontsa ki a **Java-webalkalmazás-alkalmazás-a-Azure**, majd bontsa ki a **webes**, majd kattintson duplán **index.jsp**.</span><span class="sxs-lookup"><span data-stu-id="2b66f-127">Within IntelliJ's Project Explorer view, expand **Java-Web-App-On-Azure**, then expand **web**, and then double-click **index.jsp**.</span></span>
   
    ![Nyissa meg az Index lap][05c]
6. <span data-ttu-id="2b66f-129">Az index.jsp fájl megnyitásakor az IntelliJ beépülő modul dinamikusan megjelenő szöveg **Hello World!**</span><span class="sxs-lookup"><span data-stu-id="2b66f-129">When your index.jsp file opens in IntelliJ, add in text to dynamically display **Hello World!**</span></span> <span data-ttu-id="2b66f-130">a már meglévő `<body>` elemhez.</span><span class="sxs-lookup"><span data-stu-id="2b66f-130">within the existing `<body>` element.</span></span> <span data-ttu-id="2b66f-131">A frissített `<body>` tartalma az alábbihoz kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="2b66f-131">Your updated `<body>` content should resemble the following example:</span></span>
   
    `<body><b><% out.println("Hello World!"); %></b></body>` 
7. <span data-ttu-id="2b66f-132">Mentse az index.jsp.</span><span class="sxs-lookup"><span data-stu-id="2b66f-132">Save index.jsp.</span></span>

## <a name="to-deploy-your-application-to-an-azure-web-app-container"></a><span data-ttu-id="2b66f-133">Egy Azure Web App tárolóhoz az alkalmazás központi telepítése</span><span class="sxs-lookup"><span data-stu-id="2b66f-133">To deploy your application to an Azure Web App Container</span></span>
<span data-ttu-id="2b66f-134">Számos módon Azure Java-webalkalmazás-telepíthet.</span><span class="sxs-lookup"><span data-stu-id="2b66f-134">There are several ways by which you can deploy a Java web application to Azure.</span></span> <span data-ttu-id="2b66f-135">Ez az oktatóanyag leírja a legegyszerűbb egyikét: az alkalmazás telepítve lesz az Azure Web App tároló - semmilyen különleges projekttípus sem további eszközök van szükség.</span><span class="sxs-lookup"><span data-stu-id="2b66f-135">This tutorial describes one of the simplest: your application will be deployed to an Azure Web App Container - no special project type nor additional tools are needed.</span></span> <span data-ttu-id="2b66f-136">A JDK és a webes tároló szoftver biztosítja az Ön Azure-ban, nincs szükség töltse fel a saját; szüksége a Java-webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="2b66f-136">The JDK and the web container software will be provided for you by Azure, so there is no need to upload your own; all you need is your Java Web App.</span></span> <span data-ttu-id="2b66f-137">Ennek eredményeképpen a közzétételt az alkalmazás érvénybe másodperc, nem perc.</span><span class="sxs-lookup"><span data-stu-id="2b66f-137">As a result, the publishing process for your application will take seconds, not minutes.</span></span>

<span data-ttu-id="2b66f-138">Az alkalmazás közzététele előtt először a modul beállítások konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="2b66f-138">Before you publish your application, you first need to configure your module settings.</span></span> <span data-ttu-id="2b66f-139">Ehhez a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="2b66f-139">To do so, use the following steps:</span></span>

1. <span data-ttu-id="2b66f-140">Az intellij-t a Project Explorer, kattintson a jobb gombbal a **Java-webalkalmazás-alkalmazás-a-Azure** projekt.</span><span class="sxs-lookup"><span data-stu-id="2b66f-140">In IntelliJ's Project Explorer, right-click the **Java-Web-App-On-Azure** project.</span></span> <span data-ttu-id="2b66f-141">Amikor a helyi menü megjelenik, kattintson a **beállítások modul megnyitása**.</span><span class="sxs-lookup"><span data-stu-id="2b66f-141">When the context menu appears, click **Open Module Settings**.</span></span>

    ![Nyissa meg a modul beállításai][05a]
2. <span data-ttu-id="2b66f-143">Amikor megjelenik a projekt struktúra párbeszédpanel:</span><span class="sxs-lookup"><span data-stu-id="2b66f-143">When the Project Structure dialog box appears:</span></span>

   <span data-ttu-id="2b66f-144">a.</span><span class="sxs-lookup"><span data-stu-id="2b66f-144">a.</span></span> <span data-ttu-id="2b66f-145">Kattintson a **összetevők** listájában **Projektbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="2b66f-145">Click **Artifacts** in the list of **Project Settings**.</span></span>
   <span data-ttu-id="2b66f-146">b.</span><span class="sxs-lookup"><span data-stu-id="2b66f-146">b.</span></span> <span data-ttu-id="2b66f-147">Az összetevő neve, a módosítása a **neve** jelölőnégyzetet, hogy nem tartalmaz szóközt vagy speciális karaktereket; Ez azért szükséges, mivel a név lesz a az egységes erőforrás-azonosító (URI).</span><span class="sxs-lookup"><span data-stu-id="2b66f-147">Change the artifact name in the **Name** box so that it doesn't contain whitespace or special characters; this is necessary since the name will be used in the Uniform Resource Identifier (URI).</span></span>
   <span data-ttu-id="2b66f-148">c.</span><span class="sxs-lookup"><span data-stu-id="2b66f-148">c.</span></span> <span data-ttu-id="2b66f-149">Módosítsa a **típus** való **webes alkalmazás: archív**.</span><span class="sxs-lookup"><span data-stu-id="2b66f-149">Change the **Type** to **Web Application: Archive**.</span></span>
   <span data-ttu-id="2b66f-150">d.</span><span class="sxs-lookup"><span data-stu-id="2b66f-150">d.</span></span> <span data-ttu-id="2b66f-151">Kattintson a **OK** a projekt struktúra párbeszédpanel bezárásához.</span><span class="sxs-lookup"><span data-stu-id="2b66f-151">Click **OK** to close the Project Structure dialog box.</span></span>

    ![Nyissa meg a modul beállításai][05b]

<span data-ttu-id="2b66f-153">A modul beállítások konfigurálásakor az alábbi lépéseket követve teheti közzé az alkalmazás az Azure-bA:</span><span class="sxs-lookup"><span data-stu-id="2b66f-153">When you have configured your module settings, you can publish your application to Azure by using the following steps:</span></span>

1. <span data-ttu-id="2b66f-154">Az intellij-t a Project Explorer, kattintson a jobb gombbal a **Java-webalkalmazás-alkalmazás-a-Azure** projekt.</span><span class="sxs-lookup"><span data-stu-id="2b66f-154">In IntelliJ's Project Explorer, right-click the **Java-Web-App-On-Azure** project.</span></span> <span data-ttu-id="2b66f-155">Ha a helyi menü megjelenik, válassza ki a **Azure**, és kattintson a **Azure Web Apps közzététel...**</span><span class="sxs-lookup"><span data-stu-id="2b66f-155">When the context menu appears, select **Azure**, and then click **Publish as Azure Web App...**</span></span>
   
    ![Azure közzététel helyi menü][06]
2. <span data-ttu-id="2b66f-157">Ha már nem regisztrált az Azure az intellij-t, kérni fogja az Azure-fiókjával bejelentkezni.</span><span class="sxs-lookup"><span data-stu-id="2b66f-157">If you have not already signed into Azure from IntelliJ, you will be prompted to sign into your Azure account.</span></span> <span data-ttu-id="2b66f-158">(Még több Azure-fiókra, egyes az utasításokat a bejelentkezési folyamat során előfordulhat, hogy megjelenik-e egynél többször, akkor is, ha úgy tűnik, mintha azonos.</span><span class="sxs-lookup"><span data-stu-id="2b66f-158">(If you have multiple Azure accounts, some of the prompts during the sign in process may be shown more than once, even if they appear to be the same.</span></span> <span data-ttu-id="2b66f-159">Ha ez történik, továbbra is kövesse az utasításokat a bejelentkezéskor.)</span><span class="sxs-lookup"><span data-stu-id="2b66f-159">When this happens, continue to follow the sign in instructions.)</span></span>
   
    ![Az Azure bejelentkezési párbeszédpanel][07]
3. <span data-ttu-id="2b66f-161">Miután sikeresen bejelentkezett az Azure-fiókjával, a a **előfizetések kezelése oldalt** párbeszédpanelen jelenik meg a hitelesítő adatok társított előfizetések listáját.</span><span class="sxs-lookup"><span data-stu-id="2b66f-161">After you have successfully signed into your Azure account, the **Manage Subscriptions** dialog box will display a list of subscriptions that are associated with your credentials.</span></span> <span data-ttu-id="2b66f-162">(Ha nem szerepel a listában több előfizetéssel, és csak egy adott részével azokat használni kívánt, előfordulhat, hogy opcionálisan, törölje a jelet az előfizetések nem kívánja használni.) Miután kiválasztotta az előfizetések, kattintson a **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="2b66f-162">(If there are multiple subscriptions listed and you want to work with only a specific subset of them, you may optionally uncheck the subscriptions you don't want to use.) When you have selected your subscriptions, click **Close**.</span></span>
   
    ![Előfizetések kezelése][08]
4. <span data-ttu-id="2b66f-164">Ha a **központi telepítése az Azure Web App tárolóhoz** párbeszédpanel jelenik meg, jeleníti meg, hogy a korábban létrehozott webalkalmazás tárolókkal; Ha nem hozott létre tárolót, a lista üres lesz.</span><span class="sxs-lookup"><span data-stu-id="2b66f-164">When the **Deploy to Azure Web App Container** dialog box appears, it will display any Web App containers that you have previously created; if you have not created any containers, the list will be empty.</span></span>
   
    ![Alkalmazás tárolók][09]
5. <span data-ttu-id="2b66f-166">Ha nem hozott létre egy Azure webes alkalmazás tároló előtt, vagy ha azt szeretné, az alkalmazás egy új tároló közzétételére, kövesse az alábbi lépéseket.</span><span class="sxs-lookup"><span data-stu-id="2b66f-166">If you have not created an Azure Web App Container before, or if you would like to publish your application to a new container, use the following steps.</span></span> <span data-ttu-id="2b66f-167">Ellenkező esetben válassza ki a meglévő Web App tároló, és folytassa a 6. lépés alatt.</span><span class="sxs-lookup"><span data-stu-id="2b66f-167">Otherwise, select an existing Web App Container and skip to step 6 below.</span></span>
   
   1. <span data-ttu-id="2b66f-168">Kattintson a**+**</span><span class="sxs-lookup"><span data-stu-id="2b66f-168">Click **+**</span></span>
      
       ![Alkalmazás-tároló hozzáadása][10]
   2. <span data-ttu-id="2b66f-170">A **új webes alkalmazás tároló** párbeszédpanel jelenik meg, amely az alábbi lépéseket követve fog történni.</span><span class="sxs-lookup"><span data-stu-id="2b66f-170">The **New Web App Container** dialog box will be displayed, which will be used for the next several steps.</span></span>
      
       ![Új alkalmazás-tároló][11a]
   3. <span data-ttu-id="2b66f-172">Adjon meg egy **DNS-címke** a webes alkalmazás tároló; ez alkotó a levél DNS-címke a gazdagépre URL-címének a webalkalmazás az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="2b66f-172">Enter a **DNS Label** for your Web App Container; this will form the leaf DNS label of the host URL for your web application in Azure.</span></span> <span data-ttu-id="2b66f-173">Vegye figyelembe, hogy a név kell elérhetőnek lennie Azure Web Apps követelmények elnevezési felelnek meg.</span><span class="sxs-lookup"><span data-stu-id="2b66f-173">Note that the name must be available and conform to Azure Web App naming requirements.</span></span>
   4. <span data-ttu-id="2b66f-174">Az a **webes tároló** legördülő menüben válassza ki a megfelelő szoftver az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="2b66f-174">In the **Web Container** drop-down menu, select the appropriate software for your application.</span></span>
      
       <span data-ttu-id="2b66f-175">Jelenleg Tomcat 8 Tomcat 7 vagy Jetty 9 közül választhat.</span><span class="sxs-lookup"><span data-stu-id="2b66f-175">Currently, you can choose from Tomcat 8, Tomcat 7 or Jetty 9.</span></span> <span data-ttu-id="2b66f-176">A kiválasztott szoftverfrissítés legutóbbi terjesztési Azure biztosítja, és akkor fog futni, a legutóbbi terjesztésipont JDK 8 Oracle által létrehozott és az Azure által biztosított.</span><span class="sxs-lookup"><span data-stu-id="2b66f-176">A recent distribution of the selected software will be provided by Azure, and it will run on a recent distribution of JDK 8 created by Oracle and provided by Azure.</span></span>
   5. <span data-ttu-id="2b66f-177">Az a **előfizetés** legördülő menüben válassza ki az ehhez a központi telepítéshez használni kívánt előfizetést.</span><span class="sxs-lookup"><span data-stu-id="2b66f-177">In the **Subscription** drop-down menu, select the subscription you want to use for this deployment.</span></span>
   6. <span data-ttu-id="2b66f-178">Az a **erőforráscsoport** legördülő menüben válasszon, amelyhez a webalkalmazás hozzárendelni kívánt erőforráscsoportot.</span><span class="sxs-lookup"><span data-stu-id="2b66f-178">In the **Resource Group** drop-down menu, select the Resource Group with which you want to associate your Web App.</span></span> <span data-ttu-id="2b66f-179">(Az azure erőforráscsoportok lehetővé teszik kapcsolódó erőforrások egy csoportba, így például, hogy törölheti együtt.)</span><span class="sxs-lookup"><span data-stu-id="2b66f-179">(Azure Resource Groups allow you to group related resources together so that, for example, they can be deleted together.)</span></span>
      
       <span data-ttu-id="2b66f-180">Válasszon ki egy meglévő erőforráscsoportot (ha vannak ilyenek) és a skip. lépés: az alábbi g, vagy hozzon létre egy új erőforráscsoportot a következő lépések segítségével:</span><span class="sxs-lookup"><span data-stu-id="2b66f-180">You can select an existing Resource Group (if you have any) and skip to step g below, or use the following steps to create a new Resource Group:</span></span>
      
      * <span data-ttu-id="2b66f-181">Válassza ki  **&lt; &lt; hozzon létre új erőforráscsoportot &gt; &gt;**  a a **erőforráscsoport** legördülő menü.</span><span class="sxs-lookup"><span data-stu-id="2b66f-181">Select **&lt;&lt; Create new Resource Group &gt;&gt;** in the **Resource Group** drop-down menu.</span></span>
      * <span data-ttu-id="2b66f-182">A **új erőforráscsoport** párbeszédpanel fog megjelenni:</span><span class="sxs-lookup"><span data-stu-id="2b66f-182">The **New Resource Group** dialog box will be displayed:</span></span>
        
          ![Új erőforráscsoport][12]
      * <span data-ttu-id="2b66f-184">Az a a **neve** szövegmező, adja meg az új erőforráscsoport nevét.</span><span class="sxs-lookup"><span data-stu-id="2b66f-184">In the the **Name** textbox, specify a name for your new Resource Group.</span></span>
      * <span data-ttu-id="2b66f-185">Az a a **régió** legördülő menüben válassza a megfelelő Azure adatközpont-erőforrásnak a helyét.</span><span class="sxs-lookup"><span data-stu-id="2b66f-185">In the the **Region** drop-down menu, select the appropriate Azure data center location for your Resource Group.</span></span>
      * <span data-ttu-id="2b66f-186">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="2b66f-186">Click **OK**.</span></span>
   7. <span data-ttu-id="2b66f-187">A **App Service-csomag** legördülő menü mutatja az erőforráscsoport kiválasztott társított app service-csomagokról.</span><span class="sxs-lookup"><span data-stu-id="2b66f-187">The **App Service Plan** drop-down menu lists the app service plans that are associated with the Resource Group that you selected.</span></span> <span data-ttu-id="2b66f-188">(A következő információkat, például a Web App alkalmazásban az árképzési szint és a számítási példányméretének helyét egy App Service-csomag határozza meg.</span><span class="sxs-lookup"><span data-stu-id="2b66f-188">(An App Service Plan specifies information such as the location of your Web App, the pricing tier and the compute instance size.</span></span> <span data-ttu-id="2b66f-189">Egy egyetlen App Service-csomag használható több Web Apps, ezért azt egy adott webalkalmazás telepítésből külön megmarad.)</span><span class="sxs-lookup"><span data-stu-id="2b66f-189">A single App Service Plan can be used for multiple Web Apps, which is why it is maintained separately from a specific Web App deployment.)</span></span>
      
       <span data-ttu-id="2b66f-190">Válasszon ki egy meglévő App Service-csomag (ha vannak ilyenek) és skip h az alábbi lépések, vagy hozzon létre egy új App Service-csomag a következő lépések segítségével:</span><span class="sxs-lookup"><span data-stu-id="2b66f-190">You can select an existing App Service Plan (if you have any) and skip to step h below, or use the following steps to create a new App Service Plan:</span></span>
      
      * <span data-ttu-id="2b66f-191">Válassza ki  **&lt; &lt; új App Service-csomag létrehozása &gt; &gt;**  a a **App Service-csomag** legördülő menü.</span><span class="sxs-lookup"><span data-stu-id="2b66f-191">Select **&lt;&lt; Create new App Service Plan &gt;&gt;** in the **App Service Plan** drop-down menu.</span></span>
      * <span data-ttu-id="2b66f-192">A **új App Service-csomag** párbeszédpanel fog megjelenni:</span><span class="sxs-lookup"><span data-stu-id="2b66f-192">The **New App Service Plan** dialog box will be displayed:</span></span>
        
          ![Új App Service-csomag][13]
      * <span data-ttu-id="2b66f-194">Az a a **neve** szövegmező, adjon meg egy nevet az új App Service-csomag.</span><span class="sxs-lookup"><span data-stu-id="2b66f-194">In the the **Name** textbox, specify a name for your new App Service Plan.</span></span>
      * <span data-ttu-id="2b66f-195">Az a a **hely** legördülő menüben válassza a megfelelő Azure adatközpont helye a terv.</span><span class="sxs-lookup"><span data-stu-id="2b66f-195">In the the **Location** drop-down menu, select the appropriate Azure data center location for the plan.</span></span>
      * <span data-ttu-id="2b66f-196">Az a a **Tarifacsomagot** legördülő menüben válassza ki a megfelelő árképzési séma.</span><span class="sxs-lookup"><span data-stu-id="2b66f-196">In the the **Pricing Tier** drop-down menu, select the appropriate pricing for the plan.</span></span> <span data-ttu-id="2b66f-197">Tesztelési célokra választhat **szabad**.</span><span class="sxs-lookup"><span data-stu-id="2b66f-197">For testing purposes you can choose **Free**.</span></span>
      * <span data-ttu-id="2b66f-198">Az a a **Példányméretének** legördülő menüben válassza a terv méretezés a megfelelő példányt.</span><span class="sxs-lookup"><span data-stu-id="2b66f-198">In the the **Instance Size** drop-down menu, select the appropriate instance size for the plan.</span></span> <span data-ttu-id="2b66f-199">Tesztelési célokra választhat **kis**.</span><span class="sxs-lookup"><span data-stu-id="2b66f-199">For testing purposes you can choose **Small**.</span></span>
      * <span data-ttu-id="2b66f-200">Kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="2b66f-200">Click **OK**.</span></span>
   8. <span data-ttu-id="2b66f-201">(Választható) Alapértelmezés szerint Java 8 legutóbbi eloszlásáról automatikusan telepíti a JVM-et, az Azure-ban a webes alkalmazás tároló.</span><span class="sxs-lookup"><span data-stu-id="2b66f-201">(Optional) By default, a recent distribution of Java 8 will be automatically deployed as your JVM by Azure to your web app container.</span></span> <span data-ttu-id="2b66f-202">Választhatja azonban egy másik verzió és elosztását illetően a JVM-et.</span><span class="sxs-lookup"><span data-stu-id="2b66f-202">However, you can select a different version and distribution of the JVM.</span></span> <span data-ttu-id="2b66f-203">Ehhez a következőket kell tennie:</span><span class="sxs-lookup"><span data-stu-id="2b66f-203">To do so, use the following steps:</span></span>
      
      * <span data-ttu-id="2b66f-204">Kattintson a **JDK** lapra a **új webes alkalmazás tároló** párbeszédpanel megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="2b66f-204">Click the **JDK** tab in the **New Web App Container** dialog box.</span></span>
      * <span data-ttu-id="2b66f-205">Az alábbi lehetőségek közül választhat:</span><span class="sxs-lookup"><span data-stu-id="2b66f-205">You can choose from one of the following options:</span></span>
        
        * <span data-ttu-id="2b66f-206">Az alapértelmezett JDK, amely az Azure által kínált telepítése</span><span class="sxs-lookup"><span data-stu-id="2b66f-206">Deploy the default JDK which is offered by Azure</span></span>
        * <span data-ttu-id="2b66f-207">3. fél JDK Azure rendelkezésre álló további JDKs legördülő listájából telepítése</span><span class="sxs-lookup"><span data-stu-id="2b66f-207">Deploy a 3rd party JDK from a drop-down list of additional JDKs which are available on Azure</span></span>
        * <span data-ttu-id="2b66f-208">Központi telepítése egy egyéni JDK, és be kell csomagolni, és vagy egy ZIP-fájl nyilvánosan elérhető vagy az Azure storage-fiók</span><span class="sxs-lookup"><span data-stu-id="2b66f-208">Deploy a custom JDK, which must be packaged as a ZIP file and either publicly available or in your Azure storage account</span></span>
        
        ![Új alkalmazás tároló JDK lapon][11b]
   9. <span data-ttu-id="2b66f-210">A fenti lépés befejeződött, a webes alkalmazás új tároló párbeszédpanelen a következő ábra kell hasonlítania:</span><span class="sxs-lookup"><span data-stu-id="2b66f-210">Once you have completed all of the above steps, the New Web App Container dialog box should resemble the following illustration:</span></span>
      
       ![Új alkalmazás-tároló][14]
   10. <span data-ttu-id="2b66f-212">Kattintson a **OK** az új webalkalmazáshoz tároló létrehozásának befejezéséhez.</span><span class="sxs-lookup"><span data-stu-id="2b66f-212">Click **OK** to complete the creation of your new Web App container.</span></span>
       
        <span data-ttu-id="2b66f-213">Várjon néhány másodpercet, frissíteni kell a webalkalmazás-tárolók listáját, és az újonnan létrehozott webes alkalmazás tároló most már ki a listában.</span><span class="sxs-lookup"><span data-stu-id="2b66f-213">Wait a few seconds for the list of the Web App containers to be refreshed, and your newly-created web app container should now be selected in the list.</span></span>
6. <span data-ttu-id="2b66f-214">Most már készen áll a webalkalmazás Azure; a kezdeti telepítés befejezéséhez Kattintson a **OK** a kijelölt webalkalmazás tárolóhoz a Java-alkalmazás központi telepítése.</span><span class="sxs-lookup"><span data-stu-id="2b66f-214">You are now ready to complete the initial deployment of your Web App to Azure; click **OK** to deploy your Java application to the selected Web App container.</span></span> <span data-ttu-id="2b66f-215">Alapértelmezés szerint az alkalmazás telepítve, a kiszolgáló alkönyvtára lehet.</span><span class="sxs-lookup"><span data-stu-id="2b66f-215">By default, your application will be deployed as a subdirectory of the application server.</span></span> <span data-ttu-id="2b66f-216">Ha azt szeretné, hogy a legfelső szintű alkalmazás telepíthető központilag, ellenőrizze a **legfelső szintű telepítés** való kattintás előtt jelölőnégyzet **OK**.</span><span class="sxs-lookup"><span data-stu-id="2b66f-216">If you want it to be deployed as the root application, check the **Deploy to root** checkbox before clicking **OK**.</span></span>
   
    ![Telepítse az Azure][15]
7. <span data-ttu-id="2b66f-218">Ezt követően kell megjelennie a **Azure tevékenységnapló** nézet, ami a webalkalmazás központi telepítési állapotát jelzi.</span><span class="sxs-lookup"><span data-stu-id="2b66f-218">Next, you should see the **Azure Activity Log** view, which will indicate the deployment status of your Web App.</span></span>
   
    ![Folyamatjelző][16]
   
    <span data-ttu-id="2b66f-220">A webalkalmazás az Azure-bA üzembe helyezésének folyamata gyorsabban csak néhány másodpercig.</span><span class="sxs-lookup"><span data-stu-id="2b66f-220">The process of deploying your Web App to Azure should take only a few seconds to complete.</span></span> <span data-ttu-id="2b66f-221">Ha alkalmazás készen áll, megjelenik egy hivatkozás nevű **közzétett** a a **állapot** oszlop.</span><span class="sxs-lookup"><span data-stu-id="2b66f-221">When your application ready, you will see a link named **Published** in the **Status** column.</span></span> <span data-ttu-id="2b66f-222">A hivatkozásra kattintva, időt vesz igénybe, a telepített webalkalmazás kezdőlap, vagy tallózással keresse meg a webalkalmazás a következő szakaszban a lépéseket is használhat.</span><span class="sxs-lookup"><span data-stu-id="2b66f-222">When you click the link, it will take you to your deployed Web App's home page, or you can use the steps in the following section to browse to your web app.</span></span>

## <a name="browsing-to-your-web-app-on-azure"></a><span data-ttu-id="2b66f-223">A webalkalmazás Azure tallózással</span><span class="sxs-lookup"><span data-stu-id="2b66f-223">Browsing to your Web App on Azure</span></span>
<span data-ttu-id="2b66f-224">Tallózással keresse meg a webalkalmazás az Azure-on, használhatja a **Azure Explorer** nézet.</span><span class="sxs-lookup"><span data-stu-id="2b66f-224">To browse to your Web App on Azure, you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="2b66f-225">Ha a **Azure Explorer** nézet még nincs megnyitva, majd kattintva megnyithatja azt **nézet** menü az intellij-t, majd kattintson a **eszköz Windows**, és kattintson a  **Szolgáltatás Explorer**.</span><span class="sxs-lookup"><span data-stu-id="2b66f-225">If the **Azure Explorer** view is not already open, you can open it by clicking then **View** menu in IntelliJ, then click **Tool Windows**, and then click **Service Explorer**.</span></span> <span data-ttu-id="2b66f-226">Ha korábban nem jelentkezett be, akkor arra fogja kérni teszi.</span><span class="sxs-lookup"><span data-stu-id="2b66f-226">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="2b66f-227">Ha a **Azure Explorer** nézet jelenik meg, használja a lépések végrehajtásával keresse meg a webalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="2b66f-227">When the **Azure Explorer** view is displayed, use follow these steps to browse to your Web App:</span></span> 

1. <span data-ttu-id="2b66f-228">Bontsa ki a **Azure** csomópont.</span><span class="sxs-lookup"><span data-stu-id="2b66f-228">Expand the **Azure** node.</span></span>
2. <span data-ttu-id="2b66f-229">Bontsa ki a **webalkalmazások** csomópont.</span><span class="sxs-lookup"><span data-stu-id="2b66f-229">Expand the **Web Apps** node.</span></span> 
3. <span data-ttu-id="2b66f-230">Kattintson a jobb gombbal a kívánt webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="2b66f-230">Right-click the desired Web App.</span></span>
4. <span data-ttu-id="2b66f-231">Amikor a helyi menü megjelenik, kattintson a **nyissa meg böngészőben**.</span><span class="sxs-lookup"><span data-stu-id="2b66f-231">When the context menu appears, click **Open in Browser**.</span></span>
   
    ![Keresse meg a webes alkalmazás][17]

## <a name="updating-your-web-app"></a><span data-ttu-id="2b66f-233">Webalkalmazás frissítése</span><span class="sxs-lookup"><span data-stu-id="2b66f-233">Updating your web app</span></span>
<span data-ttu-id="2b66f-234">Frissítése egy meglévő Azure-webalkalmazás fut egy gyors és egyszerű eljárás, és frissítési lehetőségei vannak:</span><span class="sxs-lookup"><span data-stu-id="2b66f-234">Updating an existing running Azure Web App is a quick and easy process, and you have two options for updating:</span></span>

* <span data-ttu-id="2b66f-235">Java-webalkalmazás telepítése frissítheti.</span><span class="sxs-lookup"><span data-stu-id="2b66f-235">You can update the deployment of an existing Java Web App.</span></span>
* <span data-ttu-id="2b66f-236">Egy webes alkalmazás tárolóhoz további Java-alkalmazást is közzétehet.</span><span class="sxs-lookup"><span data-stu-id="2b66f-236">You can publish an additional Java application to the same Web App Container.</span></span>

<span data-ttu-id="2b66f-237">Mindkét esetben a folyamat megegyezik, és csupán néhány másodpercet vesz igénybe:</span><span class="sxs-lookup"><span data-stu-id="2b66f-237">In either case, the process is identical and takes only a few seconds:</span></span>

1. <span data-ttu-id="2b66f-238">Az IntelliJ project explorer kattintson a jobb gombbal a Java-alkalmazást szeretne frissíteni, vagy meglévő Web App tároló hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="2b66f-238">In the IntelliJ project explorer, right-click the Java application you want to update or add to an existing Web App Container.</span></span>
2. <span data-ttu-id="2b66f-239">Ha a helyi menü megjelenik, válassza ki a **Azure** , majd **Azure Web Apps közzététel...**</span><span class="sxs-lookup"><span data-stu-id="2b66f-239">When the context menu appears, select **Azure** and then **Publish as Azure Web App...**</span></span>
3. <span data-ttu-id="2b66f-240">Már már bejelentkezett korábban, mivel látni fogja a meglévő Web App tárolók listája.</span><span class="sxs-lookup"><span data-stu-id="2b66f-240">Since you have already logged in previously, you will see a list of your existing Web App containers.</span></span> <span data-ttu-id="2b66f-241">Jelölje ki a közzétenni vagy újra közzé a Java-alkalmazást, és kattintson a kívánt **OK**.</span><span class="sxs-lookup"><span data-stu-id="2b66f-241">Select the one you want to publish or re-publish your Java application to and click **OK**.</span></span>

<span data-ttu-id="2b66f-242">Néhány másodperc múlva, a **Azure tevékenységnapló** nézetben megjelenik, a frissített telepítési **közzétett** és nem fogja tudni ellenőrizni a frissített alkalmazást egy webböngészőben.</span><span class="sxs-lookup"><span data-stu-id="2b66f-242">A few seconds later, the **Azure Activity Log** view will show your updated deployment as **Published** and you will be able to verify your updated application in a web browser.</span></span>

## <a name="starting-stopping-or-restarting-an-existing-web-app"></a><span data-ttu-id="2b66f-243">Indítása, leállítása vagy egy már meglévő webalkalmazás újraindítása</span><span class="sxs-lookup"><span data-stu-id="2b66f-243">Starting, stopping, or restarting an existing web app</span></span>
<span data-ttu-id="2b66f-244">Indítsa el, vagy állítsa le a meglévő Azure Web Apps tároló, (beleértve az összes telepített Java-alkalmazások azt), használja a **Azure Explorer** nézet.</span><span class="sxs-lookup"><span data-stu-id="2b66f-244">To start or stop an existing Azure Web App container, (including all the deployed Java applications in it), you can use the **Azure Explorer** view.</span></span>

<span data-ttu-id="2b66f-245">Ha a **Azure Explorer** nézet még nincs megnyitva, majd kattintva megnyithatja azt **nézet** menü az intellij-t, majd kattintson a **eszköz Windows**, és kattintson a  **Szolgáltatás Explorer**.</span><span class="sxs-lookup"><span data-stu-id="2b66f-245">If the **Azure Explorer** view is not already open, you can open it by clicking then **View** menu in IntelliJ, then click **Tool Windows**, and then click **Service Explorer**.</span></span> <span data-ttu-id="2b66f-246">Ha korábban nem jelentkezett be, akkor arra fogja kérni teszi.</span><span class="sxs-lookup"><span data-stu-id="2b66f-246">If you have not previously logged in, it will prompt you to do so.</span></span>

<span data-ttu-id="2b66f-247">Ha a **Azure Explorer** nézet jelenik meg, használjon kövesse az alábbi lépéseket indítása vagy leállítása a webalkalmazás:</span><span class="sxs-lookup"><span data-stu-id="2b66f-247">When the **Azure Explorer** view is displayed, use follow these steps to start or stop your Web App:</span></span> 

1. <span data-ttu-id="2b66f-248">Bontsa ki a **Azure** csomópont.</span><span class="sxs-lookup"><span data-stu-id="2b66f-248">Expand the **Azure** node.</span></span>
2. <span data-ttu-id="2b66f-249">Bontsa ki a **webalkalmazások** csomópont.</span><span class="sxs-lookup"><span data-stu-id="2b66f-249">Expand the **Web Apps** node.</span></span> 
3. <span data-ttu-id="2b66f-250">Kattintson a jobb gombbal a kívánt webalkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="2b66f-250">Right-click the desired Web App.</span></span>
4. <span data-ttu-id="2b66f-251">Amikor a helyi menü megjelenik, kattintson a **Start**, **leállítása**, vagy **indítsa újra a**.</span><span class="sxs-lookup"><span data-stu-id="2b66f-251">When the context menu appears, click **Start**, **Stop**, or **Restart**.</span></span> <span data-ttu-id="2b66f-252">Vegye figyelembe, hogy a menüjéből választások környezet használatát, így csak a futó webalkalmazás leállítása vagy egy webalkalmazást, ami jelenleg nem fut. Indítsa el.</span><span class="sxs-lookup"><span data-stu-id="2b66f-252">Note that the menu choices are context-aware, so you can only stop a running web app or start a web app which is not currently running.</span></span>
   
    ![A webalkalmazás leállítása][18]

## <a name="next-steps"></a><span data-ttu-id="2b66f-254">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="2b66f-254">Next Steps</span></span>
<span data-ttu-id="2b66f-255">A Java IDE környezetekhez készült Azure-eszközkészlettel kapcsolatos további információkért lásd az alábbi hivatkozásokat:</span><span class="sxs-lookup"><span data-stu-id="2b66f-255">For more information about the Azure Toolkits for Java IDEs, see the following links:</span></span>

* <span data-ttu-id="2b66f-256">[Eclipse Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="2b66f-256">[Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="2b66f-257">[Az Eclipse-hez készült Azure-eszközkészlet telepítése]</span><span class="sxs-lookup"><span data-stu-id="2b66f-257">[Installing the Azure Toolkit for Eclipse]</span></span>
  * <span data-ttu-id="2b66f-258">[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]</span><span class="sxs-lookup"><span data-stu-id="2b66f-258">[Create a Hello World Web App for Azure in Eclipse]</span></span>
  * <span data-ttu-id="2b66f-259">[Az Eclipse-hez készült Azure-eszközkészlet újdonságai]</span><span class="sxs-lookup"><span data-stu-id="2b66f-259">[What's New in the Azure Toolkit for Eclipse]</span></span>
* <span data-ttu-id="2b66f-260">[IntelliJ Azure eszköztára]</span><span class="sxs-lookup"><span data-stu-id="2b66f-260">[Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="2b66f-261">[az intellij-t az Azure eszközkészlet telepítésével]</span><span class="sxs-lookup"><span data-stu-id="2b66f-261">[Installing the Azure Toolkit for IntelliJ]</span></span>
  * <span data-ttu-id="2b66f-262">*Hello World webalkalmazás létrehozása az Azure-hoz az intellij-t (Ez a cikk)*</span><span class="sxs-lookup"><span data-stu-id="2b66f-262">*Create a Hello World Web App for Azure in IntelliJ (This Article)*</span></span>
  * <span data-ttu-id="2b66f-263">[Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]</span><span class="sxs-lookup"><span data-stu-id="2b66f-263">[What's New in the Azure Toolkit for IntelliJ]</span></span>

<a name="see-also"></a>

## <a name="see-also"></a><span data-ttu-id="2b66f-264">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="2b66f-264">See Also</span></span>
<span data-ttu-id="2b66f-265">Az Azure Javával való használatáról további információ: [Azure Java fejlesztői központ].</span><span class="sxs-lookup"><span data-stu-id="2b66f-265">For more information about using Azure with Java, see the [Azure Java Developer Center].</span></span>

<span data-ttu-id="2b66f-266">Azure Web Apps létrehozásával kapcsolatos további információkért lásd: a [webes alkalmazások – áttekintés].</span><span class="sxs-lookup"><span data-stu-id="2b66f-266">For additional information about creating Azure Web Apps, see the [Web Apps Overview].</span></span>

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[Eclipse Azure eszköztára]: ../azure-toolkit-for-eclipse.md
[IntelliJ Azure eszköztára]: ../azure-toolkit-for-intellij.md
[Hello World webalkalmazás létrehozása az Azure-hoz az eclipse-ben]: ./app-service-web-eclipse-create-hello-world-web-app.md
[Create a Hello World Web App for Azure in IntelliJ]: ./app-service-web-intellij-create-hello-world-web-app.md
[Az Eclipse-hez készült Azure-eszközkészlet telepítése]: ../azure-toolkit-for-eclipse-installation.md
[az intellij-t az Azure eszközkészlet telepítésével]: ../azure-toolkit-for-intellij-installation.md
[Az Eclipse-hez készült Azure-eszközkészlet újdonságai]: ../azure-toolkit-for-eclipse-whats-new.md
[Az IntelliJ-hez készült Azure-eszközkészlet újdonságai]: ../azure-toolkit-for-intellij-whats-new.md

[Azure Java fejlesztői központ]: https://azure.microsoft.com/develop/java/
[webes alkalmazások – áttekintés]: ./app-service-web-overview.md
[Apache Tomcat]: http://tomcat.apache.org/
[Jetty]: http://www.eclipse.org/jetty/

<!-- IMG List -->

[01]: ./media/app-service-web-intellij-create-hello-world-web-app/01-Web-Page.png
[02]: ./media/app-service-web-intellij-create-hello-world-web-app/02-File-New-Project.png
[03a]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-Dialog.png
[03b]: ./media/app-service-web-intellij-create-hello-world-web-app/03-New-Project-SDK-Dialog.png
[04]: ./media/app-service-web-intellij-create-hello-world-web-app/04-New-Project-Dialog.png
[05a]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Module-Settings.png
[05b]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Project-Structure-Dialog.png
[05c]: ./media/app-service-web-intellij-create-hello-world-web-app/05-Open-Index-Page.png
[06]: ./media/app-service-web-intellij-create-hello-world-web-app/06-Azure-Publish-Context-Menu.png
[07]: ./media/app-service-web-intellij-create-hello-world-web-app/07-Azure-Log-In-Dialog.png
[08]: ./media/app-service-web-intellij-create-hello-world-web-app/08-Manage-Subscriptions.png
[09]: ./media/app-service-web-intellij-create-hello-world-web-app/09-App-Containers.png
[10]: ./media/app-service-web-intellij-create-hello-world-web-app/10-Add-App-Container.png
[11a]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container.png
[11b]: ./media/app-service-web-intellij-create-hello-world-web-app/11-New-App-Container-JDK-Tab.png
[12]: ./media/app-service-web-intellij-create-hello-world-web-app/12-New-Resource-Group.png
[13]: ./media/app-service-web-intellij-create-hello-world-web-app/13-New-App-Service-Plan.png
[14]: ./media/app-service-web-intellij-create-hello-world-web-app/14-New-App-Container.png
[15]: ./media/app-service-web-intellij-create-hello-world-web-app/15-Deploy-To-Azure.png
[16]: ./media/app-service-web-intellij-create-hello-world-web-app/16-Progress-Indicator.png
[17]: ./media/app-service-web-intellij-create-hello-world-web-app/17-Browse-Web-App.png
[18]: ./media/app-service-web-intellij-create-hello-world-web-app/18-Stop-Web-App.png
