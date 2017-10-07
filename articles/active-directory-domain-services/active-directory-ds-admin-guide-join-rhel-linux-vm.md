---
title: "Az Azure Active Directory tartományi szolgáltatások: Csatlakozás az RHEL VM tooa felügyelt tartományhoz |} Microsoft Docs"
description: "A Red Hat Enterprise Linux virtuális gép tooAzure AD tartományi szolgáltatások"
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: stevenpo
editor: curtand
ms.assetid: 87291c47-1280-43f8-8fb2-da1bd61a4942
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: maheshu
ms.openlocfilehash: 41ca2aaf2eefbf9c403d2b834d61a1aa0943d950
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: hu-HU
ms.lasthandoff: 10/06/2017
---
# <a name="join-a-red-hat-enterprise-linux-7-virtual-machine-tooa-managed-domain"></a><span data-ttu-id="bc1b7-103">Csatlakozás Red Hat Enterprise Linux 7 virtuális gép tooa felügyelt tartományhoz</span><span class="sxs-lookup"><span data-stu-id="bc1b7-103">Join a Red Hat Enterprise Linux 7 virtual machine tooa managed domain</span></span>
<span data-ttu-id="bc1b7-104">Ez a cikk bemutatja, hogyan toojoin a Red Hat Enterprise Linux (RHEL) 7 virtuális gép tooan Azure AD tartományi szolgáltatások által felügyelt tartomány.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-104">This article shows you how toojoin a Red Hat Enterprise Linux (RHEL) 7 virtual machine tooan Azure AD Domain Services managed domain.</span></span>

## <a name="provision-a-red-hat-enterprise-linux-virtual-machine"></a><span data-ttu-id="bc1b7-105">Red Hat Enterprise Linux virtuális gép kiépítése</span><span class="sxs-lookup"><span data-stu-id="bc1b7-105">Provision a Red Hat Enterprise Linux virtual machine</span></span>
<span data-ttu-id="bc1b7-106">Hajtsa végre a következő lépéseket tooprovision egy RHEL 7 virtuális gép hello Azure-portál használatával hello.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-106">Perform hello following steps tooprovision a RHEL 7 virtual machine using hello Azure portal.</span></span>

