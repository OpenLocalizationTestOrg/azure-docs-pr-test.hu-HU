---
title: az Azure Active Directoryban aaaManaging csoportok |} Microsoft Docs
description: "Hogyan toocreate és csoportok toomanage Azure kezelése Azure Active Directory használatával felhasználókat."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: d1f5451c-3807-423c-8bac-2822d27b893f
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 07/24/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 9bee224655639740b3dd99983892b30c3c537aa0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="managing-groups-in-azure-active-directory"></a><span data-ttu-id="bfa9f-103">Csoportkezelés az Azure Active Directoryban</span><span class="sxs-lookup"><span data-stu-id="bfa9f-103">Managing groups in Azure Active Directory</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bfa9f-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="bfa9f-104">Azure portal</span></span>](active-directory-groups-create-azure-portal.md)
> * [<span data-ttu-id="bfa9f-105">klasszikus Azure portál</span><span class="sxs-lookup"><span data-stu-id="bfa9f-105">Azure classic portal</span></span>](active-directory-accessmanagement-manage-groups.md)
> * [<span data-ttu-id="bfa9f-106">PowerShell</span><span class="sxs-lookup"><span data-stu-id="bfa9f-106">PowerShell</span></span>](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

<span data-ttu-id="bfa9f-107">Hello szolgáltatások az Azure Active Directory (Azure AD) felhasználó felügyeleti egyik hello képességét toocreate felhasználók csoportját.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-107">One of hello features of Azure Active Directory (Azure AD) user management is hello ability toocreate groups of users.</span></span> <span data-ttu-id="bfa9f-108">Egy csoport tooperform felügyeleti feladatokhoz, mint az hozzárendelése licenccel, illetve engedélyeket felhasználók száma tooa egyszerre használja.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-108">You use a group tooperform management tasks such as assigning licenses or permissions tooa number of users at once.</span></span> <span data-ttu-id="bfa9f-109">Csoportok tooassign engedéllyel is használható</span><span class="sxs-lookup"><span data-stu-id="bfa9f-109">You can also use groups tooassign access permission to</span></span>

* <span data-ttu-id="bfa9f-110">Erőforrások, például a hello directory objektumok</span><span class="sxs-lookup"><span data-stu-id="bfa9f-110">Resources such as objects in hello directory</span></span>
* <span data-ttu-id="bfa9f-111">Erőforrások külső toohello címtár, például az SaaS-alkalmazásokhoz, Azure-szolgáltatásokkal, SharePoint-webhelyek vagy a helyszíni erőforrások</span><span class="sxs-lookup"><span data-stu-id="bfa9f-111">Resources external toohello directory such as SaaS applications, Azure services, SharePoint sites, or on-premises resources</span></span>

<span data-ttu-id="bfa9f-112">Ezenkívül erőforrás tulajdonosa is hozzárendelhetők hozzáférés tooa erőforrás tooan az Azure AD-csoport valaki más a tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-112">In addition, a resource owner can also assign access tooa resource tooan Azure AD group owned by someone else.</span></span> <span data-ttu-id="bfa9f-113">Ez a hozzárendelés tagjai hello csoport hozzáférés toohello erőforrás.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-113">This assignment grants hello members of that group access toohello resource.</span></span> <span data-ttu-id="bfa9f-114">Ezt követően hello csoport hello tulajdonosa kezeli hello csoport tagjának kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-114">Then, hello owner of hello group manages membership in hello group.</span></span> <span data-ttu-id="bfa9f-115">Gyakorlatilag hello erőforrás tulajdonosa delegáltak toohello hello csoport hello engedély tooassign felhasználók tootheir erőforrás tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-115">Effectively, hello resource owner delegates toohello owner of hello group hello permission tooassign users tootheir resource.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bfa9f-116">A Microsoft azt javasolja, hogy a hello használata az Azure AD kezelése [az Azure AD felügyeleti központban](https://aad.portal.azure.com) hello az Azure portál használata helyett hello hivatkozott ebben a cikkben a klasszikus Azure portálon.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-116">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> <span data-ttu-id="bfa9f-117">Hogyan toomanage hello Azure AD felügyeleti központban csoportosítja, lásd: [hozzon létre egy csoportot, és tagokat vehet az Azure Active Directoryban](active-directory-groups-create-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="bfa9f-117">For how toomanage groups in hello Azure AD admin center, see [Create a group and add members in Azure Active Directory](active-directory-groups-create-azure-portal.md).</span></span>

## <a name="how-do-i-create-a-group"></a><span data-ttu-id="bfa9f-118">Hogyan hozható létre csoport?</span><span class="sxs-lookup"><span data-stu-id="bfa9f-118">How do I create a group?</span></span>
<span data-ttu-id="bfa9f-119">Attól függően, hogy hello szolgáltatások toowhich a szervezet kér le létrehozhat egy csoportot a következő hello:</span><span class="sxs-lookup"><span data-stu-id="bfa9f-119">Depending on hello services toowhich your organization has subscribed, you can create a group using one of hello following:</span></span>

* <span data-ttu-id="bfa9f-120">hello a klasszikus Azure portálon</span><span class="sxs-lookup"><span data-stu-id="bfa9f-120">hello Azure classic portal</span></span>
* <span data-ttu-id="bfa9f-121">hello Office 365 fiókportálon</span><span class="sxs-lookup"><span data-stu-id="bfa9f-121">hello Office 365 account portal</span></span>
* <span data-ttu-id="bfa9f-122">hello Windows Intune-fiókportál</span><span class="sxs-lookup"><span data-stu-id="bfa9f-122">hello Windows Intune account portal</span></span>

<span data-ttu-id="bfa9f-123">Feladatok, a klasszikus Azure portálon hello végre ismerteti.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-123">We'll describe tasks as performed in hello Azure classic portal.</span></span> <span data-ttu-id="bfa9f-124">Az Azure AD-címtár nem Azure portálon toomanage használatáról további információk: [az Azure AD-címtár felügyelete](active-directory-administer.md).</span><span class="sxs-lookup"><span data-stu-id="bfa9f-124">For more information about using non-Azure portals toomanage your Azure AD directory, see [Administering your Azure AD directory](active-directory-administer.md).</span></span>

1. <span data-ttu-id="bfa9f-125">A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd válassza ki a szervezet hello hello könyvtár nevét.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-125">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select hello name of hello directory for your organization.</span></span>
2. <span data-ttu-id="bfa9f-126">Jelölje be hello **csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-126">Select hello **Groups** tab.</span></span>
3. <span data-ttu-id="bfa9f-127">Válassza ki a **Csoport hozzáadása** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-127">Select **Add Group**.</span></span>
4. <span data-ttu-id="bfa9f-128">A hello **csoport hozzáadása** ablakban hello nevét adja meg, és a csoport leírása hello.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-128">In hello **Add Group** window, specify hello name and hello description of a group.</span></span>

## <a name="how-do-i-add-or-remove-individual-users-in-a-security-group"></a><span data-ttu-id="bfa9f-129">Hogyan lehet adott felhasználókat felvenni egy biztonsági csoportba vagy eltávolítani onnan?</span><span class="sxs-lookup"><span data-stu-id="bfa9f-129">How do I add or remove individual users in a security group?</span></span>
<span data-ttu-id="bfa9f-130">**egy adott felhasználó tooa csoport tooadd**</span><span class="sxs-lookup"><span data-stu-id="bfa9f-130">**tooadd an individual user tooa group**</span></span>

1. <span data-ttu-id="bfa9f-131">A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd válassza ki a szervezet hello hello könyvtár nevét.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-131">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select hello name of hello directory for your organization.</span></span>
2. <span data-ttu-id="bfa9f-132">Jelölje be hello **csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-132">Select hello **Groups** tab.</span></span>
3. <span data-ttu-id="bfa9f-133">Nyissa meg a kívánt tooadd tagok hello csoport toowhich.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-133">Open hello group toowhich you want tooadd members.</span></span> <span data-ttu-id="bfa9f-134">Nyissa meg hello **tagok** hello lapján csoport ki, ha azt már nem megjelenítése.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-134">Open hello **Members** tab of hello selected group if it not already displaying.</span></span>
4. <span data-ttu-id="bfa9f-135">Válassza ki a **Tagok hozzáadása** lehetőséget.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-135">Select **Add Members**.</span></span>
5. <span data-ttu-id="bfa9f-136">A hello **tagok hozzáadása** lap, hello felhasználó vagy egy csoport, amelyet az tooadd, ez a csoport tagjaként válassza hello nevét.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-136">On hello **Add Members** page, select hello name of hello user or a group that you want tooadd as a member of this group.</span></span> <span data-ttu-id="bfa9f-137">Győződjön meg arról, hogy ez a név szerepel-e toohello **kijelölt** ablaktáblán.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-137">Make sure that this name is added toohello **Selected** pane.</span></span>

<span data-ttu-id="bfa9f-138">**tooremove egy felhasználót egy csoportból**</span><span class="sxs-lookup"><span data-stu-id="bfa9f-138">**tooremove an individual user from a group**</span></span>

1. <span data-ttu-id="bfa9f-139">A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd válassza ki a szervezet hello hello könyvtár nevét.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-139">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select hello name of hello directory for your organization.</span></span>
2. <span data-ttu-id="bfa9f-140">Jelölje be hello **csoportok** fülre.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-140">Select hello **Groups** tab.</span></span>
3. <span data-ttu-id="bfa9f-141">Nyissa meg a hello csoportot, amelyből el kívánja tooremove tagokat.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-141">Open hello group from which you want tooremove members.</span></span>
4. <span data-ttu-id="bfa9f-142">Jelölje be hello **tagok** lap, válassza hello neve hello tag szeretné, hogy tooremove ebből a csoportból, és kattintson **eltávolítása**.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-142">Select hello **Members** tab, select hello name of hello member that you want tooremove from this group, and then click **Remove**.</span></span>
5. <span data-ttu-id="bfa9f-143">Erősítse meg hello parancssorba tooremove a hello csoport tagja.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-143">Confirm at hello prompt that you want tooremove this member from hello group.</span></span>

## <a name="how-can-i-manage-hello-membership-of-a-group-dynamically"></a><span data-ttu-id="bfa9f-144">Hogyan kezelhetők a csoportok tagsága hello dinamikusan?</span><span class="sxs-lookup"><span data-stu-id="bfa9f-144">How can I manage hello membership of a group dynamically?</span></span>
<span data-ttu-id="bfa9f-145">Az Azure AD-nagyon egyszerűen állíthat be egy egyszerű szabályt toodetermine, mely felhasználók toobe hello csoport tagjai.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-145">In Azure AD, you can very easily set up a simple rule toodetermine which users are toobe members of hello group.</span></span> <span data-ttu-id="bfa9f-146">Az egyszerű szabály egy olyan szabály, amely csak egyszeres összehasonlítást végez.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-146">A simple rule is one that makes only a single comparison.</span></span> <span data-ttu-id="bfa9f-147">Például ha egy csoport tooa SaaS-alkalmazáshoz van hozzárendelve, állíthatja be a szabály tooadd "Értékesítési Rep." beosztással rendelkező felhasználók</span><span class="sxs-lookup"><span data-stu-id="bfa9f-147">For example, if a group is assigned tooa SaaS application, you can set up a rule tooadd users who have a job title of "Sales Rep."</span></span> <span data-ttu-id="bfa9f-148">Ez a szabály majd hozzáférést toothis SaaS alkalmazás tooall felhasználóinak, hogy a címtárban beosztás.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-148">This rule then grants access toothis SaaS application tooall users with that job title in your directory.</span></span>

<span data-ttu-id="bfa9f-149">Amikor egy felhasználó módosítása semmilyen attribútumot hello rendszer kiértékeli egy könyvtár toosee hello attribútum módosítása hello felhasználó bármely csoport kiváltották, ha az összes dinamikus csoport szabály hozzáadása vagy eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-149">When any attributes of a user change, hello system evaluates all dynamic group rules in a directory toosee if hello attribute change of hello user would trigger any group adds or removes.</span></span> <span data-ttu-id="bfa9f-150">Ha egy felhasználói csoporton szabály megfelel, hozzáadásuk után tag toothat csoportként.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-150">If a user satisfies a rule on a group, they are added as a member toothat group.</span></span> <span data-ttu-id="bfa9f-151">Azok a csoport tagjai hello szabály már nem felel meg, ha eltávolítja a tagjai a csoportból.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-151">If they no longer satisfy hello rule of a group they are a member of, they are removed as a members from that group.</span></span>

> [!NOTE]
> <span data-ttu-id="bfa9f-152">Biztonsági vagy Office 365-csoportok esetében dinamikustagság-szabály beállítására is lehetőség van.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-152">You can set up a rule for dynamic membership on security groups or Office 365 groups.</span></span> <span data-ttu-id="bfa9f-153">A beágyazott csoporttagság biztonságicsoport-alapú hozzárendelés tooapplications jelenleg nem támogatott.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-153">Nested group memberships aren't currently supported for group-based assignment tooapplications.</span></span>
>
> <span data-ttu-id="bfa9f-154">Dinamikus csoporttagság az Azure AD Premium licenc toobe rendelt</span><span class="sxs-lookup"><span data-stu-id="bfa9f-154">Dynamic memberships for groups require an Azure AD Premium license toobe assigned to</span></span>
>
> * <span data-ttu-id="bfa9f-155">hello felügyelő rendszergazdához; csoporton hello szabály</span><span class="sxs-lookup"><span data-stu-id="bfa9f-155">hello administrator who manages hello rule on a group</span></span>
> * <span data-ttu-id="bfa9f-156">Hello csoport minden tagja</span><span class="sxs-lookup"><span data-stu-id="bfa9f-156">All members of hello group</span></span>
>
>

<span data-ttu-id="bfa9f-157">**tooenable dinamikus csoporttagság**</span><span class="sxs-lookup"><span data-stu-id="bfa9f-157">**tooenable dynamic membership for a group**</span></span>

1. <span data-ttu-id="bfa9f-158">A hello [a klasszikus Azure portálon](https://manage.windowsazure.com), jelölje be **Active Directory**, majd válassza ki a szervezet hello hello könyvtár nevét.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-158">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then select hello name of hello directory for your organization.</span></span>
2. <span data-ttu-id="bfa9f-159">Jelölje be hello **csoportok** fülre, és azt szeretné, hogy tooedit nyitott hello csoport.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-159">Select hello **Groups** tab, and open hello group you want tooedit.</span></span>
3. <span data-ttu-id="bfa9f-160">Jelölje be hello **konfigurálása** lapot, és utána állítsa be **dinamikus csoporttagságok engedélyezése** túl**Igen**.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-160">Select hello **Configure** tab, and then set **Enable Dynamic Memberships** too**Yes**.</span></span>
4. <span data-ttu-id="bfa9f-161">Állítson be egy egyszerű szabályt hello csoport toocontrol dinamikus csoporttagság működését.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-161">Set up a simple single rule for hello group toocontrol how dynamic membership for this group functions.</span></span> <span data-ttu-id="bfa9f-162">Győződjön meg arról, hogy hello **felhasználók hozzáadása az adott** beállítás van kiválasztva, majd válassza ki egy felhasználói tulajdonságot (például részleg, beosztás, stb.), hello listából</span><span class="sxs-lookup"><span data-stu-id="bfa9f-162">Make sure hello **Add users where** option is selected, and then select a user property from hello list (for example, department, jobTitle, etc.),</span></span>
5. <span data-ttu-id="bfa9f-163">Ezt követően válasszon ki egy állapotot (nem egyenlő, egyenlő, nem ezzel kezdődik, ezzel kezdődik, nem tartalmazza, tartalmazza, nem egyezik, megegyezik).</span><span class="sxs-lookup"><span data-stu-id="bfa9f-163">Next, select a condition (Not Equals, Equals, Not Starts With, Starts With, Not Contains, Contains, Not Match, Match).</span></span>
6. <span data-ttu-id="bfa9f-164">Adja meg az összehasonlítási értéket hello kiválasztott felhasználói tulajdonságnak.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-164">Specify a comparison value for hello selected user property.</span></span>

<span data-ttu-id="bfa9f-165">arról, hogyan toolearn toocreate *speciális* (szabályok többszörös összehasonlítást is tartalmazó) szabályok dinamikus csoporttagság, lásd: [toocreate attribútumok használata speciális szabályok](active-directory-accessmanagement-groups-with-advanced-rules.md).</span><span class="sxs-lookup"><span data-stu-id="bfa9f-165">toolearn about how toocreate *advanced* rules (rules that can contain multiple comparisons) for dynamic group membership, see [Using attributes toocreate advanced rules](active-directory-accessmanagement-groups-with-advanced-rules.md).</span></span>

## <a name="additional-information"></a><span data-ttu-id="bfa9f-166">További információ</span><span class="sxs-lookup"><span data-stu-id="bfa9f-166">Additional information</span></span>
<span data-ttu-id="bfa9f-167">E cikkekben további információk találhatók az Azure Active Directoryval kapcsolatban.</span><span class="sxs-lookup"><span data-stu-id="bfa9f-167">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="bfa9f-168">Hozzáférés tooresources kezelése az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="bfa9f-168">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="bfa9f-169">Azure Active Directory-parancsmagok csoportbeállítások konfigurálásához</span><span class="sxs-lookup"><span data-stu-id="bfa9f-169">Azure Active Directory cmdlets for configuring group settings</span></span>](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [<span data-ttu-id="bfa9f-170">Az Azure Active Directory segítségével végzett alkalmazásfelügyeletre vonatkozó cikkek jegyzéke</span><span class="sxs-lookup"><span data-stu-id="bfa9f-170">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="bfa9f-171">Mi az az Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="bfa9f-171">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="bfa9f-172">Helyszíni identitások integrálása az Azure Active Directoryval</span><span class="sxs-lookup"><span data-stu-id="bfa9f-172">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
