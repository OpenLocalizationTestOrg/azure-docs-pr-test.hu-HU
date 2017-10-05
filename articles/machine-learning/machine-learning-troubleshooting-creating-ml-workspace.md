---
title: "Hibaelhárítás: Hozzon létre, és kapcsolódjon a Machine Learning-munkaterület |} Microsoft Docs"
description: "Megoldások gyakori problémákat létrehozása és az Azure Machine Learning-munkaterülethez csatlakozik"
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
ms.openlocfilehash: 398ac3d9c9d32a1ab10413ce0d7ce8d448890409
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-guide-create-and-connect-to-an-machine-learning-workspace"></a><span data-ttu-id="f7160-103">Hibaelhárítási útmutató: Machine Learning-munkaterület létrehozása és kapcsolódás a Machine Learning-munkaterülethez</span><span class="sxs-lookup"><span data-stu-id="f7160-103">Troubleshooting guide: Create and connect to an Machine Learning workspace</span></span>
<span data-ttu-id="f7160-104">Ez az útmutató egyes megoldások gyakran ütközött kihívást állítja be az Azure Machine Learning munkaterületek.</span><span class="sxs-lookup"><span data-stu-id="f7160-104">This guide provides solutions for some frequently encountered challenges when you are setting up Azure Machine Learning workspaces.</span></span>

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]

## <a name="workspace-owner"></a><span data-ttu-id="f7160-105">Munkaterület tulajdonosa</span><span class="sxs-lookup"><span data-stu-id="f7160-105">Workspace owner</span></span>
<span data-ttu-id="f7160-106">A munkaterület megnyitásához a Machine Learning Studióban be kell jelentkeznie Microsoft Account a munkaterület létrehozásához használt, vagy meghívót kapott a tulajdonos a munkaterület csatlakozni szeretne.</span><span class="sxs-lookup"><span data-stu-id="f7160-106">To open a workspace in Machine Learning Studio, you must be signed in to the Microsoft Account you used to create the workspace, or you need to receive an invitation from the owner to join the workspace.</span></span> <span data-ttu-id="f7160-107">Az Azure-portálon kezelheti a munkaterületen található, amely lehetővé teszi a hozzáférés konfigurálása.</span><span class="sxs-lookup"><span data-stu-id="f7160-107">From the Azure portal you can manage the workspace, which includes the ability to configure access.</span></span>

<span data-ttu-id="f7160-108">A munkaterület kezeléséről további információkért lásd: [kezelése az Azure Machine Learning-munkaterület].</span><span class="sxs-lookup"><span data-stu-id="f7160-108">For more information on managing a workspace, see [Manage an Azure Machine Learning workspace].</span></span>

<span data-ttu-id="f7160-109">[kezelése az Azure Machine Learning-munkaterület]: machine-learning-manage-workspace.md</span><span class="sxs-lookup"><span data-stu-id="f7160-109">[Manage an Azure Machine Learning workspace]: machine-learning-manage-workspace.md</span></span>

## <a name="allowed-regions"></a><span data-ttu-id="f7160-110">Engedélyezett régiók</span><span class="sxs-lookup"><span data-stu-id="f7160-110">Allowed regions</span></span>
<span data-ttu-id="f7160-111">Gépi tanulás már régiók korlátozott számú érhető el.</span><span class="sxs-lookup"><span data-stu-id="f7160-111">Machine Learning is currently available in a limited number of regions.</span></span> <span data-ttu-id="f7160-112">Az előfizetés nem tartalmazza a régiók egyikéhez sem, ha a hiba üzenet jelenhet meg, "Nincs előfizetése van a megengedett régiókban."</span><span class="sxs-lookup"><span data-stu-id="f7160-112">If your subscription does not include one of these regions, you may see the error message, “You have no subscriptions in the allowed regions.”</span></span>

<span data-ttu-id="f7160-113">Kérelem, az előfizetés fel egy régiót, hozzon létre egy új Microsoft-támogatási kérelmet az Azure-portálon, válassza a **számlázási** probléma típusa és kövesse a megjelenő utasításokat küldje el a kérést.</span><span class="sxs-lookup"><span data-stu-id="f7160-113">To request that a region be added to your subscription, create a new Microsoft support request from the Azure portal, choose **Billing** as the problem type, and follow the prompts to submit your request.</span></span>

## <a name="storage-account"></a><span data-ttu-id="f7160-114">Tárfiók</span><span class="sxs-lookup"><span data-stu-id="f7160-114">Storage account</span></span>
<span data-ttu-id="f7160-115">A Machine Learning szolgáltatás szükséges a tárfiók adatok tárolására.</span><span class="sxs-lookup"><span data-stu-id="f7160-115">The Machine Learning service needs a storage account to store data.</span></span> <span data-ttu-id="f7160-116">Meglévő tárfiókot is használhatja, vagy létrehozhat egy új tárfiókot, az új Machine Learning munkaterületének létrehozásakor (ha van kvótát egy új tárfiók létrehozása).</span><span class="sxs-lookup"><span data-stu-id="f7160-116">You can use an existing storage account, or you can create a new storage account when you create the new Machine Learning workspace (if you have quota to create a new storage account).</span></span>

