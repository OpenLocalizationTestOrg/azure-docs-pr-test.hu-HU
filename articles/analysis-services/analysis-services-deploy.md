---
title: "Üzembe helyezés az Azure Analysis Servicesben az SSDT-vel | Microsoft Docs"
description: "Megismerheti, hogyan helyezhet üzembe egy táblázatos modellt az Azure Analysis Servicesre az SSDT-vel."
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
ms.openlocfilehash: e9a3aedfb6e55696e1525e226fada1062fd5eda8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-model-from-ssdt"></a><span data-ttu-id="ff032-103">Modell üzembe helyezése SSDT-ről</span><span class="sxs-lookup"><span data-stu-id="ff032-103">Deploy a model from SSDT</span></span>
<span data-ttu-id="ff032-104">Miután létrehozott egy kiszolgálót az Azure-előfizetésében, készen áll a táblázatos modelladatbázis üzembe helyezésére.</span><span class="sxs-lookup"><span data-stu-id="ff032-104">Once you've created a server in your Azure subscription, you're ready to deploy a tabular model database to it.</span></span> <span data-ttu-id="ff032-105">Az SQL Server Data Tools (SSDT) segítségével létrehozhatja és üzembe helyezheti a táblázatosmodell-projektet, amelyen dolgozik.</span><span class="sxs-lookup"><span data-stu-id="ff032-105">You can use SQL Server Data Tools (SSDT) to build and deploy a tabular model project you're working on.</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="ff032-106">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="ff032-106">Prerequisites</span></span>
<span data-ttu-id="ff032-107">A kezdéshez a következők szükségesek:</span><span class="sxs-lookup"><span data-stu-id="ff032-107">To get started, you need:</span></span>

