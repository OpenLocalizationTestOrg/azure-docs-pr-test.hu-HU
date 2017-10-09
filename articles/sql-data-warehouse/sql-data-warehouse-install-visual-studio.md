---
title: "aaaInstall Visual Studio és az SSDT az SQL Data Warehouse |} Microsoft Docs"
description: "A Visual Studio és az SQL Server Development Tools (SSDT) telepítése az Azure SQL Data Warehouse-hoz"
services: sql-data-warehouse
documentationcenter: NA
author: antvgski
manager: jhubbard
editor: 
ms.assetid: 0ed9b406-9b42-4fe6-b963-fe0a5b48aac1
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: connect
ms.date: 03/30/2017
ms.author: anvang;barbkess
ms.openlocfilehash: cf49c13d5cab598ed127f5702c04168b62ede0dc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="install-visual-studio-and-ssdt-for-sql-data-warehouse"></a><span data-ttu-id="51ace-103">A Visual Studio és az SSDT telepítése SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="51ace-103">Install Visual Studio and SSDT for SQL Data Warehouse</span></span>
<span data-ttu-id="51ace-104">az SQL Data Warehouse alkalmazások toodevelop, azt javasoljuk, a Visual Studio legújabb verziójának hello hello legújabb verziójú SQL Server Data Tools (SSDT).</span><span class="sxs-lookup"><span data-stu-id="51ace-104">toodevelop applications for SQL Data Warehouse, we recommend using hello most recent version of Visual Studio with hello most recent version of SQL Server Data Tools (SSDT).</span></span>  <span data-ttu-id="51ace-105">A Visual Studio 2013 Update 5 és az SSDT együttes használata is támogatott a visszamenőleges kompatibilitás érdekében.</span><span class="sxs-lookup"><span data-stu-id="51ace-105">Visual Studio 2013 Update 5 with SSDT is also supported for backward compatibility.</span></span>  

<span data-ttu-id="51ace-106">Visual Studio és az SSDT együttes használata lehetővé teszi a toouse hello SQL Server Object Explorer toovisually megismerkedhet a táblákat, nézeteket, tárolt eljárásokat és sok más objektumot az SQL Data Warehouse, valamint a lekérdezések futtatása.</span><span class="sxs-lookup"><span data-stu-id="51ace-106">Using Visual Studio with SSDT will allow you toouse hello SQL Server Object Explorer toovisually explore tables, views, stored procedures and many more objects in your SQL Data Warehouse as well as run queries.</span></span>

> [!NOTE]
> <span data-ttu-id="51ace-107">Az SQL Data Warehouse még nem támogatja a Visual Studio-adatbázisprojekteket.</span><span class="sxs-lookup"><span data-stu-id="51ace-107">SQL Data Warehouse does not yet support Visual Studio Database Projects.</span></span>  <span data-ttu-id="51ace-108">Ez a szolgáltatás egy későbbi verzióban lesz elérhető.</span><span class="sxs-lookup"><span data-stu-id="51ace-108">This feature will be added in a future version.</span></span>
> 
> 

## <a name="step-1-install-visual-studio"></a><span data-ttu-id="51ace-109">1. lépés: A Visual Studio telepítése</span><span class="sxs-lookup"><span data-stu-id="51ace-109">Step 1: Install Visual Studio</span></span>
<span data-ttu-id="51ace-110">Kövesse az alábbi hivatkozások toodownload, és telepítse a Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="51ace-110">Follow these links toodownload and install Visual Studio.</span></span> <span data-ttu-id="51ace-111">Ha már rendelkezik a Visual Studio 2013 vagy újabb verziója, kihagyhatja tooStep 2, az SSDT telepítése.</span><span class="sxs-lookup"><span data-stu-id="51ace-111">If you already have Visual Studio 2013 or later installed, you can skip tooStep 2, install SSDT.</span></span>

1. <span data-ttu-id="51ace-112">[Töltse le a Visual Studio][].</span><span class="sxs-lookup"><span data-stu-id="51ace-112">[Download Visual Studio][].</span></span>
2. <span data-ttu-id="51ace-113">Hajtsa végre a hello [Visual Studio telepítése] [ Installing Visual Studio] útmutatót az MSDN Webhelyén, és válassza ki a hello alapértelmezett konfigurációkat.</span><span class="sxs-lookup"><span data-stu-id="51ace-113">Follow hello [Installing Visual Studio][Installing Visual Studio] guide on MSDN and choose hello default configurations.</span></span>