<span data-ttu-id="f7160-117">Az új Machine Learning-munkaterület létrehozása után is bejelentkezik a Machine Learning Studio a munkaterület létrehozásához használt Microsoft-fiók használatával.</span><span class="sxs-lookup"><span data-stu-id="f7160-117">After the new Machine Learning workspace is created, you can sign in to Machine Learning Studio by using the Microsoft account you used to create the workspace.</span></span> <span data-ttu-id="f7160-118">Ha a hibaüzenet a következő, "Munkaterület nem található" (az alábbi képernyőfelvételhez hasonló) észlel, akkor használja a következő lépéseket a böngésző cookie-k törléséhez.</span><span class="sxs-lookup"><span data-stu-id="f7160-118">If you encounter the error message, “Workspace Not Found” (similar to the following screenshot), please use the following steps to delete your browser cookies.</span></span>

![Nem található munkaterület][screen3]

<span data-ttu-id="f7160-120">**Törli a böngésző cookie-k**</span><span class="sxs-lookup"><span data-stu-id="f7160-120">**To delete browser cookies**</span></span>

1. <span data-ttu-id="f7160-121">Ha az Internet Explorer, kattintson a **eszközök** gombra a jobb felső sarokban, majd az **Internetbeállítások**.</span><span class="sxs-lookup"><span data-stu-id="f7160-121">If you use Internet Explorer, click the **Tools** button in the upper-right corner and select **Internet options**.</span></span>  

![Internetbeállítások][screen4]

2. <span data-ttu-id="f7160-123">Az a **általános** lapra, majd **törlése...**</span><span class="sxs-lookup"><span data-stu-id="f7160-123">Under the **General** tab, click **Delete…**</span></span>

![Általános lap][screen5]

3. <span data-ttu-id="f7160-125">A a **böngészési előzmények törlése** párbeszédpanelen győződjön meg arról, hogy **cookie-k és a webhely adatok** van kiválasztva, és kattintson a **törlése**.</span><span class="sxs-lookup"><span data-stu-id="f7160-125">In the **Delete Browsing History** dialog box, make sure **Cookies and website data** is selected, and click **Delete**.</span></span>

![Törölje a cookie-k][screen6]

<span data-ttu-id="f7160-127">Miután a cookie-k törlődnek, indítsa újra a böngészőt, és folytassa a a [Microsoft Azure Machine Learning](https://studio.azureml.net) lap.</span><span class="sxs-lookup"><span data-stu-id="f7160-127">After the cookies are deleted, restart the browser and then go to the [Microsoft Azure Machine Learning](https://studio.azureml.net) page.</span></span> <span data-ttu-id="f7160-128">Amikor a felhasználónevet és jelszót kéri, adja meg a Microsoft-fiók a munkaterület létrehozásához használt.</span><span class="sxs-lookup"><span data-stu-id="f7160-128">When you are prompted for a user name and password, enter the same Microsoft account you used to create the workspace.</span></span>

## <a name="comments"></a><span data-ttu-id="f7160-129">Megjegyzések</span><span class="sxs-lookup"><span data-stu-id="f7160-129">Comments</span></span>

<span data-ttu-id="f7160-130">Célunk, zökkenőmentes lehető ellenőrizze a Machine Learning élmény.</span><span class="sxs-lookup"><span data-stu-id="f7160-130">Our goal is to make the Machine Learning experience as seamless as possible.</span></span> <span data-ttu-id="f7160-131">Tegye a megjegyzések és részén a problémák a [Azure Machine Learning fórum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) elküldésével kiszolgálására jobb.</span><span class="sxs-lookup"><span data-stu-id="f7160-131">Please post any comments and issues at the [Azure Machine Learning forum](http://social.msdn.microsoft.com/Forums/windowsazure/home?forum=MachineLearning) to help us serve you better.</span></span>

[screen1]:media/machine-learning-troubleshooting-creating-ml-workspace/screen1.png
[screen2]:media/machine-learning-troubleshooting-creating-ml-workspace/screen2.png
[screen3]:media/machine-learning-troubleshooting-creating-ml-workspace/screen3.png
[screen4]:media/machine-learning-troubleshooting-creating-ml-workspace/screen4.png
[screen5]:media/machine-learning-troubleshooting-creating-ml-workspace/screen5.png
[screen6]:media/machine-learning-troubleshooting-creating-ml-workspace/screen6.png
