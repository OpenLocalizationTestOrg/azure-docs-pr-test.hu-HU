---
title: "hibrid kapcsolatok használata az Azure App Service aaaAccess a helyszíni erőforrások"
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
ms.openlocfilehash: de7c57b94f4bd6250a93757817178e8455daae4a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="access-on-premises-resources-using-hybrid-connections-in-azure-app-service"></a><span data-ttu-id="a87f1-103">Helyszíni erőforrások elérése hibrid kapcsolatok használatával az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="a87f1-103">Access on-premises resources using hybrid connections in Azure App Service</span></span>
<span data-ttu-id="a87f1-104">Csatlakoztathatja egy Azure App Service alkalmazás tooany helyszíni-erőforrást, amely a statikus TCP-port, például az SQL Server, a MySQL, a HTTP webes API-k és a legtöbb egyéni webszolgáltatások használja.</span><span class="sxs-lookup"><span data-stu-id="a87f1-104">You can connect an Azure App Service app tooany on-premises resource that uses a static TCP port, such as SQL Server, MySQL, HTTP Web APIs, and most custom Web Services.</span></span> <span data-ttu-id="a87f1-105">Ez a cikk bemutatja, hogyan toocreate egy hibrid kapcsolat App Service és a helyszíni SQL Server-adatbázis között.</span><span class="sxs-lookup"><span data-stu-id="a87f1-105">This article shows you how toocreate a hybrid connection between App Service and an on-premises SQL Server database.</span></span>

