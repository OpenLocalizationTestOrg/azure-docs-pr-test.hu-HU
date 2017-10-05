---
title: "A csoportok dinamikus tagság hibaelhárítása |} Microsoft Docs"
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
ms.openlocfilehash: 32947e8cc69c9a48d9a285bf0a37ab3398571f86
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshooting-dynamic-memberships-for-groups"></a><span data-ttu-id="a78fd-103">Dinamikus csoporttagságok hibaelhárítása</span><span class="sxs-lookup"><span data-stu-id="a78fd-103">Troubleshooting dynamic memberships for groups</span></span>
<span data-ttu-id="a78fd-104">**Egy szabály beállítása a csoport, de nincs tagságát módosul, a csoport**</span><span class="sxs-lookup"><span data-stu-id="a78fd-104">**I configured a rule on a group but no memberships get updated in the group**</span></span><br/><span data-ttu-id="a78fd-105">Ellenőrizze, hogy a **engedélyezése delegált csoportkezelés** beállítása **Igen** a a **konfigurálása** fülre.</span><span class="sxs-lookup"><span data-stu-id="a78fd-105">Verify that the **Enable delegated group management** setting is set to **Yes** in the **Configure** tab.</span></span> <span data-ttu-id="a78fd-106">Ez a beállítás csak akkor, ha Ön felhasználóként van bejelentkezve a felhasználó, akinek hozzá van rendelve egy Azure Active Directory Premium licenc jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="a78fd-106">You will see this setting only if you are signed in as a user to whom an Azure Active Directory Premium license is assigned.</span></span> <span data-ttu-id="a78fd-107">Ellenőrizze a szabály a felhasználói attribútumok értékeit: vannak-e, amely megfelel a szabálynak felhasználók?</span><span class="sxs-lookup"><span data-stu-id="a78fd-107">Verify the values for user attributes on the rule: are there users that satisfy the rule?</span></span>

<span data-ttu-id="a78fd-108">**Egy szabály beállítása, de most a szabály meglévő tagjai törlődnek**</span><span class="sxs-lookup"><span data-stu-id="a78fd-108">**I configured a rule, but now the existing members of the rule are removed**</span></span><br/><span data-ttu-id="a78fd-109">Ez az elvárt működés.</span><span class="sxs-lookup"><span data-stu-id="a78fd-109">This is expected behavior.</span></span> <span data-ttu-id="a78fd-110">A csoport tagjai meglévő törlődnek, amikor a szabály engedélyezve van-e módosítani.</span><span class="sxs-lookup"><span data-stu-id="a78fd-110">Existing members of the group are removed when a rule is enabled or changed.</span></span> <span data-ttu-id="a78fd-111">A szabály értékelése által visszaadott felhasználók tagjai a csoportba kerülnek.</span><span class="sxs-lookup"><span data-stu-id="a78fd-111">The users returned from evaluation of the rule are added as members to the group.</span></span>     

<span data-ttu-id="a78fd-112">**Nem szerepel az tagsága instantly hozzáadása vagy szabály, miért nem módosítása esetén?**</span><span class="sxs-lookup"><span data-stu-id="a78fd-112">**I don’t see membership changes instantly when I add or change a rule, why not?**</span></span><br/><span data-ttu-id="a78fd-113">Dedikált gyűjteménytagság értékelése egy aszinkron háttér folyamat rendszeres időközönként történik.</span><span class="sxs-lookup"><span data-stu-id="a78fd-113">Dedicated membership evaluation is done periodically in an asynchronous background process.</span></span> <span data-ttu-id="a78fd-114">Mennyi ideig tart határozza meg a könyvtárban lévő felhasználók számát és méretét a csoport, a szabály eredményeként jött létre.</span><span class="sxs-lookup"><span data-stu-id="a78fd-114">How long the process takes is determined by the number of users in your directory and the size of the group created as a result of the rule.</span></span> <span data-ttu-id="a78fd-115">Felhasználók kis mennyiségű mappák általában, megjelenik a csoporttagsági változások legfeljebb néhány perc múlva.</span><span class="sxs-lookup"><span data-stu-id="a78fd-115">Typically, directories with small numbers of users will see the group membership changes in less than a few minutes.</span></span> <span data-ttu-id="a78fd-116">A felhasználók sok könyvtárak 30 percet is igénybe vehet, vagy hosszabb adatokkal való feltöltéséhez.</span><span class="sxs-lookup"><span data-stu-id="a78fd-116">Directories with a large number of users can take 30 minutes or longer to populate.</span></span>

### <a name="next-steps"></a><span data-ttu-id="a78fd-117">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="a78fd-117">Next steps</span></span>
<span data-ttu-id="a78fd-118">E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="a78fd-118">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="a78fd-119">Erőforráshozzáférés-kezelés Azure Active Directory-csoportokkal</span><span class="sxs-lookup"><span data-stu-id="a78fd-119">Managing access to resources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="a78fd-120">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="a78fd-120">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="a78fd-121">Mi az az Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="a78fd-121">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="a78fd-122">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="a78fd-122">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
