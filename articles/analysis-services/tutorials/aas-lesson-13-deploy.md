---
title: "Az Azure Analysis Services oktatóanyaga – 13. lecke: Üzembe helyezés | Microsoft Docs"
description: "A lecke az oktatóanyag projektjének az Azure Analysis Services szolgáltatásban való üzembe helyezését ismerteti."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 07/17/2017
ms.author: owend
ms.openlocfilehash: 70dbf5786262f75199270aa8009e03b9b48b8559
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 08/29/2017
---
# <a name="lesson-13-deploy"></a><span data-ttu-id="09607-103">13. lecke: Üzembe helyezés</span><span class="sxs-lookup"><span data-stu-id="09607-103">Lesson 13: Deploy</span></span>

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

<span data-ttu-id="09607-104">Ebben a leckében az üzembehelyezési tulajdonságokat fogja konfigurálni: megad egy Azure Analysis Services-kiszolgálót, amelyen az üzembe helyezést végzi, majd elnevezi a modellt.</span><span class="sxs-lookup"><span data-stu-id="09607-104">In this lesson, you configure deployment properties; specifying an Azure Analysis Services server to deploy to and a name for the model.</span></span> <span data-ttu-id="09607-105">Ezután üzembe helyezi a modellt az adott példányon.</span><span class="sxs-lookup"><span data-stu-id="09607-105">You then deploy the model to that instance.</span></span> <span data-ttu-id="09607-106">Miután a modell üzembe lett helyezve, a felhasználók egy jelentéskészítő ügyfélalkalmazás segítségével csatlakozhatnak hozzá.</span><span class="sxs-lookup"><span data-stu-id="09607-106">After your model is deployed, users can connect to it by using a reporting client application.</span></span> <span data-ttu-id="09607-107">További információkért lásd [az Azure Analysis Servicesben történő üzembe helyezést](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy) ismertető cikket.</span><span class="sxs-lookup"><span data-stu-id="09607-107">To learn more, see [Deploy to Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy).</span></span>  
  
<span data-ttu-id="09607-108">A lecke elvégzésének várható időtartama: **5 perc**</span><span class="sxs-lookup"><span data-stu-id="09607-108">Estimated time to complete this lesson: **5 minutes**</span></span>  
  
## <a name="prerequisites"></a><span data-ttu-id="09607-109">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="09607-109">Prerequisites</span></span>  
<span data-ttu-id="09607-110">Ez a témakör a táblázatos modellezéssel foglalkozó oktatóanyag részét képezi, amelyet a megfelelő sorrendben kell elvégezni.</span><span class="sxs-lookup"><span data-stu-id="09607-110">This topic is part of a tabular modeling tutorial, which should be completed in order.</span></span> <span data-ttu-id="09607-111">A jelen leckében található feladatok végrehajtása előtt be kell fejeznie az előző leckét ([12. lecke: Elemzés az Excelben](../tutorials/aas-lesson-12-analyze-in-excel.md)).</span><span class="sxs-lookup"><span data-stu-id="09607-111">Before performing the tasks in this lesson, you should have completed the previous lesson: [Lesson 12: Analyze in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md).</span></span>  

> [!IMPORTANT]  
> <span data-ttu-id="09607-112">Ahhoz, hogy üzembe helyezést végezhessen a távoli Analysis Services-kiszolgálón, [rendszergazdai jogosultságokkal](../analysis-services-server-admins.md) kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="09607-112">You must have [Administrator permissions](../analysis-services-server-admins.md) on the remote Analysis Services server in-order to deploy to it.</span></span>  

> [!IMPORTANT]  
> <span data-ttu-id="09607-113">Ha egy helyszíni SQL Server kiszolgálóra telepítette az AdventureWorksDW2014 mintaadatbázist, és egy Azure Analysis Services-kiszolgálón helyezi üzembe a modellt, akkor szükség van egy [helyszíni adatátjáróra](../analysis-services-gateway.md).</span><span class="sxs-lookup"><span data-stu-id="09607-113">If you installed the AdventureWorksDW2014 sample database on an on-premises SQL Server, and you're deploying your model to an Azure Analysis Services server, an [On-premises data gateway](../analysis-services-gateway.md) is required.</span></span>
  
