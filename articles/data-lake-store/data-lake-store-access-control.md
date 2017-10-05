---
title: "A Data Lake Store szolgáltatásban található hozzáférés-vezérlés áttekintése | Microsoft Docs"
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
ms.openlocfilehash: 99fbad770290d280bdec490d988391ad276ce1ee
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="access-control-in-azure-data-lake-store"></a><span data-ttu-id="75ee6-103">Az Azure Data Lake Store szolgáltatásban található hozzáférés-vezérlés</span><span class="sxs-lookup"><span data-stu-id="75ee6-103">Access control in Azure Data Lake Store</span></span>

<span data-ttu-id="75ee6-104">Az Azure Data Lake Store egy HDFS-ből, valamint a POSIX hozzáférés-vezérlési modellből származó hozzáférés-vezérlési modellt biztosít.</span><span class="sxs-lookup"><span data-stu-id="75ee6-104">Azure Data Lake Store implements an access control model that derives from HDFS, which in turn derives from the POSIX access control model.</span></span> <span data-ttu-id="75ee6-105">Ez a cikk a Data Lake Store hozzáférés-vezérlési modelljének alapjait foglalja össze.</span><span class="sxs-lookup"><span data-stu-id="75ee6-105">This article summarizes the basics of the access control model for Data Lake Store.</span></span> <span data-ttu-id="75ee6-106">A HDFS hozzáférés-vezérlési modelljéről további információt a [HDFS Permissions Guide](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html) (HDFS-engedélyek útmutatója) oldalon talál.</span><span class="sxs-lookup"><span data-stu-id="75ee6-106">To learn more about the HDFS access control model, see [HDFS Permissions Guide](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html).</span></span>

## <a name="access-control-lists-on-files-and-folders"></a><span data-ttu-id="75ee6-107">Fájlokra és mappákra vonatkozó hozzáférés-vezérlési listák</span><span class="sxs-lookup"><span data-stu-id="75ee6-107">Access control lists on files and folders</span></span>

<span data-ttu-id="75ee6-108">Kétféle hozzáférés-vezérlési lista (ACL) létezik – a **hozzáférési ACL** és az **alapértelmezett ACL**.</span><span class="sxs-lookup"><span data-stu-id="75ee6-108">There are two kinds of access control lists (ACLs), **Access ACLs** and **Default ACLs**.</span></span>

* <span data-ttu-id="75ee6-109">**Hozzáférési ACL-ek**: Egy objektum hozzáféréseit vezérlik.</span><span class="sxs-lookup"><span data-stu-id="75ee6-109">**Access ACLs**: These control access to an object.</span></span> <span data-ttu-id="75ee6-110">A fájlok és mappák egyaránt rendelkeznek hozzáférési ACL-lel.</span><span class="sxs-lookup"><span data-stu-id="75ee6-110">Files and folders both have Access ACLs.</span></span>

* <span data-ttu-id="75ee6-111">**Alapértelmezett ACL**: A mappához társított ACL „sablon”, amely meghatározza az adott mappában létrehozott gyermekelemek hozzáférési ACL-jeit.</span><span class="sxs-lookup"><span data-stu-id="75ee6-111">**Default ACLs**: A "template" of ACLs associated with a folder that determine the Access ACLs for any child items that are created under that folder.</span></span> <span data-ttu-id="75ee6-112">A fájlok nem rendelkeznek alapértelmezett ACL-ekkel.</span><span class="sxs-lookup"><span data-stu-id="75ee6-112">Files do not have Default ACLs.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-1.png)

<span data-ttu-id="75ee6-114">A hozzáférési ACL-ek és alapértelmezett ACL-ek ugyanazzal a struktúrával rendelkeznek.</span><span class="sxs-lookup"><span data-stu-id="75ee6-114">Both Access ACLs and Default ACLs have the same structure.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-2.png)



> [!NOTE]
> <span data-ttu-id="75ee6-116">Ha megváltoztatja az alapértelmezett ACL-eket egy szülő objektumon, akkor az nem módosítja a már létező gyermekelemek hozzáférési ACL-jeit és alapértelmezett ACL-jeit.</span><span class="sxs-lookup"><span data-stu-id="75ee6-116">Changing the Default ACL on a parent does not affect the Access ACL or Default ACL of child items that already exist.</span></span>
>
>

## <a name="users-and-identities"></a><span data-ttu-id="75ee6-117">Felhasználók és identitások</span><span class="sxs-lookup"><span data-stu-id="75ee6-117">Users and identities</span></span>

<span data-ttu-id="75ee6-118">Minden fájl és mappa külön engedélyekkel rendelkezik az alábbi identitásokhoz:</span><span class="sxs-lookup"><span data-stu-id="75ee6-118">Every file and folder has distinct permissions for these identities:</span></span>

* <span data-ttu-id="75ee6-119">A fájl tulajdonos felhasználója</span><span class="sxs-lookup"><span data-stu-id="75ee6-119">The owning user of the file</span></span>
* <span data-ttu-id="75ee6-120">A tulajdonoscsoport</span><span class="sxs-lookup"><span data-stu-id="75ee6-120">The owning group</span></span>
* <span data-ttu-id="75ee6-121">Nevesített felhasználók</span><span class="sxs-lookup"><span data-stu-id="75ee6-121">Named users</span></span>
* <span data-ttu-id="75ee6-122">Nevesített csoportok</span><span class="sxs-lookup"><span data-stu-id="75ee6-122">Named groups</span></span>
* <span data-ttu-id="75ee6-123">Minden egyéb felhasználó</span><span class="sxs-lookup"><span data-stu-id="75ee6-123">All other users</span></span>

<span data-ttu-id="75ee6-124">A felhasználók és csoportok identitása Azure Active Directory- (Azure AD-) indentitás.</span><span class="sxs-lookup"><span data-stu-id="75ee6-124">The identities of users and groups are Azure Active Directory (Azure AD) identities.</span></span> <span data-ttu-id="75ee6-125">Így ha nincs másképp jelölve, a „felhasználó” – a Data Lake Store kontextusában – egy Azure AD-felhasználót vagy Azure AD biztonsági csoportot is jelenthet.</span><span class="sxs-lookup"><span data-stu-id="75ee6-125">So unless otherwise noted, a "user," in the context of Data Lake Store, can either mean an Azure AD user or an Azure AD security group.</span></span>

## <a name="permissions"></a><span data-ttu-id="75ee6-126">Engedélyek</span><span class="sxs-lookup"><span data-stu-id="75ee6-126">Permissions</span></span>

<span data-ttu-id="75ee6-127">A fájlrendszer objektumaira vonatkozó engedélyek a következők: **Olvasás**, **Írás**, és **Végrehajtás**. Ezek az engedélyek használhatók fájlok és mappák esetében a következő táblázatban látható módon:</span><span class="sxs-lookup"><span data-stu-id="75ee6-127">The permissions on a filesystem object are **Read**, **Write**, and **Execute**, and they can be used on files and folders as shown in the following table:</span></span>

