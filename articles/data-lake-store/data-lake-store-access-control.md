---
title: "a hozzáférés-vezérlés a Data Lake Store aaaOverview |} Microsoft Docs"
description: "A hozzáférés-vezérlés működésének megismerése az Azure Data Lake Store szolgáltatásban"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: d16f8c09-c954-40d3-afab-c86ffa8c353d
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/29/2017
ms.author: nitinme
ms.openlocfilehash: 1cc5d578f22ef0a123a1547abebfb4795ea09139
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="access-control-in-azure-data-lake-store"></a><span data-ttu-id="0e97c-103">Az Azure Data Lake Store szolgáltatásban található hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="0e97c-103">Access control in Azure Data Lake Store</span></span>

<span data-ttu-id="0e97c-104">Azure Data Lake Store bevezet egy hozzáférés-vezérlési modell, amely a HDFS, ami viszont származik hello POSIX hozzáférés-vezérlési modell származik.</span><span class="sxs-lookup"><span data-stu-id="0e97c-104">Azure Data Lake Store implements an access control model that derives from HDFS, which in turn derives from hello POSIX access control model.</span></span> <span data-ttu-id="0e97c-105">Ez a cikk a hozzáférés-vezérlési modell hello a Data Lake Store hello alapjait foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="0e97c-105">This article summarizes hello basics of hello access control model for Data Lake Store.</span></span> <span data-ttu-id="0e97c-106">toolearn hello HDFS hozzáférés-vezérlési modell, kapcsolatos további információkért lásd: [HDFS engedélyek útmutató](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).</span><span class="sxs-lookup"><span data-stu-id="0e97c-106">toolearn more about hello HDFS access control model, see [HDFS Permissions Guide](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).</span></span>

## <a name="access-control-lists-on-files-and-folders"></a><span data-ttu-id="0e97c-107">Fájlokra és mappákra vonatkozó hozzáférés-vezérlési listák</span><span class="sxs-lookup"><span data-stu-id="0e97c-107">Access control lists on files and folders</span></span>

<span data-ttu-id="0e97c-108">Kétféle hozzáférés-vezérlési lista (ACL) létezik – a **hozzáférési ACL** és az **alapértelmezett ACL**.</span><span class="sxs-lookup"><span data-stu-id="0e97c-108">There are two kinds of access control lists (ACLs), **Access ACLs** and **Default ACLs**.</span></span>

* <span data-ttu-id="0e97c-109">**ACL-ek hozzáférés**: A hozzáférés tooan vezérlőobjektumot.</span><span class="sxs-lookup"><span data-stu-id="0e97c-109">**Access ACLs**: These control access tooan object.</span></span> <span data-ttu-id="0e97c-110">A fájlok és mappák egyaránt rendelkeznek hozzáférési ACL-lel.</span><span class="sxs-lookup"><span data-stu-id="0e97c-110">Files and folders both have Access ACLs.</span></span>

* <span data-ttu-id="0e97c-111">**Hozzáférés-vezérlési listákat alapértelmezett**: A "sablon" ACL-ek társított egy mappát, amelyekkel meghatározhatja, hogy a mappa alatti létrehozott gyermekelemeket hello hozzáférés-vezérlési listáit.</span><span class="sxs-lookup"><span data-stu-id="0e97c-111">**Default ACLs**: A "template" of ACLs associated with a folder that determine hello Access ACLs for any child items that are created under that folder.</span></span> <span data-ttu-id="0e97c-112">A fájlok nem rendelkeznek alapértelmezett ACL-ekkel.</span><span class="sxs-lookup"><span data-stu-id="0e97c-112">Files do not have Default ACLs.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

<span data-ttu-id="0e97c-114">Hozzáférés-vezérlési listák és a hozzáférés-vezérlési listákat alapértelmezett rendelkezik hello azonos struktúra.</span><span class="sxs-lookup"><span data-stu-id="0e97c-114">Both Access ACLs and Default ACLs have hello same structure.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-2.png)



> [!NOTE]
> <span data-ttu-id="0e97c-116">Változó hello alapértelmezett hozzáférés-vezérlési lista egy szülő nincs hatással Access ACL vagy alapértelmezett hozzáférés-vezérlési lista gyermek elemek, amelyek már létezőek hello.</span><span class="sxs-lookup"><span data-stu-id="0e97c-116">Changing hello Default ACL on a parent does not affect hello Access ACL or Default ACL of child items that already exist.</span></span>
>
>

## <a name="users-and-identities"></a><span data-ttu-id="0e97c-117">Felhasználók és identitások</span><span class="sxs-lookup"><span data-stu-id="0e97c-117">Users and identities</span></span>

<span data-ttu-id="0e97c-118">Minden fájl és mappa külön engedélyekkel rendelkezik az alábbi identitásokhoz:</span><span class="sxs-lookup"><span data-stu-id="0e97c-118">Every file and folder has distinct permissions for these identities:</span></span>

* <span data-ttu-id="0e97c-119">tulajdonos felhasználó hello fájl hello</span><span class="sxs-lookup"><span data-stu-id="0e97c-119">hello owning user of hello file</span></span>
* <span data-ttu-id="0e97c-120">hello tulajdonos csoport</span><span class="sxs-lookup"><span data-stu-id="0e97c-120">hello owning group</span></span>
* <span data-ttu-id="0e97c-121">Nevesített felhasználók</span><span class="sxs-lookup"><span data-stu-id="0e97c-121">Named users</span></span>
* <span data-ttu-id="0e97c-122">Nevesített csoportok</span><span class="sxs-lookup"><span data-stu-id="0e97c-122">Named groups</span></span>
* <span data-ttu-id="0e97c-123">Minden egyéb felhasználó</span><span class="sxs-lookup"><span data-stu-id="0e97c-123">All other users</span></span>

<span data-ttu-id="0e97c-124">a felhasználók és csoportok hello identitások olyan Azure Active Directory (Azure AD) identitás.</span><span class="sxs-lookup"><span data-stu-id="0e97c-124">hello identities of users and groups are Azure Active Directory (Azure AD) identities.</span></span> <span data-ttu-id="0e97c-125">Hacsak "," Data Lake Store hello környezetében műveleteket hajthatja végre, vagy: az Azure AD-felhasználó vagy az Azure Active Directory-biztonsági csoportnak.</span><span class="sxs-lookup"><span data-stu-id="0e97c-125">So unless otherwise noted, a "user," in hello context of Data Lake Store, can either mean an Azure AD user or an Azure AD security group.</span></span>

## <a name="permissions"></a><span data-ttu-id="0e97c-126">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="0e97c-126">Permissions</span></span>

<span data-ttu-id="0e97c-127">a fájlrendszer objektumon hello engedélyek **olvasási**, **írási**, és **Execute**, és azok is használható a fájlok és mappák hello a következő táblázatban ismertetett módon:</span><span class="sxs-lookup"><span data-stu-id="0e97c-127">hello permissions on a filesystem object are **Read**, **Write**, and **Execute**, and they can be used on files and folders as shown in hello following table:</span></span>

