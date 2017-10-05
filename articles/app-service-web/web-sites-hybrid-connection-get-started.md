---
title: "Helyszíni erőforrások elérése hibrid kapcsolatok használatával az Azure App Service-ben"
description: "Hozzon létre egy webalkalmazást az Azure App Service-ben, és egy helyszíni erőforrás egy statikus TCP-portot használó közötti kapcsolat"
services: app-service
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: a46ed26b-df8e-4fc3-8e05-2d002a6ee508
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2016
ms.author: cephalin
ms.openlocfilehash: fbd22e6e285c5ddaef2a473671d4a06a97384b4a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="access-on-premises-resources-using-hybrid-connections-in-azure-app-service"></a><span data-ttu-id="b0171-103">Helyszíni erőforrások elérése hibrid kapcsolatok használatával az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="b0171-103">Access on-premises resources using hybrid connections in Azure App Service</span></span>
<span data-ttu-id="b0171-104">Kapcsolódás az Azure App Service-alkalmazások összes helyszíni erőforrás a statikus TCP-port, például az SQL Server, a MySQL, a HTTP webes API-k és a legtöbb egyéni webszolgáltatásokat használó.</span><span class="sxs-lookup"><span data-stu-id="b0171-104">You can connect an Azure App Service app to any on-premises resource that uses a static TCP port, such as SQL Server, MySQL, HTTP Web APIs, and most custom Web Services.</span></span> <span data-ttu-id="b0171-105">Ez a cikk bemutatja, hogyan hozzon létre egy hibrid kapcsolat App Service és a helyszíni SQL Server-adatbázis között.</span><span class="sxs-lookup"><span data-stu-id="b0171-105">This article shows you how to create a hybrid connection between App Service and an on-premises SQL Server database.</span></span>

