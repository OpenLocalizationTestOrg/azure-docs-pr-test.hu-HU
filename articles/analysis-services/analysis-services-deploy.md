---
title: "aaaDeploy tooAzure Analysis Services SSDT használatával |} Microsoft Docs"
description: "Ismerje meg, hogyan toodeploy egy táblázatos modell tooan Azure Analysis Services-kiszolgáló SSDT használatával."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 5f1f0ae7-11de-4923-a3da-888b13a3638c
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 08/01/2017
ms.author: owend
ms.openlocfilehash: e3f3771fe32a37b9e0173c274080c647152edd4c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-a-model-from-ssdt"></a><span data-ttu-id="e18fb-103">Modell üzembe helyezése SSDT-ről</span><span class="sxs-lookup"><span data-stu-id="e18fb-103">Deploy a model from SSDT</span></span>
<span data-ttu-id="e18fb-104">A létrehozását követően a kiszolgáló az Azure-előfizetése, most készen áll a toodeploy egy táblázatos modell adatbázis tooit.</span><span class="sxs-lookup"><span data-stu-id="e18fb-104">Once you've created a server in your Azure subscription, you're ready toodeploy a tabular model database tooit.</span></span> <span data-ttu-id="e18fb-105">SQL Server Data Tools (SSDT) toobuild használja, és dolgozunk a táblázatos modell projekt telepítése.</span><span class="sxs-lookup"><span data-stu-id="e18fb-105">You can use SQL Server Data Tools (SSDT) toobuild and deploy a tabular model project you're working on.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="e18fb-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="e18fb-106">Prerequisites</span></span>
<span data-ttu-id="e18fb-107">tooget elindult, lesz szüksége:</span><span class="sxs-lookup"><span data-stu-id="e18fb-107">tooget started, you need:</span></span>

