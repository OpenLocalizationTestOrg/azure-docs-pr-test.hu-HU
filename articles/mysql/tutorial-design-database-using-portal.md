---
title: "az első Azure adatbázis a MySQL-adatbázis - Azure Portal aaaDesign |} Microsoft Docs"
description: "Ez az oktatóanyag azt ismerteti, hogyan toocreate és kezelése az Azure-adatbázis MySQL-kiszolgáló és az adatbázis-Azure portál használatával."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 06/06/2017
ms.custom: mvc
ms.openlocfilehash: 06dd952acc5356b8cccaf36917df1ff8db4f7139
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a><span data-ttu-id="eb2b3-103">Az első Azure-adatbázis MySQL-adatbázis megtervezése</span><span class="sxs-lookup"><span data-stu-id="eb2b3-103">Design your first Azure Database for MySQL database</span></span>
<span data-ttu-id="eb2b3-104">Azure MySQL-adatbázis egy felügyelt szolgáltatás, amely lehetővé teszi toorun, kezelése, és magas rendelkezésre állású MySQL-adatbázisok hello felhőben méretezni.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-104">Azure Database for MySQL is a managed service that enables you toorun, manage, and scale highly available MySQL databases in hello cloud.</span></span> <span data-ttu-id="eb2b3-105">Hello Azure-portált használja, egyszerűen kezelheti a kiszolgálót, és egy adatbázis tervezése.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-105">Using hello Azure portal, you can easily manage your server and design a database.</span></span>

<span data-ttu-id="eb2b3-106">Ebben az oktatóanyagban használhatja az Azure portál toolearn hello hogyan számára:</span><span class="sxs-lookup"><span data-stu-id="eb2b3-106">In this tutorial, you use hello Azure portal toolearn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="eb2b3-107">A MySQL az Azure-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="eb2b3-107">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="eb2b3-108">Hello kiszolgáló tűzfal konfigurálása</span><span class="sxs-lookup"><span data-stu-id="eb2b3-108">Configure hello server firewall</span></span>
> * <span data-ttu-id="eb2b3-109">Mysql parancssori eszköz toocreate adatbázis használata</span><span class="sxs-lookup"><span data-stu-id="eb2b3-109">Use mysql command-line tool toocreate a database</span></span>
> * <span data-ttu-id="eb2b3-110">Mintaadatok betöltése</span><span class="sxs-lookup"><span data-stu-id="eb2b3-110">Load sample data</span></span>
> * <span data-ttu-id="eb2b3-111">Adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="eb2b3-111">Query data</span></span>
> * <span data-ttu-id="eb2b3-112">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="eb2b3-112">Update data</span></span>
> * <span data-ttu-id="eb2b3-113">Adatok visszaállítása</span><span class="sxs-lookup"><span data-stu-id="eb2b3-113">Restore data</span></span>

