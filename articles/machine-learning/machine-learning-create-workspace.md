---
title: "A Machine Learning-munkaterület létrehozása |} Microsoft Docs"
description: "Egy munkaterület létrehozása az Azure Machine Learning Studióban"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: aa96b784-ac6c-44bc-a28a-85d49fbe90a2
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/27/2017
ms.author: garye;bradsev;ahgyger
ms.openlocfilehash: 182a34822e71d63f4d7229548ae3f59d9f195337
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-share-an-azure-machine-learning-workspace"></a><span data-ttu-id="960e8-103">Azure Machine Learning-munkaterület létrehozása és megosztása</span><span class="sxs-lookup"><span data-stu-id="960e8-103">Create and share an Azure Machine Learning workspace</span></span>
<span data-ttu-id="960e8-104">Ez a menüben a témakörök, amelyek bemutatják, hogyan állíthatja be a különböző adatok tudományos környezetekben a Cortana Analytics folyamat (nagybetűs) által használt mutató hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="960e8-104">This menu links to topics that describe how to set up the various data science environments used by the Cortana Analytics Process (CAPS).</span></span>

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

<span data-ttu-id="960e8-105">Azure Machine Learning Studio használatához meg kell rendelkeznie a Machine Learning munkaterülettel.</span><span class="sxs-lookup"><span data-stu-id="960e8-105">To use Azure Machine Learning Studio, you need to have a Machine Learning workspace.</span></span> <span data-ttu-id="960e8-106">A munkaterület létrehozására, kezelésére és kísérletek közzétételéhez szükséges eszközöket tartalmazza.</span><span class="sxs-lookup"><span data-stu-id="960e8-106">This workspace contains the tools you need to create, manage, and publish experiments.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