1. <span data-ttu-id="bc1b7-107">Jelentkezzen be toohello [Azure-portálon](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="bc1b7-107">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>

    ![Azure-portál irányítópultjának](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-dashboard.png)
2. <span data-ttu-id="bc1b7-109">Kattintson a **új** a bal oldali ablaktábla és a típus hello **Red Hat** hello keresősávban, ahogy az alábbi képernyőfelvétel a hello be.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-109">Click **New** on hello left pane and type **Red Hat** into hello search bar as shown in hello following screenshot.</span></span> <span data-ttu-id="bc1b7-110">Red Hat Enterprise Linux bejegyzéseket hello találatok jelennek meg.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-110">Entries for Red Hat Enterprise Linux appear in hello search results.</span></span> <span data-ttu-id="bc1b7-111">Kattintson a **Red Hat Enterprise Linux 7.2**.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-111">Click **Red Hat Enterprise Linux 7.2**.</span></span>

    ![Válassza ki az RHEL eredmények](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-find-rhel-image.png)
3. <span data-ttu-id="bc1b7-113">Keresés hello hello **mindent** ablaktábla szerepelnie kell a Red Hat Enterprise Linux 7.2 hello kép.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-113">hello search results in hello **Everything** pane should list hello Red Hat Enterprise Linux 7.2 image.</span></span> <span data-ttu-id="bc1b7-114">Kattintson a **Red Hat Enterprise Linux 7.2** tooview hello virtuálisgép-lemezkép további információt.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-114">Click **Red Hat Enterprise Linux 7.2** tooview more information about hello virtual machine image.</span></span>

    ![Válassza ki az RHEL eredmények](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-rhel-image.png)
4. <span data-ttu-id="bc1b7-116">A hello **Red Hat Enterprise Linux 7.2** ablaktáblán hello virtuálisgép-lemezkép további információt kell megjelennie.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-116">In hello **Red Hat Enterprise Linux 7.2** pane, you should see more information about hello virtual machine image.</span></span> <span data-ttu-id="bc1b7-117">A hello **telepítési modell kiválasztása** legördülő menüben válasszon ki **klasszikus**.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-117">In hello **Select a deployment model** dropdown, select **Classic**.</span></span> <span data-ttu-id="bc1b7-118">Kattintson a hello **létrehozása** gombra.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-118">Then click hello **Create** button.</span></span>

    ![Kép részleteinek megtekintése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-clicked.png)
5. <span data-ttu-id="bc1b7-120">A hello **alapjai** hello oldalán **hozzon létre virtuális gépet** varázsló, adja meg a hello **állomásnév** hello új virtuális gép.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-120">In hello **Basics** page of hello **Create virtual machine** wizard, enter hello **Host Name** for hello new virtual machine.</span></span> <span data-ttu-id="bc1b7-121">Ezenfelül adja meg a helyi rendszergazdai felhasználónév hello **felhasználónév** mező és egy **jelszó**.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-121">Also specify a local administrator user name in hello **User name** field and a **Password**.</span></span> <span data-ttu-id="bc1b7-122">Dönthet úgy is toouse az SSH-kulcs tooauthenticate hello helyi rendszergazda felhasználó.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-122">You may also choose toouse an SSH key tooauthenticate hello local administrator user.</span></span> <span data-ttu-id="bc1b7-123">Jelölje ki a **Tarifacsomagot** hello virtuális géphez.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-123">Also select a **Pricing Tier** for hello virtual machine.</span></span>

    ![Hozzon létre virtuális Gépet - alapismeretek lap](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-basic-details.png)
6. <span data-ttu-id="bc1b7-125">A hello **mérete** hello oldalán **hozzon létre virtuális gépet** hello virtuális gép varázsló, jelölje be hello mérete.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-125">In hello **Size** page of hello **Create virtual machine** wizard, select hello size for hello virtual machine.</span></span>

    ![Hozzon létre Virtuálisgép - méret kiválasztása](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-select-vm-size.png)

7. <span data-ttu-id="bc1b7-127">A hello **beállítások** hello oldalán **hozzon létre virtuális gépet** hello virtuális gép varázsló, jelölje be hello storage-fiókjához.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-127">In hello **Settings** page of hello **Create virtual machine** wizard, select hello storage account for hello virtual machine.</span></span> <span data-ttu-id="bc1b7-128">Kattintson a **virtuális hálózati** tooselect hello virtuális hálózati toowhich hello Linux virtuális Gépre kell telepíteni.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-128">Click **Virtual Network** tooselect hello virtual network toowhich hello Linux VM should be deployed.</span></span> <span data-ttu-id="bc1b7-129">A hello **virtuális hálózati** panelen, jelölje be hello virtuális hálózat, amelyben az Azure AD tartományi szolgáltatások érhető el.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-129">In hello **Virtual Network** blade, select hello virtual network in which Azure AD Domain Services is available.</span></span> <span data-ttu-id="bc1b7-130">Ebben a példában azt válasszon hello "MyPreviewVNet" virtuális hálózatot.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-130">In this example, we pick hello 'MyPreviewVNet' virtual network.</span></span>

    ![Virtuális gép létrehozása – válassza ki a virtuális hálózat](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-select-vnet.png)
8. <span data-ttu-id="bc1b7-132">A hello **összegzés** hello oldalán **hozzon létre virtuális gépet** varázsló, tekintse át, és kattintson hello **OK** gombra.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-132">On hello **Summary** page of hello **Create virtual machine** wizard, review and click hello **OK** button.</span></span>

    ![VM - kiválasztott virtuális hálózat létrehozása](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-vnet-selected.png)
9. <span data-ttu-id="bc1b7-134">Hello hello RHEL 7.2 lemezképen alapuló új virtuális gép telepítését kell kezdődnie.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-134">Deployment of hello new virtual machine based on hello RHEL 7.2 image should start.</span></span>

    ![Hozzon létre virtuális Gépet - KözpontiTelepítés elindítva](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployment-started.png)
10. <span data-ttu-id="bc1b7-136">Néhány perc elteltével hello virtuális gép sikeresen telepített és használatra kész állapotba kell lennie.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-136">After a few minutes, hello virtual machine should be deployed successfully and ready for use.</span></span>

    ![Hozzon létre virtuális Gépet - telepítve](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-create-vm-deployed.png)

## <a name="connect-remotely-toohello-newly-provisioned-linux-virtual-machine"></a><span data-ttu-id="bc1b7-138">Távoli csatlakozás az újonnan kiépített toohello Linux virtuális gép</span><span class="sxs-lookup"><span data-stu-id="bc1b7-138">Connect remotely toohello newly provisioned Linux virtual machine</span></span>
<span data-ttu-id="bc1b7-139">hello RHEL 7.2 rendszerű virtuális gép az Azure-ban van kiépítve.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-139">hello RHEL 7.2 virtual machine has been provisioned in Azure.</span></span> <span data-ttu-id="bc1b7-140">hello következő feladata tooconnect távolról toohello virtuális gépet.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-140">hello next task is tooconnect remotely toohello virtual machine.</span></span>

<span data-ttu-id="bc1b7-141">**Csatlakozás toohello RHEL 7.2 rendszerű virtuális gép** hello hello cikk utasításait követve [hogyan toolog a Linux rendszerű virtuális gép tooa](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bc1b7-141">**Connect toohello RHEL 7.2 virtual machine** Follow hello instructions in hello article [How toolog on tooa virtual machine running Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

<span data-ttu-id="bc1b7-142">hello további hello lépéseit azt feltételezik, hogy hello PuTTY SSH ügyfél tooconnect toohello RHEL virtuális gép használja.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-142">hello rest of hello steps assume you use hello PuTTY SSH client tooconnect toohello RHEL virtual machine.</span></span> <span data-ttu-id="bc1b7-143">További információkért lásd: hello [PuTTY letöltési oldalát](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span><span class="sxs-lookup"><span data-stu-id="bc1b7-143">For more information, see hello [PuTTY Download page](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).</span></span>

1. <span data-ttu-id="bc1b7-144">Nyissa meg hello PuTTY program.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-144">Open hello PuTTY program.</span></span>
2. <span data-ttu-id="bc1b7-145">Adja meg a hello **állomásnév** az RHEL virtuális gép található, újonnan létrehozott hello.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-145">Enter hello **Host Name** for hello newly created RHEL virtual machine.</span></span> <span data-ttu-id="bc1b7-146">Ebben a példában a virtuális gép van hello állomás neve "contoso-rhel.cloudapp .net".</span><span class="sxs-lookup"><span data-stu-id="bc1b7-146">In this example, our virtual machine has hello host name 'contoso-rhel.cloudapp.net'.</span></span> <span data-ttu-id="bc1b7-147">Ha nem biztos a virtuális gép hello állomásnevét, tekintse meg a toohello VM irányítópult hello Azure-portálon.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-147">If you are not sure of hello host name of your VM, refer toohello VM dashboard on hello Azure portal.</span></span>

    ![A puTTY-csatlakozás](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-connect.png)
3. <span data-ttu-id="bc1b7-149">Jelentkezzen be toohello virtuális gép hello virtuális gép létrehozásakor megadott hello helyi rendszergazdai hitelesítő adatokkal.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-149">Log on toohello virtual machine using hello local administrator credentials you specified when hello virtual machine was created.</span></span> <span data-ttu-id="bc1b7-150">Ebben a példában hello helyi rendszergazdai fiók "mahesh" használtuk.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-150">In this example, we used hello local administrator account "mahesh".</span></span>

    ![PuTTY bejelentkezés](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-login.png)

## <a name="install-required-packages-on-hello-linux-virtual-machine"></a><span data-ttu-id="bc1b7-152">Telepíti a szükséges csomagokat hello Linux virtuális gépen</span><span class="sxs-lookup"><span data-stu-id="bc1b7-152">Install required packages on hello Linux virtual machine</span></span>
<span data-ttu-id="bc1b7-153">Csatlakozó toohello virtuális gép után hello következő feladata a tooinstall csomagok szükséges a tartományhoz való csatlakozást hello virtuális gépen.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-153">After connecting toohello virtual machine, hello next task is tooinstall packages required for domain join on hello virtual machine.</span></span> <span data-ttu-id="bc1b7-154">Hajtsa végre az alábbi lépésekkel hello:</span><span class="sxs-lookup"><span data-stu-id="bc1b7-154">Perform hello following steps:</span></span>

1. <span data-ttu-id="bc1b7-155">**Telepítse a realmd:** hello realmd csomag szolgál a tartományhoz való csatlakozást.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-155">**Install realmd:** hello realmd package is used for domain join.</span></span> <span data-ttu-id="bc1b7-156">A PuTTY terminál írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="bc1b7-156">In your PuTTY terminal, type hello following command:</span></span>

    <span data-ttu-id="bc1b7-157">sudo yum telepítés realmd</span><span class="sxs-lookup"><span data-stu-id="bc1b7-157">sudo yum install realmd</span></span>

    ![Realmd telepítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-realmd.png)

    <span data-ttu-id="bc1b7-159">Néhány perc múlva hello realmd csomag hello virtuális gép első telepíthető.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-159">After a few minutes, hello realmd package should get installed on hello virtual machine.</span></span>

    ![telepített realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-installed.png)
2. <span data-ttu-id="bc1b7-161">**Telepítse a sssd:** hello realmd csomag sssd tooperform tartomány összekapcsolási műveletek függ.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-161">**Install sssd:** hello realmd package depends on sssd tooperform domain join operations.</span></span> <span data-ttu-id="bc1b7-162">A PuTTY terminál írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="bc1b7-162">In your PuTTY terminal, type hello following command:</span></span>

    <span data-ttu-id="bc1b7-163">sudo yum telepítés sssd</span><span class="sxs-lookup"><span data-stu-id="bc1b7-163">sudo yum install sssd</span></span>

    ![Sssd telepítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-sssd.png)

    <span data-ttu-id="bc1b7-165">Néhány perc múlva hello sssd csomag hello virtuális gép első telepíthető.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-165">After a few minutes, hello sssd package should get installed on hello virtual machine.</span></span>

    ![telepített realmd](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-sssd-installed.png)
3. <span data-ttu-id="bc1b7-167">**Telepítse a kerberos:** a PuTTY terminál, írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="bc1b7-167">**Install kerberos:** In your PuTTY terminal, type hello following command:</span></span>

    <span data-ttu-id="bc1b7-168">sudo yum telepítés krb5-munkaállomás krb5-függvénytárak</span><span class="sxs-lookup"><span data-stu-id="bc1b7-168">sudo yum install krb5-workstation krb5-libs</span></span>

    ![Kerberos telepítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-install-kerberos.png)

    <span data-ttu-id="bc1b7-170">Néhány perc múlva hello realmd csomag hello virtuális gép első telepíthető.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-170">After a few minutes, hello realmd package should get installed on hello virtual machine.</span></span>

    ![A Kerberos telepítve](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kerberos-installed.png)

## <a name="join-hello-linux-virtual-machine-toohello-managed-domain"></a><span data-ttu-id="bc1b7-172">Csatlakozás hello Linux virtuális gép toohello felügyelt tartományhoz</span><span class="sxs-lookup"><span data-stu-id="bc1b7-172">Join hello Linux virtual machine toohello managed domain</span></span>
<span data-ttu-id="bc1b7-173">Most, hogy a szükséges hello csomagok hello Linux virtuális gépek vannak telepítve, hello következő feladata toojoin hello virtuális gép toohello által felügyelt tartományokhoz.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-173">Now that hello required packages are installed on hello Linux virtual machine, hello next task is toojoin hello virtual machine toohello managed domain.</span></span>

1. <span data-ttu-id="bc1b7-174">Hello AAD tartományi szolgáltatásokra által kezelt tartomány felderítése.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-174">Discover hello AAD Domain Services managed domain.</span></span> <span data-ttu-id="bc1b7-175">A PuTTY terminál írja be a következő parancs hello:</span><span class="sxs-lookup"><span data-stu-id="bc1b7-175">In your PuTTY terminal, type hello following command:</span></span>

    <span data-ttu-id="bc1b7-176">sudo tartomány CONTOSO100.COM felderítése</span><span class="sxs-lookup"><span data-stu-id="bc1b7-176">sudo realm discover CONTOSO100.COM</span></span>

    ![A kezdőtartomány felderítése](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-discover.png)

    <span data-ttu-id="bc1b7-178">Ha **a kezdőtartomány felderítése** értéke nem lehet toofind a felügyelt tartományok, bizonyosodjon meg arról, hogy hello tartomány elérhető hello (próbálja ping) virtuális gépről.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-178">If **realm discover** is unable toofind your managed domain, ensure that hello domain is reachable from hello virtual machine (try ping).</span></span> <span data-ttu-id="bc1b7-179">Gondoskodjon arról is, hogy hello a virtuális gép valóban már telepített toohello ugyanazt a virtuális hálózatot, mely hello által kezelt tartomány érhető el.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-179">Also ensure that hello virtual machine has indeed been deployed toohello same virtual network in which hello managed domain is available.</span></span>
2. <span data-ttu-id="bc1b7-180">Kerberos inicializálni.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-180">Initialize kerberos.</span></span> <span data-ttu-id="bc1b7-181">A PuTTY terminál írja be a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-181">In your PuTTY terminal, type hello following command.</span></span> <span data-ttu-id="bc1b7-182">Adjon meg egy felhasználót, aki toohello "AAD DC rendszergazdák" csoportba tartozik.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-182">Ensure that you specify a user who belongs toohello 'AAD DC Administrators' group.</span></span> <span data-ttu-id="bc1b7-183">Csak ezek a felhasználók kapcsolódhatnak számítógépek toohello által felügyelt tartományokhoz.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-183">Only these users can join computers toohello managed domain.</span></span>

    <span data-ttu-id="bc1b7-184">kinitbob@CONTOSO100.COM</span><span class="sxs-lookup"><span data-stu-id="bc1b7-184">kinit bob@CONTOSO100.COM</span></span>

    ![Kinit](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-kinit.png)

    <span data-ttu-id="bc1b7-186">Győződjön meg arról, hogy hello tartománynév nagybetűvel adja meg, más kinit sikertelen lesz.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-186">Ensure that you specify hello domain name in capital letters, else kinit fails.</span></span>
3. <span data-ttu-id="bc1b7-187">Hello gép toohello tartományhoz.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-187">Join hello machine toohello domain.</span></span> <span data-ttu-id="bc1b7-188">A PuTTY terminál írja be a következő parancs hello.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-188">In your PuTTY terminal, type hello following command.</span></span> <span data-ttu-id="bc1b7-189">Adja meg a hello hello megelőző lépést ("kinit") megadott ugyanahhoz a felhasználóhoz.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-189">Specify hello same user you specified in hello preceding step ('kinit').</span></span>

    <span data-ttu-id="bc1b7-190">sudo tartomány illesztési – részletes CONTOSO100.COM -U 'bob@CONTOSO100.COM"</span><span class="sxs-lookup"><span data-stu-id="bc1b7-190">sudo realm join --verbose CONTOSO100.COM -U 'bob@CONTOSO100.COM'</span></span>

    ![A kezdőtartomány-csatlakozás](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-realmd-join.png)

<span data-ttu-id="bc1b7-192">Egy üzenet ("sikeresen regisztrált számítógép tartomány") kell kapnia hello gép sikeresen csatlakoztatva toohello által kezelt tartomány esetén.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-192">You should get a message ("Successfully enrolled machine in realm") when hello machine is successfully joined toohello managed domain.</span></span>

## <a name="verify-domain-join"></a><span data-ttu-id="bc1b7-193">Ellenőrizze a tartományhoz való csatlakozást</span><span class="sxs-lookup"><span data-stu-id="bc1b7-193">Verify domain join</span></span>
<span data-ttu-id="bc1b7-194">Gyorsan ellenőrizheti, hogy hello gép megtörtént-e sikeresen csatlakoztatva toohello által felügyelt tartományokhoz.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-194">You can quickly verify whether hello machine has been successfully joined toohello managed domain.</span></span> <span data-ttu-id="bc1b7-195">Csatlakozás toohello újonnan tartományhoz RHEL virtuális gép SSH és a tartományi felhasználói fiókot, majd ellenőrizze toosee megoldódott hello felhasználói fiókja helyes.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-195">Connect toohello newly domain joined RHEL VM using SSH and a domain user account and then check toosee if hello user account is resolved correctly.</span></span>

1. <span data-ttu-id="bc1b7-196">A Terminálszolgáltatások, PuTTY a következő parancs tooconnect toohello újonnan típus hello a tartományhoz csatlakoztatott RHEL virtuális gép SSH használatával.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-196">In your PuTTY terminal, type hello following command tooconnect toohello newly domain joined RHEL virtual machine using SSH.</span></span> <span data-ttu-id="bc1b7-197">Toohello felügyelt tartományhoz tartozó tartományi fiókot használni (például "bob@CONTOSO100.COM" Ebben az esetben.)</span><span class="sxs-lookup"><span data-stu-id="bc1b7-197">Use a domain account that belongs toohello managed domain (for example, 'bob@CONTOSO100.COM' in this case.)</span></span>

    <span data-ttu-id="bc1b7-198">ssh -l bob@CONTOSO100.COM contoso-rhel.cloudapp.net</span><span class="sxs-lookup"><span data-stu-id="bc1b7-198">ssh -l bob@CONTOSO100.COM contoso-rhel.cloudapp.net</span></span>
2. <span data-ttu-id="bc1b7-199">A PuTTY terminálon írja be a következő parancs toosee, ha hello kezdőkönyvtár megfelelően inicializálva hello.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-199">In your PuTTY terminal, type hello following command toosee if hello home directory was initialized correctly.</span></span>

    <span data-ttu-id="bc1b7-200">pwd</span><span class="sxs-lookup"><span data-stu-id="bc1b7-200">pwd</span></span>
3. <span data-ttu-id="bc1b7-201">Írja be a PuTTY terminál, ha hello csoporttagságot lehet megoldani az megfelelően a következő parancs toosee hello.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-201">In your PuTTY terminal, type hello following command toosee if hello group memberships are being resolved correctly.</span></span>

    <span data-ttu-id="bc1b7-202">id</span><span class="sxs-lookup"><span data-stu-id="bc1b7-202">id</span></span>

<span data-ttu-id="bc1b7-203">Egy minta kimenet parancs a következőképpen:</span><span class="sxs-lookup"><span data-stu-id="bc1b7-203">A sample output of these commands follows:</span></span>

![Ellenőrizze a tartományhoz való csatlakozást](./media/active-directory-domain-services-admin-guide/rhel-join-azure-portal-putty-verify-domain-join.png)

## <a name="troubleshooting-domain-join"></a><span data-ttu-id="bc1b7-205">Hibaelhárítás a tartományhoz való csatlakozást</span><span class="sxs-lookup"><span data-stu-id="bc1b7-205">Troubleshooting domain join</span></span>
<span data-ttu-id="bc1b7-206">Tekintse meg a toohello [hibaelhárítás tartományhoz való csatlakozást](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) cikk.</span><span class="sxs-lookup"><span data-stu-id="bc1b7-206">Refer toohello [Troubleshooting domain join](active-directory-ds-admin-guide-join-windows-vm.md#troubleshooting-domain-join) article.</span></span>

## <a name="related-content"></a><span data-ttu-id="bc1b7-207">Kapcsolódó tartalom</span><span class="sxs-lookup"><span data-stu-id="bc1b7-207">Related Content</span></span>
* [<span data-ttu-id="bc1b7-208">Azure AD tartományi szolgáltatások – első lépések útmutató</span><span class="sxs-lookup"><span data-stu-id="bc1b7-208">Azure AD Domain Services - Getting Started guide</span></span>](active-directory-ds-getting-started.md)
* [<span data-ttu-id="bc1b7-209">Csatlakozás egy Windows Server virtuális gép tooan Azure AD tartományi szolgáltatások által felügyelt tartományokhoz</span><span class="sxs-lookup"><span data-stu-id="bc1b7-209">Join a Windows Server virtual machine tooan Azure AD Domain Services managed domain</span></span>](active-directory-ds-admin-guide-join-windows-vm.md)
* <span data-ttu-id="bc1b7-210">[Hogyan toolog a Linux rendszerű virtuális gép tooa](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bc1b7-210">[How toolog on tooa virtual machine running Linux](../virtual-machines/linux/mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
* [<span data-ttu-id="bc1b7-211">Kerberos telepítése</span><span class="sxs-lookup"><span data-stu-id="bc1b7-211">Installing Kerberos</span></span>](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Managing_Smart_Cards/installing-kerberos.html)
* [<span data-ttu-id="bc1b7-212">Red Hat Enterprise Linux 7 - Windows-integrációs útmutatója</span><span class="sxs-lookup"><span data-stu-id="bc1b7-212">Red Hat Enterprise Linux 7 - Windows Integration Guide</span></span>](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/7/html/Windows_Integration_Guide/index.html)
