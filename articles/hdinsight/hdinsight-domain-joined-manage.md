---
title: "aaaManage tartományhoz a HDInsight-fürtök - Azure |} Microsoft Docs"
description: "Ismerje meg, hogyan toomanage tartományhoz HDInsight-fürtök"
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
ms.openlocfilehash: 233ddf0953e981f9a24b77d9dde194d590e5e6d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="manage-domain-joined-hdinsight-clusters-preview"></a><span data-ttu-id="9f77f-103">Tartományhoz csatlakozó HDInsight-fürtök (előzetes verzió) kezelése</span><span class="sxs-lookup"><span data-stu-id="9f77f-103">Manage Domain-joined HDInsight clusters (Preview)</span></span>
<span data-ttu-id="9f77f-104">Ismerje meg, hello felhasználók és a tartományhoz, és hogyan toomanage tartományhoz HDInsight-fürtök hello szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="9f77f-104">Learn hello users and hello roles in Domain-joined HDInsight, and how toomanage domain-joined HDInsight clusters.</span></span>

## <a name="users-of-domain-joined-hdinsight-clusters"></a><span data-ttu-id="9f77f-105">A tartományhoz a HDInsight-fürtök felhasználók</span><span class="sxs-lookup"><span data-stu-id="9f77f-105">Users of Domain-joined HDInsight clusters</span></span>
<span data-ttu-id="9f77f-106">Egy HDInsight-fürthöz nem csatlakozó tartomány-van két olyan felhasználói fiókot, amely hello fürt létrehozása során jönnek létre:</span><span class="sxs-lookup"><span data-stu-id="9f77f-106">An HDInsight cluster that is not domain-joined has two user accounts that are created during hello cluster creation:</span></span>

* <span data-ttu-id="9f77f-107">**Ambari admin**: Ez a fiók akkor is *Hadoop felhasználói* vagy *HTTP felhasználói*.</span><span class="sxs-lookup"><span data-stu-id="9f77f-107">**Ambari admin**: This account is also known as *Hadoop user* or *HTTP user*.</span></span> <span data-ttu-id="9f77f-108">Ez a fiók lehet a következő https:// tooAmbari használt toolog&lt;clustername >. azurehdinsight.net.</span><span class="sxs-lookup"><span data-stu-id="9f77f-108">This account can be used toolog on tooAmbari at https://&lt;clustername>.azurehdinsight.net.</span></span> <span data-ttu-id="9f77f-109">Azt is használt toorun lekérdezései Ambari nézetek, feladatok külső eszközök (azaz PowerShell, lépni a Templeton, Visual Studio) keresztül hajtható végre, és hello Hive ODBC-illesztővel és az Üzletiintelligencia-eszközök (pl. Excel, Power bi vagy Tableau) a hitelesítést.</span><span class="sxs-lookup"><span data-stu-id="9f77f-109">It can also be used toorun queries on Ambari views, execute jobs via external tools (i.e. PowerShell, Templeton, Visual Studio), and authenticate with hello Hive ODBC driver and BI tools (i.e. Excel, PowerBI, or Tableau).</span></span>
* <span data-ttu-id="9f77f-110">**SSH-felhasználó**: Ez a fiók SSH együtt, és sudo parancsok.</span><span class="sxs-lookup"><span data-stu-id="9f77f-110">**SSH user**:  This account can be used with SSH, and execute sudo commands.</span></span> <span data-ttu-id="9f77f-111">Legfelső szintű jogosultságokkal toohello Linux virtuális gépeket tartalmaz.</span><span class="sxs-lookup"><span data-stu-id="9f77f-111">It has root privileges toohello Linux VMs.</span></span>

<span data-ttu-id="9f77f-112">Egy tartományhoz csatlakozó HDInsight-fürt rendelkezik három új felhasználók hozzáadása tooAmbari rendszergazda és az SSH-felhasználó.</span><span class="sxs-lookup"><span data-stu-id="9f77f-112">A domain-joined HDInsight cluster has three new users in addition tooAmbari Admin and SSH user.</span></span>