|            |    <span data-ttu-id="75ee6-128">Fájl</span><span class="sxs-lookup"><span data-stu-id="75ee6-128">File</span></span>     |   <span data-ttu-id="75ee6-129">Mappa</span><span class="sxs-lookup"><span data-stu-id="75ee6-129">Folder</span></span> |
|------------|-------------|----------|
| <span data-ttu-id="75ee6-130">**Olvasás (R)**</span><span class="sxs-lookup"><span data-stu-id="75ee6-130">**Read (R)**</span></span> | <span data-ttu-id="75ee6-131">Olvashatja a fájl tartalmát</span><span class="sxs-lookup"><span data-stu-id="75ee6-131">Can read the contents of a file</span></span> | <span data-ttu-id="75ee6-132">**Olvasás** és **Végrehajtás** szükséges a mappa tartalmának listázásához</span><span class="sxs-lookup"><span data-stu-id="75ee6-132">Requires **Read** and **Execute** to list the contents of the folder</span></span>|
| <span data-ttu-id="75ee6-133">**Írás (W)**</span><span class="sxs-lookup"><span data-stu-id="75ee6-133">**Write (W)**</span></span> | <span data-ttu-id="75ee6-134">Írhatja a fájlt vagy hozzáfűzhet a fájlhoz</span><span class="sxs-lookup"><span data-stu-id="75ee6-134">Can write or append to a file</span></span> | <span data-ttu-id="75ee6-135">**Írás** és **Végrehajtás** szükséges gyermekelemek létrehozásához a mappában</span><span class="sxs-lookup"><span data-stu-id="75ee6-135">Requires **Write** and **Execute** to create child items in a folder</span></span> |
| <span data-ttu-id="75ee6-136">**Végrehajtás (X)**</span><span class="sxs-lookup"><span data-stu-id="75ee6-136">**Execute (X)**</span></span> | <span data-ttu-id="75ee6-137">Nem jelent semmit a Data Lake Store környezetében</span><span class="sxs-lookup"><span data-stu-id="75ee6-137">Does not mean anything in the context of Data Lake Store</span></span> | <span data-ttu-id="75ee6-138">Szükséges egy mappa gyermekelemeinek bejárásához</span><span class="sxs-lookup"><span data-stu-id="75ee6-138">Required to traverse the child items of a folder</span></span> |

### <a name="short-forms-for-permissions"></a><span data-ttu-id="75ee6-139">Az engedélyek rövid alakjai</span><span class="sxs-lookup"><span data-stu-id="75ee6-139">Short forms for permissions</span></span>

<span data-ttu-id="75ee6-140">**RWX** az **Olvasás + Írás + Végrehajtás** engedélyt jelöli.</span><span class="sxs-lookup"><span data-stu-id="75ee6-140">**RWX** is used to indicate **Read + Write + Execute**.</span></span> <span data-ttu-id="75ee6-141">Egy tömörebb, numerikus formátum is létezik, amelyben **Olvasás=4**, **Írás=2** és **Végrehajtás=1**, és az összegük jelöli az engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="75ee6-141">A more condensed numeric form exists in which **Read=4**, **Write=2**, and **Execute=1**, the sum of which represents the permissions.</span></span> <span data-ttu-id="75ee6-142">Az alábbiakban néhány példa látható.</span><span class="sxs-lookup"><span data-stu-id="75ee6-142">Following are some examples.</span></span>

| <span data-ttu-id="75ee6-143">Numerikus alak</span><span class="sxs-lookup"><span data-stu-id="75ee6-143">Numeric form</span></span> | <span data-ttu-id="75ee6-144">Rövid alak</span><span class="sxs-lookup"><span data-stu-id="75ee6-144">Short form</span></span> |      <span data-ttu-id="75ee6-145">Jelentés</span><span class="sxs-lookup"><span data-stu-id="75ee6-145">What it means</span></span>     |
|--------------|------------|------------------------|
| <span data-ttu-id="75ee6-146">7</span><span class="sxs-lookup"><span data-stu-id="75ee6-146">7</span></span>            | <span data-ttu-id="75ee6-147">RWX</span><span class="sxs-lookup"><span data-stu-id="75ee6-147">RWX</span></span>        | <span data-ttu-id="75ee6-148">Olvasás + Írás + Végrehajtás</span><span class="sxs-lookup"><span data-stu-id="75ee6-148">Read + Write + Execute</span></span> |
| <span data-ttu-id="75ee6-149">5</span><span class="sxs-lookup"><span data-stu-id="75ee6-149">5</span></span>            | <span data-ttu-id="75ee6-150">R-X</span><span class="sxs-lookup"><span data-stu-id="75ee6-150">R-X</span></span>        | <span data-ttu-id="75ee6-151">Olvasás + Végrehajtás</span><span class="sxs-lookup"><span data-stu-id="75ee6-151">Read + Execute</span></span>         |
| <span data-ttu-id="75ee6-152">4</span><span class="sxs-lookup"><span data-stu-id="75ee6-152">4</span></span>            | <span data-ttu-id="75ee6-153">R--</span><span class="sxs-lookup"><span data-stu-id="75ee6-153">R--</span></span>        | <span data-ttu-id="75ee6-154">Olvasás</span><span class="sxs-lookup"><span data-stu-id="75ee6-154">Read</span></span>                   |
| <span data-ttu-id="75ee6-155">0</span><span class="sxs-lookup"><span data-stu-id="75ee6-155">0</span></span>            | ---        | <span data-ttu-id="75ee6-156">Nincs engedély</span><span class="sxs-lookup"><span data-stu-id="75ee6-156">No permissions</span></span>         |


### <a name="permissions-do-not-inherit"></a><span data-ttu-id="75ee6-157">Az engedélyek nem öröklődnek</span><span class="sxs-lookup"><span data-stu-id="75ee6-157">Permissions do not inherit</span></span>

<span data-ttu-id="75ee6-158">A Data Lake Store által használt POSIX-stílusú modellben az elemhez tartozó engedélyek magában az elemben tárolódnak.</span><span class="sxs-lookup"><span data-stu-id="75ee6-158">In the POSIX-style model that's used by Data Lake Store, permissions for an item are stored on the item itself.</span></span> <span data-ttu-id="75ee6-159">Vagyis egy elem az engedélyeit nem örökölheti a szülőelemektől.</span><span class="sxs-lookup"><span data-stu-id="75ee6-159">In other words, permissions for an item cannot be inherited from the parent items.</span></span>

## <a name="common-scenarios-related-to-permissions"></a><span data-ttu-id="75ee6-160">Az engedélyekhez kapcsolódó gyakori helyzetek</span><span class="sxs-lookup"><span data-stu-id="75ee6-160">Common scenarios related to permissions</span></span>

<span data-ttu-id="75ee6-161">Az alábbiakban néhány gyakori alkalmazási helyzet szerepel, amelyekkel megismerhető, hogy bizonyos műveletek elvégzéséhez milyen engedélyek szükségesek a Data Lake Store-fiókban.</span><span class="sxs-lookup"><span data-stu-id="75ee6-161">Following are some common scenarios to help you understand which permissions are needed to perform certain operations on a Data Lake Store account.</span></span>

### <a name="permissions-needed-to-read-a-file"></a><span data-ttu-id="75ee6-162">Fájl olvasásához szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="75ee6-162">Permissions needed to read a file</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-3.png)

* <span data-ttu-id="75ee6-164">Az olvasni kívánt fájlhoz a hívónak **Olvasás** engedéllyel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="75ee6-164">For the file to be read, the caller needs **Read** permissions.</span></span>
* <span data-ttu-id="75ee6-165">A hívónak a fájlt tartalmazó mappastruktúra összes mappájához **Végrehajtás** engedéllyel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="75ee6-165">For all the folders in the folder structure that contain the file, the caller needs **Execute** permissions.</span></span>

### <a name="permissions-needed-to-append-to-a-file"></a><span data-ttu-id="75ee6-166">Fájlhoz való hozzáfűzéshez szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="75ee6-166">Permissions needed to append to a file</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-4.png)

* <span data-ttu-id="75ee6-168">A hozzáfűzni kívánt fájlhoz a hívónak **Írás** engedéllyel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="75ee6-168">For the file to be appended to, the caller needs **Write** permissions.</span></span>
* <span data-ttu-id="75ee6-169">A hívónak a fájlt tartalmazó összes mappához **Végrehajtás** engedéllyel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="75ee6-169">For all the folders that contain the file, the caller needs **Execute** permissions.</span></span>

### <a name="permissions-needed-to-delete-a-file"></a><span data-ttu-id="75ee6-170">Fájl törléséhez szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="75ee6-170">Permissions needed to delete a file</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-5.png)

