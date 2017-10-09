---
title: "MySQL az Azure-adatbázis aaaSSL kapcsolat |} Microsoft Docs"
description: "Információk az Azure-adatbázis beállítása a MySQL és a társított alkalmazás tooproperly SSL-kapcsolat használata"
services: mysql
author: JasonMAnderson
ms.author: janders
editor: jasonwhowell
manager: jhubbard
ms.service: mysql-database
ms.topic: article
ms.date: 05/10/2017
ms.openlocfilehash: 6fca7c88fc0f1fd6058d68fcff90fd409abd97a7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a><span data-ttu-id="74af1-103">SSL-kapcsolatot MySQL az Azure-adatbázis</span><span class="sxs-lookup"><span data-stu-id="74af1-103">SSL connectivity in Azure Database for MySQL</span></span>
<span data-ttu-id="74af1-104">MySQL az Azure-adatbázishoz való csatlakozást az adatbázis-kiszolgáló tooclient alkalmazások Secure Sockets Layer (SSL) használatával támogatja.</span><span class="sxs-lookup"><span data-stu-id="74af1-104">Azure Database for MySQL supports connecting your database server tooclient applications using Secure Sockets Layer (SSL).</span></span> <span data-ttu-id="74af1-105">Az adatbázis-kiszolgáló és az ügyfél alkalmazások közötti SSL-kapcsolatok kényszerítése megvédi a "hello középső man" támadások ellen hello adatfolyam hello kiszolgáló és az alkalmazás közötti titkosításával.</span><span class="sxs-lookup"><span data-stu-id="74af1-105">Enforcing SSL connections between your database server and your client applications helps protect against "man in hello middle" attacks by encrypting hello data stream between hello server and your application.</span></span>

## <a name="default-settings"></a><span data-ttu-id="74af1-106">Alapértelmezett beállítások</span><span class="sxs-lookup"><span data-stu-id="74af1-106">Default settings</span></span>
<span data-ttu-id="74af1-107">Alapértelmezés szerint hello adatbázis-szolgáltatás kell konfigurált toorequire SSL-kapcsolatok tooMySQL kapcsolódáskor.</span><span class="sxs-lookup"><span data-stu-id="74af1-107">By default, hello database service should be configured toorequire SSL connections when connecting tooMySQL.</span></span>  <span data-ttu-id="74af1-108">Ajánlott, amikor csak lehetséges hello SSL-beállítást a letiltása.</span><span class="sxs-lookup"><span data-stu-id="74af1-108">It is recommended avoid disabling hello SSL option whenever possible.</span></span> 

<span data-ttu-id="74af1-109">Ha egy új Azure-adatbázist a MySQL-kiszolgálóval hello Azure-portálon keresztül és a parancssori felület, SSL-kapcsolatok végrehajtásának alapértelmezés szerint engedélyezve van.</span><span class="sxs-lookup"><span data-stu-id="74af1-109">When provisioning a new Azure Database for MySQL server through hello Azure portal and CLI, enforcement of SSL connections is enabled by default.</span></span> 

<span data-ttu-id="74af1-110">Hasonlóképpen hello "Kapcsolati karakterláncok" beállításokat a kiszolgálón a hello Azure-portálon előre definiált kapcsolati karakterláncok tartalmaznak hello szükséges paraméterek közös nyelvek tooconnect tooyour adatbázis-kiszolgáló SSL-t használ.</span><span class="sxs-lookup"><span data-stu-id="74af1-110">Likewise, connection strings that are pre-defined in hello "Connection Strings" settings under your server in hello Azure portal include hello required parameters for common languages tooconnect tooyour database server using SSL.</span></span> <span data-ttu-id="74af1-111">hello SSL paraméter függ hello összekötő, például "ssl = true" vagy "sslmode = szükséges" vagy "sslmode = szükséges" és egyéb változatok.</span><span class="sxs-lookup"><span data-stu-id="74af1-111">hello SSL parameter varies based on hello connector, for example "ssl=true" or "sslmode=require" or "sslmode=required" and other variations.</span></span>

<span data-ttu-id="74af1-112">Hogyan tooenable vagy letilthatja az SSL-kapcsolatot, alkalmazások fejlesztése során tanulmányozza a túl toolearn[hogyan tooconfigure SSL](howto-configure-ssl.md).</span><span class="sxs-lookup"><span data-stu-id="74af1-112">toolearn how tooenable or disable SSL connection when developing application, please refer too[How tooconfigure SSL](howto-configure-ssl.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="74af1-113">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="74af1-113">Next steps</span></span>
[<span data-ttu-id="74af1-114">Adatkapcsolattárak MySQL az Azure-adatbázis</span><span class="sxs-lookup"><span data-stu-id="74af1-114">Connection libraries for Azure Database for MySQL</span></span>](concepts-connection-libraries.md)
