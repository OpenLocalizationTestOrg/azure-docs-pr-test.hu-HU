---
title: "aaaConnect tooSQL adatbázist az SQL Server Management Studio használatával az Azure Remoteappban |} Microsoft Docs"
description: "Az oktatóanyag toolearn hogyan használja a biztonsági és teljesítménynövelő tooSQL adatbázishoz kapcsolódáskor SQL Server Management Studio az Azure Remoteappban toouse"
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
ms.openlocfilehash: 73994f9a1eb3e48efa5d7c4f976b00cfcbc88d75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="use-sql-server-management-studio-in-azure-remoteapp-tooconnect-toosql-database"></a><span data-ttu-id="b21bd-103">SQL Server Management Studio használata az Azure RemoteApp tooconnect tooSQL adatbázis</span><span class="sxs-lookup"><span data-stu-id="b21bd-103">Use SQL Server Management Studio in Azure RemoteApp tooconnect tooSQL Database</span></span>

> [!IMPORTANT]
> <span data-ttu-id="b21bd-104">Azure RemoteApp hamarosan megszűnik.</span><span class="sxs-lookup"><span data-stu-id="b21bd-104">Azure RemoteApp is being discontinued.</span></span> <span data-ttu-id="b21bd-105">Olvasási hello [közlemény](https://go.microsoft.com/fwlink/?linkid=821148) részleteiről.</span><span class="sxs-lookup"><span data-stu-id="b21bd-105">Read hello [announcement](https://go.microsoft.com/fwlink/?linkid=821148) for details.</span></span>
>

## <a name="introduction"></a><span data-ttu-id="b21bd-106">Bevezetés</span><span class="sxs-lookup"><span data-stu-id="b21bd-106">Introduction</span></span>
<span data-ttu-id="b21bd-107">Az oktatóanyag bemutatja, hogyan toouse SQL Server Management Studio (SSMS) az Azure RemoteApp tooconnect tooSQL adatbázis.</span><span class="sxs-lookup"><span data-stu-id="b21bd-107">This tutorial shows you how toouse SQL Server Management Studio (SSMS) in Azure RemoteApp tooconnect tooSQL Database.</span></span> <span data-ttu-id="b21bd-108">Az SQL Server Management Studio az Azure Remoteappban beállításának folyamatán hello bemutatja, hogyan, hello előnyeit ismerteti, és jeleníti meg a biztonsági funkciók használható az Azure Active Directoryban.</span><span class="sxs-lookup"><span data-stu-id="b21bd-108">It walks you through hello process of setting up SQL Server Management Studio in Azure RemoteApp, explains hello benefits, and shows security features that you can use in Azure Active Directory.</span></span>

<span data-ttu-id="b21bd-109">**Becsült idő toocomplete:** 45 percben</span><span class="sxs-lookup"><span data-stu-id="b21bd-109">**Estimated time toocomplete:** 45 minutes</span></span>

## <a name="ssms-in-azure-remoteapp"></a><span data-ttu-id="b21bd-110">Az Azure Remoteappban SSMS</span><span class="sxs-lookup"><span data-stu-id="b21bd-110">SSMS in Azure RemoteApp</span></span>
<span data-ttu-id="b21bd-111">Az Azure RemoteApp egy olyan távoli asztali szolgáltatás letölti a alkalmazások az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="b21bd-111">Azure RemoteApp is an RDS service in Azure that delivers applications.</span></span> <span data-ttu-id="b21bd-112">További információk itt: [Mi az RemoteApp?](../remoteapp/remoteapp-whatis.md)</span><span class="sxs-lookup"><span data-stu-id="b21bd-112">You can learn more about it here: [What is RemoteApp?](../remoteapp/remoteapp-whatis.md)</span></span>

<span data-ttu-id="b21bd-113">Azure RemoteApp által biztosított futó SSMS hello felhasználói élmény ugyanaz, mint SSMS helyileg futó.</span><span class="sxs-lookup"><span data-stu-id="b21bd-113">SSMS running in Azure RemoteApp gives you hello same experience as running SSMS locally.</span></span>

![Az Azure Remoteappban futtató SSMS ábrázoló képernyőfelvétel][1]

## <a name="benefits"></a><span data-ttu-id="b21bd-115">Előnyök</span><span class="sxs-lookup"><span data-stu-id="b21bd-115">Benefits</span></span>
<span data-ttu-id="b21bd-116">Nincsenek számos előnyt toousing SSMS az Azure Remoteappban, beleértve:</span><span class="sxs-lookup"><span data-stu-id="b21bd-116">There are many benefits toousing SSMS in Azure RemoteApp, including:</span></span>

* <span data-ttu-id="b21bd-117">Azure SQL server a 1433-as port nem rendelkezik a külsőleg (Azure-on kívüli) elérhetővé tett toobe.</span><span class="sxs-lookup"><span data-stu-id="b21bd-117">Port 1433 on Azure SQL server does not have toobe exposed externally (outside of Azure).</span></span>
* <span data-ttu-id="b21bd-118">Nincs szükség tookeep hozzáadása és eltávolítása az IP-címek hello Azure SQL server tűzfalon.</span><span class="sxs-lookup"><span data-stu-id="b21bd-118">No need tookeep adding and removing IP addresses in hello Azure SQL server firewall.</span></span>
* <span data-ttu-id="b21bd-119">Az összes Azure RemoteApp kapcsolat fordulhat elő, HTTPS-KAPCSOLATON keresztül a 443-as port használatával titkosítja a távoli asztal protokoll</span><span class="sxs-lookup"><span data-stu-id="b21bd-119">All Azure RemoteApp connections occur over HTTPS on port 443 using encrypted Remote Desktop protocol</span></span>
* <span data-ttu-id="b21bd-120">Többfelhasználós és méretezhető.</span><span class="sxs-lookup"><span data-stu-id="b21bd-120">It is multi-user and can scale.</span></span>
* <span data-ttu-id="b21bd-121">Nincs jobb a teljesítménye az SSMS rendelkező hello hello SQL-adatbázis megegyező régióban.</span><span class="sxs-lookup"><span data-stu-id="b21bd-121">There is a performance gain from having SSMS in hello same region as hello SQL Database.</span></span>
* <span data-ttu-id="b21bd-122">Az Azure RemoteApp használata az Azure Active Directoryban, amely rendelkezik a felhasználói tevékenység jelentések hello prémium kiadásával naplózhatók.</span><span class="sxs-lookup"><span data-stu-id="b21bd-122">You can audit use of Azure RemoteApp with hello Premium edition of Azure Active Directory which has user activity reports.</span></span>
* <span data-ttu-id="b21bd-123">Többtényezős hitelesítés (MFA) is engedélyezheti.</span><span class="sxs-lookup"><span data-stu-id="b21bd-123">You can enable multi-factor authentication (MFA).</span></span>
* <span data-ttu-id="b21bd-124">Hozzáférés bárhonnan esetén hello SSMS támogatott Azure RemoteApp-ügyfelek, mely az iOS, Android, Mac, Windows Phone és Windows-Számítógépet tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b21bd-124">Access SSMS anywhere when using any of hello supported Azure RemoteApp clients which includes iOS, Android, Mac, Windows Phone, and Windows PC’s.</span></span>

## <a name="create-hello-azure-remoteapp-collection"></a><span data-ttu-id="b21bd-125">Hello Azure RemoteApp-gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="b21bd-125">Create hello Azure RemoteApp collection</span></span>
<span data-ttu-id="b21bd-126">Az alábbiakban hello lépéseket toocreate az Azure RemoteApp-gyűjteménnyel ssms alkalmazásával:</span><span class="sxs-lookup"><span data-stu-id="b21bd-126">Here are hello steps toocreate your Azure RemoteApp collection with SSMS:</span></span>

### <a name="1-create-a-new-windows-vm-from-image"></a><span data-ttu-id="b21bd-127">1. Hozzon létre egy új Windows virtuális Gépet lemezképből</span><span class="sxs-lookup"><span data-stu-id="b21bd-127">1. Create a new Windows VM from Image</span></span>
<span data-ttu-id="b21bd-128">Hello hello gyűjtemény toomake "Windows Server távoli asztali munkamenet gazdagépen Windows Server 2012 R2" lemezképet az új virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="b21bd-128">Use hello "Windows Server Remote Desktop Session Host Windows Server 2012 R2" Image from hello Gallery toomake your new VM.</span></span>

### <a name="2-install-ssms-from-sql-express"></a><span data-ttu-id="b21bd-129">2. Az SQL Express SSMS telepítése</span><span class="sxs-lookup"><span data-stu-id="b21bd-129">2. Install SSMS from SQL Express</span></span>
<span data-ttu-id="b21bd-130">Nyissa meg az alakzatot hello új virtuális gép, és keresse meg a letöltési oldal toothis: [Microsoft® SQL Server® 2014 Express](https://www.microsoft.com/download/details.aspx?id=42299)</span><span class="sxs-lookup"><span data-stu-id="b21bd-130">Go onto hello new VM and navigate toothis download page: [Microsoft® SQL Server® 2014 Express](https://www.microsoft.com/download/details.aspx?id=42299)</span></span>

<span data-ttu-id="b21bd-131">A beállítás tooonly letöltését SSMS van.</span><span class="sxs-lookup"><span data-stu-id="b21bd-131">There is an option tooonly download SSMS.</span></span> <span data-ttu-id="b21bd-132">Letöltés után nyissa meg a hello telepítési könyvtár, és futtassa a telepítő tooinstall SSMS.</span><span class="sxs-lookup"><span data-stu-id="b21bd-132">After download, go into hello install directory and run Setup tooinstall SSMS.</span></span>

<span data-ttu-id="b21bd-133">SQL Server 2014 SP1 tooinstall is kell.</span><span class="sxs-lookup"><span data-stu-id="b21bd-133">You also need tooinstall SQL Server 2014 Service Pack 1.</span></span> <span data-ttu-id="b21bd-134">Innen tölthető le: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/download/details.aspx?id=46694)</span><span class="sxs-lookup"><span data-stu-id="b21bd-134">You can download it here: [Microsoft SQL Server 2014 Service Pack 1 (SP1)](https://www.microsoft.com/download/details.aspx?id=46694)</span></span>

<span data-ttu-id="b21bd-135">SQL Server 2014 Service Pack 1 az Azure SQL Database használatához alapvető funkcióit tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="b21bd-135">SQL Server 2014 Service Pack 1 includes essential functionality for working with Azure SQL Database.</span></span>

### <a name="3-run-validate-script-and-sysprep"></a><span data-ttu-id="b21bd-136">3. Parancsfájl-futtatási ellenőrzése és a Sysprep</span><span class="sxs-lookup"><span data-stu-id="b21bd-136">3. Run Validate script and Sysprep</span></span>
<span data-ttu-id="b21bd-137">Hello hello virtuális gép asztalát az érvényesítés nevű PowerShell parancsfájl.</span><span class="sxs-lookup"><span data-stu-id="b21bd-137">On hello desktop of hello VM is a PowerShell script called Validate.</span></span> <span data-ttu-id="b21bd-138">Ehhez kattintson duplán a futtatásához.</span><span class="sxs-lookup"><span data-stu-id="b21bd-138">Run this by double-clicking.</span></span> <span data-ttu-id="b21bd-139">Azt ellenőrzi, hogy adott hello virtuális gép készen áll az alkalmazások távoli üzemeltetéséhez használt toobe.</span><span class="sxs-lookup"><span data-stu-id="b21bd-139">It will verify that hello VM is ready toobe used for remote hosting of applications.</span></span> <span data-ttu-id="b21bd-140">Ha ellenőrzés befejeződött, azt fogja kérni toorun sysprep - toorun válassza ki azt.</span><span class="sxs-lookup"><span data-stu-id="b21bd-140">When verification is complete, it will ask toorun sysprep - choose toorun it.</span></span>

<span data-ttu-id="b21bd-141">A sysprep befejezését követően az hello virtuális gép le fog állni.</span><span class="sxs-lookup"><span data-stu-id="b21bd-141">When sysprep completes, it will shut down hello VM.</span></span>

<span data-ttu-id="b21bd-142">További információ az Azure RemoteApp lemezkép, létrehozása toolearn lásd: [hogyan toocreate RemoteApp sablon rendszerképet az Azure-ban](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)</span><span class="sxs-lookup"><span data-stu-id="b21bd-142">toolearn more about creating a Azure RemoteApp image, see: [How toocreate a RemoteApp template image in Azure](http://blogs.msdn.com/b/rds/archive/2015/03/17/how-to-create-a-remoteapp-template-image-in-azure.aspx)</span></span>

### <a name="4-capture-image"></a><span data-ttu-id="b21bd-143">4. Lemezkép rögzítése</span><span class="sxs-lookup"><span data-stu-id="b21bd-143">4. Capture image</span></span>
<span data-ttu-id="b21bd-144">Hello virtuális gép leállt, ha az aktuális hello portál található, és rögzítse.</span><span class="sxs-lookup"><span data-stu-id="b21bd-144">When hello VM has stopped running, find it in hello current portal and capture it.</span></span>

<span data-ttu-id="b21bd-145">További információ az lemezképek rögzítését toolearn lásd [egy hello klasszikus telepítési modellel létrehozott Azure Windows virtuális gép lemezképének rögzítése](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span><span class="sxs-lookup"><span data-stu-id="b21bd-145">toolearn more about capturing an image, see [Capture an image of an Azure Windows virtual machine created with hello classic deployment model](../virtual-machines/windows/classic/capture-image.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json)</span></span>

### <a name="5-add-tooazure-remoteapp-template-images"></a><span data-ttu-id="b21bd-146">5. Adja hozzá a tooAzure RemoteApp sablon rendszerképek</span><span class="sxs-lookup"><span data-stu-id="b21bd-146">5. Add tooAzure RemoteApp Template images</span></span>
<span data-ttu-id="b21bd-147">Az Azure RemoteApp szakasz hello aktuális portál hello nyissa meg toohello sablon lemezképek lapra, és kattintson a Hozzáadás gombra.</span><span class="sxs-lookup"><span data-stu-id="b21bd-147">In hello Azure RemoteApp section of hello current portal, go toohello Template Images tab and click Add.</span></span> <span data-ttu-id="b21bd-148">A hello előugró párbeszédpanelen válassza a "Képek importálása a virtuális gépek könyvtárból", és válassza a hello újonnan létrehozott lemezképet.</span><span class="sxs-lookup"><span data-stu-id="b21bd-148">In hello pop-up box, select "Import an image from your Virtual Machines library" and then choose hello Image that you just created.</span></span>

### <a name="6-create-cloud-collection"></a><span data-ttu-id="b21bd-149">6. Felhőalapú gyűjtemény létrehozása</span><span class="sxs-lookup"><span data-stu-id="b21bd-149">6. Create cloud collection</span></span>
<span data-ttu-id="b21bd-150">Hello aktuális portálon hozzon létre egy új Azure RemoteApp felhőalapú gyűjtemény.</span><span class="sxs-lookup"><span data-stu-id="b21bd-150">In hello current portal, create a new Azure RemoteApp Cloud Collection.</span></span> <span data-ttu-id="b21bd-151">Válassza ki a hello sablon rendszerképet, amely az imént importált ssms alkalmazásával telepítve van-e.</span><span class="sxs-lookup"><span data-stu-id="b21bd-151">Choose hello Template Image that you just imported with SSMS installed on it.</span></span>

![Új felhőalapú gyűjtemény létrehozása][2]

### <a name="7-publish-ssms"></a><span data-ttu-id="b21bd-153">7. SSMS közzététele</span><span class="sxs-lookup"><span data-stu-id="b21bd-153">7. Publish SSMS</span></span>
<span data-ttu-id="b21bd-154">A lap az új felhőalapú gyűjtemény, jelölje be a közzététel az alkalmazás közzététele hello hello a Start menüben, és válassza a SSMS hello listából.</span><span class="sxs-lookup"><span data-stu-id="b21bd-154">On hello Publishing tab of your new cloud collection, select Publish an application from hello Start Menu and then choose SSMS from hello list.</span></span>

![Alkalmazás közzététele][5]

### <a name="8-add-users"></a><span data-ttu-id="b21bd-156">8. Felhasználók hozzáadása</span><span class="sxs-lookup"><span data-stu-id="b21bd-156">8. Add users</span></span>
<span data-ttu-id="b21bd-157">Hello felhasználói hozzáférés lapon kiválaszthatja, hogy hozzáférési toothis Azure RemoteApp-gyűjtemény csak tartalmazza az SSMS hello felhasználók.</span><span class="sxs-lookup"><span data-stu-id="b21bd-157">On hello User Access tab you can select hello users that will have access toothis Azure RemoteApp collection which only includes SSMS.</span></span>

![Felhasználó hozzáadása][6]

### <a name="9-install-hello-azure-remoteapp-client-application"></a><span data-ttu-id="b21bd-159">9. Hello Azure RemoteApp ügyfélalkalmazás telepítése</span><span class="sxs-lookup"><span data-stu-id="b21bd-159">9. Install hello Azure RemoteApp client application</span></span>
<span data-ttu-id="b21bd-160">Töltse le, és telepítse az Azure RemoteApp-ügyfélen itt: [letöltése |} Az Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)</span><span class="sxs-lookup"><span data-stu-id="b21bd-160">You can download and install a Azure RemoteApp client here: [Download | Azure RemoteApp](https://www.remoteapp.windowsazure.com/en/clients.aspx)</span></span>

## <a name="configure-azure-sql-server"></a><span data-ttu-id="b21bd-161">Azure SQL-kiszolgáló konfigurálása</span><span class="sxs-lookup"><span data-stu-id="b21bd-161">Configure Azure SQL server</span></span>
<span data-ttu-id="b21bd-162">hello csak szükséges konfiguráció Azure Services tooensure engedélyezve van a hello tűzfal.</span><span class="sxs-lookup"><span data-stu-id="b21bd-162">hello only configuration needed is tooensure that Azure Services is enabled for hello firewall.</span></span> <span data-ttu-id="b21bd-163">Ha ez a megoldás használja, majd nem kell tooadd bármely IP-címek tooopen hello tűzfal.</span><span class="sxs-lookup"><span data-stu-id="b21bd-163">If you use this solution, then you do not need tooadd any IP addresses tooopen hello firewall.</span></span> <span data-ttu-id="b21bd-164">SQL Server toohello engedélyezett hello hálózati forgalom származik, más Azure-szolgáltatásokkal.</span><span class="sxs-lookup"><span data-stu-id="b21bd-164">hello network traffic that is allowed toohello SQL Server is from other Azure services.</span></span>

![Azure engedélyezése][4]

## <a name="multi-factor-authentication-mfa"></a><span data-ttu-id="b21bd-166">Többtényezős hitelesítés (MFA)</span><span class="sxs-lookup"><span data-stu-id="b21bd-166">Multi-Factor Authentication (MFA)</span></span>
<span data-ttu-id="b21bd-167">Többtényezős hitelesítés is engedélyezhető az alkalmazás kifejezetten.</span><span class="sxs-lookup"><span data-stu-id="b21bd-167">MFA can be enabled for this application specifically.</span></span> <span data-ttu-id="b21bd-168">Nyissa meg az Azure Active Directory toohello alkalmazások lapján.</span><span class="sxs-lookup"><span data-stu-id="b21bd-168">Go toohello Applications tab of your Azure Active Directory.</span></span> <span data-ttu-id="b21bd-169">A Microsoft Azure RemoteApp található bejegyzés.</span><span class="sxs-lookup"><span data-stu-id="b21bd-169">You will find an entry for Microsoft Azure RemoteApp.</span></span> <span data-ttu-id="b21bd-170">Kattintson az adott alkalmazáshoz, és adja meg, ha megjelenik a hello lap az alábbi ahol engedélyezheti az MFA ehhez az alkalmazáshoz.</span><span class="sxs-lookup"><span data-stu-id="b21bd-170">If you click that application and then configure, you will see hello page below where you can enable MFA for this application.</span></span>

![Többtényezős hitelesítés engedélyezése][3]

## <a name="audit-user-activity-with-azure-active-directory-premium"></a><span data-ttu-id="b21bd-172">Az Azure Active Directory Premium felhasználói műveletek naplózása</span><span class="sxs-lookup"><span data-stu-id="b21bd-172">Audit user activity with Azure Active Directory Premium</span></span>
<span data-ttu-id="b21bd-173">Ha nincs Azure AD Premium számára, akkor el kell tooturn legyen a licencek szakasz, Directory-hello.</span><span class="sxs-lookup"><span data-stu-id="b21bd-173">If you do not have Azure AD Premium, then you have tooturn it on in hello Licenses section of your directory.</span></span> <span data-ttu-id="b21bd-174">Prémium szintű engedélyezve van, a felhasználók toohello Premium szintet rendelhet.</span><span class="sxs-lookup"><span data-stu-id="b21bd-174">With Premium enabled, you can assign users toohello Premium level.</span></span>

<span data-ttu-id="b21bd-175">Amikor tooa felhasználói az Azure Active Directoryban, majd lépjen toohello tevékenység lapon toosee bejelentkezési adatokat tooAzure RemoteApp.</span><span class="sxs-lookup"><span data-stu-id="b21bd-175">When you go tooa user in your Azure Active Directory, you can then go toohello Activity tab toosee login information tooAzure RemoteApp.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b21bd-176">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="b21bd-176">Next steps</span></span>
<span data-ttu-id="b21bd-177">Miután befejezte a fenti lépéseket minden hello, akkor lesz kell tudni toorun hello Azure RemoteApp-ügyfelet és a bejelentkezést egy hozzárendelt felhasználó.</span><span class="sxs-lookup"><span data-stu-id="b21bd-177">After completing all hello above steps, you will be able toorun hello Azure RemoteApp client and log-in with an assigned user.</span></span> <span data-ttu-id="b21bd-178">Meg fog jelenni SSMS az alkalmazások rendelkezésre álló, és ha volt telepítve a számítógépen, az access tooAzure SQL server ugyanúgy futtathatja.</span><span class="sxs-lookup"><span data-stu-id="b21bd-178">You will be presented with SSMS as one of your applications, and you can run it as you would if it were installed on your computer with access tooAzure SQL server.</span></span>

<span data-ttu-id="b21bd-179">Hogyan toomake hello kapcsolat tooSQL adatbázis további információkért lásd: [tooSQL adatbázis csatlakozzon az SQL Server Management Studio eszközt, és minta T-SQL-lekérdezést végrehajtani a](sql-database-connect-query-ssms.md).</span><span class="sxs-lookup"><span data-stu-id="b21bd-179">For more information on how toomake hello connection tooSQL Database, see [Connect tooSQL Database with SQL Server Management Studio and perform a sample T-SQL query](sql-database-connect-query-ssms.md).</span></span>

<span data-ttu-id="b21bd-180">Ez minden most.</span><span class="sxs-lookup"><span data-stu-id="b21bd-180">That's everything for now.</span></span> <span data-ttu-id="b21bd-181">Jó munkát!</span><span class="sxs-lookup"><span data-stu-id="b21bd-181">Enjoy!</span></span>

<!--Image references-->
[1]: ./media/sql-database-ssms-remoteapp/ssms.png
[2]: ./media/sql-database-ssms-remoteapp/newcloudcollection.png
[3]: ./media/sql-database-ssms-remoteapp/mfa.png
[4]: ./media/sql-database-ssms-remoteapp/allowazure.png
[5]: ./media/sql-database-ssms-remoteapp/publish.png
[6]: ./media/sql-database-ssms-remoteapp/user.png