* <span data-ttu-id="75ee6-172">A hívónak a szülőmappához **Olvasás + Végrehajtás** engedéllyel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="75ee6-172">For the parent folder, the caller needs **Write + Execute** permissions.</span></span>
* <span data-ttu-id="75ee6-173">A hívónak a fájl elérési útján található összes mappához **Végrehajtás** engedéllyel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="75ee6-173">For all the other folders in the file’s path, the caller needs **Execute** permissions.</span></span>



> [!NOTE]
> <span data-ttu-id="75ee6-174">A fájlra vonatkozó írási engedélyek nem szükségesek a fájl törléséhez, amíg az előbbi két feltétel teljesül.</span><span class="sxs-lookup"><span data-stu-id="75ee6-174">Write permissions on the file are not required to delete it as long as the previous two conditions are true.</span></span>
>
>

### <a name="permissions-needed-to-enumerate-a-folder"></a><span data-ttu-id="75ee6-175">Mappa felsorolásához szükséges engedélyek</span><span class="sxs-lookup"><span data-stu-id="75ee6-175">Permissions needed to enumerate a folder</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-6.png)

* <span data-ttu-id="75ee6-177">A hívónak a felsorolandó mappához **Olvasás + Végrehajtás** engedéllyel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="75ee6-177">For the folder to enumerate, the caller needs **Read + Execute** permissions.</span></span>
* <span data-ttu-id="75ee6-178">A hívónak az összes elődmappához **Végrehajtás** engedéllyel kell rendelkeznie.</span><span class="sxs-lookup"><span data-stu-id="75ee6-178">For all the ancestor folders, the caller needs **Execute** permissions.</span></span>

## <a name="viewing-permissions-in-the-azure-portal"></a><span data-ttu-id="75ee6-179">Engedélyek megtekintése az Azure Portalon</span><span class="sxs-lookup"><span data-stu-id="75ee6-179">Viewing permissions in the Azure portal</span></span>

<span data-ttu-id="75ee6-180">A Data Lake Store-fiók **Adatkezelő** panelén kattintson a **Hozzáférés** elemre az egyes fájlokhoz vagy mappákhoz tartozó ACL-ek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="75ee6-180">From the **Data Explorer** blade of the Data Lake Store account, click **Access** to see the ACLs for a file or a folder.</span></span> <span data-ttu-id="75ee6-181">Kattintson a **Hozzáférés** lehetőségre a **mydatastore** fiókban lévő **catalog** mappához tartozó ACL-ek megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="75ee6-181">Click **Access** to see the ACLs for the **catalog** folder under the **mydatastore** account.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-show-acls-1.png)

<span data-ttu-id="75ee6-183">Ezen panel felső részén áttekintés látható arról, hogy milyen engedélyekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="75ee6-183">On this blade, the top section shows an overview of the permissions that you have.</span></span> <span data-ttu-id="75ee6-184">(A képernyőképen a felhasználó Bob.) Alatta a hozzáférési engedélyek láthatók.</span><span class="sxs-lookup"><span data-stu-id="75ee6-184">(In the screenshot, the user is Bob.) Following that, the access permissions are shown.</span></span> <span data-ttu-id="75ee6-185">Ezután a **Hozzáférés** panelen kattintson az **Egyszerű nézet** lehetőségre az egyszerűbb nézet megtekintéséhez.</span><span class="sxs-lookup"><span data-stu-id="75ee6-185">After that, from the **Access** blade, click **Simple View** to see the simpler view.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-show-acls-simple-view.png)

<span data-ttu-id="75ee6-187">Kattintson az **Advanced View** (Speciális nézet) elemre a speciálisabb nézet megtekintéséhez, ahol az alapértelmezett ACL-ek, a maszk és a felügyelő koncepciója látható.</span><span class="sxs-lookup"><span data-stu-id="75ee6-187">Click **Advanced View** to see the more advanced view, where the concepts of Default ACLs, mask, and super-user are shown.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-show-acls-advance-view.png)

## <a name="the-super-user"></a><span data-ttu-id="75ee6-189">A felügyelő</span><span class="sxs-lookup"><span data-stu-id="75ee6-189">The super-user</span></span>

<span data-ttu-id="75ee6-190">A felügyelő rendelkezik a legtöbb joggal a Data Lake Store felhasználói közül.</span><span class="sxs-lookup"><span data-stu-id="75ee6-190">A super-user has the most rights of all the users in the Data Lake Store.</span></span> <span data-ttu-id="75ee6-191">A felügyelő:</span><span class="sxs-lookup"><span data-stu-id="75ee6-191">A super-user:</span></span>

* <span data-ttu-id="75ee6-192">RWX-engedéllyel (olvasás, írás és végrehajtás) rendelkezik az **összes** fájlhoz és mappához.</span><span class="sxs-lookup"><span data-stu-id="75ee6-192">Has RWX Permissions to **all** files and folders.</span></span>
* <span data-ttu-id="75ee6-193">Bármely fájl vagy mappa engedélyeit megváltoztathatja.</span><span class="sxs-lookup"><span data-stu-id="75ee6-193">Can change the permissions on any file or folder.</span></span>
* <span data-ttu-id="75ee6-194">Bármely fájl vagy mappa tulajdonosát vagy tulajdonoscsoportját megváltoztathatja.</span><span class="sxs-lookup"><span data-stu-id="75ee6-194">Can change the owning user or owning group of any file or folder.</span></span>

<span data-ttu-id="75ee6-195">A Data Lake Store-fiók számos szerepkörrel rendelkezik az Azure-ban, többek között:</span><span class="sxs-lookup"><span data-stu-id="75ee6-195">In Azure, a Data Lake Store account has several Azure roles, including:</span></span>

* <span data-ttu-id="75ee6-196">Tulajdonosok</span><span class="sxs-lookup"><span data-stu-id="75ee6-196">Owners</span></span>
* <span data-ttu-id="75ee6-197">Közreműködők</span><span class="sxs-lookup"><span data-stu-id="75ee6-197">Contributors</span></span>
* <span data-ttu-id="75ee6-198">Olvasók</span><span class="sxs-lookup"><span data-stu-id="75ee6-198">Readers</span></span>

<span data-ttu-id="75ee6-199">Egy Data Lake Store-fiók **Tulajdonosok** szerepkörében mindenki automatikusan a fiók felügyelője lesz.</span><span class="sxs-lookup"><span data-stu-id="75ee6-199">Everyone in the **Owners** role for a Data Lake Store account is automatically a super-user for that account.</span></span> <span data-ttu-id="75ee6-200">További tudnivalókért lásd a [szerepköralapú hozzáférés-vezérlést](../active-directory/role-based-access-control-configure.md) bemutató szakaszt.</span><span class="sxs-lookup"><span data-stu-id="75ee6-200">To learn more, see [Role-based access control](../active-directory/role-based-access-control-configure.md).</span></span>
<span data-ttu-id="75ee6-201">Ha létre szeretne hozni egy egyéni, szerepköralapú hozzáférés-vezérlési (RBAC) szerepkört, amely felügyelői engedélyekkel rendelkezik, akkor annak a következő engedélyekkel kell rendelkeznie:</span><span class="sxs-lookup"><span data-stu-id="75ee6-201">If you want to create a custom role-based-access control (RBAC) role that has super-user permissions, it needs to have the following permissions:</span></span>
- <span data-ttu-id="75ee6-202">Microsoft.DataLakeStore/accounts/Superuser/action</span><span class="sxs-lookup"><span data-stu-id="75ee6-202">Microsoft.DataLakeStore/accounts/Superuser/action</span></span>
- <span data-ttu-id="75ee6-203">Microsoft.Authorization/roleAssignments/write</span><span class="sxs-lookup"><span data-stu-id="75ee6-203">Microsoft.Authorization/roleAssignments/write</span></span>