## <a name="sign-in-toohello-azure-portal"></a><span data-ttu-id="eb2b3-114">Jelentkezzen be toohello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="eb2b3-114">Sign in toohello Azure portal</span></span>
<span data-ttu-id="eb2b3-115">Nyissa meg a kedvenc webböngészőt, és látogasson el a hello [Microsoft Azure-portálon](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="eb2b3-115">Open your favorite web browser, and visit hello [Microsoft Azure portal](https://portal.azure.com/).</span></span> <span data-ttu-id="eb2b3-116">Adja meg a hitelesítő adatok toosign toohello portálon.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-116">Enter your credentials toosign in toohello portal.</span></span> <span data-ttu-id="eb2b3-117">az alapértelmezett nézet hello a szolgáltatás irányítópultján.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-117">hello default view is your service dashboard.</span></span>

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="eb2b3-118">Azure-adatbázis létrehozása MySQL-kiszolgálóhoz</span><span class="sxs-lookup"><span data-stu-id="eb2b3-118">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="eb2b3-119">A MySQL-kiszolgálóhoz készült Azure-adatbázis [számítási és tárolási erőforrások](./concepts-compute-unit-and-storage.md) egy meghatározott készletével együtt jön létre.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-119">An Azure Database for MySQL server is created with a defined set of [compute and storage resources](./concepts-compute-unit-and-storage.md).</span></span> <span data-ttu-id="eb2b3-120">hello server belül létrejön egy [Azure erőforráscsoport](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span><span class="sxs-lookup"><span data-stu-id="eb2b3-120">hello server is created within an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview).</span></span>

1. <span data-ttu-id="eb2b3-121">Keresse meg a túl**adatbázisok** > **MySQL az Azure-adatbázis**.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-121">Navigate too**Databases** > **Azure Database for MySQL**.</span></span> <span data-ttu-id="eb2b3-122">Ha nem találja a MySQL-kiszolgáló **adatbázisok** kategória, kattintson a **láthatja az összes** tooshow összes elérhető szolgáltatások adatbázisában.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-122">If you cannot find MySQL Server under **Databases** category, click **See all** tooshow all available database services.</span></span> <span data-ttu-id="eb2b3-123">Is beírhat **MySQL az Azure-adatbázis** hello Keresés mezőbe tooquickly található hello szolgáltatást.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-123">You can also type **Azure Database for MySQL** in hello search box tooquickly find hello service.</span></span>
<span data-ttu-id="eb2b3-124">![2-1 Navigálás tooMySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)</span><span class="sxs-lookup"><span data-stu-id="eb2b3-124">![2-1 Navigate tooMySQL](./media/tutorial-design-database-using-portal/2_1-Navigate-to-MySQL.png)</span></span>

2. <span data-ttu-id="eb2b3-125">Kattintson a **MySQL az Azure-adatbázis** csempére, majd **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-125">Click **Azure Database for MySQL** tile, and then click **Create**.</span></span>

<span data-ttu-id="eb2b3-126">A példa kedvéért töltse ki a hello Azure adatbázis MySQL űrlap a következő információ hello:</span><span class="sxs-lookup"><span data-stu-id="eb2b3-126">In our example, fill out hello Azure Database for MySQL form with hello following information:</span></span>

| <span data-ttu-id="eb2b3-127">**Beállítás**</span><span class="sxs-lookup"><span data-stu-id="eb2b3-127">**Setting**</span></span> | <span data-ttu-id="eb2b3-128">**Ajánlott érték**</span><span class="sxs-lookup"><span data-stu-id="eb2b3-128">**Suggested value**</span></span> | <span data-ttu-id="eb2b3-129">**Mező leírása**</span><span class="sxs-lookup"><span data-stu-id="eb2b3-129">**Field Description**</span></span> |
|---|---|---|
| <span data-ttu-id="eb2b3-130">*Kiszolgálónév*</span><span class="sxs-lookup"><span data-stu-id="eb2b3-130">*Server name*</span></span> | <span data-ttu-id="eb2b3-131">myserver4demo</span><span class="sxs-lookup"><span data-stu-id="eb2b3-131">myserver4demo</span></span>  | <span data-ttu-id="eb2b3-132">Kiszolgálónév a globálisan egyedi toobe tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-132">Server name has toobe globally unique.</span></span> |
| <span data-ttu-id="eb2b3-133">*Előfizetés*</span><span class="sxs-lookup"><span data-stu-id="eb2b3-133">*Subscription*</span></span> | <span data-ttu-id="eb2b3-134">mysubscription</span><span class="sxs-lookup"><span data-stu-id="eb2b3-134">mysubscription</span></span> | <span data-ttu-id="eb2b3-135">Jelölje ki az előfizetését a hello legördülő.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-135">Select your subscription from hello drop-down.</span></span> |
| <span data-ttu-id="eb2b3-136">*Erőforráscsoport*</span><span class="sxs-lookup"><span data-stu-id="eb2b3-136">*Resource group*</span></span> | <span data-ttu-id="eb2b3-137">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="eb2b3-137">myresourcegroup</span></span> | <span data-ttu-id="eb2b3-138">Hozzon létre erőforráscsoportot, vagy használjon egy meglévőt.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-138">Create a resource group or use an existing one.</span></span> |
| <span data-ttu-id="eb2b3-139">*Kiszolgálói rendszergazdai bejelentkezés*</span><span class="sxs-lookup"><span data-stu-id="eb2b3-139">*Server admin login*</span></span> | <span data-ttu-id="eb2b3-140">myadmin</span><span class="sxs-lookup"><span data-stu-id="eb2b3-140">myadmin</span></span> | <span data-ttu-id="eb2b3-141">A telepítő a rendszergazdai fiók nevét.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-141">Setup admin account name.</span></span> |
| <span data-ttu-id="eb2b3-142">*Jelszó*</span><span class="sxs-lookup"><span data-stu-id="eb2b3-142">*Password*</span></span> |  | <span data-ttu-id="eb2b3-143">Állítson be erős jelszót a rendszergazdafiókhoz.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-143">Set a strong admin account password.</span></span> |
| <span data-ttu-id="eb2b3-144">*Jelszó megerősítése*</span><span class="sxs-lookup"><span data-stu-id="eb2b3-144">*Confirm password*</span></span> |  | <span data-ttu-id="eb2b3-145">Erősítse meg a hello rendszergazdai fiók jelszavát.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-145">Confirm hello admin account password.</span></span> |
| <span data-ttu-id="eb2b3-146">*Hely*</span><span class="sxs-lookup"><span data-stu-id="eb2b3-146">*Location*</span></span> |  | <span data-ttu-id="eb2b3-147">Válasszon a rendelkezésre álló régiók közül.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-147">Select an available region.</span></span> |
| <span data-ttu-id="eb2b3-148">*Verzió*</span><span class="sxs-lookup"><span data-stu-id="eb2b3-148">*Version*</span></span> | <span data-ttu-id="eb2b3-149">5.7</span><span class="sxs-lookup"><span data-stu-id="eb2b3-149">5.7</span></span> | <span data-ttu-id="eb2b3-150">Válassza ki a hello legújabb verziót.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-150">Choose hello latest version.</span></span> |
| <span data-ttu-id="eb2b3-151">*Teljesítmény konfigurálása*</span><span class="sxs-lookup"><span data-stu-id="eb2b3-151">*Configure performance*</span></span> | <span data-ttu-id="eb2b3-152">Alapszintű, 50 számítási egység, 50 GB</span><span class="sxs-lookup"><span data-stu-id="eb2b3-152">Basic, 50 compute units, 50 GB</span></span>  | <span data-ttu-id="eb2b3-153">Válassza ki a **Tarifacsomagot**, a **Számítási egységek** számát, a GB-ban mért **Tárterületet** , majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-153">Choose **Pricing tier**, **Compute Units**, **Storage (GB)**, and then click **OK**.</span></span> |
| <span data-ttu-id="eb2b3-154">*PIN-kód tooDashboard*</span><span class="sxs-lookup"><span data-stu-id="eb2b3-154">*Pin tooDashboard*</span></span> | <span data-ttu-id="eb2b3-155">Jelölőnégyzet</span><span class="sxs-lookup"><span data-stu-id="eb2b3-155">Check</span></span> | <span data-ttu-id="eb2b3-156">Ajánlott a ebben a mezőben toocheck így könnyen később a előfordulhat hello kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="eb2b3-156">Recommended toocheck this box so you may find hello server easily later on</span></span> |
<span data-ttu-id="eb2b3-157">Ezt követően kattintson a **Create** (Létrehozás) gombra.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-157">Then, click **Create**.</span></span> <span data-ttu-id="eb2b3-158">Egy-két percen egy új Azure-adatbázis MySQL-kiszolgáló fut. hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-158">In a minute or two, a new Azure Database for MySQL server is running in hello cloud.</span></span> <span data-ttu-id="eb2b3-159">Kattinthat **értesítések** hello eszköztár toomonitor hello telepítési folyamatának gombjára.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-159">You can click **Notifications** button on hello toolbar toomonitor hello deployment process.</span></span>

## <a name="configure-firewall"></a><span data-ttu-id="eb2b3-160">Tűzfal konfigurálása</span><span class="sxs-lookup"><span data-stu-id="eb2b3-160">Configure firewall</span></span>
<span data-ttu-id="eb2b3-161">Az Azure-adatbázis MySQL adatbázis tűzfal védi meg.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-161">Azure Databases for MySQL are protected by a firewall.</span></span> <span data-ttu-id="eb2b3-162">Alapértelmezés szerint minden kapcsolatok toohello server és hello adatbázisok hello-kiszolgálón belül utasítja el.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-162">By default, all connections toohello server and hello databases inside hello server are rejected.</span></span> <span data-ttu-id="eb2b3-163">Csatlakozás előtt tooAzure adatbázis a MySQL hello először, konfigurálja a hello tűzfal tooadd hello ügyfél gép nyilvános IP-címe (vagy IP-címtartomány).</span><span class="sxs-lookup"><span data-stu-id="eb2b3-163">Before connecting tooAzure Database for MySQL for hello first time, configure hello firewall tooadd hello client machine's public network IP address (or IP address range).</span></span>

1. <span data-ttu-id="eb2b3-164">Kattintson az újonnan létrehozott kiszolgáló, majd a **kapcsolatbiztonsági**.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-164">Click your newly created server, and then click **Connection security**.</span></span>
   <span data-ttu-id="eb2b3-165">![3-1 kapcsolatbiztonsági](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)</span><span class="sxs-lookup"><span data-stu-id="eb2b3-165">![3-1 Connection security](./media/tutorial-design-database-using-portal/3_1-Connection-security.png)</span></span>
2. <span data-ttu-id="eb2b3-166">Is **hozzáadása a saját IP**, vagy itt a tűzfalszabályok konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-166">You can **Add My IP**, or configure firewall rules here.</span></span> <span data-ttu-id="eb2b3-167">Ne feledje tooclick **mentése** hello szabályok létrehozása után.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-167">Remember tooclick **Save** after you have created hello rules.</span></span>
<span data-ttu-id="eb2b3-168">Most toohello server mysql parancssori eszköz vagy MySQL munkaterület grafikus felhasználói felület használatával is elérheti.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-168">You can now connect toohello server using mysql command-line tool or MySQL Workbench GUI tool.</span></span>

> [!TIP]
> <span data-ttu-id="eb2b3-169">MySQL-kiszolgálóhoz tartozó Azure-adatbázis 3306 porton keresztül kommunikál.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-169">Azure Database for MySQL server communicates over port 3306.</span></span> <span data-ttu-id="eb2b3-170">Ha a vállalati hálózatból származó tooconnect, a hálózati tűzfal előfordulhat, hogy nem engedélyezett a port 3306 kimenő forgalmát.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-170">If you are trying tooconnect from within a corporate network, outbound traffic over port 3306 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="eb2b3-171">Ha igen, kivéve, ha az IT-részleg 3306 portot nyit meg tooAzure MySQL-kiszolgáló nem lehet csatlakoztatni.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-171">If so, you cannot connect tooAzure MySQL server unless your IT department opens port 3306.</span></span>

## <a name="get-connection-information"></a><span data-ttu-id="eb2b3-172">Kapcsolatadatok lekérése</span><span class="sxs-lookup"><span data-stu-id="eb2b3-172">Get connection information</span></span>
<span data-ttu-id="eb2b3-173">Teljesen minősített Get hello **kiszolgálónév** és **kiszolgálói rendszergazda bejelentkezési név** hello Azure-portálon a MySQL-kiszolgáló az Azure-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-173">Get hello fully qualified **Server name** and **Server admin login name** for your Azure Database for MySQL server from hello Azure portal.</span></span> <span data-ttu-id="eb2b3-174">Hello teljesen minősített neve tooconnect tooyour kiszolgálók mysql parancssori eszközzel használja.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-174">You use hello fully qualified server name tooconnect tooyour server using mysql command-line tool.</span></span> 

1. <span data-ttu-id="eb2b3-175">A [Azure-portálon](https://portal.azure.com/), kattintson a **összes erőforrás** hello bal oldali menüből hello típusnév és keresse meg a MySQL-kiszolgálóhoz tartozó Azure-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-175">In [Azure portal](https://portal.azure.com/), click **All resources** from hello left-hand menu, type hello name, and search for your Azure Database for MySQL server.</span></span> <span data-ttu-id="eb2b3-176">Válassza ki a hello kiszolgáló neve tooview hello adatait.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-176">Select hello server name tooview hello details.</span></span>

2. <span data-ttu-id="eb2b3-177">A hello beállítások elemcsoportban kattintson **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-177">Under hello Settings heading, click **Properties**.</span></span> <span data-ttu-id="eb2b3-178">Jegyezze fel **kiszolgálónév** és **KISZOLGÁLÓI rendszergazda bejelentkezési név**.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-178">Note down **SERVER NAME** and **SERVER ADMIN LOGIN NAME**.</span></span> <span data-ttu-id="eb2b3-179">Hello Másolás gombra következő tooeach mező toocopy toohello vágólapra kattintva.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-179">You may click hello copy button next tooeach field toocopy toohello clipboard.</span></span>
   <span data-ttu-id="eb2b3-180">![4-2 kiszolgáló tulajdonságai](./media/tutorial-design-database-using-portal/4_2-server-properties.png)</span><span class="sxs-lookup"><span data-stu-id="eb2b3-180">![4-2 server properties](./media/tutorial-design-database-using-portal/4_2-server-properties.png)</span></span>

<span data-ttu-id="eb2b3-181">Ebben a példában hello kiszolgáló neve, *myserver4demo.mysql.database.azure.com*, és a kiszolgáló-rendszergazdai bejelentkezés hello  *myadmin@myserver4demo* .</span><span class="sxs-lookup"><span data-stu-id="eb2b3-181">In this example, hello server name is *myserver4demo.mysql.database.azure.com*, and hello server admin login is *myadmin@myserver4demo*.</span></span>

## <a name="connect-toohello-server-using-mysql"></a><span data-ttu-id="eb2b3-182">Csatlakoztassa a kiszolgálót a mysql használata toohello</span><span class="sxs-lookup"><span data-stu-id="eb2b3-182">Connect toohello server using mysql</span></span>
<span data-ttu-id="eb2b3-183">Használjon [mysql parancssori eszköz](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) tooestablish MySQL-kiszolgáló kapcsolati tooyour Azure-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-183">Use [mysql command-line tool](https://dev.mysql.com/doc/refman/5.7/en/mysql.html) tooestablish a connection tooyour Azure Database for MySQL server.</span></span> <span data-ttu-id="eb2b3-184">Hello mysql parancssori eszköz hello Azure Cloud rendszerhéj hello böngészőben, illetve a saját gép mysql-eszközök telepítve helyileg futtathatja.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-184">You can run hello mysql command-line tool from hello Azure Cloud Shell in hello browser or from your own machine using mysql tools installed locally.</span></span> <span data-ttu-id="eb2b3-185">toolaunch hello Azure Cloud rendszerhéj, kattintson a hello `Try It` ebben a cikkben egy kódblokk gombra, vagy keresse fel a hello Azure-portálon, és kattintson a hello `>_` hello felső, jobb eszköztárban látható ikonra.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-185">toolaunch hello Azure Cloud Shell, click hello `Try It` button on a code block in this article, or visit hello Azure portal and click hello `>_` icon in hello top right toolbar.</span></span> 

<span data-ttu-id="eb2b3-186">Írja be a hello parancs tooconnect:</span><span class="sxs-lookup"><span data-stu-id="eb2b3-186">Type hello command tooconnect:</span></span>
```azurecli-interactive
mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

## <a name="create-a-blank-database"></a><span data-ttu-id="eb2b3-187">Hozzon létre egy üres adatbázist</span><span class="sxs-lookup"><span data-stu-id="eb2b3-187">Create a blank database</span></span>
<span data-ttu-id="eb2b3-188">Ha már csatlakoztatott toohello kiszolgáló, hozzon létre egy üres adatbázist toowork rendelkező.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-188">Once you’re connected toohello server, create a blank database toowork with.</span></span>
```sql
CREATE DATABASE mysampledb;
```

<span data-ttu-id="eb2b3-189">Hello a parancssorból futtassa a következő parancs tooswitch kapcsolat található, újonnan létrehozott toothis adatbázis hello:</span><span class="sxs-lookup"><span data-stu-id="eb2b3-189">At hello prompt, run hello following command tooswitch connection toothis newly created database:</span></span>
```sql
USE mysampledb;
```

## <a name="create-tables-in-hello-database"></a><span data-ttu-id="eb2b3-190">Táblázatok létrehozása hello adatbázis</span><span class="sxs-lookup"><span data-stu-id="eb2b3-190">Create tables in hello database</span></span>
<span data-ttu-id="eb2b3-191">Most, hogy tudja, hogyan tooconnect toohello Azure adatbázis MySQL-adatbázis, azt is ismerteti hogyan toocomplete néhány alapvető műveleteket.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-191">Now that you know how tooconnect toohello Azure Database for MySQL database, we can go over how toocomplete some basic tasks.</span></span>

<span data-ttu-id="eb2b3-192">Először hogy hozzon létre egy táblát, és töltse be adatokkal.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-192">First, we can create a table and load it with some data.</span></span> <span data-ttu-id="eb2b3-193">Hozzon létre egy tábla tárolja a leltáradatokat.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-193">Let's create a table that stores inventory information.</span></span>
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-hello-tables"></a><span data-ttu-id="eb2b3-194">Adatok betöltése az hello táblák</span><span class="sxs-lookup"><span data-stu-id="eb2b3-194">Load data into hello tables</span></span>
<span data-ttu-id="eb2b3-195">Most, hogy a tábla, azt beilleszthet néhány adat azt.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-195">Now that we have a table, we can insert some data into it.</span></span> <span data-ttu-id="eb2b3-196">Hello nyissa meg a parancssort időszakban futtassa a következő lekérdezés tooinsert hello néhány sornyi adatot.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-196">At hello open command prompt window, run hello following query tooinsert some rows of data.</span></span>
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

<span data-ttu-id="eb2b3-197">Most már rendelkezik a korábban létrehozott hello táblába mintaadatok két sorát.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-197">Now you have two rows of sample data into hello table you created earlier.</span></span>

## <a name="query-and-update-hello-data-in-hello-tables"></a><span data-ttu-id="eb2b3-198">Hello táblázatok lekérdezési és frissítési hello adatait</span><span class="sxs-lookup"><span data-stu-id="eb2b3-198">Query and update hello data in hello tables</span></span>
<span data-ttu-id="eb2b3-199">Hajtható végre a következő lekérdezés tooretrieve információ hello adatbázis táblából hello.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-199">Execute hello following query tooretrieve information from hello database table.</span></span>
```sql
SELECT * FROM inventory;
```

<span data-ttu-id="eb2b3-200">Hello hello-táblázatok adatait is frissítheti.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-200">You can also update hello data in hello tables.</span></span>
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

<span data-ttu-id="eb2b3-201">az adatok hello sor ennek megfelelően frissül.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-201">hello row gets updated accordingly when you retrieve data.</span></span>
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-tooa-previous-point-in-time"></a><span data-ttu-id="eb2b3-202">Egy adatbázis tooa korábbi időpontra időbeli visszaállítása</span><span class="sxs-lookup"><span data-stu-id="eb2b3-202">Restore a database tooa previous point in time</span></span>
<span data-ttu-id="eb2b3-203">Tegyük fel, egy fontos adatbázistábla véletlenül törölt, és az adatok nem állíthatók vissza hello könnyen.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-203">Imagine you have accidentally deleted an important database table, and cannot recover hello data easily.</span></span> <span data-ttu-id="eb2b3-204">Azure MySQL-adatbázis lehetővé teszi toorestore hello server tooa pont időben hello adatbázis másolatának létrehozása az új kiszolgálóra.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-204">Azure Database for MySQL allows you toorestore hello server tooa point in time, creating a copy of hello databases into new server.</span></span> <span data-ttu-id="eb2b3-205">Az új kiszolgáló toorecover használhatja a törölt adatokat.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-205">You can use this new server toorecover your deleted data.</span></span> <span data-ttu-id="eb2b3-206">hello lépéseket visszaállítási hello minta kiszolgáló tooa pont követően hello tábla hozzá lett adva.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-206">hello following steps restore hello sample server tooa point before hello table was added.</span></span>

1. <span data-ttu-id="eb2b3-207">Hello Azure-portálon keresse meg a MySQL az Azure-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-207">In hello Azure portal, locate your Azure Database for MySQL.</span></span> <span data-ttu-id="eb2b3-208">A hello **áttekintése** kattintson **visszaállítása** hello eszköztáron.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-208">On hello **Overview** page, click **Restore** on hello toolbar.</span></span> <span data-ttu-id="eb2b3-209">hello visszaállítási lap nyílik meg.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-209">hello Restore page opens.</span></span>

   ![10-1 visszaállítási adatbázis](./media/tutorial-design-database-using-portal/10_1-restore-a-db.png)

2. <span data-ttu-id="eb2b3-211">Töltse ki a hello **visszaállítása** hello szükséges információt a képernyőn.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-211">Fill out hello **Restore** form with hello required information.</span></span>
   
   ![10-2 visszaállítási képernyő](./media/tutorial-design-database-using-portal/10_2-restore-form.png)
   
   - <span data-ttu-id="eb2b3-213">**Visszaállítási pont**: válassza ki a-időpontban, amelyet az toorestore, felsorolt hello időkereten belül.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-213">**Restore point**: Select a point-in-time that you want toorestore to, within hello timeframe listed.</span></span> <span data-ttu-id="eb2b3-214">Győződjön meg arról, hogy tooconvert a helyi időzónára tooUTC.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-214">Make sure tooconvert your local timezone tooUTC.</span></span>
   - <span data-ttu-id="eb2b3-215">**Toonew-kiszolgáló helyreállítása**: Adjon meg egy új azt szeretné, hogy toorestore kiszolgálónevet.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-215">**Restore toonew server**: Provide a new server name you want toorestore to.</span></span>
   - <span data-ttu-id="eb2b3-216">**Hely**: hello a régióban legyen, mint a forráskiszolgálón hello, és nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-216">**Location**: hello region is same as hello source server, and cannot be changed.</span></span>
   - <span data-ttu-id="eb2b3-217">**IP-címek**: tarifacsomag hello hello hello azonos a forrás kiszolgálón, és nem módosítható.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-217">**Pricing tier**: hello pricing tier is hello same as hello source server, and cannot be changed.</span></span>
   
3. <span data-ttu-id="eb2b3-218">Kattintson a **OK** toorestore hello server túl[visszaállítási tooa pont időben](./howto-restore-server-portal.md) előtt hello tábla törölve lett.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-218">Click **OK** toorestore hello server too[restore tooa point in time](./howto-restore-server-portal.md) before hello table was deleted.</span></span> <span data-ttu-id="eb2b3-219">A kiszolgáló visszaállítása másolatot hoz létre új hello kiszolgáló frissítésétől hello pont időpontban.</span><span class="sxs-lookup"><span data-stu-id="eb2b3-219">Restoring a server creates a new copy of hello server, as of hello point in time you specify.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="eb2b3-220">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="eb2b3-220">Next steps</span></span>
<span data-ttu-id="eb2b3-221">Ebben az oktatóanyagban használhatja az Azure portál toolearned hello hogyan számára:</span><span class="sxs-lookup"><span data-stu-id="eb2b3-221">In this tutorial, you use hello Azure portal toolearned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="eb2b3-222">A MySQL az Azure-adatbázis létrehozása</span><span class="sxs-lookup"><span data-stu-id="eb2b3-222">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="eb2b3-223">Hello kiszolgáló tűzfal konfigurálása</span><span class="sxs-lookup"><span data-stu-id="eb2b3-223">Configure hello server firewall</span></span>
> * <span data-ttu-id="eb2b3-224">Mysql parancssori eszköz toocreate adatbázis használata</span><span class="sxs-lookup"><span data-stu-id="eb2b3-224">Use mysql command-line tool toocreate a database</span></span>
> * <span data-ttu-id="eb2b3-225">Mintaadatok betöltése</span><span class="sxs-lookup"><span data-stu-id="eb2b3-225">Load sample data</span></span>
> * <span data-ttu-id="eb2b3-226">Adatok lekérdezése</span><span class="sxs-lookup"><span data-stu-id="eb2b3-226">Query data</span></span>
> * <span data-ttu-id="eb2b3-227">Adatok frissítése</span><span class="sxs-lookup"><span data-stu-id="eb2b3-227">Update data</span></span>
> * <span data-ttu-id="eb2b3-228">Adatok visszaállítása</span><span class="sxs-lookup"><span data-stu-id="eb2b3-228">Restore data</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="eb2b3-229">Hogyan tooconnect alkalmazások tooAzure MySQL adatbázist</span><span class="sxs-lookup"><span data-stu-id="eb2b3-229">How tooconnect applications tooAzure Database for MySQL</span></span>](./howto-connection-string.md)
