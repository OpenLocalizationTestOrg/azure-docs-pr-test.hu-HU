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
# <a name="manage-analysis-services"></a><span data-ttu-id="4ea56-103">Analysis Services kezelése</span><span class="sxs-lookup"><span data-stu-id="4ea56-103">Manage Analysis Services</span></span>
<span data-ttu-id="4ea56-104">Ha az Analysis Services-kiszolgáló létrehozta az Azure-ban, lehetséges, hogy néhány adminisztrációs és kezelőkonzol feladatot tooperform azonnal kell vagy valamikor lefelé hello közúti.</span><span class="sxs-lookup"><span data-stu-id="4ea56-104">Once you've created an Analysis Services server in Azure, there may be some administration and management tasks you need tooperform right away or sometime down hello road.</span></span> <span data-ttu-id="4ea56-105">Futtasson például toohello frissítési adatok, akik hello modellek a kiszolgáló eléréséhez, vagy a kiszolgáló állapotának figyeléséhez vezérlő feldolgozása.</span><span class="sxs-lookup"><span data-stu-id="4ea56-105">For example, run processing toohello refresh data, control who can access hello models on your server, or monitor your server's health.</span></span> <span data-ttu-id="4ea56-106">Egyes felügyeleti feladatok csak az Azure portálon, az SQL Server Management Studio (SSMS), mások hajtható végre, és néhány feladatot teheti meg.</span><span class="sxs-lookup"><span data-stu-id="4ea56-106">Some management tasks can only be performed in Azure portal, others in SQL Server Management Studio (SSMS), and some tasks can be done in either.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="4ea56-107">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4ea56-107">Azure portal</span></span>
<span data-ttu-id="4ea56-108">[Azure-portálon](http://portal.azure.com/) is hozzon létre és törölhetnek kiszolgálókat, figyelheti a kiszolgáló erőforrásait, Oldalméret módosítása oly módon, és tooyour kiszolgálók kezeléséhez.</span><span class="sxs-lookup"><span data-stu-id="4ea56-108">[Azure portal](http://portal.azure.com/) is where you can create and delete servers, monitor server resources, change size, and manage who has access tooyour servers.</span></span>  <span data-ttu-id="4ea56-109">Ha olyan problémákat tapasztal, is küldhet a támogatási kérelmet.</span><span class="sxs-lookup"><span data-stu-id="4ea56-109">If you're having some problems, you can also submit a support request.</span></span>

![A kiszolgáló nevének lekérése az Azure-ban](./media/analysis-services-manage/aas-manage-portal.png)

## <a name="sql-server-management-studio"></a><span data-ttu-id="4ea56-111">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="4ea56-111">SQL Server Management Studio</span></span>
<span data-ttu-id="4ea56-112">Csatlakozás az Azure-ban tooyour kiszolgáló van hasonlóan csatlakozás tooa server-példány a saját szervezetében.</span><span class="sxs-lookup"><span data-stu-id="4ea56-112">Connecting tooyour server in Azure is just like connecting tooa server instance in your own organization.</span></span> <span data-ttu-id="4ea56-113">Az SSMS azonos feladatokat, például a folyamat vagy hozzon létre egy feldolgozási parancsprogramot, szerepkörök kezelése és használhatja a PowerShell hello számos végezhet.</span><span class="sxs-lookup"><span data-stu-id="4ea56-113">From SSMS, you can perform many of hello same tasks such as process data or create a processing script, manage roles, and use PowerShell.</span></span>
  
![SQL Server Management Studio](./media/analysis-services-manage/aas-manage-ssms.png)

### <a name="download-and-install-ssms"></a><span data-ttu-id="4ea56-115">Töltse le és telepítse az SSMS</span><span class="sxs-lookup"><span data-stu-id="4ea56-115">Download and install SSMS</span></span>
<span data-ttu-id="4ea56-116">tooget összes hello legújabb funkcióit és hello legegyenletesebb élmény tooyour Azure Analysis Services-kiszolgálóhoz való csatlakozáskor, mindenképpen SSMS legfrissebb verziójának hello használata.</span><span class="sxs-lookup"><span data-stu-id="4ea56-116">tooget all hello latest features, and hello smoothest experience when connecting tooyour Azure Analysis Services server, be sure you're using hello latest version of SSMS.</span></span> 

<span data-ttu-id="4ea56-117">[Töltse le az SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span><span class="sxs-lookup"><span data-stu-id="4ea56-117">[Download SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).</span></span>


### <a name="tooconnect-with-ssms"></a><span data-ttu-id="4ea56-118">tooconnect ssms alkalmazásával</span><span class="sxs-lookup"><span data-stu-id="4ea56-118">tooconnect with SSMS</span></span>
 <span data-ttu-id="4ea56-119">SSMS, használata előtt csatlakozó tooyour kiszolgáló köszönésére először, ellenőrizze, a felhasználónév tagja hello Analysis Services-rendszergazdák csoportnak.</span><span class="sxs-lookup"><span data-stu-id="4ea56-119">When using SSMS, before connecting tooyour server hello first time, make sure your username is included in hello Analysis Services Admins group.</span></span> <span data-ttu-id="4ea56-120">toolearn több, lásd: [rendszergazdái](#server-administrators) című cikkben.</span><span class="sxs-lookup"><span data-stu-id="4ea56-120">toolearn more, see [Server administrators](#server-administrators) later in this article.</span></span>

1. <span data-ttu-id="4ea56-121">Csatlakozás előtt kell tooget hello kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="4ea56-121">Before you connect, you need tooget hello server name.</span></span> <span data-ttu-id="4ea56-122">A **Azure-portálon** > server > **áttekintése** > **kiszolgálónév**, hello server példányneve.</span><span class="sxs-lookup"><span data-stu-id="4ea56-122">In **Azure portal** > server > **Overview** > **Server name**, copy hello server name.</span></span>
   
    ![A kiszolgáló nevének lekérése az Azure-ban](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. <span data-ttu-id="4ea56-124">Az SSMS > **Object Explorer**, kattintson a **Connect** > **Analysis Services**.</span><span class="sxs-lookup"><span data-stu-id="4ea56-124">In SSMS > **Object Explorer**, click **Connect** > **Analysis Services**.</span></span>
3. <span data-ttu-id="4ea56-125">A hello **tooServer csatlakozás** párbeszédpanelen illessze be a hello kiszolgáló nevét, majd a **hitelesítési**, válasszon egyet a következő hitelesítési típusok hello:</span><span class="sxs-lookup"><span data-stu-id="4ea56-125">In hello **Connect tooServer** dialog box, paste in hello server name, then in **Authentication**, choose one of hello following authentication types:</span></span>
   
    <span data-ttu-id="4ea56-126">**Windows-hitelesítés** toouse a Windows tartomány\felhasználónév és jelszó hitelesítő adatait.</span><span class="sxs-lookup"><span data-stu-id="4ea56-126">**Windows Authentication** toouse your Windows domain\username and password credentials.</span></span>

    <span data-ttu-id="4ea56-127">**Active Directory jelszavas hitelesítést** toouse szervezeti fiókkal.</span><span class="sxs-lookup"><span data-stu-id="4ea56-127">**Active Directory Password Authentication** toouse an organizational account.</span></span> <span data-ttu-id="4ea56-128">Például ha egy a tartományhoz nem csatlakozó csatlakozó számítógép.</span><span class="sxs-lookup"><span data-stu-id="4ea56-128">For example, when connecting from a non-domain joined computer.</span></span>

    <span data-ttu-id="4ea56-129">**Active Directory univerzális hitelesítési** toouse [nem interaktív vagy a multi-factor authentication](../sql-database/sql-database-ssms-mfa-authentication.md).</span><span class="sxs-lookup"><span data-stu-id="4ea56-129">**Active Directory Universal Authentication** toouse [non-interactive or multi-factor authentication](../sql-database/sql-database-ssms-mfa-authentication.md).</span></span> 
   
    ![Csatlakozás a szolgáltatáshoz az ssms](./media/analysis-services-manage/aas-manage-connect-ssms.png)

## <a name="server-administrators-and-database-users"></a><span data-ttu-id="4ea56-131">Kiszolgáló-rendszergazdák és adatbázis-felhasználók</span><span class="sxs-lookup"><span data-stu-id="4ea56-131">Server administrators and database users</span></span>
<span data-ttu-id="4ea56-132">Az Azure Analysis Services, két típusa van a felhasználók, a kiszolgáló-rendszergazdák és az adatbázis-felhasználók.</span><span class="sxs-lookup"><span data-stu-id="4ea56-132">In Azure Analysis Services, there are two types of users, server administrators and database users.</span></span> <span data-ttu-id="4ea56-133">Mindkét típusú felhasználók kell lennie az Azure Active Directoryban, és a szervezeti e-mail címet, vagy az egyszerű Felhasználónevet kell megadni.</span><span class="sxs-lookup"><span data-stu-id="4ea56-133">Both types of users must be in your Azure Active Directory and must be specified by organizational email address or UPN.</span></span> <span data-ttu-id="4ea56-134">több, lásd: toolearn [hitelesítés és a felhasználói engedélyek](analysis-services-manage-users.md).</span><span class="sxs-lookup"><span data-stu-id="4ea56-134">toolearn more, see [Authentication and user permissions](analysis-services-manage-users.md).</span></span>


## <a name="troubleshooting-connection-problems"></a><span data-ttu-id="4ea56-135">Kapcsolati problémák elhárítása</span><span class="sxs-lookup"><span data-stu-id="4ea56-135">Troubleshooting connection problems</span></span>
<span data-ttu-id="4ea56-136">Ha SSMS, ha problémát tapasztal, szükség lehet a tooclear hello bejelentkezési gyorsítótár.</span><span class="sxs-lookup"><span data-stu-id="4ea56-136">When connecting using SSMS, if you run into problems, you may need tooclear hello login cache.</span></span> <span data-ttu-id="4ea56-137">Gyorsítótárazott toodisc semmi.</span><span class="sxs-lookup"><span data-stu-id="4ea56-137">Nothing is cached toodisc.</span></span> <span data-ttu-id="4ea56-138">tooclear hello gyorsítótár, zárja be és indítsa újra hello csatlakozzon a folyamat.</span><span class="sxs-lookup"><span data-stu-id="4ea56-138">tooclear hello cache, close and restart hello connect process.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="4ea56-139">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="4ea56-139">Next steps</span></span>
<span data-ttu-id="4ea56-140">Ha már a táblázatos modell tooyour új kiszolgáló még nem telepítette, most már van egy időben.</span><span class="sxs-lookup"><span data-stu-id="4ea56-140">If you haven't already deployed a tabular model tooyour new server, now is a good time.</span></span> <span data-ttu-id="4ea56-141">több, lásd: toolearn [tooAzure Analysis Services telepítése](analysis-services-deploy.md).</span><span class="sxs-lookup"><span data-stu-id="4ea56-141">toolearn more, see [Deploy tooAzure Analysis Services](analysis-services-deploy.md).</span></span>

<span data-ttu-id="4ea56-142">A modell tooyour kiszolgáló telepítése után, ha most készen áll a tooconnect tooit egy ügyfél vagy a böngésző használatával.</span><span class="sxs-lookup"><span data-stu-id="4ea56-142">If you've deployed a model tooyour server, you're ready tooconnect tooit using a client or browser.</span></span> <span data-ttu-id="4ea56-143">több, lásd: toolearn [adatok beolvasása az Azure Analysis Services-kiszolgáló](analysis-services-connect.md).</span><span class="sxs-lookup"><span data-stu-id="4ea56-143">toolearn more, see [Get data from Azure Analysis Services server](analysis-services-connect.md).</span></span>