## <a name="the-owning-user"></a><span data-ttu-id="75ee6-204">A tulajdonos felhasználó</span><span class="sxs-lookup"><span data-stu-id="75ee6-204">The owning user</span></span>

<span data-ttu-id="75ee6-205">Automatikusan az elem tulajdonosa lesz az a felhasználó, aki létrehozta az elemet.</span><span class="sxs-lookup"><span data-stu-id="75ee6-205">The user who created the item is automatically the owning user of the item.</span></span> <span data-ttu-id="75ee6-206">A tulajdonos felhasználó:</span><span class="sxs-lookup"><span data-stu-id="75ee6-206">An owning user can:</span></span>

* <span data-ttu-id="75ee6-207">Megváltoztathatja a tulajdonában lévő fájl engedélyeit.</span><span class="sxs-lookup"><span data-stu-id="75ee6-207">Change the permissions of a file that is owned.</span></span>
* <span data-ttu-id="75ee6-208">megváltoztathatja a tulajdonában lévő fájl tulajdonos csoportját, ha a tulajdonos felhasználó szintén tagja ennek a csoportnak.</span><span class="sxs-lookup"><span data-stu-id="75ee6-208">Change the owning group of a file that is owned, as long as the owning user is also a member of the target group.</span></span>

> [!NOTE]
> <span data-ttu-id="75ee6-209">A tulajdonos *nem* változtathatja meg a tulajdonában lévő fájl tulajdonos felhasználóját.</span><span class="sxs-lookup"><span data-stu-id="75ee6-209">The owning user *cannot* change the owning user of another owned file.</span></span> <span data-ttu-id="75ee6-210">Csak a felügyelők változtathatják meg egy fájl vagy mappa tulajdonosát vagy tulajdonoscsoportját.</span><span class="sxs-lookup"><span data-stu-id="75ee6-210">Only super-users can change the owning user of a file or folder.</span></span>
>
>

## <a name="the-owning-group"></a><span data-ttu-id="75ee6-211">A tulajdonoscsoport</span><span class="sxs-lookup"><span data-stu-id="75ee6-211">The owning group</span></span>

<span data-ttu-id="75ee6-212">A POSIX ACL-ekben minden felhasználó társítva van egy „elsődleges csoporttal”.</span><span class="sxs-lookup"><span data-stu-id="75ee6-212">In the POSIX ACLs, every user is associated with a "primary group."</span></span> <span data-ttu-id="75ee6-213">Például az „alice” nevű felhasználó a „finance” csoportba tartozhat.</span><span class="sxs-lookup"><span data-stu-id="75ee6-213">For example, user "alice" might belong to the "finance" group.</span></span> <span data-ttu-id="75ee6-214">Alice több csoporthoz is tartozhat, de egy csoport mindig ki van jelölve elsődleges csoportjaként.</span><span class="sxs-lookup"><span data-stu-id="75ee6-214">Alice might also belong to multiple groups, but one group is always designated as her primary group.</span></span> <span data-ttu-id="75ee6-215">A POSIX-ben ha Alice létrehoz egy fájlt, a fájl tulajdonoscsoportja Alice elsődleges csoportja lesz, ami ebben az esetben a „finance”.</span><span class="sxs-lookup"><span data-stu-id="75ee6-215">In POSIX, when Alice creates a file, the owning group of that file is set to her primary group, which in this case is "finance."</span></span>

<span data-ttu-id="75ee6-216">Egy új fájlrendszerbeli elem létrehozásakor a Data Lake Store értéket rendel hozzá a tulajdonoscsoporthoz.</span><span class="sxs-lookup"><span data-stu-id="75ee6-216">When a new filesystem item is created, Data Lake Store assigns a value to the owning group.</span></span>

* <span data-ttu-id="75ee6-217">**1. eset**: A gyökérmappa „/”.</span><span class="sxs-lookup"><span data-stu-id="75ee6-217">**Case 1**: The root folder "/".</span></span> <span data-ttu-id="75ee6-218">A mappa a Data Lake Store-fiók létrehozásakor jön létre.</span><span class="sxs-lookup"><span data-stu-id="75ee6-218">This folder is created when a Data Lake Store account is created.</span></span> <span data-ttu-id="75ee6-219">Ebben az esetben a tulajdonoscsoport azon felhasználó szerint lesz beállítva, aki létrehozta a fiókot.</span><span class="sxs-lookup"><span data-stu-id="75ee6-219">In this case, the owning group is set to the user who created the account.</span></span>
* <span data-ttu-id="75ee6-220">**2. eset** (minden egyéb eset): Egy új elem létrehozásakor a tulajdonoscsoport a szülőmappából másolódik át.</span><span class="sxs-lookup"><span data-stu-id="75ee6-220">**Case 2** (Every other case): When a new item is created, the owning group is copied from the parent folder.</span></span>

<span data-ttu-id="75ee6-221">A tulajdonoscsoportot megváltoztathatja:</span><span class="sxs-lookup"><span data-stu-id="75ee6-221">The owning group can be changed by:</span></span>
* <span data-ttu-id="75ee6-222">Bármely felügyelő.</span><span class="sxs-lookup"><span data-stu-id="75ee6-222">Any super-users.</span></span>
* <span data-ttu-id="75ee6-223">a tulajdonos, ha szintén tagja ennek a csoportnak.</span><span class="sxs-lookup"><span data-stu-id="75ee6-223">The owning user, if the owning user is also a member of the target group.</span></span>

## <a name="access-check-algorithm"></a><span data-ttu-id="75ee6-224">Hozzáférés-ellenőrzési algoritmus</span><span class="sxs-lookup"><span data-stu-id="75ee6-224">Access check algorithm</span></span>

<span data-ttu-id="75ee6-225">Az alábbi ábra a Data Lake Store-fiókhoz tartozó hozzáférés-ellenőrzési algoritmust mutatja be.</span><span class="sxs-lookup"><span data-stu-id="75ee6-225">The following illustration represents the access check algorithm for Data Lake Store accounts.</span></span>

![Data Lake Store ACL-ek algoritmusa](./media/data-lake-store-access-control/data-lake-store-acls-algorithm.png)


## <a name="the-mask-and-effective-permissions"></a><span data-ttu-id="75ee6-227">A maszk és „hatályos engedélyek”</span><span class="sxs-lookup"><span data-stu-id="75ee6-227">The mask and "effective permissions"</span></span>

<span data-ttu-id="75ee6-228">A **maszk** egy olyan RWX-érték, amely a **nevesített felhasználókhoz**, a **tulajdonoscsoporthoz** és a **nevesített csoportokhoz** történő hozzáférést korlátozza a hozzáférés-ellenőrzési algoritmus futtatása közben.</span><span class="sxs-lookup"><span data-stu-id="75ee6-228">The **mask** is an RWX value that is used to limit access for **named users**, the **owning group**, and **named groups** when you're performing the access check algorithm.</span></span> <span data-ttu-id="75ee6-229">Itt ismertetjük a maszk legfontosabb fogalmait.</span><span class="sxs-lookup"><span data-stu-id="75ee6-229">Here are the key concepts for the mask.</span></span>

* <span data-ttu-id="75ee6-230">A maszk „hatályos engedélyeket” hoz létre.</span><span class="sxs-lookup"><span data-stu-id="75ee6-230">The mask creates "effective permissions."</span></span> <span data-ttu-id="75ee6-231">Ez azt jelenti, hogy módosítja az engedélyeket a hozzáférés-ellenőrzés során.</span><span class="sxs-lookup"><span data-stu-id="75ee6-231">That is, it modifies the permissions at the time of access check.</span></span>
* <span data-ttu-id="75ee6-232">A maszkot közvetlenül szerkesztheti a fájl tulajdonosa vagy bármely felügyelő.</span><span class="sxs-lookup"><span data-stu-id="75ee6-232">The mask can be directly edited by the file owner and any super-users.</span></span>
* <span data-ttu-id="75ee6-233">A maszk eltávolíthat engedélyeket a hatályos engedélyek létrehozása érdekében.</span><span class="sxs-lookup"><span data-stu-id="75ee6-233">The mask can remove permissions to create the effective permission.</span></span> <span data-ttu-id="75ee6-234">A maszk *nem* adhat hozzá engedélyeket a hatályos engedélyekhez.</span><span class="sxs-lookup"><span data-stu-id="75ee6-234">The mask *cannot* add permissions to the effective permission.</span></span>

