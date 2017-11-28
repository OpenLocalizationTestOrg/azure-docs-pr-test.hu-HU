---
title: "aaaConnect tooon helyszíni SQL Server rendszert egy webalkalmazást az Azure App Service hibrid kapcsolatok használata"
description: "A Microsoft Azure-webalkalmazás létrehozása, és csatlakoztassa tooan a helyszíni SQL Server-adatbázis"
services: app-service\web
documentationcenter: 
author: cephalin
manager: erikre
editor: mollybos
ms.assetid: 2b4e0539-1a0b-4aa1-8a69-b4b053c3b2e5
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/09/2016
ms.author: cephalin
ms.openlocfilehash: 2e8f8f7e0b9733cfb0433697615faba4358c6023
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-tooon-premises-sql-server-from-a-web-app-in-azure-app-service-using-hybrid-connections"></a><span data-ttu-id="51e84-103">Kapcsolódó tooon helyszíni SQL Server egy webalkalmazást az Azure App Service hibrid kapcsolatok használata</span><span class="sxs-lookup"><span data-stu-id="51e84-103">Connect tooon-premises SQL Server from a web app in Azure App Service using Hybrid Connections</span></span>
<span data-ttu-id="51e84-104">Hibrid kapcsolatok kapcsolódhatnak [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) statikus TCP-portot használó webalkalmazások tooon helyszíni erőforrások.</span><span class="sxs-lookup"><span data-stu-id="51e84-104">Hybrid Connections can connect [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps tooon-premises resources that use a static TCP port.</span></span> <span data-ttu-id="51e84-105">Támogatott erőforrások közé tartoznak a Microsoft SQL Server, a MySQL, a HTTP webes API-k, a App Service és a legtöbb egyéni webszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="51e84-105">Supported resources include Microsoft SQL Server, MySQL, HTTP Web APIs, App Service, and most custom Web Services.</span></span>

<span data-ttu-id="51e84-106">Ebből az oktatóanyagból megtudhatja, hogyan toocreate egy App Service webalkalmazás a hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), hello web app tooyour helyi helyszíni SQL Server adatbázis hello új hibrid kapcsolat funkció használata csatlakozáshoz, hozzon létre egy egyszerű ASP.NET az alkalmazás hello hibrid kapcsolat használatát, és hello alkalmazás toohello App Service-webalkalmazások telepítése.</span><span class="sxs-lookup"><span data-stu-id="51e84-106">In this tutorial, you will learn how toocreate an App Service web app in hello [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), connect hello web app tooyour local on-premises SQL Server database using hello new Hybrid Connection feature, create a simple ASP.NET application that will use hello hybrid connection, and deploy hello application toohello App Service web app.</span></span> <span data-ttu-id="51e84-107">hello végrehajtani a webalkalmazás Azure felhasználói hitelesítő adatokat, amely a helyszíni tagsági adatbázisban tárolja.</span><span class="sxs-lookup"><span data-stu-id="51e84-107">hello completed web app on Azure stores user credentials in a membership database that is on-premises.</span></span> <span data-ttu-id="51e84-108">hello oktatóanyag feltételezi, hogy nincs korábbi tapasztalata az Azure vagy az ASP.NET használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="51e84-108">hello tutorial assumes no prior experience using Azure or ASP.NET.</span></span>