|            |    <span data-ttu-id="0e97c-128">Fájl</span><span class="sxs-lookup"><span data-stu-id="0e97c-128">File</span></span>     |   <span data-ttu-id="0e97c-129">Mappa</span><span class="sxs-lookup"><span data-stu-id="0e97c-129">Folder</span></span> |
|------------|-------------|----------|
| <span data-ttu-id="0e97c-130">**Olvasás (R)**</span><span class="sxs-lookup"><span data-stu-id="0e97c-130">**Read (R)**</span></span> | <span data-ttu-id="0e97c-131">A fájl tartalmának hello olvashatja</span><span class="sxs-lookup"><span data-stu-id="0e97c-131">Can read hello contents of a file</span></span> | <span data-ttu-id="0e97c-132">Szükséges **olvasási** és **Execute** toolist hello hello mappa tartalma</span><span class="sxs-lookup"><span data-stu-id="0e97c-132">Requires **Read** and **Execute** toolist hello contents of hello folder</span></span>|
| <span data-ttu-id="0e97c-133">**Írás (W)**</span><span class="sxs-lookup"><span data-stu-id="0e97c-133">**Write (W)**</span></span> | <span data-ttu-id="0e97c-134">Kiírhatja vagy tooa fájl hozzáfűzése</span><span class="sxs-lookup"><span data-stu-id="0e97c-134">Can write or append tooa file</span></span> | <span data-ttu-id="0e97c-135">Szükséges **írási** és **Execute** toocreate gyermek elemek mappában</span><span class="sxs-lookup"><span data-stu-id="0e97c-135">Requires **Write** and **Execute** toocreate child items in a folder</span></span> |
| <span data-ttu-id="0e97c-136">**Végrehajtás (X)**</span><span class="sxs-lookup"><span data-stu-id="0e97c-136">**Execute (X)**</span></span> | <span data-ttu-id="0e97c-137">Nem jelenti azt minden Data Lake Store hello kontextusában</span><span class="sxs-lookup"><span data-stu-id="0e97c-137">Does not mean anything in hello context of Data Lake Store</span></span> | <span data-ttu-id="0e97c-138">Szükséges tootraverse hello gyermekincidenseket mappa</span><span class="sxs-lookup"><span data-stu-id="0e97c-138">Required tootraverse hello child items of a folder</span></span> |

### <a name="short-forms-for-permissions"></a><span data-ttu-id="0e97c-139">Az engedélyek rövid alakjai</span><span class="sxs-lookup"><span data-stu-id="0e97c-139">Short forms for permissions</span></span>

<span data-ttu-id="0e97c-140">**RWX** használt tooindicate van **olvasási + írási + hajtható végre**.</span><span class="sxs-lookup"><span data-stu-id="0e97c-140">**RWX** is used tooindicate **Read + Write + Execute**.</span></span> <span data-ttu-id="0e97c-141">Több tömörített numerikus űrlap szerepel, amely **olvasási = 4**, **írási = 2**, és **Execute = 1**, hello sum, amelyek hello engedélyek jelöli.</span><span class="sxs-lookup"><span data-stu-id="0e97c-141">A more condensed numeric form exists in which **Read=4**, **Write=2**, and **Execute=1**, hello sum of which represents hello permissions.</span></span> <span data-ttu-id="0e97c-142">Az alábbiakban néhány példa látható.</span><span class="sxs-lookup"><span data-stu-id="0e97c-142">Following are some examples.</span></span>

| <span data-ttu-id="0e97c-143">Numerikus alak</span><span class="sxs-lookup"><span data-stu-id="0e97c-143">Numeric form</span></span> | <span data-ttu-id="0e97c-144">Rövid alak</span><span class="sxs-lookup"><span data-stu-id="0e97c-144">Short form</span></span> |      <span data-ttu-id="0e97c-145">Jelentés</span><span class="sxs-lookup"><span data-stu-id="0e97c-145">What it means</span></span>     |
|--------------|------------|------------------------|
| <span data-ttu-id="0e97c-146">7</span><span class="sxs-lookup"><span data-stu-id="0e97c-146">7</span></span>            | <span data-ttu-id="0e97c-147">RWX</span><span class="sxs-lookup"><span data-stu-id="0e97c-147">RWX</span></span>        | <span data-ttu-id="0e97c-148">Olvasás + Írás + Végrehajtás</span><span class="sxs-lookup"><span data-stu-id="0e97c-148">Read + Write + Execute</span></span> |
| <span data-ttu-id="0e97c-149">5</span><span class="sxs-lookup"><span data-stu-id="0e97c-149">5</span></span>            | <span data-ttu-id="0e97c-150">R-X</span><span class="sxs-lookup"><span data-stu-id="0e97c-150">R-X</span></span>        | <span data-ttu-id="0e97c-151">Olvasás + Végrehajtás</span><span class="sxs-lookup"><span data-stu-id="0e97c-151">Read + Execute</span></span>         |
| <span data-ttu-id="0e97c-152">4</span><span class="sxs-lookup"><span data-stu-id="0e97c-152">4</span></span>            | <span data-ttu-id="0e97c-153">R--</span><span class="sxs-lookup"><span data-stu-id="0e97c-153">R--</span></span>        | <span data-ttu-id="0e97c-154">Olvasás</span><span class="sxs-lookup"><span data-stu-id="0e97c-154">Read</span></span>                   |
| <span data-ttu-id="0e97c-155">0</span><span class="sxs-lookup"><span data-stu-id="0e97c-155">0</span></span>            | ---        | <span data-ttu-id="0e97c-156">Nincs engedély</span><span class="sxs-lookup"><span data-stu-id="0e97c-156">No permissions</span></span>         |


### <a name="permissions-do-not-inherit"></a><span data-ttu-id="0e97c-157">Az engedélyek nem öröklődnek</span><span class="sxs-lookup"><span data-stu-id="0e97c-157">Permissions do not inherit</span></span>

<span data-ttu-id="0e97c-158">Egy elem engedélyek hello POSIX-stílusú modellben, melynek használatával a Data Lake Store hello elemet tárolja.</span><span class="sxs-lookup"><span data-stu-id="0e97c-158">In hello POSIX-style model that's used by Data Lake Store, permissions for an item are stored on hello item itself.</span></span> <span data-ttu-id="0e97c-159">Más szóval engedélyek egy elem nem örökölhetők hello szülő elemeket.</span><span class="sxs-lookup"><span data-stu-id="0e97c-159">In other words, permissions for an item cannot be inherited from hello parent items.</span></span>

## <a name="common-scenarios-related-toopermissions"></a><span data-ttu-id="0e97c-160">Gyakori forgatókönyvek kapcsolódó toopermissions</span><span class="sxs-lookup"><span data-stu-id="0e97c-160">Common scenarios related toopermissions</span></span>

<span data-ttu-id="0e97c-161">Az alábbiakban néhány gyakori forgatókönyvek toohelp tisztában milyen engedélyekre van szükség tooperform a Data Lake Store-fiók bizonyos műveletek.</span><span class="sxs-lookup"><span data-stu-id="0e97c-161">Following are some common scenarios toohelp you understand which permissions are needed tooperform certain operations on a Data Lake Store account.</span></span>

### <a name="permissions-needed-tooread-a-file"></a><span data-ttu-id="0e97c-162">A fájl tooread szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="0e97c-162">Permissions needed tooread a file</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* <span data-ttu-id="0e97c-164">Hello fájl toobe olvasni, a hello hívó igények **olvasási** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="0e97c-164">For hello file toobe read, hello caller needs **Read** permissions.</span></span>
* <span data-ttu-id="0e97c-165">Az összes hello hello gyökérmappa-szerkezetében lévő hello fájlt tartalmazó mappához, hello hívó igények **Execute** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="0e97c-165">For all hello folders in hello folder structure that contain hello file, hello caller needs **Execute** permissions.</span></span>

### <a name="permissions-needed-tooappend-tooa-file"></a><span data-ttu-id="0e97c-166">A szükséges engedélyek tooappend tooa fájl</span><span class="sxs-lookup"><span data-stu-id="0e97c-166">Permissions needed tooappend tooa file</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* <span data-ttu-id="0e97c-168">A hello fájl toobe fűzött, hello hívó igények **írási** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="0e97c-168">For hello file toobe appended to, hello caller needs **Write** permissions.</span></span>
* <span data-ttu-id="0e97c-169">Az összes hello hello fájlt tartalmazó mappához, hello hívó igények **Execute** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="0e97c-169">For all hello folders that contain hello file, hello caller needs **Execute** permissions.</span></span>