* <span data-ttu-id="9f77f-113">**Pletyka admin**: Ez egy hello helyi Apache Pletyka rendszergazdai fiók.</span><span class="sxs-lookup"><span data-stu-id="9f77f-113">**Ranger admin**:  This account is hello local Apache Ranger admin account.</span></span> <span data-ttu-id="9f77f-114">Az active directory tartományi felhasználó nincs.</span><span class="sxs-lookup"><span data-stu-id="9f77f-114">It is not an active directory domain user.</span></span> <span data-ttu-id="9f77f-115">Ehhez a fiókhoz használt toosetup házirendek kell, és ellenőrizze a más felhasználók rendszergazdák vagy delegált rendszergazdák (így ezen házirendek kezelése).</span><span class="sxs-lookup"><span data-stu-id="9f77f-115">This account can be used toosetup policies and make other users admins or delegated admins (so that those users can manage policies).</span></span> <span data-ttu-id="9f77f-116">Alapértelmezés szerint hello felhasználónév az *admin* és hello jelszó van hello megegyeznek a hello Ambari rendszergazdai jelszavát.</span><span class="sxs-lookup"><span data-stu-id="9f77f-116">By default, hello username is *admin* and hello password is hello same as hello Ambari admin password.</span></span> <span data-ttu-id="9f77f-117">hello jelszó hello beállításai lapon a Pletyka lehet frissíteni.</span><span class="sxs-lookup"><span data-stu-id="9f77f-117">hello password can be updated from hello Settings page in Ranger.</span></span>
* <span data-ttu-id="9f77f-118">**Fürt tartományi felhasználót a rendszergazda**: ezt a fiókot az active directory tartományi felhasználó, beleértve az Ambari és Pletyka Hadoop-fürt felügyeleti hello kijelölt.</span><span class="sxs-lookup"><span data-stu-id="9f77f-118">**Cluster admin domain user**: This account is an active directory domain user designated as hello Hadoop cluster admin including Ambari and Ranger.</span></span> <span data-ttu-id="9f77f-119">Fürt létrehozása során a felhasználó hitelesítő adatait kell megadnia.</span><span class="sxs-lookup"><span data-stu-id="9f77f-119">You must provide this user’s credentials during cluster creation.</span></span> <span data-ttu-id="9f77f-120">Ez a felhasználó rendelkezik jogosultsággal a következő hello:</span><span class="sxs-lookup"><span data-stu-id="9f77f-120">This user has hello following privileges:</span></span>

  * <span data-ttu-id="9f77f-121">Gépek toohello tartományhoz, és helyezze el őket belül hello szervezeti egységet a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="9f77f-121">Join machines toohello domain and place them within hello OU that you specify during cluster creation.</span></span>
  * <span data-ttu-id="9f77f-122">Hozzon létre szolgáltatásnevekről belül hello szervezeti egységet a fürt létrehozása során.</span><span class="sxs-lookup"><span data-stu-id="9f77f-122">Create service principals within hello OU that you specify during cluster creation.</span></span>
  * <span data-ttu-id="9f77f-123">Névkeresési DNS-bejegyzéseket létrehozni.</span><span class="sxs-lookup"><span data-stu-id="9f77f-123">Create reverse DNS entries.</span></span>

    <span data-ttu-id="9f77f-124">Megjegyzés: hello más AD felhasználók is ezeket a jogokat.</span><span class="sxs-lookup"><span data-stu-id="9f77f-124">Note hello other AD users also have these privileges.</span></span>

    <span data-ttu-id="9f77f-125">Nincsenek egyes végpontok (például lépni a Templeton) hello fürtön belül Pletyka által nem kezelt, és ezért nem biztonságos.</span><span class="sxs-lookup"><span data-stu-id="9f77f-125">There are some end points within hello cluster (for example, Templeton) which are not managed by Ranger, and hence are not secure.</span></span> <span data-ttu-id="9f77f-126">A végpontok hello fürt rendszergazdai tartományi felhasználó kivételével minden felhasználóra van zárolva.</span><span class="sxs-lookup"><span data-stu-id="9f77f-126">These end points are locked down for all users except hello cluster admin domain user.</span></span>
* <span data-ttu-id="9f77f-127">**Rendszeres**: fürt létrehozása során megadhatja a több active directory-csoportokat.</span><span class="sxs-lookup"><span data-stu-id="9f77f-127">**Regular**: During cluster creation, you can provide multiple active directory groups.</span></span> <span data-ttu-id="9f77f-128">Ebben a csoportokban lévő hello felhasználók szinkronizált tooRanger és Ambari lesz.</span><span class="sxs-lookup"><span data-stu-id="9f77f-128">hello users in these groups will be synced tooRanger and Ambari.</span></span> <span data-ttu-id="9f77f-129">Ezek a felhasználók tartományi felhasználók pedig a hozzáférési tooonly Pletyka által felügyelt végpontok (például hiveserver2-n).</span><span class="sxs-lookup"><span data-stu-id="9f77f-129">These users are domain users and will have access tooonly Ranger-managed endpoints (for example, Hiveserver2).</span></span> <span data-ttu-id="9f77f-130">Összes hello RBAC-házirendek és naplózás alkalmazható toothese felhasználók.</span><span class="sxs-lookup"><span data-stu-id="9f77f-130">All hello RBAC policies and auditing will be applicable toothese users.</span></span>

