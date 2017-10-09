---
title: Azure Analysis Services aaaManage |} Microsoft Docs
description: "Ismerje meg, hogyan toomanage egy Analysis Services-kiszolgáló az Azure-ban."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 79491d0b-b00d-4e02-9ca7-adc99bc02fdb
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/15/2017
ms.author: owend
ms.openlocfilehash: b03bc440801a68162039e28cdb4f863da374014e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-analysis-services"></a>Analysis Services kezelése
Ha az Analysis Services-kiszolgáló létrehozta az Azure-ban, lehetséges, hogy néhány adminisztrációs és kezelőkonzol feladatot tooperform azonnal kell vagy valamikor lefelé hello közúti. Futtasson például toohello frissítési adatok, akik hello modellek a kiszolgáló eléréséhez, vagy a kiszolgáló állapotának figyeléséhez vezérlő feldolgozása. Egyes felügyeleti feladatok csak az Azure portálon, az SQL Server Management Studio (SSMS), mások hajtható végre, és néhány feladatot teheti meg.

## <a name="azure-portal"></a>Azure Portal
[Azure-portálon](http://portal.azure.com/) is hozzon létre és törölhetnek kiszolgálókat, figyelheti a kiszolgáló erőforrásait, Oldalméret módosítása oly módon, és tooyour kiszolgálók kezeléséhez.  Ha olyan problémákat tapasztal, is küldhet a támogatási kérelmet.

![A kiszolgáló nevének lekérése az Azure-ban](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a>SQL Server Management Studio
Csatlakozás az Azure-ban tooyour kiszolgáló van hasonlóan csatlakozás tooa server-példány a saját szervezetében. Az SSMS azonos feladatokat, például a folyamat vagy hozzon létre egy feldolgozási parancsprogramot, szerepkörök kezelése és használhatja a PowerShell hello számos végezhet.
  
![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

### <a name="download-and-install-ssms"></a>Töltse le és telepítse az SSMS
tooget összes hello legújabb funkcióit és hello legegyenletesebb élmény tooyour Azure Analysis Services-kiszolgálóhoz való csatlakozáskor, mindenképpen SSMS legfrissebb verziójának hello használata. 

[Töltse le az SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).


### <a name="tooconnect-with-ssms"></a>tooconnect ssms alkalmazásával
 SSMS, használata előtt csatlakozó tooyour kiszolgáló köszönésére először, ellenőrizze, a felhasználónév tagja hello Analysis Services-rendszergazdák csoportnak. toolearn több, lásd: [rendszergazdái](#server-administrators) című cikkben.

1. Csatlakozás előtt kell tooget hello kiszolgáló nevét. A **Azure-portálon** > server > **áttekintése** > **kiszolgálónév**, hello server példányneve.
   
    ![A kiszolgáló nevének lekérése az Azure-ban](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. Az SSMS > **Object Explorer**, kattintson a **Connect** > **Analysis Services**.
3. A hello **tooServer csatlakozás** párbeszédpanelen illessze be a hello kiszolgáló nevét, majd a **hitelesítési**, válasszon egyet a következő hitelesítési típusok hello:
   
    **Windows-hitelesítés** toouse a Windows tartomány\felhasználónév és jelszó hitelesítő adatait.

    **Active Directory jelszavas hitelesítést** toouse szervezeti fiókkal. Például ha egy a tartományhoz nem csatlakozó csatlakozó számítógép.

    **Active Directory univerzális hitelesítési** toouse [nem interaktív vagy a multi-factor authentication](../sql-database/sql-database-ssms-mfa-authentication.md). 
   
    ![Csatlakozás a szolgáltatáshoz az ssms](./media/analysis-services-manage/aas-manage-connect-ssms.png)

## <a name="server-administrators-and-database-users"></a>Kiszolgáló-rendszergazdák és adatbázis-felhasználók
Az Azure Analysis Services, két típusa van a felhasználók, a kiszolgáló-rendszergazdák és az adatbázis-felhasználók. Mindkét típusú felhasználók kell lennie az Azure Active Directoryban, és a szervezeti e-mail címet, vagy az egyszerű Felhasználónevet kell megadni. több, lásd: toolearn [hitelesítés és a felhasználói engedélyek](analysis-services-manage-users.md).


## <a name="troubleshooting-connection-problems"></a>Kapcsolati problémák elhárítása
Ha SSMS, ha problémát tapasztal, szükség lehet a tooclear hello bejelentkezési gyorsítótár. Gyorsítótárazott toodisc semmi. tooclear hello gyorsítótár, zárja be és indítsa újra hello csatlakozzon a folyamat. 

## <a name="next-steps"></a>Következő lépések
Ha már a táblázatos modell tooyour új kiszolgáló még nem telepítette, most már van egy időben. több, lásd: toolearn [tooAzure Analysis Services telepítése](analysis-services-deploy.md).

A modell tooyour kiszolgáló telepítése után, ha most készen áll a tooconnect tooit egy ügyfél vagy a böngésző használatával. több, lásd: toolearn [adatok beolvasása az Azure Analysis Services-kiszolgáló](analysis-services-connect.md).