<span data-ttu-id="75ee6-235">Lássunk néhány példát.</span><span class="sxs-lookup"><span data-stu-id="75ee6-235">Let's look at some examples.</span></span> <span data-ttu-id="75ee6-236">Az alábbi példában a maszk **RWX** értékre van állítva, ami azt jelenti, hogy a maszk nem távolít el engedélyeket.</span><span class="sxs-lookup"><span data-stu-id="75ee6-236">In the following example, the mask is set to **RWX**, which means that the mask does not remove any permissions.</span></span> <span data-ttu-id="75ee6-237">A nevesített felhasználóhoz, tulajdonoscsoporthoz és nevesített csoporthoz tartozó hatályos engedélyek nem változtak meg a hozzáférés-ellenőrzés során.</span><span class="sxs-lookup"><span data-stu-id="75ee6-237">The effective permissions for the named user, owning group, and named group are not altered during the access check.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-mask-1.png)

<span data-ttu-id="75ee6-239">Az alábbi példában a maszk a következőre van állítva: **R-X**.</span><span class="sxs-lookup"><span data-stu-id="75ee6-239">In the following example, the mask is set to **R-X**.</span></span> <span data-ttu-id="75ee6-240">Ez azt jelenti, hogy a maszk **kikapcsolja az Írás engedélyt** a **nevesített felhasználó**, a **tulajdonoscsoport** és a **nevesített csoport** számára a hozzáférés-ellenőrzés idejére.</span><span class="sxs-lookup"><span data-stu-id="75ee6-240">This means that it **turns off the Write permissions** for **named user**, **owning group**, and **named group** at the time of access check.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-mask-2.png)

<span data-ttu-id="75ee6-242">Hivatkozásként itt megtalálja, hogy hol jelenik meg egy fájlhoz vagy mappához tartozó maszk az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="75ee6-242">For reference, here is where the mask for a file or folder appears in the Azure portal.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-show-acls-mask-view.png)

> [!NOTE]
> <span data-ttu-id="75ee6-244">Az új Data Lake Store-fiókoknál a gyökérmappa („/”) hozzáférési ACL-jéhez és alapértelmezett ACL-jéhez tartozó maszk alapértelmezés szerint RWX-re van állítva.</span><span class="sxs-lookup"><span data-stu-id="75ee6-244">For a new Data Lake Store account, the mask for the Access ACL and Default ACL of the root folder ("/") defaults to RWX.</span></span>
>
>

## <a name="permissions-on-new-files-and-folders"></a><span data-ttu-id="75ee6-245">Új fájlok és mappák engedélyei</span><span class="sxs-lookup"><span data-stu-id="75ee6-245">Permissions on new files and folders</span></span>

<span data-ttu-id="75ee6-246">Az új fájlok vagy mappák meglévő mappában történő létrehozásakor a szülőmappára vonatkozó alapértelmezett ACL a következőket határozza meg:</span><span class="sxs-lookup"><span data-stu-id="75ee6-246">When a new file or folder is created under an existing folder, the Default ACL on the parent folder determines:</span></span>

- <span data-ttu-id="75ee6-247">Gyermekmappa alapértelmezett ACL-jei és hozzáférési ACL-jei.</span><span class="sxs-lookup"><span data-stu-id="75ee6-247">A child folder’s Default ACL and Access ACL.</span></span>
- <span data-ttu-id="75ee6-248">Gyermekfájl hozzáférési ACL-je (a fájloknak nincs alapértelmezett ACL-je).</span><span class="sxs-lookup"><span data-stu-id="75ee6-248">A child file's Access ACL (files do not have a Default ACL).</span></span>

### <a name="the-access-acl-of-a-child-file-or-folder"></a><span data-ttu-id="75ee6-249">Gyermekfájl vagy mappa hozzáférési ACL-je</span><span class="sxs-lookup"><span data-stu-id="75ee6-249">The Access ACL of a child file or folder</span></span>

<span data-ttu-id="75ee6-250">Gyermekfájl vagy -mappa létrehozásakor a szülő alapértelmezett ACL-jét a rendszer a gyermekfájl vagy -mappa alapértelmezett hozzáférési ACL-jeként másolja.</span><span class="sxs-lookup"><span data-stu-id="75ee6-250">When a child file or folder is created, the parent's Default ACL is copied as the Access ACL of the child file or folder.</span></span> <span data-ttu-id="75ee6-251">Emellett ha egy **másik** felhasználó RWX-engedélyekkel rendelkezik a szülő alapértelmezett ACL-jében, akkor a rendszer eltávolítja azt a gyermekelem hozzáférési ACL-jéből.</span><span class="sxs-lookup"><span data-stu-id="75ee6-251">Also, if **other** user has RWX permissions in the parent's default ACL, it is removed from the child item's Access ACL.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-child-items-1.png)

<span data-ttu-id="75ee6-253">A legtöbb helyzetben a gyermekelemek hozzáférési ACL-jének meghatározásával kapcsolatos itt szereplő információ elegendő.</span><span class="sxs-lookup"><span data-stu-id="75ee6-253">In most scenarios, the previous information is all you need to know about how a child item’s Access ACL is determined.</span></span> <span data-ttu-id="75ee6-254">Ha ismeri a POSIX-rendszert és szeretné jobban megérteni az átalakítás menetét, tekintse meg [Az umask szerepe hozzáférési az új fájlok és mappák ACL-jének létrehozásakor](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) részt a cikk későbbi részében.</span><span class="sxs-lookup"><span data-stu-id="75ee6-254">However, if you are familiar with POSIX systems and want to understand in-depth how this transformation is achieved, see the section [Umask’s role in creating the Access ACL for new files and folders](#umasks-role-in-creating-the-access-acl-for-new-files-and-folders) later in this article.</span></span>


### <a name="a-child-folders-default-acl"></a><span data-ttu-id="75ee6-255">Gyermekmappa alapértelmezett ACL-je</span><span class="sxs-lookup"><span data-stu-id="75ee6-255">A child folder's Default ACL</span></span>

<span data-ttu-id="75ee6-256">Amikor egy szülőmappán belül gyermekmappát hoz létre, a szülőmappa alapértelmezett ACL-jét a rendszer változtatás nélkül átmásolja a gyermekmappa alapértelmezett ACL-jébe.</span><span class="sxs-lookup"><span data-stu-id="75ee6-256">When a child folder is created under a parent folder, the parent folder's Default ACL is copied over as is to the child folder's Default ACL.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-child-items-2.png)

## <a name="advanced-topics-for-understanding-acls-in-data-lake-store"></a><span data-ttu-id="75ee6-258">Haladó témakörök a Data Lake Store-ban található ACL-ek megismeréséhez</span><span class="sxs-lookup"><span data-stu-id="75ee6-258">Advanced topics for understanding ACLs in Data Lake Store</span></span>

<span data-ttu-id="75ee6-259">A következőben néhány speciális témakör található, amely segíti a Data Lake Store fájljaihoz és mappáihoz tartozó ACL-ek meghatározásának megismerését.</span><span class="sxs-lookup"><span data-stu-id="75ee6-259">Following are some advanced topics to help you understand how ACLs are determined for Data Lake Store files or folders.</span></span>

### <a name="umasks-role-in-creating-the-access-acl-for-new-files-and-folders"></a><span data-ttu-id="75ee6-260">Az umask szerepe hozzáférési az új fájlok és mappák ACL-jének létrehozásakor</span><span class="sxs-lookup"><span data-stu-id="75ee6-260">Umask’s role in creating the Access ACL for new files and folders</span></span>

