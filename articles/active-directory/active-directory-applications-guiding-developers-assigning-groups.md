---
title: "aaaAssign csoportok tooAzure AD alkalmazások |} Microsoft Docs"
description: "Hogyan tooimplement csoport hozzárendelése az Azure-alkalmazások."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 29b5ba89-a1c7-4f1f-a294-248a40106617
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: kgremban
ms.custom: H1Hack27Feb2017
robots: noindex
ms.openlocfilehash: 086619df09c13bf259afc3128d45ed804b99e519
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="assign-azure-active-directory-groups-tooan-application"></a><span data-ttu-id="1a07f-103">Rendelje hozzá az Active Directory-csoportok tooan alkalmazás</span><span class="sxs-lookup"><span data-stu-id="1a07f-103">Assign Azure Active Directory groups tooan application</span></span>
<span data-ttu-id="1a07f-104">Mielőtt hozzárendelheti a felhasználók és csoportok tooan alkalmazás, felhasználó-hozzárendelés követelheti meg.</span><span class="sxs-lookup"><span data-stu-id="1a07f-104">Before you can assign users and groups tooan application, you must require user assignment.</span></span> <span data-ttu-id="1a07f-105">toolearn hogyan toorequire felhasználói kiosztása, lásd: hello [igénylő felhasználó hozzárendelés](active-directory-applications-guiding-developers-requiring-user-assignment.md) cikk.</span><span class="sxs-lookup"><span data-stu-id="1a07f-105">toolearn how toorequire user assignment, see hello [Requiring User Assignment](active-directory-applications-guiding-developers-requiring-user-assignment.md) article.</span></span>

<span data-ttu-id="1a07f-106">Ez a cikk feltételezi, hogy már létrehozott csoportok hello active Directory-alkalmazás használ.</span><span class="sxs-lookup"><span data-stu-id="1a07f-106">This article assumes that you have already created groups in hello active directory you are using for this application.</span></span>

## <a name="assigning-groups-tooan-application"></a><span data-ttu-id="1a07f-107">Csoportok tooan alkalmazás hozzárendelése</span><span class="sxs-lookup"><span data-stu-id="1a07f-107">Assigning Groups tooan Application</span></span>
1. <span data-ttu-id="1a07f-108">Jelentkezzen be toohello Azure portálra rendszergazdaként.</span><span class="sxs-lookup"><span data-stu-id="1a07f-108">Log in toohello Azure portal with an administrator account.</span></span>
2. <span data-ttu-id="1a07f-109">Kattintson a hello **minden elem** hello főmenü elemére.</span><span class="sxs-lookup"><span data-stu-id="1a07f-109">Click on hello **All Items** item in hello main menu.</span></span>
3. <span data-ttu-id="1a07f-110">Válassza ki a hello directory hello alkalmazás használja.</span><span class="sxs-lookup"><span data-stu-id="1a07f-110">Choose hello directory you are using for hello application.</span></span>
4. <span data-ttu-id="1a07f-111">Kattintson a hello **alkalmazások** fülre.</span><span class="sxs-lookup"><span data-stu-id="1a07f-111">Click on hello **APPLICATIONS** tab.</span></span>
5. <span data-ttu-id="1a07f-112">Válassza ki a hello alkalmazást ebben a könyvtárban társított alkalmazások hello listája.</span><span class="sxs-lookup"><span data-stu-id="1a07f-112">Select hello application from hello list of applications associated with this directory.</span></span>
6. <span data-ttu-id="1a07f-113">Kattintson a hello **felhasználók és csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="1a07f-113">Click hello **USERS AND GROUPS** tab.</span></span>
7. <span data-ttu-id="1a07f-114">Szűrő hello listáját az active Directoryban hello segítségével **csoportok** legördülő listából.</span><span class="sxs-lookup"><span data-stu-id="1a07f-114">Filter hello list of groups in your active directory by using hello **Groups** dropdown list.</span></span>
8. <span data-ttu-id="1a07f-115">Válassza ki a hello csoportot.</span><span class="sxs-lookup"><span data-stu-id="1a07f-115">Select hello group.</span></span>
9. <span data-ttu-id="1a07f-116">Kattintson a **hozzárendelése**.</span><span class="sxs-lookup"><span data-stu-id="1a07f-116">Click **ASSIGN**.</span></span>
10. <span data-ttu-id="1a07f-117">Kattintson a **Igen** megjelenésekor.</span><span class="sxs-lookup"><span data-stu-id="1a07f-117">Click **yes** when prompted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1a07f-118">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="1a07f-118">Next Steps</span></span>
[!INCLUDE [active-directory-applications-guiding-developers-for-lob-applications-toc.md](../../includes/active-directory-applications-guiding-developers-for-lob-applications-toc.md)]
