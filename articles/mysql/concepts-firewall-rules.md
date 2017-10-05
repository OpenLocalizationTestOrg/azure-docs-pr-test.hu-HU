---
title: "Azure-adatbázis a MySQL-kiszolgáló tűzfalszabályainak |} Microsoft Docs"
description: "A témakör ismerteti a tűzfalszabályok a MySQL-kiszolgálóhoz tartozó Azure-adatbázis."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 7456cef7a5da665ee3d70df64265b8186a79f9b3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="azure-database-for-mysql-server-firewall-rules"></a><span data-ttu-id="1ce66-103">Kiszolgáló tűzfalszabályainak MySQL Azure-adatbázis</span><span class="sxs-lookup"><span data-stu-id="1ce66-103">Azure Database for MySQL server firewall rules</span></span>
<span data-ttu-id="1ce66-104">Tűzfalak tagadni a hozzáférést minden az adatbázis-kiszolgáló csak akkor adja meg, mely számítógépek rendelkeznek engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="1ce66-104">Firewalls prevent all access to your database server until you specify which computers have permission.</span></span> <span data-ttu-id="1ce66-105">A tűzfal engedélyezi a hozzáférést a kiszolgálóhoz, az egyes kérelmek az eredeti IP-címe alapján.</span><span class="sxs-lookup"><span data-stu-id="1ce66-105">The firewall grants access to the server based on the originating IP address of each request.</span></span>

<span data-ttu-id="1ce66-106">A tűzfal konfigurálásakor olyan tűzfalszabályokat adhat meg, amelyek meghatározzák az elfogadható IP-címtartományokat.</span><span class="sxs-lookup"><span data-stu-id="1ce66-106">To configure your firewall, you create firewall rules that specify ranges of acceptable IP addresses.</span></span> <span data-ttu-id="1ce66-107">Tűzfal-szabályokat hozhat létre a kiszolgálói szinten.</span><span class="sxs-lookup"><span data-stu-id="1ce66-107">You can create firewall rules at the server level.</span></span>

<span data-ttu-id="1ce66-108">**Tűzfal-szabályok:** ezek a szabályok hozzáférés engedélyezése ügyfelek számára a teljes Azure-adatbázis MySQL-kiszolgáló esetén ez azt jelenti, hogy összes adatbázis belül az azonos logikai kiszolgáló.</span><span class="sxs-lookup"><span data-stu-id="1ce66-108">**Firewall rules:** These rules enable clients to access your entire Azure Database for MySQL server, that is, all the databases within the same logical server.</span></span> <span data-ttu-id="1ce66-109">Kiszolgálószintű tűzfal-szabályokat konfigurálhatja az Azure-portál használatával vagy az Azure parancssori felület parancsait.</span><span class="sxs-lookup"><span data-stu-id="1ce66-109">Server-level firewall rules can be configured by using the Azure portal or using Azure CLI commands.</span></span> <span data-ttu-id="1ce66-110">Kiszolgálószintű tűzfal-szabályok létrehozása, az előfizetés tulajdonosa vagy egy előfizetés közreműködői kell lennie.</span><span class="sxs-lookup"><span data-stu-id="1ce66-110">To create server-level firewall rules, you must be the subscription owner or a subscription contributor.</span></span>