<span data-ttu-id="75ee6-261">A POSIX-kompatibilis rendszerekben az általános elképzelés az, hogy az umask egy 9 bites érték a szülőmappára vonatkozóan, amely a **tulajdonos**, a **tulajdonoscsoport** és az **egyéb** felhasználók engedélyeinek átalakítására használatos az új gyermekfájlok vagy mappák hozzáférési ACL-jén.</span><span class="sxs-lookup"><span data-stu-id="75ee6-261">In a POSIX-compliant system, the general concept is that umask is a 9-bit value on the parent folder that's used to transform the permission for **owning user**, **owning group**, and **other** on the Access ACL of a new child file or folder.</span></span> <span data-ttu-id="75ee6-262">Az umask bitjei határozzák meg, hogy mely bitek legyenek kikapcsolva a gyermekelem hozzáférési ACL-jében.</span><span class="sxs-lookup"><span data-stu-id="75ee6-262">The bits of a umask identify which bits to turn off in the child item’s Access ACL.</span></span> <span data-ttu-id="75ee6-263">Ennek megfelelően arra használható, hogy a **tulajdonos**, a **tulajdonoscsoport** és az **egyéb** felhasználók engedélyének terjesztését külön-külön akadályozza meg.</span><span class="sxs-lookup"><span data-stu-id="75ee6-263">Thus it is used to selectively prevent the propagation of permissions for **owning user**, **owning group**, and **other**.</span></span>

<span data-ttu-id="75ee6-264">A HDFS-rendszerekben, az umask általában a teljes helyre vonatkozó konfigurációs beállítás, amelyet a rendszergazdák kezelnek.</span><span class="sxs-lookup"><span data-stu-id="75ee6-264">In an HDFS system, the umask is typically a sitewide configuration option that is controlled by administrators.</span></span> <span data-ttu-id="75ee6-265">A Data Lake Store **egész fiókra kiterjedő umaskot** használ, amelyet nem lehet megváltoztatni.</span><span class="sxs-lookup"><span data-stu-id="75ee6-265">Data Lake Store uses an **account-wide umask** that cannot be changed.</span></span> <span data-ttu-id="75ee6-266">Az alábbi táblázatban a Data Lake Store umask beállítása szerepel.</span><span class="sxs-lookup"><span data-stu-id="75ee6-266">The following table shows the unmask for Data Lake Store.</span></span>

| <span data-ttu-id="75ee6-267">Felhasználói csoport</span><span class="sxs-lookup"><span data-stu-id="75ee6-267">User group</span></span>  | <span data-ttu-id="75ee6-268">Beállítás</span><span class="sxs-lookup"><span data-stu-id="75ee6-268">Setting</span></span> | <span data-ttu-id="75ee6-269">Hatás az új gyermekelemek hozzáférési ACL-jére</span><span class="sxs-lookup"><span data-stu-id="75ee6-269">Effect on new child item's Access ACL</span></span> |
|------------ |---------|---------------------------------------|
| <span data-ttu-id="75ee6-270">Tulajdonos felhasználó</span><span class="sxs-lookup"><span data-stu-id="75ee6-270">Owning user</span></span> | ---     | <span data-ttu-id="75ee6-271">Nincs hatás</span><span class="sxs-lookup"><span data-stu-id="75ee6-271">No effect</span></span>                             |
| <span data-ttu-id="75ee6-272">Tulajdonoscsoport</span><span class="sxs-lookup"><span data-stu-id="75ee6-272">Owning group</span></span>| ---     | <span data-ttu-id="75ee6-273">Nincs hatás</span><span class="sxs-lookup"><span data-stu-id="75ee6-273">No effect</span></span>                             |
| <span data-ttu-id="75ee6-274">Egyéb</span><span class="sxs-lookup"><span data-stu-id="75ee6-274">Other</span></span>       | <span data-ttu-id="75ee6-275">RWX</span><span class="sxs-lookup"><span data-stu-id="75ee6-275">RWX</span></span>     | <span data-ttu-id="75ee6-276">Olvasás + Írás + Végrehajtás eltávolítása</span><span class="sxs-lookup"><span data-stu-id="75ee6-276">Remove Read + Write + Execute</span></span>         |

<span data-ttu-id="75ee6-277">Az alábbi ábrán az umask működése látható.</span><span class="sxs-lookup"><span data-stu-id="75ee6-277">The following illustration shows this umask in action.</span></span> <span data-ttu-id="75ee6-278">Az eredő hatás az **Olvasás + Írás + Végrehajtás** eltávolítása az **egyéb** felhasználóról.</span><span class="sxs-lookup"><span data-stu-id="75ee6-278">The net effect is to remove **Read + Write + Execute** for **other** user.</span></span> <span data-ttu-id="75ee6-279">Mivel az umask nem határoz meg biteket a **tulajdonos felhasználóhoz** és a **tulajdonoscsoporthoz**, a rendszer ezeket az engedélyeket nem alakítja át.</span><span class="sxs-lookup"><span data-stu-id="75ee6-279">Because the umask did not specify bits for **owning user** and **owning group**, those permissions are not transformed.</span></span>

![Data Lake Store ACL-ek](./media/data-lake-store-access-control/data-lake-store-acls-umask.png)

### <a name="the-sticky-bit"></a><span data-ttu-id="75ee6-281">Ragadós bit</span><span class="sxs-lookup"><span data-stu-id="75ee6-281">The sticky bit</span></span>

<span data-ttu-id="75ee6-282">A ragadós (sticky) bit a POSIX-fájlrendszer egy speciális funkciója.</span><span class="sxs-lookup"><span data-stu-id="75ee6-282">The sticky bit is a more advanced feature of a POSIX filesystem.</span></span> <span data-ttu-id="75ee6-283">A Data Lake Store-ral összefüggésben nem valószínű, hogy szükség lesz ragadós bitre.</span><span class="sxs-lookup"><span data-stu-id="75ee6-283">In the context of Data Lake Store, it is unlikely that the sticky bit will be needed.</span></span>

<span data-ttu-id="75ee6-284">A következő táblázat a ragadós bitek működését mutatja a Data Lake Store-ban.</span><span class="sxs-lookup"><span data-stu-id="75ee6-284">The following table shows how the sticky bit works in Data Lake Store.</span></span>

| <span data-ttu-id="75ee6-285">Felhasználói csoport</span><span class="sxs-lookup"><span data-stu-id="75ee6-285">User group</span></span>         | <span data-ttu-id="75ee6-286">Fájl</span><span class="sxs-lookup"><span data-stu-id="75ee6-286">File</span></span>    | <span data-ttu-id="75ee6-287">Mappa</span><span class="sxs-lookup"><span data-stu-id="75ee6-287">Folder</span></span> |
|--------------------|---------|-------------------------|
| <span data-ttu-id="75ee6-288">Ragadós bit **KI**</span><span class="sxs-lookup"><span data-stu-id="75ee6-288">Sticky bit **OFF**</span></span> | <span data-ttu-id="75ee6-289">Nincs hatás</span><span class="sxs-lookup"><span data-stu-id="75ee6-289">No effect</span></span>   | <span data-ttu-id="75ee6-290">Nincs hatás.</span><span class="sxs-lookup"><span data-stu-id="75ee6-290">No effect.</span></span>           |
| <span data-ttu-id="75ee6-291">Ragadós bit **BE**</span><span class="sxs-lookup"><span data-stu-id="75ee6-291">Sticky bit **ON**</span></span>  | <span data-ttu-id="75ee6-292">Nincs hatás</span><span class="sxs-lookup"><span data-stu-id="75ee6-292">No effect</span></span>   | <span data-ttu-id="75ee6-293">A gyermekelem **felügyelőjét** és **tulajdonosát** kivéve mindenkit meggátol a gyermekelem törlésében vagy átnevezésében.</span><span class="sxs-lookup"><span data-stu-id="75ee6-293">Prevents anyone except **super-users** and the **owning user** of a child item from deleting or renaming that child item.</span></span>               |

