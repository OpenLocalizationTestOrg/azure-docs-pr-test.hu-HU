---
title: "aaaConnect meglévő Azure App Service tooAzure MySQL adatbázis |} Microsoft Docs"
description: "Hogyan tooproperly egy meglévő Azure App Service tooAzure adatbázis kapcsolati MySQL utasításokat"
services: mysql
author: v-chenyh
ms.author: v-chenyh
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/23/2017
ms.openlocfilehash: 6d5b16f316e186d665370adcd8b7c7bb38c8d51a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="connect-an-existing-azure-app-service-tooazure-database-for-mysql-server"></a><span data-ttu-id="57a5f-103">Csatlakozzon egy létező Azure App Service tooAzure adatbázis MySQL-kiszolgáló</span><span class="sxs-lookup"><span data-stu-id="57a5f-103">Connect an existing Azure App Service tooAzure Database for MySQL server</span></span>
<span data-ttu-id="57a5f-104">Ez a dokumentum azt ismerteti, hogyan tooconnect egy meglévő Azure App Service tooyour Azure MySQL-kiszolgáló adatbázisában.</span><span class="sxs-lookup"><span data-stu-id="57a5f-104">This document explains how tooconnect an existing Azure App Service tooyour Azure Database for MySQL server.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="57a5f-105">Előkészületek</span><span class="sxs-lookup"><span data-stu-id="57a5f-105">Before you begin</span></span>
<span data-ttu-id="57a5f-106">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="57a5f-106">Log in toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="57a5f-107">MySQL-kiszolgáló Azure-adatbázis létrehozása.</span><span class="sxs-lookup"><span data-stu-id="57a5f-107">Create an Azure Database for MySQL server.</span></span> <span data-ttu-id="57a5f-108">Részletekért lásd a túl[hogyan toocreate Azure portálról MySQL-kiszolgáló adatbázisában](quickstart-create-mysql-server-database-using-azure-portal.md) vagy [hogyan toocreate Azure parancssori felület használatával MySQL-kiszolgáló adatbázisában](quickstart-create-mysql-server-database-using-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="57a5f-108">For details, refer too[How toocreate Azure Database for MySQL server from Portal](quickstart-create-mysql-server-database-using-azure-portal.md) or [How toocreate Azure Database for MySQL server using CLI](quickstart-create-mysql-server-database-using-azure-cli.md).</span></span>

<span data-ttu-id="57a5f-109">Jelenleg nincsenek két megoldások tooenable hozzáférés az Azure App Service tooan Azure-adatbázis a MySQL.</span><span class="sxs-lookup"><span data-stu-id="57a5f-109">Currently there are two solutions tooenable access from an Azure App Service tooan Azure Database for MySQL.</span></span> <span data-ttu-id="57a5f-110">A két megoldás tartalmaz, amely kiszolgálószintű tűzfal-szabályok beállítása.</span><span class="sxs-lookup"><span data-stu-id="57a5f-110">Both solutions involve setting up server-level firewall rules.</span></span>

## <a name="solution-1---create-a-firewall-rule-tooallow-all-ips"></a><span data-ttu-id="57a5f-111">1 - megoldás hozzon létre egy tűzfal szabály tooallow összes IP-címek</span><span class="sxs-lookup"><span data-stu-id="57a5f-111">Solution 1 - Create a firewall rule tooallow all IPs</span></span>
<span data-ttu-id="57a5f-112">Azure MySQL-adatbázis egy tűzfal tooprotect használatával az adatok biztonsági biztosít.</span><span class="sxs-lookup"><span data-stu-id="57a5f-112">Azure Database for MySQL provides access security using a firewall tooprotect your data.</span></span> <span data-ttu-id="57a5f-113">Ha a MySQL-kiszolgáló, az Azure App Service tooAzure adatbázis csatlakoztatása vegye figyelembe, hogy hello kimenő IP-címek az App Service dinamikus jellegű.</span><span class="sxs-lookup"><span data-stu-id="57a5f-113">When connecting from an Azure App Service tooAzure Database for MySQL server, keep in mind that hello outbound IPs of App Service are dynamic in nature.</span></span> 

<span data-ttu-id="57a5f-114">tooensure hello rendelkezésre állását az Azure App Service, azt javasoljuk, a megoldás tooallow összes IP-címek.</span><span class="sxs-lookup"><span data-stu-id="57a5f-114">tooensure hello availability of your Azure App Service, we recommend using this solution tooallow ALL IPs.</span></span>

> [!NOTE]
> <span data-ttu-id="57a5f-115">A hosszú távú megoldás tooavoid lehetővé teszi az összes IP-címek az Azure-szolgáltatások tooconnect tooAzure adatbázis MySQL a Microsoft szolgáltatás működik.</span><span class="sxs-lookup"><span data-stu-id="57a5f-115">Microsoft is working for a long-term solution tooavoid allowing all IPs for Azure services tooconnect tooAzure Database for MySQL.</span></span>

1. <span data-ttu-id="57a5f-116">Hello MySQL server panelen, a beállítások elemcsoportban kattintson **kapcsolatbiztonsági** tooopen hello kapcsolatbiztonsági panel hello MySQL az Azure-adatbázis.</span><span class="sxs-lookup"><span data-stu-id="57a5f-116">On hello MySQL server blade, under Settings heading, click **Connection Security** tooopen hello Connection Security blade for hello Azure Database for MySQL.</span></span>

   ![Azure portál – kattintson a kapcsolat biztonságát](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. <span data-ttu-id="57a5f-118">Adja meg **SZABÁLYNÉV**, **KEZDŐ IP**, és **záró IP-Címnél**.</span><span class="sxs-lookup"><span data-stu-id="57a5f-118">Enter **RULE NAME**, **START IP**, and **END IP**.</span></span> <span data-ttu-id="57a5f-119">Ezután kattintson a **Save** (Mentés) gombra.</span><span class="sxs-lookup"><span data-stu-id="57a5f-119">Then click **Save**.</span></span>
   - <span data-ttu-id="57a5f-120">A szabálynév: engedélyezése – minden-IP-címek</span><span class="sxs-lookup"><span data-stu-id="57a5f-120">Rule name: Allow-All-IPs</span></span>
   - <span data-ttu-id="57a5f-121">IP elindítása: 0.0.0.0</span><span class="sxs-lookup"><span data-stu-id="57a5f-121">Start IP: 0.0.0.0</span></span>
   - <span data-ttu-id="57a5f-122">Záró IP: 255.255.255.255</span><span class="sxs-lookup"><span data-stu-id="57a5f-122">End IP: 255.255.255.255</span></span>

   ![Azure portál – az összes IP-címek hozzáadása](./media/howto-connect-webapp/1_2-add-all-ips.png)

## <a name="solution-2---create-a-firewall-rule-tooexplicitly-allow-outbound-ips"></a><span data-ttu-id="57a5f-124">2 - megoldás létrehozása a tűzfal szabály tooexplicitly kimenő IP-címek engedélyezése</span><span class="sxs-lookup"><span data-stu-id="57a5f-124">Solution 2 - Create a firewall rule tooexplicitly allow outbound IPs</span></span>
<span data-ttu-id="57a5f-125">Az összes kimenő IP-címek az Azure App Service hello explicit módon adhat.</span><span class="sxs-lookup"><span data-stu-id="57a5f-125">You can explicitly add all hello outbound IPs of your Azure App Service.</span></span>

1. <span data-ttu-id="57a5f-126">A hello App Service tulajdonságok panelére léphet, tekintse meg a **kimenő IP-cím**.</span><span class="sxs-lookup"><span data-stu-id="57a5f-126">On hello App Service Properties blade, view your **OUTBOUND IP ADDRESS**.</span></span>

   ![Azure portál – nézet kimenő IP-címek](./media/howto-connect-webapp/2_1-outbound-ip-address.png)

2. <span data-ttu-id="57a5f-128">Hello MySQL-kapcsolat biztonsági panelen vegye fel a kimenő IP-címet egyenként.</span><span class="sxs-lookup"><span data-stu-id="57a5f-128">On hello MySQL Connection security blade, add outbound IPs one by one.</span></span>

   ![Azure portál – explicit IP-címek hozzáadása](./media/howto-connect-webapp/2_2-add-explicit-ips.png)

3. <span data-ttu-id="57a5f-130">Ne feledje túl**mentése** a tűzfalszabályok között.</span><span class="sxs-lookup"><span data-stu-id="57a5f-130">Remember too**Save** your firewall rules.</span></span>

<span data-ttu-id="57a5f-131">Bár a hello Azure App service tookeep IP-címek állandó próbál idővel, vannak esetek, ahol hello IP-címek megváltozhatnak.</span><span class="sxs-lookup"><span data-stu-id="57a5f-131">Though hello Azure App service attempts tookeep IP addresses constant over time, there are cases where hello IP addresses may change.</span></span> <span data-ttu-id="57a5f-132">Például ha hello app újrahasznosítja azt, vagy akkor fordul elő, a méretezési művelet, amikor új gépeket adnak a regionális az Azure data adatközpontok tooincrease hello kapacitás.</span><span class="sxs-lookup"><span data-stu-id="57a5f-132">For example, when hello app recycles or a scale operation occurs, or when new machines are added in Azure regional data centers tooincrease hello capacity.</span></span> <span data-ttu-id="57a5f-133">Ha módosulnak hello IP-címek, hello app sikerült leállásra hello esemény már nem képes kapcsolódni a toohello MySQL kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="57a5f-133">When hello IP addresses change, hello app could experience downtime in hello event it can no longer connect toohello MySQL server.</span></span> <span data-ttu-id="57a5f-134">Vegye figyelembe a lehetséges megoldások megelőző hello közül választva.</span><span class="sxs-lookup"><span data-stu-id="57a5f-134">Consider this potential when choosing one of hello preceding solutions.</span></span>

## <a name="ssl-configuration"></a><span data-ttu-id="57a5f-135">SSL-beállítása</span><span class="sxs-lookup"><span data-stu-id="57a5f-135">SSL configuration</span></span>
<span data-ttu-id="57a5f-136">Azure MySQL-adatbázis SSL engedélyezve alapértelmezés szerint rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="57a5f-136">Azure Database for MySQL has SSL Enabled by default.</span></span> <span data-ttu-id="57a5f-137">Ha az alkalmazás nem használ SSL tooconnect toohello adatbázis, akkor szüksége toodisable SSL MySQL-kiszolgálón.</span><span class="sxs-lookup"><span data-stu-id="57a5f-137">If your application is not using SSL tooconnect toohello database, then you need toodisable SSL on MySQL server.</span></span> <span data-ttu-id="57a5f-138">További részletekért tooconfigure SSL, lásd: [SSL használatával MySQL az Azure-adatbázissal](howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="57a5f-138">For details on how tooconfigure SSL, See [Using SSL with Azure Database for MySQL](howto-configure-ssl.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="57a5f-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="57a5f-139">Next steps</span></span>
<span data-ttu-id="57a5f-140">Kapcsolati karakterláncok kapcsolatos további információkért tekintse meg túl[kapcsolati karakterláncok](howto-connection-string.md).</span><span class="sxs-lookup"><span data-stu-id="57a5f-140">For more information about connection strings, refer too[Connection Strings](howto-connection-string.md).</span></span>