> [!NOTE]
> <span data-ttu-id="b0171-106">A Web Apps része a hibrid kapcsolatok szolgáltatás csak a érhető el a [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="b0171-106">The Web Apps portion of the Hybrid Connections feature is available only in the [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="b0171-107">BizTalk szolgáltatások VPN-kapcsolat létrehozásához lásd: [hibrid kapcsolatok](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span><span class="sxs-lookup"><span data-stu-id="b0171-107">To create a connection in BizTalk Services, see [Hybrid Connections](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span></span> 
> 
> <span data-ttu-id="b0171-108">Ezt a tartalmat az Azure App Service Mobile Apps is vonatkozik.</span><span class="sxs-lookup"><span data-stu-id="b0171-108">This content also applies to Mobile Apps in Azure App Service.</span></span> 
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="b0171-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="b0171-109">Prerequisites</span></span>
* <span data-ttu-id="b0171-110">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="b0171-110">An Azure subscription.</span></span> <span data-ttu-id="b0171-111">Ingyenes előfizetésre, lásd: [Azure ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="b0171-111">For a free subscription, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
  
    <span data-ttu-id="b0171-112">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="b0171-112">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="b0171-113">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="b0171-113">No credit cards required; no commitments.</span></span>
* <span data-ttu-id="b0171-114">A helyszíni SQL Server vagy SQL Server Express adatbázis használata hibrid kapcsolaton keresztül, TCP/IP kell engedélyezni a statikus port.</span><span class="sxs-lookup"><span data-stu-id="b0171-114">To use an on-premises SQL Server or SQL Server Express database with a hybrid connection, TCP/IP needs to be enabled on a static port.</span></span> <span data-ttu-id="b0171-115">Az SQL Server alapértelmezett példánnyal használata ajánlott, mivel a 1433-as port statikus használ.</span><span class="sxs-lookup"><span data-stu-id="b0171-115">Using a default instance on SQL Server is recommended because it uses static port 1433.</span></span> <span data-ttu-id="b0171-116">Információ a telepítése és konfigurálása az SQL Server Express hibrid kapcsolatok való használathoz: [csatlakozás egy helyszíni SQL Server egy hibrid kapcsolatok használata az Azure webhelyén](http://go.microsoft.com/fwlink/?LinkID=397979).</span><span class="sxs-lookup"><span data-stu-id="b0171-116">For information on installing and configuring SQL Server Express for use with hybrid connections, see [Connect to an on-premises SQL Server from an Azure web site using Hybrid Connections](http://go.microsoft.com/fwlink/?LinkID=397979).</span></span>
* <span data-ttu-id="b0171-117">A számítógép, amelyre a cikk későbbi részében a helyszíni Hybrid Connection Manager ügynök leírt telepíti:</span><span class="sxs-lookup"><span data-stu-id="b0171-117">The computer on which you install the on-premises Hybrid Connection Manager agent described later in this article:</span></span>
  
  * <span data-ttu-id="b0171-118">Port: 5671 keresztül csatlakozni az Azure-bA képesnek kell lennie</span><span class="sxs-lookup"><span data-stu-id="b0171-118">Must be able to connect to Azure over port 5671</span></span>
  * <span data-ttu-id="b0171-119">Kell tudni érnie a *állomásnév*:*portszám* a helyi erőforrás.</span><span class="sxs-lookup"><span data-stu-id="b0171-119">Must be able to reach the *hostname*:*portnumber* of your on-premises resource.</span></span> 

> [!NOTE]
> <span data-ttu-id="b0171-120">Ebben a cikkben szereplő lépések azt feltételezik, hogy a számítógépről, amely üzemelteti a helyszíni hibrid kapcsolat ügynök böngészőt használ.</span><span class="sxs-lookup"><span data-stu-id="b0171-120">The steps in this article assume that you are using the browser from the computer that will host the on-premises hybrid connection agent.</span></span>
> 
> 

## <a name="create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="b0171-121">Webalkalmazás létrehozása az Azure portálon</span><span class="sxs-lookup"><span data-stu-id="b0171-121">Create a web app in the Azure Portal</span></span>
> [!NOTE]
> <span data-ttu-id="b0171-122">Ha már létrehozott egy webes alkalmazás vagy a Mobile Apps-háttéralkalmazás az Azure portálon, hogy a jelen oktatóanyag használni kívánt, ugorjon előre [egy hibrid kapcsolat és a BizTalk szolgáltatás létrehozása](#CreateHC) , és indítsa el onnan.</span><span class="sxs-lookup"><span data-stu-id="b0171-122">If you have already created a web app or Mobile App backend in the Azure Portal that you want to use for this tutorial, you can skip ahead to [Create a Hybrid Connection and a BizTalk Service](#CreateHC) and start from there.</span></span>
> 
> 

1. <span data-ttu-id="b0171-123">A bal felső sarkában található a [Azure Portal](https://portal.azure.com), kattintson a **új** > **Web + mobil** > **webalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="b0171-123">In the upper left corner of the [Azure Portal](https://portal.azure.com), click **New** > **Web + Mobile** > **Web App**.</span></span>
   
    ![Új webalkalmazás][NewWebsite]
2. <span data-ttu-id="b0171-125">Az a **webalkalmazás** panelen adjon meg egy URL-címet, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="b0171-125">On the **Web app** blade, provide a URL and click **Create**.</span></span> 
   
    ![Webhely neve][WebsiteCreationBlade]
3. <span data-ttu-id="b0171-127">Néhány másodpercen belül a web app jön létre, és a webalkalmazás panelen jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="b0171-127">After a few moments, the web app is created and its web app blade appears.</span></span> <span data-ttu-id="b0171-128">A panel egy függőleges görgethető irányítópultot, amely lehetővé teszi, hogy a hely kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="b0171-128">The blade is a vertically scrollable dashboard that lets you manage your site.</span></span>
   
    ![Futtató webhely][WebSiteRunningBlade]
4. <span data-ttu-id="b0171-130">Annak ellenőrzéséhez, hogy a hely élő, kattintson a **Tallózás** ikonra kattintva jelenítse meg az alapértelmezett lapon.</span><span class="sxs-lookup"><span data-stu-id="b0171-130">To verify the site is live, you can click the **Browse** icon to display the default page.</span></span>
   
    ![Kattintson a Tallózás gombra, hogy a webalkalmazás][Browse]
   
    ![Alapértelmezett weblap-alkalmazás][DefaultWebSitePage]

<span data-ttu-id="b0171-133">A következő létrehozhat egy hibrid kapcsolat és a BizTalk szolgáltatás a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="b0171-133">Next, you will create a hybrid connection and a BizTalk service for the web app.</span></span>

<a name="CreateHC"></a>

## <a name="create-a-hybrid-connection-and-a-biztalk-service"></a><span data-ttu-id="b0171-134">A hibrid kapcsolat és a BizTalk szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="b0171-134">Create a Hybrid Connection and a BizTalk Service</span></span>
1. <span data-ttu-id="b0171-135">A webalkalmazás panelen kattintson a **összes beállítás** > **hálózati** > **hibrid kapcsolati végpontok konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="b0171-135">In your web app blade click on **All settings** > **Networking** > **Configure your hybrid connection endpoints**.</span></span>
   
    ![Hibrid kapcsolatok][CreateHCHCIcon]
2. <span data-ttu-id="b0171-137">A hibrid kapcsolatok paneljén kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="b0171-137">On the Hybrid connections blade, click **Add**.</span></span>
   
    <!-- ![Add a hybrid connnection][CreateHCAddHC]
   -->
3. <span data-ttu-id="b0171-138">A **egy hibrid kapcsolat hozzáadása a** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="b0171-138">The **Add a hybrid connection** blade opens.</span></span>  <span data-ttu-id="b0171-139">Az első hibrid kapcsolat, mert a **új hibrid kapcsolat** lehetőség van megadva, és a **hibrid kapcsolat létrehozása** panel megnyitása meg.</span><span class="sxs-lookup"><span data-stu-id="b0171-139">Since this is your first hybrid connection, the **New hybrid connection** option is preselected, and the **Create hybrid connection** blade opens for you.</span></span>
   
    ![A hibrid kapcsolat létrehozása][TwinCreateHCBlades]
   
    <span data-ttu-id="b0171-141">Az a **létrehozás hibrid kapcsolat panel**:</span><span class="sxs-lookup"><span data-stu-id="b0171-141">On the **Create hybrid connection blade**:</span></span>
   
   * <span data-ttu-id="b0171-142">A **neve**, adja meg a kapcsolat nevét.</span><span class="sxs-lookup"><span data-stu-id="b0171-142">For **Name**, provide a name for the connection.</span></span>
   * <span data-ttu-id="b0171-143">A **állomásnév**, adja meg a helyi számítógépen, amelyen az erőforrás nevét.</span><span class="sxs-lookup"><span data-stu-id="b0171-143">For **Hostname**, enter the name of the on-premises computer that hosts your resource.</span></span>
   * <span data-ttu-id="b0171-144">A **Port**, adja meg a portszámot, hogy a helyi erőforrás (SQL Server alapértelmezett példány 1433) használja-e.</span><span class="sxs-lookup"><span data-stu-id="b0171-144">For **Port**, enter the port number that your on-premises resource uses (1433 for a SQL Server default instance).</span></span>
   * <span data-ttu-id="b0171-145">Kattintson a **Biz előadás szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="b0171-145">Click **Biz Talk Service**</span></span>
4. <span data-ttu-id="b0171-146">A **BizTalk szolgáltatás létrehozása** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="b0171-146">The **Create BizTalk Service** blade opens.</span></span> <span data-ttu-id="b0171-147">Adjon meg egy nevet a BizTalk szolgáltatás, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0171-147">Enter a name for the BizTalk service, and then click **OK**.</span></span>
   
    ![BizTalk szolgáltatás létrehozása][CreateHCCreateBTS]
   
    <span data-ttu-id="b0171-149">A **BizTalk szolgáltatás létrehozása** panel bezárása után, és a rendszer visszairányítja a **hibrid kapcsolat létrehozása** panelen.</span><span class="sxs-lookup"><span data-stu-id="b0171-149">The **Create BizTalk Service** blade closes and you are returned to the **Create hybrid connection** blade.</span></span>
5. <span data-ttu-id="b0171-150">A hibrid kapcsolat létrehozása panelen kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="b0171-150">On the Create hybrid connection blade, click **OK**.</span></span> 
   
    ![Kattintson az OK gombra][CreateBTScomplete]
6. <span data-ttu-id="b0171-152">A folyamat befejezése után, az értesítési területen a portálon figyelmeztet, hogy a kapcsolat sikeresen létrejött.</span><span class="sxs-lookup"><span data-stu-id="b0171-152">When the process completes, the notifications area in the Portal informs you that the connection has been successfully created.</span></span>
   
    <!--- TODO
   
    Everything fails at this step. I can't create a BizTalk service in the dogfood portal. I switch to the classic portal
    (full portal) and created the BizTalk service but it doesn't seem to let you connnect them - When you finish the
    Create hybrid conn step, you get the following error
    Failed to create hybrid connection RelecIoudHC. The 
    resource type could not be found in the namespace 
    'Microsoft.BizTaIkServices for api version 2014-06-01'.
   
    The error indicates it couldn't find the type, not the instance.
    ![Success notification][CreateHCSuccessNotification]
    -->
7. <span data-ttu-id="b0171-153">A webalkalmazás panelen a **hibrid kapcsolatok** ikon már helyesen jelenik meg, hogy 1 hibrid kapcsolat létrejött.</span><span class="sxs-lookup"><span data-stu-id="b0171-153">On the web app's blade, the **Hybrid connections** icon now shows that 1 hybrid connection has been created.</span></span>
   
    ![Egy hibrid kapcsolat létrehozása][CreateHCOneConnectionCreated]

<span data-ttu-id="b0171-155">Fontos része a hibrid kapcsolat felhőalapú infrastruktúra ezen a ponton befejeződött.</span><span class="sxs-lookup"><span data-stu-id="b0171-155">At this point, you have completed an important part of the cloud hybrid connection infrastructure.</span></span> <span data-ttu-id="b0171-156">Ezután létrehozhat egy helyszíni megfelelő adat.</span><span class="sxs-lookup"><span data-stu-id="b0171-156">Next, you will create a corresponding on-premises piece.</span></span>

<a name="InstallHCM"></a>

## <a name="install-the-on-premises-hybrid-connection-manager-to-complete-the-connection"></a><span data-ttu-id="b0171-157">A kapcsolat a helyszíni hibrid Csatlakozáskezelő telepítése</span><span class="sxs-lookup"><span data-stu-id="b0171-157">Install the on-premises Hybrid Connection Manager to complete the connection</span></span>
1. <span data-ttu-id="b0171-158">A webes alkalmazás paneljén kattintson **összes beállítás** > **hálózati** > **hibrid kapcsolati végpontok konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="b0171-158">On the web app's blade, click **All settings** > **Networking** > **Configure your hybrid connection endpoints**.</span></span> 
   
    ![Hibrid kapcsolatok ikonja][HCIcon]
2. <span data-ttu-id="b0171-160">Az a **hibrid kapcsolatok** panelen a **állapot** a legutóbb hozzáadott végpont jeleníti meg az oszlop **nincs csatlakoztatva**.</span><span class="sxs-lookup"><span data-stu-id="b0171-160">On the **Hybrid connections** blade, the **Status** column for the recently added endpoint shows **Not connected**.</span></span> <span data-ttu-id="b0171-161">Kattintson a kapcsolat konfigurálását.</span><span class="sxs-lookup"><span data-stu-id="b0171-161">Click the connection to configure it.</span></span>
   
    ![Nincs csatlakoztatva][NotConnected]
   
    <span data-ttu-id="b0171-163">A hibrid kapcsolat panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="b0171-163">The Hybrid connection blade opens.</span></span>
   
    ![NotConnectedBlade][NotConnectedBlade]
3. <span data-ttu-id="b0171-165">Kattintson a panel **figyelő telepítő**.</span><span class="sxs-lookup"><span data-stu-id="b0171-165">On the blade, click **Listener Setup**.</span></span>
   
    ![Kattintson a figyelő beállítás][ClickListenerSetup]
4. <span data-ttu-id="b0171-167">A **hibrid kapcsolat tulajdonságai** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="b0171-167">The **Hybrid connection properties** blade opens.</span></span> <span data-ttu-id="b0171-168">A **helyszíni Hybrid Connection Manager**, válassza a **telepítéséhez kattintson ide**.</span><span class="sxs-lookup"><span data-stu-id="b0171-168">Under **On-premises Hybrid Connection Manager**, choose **Click here to install**.</span></span>
   
    ![Kattintson ide][ClickToInstallHCM]
5. <span data-ttu-id="b0171-170">Az alkalmazás futtatása biztonsági figyelmeztetés párbeszédpanel, válassza a **futtatása** folytatja.</span><span class="sxs-lookup"><span data-stu-id="b0171-170">In the Application Run security warning dialog, choose **Run** to continue.</span></span>
   
    ![Válassza a Futtatás a folytatáshoz][ApplicationRunWarning]
6. <span data-ttu-id="b0171-172">Az a **felhasználói fiókok felügyelete** párbeszédpanelen válasszon **Igen**.</span><span class="sxs-lookup"><span data-stu-id="b0171-172">In the **User Account Control** dialog, choose **Yes**.</span></span>
   
   ![Igen választása][UAC]
7. <span data-ttu-id="b0171-174">A Hybrid Connection Manager letöltötte és telepítette a.</span><span class="sxs-lookup"><span data-stu-id="b0171-174">The Hybrid Connection Manager is downloaded and installed for you.</span></span> 
   
    ![Telepítése][HCMInstalling]
8. <span data-ttu-id="b0171-176">A telepítés befejeztével kattintson **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="b0171-176">When the install completes, click **Close**.</span></span>
   
    ![Kattintson a Bezárás gombra][HCMInstallComplete]
   
    <span data-ttu-id="b0171-178">Az a **hibrid kapcsolatok** panelen a **állapot** most az oszlop látható **csatlakoztatva**.</span><span class="sxs-lookup"><span data-stu-id="b0171-178">On the **Hybrid connections** blade, the **Status** column now shows **Connected**.</span></span> 
   
    ![Csatlakoztatott állapot][HCStatusConnected]

<span data-ttu-id="b0171-180">Most, hogy a hibrid kapcsolat infrastruktúra befejeződött, létrehozhat egy hibrid alkalmazás, amely használja ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="b0171-180">Now that the hybrid connection infrastructure is complete, you can create a hybrid application that uses it.</span></span> 

> [!NOTE]
> <span data-ttu-id="b0171-181">Az alábbi szakaszok bemutatják a hibrid kapcsolat használata egy Mobile Apps .NET háttérrendszer-projekt esetében.</span><span class="sxs-lookup"><span data-stu-id="b0171-181">The following sections show you how to use a hybrid connection with a Mobile Apps .NET backend project.</span></span>
> 
> 

## <a name="configure-the-mobile-app-net-backend-project-to-connect-to-the-sql-server-database"></a><span data-ttu-id="b0171-182">Az SQL Server-adatbázishoz való kapcsolódáshoz a Mobile App .NET háttérrendszer-projekt konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b0171-182">Configure the Mobile App .NET backend project to connect to the SQL Server database</span></span>
<span data-ttu-id="b0171-183">Az App Service-ben a Mobile Apps .NET backend project csak ASP.NET webalkalmazás SDK-val egy további Mobile Apps telepítve és inicializálva.</span><span class="sxs-lookup"><span data-stu-id="b0171-183">In App Service, a Mobile Apps .NET backend project is just an ASP.NET web app with an additional Mobile Apps SDK installed and initialized.</span></span> <span data-ttu-id="b0171-184">A webalkalmazás a Mobile Apps-háttéralkalmazás használja, le kell [töltse le és inicializálni a Mobile Apps .NET-háttérrendszer SDK](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#install-sdk).</span><span class="sxs-lookup"><span data-stu-id="b0171-184">To use your web app as a Mobile Apps backend, you must [download and initialize the Mobile Apps .NET backend SDK](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#install-sdk).</span></span>  

<span data-ttu-id="b0171-185">Mobile Apps meg kell is adja meg a helyi adatbázis kapcsolati karakterláncot, és a háttér használata a kapcsolat módosításához.</span><span class="sxs-lookup"><span data-stu-id="b0171-185">For Mobile Apps, you also need to define a connection string for the on-premises database and modify the backend to use this connection.</span></span> 

1. <span data-ttu-id="b0171-186">A Visual Studio Solution Explorerben nyissa meg a Web.config fájlt a Mobile App .NET-háttérrendszer, keresse meg a **connectionStrings** területen írja be egy új SqlClient bejegyzés a következő, amely a helyszíni SQL Server mutat adatbázis:</span><span class="sxs-lookup"><span data-stu-id="b0171-186">In Solution Explorer in Visual Studio, open the Web.config file for your Mobile App .NET backend, locate the **connectionStrings** section, add a new SqlClient entry like the following, which points to the on-premises SQL Server database:</span></span>
   
        <add name="OnPremisesDBConnection"
         connectionString="Data Source=OnPremisesServer,1433;
         Initial Catalog=OnPremisesDB;
         User ID=HybridConnectionLogin;
         Password=<**secure_password**>;
         MultipleActiveResultSets=True"
         providerName="System.Data.SqlClient" />
   
    <span data-ttu-id="b0171-187">Ne felejtse el lecserélni `<**secure_password**>` ezt a jelszót, amelyet a karakterlánc a *HybridConnectionLogin*.</span><span class="sxs-lookup"><span data-stu-id="b0171-187">Remember to replace `<**secure_password**>` in this string with the password you created for  *HybridConnectionLogin*.</span></span>
2. <span data-ttu-id="b0171-188">Kattintson a **mentése** a Visual Studio menteni a Web.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="b0171-188">Click **Save** in Visual Studio to save the Web.config file.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="b0171-189">A kapcsolat a beállítással a helyi számítógéphez.</span><span class="sxs-lookup"><span data-stu-id="b0171-189">This connection setting is used when running on the local computer.</span></span> <span data-ttu-id="b0171-190">Ha fut az Azure-ban, a beállítás akkor felülbírálva által a portálon megadott kapcsolati beállításokat.</span><span class="sxs-lookup"><span data-stu-id="b0171-190">When running in Azure, this setting is overriden by the connection setting defined in the portal.</span></span>
   > 
   > 
3. <span data-ttu-id="b0171-191">Bontsa ki a **modellek** mappa, és nyissa meg az adatok modell fájlt, amely fejeződik be a *Context.cs*.</span><span class="sxs-lookup"><span data-stu-id="b0171-191">Expand the **Models** folder and open the data model file, which ends in *Context.cs*.</span></span>
4. <span data-ttu-id="b0171-192">Módosítsa a **DbContext** példánykonstruktor felelt meg az érték `OnPremisesDBConnection` bázis **DbContext** konstruktor a következő kódrészletet hasonló:</span><span class="sxs-lookup"><span data-stu-id="b0171-192">Modify the **DbContext** instance constructor to pass the value `OnPremisesDBConnection` to the base **DbContext** constructor, similar to the following snippet:</span></span>
   
        public class hybridService1Context : DbContext
        {
            public hybridService1Context()
                : base("OnPremisesDBConnection")
            {
            }
        }
   
    <span data-ttu-id="b0171-193">A szolgáltatás most fogja használni az SQL Server-adatbázist az új kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="b0171-193">The service will now use the new connection to the SQL Server database.</span></span>

## <a name="update-the-mobile-app-backend-to-use-the-on-premises-connection-string"></a><span data-ttu-id="b0171-194">A Mobile Apps-háttéralkalmazás használatára a helyszíni kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="b0171-194">Update the Mobile App backend to use the on-premises connection string</span></span>
<span data-ttu-id="b0171-195">Ezt követően kell az új kapcsolati karakterlánc az Alkalmazásbeállítás hozzáadása, hogy az Azure-ból is használható.</span><span class="sxs-lookup"><span data-stu-id="b0171-195">Next, you need to add an app setting for this new connection string so that it can be used from Azure.</span></span>  

1. <span data-ttu-id="b0171-196">Vissza a [Azure-portálon](https://portal.azure.com) a web Apps-háttéralkalmazás kódjához a Mobile Apps, kattintson **összes beállítás**, majd **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="b0171-196">Back in the [Azure portal](https://portal.azure.com) in the web app backend code for your Mobile App, click **All settings**, then **Application settings**.</span></span>
2. <span data-ttu-id="b0171-197">Az a **webalkalmazás-beállításainak** paneljén görgessen le a **kapcsolati karakterláncok** és adjon hozzá egy új **SQL Server** nevű kapcsolati karakterlánc `OnPremisesDBConnection` például értékű`Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>`.</span><span class="sxs-lookup"><span data-stu-id="b0171-197">In the **Web app settings** blade, scroll down to **Connection strings** and add an new **SQL Server** connection string named `OnPremisesDBConnection` with a value like `Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>`.</span></span>
   
    <span data-ttu-id="b0171-198">Cserélje le `<**secure_password**>` a helyi adatbázis biztonságos jelszavával.</span><span class="sxs-lookup"><span data-stu-id="b0171-198">Replace `<**secure_password**>` with the secure password for your on-premises database.</span></span>
   
    ![A helyi adatbázis kapcsolati karakterlánca](./media/web-sites-hybrid-connection-get-started/set-sql-server-database-connection.png)
3. <span data-ttu-id="b0171-200">Nyomja le az **mentése** menteni a hibrid kapcsolat és a kapcsolati karakterlánc az imént létrehozott.</span><span class="sxs-lookup"><span data-stu-id="b0171-200">Press **Save** to save the hybrid connection and connection string you just created.</span></span>

<span data-ttu-id="b0171-201">Ezen a ponton a projekt ismételt közzététele, és az új kapcsolat tesztelése a meglévő Mobile Apps-ügyfelekkel.</span><span class="sxs-lookup"><span data-stu-id="b0171-201">At this point you can republish the server project and test the new connection with your existing Mobile Apps clients.</span></span> <span data-ttu-id="b0171-202">Adatok olvasni és a helyi adatbázisba, a hibrid kapcsolat használatával.</span><span class="sxs-lookup"><span data-stu-id="b0171-202">Data will be read from and written to the on-premises database using the hybrid connection.</span></span>

<a name="NextSteps"></a>

## <a name="next-steps"></a><span data-ttu-id="b0171-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b0171-203">Next Steps</span></span>
* <span data-ttu-id="b0171-204">Tudnivalók a hibrid kapcsolatot használó ASP.NET webalkalmazás létrehozásáról lásd: [csatlakozás egy helyszíni SQL Server egy hibrid kapcsolatok használata az Azure webhelyén](http://go.microsoft.com/fwlink/?LinkID=397979).</span><span class="sxs-lookup"><span data-stu-id="b0171-204">For information on creating an ASP.NET web application that uses a hybrid connection, see [Connect to an on-premises SQL Server from an Azure web site using Hybrid Connections](http://go.microsoft.com/fwlink/?LinkID=397979).</span></span> 

### <a name="additional-resources"></a><span data-ttu-id="b0171-205">További források</span><span class="sxs-lookup"><span data-stu-id="b0171-205">Additional Resources</span></span>
[<span data-ttu-id="b0171-206">A hibrid kapcsolatok áttekintése</span><span class="sxs-lookup"><span data-stu-id="b0171-206">Hybrid Connections overview</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[<span data-ttu-id="b0171-207">Josh a jelölés vezet be a hibrid kapcsolatok (videó a Channel 9)</span><span class="sxs-lookup"><span data-stu-id="b0171-207">Josh Twist introduces hybrid connections (Channel 9 video)</span></span>](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[<span data-ttu-id="b0171-208">Hibrid kapcsolatok webhely</span><span class="sxs-lookup"><span data-stu-id="b0171-208">Hybrid Connections web site</span></span>](https://azure.microsoft.com/services/biztalk-services/)

[<span data-ttu-id="b0171-209">BizTalk szolgáltatások: Irányítópult, a figyelő, a méretezés, a konfigurálás és a hibrid kapcsolat lapokon</span><span class="sxs-lookup"><span data-stu-id="b0171-209">BizTalk Services: Dashboard, Monitor, Scale, Configure, and Hybrid Connection tabs</span></span>](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[<span data-ttu-id="b0171-210">Zökkenőmentes alkalmazások hordozhatóságát (videó a Channel 9) egy valós hibrid felhő felépítése</span><span class="sxs-lookup"><span data-stu-id="b0171-210">Building a Real-World Hybrid Cloud with Seamless Application Portability (Channel 9 video)</span></span>](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[<span data-ttu-id="b0171-211">Csatlakozzon egy helyi SQL Server az Azure Mobile Services használatával hibrid kapcsolatok (videó a Channel 9)</span><span class="sxs-lookup"><span data-stu-id="b0171-211">Connect to an on-premises SQL Server from Azure Mobile Services using Hybrid Connections (Channel 9 video)</span></span>](http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Connect-to-an-on-premises-SQL-Server-from-Azure-Mobile-Services-using-Hybrid-Connections)

## <a name="whats-changed"></a><span data-ttu-id="b0171-212">A változások</span><span class="sxs-lookup"><span data-stu-id="b0171-212">What's changed</span></span>
* <span data-ttu-id="b0171-213">Információk a Websites szolgáltatásról az App Service-re való váltásról: [Az Azure App Service és a hatása a meglévő Azure-szolgáltatásokra](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="b0171-213">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

<!-- IMAGES -->
[New]:./media/web-sites-hybrid-connection-get-started/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-get-started/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-get-started/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-get-started/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-get-started/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-get-started/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-get-started/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-get-started/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-get-started/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-get-started/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-get-started/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-get-started/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-get-started/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-get-started/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-get-started/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-get-started/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-get-started/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-get-started/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-get-started/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-get-started/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-get-started/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-get-started/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-get-started/D10HCStatusConnected.png