<span data-ttu-id="75ee6-294">A ragadós bit nem látható az Azure Portalon.</span><span class="sxs-lookup"><span data-stu-id="75ee6-294">The sticky bit is not shown in the Azure portal.</span></span>

## <a name="common-questions-about-acls-in-data-lake-store"></a><span data-ttu-id="75ee6-295">A Data Lake Store-ban található ACL-lel kapcsolatos gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="75ee6-295">Common questions about ACLs in Data Lake Store</span></span>

<span data-ttu-id="75ee6-296">Az alábbiakban néhány gyakori kérdést talál, amelyek a Data Lake Store-ban található ACL-ekkel kapcsolatban merülnek fel.</span><span class="sxs-lookup"><span data-stu-id="75ee6-296">Here are some questions that come up often about ACLs in Data Lake Store.</span></span>

### <a name="do-i-have-to-enable-support-for-acls"></a><span data-ttu-id="75ee6-297">Engedélyeznem kell az ACL támogatását?</span><span class="sxs-lookup"><span data-stu-id="75ee6-297">Do I have to enable support for ACLs?</span></span>

<span data-ttu-id="75ee6-298">Nem.</span><span class="sxs-lookup"><span data-stu-id="75ee6-298">No.</span></span> <span data-ttu-id="75ee6-299">Az ACL-alapú hozzáférés-vezérlés mindig be van kapcsolva egy Data Lake Store-fióknál.</span><span class="sxs-lookup"><span data-stu-id="75ee6-299">Access control via ACLs is always on for a Data Lake Store account.</span></span>

### <a name="which-permissions-are-required-to-recursively-delete-a-folder-and-its-contents"></a><span data-ttu-id="75ee6-300">Milyen engedélyek szükségesek egy mappa és tartalma rekurzív törléséhez?</span><span class="sxs-lookup"><span data-stu-id="75ee6-300">Which permissions are required to recursively delete a folder and its contents?</span></span>

* <span data-ttu-id="75ee6-301">A szülőmappának rendelkeznie kell **Írás + Végrehajtás** engedélyekkel.</span><span class="sxs-lookup"><span data-stu-id="75ee6-301">The parent folder must have **Write + Execute** permissions.</span></span>
* <span data-ttu-id="75ee6-302">A törölni kívánt mappához és a benne található összes mappához **Olvasás + Írás + Végrehajtás** engedélyre van szükség.</span><span class="sxs-lookup"><span data-stu-id="75ee6-302">The folder to be deleted, and every folder within it, requires **Read + Write + Execute** permissions.</span></span>

> [!NOTE]
> <span data-ttu-id="75ee6-303">A mappákban található fájlok törléséhez nem szükséges írási engedély.</span><span class="sxs-lookup"><span data-stu-id="75ee6-303">You do not need Write permissions to delete files in folders.</span></span> <span data-ttu-id="75ee6-304">Emellett a „/” gyökérmappa **soha** nem törölhető.</span><span class="sxs-lookup"><span data-stu-id="75ee6-304">Also, the root folder "/" can **never** be deleted.</span></span>
>
>

### <a name="who-is-the-owner-of-a-file-or-folder"></a><span data-ttu-id="75ee6-305">Ki a fájl vagy mappa tulajdonosa?</span><span class="sxs-lookup"><span data-stu-id="75ee6-305">Who is the owner of a file or folder?</span></span>

<span data-ttu-id="75ee6-306">A fájl vagy mappa létrehozója lesz a tulajdonos.</span><span class="sxs-lookup"><span data-stu-id="75ee6-306">The creator of a file or folder becomes the owner.</span></span>

### <a name="which-group-is-set-as-the-owning-group-of-a-file-or-folder-at-creation"></a><span data-ttu-id="75ee6-307">Melyik csoport lett a fájl vagy mappa tulajdonoscsoportjaként beállítva a létrehozáskor?</span><span class="sxs-lookup"><span data-stu-id="75ee6-307">Which group is set as the owning group of a file or folder at creation?</span></span>

<span data-ttu-id="75ee6-308">A tulajdonoscsoport annak a szülőmappának a tulajdonoscsoportjából lesz kimásolva, amelyben az új fájlt vagy mappát létrehozzák.</span><span class="sxs-lookup"><span data-stu-id="75ee6-308">The owning group is copied from the owning group of the parent folder under which the new file or folder is created.</span></span>

### <a name="i-am-the-owning-user-of-a-file-but-i-dont-have-the-rwx-permissions-i-need-what-do-i-do"></a><span data-ttu-id="75ee6-309">Én vagyok egy fájl tulajdonosa, de nem rendelkezem a szükséges RWX-engedéllyel.</span><span class="sxs-lookup"><span data-stu-id="75ee6-309">I am the owning user of a file but I don’t have the RWX permissions I need.</span></span> <span data-ttu-id="75ee6-310">Mit tegyek?</span><span class="sxs-lookup"><span data-stu-id="75ee6-310">What do I do?</span></span>

<span data-ttu-id="75ee6-311">A tulajdonos módosíthatja a fájlhoz tartozó engedélyeket, így bármilyen szükséges RWX-engedélyt megadhat saját magának.</span><span class="sxs-lookup"><span data-stu-id="75ee6-311">The owning user can change the permissions of the file to give themselves any RWX permissions they need.</span></span>

### <a name="when-i-look-at-acls-in-the-azure-portal-i-see-user-names-but-through-apis-i-see-guids-why-is-that"></a><span data-ttu-id="75ee6-312">Amikor megtekintem az ACL-eket az Azure Portalon, felhasználóneveket látok, az API-kon keresztül azonban GUID azonosítókat. Ez miért van?</span><span class="sxs-lookup"><span data-stu-id="75ee6-312">When I look at ACLs in the Azure portal I see user names but through APIs, I see GUIDs, why is that?</span></span>

<span data-ttu-id="75ee6-313">Az ACL-ek bejegyzéseinek tárolása olyan GUID azonosítókként történik, amelyek az Azure AD-ban található felhasználóknak felelnek meg.</span><span class="sxs-lookup"><span data-stu-id="75ee6-313">Entries in the ACLs are stored as GUIDs that correspond to users in Azure AD.</span></span> <span data-ttu-id="75ee6-314">Az API-k az aktuális állapotukban adják vissza a GUID azonosítókat.</span><span class="sxs-lookup"><span data-stu-id="75ee6-314">The APIs return the GUIDs as is.</span></span> <span data-ttu-id="75ee6-315">Az Azure Portal megpróbálja könnyebbé tenni az ACL-ek használatát a GUID azonosítóknak rövid nevekre történő lefordításával, amikor csak lehetséges.</span><span class="sxs-lookup"><span data-stu-id="75ee6-315">The Azure portal tries to make ACLs easier to use by translating the GUIDs into friendly names when possible.</span></span>

### <a name="why-do-i-sometimes-see-guids-in-the-acls-when-im-using-the-azure-portal"></a><span data-ttu-id="75ee6-316">Miért látok néha GUID azonosítókat az ACL-eken, amikor az Azure Portalt használom?</span><span class="sxs-lookup"><span data-stu-id="75ee6-316">Why do I sometimes see GUIDs in the ACLs when I'm using the Azure portal?</span></span>

<span data-ttu-id="75ee6-317">Ha a felhasználó már nem létezik az Azure AD-ben, egy GUID lesz látható.</span><span class="sxs-lookup"><span data-stu-id="75ee6-317">A GUID is shown when the user doesn't exist in Azure AD anymore.</span></span> <span data-ttu-id="75ee6-318">Ez általában akkor történik, ha a felhasználó elhagyta a vállalatot, vagy törölve lett a fiókja az Azure AD-ben.</span><span class="sxs-lookup"><span data-stu-id="75ee6-318">Usually this happens when the user has left the company or if their account has been deleted in Azure AD.</span></span>

