---
title: "Csatlakozás SQL Database adatbázishoz az SQL Server Management Studio használatával az Azure Remoteappban |} Microsoft Docs"
description: "Ez az oktatóanyag segítségével megtudhatja, hogyan kell használnia az SQL Server Management Studio eszközt az Azure Remoteappban a biztonsági és teljesítménynövelő SQL-adatbázishoz szeretne csatlakozni"
services: sql-database
documentationcenter: 
author: adhurwit
manager: jhubbard
ms.assetid: 1052c83c-e7f5-4736-922f-216194d8874b
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/01/2016
ms.author: adhurwit
ms.openlocfilehash: ae1f2fa38d38fe6c10bc7960fddb07ae330d1eeb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="use-sql-server-management-studio-in-azure-remoteapp-to-connect-to-sql-database"></a><span data-ttu-id="cbbd4-103">Azure RemoteApp SQL Server Management Studio segítségével csatlakozzon az SQL Database</span><span class="sxs-lookup"><span data-stu-id="cbbd4-103">Use SQL Server Management Studio in Azure RemoteApp to connect to SQL Database</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cbbd4-104">Azure RemoteApp hamarosan megszűnik.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-104">Azure RemoteApp is being discontinued.</span></span> <span data-ttu-id="cbbd4-105">A részletekért olvassa el a [bejelentést](https://go.microsoft.com/fwlink/?linkid=821148).</span><span class="sxs-lookup"><span data-stu-id="cbbd4-105">Read the [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
>

## <a name="introduction"></a><span data-ttu-id="cbbd4-106">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="cbbd4-106">Introduction</span></span>
<span data-ttu-id="cbbd4-107">Az oktatóanyag bemutatja, hogyan használható az SQL Server Management Studio (SSMS) az Azure Remoteappban SQL adatbázishoz való kapcsolódáshoz.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-107">This tutorial shows you how to use SQL Server Management Studio (SSMS) in Azure RemoteApp to connect to SQL Database.</span></span> <span data-ttu-id="cbbd4-108">Azt a végigvezeti Önt az SQL Server Management Studio az Azure Remoteappban beállításának folyamatán, a következő előnyöket ismerteti, és jeleníti meg a biztonsági funkciók használható az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-108">It walks you through the process of setting up SQL Server Management Studio in Azure RemoteApp, explains the benefits, and shows security features that you can use in Azure Active Directory.</span></span>

<span data-ttu-id="cbbd4-109">**Az oktatóanyag áttekintésének becsült ideje:** 45 perc</span><span class="sxs-lookup"><span data-stu-id="cbbd4-109">**Estimated time to complete:** 45 minutes</span></span>

## <a name="ssms-in-azure-remoteapp"></a><span data-ttu-id="cbbd4-110">Az Azure Remoteappban SSMS</span><span class="sxs-lookup"><span data-stu-id="cbbd4-110">SSMS in Azure RemoteApp</span></span>
<span data-ttu-id="cbbd4-111">Az Azure RemoteApp egy olyan távoli asztali szolgáltatás letölti a alkalmazások az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-111">Azure RemoteApp is an RDS service in Azure that delivers applications.</span></span> <span data-ttu-id="cbbd4-112">További információk itt: [Mi az RemoteApp?](../remoteapp/remoteapp-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="cbbd4-112">You can learn more about it here: [What is RemoteApp?](../remoteapp/remoteapp-whatis.md)</span></span>

<span data-ttu-id="cbbd4-113">Futó Azure RemoteApp szolgáltatáshoz az SSMS helyben fut az SSMS ugyanazt a felhasználói élményt biztosít.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-113">SSMS running in Azure RemoteApp gives you the same experience as running SSMS locally.</span></span>

![Az Azure Remoteappban futtató SSMS ábrázoló képernyőfelvétel][1]

## <a name="benefits"></a><span data-ttu-id="cbbd4-115">Előnyök</span><span class="sxs-lookup"><span data-stu-id="cbbd4-115">Benefits</span></span>
<span data-ttu-id="cbbd4-116">Sok előnyökkel is jár SSMS használatával az Azure Remoteappban, beleértve:</span><span class="sxs-lookup"><span data-stu-id="cbbd4-116">There are many benefits to using SSMS in Azure RemoteApp, including:</span></span>

* <span data-ttu-id="cbbd4-117">Azure SQL server a 1433-as port nem rendelkezik ki vannak téve a külsőleg (Azure-on kívüli).</span><span class="sxs-lookup"><span data-stu-id="cbbd4-117">Port 1433 on Azure SQL server does not have to be exposed externally (outside of Azure).</span></span>
* <span data-ttu-id="cbbd4-118">Nincs szükség hozzáadása és eltávolítása az Azure SQL-kiszolgáló tűzfal IP-címek.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-118">No need to keep adding and removing IP addresses in the Azure SQL server firewall.</span></span>
* <span data-ttu-id="cbbd4-119">Az összes Azure RemoteApp kapcsolat fordulhat elő, HTTPS-KAPCSOLATON keresztül a 443-as port használatával titkosítja a távoli asztal protokoll</span><span class="sxs-lookup"><span data-stu-id="cbbd4-119">All Azure RemoteApp connections occur over HTTPS on port 443 using encrypted Remote Desktop protocol</span></span>
* <span data-ttu-id="cbbd4-120">Többfelhasználós és méretezhető.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-120">It is multi-user and can scale.</span></span>
* <span data-ttu-id="cbbd4-121">Jobb a teljesítménye SSMS rendelkezik az SQL-adatbázis ugyanabban a régióban van.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-121">There is a performance gain from having SSMS in the same region as the SQL Database.</span></span>
* <span data-ttu-id="cbbd4-122">Az Azure RemoteApp használata az Azure Active Directoryban, amely rendelkezik a felhasználói tevékenység jelentések prémium kiadásával naplózhatók.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-122">You can audit use of Azure RemoteApp with the Premium edition of Azure Active Directory which has user activity reports.</span></span>
* <span data-ttu-id="cbbd4-123">Többtényezős hitelesítés (MFA) is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-123">You can enable multi-factor authentication (MFA).</span></span>
* <span data-ttu-id="cbbd4-124">Hozzáférés bárhonnan esetén a támogatott Azure RemoteApp-ügyfelek iOS, Android, Mac, Windows Phone és Windows-Számítógépet tartalmazó SSMS.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-124">Access SSMS anywhere when using any of the supported Azure RemoteApp clients which includes iOS, Android, Mac, Windows Phone, and Windows PC’s.</span></span>

## <a name="create-the-azure-remoteapp-collection"></a><span data-ttu-id="cbbd4-125">Az Azure RemoteApp-gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="cbbd4-125">Create the Azure RemoteApp collection</span></span>
<span data-ttu-id="cbbd4-126">Az Azure RemoteApp-gyűjtemény létrehozása az SSMS lépései a következők:</span><span class="sxs-lookup"><span data-stu-id="cbbd4-126">Here are the steps to create your Azure RemoteApp collection with SSMS:</span></span>

### <a name="1-create-a-new-windows-vm-from-image"></a><span data-ttu-id="cbbd4-127">1. Hozzon létre egy új Windows virtuális Gépet lemezképből</span><span class="sxs-lookup"><span data-stu-id="cbbd4-127">1. Create a new Windows VM from Image</span></span>
<span data-ttu-id="cbbd4-128">A gyűjteményből a "Windows Server távoli asztali munkamenet gazdagépen Windows Server 2012 R2" lemezkép segítségével ellenőrizze az új virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-128">Use the "Windows Server Remote Desktop Session Host Windows Server 2012 R2" Image from the Gallery to make your new VM.</span></span>

### <a name="2-install-ssms-from-sql-express"></a><span data-ttu-id="cbbd4-129">2. Az SQL Express SSMS telepítése</span><span class="sxs-lookup"><span data-stu-id="cbbd4-129">2. Install SSMS from SQL Express</span></span>
<span data-ttu-id="cbbd4-130">Nyissa meg az új virtuális Gépet, és keresse meg a letöltési lapra: [Microsoft® SQL Server® 2014 Express](https://www.microsoft.com/download/details.aspx?id=42299)</span><span class="sxs-lookup"><span data-stu-id="cbbd4-130">Go onto the new VM and navigate to this download page: [Microsoft® SQL Server® 2014 Express](https://www.microsoft.com/download/details.aspx?id=42299)</span></span>

<span data-ttu-id="cbbd4-131">A rendszer felajánlja a csak az SSMS letöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-131">There is an option to only download SSMS.</span></span> <span data-ttu-id="cbbd4-132">Letöltés után nyissa meg a telepítés könyvtárba, és telepítőjének futtatásával telepítse az SSMS.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-132">After download, go into the install directory and run Setup to install SSMS.</span></span>

<span data-ttu-id="cbbd4-133">Is szeretne telepíteni az SQL Server 2014 SP1.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-133">You also need to install SQL Server 2014 Service Pack 1.</span></span> <span data-ttu-id="cbbd4-134">Innen tölthető le: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/download/details.aspx?id=46694)</span><span class="sxs-lookup"><span data-stu-id="cbbd4-134">You can download it here: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/download/details.aspx?id=46694)</span></span>

<span data-ttu-id="cbbd4-135">SQL Server 2014 Service Pack 1 az Azure SQL Database használatához alapvető funkcióit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-135">SQL Server 2014 Service Pack 1 includes essential functionality for working with Azure SQL Database.</span></span>

### <a name="3-run-validate-script-and-sysprep"></a><span data-ttu-id="cbbd4-136">3. Parancsfájl-futtatási ellenőrzése és a Sysprep</span><span class="sxs-lookup"><span data-stu-id="cbbd4-136">3. Run Validate script and Sysprep</span></span>
<span data-ttu-id="cbbd4-137">A virtuális gép asztalán a PowerShell parancsfájl neve ellenőrzése.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-137">On the desktop of the VM is a PowerShell script called Validate.</span></span> <span data-ttu-id="cbbd4-138">Ehhez kattintson duplán a futtatásához.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-138">Run this by double-clicking.</span></span> <span data-ttu-id="cbbd4-139">Ellenőrzi, hogy a távoli gazda az alkalmazások készen áll-e a virtuális Gépet.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-139">It will verify that the VM is ready to be used for remote hosting of applications.</span></span> <span data-ttu-id="cbbd4-140">Ha az ellenőrzés befejeződött, a program kéri futtatni a Sysprep programot – válassza ki a futtatni.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-140">When verification is complete, it will ask to run sysprep - choose to run it.</span></span>

<span data-ttu-id="cbbd4-141">A sysprep befejezése után azt a virtuális gép le fog állni.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-141">When sysprep completes, it will shut down the VM.</span></span>

<span data-ttu-id="cbbd4-142">A Azure RemoteApp-lemezképek létrehozásával kapcsolatos további tudnivalókért lásd: [egy RemoteApp-sablonlemezkép létrehozása az Azure-ban](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="cbbd4-142">To learn more about creating a Azure RemoteApp image, see: [How to create a RemoteApp template image in Azure](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)</span></span>

### <a name="4-capture-image"></a><span data-ttu-id="cbbd4-143">4. Lemezkép rögzítése</span><span class="sxs-lookup"><span data-stu-id="cbbd4-143">4. Capture image</span></span>
<span data-ttu-id="cbbd4-144">Ha a virtuális gép leállt, az aktuális portálon található, és rögzítse.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-144">When the VM has stopped running, find it in the current portal and capture it.</span></span>

<span data-ttu-id="cbbd4-145">Egy lemezkép rögzítésével kapcsolatos további információkért lásd: [egy Azure Windows virtuális gép létrehozása a klasszikus üzembe helyezési modellel lemezképének rögzítése](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="cbbd4-145">To learn more about capturing an image, see [Capture an image of an Azure Windows virtual machine created with the classic deployment model](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

### <a name="5-add-to-azure-remoteapp-template-images"></a><span data-ttu-id="cbbd4-146">5. Azure RemoteApp sablon rendszerképei hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cbbd4-146">5. Add to Azure RemoteApp Template images</span></span>
<span data-ttu-id="cbbd4-147">Az aktuális portálon az Azure RemoteApp szakaszában nyissa meg a sablon lemezképek lapra, és kattintson a Hozzáadás gombra.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-147">In the Azure RemoteApp section of the current portal, go to the Template Images tab and click Add.</span></span> <span data-ttu-id="cbbd4-148">A legördülő mezőben válassza a "Képek importálása a virtuális gépek könyvtárból", és válassza ki az imént létrehozott képet.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-148">In the pop-up box, select "Import an image from your Virtual Machines library" and then choose the Image that you just created.</span></span>

### <a name="6-create-cloud-collection"></a><span data-ttu-id="cbbd4-149">6. Felhőalapú gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="cbbd4-149">6. Create cloud collection</span></span>
<span data-ttu-id="cbbd4-150">Az aktuális portálon hozzon létre egy új Azure RemoteApp felhőalapú gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-150">In the current portal, create a new Azure RemoteApp Cloud Collection.</span></span> <span data-ttu-id="cbbd4-151">Válassza ki a sablon lemezképe most importált ssms alkalmazásával telepítve van-e.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-151">Choose the Template Image that you just imported with SSMS installed on it.</span></span>

![Új felhőalapú gyűjtemény létrehozása][2]

### <a name="7-publish-ssms"></a><span data-ttu-id="cbbd4-153">7. SSMS közzététele</span><span class="sxs-lookup"><span data-stu-id="cbbd4-153">7. Publish SSMS</span></span>
<span data-ttu-id="cbbd4-154">A közzétételi lap az új felhőalapú gyűjtemény, válassza ki a közzétenni egy alkalmazást a Start menüben, és SSMS listájából válasszon ki.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-154">On the Publishing tab of your new cloud collection, select Publish an application from the Start Menu and then choose SSMS from the list.</span></span>

![Alkalmazás közzététele][5]

### <a name="8-add-users"></a><span data-ttu-id="cbbd4-156">8. Felhasználók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="cbbd4-156">8. Add users</span></span>
<span data-ttu-id="cbbd4-157">A felhasználói hozzáférés lapon válassza ki a felhasználók, hozzáférhet a szolgáltatáshoz az SSMS tartalmazó Azure RemoteApp-gyűjteményhez.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-157">On the User Access tab you can select the users that will have access to this Azure RemoteApp collection which only includes SSMS.</span></span>

![Felhasználó hozzáadása][6]

### <a name="9-install-the-azure-remoteapp-client-application"></a><span data-ttu-id="cbbd4-159">9. Az Azure RemoteApp ügyfélalkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="cbbd4-159">9. Install the Azure RemoteApp client application</span></span>
<span data-ttu-id="cbbd4-160">Töltse le, és telepítse az Azure RemoteApp-ügyfélen itt: [letöltése |} Az Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)</span><span class="sxs-lookup"><span data-stu-id="cbbd4-160">You can download and install a Azure RemoteApp client here: [Download | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)</span></span>

## <a name="configure-azure-sql-server"></a><span data-ttu-id="cbbd4-161">Azure SQL-kiszolgáló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="cbbd4-161">Configure Azure SQL server</span></span>
<span data-ttu-id="cbbd4-162">Győződjön meg arról, hogy az Azure szolgáltatás engedélyezve van-e tűzfal szükséges egyetlen konfiguráció is.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-162">The only configuration needed is to ensure that Azure Services is enabled for the firewall.</span></span> <span data-ttu-id="cbbd4-163">Ha ez a megoldás használ, akkor nem szüksége megnyitásához a tűzfal IP-címek hozzáadása.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-163">If you use this solution, then you do not need to add any IP addresses to open the firewall.</span></span> <span data-ttu-id="cbbd4-164">A hálózati forgalmat, hogy az SQL Server más Azure-szolgáltatásokkal származik.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-164">The network traffic that is allowed to the SQL Server is from other Azure services.</span></span>

![Azure engedélyezése][4]

## <a name="multi-factor-authentication-mfa"></a><span data-ttu-id="cbbd4-166">Többtényezős hitelesítés (MFA)</span><span class="sxs-lookup"><span data-stu-id="cbbd4-166">Multi-Factor Authentication (MFA)</span></span>
<span data-ttu-id="cbbd4-167">Többtényezős hitelesítés is engedélyezhető az alkalmazás kifejezetten.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-167">MFA can be enabled for this application specifically.</span></span> <span data-ttu-id="cbbd4-168">Ugrás az Azure Active Directory-alkalmazások fülre.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-168">Go to the Applications tab of your Azure Active Directory.</span></span> <span data-ttu-id="cbbd4-169">A Microsoft Azure RemoteApp található bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-169">You will find an entry for Microsoft Azure RemoteApp.</span></span> <span data-ttu-id="cbbd4-170">Kattintson az adott alkalmazáshoz, és adja meg, ha látni fogja az oldalon az alábbi ahol engedélyezheti az MFA ehhez az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-170">If you click that application and then configure, you will see the page below where you can enable MFA for this application.</span></span>

![Többtényezős hitelesítés engedélyezése][3]

## <a name="audit-user-activity-with-azure-active-directory-premium"></a><span data-ttu-id="cbbd4-172">Az Azure Active Directory Premium felhasználói műveletek naplózása</span><span class="sxs-lookup"><span data-stu-id="cbbd4-172">Audit user activity with Azure Active Directory Premium</span></span>
<span data-ttu-id="cbbd4-173">Ha még nem rendelkezik Azure AD Premium, majd akkor kapcsolja be a könyvtár licencek részében.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-173">If you do not have Azure AD Premium, then you have to turn it on in the Licenses section of your directory.</span></span> <span data-ttu-id="cbbd4-174">Prémium szintű engedélyezve van, a felhasználókat rendelhet a prémium szintű.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-174">With Premium enabled, you can assign users to the Premium level.</span></span>

<span data-ttu-id="cbbd4-175">Amikor egy felhasználó számára az Azure Active Directoryban, majd lépjen az Azure Remoteapphoz bejelentkezési adatok megjelenítéséhez tevékenység lapját.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-175">When you go to a user in your Azure Active Directory, you can then go to the Activity tab to see login information to Azure RemoteApp.</span></span>

## <a name="next-steps"></a><span data-ttu-id="cbbd4-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="cbbd4-176">Next steps</span></span>
<span data-ttu-id="cbbd4-177">A fenti lépések végrehajtását követően fogja tudni futtatni az Azure RemoteApp-ügyfelet és a bejelentkezést egy hozzárendelt felhasználóval.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-177">After completing all the above steps, you will be able to run the Azure RemoteApp client and log-in with an assigned user.</span></span> <span data-ttu-id="cbbd4-178">Meg fog jelenni SSMS az alkalmazások rendelkezésre álló, és ha volt telepítve a számítógépen az Azure SQL-kiszolgálóhoz hozzáféréssel rendelkező ugyanúgy futtathatja.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-178">You will be presented with SSMS as one of your applications, and you can run it as you would if it were installed on your computer with access to Azure SQL server.</span></span>

<span data-ttu-id="cbbd4-179">Hogyan végezheti el a kapcsolatot az SQL Database további információkért lásd: [Csatlakozás SQL Database adatbázishoz az SQL Server Management Studio eszközt, és végezze el a T-SQL-mintalekérdezés](sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="cbbd4-179">For more information on how to make the connection to SQL Database, see [Connect to SQL Database with SQL Server Management Studio and perform a sample T-SQL query](sql-database-connect-query-ssms.md).</span></span>

<span data-ttu-id="cbbd4-180">Ez minden most.</span><span class="sxs-lookup"><span data-stu-id="cbbd4-180">That's everything for now.</span></span> <span data-ttu-id="cbbd4-181">Jó munkát!</span><span class="sxs-lookup"><span data-stu-id="cbbd4-181">Enjoy!</span></span>

<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png