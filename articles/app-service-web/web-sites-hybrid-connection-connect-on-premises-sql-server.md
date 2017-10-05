---
title: "Csatlakoztassa a helyszíni SQL Server egy webalkalmazást az Azure App Service hibrid kapcsolatok használata"
description: "A Microsoft Azure-webalkalmazás létrehozása, és csatlakoztassa a helyszíni SQL Server-adatbázis"
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
ms.openlocfilehash: 12456ef3e2aecfa7a03cca97de2ff6ffd9602357
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="connect-to-on-premises-sql-server-from-a-web-app-in-azure-app-service-using-hybrid-connections"></a><span data-ttu-id="3daa5-103">Csatlakoztassa a helyszíni SQL Server egy webalkalmazást az Azure App Service hibrid kapcsolatok használata</span><span class="sxs-lookup"><span data-stu-id="3daa5-103">Connect to on-premises SQL Server from a web app in Azure App Service using Hybrid Connections</span></span>
<span data-ttu-id="3daa5-104">Hibrid kapcsolatok kapcsolódhatnak [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps statikus TCP-port használatára a helyszíni erőforrásokhoz.</span><span class="sxs-lookup"><span data-stu-id="3daa5-104">Hybrid Connections can connect [Azure App Service](http://go.microsoft.com/fwlink/?LinkId=529714) Web Apps to on-premises resources that use a static TCP port.</span></span> <span data-ttu-id="3daa5-105">Támogatott erőforrások közé tartoznak a Microsoft SQL Server, a MySQL, a HTTP webes API-k, a App Service és a legtöbb egyéni webszolgáltatások.</span><span class="sxs-lookup"><span data-stu-id="3daa5-105">Supported resources include Microsoft SQL Server, MySQL, HTTP Web APIs, App Service, and most custom Web Services.</span></span>

<span data-ttu-id="3daa5-106">Ebből az oktatóanyagból megtudhatja hogyan egy App Service webalkalmazás létrehozása a [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), a webes alkalmazás csatlakoztatása az új hibrid kapcsolat funkciójával helyi helyszíni SQL Server-adatbázis, hozzon létre egy egyszerű ASP.NET-alkalmazást, amely a hibrid kapcsolat használatát, és telepítse központilag az alkalmazást az App Service webalkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="3daa5-106">In this tutorial, you will learn how to create an App Service web app in the [Azure Portal](http://go.microsoft.com/fwlink/?LinkId=529715), connect the web app to your local on-premises SQL Server database using the new Hybrid Connection feature, create a simple ASP.NET application that will use the hybrid connection, and deploy the application to the App Service web app.</span></span> <span data-ttu-id="3daa5-107">Az elkészült webalkalmazás az Azure felhasználói hitelesítő adatokat, amely a helyszíni tagsági adatbázisban tárolja.</span><span class="sxs-lookup"><span data-stu-id="3daa5-107">The completed web app on Azure stores user credentials in a membership database that is on-premises.</span></span> <span data-ttu-id="3daa5-108">Az oktatóanyag azt feltételezi, hogy nincs korábbi tapasztalata az Azure vagy az ASP.NET használatával kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="3daa5-108">The tutorial assumes no prior experience using Azure or ASP.NET.</span></span>

> [!NOTE]
> <span data-ttu-id="3daa5-109">Ha az Azure App Service-t az Azure-fiók regisztrálása előtt szeretné kipróbálni, ugorjon [Az Azure App Service kipróbálása](https://azure.microsoft.com/try/app-service/) oldalra. Itt azonnal létrehozhat egy ideiglenes, kezdő szintű webalkalmazást az App Service szolgáltatásban.</span><span class="sxs-lookup"><span data-stu-id="3daa5-109">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="3daa5-110">Ehhez nincs szükség bankkártyára, és nem jár kötelezettségekkel.</span><span class="sxs-lookup"><span data-stu-id="3daa5-110">No credit cards required; no commitments.</span></span>
> 
> <span data-ttu-id="3daa5-111">A Web Apps része a hibrid kapcsolatok szolgáltatás csak a érhető el a [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3daa5-111">The Web Apps portion of the Hybrid Connections feature is available only in the [Azure Portal](https://portal.azure.com).</span></span> <span data-ttu-id="3daa5-112">BizTalk szolgáltatások VPN-kapcsolat létrehozásához lásd: [hibrid kapcsolatok](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span><span class="sxs-lookup"><span data-stu-id="3daa5-112">To create a connection in BizTalk Services, see [Hybrid Connections](http://go.microsoft.com/fwlink/p/?LinkID=397274).</span></span>  
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="3daa5-113">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="3daa5-113">Prerequisites</span></span>
<span data-ttu-id="3daa5-114">Az oktatóanyag teljesítéséhez szüksége lesz a következő termékek.</span><span class="sxs-lookup"><span data-stu-id="3daa5-114">To complete this tutorial, you'll need the following products.</span></span> <span data-ttu-id="3daa5-115">Az összes szabad verziók, elérhetők nekiláthat teljes mértékben az Azure ingyenes.</span><span class="sxs-lookup"><span data-stu-id="3daa5-115">All are available in free versions, so you can start developing for Azure entirely for free.</span></span>

* <span data-ttu-id="3daa5-116">**Azure-előfizetés** - ingyenes előfizetésre, lásd: [Azure ingyenes próbaverzió](/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="3daa5-116">**Azure subscription** - For a free subscription, see [Azure Free Trial](/pricing/free-trial/).</span></span>
* <span data-ttu-id="3daa5-117">**A Visual Studio 2013** – a Visual Studio 2013 ingyenes próbaverziójának letöltéséhez lásd: [Visual Studio letöltések](http://www.visualstudio.com/downloads/download-visual-studio-vs).</span><span class="sxs-lookup"><span data-stu-id="3daa5-117">**Visual Studio 2013** - To download a free trial version of Visual Studio 2013, see [Visual Studio Downloads](http://www.visualstudio.com/downloads/download-visual-studio-vs).</span></span> <span data-ttu-id="3daa5-118">Telepítse a folytatás előtt.</span><span class="sxs-lookup"><span data-stu-id="3daa5-118">Install this before continuing.</span></span>
* <span data-ttu-id="3daa5-119">**A Microsoft .NET Framework 3.5 Service Pack 1** – Ha az operációs rendszer Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7, vagy Windows Server 2008 R2, engedélyezheti a Vezérlőpult > Programok és szolgáltatások > Windows-szolgáltatások be- és kikapcsolása.</span><span class="sxs-lookup"><span data-stu-id="3daa5-119">**Microsoft .NET Framework 3.5 Service Pack 1** - If your operating system is Windows 8.1, Windows Server 2012 R2, Windows 8, Windows Server 2012, Windows 7, or Windows Server 2008 R2, you can enable this in Control Panel > Programs and Features > Turn Windows features on or off.</span></span> <span data-ttu-id="3daa5-120">Ellenkező esetben töltheti le a [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).</span><span class="sxs-lookup"><span data-stu-id="3daa5-120">Otherwise, you can download it from the [Microsoft Download Center](http://www.microsoft.com/download/en/details.aspx?displaylang=en&id=22).</span></span>
* <span data-ttu-id="3daa5-121">**SQL Server 2014 Express eszközökkel** -Microsoft SQL Server Express ingyenesen letölthető a [Microsoft Web Platform adatbázissal kapcsolatos beállításainak lapja](http://www.microsoft.com/web/platform/database.aspx).</span><span class="sxs-lookup"><span data-stu-id="3daa5-121">**SQL Server 2014 Express with Tools** - download Microsoft SQL Server Express for free at the [Microsoft Web Platform Database page](http://www.microsoft.com/web/platform/database.aspx).</span></span> <span data-ttu-id="3daa5-122">Válassza ki a **Express** (nem LocalDB) verziója.</span><span class="sxs-lookup"><span data-stu-id="3daa5-122">Choose the **Express** (not LocalDB) version.</span></span> <span data-ttu-id="3daa5-123">A **eszközökkel Express** tartalmazza az ebben az oktatóanyagban használandó SQL Server Management Studio.</span><span class="sxs-lookup"><span data-stu-id="3daa5-123">The **Express with Tools** version includes SQL Server Management Studio, which you will use in this tutorial.</span></span>
* <span data-ttu-id="3daa5-124">**SQL Server Management Studio Express** – Ez tartalmazza az SQL Server 2014 Express a fent említett eszközök letöltése, de ha külön telepíteni kell, töltse le és telepítse innen a [SQL Server Express letöltési oldala](http://www.microsoft.com/web/platform/database.aspx).</span><span class="sxs-lookup"><span data-stu-id="3daa5-124">**SQL Server Management Studio Express** - This is included with the SQL Server 2014 Express with Tools download mentioned above, but if you need to install it separately, you can download and install it from the [SQL Server Express download page](http://www.microsoft.com/web/platform/database.aspx).</span></span>

<span data-ttu-id="3daa5-125">Az oktatóanyag feltételezi, hogy rendelkezik-e az Azure-előfizetésre, hogy a Visual Studio 2013 telepítve van, és, hogy telepítve vagy engedélyezve van a .NET-keretrendszer 3.5.</span><span class="sxs-lookup"><span data-stu-id="3daa5-125">The tutorial assumes that you have an Azure subscription, that you have installed Visual Studio 2013, and that you have installed or enabled .NET Framework 3.5.</span></span> <span data-ttu-id="3daa5-126">Az oktatóanyag bemutatja, hogyan telepítse az SQL Server 2014 Express konfiguráció esetén, amely az Azure hibrid kapcsolatok funkció (egy alapértelmezett példány statikus TCP-port) megfelelően működik.</span><span class="sxs-lookup"><span data-stu-id="3daa5-126">The tutorial shows you how to install SQL Server 2014 Express in a configuration that works well with the Azure Hybrid Connections feature (a default instance with a static TCP port).</span></span> <span data-ttu-id="3daa5-127">Az oktatóanyag megkezdése előtt letöltése SQL Server 2014 Express eszközökkel a fent említett, ha nincs telepítve az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3daa5-127">Before starting the tutorial, download SQL Server 2014 Express with Tools from the location mentioned above if you do not have SQL Server installed.</span></span>

### <a name="notes"></a><span data-ttu-id="3daa5-128">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="3daa5-128">Notes</span></span>
<span data-ttu-id="3daa5-129">A helyszíni SQL Server vagy SQL Server Express adatbázis használata hibrid kapcsolaton keresztül, TCP/IP kell engedélyezni a statikus port.</span><span class="sxs-lookup"><span data-stu-id="3daa5-129">To use an on-premises SQL Server or SQL Server Express database with a hybrid connection, TCP/IP needs to be enabled on a static port.</span></span> <span data-ttu-id="3daa5-130">SQL Server alapértelmezett példány statikus 1433-as port használja, mivel elnevezett példányok azonban nem.</span><span class="sxs-lookup"><span data-stu-id="3daa5-130">Default instances on SQL Server use static port 1433, whereas named instances do not.</span></span>

<span data-ttu-id="3daa5-131">A számítógép, amelyen a helyszíni Hybrid Connection Manager ügynököt telepíti:</span><span class="sxs-lookup"><span data-stu-id="3daa5-131">The computer on which you install the on-premises Hybrid Connection Manager agent:</span></span>

* <span data-ttu-id="3daa5-132">Rendelkeznie kell Azure kimenő kapcsolódás keresztül:</span><span class="sxs-lookup"><span data-stu-id="3daa5-132">Must have outbound connectivity to Azure over:</span></span>

| <span data-ttu-id="3daa5-133">Port</span><span class="sxs-lookup"><span data-stu-id="3daa5-133">Port</span></span> | <span data-ttu-id="3daa5-134">miért</span><span class="sxs-lookup"><span data-stu-id="3daa5-134">Why</span></span> |
| --- | --- |
| <span data-ttu-id="3daa5-135">80</span><span class="sxs-lookup"><span data-stu-id="3daa5-135">80</span></span> |<span data-ttu-id="3daa5-136">**Szükséges** leellenőrizni a tanúsítvány és választhatóan adatkapcsolattal HTTP-porthoz.</span><span class="sxs-lookup"><span data-stu-id="3daa5-136">**Required** for HTTP port for certificate validation and optionally for data connectivity.</span></span> |
| <span data-ttu-id="3daa5-137">443</span><span class="sxs-lookup"><span data-stu-id="3daa5-137">443</span></span> |<span data-ttu-id="3daa5-138">**Nem kötelező** az adatkapcsolat.</span><span class="sxs-lookup"><span data-stu-id="3daa5-138">**Optional** for data connectivity.</span></span> <span data-ttu-id="3daa5-139">Ha a 443-as kimenő kapcsolat nem érhető el, 80-as TCP-portot használja.</span><span class="sxs-lookup"><span data-stu-id="3daa5-139">If outbound connectivity to 443 is unavailable, TCP port 80 is used.</span></span> |
| <span data-ttu-id="3daa5-140">5671 és 9352</span><span class="sxs-lookup"><span data-stu-id="3daa5-140">5671 and 9352</span></span> |<span data-ttu-id="3daa5-141">**Ajánlott** , de az adatkapcsolat nem kötelező.</span><span class="sxs-lookup"><span data-stu-id="3daa5-141">**Recommended** but Optional for data connectivity.</span></span> <span data-ttu-id="3daa5-142">Megjegyzés: Ebben a módban általában nagyobb átviteli teljesítményt eredményez.</span><span class="sxs-lookup"><span data-stu-id="3daa5-142">Note this mode usually yields higher throughput.</span></span> <span data-ttu-id="3daa5-143">Ha nem érhető el kimenő kapcsolódás ezeket a portokat, 443-as TCP-portot használja.</span><span class="sxs-lookup"><span data-stu-id="3daa5-143">If outbound connectivity to these ports is unavailable, TCP port 443 is used.</span></span> |

* <span data-ttu-id="3daa5-144">Kell tudni érnie a *állomásnév*:*portszám* a helyi erőforrás.</span><span class="sxs-lookup"><span data-stu-id="3daa5-144">Must be able to reach the *hostname*:*portnumber* of your on-premises resource.</span></span>

<span data-ttu-id="3daa5-145">Ebben a cikkben szereplő lépések azt feltételezik, hogy a számítógépről, amely üzemelteti a helyszíni hibrid kapcsolat ügynök böngészőt használ.</span><span class="sxs-lookup"><span data-stu-id="3daa5-145">The steps in this article assume that you are using the browser from the computer that will host the on-premises hybrid connection agent.</span></span>

<span data-ttu-id="3daa5-146">Ha már van telepítve a konfigurációban, és egy környezetben, amely megfelel a fenti körülmények között SQL Server, kihagyhatja azokat, amelyek, és kezdje [egy SQL Server-adatbázis létrehozása a helyszínen](#CreateSQLDB).</span><span class="sxs-lookup"><span data-stu-id="3daa5-146">If you already have SQL Server installed in a configuration and in an environment that meets the conditions described above, you can skip ahead and start with [Create a SQL Server database on-premises](#CreateSQLDB).</span></span>

<a name="InstallSQL"></a>

## <a name="a-install-sql-server-express-enable-tcpip-and-create-a-sql-server-database-on-premises"></a><span data-ttu-id="3daa5-147">A.</span><span class="sxs-lookup"><span data-stu-id="3daa5-147">A.</span></span> <span data-ttu-id="3daa5-148">Telepítse az SQL Server Expresst, engedélyezze a TCP/IP protokollt, és a helyszíni SQL Server-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="3daa5-148">Install SQL Server Express, enable TCP/IP, and create a SQL Server database on-premises</span></span>
<span data-ttu-id="3daa5-149">Ez a szakasz bemutatja, hogyan telepíti az SQL Server Expresst, engedélyezze a TCP/IP protokollt, és hozzon létre egy adatbázist, így a webes alkalmazás működni fog-e az Azure portál.</span><span class="sxs-lookup"><span data-stu-id="3daa5-149">This section shows you how to install SQL Server Express, enable TCP/IP, and create a database so that your web application will work with the Azure Portal.</span></span>

### <a name="install-sql-server-express"></a><span data-ttu-id="3daa5-150">SQL Server Express telepítése</span><span class="sxs-lookup"><span data-stu-id="3daa5-150">Install SQL Server Express</span></span>
1. <span data-ttu-id="3daa5-151">SQL Server Express telepítéséhez futtassa a **SQLEXPRWT_x64_ENU.exe** vagy **SQLEXPR_x86_ENU.exe** letöltött fájlban.</span><span class="sxs-lookup"><span data-stu-id="3daa5-151">To install SQL Server Express, run the **SQLEXPRWT_x64_ENU.exe** or **SQLEXPR_x86_ENU.exe** file that you downloaded.</span></span> <span data-ttu-id="3daa5-152">Az SQL Server telepítési központjának varázsló jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3daa5-152">The SQL Server Installation Center wizard appears.</span></span>
   
    ![SQL-kiszolgáló telepítése][SQLServerInstall]
2. <span data-ttu-id="3daa5-154">Válasszon **új SQL Server önálló telepítése vagy szolgáltatások hozzáadása egy meglévő telepítéshez**.</span><span class="sxs-lookup"><span data-stu-id="3daa5-154">Choose **New SQL Server stand-alone installation or add features to an existing installation**.</span></span> <span data-ttu-id="3daa5-155">Kövesse az utasításokat, elfogadja az alapértelmezett választási lehetőségek és beállítások, amíg elér a **példány konfigurációja** lap.</span><span class="sxs-lookup"><span data-stu-id="3daa5-155">Follow the instructions, accepting the default choices and settings, until you get to the **Instance Configuration** page.</span></span>
3. <span data-ttu-id="3daa5-156">Az a **példány konfigurációja** lapon, válassza ki **alapértelmezett példány**.</span><span class="sxs-lookup"><span data-stu-id="3daa5-156">On the **Instance Configuration** page, choose **Default instance**.</span></span>
   
    ![Válassza ki az alapértelmezett példány][ChooseDefaultInstance]
   
    <span data-ttu-id="3daa5-158">A statikus 1433-as port az SQL Server-ügyfelektől érkező kéréseket figyeli az SQL Server alapértelmezett példányát alapértelmezés szerint ez a hibrid kapcsolatok szolgáltatás szükséges.</span><span class="sxs-lookup"><span data-stu-id="3daa5-158">By default, the default instance of SQL Server listens for requests from SQL Server clients on static port 1433, which is what the Hybrid Connections feature requires.</span></span> <span data-ttu-id="3daa5-159">Elnevezett példányok használata a dinamikus portok és UDP, amely nem támogatja a hibrid kapcsolatok.</span><span class="sxs-lookup"><span data-stu-id="3daa5-159">Named instances use dynamic ports and UDP, which are not supported by Hybrid Connections.</span></span>
4. <span data-ttu-id="3daa5-160">Fogadja el az alapértelmezett a a **kiszolgálókonfiguráció** lap.</span><span class="sxs-lookup"><span data-stu-id="3daa5-160">Accept the defaults on the **Server Configuration** page.</span></span>
5. <span data-ttu-id="3daa5-161">Az a **adatbázismotor beállítása** lap **hitelesítési mód**, válassza a **vegyes üzemmódú (SQL Server-hitelesítést és a Windows-hitelesítés)**, és adjon meg egy jelszót.</span><span class="sxs-lookup"><span data-stu-id="3daa5-161">On the **Database Engine Configuration** page, under **Authentication Mode**, choose **Mixed Mode (SQL Server authentication and Windows authentication)**, and provide a password.</span></span>
   
    ![Válassza ki a kevert üzemmód][ChooseMixedMode]
   
    <span data-ttu-id="3daa5-163">Ebben az oktatóanyagban fog használni az SQL Server-hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="3daa5-163">In this tutorial, you will be using SQL Server authentication.</span></span> <span data-ttu-id="3daa5-164">Ne felejtse el a jelszót ad meg, mert később szüksége lesz az.</span><span class="sxs-lookup"><span data-stu-id="3daa5-164">Be sure to remember the password that you provide, because you will need it later.</span></span>
6. <span data-ttu-id="3daa5-165">A továbbiakban a telepítés befejezéséhez a varázsló lépéseit.</span><span class="sxs-lookup"><span data-stu-id="3daa5-165">Step through the rest of the wizard to complete the installation.</span></span>

### <a name="enable-tcpip"></a><span data-ttu-id="3daa5-166">Engedélyezze a TCP/IP protokollt</span><span class="sxs-lookup"><span data-stu-id="3daa5-166">Enable TCP/IP</span></span>
<span data-ttu-id="3daa5-167">Engedélyezze a TCP/IP protokollt, szüksége lesz lett telepítve az SQL Server Express telepítése során az SQL Server Configuration Manager.</span><span class="sxs-lookup"><span data-stu-id="3daa5-167">To enable TCP/IP, you will use SQL Server Configuration Manager, which was installed when you installed SQL Server Express.</span></span> <span data-ttu-id="3daa5-168">Kövesse a [engedélyezése TCP/IP hálózati protokollt az SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) folytatása előtt.</span><span class="sxs-lookup"><span data-stu-id="3daa5-168">Follow the steps in [Enable TCP/IP Network Protocol for SQL Server](http://technet.microsoft.com/library/hh231672%28v=sql.110%29.aspx) before continuing.</span></span>

<a name="CreateSQLDB"></a>

### <a name="create-a-sql-server-database-on-premises"></a><span data-ttu-id="3daa5-169">A helyszíni SQL Server-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="3daa5-169">Create a SQL Server database on-premises</span></span>
<span data-ttu-id="3daa5-170">A Visual Studio webalkalmazás az Azure-ban elérhető tagsági adatbázis szükséges.</span><span class="sxs-lookup"><span data-stu-id="3daa5-170">Your Visual Studio web application requires a membership database that can be accessed by Azure.</span></span> <span data-ttu-id="3daa5-171">Ehhez szükséges, hogy egy SQL Server vagy SQL Server Express adatbázist (nem a LocalDB adatbázisban, amely alapértelmezés szerint az MVC-sablont használ), így hoz létre, a tagsági adatbázisokból mellett.</span><span class="sxs-lookup"><span data-stu-id="3daa5-171">This requires a SQL Server or SQL Server Express database (not the LocalDB database that the MVC template uses by default), so you'll create the membership database next.</span></span>

1. <span data-ttu-id="3daa5-172">A SQL Server Management Studio eszközben csatlakozzon az éppen most telepítette az SQL Server.</span><span class="sxs-lookup"><span data-stu-id="3daa5-172">In SQL Server Management Studio, connect to the SQL Server you just installed.</span></span> <span data-ttu-id="3daa5-173">(Ha a **kapcsolódás a kiszolgálóhoz** párbeszédpanelen nem jelenik meg automatikusan, navigáljon a **Object Explorer** a bal oldali ablaktáblán kattintson a **Connect**, és kattintson a **adatbázismotor**.) ![Csatlakozás kiszolgálóhoz][SSMSConnectToServer]</span><span class="sxs-lookup"><span data-stu-id="3daa5-173">(If the **Connect to Server** dialog does not appear automatically, navigate to **Object Explorer** in the left pane, click **Connect**, and then click **Database Engine**.) ![Connect to Server][SSMSConnectToServer]</span></span>
   
    <span data-ttu-id="3daa5-174">A **kiszolgálótípus**, válassza a **adatbázismotor**.</span><span class="sxs-lookup"><span data-stu-id="3daa5-174">For **Server type**, choose **Database Engine**.</span></span> <span data-ttu-id="3daa5-175">A **kiszolgálónév**, használhat **localhost** vagy az Ön által használt számítógép nevét.</span><span class="sxs-lookup"><span data-stu-id="3daa5-175">For **Server name**, you can use **localhost** or the name of the computer that you are using.</span></span> <span data-ttu-id="3daa5-176">Válasszon **SQL Server-hitelesítés**, majd jelentkezzen be a rendszergazdai felhasználónevet és a korábban létrehozott jelszót.</span><span class="sxs-lookup"><span data-stu-id="3daa5-176">Choose **SQL Server authentication**, and then log in with the sa user name and the password that you created earlier.</span></span>
2. <span data-ttu-id="3daa5-177">Az SQL Server Management Studio használatával hozzon létre egy új adatbázist, kattintson a jobb gombbal **adatbázisok** az Object Explorer, és kattintson a **új adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="3daa5-177">To create a new database by using SQL Server Management Studio, right-click **Databases** in Object Explorer, and then click **New Database**.</span></span>
   
    ![Új adatbázis létrehozása][SSMScreateNewDB]
3. <span data-ttu-id="3daa5-179">A a **új adatbázis** párbeszédpanelen adja meg MembershipDB az adatbázis nevét, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="3daa5-179">In the **New Database** dialog, enter MembershipDB for the database name, and then click **OK**.</span></span>
   
    ![Adja meg az adatbázis neve][SSMSprovideDBname]
   
    <span data-ttu-id="3daa5-181">Vegye figyelembe, hogy nem módosíthatja az adatbázis ezen a ponton.</span><span class="sxs-lookup"><span data-stu-id="3daa5-181">Note that you do not make any changes to the database at this point.</span></span> <span data-ttu-id="3daa5-182">A tagsági információ bekerül később automatikusan a webes alkalmazás futtatásakor.</span><span class="sxs-lookup"><span data-stu-id="3daa5-182">The membership information will be added automatically later by your web application when you run it.</span></span>
4. <span data-ttu-id="3daa5-183">Az Object Explorerben, ha kibontja **adatbázisok**, látni fogja, hogy a tagsági adatbázisokból létrejött-e.</span><span class="sxs-lookup"><span data-stu-id="3daa5-183">In Object Explorer, if you expand **Databases**, you will see that the membership database has been created.</span></span>
   
    ![MembershipDB létrehozása][SSMSMembershipDBCreated]

<a name="CreateSite"></a>

## <a name="b-create-a-web-app-in-the-azure-portal"></a><span data-ttu-id="3daa5-185">B.</span><span class="sxs-lookup"><span data-stu-id="3daa5-185">B.</span></span> <span data-ttu-id="3daa5-186">Webalkalmazás létrehozása az Azure portálon</span><span class="sxs-lookup"><span data-stu-id="3daa5-186">Create a web app in the Azure Portal</span></span>
> [!NOTE]
> <span data-ttu-id="3daa5-187">Ha már létrehozott egy webalkalmazást az Azure portálon, hogy a jelen oktatóanyag használni kívánt, ugorjon előre [egy hibrid kapcsolat és a BizTalk szolgáltatás létrehozása](#CreateHC) és továbbra is onnan.</span><span class="sxs-lookup"><span data-stu-id="3daa5-187">If you have already created a web app in the Azure Portal that you want to use for this tutorial, you can skip ahead to [Create a Hybrid Connection and a BizTalk Service](#CreateHC) and continue from there.</span></span>
> 
> 

1. <span data-ttu-id="3daa5-188">Az a [Azure Portal](https://portal.azure.com), kattintson a **új** > **Web + mobil** > **webalkalmazás**.</span><span class="sxs-lookup"><span data-stu-id="3daa5-188">In the [Azure Portal](https://portal.azure.com), click **New** > **Web + Mobile** > **Web app**.</span></span>
   
    ![Új gomb][New]
2. <span data-ttu-id="3daa5-190">A webalkalmazás konfigurálása, és kattintson a **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="3daa5-190">Configure your web app, and then click **Create**.</span></span>
   
    ![Webhely neve][WebsiteCreationBlade]
3. <span data-ttu-id="3daa5-192">Néhány másodpercen belül a web app jön létre, és a webalkalmazás panelen jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3daa5-192">After a few moments, the web app is created and its web app blade appears.</span></span> <span data-ttu-id="3daa5-193">A panel függőleges görgethető irányítópultot, amellyel kezelheti a webes alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3daa5-193">The blade is a vertically scrollable dashboard that lets you manage your web app.</span></span>
   
    ![Futtató webhely][WebSiteRunningBlade]
   
    <span data-ttu-id="3daa5-195">Annak ellenőrzéséhez, hogy a webalkalmazás élő, kattintson a **Tallózás** ikonra kattintva jelenítse meg az alapértelmezett lapon.</span><span class="sxs-lookup"><span data-stu-id="3daa5-195">To verify the web app is live, you can click the **Browse** icon to display the default page.</span></span>

<span data-ttu-id="3daa5-196">A következő létrehozhat egy hibrid kapcsolat és a BizTalk szolgáltatás a webalkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3daa5-196">Next, you will create a hybrid connection and a BizTalk service for the web app.</span></span>

<a name="CreateHC"></a>

## <a name="c-create-a-hybrid-connection-and-a-biztalk-service"></a><span data-ttu-id="3daa5-197">C.</span><span class="sxs-lookup"><span data-stu-id="3daa5-197">C.</span></span> <span data-ttu-id="3daa5-198">A hibrid kapcsolat és a BizTalk szolgáltatás létrehozása</span><span class="sxs-lookup"><span data-stu-id="3daa5-198">Create a Hybrid Connection and a BizTalk Service</span></span>
1. <span data-ttu-id="3daa5-199">Vissza a portálon, válassza a beállítások, és kattintson a **hálózati** > **hibrid kapcsolati végpontok konfigurálása**.</span><span class="sxs-lookup"><span data-stu-id="3daa5-199">Back in the Portal, go to settings and click **Networking** > **Configure your hybrid connection endpoints**.</span></span>
   
    ![Hibrid kapcsolatok][CreateHCHCIcon]
2. <span data-ttu-id="3daa5-201">A hibrid kapcsolatok paneljén kattintson **Hozzáadás** > **új hibrid kapcsolat**.</span><span class="sxs-lookup"><span data-stu-id="3daa5-201">On the Hybrid connections blade, click **Add** > **New hybrid connection**.</span></span>
3. <span data-ttu-id="3daa5-202">Az a **hibrid kapcsolat létrehozása** panel:</span><span class="sxs-lookup"><span data-stu-id="3daa5-202">On the **Create hybrid connection** blade:</span></span>
   
   * <span data-ttu-id="3daa5-203">A **neve**, adja meg a kapcsolat nevét.</span><span class="sxs-lookup"><span data-stu-id="3daa5-203">For **Name**, provide a name for the connection.</span></span>
   * <span data-ttu-id="3daa5-204">A **állomásnév**, az SQL Server számítógép a számítógép nevének megadását.</span><span class="sxs-lookup"><span data-stu-id="3daa5-204">For **Hostname**, enter the computer name of your SQL Server host computer.</span></span>
   * <span data-ttu-id="3daa5-205">A **Port**, adja meg az alapértelmezett 1433-as (az SQL Server).</span><span class="sxs-lookup"><span data-stu-id="3daa5-205">For **Port**, enter 1433 (the default port for SQL Server).</span></span>
   * <span data-ttu-id="3daa5-206">Kattintson a **BizTalk szolgáltatás** > **új BizTalk szolgáltatás** , és írja be a BizTalk szolgáltatás nevét.</span><span class="sxs-lookup"><span data-stu-id="3daa5-206">Click **BizTalk Service** > **New BizTalk Service** and enter a name for the BizTalk service.</span></span>
     
     ![A hibrid kapcsolat létrehozása][TwinCreateHCBlades]
4. <span data-ttu-id="3daa5-208">Kattintson a **OK** kétszer.</span><span class="sxs-lookup"><span data-stu-id="3daa5-208">Click **OK** twice.</span></span>
   
    <span data-ttu-id="3daa5-209">A folyamat befejezése után a **értesítések** terület villogjon, egy zöld **sikeres** és a **a hibrid kapcsolat** panelen jelennek meg az új hibrid kapcsolat a a állapotának **nincs csatlakoztatva**.</span><span class="sxs-lookup"><span data-stu-id="3daa5-209">When the process completes, the **Notifications** area will flash a green **SUCCESS** and the **Hybrid connection** blade will show the new hybrid connection with the status as **Not connected**.</span></span>
   
    ![Egy hibrid kapcsolat létrehozása][CreateHCOneConnectionCreated]

<span data-ttu-id="3daa5-211">Fontos része a hibrid kapcsolat felhőalapú infrastruktúra ezen a ponton befejeződött.</span><span class="sxs-lookup"><span data-stu-id="3daa5-211">At this point, you have completed an important part of the cloud hybrid connection infrastructure.</span></span> <span data-ttu-id="3daa5-212">Ezután létrehozhat egy helyszíni megfelelő adat.</span><span class="sxs-lookup"><span data-stu-id="3daa5-212">Next, you will create a corresponding on-premises piece.</span></span>

<a name="InstallHCM"></a>

## <a name="d-install-the-on-premises-hybrid-connection-manager-to-complete-the-connection"></a><span data-ttu-id="3daa5-213">D.</span><span class="sxs-lookup"><span data-stu-id="3daa5-213">D.</span></span> <span data-ttu-id="3daa5-214">A kapcsolat a helyszíni hibrid Csatlakozáskezelő telepítése</span><span class="sxs-lookup"><span data-stu-id="3daa5-214">Install the on-premises Hybrid Connection Manager to complete the connection</span></span>
[!INCLUDE [app-service-hybrid-connections-manager-install](../../includes/app-service-hybrid-connections-manager-install.md)]

<span data-ttu-id="3daa5-215">Most, hogy a hibrid kapcsolat infrastruktúra befejeződött, létrehozhat egy webes alkalmazás, amely használja ezt a szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="3daa5-215">Now that the hybrid connection infrastructure is complete, you will create a web application that uses it.</span></span>

<a name="CreateASPNET"></a>

## <a name="e-create-a-basic-aspnet-web-project-edit-the-database-connection-string-and-run-the-project-locally"></a><span data-ttu-id="3daa5-216">E.</span><span class="sxs-lookup"><span data-stu-id="3daa5-216">E.</span></span> <span data-ttu-id="3daa5-217">Hozzon létre egy egyszerű ASP.NET webes projekt, az adatbázis-kapcsolati karakterlánc szerkesztése és a projekt helyi futtatása</span><span class="sxs-lookup"><span data-stu-id="3daa5-217">Create a basic ASP.NET web project, edit the database connection string, and run the project locally</span></span>
### <a name="create-a-basic-aspnet-project"></a><span data-ttu-id="3daa5-218">Egy egyszerű ASP.NET-projekt létrehozása</span><span class="sxs-lookup"><span data-stu-id="3daa5-218">Create a basic ASP.NET project</span></span>
1. <span data-ttu-id="3daa5-219">A Visual Studio a a **fájl** menüben hozzon létre egy új projektet:</span><span class="sxs-lookup"><span data-stu-id="3daa5-219">In Visual Studio, on the **File** menu, create a new Project:</span></span>
   
    ![Új Visual Studio-projekt][HCVSNewProject]
2. <span data-ttu-id="3daa5-221">A a **sablonok** szakasza a **új projekt** párbeszédablakban válassza **webes** válassza **ASP.NET Web Application**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="3daa5-221">In the **Templates** section of the **New Project** dialog, select **Web** and choose **ASP.NET Web Application**, and then click **OK**.</span></span>
   
    ![ASP.NET webalkalmazás kiválasztása][HCVSChooseASPNET]
3. <span data-ttu-id="3daa5-223">Az a **új ASP.NET projekt** párbeszédpanelen válasszon **MVC**, és kattintson a **OK**.</span><span class="sxs-lookup"><span data-stu-id="3daa5-223">In the **New ASP.NET Project** dialog, choose **MVC**, and then click **OK**.</span></span>
   
    ![MVC kiválasztása][HCVSChooseMVC]
4. <span data-ttu-id="3daa5-225">A projekt létrehozása után az alkalmazás információs lap jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="3daa5-225">When the project has been created, the application readme page appears.</span></span> <span data-ttu-id="3daa5-226">Nem működnek a webes projekt még.</span><span class="sxs-lookup"><span data-stu-id="3daa5-226">Do not run the web project yet.</span></span>
   
    ![Információs oldal][HCVSReadmePage]

### <a name="edit-the-database-connection-string-for-the-application"></a><span data-ttu-id="3daa5-228">Az adatbázis-kapcsolati karakterlánc az alkalmazás szerkesztése</span><span class="sxs-lookup"><span data-stu-id="3daa5-228">Edit the database connection string for the application</span></span>
<span data-ttu-id="3daa5-229">Ebben a lépésben szerkeszteni a kapcsolati karakterlánc, amely közli az alkalmazás hol található a helyi SQL Server Express adatbázisban.</span><span class="sxs-lookup"><span data-stu-id="3daa5-229">In this step, you edit the connection string that tells your application where to find your local SQL Server Express database.</span></span> <span data-ttu-id="3daa5-230">A kapcsolati karakterlánc: az alkalmazás Web.config fájl, amely az alkalmazás konfigurációs információit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="3daa5-230">The connection string is in the application's Web.config file, which contains configuration information for the application.</span></span>

> [!NOTE]
> <span data-ttu-id="3daa5-231">Győződjön meg arról, hogy az alkalmazás használja az adatbázis az SQL Server Express, és nem a Visual Studio alapértelmezett LocalDB létrehozott, fontos, hogy a projekt futtatása előtt fejezze be ezt a lépést.</span><span class="sxs-lookup"><span data-stu-id="3daa5-231">To ensure that your application uses the database that you created in SQL Server Express, and not the one in Visual Studio's default LocalDB, it is important that you complete this step before running your project.</span></span>
> 
> 

1. <span data-ttu-id="3daa5-232">A Megoldáskezelőben kattintson duplán a Web.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="3daa5-232">In Solution Explorer, double-click the Web.config file.</span></span>
   
    ![Web.config][HCVSChooseWebConfig]
2. <span data-ttu-id="3daa5-234">Szerkessze a **connectionStrings** mutasson az SQL Server-adatbázis, a helyi számítógépen, a szintaxis a következő példában a következő szakaszban:</span><span class="sxs-lookup"><span data-stu-id="3daa5-234">Edit the **connectionStrings** section to point to the SQL Server database on your local machine, following the syntax in the following example:</span></span>
   
    ![Kapcsolati karakterlánc][HCVSConnectionString]
   
    <span data-ttu-id="3daa5-236">A kapcsolati karakterlánc szerkesztésekor vegye figyelembe a következőket:</span><span class="sxs-lookup"><span data-stu-id="3daa5-236">When composing the connection string, keep in mind the following:</span></span>
   
   * <span data-ttu-id="3daa5-237">Ha egy alapértelmezett példány (például YourServer\SQLEXPRESS) helyett egy megnevezett példányt csatlakozik, konfigurálnia kell az SQL Server statikus port használatára.</span><span class="sxs-lookup"><span data-stu-id="3daa5-237">If you are connecting to a named instance instead of a default instance (for example, YourServer\SQLEXPRESS), you must configure your SQL Server to use static ports.</span></span> <span data-ttu-id="3daa5-238">A statikus használt portok konfigurálásáról további információkért lásd: [konfigurálása egy adott portot az SQL Server](http://support.microsoft.com/kb/823938).</span><span class="sxs-lookup"><span data-stu-id="3daa5-238">For information on configuring static ports, see [How to configure SQL Server to listen on a specific port](http://support.microsoft.com/kb/823938).</span></span> <span data-ttu-id="3daa5-239">Alapértelmezés szerint a elnevezett példányok UDP és dinamikus portok, amely nem támogatja a hibrid kapcsolatok használatára.</span><span class="sxs-lookup"><span data-stu-id="3daa5-239">By default, named instances use UDP and dynamic ports, which are not supported by Hybrid Connections.</span></span>
   * <span data-ttu-id="3daa5-240">Javasoljuk, hogy megadja a port (a példában látható módon alapértelmezés szerint 1433) a kapcsolati karakterlánc, hogy akkor is ügyeljen arra, hogy a helyi SQL Server TCP engedélyezve van, és a megfelelő portot használ.</span><span class="sxs-lookup"><span data-stu-id="3daa5-240">It is recommended that you specify the port (1433 by default, as shown in the example) on the connection string so that you can be sure that your local SQL Server has TCP enabled and is using the correct port.</span></span>
   * <span data-ttu-id="3daa5-241">Fontos, hogy az SQL Server-hitelesítés szeretne csatlakozni, a felhasználói azonosító és jelszó megadásával a kapcsolati karakterláncban.</span><span class="sxs-lookup"><span data-stu-id="3daa5-241">Remember to use SQL Server Authentication to connect, specifying the user ID and password in your connection string.</span></span>
3. <span data-ttu-id="3daa5-242">Kattintson a **mentése** a Visual Studio menteni a Web.config fájlt.</span><span class="sxs-lookup"><span data-stu-id="3daa5-242">Click **Save** in Visual Studio to save the Web.config file.</span></span>

### <a name="run-the-project-locally-and-register-a-new-user"></a><span data-ttu-id="3daa5-243">A projekt helyi futtatása, és egy új felhasználó regisztrálása</span><span class="sxs-lookup"><span data-stu-id="3daa5-243">Run the project locally and register a new user</span></span>
1. <span data-ttu-id="3daa5-244">Futtassa az új webes projekt helyi csoport hibakeresési a Tallózás gombra kattintva.</span><span class="sxs-lookup"><span data-stu-id="3daa5-244">Now, run your new web project locally by clicking the browse button under Debug.</span></span> <span data-ttu-id="3daa5-245">Ez a példa az Internet Explorerben.</span><span class="sxs-lookup"><span data-stu-id="3daa5-245">This example uses Internet Explorer.</span></span>
   
    ![Futtassa a projekt][HCVSRunProject]
2. <span data-ttu-id="3daa5-247">A képernyő jobb felső sarkában az alapértelmezett webes lapra, válassza **regisztrálása** regisztrálni egy új fiókot:</span><span class="sxs-lookup"><span data-stu-id="3daa5-247">On the upper right of the default web page, choose **Register** to register a new account:</span></span>
   
    ![Egy új fiók regisztrálása][HCVSRegisterLocally]
3. <span data-ttu-id="3daa5-249">Adjon meg egy felhasználónevet és jelszót:</span><span class="sxs-lookup"><span data-stu-id="3daa5-249">Enter a user name and password:</span></span>
   
    ![Adja meg a felhasználónevet és jelszót][HCVSCreateNewAccount]
   
    <span data-ttu-id="3daa5-251">Ez automatikusan létrehoz egy adatbázis a helyi SQL-kiszolgálón, amely tárolja a csoporttagsági információkat az alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3daa5-251">This automatically creates a database on your local SQL Server that holds the membership information for your application.</span></span> <span data-ttu-id="3daa5-252">A tábla (**dbo. AspNetUsers**) tartás webes alkalmazás felhasználói hitelesítő adatokat, mint a csak a megadott.</span><span class="sxs-lookup"><span data-stu-id="3daa5-252">One of the tables (**dbo.AspNetUsers**) holds web app user credentials like the ones that you just entered.</span></span> <span data-ttu-id="3daa5-253">Ebben a táblázatban láthatja az oktatóanyag későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="3daa5-253">You will see this table later in the tutorial.</span></span>
4. <span data-ttu-id="3daa5-254">Zárja be a böngészőablakot, az alapértelmezett weblap.</span><span class="sxs-lookup"><span data-stu-id="3daa5-254">Close the browser window of the default web page.</span></span> <span data-ttu-id="3daa5-255">Ezzel leállítja a Visual Studio alkalmazás.</span><span class="sxs-lookup"><span data-stu-id="3daa5-255">This stops the application in Visual Studio.</span></span>

<span data-ttu-id="3daa5-256">Most már készen áll a következő lépéshez, és tegye közzé az alkalmazást az Azure-ba, és tesztelik azt.</span><span class="sxs-lookup"><span data-stu-id="3daa5-256">You are now ready for the next step, which is to publish the application to Azure and test it.</span></span>

<a name="PubNTest"></a>

## <a name="f-publish-the-web-application-to-azure-and-test-it"></a><span data-ttu-id="3daa5-257">F.</span><span class="sxs-lookup"><span data-stu-id="3daa5-257">F.</span></span> <span data-ttu-id="3daa5-258">Tegye közzé a webalkalmazást az Azure-ba, és tesztelik azt</span><span class="sxs-lookup"><span data-stu-id="3daa5-258">Publish the web application to Azure and test it</span></span>
<span data-ttu-id="3daa5-259">Most lesz az alkalmazás az App Service webalkalmazásba közzétételére, és ellenőrizze, hogy kiderüljön, a hibrid kapcsolat korábban konfigurált használatának adatbázishoz való kapcsolódáshoz a webes alkalmazás a helyi gépen.</span><span class="sxs-lookup"><span data-stu-id="3daa5-259">Now, you'll publish your application to your App Service web app and then test it to see how the hybrid connection you configured earlier is being used to connect your web app to the database on your local machine.</span></span>

### <a name="publish-the-web-application"></a><span data-ttu-id="3daa5-260">Tegye közzé a webalkalmazást</span><span class="sxs-lookup"><span data-stu-id="3daa5-260">Publish the web application</span></span>
1. <span data-ttu-id="3daa5-261">A közzétételi profil az App Service webalkalmazás az Azure portálon töltheti le.</span><span class="sxs-lookup"><span data-stu-id="3daa5-261">You can download your publishing profile for the App Service web app in the Azure Portal.</span></span> <span data-ttu-id="3daa5-262">A webalkalmazás paneljén kattintson **Get közzétételi profil**, és mentse a fájlt a számítógépre.</span><span class="sxs-lookup"><span data-stu-id="3daa5-262">On the blade for your web app, click **Get publish profile**, and then save the file to your computer.</span></span>
   
    ![Közzétételi profil letöltése][PortalDownloadPublishProfile]
   
    <span data-ttu-id="3daa5-264">A következő importálni ezt a fájlt a Visual Studio web alkalmazásba.</span><span class="sxs-lookup"><span data-stu-id="3daa5-264">Next, you will import this file into your Visual Studio web application.</span></span>
2. <span data-ttu-id="3daa5-265">A Visual Studióban, kattintson a jobb gombbal a projekt nevére a Megoldáskezelőben, és válassza ki **közzététel**.</span><span class="sxs-lookup"><span data-stu-id="3daa5-265">In Visual Studio, right-click the project name in Solution Explorer and select **Publish**.</span></span>
   
    ![Válassza ki közzététele][HCVSRightClickProjectSelectPublish]
3. <span data-ttu-id="3daa5-267">Az a **webhely közzététele** párbeszédpanelen, a a **profil** lapra, majd **importálási**.</span><span class="sxs-lookup"><span data-stu-id="3daa5-267">In the **Publish Web** dialog, on the **Profile** tab, choose **Import**.</span></span>
   
    ![Importálás][HCVSPublishWebDialogImport]
4. <span data-ttu-id="3daa5-269">Keresse meg a letöltött közzétételi profilt, válassza ki azt, és kattintson **OK**.</span><span class="sxs-lookup"><span data-stu-id="3daa5-269">Browse to your downloaded publishing profile, select it, and then click **OK**.</span></span>
   
    ![Keresse meg a profil][HCVSBrowseToImportPubProfile]
5. <span data-ttu-id="3daa5-271">A közzétételi információkat importálja, és megjeleníti a **kapcsolat** párbeszédpanel lapján.</span><span class="sxs-lookup"><span data-stu-id="3daa5-271">Your publishing information is imported and displays on the **Connection** tab of the dialog.</span></span>
   
    ![Kattintson a közzététel][HCVSClickPublish]
   
    <span data-ttu-id="3daa5-273">Kattintson a **Publish** (Közzététel) gombra.</span><span class="sxs-lookup"><span data-stu-id="3daa5-273">Click **Publish**.</span></span>
   
    <span data-ttu-id="3daa5-274">Közzététel befejezése után a böngésző elindul, és a már ismerős ASP.NET-alkalmazás – megjelenítése, azzal a különbséggel, hogy most már az élő Azure felhőben!</span><span class="sxs-lookup"><span data-stu-id="3daa5-274">When publishing completes, your browser will launch and show your now familiar ASP.NET application -- except that now it is live in the Azure cloud!</span></span>

<span data-ttu-id="3daa5-275">A következő élő webalkalmazásokat használatával fog látni a hibrid kapcsolatot, a művelet.</span><span class="sxs-lookup"><span data-stu-id="3daa5-275">Next, you will use your live web application to see its Hybrid Connection in action.</span></span>

### <a name="test-the-completed-web-application-on-azure"></a><span data-ttu-id="3daa5-276">Az elkészült webalkalmazás az Azure-on tesztelése</span><span class="sxs-lookup"><span data-stu-id="3daa5-276">Test the completed web application on Azure</span></span>
1. <span data-ttu-id="3daa5-277">A felső sarkában található weblap az Azure-on, válassza ki **jelentkezzen be**.</span><span class="sxs-lookup"><span data-stu-id="3daa5-277">On the top right of your web page on Azure, choose **Log in**.</span></span>
   
    ![A teszt-napló][HCTestLogIn]
2. <span data-ttu-id="3daa5-279">Az App Service webalkalmazásba most csatlakozik a webes alkalmazás tagsági adatbázisokból a helyi számítógépen.</span><span class="sxs-lookup"><span data-stu-id="3daa5-279">Your App Service web app is now connected to your web application's membership database on your local machine.</span></span> <span data-ttu-id="3daa5-280">Ennek ellenőrzéséhez jelentkezzen be ugyanazon hitelesítő adatokkal, a helyi adatbázis korábban megadott.</span><span class="sxs-lookup"><span data-stu-id="3daa5-280">To verify this, log in with the same credentials that you entered in the local database earlier.</span></span>
   
    ![Hello üdvözlőlap][HCTestHelloContoso]
3. <span data-ttu-id="3daa5-282">Az új hibrid kapcsolat további teszteléséhez jelentkezzen ki az Azure-webalkalmazás, és regisztrálja egy másik felhasználóként.</span><span class="sxs-lookup"><span data-stu-id="3daa5-282">To further test your new hybrid connection, log off of your Azure web application and register as another user.</span></span> <span data-ttu-id="3daa5-283">Adjon meg egy új felhasználónevet és jelszót, és kattintson a **regisztrálása**.</span><span class="sxs-lookup"><span data-stu-id="3daa5-283">Provide a new user name and password, and then click **Register**.</span></span>
   
    ![Teszt regisztrálja egy másik felhasználó][HCTestRegisterRelecloud]
4. <span data-ttu-id="3daa5-285">Győződjön meg arról, hogy az új felhasználói hitelesítő adatok a hibrid kapcsolaton keresztül a helyi adatbázisban tárolja, a helyi számítógépen nyissa meg az SQL Management Studio.</span><span class="sxs-lookup"><span data-stu-id="3daa5-285">To verify that the new user's credentials have been stored in your local database through your hybrid connection, open SQL Management Studio on your local computer.</span></span> <span data-ttu-id="3daa5-286">Az Object Explorerben bontsa ki a **MembershipDB** adatbázisából, és végül **táblák**.</span><span class="sxs-lookup"><span data-stu-id="3daa5-286">In Object Explorer, expand the **MembershipDB** database, and then expand **Tables**.</span></span> <span data-ttu-id="3daa5-287">Kattintson a jobb gombbal a **dbo. AspNetUsers** tagsági tábla, és válassza a **legfelső 1000 sor kiválasztása** az eredmények megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="3daa5-287">Right-click the **dbo.AspNetUsers** membership table and choose **Select Top 1000 Rows** to view the results.</span></span>
   
    ![Az eredmények megtekintése][HCTestSSMSTree]
5. <span data-ttu-id="3daa5-289">A helyi csoporttagság most táblázat fiókot is - helyileg létrehozó, és az Azure felhőben létrehozott.</span><span class="sxs-lookup"><span data-stu-id="3daa5-289">Your local membership table now shows both accounts - the one that you created locally, and the one that you created in the Azure cloud.</span></span> <span data-ttu-id="3daa5-290">Az egy, a felhőben Azure hibrid kapcsolat funkción keresztül a helyi adatbázisba mentése megtörtént.</span><span class="sxs-lookup"><span data-stu-id="3daa5-290">The one that you created in the cloud has been saved to your on-premises database through Azure's Hybrid Connection feature.</span></span>
   
    ![A helyszíni adatbázisban regisztrált felhasználók][HCTestShowMemberDb]

<span data-ttu-id="3daa5-292">Most már létrehozta és telepítve az ASP.NET webalkalmazások által használt egy hibrid kapcsolat egy webalkalmazást az Azure felhőben és helyszíni SQL Server-adatbázis között.</span><span class="sxs-lookup"><span data-stu-id="3daa5-292">You have now created and deployed an ASP.NET web application that uses a hybrid connection between a web app in the Azure cloud and an on-premises SQL Server database.</span></span> <span data-ttu-id="3daa5-293">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="3daa5-293">Congratulations!</span></span>

## <a name="see-also"></a><span data-ttu-id="3daa5-294">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="3daa5-294">See Also</span></span>
[<span data-ttu-id="3daa5-295">A hibrid kapcsolatok áttekintése</span><span class="sxs-lookup"><span data-stu-id="3daa5-295">Hybrid Connections overview</span></span>](http://go.microsoft.com/fwlink/p/?LinkID=397274)

[<span data-ttu-id="3daa5-296">Josh a jelölés vezet be a hibrid kapcsolatok (videó a Channel 9)</span><span class="sxs-lookup"><span data-stu-id="3daa5-296">Josh Twist introduces hybrid connections (Channel 9 video)</span></span>](http://channel9.msdn.com/Shows/Azure-Friday/Josh-Twist-introduces-hybrid-connections)

[<span data-ttu-id="3daa5-297">A hibrid kapcsolatok áttekintése</span><span class="sxs-lookup"><span data-stu-id="3daa5-297">Hybrid Connections overview</span></span>](/services/biztalk-services/)

[<span data-ttu-id="3daa5-298">BizTalk szolgáltatások: Irányítópult, a figyelő, a méretezés, a konfigurálás és a hibrid kapcsolat lapokon</span><span class="sxs-lookup"><span data-stu-id="3daa5-298">BizTalk Services: Dashboard, Monitor, Scale, Configure, and Hybrid Connection tabs</span></span>](../biztalk-services/biztalk-dashboard-monitor-scale-tabs.md)

[<span data-ttu-id="3daa5-299">Zökkenőmentes alkalmazások hordozhatóságát (videó a Channel 9) egy valós hibrid felhő felépítése</span><span class="sxs-lookup"><span data-stu-id="3daa5-299">Building a Real-World Hybrid Cloud with Seamless Application Portability (Channel 9 video)</span></span>](http://channel9.msdn.com/events/TechEd/NorthAmerica/2014/DCIM-B323#fbid=)

[<span data-ttu-id="3daa5-300">Hozzáférés a helyszíni erőforrások hibrid kapcsolatok használata az Azure App Service-ben</span><span class="sxs-lookup"><span data-stu-id="3daa5-300">Access on-premises resources using hybrid connections in Azure App Service</span></span>](web-sites-hybrid-connection-get-started.md)

[<span data-ttu-id="3daa5-301">ASP.NET identitás áttekintése</span><span class="sxs-lookup"><span data-stu-id="3daa5-301">ASP.NET Identity Overview</span></span>](http://www.asp.net/identity)

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
