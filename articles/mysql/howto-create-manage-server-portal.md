---
title: "aaaCreate és kezelheti az Azure-adatbázis MySQL kiszolgáló Azure-portál használatával |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan gyorsan létrehozhat egy új Azure-adatbázist a MySQL-kiszolgáló és hello Azure portál használatával hello kiszolgáló kezeléséhez."
services: mysql
author: v-chenyh
ms.author: nolanwu
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: c532df43b3d2124d7bd34875b32d52357f162af8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-mysql-server-using-azure-portal"></a><span data-ttu-id="bf7b7-103">Létrehozása és kezelése az Azure-adatbázis MySQL-kiszolgáló Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="bf7b7-103">Create and manage Azure Database for MySQL server using Azure portal</span></span>
<span data-ttu-id="bf7b7-104">Ez a cikk ismerteti, hogyan gyorsan létrehozhat egy új Azure-adatbázist a MySQL-kiszolgáló és hello Azure portál használatával hello kiszolgáló kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-104">This article describes how you can quickly create a new Azure Database for MySQL server and manage hello server using hello Azure Portal.</span></span> <span data-ttu-id="bf7b7-105">Kiszolgáló kezelése tartalmaz kiszolgálóadatok & adatbázisok megtekintéséhez, a jelszó alaphelyzetbe állítása és a hello kiszolgáló törlése.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-105">Server management includes viewing server details & databases, resetting password and deleting hello server.</span></span>

