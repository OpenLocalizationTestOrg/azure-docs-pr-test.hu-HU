---
title: "a Machine Learning-munkaterület aaaCreate |} Microsoft Docs"
description: "Hogyan toocreate az Azure Machine Learning Studio munkaterület"
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
ms.openlocfilehash: 178293af222365993fade666124f34269d892325
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-share-an-azure-machine-learning-workspace"></a><span data-ttu-id="a1211-103">Azure Machine Learning-munkaterület létrehozása és megosztása</span><span class="sxs-lookup"><span data-stu-id="a1211-103">Create and share an Azure Machine Learning workspace</span></span>
<span data-ttu-id="a1211-104">Ebben a menüben be hello tooset különböző adatok tudományos környezetekben általi felhasználásáról hello Cortana Analytics folyamat (CAP-ok) leíró tootopics hivatkozásokat tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="a1211-104">This menu links tootopics that describe how tooset up hello various data science environments used by hello Cortana Analytics Process (CAPS).</span></span>

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

<span data-ttu-id="a1211-105">Azure Machine Learning Studio toouse, toohave Machine Learning-munkaterület szükséges.</span><span class="sxs-lookup"><span data-stu-id="a1211-105">toouse Azure Machine Learning Studio, you need toohave a Machine Learning workspace.</span></span> <span data-ttu-id="a1211-106">A munkaterület toocreate kell hello eszközöket tartalmazza, kezelése és kísérletek közzététele.</span><span class="sxs-lookup"><span data-stu-id="a1211-106">This workspace contains hello tools you need toocreate, manage, and publish experiments.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