## <a name="deploy-the-model"></a><span data-ttu-id="09607-114">A modell üzembe helyezése</span><span class="sxs-lookup"><span data-stu-id="09607-114">Deploy the model</span></span>  
  
#### <a name="to-configure-deployment-properties"></a><span data-ttu-id="09607-115">Az üzembehelyezési tulajdonságok konfigurálása</span><span class="sxs-lookup"><span data-stu-id="09607-115">To configure deployment properties</span></span>  

  
1.  <span data-ttu-id="09607-116">A **Megoldáskezelő** területén kattintson a jobb gombbal az **AW Internet Sales** projektre, majd kattintson a **Tulajdonságok** elemre.</span><span class="sxs-lookup"><span data-stu-id="09607-116">In **Solution Explorer**, right-click the **AW Internet Sales** project, and then click **Properties**.</span></span>  
  
2.  <span data-ttu-id="09607-117">Az **AW Internet Sales tulajdonságlapjainak** párbeszédpanelének **Központi telepítési kiszolgáló** területén a **Kiszolgáló** tulajdonság alatt adja meg a teljes kiszolgálót.</span><span class="sxs-lookup"><span data-stu-id="09607-117">In the **AW Internet Sales Property Pages** dialog box, under **Deployment Server**, in the **Server** property, enter the full server.</span></span>  

    ![aas-lesson13-deploy-property](../tutorials/media/aas-lesson13-deploy-property.png)
  
3.  <span data-ttu-id="09607-119">Az **Adatbázis** tulajdonságnál írja be az **Adventure Works Internet Sales** nevet.</span><span class="sxs-lookup"><span data-stu-id="09607-119">In the **Database** property, type **Adventure Works Internet Sales**.</span></span>  
  
4.  <span data-ttu-id="09607-120">A **Modell neve** tulajdonságnál írja be az **Adventure Works Internet Sales Model** nevet.</span><span class="sxs-lookup"><span data-stu-id="09607-120">In the **Model Name** property, type **Adventure Works Internet Sales Model**.</span></span>  
  
5.  <span data-ttu-id="09607-121">Ellenőrizze a beállításokat, majd kattintson az **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="09607-121">Verify your selections and then click **OK**.</span></span>  
  
#### <a name="to-deploy-the-adventure-works-internet-sales"></a><span data-ttu-id="09607-122">Az Adventure Works internetes értékesítési modell központi telepítése</span><span class="sxs-lookup"><span data-stu-id="09607-122">To deploy the Adventure Works Internet Sales</span></span>
  
1.  <span data-ttu-id="09607-123">A **Megoldáskezelő** területén kattintson a jobb gombbal az **AW Internet Sales** projektre, majd kattintson a **Létrehozás** parancsra.</span><span class="sxs-lookup"><span data-stu-id="09607-123">In **Solution Explorer**, right-click the **AW Internet Sales** project > **Build**.</span></span>  

2.  <span data-ttu-id="09607-124">Kattintson a jobb gombbal az **AW Internet Sales** projektre, majd kattintson az **Üzembe helyezés** parancsra.</span><span class="sxs-lookup"><span data-stu-id="09607-124">Right-click the **AW Internet Sales** project > **Deploy**.</span></span>

    <span data-ttu-id="09607-125">Ha az üzembe helyezést az Azure Analysis Services szolgáltatásban végzi, a rendszer felkérheti, hogy adja meg a fiókja adatait.</span><span class="sxs-lookup"><span data-stu-id="09607-125">When deploying to Azure Analysis Services, you may be prompted to enter your account.</span></span> <span data-ttu-id="09607-126">Adja meg céges fiókját és jelszavát, például: nancy@adventureworks.com. Ennek a fióknak a kiszolgáló Rendszergazdák csoportjába kell tartoznia.</span><span class="sxs-lookup"><span data-stu-id="09607-126">Enter your organizational account and password, for example nancy@adventureworks.com. This account must be in Admins on the server.</span></span>
  
    <span data-ttu-id="09607-127">Megjelenik az Üzembe helyezés párbeszédpanel, amelyen a modell metaadatainak és egyes tábláinak üzembehelyezési állapota látható.</span><span class="sxs-lookup"><span data-stu-id="09607-127">The Deploy dialog box appears and displays the deployment status of the metadata and each table included in the model.</span></span>  
    
    ![aas-lesson13-deploy-status](../tutorials/media/aas-lesson13-deploy-status.png)
  
