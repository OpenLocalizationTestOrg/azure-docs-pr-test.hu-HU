---
title: "Tartományhoz csatlakozó HDInsight-fürtök - Azure kezelése |} Microsoft Docs"
description: "A tartományhoz a HDInsight-fürtök kezelése"
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: 
ms.assetid: 6ebc4d2f-2f6a-4e1e-ab6d-af4db6b4c87c
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/25/2016
ms.author: saurinsh
ms.openlocfilehash: 9b56ce6cc5bdd3b8d48d047751e978cad08598e1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 07/11/2017
---
# <a name="manage-domain-joined-hdinsight-clusters-preview"></a><span data-ttu-id="c4fc6-103">Tartományhoz csatlakozó HDInsight-fürtök (előzetes verzió) kezelése</span><span class="sxs-lookup"><span data-stu-id="c4fc6-103">Manage Domain-joined HDInsight clusters (Preview)</span></span>
<span data-ttu-id="c4fc6-104">Ismerje meg, a felhasználók és a szerepköröket, a tartományhoz, és a tartományhoz a HDInsight-fürtök kezelése.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-104">Learn the users and the roles in Domain-joined HDInsight, and how to manage domain-joined HDInsight clusters.</span></span>

## <a name="users-of-domain-joined-hdinsight-clusters"></a><span data-ttu-id="c4fc6-105">A tartományhoz a HDInsight-fürtök felhasználók</span><span class="sxs-lookup"><span data-stu-id="c4fc6-105">Users of Domain-joined HDInsight clusters</span></span>
<span data-ttu-id="c4fc6-106">Egy HDInsight-fürthöz nem csatlakozó tartomány-van két olyan felhasználói fiókot, amely a fürt létrehozása során jönnek létre:</span><span class="sxs-lookup"><span data-stu-id="c4fc6-106">An HDInsight cluster that is not domain-joined has two user accounts that are created during the cluster creation:</span></span>

* <span data-ttu-id="c4fc6-107">**Ambari admin**: Ez a fiók akkor is *Hadoop felhasználói* vagy *HTTP felhasználói*.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-107">**Ambari admin**: This account is also known as *Hadoop user* or *HTTP user*.</span></span> <span data-ttu-id="c4fc6-108">Ez a fiók használható jelentkezhet be az Ambari https://&lt;clustername >. azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-108">This account can be used to log on to Ambari at https://&lt;clustername>.azurehdinsight.net.</span></span> <span data-ttu-id="c4fc6-109">Is használható lekérdezéseket futtathat az Ambari nézetek, külső eszközök (azaz PowerShell, lépni a Templeton, Visual Studio) keresztül feladatok végrehajtása és a Hive ODBC-illesztővel és az Üzletiintelligencia-eszközök (pl. Excel, Power bi vagy Tableau) a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-109">It can also be used to run queries on Ambari views, execute jobs via external tools (i.e. PowerShell, Templeton, Visual Studio), and authenticate with the Hive ODBC driver and BI tools (i.e. Excel, PowerBI, or Tableau).</span></span>
* <span data-ttu-id="c4fc6-110">**SSH-felhasználó**: Ez a fiók SSH együtt, és sudo parancsok.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-110">**SSH user**:  This account can be used with SSH, and execute sudo commands.</span></span> <span data-ttu-id="c4fc6-111">A Linux virtuális gépek legfelső szintű engedélyekkel rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-111">It has root privileges to the Linux VMs.</span></span>

<span data-ttu-id="c4fc6-112">A tartományhoz csatlakoztatott HDInsight-fürt három mellett Ambari rendszergazda új felhasználókat és SSH-felhasználó rendelkezik.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-112">A domain-joined HDInsight cluster has three new users in addition to Ambari Admin and SSH user.</span></span>