## <a name="firewall-overview"></a><span data-ttu-id="1ce66-111">Tűzfal áttekintése</span><span class="sxs-lookup"><span data-stu-id="1ce66-111">Firewall overview</span></span>
<span data-ttu-id="1ce66-112">Az adatbázis elérésére az Azure-adatbázis MySQL-kiszolgáló alapértelmezés szerint a tűzfal blokkolja.</span><span class="sxs-lookup"><span data-stu-id="1ce66-112">All database access to your Azure Database for MySQL server is blocked by the firewall by default.</span></span> <span data-ttu-id="1ce66-113">A kiszolgáló egy másik számítógépről használatának megkezdéséhez meg kell adnia egy vagy több kiszolgálószintű engedélyezésére szolgáló tűzfalszabályok a kiszolgáló elérését.</span><span class="sxs-lookup"><span data-stu-id="1ce66-113">To begin using your server from another computer, you need to specify one or more server-level firewall rules to enable access to your server.</span></span> <span data-ttu-id="1ce66-114">Használja a tűzfalszabályok adja meg, melyik IP-címtartományok engedélyezi az internetről.</span><span class="sxs-lookup"><span data-stu-id="1ce66-114">Use the firewall rules to specify which IP address ranges from the Internet to allow.</span></span> <span data-ttu-id="1ce66-115">A tűzfalszabályok nincs hatással a hozzáférést az Azure portál magát.</span><span class="sxs-lookup"><span data-stu-id="1ce66-115">Access to the Azure portal website itself is not impacted by the firewall rules.</span></span>

<span data-ttu-id="1ce66-116">Kapcsolódási kísérletek az internetről és az Azure először eltelik a tűzfalon keresztül azok képes elérni az Azure-adatbázis MySQL-adatbázis, az alábbi ábrán látható módon:</span><span class="sxs-lookup"><span data-stu-id="1ce66-116">Connection attempts from the Internet and Azure must first pass through the firewall before they can reach your Azure Database for MySQL database, as shown in the following diagram:</span></span>

![A tűzfal működése áramló – példa](./media/concepts-firewall-rules/1-firewall-concept.png)

## <a name="connecting-from-the-internet"></a><span data-ttu-id="1ce66-118">Csatlakozás az internetről</span><span class="sxs-lookup"><span data-stu-id="1ce66-118">Connecting from the Internet</span></span>
<span data-ttu-id="1ce66-119">Az Azure-adatbázishoz a MySQL-kiszolgálón lévő összes adatbázis kiszolgálószintű tűzfal-szabályokat alkalmazni.</span><span class="sxs-lookup"><span data-stu-id="1ce66-119">Server-level firewall rules apply to all databases on the Azure Database for MySQL server.</span></span>

<span data-ttu-id="1ce66-120">Ha a kérés IP-címe a kiszolgálószintű tűzfalszabályokban megadott tartományok egyikében található, a tűzfal engedélyezi a csatlakozást.</span><span class="sxs-lookup"><span data-stu-id="1ce66-120">If the IP address of the request is within one of the ranges specified in the server-level firewall rules, the connection is granted.</span></span>

<span data-ttu-id="1ce66-121">Ha az IP-cím, a kérelem nem az adatbázis-szintjére, vagy a kiszolgálószintű tűzfalszabály megadott tartományok belül van, a kapcsolódási kérelem sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="1ce66-121">If the IP address of the request is not within the ranges specified in any of the database-level or server-level firewall rules, the connection request fails.</span></span>