## <a name="roles-of-domain-joined-hdinsight-clusters"></a><span data-ttu-id="9f77f-131">A tartományhoz a HDInsight-fürtök szerepkörök</span><span class="sxs-lookup"><span data-stu-id="9f77f-131">Roles of Domain-joined HDInsight clusters</span></span>
<span data-ttu-id="9f77f-132">A HDInsight a tartományhoz a következő szerepkörök hello rendelkezik:</span><span class="sxs-lookup"><span data-stu-id="9f77f-132">Domain-joined HDInsight have hello following roles:</span></span>

* <span data-ttu-id="9f77f-133">A Fürtfelügyelő</span><span class="sxs-lookup"><span data-stu-id="9f77f-133">Cluster Administrator</span></span>
* <span data-ttu-id="9f77f-134">Fürt operátor</span><span class="sxs-lookup"><span data-stu-id="9f77f-134">Cluster Operator</span></span>
* <span data-ttu-id="9f77f-135">Szolgáltatás-rendszergazda</span><span class="sxs-lookup"><span data-stu-id="9f77f-135">Service Administrator</span></span>
* <span data-ttu-id="9f77f-136">Szolgáltatás operátor</span><span class="sxs-lookup"><span data-stu-id="9f77f-136">Service Operator</span></span>
* <span data-ttu-id="9f77f-137">Fürt felhasználói</span><span class="sxs-lookup"><span data-stu-id="9f77f-137">Cluster User</span></span>

<span data-ttu-id="9f77f-138">**Ezek a szerepkörök toosee hello engedélyei**</span><span class="sxs-lookup"><span data-stu-id="9f77f-138">**toosee hello permissions of these roles**</span></span>

