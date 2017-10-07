---
title: "Hibáinak elhárítása: Hozzon létre, és csatlakozzon a Machine Learning-munkaterület tooa |} Microsoft Docs"
description: "A gyakori problémákat hoz létre és tooan Azure Machine Learning-munkaterület csatlakoztatja a megoldások"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 1a8aec4b-35f9-44e8-9570-2575b8979ab1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
ms.openlocfilehash: 965a0025e85ba4e22c2b037edfa923e7f7599069
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-guide-create-and-connect-tooan-machine-learning-workspace"></a><span data-ttu-id="07293-103">Hibaelhárítási útmutató: hozzon létre, és csatlakozzon a Machine Learning-munkaterület tooan</span><span class="sxs-lookup"><span data-stu-id="07293-103">Troubleshooting guide: Create and connect tooan Machine Learning workspace</span></span>
<span data-ttu-id="07293-104">Ez az útmutató egyes megoldások gyakran ütközött kihívást állítja be az Azure Machine Learning munkaterületek.</span><span class="sxs-lookup"><span data-stu-id="07293-104">This guide provides solutions for some frequently encountered challenges when you are setting up Azure Machine Learning workspaces.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a><span data-ttu-id="07293-105">Munkaterület tulajdonosa</span><span class="sxs-lookup"><span data-stu-id="07293-105">Workspace owner</span></span>
<span data-ttu-id="07293-106">a Machine Learning Studio munkaterületének tooopen, kell lennie toohello Microsoft Account bejelentkezve toocreate hello munkaterület használt, vagy tooreceive hello tulajdonos toojoin hello munkaterületről meghívót van szüksége.</span><span class="sxs-lookup"><span data-stu-id="07293-106">tooopen a workspace in Machine Learning Studio, you must be signed in toohello Microsoft Account you used toocreate hello workspace, or you need tooreceive an invitation from hello owner toojoin hello workspace.</span></span> <span data-ttu-id="07293-107">Hello Azure-portálon kezelheti tartoznak a hello képességét tooconfigure hello munkaterületen.</span><span class="sxs-lookup"><span data-stu-id="07293-107">From hello Azure portal you can manage hello workspace, which includes hello ability tooconfigure access.</span></span>

<span data-ttu-id="07293-108">A munkaterület kezeléséről további információkért lásd: [kezelése az Azure Machine Learning-munkaterület].</span><span class="sxs-lookup"><span data-stu-id="07293-108">For more information on managing a workspace, see [Manage an Azure Machine Learning workspace].</span></span>

[kezelése az Azure Machine Learning-munkaterület]: machine-learning-manage-workspace.md

## <a name="allowed-regions"></a><span data-ttu-id="07293-110">Engedélyezett régiók</span><span class="sxs-lookup"><span data-stu-id="07293-110">Allowed regions</span></span>
<span data-ttu-id="07293-111">Gépi tanulás már régiók korlátozott számú érhető el.</span><span class="sxs-lookup"><span data-stu-id="07293-111">Machine Learning is currently available in a limited number of regions.</span></span> <span data-ttu-id="07293-112">Az előfizetés nem tartalmazza a régiók egyikéhez sem, ha lehetséges, hogy hello hibaüzenet jelenik meg, "Nincs előfizetése van az engedélyezett régiók hello."</span><span class="sxs-lookup"><span data-stu-id="07293-112">If your subscription does not include one of these regions, you may see hello error message, “You have no subscriptions in hello allowed regions.”</span></span>

<span data-ttu-id="07293-113">amely egy régiót kell toorequest hozzáadott tooyour előfizetés, egy új Microsoft-támogatási kérelem létrehozása hello Azure-portálon, majd a **számlázási** hello probléma típusa, és hajtsa végre hello kéri toosubmit a kérést.</span><span class="sxs-lookup"><span data-stu-id="07293-113">toorequest that a region be added tooyour subscription, create a new Microsoft support request from hello Azure portal, choose **Billing** as hello problem type, and follow hello prompts toosubmit your request.</span></span>