### <a name="to-create-a-workspace"></a><span data-ttu-id="960e8-107">Munkaterület létrehozása</span><span class="sxs-lookup"><span data-stu-id="960e8-107">To create a workspace</span></span>
1. <span data-ttu-id="960e8-108">Jelentkezzen be a [Azure-portálon](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="960e8-108">Sign in to the [Azure portal](https://portal.azure.com/)</span></span>

    > [!NOTE]
    > <span data-ttu-id="960e8-109">Jelentkezzen be, és hozzon létre egy munkaterület, kell lennie az Azure-előfizetési rendszergazda.</span><span class="sxs-lookup"><span data-stu-id="960e8-109">To sign in and create a workspace, you need to be an Azure subscription administrator.</span></span> 
    >
    > 

2. <span data-ttu-id="960e8-110">Kattintson a **+ új**</span><span class="sxs-lookup"><span data-stu-id="960e8-110">Click **+New**</span></span>

3. <span data-ttu-id="960e8-111">Válassza ki **Eszközintelligencia + analitika**, kattintson a **Machine Learning-munkaterület**, majd kattintson a **létrehozása**</span><span class="sxs-lookup"><span data-stu-id="960e8-111">Select **Intelligence + analytics**, click **Machine Learning Workspace**, then click **Create**</span></span>

4. <span data-ttu-id="960e8-112">Adja meg a munkaterület adatokat</span><span class="sxs-lookup"><span data-stu-id="960e8-112">Enter your workspace information</span></span>

    - <span data-ttu-id="960e8-113">A *munkaterületnevet* nem adható meg a befejezési legfeljebb 260 karakter lehet.</span><span class="sxs-lookup"><span data-stu-id="960e8-113">The *workspace name* may be up to 260 characters, not ending in a space.</span></span> <span data-ttu-id="960e8-114">A neve nem tartalmazhatja a következő karaktereket:`< > * % & : \ ? + /`</span><span class="sxs-lookup"><span data-stu-id="960e8-114">The name can't include these characters: `< > * % & : \ ? + /`</span></span>
    - <span data-ttu-id="960e8-115">A *web service-csomag* akkor válasszon (vagy hozzon létre), valamint a társított *tarifacsomag* válassza ki, akkor használatos, ha a munkaterület webszolgáltatások telepítése.</span><span class="sxs-lookup"><span data-stu-id="960e8-115">The *web service plan* you choose (or create), along with the associated *pricing tier* you select, is used if you deploy web services from this workspace.</span></span>

    ![Új munkaterület létrehozása](media/machine-learning-create-workspace/create-new-workspace.png)

5. <span data-ttu-id="960e8-117">Kattintson a **Create** (Létrehozás) gombra</span><span class="sxs-lookup"><span data-stu-id="960e8-117">Click **Create**</span></span>

<span data-ttu-id="960e8-118">Ha a munkaterületet van telepítve, a Machine Learning Studióban indíthatja el.</span><span class="sxs-lookup"><span data-stu-id="960e8-118">Once the workspace is deployed, you can open it in Machine Learning Studio.</span></span>

1. <span data-ttu-id="960e8-119">Tallózás a Machine Learning Studióba [https://studio.azureml.net/](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="960e8-119">Browse to Machine Learning Studio at [https://studio.azureml.net/](https://studio.azureml.net/).</span></span>

2. <span data-ttu-id="960e8-120">A munkaterület kiválasztása a felső – jobb sarkában található.</span><span class="sxs-lookup"><span data-stu-id="960e8-120">Select your workspace in the upper-right-hand corner.</span></span>

    ![Munkaterület kiválasztása](media/machine-learning-create-workspace/open-workspace.png)

3. <span data-ttu-id="960e8-122">Kattintson a **saját kísérletek opcióra**.</span><span class="sxs-lookup"><span data-stu-id="960e8-122">Click **my experiments**.</span></span>

    ![Nyissa meg benne](media/machine-learning-create-workspace/my-experiments.png)

<span data-ttu-id="960e8-124">A munkaterület kezelésével kapcsolatos információkért lásd: [kezelése az Azure Machine Learning-munkaterület](machine-learning-manage-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="960e8-124">For information about managing your workspace, see [Manage an Azure Machine Learning workspace](machine-learning-manage-workspace.md).</span></span>
<span data-ttu-id="960e8-125">Ha a munkaterület létrehozását probléma adódik, tekintse meg [hibaelhárítási útmutatója: hozzon létre, és kapcsolódjon a Machine Learning-munkaterület](machine-learning-troubleshooting-creating-ml-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="960e8-125">If you encounter a problem creating your workspace, see [Troubleshooting guide: Create and connect to a Machine Learning workspace](machine-learning-troubleshooting-creating-ml-workspace.md).</span></span>


## <a name="sharing-an-azure-machine-learning-workspace"></a><span data-ttu-id="960e8-126">Egy Azure Machine Learning munkaterülettel megosztása</span><span class="sxs-lookup"><span data-stu-id="960e8-126">Sharing an Azure Machine Learning workspace</span></span>
<span data-ttu-id="960e8-127">Egyszer a Machine Learning munkaterület jön létre, felajánlhatja a felhasználóknak a munkaterülethez való fájlmegosztás elérését a munkaterület és minden a kísérletek adatkészletek, jegyzetfüzeteket, stb. Hozzáadhat felhasználókat két szerepkör egyikében:</span><span class="sxs-lookup"><span data-stu-id="960e8-127">Once a Machine Learning workspace is created, you can invite users to your workspace to share access to your workspace and all its experiments, datasets, notebooks, etc. You can add users in one of two roles:</span></span>

* <span data-ttu-id="960e8-128">**Felhasználói** -munkaterület felhasználó létrehozása, megnyithat, módosíthatja, és kísérleteket, adatkészleteket, stb. a munkaterület törlése.</span><span class="sxs-lookup"><span data-stu-id="960e8-128">**User** - A workspace user can create, open, modify, and delete experiments, datasets, etc. in the workspace.</span></span>
* <span data-ttu-id="960e8-129">**Tulajdonos** - kérhetnek fel egy olyan tulajdonost, és távolítsa el a munkaterületen milyen felhasználói mellett végezhető műveletek.</span><span class="sxs-lookup"><span data-stu-id="960e8-129">**Owner** - An owner can invite and remove users in the workspace, in addition to what a user can do.</span></span>

> [!NOTE]
> <span data-ttu-id="960e8-130">A rendszergazdai fiók, amely létrehozza a munkaterület tulajdonos munkaterületként automatikusan hozzáadódik a munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="960e8-130">The administrator account that creates the workspace is automatically added to the workspace as workspace Owner.</span></span> <span data-ttu-id="960e8-131">Azonban más rendszergazdák vagy a felhasználók az adott előfizetés nem automatikusan hozzáférést kapnak a munkaterület - kell explicit módon hívhat meg.</span><span class="sxs-lookup"><span data-stu-id="960e8-131">However, other administrators or users in that subscription are not automatically granted access to the workspace - you need to invite them explicitly.</span></span>
> 
> 

### <a name="to-share-a-workspace"></a><span data-ttu-id="960e8-132">A munkaterület megosztása</span><span class="sxs-lookup"><span data-stu-id="960e8-132">To share a workspace</span></span>

1. <span data-ttu-id="960e8-133">Jelentkezzen be a Machine Learning Studióba [https://studio.azureml.net/Home](https://studio.azureml.net/Home)</span><span class="sxs-lookup"><span data-stu-id="960e8-133">Sign in to Machine Learning Studio at [https://studio.azureml.net/Home](https://studio.azureml.net/Home)</span></span>

2. <span data-ttu-id="960e8-134">A bal oldali panelen kattintson a **beállítások**</span><span class="sxs-lookup"><span data-stu-id="960e8-134">In the left panel, click **SETTINGS**</span></span>

3. <span data-ttu-id="960e8-135">Kattintson a **felhasználók** lap</span><span class="sxs-lookup"><span data-stu-id="960e8-135">Click the **USERS** tab</span></span>

4. <span data-ttu-id="960e8-136">Kattintson a **több felhasználók MEGHÍVÁSA** a lap alján</span><span class="sxs-lookup"><span data-stu-id="960e8-136">Click **INVITE MORE USERS** at the bottom of the page</span></span>

    ![Studio beállításai](media/machine-learning-create-workspace/settings.png)

5. <span data-ttu-id="960e8-138">Adjon meg egy vagy több e-mail címet.</span><span class="sxs-lookup"><span data-stu-id="960e8-138">Enter one or more email addresses.</span></span> <span data-ttu-id="960e8-139">A felhasználók egy érvényes Microsoft-fiókkal vagy (az Azure Active Directory) szervezeti fiók szükséges.</span><span class="sxs-lookup"><span data-stu-id="960e8-139">The users need a valid Microsoft account or an organizational account (from Azure Active Directory).</span></span>

6. <span data-ttu-id="960e8-140">Válassza ki, hogy a felhasználók hozzáadása a tulajdonos vagy a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="960e8-140">Select whether you want to add the users as Owner or User.</span></span>

7. <span data-ttu-id="960e8-141">Kattintson a **OK** pipa gombra.</span><span class="sxs-lookup"><span data-stu-id="960e8-141">Click the **OK** checkmark button.</span></span>

<span data-ttu-id="960e8-142">Minden felhasználóhoz hozzá jelentkezzen be a megosztott munkaterület kapcsolatos utasításokat tartalmazó e-mailt fog kapni.</span><span class="sxs-lookup"><span data-stu-id="960e8-142">Each user you add will receive an email with instructions on how to sign in to the shared workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="960e8-143">A felhasználók tudnak telepíteni, vagy a munkaterület webszolgáltatások kezelése fel kell egy közreműködő vagy az Azure-előfizetés rendszergazdája.</span><span class="sxs-lookup"><span data-stu-id="960e8-143">For users to be able to deploy or manage web services in this workspace, they must be a contributor or administrator in the Azure subscription.</span></span> 



