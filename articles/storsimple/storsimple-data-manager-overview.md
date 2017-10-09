---
title: "aaaMicrosoft Azure StorSimple adatkezelő áttekintése |} Microsoft Docs"
description: "Hello StorSimple adatok Manager szolgáltatással (magán előnézetben) áttekintése"
services: storsimple
documentationcenter: NA
author: vidarmsft
manager: syadav
editor: 
ms.assetid: 
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 11/22/2016
ms.author: vidarmsft
ms.openlocfilehash: 5d29f7d26be9f2b36857526bdea770d991cece6c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="storsimple-data-manager-overview-private-preview"></a><span data-ttu-id="8c7a1-103">StorSimple adatkezelő áttekintése (magán előnézetben)</span><span class="sxs-lookup"><span data-stu-id="8c7a1-103">StorSimple Data Manager overview (Private Preview)</span></span>

## <a name="overview"></a><span data-ttu-id="8c7a1-104">Áttekintés</span><span class="sxs-lookup"><span data-stu-id="8c7a1-104">Overview</span></span>

<span data-ttu-id="8c7a1-105">A Microsoft Azure StorSimple hibrid felhőalapú tárolási megoldás, amely címek hello gyakran társított fájlmegosztások strukturálatlan adatok bonyolultságára.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-105">Microsoft Azure StorSimple is a hybrid cloud storage solution that addresses hello complexities of unstructured data commonly associated with file shares.</span></span> <span data-ttu-id="8c7a1-106">StorSimple felhőbeli tárhelyét használja az hello kiterjesztése a helyszíni megoldás, és automatikusan tiers adatok között hello a helyszíni és felhőalapú tárolására.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-106">StorSimple uses cloud storage as an extension of hello on-premises solution and automatically tiers data across hello on-premises storage and cloud storage.</span></span> <span data-ttu-id="8c7a1-107">Az adatvédelem integrálva van a helyi és felhőalapú pillanatfelvételek, szükségtelenné teszi hello sprawling tároló-infrastruktúra.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-107">Integrated data protection, with local and cloud snapshots, eliminates hello need for a sprawling storage infrastructure.</span></span> <span data-ttu-id="8c7a1-108">Archiválás és katasztrófa-helyreállítás is zökkenőmentes hello a felhő egy külső helyszín működött.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-108">Archival and disaster recovery is also seamless with hello cloud acting as an offsite location.</span></span>

<span data-ttu-id="8c7a1-109">hello átalakítása ADATSZOLGÁLTATÁSNÁL, amely ebben a dokumentumban vezetjük lehetővé teszi, hogy Ön tooseamlessly hozzáférés hello StorSimple adatok hello felhőben.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-109">hello data transformation service that we are introducing in this document, allows you tooseamlessly access hello StorSimple data in hello cloud.</span></span> <span data-ttu-id="8c7a1-110">Ez a szolgáltatás API-k tooextract adatokat biztosít a StorSimple, és azt tooother Azure bemutatja a szolgáltatások, amelyek könnyen használható formátumban.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-110">This service provides APIs tooextract data from StorSimple and present it tooother Azure services in formats that can be readily consumed.</span></span> <span data-ttu-id="8c7a1-111">Ebben az előzetes verzióban támogatott hello formátumok a következők: Azure-blobokat és Azure Media Services eszközök.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-111">hello formats supported in this preview are Azure blobs and Azure Media Services assets.</span></span> <span data-ttu-id="8c7a1-112">Ez a transzformáció lehetővé teszi, hogy Ön tooeasily vezetékes szolgáltatások, például az Azure Media Services, az Azure HDInsight, az Azure Machine Learning és az Azure Search toooperate adatok a StorSimple 8000 series a helyi eszközön.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-112">This transformation enables you tooeasily wire up services such as Azure Media Services, Azure HDInsight, Azure Machine Learning, and Azure Search toooperate data on StorSimple 8000 series on-premises device.</span></span>

<span data-ttu-id="8c7a1-113">Ez ábrázoló diagram elérésű magas szintű alább láthatók.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-113">A high-level block diagram illustrating this is shown below.</span></span>

