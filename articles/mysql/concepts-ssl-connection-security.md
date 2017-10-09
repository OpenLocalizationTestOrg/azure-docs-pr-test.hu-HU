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
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>SSL-kapcsolatot MySQL az Azure-adatbázis
MySQL az Azure-adatbázishoz való csatlakozást az adatbázis-kiszolgáló tooclient alkalmazások Secure Sockets Layer (SSL) használatával támogatja. Az adatbázis-kiszolgáló és az ügyfél alkalmazások közötti SSL-kapcsolatok kényszerítése megvédi a "hello középső man" támadások ellen hello adatfolyam hello kiszolgáló és az alkalmazás közötti titkosításával.

## <a name="default-settings"></a>Alapértelmezett beállítások
Alapértelmezés szerint hello adatbázis-szolgáltatás kell konfigurált toorequire SSL-kapcsolatok tooMySQL kapcsolódáskor.  Ajánlott, amikor csak lehetséges hello SSL-beállítást a letiltása. 

Ha egy új Azure-adatbázist a MySQL-kiszolgálóval hello Azure-portálon keresztül és a parancssori felület, SSL-kapcsolatok végrehajtásának alapértelmezés szerint engedélyezve van. 

Hasonlóképpen hello "Kapcsolati karakterláncok" beállításokat a kiszolgálón a hello Azure-portálon előre definiált kapcsolati karakterláncok tartalmaznak hello szükséges paraméterek közös nyelvek tooconnect tooyour adatbázis-kiszolgáló SSL-t használ. hello SSL paraméter függ hello összekötő, például "ssl = true" vagy "sslmode = szükséges" vagy "sslmode = szükséges" és egyéb változatok.

Hogyan tooenable vagy letilthatja az SSL-kapcsolatot, alkalmazások fejlesztése során tanulmányozza a túl toolearn[hogyan tooconfigure SSL](howto-configure-ssl.md).

## <a name="next-steps"></a>Következő lépések
[Adatkapcsolattárak MySQL az Azure-adatbázis](concepts-connection-libraries.md)