* <span data-ttu-id="c4fc6-113">**Pletyka admin**: Ez egy a Apache Pletyka helyi rendszergazdai fiók.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-113">**Ranger admin**:  This account is the local Apache Ranger admin account.</span></span> <span data-ttu-id="c4fc6-114">Az active directory tartományi felhasználó nincs.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-114">It is not an active directory domain user.</span></span> <span data-ttu-id="c4fc6-115">Ez a fiók használható házirendek beállítása, és más felhasználók rendszergazdák vagy delegált rendszergazdák (így ezen házirendek kezelése).</span><span class="sxs-lookup"><span data-stu-id="c4fc6-115">This account can be used to setup policies and make other users admins or delegated admins (so that those users can manage policies).</span></span> <span data-ttu-id="c4fc6-116">Alapértelmezés szerint a felhasználónév az *admin* és a jelszó megegyezik az Ambari rendszergazdai jelszavát.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-116">By default, the username is *admin* and the password is the same as the Ambari admin password.</span></span> <span data-ttu-id="c4fc6-117">A jelszó a Pletyka a beállítások lapon lehet frissíteni.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-117">The password can be updated from the Settings page in Ranger.</span></span>
* <span data-ttu-id="c4fc6-118">**Fürt tartományi felhasználót a rendszergazda**: A fiók egy active directory tartományi felhasználóra, beleértve az Ambari és Pletyka Hadoop fürthöz rendszergazdaként kijelölt legyen.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-118">**Cluster admin domain user**: This account is an active directory domain user designated as the Hadoop cluster admin including Ambari and Ranger.</span></span> <span data-ttu-id="c4fc6-119">Fürt létrehozása során a felhasználó hitelesítő adatait kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-119">You must provide this user’s credentials during cluster creation.</span></span> <span data-ttu-id="c4fc6-120">A felhasználó rendelkezik-e a következő engedélyekkel:</span><span class="sxs-lookup"><span data-stu-id="c4fc6-120">This user has the following privileges:</span></span>

  * <span data-ttu-id="c4fc6-121">Számítógépek csatlakoztatása a tartományhoz, és helyezze el őket a szervezeti egységet a fürt létrehozása során belül.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-121">Join machines to the domain and place them within the OU that you specify during cluster creation.</span></span>
  * <span data-ttu-id="c4fc6-122">Hozzon létre szolgáltatásnevekről belül a szervezeti egységet a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-122">Create service principals within the OU that you specify during cluster creation.</span></span>
  * <span data-ttu-id="c4fc6-123">Névkeresési DNS-bejegyzéseket létrehozni.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-123">Create reverse DNS entries.</span></span>

    <span data-ttu-id="c4fc6-124">Megjegyzés: az egyéb AD a felhasználóknak is ezeket a jogokat.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-124">Note the other AD users also have these privileges.</span></span>

    <span data-ttu-id="c4fc6-125">Nincsenek egyes végpontok a fürtben (például lépni a Templeton) Pletyka által nem kezelt, és ezért nem biztonságos.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-125">There are some end points within the cluster (for example, Templeton) which are not managed by Ranger, and hence are not secure.</span></span> <span data-ttu-id="c4fc6-126">A végpontok az összes felhasználó számára, kivéve a fürt rendszergazdai tartományi felhasználó van zárolva.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-126">These end points are locked down for all users except the cluster admin domain user.</span></span>
* <span data-ttu-id="c4fc6-127">**Rendszeres**: fürt létrehozása során megadhatja a több active directory-csoportokat.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-127">**Regular**: During cluster creation, you can provide multiple active directory groups.</span></span> <span data-ttu-id="c4fc6-128">Ebben a csoportokban lévő felhasználók Pletyka és az Ambari lesznek szinkronizálva.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-128">The users in these groups will be synced to Ranger and Ambari.</span></span> <span data-ttu-id="c4fc6-129">Ezek a felhasználók tartományi felhasználók, és hozzáférhetnek csak Pletyka által felügyelt végpontok (például hiveserver2-n).</span><span class="sxs-lookup"><span data-stu-id="c4fc6-129">These users are domain users and will have access to only Ranger-managed endpoints (for example, Hiveserver2).</span></span> <span data-ttu-id="c4fc6-130">Az RBAC-házirendek és ezek a felhasználók használható naplózási lesz.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-130">All the RBAC policies and auditing will be applicable to these users.</span></span>

