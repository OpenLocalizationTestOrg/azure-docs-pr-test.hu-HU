---
title: "SSL-kapcsolatot a MySQL az Azure-adatbázis |} Microsoft Docs"
description: "Azure-adatbázis a MySQL és a társított alkalmazások megfelelően használni az SSL-kapcsolatok konfigurálását."
services: mysql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 4b03b3a2dbfad92cc0cfa84777b38ddff90452cf
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a><span data-ttu-id="bc3a3-103">SSL-kapcsolatot MySQL az Azure-adatbázis</span><span class="sxs-lookup"><span data-stu-id="bc3a3-103">SSL connectivity in Azure Database for MySQL</span></span>
<span data-ttu-id="bc3a3-104">Azure MySQL-adatbázis támogatja az adatbázis-kiszolgáló csatlakozik a Secure Sockets Layer (SSL) használó ügyfélalkalmazások.</span><span class="sxs-lookup"><span data-stu-id="bc3a3-104">Azure Database for MySQL supports connecting your database server to client applications using Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="bc3a3-105">Az adatbázis-kiszolgáló és az ügyfél alkalmazások közötti SSL-kapcsolatok kényszerítése segít lánctámadások elleni védelem érdekében "man a középső" az adatfolyamot a kiszolgáló és az alkalmazás közötti titkosításával.</span><span class="sxs-lookup"><span data-stu-id="bc3a3-105">Enforcing SSL connections between your database server and your client applications helps protect against "man in the middle" attacks by encrypting the data stream between the server and your application.</span></span>

## <a name="default-settings"></a><span data-ttu-id="bc3a3-106">Alapértelmezett beállítások</span><span class="sxs-lookup"><span data-stu-id="bc3a3-106">Default settings</span></span>
<span data-ttu-id="bc3a3-107">Alapértelmezés szerint az adatbázis-szolgáltatás úgy konfigurálni, hogy SSL-kapcsolatok megkövetelése MySQL történő csatlakozás során.</span><span class="sxs-lookup"><span data-stu-id="bc3a3-107">By default, the database service should be configured to require SSL connections when connecting to MySQL.</span></span>  <span data-ttu-id="bc3a3-108">Javasoljuk, ne tiltsa le az SSL-beállítást, amikor csak lehetséges.</span><span class="sxs-lookup"><span data-stu-id="bc3a3-108">It is recommended avoid disabling the SSL option whenever possible.</span></span> 

<span data-ttu-id="bc3a3-109">Ha egy új Azure Database az Azure portálon keresztül MySQL-kiszolgáló és a parancssori felület, SSL-kapcsolatok végrehajtásának alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="bc3a3-109">When provisioning a new Azure Database for MySQL server through the Azure portal and CLI, enforcement of SSL connections is enabled by default.</span></span> 

<span data-ttu-id="bc3a3-110">Hasonlóképpen a "Kapcsolati karakterláncok" beállításokat a kiszolgálón az Azure-portálon előre definiált kapcsolati karakterláncok közös nyelvek SSL használatával az adatbázis-kiszolgálóhoz való csatlakozáshoz szükséges paraméterek közé tartoznak.</span><span class="sxs-lookup"><span data-stu-id="bc3a3-110">Likewise, connection strings that are pre-defined in the "Connection Strings" settings under your server in the Azure portal include the required parameters for common languages to connect to your database server using SSL.</span></span> <span data-ttu-id="bc3a3-111">Az SSL-paraméter attól függően változik, az összekötő, például "ssl = true" vagy "sslmode = szükséges" vagy "sslmode = szükséges" és egyéb változatok.</span><span class="sxs-lookup"><span data-stu-id="bc3a3-111">The SSL parameter varies based on the connector, for example "ssl=true" or "sslmode=require" or "sslmode=required" and other variations.</span></span>

<span data-ttu-id="bc3a3-112">Annak megismerése, hogyan engedélyezheti vagy tilthatja le az SSL-kapcsolatot, alkalmazások fejlesztése során, tekintse meg [SSL konfigurálása](howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="bc3a3-112">To learn how to enable or disable SSL connection when developing application, please refer to [How to configure SSL](howto-configure-ssl.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="bc3a3-113">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="bc3a3-113">Next steps</span></span>
[<span data-ttu-id="bc3a3-114">Adatkapcsolattárak MySQL az Azure-adatbázis</span><span class="sxs-lookup"><span data-stu-id="bc3a3-114">Connection libraries for Azure Database for MySQL</span></span>](concepts-connection-libraries.md)
