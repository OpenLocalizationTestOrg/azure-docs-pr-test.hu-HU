---
title: "dinamikus csoporttagság aaaTroubleshooting csoportok |} Microsoft Docs"
description: "Dinamikus csoporttagság csoportok tippek az Azure ad-ben."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 89bb04b6-a379-49c2-8465-fe386641816a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: curtand
ms.openlocfilehash: d792fc406288844e2c5dbe3702c2c9870d09473e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a><span data-ttu-id="d64b2-103">Dinamikus csoporttagságok hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="d64b2-103">Troubleshooting dynamic memberships for groups</span></span>
<span data-ttu-id="d64b2-104">**Egy szabály beállítása a csoporton, de nincs tagságát frissített hello csoportban**</span><span class="sxs-lookup"><span data-stu-id="d64b2-104">**I configured a rule on a group but no memberships get updated in hello group**</span></span><br/><span data-ttu-id="d64b2-105">Győződjön meg arról, hogy hello **engedélyezése delegált csoportkezelés** beállítás értéke túl**Igen** a hello **konfigurálása** fülre. Ez a beállítás csak akkor, ha nincs bejelentkezve a felhasználó toowhom egy Azure Active Directory Premium licenc rendeli jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="d64b2-105">Verify that hello **Enable delegated group management** setting is set too**Yes** in hello **Configure** tab. You will see this setting only if you are signed in as a user toowhom an Azure Active Directory Premium license is assigned.</span></span> <span data-ttu-id="d64b2-106">Ellenőrizze a felhasználói attribútumokat a hello szabály hello értékeket: vannak-e, amely megfelel a hello szabály felhasználók?</span><span class="sxs-lookup"><span data-stu-id="d64b2-106">Verify hello values for user attributes on hello rule: are there users that satisfy hello rule?</span></span>

<span data-ttu-id="d64b2-107">**Egy szabály beállítása, de most hello meglévő hello szabály tagjai törlődnek**</span><span class="sxs-lookup"><span data-stu-id="d64b2-107">**I configured a rule, but now hello existing members of hello rule are removed**</span></span><br/><span data-ttu-id="d64b2-108">Ez az elvárt működés.</span><span class="sxs-lookup"><span data-stu-id="d64b2-108">This is expected behavior.</span></span> <span data-ttu-id="d64b2-109">Hello csoport tagjai meglévő törlődnek, amikor a szabály engedélyezve van-e módosítani.</span><span class="sxs-lookup"><span data-stu-id="d64b2-109">Existing members of hello group are removed when a rule is enabled or changed.</span></span> <span data-ttu-id="d64b2-110">hello szabály értékelése által visszaadott hello felhasználók tagok toohello csoport hozzá szeretné adni.</span><span class="sxs-lookup"><span data-stu-id="d64b2-110">hello users returned from evaluation of hello rule are added as members toohello group.</span></span>     

<span data-ttu-id="d64b2-111">**Nem szerepel az tagsága instantly hozzáadása vagy szabály, miért nem módosítása esetén?**</span><span class="sxs-lookup"><span data-stu-id="d64b2-111">**I don’t see membership changes instantly when I add or change a rule, why not?**</span></span><br/><span data-ttu-id="d64b2-112">Dedikált gyűjteménytagság értékelése egy aszinkron háttér folyamat rendszeres időközönként történik.</span><span class="sxs-lookup"><span data-stu-id="d64b2-112">Dedicated membership evaluation is done periodically in an asynchronous background process.</span></span> <span data-ttu-id="d64b2-113">Mennyi ideig hello folyamat veszi hello csoport hello szabály eredményeként jött létre a könyvtár és hello méretű felhasználók hello számát határozza meg.</span><span class="sxs-lookup"><span data-stu-id="d64b2-113">How long hello process takes is determined by hello number of users in your directory and hello size of hello group created as a result of hello rule.</span></span> <span data-ttu-id="d64b2-114">Felhasználók kis mennyiségű mappák általában, lásd a hello csoporttagsági változások legfeljebb néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="d64b2-114">Typically, directories with small numbers of users will see hello group membership changes in less than a few minutes.</span></span> <span data-ttu-id="d64b2-115">A felhasználók sok könyvtárak is igénybe vehet, 30 perc vagy hosszabb toopopulate.</span><span class="sxs-lookup"><span data-stu-id="d64b2-115">Directories with a large number of users can take 30 minutes or longer toopopulate.</span></span>

### <a name="next-steps"></a><span data-ttu-id="d64b2-116">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="d64b2-116">Next steps</span></span>
<span data-ttu-id="d64b2-117">E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="d64b2-117">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="d64b2-118">Hozzáférés tooresources kezelése az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d64b2-118">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="d64b2-119">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="d64b2-119">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="d64b2-120">Mi az az Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="d64b2-120">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="d64b2-121">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="d64b2-121">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