### <a name="permissions-needed-toodelete-a-file"></a><span data-ttu-id="0e97c-170">A fájl toodelete szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="0e97c-170">Permissions needed toodelete a file</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* <span data-ttu-id="0e97c-172">Hello szülőmappa hello a hívó igények **írási + hajtható végre** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="0e97c-172">For hello parent folder, hello caller needs **Write + Execute** permissions.</span></span>
* <span data-ttu-id="0e97c-173">Az összes hello más mappákat hello fájl elérési útját, hello hívó igények **Execute** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="0e97c-173">For all hello other folders in hello file’s path, hello caller needs **Execute** permissions.</span></span>



> [!NOTE]
> <span data-ttu-id="0e97c-174">Az írási hello fájlra vonatkozó engedélyeket nincsenek azt is hello az előző két feltétel teljesülése szükséges toodelete.</span><span class="sxs-lookup"><span data-stu-id="0e97c-174">Write permissions on hello file are not required toodelete it as long as hello previous two conditions are true.</span></span>
>
>

### <a name="permissions-needed-tooenumerate-a-folder"></a><span data-ttu-id="0e97c-175">A mappa tooenumerate szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="0e97c-175">Permissions needed tooenumerate a folder</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* <span data-ttu-id="0e97c-177">Hello mappa tooenumerate, hello a hívó igények **olvasási + Execute** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="0e97c-177">For hello folder tooenumerate, hello caller needs **Read + Execute** permissions.</span></span>
* <span data-ttu-id="0e97c-178">Az összes hello elődje mappák, hello hívó igények **Execute** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="0e97c-178">For all hello ancestor folders, hello caller needs **Execute** permissions.</span></span>

## <a name="viewing-permissions-in-hello-azure-portal"></a><span data-ttu-id="0e97c-179">Engedélyek megtekintése a hello Azure-portálon</span><span class="sxs-lookup"><span data-stu-id="0e97c-179">Viewing permissions in hello Azure portal</span></span>

<span data-ttu-id="0e97c-180">A hello **adatkezelő** panelen található hello Data Lake Store-fiókot, kattintson a **hozzáférés** toosee hello ACL-ek egy fájl vagy mappa.</span><span class="sxs-lookup"><span data-stu-id="0e97c-180">From hello **Data Explorer** blade of hello Data Lake Store account, click **Access** toosee hello ACLs for a file or a folder.</span></span> <span data-ttu-id="0e97c-181">Kattintson a **hozzáférés** toosee hello hozzáférés-vezérlési listákat a hello **katalógus** hello mappája **mydatastore** fiók.</span><span class="sxs-lookup"><span data-stu-id="0e97c-181">Click **Access** toosee hello ACLs for hello **catalog** folder under hello **mydatastore** account.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

<span data-ttu-id="0e97c-183">A panel felső részében a hello hello az engedélyeket, hogy áttekintését mutatja.</span><span class="sxs-lookup"><span data-stu-id="0e97c-183">On this blade, hello top section shows an overview of hello permissions that you have.</span></span> <span data-ttu-id="0e97c-184">(Hello képernyőkép hello felhasználó, Bob.) Hogy a következő hello hozzáférési engedélyek jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="0e97c-184">(In hello screenshot, hello user is Bob.) Following that, hello access permissions are shown.</span></span> <span data-ttu-id="0e97c-185">Ezután a hello **hozzáférés** panelen kattintson a **egyszerű nézet** toosee hello egyszerűbb.</span><span class="sxs-lookup"><span data-stu-id="0e97c-185">After that, from hello **Access** blade, click **Simple View** toosee hello simpler view.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

<span data-ttu-id="0e97c-187">Kattintson a **speciális nézet** toosee hello fejlettebb nézet, ahol alapértelmezett hozzáférés-vezérlési listákat, maszk és felettes felhasználói hello elveit jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="0e97c-187">Click **Advanced View** toosee hello more advanced view, where hello concepts of Default ACLs, mask, and super-user are shown.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="hello-super-user"></a><span data-ttu-id="0e97c-189">hello felettes felhasználó</span><span class="sxs-lookup"><span data-stu-id="0e97c-189">hello super-user</span></span>

<span data-ttu-id="0e97c-190">Felettes felhasználói jogosultsága hello legtöbb felhasználók hello a Data Lake Store hello.</span><span class="sxs-lookup"><span data-stu-id="0e97c-190">A super-user has hello most rights of all hello users in hello Data Lake Store.</span></span> <span data-ttu-id="0e97c-191">A felügyelő:</span><span class="sxs-lookup"><span data-stu-id="0e97c-191">A super-user:</span></span>

* <span data-ttu-id="0e97c-192">RWX jogosult túl**összes** fájlok és mappák.</span><span class="sxs-lookup"><span data-stu-id="0e97c-192">Has RWX Permissions too**all** files and folders.</span></span>
* <span data-ttu-id="0e97c-193">Módosíthatja bármely fájl vagy mappa hello engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="0e97c-193">Can change hello permissions on any file or folder.</span></span>
* <span data-ttu-id="0e97c-194">Tulajdonos felhasználó vagy csoport bármely fájl vagy mappa a tulajdonos hello módosíthatja.</span><span class="sxs-lookup"><span data-stu-id="0e97c-194">Can change hello owning user or owning group of any file or folder.</span></span>

<span data-ttu-id="0e97c-195">A Data Lake Store-fiók számos szerepkörrel rendelkezik az Azure-ban, többek között:</span><span class="sxs-lookup"><span data-stu-id="0e97c-195">In Azure, a Data Lake Store account has several Azure roles, including:</span></span>

* <span data-ttu-id="0e97c-196">Tulajdonosok</span><span class="sxs-lookup"><span data-stu-id="0e97c-196">Owners</span></span>
* <span data-ttu-id="0e97c-197">Közreműködők</span><span class="sxs-lookup"><span data-stu-id="0e97c-197">Contributors</span></span>
* <span data-ttu-id="0e97c-198">Olvasók</span><span class="sxs-lookup"><span data-stu-id="0e97c-198">Readers</span></span>