## <a name="storage-account"></a><span data-ttu-id="07293-114">Tárfiók</span><span class="sxs-lookup"><span data-stu-id="07293-114">Storage account</span></span>
<span data-ttu-id="07293-115">hello Machine Learning szolgáltatás a tárolási fiók toostore adatokra van szüksége.</span><span class="sxs-lookup"><span data-stu-id="07293-115">hello Machine Learning service needs a storage account toostore data.</span></span> <span data-ttu-id="07293-116">Meglévő tárfiókot is használhatja, vagy létrehozhat egy új tárfiókot, (ha kvóta toocreate új tárfiók) hello új Machine Learning munkaterületének létrehozásakor.</span><span class="sxs-lookup"><span data-stu-id="07293-116">You can use an existing storage account, or you can create a new storage account when you create hello new Machine Learning workspace (if you have quota toocreate a new storage account).</span></span>

<span data-ttu-id="07293-117">Hello új Machine Learning-munkaterület létrehozása után által használt toocreate hello munkaterület hello Microsoft-fiókjával bejelentkezhet tooMachine Learning Studióban.</span><span class="sxs-lookup"><span data-stu-id="07293-117">After hello new Machine Learning workspace is created, you can sign in tooMachine Learning Studio by using hello Microsoft account you used toocreate hello workspace.</span></span> <span data-ttu-id="07293-118">Ha hello hibaüzenet jelenik meg, "Munkaterület nem található" (hasonló toohello a következő képernyőkép), használja a következő lépéseket toodelete hello a böngésző cookie-kat.</span><span class="sxs-lookup"><span data-stu-id="07293-118">If you encounter hello error message, “Workspace Not Found” (similar toohello following screenshot), please use hello following steps toodelete your browser cookies.</span></span>

![Nem található munkaterület][screen3]

<span data-ttu-id="07293-120">**toodelete böngésző cookie-k**</span><span class="sxs-lookup"><span data-stu-id="07293-120">**toodelete browser cookies**</span></span>

1. <span data-ttu-id="07293-121">Ha az Internet Explorer, kattintson a hello **eszközök** hello jobb felső sarkában található gombra, majd az **Internetbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="07293-121">If you use Internet Explorer, click hello **Tools** button in hello upper-right corner and select **Internet options**.</span></span>  

![Internetbeállítások][screen4]

2. <span data-ttu-id="07293-123">A hello **általános** lapra, majd **törlése...**</span><span class="sxs-lookup"><span data-stu-id="07293-123">Under hello **General** tab, click **Delete…**</span></span>

![Általános lap][screen5]

3. <span data-ttu-id="07293-125">A hello **böngészési előzmények törlése** párbeszédpanelen győződjön meg arról, hogy **cookie-k és a webhely adatok** van kiválasztva, és kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="07293-125">In hello **Delete Browsing History** dialog box, make sure **Cookies and website data** is selected, and click **Delete**.</span></span>

![Törölje a cookie-k][screen6]

<span data-ttu-id="07293-127">Miután hello cookie-k törlődnek, indítsa újra hello böngészőt, és folytassa a toohello [Microsoft Azure Machine Learning](https://studio.azureml.net) lap.</span><span class="sxs-lookup"><span data-stu-id="07293-127">After hello cookies are deleted, restart hello browser and then go toohello [Microsoft Azure Machine Learning](https://studio.azureml.net) page.</span></span> <span data-ttu-id="07293-128">Amikor a rendszer kéri a felhasználónevet és jelszót, írja be a hello toocreate hello munkaterület használt Microsoft-fiók.</span><span class="sxs-lookup"><span data-stu-id="07293-128">When you are prompted for a user name and password, enter hello same Microsoft account you used toocreate hello workspace.</span></span>

## <a name="comments"></a><span data-ttu-id="07293-129">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="07293-129">Comments</span></span>

<span data-ttu-id="07293-130">Célunk toomake hello Machine Learning élmény, a lehető zökkenőmentes.</span><span class="sxs-lookup"><span data-stu-id="07293-130">Our goal is toomake hello Machine Learning experience as seamless as possible.</span></span> <span data-ttu-id="07293-131">Tegye a megjegyzések és a problémák, hello [Azure Machine Learning fórum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) velünk kiszolgálására, jobb toohelp.</span><span class="sxs-lookup"><span data-stu-id="07293-131">Please post any comments and issues at hello [Azure Machine Learning forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) toohelp us serve you better.</span></span>

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