* <span data-ttu-id="ff032-108">**Analysis Services-kiszolgáló** az Azure-ban.</span><span class="sxs-lookup"><span data-stu-id="ff032-108">**Analysis Services server** in Azure.</span></span> <span data-ttu-id="ff032-109">További információkért lásd [az Azure Analysis Services-kiszolgáló létrehozásával kapcsolatos](analysis-services-create-server.md) témakört.</span><span class="sxs-lookup"><span data-stu-id="ff032-109">To learn more, see [Create an Azure Analysis Services server](analysis-services-create-server.md).</span></span>
* <span data-ttu-id="ff032-110">**Táblázatosmodell-projekt** az SSDT-n, vagy egy meglévő táblázatos modell az 1200-as vagy magasabb kompatibilitási szinten.</span><span class="sxs-lookup"><span data-stu-id="ff032-110">**Tabular model project** in SSDT or an existing tabular model at the 1200 or higher compatibility level.</span></span> <span data-ttu-id="ff032-111">Korábban még nem hozott létre egyet sem?</span><span class="sxs-lookup"><span data-stu-id="ff032-111">Never created one?</span></span> <span data-ttu-id="ff032-112">Próbálkozzon [az Adventure Works internetes értékesítési modell központi telepítésének útmutatójával](https://msdn.microsoft.com/library/hh231691.aspx).</span><span class="sxs-lookup"><span data-stu-id="ff032-112">Try the [Adventure Works Internet sales tabular modeling tutorial](https://msdn.microsoft.com/library/hh231691.aspx).</span></span>
* <span data-ttu-id="ff032-113">**Helyszíni átjáró** – Ha a szervezete hálózatában egy vagy több helyszíni adatforrás található, telepítenie kell egy [helyszíni adatátjárót](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="ff032-113">**On-premises gateway** - If one or more data sources are on-premises in your organization's network, you need to install an [On-premises data gateway](analysis-services-gateway.md).</span></span> <span data-ttu-id="ff032-114">Az átjáróra azért van szükség, hogy a felhőben található kiszolgálója csatlakozni tudjon a helyszíni adatforrásaihoz a modellben található adatok feldolgozásához és frissítéséhez.</span><span class="sxs-lookup"><span data-stu-id="ff032-114">The gateway is necessary for your server in the cloud connect to your on-premises data sources to process and refresh data in the model.</span></span>

> [!TIP]
> <span data-ttu-id="ff032-115">Az üzembe helyezés előtt győződjön meg róla, hogy a tábláiban található adatok feldolgozhatók.</span><span class="sxs-lookup"><span data-stu-id="ff032-115">Before you deploy, make sure you can process the data in your tables.</span></span> <span data-ttu-id="ff032-116">Az SSDT-ben kattintson a **Modell** > **Feldolgozás** > **Az összes feldolgozása** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ff032-116">In SSDT, click **Model** > **Process** > **Process All**.</span></span> <span data-ttu-id="ff032-117">Ha a feldolgozás meghiúsul, nem fog sikerülni a telepítés.</span><span class="sxs-lookup"><span data-stu-id="ff032-117">If processing fails, you cannot successfully deploy.</span></span>
> 
> 

## <a name="to-deploy-a-tabular-model-from-ssdt"></a><span data-ttu-id="ff032-118">Táblázatos modell üzembe helyezése az SSDT-ből</span><span class="sxs-lookup"><span data-stu-id="ff032-118">To deploy a tabular model from SSDT</span></span>

1. <span data-ttu-id="ff032-119">Az üzembe helyezés előtt kérje le a kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="ff032-119">Before you deploy, you need to get the server name.</span></span> <span data-ttu-id="ff032-120">Másolja a kiszolgáló nevét az **Azure Portal** > kiszolgáló > **Áttekintés** > **Kiszolgálónév** részéből.</span><span class="sxs-lookup"><span data-stu-id="ff032-120">In **Azure portal** > server > **Overview** > **Server name**, copy the server name.</span></span>
   
    ![A kiszolgáló nevének lekérése az Azure-ban](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. <span data-ttu-id="ff032-122">Az SSDT > **Megoldáskezelőben** kattintson a jobb gombbal a projektre, majd kattintson a **Tulajdonságok** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ff032-122">In SSDT > **Solution Explorer**, right-click the project > **Properties**.</span></span> <span data-ttu-id="ff032-123">Ezután az **Üzembe helyezés** > **Kiszolgáló** területre illessze be a kiszolgáló nevét.</span><span class="sxs-lookup"><span data-stu-id="ff032-123">Then in **Deployment** > **Server** paste the server name.</span></span>   
   
    ![Az üzembehelyezési kiszolgáló tulajdonságához illessze be a kiszolgáló nevét.](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
3. <span data-ttu-id="ff032-125">A **Megoldáskezelőben** kattintson a jobb gombbal a **Tulajdonságok** elemre, majd kattintson az **Üzembe helyezés** lehetőségre.</span><span class="sxs-lookup"><span data-stu-id="ff032-125">In **Solution Explorer**, right-click **Properties**, then click **Deploy**.</span></span> <span data-ttu-id="ff032-126">Lehet, hogy a rendszer arra kéri, hogy jelentkezzen be az Azure-ba.</span><span class="sxs-lookup"><span data-stu-id="ff032-126">You may be prompted to sign in to Azure.</span></span>
   
    ![Üzembe helyezés kiszolgálóra](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    <span data-ttu-id="ff032-128">Az üzembe helyezés állapota látható a kimeneti ablakban és az Üzembe helyezés területen is.</span><span class="sxs-lookup"><span data-stu-id="ff032-128">Deployment status appears in both the Output window and in Deploy.</span></span>
   
    ![Üzembe helyezés állapota](./media/analysis-services-deploy/aas-deploy-status.png)

<span data-ttu-id="ff032-130">Ennyi az egész!</span><span class="sxs-lookup"><span data-stu-id="ff032-130">That's all there is to it!</span></span>


## <a name="troubleshooting"></a><span data-ttu-id="ff032-131">Hibaelhárítás</span><span class="sxs-lookup"><span data-stu-id="ff032-131">Troubleshooting</span></span>
<span data-ttu-id="ff032-132">Ha a metaadatok telepítésekor a telepítés sikertelen, annak valószínűleg az az oka, hogy az SSDT nem tudott csatlakozni a kiszolgálóhoz.</span><span class="sxs-lookup"><span data-stu-id="ff032-132">If deployment fails when deploying metadata, it's likely because SSDT couldn't connect to your server.</span></span> <span data-ttu-id="ff032-133">Győződjön meg róla, hogy tud csatlakozni a kiszolgálóhoz az SSMS használatával.</span><span class="sxs-lookup"><span data-stu-id="ff032-133">Make sure you can connect to your server using SSMS.</span></span> <span data-ttu-id="ff032-134">Ezt követően ellenőrizze, hogy helyes a projekt Üzembehelyezési kiszolgáló tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="ff032-134">Then make sure the Deployment Server property for the project is correct.</span></span>

<span data-ttu-id="ff032-135">Ha a telepítés egy táblán sikertelen, annak valószínűleg az az oka, hogy a kiszolgálója nem tudott csatlakozni egy adatforráshoz.</span><span class="sxs-lookup"><span data-stu-id="ff032-135">If deployment fails on a table, it's likely because your server couldn't connect to a data source.</span></span> <span data-ttu-id="ff032-136">Ha a szervezete hálózatában helyszíni adatforrás található, mindenképp telepítsen egy [helyszíni adatátjárót](analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="ff032-136">If your data source is on-premises in your organization's network, be sure to install an [On-premises data gateway](analysis-services-gateway.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff032-137">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="ff032-137">Next steps</span></span>
<span data-ttu-id="ff032-138">Miután sikeresen telepítette a kiszolgálóra a táblázatos modellt, azonnal csatlakozhat is hozzá.</span><span class="sxs-lookup"><span data-stu-id="ff032-138">Now that you have your tabular model deployed to your server, you're ready to connect to it.</span></span> <span data-ttu-id="ff032-139">A kezeléséhez [csatlakozzon hozzá az SSMS-sel](analysis-services-manage.md).</span><span class="sxs-lookup"><span data-stu-id="ff032-139">You can [connect to it with SSMS](analysis-services-manage.md) to manage it.</span></span> <span data-ttu-id="ff032-140">Továbbá [csatlakozhat hozzá ügyféleszközzel](analysis-services-connect.md) is, például Power BI, Power BI Desktop vagy Excel segítségével, és megkezdheti a jelentések létrehozását.</span><span class="sxs-lookup"><span data-stu-id="ff032-140">And, you can [connect to it using a client tool](analysis-services-connect.md) like Power BI, Power BI Desktop, or Excel, and start creating reports.</span></span>