![Magas szintű diagramját](./media//storsimple-data-manager-overview/high-level-diagram.png)

<span data-ttu-id="8c7a1-115">Ez a dokumentum azt ismerteti, hogyan regisztrálhat a egy, a szolgáltatás private Preview verziójára.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-115">This document explains how you can sign up for a private preview of this service.</span></span> <span data-ttu-id="8c7a1-116">Azt is bemutatja, hogyan használhatja a StorSimple adatok és más Azure-szolgáltatásokat használó hello felhőben szolgáltatás toowrite alkalmazások.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-116">It also explains how you can use this service toowrite applications that use StorSimple data and other Azure services in hello cloud.</span></span>

## <a name="sign-up-for-data-manager-preview"></a><span data-ttu-id="8c7a1-117">Az adatkezelő előzetes regisztráció</span><span class="sxs-lookup"><span data-stu-id="8c7a1-117">Sign up for Data Manager preview</span></span>
<span data-ttu-id="8c7a1-118">Hello adatkezelő szolgáltatás regisztrálása előtt tekintse át a következő előfeltételek hello.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-118">Before you sign up for hello Data Manager service, review hello following prerequisites.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="8c7a1-119">Előfeltételek</span><span class="sxs-lookup"><span data-stu-id="8c7a1-119">Prerequisites</span></span>

<span data-ttu-id="8c7a1-120">A gyakorlat feltételezi, hogy rendelkezik</span><span class="sxs-lookup"><span data-stu-id="8c7a1-120">This exercise assumes that you have</span></span>
* <span data-ttu-id="8c7a1-121">Egy aktív Azure-előfizetéssel.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-121">an active Azure subscription.</span></span>
* <span data-ttu-id="8c7a1-122">hozzáférés tooa regisztrálni a StorSimple 8000 series eszköz</span><span class="sxs-lookup"><span data-stu-id="8c7a1-122">access tooa registered StorSimple 8000 series device</span></span>
* <span data-ttu-id="8c7a1-123">az összes hello hello StorSimple 8000 series eszközhöz társított kulcsok.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-123">all hello keys associated with hello StorSimple 8000 series device.</span></span>

### <a name="sign-up"></a><span data-ttu-id="8c7a1-124">Regisztráció</span><span class="sxs-lookup"><span data-stu-id="8c7a1-124">Sign up</span></span>

<span data-ttu-id="8c7a1-125">StorSimple adatkezelő hello van private Preview verziójára.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-125">hello StorSimple Data Manager is in private preview.</span></span> <span data-ttu-id="8c7a1-126">Hajtsa végre a következő lépéseket toosign feliratkozott egy, a szolgáltatás private Preview verziójára hello:</span><span class="sxs-lookup"><span data-stu-id="8c7a1-126">Perform hello following steps toosign up for a private preview of this service:</span></span>

1.  <span data-ttu-id="8c7a1-127">Jelentkezzen be hello Azure-portálon: hello StorSimple adatkezelő kiterjesztésű: [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager).</span><span class="sxs-lookup"><span data-stu-id="8c7a1-127">Log into hello Azure portal with hello StorSimple Data Manager extension at: [https://aka.ms/HybridDataManager](https://aka.ms/HybridDataManager).</span></span> <span data-ttu-id="8c7a1-128">Az Azure hitelesítő adataival toolog a használata.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-128">Use your Azure credentials toolog in.</span></span>

2.  <span data-ttu-id="8c7a1-129">Kattintson a hello  **+**  ikon toocreate szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-129">Click hello **+** icon toocreate a service.</span></span> <span data-ttu-id="8c7a1-130">Kattintson a **tárolási** , majd **tekintse meg az összes** megnyíló hello panelen.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-130">Click **Storage** and then click **See All** in hello blade that opens up.</span></span>

    ![Keresési StorSimple adatkezelő ikon](./media/storsimple-data-manager-overview/search-data-manager-icon.png)

3. <span data-ttu-id="8c7a1-132">Hello StorSimple adatkezelő ikon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-132">You see hello StorSimple Data Manager icon.</span></span>

    ![Válassza ki a StorSimple Manager ikon](./media/storsimple-data-manager-overview/select-data-manager-icon.png)

4. <span data-ttu-id="8c7a1-134">Kattintson a StorSimple adatkezelő ikonra, majd **létrehozása**.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-134">Click StorSimple Data Manager icon and then click **Create**.</span></span> <span data-ttu-id="8c7a1-135">Válassza ki a hello előfizetés tooenable hello private Preview verziójára szeretne, és kattintson a **bejelentkezés!**</span><span class="sxs-lookup"><span data-stu-id="8c7a1-135">Pick hello subscription that you want tooenable for hello private preview and then click **Sign me up!**</span></span>

    ![Bejelentkezés](./media/storsimple-data-manager-overview/sign-me-up.png)

5. <span data-ttu-id="8c7a1-137">Ez egy kérelem tooonboard elküldi azt.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-137">This sends a request tooonboard you.</span></span> <span data-ttu-id="8c7a1-138">Azt is megjelenik majd, amint lehetséges.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-138">We will onboard you as soon as possible.</span></span> <span data-ttu-id="8c7a1-139">Az előfizetés engedélyezése után létrehozhat egy StorSimple adatkezelő szolgáltatás.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-139">After your subscription is enabled, you can create a StorSimple Data Manager service.</span></span>

6. <span data-ttu-id="8c7a1-140">tooeasily hello StorSimple adatkezelő szolgáltatás, kattintson a hello csillagra toopin azt tooyour Kedvencek.</span><span class="sxs-lookup"><span data-stu-id="8c7a1-140">tooeasily access hello StorSimple Data Manager service, click hello star icon toopin it tooyour favorites.</span></span>

    ![Hozzáférés a StorSimple adatok kezelők](./media/storsimple-data-manager-overview/access-data-managers.png)


## <a name="next-steps"></a><span data-ttu-id="8c7a1-142">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="8c7a1-142">Next steps</span></span>

<span data-ttu-id="8c7a1-143">[StorSimple adatokat kezelő felhasználói felületén tootransform használja az adatok](storsimple-data-manager-ui.md).</span><span class="sxs-lookup"><span data-stu-id="8c7a1-143">[Use StorSimple Data Manager UI tootransform your data](storsimple-data-manager-ui.md).</span></span>