* <span data-ttu-id="e18fb-108">**Analysis Services-kiszolgáló** az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="e18fb-108">**Analysis Services server** in Azure.</span></span> <span data-ttu-id="e18fb-109">több, lásd: toolearn [hozzon létre egy Azure Analysis Services-kiszolgáló](analysis-services-create-server.md).</span><span class="sxs-lookup"><span data-stu-id="e18fb-109">toolearn more, see [Create an Azure Analysis Services server](analysis-services-create-server.md).</span></span>
* <span data-ttu-id="e18fb-110">**A táblázatos modell projekt** SSDT vagy meglévő hello 1200-as vagy magasabb kompatibilitási szintű táblázatos modell.</span><span class="sxs-lookup"><span data-stu-id="e18fb-110">**Tabular model project** in SSDT or an existing tabular model at hello 1200 or higher compatibility level.</span></span> <span data-ttu-id="e18fb-111">Korábban még nem hozott létre egyet sem?</span><span class="sxs-lookup"><span data-stu-id="e18fb-111">Never created one?</span></span> <span data-ttu-id="e18fb-112">Próbálja meg hello [Adventure Works Internet értékesítési táblázatos modellezési oktatóanyag](https://msdn.microsoft.com/library/hh231691.aspx).</span><span class="sxs-lookup"><span data-stu-id="e18fb-112">Try hello [Adventure Works Internet sales tabular modeling tutorial](https://msdn.microsoft.com/library/hh231691.aspx).</span></span>
* <span data-ttu-id="e18fb-113">**A helyszíni átjáró** – Ha egy vagy több adatforrást a vállalati hálózathoz a helyszíni, tooinstall kell egy [helyszíni adatátjáró](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="e18fb-113">**On-premises gateway** - If one or more data sources are on-premises in your organization's network, you need tooinstall an [On-premises data gateway](analysis-services-gateway.md).</span></span> <span data-ttu-id="e18fb-114">hello átjáró szükség, az a kiszolgáló hello felhőben csatlakozás tooyour a helyszíni adatok források tooprocess és frissítési adatok hello modellben.</span><span class="sxs-lookup"><span data-stu-id="e18fb-114">hello gateway is necessary for your server in hello cloud connect tooyour on-premises data sources tooprocess and refresh data in hello model.</span></span>

> [!TIP]
> <span data-ttu-id="e18fb-115">Ahhoz, hogy telepíteni, győződjön meg arról is feldolgozhat hello adatok a táblázatban.</span><span class="sxs-lookup"><span data-stu-id="e18fb-115">Before you deploy, make sure you can process hello data in your tables.</span></span> <span data-ttu-id="e18fb-116">Az SSDT-ben kattintson a **Modell** > **Feldolgozás** > **Az összes feldolgozása** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="e18fb-116">In SSDT, click **Model** > **Process** > **Process All**.</span></span> <span data-ttu-id="e18fb-117">Ha a feldolgozás meghiúsul, nem fog sikerülni a telepítés.</span><span class="sxs-lookup"><span data-stu-id="e18fb-117">If processing fails, you cannot successfully deploy.</span></span>
> 
> 

## <a name="toodeploy-a-tabular-model-from-ssdt"></a><span data-ttu-id="e18fb-118">a táblázatos modellek az SSDT toodeploy</span><span class="sxs-lookup"><span data-stu-id="e18fb-118">toodeploy a tabular model from SSDT</span></span>

1. <span data-ttu-id="e18fb-119">Telepít, meg kell tooget hello kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="e18fb-119">Before you deploy, you need tooget hello server name.</span></span> <span data-ttu-id="e18fb-120">A **Azure-portálon** > server > **áttekintése** > **kiszolgálónév**, hello server példányneve.</span><span class="sxs-lookup"><span data-stu-id="e18fb-120">In **Azure portal** > server > **Overview** > **Server name**, copy hello server name.</span></span>
   
    ![A kiszolgáló nevének lekérése az Azure-ban](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. <span data-ttu-id="e18fb-122">Az SSDT > **Megoldáskezelőben**, kattintson a jobb gombbal hello project > **tulajdonságok**.</span><span class="sxs-lookup"><span data-stu-id="e18fb-122">In SSDT > **Solution Explorer**, right-click hello project > **Properties**.</span></span> <span data-ttu-id="e18fb-123">Ezt a **telepítési** > **Server** illessze be a hello kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="e18fb-123">Then in **Deployment** > **Server** paste hello server name.</span></span>   
   
    ![Az üzembehelyezési kiszolgáló tulajdonságához illessze be a kiszolgáló nevét.](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
3. <span data-ttu-id="e18fb-125">A **Megoldáskezelőben** kattintson a jobb gombbal a **Tulajdonságok** elemre, majd kattintson az **Üzembe helyezés** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="e18fb-125">In **Solution Explorer**, right-click **Properties**, then click **Deploy**.</span></span> <span data-ttu-id="e18fb-126">Előfordulhat, hogy a tooAzure rákérdezéses toosign.</span><span class="sxs-lookup"><span data-stu-id="e18fb-126">You may be prompted toosign in tooAzure.</span></span>
   
    ![Tooserver telepítése](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    <span data-ttu-id="e18fb-128">Telepítés állapota akkor jelenik meg, mindkét hello kimeneti ablakban és a telepítés.</span><span class="sxs-lookup"><span data-stu-id="e18fb-128">Deployment status appears in both hello Output window and in Deploy.</span></span>
   
    ![Üzembe helyezés állapota](./media/analysis-services-deploy/aas-deploy-status.png)

<span data-ttu-id="e18fb-130">Ez minden nincs tooit!</span><span class="sxs-lookup"><span data-stu-id="e18fb-130">That's all there is tooit!</span></span>


## <a name="troubleshooting"></a><span data-ttu-id="e18fb-131">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="e18fb-131">Troubleshooting</span></span>
<span data-ttu-id="e18fb-132">Ha nem sikerül a telepítés, metaadatok telepítésekor, valószínű, mert az SSDT tooyour kiszolgáló nem tudott kapcsolódni.</span><span class="sxs-lookup"><span data-stu-id="e18fb-132">If deployment fails when deploying metadata, it's likely because SSDT couldn't connect tooyour server.</span></span> <span data-ttu-id="e18fb-133">Ellenőrizze, hogy tooyour server SSMS használatával is elérheti.</span><span class="sxs-lookup"><span data-stu-id="e18fb-133">Make sure you can connect tooyour server using SSMS.</span></span> <span data-ttu-id="e18fb-134">Végezze el, hogy megfelelő-e a központi telepítési kiszolgáló tulajdonság hello projekt hello.</span><span class="sxs-lookup"><span data-stu-id="e18fb-134">Then make sure hello Deployment Server property for hello project is correct.</span></span>

<span data-ttu-id="e18fb-135">Ha táblán nem sikerül a telepítés, valószínű, mert a kiszolgáló nem tudott kapcsolódni a tooa adatforrás.</span><span class="sxs-lookup"><span data-stu-id="e18fb-135">If deployment fails on a table, it's likely because your server couldn't connect tooa data source.</span></span> <span data-ttu-id="e18fb-136">Ha az adatforrás a vállalati hálózathoz a helyszíni, hogy meg arról, hogy tooinstall egy [helyszíni adatátjáró](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="e18fb-136">If your data source is on-premises in your organization's network, be sure tooinstall an [On-premises data gateway](analysis-services-gateway.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="e18fb-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="e18fb-137">Next steps</span></span>
<span data-ttu-id="e18fb-138">Most, hogy a táblázatos modell telepített tooyour kiszolgáló, most készen áll a tooconnect tooit.</span><span class="sxs-lookup"><span data-stu-id="e18fb-138">Now that you have your tabular model deployed tooyour server, you're ready tooconnect tooit.</span></span> <span data-ttu-id="e18fb-139">Is [tooit kapcsolattartásnak SSMS](analysis-services-manage.md) toomanage azt.</span><span class="sxs-lookup"><span data-stu-id="e18fb-139">You can [connect tooit with SSMS](analysis-services-manage.md) toomanage it.</span></span> <span data-ttu-id="e18fb-140">Ráadásul, [tooit ügyfél eszköz használatával csatlakoztassa](analysis-services-connect.md) például a Power bi-ban, a Power BI Desktopban, vagy az Excel és a jelentések létrehozásának indítása.</span><span class="sxs-lookup"><span data-stu-id="e18fb-140">And, you can [connect tooit using a client tool](analysis-services-connect.md) like Power BI, Power BI Desktop, or Excel, and start creating reports.</span></span>