## <a name="programmatically-managing-firewall-rules"></a><span data-ttu-id="1ce66-122">Tűzfalszabályok szoftveres kezelése</span><span class="sxs-lookup"><span data-stu-id="1ce66-122">Programmatically managing firewall rules</span></span>
<span data-ttu-id="1ce66-123">Az Azure portálon kívül tűzfalszabályok programozott módon, Azure parancssori Felülettel kezelhetők.</span><span class="sxs-lookup"><span data-stu-id="1ce66-123">In addition to the Azure portal, firewall rules can be managed programmatically using Azure CLI.</span></span> <span data-ttu-id="1ce66-124">Lásd még: [hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályok Azure parancssori felület használatával](./howto-manage-firewall-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="1ce66-124">See also [Create and manage Azure Database for MySQL firewall rules using Azure CLI](./howto-manage-firewall-using-cli.md)</span></span>

## <a name="troubleshooting-the-database-firewall"></a><span data-ttu-id="1ce66-125">Az adatbázistűzfal hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="1ce66-125">Troubleshooting the database firewall</span></span>
<span data-ttu-id="1ce66-126">Amikor a Microsoft Azure-adatbázis eléréséhez a MySQL-kiszolgáló szolgáltatás nem a várt módon működnek várja, vegye figyelembe a következő szempontokat:</span><span class="sxs-lookup"><span data-stu-id="1ce66-126">Consider the following points when access to the Microsoft Azure Database for MySQL server service does not behave as you expect:</span></span>

* <span data-ttu-id="1ce66-127">**Az engedélyezési listán módosításai nem lépnek érvénybe még:** lehet, mint amennyit egy 5 perces késleltetést használ az vált, az Azure-adatbázishoz a MySQL-kiszolgáló tűzfal konfigurációjának érvénybe léptetéséhez.</span><span class="sxs-lookup"><span data-stu-id="1ce66-127">**Changes to the allow list have not taken effect yet:** There may be as much as a five-minute delay for changes to the Azure Database for MySQL Server firewall configuration to take effect.</span></span>

* <span data-ttu-id="1ce66-128">**A bejelentkezés nem engedélyezett, vagy helytelen jelszót használt:** egy bejelentkezési azonosító nem rendelkezik engedélyekkel a MySQL-kiszolgálóhoz tartozó Azure-adatbázis, vagy a használt jelszó nem megfelelő, ha a kapcsolat a MySQL-kiszolgálóhoz tartozó Azure-adatbázis megtagadva.</span><span class="sxs-lookup"><span data-stu-id="1ce66-128">**The login is not authorized or an incorrect password was used:** If a login does not have permissions on the Azure Database for MySQL server or the password used is incorrect, the connection to the Azure Database for MySQL server is denied.</span></span> <span data-ttu-id="1ce66-129">Egy tűzfalbeállítás létrehozása lehetővé teszi az ügyfelek számára a kiszolgálóhoz való csatlakozás megkísérlését, azonban minden egyes ügyfélnek meg kell adnia a szükséges biztonsági hitelesítő adatokat.</span><span class="sxs-lookup"><span data-stu-id="1ce66-129">Creating a firewall setting only provides clients with an opportunity to attempt connecting to your server; each client must provide the necessary security credentials.</span></span>

* <span data-ttu-id="1ce66-130">**Dinamikus IP-cím**: Ha az internetkapcsolata dinamikus IP-címkezeléssel rendelkezik, és problémákat okoz a tűzfalon való átjutás, próbálja ki a következő megoldások valamelyikét:</span><span class="sxs-lookup"><span data-stu-id="1ce66-130">**Dynamic IP address:** If you have an Internet connection with dynamic IP addressing and you are having trouble getting through the firewall, you could try one of the following solutions:</span></span>

* <span data-ttu-id="1ce66-131">Az internetszolgáltató (ISP) kérjen a MySQL-kiszolgálóhoz tartozó Azure-adatbázis elérő ügyfélszámítógépeken rendelt IP-címtartományt, és adja hozzá az IP-címtartományt, egy tűzfalszabályt.</span><span class="sxs-lookup"><span data-stu-id="1ce66-131">Ask your Internet Service Provider (ISP) for the IP address range assigned to your client computers that access the Azure Database for MySQL server, and then add the IP address range as a firewall rule.</span></span>

* <span data-ttu-id="1ce66-132">Állítson be statikus IP-címeket az ügyfélszámítógépei számára, majd adja meg az IP-címeket tűzfalszabályokként.</span><span class="sxs-lookup"><span data-stu-id="1ce66-132">Get static IP addressing instead for your client computers, and then add the IP addresses as firewall rules.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1ce66-133">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1ce66-133">Next steps</span></span>

<span data-ttu-id="1ce66-134">[Hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályokat az Azure portál használatával](./howto-manage-firewall-using-portal.md)
[hozzon létre és kezelheti az Azure-adatbázis MySQL tűzfalszabályok Azure parancssori felület használatával](./howto-manage-firewall-using-cli.md)</span><span class="sxs-lookup"><span data-stu-id="1ce66-134">[Create and manage Azure Database for MySQL firewall rules using the Azure portal](./howto-manage-firewall-using-portal.md)
[Create and manage Azure Database for MySQL firewall rules using Azure CLI](./howto-manage-firewall-using-cli.md)</span></span>