## <a name="log-in-toohello-azure-portal"></a><span data-ttu-id="bf7b7-106">Jelentkezzen be toohello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="bf7b7-106">Log in toohello Azure portal</span></span>
<span data-ttu-id="bf7b7-107">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="bf7b7-107">Log in toohello [Azure portal](https://portal.azure.com).</span></span>

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="bf7b7-108">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="bf7b7-108">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="bf7b7-109">Kövesse ezeket a lépéseket toocreate egy "mysqlserver4demo" nevű MySQL-kiszolgálóhoz tartozó Azure-adatbázis</span><span class="sxs-lookup"><span data-stu-id="bf7b7-109">Follow these steps toocreate an Azure Database for MySQL server named “mysqlserver4demo”</span></span>

<span data-ttu-id="bf7b7-110">1 kattintással **új** hello bal felső sarkában hello Azure-portálon található gombra.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-110">1- Click **New** button found on hello upper left-hand corner of hello Azure portal.</span></span>

<span data-ttu-id="bf7b7-111">2-válassza **adatbázisok** hello oldalát, és válassza ki a **MySQL az Azure-adatbázis** hello adatbázisok oldalról.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-111">2- Select **Databases** from hello New page, and select **Azure Database for MySQL** from hello Databases page.</span></span>

> <span data-ttu-id="bf7b7-112">Egy MySQL-kiszolgálóhoz tartozó Azure-adatbázis jön létre egy adott csoportján [számítási és tárolási](./concepts-compute-unit-and-storage.md) erőforrásokat.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-112">An Azure Database for MySQL server is created with a defined set of [compute and storage](./concepts-compute-unit-and-storage.md) resources.</span></span> <span data-ttu-id="bf7b7-113">hello adatbázis egy Azure erőforráscsoporton belül, és a MySQL-kiszolgáló egy Azure-adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-113">hello database is created within an Azure resource group and in an Azure Database for MySQL server.</span></span>

![– új-kiszolgáló létrehozása](./media/howto-create-manage-server-portal/create-new-server.png)

<span data-ttu-id="bf7b7-115">3 - hello Azure adatbázis MySQL űrlap a következő információ hello kitöltötte:</span><span class="sxs-lookup"><span data-stu-id="bf7b7-115">3- Fill out hello Azure Database for MySQL form with hello following information:</span></span>

| <span data-ttu-id="bf7b7-116">**Űrlapmező**</span><span class="sxs-lookup"><span data-stu-id="bf7b7-116">**Form Field**</span></span> | <span data-ttu-id="bf7b7-117">**Mező leírása**</span><span class="sxs-lookup"><span data-stu-id="bf7b7-117">**Field Description**</span></span> |
|----------------|-----------------------|
| <span data-ttu-id="bf7b7-118">*Kiszolgálónév*</span><span class="sxs-lookup"><span data-stu-id="bf7b7-118">*Server name*</span></span> | <span data-ttu-id="bf7b7-119">Azure-beli mysql (a kiszolgáló neve, globálisan egyedi)</span><span class="sxs-lookup"><span data-stu-id="bf7b7-119">azure-mysql (server name is globally unique)</span></span> |
| <span data-ttu-id="bf7b7-120">*Előfizetés*</span><span class="sxs-lookup"><span data-stu-id="bf7b7-120">*Subscription*</span></span> | <span data-ttu-id="bf7b7-121">MySQLaaS (válassza ki a legördülő lista)</span><span class="sxs-lookup"><span data-stu-id="bf7b7-121">MySQLaaS (select from drop down)</span></span> |
| <span data-ttu-id="bf7b7-122">*Erőforráscsoport*</span><span class="sxs-lookup"><span data-stu-id="bf7b7-122">*Resource group*</span></span> | <span data-ttu-id="bf7b7-123">erőforrás (hozzon létre egy új erőforráscsoportot, vagy használjon egy meglévőt)</span><span class="sxs-lookup"><span data-stu-id="bf7b7-123">myresource (create a new resource group or use an existing one)</span></span> |
| <span data-ttu-id="bf7b7-124">*Kiszolgálói rendszergazdai bejelentkezés*</span><span class="sxs-lookup"><span data-stu-id="bf7b7-124">*Server admin login*</span></span> | <span data-ttu-id="bf7b7-125">myadmin (állítsa be a rendszergazdafiók nevét)</span><span class="sxs-lookup"><span data-stu-id="bf7b7-125">myadmin (setup admin account name)</span></span> |
| <span data-ttu-id="bf7b7-126">*Jelszó*</span><span class="sxs-lookup"><span data-stu-id="bf7b7-126">*Password*</span></span> | <span data-ttu-id="bf7b7-127">a telepítő rendszergazda fiók jelszava</span><span class="sxs-lookup"><span data-stu-id="bf7b7-127">setup admin account password</span></span> |
| <span data-ttu-id="bf7b7-128">*Jelszó megerősítése*</span><span class="sxs-lookup"><span data-stu-id="bf7b7-128">*Confirm password*</span></span> | <span data-ttu-id="bf7b7-129">erősítse meg a rendszergazdafiók jelszavát</span><span class="sxs-lookup"><span data-stu-id="bf7b7-129">confirm admin account password</span></span> |
| <span data-ttu-id="bf7b7-130">*Hely*</span><span class="sxs-lookup"><span data-stu-id="bf7b7-130">*Location*</span></span> | <span data-ttu-id="bf7b7-131">Észak-Európa (válassza ki a Észak-Európában és USA nyugati régiója között)</span><span class="sxs-lookup"><span data-stu-id="bf7b7-131">North Europe (select between North Europe and West US)</span></span> |
| <span data-ttu-id="bf7b7-132">*Verzió*</span><span class="sxs-lookup"><span data-stu-id="bf7b7-132">*Version*</span></span> | <span data-ttu-id="bf7b7-133">5.6 (Azure-adatbázis kiválasztása a MySQL-kiszolgáló verziója)</span><span class="sxs-lookup"><span data-stu-id="bf7b7-133">5.6 (choose Azure Database for MySQL server version)</span></span> |

<span data-ttu-id="bf7b7-134">4 kattintással **tarifacsomag** toospecify hello és teljesítményszintet szolgáltatásszint az új kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-134">4- Click **Pricing tier** toospecify hello service tier and performance level for your new server.</span></span> <span data-ttu-id="bf7b7-135">Számítási egység közötti 50-100 alapszintű rétegben, 100, 200 normál rétegben, konfigurálhatók, és adható tárterület belefoglalt mennyisége alapján.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-135">Compute Unit can be configured between 50 and 100 in Basic tier, 100 and 200 in Standard tier, and storage can be added based on included amount.</span></span> <span data-ttu-id="bf7b7-136">Ez az útmutató útmutató most válassza 50 számítási egység és 50GB.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-136">For this HowTo guide, let’s choose 50 Compute Unit and 50GB.</span></span> <span data-ttu-id="bf7b7-137">Kattintson a **OK** toosave a kijelölés.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-137">Click **OK** toosave your selection.</span></span>
<span data-ttu-id="bf7b7-138">![Hozzon létre-kiszolgáló--tarifacsomagot](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)</span><span class="sxs-lookup"><span data-stu-id="bf7b7-138">![create-server-pricing-tier](./media/howto-create-manage-server-portal/create-server-pricing-tier.png)</span></span>

<span data-ttu-id="bf7b7-139">5 kattintással **létrehozása** tooprovision hello kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-139">5- Click **Create** tooprovision hello server.</span></span> <span data-ttu-id="bf7b7-140">Az üzembe helyezés eltarthat néhány percig.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-140">Provisioning takes a few minutes.</span></span>

> <span data-ttu-id="bf7b7-141">Ellenőrizze a hello **PIN-kód toodashboard** beállítás tooallow könnyű nyomon a központi telepítés.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-141">Check hello **Pin toodashboard** option tooallow easy tracking of your deployments.</span></span>
> [!NOTE]
> <span data-ttu-id="bf7b7-142">Bár az alapszintű rétegben too1000GB és 10000GB, a Standard szint is támogatottak lesznek a tároláshoz, Public Preview hello maximális tárolási ideiglenesen továbbra is korlátozott too1000GB.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-142">Although up too1000GB in Basic tier and 10000GB in Standard tier will be supported for storage, for Public Preview, hello maximum storage is still limited too1000GB temporarily.</span></span> 
</Include>

## <a name="update-an-azure-database-for-mysql-server"></a><span data-ttu-id="bf7b7-143">Egy MySQL-kiszolgálóhoz tartozó Azure-adatbázis frissítése</span><span class="sxs-lookup"><span data-stu-id="bf7b7-143">Update an Azure Database for MySQL server</span></span>
<span data-ttu-id="bf7b7-144">Új kiszolgáló üzembe helyezése után felhasználó rendelkezik-e 2 beállítások tooedit egy meglévő kiszolgálóról: alaphelyzetbe állítja a rendszergazdai jelszót vagy skálája fel/le hello server hello számítási – egység módosításával.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-144">After new server is provisioned, user has 2 options tooedit an existing server: reset administrator password or scale up/down hello server by changing hello compute-units.</span></span>

### <a name="change-hello-administrator-user-password"></a><span data-ttu-id="bf7b7-145">Hello rendszergazda felhasználói jelszó módosítása</span><span class="sxs-lookup"><span data-stu-id="bf7b7-145">Change hello administrator user password</span></span>
<span data-ttu-id="bf7b7-146">1 - kiszolgálón hello **áttekintése** panelen kattintson a **jelszó-átállítási** toopopulate egy jelszó bemeneti és a megerősítő ablakot.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-146">1- On hello server **Overview** blade, click **Reset password** toopopulate a password input and confirmation window.</span></span>

<span data-ttu-id="bf7b7-147">2 - hello jelszó hello ablakban az alábbi, új jelszó megadására és megerősítésére: ![jelszó alaphelyzetbe állítása](./media/howto-create-manage-server-portal/reset-password.png)</span><span class="sxs-lookup"><span data-stu-id="bf7b7-147">2- Enter new password and confirm hello password in hello window as below: ![reset-password](./media/howto-create-manage-server-portal/reset-password.png)</span></span>

<span data-ttu-id="bf7b7-148">3 kattintással **OK** toosave hello új jelszót.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-148">3- Click **OK** toosave hello new password.</span></span>

### <a name="scale-updown-by-changing-compute-units"></a><span data-ttu-id="bf7b7-149">Fel/le méretezhető számítási egység módosítása</span><span class="sxs-lookup"><span data-stu-id="bf7b7-149">Scale up/down by changing Compute Units</span></span>

<span data-ttu-id="bf7b7-150">1 – a hello server panelen a **beállítások**, kattintson a **tarifacsomag** tooopen hello árazás réteg panel az Azure-adatbázis hello MySQL-kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-150">1- On hello server blade, under **Settings**, click **Pricing tier** tooopen hello Pricing tier blade for hello Azure Database for MySQL server.</span></span>

<span data-ttu-id="bf7b7-151">4 lépés 2 követhető **MySQL-kiszolgáló Azure-adatbázis létrehozása** toochange számítási egység hello azonos IP-címek.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-151">2- Follow Step 4 in **Create an Azure Database for MySQL server** toochange Compute Units in hello same pricing tier.</span></span>

## <a name="delete-an-azure-database-for-mysql-server"></a><span data-ttu-id="bf7b7-152">Egy MySQL-kiszolgálóhoz tartozó Azure-adatbázis törlése</span><span class="sxs-lookup"><span data-stu-id="bf7b7-152">Delete an Azure Database for MySQL server</span></span>

<span data-ttu-id="bf7b7-153">1 - kiszolgálón hello **áttekintése** panelen kattintson a **törlése** parancs gomb tooopen hello törlése megerősítő panelen.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-153">1- On hello server **Overview** blade, click **Delete** command button tooopen hello Deleting confirmation blade.</span></span>

<span data-ttu-id="bf7b7-154">2-típus hello helyes kiszolgálónevet hello panelről dupla megerősítés beviteli mezőbe.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-154">2- Type hello correct server name in input box of hello blade for double confirmation.</span></span>

<span data-ttu-id="bf7b7-155">3 kattintással **törlése** tooconfirm törlésekor a művelet újra gombra, és várja meg, "Törlése sikeres" előugró hello értesítési sáv.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-155">3- Click **Delete** button again tooconfirm deleting action and wait for “Deleting success” popup on hello notification bar.</span></span>

## <a name="list-hello-azure-database-for-mysql-databases"></a><span data-ttu-id="bf7b7-156">Lista hello Azure-adatbázis a MySQL-adatbázisok</span><span class="sxs-lookup"><span data-stu-id="bf7b7-156">List hello Azure Database for MySQL databases</span></span>
<span data-ttu-id="bf7b7-157">Hello kiszolgálón **áttekintése** panelen görgessen lefelé, amíg megjelenik a hello adatbázis hello alsó csempét.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-157">On hello server **Overview** blade, scroll down until you see hello database tile on hello bottom.</span></span> <span data-ttu-id="bf7b7-158">Összes hello adatbázis hello táblázatban jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-158">All hello databases will be listed in hello table.</span></span> <span data-ttu-id="bf7b7-159">Kattintson a **törlése** parancs gomb tooopen hello törlése megerősítő panelen.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-159">click **Delete** command button tooopen hello Deleting confirmation blade.</span></span>

![megjelenítése-adatbázisok](./media/howto-create-manage-server-portal/show-databases.png)

## <a name="show-details-of-an-azure-database-for-mysql-server"></a><span data-ttu-id="bf7b7-161">Egy Azure-adatbázis MySQL-kiszolgáló részleteinek megjelenítése</span><span class="sxs-lookup"><span data-stu-id="bf7b7-161">Show details of an Azure Database for MySQL server</span></span>
<span data-ttu-id="bf7b7-162">Kattintson a **tulajdonságok** alatt **beállítások** hello kiszolgálón panel nyílik hello **tulajdonságok** panelen.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-162">Click **Properties** under **Settings** on hello server blade will open hello **Properties** blade.</span></span> <span data-ttu-id="bf7b7-163">Majd hello kiszolgálóval kapcsolatos összes részletes információk is megtekinthetők.</span><span class="sxs-lookup"><span data-stu-id="bf7b7-163">Then you can view all detailed information about hello server.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bf7b7-164">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bf7b7-164">Next steps</span></span>

[<span data-ttu-id="bf7b7-165">Gyors üzembe helyezés: Azure-adatbázis létrehozása a MySQL-kiszolgáló Azure-portál használatával</span><span class="sxs-lookup"><span data-stu-id="bf7b7-165">Quickstart: Create Azure Database for MySQL server using Azure portal</span></span>](./quickstart-create-mysql-server-database-using-azure-portal.md)