## <a name="roles-of-domain-joined-hdinsight-clusters"></a><span data-ttu-id="c4fc6-131">A tartományhoz a HDInsight-fürtök szerepkörök</span><span class="sxs-lookup"><span data-stu-id="c4fc6-131">Roles of Domain-joined HDInsight clusters</span></span>
<span data-ttu-id="c4fc6-132">Tartományhoz csatlakozó HDInsight rendelkezik a következő szerepkörök:</span><span class="sxs-lookup"><span data-stu-id="c4fc6-132">Domain-joined HDInsight have the following roles:</span></span>

* <span data-ttu-id="c4fc6-133">A Fürtfelügyelő</span><span class="sxs-lookup"><span data-stu-id="c4fc6-133">Cluster Administrator</span></span>
* <span data-ttu-id="c4fc6-134">Fürt operátor</span><span class="sxs-lookup"><span data-stu-id="c4fc6-134">Cluster Operator</span></span>
* <span data-ttu-id="c4fc6-135">Szolgáltatás-rendszergazda</span><span class="sxs-lookup"><span data-stu-id="c4fc6-135">Service Administrator</span></span>
* <span data-ttu-id="c4fc6-136">Szolgáltatás operátor</span><span class="sxs-lookup"><span data-stu-id="c4fc6-136">Service Operator</span></span>
* <span data-ttu-id="c4fc6-137">Fürt felhasználói</span><span class="sxs-lookup"><span data-stu-id="c4fc6-137">Cluster User</span></span>

<span data-ttu-id="c4fc6-138">**Ezek a szerepkörök engedélyeinek megtekintéséhez**</span><span class="sxs-lookup"><span data-stu-id="c4fc6-138">**To see the permissions of these roles**</span></span>