<span data-ttu-id="0e97c-199">Mindenki hello **tulajdonosok** Data Lake Store-fiók szerepköre automatikusan egy felettes felhasználói fiók.</span><span class="sxs-lookup"><span data-stu-id="0e97c-199">Everyone in hello **Owners** role for a Data Lake Store account is automatically a super-user for that account.</span></span> <span data-ttu-id="0e97c-200">több, lásd: toolearn [szerepköralapú hozzáférés-vezérlés](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="0e97c-200">toolearn more, see [Role-based access control](../active-directory/role-based-access-control-configure.md).</span></span>
<span data-ttu-id="0e97c-201">Egy egyéni szerepkör alapú hozzáférés-vezérlést (RBAC) szerepkör, amely felettes felhasználó jogosult toocreate azt szeretné, hogy szükséges-e toohave hello alábbi engedélyek használata:</span><span class="sxs-lookup"><span data-stu-id="0e97c-201">If you want toocreate a custom role-based-access control (RBAC) role that has super-user permissions, it needs toohave hello following permissions:</span></span>
- <span data-ttu-id="0e97c-202">Microsoft.DataLakeStore/accounts/Superuser/action</span><span class="sxs-lookup"><span data-stu-id="0e97c-202">Microsoft.DataLakeStore/accounts/Superuser/action</span></span>
- <span data-ttu-id="0e97c-203">Microsoft.Authorization/roleAssignments/write</span><span class="sxs-lookup"><span data-stu-id="0e97c-203">Microsoft.Authorization/roleAssignments/write</span></span>


## <a name="hello-owning-user"></a><span data-ttu-id="0e97c-204">hello tulajdonos felhasználó</span><span class="sxs-lookup"><span data-stu-id="0e97c-204">hello owning user</span></span>

<span data-ttu-id="0e97c-205">hello hello elemet létrehozó felhasználó a rendszer automatikusan a tulajdonos felhasználó hello elem hello.</span><span class="sxs-lookup"><span data-stu-id="0e97c-205">hello user who created hello item is automatically hello owning user of hello item.</span></span> <span data-ttu-id="0e97c-206">A tulajdonos felhasználó:</span><span class="sxs-lookup"><span data-stu-id="0e97c-206">An owning user can:</span></span>

* <span data-ttu-id="0e97c-207">A fájl birtokolt hello engedélyeinek módosítása.</span><span class="sxs-lookup"><span data-stu-id="0e97c-207">Change hello permissions of a file that is owned.</span></span>
* <span data-ttu-id="0e97c-208">Tulajdonos, amelyet a tulajdonosa, csoport, mindaddig, amíg hello tulajdonos felhasználó tagja is hello célcsoport hello módosítása.</span><span class="sxs-lookup"><span data-stu-id="0e97c-208">Change hello owning group of a file that is owned, as long as hello owning user is also a member of hello target group.</span></span>

> [!NOTE]
> <span data-ttu-id="0e97c-209">tulajdonos felhasználó hello *nem* hello tulajdonos felhasználó egy másik tulajdonában lévő fájl módosítása.</span><span class="sxs-lookup"><span data-stu-id="0e97c-209">hello owning user *cannot* change hello owning user of another owned file.</span></span> <span data-ttu-id="0e97c-210">Csak felettes módosíthatók hello tulajdonos felhasználó a fájl vagy mappa.</span><span class="sxs-lookup"><span data-stu-id="0e97c-210">Only super-users can change hello owning user of a file or folder.</span></span>
>
>

## <a name="hello-owning-group"></a><span data-ttu-id="0e97c-211">hello tulajdonos csoport</span><span class="sxs-lookup"><span data-stu-id="0e97c-211">hello owning group</span></span>

<span data-ttu-id="0e97c-212">Hello POSIX hozzáférés-vezérlési listákat, az összes felhasználó csoportjához társítva egy "elsődleges."</span><span class="sxs-lookup"><span data-stu-id="0e97c-212">In hello POSIX ACLs, every user is associated with a "primary group."</span></span> <span data-ttu-id="0e97c-213">Felhasználói "alice" lehet, hogy például toohello "Pénzügy" csoport tartozik.</span><span class="sxs-lookup"><span data-stu-id="0e97c-213">For example, user "alice" might belong toohello "finance" group.</span></span> <span data-ttu-id="0e97c-214">Alice is lehet, hogy tartozik toomultiple csoportok, de egy csoport mindig az elsődleges csoportja van kijelölve.</span><span class="sxs-lookup"><span data-stu-id="0e97c-214">Alice might also belong toomultiple groups, but one group is always designated as her primary group.</span></span> <span data-ttu-id="0e97c-215">POSIX Ágnes fájlt hoz létre, amikor tulajdonos, hogy a fájl csoport hello beállítása tooher elsődleges csoport, amely ebben az esetben "Pénzügy."</span><span class="sxs-lookup"><span data-stu-id="0e97c-215">In POSIX, when Alice creates a file, hello owning group of that file is set tooher primary group, which in this case is "finance."</span></span>

<span data-ttu-id="0e97c-216">Amikor egy új fájlrendszer elem jön létre, a Data Lake Store egy érték toohello tulajdonos csoport rendeli hozzá.</span><span class="sxs-lookup"><span data-stu-id="0e97c-216">When a new filesystem item is created, Data Lake Store assigns a value toohello owning group.</span></span>

* <span data-ttu-id="0e97c-217">**1. eset**: hello gyökérmappa "/".</span><span class="sxs-lookup"><span data-stu-id="0e97c-217">**Case 1**: hello root folder "/".</span></span> <span data-ttu-id="0e97c-218">A mappa a Data Lake Store-fiók létrehozásakor jön létre.</span><span class="sxs-lookup"><span data-stu-id="0e97c-218">This folder is created when a Data Lake Store account is created.</span></span> <span data-ttu-id="0e97c-219">Ebben az esetben hello tulajdonos csoportja toohello hello fiókot létrehozó felhasználó.</span><span class="sxs-lookup"><span data-stu-id="0e97c-219">In this case, hello owning group is set toohello user who created hello account.</span></span>
* <span data-ttu-id="0e97c-220">**2. eset** (minden más esetben): egy új elem létrehozásakor hello tulajdonos csoport hello szülőmappa átmásolva.</span><span class="sxs-lookup"><span data-stu-id="0e97c-220">**Case 2** (Every other case): When a new item is created, hello owning group is copied from hello parent folder.</span></span>

<span data-ttu-id="0e97c-221">hello tulajdonos csoport módosíthatja:</span><span class="sxs-lookup"><span data-stu-id="0e97c-221">hello owning group can be changed by:</span></span>
* <span data-ttu-id="0e97c-222">Bármely felügyelő.</span><span class="sxs-lookup"><span data-stu-id="0e97c-222">Any super-users.</span></span>
* <span data-ttu-id="0e97c-223">tulajdonos felhasználó, ha hello tulajdonos felhasználó tagja is hello célcsoport hello.</span><span class="sxs-lookup"><span data-stu-id="0e97c-223">hello owning user, if hello owning user is also a member of hello target group.</span></span>

## <a name="access-check-algorithm"></a><span data-ttu-id="0e97c-224">Hozzáférés-ellenőrzési algoritmus</span><span class="sxs-lookup"><span data-stu-id="0e97c-224">Access check algorithm</span></span>

<span data-ttu-id="0e97c-225">a következő ábra hello hello hozzáférés ellenőrzése algoritmus Data Lake Store-fiók jelöli.</span><span class="sxs-lookup"><span data-stu-id="0e97c-225">hello following illustration represents hello access check algorithm for Data Lake Store accounts.</span></span>

![Data Lake Store ACL-ek algoritmusa](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="hello-mask-and-effective-permissions"></a><span data-ttu-id="0e97c-227">hello maszk és a "hatályos engedélyek"</span><span class="sxs-lookup"><span data-stu-id="0e97c-227">hello mask and "effective permissions"</span></span>

<span data-ttu-id="0e97c-228">Hello **maszk** van egy RWX érték, amely használt toolimit hozzáférés **megnevezett felhasználó**, hello **tulajdonos csoport**, és **csoportok nevű** közben hello hozzáférés ellenőrzése algoritmus végrehajtása.</span><span class="sxs-lookup"><span data-stu-id="0e97c-228">hello **mask** is an RWX value that is used toolimit access for **named users**, hello **owning group**, and **named groups** when you're performing hello access check algorithm.</span></span> <span data-ttu-id="0e97c-229">Az alábbiakban hello alapfogalmakat hello maszk.</span><span class="sxs-lookup"><span data-stu-id="0e97c-229">Here are hello key concepts for hello mask.</span></span>

* <span data-ttu-id="0e97c-230">hello maszk hoz "felhasználóra vonatkozó engedélyeket."</span><span class="sxs-lookup"><span data-stu-id="0e97c-230">hello mask creates "effective permissions."</span></span> <span data-ttu-id="0e97c-231">Ez azt jelenti, hogy módosítja a hello engedélyeket a hozzáférés-ellenőrzés hello időpontjában.</span><span class="sxs-lookup"><span data-stu-id="0e97c-231">That is, it modifies hello permissions at hello time of access check.</span></span>
* <span data-ttu-id="0e97c-232">hello maszk hello fájltulajdonos és felettes felhasználók közvetlenül szerkeszthetik.</span><span class="sxs-lookup"><span data-stu-id="0e97c-232">hello mask can be directly edited by hello file owner and any super-users.</span></span>
* <span data-ttu-id="0e97c-233">hello maszk engedélyek toocreate hello hatékony engedély eltávolítása.</span><span class="sxs-lookup"><span data-stu-id="0e97c-233">hello mask can remove permissions toocreate hello effective permission.</span></span> <span data-ttu-id="0e97c-234">hello maszk *nem* engedélyek toohello hatékony engedély hozzá.</span><span class="sxs-lookup"><span data-stu-id="0e97c-234">hello mask *cannot* add permissions toohello effective permission.</span></span>

<span data-ttu-id="0e97c-235">Lássunk néhány példát.</span><span class="sxs-lookup"><span data-stu-id="0e97c-235">Let's look at some examples.</span></span> <span data-ttu-id="0e97c-236">A következő példa hello, hello maszk túl van beállítva**RWX**, ami azt jelenti, hogy hello maszk nem távolítja el azokat az engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="0e97c-236">In hello following example, hello mask is set too**RWX**, which means that hello mask does not remove any permissions.</span></span> <span data-ttu-id="0e97c-237">a felhasználó, csoport tulajdonos, és a csoport neve nevű hello hello érvényes engedélyeit nem változnak hello hozzáférés-ellenőrzés során.</span><span class="sxs-lookup"><span data-stu-id="0e97c-237">hello effective permissions for hello named user, owning group, and named group are not altered during hello access check.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

<span data-ttu-id="0e97c-239">A következő példa hello, hello maszk túl van beállítva**R-X**.</span><span class="sxs-lookup"><span data-stu-id="0e97c-239">In hello following example, hello mask is set too**R-X**.</span></span> <span data-ttu-id="0e97c-240">Ez azt jelenti, hogy az informatikai **hello írási engedélyek kikapcsolja** a **megnevezett felhasználó**, **tulajdonos csoport**, és **nevű csoport** hozzáférés hello helyreállításkor jelölőnégyzet.</span><span class="sxs-lookup"><span data-stu-id="0e97c-240">This means that it **turns off hello Write permissions** for **named user**, **owning group**, and **named group** at hello time of access check.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

<span data-ttu-id="0e97c-242">Összehasonlításul ez hol egy fájl vagy mappa hello maszkját hello Azure-portálon jelenik meg.</span><span class="sxs-lookup"><span data-stu-id="0e97c-242">For reference, here is where hello mask for a file or folder appears in hello Azure portal.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

> [!NOTE]
> <span data-ttu-id="0e97c-244">Az új Data Lake Store-fiókba, hello hozzáférés ACL hello maszkját és alapértelmezett hozzáférés-vezérlési lista hello legfelső szintű mappa ("/") tooRWX alapértelmezés szerint.</span><span class="sxs-lookup"><span data-stu-id="0e97c-244">For a new Data Lake Store account, hello mask for hello Access ACL and Default ACL of hello root folder ("/") defaults tooRWX.</span></span>
>
>

## <a name="permissions-on-new-files-and-folders"></a><span data-ttu-id="0e97c-245">Új fájlok és mappák engedélyei</span><span class="sxs-lookup"><span data-stu-id="0e97c-245">Permissions on new files and folders</span></span>

<span data-ttu-id="0e97c-246">Amikor egy új fájl vagy mappa egy meglévő mappában jön létre, hello alapértelmezett hozzáférés-vezérlési listája hello szülőmappa határozza meg:</span><span class="sxs-lookup"><span data-stu-id="0e97c-246">When a new file or folder is created under an existing folder, hello Default ACL on hello parent folder determines:</span></span>

- <span data-ttu-id="0e97c-247">Gyermekmappa alapértelmezett ACL-jei és hozzáférési ACL-jei.</span><span class="sxs-lookup"><span data-stu-id="0e97c-247">A child folder’s Default ACL and Access ACL.</span></span>
- <span data-ttu-id="0e97c-248">Gyermekfájl hozzáférési ACL-je (a fájloknak nincs alapértelmezett ACL-je).</span><span class="sxs-lookup"><span data-stu-id="0e97c-248">A child file's Access ACL (files do not have a Default ACL).</span></span>

### <a name="hello-access-acl-of-a-child-file-or-folder"></a><span data-ttu-id="0e97c-249">hello hozzáférés ACL gyermek fájl vagy mappa</span><span class="sxs-lookup"><span data-stu-id="0e97c-249">hello Access ACL of a child file or folder</span></span>

<span data-ttu-id="0e97c-250">Gyermek-fájl vagy mappa jön létre, hello szülő alapértelmezett hozzáférés-vezérlési lista másolja a program hello hozzáférés ACL hello gyermek fájl vagy mappa.</span><span class="sxs-lookup"><span data-stu-id="0e97c-250">When a child file or folder is created, hello parent's Default ACL is copied as hello Access ACL of hello child file or folder.</span></span> <span data-ttu-id="0e97c-251">Emellett ha **más** felhasználói hello szülő alapértelmezett hozzáférés-vezérlési lista RWX jogokkal rendelkezik, a rendszer eltávolítja hello alárendelt elem hozzáférés ACL.</span><span class="sxs-lookup"><span data-stu-id="0e97c-251">Also, if **other** user has RWX permissions in hello parent's default ACL, it is removed from hello child item's Access ACL.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

<span data-ttu-id="0e97c-253">A legtöbb esetben hello előző információk is, minden van szüksége arról, hogyan határozza meg, a gyermek-konfigurációelem hozzáférés ACL tooknow.</span><span class="sxs-lookup"><span data-stu-id="0e97c-253">In most scenarios, hello previous information is all you need tooknow about how a child item’s Access ACL is determined.</span></span> <span data-ttu-id="0e97c-254">Azonban ha már ismeri POSIX rendszerekkel és a kívánt toounderstand részletes hogyan lehet teljesíteni az átalakítási, című hello [Umask tartozó szerepkör hello hozzáférés ACL, új fájlok és mappák létrehozása a](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) című cikkben.</span><span class="sxs-lookup"><span data-stu-id="0e97c-254">However, if you are familiar with POSIX systems and want toounderstand in-depth how this transformation is achieved, see hello section [Umask’s role in creating hello Access ACL for new files and folders](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) later in this article.</span></span>


### <a name="a-child-folders-default-acl"></a><span data-ttu-id="0e97c-255">Gyermekmappa alapértelmezett ACL-je</span><span class="sxs-lookup"><span data-stu-id="0e97c-255">A child folder's Default ACL</span></span>

<span data-ttu-id="0e97c-256">Amikor egy gyermekmappába létrejön egy fölérendelt mappája, hello szülőmappa alapértelmezett hozzáférés-vezérlési lista másolja keresztül, toohello alárendelt mappát alapértelmezett hozzáférés-vezérlési lista.</span><span class="sxs-lookup"><span data-stu-id="0e97c-256">When a child folder is created under a parent folder, hello parent folder's Default ACL is copied over as is toohello child folder's Default ACL.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a><span data-ttu-id="0e97c-258">Haladó témakörök a Data Lake Store-ban található ACL-ek megismeréséhez</span><span class="sxs-lookup"><span data-stu-id="0e97c-258">Advanced topics for understanding ACLs in Data Lake Store</span></span>

<span data-ttu-id="0e97c-259">Az alábbiakban néhány speciális témakörök toohelp megismerte a hozzáférés-vezérlési listák hogyan határozza meg a Data Lake Store-fájlok és mappák.</span><span class="sxs-lookup"><span data-stu-id="0e97c-259">Following are some advanced topics toohelp you understand how ACLs are determined for Data Lake Store files or folders.</span></span>

### <a name="umasks-role-in-creating-hello-access-acl-for-new-files-and-folders"></a><span data-ttu-id="0e97c-260">A hozzáférés ACL hello új fájlok és mappák létrehozása Umask tartozó szerepkör</span><span class="sxs-lookup"><span data-stu-id="0e97c-260">Umask’s role in creating hello Access ACL for new files and folders</span></span>

<span data-ttu-id="0e97c-261">A POSIX-kompatibilis rendszer hello általános fogalma a umask értéke 9 bites hello szülőmappától tootransform hello engedély által használt **tulajdonos felhasználó**, **tulajdonos csoport**, és  **más** a hello hozzáférés ACL új gyermek-fájl vagy mappa.</span><span class="sxs-lookup"><span data-stu-id="0e97c-261">In a POSIX-compliant system, hello general concept is that umask is a 9-bit value on hello parent folder that's used tootransform hello permission for **owning user**, **owning group**, and **other** on hello Access ACL of a new child file or folder.</span></span> <span data-ttu-id="0e97c-262">hello bitek száma egy umask ki mely bits tooturn hello alárendelt elem hozzáférés hozzáférés-vezérlési listában azonosításához.</span><span class="sxs-lookup"><span data-stu-id="0e97c-262">hello bits of a umask identify which bits tooturn off in hello child item’s Access ACL.</span></span> <span data-ttu-id="0e97c-263">Így a rendszer tooselectively megakadályozása hello propagálás engedélyeinek **tulajdonos felhasználó**, **tulajdonos csoport**, és **más**.</span><span class="sxs-lookup"><span data-stu-id="0e97c-263">Thus it is used tooselectively prevent hello propagation of permissions for **owning user**, **owning group**, and **other**.</span></span>

<span data-ttu-id="0e97c-264">Egy HDFS rendszerben hello umask általában a rendszergazdák által szabályozott sitewide konfigurációs beállítás.</span><span class="sxs-lookup"><span data-stu-id="0e97c-264">In an HDFS system, hello umask is typically a sitewide configuration option that is controlled by administrators.</span></span> <span data-ttu-id="0e97c-265">A Data Lake Store **egész fiókra kiterjedő umaskot** használ, amelyet nem lehet megváltoztatni.</span><span class="sxs-lookup"><span data-stu-id="0e97c-265">Data Lake Store uses an **account-wide umask** that cannot be changed.</span></span> <span data-ttu-id="0e97c-266">a következő táblázat azt mutatja be hello hello fedje fel a Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="0e97c-266">hello following table shows hello unmask for Data Lake Store.</span></span>

| <span data-ttu-id="0e97c-267">Felhasználói csoport</span><span class="sxs-lookup"><span data-stu-id="0e97c-267">User group</span></span>  | <span data-ttu-id="0e97c-268">Beállítás</span><span class="sxs-lookup"><span data-stu-id="0e97c-268">Setting</span></span> | <span data-ttu-id="0e97c-269">Hatás az új gyermekelemek hozzáférési ACL-jére</span><span class="sxs-lookup"><span data-stu-id="0e97c-269">Effect on new child item's Access ACL</span></span> |
|------------ |---------|---------------------------------------|
| <span data-ttu-id="0e97c-270">Tulajdonos felhasználó</span><span class="sxs-lookup"><span data-stu-id="0e97c-270">Owning user</span></span> | ---     | <span data-ttu-id="0e97c-271">Nincs hatás</span><span class="sxs-lookup"><span data-stu-id="0e97c-271">No effect</span></span>                             |
| <span data-ttu-id="0e97c-272">Tulajdonoscsoport</span><span class="sxs-lookup"><span data-stu-id="0e97c-272">Owning group</span></span>| ---     | <span data-ttu-id="0e97c-273">Nincs hatás</span><span class="sxs-lookup"><span data-stu-id="0e97c-273">No effect</span></span>                             |
| <span data-ttu-id="0e97c-274">Egyéb</span><span class="sxs-lookup"><span data-stu-id="0e97c-274">Other</span></span>       | <span data-ttu-id="0e97c-275">RWX</span><span class="sxs-lookup"><span data-stu-id="0e97c-275">RWX</span></span>     | <span data-ttu-id="0e97c-276">Olvasás + Írás + Végrehajtás eltávolítása</span><span class="sxs-lookup"><span data-stu-id="0e97c-276">Remove Read + Write + Execute</span></span>         |

<span data-ttu-id="0e97c-277">a következő ábra hello a umask művelet jeleníti meg.</span><span class="sxs-lookup"><span data-stu-id="0e97c-277">hello following illustration shows this umask in action.</span></span> <span data-ttu-id="0e97c-278">hello nettó hatása az tooremove **olvasási + írási + hajtható végre** a **más** felhasználó.</span><span class="sxs-lookup"><span data-stu-id="0e97c-278">hello net effect is tooremove **Read + Write + Execute** for **other** user.</span></span> <span data-ttu-id="0e97c-279">Mivel hello umask nem adta meg a bits **tulajdonos felhasználó** és **tulajdonos csoport**, ezeket az engedélyeket nem átalakításából származnak.</span><span class="sxs-lookup"><span data-stu-id="0e97c-279">Because hello umask did not specify bits for **owning user** and **owning group**, those permissions are not transformed.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-umask.png)

### <a name="hello-sticky-bit"></a><span data-ttu-id="0e97c-281">hello állandóságát bit</span><span class="sxs-lookup"><span data-stu-id="0e97c-281">hello sticky bit</span></span>

<span data-ttu-id="0e97c-282">hello állandóságát bit POSIX fájlrendszer egy speciális funkciója.</span><span class="sxs-lookup"><span data-stu-id="0e97c-282">hello sticky bit is a more advanced feature of a POSIX filesystem.</span></span> <span data-ttu-id="0e97c-283">A Data Lake Store hello környezetében nem valószínű, hogy hello állandóságát bit van szükség.</span><span class="sxs-lookup"><span data-stu-id="0e97c-283">In hello context of Data Lake Store, it is unlikely that hello sticky bit will be needed.</span></span>

<span data-ttu-id="0e97c-284">hello következő táblázatban a Data Lake Store hello állandóságát bit működése.</span><span class="sxs-lookup"><span data-stu-id="0e97c-284">hello following table shows how hello sticky bit works in Data Lake Store.</span></span>

| <span data-ttu-id="0e97c-285">Felhasználói csoport</span><span class="sxs-lookup"><span data-stu-id="0e97c-285">User group</span></span>         | <span data-ttu-id="0e97c-286">Fájl</span><span class="sxs-lookup"><span data-stu-id="0e97c-286">File</span></span>    | <span data-ttu-id="0e97c-287">Mappa</span><span class="sxs-lookup"><span data-stu-id="0e97c-287">Folder</span></span> |
|--------------------|---------|-------------------------|
| <span data-ttu-id="0e97c-288">Ragadós bit **KI**</span><span class="sxs-lookup"><span data-stu-id="0e97c-288">Sticky bit **OFF**</span></span> | <span data-ttu-id="0e97c-289">Nincs hatás</span><span class="sxs-lookup"><span data-stu-id="0e97c-289">No effect</span></span>   | <span data-ttu-id="0e97c-290">Nincs hatás.</span><span class="sxs-lookup"><span data-stu-id="0e97c-290">No effect.</span></span>           |
| <span data-ttu-id="0e97c-291">Ragadós bit **BE**</span><span class="sxs-lookup"><span data-stu-id="0e97c-291">Sticky bit **ON**</span></span>  | <span data-ttu-id="0e97c-292">Nincs hatás</span><span class="sxs-lookup"><span data-stu-id="0e97c-292">No effect</span></span>   | <span data-ttu-id="0e97c-293">Megakadályozza, hogy a kívül senkivel **felettes felhasználók** és hello **tulajdonos felhasználó** egy alárendelt elem törlését vagy, hogy az alárendelt elem átnevezése.</span><span class="sxs-lookup"><span data-stu-id="0e97c-293">Prevents anyone except **super-users** and hello **owning user** of a child item from deleting or renaming that child item.</span></span>               |

<span data-ttu-id="0e97c-294">hello állandóságát bit hello Azure-portál nem látható.</span><span class="sxs-lookup"><span data-stu-id="0e97c-294">hello sticky bit is not shown in hello Azure portal.</span></span>

## <a name="common-questions-about-acls-in-data-lake-store"></a><span data-ttu-id="0e97c-295">A Data Lake Store-ban található ACL-lel kapcsolatos gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="0e97c-295">Common questions about ACLs in Data Lake Store</span></span>

<span data-ttu-id="0e97c-296">Az alábbiakban néhány gyakori kérdést talál, amelyek a Data Lake Store-ban található ACL-ekkel kapcsolatban merülnek fel.</span><span class="sxs-lookup"><span data-stu-id="0e97c-296">Here are some questions that come up often about ACLs in Data Lake Store.</span></span>

### <a name="do-i-have-tooenable-support-for-acls"></a><span data-ttu-id="0e97c-297">ACL-ek támogatása tooenable van?</span><span class="sxs-lookup"><span data-stu-id="0e97c-297">Do I have tooenable support for ACLs?</span></span>

<span data-ttu-id="0e97c-298">Nem.</span><span class="sxs-lookup"><span data-stu-id="0e97c-298">No.</span></span> <span data-ttu-id="0e97c-299">Az ACL-alapú hozzáférés-vezérlés mindig be van kapcsolva egy Data Lake Store-fióknál.</span><span class="sxs-lookup"><span data-stu-id="0e97c-299">Access control via ACLs is always on for a Data Lake Store account.</span></span>

### <a name="which-permissions-are-required-toorecursively-delete-a-folder-and-its-contents"></a><span data-ttu-id="0e97c-300">Milyen engedélyek a szükséges toorecursively törölje a mappát és annak tartalmát?</span><span class="sxs-lookup"><span data-stu-id="0e97c-300">Which permissions are required toorecursively delete a folder and its contents?</span></span>

* <span data-ttu-id="0e97c-301">rendelkeznie kell hello szülőmappa **írási + hajtható végre** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="0e97c-301">hello parent folder must have **Write + Execute** permissions.</span></span>
* <span data-ttu-id="0e97c-302">hello a törölt mappa toobe, és minden mappában, igényel **olvasási + írási + hajtható végre** engedélyek.</span><span class="sxs-lookup"><span data-stu-id="0e97c-302">hello folder toobe deleted, and every folder within it, requires **Read + Write + Execute** permissions.</span></span>

> [!NOTE]
> <span data-ttu-id="0e97c-303">Nem kell a mappákban toodelete fájlok írási engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="0e97c-303">You do not need Write permissions toodelete files in folders.</span></span> <span data-ttu-id="0e97c-304">Emellett gyökérmappa hello "/" is **soha nem** törölhető.</span><span class="sxs-lookup"><span data-stu-id="0e97c-304">Also, hello root folder "/" can **never** be deleted.</span></span>
>
>

### <a name="who-is-hello-owner-of-a-file-or-folder"></a><span data-ttu-id="0e97c-305">Egy fájl vagy mappa hello tulajdonosa?</span><span class="sxs-lookup"><span data-stu-id="0e97c-305">Who is hello owner of a file or folder?</span></span>

<span data-ttu-id="0e97c-306">egy fájl vagy mappa hello létrehozója lesz hello tulajdonosa.</span><span class="sxs-lookup"><span data-stu-id="0e97c-306">hello creator of a file or folder becomes hello owner.</span></span>

### <a name="which-group-is-set-as-hello-owning-group-of-a-file-or-folder-at-creation"></a><span data-ttu-id="0e97c-307">Mely csoportja hello tulajdonos a fájl vagy mappa a létrehozásakor csoport?</span><span class="sxs-lookup"><span data-stu-id="0e97c-307">Which group is set as hello owning group of a file or folder at creation?</span></span>

<span data-ttu-id="0e97c-308">hello tulajdonos csoport tulajdonos csoport mely hello alatt új fájl vagy mappa létre hello szülőmappa hello átmásolva.</span><span class="sxs-lookup"><span data-stu-id="0e97c-308">hello owning group is copied from hello owning group of hello parent folder under which hello new file or folder is created.</span></span>

### <a name="i-am-hello-owning-user-of-a-file-but-i-dont-have-hello-rwx-permissions-i-need-what-do-i-do"></a><span data-ttu-id="0e97c-309">Tulajdonos felhasználó fájl hello vagyok, de nincs hello RWX engedélyeket kell.</span><span class="sxs-lookup"><span data-stu-id="0e97c-309">I am hello owning user of a file but I don’t have hello RWX permissions I need.</span></span> <span data-ttu-id="0e97c-310">Mit tegyek?</span><span class="sxs-lookup"><span data-stu-id="0e97c-310">What do I do?</span></span>

<span data-ttu-id="0e97c-311">hello tulajdonos a felhasználók módosíthatják hello fájl toogive hello engedélyeit maguk RWX engedéllyel a van szükségük.</span><span class="sxs-lookup"><span data-stu-id="0e97c-311">hello owning user can change hello permissions of hello file toogive themselves any RWX permissions they need.</span></span>

### <a name="when-i-look-at-acls-in-hello-azure-portal-i-see-user-names-but-through-apis-i-see-guids-why-is-that"></a><span data-ttu-id="0e97c-312">Hozzáférés-vezérlési listák el az Azure-portálon hello felhasználónevek látható, de API-k, segítségével látható GUID, miért van, amely?</span><span class="sxs-lookup"><span data-stu-id="0e97c-312">When I look at ACLs in hello Azure portal I see user names but through APIs, I see GUIDs, why is that?</span></span>

<span data-ttu-id="0e97c-313">Hello ACL bejegyzések GUID-EK toousers megfelelnek az Azure ad-ben tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="0e97c-313">Entries in hello ACLs are stored as GUIDs that correspond toousers in Azure AD.</span></span> <span data-ttu-id="0e97c-314">API-k hello hello GUID, adja vissza.</span><span class="sxs-lookup"><span data-stu-id="0e97c-314">hello APIs return hello GUIDs as is.</span></span> <span data-ttu-id="0e97c-315">hello Azure-portálon toomake hozzáférés-vezérlési listák könnyebb toouse megpróbál által fordítása hello GUID rövid nevekké, amikor lehetséges.</span><span class="sxs-lookup"><span data-stu-id="0e97c-315">hello Azure portal tries toomake ACLs easier toouse by translating hello GUIDs into friendly names when possible.</span></span>

### <a name="why-do-i-sometimes-see-guids-in-hello-acls-when-im-using-hello-azure-portal"></a><span data-ttu-id="0e97c-316">Miért néha látható GUID hello ACL hello Azure portál használata során?</span><span class="sxs-lookup"><span data-stu-id="0e97c-316">Why do I sometimes see GUIDs in hello ACLs when I'm using hello Azure portal?</span></span>

<span data-ttu-id="0e97c-317">A GUID látható hello felhasználó az Azure AD többé nem létezik.</span><span class="sxs-lookup"><span data-stu-id="0e97c-317">A GUID is shown when hello user doesn't exist in Azure AD anymore.</span></span> <span data-ttu-id="0e97c-318">Általában ez történik, amikor hello felhasználó elhagyta hello vállalati, vagy ha a fiók törölve lett az Azure ad-ben.</span><span class="sxs-lookup"><span data-stu-id="0e97c-318">Usually this happens when hello user has left hello company or if their account has been deleted in Azure AD.</span></span>

### <a name="does-data-lake-store-support-inheritance-of-acls"></a><span data-ttu-id="0e97c-319">Támogatja a Data Lake Store az ACL-ek öröklését?</span><span class="sxs-lookup"><span data-stu-id="0e97c-319">Does Data Lake Store support inheritance of ACLs?</span></span>

<span data-ttu-id="0e97c-320">Nem.</span><span class="sxs-lookup"><span data-stu-id="0e97c-320">No.</span></span>

### <a name="what-is-hello-difference-between-mask-and-umask"></a><span data-ttu-id="0e97c-321">Mi az, hogy a maszk és umask hello különbségének?</span><span class="sxs-lookup"><span data-stu-id="0e97c-321">What is hello difference between mask and umask?</span></span>

| <span data-ttu-id="0e97c-322">mask</span><span class="sxs-lookup"><span data-stu-id="0e97c-322">mask</span></span> | <span data-ttu-id="0e97c-323">umask</span><span class="sxs-lookup"><span data-stu-id="0e97c-323">umask</span></span>|
|------|------|
| <span data-ttu-id="0e97c-324">Hello **maszk** tulajdonság érhető el az összes fájlt és mappát.</span><span class="sxs-lookup"><span data-stu-id="0e97c-324">hello **mask** property is available on every file and folder.</span></span> | <span data-ttu-id="0e97c-325">Hello **umask** hello Data Lake Store-fiók tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="0e97c-325">hello **umask** is a property of hello Data Lake Store account.</span></span> <span data-ttu-id="0e97c-326">Így nincs csak egyetlen umask hello Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="0e97c-326">So there is only a single umask in hello Data Lake Store.</span></span>    |
| <span data-ttu-id="0e97c-327">tulajdonos felhasználó vagy a tulajdonos a fájl vagy egy felettes felhasználói csoport hello hello mask tulajdonság egy fájl vagy mappa lehet megváltoztatni.</span><span class="sxs-lookup"><span data-stu-id="0e97c-327">hello mask property on a file or folder can be altered by hello owning user or owning group of a file or a super-user.</span></span> | <span data-ttu-id="0e97c-328">bármely felhasználó, akár egy felettes felhasználó hello umask tulajdonság nem lehet módosítani.</span><span class="sxs-lookup"><span data-stu-id="0e97c-328">hello umask property cannot be modified by any user, even a super-user.</span></span> <span data-ttu-id="0e97c-329">Ez egy megváltoztathatatlan, állandó érték.</span><span class="sxs-lookup"><span data-stu-id="0e97c-329">It is an unchangeable, constant value.</span></span>|
| <span data-ttu-id="0e97c-330">hello mask tulajdonság használatos hello hozzáférés ellenőrzése algoritmus futásidejű toodetermine, hogy egy felhasználó hello jobb tooperform rendelkezik a művelet egy fájl vagy mappa.</span><span class="sxs-lookup"><span data-stu-id="0e97c-330">hello mask property is used during hello access check algorithm at runtime toodetermine whether a user has hello right tooperform on operation on a file or folder.</span></span> <span data-ttu-id="0e97c-331">hello hello maszk szerepköre toocreate "hatályos engedélyek" a hozzáférés-ellenőrzés hello időpontjában.</span><span class="sxs-lookup"><span data-stu-id="0e97c-331">hello role of hello mask is toocreate "effective permissions" at hello time of access check.</span></span> | <span data-ttu-id="0e97c-332">hello umask nem minden használt hozzáférés-ellenőrzés során.</span><span class="sxs-lookup"><span data-stu-id="0e97c-332">hello umask is not used during access check at all.</span></span> <span data-ttu-id="0e97c-333">hello umask használt toodetermine hello hozzáférés ACL az új gyermek cikkek mappa.</span><span class="sxs-lookup"><span data-stu-id="0e97c-333">hello umask is used toodetermine hello Access ACL of new child items of a folder.</span></span> |
| <span data-ttu-id="0e97c-334">hello maszk értéke egy 3 bites RWX toonamed felhasználói, a nevesített csoport és a tulajdonos felhasználó alkalmazó a hozzáférés-ellenőrzés hello időpontjában.</span><span class="sxs-lookup"><span data-stu-id="0e97c-334">hello mask is a 3-bit RWX value that applies toonamed user, named group, and owning user at hello time of access check.</span></span>| <span data-ttu-id="0e97c-335">hello umask értéke 9 bites tulajdonos felhasználó, csoport, a tulajdonos toohello alkalmazó és **más** új gyermek.</span><span class="sxs-lookup"><span data-stu-id="0e97c-335">hello umask is a 9-bit value that applies toohello owning user, owning group, and **other** of a new child.</span></span>|

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a><span data-ttu-id="0e97c-336">Hol tudhatok meg többet a POSIX hozzáférés-vezérlési modellről?</span><span class="sxs-lookup"><span data-stu-id="0e97c-336">Where can I learn more about POSIX access control model?</span></span>

* [<span data-ttu-id="0e97c-337">POSIX hozzáférés-vezérlési listák Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="0e97c-337">POSIX Access Control Lists on Linux</span></span>](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [<span data-ttu-id="0e97c-338">HDFS-engedélyek útmutatója</span><span class="sxs-lookup"><span data-stu-id="0e97c-338">HDFS permission guide</span></span>](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)

* [<span data-ttu-id="0e97c-339">POSIX – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="0e97c-339">POSIX FAQ</span></span>](http://www.opengroup.org/austin/papers/posix_faq.html)

* [<span data-ttu-id="0e97c-340">POSIX 1003.1 2008</span><span class="sxs-lookup"><span data-stu-id="0e97c-340">POSIX 1003.1 2008</span></span>](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [<span data-ttu-id="0e97c-341">POSIX 1003.1 2013</span><span class="sxs-lookup"><span data-stu-id="0e97c-341">POSIX 1003.1 2013</span></span>](http://pubs.opengroup.org/onlinepubs/9699919799.2013edition/)

* [<span data-ttu-id="0e97c-342">POSIX 1003.1 2016</span><span class="sxs-lookup"><span data-stu-id="0e97c-342">POSIX 1003.1 2016</span></span>](http://pubs.opengroup.org/onlinepubs/9699919799.2016edition/)

* [<span data-ttu-id="0e97c-343">POSIX ACL Ubuntu rendszeren</span><span class="sxs-lookup"><span data-stu-id="0e97c-343">POSIX ACL on Ubuntu</span></span>](https://help.ubuntu.com/community/FilePermissionsACLs)

* [<span data-ttu-id="0e97c-344">Hozzáférés-vezérlési listákat használó ACL Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="0e97c-344">ACL using access control lists on Linux</span></span>](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a><span data-ttu-id="0e97c-345">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="0e97c-345">See also</span></span>

* [<span data-ttu-id="0e97c-346">Az Azure Data Lake Store áttekintése</span><span class="sxs-lookup"><span data-stu-id="0e97c-346">Overview of Azure Data Lake Store</span></span>](data-lake-store-overview.md)
