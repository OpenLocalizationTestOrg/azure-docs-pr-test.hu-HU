---
title: "Azure parancssori felület használatával PostgreSQL aaaConfigure és hozzáférést kiszolgálónaplókban |} Microsoft Docs"
description: "Ez a cikk ismerteti, hogyan tooconfigure és hozzáférési hello-kiszolgáló naplóit Azure-adatbázis használata az Azure CLI parancssori PostgreSQL."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 924d4e7634cc48405bcb0f813cf61d8b72ad8aa0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a>Konfigurálja, és hozzáférést kiszolgálónaplókban olvashatók Azure parancssori felület használatával
Hello PostgreSQL server hibanaplójában talál hello parancssori felület (Azure CLI) használatával töltheti le. Hozzáférés tootransaction naplókat azonban nem támogatott. 

## <a name="prerequisites"></a>Előfeltételek
Ez hogyan tooguide keresztül toostep, lesz szüksége:
- Egy [PostgreSQL-kiszolgáló Azure-adatbázis](quickstart-create-server-database-azure-cli.md)
- Telepítés [Azure CLI 2.0](/cli/azure/install-azure-cli) parancssori segédprogram, vagy használjon hello Azure Cloud rendszerhéj hello böngészőben.

## <a name="configure-logging-for-azure-database-for-postgresql"></a>PostgreSQL az Azure-adatbázis naplózásának konfigurálása
Hello tooaccess lekérdezés naplói és a hibanaplókat is konfigurálhatja. A hibanaplókban automatikus vákuumszivattyú és a kapcsolat és az ellenőrzőpontok információkat is tartalmazhat.
1. Naplózás bekapcsolása
2. Update naplóját\_utasítás és a naplófájlok\_min\_időtartam\_utasítás tooenable lekérdezések naplózása
3. Frissítse a megőrzési időtartam

További információkért lásd: [kiszolgáló konfigurációs paraméterek testreszabása](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs-for-azure-database-for-postgresql-server"></a>Lista naplók az Azure Database PostgreSQL-kiszolgáló
toolist hello elérhető naplófájlokban található a kiszolgálón, futtassa a hello [az postgres server-naplók lista](/cli/azure/postgres/server-logs#list) parancsot.

Hello naplófájlok kiszolgáló listázhatja **mypgserver-20170401.postgres.database.azure.com** erőforráscsoportba tartozó **myresourcegroup**, és közvetlen azt tooa szöveg nevű **napló\_fájlok\_lista.txt.**
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mypgserver-20170401 > log_files_list.txt
```
## <a name="download-logs-locally-from-hello-server"></a>Naplók letöltése helyileg hello kiszolgálóról
Hello [az postgres server-naplók letöltése](/cli/azure/postgres/server-logs#download) parancs toodownload külön naplófájlba a kiszolgáló lehetővé teszi. 

Ez a példa letöltések hello hello kiszolgáló adott naplófájl **mypgserver-20170401.postgres.database.azure.com** erőforráscsoportba tartozó **myresourcegroup** tooyour helyi környezetben.
```azurecli-interactive
az postgres server-logs download --name 20170414-mypgserver-20170401-postgresql.log --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="next-steps"></a>Következő lépések
- toolearn server-naplók kapcsolatos további információkért lásd: [Server PostgreSQL az Azure Database-naplók](concepts-server-logs.md)
- A kiszolgáló paraméterek további információkért lásd: [testre szabhatja a kiszolgáló konfigurációs paraméterek Azure parancssori felület használatával](howto-configure-server-parameters-using-cli.md)