1. <span data-ttu-id="c4fc6-139">Nyissa meg az Ambari felügyeleti felhasználói Felületét.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-139">Open the Ambari Management UI.</span></span>  <span data-ttu-id="c4fc6-140">Lásd: [nyissa meg az Ambari kezelőfelület](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="c4fc6-140">See [Open the Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="c4fc6-141">Kattintson a bal oldali menü **szerepkörök**.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-141">From the left menu, click **Roles**.</span></span>
3. <span data-ttu-id="c4fc6-142">Kattintson a kék kérdőjel az engedélyek megtekintéséhez:</span><span class="sxs-lookup"><span data-stu-id="c4fc6-142">Click the blue question mark to see the permissions:</span></span>

    ![HDInsight szerepkörök engedélyekkel a tartományhoz](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-the-ambari-management-ui"></a><span data-ttu-id="c4fc6-144">Nyissa meg az Ambari kezelőfelület</span><span class="sxs-lookup"><span data-stu-id="c4fc6-144">Open the Ambari Management UI</span></span>
1. <span data-ttu-id="c4fc6-145">Jelentkezzen be az [Azure Portalra](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="c4fc6-145">Sign on to the [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="c4fc6-146">Nyissa meg a HDInsight-fürt egy panelen.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-146">Open your HDInsight cluster in a blade.</span></span> <span data-ttu-id="c4fc6-147">Lásd: [listája és megjelenítése fürtök](hdinsight-administer-use-management-portal.md#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="c4fc6-147">See [List and show clusters](hdinsight-administer-use-management-portal.md#list-and-show-clusters).</span></span>
3. <span data-ttu-id="c4fc6-148">Kattintson a **irányítópult** a felső menüben Ambari megnyitásához.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-148">Click **Dashboard** from the top menu to open Ambari.</span></span>
4. <span data-ttu-id="c4fc6-149">Jelentkezzen be az Ambari segítségével a fürt rendszergazdai tartományi felhasználónevet és jelszót.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-149">Log on to Ambari using the cluster administrator domain user name and password.</span></span>
5. <span data-ttu-id="c4fc6-150">Kattintson a **Admin** legördülő menü felső sarokban található jobbra, és kattintson a **kezelése az Ambari**.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-150">Click the **Admin** dropdown menu from the upper right corner, and then click **Manage Ambari**.</span></span>

    ![Tartományhoz csatlakozó HDInsight kezelése az Ambari](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    <span data-ttu-id="c4fc6-152">A felhasználói felület néz ki:</span><span class="sxs-lookup"><span data-stu-id="c4fc6-152">The UI looks like:</span></span>

    ![Tartományhoz csatlakozó HDInsight Ambari kezelőfelület](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-the-domain-users-synchronized-from-your-active-directory"></a><span data-ttu-id="c4fc6-154">A tartományi felhasználók az Active Directoryból szinkronizált felsorolása</span><span class="sxs-lookup"><span data-stu-id="c4fc6-154">List the domain users synchronized from your Active Directory</span></span>
1. <span data-ttu-id="c4fc6-155">Nyissa meg az Ambari felügyeleti felhasználói Felületét.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-155">Open the Ambari Management UI.</span></span>  <span data-ttu-id="c4fc6-156">Lásd: [nyissa meg az Ambari kezelőfelület](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="c4fc6-156">See [Open the Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="c4fc6-157">Kattintson a bal oldali menü **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-157">From the left menu, click **Users**.</span></span> <span data-ttu-id="c4fc6-158">Ekkor megjelenik a HDInsight-fürthöz az Active Directoryból szinkronizált felhasználók.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-158">You shall see all the users synced from your Active Directory to the HDInsight cluster.</span></span>

    ![Tartományhoz csatlakozó HDInsight Ambari felügyeleti felhasználói felület felhasználók listázása](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-the-domain-groups-synchronized-from-your-active-directory"></a><span data-ttu-id="c4fc6-160">Az Active Directoryból szinkronizált csoportok listázása</span><span class="sxs-lookup"><span data-stu-id="c4fc6-160">List the domain groups synchronized from your Active Directory</span></span>
1. <span data-ttu-id="c4fc6-161">Nyissa meg az Ambari felügyeleti felhasználói Felületét.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-161">Open the Ambari Management UI.</span></span>  <span data-ttu-id="c4fc6-162">Lásd: [nyissa meg az Ambari kezelőfelület](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="c4fc6-162">See [Open the Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="c4fc6-163">Kattintson a bal oldali menü **csoportok**.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-163">From the left menu, click **Groups**.</span></span> <span data-ttu-id="c4fc6-164">Ekkor megjelenik az összes, a a HDInsight-fürthöz az Active Directoryból szinkronizált csoportok.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-164">You shall see all the groups synced from your Active Directory to the HDInsight cluster.</span></span>

    ![Tartományhoz csatlakozó HDInsight Ambari felügyeleti felhasználói felület listázza a csoportokat](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a><span data-ttu-id="c4fc6-166">Nézetek Hive-engedélyek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c4fc6-166">Configure Hive Views permissions</span></span>
1. <span data-ttu-id="c4fc6-167">Nyissa meg az Ambari felügyeleti felhasználói Felületét.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-167">Open the Ambari Management UI.</span></span>  <span data-ttu-id="c4fc6-168">Lásd: [nyissa meg az Ambari kezelőfelület](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="c4fc6-168">See [Open the Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="c4fc6-169">Kattintson a bal oldali menü **nézetek**.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-169">From the left menu, click **Views**.</span></span>
3. <span data-ttu-id="c4fc6-170">Kattintson a **HIVE** a részletek megjelenítéséhez.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-170">Click **HIVE** to show the details.</span></span>

    ![HDInsight Ambari felügyeleti tartományhoz felhasználói felület Hive-nézetek](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. <span data-ttu-id="c4fc6-172">Kattintson a **Hive View** hivatkozás Hive nézetek konfigurálásához.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-172">Click the **Hive View** link to configure Hive Views.</span></span>
5. <span data-ttu-id="c4fc6-173">Görgessen le a **engedélyek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-173">Scroll down to the **Permissions** section.</span></span>

    ![HDInsight Ambari felügyeleti tartományhoz felhasználói felület Hive nézetek engedélyek konfigurálása](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. <span data-ttu-id="c4fc6-175">Kattintson a **felhasználó hozzáadása** vagy **csoport hozzáadása**, és adja meg a felhasználókat vagy csoportokat, amelyek a Hive-nézetek használatával.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-175">Click **Add User** or **Add Group**, and then specify the users or groups that can use Hive Views.</span></span>

## <a name="configure-users-for-the-roles"></a><span data-ttu-id="c4fc6-176">A szerepkörök a felhasználók konfigurálása</span><span class="sxs-lookup"><span data-stu-id="c4fc6-176">Configure users for the roles</span></span>
 <span data-ttu-id="c4fc6-177">Szerepkörök és a rájuk vonatkozó engedélyek listájának megtekintéséhez lásd: [szerepkörök a tartományhoz csatlakoztatott HDInsight-fürtök](#roles-of-domain---joined-hdinsight-clusters).</span><span class="sxs-lookup"><span data-stu-id="c4fc6-177">To see a list of roles and their permissions, see [Roles of Domain-joined HDInsight clusters](#roles-of-domain---joined-hdinsight-clusters).</span></span>

1. <span data-ttu-id="c4fc6-178">Nyissa meg az Ambari felügyeleti felhasználói Felületét.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-178">Open the Ambari Management UI.</span></span>  <span data-ttu-id="c4fc6-179">Lásd: [nyissa meg az Ambari kezelőfelület](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="c4fc6-179">See [Open the Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="c4fc6-180">Kattintson a bal oldali menü **szerepkörök**.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-180">From the left menu, click **Roles**.</span></span>
3. <span data-ttu-id="c4fc6-181">Kattintson a **felhasználó hozzáadása** vagy **csoport hozzáadása** felhasználók és csoportok hozzárendelése különböző szerepeknek.</span><span class="sxs-lookup"><span data-stu-id="c4fc6-181">Click **Add User** or **Add Group** to assign users and groups to different roles.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c4fc6-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="c4fc6-182">Next steps</span></span>
* <span data-ttu-id="c4fc6-183">A tartományhoz csatlakoztatott HDInsight-fürtök konfigurálásához lásd: [Tartományhoz csatlakoztatott HDInsight-fürtök konfigurálása](hdinsight-domain-joined-configure.md).</span><span class="sxs-lookup"><span data-stu-id="c4fc6-183">For configuring a Domain-joined HDInsight cluster, see [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md).</span></span>
* <span data-ttu-id="c4fc6-184">A Hive-házirendek konfigurálásához és a Hive-lekérdezések futtatásához lásd: [Hive-házirendek konfigurálása a tartományhoz csatlakoztatott HDInsight-fürtökben](hdinsight-domain-joined-run-hive.md).</span><span class="sxs-lookup"><span data-stu-id="c4fc6-184">For configuring Hive policies and run Hive queries, see [Configure Hive policies for Domain-joined HDInsight clusters](hdinsight-domain-joined-run-hive.md).</span></span>
* <span data-ttu-id="c4fc6-185">Az SSH használata a tartományhoz csatlakoztatott HDInsight-fürtökön Hive-lekérdezéseket futtat, tekintse meg a [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span><span class="sxs-lookup"><span data-stu-id="c4fc6-185">For running Hive queries using SSH on Domain-joined HDInsight clusters, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span></span>