1. <span data-ttu-id="9f77f-139">Nyissa meg a hello Ambari kezelő felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="9f77f-139">Open hello Ambari Management UI.</span></span>  <span data-ttu-id="9f77f-140">Lásd: [nyitott hello Ambari felügyeleti felhasználói felület](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="9f77f-140">See [Open hello Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="9f77f-141">Hello bal oldali menüben kattintson **szerepkörök**.</span><span class="sxs-lookup"><span data-stu-id="9f77f-141">From hello left menu, click **Roles**.</span></span>
3. <span data-ttu-id="9f77f-142">Kattintson a kék hello kérdőjel toosee hello engedélyek:</span><span class="sxs-lookup"><span data-stu-id="9f77f-142">Click hello blue question mark toosee hello permissions:</span></span>

    ![HDInsight szerepkörök engedélyekkel a tartományhoz](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-roles-permissions.png)

## <a name="open-hello-ambari-management-ui"></a><span data-ttu-id="9f77f-144">Nyissa meg a hello Ambari felügyeleti felhasználói felület</span><span class="sxs-lookup"><span data-stu-id="9f77f-144">Open hello Ambari Management UI</span></span>
1. <span data-ttu-id="9f77f-145">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="9f77f-145">Sign on toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="9f77f-146">Nyissa meg a HDInsight-fürt egy panelen.</span><span class="sxs-lookup"><span data-stu-id="9f77f-146">Open your HDInsight cluster in a blade.</span></span> <span data-ttu-id="9f77f-147">Lásd: [listája és megjelenítése fürtök](hdinsight-administer-use-management-portal.md#list-and-show-clusters).</span><span class="sxs-lookup"><span data-stu-id="9f77f-147">See [List and show clusters](hdinsight-administer-use-management-portal.md#list-and-show-clusters).</span></span>
3. <span data-ttu-id="9f77f-148">Kattintson a **irányítópult** a hello felső menüjében tooopen Ambari.</span><span class="sxs-lookup"><span data-stu-id="9f77f-148">Click **Dashboard** from hello top menu tooopen Ambari.</span></span>
4. <span data-ttu-id="9f77f-149">Jelentkezzen be tooAmbari hello fürt rendszergazdai tartományi felhasználónevet és jelszót használja.</span><span class="sxs-lookup"><span data-stu-id="9f77f-149">Log on tooAmbari using hello cluster administrator domain user name and password.</span></span>
5. <span data-ttu-id="9f77f-150">Hello kattintson **Admin** hello jobb felső sarokban, és kattintson a legördülő menü **kezelése az Ambari**.</span><span class="sxs-lookup"><span data-stu-id="9f77f-150">Click hello **Admin** dropdown menu from hello upper right corner, and then click **Manage Ambari**.</span></span>

    ![Tartományhoz csatlakozó HDInsight kezelése az Ambari](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-manage-ambari.png)

    <span data-ttu-id="9f77f-152">felhasználói felület hello néz ki:</span><span class="sxs-lookup"><span data-stu-id="9f77f-152">hello UI looks like:</span></span>

    ![Tartományhoz csatlakozó HDInsight Ambari kezelőfelület](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui.png)

## <a name="list-hello-domain-users-synchronized-from-your-active-directory"></a><span data-ttu-id="9f77f-154">Az Active Directoryból szinkronizált hello tartományi felhasználók listázása</span><span class="sxs-lookup"><span data-stu-id="9f77f-154">List hello domain users synchronized from your Active Directory</span></span>
1. <span data-ttu-id="9f77f-155">Nyissa meg a hello Ambari kezelő felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="9f77f-155">Open hello Ambari Management UI.</span></span>  <span data-ttu-id="9f77f-156">Lásd: [nyitott hello Ambari felügyeleti felhasználói felület](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="9f77f-156">See [Open hello Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="9f77f-157">Hello bal oldali menüben kattintson **felhasználók**.</span><span class="sxs-lookup"><span data-stu-id="9f77f-157">From hello left menu, click **Users**.</span></span> <span data-ttu-id="9f77f-158">Ekkor megjelenik az Active Directory toohello HDInsight-fürtjéhez szinkronizált senki hello.</span><span class="sxs-lookup"><span data-stu-id="9f77f-158">You shall see all hello users synced from your Active Directory toohello HDInsight cluster.</span></span>

    ![Tartományhoz csatlakozó HDInsight Ambari felügyeleti felhasználói felület felhasználók listázása](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-users.png)

## <a name="list-hello-domain-groups-synchronized-from-your-active-directory"></a><span data-ttu-id="9f77f-160">Az Active Directoryból szinkronizált hello tartomány csoportjainak</span><span class="sxs-lookup"><span data-stu-id="9f77f-160">List hello domain groups synchronized from your Active Directory</span></span>
1. <span data-ttu-id="9f77f-161">Nyissa meg a hello Ambari kezelő felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="9f77f-161">Open hello Ambari Management UI.</span></span>  <span data-ttu-id="9f77f-162">Lásd: [nyitott hello Ambari felügyeleti felhasználói felület](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="9f77f-162">See [Open hello Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="9f77f-163">Hello bal oldali menüben kattintson **csoportok**.</span><span class="sxs-lookup"><span data-stu-id="9f77f-163">From hello left menu, click **Groups**.</span></span> <span data-ttu-id="9f77f-164">Ekkor megjelenik az Active Directory toohello HDInsight-fürtjéhez szinkronizált összes hello csoport.</span><span class="sxs-lookup"><span data-stu-id="9f77f-164">You shall see all hello groups synced from your Active Directory toohello HDInsight cluster.</span></span>

    ![Tartományhoz csatlakozó HDInsight Ambari felügyeleti felhasználói felület listázza a csoportokat](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-groups.png)

## <a name="configure-hive-views-permissions"></a><span data-ttu-id="9f77f-166">Nézetek Hive-engedélyek konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9f77f-166">Configure Hive Views permissions</span></span>
1. <span data-ttu-id="9f77f-167">Nyissa meg a hello Ambari kezelő felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="9f77f-167">Open hello Ambari Management UI.</span></span>  <span data-ttu-id="9f77f-168">Lásd: [nyitott hello Ambari felügyeleti felhasználói felület](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="9f77f-168">See [Open hello Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="9f77f-169">Hello bal oldali menüben kattintson **nézetek**.</span><span class="sxs-lookup"><span data-stu-id="9f77f-169">From hello left menu, click **Views**.</span></span>
3. <span data-ttu-id="9f77f-170">Kattintson a **HIVE** tooshow hello részleteit.</span><span class="sxs-lookup"><span data-stu-id="9f77f-170">Click **HIVE** tooshow hello details.</span></span>

    ![HDInsight Ambari felügyeleti tartományhoz felhasználói felület Hive-nézetek](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views.png)
4. <span data-ttu-id="9f77f-172">Kattintson a hello **Hive View** tooconfigure Hive nézetekhez kapcsolni.</span><span class="sxs-lookup"><span data-stu-id="9f77f-172">Click hello **Hive View** link tooconfigure Hive Views.</span></span>
5. <span data-ttu-id="9f77f-173">Görgessen lefelé toohello **engedélyek** szakasz.</span><span class="sxs-lookup"><span data-stu-id="9f77f-173">Scroll down toohello **Permissions** section.</span></span>

    ![HDInsight Ambari felügyeleti tartományhoz felhasználói felület Hive nézetek engedélyek konfigurálása](./media/hdinsight-domain-joined-manage/hdinsight-domain-joined-ambari-management-ui-hive-views-permissions.png)
6. <span data-ttu-id="9f77f-175">Kattintson a **felhasználó hozzáadása** vagy **csoport hozzáadása**, majd adja meg a hello felhasználók vagy csoportok, a Hive-nézetek használatával.</span><span class="sxs-lookup"><span data-stu-id="9f77f-175">Click **Add User** or **Add Group**, and then specify hello users or groups that can use Hive Views.</span></span>

## <a name="configure-users-for-hello-roles"></a><span data-ttu-id="9f77f-176">Felhasználók hello szerepkörök konfigurálása</span><span class="sxs-lookup"><span data-stu-id="9f77f-176">Configure users for hello roles</span></span>
 <span data-ttu-id="9f77f-177">toosee szerepköröket és engedélyeiket, listáját lásd: [szerepkörök a tartományhoz csatlakoztatott HDInsight-fürtök](#roles-of-domain---joined-hdinsight-clusters).</span><span class="sxs-lookup"><span data-stu-id="9f77f-177">toosee a list of roles and their permissions, see [Roles of Domain-joined HDInsight clusters](#roles-of-domain---joined-hdinsight-clusters).</span></span>

1. <span data-ttu-id="9f77f-178">Nyissa meg a hello Ambari kezelő felhasználói felületén.</span><span class="sxs-lookup"><span data-stu-id="9f77f-178">Open hello Ambari Management UI.</span></span>  <span data-ttu-id="9f77f-179">Lásd: [nyitott hello Ambari felügyeleti felhasználói felület](#open-the-ambari-management-ui).</span><span class="sxs-lookup"><span data-stu-id="9f77f-179">See [Open hello Ambari Management UI](#open-the-ambari-management-ui).</span></span>
2. <span data-ttu-id="9f77f-180">Hello bal oldali menüben kattintson **szerepkörök**.</span><span class="sxs-lookup"><span data-stu-id="9f77f-180">From hello left menu, click **Roles**.</span></span>
3. <span data-ttu-id="9f77f-181">Kattintson a **felhasználó hozzáadása** vagy **csoport hozzáadása** tooassign felhasználók és csoportok toodifferent szerepkörök.</span><span class="sxs-lookup"><span data-stu-id="9f77f-181">Click **Add User** or **Add Group** tooassign users and groups toodifferent roles.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9f77f-182">Következő lépések</span><span class="sxs-lookup"><span data-stu-id="9f77f-182">Next steps</span></span>
* <span data-ttu-id="9f77f-183">A tartományhoz csatlakoztatott HDInsight-fürtök konfigurálásához lásd: [Tartományhoz csatlakoztatott HDInsight-fürtök konfigurálása](hdinsight-domain-joined-configure.md).</span><span class="sxs-lookup"><span data-stu-id="9f77f-183">For configuring a Domain-joined HDInsight cluster, see [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md).</span></span>
* <span data-ttu-id="9f77f-184">A Hive-házirendek konfigurálásához és a Hive-lekérdezések futtatásához lásd: [Hive-házirendek konfigurálása a tartományhoz csatlakoztatott HDInsight-fürtökben](hdinsight-domain-joined-run-hive.md).</span><span class="sxs-lookup"><span data-stu-id="9f77f-184">For configuring Hive policies and run Hive queries, see [Configure Hive policies for Domain-joined HDInsight clusters](hdinsight-domain-joined-run-hive.md).</span></span>
* <span data-ttu-id="9f77f-185">Az SSH használata a tartományhoz csatlakoztatott HDInsight-fürtökön Hive-lekérdezéseket futtat, tekintse meg a [az SSH a Hdinsighttal](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span><span class="sxs-lookup"><span data-stu-id="9f77f-185">For running Hive queries using SSH on Domain-joined HDInsight clusters, see [Use SSH with HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).</span></span>