### <a name="does-data-lake-store-support-inheritance-of-acls"></a><span data-ttu-id="75ee6-319">Támogatja a Data Lake Store az ACL-ek öröklését?</span><span class="sxs-lookup"><span data-stu-id="75ee6-319">Does Data Lake Store support inheritance of ACLs?</span></span>

<span data-ttu-id="75ee6-320">Nem.</span><span class="sxs-lookup"><span data-stu-id="75ee6-320">No.</span></span>

### <a name="what-is-the-difference-between-mask-and-umask"></a><span data-ttu-id="75ee6-321">Mi a különbség a mask és az umask között?</span><span class="sxs-lookup"><span data-stu-id="75ee6-321">What is the difference between mask and umask?</span></span>

| <span data-ttu-id="75ee6-322">mask</span><span class="sxs-lookup"><span data-stu-id="75ee6-322">mask</span></span> | <span data-ttu-id="75ee6-323">umask</span><span class="sxs-lookup"><span data-stu-id="75ee6-323">umask</span></span>|
|------|------|
| <span data-ttu-id="75ee6-324">A **mask** tulajdonság minden fájl és mappa esetében elérhető.</span><span class="sxs-lookup"><span data-stu-id="75ee6-324">The **mask** property is available on every file and folder.</span></span> | <span data-ttu-id="75ee6-325">Az **umask** a Data Lake Store-fiók egyik tulajdonsága.</span><span class="sxs-lookup"><span data-stu-id="75ee6-325">The **umask** is a property of the Data Lake Store account.</span></span> <span data-ttu-id="75ee6-326">Tehát a Data Lake Store csak egyetlen umask tulajdonságot tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="75ee6-326">So there is only a single umask in the Data Lake Store.</span></span>    |
| <span data-ttu-id="75ee6-327">Egy fájl vagy mappa mask tulajdonságát a fájl tulajdonosa vagy tulajdonoscsoportja, illetve egy felügyelő változtathatja meg.</span><span class="sxs-lookup"><span data-stu-id="75ee6-327">The mask property on a file or folder can be altered by the owning user or owning group of a file or a super-user.</span></span> | <span data-ttu-id="75ee6-328">Az umask tulajdonságot semmilyen felhasználó nem módosíthatja, még a felügyelők sem.</span><span class="sxs-lookup"><span data-stu-id="75ee6-328">The umask property cannot be modified by any user, even a super-user.</span></span> <span data-ttu-id="75ee6-329">Ez egy megváltoztathatatlan, állandó érték.</span><span class="sxs-lookup"><span data-stu-id="75ee6-329">It is an unchangeable, constant value.</span></span>|
| <span data-ttu-id="75ee6-330">A mask tulajdonság a hozzáférés-ellenőrzési algoritmus futásakor használható annak megállapítására, hogy egy felhasználó jogosult-e műveletek elvégzésére egy fájlon vagy mappán.</span><span class="sxs-lookup"><span data-stu-id="75ee6-330">The mask property is used during the access check algorithm at runtime to determine whether a user has the right to perform on operation on a file or folder.</span></span> <span data-ttu-id="75ee6-331">A mask szerepe a „hatályos engedélyek létrehozása” a hozzáférés-ellenőrzés során.</span><span class="sxs-lookup"><span data-stu-id="75ee6-331">The role of the mask is to create "effective permissions" at the time of access check.</span></span> | <span data-ttu-id="75ee6-332">Az umask a hozzáférés-ellenőrzés során egyáltalán nincs használatban.</span><span class="sxs-lookup"><span data-stu-id="75ee6-332">The umask is not used during access check at all.</span></span> <span data-ttu-id="75ee6-333">Az umask egy mappa új gyermekelemei hozzáférési ACL-jeinek meghatározására használható.</span><span class="sxs-lookup"><span data-stu-id="75ee6-333">The umask is used to determine the Access ACL of new child items of a folder.</span></span> |
| <span data-ttu-id="75ee6-334">A mask egy 3 bites RWX-érték, amely a nevesített felhasználóra, nevesített csoportra és a tulajdonosra lesz alkalmazva a hozzáférés-ellenőrzés időpontjában.</span><span class="sxs-lookup"><span data-stu-id="75ee6-334">The mask is a 3-bit RWX value that applies to named user, named group, and owning user at the time of access check.</span></span>| <span data-ttu-id="75ee6-335">Az umask egy 9 bites érték, amely egy új gyermek tulajdonosára, tulajdonoscsoportjára vagy **egyéb** jellemzőjére lesz alkalmazva.</span><span class="sxs-lookup"><span data-stu-id="75ee6-335">The umask is a 9-bit value that applies to the owning user, owning group, and **other** of a new child.</span></span>|

### <a name="where-can-i-learn-more-about-posix-access-control-model"></a><span data-ttu-id="75ee6-336">Hol tudhatok meg többet a POSIX hozzáférés-vezérlési modellről?</span><span class="sxs-lookup"><span data-stu-id="75ee6-336">Where can I learn more about POSIX access control model?</span></span>

* [<span data-ttu-id="75ee6-337">POSIX hozzáférés-vezérlési listák Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="75ee6-337">POSIX Access Control Lists on Linux</span></span>](http://www.vanemery.com/Linux/ACL/POSIX_ACL_on_Linux.html)

* [<span data-ttu-id="75ee6-338">HDFS-engedélyek útmutatója</span><span class="sxs-lookup"><span data-stu-id="75ee6-338">HDFS permission guide</span></span>](http://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsPermissionsGuide.html)

* [<span data-ttu-id="75ee6-339">POSIX – gyakori kérdések</span><span class="sxs-lookup"><span data-stu-id="75ee6-339">POSIX FAQ</span></span>](http://www.opengroup.org/austin/papers/posix_faq.html)

* [<span data-ttu-id="75ee6-340">POSIX 1003.1 2008</span><span class="sxs-lookup"><span data-stu-id="75ee6-340">POSIX 1003.1 2008</span></span>](http://standards.ieee.org/findstds/standard/1003.1-2008.html)

* [<span data-ttu-id="75ee6-341">POSIX 1003.1 2013</span><span class="sxs-lookup"><span data-stu-id="75ee6-341">POSIX 1003.1 2013</span></span>](http://pubs.opengroup.org/onlinepubs/9699919799.2013edition/)

* [<span data-ttu-id="75ee6-342">POSIX 1003.1 2016</span><span class="sxs-lookup"><span data-stu-id="75ee6-342">POSIX 1003.1 2016</span></span>](http://pubs.opengroup.org/onlinepubs/9699919799.2016edition/)

* [<span data-ttu-id="75ee6-343">POSIX ACL Ubuntu rendszeren</span><span class="sxs-lookup"><span data-stu-id="75ee6-343">POSIX ACL on Ubuntu</span></span>](https://help.ubuntu.com/community/FilePermissionsACLs)

* [<span data-ttu-id="75ee6-344">Hozzáférés-vezérlési listákat használó ACL Linux rendszeren</span><span class="sxs-lookup"><span data-stu-id="75ee6-344">ACL using access control lists on Linux</span></span>](http://bencane.com/2012/05/27/acl-using-access-control-lists-on-linux/)

## <a name="see-also"></a><span data-ttu-id="75ee6-345">Lásd még:</span><span class="sxs-lookup"><span data-stu-id="75ee6-345">See also</span></span>

* [<span data-ttu-id="75ee6-346">Az Azure Data Lake Store áttekintése</span><span class="sxs-lookup"><span data-stu-id="75ee6-346">Overview of Azure Data Lake Store</span></span>](data-lake-store-overview.md)