### <a name="toocreate-a-workspace"></a><span data-ttu-id="a1211-107">a munkaterület toocreate</span><span class="sxs-lookup"><span data-stu-id="a1211-107">toocreate a workspace</span></span>
1. <span data-ttu-id="a1211-108">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com/)</span><span class="sxs-lookup"><span data-stu-id="a1211-108">Sign in toohello [Azure portal](https://portal.azure.com/)</span></span>

    > [!NOTE]
    > <span data-ttu-id="a1211-109">a toosign és munkaterület létrehozása, az Azure-előfizetési rendszergazda toobe van szüksége.</span><span class="sxs-lookup"><span data-stu-id="a1211-109">toosign in and create a workspace, you need toobe an Azure subscription administrator.</span></span> 
    >
    > 

2. <span data-ttu-id="a1211-110">Kattintson a **+ új**</span><span class="sxs-lookup"><span data-stu-id="a1211-110">Click **+New**</span></span>

3. <span data-ttu-id="a1211-111">Válassza ki **Eszközintelligencia + analitika**, kattintson a **Machine Learning-munkaterület**, majd kattintson a **létrehozása**</span><span class="sxs-lookup"><span data-stu-id="a1211-111">Select **Intelligence + analytics**, click **Machine Learning Workspace**, then click **Create**</span></span>

4. <span data-ttu-id="a1211-112">Adja meg a munkaterület adatokat</span><span class="sxs-lookup"><span data-stu-id="a1211-112">Enter your workspace information</span></span>

    - <span data-ttu-id="a1211-113">Hello *munkaterületnevet* too260 karakterből állhat, nem adhatja végződése fel lehet.</span><span class="sxs-lookup"><span data-stu-id="a1211-113">hello *workspace name* may be up too260 characters, not ending in a space.</span></span> <span data-ttu-id="a1211-114">hello neve nem tartalmazhatja a következő karaktereket:`< > * % & : \ ? + /`</span><span class="sxs-lookup"><span data-stu-id="a1211-114">hello name can't include these characters: `< > * % & : \ ? + /`</span></span>
    - <span data-ttu-id="a1211-115">Hello *web service-csomag* akkor válasszon (vagy hozzon létre), és társított hello *tarifacsomag* válassza ki, akkor használatos, ha a munkaterület webszolgáltatások telepítése.</span><span class="sxs-lookup"><span data-stu-id="a1211-115">hello *web service plan* you choose (or create), along with hello associated *pricing tier* you select, is used if you deploy web services from this workspace.</span></span>

    ![Új munkaterület létrehozása](media/machine-learning-create-workspace/create-new-workspace.png)

5. <span data-ttu-id="a1211-117">Kattintson a **Create** (Létrehozás) gombra</span><span class="sxs-lookup"><span data-stu-id="a1211-117">Click **Create**</span></span>

<span data-ttu-id="a1211-118">Miután hello munkaterület van telepítve, a Machine Learning Studióban indíthatja el.</span><span class="sxs-lookup"><span data-stu-id="a1211-118">Once hello workspace is deployed, you can open it in Machine Learning Studio.</span></span>

1. <span data-ttu-id="a1211-119">Keresse meg a tooMachine Learning Studio: [https://studio.azureml.net/](https://studio.azureml.net/).</span><span class="sxs-lookup"><span data-stu-id="a1211-119">Browse tooMachine Learning Studio at [https://studio.azureml.net/](https://studio.azureml.net/).</span></span>

2. <span data-ttu-id="a1211-120">Válassza ki a munkaterület hello felső – jobb sarkában található.</span><span class="sxs-lookup"><span data-stu-id="a1211-120">Select your workspace in hello upper-right-hand corner.</span></span>

    ![Munkaterület kiválasztása](media/machine-learning-create-workspace/open-workspace.png)

3. <span data-ttu-id="a1211-122">Kattintson a **saját kísérletek opcióra**.</span><span class="sxs-lookup"><span data-stu-id="a1211-122">Click **my experiments**.</span></span>

    ![Nyissa meg benne](media/machine-learning-create-workspace/my-experiments.png)

<span data-ttu-id="a1211-124">A munkaterület kezelésével kapcsolatos információkért lásd: [kezelése az Azure Machine Learning-munkaterület](machine-learning-manage-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="a1211-124">For information about managing your workspace, see [Manage an Azure Machine Learning workspace](machine-learning-manage-workspace.md).</span></span>
<span data-ttu-id="a1211-125">Ha a munkaterület létrehozását probléma adódik, tekintse meg [hibaelhárítási útmutatója: hozzon létre, és csatlakozzon a Machine Learning-munkaterület tooa](machine-learning-troubleshooting-creating-ml-workspace.md).</span><span class="sxs-lookup"><span data-stu-id="a1211-125">If you encounter a problem creating your workspace, see [Troubleshooting guide: Create and connect tooa Machine Learning workspace](machine-learning-troubleshooting-creating-ml-workspace.md).</span></span>


## <a name="sharing-an-azure-machine-learning-workspace"></a><span data-ttu-id="a1211-126">Egy Azure Machine Learning munkaterülettel megosztása</span><span class="sxs-lookup"><span data-stu-id="a1211-126">Sharing an Azure Machine Learning workspace</span></span>
<span data-ttu-id="a1211-127">A Machine Learning-munkaterület létrehozása után felajánlhatja a felhasználóknak tooyour munkaterület tooshare hozzáférés tooyour munkaterület és minden a kísérletek, adathalmazokat, notebookok stb. Hozzáadhat felhasználókat két szerepkör egyikében:</span><span class="sxs-lookup"><span data-stu-id="a1211-127">Once a Machine Learning workspace is created, you can invite users tooyour workspace tooshare access tooyour workspace and all its experiments, datasets, notebooks, etc. You can add users in one of two roles:</span></span>

* <span data-ttu-id="a1211-128">**Felhasználói** -munkaterület felhasználó is létrehozása, nyissa meg a, módosítása és törlése kísérleteket, adatkészleteket, hello munkaterületen stb.</span><span class="sxs-lookup"><span data-stu-id="a1211-128">**User** - A workspace user can create, open, modify, and delete experiments, datasets, etc. in hello workspace.</span></span>
* <span data-ttu-id="a1211-129">**Tulajdonos** - tulajdonos kérhetnek és eltávolíthasson felhasználókat hello munkaterületén, a felhasználó hozzáadása toowhat teheti meg.</span><span class="sxs-lookup"><span data-stu-id="a1211-129">**Owner** - An owner can invite and remove users in hello workspace, in addition toowhat a user can do.</span></span>

> [!NOTE]
> <span data-ttu-id="a1211-130">hello rendszergazdai fiók hello munkaterület létrehozó tulajdonos munkaterületként automatikusan kerül toohello munkaterület.</span><span class="sxs-lookup"><span data-stu-id="a1211-130">hello administrator account that creates hello workspace is automatically added toohello workspace as workspace Owner.</span></span> <span data-ttu-id="a1211-131">Azonban más rendszergazdák vagy a felhasználók, előfizetés nem automatikusan hozzáféréssel toohello munkaterület - tooinvite kell azokat explicit módon.</span><span class="sxs-lookup"><span data-stu-id="a1211-131">However, other administrators or users in that subscription are not automatically granted access toohello workspace - you need tooinvite them explicitly.</span></span>
> 
> 

### <a name="tooshare-a-workspace"></a><span data-ttu-id="a1211-132">a munkaterület tooshare</span><span class="sxs-lookup"><span data-stu-id="a1211-132">tooshare a workspace</span></span>

1. <span data-ttu-id="a1211-133">Jelentkezzen be tooMachine Learning Studióba [https://studio.azureml.net/Home](https://studio.azureml.net/Home)</span><span class="sxs-lookup"><span data-stu-id="a1211-133">Sign in tooMachine Learning Studio at [https://studio.azureml.net/Home](https://studio.azureml.net/Home)</span></span>

2. <span data-ttu-id="a1211-134">A hello bal oldali panelen kattintson a **beállítások**</span><span class="sxs-lookup"><span data-stu-id="a1211-134">In hello left panel, click **SETTINGS**</span></span>

3. <span data-ttu-id="a1211-135">Kattintson a hello **felhasználók** lap</span><span class="sxs-lookup"><span data-stu-id="a1211-135">Click hello **USERS** tab</span></span>

4. <span data-ttu-id="a1211-136">Kattintson a **több felhasználók MEGHÍVÁSA** hello lap hello aljához</span><span class="sxs-lookup"><span data-stu-id="a1211-136">Click **INVITE MORE USERS** at hello bottom of hello page</span></span>

    ![Studio beállításai](media/machine-learning-create-workspace/settings.png)

5. <span data-ttu-id="a1211-138">Adjon meg egy vagy több e-mail címet.</span><span class="sxs-lookup"><span data-stu-id="a1211-138">Enter one or more email addresses.</span></span> <span data-ttu-id="a1211-139">hello felhasználók kell egy érvényes Microsoft-fiókkal vagy szervezeti fiókkal (az Azure Active Directory).</span><span class="sxs-lookup"><span data-stu-id="a1211-139">hello users need a valid Microsoft account or an organizational account (from Azure Active Directory).</span></span>

6. <span data-ttu-id="a1211-140">Válassza ki, hogy tooadd hello felhasználók tulajdonosa vagy a felhasználó.</span><span class="sxs-lookup"><span data-stu-id="a1211-140">Select whether you want tooadd hello users as Owner or User.</span></span>

7. <span data-ttu-id="a1211-141">Kattintson a hello **OK** pipa gombra.</span><span class="sxs-lookup"><span data-stu-id="a1211-141">Click hello **OK** checkmark button.</span></span>

<span data-ttu-id="a1211-142">Minden felhasználóhoz hozzá lépéseit hogyan toosign toohello a megosztott munkaterület e-mailt fog kapni.</span><span class="sxs-lookup"><span data-stu-id="a1211-142">Each user you add will receive an email with instructions on how toosign in toohello shared workspace.</span></span>

> [!NOTE]
> <span data-ttu-id="a1211-143">A felhasználók toobe képes toodeploy vagy a munkaterület webszolgáltatások kezelése, a közreműködői vagy hello Azure-előfizetés rendszergazdája kell lenniük.</span><span class="sxs-lookup"><span data-stu-id="a1211-143">For users toobe able toodeploy or manage web services in this workspace, they must be a contributor or administrator in hello Azure subscription.</span></span> 



