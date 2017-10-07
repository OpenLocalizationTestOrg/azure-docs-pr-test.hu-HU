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
# <a name="connect-an-existing-azure-app-service-tooazure-database-for-mysql-server"></a>Csatlakozzon egy létező Azure App Service tooAzure adatbázis MySQL-kiszolgáló
Ez a dokumentum azt ismerteti, hogyan tooconnect egy meglévő Azure App Service tooyour Azure MySQL-kiszolgáló adatbázisában.

## <a name="before-you-begin"></a>Előkészületek
Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com). MySQL-kiszolgáló Azure-adatbázis létrehozása. Részletekért lásd a túl[hogyan toocreate Azure portálról MySQL-kiszolgáló adatbázisában](quickstart-create-mysql-server-database-using-azure-portal.md) vagy [hogyan toocreate Azure parancssori felület használatával MySQL-kiszolgáló adatbázisában](quickstart-create-mysql-server-database-using-azure-cli.md).

Jelenleg nincsenek két megoldások tooenable hozzáférés az Azure App Service tooan Azure-adatbázis a MySQL. A két megoldás tartalmaz, amely kiszolgálószintű tűzfal-szabályok beállítása.

## <a name="solution-1---create-a-firewall-rule-tooallow-all-ips"></a>1 - megoldás hozzon létre egy tűzfal szabály tooallow összes IP-címek
Azure MySQL-adatbázis egy tűzfal tooprotect használatával az adatok biztonsági biztosít. Ha a MySQL-kiszolgáló, az Azure App Service tooAzure adatbázis csatlakoztatása vegye figyelembe, hogy hello kimenő IP-címek az App Service dinamikus jellegű. 

tooensure hello rendelkezésre állását az Azure App Service, azt javasoljuk, a megoldás tooallow összes IP-címek.

> [!NOTE]
> A hosszú távú megoldás tooavoid lehetővé teszi az összes IP-címek az Azure-szolgáltatások tooconnect tooAzure adatbázis MySQL a Microsoft szolgáltatás működik.

1. Hello MySQL server panelen, a beállítások elemcsoportban kattintson **kapcsolatbiztonsági** tooopen hello kapcsolatbiztonsági panel hello MySQL az Azure-adatbázis.

   ![Azure portál – kattintson a kapcsolat biztonságát](./media/howto-manage-firewall-using-portal/1-connection-security.png)

2. Adja meg **SZABÁLYNÉV**, **KEZDŐ IP**, és **záró IP-Címnél**. Ezután kattintson a **Save** (Mentés) gombra.
   - A szabálynév: engedélyezése – minden-IP-címek
   - IP elindítása: 0.0.0.0
   - Záró IP: 255.255.255.255

   ![Azure portál – az összes IP-címek hozzáadása](./media/howto-connect-webapp/1_2-add-all-ips.png)

## <a name="solution-2---create-a-firewall-rule-tooexplicitly-allow-outbound-ips"></a>2 - megoldás létrehozása a tűzfal szabály tooexplicitly kimenő IP-címek engedélyezése
Az összes kimenő IP-címek az Azure App Service hello explicit módon adhat.

1. A hello App Service tulajdonságok panelére léphet, tekintse meg a **kimenő IP-cím**.

   ![Azure portál – nézet kimenő IP-címek](./media/howto-connect-webapp/2_1-outbound-ip-address.png)

2. Hello MySQL-kapcsolat biztonsági panelen vegye fel a kimenő IP-címet egyenként.

   ![Azure portál – explicit IP-címek hozzáadása](./media/howto-connect-webapp/2_2-add-explicit-ips.png)

3. Ne feledje túl**mentése** a tűzfalszabályok között.

Bár a hello Azure App service tookeep IP-címek állandó próbál idővel, vannak esetek, ahol hello IP-címek megváltozhatnak. Például ha hello app újrahasznosítja azt, vagy akkor fordul elő, a méretezési művelet, amikor új gépeket adnak a regionális az Azure data adatközpontok tooincrease hello kapacitás. Ha módosulnak hello IP-címek, hello app sikerült leállásra hello esemény már nem képes kapcsolódni a toohello MySQL kiszolgáló. Vegye figyelembe a lehetséges megoldások megelőző hello közül választva.

## <a name="ssl-configuration"></a>SSL-beállítása
Azure MySQL-adatbázis SSL engedélyezve alapértelmezés szerint rendelkezik. Ha az alkalmazás nem használ SSL tooconnect toohello adatbázis, akkor szüksége toodisable SSL MySQL-kiszolgálón. További részletekért tooconfigure SSL, lásd: [SSL használatával MySQL az Azure-adatbázissal](howto-configure-ssl.md).

## <a name="next-steps"></a>Következő lépések
Kapcsolati karakterláncok kapcsolatos további információkért tekintse meg túl[kapcsolati karakterláncok](howto-connection-string.md).