> [!NOTE]
> <span data-ttu-id="51e84-109">Ha azt szeretné, hogy az az Azure-fiók regisztrálása előtt az Azure App Service lépései tooget, nyissa meg túl[App Service kipróbálása](https://azure.microsoft.com/try/app-service/), ahol azonnal létrehozhat egy rövid élettartamú alapszintű webalkalmazást az App Service-ben.</span><span class="sxs-lookup"><span data-stu-id="51e84-109">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="51e84-110">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="51e84-110">No credit cards required; no commitments.</span></span>
> 
> <span data-ttu-id="51e84-111">hello webalkalmazások hello hibrid kapcsolatok szolgáltatás része csak a hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="51e84-111">hello Web Apps portion of hello Hybrid Connections feature is available only in hello [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="51e84-112">BizTalk szolgáltatások, a kapcsolat toocreate lásd [hibrid kapcsolatok](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span><span class="sxs-lookup"><span data-stu-id="51e84-112">toocreate a connection in BizTalk Services, see [Hybrid Connections](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span></span>  
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="51e84-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="51e84-113">Prerequisites</span></span>
<span data-ttu-id="51e84-114">toocomplete ebben az oktatóanyagban szüksége lesz a következő termékek hello.</span><span class="sxs-lookup"><span data-stu-id="51e84-114">toocomplete this tutorial, you'll need hello following products.</span></span> <span data-ttu-id="51e84-115">Az összes szabad verziók, elérhetők nekiláthat teljes mértékben az Azure ingyenes.</span><span class="sxs-lookup"><span data-stu-id="51e84-115">All are available in free versions, so you can start developing for Azure entirely for free.</span></span>

* <span data-ttu-id="51e84-116">**Azure-előfizetés** - ingyenes előfizetésre, lásd: [Azure ingyenes próbaverzió](/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="51e84-116">**Azure subscription** - For a free subscription, see [Azure Free Trial](/pricing/free-trial/).</span></span>
* <span data-ttu-id="51e84-117">**A Visual Studio 2013** -toodownload egy ingyenes próbaverzióját Visual Studio 2013, lásd: [Visual Studio letöltések](http://www.visualstudio.com/downloads/download-visual-studio-vs).</span><span class="sxs-lookup"><span data-stu-id="51e84-117">**Visual Studio 2013** - toodownload a free trial version of Visual Studio 2013, see [Visual Studio Downloads](http://www.visualstudio.com/downloads/download-visual-studio-vs).</span></span> <span data-ttu-id="51e84-118">Telepítse a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="51e84-118">Install this before continuing.</span></span>
* <span data-ttu-id="51e84-119">**A Microsoft .NET Framework 3.5 Service Pack 1** – Ha az operációs rendszer Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7, vagy Windows Server 2008 R2, engedélyezheti a Vezérlőpult > Programok és szolgáltatások > Windows-szolgáltatások be- és kikapcsolása.</span><span class="sxs-lookup"><span data-stu-id="51e84-119">**Microsoft .NET Framework 3.5 Service Pack 1** - If your operating system is Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7, or Windows Server 2008 R2, you can enable this in Control Panel > Programs and Features > Turn Windows features on or off.</span></span> <span data-ttu-id="51e84-120">Ellenkező esetben letöltheti a hello [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).</span><span class="sxs-lookup"><span data-stu-id="51e84-120">Otherwise, you can download it from hello [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).</span></span>
* <span data-ttu-id="51e84-121">**SQL Server 2014 Express eszközökkel** -Microsoft SQL Server Express ingyenesen letölthető: hello [Microsoft Web Platform adatbázissal kapcsolatos beállításainak lapja](http://www.microsoft.com/web/platform/database.aspx).</span><span class="sxs-lookup"><span data-stu-id="51e84-121">**SQL Server 2014 Express with Tools** - download Microsoft SQL Server Express for free at hello [Microsoft Web Platform Database page](http://www.microsoft.com/web/platform/database.aspx).</span></span> <span data-ttu-id="51e84-122">Válassza ki a hello **Express** (nem LocalDB) verziója.</span><span class="sxs-lookup"><span data-stu-id="51e84-122">Choose hello **Express** (not LocalDB) version.</span></span> <span data-ttu-id="51e84-123">Hello **eszközökkel Express** tartalmazza az ebben az oktatóanyagban használandó SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="51e84-123">hello **Express with Tools** version includes SQL Server Management Studio, which you will use in this tutorial.</span></span>
* <span data-ttu-id="51e84-124">**SQL Server Management Studio Express** – Ez az SQL Server 2014 Express hello tartalmazza a fent említett eszközök letöltése, de ha tooinstall van szüksége, külön-külön, letöltheti és telepítse azt hello [SQL Server Express letöltési oldalát](http://www.microsoft.com/web/platform/database.aspx).</span><span class="sxs-lookup"><span data-stu-id="51e84-124">**SQL Server Management Studio Express** - This is included with hello SQL Server 2014 Express with Tools download mentioned above, but if you need tooinstall it separately, you can download and install it from hello [SQL Server Express download page](http://www.microsoft.com/web/platform/database.aspx).</span></span>

<span data-ttu-id="51e84-125">hello oktatóanyag feltételezi, hogy rendelkezik-e az Azure-előfizetésre, hogy a Visual Studio 2013 telepítve van, és, hogy telepítve vagy engedélyezve van a .NET-keretrendszer 3.5.</span><span class="sxs-lookup"><span data-stu-id="51e84-125">hello tutorial assumes that you have an Azure subscription, that you have installed Visual Studio 2013, and that you have installed or enabled .NET Framework 3.5.</span></span> <span data-ttu-id="51e84-126">hello oktatóanyag bemutatja, hogyan a konfigurációkat, amelyek hello Azure hibrid kapcsolatok jól működik az SQL Server 2014 Express tooinstall funkció (egy alapértelmezett példány statikus TCP-port).</span><span class="sxs-lookup"><span data-stu-id="51e84-126">hello tutorial shows you how tooinstall SQL Server 2014 Express in a configuration that works well with hello Azure Hybrid Connections feature (a default instance with a static TCP port).</span></span> <span data-ttu-id="51e84-127">Hello oktatóanyag megkezdése előtt letöltése SQL Server 2014 Express eszközökkel hello fent említett, ha nincs telepítve az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="51e84-127">Before starting hello tutorial, download SQL Server 2014 Express with Tools from hello location mentioned above if you do not have SQL Server installed.</span></span>

### <a name="notes"></a><span data-ttu-id="51e84-128">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="51e84-128">Notes</span></span>
<span data-ttu-id="51e84-129">toouse egy helyi SQL Server vagy SQL Server Express adatbázist egy hibrid kapcsolat, az TCP/IP toobe statikus port engedélyezve van szüksége.</span><span class="sxs-lookup"><span data-stu-id="51e84-129">toouse an on-premises SQL Server or SQL Server Express database with a hybrid connection, TCP/IP needs toobe enabled on a static port.</span></span> <span data-ttu-id="51e84-130">SQL Server alapértelmezett példány statikus 1433-as port használja, mivel elnevezett példányok azonban nem.</span><span class="sxs-lookup"><span data-stu-id="51e84-130">Default instances on SQL Server use static port 1433, whereas named instances do not.</span></span>

<span data-ttu-id="51e84-131">hello számítógép, amelyen hello helyszíni Hybrid Connection Manager ügynököt telepíti:</span><span class="sxs-lookup"><span data-stu-id="51e84-131">hello computer on which you install hello on-premises Hybrid Connection Manager agent:</span></span>

* <span data-ttu-id="51e84-132">Kimenő kapcsolódás tooAzure kell rendelkeznie a keresztül:</span><span class="sxs-lookup"><span data-stu-id="51e84-132">Must have outbound connectivity tooAzure over:</span></span>

| <span data-ttu-id="51e84-133">Port</span><span class="sxs-lookup"><span data-stu-id="51e84-133">Port</span></span> | <span data-ttu-id="51e84-134">miért</span><span class="sxs-lookup"><span data-stu-id="51e84-134">Why</span></span> |
| --- | --- |
| <span data-ttu-id="51e84-135">80</span><span class="sxs-lookup"><span data-stu-id="51e84-135">80</span></span> |<span data-ttu-id="51e84-136">**Szükséges** leellenőrizni a tanúsítvány és választhatóan adatkapcsolattal HTTP-porthoz.</span><span class="sxs-lookup"><span data-stu-id="51e84-136">**Required** for HTTP port for certificate validation and optionally for data connectivity.</span></span> |
| <span data-ttu-id="51e84-137">443</span><span class="sxs-lookup"><span data-stu-id="51e84-137">443</span></span> |<span data-ttu-id="51e84-138">**Nem kötelező** az adatkapcsolat.</span><span class="sxs-lookup"><span data-stu-id="51e84-138">**Optional** for data connectivity.</span></span> <span data-ttu-id="51e84-139">Kimenő kapcsolódás too443 nem érhető el, ha a 80-as TCP-port használatos.</span><span class="sxs-lookup"><span data-stu-id="51e84-139">If outbound connectivity too443 is unavailable, TCP port 80 is used.</span></span> |
| <span data-ttu-id="51e84-140">5671 és 9352</span><span class="sxs-lookup"><span data-stu-id="51e84-140">5671 and 9352</span></span> |<span data-ttu-id="51e84-141">**Ajánlott** , de az adatkapcsolat nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="51e84-141">**Recommended** but Optional for data connectivity.</span></span> <span data-ttu-id="51e84-142">Megjegyzés: Ebben a módban általában nagyobb átviteli teljesítményt eredményez.</span><span class="sxs-lookup"><span data-stu-id="51e84-142">Note this mode usually yields higher throughput.</span></span> <span data-ttu-id="51e84-143">Kimenő kapcsolódás toothese portok nem érhető el, ha a 443-as TCP-port használatos.</span><span class="sxs-lookup"><span data-stu-id="51e84-143">If outbound connectivity toothese ports is unavailable, TCP port 443 is used.</span></span> |

* <span data-ttu-id="51e84-144">Képes tooreach hello kell *állomásnév*:*portszám* a helyi erőforrás.</span><span class="sxs-lookup"><span data-stu-id="51e84-144">Must be able tooreach hello *hostname*:*portnumber* of your on-premises resource.</span></span>

<span data-ttu-id="51e84-145">Ez a cikk hello lépések azt feltételezik, hogy hello helyszíni hibrid kapcsolat ügynököt futtató számítógépről hello hello böngészőt használ.</span><span class="sxs-lookup"><span data-stu-id="51e84-145">hello steps in this article assume that you are using hello browser from hello computer that will host hello on-premises hybrid connection agent.</span></span>

<span data-ttu-id="51e84-146">Ha már van telepítve a konfigurációban, és olyan környezetben, a fent leírt hello feltételeknek megfelelő SQL Server, kihagyhatja azokat, amelyek, és kezdje [egy SQL Server-adatbázis létrehozása a helyszínen](#CreateSQLDB).</span><span class="sxs-lookup"><span data-stu-id="51e84-146">If you already have SQL Server installed in a configuration and in an environment that meets hello conditions described above, you can skip ahead and start with [Create a SQL Server database on-premises](#CreateSQLDB).</span></span>

<a name="InstallSQL"></a>

## <a name="a-install-sql-server-express-enable-tcpip-and-create-a-sql-server-database-on-premises"></a><span data-ttu-id="51e84-147">A.</span><span class="sxs-lookup"><span data-stu-id="51e84-147">A.</span></span> <span data-ttu-id="51e84-148">Telepítse az SQL Server Expresst, engedélyezze a TCP/IP protokollt, és a helyszíni SQL Server-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="51e84-148">Install SQL Server Express, enable TCP/IP, and create a SQL Server database on-premises</span></span>
<span data-ttu-id="51e84-149">Ez a szakasz bemutatja, hogyan tooinstall SQL Server Express, engedélyezze a TCP/IP protokollt, és, hogy a webes alkalmazás működni fog-e hello Azure Portal-adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="51e84-149">This section shows you how tooinstall SQL Server Express, enable TCP/IP, and create a database so that your web application will work with hello Azure Portal.</span></span>

### <a name="install-sql-server-express"></a><span data-ttu-id="51e84-150">SQL Server Express telepítése</span><span class="sxs-lookup"><span data-stu-id="51e84-150">Install SQL Server Express</span></span>
1. <span data-ttu-id="51e84-151">hello futtassa az SQL Server Express tooinstall **SQLEXPRWT_x64_ENU.exe** vagy **SQLEXPR_x86_ENU.exe** letöltött fájlban.</span><span class="sxs-lookup"><span data-stu-id="51e84-151">tooinstall SQL Server Express, run hello **SQLEXPRWT_x64_ENU.exe** or **SQLEXPR_x86_ENU.exe** file that you downloaded.</span></span> <span data-ttu-id="51e84-152">hello SQL Server telepítési központjának varázsló jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="51e84-152">hello SQL Server Installation Center wizard appears.</span></span>
   
    ![SQL-kiszolgáló telepítése][SQLServerInstall]
2. <span data-ttu-id="51e84-154">Válasszon **új SQL Server önálló telepítése vagy szolgáltatások tooan meglévő telepítési hozzáadása**.</span><span class="sxs-lookup"><span data-stu-id="51e84-154">Choose **New SQL Server stand-alone installation or add features tooan existing installation**.</span></span> <span data-ttu-id="51e84-155">Útmutatás alapján hello, elfogadása hello alapértelmezett választási lehetőségek és beállítások, amíg elér toohello **példány konfigurációja** lap.</span><span class="sxs-lookup"><span data-stu-id="51e84-155">Follow hello instructions, accepting hello default choices and settings, until you get toohello **Instance Configuration** page.</span></span>
3. <span data-ttu-id="51e84-156">A hello **példány konfigurációja** lapon, válassza ki **alapértelmezett példány**.</span><span class="sxs-lookup"><span data-stu-id="51e84-156">On hello **Instance Configuration** page, choose **Default instance**.</span></span>
   
    ![Válassza ki az alapértelmezett példány][ChooseDefaultInstance]
   
    <span data-ttu-id="51e84-158">A statikus 1433-as port az SQL Server-ügyfelektől érkező kéréseket figyeli az SQL Server alapértelmezett példányát hello alapértelmezés szerint ez milyen hello szolgáltatáshoz szükséges a hibrid kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="51e84-158">By default, hello default instance of SQL Server listens for requests from SQL Server clients on static port 1433, which is what hello Hybrid Connections feature requires.</span></span> <span data-ttu-id="51e84-159">Elnevezett példányok használata a dinamikus portok és UDP, amely nem támogatja a hibrid kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="51e84-159">Named instances use dynamic ports and UDP, which are not supported by Hybrid Connections.</span></span>
4. <span data-ttu-id="51e84-160">Fogadja el a hello hello alapértelmezett **kiszolgálókonfiguráció** lap.</span><span class="sxs-lookup"><span data-stu-id="51e84-160">Accept hello defaults on hello **Server Configuration** page.</span></span>
5. <span data-ttu-id="51e84-161">A hello **adatbázismotor beállítása** lap **hitelesítési mód**, válassza ki **vegyes üzemmódú (SQL Server-hitelesítést és a Windows-hitelesítés)**, és adja meg a jelszó.</span><span class="sxs-lookup"><span data-stu-id="51e84-161">On hello **Database Engine Configuration** page, under **Authentication Mode**, choose **Mixed Mode (SQL Server authentication and Windows authentication)**, and provide a password.</span></span>
   
    ![Válassza ki a kevert üzemmód][ChooseMixedMode]
   
    <span data-ttu-id="51e84-163">Ebben az oktatóanyagban fog használni az SQL Server-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="51e84-163">In this tutorial, you will be using SQL Server authentication.</span></span> <span data-ttu-id="51e84-164">Lehet, hogy tooremember hello jelszót ad meg, mert később szüksége lesz az.</span><span class="sxs-lookup"><span data-stu-id="51e84-164">Be sure tooremember hello password that you provide, because you will need it later.</span></span>
6. <span data-ttu-id="51e84-165">Hello részeinek hello varázsló toocomplete hello telepítési lépéseit.</span><span class="sxs-lookup"><span data-stu-id="51e84-165">Step through hello rest of hello wizard toocomplete hello installation.</span></span>

### <a name="enable-tcpip"></a><span data-ttu-id="51e84-166">Engedélyezze a TCP/IP protokollt</span><span class="sxs-lookup"><span data-stu-id="51e84-166">Enable TCP/IP</span></span>
<span data-ttu-id="51e84-167">TCP/IP tooenable, szüksége lesz a lett telepítve az SQL Server Express telepítése során az SQL Server Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="51e84-167">tooenable TCP/IP, you will use SQL Server Configuration Manager, which was installed when you installed SQL Server Express.</span></span> <span data-ttu-id="51e84-168">Hello kövesse [engedélyezése TCP/IP hálózati protokollt az SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) folytatása előtt.</span><span class="sxs-lookup"><span data-stu-id="51e84-168">Follow hello steps in [Enable TCP/IP Network Protocol for SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) before continuing.</span></span>

<a name="CreateSQLDB"></a>

### <a name="create-a-sql-server-database-on-premises"></a><span data-ttu-id="51e84-169">A helyszíni SQL Server-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="51e84-169">Create a SQL Server database on-premises</span></span>
<span data-ttu-id="51e84-170">A Visual Studio webalkalmazás az Azure-ban elérhető tagsági adatbázis szükséges.</span><span class="sxs-lookup"><span data-stu-id="51e84-170">Your Visual Studio web application requires a membership database that can be accessed by Azure.</span></span> <span data-ttu-id="51e84-171">Ehhez szükséges, hogy egy SQL Server vagy SQL Server Express adatbázist (nem hello LocalDB adatbázisban, amely az MVC-sablont használ hello alapértelmezés szerint), így hello tagsági adatbázisokból mellett hoz létre.</span><span class="sxs-lookup"><span data-stu-id="51e84-171">This requires a SQL Server or SQL Server Express database (not hello LocalDB database that hello MVC template uses by default), so you'll create hello membership database next.</span></span>

1. <span data-ttu-id="51e84-172">Az SQL Server Management Studio eszközben csatlakozzon a toohello éppen most telepítette az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="51e84-172">In SQL Server Management Studio, connect toohello SQL Server you just installed.</span></span> <span data-ttu-id="51e84-173">(Ha hello **tooServer csatlakozás** párbeszédpanelen nem jelenik meg automatikusan, keresse meg a túl**Object Explorer** hello bal oldali ablaktáblában kattintson **Connect**, és kattintson a **Adatbázis-motor**.) ![Csatlakozás tooServer][SSMSConnectToServer]</span><span class="sxs-lookup"><span data-stu-id="51e84-173">(If hello **Connect tooServer** dialog does not appear automatically, navigate too**Object Explorer** in hello left pane, click **Connect**, and then click **Database Engine**.) ![Connect tooServer][SSMSConnectToServer]</span></span>
   
    <span data-ttu-id="51e84-174">A **kiszolgálótípus**, válassza a **adatbázismotor**.</span><span class="sxs-lookup"><span data-stu-id="51e84-174">For **Server type**, choose **Database Engine**.</span></span> <span data-ttu-id="51e84-175">A **kiszolgálónév**, használhat **localhost** vagy hello számítógép által használt hello nevét.</span><span class="sxs-lookup"><span data-stu-id="51e84-175">For **Server name**, you can use **localhost** or hello name of hello computer that you are using.</span></span> <span data-ttu-id="51e84-176">Válasszon **SQL Server-hitelesítés**, majd jelentkezzen be hello sa felhasználónév és a korábban létrehozott hello jelszót.</span><span class="sxs-lookup"><span data-stu-id="51e84-176">Choose **SQL Server authentication**, and then log in with hello sa user name and hello password that you created earlier.</span></span>
2. <span data-ttu-id="51e84-177">Kattintson jobb gombbal az új adatbázis SQL Server Management Studio használatával toocreate **adatbázisok** az Object Explorer, és kattintson a **új adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="51e84-177">toocreate a new database by using SQL Server Management Studio, right-click **Databases** in Object Explorer, and then click **New Database**.</span></span>
   
    ![Új adatbázis létrehozása][SSMScreateNewDB]
3. <span data-ttu-id="51e84-179">A hello **új adatbázis** párbeszédpanelen adja meg MembershipDB hello adatbázis nevét, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="51e84-179">In hello **New Database** dialog, enter MembershipDB for hello database name, and then click **OK**.</span></span>
   
    ![Adja meg az adatbázis neve][SSMSprovideDBname]
   
    <span data-ttu-id="51e84-181">Vegye figyelembe, hogy nem végzi el a módosításokat toohello adatbázis ezen a ponton.</span><span class="sxs-lookup"><span data-stu-id="51e84-181">Note that you do not make any changes toohello database at this point.</span></span> <span data-ttu-id="51e84-182">hello csoporttagsági információkat megkapja később automatikusan a webes alkalmazás futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="51e84-182">hello membership information will be added automatically later by your web application when you run it.</span></span>
4. <span data-ttu-id="51e84-183">Az Object Explorerben, ha kibontja **adatbázisok**, látni fogja, hogy hello tagsági az adatbázis létrehozását.</span><span class="sxs-lookup"><span data-stu-id="51e84-183">In Object Explorer, if you expand **Databases**, you will see that hello membership database has been created.</span></span>
   
    ![MembershipDB létrehozása][SSMSMembershipDBCreated]

<a name="CreateSite"></a>

## <a name="b-create-a-web-app-in-hello-azure-portal"></a><span data-ttu-id="51e84-185">B.</span><span class="sxs-lookup"><span data-stu-id="51e84-185">B.</span></span> <span data-ttu-id="51e84-186">A webalkalmazás létrehozása az Azure portál hello</span><span class="sxs-lookup"><span data-stu-id="51e84-186">Create a web app in hello Azure Portal</span></span>
> [!NOTE]
> <span data-ttu-id="51e84-187">Ha már létrehozott egy webalkalmazást az Azure portálon megjeleníteni kívánt toouse ebben az oktatóanyagban hello, akkor kihagyhatja azokat, amelyek túl[egy hibrid kapcsolat és a BizTalk szolgáltatás létrehozása](#CreateHC) és továbbra is onnan.</span><span class="sxs-lookup"><span data-stu-id="51e84-187">If you have already created a web app in hello Azure Portal that you want toouse for this tutorial, you can skip ahead too[Create a Hybrid Connection and a BizTalk Service](#CreateHC) and continue from there.</span></span>
> 
> 

1. <span data-ttu-id="51e84-188">A hello [Azure Portal](https://portal.azure.com), kattintson a **új** > **Web + mobil** > **webalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="51e84-188">In hello [Azure Portal](https://portal.azure.com), click **New** > **Web + Mobile** > **Web app**.</span></span>
   
    ![Új gomb][New]
2. <span data-ttu-id="51e84-190">A webalkalmazás konfigurálása, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="51e84-190">Configure your web app, and then click **Create**.</span></span>
   
    ![Webhely neve][WebsiteCreationBlade]
3. <span data-ttu-id="51e84-192">Néhány másodpercen belül hello webalkalmazás jön létre, és a webalkalmazás panelen jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="51e84-192">After a few moments, hello web app is created and its web app blade appears.</span></span> <span data-ttu-id="51e84-193">hello panel függőleges görgethető irányítópultot, amellyel kezelheti a webes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="51e84-193">hello blade is a vertically scrollable dashboard that lets you manage your web app.</span></span>
   
    ![Futtató webhely][WebSiteRunningBlade]
   
    <span data-ttu-id="51e84-195">élő tooverify hello webalkalmazás, rákattinthat hello **Tallózás** ikon toodisplay hello alapértelmezett oldalt.</span><span class="sxs-lookup"><span data-stu-id="51e84-195">tooverify hello web app is live, you can click hello **Browse** icon toodisplay hello default page.</span></span>

<span data-ttu-id="51e84-196">Ezt követően egy hibrid kapcsolat és a BizTalk szolgáltatás hello webalkalmazás hoz létre.</span><span class="sxs-lookup"><span data-stu-id="51e84-196">Next, you will create a hybrid connection and a BizTalk service for hello web app.</span></span>

<a name="CreateHC"></a>

## <a name="c-create-a-hybrid-connection-and-a-biztalk-service"></a><span data-ttu-id="51e84-197">C.</span><span class="sxs-lookup"><span data-stu-id="51e84-197">C.</span></span> <span data-ttu-id="51e84-198">A hibrid kapcsolat és a BizTalk szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="51e84-198">Create a Hybrid Connection and a BizTalk Service</span></span>
1. <span data-ttu-id="51e84-199">Vissza a hello Portal, nyissa meg toosettings és **hálózati** > **hibrid kapcsolati végpontok konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="51e84-199">Back in hello Portal, go toosettings and click **Networking** > **Configure your hybrid connection endpoints**.</span></span>
   
    ![Hibrid kapcsolatok][CreateHCHCIcon]
2. <span data-ttu-id="51e84-201">Hello hibrid kapcsolatok paneljén kattintson **Hozzáadás** > **új hibrid kapcsolat**.</span><span class="sxs-lookup"><span data-stu-id="51e84-201">On hello Hybrid connections blade, click **Add** > **New hybrid connection**.</span></span>
3. <span data-ttu-id="51e84-202">A hello **hibrid kapcsolat létrehozása** panel:</span><span class="sxs-lookup"><span data-stu-id="51e84-202">On hello **Create hybrid connection** blade:</span></span>
   
   * <span data-ttu-id="51e84-203">A **neve**, hello kapcsolat nevét adja meg.</span><span class="sxs-lookup"><span data-stu-id="51e84-203">For **Name**, provide a name for hello connection.</span></span>
   * <span data-ttu-id="51e84-204">A **állomásnév**, az SQL Server számítógép hello számítógép nevének megadását.</span><span class="sxs-lookup"><span data-stu-id="51e84-204">For **Hostname**, enter hello computer name of your SQL Server host computer.</span></span>
   * <span data-ttu-id="51e84-205">A **Port**, adja meg az alapértelmezett 1433-as (hello az SQL Server).</span><span class="sxs-lookup"><span data-stu-id="51e84-205">For **Port**, enter 1433 (hello default port for SQL Server).</span></span>
   * <span data-ttu-id="51e84-206">Kattintson a **BizTalk szolgáltatás** > **új BizTalk szolgáltatás** , és írja be a hello BizTalk szolgáltatás nevét.</span><span class="sxs-lookup"><span data-stu-id="51e84-206">Click **BizTalk Service** > **New BizTalk Service** and enter a name for hello BizTalk service.</span></span>
     
     ![A hibrid kapcsolat létrehozása][TwinCreateHCBlades]
4. <span data-ttu-id="51e84-208">Kattintson a **OK** kétszer.</span><span class="sxs-lookup"><span data-stu-id="51e84-208">Click **OK** twice.</span></span>
   
    <span data-ttu-id="51e84-209">Hello folyamat befejeződésekor hello **értesítések** terület villogjon, egy zöld **sikeres** és hello **a hibrid kapcsolat** panelen megjelenik a hello új hibrid kapcsolat az hello állapotának **nincs csatlakoztatva**.</span><span class="sxs-lookup"><span data-stu-id="51e84-209">When hello process completes, hello **Notifications** area will flash a green **SUCCESS** and hello **Hybrid connection** blade will show hello new hybrid connection with hello status as **Not connected**.</span></span>
   
    ![Egy hibrid kapcsolat létrehozása][CreateHCOneConnectionCreated]

<span data-ttu-id="51e84-211">Ezen a ponton befejeződött hello felhőalapú hibrid kapcsolat infrastruktúra fontos része.</span><span class="sxs-lookup"><span data-stu-id="51e84-211">At this point, you have completed an important part of hello cloud hybrid connection infrastructure.</span></span> <span data-ttu-id="51e84-212">Ezután létrehozhat egy helyszíni megfelelő adat.</span><span class="sxs-lookup"><span data-stu-id="51e84-212">Next, you will create a corresponding on-premises piece.</span></span>

<a name="InstallHCM"></a>

## <a name="d-install-hello-on-premises-hybrid-connection-manager-toocomplete-hello-connection"></a><span data-ttu-id="51e84-213">D.</span><span class="sxs-lookup"><span data-stu-id="51e84-213">D.</span></span> <span data-ttu-id="51e84-214">Hello helyszíni Hybrid Connection Manager toocomplete hello kapcsolat telepítése</span><span class="sxs-lookup"><span data-stu-id="51e84-214">Install hello on-premises Hybrid Connection Manager toocomplete hello connection</span></span>
[!INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

<span data-ttu-id="51e84-215">Most, hogy hello hibrid kapcsolat infrastruktúra befejeződött, létrehozhat egy webes alkalmazás, amely használja ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="51e84-215">Now that hello hybrid connection infrastructure is complete, you will create a web application that uses it.</span></span>

<a name="CreateASPNET"></a>

## <a name="e-create-a-basic-aspnet-web-project-edit-hello-database-connection-string-and-run-hello-project-locally"></a><span data-ttu-id="51e84-216">E.</span><span class="sxs-lookup"><span data-stu-id="51e84-216">E.</span></span> <span data-ttu-id="51e84-217">Hozzon létre egy egyszerű ASP.NET webes projekt hello adatbázis-kapcsolati karakterlánc szerkesztése és hello projekt helyi futtatása</span><span class="sxs-lookup"><span data-stu-id="51e84-217">Create a basic ASP.NET web project, edit hello database connection string, and run hello project locally</span></span>
### <a name="create-a-basic-aspnet-project"></a><span data-ttu-id="51e84-218">Egy egyszerű ASP.NET-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="51e84-218">Create a basic ASP.NET project</span></span>
1. <span data-ttu-id="51e84-219">A Visual Studio, a hello **fájl** menüben hozzon létre egy új projektet:</span><span class="sxs-lookup"><span data-stu-id="51e84-219">In Visual Studio, on hello **File** menu, create a new Project:</span></span>
   
    ![Új Visual Studio-projekt][HCVSNewProject]
2. <span data-ttu-id="51e84-221">A hello **sablonok** hello szakasza **új projekt** párbeszédablakban válassza **webes** , és válassza **ASP.NET Web Application**, és kattintson a  **OK**.</span><span class="sxs-lookup"><span data-stu-id="51e84-221">In hello **Templates** section of hello **New Project** dialog, select **Web** and choose **ASP.NET Web Application**, and then click **OK**.</span></span>
   
    ![ASP.NET webalkalmazás kiválasztása][HCVSChooseASPNET]
3. <span data-ttu-id="51e84-223">A hello **új ASP.NET projekt** párbeszédpanelen válasszon **MVC**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="51e84-223">In hello **New ASP.NET Project** dialog, choose **MVC**, and then click **OK**.</span></span>
   
    ![MVC kiválasztása][HCVSChooseMVC]
4. <span data-ttu-id="51e84-225">Hello projekt létrehozásakor hello alkalmazás információs lap jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="51e84-225">When hello project has been created, hello application readme page appears.</span></span> <span data-ttu-id="51e84-226">Ne futtassa a még hello webes projektet.</span><span class="sxs-lookup"><span data-stu-id="51e84-226">Do not run hello web project yet.</span></span>
   
    ![Információs oldal][HCVSReadmePage]

### <a name="edit-hello-database-connection-string-for-hello-application"></a><span data-ttu-id="51e84-228">Adatbázis-kapcsolati karakterlánc hello a hello alkalmazás szerkesztése</span><span class="sxs-lookup"><span data-stu-id="51e84-228">Edit hello database connection string for hello application</span></span>
<span data-ttu-id="51e84-229">Ebben a lépésben szerkeszteni hello kapcsolati karakterlánc, amely közli az alkalmazás hol toofind a helyi SQL Server Express adatbázist.</span><span class="sxs-lookup"><span data-stu-id="51e84-229">In this step, you edit hello connection string that tells your application where toofind your local SQL Server Express database.</span></span> <span data-ttu-id="51e84-230">hello kapcsolati karakterlánca hello alkalmazás Web.config fájl, amely hello alkalmazás konfigurációs információit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="51e84-230">hello connection string is in hello application's Web.config file, which contains configuration information for hello application.</span></span>

> [!NOTE]
> <span data-ttu-id="51e84-231">az alkalmazás által használt hello adatbázis az SQL Server Express, és nem hello egy Visual Studio alapértelmezett LocalDB létrehozott tooensure, fontos, hogy a projekt futtatása előtt fejezze be ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="51e84-231">tooensure that your application uses hello database that you created in SQL Server Express, and not hello one in Visual Studio's default LocalDB, it is important that you complete this step before running your project.</span></span>
> 
> 

1. <span data-ttu-id="51e84-232">A Megoldáskezelőben kattintson duplán a hello Web.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="51e84-232">In Solution Explorer, double-click hello Web.config file.</span></span>
   
    ![Web.config][HCVSChooseWebConfig]
2. <span data-ttu-id="51e84-234">Hello szerkesztése **connectionStrings** szakasz toopoint toohello SQL Server-adatbázisban a helyi gépén, a következő példa hello hello szintaxist a következő:</span><span class="sxs-lookup"><span data-stu-id="51e84-234">Edit hello **connectionStrings** section toopoint toohello SQL Server database on your local machine, following hello syntax in hello following example:</span></span>
   
    ![Kapcsolati karakterlánc][HCVSConnectionString]
   
    <span data-ttu-id="51e84-236">Hello kapcsolati karakterlánc szerkesztésekor tartsa szem előtt tartva hello következő:</span><span class="sxs-lookup"><span data-stu-id="51e84-236">When composing hello connection string, keep in mind hello following:</span></span>
   
   * <span data-ttu-id="51e84-237">Ha megnevezett példány helyett egy alapértelmezett példány (például YourServer\SQLEXPRESS) tooa kapcsolódik, konfigurálnia kell az SQL-kiszolgáló toouse statikus port.</span><span class="sxs-lookup"><span data-stu-id="51e84-237">If you are connecting tooa named instance instead of a default instance (for example, YourServer\SQLEXPRESS), you must configure your SQL Server toouse static ports.</span></span> <span data-ttu-id="51e84-238">A statikus használt portok konfigurálásáról további információkért lásd: [hogyan tooconfigure SQL Server toolisten egy adott portot](http://support.microsoft.com/kb/823938).</span><span class="sxs-lookup"><span data-stu-id="51e84-238">For information on configuring static ports, see [How tooconfigure SQL Server toolisten on a specific port](http://support.microsoft.com/kb/823938).</span></span> <span data-ttu-id="51e84-239">Alapértelmezés szerint a elnevezett példányok UDP és dinamikus portok, amely nem támogatja a hibrid kapcsolatok használatára.</span><span class="sxs-lookup"><span data-stu-id="51e84-239">By default, named instances use UDP and dynamic ports, which are not supported by Hybrid Connections.</span></span>
   * <span data-ttu-id="51e84-240">Javasoljuk, hogy megadja a hello port hello kapcsolati karakterlánc (hello példa látható alapértelmezés szerint 1433), így biztosíthatja, hogy a helyi SQL Server TCP engedélyezve van, és hello megfelelő portot használ.</span><span class="sxs-lookup"><span data-stu-id="51e84-240">It is recommended that you specify hello port (1433 by default, as shown in hello example) on hello connection string so that you can be sure that your local SQL Server has TCP enabled and is using hello correct port.</span></span>
   * <span data-ttu-id="51e84-241">Ne felejtse el toouse SQL Server-hitelesítés tooconnect hello Felhasználóazonosító és jelszó megadásával a kapcsolati karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="51e84-241">Remember toouse SQL Server Authentication tooconnect, specifying hello user ID and password in your connection string.</span></span>
3. <span data-ttu-id="51e84-242">Kattintson a **mentése** Visual Studio toosave hello Web.config fájlban.</span><span class="sxs-lookup"><span data-stu-id="51e84-242">Click **Save** in Visual Studio toosave hello Web.config file.</span></span>

### <a name="run-hello-project-locally-and-register-a-new-user"></a><span data-ttu-id="51e84-243">Futtassa helyileg a hello projektet, és regisztrálni egy új felhasználót</span><span class="sxs-lookup"><span data-stu-id="51e84-243">Run hello project locally and register a new user</span></span>
1. <span data-ttu-id="51e84-244">Futtassa az új webes projekt helyi csoport hibakeresési hello Tallózás gombra kattintva.</span><span class="sxs-lookup"><span data-stu-id="51e84-244">Now, run your new web project locally by clicking hello browse button under Debug.</span></span> <span data-ttu-id="51e84-245">Ez a példa az Internet Explorerben.</span><span class="sxs-lookup"><span data-stu-id="51e84-245">This example uses Internet Explorer.</span></span>
   
    ![Futtassa a projekt][HCVSRunProject]
2. <span data-ttu-id="51e84-247">Az hello jobb felső sarkában hello alapértelmezett weblapot, majd válassza **regisztrálása** tooregister egy új fiókot:</span><span class="sxs-lookup"><span data-stu-id="51e84-247">On hello upper right of hello default web page, choose **Register** tooregister a new account:</span></span>
   
    ![Egy új fiók regisztrálása][HCVSRegisterLocally]
3. <span data-ttu-id="51e84-249">Adjon meg egy felhasználónevet és jelszót:</span><span class="sxs-lookup"><span data-stu-id="51e84-249">Enter a user name and password:</span></span>
   
    ![Adja meg a felhasználónevet és jelszót][HCVSCreateNewAccount]
   
    <span data-ttu-id="51e84-251">Ez automatikusan létrehoz egy adatbázis SQL-kiszolgálón a helyi tároló hello csoporttagsági információkat az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="51e84-251">This automatically creates a database on your local SQL Server that holds hello membership information for your application.</span></span> <span data-ttu-id="51e84-252">Hello tábla (**dbo. AspNetUsers**) tartás webes alkalmazás felhasználói hitelesítő adatok, például hello azokat az előbb hozzáadott.</span><span class="sxs-lookup"><span data-stu-id="51e84-252">One of hello tables (**dbo.AspNetUsers**) holds web app user credentials like hello ones that you just entered.</span></span> <span data-ttu-id="51e84-253">Ez a táblázat hello oktatóanyag későbbi részében jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="51e84-253">You will see this table later in hello tutorial.</span></span>
4. <span data-ttu-id="51e84-254">Zárja be a böngészőablakot hello hello alapértelmezett weblap.</span><span class="sxs-lookup"><span data-stu-id="51e84-254">Close hello browser window of hello default web page.</span></span> <span data-ttu-id="51e84-255">Ezzel leállítja a hello alkalmazást a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="51e84-255">This stops hello application in Visual Studio.</span></span>

<span data-ttu-id="51e84-256">Most már készen áll a hello következő lépésre, amely toopublish hello alkalmazás tooAzure és tesztelik azt.</span><span class="sxs-lookup"><span data-stu-id="51e84-256">You are now ready for hello next step, which is toopublish hello application tooAzure and test it.</span></span>

<a name="PubNTest"></a>

## <a name="f-publish-hello-web-application-tooazure-and-test-it"></a><span data-ttu-id="51e84-257">F.</span><span class="sxs-lookup"><span data-stu-id="51e84-257">F.</span></span> <span data-ttu-id="51e84-258">Hello webes alkalmazás tooAzure közzététele és tesztelik azt</span><span class="sxs-lookup"><span data-stu-id="51e84-258">Publish hello web application tooAzure and test it</span></span>
<span data-ttu-id="51e84-259">Most, lesz az alkalmazás tooyour App Service webalkalmazás közzététele, és korábban konfigurált hello hibrid kapcsolat Mitől toosee tesztelése alatt használt tooconnect a webes alkalmazás toohello adatbázis a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="51e84-259">Now, you'll publish your application tooyour App Service web app and then test it toosee how hello hybrid connection you configured earlier is being used tooconnect your web app toohello database on your local machine.</span></span>

### <a name="publish-hello-web-application"></a><span data-ttu-id="51e84-260">Hello webes alkalmazás közzététele</span><span class="sxs-lookup"><span data-stu-id="51e84-260">Publish hello web application</span></span>
1. <span data-ttu-id="51e84-261">A közzétételi profil hello App Service webalkalmazás az Azure portál hello töltheti le.</span><span class="sxs-lookup"><span data-stu-id="51e84-261">You can download your publishing profile for hello App Service web app in hello Azure Portal.</span></span> <span data-ttu-id="51e84-262">A webalkalmazás hello paneljén kattintson **Get közzétételi profil**, majd mentse a hello fájl tooyour számítógép.</span><span class="sxs-lookup"><span data-stu-id="51e84-262">On hello blade for your web app, click **Get publish profile**, and then save hello file tooyour computer.</span></span>
   
    ![Közzétételi profil letöltése][PortalDownloadPublishProfile]
   
    <span data-ttu-id="51e84-264">A következő importálni ezt a fájlt a Visual Studio web alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="51e84-264">Next, you will import this file into your Visual Studio web application.</span></span>
2. <span data-ttu-id="51e84-265">A Visual Studióban, kattintson a jobb gombbal a hello projekt nevére a Megoldáskezelőben, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="51e84-265">In Visual Studio, right-click hello project name in Solution Explorer and select **Publish**.</span></span>
   
    ![Válassza ki közzététele][HCVSRightClickProjectSelectPublish]
3. <span data-ttu-id="51e84-267">A hello **webhely közzététele** párbeszédpanel, hello **profil** lapra, majd **importálási**.</span><span class="sxs-lookup"><span data-stu-id="51e84-267">In hello **Publish Web** dialog, on hello **Profile** tab, choose **Import**.</span></span>
   
    ![Importálás][HCVSPublishWebDialogImport]
4. <span data-ttu-id="51e84-269">Tallózás tooyour letöltött közzétételi profil, válassza ki azt, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="51e84-269">Browse tooyour downloaded publishing profile, select it, and then click **OK**.</span></span>
   
    ![Keresse meg a tooprofile][HCVSBrowseToImportPubProfile]
5. <span data-ttu-id="51e84-271">A közzétételi információkat importálja, és megjeleníti a hello **kapcsolat** hello párbeszédpanel lapján.</span><span class="sxs-lookup"><span data-stu-id="51e84-271">Your publishing information is imported and displays on hello **Connection** tab of hello dialog.</span></span>
   
    ![Kattintson a közzététel][HCVSClickPublish]
   
    <span data-ttu-id="51e84-273">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="51e84-273">Click **Publish**.</span></span>
   
    <span data-ttu-id="51e84-274">Közzététel befejezése után a böngésző elindul, és a már ismerős ASP.NET-alkalmazás – megjelenítése, azzal a különbséggel, hogy mostantól az Azure-felhőbe hello élő!</span><span class="sxs-lookup"><span data-stu-id="51e84-274">When publishing completes, your browser will launch and show your now familiar ASP.NET application -- except that now it is live in hello Azure cloud!</span></span>

<span data-ttu-id="51e84-275">A következő fogja használni az élő webes alkalmazás toosee a hibrid kapcsolat működés közben.</span><span class="sxs-lookup"><span data-stu-id="51e84-275">Next, you will use your live web application toosee its Hybrid Connection in action.</span></span>

### <a name="test-hello-completed-web-application-on-azure"></a><span data-ttu-id="51e84-276">Teszt hello Azure webalkalmazás befejeződött</span><span class="sxs-lookup"><span data-stu-id="51e84-276">Test hello completed web application on Azure</span></span>
1. <span data-ttu-id="51e84-277">Hello felső sarkában található weblap az Azure-on, válassza ki **jelentkezzen be**.</span><span class="sxs-lookup"><span data-stu-id="51e84-277">On hello top right of your web page on Azure, choose **Log in**.</span></span>
   
    ![A teszt-napló][HCTestLogIn]
2. <span data-ttu-id="51e84-279">Az App Service webalkalmazás már csatlakoztatott tooyour webes alkalmazás tagsági adatbázisokból a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="51e84-279">Your App Service web app is now connected tooyour web application's membership database on your local machine.</span></span> <span data-ttu-id="51e84-280">tooverify e, jelentkezzen be a korábbi adatbázis-hitelesítő adatokkal, hogy a megadott hello helyi hello.</span><span class="sxs-lookup"><span data-stu-id="51e84-280">tooverify this, log in with hello same credentials that you entered in hello local database earlier.</span></span>
   
    ![Hello üdvözlőlap][HCTestHelloContoso]
3. <span data-ttu-id="51e84-282">toofurther az új hibrid kapcsolat tesztelése, jelentkezzen ki az Azure-webalkalmazás, és regisztrálja egy másik felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="51e84-282">toofurther test your new hybrid connection, log off of your Azure web application and register as another user.</span></span> <span data-ttu-id="51e84-283">Adjon meg egy új felhasználónevet és jelszót, és kattintson a **regisztrálása**.</span><span class="sxs-lookup"><span data-stu-id="51e84-283">Provide a new user name and password, and then click **Register**.</span></span>
   
    ![Teszt regisztrálja egy másik felhasználó][HCTestRegisterRelecloud]
4. <span data-ttu-id="51e84-285">tooverify, hogy hello új felhasználói hitelesítő adatok a hibrid kapcsolaton keresztül a helyi adatbázisban tárolja a helyi számítógépen nyissa meg az SQL Management Studio.</span><span class="sxs-lookup"><span data-stu-id="51e84-285">tooverify that hello new user's credentials have been stored in your local database through your hybrid connection, open SQL Management Studio on your local computer.</span></span> <span data-ttu-id="51e84-286">Az Object Explorerben bontsa ki a hello **MembershipDB** adatbázisából, és végül **táblák**.</span><span class="sxs-lookup"><span data-stu-id="51e84-286">In Object Explorer, expand hello **MembershipDB** database, and then expand **Tables**.</span></span> <span data-ttu-id="51e84-287">Kattintson a jobb gombbal hello **dbo. AspNetUsers** tagsági tábla, és válassza a **legfelső 1000 sor kiválasztása** tooview hello eredmények.</span><span class="sxs-lookup"><span data-stu-id="51e84-287">Right-click hello **dbo.AspNetUsers** membership table and choose **Select Top 1000 Rows** tooview hello results.</span></span>
   
    ![Hello eredmények megtekintése][HCTestSSMSTree]
5. <span data-ttu-id="51e84-289">A helyi csoporttagság most táblázat mindkét fiókok – hello helyileg létre, és egy, az Azure-felhőbe hello létrehozott hello.</span><span class="sxs-lookup"><span data-stu-id="51e84-289">Your local membership table now shows both accounts - hello one that you created locally, and hello one that you created in hello Azure cloud.</span></span> <span data-ttu-id="51e84-290">hello felhőben létre hello Azure hibrid kapcsolat funkción keresztül tooyour a helyi adatbázis mentése megtörtént.</span><span class="sxs-lookup"><span data-stu-id="51e84-290">hello one that you created in hello cloud has been saved tooyour on-premises database through Azure's Hybrid Connection feature.</span></span>
   
    ![A helyszíni adatbázisban regisztrált felhasználók][HCTestShowMemberDb]

<span data-ttu-id="51e84-292">Most már létrehozta és telepítve az ASP.NET webalkalmazások által használt egy hibrid kapcsolat egy webalkalmazást az Azure-felhőbe hello és a helyszíni SQL Server-adatbázis között.</span><span class="sxs-lookup"><span data-stu-id="51e84-292">You have now created and deployed an ASP.NET web application that uses a hybrid connection between a web app in hello Azure cloud and an on-premises SQL Server database.</span></span> <span data-ttu-id="51e84-293">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="51e84-293">Congratulations!</span></span>

## <a name="see-also"></a><span data-ttu-id="51e84-294">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="51e84-294">See Also</span></span>
[<span data-ttu-id="51e84-295">A hibrid kapcsolatok áttekintése</span><span class="sxs-lookup"><span data-stu-id="51e84-295">Hybrid Connections overview</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[<span data-ttu-id="51e84-296">Josh a jelölés vezet be a hibrid kapcsolatok (videó a Channel 9)</span><span class="sxs-lookup"><span data-stu-id="51e84-296">Josh Twist introduces hybrid connections (Channel 9 video)</span></span>](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[<span data-ttu-id="51e84-297">A hibrid kapcsolatok áttekintése</span><span class="sxs-lookup"><span data-stu-id="51e84-297">Hybrid Connections overview</span></span>](/services/biztalk-services/)

[<span data-ttu-id="51e84-298">BizTalk szolgáltatások: Irányítópult, a figyelő, a méretezés, a konfigurálás és a hibrid kapcsolat lapokon</span><span class="sxs-lookup"><span data-stu-id="51e84-298">BizTalk Services: Dashboard, Monitor, Scale, Configure, and Hybrid Connection tabs</span></span>](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[<span data-ttu-id="51e84-299">Zökkenőmentes alkalmazások hordozhatóságát (videó a Channel 9) egy valós hibrid felhő felépítése</span><span class="sxs-lookup"><span data-stu-id="51e84-299">Building a Real-World Hybrid Cloud with Seamless Application Portability (Channel 9 video)</span></span>](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[<span data-ttu-id="51e84-300">Hozzáférés a helyszíni erőforrások hibrid kapcsolatok használata az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="51e84-300">Access on-premises resources using hybrid connections in Azure App Service</span></span>](web-sites-hybrid-connection-get-started.md)

[<span data-ttu-id="51e84-301">ASP.NET identitás áttekintése</span><span class="sxs-lookup"><span data-stu-id="51e84-301">ASP.NET Identity Overview</span></span>](http://www.asp.net/identity)

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

<!-- IMAGES -->
[SQLServerInstall]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A01SQLServerInstall.png
[ChooseDefaultInstance]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A02ChooseDefaultInstance.png
[ChooseMixedMode]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A03ChooseMixedMode.png
[SSMSConnectToServer]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A04SSMSConnectToServer.png
[SSMScreateNewDB]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A05SSMScreateNewDBlh.png
[SSMSprovideDBname]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A06SSMSprovideDBname.png
[SSMSMembershipDBCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/A07SSMSMembershipDBCreated.png
[New]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B01New.png
[NewWebsite]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B02NewWebsite.png
[WebsiteCreationBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B03WebsiteCreationBlade.png
[WebSiteRunningBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B04WebSiteRunningBlade.png
[Browse]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B05Browse.png
[DefaultWebSitePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/B06DefaultWebSitePage.png
[CreateHCHCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C01CreateHCHCIcon.png
[CreateHCAddHC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C02CreateHCAddHC.png
[TwinCreateHCBlades]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C03TwinCreateHCBlades.png
[CreateHCCreateBTS]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C04CreateHCCreateBTS.png
[CreateBTScomplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C05CreateBTScomplete.png
[CreateHCSuccessNotification]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C06CreateHCSuccessNotification.png
[CreateHCOneConnectionCreated]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/C07CreateHCOneConnectionCreated.png
[HCIcon]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D01HCIcon.png
[NotConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D02NotConnected.png
[NotConnectedBlade]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D03NotConnectedBlade.png
[ClickListenerSetup]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D04ClickListenerSetup.png
[ClickToInstallHCM]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D05ClickToInstallHCM.png
[ApplicationRunWarning]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D06ApplicationRunWarning.png
[UAC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D07UAC.png
[HCMInstalling]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D08HCMInstalling.png
[HCMInstallComplete]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D09HCMInstallComplete.png
[HCStatusConnected]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/D10HCStatusConnected.png
[HCVSNewProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E01HCVSNewProject.png
[HCVSChooseASPNET]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E02HCVSChooseASPNET.png
[HCVSChooseMVC]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E03HCVSChooseMVC.png
[HCVSReadmePage]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E04HCVSReadmePage.png
[HCVSChooseWebConfig]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E05HCVSChooseWebConfig.png
[HCVSConnectionString]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSConnectionString.png
[HCVSRunProject]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E06HCVSRunProject.png
[HCVSRegisterLocally]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E07HCVSRegisterLocally.png
[HCVSCreateNewAccount]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/E08HCVSCreateNewAccount.png
[PortalDownloadPublishProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F01PortalDownloadPublishProfile.png
[HCVSPublishProfileInDownloadsFolder]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F02HCVSPublishProfileInDownloadsFolder.png
[HCVSRightClickProjectSelectPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F03HCVSRightClickProjectSelectPublish.png
[HCVSPublishWebDialogImport]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F04HCVSPublishWebDialogImport.png
[HCVSBrowseToImportPubProfile]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F05HCVSBrowseToImportPubProfile.png
[HCVSClickPublish]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F06HCVSClickPublish.png
[HCTestLogIn]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F07HCTestLogIn.png
[HCTestHelloContoso]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F08HCTestHelloContoso.png
[HCTestRegisterRelecloud]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F09HCTestRegisterRelecloud.png
[HCTestSSMSTree]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F10HCTestSSMSTree.png
[HCTestShowMemberDb]:./media/web-sites-hybrid-connection-connect-on-premises-sql-server/F11HCTestShowMemberDb.png