## <a name="step-2-install-ssdt"></a><span data-ttu-id="51ace-114">2. lépés: Az SSDT telepítése</span><span class="sxs-lookup"><span data-stu-id="51ace-114">Step 2: Install SSDT</span></span>
<span data-ttu-id="51ace-115">az SSDT a Visual Studio tooinstall egyszerűen keressen a Visual studióban az SSDT-frissítéseket az alábbiak szerint.</span><span class="sxs-lookup"><span data-stu-id="51ace-115">tooinstall SSDT for Visual Studio simply check for an SSDT update from within Visual Studio by following these steps.</span></span>

1. <span data-ttu-id="51ace-116">A Visual Studióban kattintson a **eszközök** / **bővítmények és frissítések...**</span><span class="sxs-lookup"><span data-stu-id="51ace-116">In Visual Studio click on **Tools** / **Extensions and Updates…**</span></span><span data-ttu-id="51ace-117"> / **Frissítések**</span><span class="sxs-lookup"><span data-stu-id="51ace-117"> / **Updates**</span></span>
2. <span data-ttu-id="51ace-118">Válassza a **Product Updates** (Termékfrissítések) lehetőséget, majd keresse a **Microsoft SQL Server Update for database tooling** (Microsoft SQL Server frissítése adatbáziseszközökkel) elemet</span><span class="sxs-lookup"><span data-stu-id="51ace-118">Select **Product Updates** and then look for **Microsoft SQL Server Update for database tooling**</span></span>

<span data-ttu-id="51ace-119">Ha a frissítés nem található, akkor szüksége lesz hello legfrissebb verzió van telepítve.</span><span class="sxs-lookup"><span data-stu-id="51ace-119">If an update is not found, then you should have hello latest version installed.</span></span>  <span data-ttu-id="51ace-120">tooconfirm az SSDT telepítve van, kattintson a **súgó** / **kapcsolatban a Microsoft Visual Studio** és hello listában keresse meg az SQL Server Data Tools összetevővel.</span><span class="sxs-lookup"><span data-stu-id="51ace-120">tooconfirm SSDT is installed, click on **Help** / **About Microsoft Visual Studio** and look for SQL Server Data Tools in hello list.</span></span>  <span data-ttu-id="51ace-121">hello SSDT legújabb verziója a 14.0.60525.0-s verzió.</span><span class="sxs-lookup"><span data-stu-id="51ace-121">hello latest version of SSDT is 14.0.60525.0.</span></span>  <span data-ttu-id="51ace-122">Ha hello beállítás tooinstall nem érhető el a Visual Studióból, ellátogathat hello [SSDT letöltése] [ SSDT Download] toodownload lapon, és az SSDT manuális telepítése.</span><span class="sxs-lookup"><span data-stu-id="51ace-122">If hello option tooinstall is not available from Visual Studio, alternatively you can visit hello [SSDT Download][SSDT Download] page toodownload and install SSDT manually.</span></span>

## <a name="next-steps"></a><span data-ttu-id="51ace-123">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="51ace-123">Next steps</span></span>
<span data-ttu-id="51ace-124">Most, hogy hello SSDT legújabb verzióját, készen áll túl[csatlakozás] [ connect] tooyour SQL Data Warehouse.</span><span class="sxs-lookup"><span data-stu-id="51ace-124">Now that you have hello latest version of SSDT, you are ready too[connect][connect] tooyour SQL Data Warehouse.</span></span>

<!--Anchors-->

<!--Image references-->

<!--Articles-->
[connect]: ./sql-data-warehouse-query-visual-studio.md

<!--Other-->
[Töltse le a Visual Studio]: https://www.visualstudio.com/downloads/
[Installing Visual Studio]: https://msdn.microsoft.com/library/e2h7fzkw.aspx
[SSDT Download]: https://msdn.microsoft.com/library/mt204009.aspx