3. <span data-ttu-id="09607-129">Miután az üzembe helyezés sikeresen befejeződött, nyugodtan kattintson a **Bezárás** gombra.</span><span class="sxs-lookup"><span data-stu-id="09607-129">When deployment successfully completes, go ahead and click **Close**.</span></span>  
  
## <a name="conclusion"></a><span data-ttu-id="09607-130">Összegzés</span><span class="sxs-lookup"><span data-stu-id="09607-130">Conclusion</span></span>  
<span data-ttu-id="09607-131">Gratulálunk!</span><span class="sxs-lookup"><span data-stu-id="09607-131">Congratulations!</span></span> <span data-ttu-id="09607-132">Végzett az első Analysis Services-beli táblázatos modellje elkészítésével és üzembe helyezésével.</span><span class="sxs-lookup"><span data-stu-id="09607-132">You're finished authoring and deploying your first Analysis Services Tabular model.</span></span> <span data-ttu-id="09607-133">Ez az oktatóanyag végigvezette a táblázatos modellek létrehozásával kapcsolatos leggyakoribb feladatokon.</span><span class="sxs-lookup"><span data-stu-id="09607-133">This tutorial has helped guide you through completing the most common tasks in creating a tabular model.</span></span> <span data-ttu-id="09607-134">Most, hogy az Adventure Works internetes értékesítési modell üzembe lett helyezve, az SQL Server Management Studio segítségével felügyelheti azt – létrehozhat feldolgozási szkripteket és egy biztonsági mentési tervet.</span><span class="sxs-lookup"><span data-stu-id="09607-134">Now that your Adventure Works Internet Sales model is deployed, you can use SQL Server Management Studio to manage the model; create process scripts and a backup plan.</span></span> <span data-ttu-id="09607-135">Most már a felhasználók is kapcsolódhatnak a modellhez jelentéskészítő ügyfélalkalmazások útján (pl.: Microsoft Excel vagy Power BI).</span><span class="sxs-lookup"><span data-stu-id="09607-135">Users can also now connect to the model using a reporting client application such as Microsoft Excel or Power BI.</span></span>  

![aas-lesson13-ssms](../tutorials/media/aas-lesson13-ssms.png)
  
  
  
## <a name="whats-next"></a><span data-ttu-id="09607-137">A következő lépések</span><span class="sxs-lookup"><span data-stu-id="09607-137">What's next?</span></span>
<span data-ttu-id="09607-138">[Kapcsolódás Power BI Desktoppal](../analysis-services-connect-pbi.md) </span><span class="sxs-lookup"><span data-stu-id="09607-138">[Connect with Power BI Desktop](../analysis-services-connect-pbi.md) </span></span>  
<span data-ttu-id="09607-139">[Kiegészítő lecke – Dinamikus biztonság](../tutorials/aas-supplemental-lesson-dynamic-security.md) </span><span class="sxs-lookup"><span data-stu-id="09607-139">[Supplemental Lesson - Dynamic security](../tutorials/aas-supplemental-lesson-dynamic-security.md) </span></span>  
<span data-ttu-id="09607-140">[Kiegészítő lecke – Részletsorok](../tutorials/aas-supplemental-lesson-detail-rows.md) </span><span class="sxs-lookup"><span data-stu-id="09607-140">[Supplemental Lesson - Detail rows](../tutorials/aas-supplemental-lesson-detail-rows.md) </span></span>  
[<span data-ttu-id="09607-141">Kiegészítő lecke – Hézagos hierarchiák</span><span class="sxs-lookup"><span data-stu-id="09607-141">Supplemental Lesson - Ragged hierarchies</span></span>](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)   