> [!NOTE]
> <span data-ttu-id="a87f1-106">hello webalkalmazások hello hibrid kapcsolatok szolgáltatás része csak a hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="a87f1-106">hello Web Apps portion of hello Hybrid Connections feature is available only in hello [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="a87f1-107">BizTalk szolgáltatások, a kapcsolat toocreate lásd [hibrid kapcsolatok](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span><span class="sxs-lookup"><span data-stu-id="a87f1-107">toocreate a connection in BizTalk Services, see [Hybrid Connections](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span></span> 
> 
> <span data-ttu-id="a87f1-108">Ez a tartalom is érvényes tooMobile alkalmazások az Azure App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="a87f1-108">This content also applies tooMobile Apps in Azure App Service.</span></span> 
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="a87f1-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="a87f1-109">Prerequisites</span></span>
* <span data-ttu-id="a87f1-110">Azure-előfizetés.</span><span class="sxs-lookup"><span data-stu-id="a87f1-110">An Azure subscription.</span></span> <span data-ttu-id="a87f1-111">Ingyenes előfizetésre, lásd: [Azure ingyenes próbaverzió](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="a87f1-111">For a free subscription, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span> 
  
    <span data-ttu-id="a87f1-112">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="a87f1-112">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="a87f1-113">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="a87f1-113">No credit cards required; no commitments.</span></span>
* <span data-ttu-id="a87f1-114">toouse egy helyi SQL Server vagy SQL Server Express adatbázist egy hibrid kapcsolat, az TCP/IP toobe statikus port engedélyezve van szüksége.</span><span class="sxs-lookup"><span data-stu-id="a87f1-114">toouse an on-premises SQL Server or SQL Server Express database with a hybrid connection, TCP/IP needs toobe enabled on a static port.</span></span> <span data-ttu-id="a87f1-115">Az SQL Server alapértelmezett példánnyal használata ajánlott, mivel a 1433-as port statikus használ.</span><span class="sxs-lookup"><span data-stu-id="a87f1-115">Using a default instance on SQL Server is recommended because it uses static port 1433.</span></span> <span data-ttu-id="a87f1-116">Információ a telepítése és konfigurálása az SQL Server Express hibrid kapcsolatok való használathoz: [Connect tooan a helyszíni SQL Server egy hibrid kapcsolatok használata az Azure webhelyén](http://go.microsoft.com/fwlink/?LinkID=397979).</span><span class="sxs-lookup"><span data-stu-id="a87f1-116">For information on installing and configuring SQL Server Express for use with hybrid connections, see [Connect tooan on-premises SQL Server from an Azure web site using Hybrid Connections](http://go.microsoft.com/fwlink/?LinkID=397979).</span></span>
* <span data-ttu-id="a87f1-117">hello számítógép, amelyre a cikk későbbi részében leírt hello helyszíni Hybrid Connection Manager ügynök telepíti:</span><span class="sxs-lookup"><span data-stu-id="a87f1-117">hello computer on which you install hello on-premises Hybrid Connection Manager agent described later in this article:</span></span>
  
  * <span data-ttu-id="a87f1-118">Képes tooconnect tooAzure kell port: 5671</span><span class="sxs-lookup"><span data-stu-id="a87f1-118">Must be able tooconnect tooAzure over port 5671</span></span>
  * <span data-ttu-id="a87f1-119">Képes tooreach hello kell *állomásnév*:*portszám* a helyi erőforrás.</span><span class="sxs-lookup"><span data-stu-id="a87f1-119">Must be able tooreach hello *hostname*:*portnumber* of your on-premises resource.</span></span> 

> [!NOTE]
> <span data-ttu-id="a87f1-120">Ez a cikk hello lépések azt feltételezik, hogy hello helyszíni hibrid kapcsolat ügynököt futtató számítógépről hello hello böngészőt használ.</span><span class="sxs-lookup"><span data-stu-id="a87f1-120">hello steps in this article assume that you are using hello browser from hello computer that will host hello on-premises hybrid connection agent.</span></span>
> 
> 

## <a name="create-a-web-app-in-hello-azure-portal"></a><span data-ttu-id="a87f1-121">A webalkalmazás létrehozása az Azure portál hello</span><span class="sxs-lookup"><span data-stu-id="a87f1-121">Create a web app in hello Azure Portal</span></span>
> [!NOTE]
> <span data-ttu-id="a87f1-122">Ha már létrehozott egy webes alkalmazás vagy a Mobile Apps-háttéralkalmazás hello Azure portálon megjeleníteni kívánt toouse ehhez az oktatóanyaghoz, akkor kihagyhatja azokat, amelyek túl[egy hibrid kapcsolat és a BizTalk szolgáltatás létrehozása](#CreateHC) , és indítsa el onnan.</span><span class="sxs-lookup"><span data-stu-id="a87f1-122">If you have already created a web app or Mobile App backend in hello Azure Portal that you want toouse for this tutorial, you can skip ahead too[Create a Hybrid Connection and a BizTalk Service](#CreateHC) and start from there.</span></span>
> 
> 

1. <span data-ttu-id="a87f1-123">A hello bal felső sarkában hello [Azure Portal](https://portal.azure.com), kattintson a **új** > **Web + mobil** > **webalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="a87f1-123">In hello upper left corner of hello [Azure Portal](https://portal.azure.com), click **New** > **Web + Mobile** > **Web App**.</span></span>
   
    ![Új webalkalmazás][NewWebsite]
2. <span data-ttu-id="a87f1-125">A hello **webalkalmazás** panelen adjon meg egy URL-címet, és kattintson **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="a87f1-125">On hello **Web app** blade, provide a URL and click **Create**.</span></span> 
   
    ![Webhely neve][WebsiteCreationBlade]
3. <span data-ttu-id="a87f1-127">Néhány másodpercen belül hello webalkalmazás jön létre, és a webalkalmazás panelen jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a87f1-127">After a few moments, hello web app is created and its web app blade appears.</span></span> <span data-ttu-id="a87f1-128">hello panel egy függőleges görgethető irányítópultot, amely lehetővé teszi, hogy a hely kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="a87f1-128">hello blade is a vertically scrollable dashboard that lets you manage your site.</span></span>
   
    ![Futtató webhely][WebSiteRunningBlade]
4. <span data-ttu-id="a87f1-130">élő tooverify hello hely, rákattinthat hello **Tallózás** ikon toodisplay hello alapértelmezett oldalt.</span><span class="sxs-lookup"><span data-stu-id="a87f1-130">tooverify hello site is live, you can click hello **Browse** icon toodisplay hello default page.</span></span>
   
    ![Kattintson a Tallózás toosee a webalkalmazás][Browse]
   
    ![Alapértelmezett weblap-alkalmazás][DefaultWebSitePage]

<span data-ttu-id="a87f1-133">Ezt követően egy hibrid kapcsolat és a BizTalk szolgáltatás hello webalkalmazás hoz létre.</span><span class="sxs-lookup"><span data-stu-id="a87f1-133">Next, you will create a hybrid connection and a BizTalk service for hello web app.</span></span>

<a name="CreateHC"></a>

## <a name="create-a-hybrid-connection-and-a-biztalk-service"></a><span data-ttu-id="a87f1-134">A hibrid kapcsolat és a BizTalk szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="a87f1-134">Create a Hybrid Connection and a BizTalk Service</span></span>
1. <span data-ttu-id="a87f1-135">A webalkalmazás panelen kattintson a **összes beállítás** > **hálózati** > **hibrid kapcsolati végpontok konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="a87f1-135">In your web app blade click on **All settings** > **Networking** > **Configure your hybrid connection endpoints**.</span></span>
   
    ![Hibrid kapcsolatok][CreateHCHCIcon]
2. <span data-ttu-id="a87f1-137">Hello hibrid kapcsolatok paneljén kattintson **Hozzáadás**.</span><span class="sxs-lookup"><span data-stu-id="a87f1-137">On hello Hybrid connections blade, click **Add**.</span></span>
   
    <!-- ![Add a hybrid connnection][CreateHCAddHC]
   -->
3. <span data-ttu-id="a87f1-138">Hello **egy hibrid kapcsolat hozzáadása a** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="a87f1-138">hello **Add a hybrid connection** blade opens.</span></span>  <span data-ttu-id="a87f1-139">Mivel ez az első a hibrid kapcsolat, hello **új hibrid kapcsolat** beállítás van megadva, és hello **hibrid kapcsolat létrehozása** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="a87f1-139">Since this is your first hybrid connection, hello **New hybrid connection** option is preselected, and hello **Create hybrid connection** blade opens for you.</span></span>
   
    ![A hibrid kapcsolat létrehozása][TwinCreateHCBlades]
   
    <span data-ttu-id="a87f1-141">A hello **létrehozás hibrid kapcsolat panel**:</span><span class="sxs-lookup"><span data-stu-id="a87f1-141">On hello **Create hybrid connection blade**:</span></span>
   
   * <span data-ttu-id="a87f1-142">A **neve**, hello kapcsolat nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="a87f1-142">For **Name**, provide a name for hello connection.</span></span>
   * <span data-ttu-id="a87f1-143">A **állomásnév**, adja meg, amelyen az erőforrás hello a helyi számítógép hello nevét.</span><span class="sxs-lookup"><span data-stu-id="a87f1-143">For **Hostname**, enter hello name of hello on-premises computer that hosts your resource.</span></span>
   * <span data-ttu-id="a87f1-144">A **Port**, adja meg, hogy a helyi erőforrás használja (SQL Server alapértelmezett példány 1433) hello portszámot.</span><span class="sxs-lookup"><span data-stu-id="a87f1-144">For **Port**, enter hello port number that your on-premises resource uses (1433 for a SQL Server default instance).</span></span>
   * <span data-ttu-id="a87f1-145">Kattintson a **Biz előadás szolgáltatás**</span><span class="sxs-lookup"><span data-stu-id="a87f1-145">Click **Biz Talk Service**</span></span>
4. <span data-ttu-id="a87f1-146">Hello **BizTalk szolgáltatás létrehozása** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="a87f1-146">hello **Create BizTalk Service** blade opens.</span></span> <span data-ttu-id="a87f1-147">Adjon meg egy nevet hello BizTalk szolgáltatás, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="a87f1-147">Enter a name for hello BizTalk service, and then click **OK**.</span></span>
   
    ![BizTalk szolgáltatás létrehozása][CreateHCCreateBTS]
   
    <span data-ttu-id="a87f1-149">Hello **BizTalk szolgáltatás létrehozása** panel bezárása után, és ismét toohello **hibrid kapcsolat létrehozása** panelen.</span><span class="sxs-lookup"><span data-stu-id="a87f1-149">hello **Create BizTalk Service** blade closes and you are returned toohello **Create hybrid connection** blade.</span></span>
5. <span data-ttu-id="a87f1-150">Hello létrehozása a hibrid kapcsolat paneljén kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="a87f1-150">On hello Create hybrid connection blade, click **OK**.</span></span> 
   
    ![Kattintson az OK gombra][CreateBTScomplete]
6. <span data-ttu-id="a87f1-152">Hello folyamat befejezése után a hello értesítési terület, a portál hello arról tájékoztatja, hogy hello kapcsolat sikeresen létrejött.</span><span class="sxs-lookup"><span data-stu-id="a87f1-152">When hello process completes, hello notifications area in hello Portal informs you that hello connection has been successfully created.</span></span>
   
    <!--- TODO
   
    Everything fails at this step. I can't create a BizTalk service in hello dogfood portal. I switch toohello classic portal
    (full portal) and created hello BizTalk service but it doesn't seem toolet you connnect them - When you finish the
    Create hybrid conn step, you get hello following error
    Failed toocreate hybrid connection RelecIoudHC. hello 
    resource type could not be found in hello namespace 
    'Microsoft.BizTaIkServices for api version 2014-06-01'.
   
    hello error indicates it couldn't find hello type, not hello instance.
    ![Success notification][CreateHCSuccessNotification]
    -->
7. <span data-ttu-id="a87f1-153">Hello webes alkalmazás paneljén hello **hibrid kapcsolatok** ikon már helyesen jelenik meg, hogy 1 hibrid kapcsolat létrejött.</span><span class="sxs-lookup"><span data-stu-id="a87f1-153">On hello web app's blade, hello **Hybrid connections** icon now shows that 1 hybrid connection has been created.</span></span>
   
    ![Egy hibrid kapcsolat létrehozása][CreateHCOneConnectionCreated]

<span data-ttu-id="a87f1-155">Ezen a ponton befejeződött hello felhőalapú hibrid kapcsolat infrastruktúra fontos része.</span><span class="sxs-lookup"><span data-stu-id="a87f1-155">At this point, you have completed an important part of hello cloud hybrid connection infrastructure.</span></span> <span data-ttu-id="a87f1-156">Ezután létrehozhat egy helyszíni megfelelő adat.</span><span class="sxs-lookup"><span data-stu-id="a87f1-156">Next, you will create a corresponding on-premises piece.</span></span>

<a name="InstallHCM"></a>

## <a name="install-hello-on-premises-hybrid-connection-manager-toocomplete-hello-connection"></a><span data-ttu-id="a87f1-157">Hello helyszíni Hybrid Connection Manager toocomplete hello kapcsolat telepítése</span><span class="sxs-lookup"><span data-stu-id="a87f1-157">Install hello on-premises Hybrid Connection Manager toocomplete hello connection</span></span>
1. <span data-ttu-id="a87f1-158">Hello webes alkalmazás paneljén kattintson **összes beállítás** > **hálózati** > **hibrid kapcsolati végpontok konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="a87f1-158">On hello web app's blade, click **All settings** > **Networking** > **Configure your hybrid connection endpoints**.</span></span> 
   
    ![Hibrid kapcsolatok ikonja][HCIcon]
2. <span data-ttu-id="a87f1-160">A hello **hibrid kapcsolatok** panelen, hello **állapot** hello oszlopában nemrég adta hozzá a végpont látható **nincs csatlakoztatva**.</span><span class="sxs-lookup"><span data-stu-id="a87f1-160">On hello **Hybrid connections** blade, hello **Status** column for hello recently added endpoint shows **Not connected**.</span></span> <span data-ttu-id="a87f1-161">Kattintson a hello kapcsolat tooconfigure azt.</span><span class="sxs-lookup"><span data-stu-id="a87f1-161">Click hello connection tooconfigure it.</span></span>
   
    ![Nincs csatlakoztatva][NotConnected]
   
    <span data-ttu-id="a87f1-163">hello hibrid kapcsolat panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="a87f1-163">hello Hybrid connection blade opens.</span></span>
   
    ![NotConnectedBlade][NotConnectedBlade]
3. <span data-ttu-id="a87f1-165">Hello paneljén kattintson **figyelő telepítő**.</span><span class="sxs-lookup"><span data-stu-id="a87f1-165">On hello blade, click **Listener Setup**.</span></span>
   
    ![Kattintson a figyelő beállítás][ClickListenerSetup]
4. <span data-ttu-id="a87f1-167">Hello **hibrid kapcsolat tulajdonságai** panel nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="a87f1-167">hello **Hybrid connection properties** blade opens.</span></span> <span data-ttu-id="a87f1-168">A **helyszíni Hybrid Connection Manager**, válassza a **ide tooinstall**.</span><span class="sxs-lookup"><span data-stu-id="a87f1-168">Under **On-premises Hybrid Connection Manager**, choose **Click here tooinstall**.</span></span>
   
    ![Kattintson ide tooinstall][ClickToInstallHCM]
5. <span data-ttu-id="a87f1-170">Hello alkalmazás futtatása biztonsági figyelmeztetés párbeszédpanel, válassza a **futtatása** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="a87f1-170">In hello Application Run security warning dialog, choose **Run** toocontinue.</span></span>
   
    ![Válassza a Futtatás toocontinue][ApplicationRunWarning]
6. <span data-ttu-id="a87f1-172">A hello **felhasználói fiókok felügyelete** párbeszédpanelen válasszon **Igen**.</span><span class="sxs-lookup"><span data-stu-id="a87f1-172">In hello **User Account Control** dialog, choose **Yes**.</span></span>
   
   ![Igen választása][UAC]
7. <span data-ttu-id="a87f1-174">hello Hybrid Connection Manager letöltötte és telepítette a.</span><span class="sxs-lookup"><span data-stu-id="a87f1-174">hello Hybrid Connection Manager is downloaded and installed for you.</span></span> 
   
    ![Telepítése][HCMInstalling]
8. <span data-ttu-id="a87f1-176">Hello telepítés befejeztével kattintson **Bezárás**.</span><span class="sxs-lookup"><span data-stu-id="a87f1-176">When hello install completes, click **Close**.</span></span>
   
    ![Kattintson a Bezárás gombra][HCMInstallComplete]
   
    <span data-ttu-id="a87f1-178">A hello **hibrid kapcsolatok** panelen, hello **állapot** most az oszlop látható **csatlakoztatva**.</span><span class="sxs-lookup"><span data-stu-id="a87f1-178">On hello **Hybrid connections** blade, hello **Status** column now shows **Connected**.</span></span> 
   
    ![Csatlakoztatott állapot][HCStatusConnected]

<span data-ttu-id="a87f1-180">Most, hogy hello hibrid kapcsolat infrastruktúra befejeződött, létrehozhat egy hibrid alkalmazás, amely használja ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="a87f1-180">Now that hello hybrid connection infrastructure is complete, you can create a hybrid application that uses it.</span></span> 

> [!NOTE]
> <span data-ttu-id="a87f1-181">hello következő szakaszok bemutatják, hogyan toouse egy hibrid kapcsolat egy Mobile Apps .NET háttérrendszer-projekt esetében.</span><span class="sxs-lookup"><span data-stu-id="a87f1-181">hello following sections show you how toouse a hybrid connection with a Mobile Apps .NET backend project.</span></span>
> 
> 

## <a name="configure-hello-mobile-app-net-backend-project-tooconnect-toohello-sql-server-database"></a><span data-ttu-id="a87f1-182">Hello Mobile App .NET backend project tooconnect toohello SQL Server-adatbázis konfigurálása</span><span class="sxs-lookup"><span data-stu-id="a87f1-182">Configure hello Mobile App .NET backend project tooconnect toohello SQL Server database</span></span>
<span data-ttu-id="a87f1-183">Az App Service-ben a Mobile Apps .NET backend project csak ASP.NET webalkalmazás SDK-val egy további Mobile Apps telepítve és inicializálva.</span><span class="sxs-lookup"><span data-stu-id="a87f1-183">In App Service, a Mobile Apps .NET backend project is just an ASP.NET web app with an additional Mobile Apps SDK installed and initialized.</span></span> <span data-ttu-id="a87f1-184">toouse kell a webalkalmazást, mint a Mobile Apps-háttéralkalmazás, [töltse le és hello Mobile Apps .NET-háttérrendszer SDK inicializálása](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#install-sdk).</span><span class="sxs-lookup"><span data-stu-id="a87f1-184">toouse your web app as a Mobile Apps backend, you must [download and initialize hello Mobile Apps .NET backend SDK](../app-service-mobile/app-service-mobile-dotnet-backend-how-to-use-server-sdk.md#install-sdk).</span></span>  

<span data-ttu-id="a87f1-185">A Mobile Apps is hello a helyi adatbázis toodefine egy kapcsolati karakterláncot kell, majd módosítsa hello háttér toouse a kapcsolatot.</span><span class="sxs-lookup"><span data-stu-id="a87f1-185">For Mobile Apps, you also need toodefine a connection string for hello on-premises database and modify hello backend toouse this connection.</span></span> 

1. <span data-ttu-id="a87f1-186">A Solution Explorerben a Visual Studio, nyissa meg hello Web.config fájlja a Mobile App .NET-háttérrendszer, keresse meg a hello **connectionStrings** területen hasonló hello, mely pontok toohello a helyszíni SQL új SqlClient bejegyzés hozzáadása Server-adatbázist:</span><span class="sxs-lookup"><span data-stu-id="a87f1-186">In Solution Explorer in Visual Studio, open hello Web.config file for your Mobile App .NET backend, locate hello **connectionStrings** section, add a new SqlClient entry like hello following, which points toohello on-premises SQL Server database:</span></span>
   
        <add name="OnPremisesDBConnection"
         connectionString="Data Source=OnPremisesServer,1433;
         Initial Catalog=OnPremisesDB;
         User ID=HybridConnectionLogin;
         Password=<**secure_password**>;
         MultipleActiveResultSets=True"
         providerName="System.Data.SqlClient" />
   
    <span data-ttu-id="a87f1-187">Ne feledje tooreplace `<**secure_password**>` a karakterláncban létrehozott hello jelszóval *HybridConnectionLogin*.</span><span class="sxs-lookup"><span data-stu-id="a87f1-187">Remember tooreplace `<**secure_password**>` in this string with hello password you created for  *HybridConnectionLogin*.</span></span>
2. <span data-ttu-id="a87f1-188">Kattintson a **mentése** Visual Studio toosave hello Web.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="a87f1-188">Click **Save** in Visual Studio toosave hello Web.config file.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="a87f1-189">A beállítás akkor használatos, ha a hello helyi számítógépen futó.</span><span class="sxs-lookup"><span data-stu-id="a87f1-189">This connection setting is used when running on hello local computer.</span></span> <span data-ttu-id="a87f1-190">Ha fut az Azure-ban, a beállítás akkor felülbírálva hello portálon definiált hello-kapcsolat beállítása.</span><span class="sxs-lookup"><span data-stu-id="a87f1-190">When running in Azure, this setting is overriden by hello connection setting defined in hello portal.</span></span>
   > 
   > 
3. <span data-ttu-id="a87f1-191">Bontsa ki a hello **modellek** mappa és fejeződik be a nyitott hello adatok modell fájljából *Context.cs*.</span><span class="sxs-lookup"><span data-stu-id="a87f1-191">Expand hello **Models** folder and open hello data model file, which ends in *Context.cs*.</span></span>
4. <span data-ttu-id="a87f1-192">Hello módosítása **DbContext** példány konstruktor toopass hello érték `OnPremisesDBConnection` alap toohello **DbContext** konstruktor, hasonló toohello a következő kódrészletet:</span><span class="sxs-lookup"><span data-stu-id="a87f1-192">Modify hello **DbContext** instance constructor toopass hello value `OnPremisesDBConnection` toohello base **DbContext** constructor, similar toohello following snippet:</span></span>
   
        public class hybridService1Context : DbContext
        {
            public hybridService1Context()
                : base("OnPremisesDBConnection")
            {
            }
        }
   
    <span data-ttu-id="a87f1-193">hello szolgáltatás most hello új kapcsolat toohello SQL Server-adatbázist használja.</span><span class="sxs-lookup"><span data-stu-id="a87f1-193">hello service will now use hello new connection toohello SQL Server database.</span></span>

## <a name="update-hello-mobile-app-backend-toouse-hello-on-premises-connection-string"></a><span data-ttu-id="a87f1-194">Hello mobilalkalmazás háttér toouse hello helyszíni kapcsolati karakterlánc frissítése</span><span class="sxs-lookup"><span data-stu-id="a87f1-194">Update hello Mobile App backend toouse hello on-premises connection string</span></span>
<span data-ttu-id="a87f1-195">A következő lépésben tooadd Alkalmazásbeállítás a új kapcsolati karakterlánccal, hogy az Azure-ból is használható.</span><span class="sxs-lookup"><span data-stu-id="a87f1-195">Next, you need tooadd an app setting for this new connection string so that it can be used from Azure.</span></span>  

1. <span data-ttu-id="a87f1-196">Vissza a hello [Azure-portálon](https://portal.azure.com) hello web Apps-háttéralkalmazás kódjához a Mobile Apps, kattintson **összes beállítás**, majd **Alkalmazásbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="a87f1-196">Back in hello [Azure portal](https://portal.azure.com) in hello web app backend code for your Mobile App, click **All settings**, then **Application settings**.</span></span>
2. <span data-ttu-id="a87f1-197">A hello **webalkalmazás-beállításainak** panelen görgessen lefelé, túl**kapcsolati karakterláncok** és adjon hozzá egy új **SQL Server** nevű kapcsolati karakterlánc `OnPremisesDBConnection` például értékkel`Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>`.</span><span class="sxs-lookup"><span data-stu-id="a87f1-197">In hello **Web app settings** blade, scroll down too**Connection strings** and add an new **SQL Server** connection string named `OnPremisesDBConnection` with a value like `Server=OnPremisesServer,1433;Database=OnPremisesDB;User ID=HybridConnectionsLogin;Password=<**secure_password**>`.</span></span>
   
    <span data-ttu-id="a87f1-198">Cserélje le `<**secure_password**>` hello biztonságos jelszó a helyi adatbázis.</span><span class="sxs-lookup"><span data-stu-id="a87f1-198">Replace `<**secure_password**>` with hello secure password for your on-premises database.</span></span>
   
    ![A helyi adatbázis kapcsolati karakterlánca](./media/web-sites-hybrid-connection-get-started/set-sql-server-database-connection.png)
3. <span data-ttu-id="a87f1-200">Nyomja le az **mentése** toosave hello hibrid kapcsolat és a kapcsolati karakterlánc újonnan létrehozott.</span><span class="sxs-lookup"><span data-stu-id="a87f1-200">Press **Save** toosave hello hybrid connection and connection string you just created.</span></span>

<span data-ttu-id="a87f1-201">Ezen a ponton hello kiszolgálóprojektet ismételt közzététele, és hello új kapcsolat tesztelése a meglévő Mobile Apps-ügyfelekkel.</span><span class="sxs-lookup"><span data-stu-id="a87f1-201">At this point you can republish hello server project and test hello new connection with your existing Mobile Apps clients.</span></span> <span data-ttu-id="a87f1-202">Adatok olvasni és írt toohello, helyszíni adatbázisokhoz hello hibrid kapcsolat használatával.</span><span class="sxs-lookup"><span data-stu-id="a87f1-202">Data will be read from and written toohello on-premises database using hello hybrid connection.</span></span>

<a name="NextSteps"></a>

## <a name="next-steps"></a><span data-ttu-id="a87f1-203">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a87f1-203">Next Steps</span></span>
* <span data-ttu-id="a87f1-204">Tudnivalók a hibrid kapcsolatot használó ASP.NET webalkalmazás létrehozásáról lásd: [Connect tooan a helyszíni SQL Server egy hibrid kapcsolatok használata az Azure webhelyén](http://go.microsoft.com/fwlink/?LinkID=397979).</span><span class="sxs-lookup"><span data-stu-id="a87f1-204">For information on creating an ASP.NET web application that uses a hybrid connection, see [Connect tooan on-premises SQL Server from an Azure web site using Hybrid Connections](http://go.microsoft.com/fwlink/?LinkID=397979).</span></span> 

### <a name="additional-resources"></a><span data-ttu-id="a87f1-205">További források</span><span class="sxs-lookup"><span data-stu-id="a87f1-205">Additional Resources</span></span>
[<span data-ttu-id="a87f1-206">A hibrid kapcsolatok áttekintése</span><span class="sxs-lookup"><span data-stu-id="a87f1-206">Hybrid Connections overview</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[<span data-ttu-id="a87f1-207">Josh a jelölés vezet be a hibrid kapcsolatok (videó a Channel 9)</span><span class="sxs-lookup"><span data-stu-id="a87f1-207">Josh Twist introduces hybrid connections (Channel 9 video)</span></span>](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[<span data-ttu-id="a87f1-208">Hibrid kapcsolatok webhely</span><span class="sxs-lookup"><span data-stu-id="a87f1-208">Hybrid Connections web site</span></span>](https://azure.microsoft.com/services/biztalk-services/)

[<span data-ttu-id="a87f1-209">BizTalk szolgáltatások: Irányítópult, a figyelő, a méretezés, a konfigurálás és a hibrid kapcsolat lapokon</span><span class="sxs-lookup"><span data-stu-id="a87f1-209">BizTalk Services: Dashboard, Monitor, Scale, Configure, and Hybrid Connection tabs</span></span>](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[<span data-ttu-id="a87f1-210">Zökkenőmentes alkalmazások hordozhatóságát (videó a Channel 9) egy valós hibrid felhő felépítése</span><span class="sxs-lookup"><span data-stu-id="a87f1-210">Building a Real-World Hybrid Cloud with Seamless Application Portability (Channel 9 video)</span></span>](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[<span data-ttu-id="a87f1-211">Csatlakozás tooan a helyszíni SQL Server az Azure Mobile Services használatával hibrid kapcsolatok (videó a Channel 9)</span><span class="sxs-lookup"><span data-stu-id="a87f1-211">Connect tooan on-premises SQL Server from Azure Mobile Services using Hybrid Connections (Channel 9 video)</span></span>](http://channel9.msdn.com/Series/Windows-Azure-Mobile-Services/Connect-to-an-on-premises-SQL-Server-from-Azure-Mobile-Services-using-Hybrid-Connections)

## <a name="whats-changed"></a><span data-ttu-id="a87f1-212">A változások</span><span class="sxs-lookup"><span data-stu-id="a87f1-212">What's changed</span></span>
* <span data-ttu-id="a87f1-213">Egy útmutató toohello webhelyek tooApp szolgáltatás változás lásd: [Azure App Service és a hatása a meglévő Azure-szolgáltatások](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="a87f1-213">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

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
